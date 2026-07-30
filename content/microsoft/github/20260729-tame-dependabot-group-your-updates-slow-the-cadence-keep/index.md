---
categories:
- MS
- GitHub
date: '2026-07-29T16:00:00+00:00'
description: 'If you maintain an active repository, you know the feeling. You open
  your notifications on a Monday morning and there they are: five, 10, sometimes a
  dozen Depe'
draft: false
original_url: https://github.blog/security/supply-chain-security/tame-dependabot-group-your-updates-slow-the-cadence-keep-security-fast/
source: GitHub Blog
tags:
- Engineering
- Security
- Supply chain security
- Dependabot
- GitHub Actions
- open source
- supply chain security
title: 'Tame Dependabot: Group your updates, slow the cadence, keep security fast'
---

If you maintain an active repository, you know the feeling. You open your notifications on a Monday morning and there they are: five, 10, sometimes a dozen Dependabot pull requests, each bumping a single dependency by a single patch version. Individually, every one of them is helpful. Collectively, they&rsquo;re noise. And noise is how important updates get ignored.



We looked at Microsoft&rsquo;s GCToolkit, an open source Java library for analyzing garbage collection logs. As of July 2026, a git log of the repository showed that 92 of its 578 commits, roughly one in six, were Dependabot version bumps, with 61 in the previous 12 months alone, sometimes several in a single day. That&rsquo;s a lot of review, merge, and CI cycles spent on routine maintenance.



The good news: Dependabot already ships with the features to fix this. In a recent pull request, the project changed its dependabot.yml in three small but meaningful ways, turning a daily drip of single-dependency pull requests into a predictable, grouped, monthly batch per ecosystem. Here&rsquo;s what changed, why it works, and how to apply the same pattern to your own repositories, following the GCToolkit example.



The problem: Good defaults, wrong cadence



Here&rsquo;s what GCToolkit&rsquo;s configuration looked like before:



version: 2
updates:
- package-ecosystem: github-actions
  directory: "/"
  schedule:
    interval: daily
  open-pull-requests-limit: 10



This is a common starting point, but the daily interval here was a deliberate choice, not a default: schedule.interval is required, and GitHub&rsquo;s suggested starter template uses weekly. Two things make this configuration noisy:




interval: daily tells Dependabot to check for updates every weekday (Monday through Friday). For a repository that references a handful of GitHub Actions, that can mean new pull requests landing on any weekday.



No grouping means every dependency gets its own pull request. Ten available updates equals 10 pull requests, 10 CI runs, and 10 review notifications.




The open-pull-requests-limit: 10 line is a symptom, not a cure: it caps the flood at 10 open pull requests, but it doesn&rsquo;t stop the flood.



The fix: Three changes that compound



Here&rsquo;s the configuration after the change:



version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "monthly"
    groups:
      monthly-batch:
        patterns:
          - "*"

  - package-ecosystem: "maven"
    directory: "/"
    schedule:
      interval: "monthly"
    groups:
      monthly-batch:
        patterns:
          - "*"



Three things are happening here, and they build on each other.



1. Group everything into a single pull request



The groups block is the heart of this change:



groups:
  monthly-batch:
    patterns:
      - "*"



A Dependabot group bundles multiple dependency updates into one pull request. The name (monthly-batch) is yours to choose. It shows up in the pull request title and branch name. The patterns list decides which dependencies belong to the group, and "*" is a wildcard that matches all of them.



So instead of 10 pull requests, you get one pull request titled something like &ldquo;Bump the monthly-batch group with 10 updates.&rdquo; One branch. One CI run. One review. If the whole batch is green, you merge once and you&rsquo;re done. If something breaks, it&rsquo;s contained in a single, reviewable place.



For larger projects, you don&rsquo;t have to lump everything together. You can define multiple named groups with more specific patterns. For example, you could keep all your testing libraries in one group and your production dependencies in another, so related updates travel together and unrelated ones stay separate.



