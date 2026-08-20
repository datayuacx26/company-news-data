---
schema_version: "1.0.0"
document_id: "f4fb2961e21a1b54b6ef192f30ef34a7fa819616f9199840b79b13633c3fe40a"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/launching-nao-automations/"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:728befeaa8f7e02453b5e8f39e6f6400ca51a1b9fac2cfbfc58fc6de9d69f532"
---

# Launching nao automations

[Blog](https://getnao.io/blog/) /


product updates


# Launching nao automations


Schedule your nao analytics agents to run in the background. Recurring reports, conditional alerts, and a new Feed to track all agent activity.


28 May 2026


By Claire Gouze


Founder @ nao


We just launched nao automations.


Your analytics agent now runs on a schedule - in the background, with zero manual effort. Set it up once, and it delivers analysis to your inbox or Slack on the cadence you choose.


## Why automations


Data teams run the same reports every week. Same queries, same charts, same Slack message. It's tedious, and it's exactly what an agent should handle.


With automations, your nao agent does the recurring work:


- **Scheduled reports** : "Every Monday, analyze our GitHub repo trends and email me the summary with charts."
- **Conditional alerts** : "Check daily if revenue dropped more than 10%. Only email me if it did."
- **Proactive monitoring** : "Watch for anomalies in our pipeline metrics. Notify Slack when something looks off."


The key difference from a cron job or a scheduled query: the agent *reasons* about your data. It uses the same[context layer](https://getnao.io/blog/how-to-do-context-engineering-for-data-teams) , the same tools, the same agentic loop as a regular nao chat. It generates charts, writes analysis, and makes decisions - like skipping a notification when there's nothing to report.


## How it works


### Creating an automation


From the nao UI, click **Create Automation** and configure:


1. **A prompt** - describe what the agent should do in plain English. This is the same as typing a question in chat, but it runs on autopilot.
2. **A schedule** - daily, weekly, or custom cron expression.
3. **Delivery** - choose email, Slack, or both. The agent sends the full analysis with charts and numbers directly to your inbox or channel.


You can also trigger a manual run anytime to test before scheduling.


### The agent run


When the automation triggers, the agent behaves exactly like a regular nao chat:


1. Assembles context from your project (schemas, rules, documentation)
2. Generates and executes SQL against your warehouse
3. Creates charts and analysis
4. Delivers results via your chosen channel


The difference is no one needs to be there. The agent runs, reasons, and reports back.


### Conditional logic


This is where automations get interesting. You can write prompts with conditions:


> "Check if it's Christophe's birthday. If yes, send an email. If not, do nothing."


The agent evaluates the condition, queries the data, and decides whether to notify. This turns automations into a lightweight alert system - no static thresholds, no pipeline to maintain.


## The Feed


We built a new tab in nao called **Feed** . Every automation run shows up here with a summary, so you have a single place to track all agent activity.


Think of it as your agent activity log:


- See which automations ran and when
- Read summaries of each run without opening the full chat
- Track unread messages since your last visit
- Monitor agent activity across your whole team


The Feed works for all agent runs - both automations and refreshed stories.


## Try it


Automations are available now in the open source version of nao.


1. Pull the latest:[github.com/getnao/nao](https://github.com/getnao/nao)
2. Go to the **Automations** section in your nao UI
3. Create your first automation and trigger a manual run
4. Set up a schedule and let the agent work for you


Full docs:[docs.getnao.io/nao-agent/chat/capabilities/automations](https://docs.getnao.io/nao-agent/chat/capabilities/automations)


Star us on GitHub if this is useful:[github.com/getnao/nao](https://github.com/getnao/nao)


## Related articles


[insights The Agentic Analytics Playbook is out Learn how to choose your harness, build your context layer, plan your rollout, measure success, and get examples from 7 real-life companies.](https://getnao.io/blog/agentic-analytics-playbook/)[product updates We're launching the first Open Source Analytics Agent Builder We're open sourcing nao — an analytics agent framework built on context engineering. Here's our vision for what comes after black-box BI.](https://getnao.io/blog/open-source-analytics-agent-launch/)[product updates nao has a new look We rebuilt the nao interface from the ground up. New home screen, a prompt queue, visible agent reasoning, redesigned charts and stories.](https://getnao.io/blog/nao-redesign/)


Claire


For nao team
