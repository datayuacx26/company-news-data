---
schema_version: "1.0.0"
document_id: "7fed4633f3ea59863883ee6417e385bb7266ec3ad0bf6a11907792ad62236d55"
company_key: "yc-zapier"
company: "Zapier"
source_id: "yc-zapier-rss-44f77ec59be6"
canonical_url: "https://zapier.com/blog/ai-by-zapier-guide"
published_at: "2026-07-22T07:00:00+00:00"
first_seen_at: "2026-07-22T22:04:07.384258+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:27c7a4f575d9452e2c7ce02ac318af2729c18c7db6d0d5fdb67f3dc37b7b3c5d"
---

# AI by Zapier: Add agentic AI steps to your workflows

Guilty of[tokenmaxxing](https://zapier.com/blog/tokenmaxxing/) ? I get it. Now that AI is ubiquitous in all of our work, it's easy to let an agent harness take the reins for every workflow. But tossing your important tasks into the agent void and letting it run amok in your wallet isn't always the cheapest—or smartest—way to go.


Some work requires determinism, which means the same input produces the same output every time. Conventional automation handles that more reliably than AI. And it's also less expensive. There's no reason to pay agentic AI prices for work that logic and rules can do.


With[AI by Zapier](https://zapier.com/apps/ai/integrations) you can build workflows that combine if-then automation with AI exactly where it adds value. In this guide, I'll show you what it can do and how to build with it.


**AI by Zapier** is a built-in tool available on Pro[plans](https://zapier.com/pricing) and higher tiers. A standard step costs one task, an advanced step costs three, and a premium step costs five. Learn more about step tiers below.


### Table of contents


-


What is AI by Zapier?


-


What you can do with AI by Zapier


-


How to get started with AI by Zapier


## What is AI by Zapier?


AI by Zapier is a built-in tool that lets you add AI action steps to your Zap workflows. Without it, a Zap runs solely on if-then logic, consisting of a trigger (the event that kicks off your automation) followed by at least one action (whatever happens next). You can also add[paths](https://zapier.com/blog/zapier-paths-conditional-workflows/) and[filters](https://zapier.com/blog/filter-by-zapier-guide/) for conditional logic, and[code steps](https://zapier.com/blog/code-by-zapier-guide/) .


This kind of "traditional" Zap follows rules you set in advance. Even a path step, which splits a workflow into branches, branches the same way every time. An input always goes down the route you mapped for it.


Take this lead management workflow, for example. When a new response enters your form, the workflow branches in one of two ways. A budget under $20K creates a deal in HubSpot assigned to a small-business sales rep. A budget of $20K or higher creates a deal assigned to an enterprise account executive.


This visual diagram was created with[Zapier Canvas](https://zapier.com/blog/zapier-canvas-guide/) , our free built-in tool for diagramming your workflows.[See a larger image.](https://cdn.zappy.app/a676de8b0b82dc5387806fde85201c75.png)


Some work, however, calls for reasoning, not rules—and that's when you'd add AI by Zapier into the mix.


Instead of following logic you wired ahead of time, an AI step works out its response in the moment. In our lead management workflow, maybe you add an AI step before the path. AI reads whatever the lead typed into the open-ended "What are you looking to do?" field and evaluates it. Based on your prompt, AI might pick up on signals that they need help pronto or are just gathering info—then, it weighs that against their budget and scores the lead. The path then branches on that more nuanced assessment. A score of 1–5 routes to a small-business rep, and anything higher to an enterprise rep.


[See a larger image.](https://cdn.zappy.app/a676de8b0b82dc5387806fde85201c75.png)


In the rules-only Zap, a budget of $22K always ends the same way. With an AI step, a modest budget paired with an urgent, specific need might outrank a bigger budget from someone just kicking the tires. That's because the model weighs the whole response, not one field.


So far, I've been describing a *standard* AI step: one that reasons, produces one output, and doesn't touch your other apps. But you can also make an AI step *agentic* by giving it tools. AI itself would decide what to do—which tools to call, in what order, and how many times. ("Tools" are just Zapier app actions you let the step call at runtime, like *Send Channel Message* for Slack or *Find Contact* for HubSpot, along with web search and knowledge source lookups.)


For our lead management workflow, that might look like giving the AI step a Slack search tool. That way, it can pull up how your team handled similar accounts in the past and drop a quick summary in the deal notes. But you can get way more complex than that, adding web search so the step can pull recent news on the lead's company, or connecting a knowledge source so your reps get talking points straight from your own sales playbook.


Standard or agentic, every AI step runs on a model. And AI by Zapier lets you pick the one you want, from one of three tiers. The cost-effective standard models work great for simple tasks. The advanced models can handle more complex prompts. And the premium models can run deep analysis and multi-step reasoning. I know that "simple," "complex," and even "deep analysis" can be subjective terms, so later in this guide, I'll shareexample use cases for each tier .


You don't even need a separate AI account to use AI steps. Several models are included for free, like GPT-5 mini from OpenAI and Gemini 2.5 Flash from Google. You've also got access to the newest frontier models. For extra[security](https://zapier.com/security-compliance) , you can even bring your own key through Amazon Bedrock.


Key features include:


-


**AI-assisted building:** Ask[Zapier Copilot](https://zapier.com/blog/zapier-copilot-guide/) , our built-in AI assistant, for help with setup—like deciding which tier fits your task or wording your prompt effectively.


-


**Output fields:** AI by Zapier automatically structures its results into output fields you can map into later Zap steps. Plus, you can define your own fields to get exactly the data you need.


-


**Preview regeneration:** Refining prompts is an iterative process. For a quick feedback loop, edit your instructions, then press a button to instantly see the updated result. There's no need to run the whole Zap to test an edit to your prompt.


-


**Model selection:** Choose among the latest AI models from[OpenAI](https://zapier.com/apps/chatgpt/integrations) ,[Anthropic](https://zapier.com/apps/anthropic-claude/integrations) ,[Google](https://zapier.com/apps/google-ai-studio/integrations) , and[Azure OpenAI](https://zapier.com/apps/azure-openai/integrations) . Several are included for free, and you can bring your own key through Amazon Bedrock. Choose from standard, advanced, or premium models, picking the tier that fits the task, so you don't pay premium prices for simpler work.


-


**Knowledge sources:** Bring context into AI steps by connecting tools like Google Drive, Notion, and[Zapier Tables](https://zapier.com/blog/zapier-tables-guide/) . AI by Zapier can reference synced documents, pages, or files to ground its responses.


**Pro tip:** Want to bake an extra layer of security into your AI by Zapier workflows? Try adding an AI Guardrails by Zapier step. It's a built-in tool for detecting PII, toxic language, prompt injection attempts, and negative sentiment.[Learn how it works in our feature guide.](https://zapier.com/blog/ai-guardrails-guide/)


## What you can do with AI by Zapier


What can AI do for your workflows? You're limited only by your imagination. If you need a little inspo, check out these use cases, each one tailored to the model tier that fits the job.


### Data capture (standard tier)


You want to copy updates from your chat app into new, clean records in a spreadsheet.


**What this might look like:**


1.


A team member posts a project update or status note in a channel.


2.


AI by Zapier extracts the details from the message, like the project name, a summary of the status, and the owner.


3.


A new row is automatically written to your tracking spreadsheet with those details.


[Try it in the Zap editor](http://zapier.com/editor)


### Customer support (advanced tier)


You want AI to classify incoming requests and pull from your knowledge base to draft an on-brand response.


**What this might look like:**


1.


A customer submits a support request through your ticketing portal.


2.


AI by Zapier classifies the ticket by intent, issue type, and complexity, then uses your connected knowledge sources to draft a reply in your preferred tone.


3.


A path step reads the classification and assigns the ticket to the appropriate team's queue.


4.


The drafted reply is saved directly in each ticket for human review.


[See a larger image.](https://cdn.zappy.app/46aeb9fcef9fa05cf57724ef8ba721d9.png)


[Try it in the Zap editor](http://zapier.com/editor)


### Deal intelligence (premium tier)


You want to capture every meaningful detail from sales conversations and turn them into structured records and next steps.


**What this might look like:**


1.


A sales call recording lands in your meeting tool.


2.


AI by Zapier analyzes the transcript using multi-step reasoning—extracting contacts discussed, commitments made, objections raised, next steps, and close signals—then cross-references that against your existing deal record.


3.


The structured fields are logged to your CRM.


4.


A summary with recommended follow-up actions is posted to your sales team's channel.


[Try it in the Zap editor](http://zapier.com/editor)


Discover more use cases, learn what role AI should play in your workflows, and find governance strategies for an evolving AI stack in the[AI Workflow Index](https://zapier.com/ai/workflow-index/q2-2026-report) , Zapier's data-backed report on how leading companies use AI with automation.


## How to get started with AI by Zapier


1. Log in to your Zapier account, then either create a new Zap in the[Zap editor](http://zapier.com/editor) or modify an existing one.


2. Create your trigger step first. Search for and select your trigger app, select a trigger event, then connect your account. Once that's set up, customize any necessary fields and test the step to make sure it's working properly. Then click **Continue** .


3. Add an action step and select **AI by Zapier** from the list of built-in tools.


4. Start editing your AI step by clicking **Use a template** to begin with a pre-written prompt. Or write out your prompt from scratch in the text field.


5. To[map data from a previous step](https://help.zapier.com/hc/en-us/articles/8496343026701-Send-data-between-steps-by-mapping-fields) , type a forward slash (/) in the text field. A modal will appear with options for all the information you can use. In this example, I want to map data from my trigger step—a new Slack message in a project updates channel—so I'll click the appropriate field in the modal.


6. In the text field, you can select your preferred model tier from a dropdown menu. Choose **Auto** to let Zapier pick the best model for the task, or specify the exact model you want.


When you add an AI step to your workflow, you get to choose which model powers it. Not sure which to pick? Check the leaderboard for[AutomationBench](https://zapier.com/benchmarks) , Zapier's open benchmark that tests models on real business workflows. You'll get an up-to-date view of exactly which models excel at which kinds of tasks.


7. If you select the **Advanced** or **Premium** tier, you'll be able to toggle on web search or add other tools.


8. When you click **Add tool,** a modal will appear with options to add an app action…


…or a knowledge source.


Add as many as your AI step needs, then return to the main AI step configuration page.


**Tip:** If a step hits 75 tasks, your Zap will pause. That's approximately 74, 24, or 14 tool calls for the advanced, premium, and bring-your-own-key tiers, respectively.


9. From the configuration page, you can adjust your output fields (or add more), choose to return the output as line items, and preview the step output.


10. If you need help coming up with a prompt or selecting a tier, or you have other questions about your AI step, click **Ask Copilot.** This will return you to the Zap editor and begin a chat with our built-in AI assistant, Zapier Copilot. (Everything you've entered in your AI step will be saved.)


11. If you're still on the AI step configuration page, click **Continue** to return to the Zap editor and keep building your workflow. From here, you should add additional steps to send your AI-generated content wherever you intend it to go.


Need more help? Read our[AI by Zapier quickstart guide](https://help.zapier.com/hc/en-us/articles/8496342944013-Use-AI-by-Zapier-to-add-a-built-in-AI-action) for step-by-step instructions on adding AI actions to your workflows.


## Bring agentic AI into your automated workflows


It takes minutes to set up an AI step that completely transforms the way you do business. **Give**[AI by Zapier](https://zapier.com/apps/ai/integrations) **a go today to see what you can create.**


*This article was originally published in March 2025 and was previously updated by Elena Alston. It was most recently updated in July 2026.*
