---
schema_version: "1.0.0"
document_id: "2e0bfe1768854977eb6a8dbeeacadf0ef3246ca205d5f243548136cdf694919a"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-5eeb9d1ed7ff"
canonical_url: "https://devblogs.microsoft.com/blog/how-to-test-agent-experience-changes-without-shipping-them/"
published_at: "2026-07-21T07:15:12+00:00"
first_seen_at: "2026-07-28T20:49:44.844844+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:40caf90a5aff2f51b1c049d0ae0ef704a2bbd0a3c800f19f4ad93a6d98b2e3c7"
---

# How to test agent experience changes without shipping them

You’ve built an eval. You’ve[established a baseline](https://developer.microsoft.com/blog/is-your-agent-extension-actually-working/) and found the gaps. Now you have hypotheses about how to fix them: maybe a documentation rewrite, or a different CLI output format. The natural next step is to ship the change and measure again. Don’t do that just yet.


## Why most hypotheses fail


Every improvement you think of is a hypothesis. “If I add a warning callout to the release notes, the agent will use the CLI instead of attempting a manual upgrade.” It sounds reasonable, might even be obvious. But LLMs don’t reason the way you expect them to, and obvious changes produce counterintuitive results with uncomfortable regularity.


We’ve seen this first-hand working on a SharePoint Framework (SPFx) project upgrade scenario. SPFx projects upgrade incrementally, version by version, each step introducing its own configuration- and dependency changes. The agent needed to discover a CLI tool from documentation. The documentation included a tip recommending the CLI. Straightforward, right? The agent ignored it in every single run. The tip was there, the agent read the page, and it did nothing differently.


The intuition was that a recommendation in the docs would influence the agent’s behavior. The reality was that the agent had[already formed a plan](https://developer.microsoft.com/blog/your-agent-already-has-a-plan/) from its training data before it fetched the page. The documentation confirmed parts of that plan (package names, version numbers) and the tip got filed away as optional side information. Advisory content does not override an existing plan. The agent treats “you could also try X” as noise when it already has a strategy it believes will work.


## What actually worked was not what we expected


We tested a dozen variations. We moved the tip to a different position on the page. Then, we reworded it. We even included the exact command to run. Nothing made a difference, and the most useful mental model for understanding why: the agent processes an entire page and gravitates toward actionable step-by-step content. A tip, by definition, is not a directive.


What finally worked was a warning that told the agent its current plan would fail. Not “here’s an alternative” but “your approach will result in build failures.” Five out of five runs flipped to using the CLI. The agent abandoned its plan because the documentation told it the plan was broken, not because a better alternative existed.


Nobody on the team predicted this. We assumed stronger wording in the tip would suffice. We were wrong, and we would have shipped a useless documentation change to production if we hadn’t tested it first.


## The compounding problem


That was just one hypothesis. In the same engagement, we tested over a dozen more, and the hit rate was not encouraging:


- Removing links to a competing migration guide helped CLI adoption but destroyed overall quality (dependency accuracy dropped from 34/50 to 18/50) because the four out of five runs that still didn’t find the CLI no longer had the guide to work from.
- Adding structured JSON output to the CLI improved configuration correctness from 72/85 to 85/85, but switching to JSONL (which seems superficially similar) cost 2.6x more tokens with worse outcomes across every dimension.
- PowerShell syntax in code fences bled into the agent’s other work, degrading idiomatic code quality even in files unrelated to the upgrade task.
- Providing an explicit script to execute did not cause the agent to execute it. In zero runs out of ten did the agent pipe the script to a shell. It always decomposed and applied steps individually.


Each of these results contradicts a reasonable assumption. “JSONL is structured too, it should work as well as JSON.” “If I give the agent a script, it’ll run it.” “PowerShell is just a different shell syntax.” It turns out, that every one of these intuitions was wrong. Not slightly wrong: wrong to the point of tripling token costs or halving quality scores.


## Why you need to emulate before you ship


If you’re changing documentation on a live site, every hypothesis means a pull request. Someone reviews it, someone merges it, it deploys, and then you measure. If the hypothesis was wrong (and many of them will be), you now have a live documentation change that’s either neutral or actively harmful, and you need another PR to revert it.


If you’re changing an MCP server response format, you’re deploying a new version to test something that might not work. If it’s a public API behind your tooling, you’re shipping changes that affect real users while you figure out whether your theory holds.


The iteration cycle for “change live infrastructure, measure, revert if wrong” is measured in days. The iteration cycle for “emulate the change locally, measure, ship only what works” is measured in minutes. When you’re testing a dozen hypotheses and most of them won’t pan out, the difference is between testing three ideas in a week and testing thirty.


## What emulation looks like in practice


You intercept the requests your agent makes and return modified responses as if the change were already live. Your agent calls the same URLs, gets responses that reflect your hypothesis, and you measure whether behavior improves.


For the SPFx work, we used[Dev Proxy](https://aka.ms/devproxy) to intercept requests to Microsoft Learn and return modified page content. The agent called` https://learn.microsoft.com/en-us/sharepoint/dev/spfx/release-1.22` and got back a version of the page where we’d added the warning callout, without touching the live page. When the hypothesis worked, we shipped the change. When it didn’t (like the tip-only experiments), we moved on in minutes instead of days.


```text
{
"request": {
"url": "https://learn.microsoft.com/en-us/sharepoint/dev/spfx/release-1.22",
"method": "GET"
},
"response": {
"statusCode": 200,
"body": "@modified-release-notes.html",
"headers": [
{ "name": "Content-Type", "value": "text/html" }
]
}
}


```


The same pattern applies to MCP servers. If your agent calls an MCP endpoint and you want to test whether a different response format produces better outcomes, you can emulate the modified response without deploying a new version. Test locally, validate the hypothesis, ship only what’s proven.


No test environment to stand up, no PRs to merge and revert. The agent doesn’t know or care that a proxy is answering instead of the real server. You get deterministic, repeatable experiments against the same stable URLs your agent will use in production.


## The experiment mindset


Improving agent experience requires an experimental discipline most teams aren’t used to. When you optimize a REST API, you can reason about the outcome: faster query, smaller payload. The relationship between change and outcome is predictable. With LLMs it isn’t. A change that seems cosmetic (switching from a tip to a warning) can flip behavior completely. A change that seems significant (providing a ready-to-run script) can have zero effect.


The only way to know is to measure. And measuring is only practical if the cost of each experiment is low enough to run many of them. This is why the teams that improve agent experience fastest are the ones that make experimentation cheap. They aren’t smarter about predicting LLM behavior. They just test more hypotheses per week because their iteration loop is shorter. The[eval infrastructure](https://developer.microsoft.com/blog/building-ax-evals-that-actually-work/) is what gives you the ability to measure. Emulation is what gives you the ability to iterate.


## Start with what you control


You don’t need a complex setup to begin. If your agent consumes documentation, you can emulate documentation changes. If it calls an API, you can emulate response changes. And you can also introduce new API endpoints, -operations and docs pages. Start with whatever surface your agent interacts with most, form a hypothesis about what would improve it, and test that hypothesis without shipping anything.


Here’s why this matters: every finding in this article came from our own experience of starting with a reasonable assumption and discovering through measurement that our assumption was wrong. We genuinely expected the tip to work. We expected the script to get executed. We were wrong on both counts, and on most of the others too.[Models don’t have preferences, they have context](https://developer.microsoft.com/blog/models-dont-have-preferences-they-have-context/) . The context you provide determines what happens, but predicting exactly *how* is not something humans are good at yet.


Test your hypotheses. Most of them will be wrong. Make that cheap, and you’ll converge on what actually works.
