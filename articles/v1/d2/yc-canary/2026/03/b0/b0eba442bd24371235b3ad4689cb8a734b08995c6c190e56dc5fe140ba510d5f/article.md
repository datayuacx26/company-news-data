---
schema_version: "1.0.0"
document_id: "b0eba442bd24371235b3ad4689cb8a734b08995c6c190e56dc5fe140ba510d5f"
company_key: "yc-canary"
company: "Canary"
source_id: "yc-canary-news-import-4de951242b25"
canonical_url: "https://runcanary.ai/blog/qa-bench-v0"
published_at: "2026-03-09T00:00:00+00:00"
first_seen_at: "2026-07-21T12:13:40.969791+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:9bd8d9a14513a9e8e0509c30b91030faa8aba1743fff1d380f6bb3a82f790b4f"
---

# QA-Bench v0: Measuring How AI Models Handle Code Verification

TL;DR


We built **QA-Bench v0** , an early evaluation for a task no existing benchmark measures: given a real pull request on a production codebase, can an AI model identify every affected user flow and generate relevant tests?


We tested a **purpose-built QA agent** against **GPT 5.4** , **Claude Code (Opus 4.6)** , and **Sonnet 4.6** across **35 PRs** on four production-scale open-source repos.


**The biggest finding:** general-purpose models write well-structured tests but consistently miss the user flows a PR actually touches. **Coverage** , the metric that measures whether you tested everything that changed, is where purpose-built QA agents pull away. GPT 5.4 is the strongest general-purpose baseline but still trails by **11 points on Coverage** . All three general-purpose models score higher on Coherence.


Full methodology, rubric, and PR list published below.


Overall scores


83.1


Canary


80.2


GPT 5.4


78.0


Claude Code (Opus 4.6)


73.2


Sonnet 4.6


## Why we built this


At Canary we build QA agents that read codebases, generate tests, and execute them. We needed a way to measure whether our system was actually working on real pull requests. We looked for an existing evaluation and couldn't find one.


Existing benchmarks (SWE-bench, Aider, LiveCodeBench) measure code generation. None measure code verification: **given a change to a codebase, can you figure out what might break?** That requires reading the PR, understanding which components are affected, mapping user flows, and generating tests that target the actual changes. We built QA-Bench v0 as an early attempt at measuring this.


---


## What we're measuring


We evaluate on three criteria. Each one maps to a distinct failure mode we observed while building and testing QA agents.


**Relevance:** Are the generated tests targeting the PR's changes, or testing unrelated functionality? The most common failure mode: a model sees a PR touching a drag-and-drop component and generates generic drag-and-drop tests rather than tests for the specific behavior the PR modified.


**Coverage:** Did the agent identify all critical user flows affected by the change? A PR that modifies a shared component might affect three user journeys. Testing one and missing two is a coverage failure. This is the metric most directly correlated with preventing regressions.


**Coherence:** Are the test steps clear, complete, and actionable? We consider this less important than the other two: a test that's slightly rough but targets the right thing is more valuable than a polished test targeting the wrong thing. A higher-level test plan that identifies the right flows is sufficient at generation time — intermediate steps and assertions can be resolved during test execution.


**Overall** is the simple average of all three metrics.


Scores by metric


Relevance


Canary


87.4


GPT 5.4


82.8


Claude Code (Opus 4.6)


83.2


Sonnet 4.6


77.2


Coverage


Canary


84.5


GPT 5.4


73.2


Claude Code (Opus 4.6)


66.3


Sonnet 4.6


58.8


Coherence


Canary


77.4


GPT 5.4


84.5


Claude Code (Opus 4.6)


84.5


Sonnet 4.6


83.7


Canary


GPT 5.4


Claude Code (Opus 4.6)


Sonnet 4.6


---


## Dataset


**Repos.** We selected four open-source repositories with complex frontend user flows:


- **Grafana** — dashboards, data visualization, panel editing (15 PRs)
- **Mattermost** — team messaging, channels, thread management (8 PRs)
- **Cal.com** — scheduling, booking flows, event management (6 PRs)
- **Apache Superset** — data exploration, charting, SQL workflows (6 PRs)


These are production-scale applications with thousands of contributors, complex state management, and real user flows that break in non-obvious ways.


