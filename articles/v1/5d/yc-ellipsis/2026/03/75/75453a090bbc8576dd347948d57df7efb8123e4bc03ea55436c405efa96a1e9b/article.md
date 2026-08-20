---
schema_version: "1.0.0"
document_id: "75453a090bbc8576dd347948d57df7efb8123e4bc03ea55436c405efa96a1e9b"
company_key: "yc-ellipsis"
company: "Ellipsis"
source_id: "yc-ellipsis-news-import-97656c4ed8a9"
canonical_url: "https://www.ellipsis.dev/blog/automating-your-daily-standup-with-ellipsis"
published_at: "2026-03-23T00:00:00+00:00"
first_seen_at: "2026-07-24T05:57:47.052865+00:00"
fetched_at: "2026-07-27T09:02:31.696288+00:00"
content_hash: "sha256:dc6606e4eeb4bc68a2cbda8be3ce22265c1e70e594d3d65aacb705f8b1a55d36"
---

# Automating Your Daily Standup with Ellipsis

Typical engineering teams have a 20-minute daily standup. How much of it goes to resolving blockers, and how much goes to each developer recounting what they shipped yesterday? In our experience, the status half eats the time the blockers half deserves.


That status half is exactly the kind of recurring toil an Ellipsis cloud agent absorbs. You define the agent as one YAML file in your repository, give it a cron trigger, and every weekday morning it reads everything that merged in the last 24 hours and writes a product-level summary your team can skim before standup. No local setup, no CI workflow, no scheduler infrastructure: the schedule deploys with git push.


## One YAML file on a cron schedule


Here is the agent. It ships as the[Daily Standup template](https://www.ellipsis.dev/agents/templates/daily-standup) , so you can deploy it as-is; this is a condensed version of the same config to show the moving parts:


```text
ellipsis:
version: v1
name: Daily standup
description: A product-level summary of yesterday's merged pull requests


claude:
model: claude-sonnet-5
system: |
You write the engineering team's daily standup: one product-level
summary of everything that merged across the account's repositories
in the last 24 hours.


Use the pre-authenticated gh CLI to find every pull request merged
in the window, and read enough of each one to understand the change,
not just the title.


Do not write a PR-by-PR changelog. Group related pull requests into
the themes of work they add up to, lead with what changed for users,
and collapse routine maintenance into one short mention at the end.
Keep it tight: a one-sentence headline, then 2-5 themed bullets with
PR numbers in parentheses as evidence.


Return the finished standup as your answer, formatted as Slack-ready
markdown.


trigger:
type: cron
schedule: "0 13 * * 1-5"


sandbox:
github:
permissions: read_only


budget:
session: 1.00
day: 2.00
```


Three parts do the work. The` trigger` runs the agent at 13:00 UTC every weekday; Ellipsis registers the schedule when the file lands on your default branch. The` system` prompt is the interesting part: it pushes the agent past a PR-by-PR changelog into a themed narrative of what the team actually accomplished. And the` budget` caps what any single session, and any single day, can spend.


## Read-only by construction


A standup writer needs to read your pull requests. It has no business writing to them. This line is the whole security story:


```text
sandbox:
github:
permissions: read_only
```


Each session runs in an isolated cloud sandbox with a short-lived GitHub token minted for that session, and` read_only` narrows that token to read access at the mint. The narrowing is enforced by GitHub itself, so nothing running in the sandbox can exceed it: not a misbehaving tool, not a prompt injection hiding in a PR description. The token dies with the sandbox. Because the permission grant is YAML in git, the agent's blast radius is explicit, reviewed, and auditable like the rest of your code.


## What lands in Slack


The agent returns the finished standup as its answer, formatted so it can be posted to your engineering channel as-is. A morning's output reads like this:


```text
Merged 7 PRs across 3 repos. Shift-trade expiry shipped end to end.


- Trade requests now expire when the counterparty account is
deactivated, closing the stuck-pending loop (#431, #438).
- Managers can export tip-pool reports as CSV from the dashboard
(#212).
- Groundwork for rest-period rules: the scheduling engine now tracks
per-worker minimum gaps between shifts (#440).


Plus routine dependency and CI upkeep across 4 repos.
```


If your Slack workspace is connected to Ellipsis, the agent has Slack tools in the sandbox: add one line to the prompt and it posts the summary to #engineering itself. Either way, every session is recorded, so you can open the transcript and see exactly which pull requests it read and what it did with them.


## Deploy it


The fastest path is the[Daily Standup template](https://www.ellipsis.dev/agents/templates/daily-standup) : deploying it opens a pull request that adds the config file to your repository, and the agent is live the moment it merges. Adjust the schedule, the channel, or the voice by editing the YAML; every change is a reviewed diff.


For the full picture of cron triggers, schedule syntax, and running several schedules per agent, see the[scheduling guide](https://www.ellipsis.dev/docs/guides/spawn-an-agent-session-on-a-cron-schedule) in the documentation.
