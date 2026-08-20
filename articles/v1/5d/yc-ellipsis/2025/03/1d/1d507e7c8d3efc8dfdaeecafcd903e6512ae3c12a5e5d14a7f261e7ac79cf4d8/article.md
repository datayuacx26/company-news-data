---
schema_version: "1.0.0"
document_id: "1d507e7c8d3efc8dfdaeecafcd903e6512ae3c12a5e5d14a7f261e7ac79cf4d8"
company_key: "yc-ellipsis"
company: "Ellipsis"
source_id: "yc-ellipsis-news-import-97656c4ed8a9"
canonical_url: "https://www.ellipsis.dev/blog/automated-documentation-maintenance"
published_at: "2025-03-13T00:00:00+00:00"
first_seen_at: "2026-07-24T05:57:47.052865+00:00"
fetched_at: "2026-07-28T10:43:22.567160+00:00"
content_hash: "sha256:af165693006954f8233161f7ad4cca007bb2fea7eb1b605e99cf9c9733a88f7f"
---

# Maintaining Documentation Without a Human

Every engineering team has the same documentation problem. Someone writes comprehensive docs during a launch. Six months later, the code has diverged. The docs describe an API that no longer exists, reference configuration options that have been renamed, and omit three features that shipped since the last update.


The root cause is not laziness. It is incentive structure. Writing docs is high-effort, low-urgency work that competes with feature development for engineering time. Docs are always the first thing deprioritized and the last thing updated.


We solved this by removing the human from the loop entirely.


## Why documentation rots


Documentation rot follows a predictable pattern. A team writes docs as part of a launch milestone. The docs are accurate on day one. Then the codebase evolves — a parameter is renamed, a new endpoint is added, a configuration format changes — and nobody updates the docs because there is no trigger to do so.


The problem compounds. Once docs drift far enough from reality, developers stop trusting them. Once developers stop trusting docs, they stop reading them. Once developers stop reading docs, nobody notices when the docs drift further. The feedback loop is entirely negative.


\[Diagram: Negative feedback loop — docs drift → trust decreases → usage drops → drift accelerates → docs become useless\]


## The agent-based solution


Ellipsis automations can run on a cron schedule. You define a trigger, point the agent at your docs, and give it a prompt. Every week (or whatever cadence you choose), the agent:


1. Checks for code changes since its last run
2. Compares the current codebase against existing documentation
3. Identifies sections where the docs have drifted from the code
4. Generates a pull request with the necessary updates
5. Tags the relevant team for review


The human reviews the PR, not the documentation. They approve, request changes, or close it. The agent handles the diff; the engineer handles the judgment call.


## Configuration


Setting this up in Ellipsis takes one block in your` ellipsis.yaml` file:


```text
automations:
- name: "Weekly docs sync"
trigger:
schedule: "0 9 * * 1"  # Every Monday at 9am
agent: code-generation
prompt: |
Review all files in /docs and compare against the current codebase.
Identify any documentation that has drifted from the implementation.
Generate a PR updating the affected docs sections.
Focus on: API endpoints, configuration options, environment variables,
and setup instructions.
branch_prefix: "docs/auto-sync"


```


The agent runs in a sandboxed environment with read access to your repository. It uses the codebase as the source of truth and the documentation as the target. When it detects drift, it writes the update and opens a PR.


## What the agent catches


In production, we have seen the docs agent catch the following categories of drift:


- **API changes.** Renamed parameters, deprecated endpoints, new response fields. These are the most common and the most impactful — developers hitting a 400 error because the docs said to pass` user_id` when the API now expects` userId` .
- **Configuration drift.** Environment variables added or removed, default values changed, new required fields in config files.
- **Setup instructions.** New dependencies, changed build commands, updated Docker configurations.
- **Feature additions.** New features that shipped without corresponding documentation updates.


\[Screenshot: GitHub PR opened by Ellipsis titled "docs: update API reference for v2.3 changes" — showing a diff updating three endpoint descriptions and adding a new configuration section\]


## Results


Teams running the docs automation typically see three things happen:


- **Docs accuracy increases.** The obvious one. Weekly checks catch drift before it compounds.
- **Developer trust in docs returns.** Once the team knows docs are actively maintained, they start reading them again. Support tickets decrease. Onboarding time decreases.
- **Engineers stop dreading docs work.** The agent handles the tedious part — identifying what changed and writing the update. The human handles the editorial part — ensuring the update is clear and accurate. The work that engineers dislike is eliminated; the work that requires judgment is preserved.


## Limitations


The agent is not a technical writer. It updates documentation to reflect code changes, but it does not restructure documentation for clarity, write tutorials, or create conceptual guides. It is a maintenance tool, not a content creation tool.


For teams with large, complex documentation sites, we recommend starting with API references and configuration docs — the sections most likely to drift and most straightforward for the agent to update. Expand the scope as you build confidence in the output quality.


Documentation maintenance is a solved problem. The solution is not discipline — it is automation.