**PRs.** 35 total. We filtered for recent merged pull requests with frontend-affecting changes across some of the most widely-used open-source repositories that ship a frontend webapp we can run tests against. We excluded backend-only changes, dependency bumps, and documentation PRs. PR selection was not cherry-picked for any agent — the filter was applied before any agent outputs were generated.


---


## Agents & Judge


**Agents.** All agents were given identical access: full repository context and the PR diff.


- **Canary:** Our purpose-built system. It first analyzes the PR to identify all affected components, then maps user flows touching those components, then generates tests per flow via specialized subagents. Critically, Canary is built to take deterministic actions at each step and figure out missing context during execution rather than relying on one-shotting the entire test sequence during generation. The output is high-level test plans with user journey steps.
- **GPT 5.4:** Given the full repo and PR context, asked to analyze affected components and generate tests. Single-pass generation.
- **Claude Code (Opus 4.6):** Using Claude Code's agentic loop with multi-turn tool use, file exploration, and iterative generation.
- **Sonnet 4.6:** Given the full repo and PR context, asked to analyze affected components and generate tests. Single-pass generation.


**Judge.** Opus 4 with access to the full repository and PR changes. All agent outputs were anonymized before evaluation, labeled Agent A, B, C, D with randomized ordering per PR. The judge scores each agent's output against the actual PR changes on the three criteria above. V1 will move to human evaluation with binary pass/fail scoring.


---


## What we see in the data


Relevance Coverage Coherence Overall


Canary 87.4 84.5 77.4 83.1


GPT 5.4 82.8 73.2 84.5 80.2


Claude Code (Opus 4.6) 83.2 66.3 84.5 78.0


Sonnet 4.6 77.2 58.8 83.7 73.2


### Coverage is where purpose-built agents pull away


Across all four repos, Canary scores 84.5 on Coverage compared to 73.2 for GPT 5.4, 66.3 for Claude Code, and 58.8 for Sonnet 4.6. The gap is consistent: Grafana (83.5 vs 72.4 vs 65.7 vs 61.3), Mattermost (84.7 vs 70.0 vs 61.8 vs 60.8), Cal.com (86.1 vs 77.3 vs 65.6 vs 64.6), and Superset (83.5 vs 73.2 vs 72.1 vs 48.3).


Coverage by repository


Grafana 15 PRs


Canary


83.5


GPT 5.4


72.4


Claude Code (Opus 4.6)


65.7


Sonnet 4.6


61.3


Mattermost 8 PRs


Canary


84.7


GPT 5.4


70.0


Claude Code (Opus 4.6)


61.8


Sonnet 4.6


60.8


Cal.com 6 PRs


Canary


86.1


GPT 5.4


77.3


Claude Code (Opus 4.6)


65.6


Sonnet 4.6


64.6


Superset 6 PRs


Canary


83.5


GPT 5.4


73.2


Claude Code (Opus 4.6)


72.1


Sonnet 4.6


48.3


Canary


GPT 5.4


Claude Code (Opus 4.6)


Sonnet 4.6


The mechanism is straightforward. General-purpose models approach test generation as a single pass: read the PR, identify the most obvious change, generate tests for it. They typically produce a few well-crafted tests for the primary flow and stop. Our agent's architecture forces a different process: first identify all affected components, then map every user flow touching those components, then generate tests per flow. This systematically surfaces secondary effects, edge cases, and cross-component impacts that single-pass generation misses.


### Relevance follows a similar pattern


Canary scores 87.4 on Relevance, 4 to 10 points higher than the baselines. General-purpose models sometimes test the component's general functionality rather than the specific change introduced by the PR. They test that a feature works rather than testing what specifically changed about it. For a QA workflow triggered on every PR, this distinction is critical. See theappendix for the per-repo Relevance breakdown.


### Coherence goes the other direction


All three general-purpose models outscore Canary on Coherence (84.5, 84.5, 83.7 vs 77.4). This is a real tradeoff. General-purpose models produce tighter, more executable test scripts with specific assertions and clear step-by-step instructions. Canary produces higher-level test plans that cover more ground but require an execution layer to become fully runnable.


We think this is the right tradeoff for now. A comprehensive plan that identifies all five affected flows is more valuable than a polished script that only tests one. But we acknowledge the gap and are actively working on closing it. The per-repo Coherence breakdown is in theappendix .


