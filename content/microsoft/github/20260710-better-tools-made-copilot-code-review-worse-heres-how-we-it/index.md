---
categories:
- MS
- GitHub
date: '2026-07-10T15:57:47+00:00'
description: 'Give an agent better tools and it should do better work. That&rsquo;s
  the instinct, anyway.




  When you open a pull request, Copilot code review reads the diff '
draft: false
original_url: https://github.blog/ai-and-ml/github-copilot/better-tools-made-copilot-code-review-worse-heres-how-we-actually-improved-it/
source: GitHub Blog
tags:
- AI & ML
- Architecture & optimization
- Engineering
- GitHub Copilot
- LLMs
- agentic workflows
- developer tools
- GitHub Copilot code review
title: Better tools made Copilot code review worse. Here’s how we actually improved
  it.
---

Give an agent better tools and it should do better work. That&rsquo;s the instinct, anyway.



When you open a pull request, Copilot code review reads the diff and explores the surrounding code to find the problems that matter before they ship. To do that, it used its own code exploration tools. So when we swapped in the better-maintained, shared tools that power the Copilot CLI, grep, glob, and view, we expected a clean upgrade.



Instead, in our benchmarks, we found that the cost of reviews was higher and fewer issues were being caught.



But the tools weren&rsquo;t the problem. The instructions were. Once we rewrote them for the way a reviewer actually reads a pull request, the regression flipped into a win: roughly 20% lower average review cost, while maintaining the same review quality.



This is the story of how adjusting the workflows around the tools led us to a fix.



Same tools, wrong instincts



If you&rsquo;ve built on top of an agent framework, you&rsquo;ve probably inherited its tools too. They work, so you keep them, until the day your use case drifts far enough from what they were designed for that they quietly start working against you. That&rsquo;s the situation we were in. Before trying to use the shared CLI tools, Copilot code review used its own code exploration tools. That tool layer was inspired by earlier agentic systems, including ideas from SWE-agent-style repository navigation and GitHub Copilot Autofix: list directories, search files, search directories, and read code. Those tools worked, but they were specific to Copilot code review, and they were designed for how models behaved at the time. Earlier agentic coding models made fewer tool calls and were worse at automatically pulling in necessary context. This meant it was more important to include all relevant information in the few tool calls that the model made.



Meanwhile, the Copilot CLI harness has a shared set of Unix-inspired code exploration tools: grep, glob, and view. That harness is also used by a growing number of Copilot agent products, including GitHub Copilot cloud agent, so harness improvements can benefit more than one product. We wanted to clean up and share infrastructure where possible, so we experimented with using the tools from the Copilot CLI harness in Copilot code review. The goal was to reduce duplicated tool implementations, create one shared place to improve code exploration tools, and make it easier to carry those improvements across Copilot products.



On paper, the migration looked simple:



Old Copilot code reviewGitHub Copilot CLIPurposelist_dir&nbsp;glob&nbsp;Discover candidate files and directories before opening&nbsp;code.&nbsp;search_file&nbsp;and&nbsp;search_dir&nbsp;grep&nbsp;Search code for matching text, symbols, or call sites.&nbsp;read_code&nbsp;view&nbsp;Read the relevant file contents once a path or range is known.&nbsp;



The existing review tools were not thin wrappers. When searching for a directory or reading a code range, they could return the matched or requested lines plus extra surrounding code context. That added token cost, but it also matched how earlier models often benefited from having nearby context included automatically.



Initially, we hoped this would be a simple migration: swap one set of tools for another. But when we tested the shared tools in offline benchmarks, the review agent became less efficient and less effective. Average cost increased, and the number of useful comments dropped.



The trace revealed a browsing loop



Our internal Copilot code review benchmarks were useful because they show more than a final score. They show the path the agent took, including which tools it called, how much output came back, where errors happened, and whether it was narrowing toward evidence or widening the search.



When we first tried the shared Copilot CLI tools in offline benchmarks, the agent often behaved as if it was browsing a repository instead of investigating a pull request. It would search broadly, guess likely paths, read broadly, find more things to search, and carry that extra context forward.



Figure 1:&nbsp;Before &mdash; a&nbsp;simplified&nbsp;illustration&nbsp;of the general-purpose&nbsp;behavior we&nbsp;observed:&nbsp;widening&nbsp;the search, guessing&nbsp;paths, and accumulating context.



That pattern is understandable. Broad exploration can be useful when the task is &ldquo;understand this repo.&rdquo; But it&rsquo;s not how a reviewer would usually review a pull request.



When I review a pull request, I start from the diff and ask targeted questions:




Where is this function called?



Is this config key used anywhere else?



Is there a test or helper with the same pattern?



What is the smallest nearby code range that explains this behavior?




I do not want to open a large part of the repository before I know what I am looking for. I want the minimal context needed to answer the question, without overloading the review with unrelated code.



That matters because every tool result becomes part of the agent&rsquo;s working context. Extra file contents can be carried forward into later reasoning, increasing cost and sometimes making the review less focused. A tool result is not a disposable printout; for an agent, it&rsquo;s extra tokens that stay in the context window.



The traces made that difference visible. The shared tools were not the problem. The instructions were giving the agent the wrong instincts to do an efficient and effective review.



The tools themselves worked, but their instructions were tuned for their use within the Copilot CLI and implied the wrong workflow: the agent used grep, glob, and view like a broad coding assistant instead of a reviewer. A coding assistant may map a whole area before making a change to ensure it doesn&rsquo;t break some other corner of the code. On the other hand, a reviewer usually starts from the diff, asks whether the change introduced a problem, and then looks for the narrowest nearby evidence required to confirm or dismiss it.



