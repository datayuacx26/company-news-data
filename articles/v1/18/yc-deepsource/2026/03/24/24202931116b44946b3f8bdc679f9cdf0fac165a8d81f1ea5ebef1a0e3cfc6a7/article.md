---
schema_version: "1.0.0"
document_id: "24202931116b44946b3f8bdc679f9cdf0fac165a8d81f1ea5ebef1a0e3cfc6a7"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/deepsource-cli-with-agents"
published_at: "2026-03-05T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:d7594684d79ebc15a30667ef0dbacceaa66266f9e353b7d02a5760cc72801d79"
---

# Code review for AI agents

Last updated on Mar 5, 2026


Code review results live in dashboards and PR comments. Agents can't parse either. Developers have to context-switch to read them and often copy-paste into their AI agent chat box.


We've made several improvements to[DeepSource's CLI](https://deepsource.com/changelog/2026-03-06) to make it easier for your AI agent to take action on DeepSource's results.


```text
curl   -fsSL   https://cli.deepsource.com/install   |   sh


```


The CLI can now pull review results into the terminal as structured JSON — issues by file, Report Card grades, dependency vulnerabilities, CVSS scores. If you're building an agent workflow or scripting review checks in CI, you can now automate parsing DeepSource's review and ask your agent to fix issues autonomously.


## The feedback loop


1. Your agent writes code and opens a PR.
2. DeepSource reviews it. The hybrid engine runs thousands of static analysis rules alongside the AI review agent, then grades the PR across five dimensions in the Report Card — Security, Reliability, Complexity, Hygiene, and Coverage.
3. Your agent calls the CLI to pull findings into its context: issues by file, report card grades, dependency vulnerabilities, all as structured JSON.
4. Your agent gets the list of issues, key metrics, and feedback on what it can do better from a code quality and security perspective. It can iterativelty fix issues, commit, and try again until the PR is clean.


## Structured, not noisy


A flat list of issues is not enough. Some are useful, some noise, and usually no way to tell where to focus on the most.


DeepSource's[Report Card](https://docs.deepsource.com/docs/platform/dashboard/repository/history#report-card) grades every PR across the five dimensions important for writing clean and secure code. With the CLI's new` --output json` flag, this feedback comes to your agent directly. Instead of "here are 47 issues," you get "Security is at C, everything else is passing."


Here are some sample queries your agent can perform:


### Issues scoped to specific files or severities


```text
deepsource   issues   --file   src/auth/login.ts   --output   json
deepsource   issues   --severity   critical   --category   security   --output   json


```


```text
[
{
"file"  :   "src/auth/login.ts"  ,
"line"  :   42  ,
"severity"  :   "critical"  ,
"category"  :   "security"  ,
"code"  :   "JS-W1043"  ,
"message"  :   "Unsanitized user input passed to SQL query"
}
]


```


### Report Card


```text
deepsource   report-card   --output   json


```


### OSS dependency vulnerabilities


```text
deepsource   vulnerabilities   --output   json


```


### Repository metrics


```text
deepsource   metrics   --output   json
deepsource   repo   status   --output   json


```


## Agent Skills


[DeepSource Skills](https://github.com/deepsourcecorp/skills) are pre-built skill packages that give coding agents direct access to the CLI. They work with Claude Code, Cursor, Copilot, Cline, Codex, and 18+ other agents.


```text
npx   skills   add   DeepSourceCorp/skills


```
