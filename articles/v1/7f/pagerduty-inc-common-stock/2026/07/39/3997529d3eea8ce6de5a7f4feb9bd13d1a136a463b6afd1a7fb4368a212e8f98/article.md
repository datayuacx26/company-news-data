---
schema_version: "1.0.0"
document_id: "3997529d3eea8ce6de5a7f4feb9bd13d1a136a463b6afd1a7fb4368a212e8f98"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-6c10cddc543b"
canonical_url: "https://www.pagerduty.com/blog/ai/pagerduty-agent-app-in-github-agent-hq/"
published_at: "2026-07-01T11:30:15+00:00"
first_seen_at: "2026-07-25T01:13:16.087688+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:b931a306f247395f11b8de8ceaf4e619ebef5ee5415696dabf0752303659c53f"
---

# PagerDuty agent app in GitHub: incident context where you already work by Hannah Culver

This blog post is part of PagerDuty’s ongoing series on how we’re helping customers navigate their journey toward autonomous operations. Read on to learn about the


[PagerDuty agent app in GitHub (Early Access)](https://www.pagerduty.com/drops/) and how it builds toward this vision.


How many tabs do you have open right now? And how many more do you open the moment an incident hits?


Context switching during incident response is one of the most persistent sources of toil in engineering. Critical data including incident history, deployment correlations, service dependencies, runbooks, and more lives across a dozen different tools. You tab hop, piece it together manually, and by the time you have a picture of what’s happening, you’ve already lost focus on the fix.


And some of these incidents are completely preventable. With the right information ahead of a deploy, a developer can make a smarter call before the code ever ships, shifting left and preventing an incident from happening. The information powering this exists in PagerDuty. But it’s not always where developers spend most of their time: inside GitHub. Now, with this data (plus the PagerDuty SRE agent) available via GitHub, you can stay in your tool of choice and skip the tab switching for good.


### The PR is where the fix happens. Incident context should be there too.


The PagerDuty agent app in GitHub brings live incident state, change correlations, and operational context directly into your pull requests without requiring you to leave GitHub.


Here’s what it does:


- **Surfaces live incident data in your PR.** See active incidents, service health, and change correlations the moment you open a pull request against an affected service.


- **Connects to incident history.** Query past P1s, recurring failure patterns, and incident notes across services directly from the PR using plain language.


- **Runs pre-commit risk assessments.** @-mention the PagerDuty agent app directly in a PR comment and get a risk score based on live incident data, deployment history, and service dependencies to prevent an incident from happening


- **Keeps you in your workflow.** Powered by PagerDuty’s remote MCP server and Advance MCP server (early access), the agent app chains GitHub and PagerDuty data in real time so you can shift left while working where you want.


### What this looks like in practice


Picture a developer on their first deploy to a payments service. The dev knows this service is customer impacting, and they want to reduce as much risk as possible. They open a pull request, @-mention the PagerDuty agent app, and ask: can I push to this service? How stable is it?


Within seconds, the agent app returns a risk score. Critical, in this case. Two active incidents are affecting a critical dependency. It lists the risk factors: the branch directly modifies gateway timeout logic tied to the current failure pattern, the service has logged 68 P1s in the last 90 days, and incident notes repeatedly reference the same circuit-breaker behavior.


The recommendation: hold the deploy. Wait for more stability. Then push.


The developer doesn’t ping a senior engineer. They don’t dig through a stale runbook. They don’t open Slack, check the status page, or pull up PagerDuty in a separate tab. They have what they need to make the right call, in the tool they’re already in, in minutes.


That same workflow runs in reverse when production is already on fire. When an incident hits a service you own, the agent app surfaces live incident state and change correlations directly in the PR so you can identify what recent commit triggered it, generate a fix, and link it back to the incident without ever leaving GitHub.


## The path to autonomous operations


The PagerDuty agent app in GitHub is part of PagerDuty’s broader vision for autonomous operations: the right data and actions in the tools your team already uses at the moment they matter most.


GitHub is the orchestration layer. PagerDuty is the operational signal. Put them together and you get an agent that understands not just your code, but the system it runs in.


The integration is available today in


[GitHub](https://github.com/apps/pagerduty-agent-app) . PagerDuty Advance customers can sign up for


[Early Access](https://www.pagerduty.com/early-access/) to Advance MCP, unlocking the SRE agent in this workflow. See what else we’ve been up to on our


[Product Drops page](https://www.pagerduty.com/drops/) .
