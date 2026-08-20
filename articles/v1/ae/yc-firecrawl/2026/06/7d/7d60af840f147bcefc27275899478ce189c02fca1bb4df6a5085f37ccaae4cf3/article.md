---
schema_version: "1.0.0"
document_id: "7d60af840f147bcefc27275899478ce189c02fca1bb4df6a5085f37ccaae4cf3"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/cursor-automations"
published_at: "2026-06-20T00:00:00+00:00"
first_seen_at: "2026-07-21T20:06:32.273956+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:828b81d99dd7be3d2c74da51ac993df442718a750fae17be9bf2935195d0aff7"
---

# Cursor Automations 101: How to Build AI Workflows with Firecrawl

## TL;DR


- Cursor Automations let teams deploy AI agents to the cloud with the click of a button.
- Cursor Automations comes with a large variety of plugins and[MCP](https://www.firecrawl.dev/blog/best-web-search-mcp) tools.
- All Automations run on Cursor's cloud infrastructure by default. Deploy with a toggle button.
- Automations are expensive to test and run. They always run in Max Mode for larger context windows and increased performance.
- The Firecrawl MCP gives Cursor Automations access to reliable data pipelines.


---


## What are Cursor Automations?


[Cursor Automations](https://cursor.com/automations) are one of the newest tools available from Cursor. Using Cursor Automations, users can build,[schedule](https://www.firecrawl.dev/blog/automated-web-scraping-free-2025) , and deploy agentic tasks.


What makes this especially powerful: because automations run on Cursor's cloud, you can kick them off (and check their results) from your phone via[Cursor Web](https://cursor.com/agents) . That turns workflow management into something you can do from a coffee shop, an airport, or in line at the grocery store. No laptop required.


### A few Cursor Automations from the Firecrawl team


At Firecrawl, we're huge fans of Cursor Automations. They've quietly become part of how the team ships, markets, and monitors the product day to day.


[Hiba Fathima](https://hiba.fyi/) , Growth Marketing Lead at Firecrawl, has several Cursor Automations running today:


- **Blog refresh on every product changelog** : Each time Firecrawl ships a product changelog, an Automation scans existing blog posts for outdated references (deprecated features, old API limits, renamed endpoints) and queues up edits. Readers stop landing on posts with stale info.
- **Competitor intel to Slack** : Using the[Firecrawl Monitor](https://www.firecrawl.dev/blog/automated-competitor-price-scraping) feature, an Automation watches competitor pricing pages, changelogs, and landing pages. When something shifts, a summary lands in the team's Slack channel before sales asks about it.
- **Internal interlinking** : Whenever a new post goes live, an Automation scans the rest of the blog for natural linking opportunities and opens a PR with suggested interlinks. The site's SEO graph stays dense without a manual link audit each week.
- **And plenty more** : SEO keyword tracking, social drafts pulled from changelog entries, customer story scaffolding generated from sales call transcripts.


**Wait, how does this differ from regular AI agents?**


The selling point from Cursor Automations comes from the deployment architecture. Rather than running an agent on your own infrastructure, you deploy your agents to Cursor's cloud.


### What models do Cursor Automations support?


Teams can choose from a variety of mainstream model providers. Below are some notable model families.


- **GPT** : OpenAI's GPT series, including GPT-5.5 and GPT-5.3 Codex.
- **Claude** : Anthropic's Claude family, including Claude 4.6 Sonnet, and Claude Opus 4.8.
- **Gemini** : Google's Gemini family, including Gemini 3.1 Pro and Gemini 3.5 Flash.
- **Grok** : xAI's Grok Build 0.1.
- **Composer** : Cursor's in-house model, currently Composer 2.5.


Context windows can vary drastically between models. With Automations, models have optimized context windows for long-running tasks and modularity. When you swap between models, regardless of provider, stored memories allow new models to understand context and state. When a tool fails, or a context limit is triggered, AI agents can rebuild state using context from memory.


### What are Triggers?


Triggers control when an automation runs. An automation can have more than one trigger, and it runs whenever any of them fire.


Cursor Automations offers a variety of triggers.


- **Schedule** : Schedule your Automation to run hourly, daily, or weekly at a predetermined time.
- **GitHub/GitLab** : Launch the workflow after pull requests, comments, pushes, workflow completions, and more.
- **Slack** : Trigger your Automation with messages, reactions, and channel creations in Slack.
- **Sentry** : Trigger on issue created, updated, or any issue event in your Sentry project.
- **Linear** : Trigger on issue created, status changed, or end of cycle.
- **Webhooks** : Listen for webhooks and run your Automation when your system receives them.
- **PagerDuty** : When an incident arises, run your Cursor Automation.


## What are the primary uses for Cursor Automations?


Most Cursor Automations fall into one of two patterns: reactive runs that fire on an event (a PR, a Slack message, an incident) and scheduled runs that fire on a clock. The next two sections cover each.


### How do reactive Automations work?


Reactive Automations fire on an event from one of your tools: a pull request opens, a Slack message lands, a Sentry alert triggers. The agent picks up that context and runs without anyone kicking it off manually. The two highest-leverage use cases are reviewing submissions and triaging bug reports, which are also the patterns[Jack Pertschuk](https://www.linkedin.com/in/jack-pertschuk-833196114/) and[Jon Kaplan](https://www.linkedin.com/in/jkap/) lead with in their[Automations introduction video](https://cursor.com/docs/cloud-agent/automations) .


- **Reviewing submissions** : When someone submits a pull request, your phone goes off. You need to run integration tests. You need to look at merge conflicts. This is a real pain point for developers. With Cursor Automations, you go back to sleep and let your AI agent handle it.
- **Bug reports and triage** : When something breaks, AI agents can read messages to assist in bug tracking. In some cases, AI agents running on Automations can solve them entirely.


According to Cursor Engineer,[Lee Robinson](https://x.com/leerob/status/2067638101900484993?s=20) , Cursor's own Slack channel now has bots solving customer issues as well as bots reproducing and confirming fixes. Many X users commented wishing to do the same with their own projects. Robinson reports that Cursor used their SDK to implement this. However, with Cursor Automations, it's now possible to do this without code.


And this isn't just for engineers. Cursor Automations are surprisingly accessible to non-technical folks too.[Mark Mercer](https://www.linkedin.com/in/markrussellmercer/) , Chief of Staff at Firecrawl, recently started using Cursor and had his first Automation up and running in under 30 minutes, no prior coding experience required.


### What can scheduled Automations do?


The other big selling point from Cursor Automations is the scheduling trigger. Instead of learning how to write CRON jobs, your team can just build an AI agent and tell it to run at 9:00 a.m. daily. It costs more per run than a plain cron job, but it removes the DevOps bottleneck for teams without a backend engineer to spare. The same pattern shows up across other[low-code AI workflow automation tools](https://www.firecrawl.dev/blog/best-low-code-ai-workflow-automation-tools) : the scheduler is a UI toggle, not a config file.


- **Data pipelines** : Pipelines run regularly to refresh application data. Traditionally, teams need to write a scraper. They deploy it to a server, and a Linux expert schedules a CRON job. When a selector breaks, it's an emergency. It breaks the entire pipeline schedule. With Cursor Automations, an intelligent model is looking at the data when the task fires. If a selector breaks, it doesn't matter.
- **DevOps** : Teams no longer need a DevOps expert to deploy. Nobody needs Linux or the command line. Cursor Automations run in the cloud by default. To "deploy", you just need to toggle the Automation from the Cursor UI.


## Getting started with Cursor Automations


This section walks through how to use Cursor's Automations interface to build your first Cursor Automation. You can build one using either the Cursor Desktop App or the web version at[cursor.com/automations](https://cursor.com/en-US/automations) , which works from a laptop, iPad, or mobile phone. To get started, select "Automations" on the sidebar and click the "New Automation" button.


From here, you can begin work on your Cursor Automation. From the UI, you should be able to see a toggle to set the Automation's status to "Active" and a dropdown for optionally connecting a GitHub repository.


In the middle of the page, we have a text box to input instructions to the agent in natural language. We also have a model selector. Automations always run in[Max Mode](https://cursor.com/docs/models-and-pricing#max-mode) , which extends the context window to the maximum a model supports. There is no toggle to turn Max Mode off, so the model you pick directly drives both quality and cost.


Before testing anything, I strongly recommend looking at the[models and pricing](https://cursor.com/docs/models-and-pricing#model-pricing) page. Because Automations always run in Max Mode, the model you pick is the biggest cost lever, not the tools it calls. My first two or three test runs on GPT-5.5 racked up well over $0.40 in Cursor credits before I switched to a cheaper model. The good news: the[Firecrawl MCP](https://docs.firecrawl.dev/mcp-server) actually[cuts input tokens by ~94% compared to feeding raw HTML to the same model](https://www.firecrawl.dev/token-efficiency) , so the web-data side of the bill stays small even when the model itself is expensive.


For this demo, Cursor's Composer 2.5 works just fine at much lower cost.


## Connecting Cursor Automations to GitHub and Slack


GitHub and Slack are the two most common triggers and destinations for an Automation: GitHub fires the agent on a PR or push, and Slack is where the result lands. This section wires both up end to end.


### Connecting to GitHub


If your GitHub account isn't already connected to Cursor, you'll need to connect it before adding a repository to your Automation.


When connecting, you can choose to let the model access your entire GitHub or a specific repository.


Once GitHub is connected, you can add a trigger to your Automation. In this example, I create a trigger that fires whenever somebody opens a pull request.


### Connecting to Slack


Connecting your Cursor Automation to Slack is a similar process. Add the "Send to Slack" tool. A browser window should open, prompting you to give Cursor access to your Slack Organization. Follow the prompts and finish setting up the connection.


Cursor then prompts you to allow on-demand usage for Triggers.


Choose any public channel within your organization. Then, tag` @Cursor` to bring the bot into the channel.


### Creating the workflow


Write a description of the task and paste it into the "Agent Instructions" box.


```text
/explain the pull request and what's changing in the code. Send a summary in the #cursor-alerts Slack channel


```


Click the "Save" button. Here, I open a pull request to the repo. If everything is working correctly, this should trigger the Cursor Automation.


Immediately, I've got a Slack alert summarizing the PR. The "Open in Web" button will open specific details of this run in the browser. The "View Automation" button will open the Automation settings.


## Using Firecrawl with your Cursor Automations


If you want to use Cursor with Firecrawl, this is the section to follow. Not everyone needs GitHub or Slack integrations. Sometimes, live web access is the most important tool for an AI agent. With stale data, many[AI agents](https://www.firecrawl.dev/blog/ai-agents) are useless. Firecrawl agents — Cursor Automations equipped with the Firecrawl MCP — solve that by pulling fresh pages, search results, and structured extractions on every run. For a wider picture of what else you might wire up, see our roundup of the[best MCP servers for developers](https://www.firecrawl.dev/blog/best-mcp-servers-for-developers) .


[Firecrawl](https://www.firecrawl.dev/blog/firecrawl-101) provides access to a variety of web access tools for[web search](https://www.firecrawl.dev/blog/web-search-api) , scraping, browser interactions, crawling, and site[monitoring](https://www.firecrawl.dev/blog/automated-competitor-price-scraping) . Below, I wire up Firecrawl's MCP server so the agent can pull fresh data from the live web instead of relying on its training cutoff.


At the bottom of the page, click "Add Tool or MCP" to add the Firecrawl MCP server. The name and server URL are required. You can copy the server URL in the snippet below. Replace the API key with your Firecrawl API key. If you plan to add several MCPs to one Automation, our list of the[best MCP servers for Cursor](https://www.firecrawl.dev/blog/best-mcp-servers-for-cursor) is a good place to shop.


```text
https://mcp.firecrawl.dev/  <your-firecrawl-api-key>/v2/mcp
```


In the instructions box, I add the following prompt. It tells the AI model to find the top 10 SaaS keywords and return the information as JSON. I also tell it specifically to use Firecrawl for web access.


```text
Extract the top 10 technical marketing keywords specifically for the SaaS industry. Return the information as JSON. Use firecrawl for web access.


```


After that, you can test your Automation. When you run the process, Cursor first launches an environment for the AI agent. Then, it interprets your prompt and attempts to complete the task.


In total, our Cursor Automation took just over two minutes to run. The AI model returned a JSON object with fields including` keyword` ,` category` ,` description` ,` saas_relevance` , and` example_queries` .


```text
{
"rank"  :   1  ,
"keyword"  :   "Answer Engine Optimization (AEO)"  ,
"category"  :   "AI Search & Discovery"  ,
"description"  :   "Optimizing content and digital presence so AI assistants (ChatGPT, Perplexity, Gemini, Google AI Overviews) can find, understand, and cite your SaaS product when buyers ask solution questions."  ,
"saas_relevance"  :   "B2B buyers increasingly research software via AI before contacting vendors; AEO is widely cited as the evolution beyond traditional SEO for SaaS visibility in 2026."  ,
"example_queries"  :   [
"best project management software for remote teams"  ,
"CRM alternatives for small business"  ,
"how to reduce SaaS churn"
]
},
```


### Connecting an email tool


Next, I'll add email support. You'll need a[Resend account](https://resend.com/) . Once you've got an account, create an API key. Make sure the API key is set to "Full Access". Without it, the AI agent won't be able to read contacts stored in Resend.


Now, I'll tweak the prompt. I tell the AI model to send a summary email to all available contacts. To send to multiple contacts, you'll need to configure a domain. Since this is a demonstration, I'm using Resend's default settings.


```text


Using the Resend tool, send a summary email to all available contacts.


```


In the image below, the AI agent has now searched and found the top technical marketing keywords. It sent a summary to the available contacts (just me) using Resend. Teams should notice the sender address,` onboarding@resend.dev` . When sending in production, you need to configure Resend using either domains or a simple mail transfer protocol (SMTP) server.


### Managing memory and context across multiple runs


To add memory support across runs, simply tell the agent to store a memory. In the example below, I changed the prompt again. I tell the agent to save the results to memory and to compare them against previously saved results. I also tell the AI agent to send to only one email address, mine.


**When our agent can access the same memories across multiple runs, our data becomes easier to analyze and so is our model performance.**


```text
Use firecrawl for web access.


Extract the top 10 technical marketing keywords specifically for the SaaS industry. Save the returned information as a memory.


Using the Resend tool, send a summary email comparing the results of this run to the results of the last run.


Send the email to <insert-email-address>.


```


Since the model wasn't storing previously, I need to run it twice. After the second run, I received an email comparing it to the first run. As you can see below, two new keywords have surfaced.
