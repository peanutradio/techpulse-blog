---
categories:
- MS
- GitHub
date: '2026-08-25T21:35:11+00:00'
description: 'A language model can perform well on a clean benchmark and still struggle
  with the cases that matter in production.




  Benchmarks and curated datasets are usefu'
draft: false
original_url: https://github.blog/ai-and-ml/llms/how-to-evaluate-llms-before-production/
source: GitHub Blog
tags:
- AI & ML
- LLMs
- Secret Scanning
title: How to evaluate LLMs before production
---

A language model can perform well on a clean benchmark and still struggle with the cases that matter in production.



Benchmarks and curated datasets are useful when prototyping an LLM-based system. They help teams compare models, test an initial prompt, and determine whether an idea is technically plausible.



But as a system moves closer to production, the evaluation problem changes.



Real inputs are often ambiguous. Labels may be inconsistent. Important context may be missing or truncated. The evaluation set may not reflect the production distribution. Edge cases that rarely appear in benchmarks can become common sources of failure. Even when offline metrics improve, those results may not translate cleanly into production behavior.



We encountered these challenges while evaluating an LLM-based system designed to reduce false positives in GitHub secret scanning.



Secret scanning identifies credentials such as tokens and keys that may have been committed to a repository. Because some candidate strings resemble secrets, but don&rsquo;t actually represent real credentials, developers may spend time investigating alerts that don&rsquo;t require remediation.



Rather than determine whether an LLM could classify a string correctly, we needed to understand whether the system could reduce noisy alerts while preserving enough recall to remain safe for a security workflow.



In this post, we share the practices that helped us move from promising prototype results to production. The lessons apply broadly to LLM-powered systems in code analysis, developer tools, security, data analysis, and other production workflows.







1. Start with the product decision, not the model



When an LLM system doesn&rsquo;t perform as expected, the first instinct is often to adjust its technical components.



Teams may rewrite the prompt, add context, introduce another reasoning step, adjust the surrounding pipeline, or switch models. Before making any of these changes, they should define the decision the evaluation is meant to support.



For our secret-scanning work, we asked:



Can the system reduce false positives while preserving enough recall to be safe in a production security workflow?



To answer this question, teams must decide which mistakes are acceptable, which metrics should drive the product decision, and which guardrails must remain within their defined thresholds.



In secret scanning, incorrectly suppressing a real credential can be more consequential than asking a developer to review an additional alert. We therefore did not treat precision and recall as equally interchangeable metrics.



Our primary objective was to reduce false positives and improve precision. Recall served as a safety constraint: an experiment could advance only if any decrease remained within a predefined acceptable range. This gave us a clear way to evaluate tradeoffs. We selected the configuration that achieved the strongest false-positive reduction while satisfying the recall requirement and meeting our operational guardrails.



We organized the evaluation criteria into three levels:



Primary outcome



This measured the user benefit we were trying to improve:




False-positive reduction



Precision




Safety constraint



This prevented an apparent improvement from introducing unacceptable security risk:




Recall




Operational guardrails



These determined whether the result was practical to deploy:




Latency



Cost



Reliability



Production compatibility




This distinction prevented us from treating every metric as interchangeable. A change that reduced false positives but significantly lowered recall wasn&rsquo;t automatically an improvement. Neither was a change that improved quality while making the system too slow, expensive, or difficult to integrate.



Consider two hypothetical experiment results:



Experiment&nbsp;Precision&nbsp;Recall&nbsp;Latency&nbsp;Decision&nbsp;Experiment A&nbsp;Large improvement&nbsp;Falls below the safety guardrail&nbsp;Acceptable&nbsp;Don&rsquo;t&nbsp;advance&nbsp;Experiment B&nbsp;Moderate improvement&nbsp;Remains within the guardrail&nbsp;Acceptable&nbsp;Continue testing&nbsp;



Experiment A may look stronger if precision is viewed in isolation. Experiment B is more aligned with the product goal because it improves the developer experience without violating the recall guardrail.



Before evaluating an LLM system, decide what success means for the user and which guardrails the system must respect. We want to generate evidence that supports a product decision.



2. Treat offline evaluation like integration testing



An LLM-based system continues to change after its first successful evaluation, so evaluation should not be a one-time exercise. Teams revise prompts, adopt new models, change how inputs and context are constructed, and refine the surrounding business logic.



Any of these changes can improve the system, introduce a regression, or shift its behavior in an unexpected way.



For that reason, we treated offline evaluation similarly to an end-to-end integration test. We reran it whenever we made a meaningful change to the prompt, model, input construction, or broader system logic.



The evaluation also needed to be repeatable enough that each new result could be compared against a known baseline. For every run, we recorded the prompt, model, dataset version, and system configuration.



