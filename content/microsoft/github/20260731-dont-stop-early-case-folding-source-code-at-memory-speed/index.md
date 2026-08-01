---
categories:
- MS
- GitHub
date: '2026-07-31T16:00:00+00:00'
description: 'Suppose a user searches for caf&eacute; and your corpus contains CAF&Eacute;,
  or they type stra&szlig;e and you&rsquo;ve stored STRASSE. To make these count as '
draft: false
original_url: https://github.blog/engineering/architecture-optimization/dont-stop-early-case-folding-source-code-at-memory-speed/
source: GitHub Blog
tags:
- Architecture & optimization
- Engineering
- Open Source
- case folding
- code search
- open source
- Rust
title: 'Don’t stop early: Case-folding source code at memory speed'
---

Suppose a user searches for caf&eacute; and your corpus contains CAF&Eacute;, or they type stra&szlig;e and you&rsquo;ve stored STRASSE. To make these count as matches, you need a canonical form that erases case distinctions, so that two strings which differ only in case compare equal. That form is case folding, and it shows up wherever text is matched rather than displayed: search engines, regex (?i) flags, case-insensitive usernames and hostnames.



It&rsquo;s a basic operation, but at GitHub we run it a lot. Blackbird, GitHub&rsquo;s code search engine, indexes over 180 million repositories&mdash;more than 480TB of source code. Every byte is case-folded before we extract ngrams and build the index, and for every potential query result, another (implicit or explicit) case folding operation is needed to locate matches. At that scale, the speed of even a basic operation starts to matter.



This post is about how we made it fast, and it starts somewhere counterintuitive: the biggest win in the ASCII fast path came from removing an optimization, not adding one. It turns out to be faster to sweep the whole buffer with no branches than to stop early at the first non-ASCII byte. We open-sourced the result as a Rust crate called casefold.



Folding is not lowercasing



It is tempting to reach for str::to_lowercase, but lowercasing and folding are different operations with different goals:



Lowercasing is for display, and it&rsquo;s locale- and context-sensitive: Greek final sigma lowercases to &sigmaf; at the end of a word and &sigma; elsewhere, and Turkish I lowercases differently than English I. Case folding is for comparison, and it&rsquo;s deliberately context-free and locale-independent. The point is a relation that stays stable and symmetric, so that if A folds to match B, B folds to match A in any locale. The Unicode Character Database ships an explicit CaseFolding.txt for exactly that.



The two operations diverge on real characters&mdash;&szlig;, &#304;, final sigma&mdash;which is why lowercasing as a stand-in silently produces wrong matches. This crate implements only the simple (1-to-1) folds&mdash;statuses C and S in CaseFolding.txt&mdash;and not the multi-character &ldquo;full&rdquo; folds (&szlig; &rarr; ss) or Turkic locale folds (the dotted &#304;). This isn&rsquo;t an unusual choice: common tools and regex engines like ripgrep make the same restriction, and being consistent across tools is important.



The counterintuitive core: Don&rsquo;t stop early



We deal mostly with source code, so the text we fold is overwhelmingly ASCII and making it run at memory speed is the single most important thing we can do. Everything else just has to keep the rare non-ASCII path from spoiling it.



The fold of an ASCII letter is trivial&mdash;A..=Z map to a..=z, everything else is unchanged&mdash;so the ASCII pass is really just &ldquo;sweep the buffer, lowercase in place.&rdquo; Ask any LLM for it and you might get something like this:



let bytes = s.as_bytes_mut(); 
for (i, b) in bytes.iter_mut().enumerate() { 
    if *b &gt;= 0x80 { 
        break; // non-ASCII at index i: hand the rest to the Unicode path 
    } 
    if b.is_ascii_uppercase() { 
        *b += 32; // 'A'..='Z' &rarr; 'a'..='z' 
    } 
}



