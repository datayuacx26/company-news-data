---
schema_version: "1.0.0"
document_id: "d42cecc21ab14dc43919080bab1f39ce712a8ed1010807d38b93fb5370d62952"
company_key: "yc-kestrel-ai"
company: "Kestrel AI"
source_id: "yc-kestrel-ai-news-import-7c48f7782d06"
canonical_url: "https://usekestrel.ai/blog/two-ways-to-start-building"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-25T10:40:48.612135+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:af3764a527ee3d4ca44e1b7f66f75689f3bf13999a230079ee704bbd7b856d66"
---

# Build Your First Workflows in Minutes: Suggested Workflows & the Workflow Assistant

The hardest part of automation is often a blank canvas. You know there's work worth automating — the incident runbook that lives in one person's head, the provisioning request that piles up in Slack, the shell script someone runs by hand during an incident — but staring at an empty workflow builder, it's not obvious where to start. So we built two AI-powered on-ramps that meet you where you already are. The demo above walks through both, end to end: first generating ready-to-deploy workflows from your connected integrations, then handing the assistant a runbook and watching it produce a complete workflow you can ship.


The two share a goal but pull from different places. **Suggested Workflows** turns your *integrations* into automations — it looks at what you've connected and proposes complete workflows for your stack. The **Workflow Assistant** turns your *knowledge* into automations — describe a process, ask what's possible, or upload the runbook you already follow, and it builds the workflow for you. Whichever you start from, you go from zero to a running automation in minutes.


## Suggested Workflows: from your integrations


The **Workflow Library** opens with AI-suggested workflows tailored to your connected integrations. Kestrel analyzes what you have connected and generates ready-to-deploy workflows for each one. These aren't generic templates with blanks to fill in — each is a complete workflow with the right trigger, actions, and variable wiring already configured for your environment. If you've connected Kubernetes, you'll see crash-loop RCA workflows; connect AWS and cost-anomaly triage shows up; connect PagerDuty, Vercel, or PostHog and you get suggestions that use them too.


### Browse by source


Suggestions are grouped by the integration that powers them, so you can browse the library by source — **All** , Kubernetes, AWS, PagerDuty, and the rest — and quickly find automations for the part of your stack you're focused on. Each card shows the workflow name, a short description, and how many steps it has, so you can size up what it does at a glance.


### Generate a fresh set anytime


Click **Suggest Workflows** to generate a fresh set based on your current integrations. Connect something new — a cloud account, an observability tool, an on-call provider — and the next time you generate suggestions, you'll see workflows that use it. As your stack grows, so does the set of things Kestrel knows how to automate for you. It's a great first stop right after connecting a new tool.


Click to expand


### From suggestion to live workflow


Click any suggestion to open it on the workflow builder canvas. From there it's a normal workflow: review the steps, adjust the configuration in natural language or with drag-and-drop, and activate it when you're happy. Workflows you create from a suggestion get a **Created** badge and show up under the **Created** tab, so it's easy to see what you've already adopted and what's still just an idea.


The reason this works better than a template gallery is that suggestions are grounded in *your* environment. A generic "pod crash RCA" template would still leave you to pick the cluster, name the Slack channel, choose the Jira project, and thread the right variables between steps before it would run. A Kestrel suggestion arrives with those decisions already made against the integrations you've connected — the trigger is scoped, the actions reference real targets, and the template variables are wired end to end. That's the difference between a starting point you have to finish and a workflow you can read, tweak, and activate. And because the set regenerates from your live integrations, the library stays a useful map of what's automatable as your stack evolves, not a static list that goes stale.


## The Workflow Assistant: from your knowledge


Most teams already have their operational knowledge written down somewhere — a runbook in Confluence, a Notion page, a shell script in a repo that someone runs by hand during incidents. Where Suggested Workflows turns your integrations into automations, the **Workflow Assistant** turns that knowledge into automations. It's a conversational agent that helps you build workflows from a description, a question, or your existing runbooks and scripts — and hands you back a production-ready workflow you can review, refine, and ship.


