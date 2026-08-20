---
schema_version: "1.0.0"
document_id: "998e861904909698f553cb52b03ffed16d14ad6caa8fe5cb3d3a427b97f3ab5b"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-5eeb9d1ed7ff"
canonical_url: "https://devblogs.microsoft.com/blog/not-all-model-upgrades-are-upgrades/"
published_at: "2026-07-06T07:49:44+00:00"
first_seen_at: "2026-07-20T04:34:25.944233+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:dda5802c5fb8f745c36916d139310c72deea2b7b30650d7a54c6c75e59be1876"
---

# Not all model upgrades are upgrades

A new model drops with lower per-token pricing and better benchmarks. You switch. A week later someone asks why the agent is burning 12x more tokens on the same task while producing worse output.


We ran 150 agent tasks across 15 scenarios on two models, Claude Sonnet 4.6 and Claude Sonnet 5, using GitHub Copilot Chat in VS Code on Windows. The scenarios covered two types of work: architecture and design tasks grounded in Microsoft Learn documentation, and SharePoint Framework project upgrades. Sonnet 5 is the newer model with 33% lower per-token pricing across every token category. The assumption we wanted to test: newer and cheaper means better. What we found was more complicated.


## Cheaper tokens, higher bills


Sonnet 5 is cheaper per token across the board. Here’s how the rate cards compare:


Sonnet 4.6 Sonnet 5


Input (per 1M tokens) $3.00 $2.00


Cached input $0.30 $0.20


Output $15.00 $10.00


In such a comparison, Sonnet 5 wins every line. But rate cards don’t determine your bill: token consumption does, and Sonnet 5 consumes substantially more tokens.


On architecture tasks (12 scenarios, 60 runs per model), Sonnet 5 used 12x more tokens at the median. One scenario saw a single run consume 47x the typical volume. On code upgrades (3 scenarios, 15 runs per model), the gap hit 10x. A 33% per-token discount doesn’t survive that kind of increase.


What this costs you in dollars depends on the task. On code upgrades, Sonnet 5 cost $2.01 per run versus $0.55 for Sonnet 4.6, making the “cheaper” model 3.7x more expensive. On architecture tasks, the story flipped: Sonnet 5 averaged $0.47 per run versus $0.54, making it 12% cheaper where the token increase was moderate enough for the discount to win out. **You won’t know which direction your workload goes until you measure it.**


## Quality didn’t improve either


The newer model might cost more or might cost less, depending on the task. Does it at least produce better output? On architecture work, based on our evals, no.


Both models completed the task at the same rate, 75% on our Select gate (did the agent attempt the right task at all?). Where they differed was output quality. On the 9 scenarios where both produced usable output, Sonnet 4.6 scored 90% on our Idiomatic dimension (does the output follow established patterns and conventions?) versus 78% for Sonnet 5. The older model outperformed or matched quality in 8 of 9 scenarios.


One scenario, designing an IoT analytics architecture, showed the gap most clearly. Both models completed the task every time, but Sonnet 4.6 passed Idiomatic checks in 4 out of 5 runs. Sonnet 5 managed 1. Same prompt, measurably worse output.


More tokens and worse quality on the majority of scenarios. The “upgrade” went in the wrong direction.


## When the upgrade actually matters