This made it possible to answer questions such as:




Did the new prompt improve precision without reducing recall?



Did the model upgrade help across the dataset or only within certain categories?



Did a change to the input or context fix one error pattern while introducing another?



Did a change to the surrounding logic improve the result consistently, or simply shift where errors appeared?




Without this discipline, teams can easily compare results generated under different conditions and attribute an improvement to the wrong change.



Change one major variable at a time



Repeatability alone is not enough. Experiments also need to be designed so that the cause of a result is clear.



We changed one major variable at a time and compared each run against a known baseline. For example, we evaluated a prompt revision separately from a model upgrade before testing the two together.



This mattered because even small prompt changes could shift model behavior, while a model upgrade could affect quality, cost, latency, or output consistency. If both changed in the same experiment, we would not know which one caused the improvement or regression.



We treated prompts and evaluation configurations like code. We versioned them, recorded what changed, kept previous configurations reproducible, and made rollback possible.



Run ID&nbsp;Prompt version&nbsp;Model version&nbsp;Precision&nbsp;Recall&nbsp;Latency&nbsp;Notes&nbsp;R-001&nbsp;v1&nbsp;Model A&nbsp;0.71&nbsp;0.78&nbsp;1.2s&nbsp;Baseline&nbsp;R-002&nbsp;v2&nbsp;Model A&nbsp;0.75&nbsp;0.77&nbsp;1.2s&nbsp;Prompt-only change&nbsp;R-003&nbsp;v1&nbsp;Model B&nbsp;0.74&nbsp;0.80&nbsp;1.0sModel-only change&nbsp;



The values in the evaluation run tracking table above shown are hypothetical and included only to illustrate how evaluation runs can be tracked and compared.



Test model upgrades regularly



When an LLM system underperforms, developers often respond by adding more instructions to the prompt. Sometimes that helps, but not always. For example, the prompt may be carrying complexity that comes from the model itself.



A stronger model may perform better with a simpler prompt than an older model does with extensive tuning. Simpler prompts are also easier to understand, test, and maintain.



Model upgrades still need careful evaluation. A new model may improve performance in one category while introducing regressions elsewhere. It may also affect cost, latency, output formatting, or compatibility with the existing pipeline.



The evaluation process should be inexpensive and repeatable enough that testing a new model becomes routine. Any meaningful change to the prompt, model, or pipeline should go through offline evaluation before reaching production.



3. Keep offline evaluation close to production



An offline evaluation is only useful when it resembles the task the system will perform in production.



In a secret-scanning workflow, the model is rarely evaluating one clean, isolated value. It may need to assess a specific candidate alongside surrounding code and other information that is relevant, incomplete, or potentially distracting. Differences in how that information is presented can materially affect the result.



Our offline evaluation therefore needed to preserve the important characteristics of the production task, including:




The candidate being evaluated



The surrounding context available to the model



Relevant supporting information



The way inputs are formatted and constrained



The broader system logic around the model




Even small differences can skew the results. A cleaner dataset may exclude ambiguous cases, provide more complete context, or remove nearby values that could distract the model.



Consider a simplified example:



example_token = "sample_value_for_documentation" 
production_api_key = get_secret_from_environment() 
candidate_value = "flagged_value"



Suppose candidate_value is the value the system is expected to assess. The model may instead focus on example_token because its variable name appears more security-relevant, producing a plausible explanation about the wrong value.



This kind of failure is easy to miss when evaluation examples contain only one obvious candidate. It surfaced because the offline evaluation preserved some of the ambiguity and distractions found in real secret-scanning workflows.



The closer the offline pipeline is to the production pipeline, the more useful the evaluation becomes. When the two differ, a strong offline score may simply reflect an easier problem than the one being deployed.



4. Treat production labels as signals, not unquestionable truth



Production data can make an evaluation more representative, but its labels often capture workflow outcomes rather than reliable ground truth. A dismissed or resolved secret-scanning alert, for example, does not necessarily represent a false positive.



A developer might resolve an alert because:




The credential was rotated



The risk was accepted



The alert needed to be cleared to unblock a workflow



The alert was incorrectly classified




These outcomes may look similar in product data while representing different ground-truth states.



Before using production labels, ask:




How was the label created?



Does it match the question the evaluation is trying to answer?



Are different workflow outcomes being grouped into the same category?




For important or ambiguous subsets, you may need to complete a manual review. You&rsquo;re not trying to eliminate every imperfect label, but you need to make sure the evaluation data is accurate enough to support the decision being made.



5. Use synthetic and open datasets to fill coverage gaps