It looks ideal: do the cheap byte work, and the instant you hit a non-ASCII byte, break and let the &ldquo;real&rdquo; Unicode path take over: &ldquo;only do the cheap work until you have to.&rdquo; On an Apple M4 this runs at about 3 GiB/s. That sounds fine in isolation, but it is more than 15&times; short of &ldquo;optimal&rdquo; because of the if branches.



Let&rsquo;s delete every branch, line by line:




if b &gt;= 0x80 { break } &rarr; don&rsquo;t stop at all. ORevery byte into an accumulator and test it once, after the loop: high_bit_acc |= *b. Same information (was there any non-ASCII byte?), zero branches in the body.



The A..=Z range test &rarr; make it arithmetic. b.wrapping_sub(b'A') &lt; 26 is true exactly for A..=Z (any other byte wraps to &ge; 26), yielding a 0/1 mask with no branch.



The conditional write &rarr; fold the mask into the store.| (is_upper &lt;&lt; 5)sets bit 5&mdash;turning an upper-case letter lower-case and being a no-op on everything else&mdash;the byte is always written, never branched on.




What&rsquo;s left has no branch in its body and no early exit:



let mut high_bit_acc: u8 = 0; 
for b in &amp;mut bytes { 
    high_bit_acc |= *b; // detect any non-ASCII byte 
    let is_upper = b.wrapping_sub(b'A') &lt; 26; // branchless A..=Z test 
    *b |= u8::from(is_upper) &lt;&lt; 5; // set bit 5 &rarr; lowercase, else no-op 
} 
if high_bit_acc &amp; 0x80 == 0 { 
    return bytes; // pure ASCII: already folded in place, no second buffer 
}



A loop with no data-dependent control flow is trivially vectorizable: LLVM emits 16-byte-at-a-time NEON and the whole thing runs at &gt; 45 GiB/s&mdash;essentially memory bandwidth. And we come out of the pass already knowing, from high_bit_acc, whether there&rsquo;s any non-ASCII work left to do.



How much did each step matter? Measuring the cumulative ladder on pure ASCII (Apple M4, 5.7 KB buffer):



Version&nbsp;Throughput&nbsp;Vectorized?&nbsp;naive (break + branch test)&nbsp;3.1 GiB/s&nbsp;no (0 vector&nbsp;instrs)&nbsp;&rarr; branchless test/write,&nbsp;keep&nbsp;break&nbsp;2.6 GiB/s&nbsp;no (0 vector&nbsp;instrs)&nbsp;&rarr; drop the early-exit break&nbsp;7.6 GiB/s&nbsp;partially&nbsp;(25 vector&nbsp;instrs)&nbsp;&rarr; branchless test + write (the loop)&nbsp;&gt;45&nbsp;GiB/s&nbsp;fully (41 vector&nbsp;instrs)&nbsp;



The early-exit is what gates vectorization: keep the break but make the body perfectly branch-free and you still get zero vector instructions (~2.6 GiB/s); a data-dependent loop exit is enough on its own to keep the loop scalar. Only once the break is gone can the compiler vectorize. The final step&mdash;making the upper-case fold branchless&mdash;then turns a partially vectorized loop (which still compiles the conditional store to a compare-blend-masked-store, ~7.6 GiB/s) into the straight-line arithmetic that hits memory bandwidth.



Note: Branchless is a pessimization in scalar code. Look again at the table: making the body branchless while keeping the break (2.6 GiB/s) is actually slower than the naive branchy loop (3.1 GiB/s). The asm explains why. The branchy version only stores a byte when it actually changes one; its conditional strbis skipped for every lowercase letter, digit and space (the vast majority of real text), and the well-predicted branch that guards it is nearly free. The branchless version replaces that rarely taken store with an unconditional strbevery iteration, writing back all ~5,700 bytes instead of just the handful of upper-case ones. Extra write traffic for no benefit. Branchless-write only wins once the loop vectorizes, because then the store becomes a single 16-byte vector write regardless of content, and the per-byte cost disappears. The lesson: a branchless body is worth it only as the enabler for vectorization. On its own, in scalar code, it can cost you.