Code upgrade tasks reversed the picture. We tested three[SharePoint Framework](https://learn.microsoft.com/sharepoint/dev/spfx/sharepoint-framework-overview) (SPFx) project upgrade scenarios, including the gulp-to-Heft migration and the legacy-to-flat ESLint config migration. Sonnet 4.6 passed the Select gate in 60% of runs. Sonnet 5 passed 100%.


The starkest example was upgrading from SPFx v1.21.1 to v1.22.0. Sonnet 4.6 failed all 5 runs, consistently adopting version 1.22.1 from Microsoft Learn documentation instead of the user-requested 1.22.0. Sonnet 5 followed the instruction precisely every time. When the task requires the agent to follow a specific instruction over what it finds in its context, the newer model was more reliable.


Sonnet 5 also showed a willingness to dig deeper. One run consumed 69 million tokens and met 21 out of 30 evaluation criteria by performing extensive web fetching and discovering undocumented migration steps. Sonnet 4.6 never attempted that depth. The thing is though, that you can’t count on this. 4 out of 5 runs in each scenario didn’t reach that depth either. The breakthrough run is real but not reproducible.


## The real ceiling is content, not the model


Both models hit the same quality ceiling on code upgrades. Configuration correctness was 0% across all SPFx scenarios for both models. Neither model completed the structural toolchain migration, from gulp to Heft or from legacy ESLint to flat config.


Migration guides typically cover dependency bumps and API surface changes. Structural shifts like switching build systems or config formats involve steps that aren’t documented in one place. In our SPFx scenarios, we identified seven specific file and configuration changes that neither model could discover on its own: build tool flags, package manifest restructuring, file deletions, config format migrations. All concrete, all enumerable, all absent from the documentation.


**Neither model can discover what isn’t documented. Spending more on a newer model doesn’t fix a content gap.**


This reinforces what we’ve observed across every engagement we’ve run. The agent can only use what it can find. When the grounding content is incomplete, the model version doesn’t matter. We covered this in[the AX stack](https://developer.microsoft.com/blog/the-ax-stack-whats-fixed-where-you-can-win) , and the pattern keeps showing up.


## Variance as a cost risk


Token consumption wasn’t just higher on Sonnet 5. It was also unpredictable.


Sonnet 4.6 runs clustered tightly, most between 14K and 45K tokens for straightforward architecture tasks. Budgeting is easy when your runs are consistent.


Sonnet 5 runs were all over the map. On that same IoT analytics scenario, one run consumed 6.6 million tokens while another consumed 16,000. Same scenario, same prompt. A single outlier run can cost more than an entire batch of Sonnet 4.6 runs, which makes your costs unpredictable at any scale.


Both models had high variance on complex tasks. Sonnet 4.6’s outliers were occasional though. Sonnet 5’s were the norm.


## What we measured and how


We ran two benchmarks through our evaluation platform: 12 Azure architecture scenarios (120 runs) and 3 SharePoint Framework upgrade scenarios (30 runs). Five runs per model per scenario, 150 total.


Each run was evaluated against binary criteria organized into a Select gate and quality dimensions, scored by an LLM judge calibrated for consistency. Costs were computed from actual per-turn token data, priced against GitHub Copilot’s published rates.


Here’s the full picture:


Metric Sonnet 4.6 Sonnet 5


**Architecture tasks**


Task completion 75% 75%


Output quality 90% 78%


Avg. cost per run $0.54 $0.47


**Code upgrades**


Task completion 60% 100%


Config correctness 0% 0%


Avg. cost per run $0.55 $2.01


## What this means for you


A model upgrade is a hypothesis, that newer means better for your specific tasks and your specific content. Ethan Mollick describes AI capabilities as a[jagged frontier](https://www.oneusefulthing.org/p/centaurs-and-cyborgs-on-the-jagged) : the boundary between what AI can and can’t do is uneven, and tasks that seem equally difficult end up on different sides of it. Model upgrades are jagged in the same way. Sonnet 5 jumped from 60% to 100% task completion on code upgrades while architecture quality dropped from 90% to 78%. You won’t know which side of the frontier your workload falls on until you measure it.


If you’re a platform team evaluating whether to recommend a newer model, or an enterprise team deciding which model to provision, start with your workload. On routine architecture and design work with well-documented domains, the older model delivered equal or better quality at comparable cost. On tasks requiring precise instruction following, the newer model had a genuine edge.


The workload mix determines which model is the better default. If you’re running at scale, token variance matters too: the older model’s tight clustering makes budgeting predictable, while the newer model can swing from 16K to 6.6 million tokens on the same prompt.


But before you spend time evaluating model versions at all, check whether the agent has the information it needs to do the job. We’ve seen[agent extensions](https://developer.microsoft.com/blog/the-ax-stack-whats-fixed-where-you-can-win) produce more lift per dollar than model upgrades, because they fix the information gap that both models share. If your documentation has holes, no model switch closes them. Measure first, upgrade second.
