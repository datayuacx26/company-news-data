---
schema_version: "1.0.0"
document_id: "3e1f77d66b877c936ff100d00b0ca0bcd09c24f2a72a126bd89198f023081920"
company_key: "yc-secoda"
company: "Secoda"
source_id: "yc-secoda-news-import-8239d8ce1f4c"
canonical_url: "https://www.secoda.co/blog/workshop-recap-ai-agents"
published_at: "2025-09-25T00:00:00+00:00"
first_seen_at: "2026-07-24T00:59:20.364488+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:b02db125b3420090652d0551bbdcf04a6fc11f6f33c15c36717b78a2920e9ce9"
---

# Workshop recap: Supercharge your workflows with Agents

In our latest workshop, we explored how[Secoda AI](https://www.secoda.co/secoda-ai) helps teams move beyond experimentation and bring AI into everyday work. Instead of relying on ad hoc queries or manual cleanup, Secoda AI connects context, metadata, and governance rules to deliver real impact through Chats, Agents, and Automation Blocks. From running weekly churn analyses to automatically generating documentation or tagging new tables, we showed how AI can take on the repetitive tasks that slow teams down. With these features, analysts and business users alike can move faster, get trusted answers, and scale their workflows with confidence.


#### **Check out the full recording**[here](https://www.youtube.com/watch?v=dhiR3VpOogU) **.**


## Secoda AI overview


Secoda AI was built to be a practical tool that data teams and business users can rely on daily, not just another tool to experiment with. That focus has paid off. Today,[76% of Secoda AI usage](https://www.secoda.co/blog/secoda-ai-usage-report-summer) happens in business intelligence and analytics, showing that teams are using it for meaningful, outcome-driven work rather than just testing it out.


Adoption has been strong because Secoda AI is designed for usability. It’s context-aware, pulling in metadata and lineage across the data stack, learning from past interactions, and even generating visualizations to speed up exploration. The clean interface makes it accessible to both technical and non-technical users, available directly in channels like Slack and Microsoft Teams. The result is clear ROI: teams are saving analyst time, getting insights faster, and enabling business users to self-serve with trusted answers.


Under the hood, this is all powered by a[multi-agent architecture](https://www.secoda.co/blog/technical-guide-secodas-ai-multi-agent-system) . Instead of relying on one generic model, Secoda AI works with specialized agents to handle tasks like writing SQL, searching, automating metadata updates, or running governance checks. A central orchestrator coordinates their work, ensuring responses are accurate, contextual, and continuously improving over time.


## Introducing[Agents](https://www.secoda.co/blog/secoda-ai-agents) in Secoda AI


- **What they are:** Scheduled Secoda AI chats that run automatically on a cadence, with full run history saved in the Chat interface.
- **How they work:** Create a custom Agent or use a template → save a prompt → pick a Persona → set frequency → results are delivered to your Secoda Inbox automatically. **‍**
- **Why they matter:** Instead of re-asking the same questions, Agents provide ongoing updates without manual effort.


**Example use cases:**


- Automating governance checks (e.g. scanning Snowflake schemas monthly and suggesting classifications/tags)
- Delivering recurring business metrics (e.g. weekly average order value reports)
- Sending weekly data quality reports to Slack or your inbox


## Demo: Setting up an Agent


In the demo, we walked through how to set up a weekly churn analysis Agent in Secoda AI:


- **Getting started:** From the Agents tab, click the plus button to create a new Agent. Choose either a template or custom setup, write a descriptive prompt, and set the schedule (hourly, daily, or weekly).
- **Running the Agent:** Agents execute as Chats, just like a conversation with Secoda AI. Behind the scenes, they call the right sub-agents, run SQL, and generate a full report.
- **Extra features:**


- Filter Chats to distinguish between human-triggered vs. Agent-triggered runs
- Open any run to view agent “thinking steps” for full transparency into queries and outputs
- Configure results to notify your Secoda Inbox, Slack, or email so the right people stay informed


- **The output:** Agents provide key findings, interactive charts, and actionable suggestions.


Instead of repeatedly running ad hoc queries, teams can set recurring analyses on a cadence, giving business users a reliable pulse on core metrics without depending on engineers.


## Introducing AI automation blocks


[AI automation blocks](https://www.secoda.co/blog/ai-automation-block-metadata-updates) are reusable building blocks that let AI handle repetitive metadata updates. This includes tasks like documentation, descriptions, PII tagging, classifications, and ownership assignment. These tasks can pile up as new tables, columns, and resources are created, pulling focus away from more strategic work.


With automation blocks, you can set up a workflow once and let Secoda take care of it automatically. Instead of relying on a team member to remember weekly cleanup tasks, AI ensures your metadata stays up to date in the background.


## Demo: AI automation blocks


In the demo, we showed how to set up an AI automation block for documentation:


- **Setup:** From Automations, click *Create automation* → *Start from scratch* .
- **Filtering:** Select the resources to update. Filters can be as specific as integration and resource type.
- **AI block:** Choose the field type (e.g. *Description* ). Preset instructions are available, or you can customize them for needs like BigQuery documentation.
- **Preview:** Test the AI output on up to three tables before running to ensure the prompt works as expected.
- **Run:** Create the output, toggle it on, and run the automation. A run history tab stores past results for easy review.
- **Extras:**


- Automations run on your set cadence, keeping metadata up to date in the background.
- Optional human review can be switched on, so updates are vetted before being applied.


The result: AI handles routine metadata tasks continuously, giving your team more time to focus on high-impact work.


## The complete system for AI adoption


Secoda AI works as a three-layer system designed to scale how teams adopt AI in their data stack:


- **Chats** → Real-time analysis and exploration. Business users can ask questions in plain English and get trusted answers in seconds.
- **Agents** → Turn one-off chats into recurring workflows that run on a schedule, delivering results straight to your inbox or Slack.
- **Automation blocks** → Run in the background to keep metadata, documentation, and governance tasks continuously updated.


## Top 8 questions from the workshop Q&A


**Q1. Is there a way to visually identify agent-generated responses versus manual prompts in the chats UI?**


**A:** Yes. There are two indicators:


- A blue marker appears in the chat once an agent runs, letting you revisit the agent’s response.
- A filter in the top-right corner allows you to switch between human and agent-triggered responses.


**Q2. Can we convert the Agent response to a PDF file?**


**A:** Not out-of-the-box today, but you can copy the results and paste them into a document that lives within Secoda. You can also export those documents to a PDF.


**Q3. Can we send the Agent analysis results automatically to an email or Slack?**


**A:** This capability is currently in testing on our end. Within the week, you’ll be able to automatically publish agent results to Slack, email, and Teams.


**Q4. Can we push the analysis results through API?**


**A:** API push isn’t available yet but is under consideration for the future.


**Q5. How can we make sure an AI Automation does not overwrite existing descriptions that were manually tweaked?**


**A:** You control this through an "Override existing properties" toggle in the Edit Resources action. If that's off, it will not override existing descriptions. The filter block also allows for you to control this proactively. For example, you might filter on "Description is not set" when generating AI descriptions, so you only target empty descriptions.


**Q6. Can the agent automatically update a document when it runs?**


**A:** Not yet. For now, results live in the chat, and users can paste them into documentation. Phase 2 will include direct updates to documentation and glossary terms.


**Q7. If we use Secoda AI for documenting a table, where does it get its context from?**


**A:** Secoda AI uses multiple system agents (search, lineage, query, context, policy, etc.) to gather insights. They pull from workspace assets, metadata, lineage, query history, glossary terms, and existing documentation. These system agents coordinate to analyze relationships, patterns, and governance rules. The result is documentation that reflects both technical details and your workspace’s business context.


**Q8. How do you ask the agent to validate results or provide examples to avoid hallucination?**


**A:** Secoda reduces hallucinations by combining transparency with guardrails. Every response includes the agent’s **“** thinking steps **”** , showing which sub-agents were used, what tools were called, and the exact queries that were run. On top of that, grounding the AI in certified tables and supplying detailed business context in AI settings with appropriate filters helps ensure responses stay accurate. By monitoring these steps and making small adjustments over time, teams can fine-tune results and minimize hallucinations significantly.
