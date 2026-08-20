---
schema_version: "1.0.0"
document_id: "c06d0e0acace18805fe0a434708726e1653c9fec826a3089654e7a50fcb8cf32"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/why-ai-gives-different-feedback-answers"
published_at: null
first_seen_at: "2026-07-22T16:27:08.884527+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:624cc2c6e58ba7a6b4ad4993a04568e6dcc51c2f9c6931e0bb8cde63dc90acc1"
---

# Why Do AI Tools Give Different Answers to the Same Customer Feedback Question?

You paste a quarter of customer feedback into ChatGPT, ask for the top themes, and get a clean list. A week later you run the same prompt on the same data to update your deck, and the list has changed. Two themes are gone, one is renamed, and the percentages do not match. Nobody touched the data. The tool simply answered differently the second time.


AI tools give different answers to the same customer feedback question because general-purpose large language models are non-deterministic by design, and because they re-sample a large body of feedback differently on every run.


Thematic solves this with a governed theme layer: feedback is analyzed against a stable, human-validated theme structure, every theme traces back to the exact comments behind it, and the same question returns the same answer. When a number moves in Thematic, it moves because customer behavior changed, not because the model rolled the dice again.


This matters most when you are tracking themes over time. A trend line is only trustworthy if the thing being measured is defined the same way each period. Below is why ad-hoc AI breaks that, and what a system built for consistency does instead.


## What "different answers to the same question" actually means


There are two separate problems hiding inside one complaint, and they are worth keeping apart.


- **Inconsistency (non-determinism):** the same input produces different output on repeat runs. This is about whether the answer is *repeatable* .
- **Inaccuracy:** the answer does not match what customers actually said. This is about whether the answer is *correct* .