### Just ask


Open the assistant and describe what you want to automate, or ask it what's possible. Things like:


- "What can I automate for cloud provisioning and developer requests?"
- "Help me automate our on-call escalation process."
- "What can I automate for Kubernetes pod failures?"


The assistant reasons about your connected integrations and your description, then proposes a complete workflow — with a name, a description, and a visual preview of the workflow graph right in the conversation. If your description is missing details a valid Kestrel workflow needs — which cluster or account to target, a Slack channel to post to, a Jira project, an approval threshold — it asks follow-up questions instead of guessing. It only finalizes once it has everything required, so what it hands back is always a complete, valid workflow you can deploy.


That back-and-forth is what makes the difference between a plausible-looking draft and something you can actually ship. A lot of "AI builds your workflow" tools will happily invent a channel name or pick an arbitrary cluster to fill a required field — and you don't find out until the run fails. The assistant treats missing context as a question, not a guess, and it scopes each question to what's genuinely ambiguous: if you've connected exactly one cluster it uses it; if you have several, it asks which. The result is a workflow whose every node points at a real target in your environment by the time it lands in front of you.


### Bring your runbooks and scripts


This is where it gets powerful. **Upload a runbook or script** and ask the assistant to generate workflows from your existing procedures — or point it at scripts in your connected GitHub or GitLab repos that you're still running manually. You can also pull in context from Confluence, Notion, and Glean so the workflow reflects how your team actually operates.


In the demo we hand it a cloud networking incident runbook. The assistant analyzes the steps and produces a complete workflow — name, description, and a visual preview of the graph — that investigates the change, branches on the root cause, remediates via an IaC pull request or the AWS CLI behind an approval, and notifies the team. It translates the steps a human used to follow into a deterministic automation, no canvas required to start.


That's the real unlock: the runbook stops being tribal knowledge that only works when the person who wrote it is online. Once it's a workflow, the procedure runs the same way every time, anyone on the team can trigger it, every step is logged, and the approval gates make sure a human still signs off on the consequential parts. You're not rewriting your operational knowledge from scratch — you're handing the assistant what you already have and getting back a reviewable, versioned, deterministic version of it.


Click to expand


### Save it, refine it, or ship it as code


Once the assistant generates a workflow, you choose what happens next, right from the preview:


- Save to Library


Keep it for later alongside your other suggested and created workflows.


- Create as Draft


Open it on the workflow builder canvas to refine the steps, parameters, and approvals before activating.


- Copy the Python SDK code


Grab the equivalent[Python SDK](https://usekestrel.ai/changelog/workflow-python-sdk) code if you'd rather deploy it programmatically or check it into a repo.


## Two on-ramps to the same canvas


Both paths converge on the same place: a complete, fully-editable workflow on the Kestrel canvas. Suggested Workflows is the fastest way to see what's automatable the moment you connect a new integration; the Workflow Assistant is the fastest way to turn a process you already run by hand into a deterministic automation. From either starting point, you refine in plain language, drag-and-drop, or code — and the agent that *builds* the workflow is never the thing that *runs* it, so once you activate, execution is fully deterministic. (For more on how building and running stay separate, see[Introducing Kestrel Workflows](https://usekestrel.ai/blog/introducing-kestrel-workflows) , and for what happens after a workflow goes live, see[Workflow Observability](https://usekestrel.ai/blog/workflow-observability) .)


To get started, open **Workflows → Library** and click **Suggest Workflows** for ideas tailored to your stack, or open the **Workflow Assistant** and hand it a runbook. The more you've connected — integrations, repositories, and knowledge sources — the richer the suggestions and the more accurately the assistant can turn your existing procedures into workflows.


### Start building with $1,000 in credits


Whether you start from a suggested workflow or upload your existing runbooks, you go from zero to a running automation in minutes. New accounts get $1,000 in usage credits to get started.


[Get Started](https://platform.usekestrel.ai/register)
