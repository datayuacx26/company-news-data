---
schema_version: "1.0.0"
document_id: "1493d36d40c28617790691bd70734919e0141fbdf9b885c975109fad09169d8b"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/devops-slack-agent-cosmic"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:92195bdfb07959ad5e48dd14e3a6d72af37cc10c525302d8878e39b2bc44767b"
---

# Build a DevOps Slack Agent with Cosmic: From "What Broke?" to PR in One Conversation

If you've ever been paged at 2am, opened Slack, typed 'what broke?' and then spent 20 minutes switching between terminals, dashboards, and GitHub tabs to figure out the answer — this tutorial is for you.


We're going to build a DevOps agent that lives in your Slack channel. When an engineer asks 'what broke in prod?', the agent:


1. Pulls recent access logs from your Vercel deployment
2. Identifies the error pattern (500s, failed routes, console exceptions)
3. Locates the relevant file in your GitHub repo
4. Creates a feature branch, commits a fix
5. Opens a PR with a clear description
6. Posts the PR link back in Slack — in the same thread


All of this happens in a single conversation turn. No context switching. No separate tools.


## What You're Building


This is a Cosmic Team Agent with four capabilities enabled:


- *CMS Read* — so it can reference your content model if needed
- *Code Read* — to browse your repo, read files, check deployments, and pull access logs
- *Code Write* — to create branches, commit files, and open PRs
- *Send Notifications* — to post back to Slack with structured results


The agent connects to your Slack workspace and a GitHub repo via Cosmic's native integrations. No custom webhooks. No third-party glue.


## Prerequisites


Before you start:


- A Cosmic account (free tier works:[cosmicjs.com](https://app.cosmicjs.com/signup) )
- A GitHub repo connected to a Vercel project
- Slack workspace with the Cosmic Slack integration installed (Bucket Settings > Integrations)


## Step 1: Create the Team Agent


Go to your Cosmic project and click *Team Agents* in the sidebar. Click *Create Team Agent* and fill in:


**Name:** DevOps Agent (or give it a human name — 'Morgan', 'Sam', etc.)


**Persona prompt:**


```text


```


**Capabilities to enable:**


- Code Read (gives access to , , , )
- Code Write (gives access to , , )
- Send Notifications (for posting structured Slack messages)


**Memory:** Set to *Persistent* so the agent remembers recent incidents across sessions.


## Step 2: Connect Your GitHub Repo


In the Team Agent edit form, look for the *Repository* section. Select your connected GitHub repo from the dropdown. If you haven't connected it yet, go to *Project Settings > Integrations* and add your GitHub repo first.


Once connected, the agent has access to all Code Read and Code Write tools scoped to that repository.


## Step 3: Connect Slack


In the *Channels* section of the agent form, enable *Slack* and select the channel you want the agent to live in. A , , or channel works well.


Optionally enable *Only respond when @mentioned* if you want the agent to stay quiet unless explicitly tagged. For a dedicated incidents channel, leave this off so the agent can respond to any message.


Save the agent.


## Step 4: Test It


Go to your Slack channel and send a message:


```text


```


The agent will:


1. Call with and
2. Parse the response for error patterns
3. Reply with a structured summary in Slack


Here's an example of what a response looks like:


```text


```


## Step 5: From Diagnosis to PR


After diagnosing the error, ask the agent to fix it:


```text


```


The agent will:


1. Call on
2. Identify the null-check that's missing
3. Call to create
4. Call with the corrected version of the file
5. Call with a clear title and description
6. Post the PR URL back in Slack:


```text


```


The entire flow — log pull, diagnosis, branch, commit, PR, Slack post — happens in under two minutes without the engineer leaving Slack.


## Step 6: Wire Up the Cosmic SDK (Optional)


If your app pulls content from Cosmic (blog posts, product pages, documentation), you can give the agent CMS Read capability so it can cross-reference content changes with production errors. For example:


```text


```


With CMS Read enabled, you can ask the agent:


```text


```


And it will cross-reference the access log timestamps with recently published objects to look for causation.


## Extending the Agent


Once the base agent is working, here are three natural extensions:


**Heartbeat schedule:** Enable a daily heartbeat at 9am that posts a summary of the previous 24 hours of errors to Slack automatically. No one has to ask.


**Event trigger on deploy:** Connect an inbound webhook to the agent's webhook channel endpoint. Configure Vercel to POST to that webhook after each deploy. The agent will automatically check post-deploy logs and flag any new errors introduced by the deployment.


**Escalation logic:** Add to the agent prompt: 'If you find more than 20 errors in an hour, send an email to the project owner immediately.' The agent has available and will follow this instruction.


## What This Demonstrates About Cosmic Agents


This tutorial shows a pattern that goes beyond the DevOps use case: a Cosmic Team Agent as an operational interface to your stack.


The agent isn't a chatbot with pre-scripted answers. It reads live data, makes decisions based on what it finds, writes code, and takes action in external systems — all while staying inside the communication channel your team already uses.


The same pattern applies to customer support (reads your docs bucket, escalates in Slack), content operations (monitors your CMS, fires workflows on publish), and competitive intelligence (runs on a schedule, posts findings without being asked).


Agents that act are fundamentally different from agents that answer. This is the distinction that matters.


## What to Build Next


Now that your DevOps agent is running, here are the next tutorials in this series:


- **Customer Support Agent (WhatsApp/Telegram):** A Team Agent connected to WhatsApp with CMS Read on your docs bucket. Tier-1 support automation with human escalation.
- **Competitive Intelligence Agent:** A Content Agent on a Heartbeat schedule that crawls competitor blogs and posts a Slack summary every Monday morning.
- **Localization Pipeline Agent:** An event-triggered Content Agent that auto-translates new blog posts into Spanish, French, and German on publish.


---


## Get Started


Ready to build your own DevOps agent?


[Sign up free — no credit card required](https://app.cosmicjs.com/signup)


Already have an account?[Go to Team Agents](https://app.cosmicjs.com/)


Want a walkthrough for your specific stack?[Book 15 minutes with Tony](https://calendly.com/tonyspiro/cosmic-intro)


Or read the full agent documentation:[cosmicjs.com/docs/dashboard/ai/agents](https://www.cosmicjs.com/docs/dashboard/ai/agents)