General coding-assistant tool instructions, like the ones used by Copilot CLI or Copilot cloud agent, make sense for an interactive assistant. A developer may ask it to understand a repository, plan a change, edit files, and continue over multiple turns.



Copilot code review has a narrower job: start from a pull request diff, gather enough surrounding evidence to decide whether a change introduces a real issue, and avoid loading context that is not needed for that review question.



It was therefore clear that we couldn&rsquo;t simply replace the previous Copilot code review tools with the tools from the Copilot CLI without additional prompting work. The problem became: how do we design tool instructions that use these shared tools effectively in a code review setting?



Rewriting the tool instructions for a reviewer&rsquo;s workflow



The next iterations made the guidance specific to code review. The workflow we wanted Copilot code review to follow was:




Start from the diff and form specific review questions.



Use glob when the path is uncertain and grep to find candidate files, symbols, and call sites.



Batch cheap discovery before reading files.



Use view only when the agent knows which file or line range it needs.



Batch focused reads instead of alternating between one search and one read.




In oversimplified form, this was the behavior we encoded:



Generic posture: Use the available tools to inspect repository context that may be relevant.



Review-shaped guidance: Start from the diff. Narrow first with grep and glob; read exact evidence with view. If grep fails to find relevant context, retry with a simpler escaped search. If a path is wrong, pivot to glob instead of guessing nearby paths.



For example, imagine the diff changes an authorization helper that decides whether an operation is allowed. A relevant review question is not &ldquo;show me the full contents of every file that calls this helper.&rdquo; It could instead be the narrower: &ldquo;are any request-handling callers relying on the old behavior?&rdquo;



The intended path is short:



start from the helper changed in the diff 
grep for callers of that helper 
glob for likely route, handler, or controller files 
view the most relevant caller ranges 
decide whether any caller changes the risk



The guidance also changed how the agent recovered from failed searches. If an input made grep fail, the better next step was one simpler, corrected search. If a path was wrong, the better next step was glob, not guessing neighboring paths and reading whatever happened to exist. That nudged the agent away from letting a small tool failure turn into a larger exploration loop.



Figure 2: After &mdash; a simplified illustration of the review-shaped behavior the prompt guided toward: stay anchored to the diff, narrow with&nbsp;grep&nbsp;and&nbsp;glob, then read focused ranges with&nbsp;view.



The change was small in wording and large in effect. It changed the rhythm of the agent from &ldquo;browse, read, search again&rdquo; to &ldquo;ask, narrow, read, decide.&rdquo;



Benchmarks let us debug behavior, not just scores



The shared harness gave us the tools. The internal Copilot code review benchmarks gave us the feedback loop.



We could run the same review examples, compare tool traces, update the instructions, and run again. That let us ask concrete questions:




Did the agent narrow first, or read broadly first?



Did it batch independent searches?



Did it call view only when it had a reason?



Did a tool-instruction change reduce tool errors, or just move them somewhere else?



Did the trace stay focused on evidence from the diff?



Did the review still preserve the quality metrics we cared about?




The most useful signal was not &ldquo;the instructions are better.&rdquo; It was more concrete. The agent was making a similar number of tool calls, but spending more of them on relevant evidence instead of repeatedly expanding the search.



That connected product-level outcomes to understandable engineering behavior. Instead of guessing why a score moved, we could inspect the workflow that produced it.



The result: roughly 20% lower average review cost



In production, the tuned behavior showed roughly 20% lower average review cost compared with the control. Importantly, it did not show a quality signal that could block shipping.



The reduction did not come from the tools by themselves, it came from the workflow around them. Shared code exploration tools, Copilot code review custom tool instructions, and internal benchmarks made the agent&rsquo;s behavior visible enough to tune.



That framing matters when building with agents. It can be tempting to treat tools as implementation details by swapping one tool for another, then comparing the final answer. But for an agent, the tool surface is part of the product experience. It changes what the agent notices, how it searches, how much context it carries forward, and when it decides it has enough evidence.



Tool descriptions and system instructions are closer to API documentation. Unclear API docs can leave a developer confused and lead to inefficient or wrong decisions. Unclear tool prompting can do the same for an LLM; a small wording change can affect cost, quality, and the shape of the investigation because it changes how the agent spends its attention.



Same tools, different job



We also tried to apply the same kind of focused tool instructions in the CLI, where it did not produce the same kind of win. That is a useful counterexample, and an important guardrail for the lesson.



Copilot code review is anchored to a diff and a review question. Copilot CLI handles broader, interactive coding tasks where exploration can be part of the job. There may be no single diff anchor, the user may change direction over multiple turns, and the right context may not be obvious at the start. The same grep, glob, and view tools can support both products, but the workflow around those tools has to match the product.



The takeaway is that shared tools scale when the instructions and benchmarks match the job.




Try it out yourself using GitHub Copilot code review.


The post Better tools made Copilot code review worse. Here&#8217;s how we actually improved it. appeared first on The GitHub Blog.

---
*원문: [https://github.blog/ai-and-ml/github-copilot/better-tools-made-copilot-code-review-worse-heres-how-we-actually-improved-it/](https://github.blog/ai-and-ml/github-copilot/better-tools-made-copilot-code-review-worse-heres-how-we-actually-improved-it/)*