Representative production data may be limited, sensitive, or unavailable early in development. Synthetic examples, academic benchmarks, and open datasets can help developers bootstrap an evaluation and expand coverage, but these examples should supplement rather than stand in for production-like data.



With that in mind, synthetic examples can greatly help fill in the gaps for testing cases that are rare or difficult to collect, such as ambiguous inputs, missing context, unusual formatting, and underrepresented failure patterns. A list of credential strings, for example, can test whether a model recognizes common formats, but it cannot fully evaluate how the model reasons about a candidate within real code.



We adapted external examples to match our task and reviewed labels that did not align with our product definition. We also used realistic failure patterns to create targeted synthetic cases involving nearby credential-like values, test code, placeholders, indirect references, and missing context.



6. Use error analysis to find what aggregate metrics hide



Aggregate metrics tell you whether a system improved overall. Error analysis tells you what to change next.



A higher precision score doesn&rsquo;t reveal whether the remaining errors come from ambiguous inputs, poor prompt framing, missing context, noisy labels, or a narrow dataset.



To understand those problems, inspect the failures.



We reviewed samples of false positives and false negatives and grouped them by their likely source: the model, prompt, input, pipeline, dataset, or label. The recurring issues included several already discussed, such as reasoning about the wrong candidate, missing context, and labels that did not match the evaluation definition.



Each category suggested a different response. Reasoning about the wrong value pointed to prompt or input framing, missing evidence pointed to context construction, and incorrect labels required data cleanup. Repeated domain-specific ambiguity could indicate the need for a clearer product policy or a dedicated evaluation category.



Manually reviewing dozens or hundreds of examples takes time, but it often leads to faster progress. Once a recurring failure pattern is clear, the team can make a targeted change and measure whether it solved the problem.



A useful question for each error is: Did this failure come from the model, prompt, input, pipeline, dataset, or label?



That classification turns a vague quality problem into a concrete engineering task.



7. Use LLM-as-judge to focus human review



Reviewing every evaluation example manually may not scale. LLM-as-judge can reduce that burden by classifying clear cases, identifying potentially mislabeled examples, and prioritizing ambiguous cases for human review. Because the judge can also make mistakes or agree with another model for the wrong reason, its output should be treated as another prediction rather than ground truth.



A safer pattern is to use the judge for triage:




Automatically process clear, low-risk cases.



Route low-confidence, conflicting, or high-impact cases to human reviewers.



Periodically sample high-confidence cases to check for systematic errors.



Track disagreement between the judge, the evaluated system, and human reviewers.



Version and evaluate the judge prompt like any other model component.




Used this way, the judge concentrates human attention on the cases where review is most likely to change the outcome.







8. What secret scanning taught us



Our goal was to reduce false positives while preserving recall in a security-sensitive workflow. Offline evaluation gave us a controlled way to compare prompt, model, input, and pipeline changes before beginning online experimentation.



Through repeated evaluation and targeted error analysis, we reached a 95% reduction in false positives on the evaluated offline dataset while keeping recall within our defined guardrail. More importantly, we understood how the result had been produced: the evaluation reflected the production task more closely, changes were measured against reproducible baselines, and the remaining failure patterns were documented.



Offline evaluation did not prove how the system would behave in every production scenario. It provided enough structured evidence to justify moving to online experimentation with clearly understood risks and guardrails.



Checklist: Before moving an LLM system toward production



Use this checklist to assess whether your evaluation provides enough evidence to move the system forward. Work through each section to confirm that the goals, data, experiments, and remaining production risks are clearly understood.



Product Goals




Is the product decision and primary success metric clear?



Are the safety and operational guardrails defined?




Data and Labels




Does the evaluation data resemble the production workflow and include difficult cases?



Do we understand how the labels were created and where human review is needed?




Evaluation Rigor




Are the prompt, model, dataset, and pipeline versions recorded?



Are major changes isolated and compared against a known baseline?




Error Analysis and Production Readiness




Have false positives and false negatives been reviewed by category?



Can we rerun the evaluation and explain where offline results may differ from production?




Evaluate before you trust



As LLM-based systems move into production, evaluation should become part of the regular engineering workflow. A strong offline evaluation can show whether the product goal has been met under representative conditions, where uncertainty remains, and whether the system is ready for a controlled production rollout.



Production uncertainty is unavoidable. Evaluation makes it visible, measurable, and manageable.




Explore secret scanning documentation &gt;


The post How to evaluate LLMs before production appeared first on The GitHub Blog.

---
*원문: [https://github.blog/ai-and-ml/llms/how-to-evaluate-llms-before-production/](https://github.blog/ai-and-ml/llms/how-to-evaluate-llms-before-production/)*