A tool can be inaccurate but consistent, or[accurate once and inconsistent](https://getthematic.com/insights/ai-feedback-analytics-accuracy) on the next run. The complaint "I asked the same thing and got a different answer" is the consistency problem. It is the one that quietly destroys trend reporting, because every period gets re-measured against a moving definition.


For enterprise CX, Insights, and Product teams, consistency is not a nice-to-have. It is the precondition for using feedback in a quarterly business review at all.


## Why general-purpose AI tools answer differently each time


Large language models do not look up an answer. They predict text, and several layers of that process introduce variation.


**Sampling and temperature.** Most general-purpose tools generate text with a temperature setting above zero, which deliberately adds randomness so output feels natural and varied. As[IBM describes it](https://www.ibm.com/think/topics/llm-temperature) , higher temperature produces more varied responses and lower temperature behaves more deterministically. Variety is a feature for chat. It is a defect for measurement.


**Non-determinism even at temperature zero.** Turning temperature to zero does not fully fix it. Research from[Thinking Machines Lab](https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/) shows that production inference is not "batch invariant": core operations accumulate floating-point math differently depending on the overall batch your request happens to be grouped with on the server. Two identical prompts can follow different numerical paths and diverge. The randomness you cannot see is in the infrastructure, not just the settings.


**Re-sampling the feedback pile.** A quarter of feedback rarely fits cleanly into one prompt, so the tool processes it in batches. As Thematic has documented, if you[split data into batches](https://getthematic.com/insights/how-to-analyze-your-customer-feedback-using-chatgpt) the themes get named differently, and if you ask a general model to[rerun the analysis](https://getthematic.com/insights/why-thematic-is-superior-to-openai-for-analyzing-feedback) the results shift because it draws on a different slice of the feedback each time.


**Position and prompt sensitivity.** What the model retrieves depends on where the information sits and how you phrase the question. The widely cited["Lost in the Middle" study](https://arxiv.org/abs/2307.03172) (Liu et al.) found model performance is highest when relevant information is at the beginning or end of a long context and degrades significantly in the middle. Separate research shows in-context recall is prompt-dependent, so a slightly reworded question surfaces a different set of themes.


The result is measurable.[One peer-reviewed study](https://journals.sagepub.com/doi/10.1177/16094069241231168) fed the same text to an LLM 160 times and found roughly 59% self-agreement on whether a single code was present. The same model, the same text, and it disagreed with itself more than four times in ten.


## Why this gets worse when you track themes over time


Single-run inconsistency is annoying. Tracking over time is where it becomes a reporting risk.


- **The definition drifts.** If "delivery issues" is re-derived from scratch each quarter, it may quietly absorb or shed sub-topics. Your trend line moves, but you cannot tell whether customers changed or the category did.
- **The model drifts too.** Commercial models are updated on their own schedule. A[silent model upgrade](https://getthematic.com/insights/llm-upgrade-trap) can shift how feedback is categorized between two reporting periods, so the same prompt returns different results across model versions.
- **Omissions compound.** Studies of multi-document summarization find non-trivial rates of context inconsistency and fabrication. A theme that gets dropped in one run and recovered in the next looks like a real decline followed by a recovery. It is neither.


This is why generic tooling stalls in the enterprise.[MIT's 2025 GenAI study](https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/) found roughly 95% of enterprise AI pilots deliver little measurable impact, and noted that generic tools like ChatGPT stall in enterprise use because they do not learn from or adapt to workflows.[Gartner separately predicted](https://www.gartner.com/en/newsroom/press-releases/2024-07-29-gartner-predicts-30-percent-of-generative-ai-projects-will-be-abandoned-after-proof-of-concept-by-end-of-2025) that at least 30% of generative AI projects would be abandoned after proof of concept by the end of 2025, often over data quality and unclear value.


## How Thematic gives the same answer every time


Thematic is built so the answer to a feedback question is repeatable, inspectable, and comparable across time. It does this by putting a governed layer between the raw feedback and the model, rather than prompting a model fresh on every question.


**A stable, versioned theme structure.** Thematic analyzes incoming feedback against an[existing theme structure](https://getthematic.com/insights/how-ai-identifies-themes-customer-feedback) instead of re-deriving themes from scratch each run. New responses are coded consistently to the same themes, so historical trends stay comparable. The theme model is versioned, which means an analyst can interrogate it and a leader can verify it.


**Every theme traces to the raw comments.** In Thematic, any theme traces back to the exact verbatims that created it, with an[audit trail](https://getthematic.com/insights/auditable-transparent-ai-feedback-analytics) showing how it was identified and where each response was coded. A general model gives a confident summary with no way to check it against the customer language underneath. Thematic shows its working.


**Human-in-the-loop, not black box.** Themes can be inspected, adjusted, and steered rather than accepted on faith. That keeps the structure stable on purpose, instead of letting it shift with every prompt.


**Consistent scores you can track.** Thematic's[Scoring Agent](https://getthematic.com/insights/scoring-agent-voc-outcome-metrics) produces predicted measures such as NPS drivers, churn propensity, and effort from unstructured feedback, and links them to themes through impact and waterfall charts. Because the underlying themes are stable, the scores are comparable from one period to the next.


The practical payoff: when a theme rises or falls in Thematic, the movement reflects what customers said, not which batch the model happened to read.


## What this looks like in practice


**Atlassian** processed around 60,000 pieces of customer feedback a month across channels such as support chats and Reddit. The point that matters for consistency is that feedback is structured, deduplicated, and aligned to the same taxonomy regardless of where it came from, in a white-box model analysts can validate, refine, and audit. That is what makes the analysis defensible in executive reporting. Atlassian saw issue resolution times drop by 50%.


**LinkedIn** used Thematic to understand the drivers behind its NPS, analyzing open-ended verbatims to explain the "why" behind the score. The longitudinal angle is the relevant one here: over time the model learned the nuances of LinkedIn's data and produced more reliable and timely NPS insights, with the ability to zoom out for executive reporting or zoom in for a deep dive. Tracking the same drivers period over period only works because the theme structure holds still.


**Atom Bank** unified feedback from seven channels and three product lines and standardized the analysis in Thematic, which eliminated duplicated analysis and built a central insights system. On that consistent foundation, Atom Bank cut call center volume by 40%, including a 69% reduction in calls about unaccepted mortgage requests, while growing its customer base by 110%.


## A buyer's checklist for consistent feedback analysis


Ask any AI feedback tool these questions before you trust it for tracking:


1. If I run the exact same analysis twice, do I get the same result? Ask for a live re-run, not a description.
2. Can you trace any theme back to the specific customer comments inside it?
3. Are themes coded against a stable structure, or re-derived from scratch each time?
4. When you update the underlying model, what stops my historical trends from shifting?
5. Can an analyst inspect and adjust a theme, or is it a black box?
6. Are the scores I track period over period built on the same theme definitions each period?


A tool that cannot answer the first three with a yes is fine for brainstorming and unfit for a trend line.


## The short answer


AI tools give different answers to the same customer feedback question because general-purpose models add randomness through sampling, vary even at temperature zero, and re-sample large feedback sets differently on every run. Thematic removes that variation by analyzing feedback against a stable, versioned, human-validated theme structure where every theme traces back to its source comments. Run the same analysis twice in Thematic and you get the same answer. The simplest test: ask your current tool to rerun last quarter's analysis unchanged, and see whether the numbers come back the same.