### GPT 5.4 is the strongest general-purpose baseline


It scores 80.2 overall, notably ahead of Claude Code (78.0) and Sonnet (73.2). On Cal.com it ties Canary at 83.2 overall and actually leads on Relevance (86.3 vs 85.5). But even GPT 5.4 trails by 11.3 points on Coverage. The agentic loop in Claude Code does not close the gap either. Coverage requires architectural investment, not just a stronger model or more tool use.


---


## Examples


### Grafana PR #117212


This PR changed how dashboards handle multiple datasources of the same type during export and import. The changes touched export label generation, import form rendering, query variable initialization, and backward compatibility.


#### Grafana PR #117212


Changed dashboard export/import handling for multiple datasources of the same type


Canary


12 tests


Score


82.6


- •


Import multi-datasource dashboard: independent selection per label
- •


Import form: labeled field names for clarity
- •


Import without export labels: backward compat
- •


Query variable: default datasource initialization
- •


Query variable: datasource export references
- •


Query variable: no default datasource edge case
- •


Export two Prometheus datasources: unique labels
- •


Export mixed datasource types (Prometheus + Loki)
- •


Export preserves template variable references
- •


Import labeled datasources: complete mapping and cleanup
- •


Import without labels: type-based fallback
- •


Import complete: label cleanup after mapping


GPT 5.4


5 tests


Score


80.2


- •


Export dashboard with multiple same-type datasources
- •


Import dashboard with label-based datasource selection
- •


Query variable default initialization
- •


Backward compat: dashboards without labels
- •


Datasource selection form fields


Claude Code Opus


3 tests


Score


78


- •


Export V2 dashboard with two Prometheus datasources
- •


Import V2 dashboard: separate fields for same-type datasources
- •


New query variable persisted in V2 dashboard


Sonnet 4.6


2 tests


Score


70.8


- •


Export V2 dashboard: datasource label generation
- •


Import V2 dashboard: label-based datasource mapping


**Canary** generated twelve tests across four components: multi-datasource import with independent label selection, import form validation, backward compatibility for dashboards without labels, query variable initialization with default datasource, export with unique labels for same-type datasources, mixed datasource type export (Prometheus + Loki), template variable preservation, and complete label cleanup after mapping.


**GPT 5.4** generated five tests covering export, import, variable initialization, backward compatibility, and datasource selection form fields. Solid breadth but missed mixed datasource types, form validation, and template variable edge cases.


**Claude Code** generated three tests: export with two Prometheus datasources, import with separate same-type datasource fields, and query variable persistence in V2 dashboards.


**Sonnet 4.6** generated two tests: export label generation and import label-based mapping. Well-structured and precisely targeted at the core change, but missing the majority of affected flows.


**Scores:** Canary 82.6 / GPT 5.4 80.2 / Claude Code 78.0 / Sonnet 70.8. Sonnet scored higher on Relevance (92 vs 85) because its tests were precisely targeted at the core change. It just didn't generate enough of them.


---


### Grafana PR #119349


This PR added a` data-is-dragging` attribute to query cards in the panel editor sidebar, changing their background color during drag operations. A focused change that gets missed entirely if you're not reading the code.


#### Grafana PR #119349


Added data-is-dragging attribute to query cards for visual drag feedback


Canary


3 tests


Score


91


- •


Query card visual feedback during drag: targets data-is-dragging attribute directly
- •


Transformation card drag with visual feedback: extends to second card type sharing DraggableList
- •


Single-item edge case: verifies drag feedback works with only one card


GPT 5.4


3 tests


Score


85


- •


Correctly targeted data-is-dragging attribute and background color change
- •


Tested both selected and unselected card states during drag
- •


Missed transformation cards: only tested query cards


Claude Code Opus


2 tests


Score


81


- •


Correctly identified core change: background color during drag
- •


Distinguished between selected and unselected card states
- •


Missed transformation cards and single-item edge case


Sonnet 4.6


2 tests


Score


63


- •


Tested cards 'maintain background color' during drag: close but generic
- •


Tested drag-and-drop reordering: PR didn't change this behavior
- •


Tested the component, not the change


**Canary** generated three tests: query card visual feedback targeting the attribute directly, transformation card drag (a second card type sharing the same component), and a single-item edge case. Happy path plus edge case.