There&rsquo;s also a middle ground, and it&rsquo;s what standard libraries use. Instead of testing one byte at a time, [u8]::is_ascii scans a machine word at a time&mdash;on a 64-bit target it tests 16 bytes per iteration by OR-ing two u64 lanes and checking all their high bits with a single &amp; 0x8080_8080_8080_8080 mask. You can build the ASCII fast path on top of that: chunk-scan to find the ASCII prefix, then run the branchless (vectorizable) convert over it. That keeps the early-exit ability&mdash;it still bails on the first non-ASCII block&mdash;while letting both halves go fast. The catch is that it reads the data twice (once to scan, once to convert), landing at about 23 GiB/s&mdash;roughly half of the single-pass branchless sweep, and ~7&times; the naive break loop. A solid, general-purpose default; just not the absolute ceiling when you control the whole loop and can fold detection and conversion into one branch-free pass.



Wouldn&rsquo;t fusing the two passes be faster? It&rsquo;s the obvious next thought: keep the chunked early-exit but convert each 16-byte block right after you&rsquo;ve confirmed it&rsquo;s ASCII, reading the data only once. Measured, it&rsquo;s ~2.6&times; slower&mdash;8.7 GiB/s versus the two-pass 23. The inner block convert still vectorizes to a single 16-byte op, but now there&rsquo;s a data-dependent early-exit branch every 16 bytes, and that branch pins the loop to one block at a time: the compiler doesn&rsquo;t unroll or software-pipeline across blocks, and each iteration pays the full load&rarr;test&rarr;branch&rarr;convert&rarr;store latency with nothing to hide it behind. Split into two passes, each one is clean: the scan is a branch-light, store-free word scan that races through memory, and the convert is the fully-vectorized branch-free sweep at &gt;45 GiB/s. Two fast, branch-free passes beat one branchy fused pass&mdash;even though the fused version touches the data half as many times. It&rsquo;s the same lesson one more time: in the hot loop, the branch is the enemy.



Avoiding the heap



Forty-Five GiB/s also means doing zero unnecessary allocation. simple_fold takes the input String by value, owning the heap buffer it can mutate and return it. If the OR-accumulator&rsquo;s high bit was clear, the input was pure ASCII already folded in place. We hand the same allocation straight back, no second buffer and no copy. Otherwise, we memchrto the first non-ASCII byte and scan the tail from there, leaving the output buffer unallocated (a null write cursor) until we hit a character that folds to different bytes. Text whose multibyte content never folds&mdash;CJK, Hangul, Kana, Arabic, Hebrew, symbols&mdash;also returns the original allocation untouched, never copying a byte.