Grouping keeps getting more capable, too. In a February 2026 update, Dependabot gained the ability to group updates for the same dependency across multiple directories into a single pull request. That&rsquo;s aimed squarely at monorepos: if one library is pinned in a dozen services, a single bump used to open a dozen near-identical pull requests, one per directory. Now you can point the directories key (note the plural) at a list of paths, or a glob like /apps/*, and let your group collapse all of them into one:



- package-ecosystem: "npm"
  directories:
    - "/apps/*"
  schedule:
    interval: "monthly"
  groups:
    monthly-batch:
      group-by: dependency-name
      patterns:Expand comment
        - "*"



That&rsquo;s the same monthly-batch group as before, now spanning every service in the repository instead of a single directory. For the full set of options, see the Dependabot options reference.



2. Slow the cadence from daily to monthly



schedule:
  interval: "monthly"



Switching from daily to monthly changes the rhythm from &ldquo;whenever anything changes&rdquo; to &ldquo;once, on a schedule you can plan around.&rdquo; Combined with grouping, this is the real noise reduction: Dependabot now opens one batched pull request per ecosystem, per month, instead of a steady trickle all month long.



Monthly is the right call for a mature library where dependencies are stable and updates are rarely urgent. If you want something in between, weekly is also available, and you can pin the exact day and time with schedule.day and schedule.time.



3. Cover every ecosystem you actually use



The original config only requested version updates for github-actions. But GCToolkit is a Java project built with Maven, so its application dependencies weren&rsquo;t receiving Dependabot version updates. The updated config adds a second updates entry:



- package-ecosystem: "maven"
  directory: "/"



This is an easy one to miss. Reducing noise is only half the win; the other half is making sure Dependabot is watching the dependencies that matter most. Each ecosystem gets its own schedule and its own group, so your Actions updates and your Maven updates arrive as two clean, separate batches.



But what about security updates?



This is the question every maintainer should ask before slowing anything down, and it&rsquo;s where the design really shines: by default, the groups and schedule you set here shape your version updates, not your security fixes.



Dependabot security updates are raised as soon as a vulnerability with a fix is disclosed, independent of your schedule and separate from your version-update groups. So a monthly batch cadence for routine bumps doesn&rsquo;t delay a critical patch. (You can batch security fixes on purpose with a group scoped to applies-to: security-updates, but even then they&rsquo;re triggered by disclosures, not by your version-update schedule.)



One caveat: this safety net only exists if Dependabot security updates are actually turned on for the repository, which also requires the dependency graph and Dependabot alerts to be enabled. Confirm those are on before you rely on a slower version-update cadence. Do that, and you get the best of both worlds: quiet, predictable maintenance for the routine stuff, and immediate action when a real vulnerability lands.



That separation is what makes &ldquo;slow down Dependabot&rdquo; a safe recommendation rather than a risky one.



A new safety net: default package cooldown



There&rsquo;s one more piece of noise reduction that landed recently, and it happens automatically. Dependabot now waits until a new release has been on its registry for at least three days before opening a version-update pull request. This cooldown is the default and requires no configuration.



Why wait? A brand-new release is one of the most common entry points for a supply chain attack. A compromised or simply broken version can reach your dependency updates before maintainers and the wider community have caught the problem. A short delay gives that signal time to surface, so you&rsquo;re far less likely to merge a bad release the moment it ships.



Two things worth knowing:




It only applies to version updates. Security updates still open immediately, so critical fixes are never held back by the cooldown.



You stay in control. Use the cooldown option in your .github/dependabot.yml to widen or shorten the window, tune it per semantic-versioning level, or opt out entirely:




- package-ecosystem: "maven"
  directory: "/"
  schedule:
    interval: "monthly"
  cooldown:
    default-days: 7
  groups:
    monthly-batch:
      patterns:
        - "*"



Pair cooldown with grouping and a monthly cadence and the effect compounds: fewer pull requests, and the ones you do get have had a few days to prove they&rsquo;re safe to merge.



How to apply this to your own repositories



You can adopt this pattern in a few minutes:




Open (or create) .github/dependabot.yml in the default branch of your repository.



For each package-ecosystem you depend on, set schedule.interval to weekly or monthly.



Add a groups block with a single wildcard group (patterns: ["*"]) to batch updates into one pull request per ecosystem.



Make sure every ecosystem you actually ship with is listed: not just github-actions, but maven, npm, pip, gomod, docker, and so on.



Commit, and let the next scheduled run produce a single, grouped pull request.




A few tips as you tune it:




Start broad, then split. A single wildcard group is the simplest starting point. If you later find you want, say, patch-level and major-version updates handled differently, break the wildcard into more targeted named groups.



Don&rsquo;t fold security fixes into this cadence. Dependabot security updates are triggered by vulnerability disclosures, not your version-update schedule, so a monthly cadence never delays them. You can even group them with applies-to: security-updates without slowing them down.



Lean on cooldown. The three-day default already shields you from brand-new bad releases; bump cooldown.default-days higher if you want an even wider safety margin on version updates.



Right-size the interval. Fast-moving apps may prefer weekly; stable libraries do fine on monthly.



Consolidate monorepo directories. If the same dependency lives in many directories, list them under directories and set group-by: dependency-name in the group so a single bump produces one pull request instead of one per directory.




The takeaway



Dependency updates are one of those chores that&rsquo;s easy to automate and then easy to start ignoring, which defeats the purpose. The fix isn&rsquo;t to turn Dependabot off or to merge pull requests without looking. It&rsquo;s to shape its output so that the routine work is quiet and batched, and the urgent work still cuts through.



GCToolkit did it with about a dozen lines of YAML: group everything, slow the cadence to monthly, and make sure every ecosystem is covered. Add the new default cooldown on top, and even that monthly batch has had a few days to prove itself before it reaches you. The result is fewer pull requests, fewer CI runs, and, most importantly, a review queue where the updates that matter don&rsquo;t get lost in the ones that don&rsquo;t.



Further reading: once the routine pull request noise is under control, the harder question is which security alerts to fix first. Our earlier post, Cutting through the noise: How to prioritize Dependabot alerts, walks through using EPSS scores and repository properties to turn an overwhelming alert list into a clear, risk-ranked queue.




Configure your own Dependabot updates &gt;


The post Tame Dependabot: Group your updates, slow the cadence, keep security fast appeared first on The GitHub Blog.

---
*원문: [https://github.blog/security/supply-chain-security/tame-dependabot-group-your-updates-slow-the-cadence-keep-security-fast/](https://github.blog/security/supply-chain-security/tame-dependabot-group-your-updates-slow-the-cadence-keep-security-fast/)*