**GPT 5.4** generated three tests correctly targeting the` data-is-dragging` attribute and background color change, including both selected and unselected card states. Missed transformation cards but nailed the core change.


**Claude Code** generated two tests correctly identifying the background color change and distinguishing selected/unselected states. Missed transformation cards and the edge case.


**Sonnet 4.6** generated two tests for generic drag-and-drop reordering. The PR didn't change reordering. It added visual feedback during drag. Sonnet tested the component, not the change.


**Scores:** Canary 91 / GPT 5.4 85 / Claude Code 81 / Sonnet 63. Gap driven almost entirely by Coverage.


---


## Limitations


We want to be direct about what this evaluation does not prove.


**Sample size is small.** 35 PRs across four repos shows a consistent pattern but isn't enough for high statistical confidence. This is V0.


**LLM-as-judge introduces potential bias.** We use Opus 4 to evaluate outputs that include Claude-family and OpenAI baselines. We mitigate with output anonymization and randomized presentation order. We ran the judge multiple times on a subset of PRs and observed low variance in relative rankings, though absolute scores shifted by 1-2 points between runs. All three general-purpose models outperforming Canary on Coherence is evidence that the judge isn't simply favoring the purpose-built system. We still recommend interpreting absolute scores with caution. The relative rankings and the consistency of gaps across repos are more informative than any single number.


Research on LLM-as-judge bias is active and the concerns are well-documented. Self-preference bias has been demonstrated across model families. We plan to move to human evaluation in the next iteration.


**Agent outputs differ in format.** Canary produces high-level test plans. The baselines produce more concrete test scripts. This is an advantage on Coverage (plans can describe more flows) and a disadvantage on Coherence (plans lack implementation specifics). A fairer comparison would evaluate on executed tests with pass/fail outcomes, which is what V1 targets.


**Baseline setup.** Each baseline was given the same repo context and PR diff, then asked to analyze components and generate tests. Our agent uses a multi-step architecture (PR analysis, flow mapping, test generation). Critics may argue the comparison is between a pipeline and a single prompt. We think that's the point. The architectural difference *is* the finding. But we acknowledge the baselines were not given multi-step scaffolding of their own.


---


## What's next


**Bug injection.** V1 will introduce known bugs into PRs and measure whether each agent's tests catch them. This gives us a fully deterministic, pass/fail metric that removes the LLM judge entirely for the core signal.


**Larger sample set.** 35 PRs across four repos is a starting point. Future versions will include significantly more pull requests across different types of open-source repositories, covering a wider range of frameworks, languages, and PR complexity.


**Human judge.** Moving from LLM-as-judge to human evaluation for scoring. This eliminates model-family bias concerns entirely and grounds the evaluation in how QA engineers actually assess test quality.


**More baselines.** Adding models and agent frameworks as they ship.


**Open-source eval harness.** We plan to publish the evaluation pipeline so anyone can run QA-Bench against their own agent or model. The full PR list with GitHub links will be published separately.


---


## Appendix


### Scoring Rubric


Note


Each criterion is scored on a 0-100 scale. Overall is the simple average of all three metrics.


**Relevance (0-100):** Checks whether the tests are actually relevant to the change and not just any arbitrary test. Do the tests directly target the UI/UX changes introduced in this specific PR? Are they testing the exact components and user flows that were modified? Penalize heavily if tests cover unrelated functionality or miss key changes.


- **90-100:** Perfectly aligned with PR changes, tests exactly what was modified, zero irrelevant tests
- **70-89:** Good alignment, covers most important changes with minimal irrelevant content
- **50-69:** Partially relevant, misses some key changes or includes several irrelevant tests
- **30-49:** Somewhat off-target, significant gaps in testing actual changes
- **0-29:** Mostly irrelevant, completely missing the point of what changed


**Coverage (0-100):** Whether the tests cover every important user flow affected in the PR. Do the tests cover all critical user journeys affected by the PR changes? Are important edge cases included? Coverage is about breadth across the changed features, not the entire application.


- **90-100:** Comprehensive coverage of all critical paths affected by PR changes, plus important edge cases
- **70-89:** Good coverage of main changed flows, some edge cases
- **50-69:** Covers basic scenarios but misses important modified cases
- **30-49:** Limited coverage, significant gaps in testing changed functionality
- **0-29:** Minimal coverage, most changed scenarios untested


