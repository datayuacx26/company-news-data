---
schema_version: "1.0.0"
document_id: "0de59468ffa6de821a5a6849ce0f121dcd920c259599227dadd809d128186743"
company_key: "yc-zapier"
company: "Zapier"
source_id: "yc-zapier-rss-44f77ec59be6"
canonical_url: "https://zapier.com/blog/relay-alternatives"
published_at: "2026-07-20T05:00:00+00:00"
first_seen_at: "2026-07-20T23:24:20.076749+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:ca489e1971ca25f2e16797412ac678b88b92a8348194a09b9fed050fb86481aa"
---

# Relay.app is shutting down: How to export your workflows and move to Zapier

[Relay is shutting down](https://www.relay.app/) on August 15, 2026, for free users, and on September 14 for paid plans. Either way, if you built workflows, agents, or tables in Relay, the end is near.


The good news is that Relay has made it as easy as possible to transition your workflows to a new tool. Here's how to export your Relay.app data and rebuild those workflows in Zapier, plus what to expect from Zapier as a former Relay user. We'll also cover why Zapier is your best option compared to other Relay alternatives.


### Table of contents


-


Key dates for transferring your Relay data


-


Step 1: Export your Relay.app data


-


Step 2: Rebuild your workflows in Zapier with Copilot


-


Relay alternatives and why Zapier is the best fit


-


Relay shutdown FAQ


## Key dates for transferring your Relay data


If you have existing workflows in Relay, they'll keep running until the shutdown dates:


-


**August 15, 2026** for free users


-


**September 14, 2026** for paid accounts


Every Relay user should receive an additional 25,000 steps and 10,000 AI credits to help with the transfer process. Your Relay workspace, app credentials, and tokens will be deleted automatically once your wind-down period ends.


Full workspace exports can take up to 24 hours to generate, so it's best not to wait until the last minute to start the transfer process.


## Step 1: Export your Relay.app data


Relay has[made it easy for you](https://docs.relay.app/workspace-and-account/export-your-relay.app-data) to save your workflow information, run logs, and screenshots of your visual flows. Here's what to do to preserve your Relay.app data and get ready to rebuild in Zapier.


### Export your whole workspace first


In your Relay.app dashboard, go to *Workspace settings* and click **Export workspace data** .


This downloads a .zip file with a folder for every Workflow, Sequence, MCP Server, and Table in your workspace. Each folder includes:


-


A build prompt (as a .md file)


-


Run history (as a CSV)


-


A JSON file with the workflow's exact structure


-


A JSON file per workflow run


-


Table data as a CSV (where relevant)


Depending on how much you've built, this can take up to 24 hours to generate, and you'll get an emailed link once it's ready. That link expires in 48 hours.


### Grab the build prompt for each workflow you're rebuilding


Relay's JSON export represents the exact structure of a workflow, but it's written in Relay's internal format, so dropping it into another tool doesn't do much on its own. If you're rebuilding in Zapier, you'll want to start with build prompts instead, which are plain-language descriptions of each workflow that are designed to be handed to an AI assistant like[Zapier Copilot](https://zapier.com/blog/zapier-copilot-guide/) .


Build prompts are included in your workspace .zip, but you can also grab one for a single workflow. Open the workflow, then either choose **Generate prompt** from the shutdown announcement widget or click the overflow menu in the top right and select **Copy build prompt** .


Relay generates a detailed prompt describing the trigger, every step, and the logic connecting them. Click **Copy prompt** and paste it somewhere you can find later.


### Save screenshots of anything you want a visual record of


Screenshots aren't part of the data export, so if you want a picture of what a workflow actually looked like, open it and choose **Save workflow as screenshot** .


This generates images of the full workflow and each individual step, which you can download as a set or individually.


### Export run history if you need it for reference


If you want a record of how a workflow performed historically, open its *Runs* tab and export the runs as a CSV.


You can also open a single run and export it as JSON for details like execution history, AI chat logs, and error messages.


Run data won't transfer into Zapier, but it's useful if you're troubleshooting a workflow's logic before you rebuild it, or if you just want records for your own files.


## Step 2: Rebuild your workflows in Zapier with Copilot


Once you have your build prompts in hand, the easiest way to rebuild is with Zapier Copilot, Zapier's AI building assistant. Just describe what you want to build (or paste the workflow prompt from Relay), and Copilot proposes triggers, actions, and field mappings across Zapier's 9,000+


app integrations.


### 1. Open Copilot and paste your build prompt


If you don't have a Zapier account, create one and log in. Then, start a new Zap—Zapier's automated workflows—from your[Zapier dashboard](http://zapier.app/home) .


Paste the build prompt from Relay into the Copilot chat window. You can drop it in as-is, but it helps to add a line at the top telling Copilot what you want it to do with it, like "Rebuild this workflow using the trigger and steps described below."


If the prompt is long or covers multiple distinct stages, wrap sections in headers (using markdown or XML-style tags) so Copilot doesn't blend unrelated steps together. This is more important for complex workflows with several paths or conditional branches than for a simple two-step Zap.


### 2. Choose "Ask as you build" for your first few migrations


Copilot gives you two build modes: *Auto-build* , where it sets everything up and asks you to review at the end, and *Ask as you build* , where it checks in before making each change. For a straightforward two-step Zap, Auto-build is fine. But for anything with conditional logic,[human-in-the-loop steps](https://zapier.com/blog/human-in-the-loop-guide/) , or app connections you'll need to reauthorize, Ask as you build is worth the extra clicks the first time through so you can catch mismatched field mappings before they're baked in.


### 3. Review, connect your apps, and test


Copilot will map Relay's triggers and actions to Zapier equivalents, but you'll still need to authenticate each connected app on the Zapier side and confirm field mappings look right, especially for anything Relay handled with a custom formula or a reusable Sequence.


Once the Zap looks right, run a test using real data and compare the output against the run history you exported from Relay. Tweak the Zap's mapped fields as needed until the output matches your expectations. Then, publish the Zap.


## Relay alternatives and why Zapier is the best fit


As a die-hard Zapier user even before I convinced them to hire me, I'm biased—but Zapier isn't the only place you can land after leaving Relay.[n8n](https://zapier.com/blog/n8n-vs-zapier/) and[Make](https://zapier.com/blog/zapier-vs-make/) are popular options, and either can handle your existing workflows if you're a Relay user. But I'd argue neither of them will make the transition as painless as Zapier will.


n8n is built for engineering-led teams that want full control over their own infrastructure. It connects to roughly 1,500 apps, more than seven times what Relay offered, but most of that flexibility comes from writing JavaScript expressions and code nodes rather than clicking through a builder. That's the opposite of describing what you want in plain language and letting an AI assistant build it, which is what made Relay appealing in the first place.


And compared to Zapier, n8n's main drawback is who owns the operational risk. Zapier handles retries, error handling, and security patches automatically, and processes 3 billion workflow runs a month without you thinking about any of it. On n8n, all of that unglamorous work—plus the hosting bill—is on you.


Make is the closer cousin to Relay. Both are visual, node-based builders that reward users who are willing to learn the tool properly. Make connects to about 3,500 apps, which is more than ten times Relay's count, and its Routers, Iterators, and Aggregators give you branching logic. That power comes at a cost, though. Make's own team recommends around 19 hours of coursework before you build anything complex, and its AI cobuilder, Maia, is still in early access, so you can't yet describe a workflow in plain language the way you would in Relay or with Zapier Copilot.


Zapier is the more direct upgrade for what Relay users are looking for. Relay connected to roughly 200 apps, while Zapier connects to[9,000+ apps](https://zapier.com/apps) . Zapier Copilot was also built around the same plain-language approach Relay relied on, so rebuilding a workflow should be virtually painless. And the governance you'll eventually need, like granular permissions and approval workflows backed by a full audit trail, is already built for teams at very different scales, including 69% of the Fortune 1000.


Here's a detailed breakdown of how Relay and Zapier stack up for the features that matter most when you're rebuilding.


**Relay.app**


**Zapier**


Integrations


~200 apps


9,000+


apps


AI building assistant


Relay AI assistant, workflow- and agent-specific


Copilot, which works across Zap workflows, agentic steps,[tables](https://zapier.com/blog/zapier-tables-guide/) , and[forms](https://zapier.com/blog/zapier-interfaces-guide/)


Conditional logic


Path


[Path](https://zapier.com/blog/zapier-paths-conditional-workflows/) , plus step-level[filters](https://zapier.com/blog/filter-by-zapier-guide/) for text, number, date/time, and boolean conditions


Reusable logic


Sequence


[Sub-Zap](https://zapier.com/blog/sub-zaps-guide/)


AI agents


Agent


Agent (built with[AI by Zapier](https://zapier.com/blog/ai-by-zapier-guide/) )


Database


Table


[Tables](https://zapier.com/blog/zapier-tables-guide/)


Custom extensions


MCP Server


[Zapier MCP](https://zapier.com/blog/zapier-mcp-guide/)


Human-in-the-loop


Built-in AI output reviews


[Human in the loop](https://zapier.com/blog/human-in-the-loop-guide/) steps


Error handling


Basic


Autoreplay for failed steps, plus custom[error handling](https://help.zapier.com/hc/en-us/articles/22495436062605-Set-up-custom-error-handling)


Team permissions


Basic, up to 10 users on the Team plan


Granular[role-based permissions](https://help.zapier.com/hc/en-us/articles/39698983334797-User-roles-and-permissions-in-Team-and-Enterprise-accounts) , approval workflows, up to 25 users on the Team plan


Security and compliance


SOC 2 Type II, GDPR, OAuth-based SSO


[SOC 2 Type II/III, GDPR, CCPA, SAML SSO, SCIM provisioning](https://help.zapier.com/hc/en-us/articles/8496181993613-Security-and-Compliance)


Starting price


$19/month


$19.99/month


AI billing


AI credits (separate pool)


[Tasks](https://zapier.com/blog/what-is-a-task-in-zapier/) (AI and standard actions draw from the same pool)


But since you're on the Zapier blog, don't take my word for it.[Slate](https://zapier.com/customer-stories/slate-magazine) , a digital magazine, built an agentic workflow that generated over 2,000 leads in a single month, the same kind of agent work Relay was built around in the first place.[Great Day Improvements](https://zapier.com/customer-stories/great-day-improvements) used Sub-Zap workflows, the exact feature that replaces Relay's Sequences, to automate more than 500 appointment reminders a day and lift appointment rates by 41%. Both are proof that whatever you were building in Relay, Zapier can handle it.


Try Zapier free to start rebuilding your workflows before Relay's deadline hits.


[Try Zapier](https://zapier.com/)


## Relay shutdown FAQ


### **How long do I have to export my Relay.app data?**


Free workspaces lose access on August 15, 2026. Paid workspaces lose access on September 14, 2026. Full workspace exports can take up to 24 hours to generate, and the download link is only valid for 48 hours, so it's best to start the migration process ASAP.


### **Will my Relay workflows import automatically into Zapier?**


Not automatically. Relay's JSON export isn't directly compatible with Zapier's structure. The best option is to copy the build prompt Relay generates for each workflow, then have Zapier Copilot rebuild it from that description.


### **What happens to my Relay app connections and credentials?**


Relay will delete all app credentials and tokens automatically at the end of the wind-down period. You'll need to reconnect each app on Zapier's side when you rebuild.


**Related reading:**


-


[Zapier Copilot: Build multi-product systems with an AI assistant](https://zapier.com/blog/zapier-copilot-guide/)


-


[The best AI automation tools](https://zapier.com/blog/ai-automation-tools/)


-


[Zapier vs. n8n: Which is best for your organization?](https://zapier.com/blog/n8n-vs-zapier/)


-


[Zapier vs. Make comparison: Which is best?](https://zapier.com/blog/zapier-vs-make/)