Why a second buffer rather than rewriting in place like the ASCII pass? Because folding can make the string longer: almost every fold preserves the UTF-8 length or shrinks it, but two outliers grow&mdash;U+023A (&#570;) and U+023E (&#11391;) are 2 bytes each yet fold to 3-byte characters (&#11365;, &#576;). Once one appears, the output no longer fits in the input&rsquo;s bytes, and we need somewhere new to write.



We allocate that buffer once, sized for the worst case, rather than growing it as more folds appear. Incremental reserve calls would mean re-checking capacity, occasionally reallocating, copying everything written so far, and juggling extra length/capacity bookkeeping; a single up-front allocation lets a raw write cursor run straight to the end with none of that. And since the cursor is nulluntil that first growing/changing fold, it doubles as the &ldquo;have we allocated the extra buffer yet?&rdquo; flag.



Sizing it needs a bound on growth, and those same two outliers give it: every 2 input bytes yield at most 3 output bytes, capping the output at 1.5&times; the input&mdash;exactly the capacity we reserve:



out = Vec::with_capacity(bytes.len() + bytes.len() / 2 + 4); 



After that the loop writes through a raw pointer with no capacity checks and calls set_len once at the end. Two more details keep it branch-light. The run of unchanged bytes between two folds is moved with a single copy_nonoverlapping rather than byte by byte. And each fold unconditionally writes all 4 bytes of a little-endian word before bumping the cursor by only the folded length (1&ndash;4)&mdash;dropping a branch on the output length from the hot path, with the + 4 in the reservation as the headroom that makes the final character&rsquo;s over-store safe.



Making Unicode cheap too



When a character does fold, we still don&rsquo;t want to fall off a cliff&mdash;decode UTF-8, hash lookup, re-encode. Unicode 16.0 has 1484 simple-fold mappings, but they&rsquo;re a very sparse and very structured relation. Four observations shrink them to 1776 bytes and let the fold run without ever decoding a full character.



Even on the non-ASCII path, the overwhelming majority of characters do not fold. The hot operation isn&rsquo;t really &ldquo;fold this character,&rdquo; it&rsquo;s &ldquo;does this character fold?&rdquo; Almost always no. The table has to make that negative test as cheap as possible; the actual folding is the rare case on an already-rare path. That priority is what shapes the layout below&mdash;the page bitmap exists precisely so a non-folding character is rejected in a single bit test, straight from its leading UTF-8 bytes, without decoding or scanning anything.



This is exactly why a HashMap&lt;u32, u32&gt; is the wrong shape for the job, not just a bigger one. A hash map is optimized for the hit: it finds a present key in roughly one probe, and only spends extra work (more probes, full key comparison) when load factor or collisions bite. But our workload is dominated by misses&mdash;characters that aren&rsquo;t in the table at all&mdash;and a miss is a hash map&rsquo;s least favorite query: it still has to hash the key, jump to a bucket, and walk the probe sequence far enough to prove absence.



Foldable code points cluster into 64-code-point &ldquo;pages&rdquo;



Foldable code points bunch together. Slice the code space into 64-code-point &ldquo;pages&rdquo; and the ~1484 folds touch just 59 of ~1960 possible pages. A one-bit-per-page presence bitmap answers the negative test on its own: a clear bit is a definitive &ldquo;no fold&rdquo;&mdash;copy through, done&mdash;which is what makes fold-free scripts cheap. Only on a set bit do we consult a second structure, a cumulative-popcount side table that ranks the page (how many populated pages precede it) to find its slice of entries, storing nothing for the ~1900 empty pages.



let (word_idx, bit_idx, c_len) = if lead &lt; 0xE0 { 
    (0usize, lead &amp; 0x1F, 2usize) // 2-byte: word 0 
} else if lead &lt; 0xF0 { 
    ((lead &amp; 0x0F) as usize, bytes[read + 1] &amp; 0x3F, 3) // 3-byte: word = nibble 
 
} else { 
    ( 
        (((lead &amp; 0x07) as usize) &lt;&lt; 6) | (bytes[read + 1] &amp; 0x3F) as usize, 
        bytes[read + 2] &amp; 0x3F, 
        4usize, 
    ) // 4-byte: merge 2 bytes 
}; 
// reject without decoding: clear bit &rArr; no fold 
if word_idx &gt;= PAGE_BITMAP.len() || (PAGE_BITMAP[word_idx] &gt;&gt; bit_idx) &amp; 1 == 0 { 
    read += c_len; 
    continue; 
} 



Because word_idxdepends only on the lead byte (and, for four-byte sequences, the first continuation byte), the bitmap load can be issued early.



Within a page, folds come in runs



A set page bit tells us something on this page folds, but not which code points or to what. The obvious encoding is one entry per foldable code point&mdash;but that is both bulky and slow to search: a page can hold dozens of folds, and we&rsquo;d have to scan them all to find the one matching the current code point. The structure of the data rescues us again. Adjacent code points overwhelmingly share the same delta to their fold: A&ndash;Z all map +32, and Latin Extended is full of alternating runs like 0x0100, 0x0102, 0x0104, &hellip; where every second code point folds. Instead of per-code-point entries we store runs&mdash;start, end, stride, delta&mdash;and a 1-bit stride flag covers both the contiguous and the every-other case. This interval compression collapses the ~1484 individual folds into just 238 runs across the 59 pages (&asymp;four per page), leaving the within-page search only a handful of entries to look at instead of dozens. This range-with-delta encoding (including the stride trick) is borrowed from Go&rsquo;s unicode package, whose CaseRange records store a Lo/Hi range plus per-case deltas, with an UpperLower sentinel marking the alternating blocks. Runs are split at the page boundaries so a run never straddles two pages.



A run record is two clean bytes



With both endpoints inside one page they fit in 6 bits, split across two arrays: RUN_END_LOW[``i``] = end &amp; 0x3F (the scan key) and RUN_START_STRIDE[``i``] = (start &amp; 0x3F) | ((stride &minus; 1) &lt;&lt; 6) (read only on a hit). Because each key is one clean byte, the within-page search can go wide: rather than comparing cp &amp; 0x3F against the runs one at a time, we load 8 end_low bytes into a single u64 and test all of them at once with one branchless SWAR step&mdash;(chunk | 0x80&hellip;80) &minus; broadcast(low) &amp; 0x80&hellip;80 sets the top bit of every lane whose key is &ge; cp &amp; 0x3F. A single bit-scan of that mask (the keys are sorted, so the first set lane is the run we want) finds the slot. A page holds ~4 runs on average; that one 8-wide compare almost always resolves the entire search in a single step. One unlucky page does hold 30 runs, which puts the compare inside a short loop that strides eight keys at a time&mdash;but that loop trips at most a handful of times on exactly one page in all of Unicode, and never on the common ones. Either way: no per-run branch, and no code-point reconstruction anywhere.



/// Offset of the first run with `end_low &gt;= low_v` in a page of `n` runs, 
/// or `n` if none. Scans 8 `end_low` bytes at a time via SWAR. 
#[inline] 
fn scan_end_low(lo: usize, n: usize, low_v: u8) -&gt; usize { 
    const HIGH: u64 = 0x8080_8080_8080_8080; 
    const ONES: u64 = 0x0101_0101_0101_0101; 
    let bcast = (low_v as u64).wrapping_mul(ONES); 
    let mut base = 0; 
    while base &lt; n { 
        // RUN_END_LOW is padded by 8 bytes so this read is always in bounds. 
        let chunk = u64::from_le_bytes( 
            RUN_END_LOW[lo + base..lo + base + 8] 
                .try_into() 
                .expect("8-byte slice"), 
        ); 
        // `(b | 0x80) - low_v` keeps its high bit iff `b &gt;= low_v` (no 
        // cross-lane borrow). The first set lane is the first run `&gt;= low_v`. 
        let ge = (chunk | HIGH).wrapping_sub(bcast) &amp; HIGH; 
        if ge != 0 { 
            let j = base + (ge.trailing_zeros() / 8) as usize; 
            return if j &lt; n { j } else { n }; 
        } 
        base += 8; 
    } 
    n 
} 



Folding is a little-endian byte addition



On a little-endian machine the folded character&rsquo;s UTF-8 bytes, read as a u32, equal the source bytes (as a u32) plus a per-run constant. A parallel BYTE_DELTA[i] table then turns the whole fold into a masked load, one wrapping_add, and a 4-byte store:



let word = u32::from_le_bytes(next_four_bytes) &amp; length_mask; // keep this char's bytes 
let folded = word.wrapping_add(BYTE_DELTA[i]); // the fold, as one byte add 
write_u32_le(dst, folded); // store all 4 bytes... 
dst += utf8_len(folded); // ...advance by the folded length



Both lengths in that snippet&mdash;the length_mask for the source character and the advance by the folded length for the destination&mdash;come from one more tiny trick. A UTF-8 sequence&rsquo;s length is fixed by the top four bits of its lead byte, letting the 16 possible lengths pack one nibble each into a single 64-bit constant (0x4322_1111_1111_1111); the length is then a shift and a mask, (LEN_BITS &gt;&gt; (4 * (lead &gt;&gt; 4))) &amp; 0xF&mdash;no if chain, no table memory, nothing for the predictor to get wrong. (A count leading ones&mdash;(!lead).leading_zeros()&mdash;would also work, since a lead byte carries one leading 1-bit per byte of the sequence.)



/// Number of bytes in the UTF-8 sequence whose lead byte is `lead`. 
#[inline] 
pub fn utf8_len(lead: u8) -&gt; usize { 
    const UTF8_LEN_BY_LEAD: u64 = 0x4322_1111_1111_1111; 
    ((UTF8_LEN_BY_LEAD &gt;&gt; (4 * (lead &gt;&gt; 4))) &amp; 0xF) as usize 
}



Because we advance by the folded length, this even handles length-changing folds&mdash;U+212A KELVIN SIGN (3 bytes) &rarr; k (1 byte), or U+023A &#570; (2 bytes) &rarr; U+2C65 &#11365; (3 bytes)&mdash;by writing fewer or more bytes than were read. That&rsquo;s the part we believe is genuinely new: every other folder we looked at&mdash;ICU, Go&rsquo;s unicode, Rust&rsquo;s regex, CPython, glibc&mdash;decodes UTF-8 to a code point, applies the fold there, and re-encodes (even SIMD folders decode first). Doing the arithmetic in byte space skips both the decode and the encode, which is exactly why this path can outrun a hash map that already has the answer tabulated&mdash;the hash map still has to decode its key and encode its result. The byte-space arithmetic assumes the input is well-formed, shortest-form UTF-8&mdash;every code point encoded with the minimal number of bytes. Reading the source bytes as a u32and adding a per-run delta only lands on the correct folded encoding when the source is in canonical form; an overlong encoding (a code point padded into more bytes than necessary, e.g. / as 0xC0 0xAF) has a different byte pattern and would break thelength_mask and the delta arithmetic. This is not a real restriction in Rust&mdash;&amp;str/String are guaranteed to hold valid UTF-8, which by definition rejects overlong sequences&mdash;but a caller feeding raw bytes from elsewhere must validate (or otherwise normalize) them first.



The ASCII shortcut in the tail loop



One more shortcut rounds out the tail loop. Remember the first pass already lowercased every ASCII byte, so when the scan meets an ASCII byte in the tail it advances a single byte and moves on&mdash;no page probe, no table touch at all. And it doesn&rsquo;t copy that byte either: unmodified bytes (ASCII and non-folding multibyte alike) aren&rsquo;t moved one at a time. The scan just keeps walking until it reaches a character that actually folds, then flushes the whole unchanged run between the last fold and this one with a single copy_nonoverlapping. Mixed text&mdash;CJK with ASCII spaces and punctuation, or code with the occasional accented identifier&mdash;therefore races through the ASCII filler and only consults the bitmap for genuine multibyte characters, copying in bulk rather than byte by byte.



Putting it together: the whole table



Component&nbsp;Bytes&nbsp;PAGE_BITMAP (1 bit per 64-cp page)&nbsp;248&nbsp;POPCNT_SAMPLES (cumulative&nbsp;popcount)&nbsp;32&nbsp;PAGE_OFFSET (per populated page)&nbsp;60&nbsp;RUN_END_LOW (scan key, end &amp; 0x3F, +8 pad)&nbsp;246&nbsp;RUN_START_STRIDE (start &amp; 0x3F | stride)&nbsp;238&nbsp;BYTE_DELTA (little-endian fold delta per run)&nbsp;952&nbsp;Total&nbsp;1776&nbsp;



That&rsquo;s 9.6 bits per fold entry, over half of it the BYTE_DELTA side table we trade for the decode-free path; the index + run records alone are ~4.4 bits/entry.



Next to the obvious alternatives, that 1776 bytes is an order of magnitude or more smaller&mdash;and unlike most of them, it never decodes a character:



Representation&nbsp;SizeNa&iuml;ve&nbsp;[(u32, u32); 1484]&nbsp;~11.6 KB&nbsp;regex-syntax&rsquo;s&nbsp;case_folding_simple&nbsp;table&nbsp;~70 KB&nbsp;Go&rsquo;s&nbsp;unicode.SimpleFold&nbsp;(orbit + ASCII + ranges)&nbsp;~7.3 KB&nbsp;A runtime&nbsp;HashMap&lt;u32, u32&gt;&nbsp;~17 KB&nbsp;This crate (paged bitmap + packed runs)&nbsp;1776 B&nbsp;



Where it lands against the alternatives



On the common case, ASCII, folding runs at memory bandwidth (&gt;45 GiB/s), more than an order of magnitude ahead of other real folders and more than 50% faster than the (non-equivalent) str::to_lowercase function. To get a rough &ldquo;upper bound&rdquo; for the non-ASCII case, we measured the optimized Utf8 decoding + encoding round trip without performing any actual case folding using the simdutf crate. This experiment achieves consistently about 2GB/sec and is only about twice as fast than our solution for the worst case all-folding input. A naive hash map trails everything on all workloads.



The three columns are real case folders that produce identical output: simple_fold (this crate), simd_normalizer (the simd-normalizer crate), and HashMap (naive CaseFolding.txt lookup). The workload rows are chosen to simulate different scenarios from typical to worst case:



Workload (input size)&nbsp;simple_fold&nbsp;simd_normalizer&nbsp;HashMap (byte path)&nbsp;Pure ASCII (5.7 KB)&nbsp;&gt;45&nbsp;GiB/s&nbsp;1.21 GiB/s&nbsp;213 MiB/s&nbsp;Chinese/Japanese/Korean, no folds (8.1 KB)&nbsp;2.95 GiB/s&nbsp;1.97 GiB/s&nbsp;558 MiB/s&nbsp;Symbols / Myanmar, no folds (9.0 KB)&nbsp;2.96 GiB/s&nbsp;1.56 GiB/s&nbsp;410 MiB/s&nbsp;Worst case: Latin/Greek/Cyrillic (Unicode U+0000&ndash;U+FFFF), all folding (8.8 KB)&nbsp;869 MiB/s&nbsp;922 MiB/s&nbsp;334 MiB/s&nbsp;Length-changing folds (1.7 KB)&nbsp;1.26 GiB/s&nbsp;716 MiB/s&nbsp;233 MiB/s&nbsp;



Treat the absolute figures as illustrative, not portable: the whole design leans on auto-vectorization, SWAR, and little-endian byte arithmetic, so the numbers&mdash;and even the ratios between rows&mdash;can shift substantially on a different microarchitecture (a wider or narrower vector unit, different memory bandwidth, a big-endian target, x86 vs ARM).



More details can be found in the performance section of the README.



Take this with you



Case folding is about as basic as text operations get, which is exactly why it was worth the effort: we run it across every byte we index. The wins came from two ideas that both cut against instinct&mdash;sweep the whole buffer branch-free instead of stopping early, and do the fold as byte-space arithmetic instead of decoding to a code point. Together they let the common case run at memory bandwidth and the rare fold run without a decode, in a table small enough (1776 bytes) to stay resident. The decode-free byte-space fold is the piece we believe is genuinely new; it&rsquo;s why this path can beat a hash map that already has the answer.



There&rsquo;s surely more to find here, and we&rsquo;d like to see it. The crate is casefold; the generated table and full design notes live alongside the source.

The post Don&#8217;t stop early: Case-folding source code at memory speed appeared first on The GitHub Blog.

---
*원문: [https://github.blog/engineering/architecture-optimization/dont-stop-early-case-folding-source-code-at-memory-speed/](https://github.blog/engineering/architecture-optimization/dont-stop-early-case-folding-source-code-at-memory-speed/)*