**Coherence (0-100):** Whether the tests include all correct steps without missing intermediary steps. Are the tests clear, actionable, and complete enough for a QA engineer to execute? A perfectly written test that doesn't test the right thing is worthless. Prioritize relevance and coverage over polish.


- **90-100:** Excellent structure, clear steps, maintainable, follows best practices
- **70-89:** Good structure, minor issues but still usable
- **50-69:** Acceptable structure, some clarity issues but functional
- **30-49:** Poor structure but still somewhat usable
- **0-29:** Very poor structure, hard to understand or execute


---


### Per-Repo Results


Grafana (15 PRs)


Relevance Coverage Coherence Overall


Canary 87.0 83.5 76.4 82.3


GPT 5.4 81.7 72.4 83.2 79.1


Claude Code (Opus 4.6) 82.0 65.7 84.9 77.5


Sonnet 4.6 81.4 61.3 83.1 75.3


Mattermost (8 PRs)


Relevance Coverage Coherence Overall


Canary 88.1 84.7 77.5 83.4


GPT 5.4 83.5 70.0 84.2 79.2


Claude Code (Opus 4.6) 83.3 61.8 84.8 76.6


Sonnet 4.6 80.0 60.8 84.8 75.2


Cal.com (6 PRs)


Relevance Coverage Coherence Overall


Canary 85.5 86.1 78.1 83.2


GPT 5.4 86.3 77.3 86.0 83.2


Claude Code (Opus 4.6) 82.5 65.6 83.7 77.3


Sonnet 4.6 83.1 64.6 83.1 76.9


Apache Superset (6 PRs)


Relevance Coverage Coherence Overall


Canary 89.0 83.5 77.5 83.3


GPT 5.4 79.7 73.2 84.8 79.2


Claude Code (Opus 4.6) 85.0 72.1 84.8 80.6


Sonnet 4.6 64.1 48.3 83.8 65.4


---


### Per-Metric Breakdown by Repository


Relevance by repository


Grafana 15 PRs


Canary


87.0


GPT 5.4


81.7


Claude Code (Opus 4.6)


82.0


Sonnet 4.6


81.4


Mattermost 8 PRs


Canary


88.1


GPT 5.4


83.5


Claude Code (Opus 4.6)


83.3


Sonnet 4.6


80.0


Cal.com 6 PRs


Canary


85.5


GPT 5.4


86.3


Claude Code (Opus 4.6)


82.5


Sonnet 4.6


83.1


Superset 6 PRs


Canary


89.0


GPT 5.4


79.7


Claude Code (Opus 4.6)


85.0


Sonnet 4.6


64.1


Canary


GPT 5.4


Claude Code (Opus 4.6)


Sonnet 4.6


Coverage by repository


Grafana 15 PRs


Canary


83.5


GPT 5.4


72.4


Claude Code (Opus 4.6)


65.7


Sonnet 4.6


61.3


Mattermost 8 PRs


Canary


84.7


GPT 5.4


70.0


Claude Code (Opus 4.6)


61.8


Sonnet 4.6


60.8


Cal.com 6 PRs


Canary


86.1


GPT 5.4


77.3


Claude Code (Opus 4.6)


65.6


Sonnet 4.6


64.6


Superset 6 PRs


Canary


83.5


GPT 5.4


73.2


Claude Code (Opus 4.6)


72.1


Sonnet 4.6


48.3


Canary


GPT 5.4


Claude Code (Opus 4.6)


Sonnet 4.6


Coherence by repository


Grafana 15 PRs


Canary


76.4


GPT 5.4


83.2


Claude Code (Opus 4.6)


84.9


Sonnet 4.6


83.1


Mattermost 8 PRs


Canary


77.5


GPT 5.4


84.2


Claude Code (Opus 4.6)


84.8


Sonnet 4.6


84.8


Cal.com 6 PRs


Canary


78.1


GPT 5.4


86.0


Claude Code (Opus 4.6)


83.7


Sonnet 4.6


83.1


Superset 6 PRs


Canary


77.5


GPT 5.4


84.8


Claude Code (Opus 4.6)


84.8


Sonnet 4.6


83.8


Canary


GPT 5.4


Claude Code (Opus 4.6)


Sonnet 4.6
