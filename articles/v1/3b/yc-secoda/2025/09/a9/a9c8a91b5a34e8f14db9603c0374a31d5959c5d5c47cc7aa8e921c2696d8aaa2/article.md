---
schema_version: "1.0.0"
document_id: "a9c8a91b5a34e8f14db9603c0374a31d5959c5d5c47cc7aa8e921c2696d8aaa2"
company_key: "yc-secoda"
company: "Secoda"
source_id: "yc-secoda-news-import-8239d8ce1f4c"
canonical_url: "https://www.secoda.co/blog/secoda-ai-agents"
published_at: "2025-09-19T00:00:00+00:00"
first_seen_at: "2026-07-24T00:59:20.364488+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:6ec99d595525a96c61f58e799c989e1be2a1ace6f981bfdd2cef6d7558b96584"
---

# Meet AI Agents in Secoda

Data teams lose countless hours every week to repetitive governance work like bulk tagging tables, maintaining[lineage](https://www.secoda.co/data-lineage) , writing dbt models, or keeping documentation consistent. These tasks are necessary, but they pull engineers, analysts, and stewards away from high-value analysis and decision-making.


With Secoda’s new AI Agents, you can take automation to the next level. Agents combine conversational planning with execution, giving you the ability to request complex workflows in plain English and watch them run with full transparency and oversight.


## **What are AI Agents?**


AI Agents are scheduled AI chats that automate foundational[governance](https://www.secoda.co/data-governance) work. Each Agent runs on a daily, weekly, or hourly schedule, uses a defined persona and prompt, creates a new chat for every run, and logs results directly into your workspace. You can review past runs in a dedicated tab and track activity over time.


Agents can:


- Perform internal operations like tagging, metadata updates, and cataloging.
- Orchestrate external tasks like generating dbt models, creating dashboards, or updating documentation.
- Execute with configurable personas for different team contexts


You can run Agents directly from the Secoda web app or by simply prompting @Secoda in Slack. The experience is the same in both workflows. It’s easy to configure, quick to review, and effortless to execute.


Create an agents that runs a usage trends analysis for you on a recurring monthly basis


## **How do AI Agents work?**


### Step 1: **Create your Agent**


Open[Secoda AI](https://www.secoda.co/secoda-ai) and select the Agents tab. Start by creating an agent in one of two ways:


- Custom Agent: Build from scratch with complete flexibility
- Template: Choose from pre-built templates for common use cases


### Step 2: Configure your Agent


Set up your Agent with:


- **Agent name** A clear, descriptive title
- **Persona** Choose from your available AI personas
- **Prompt** Write the specific task you want automated in plain English
- **Schedule** Run hourly, daily, or weekly
- **Tools** Enable what the Agent can use in Secoda, such as tagging, glossary creation, documentation updates, or classification scans
- **Start time** Pick when the first run should begin


### Step 3: Schedule and monitor


Your Agent runs automatically on the interval you set. View run history and status in the Agents tab, and track activity over time. Each run creates a new chat with the selected persona and prompt for easy review.


### Step 4: Get results


Each Agent run logs results to your workspace and appears as a new conversation. You can access completed runs from the Agent’s run history or from your AI chat history, with full traceability.


This keeps you in control while eliminating the hours of repetitive effort.


## ​​ **How Agents work with Secoda AI chats**


[Secoda AI](https://www.secoda.co/blog/implementing-scalable-ai-workshop) now includes two powerful ways to get work done: chats and Agents. Each is designed for different needs, and together they cover both quick questions and repeatable workflows.


- **AI chats:** Use chats when you need fast answers or ad-hoc help. You can ask about a metric, request a query, or get clarity on your data without leaving your workflow. Chats are best for one-time questions and exploration.
- **AI Agents:** Use Agents when you want to automate ongoing tasks. Agents can scan your warehouse for PII every week, keep documentation up to date, or generate recurring reports for stakeholders. You give them instructions in plain English, review their plan, and they run the workflow on your behalf.


Together, chats help you explore, and Agents help you operationalize. You can start with a simple question in a chat, then turn it into a recurring Agent to ensure the task is handled automatically going forward.


## **Example workflows for data teams**


AI Agents are designed to take natural language instructions and translate them into concrete actions inside Secoda. That means you don’t need to write code or manually repeat the same steps across your workspace. Here are a few examples of the kinds of tasks you can hand off to an Agent:


### **Metadata updates**


- “Generate descriptions for all tables in the analytics schema.”
- “Update docs for our most-used tables missing descriptions.”


### **Data discovery & cataloging**


- “Find new tables this month and create catalog entries.”
- “Discover undocumented sources and suggest owners.”


### **PII & compliance classification**


- “Scan all customer columns for potential PII and apply appropriate classification tags.”
- “Audit for GDPR compliance and create workflows.”
- “Mark all customer info tables as Sensitive and notify the privacy team.”


### **Data contracts**


- “Create contracts for new Salesforce data with monitoring.”
- “Generate contracts for all customer schema tables.”


### **dbt models & documentation**


- “Create dbt models for customer health scores and update lineage.”
- “Generate staging models for payment data.”


## **Example workflows for business teams**


AI Agents aren’t just for data team members. They also make it easier for business users to ask questions, explore data, and generate insights without needing to write complex SQL or maintain documentation themselves. Here are some common ways business teams can use Agents:


### **Business metric analysis**


- “Analyze our customer churn rate and identify key contributing factors”
- “Show me revenue trends across all channels and explain anomalies”
- “Compare this quarter’s performance to last year with actionable insights”


### **Multi-source data discovery**


- “Find all data sources about enterprise customers and show relationships”
- “Discover what marketing data we have and how it connects to sales outcomes”
- “Locate all product usage data and explain how it relates to customer satisfaction”


### **Automated executive reporting**


- “Create our monthly business review with key metrics and trends”
- “Generate a board presentation showing customer growth and retention”
- “Build an executive report tracking our North Star metrics”


### **Query & analysis support**


- “Help me analyze customer lifetime value by acquisition channel”
- “Write a query to calculate MRR growth by segment”
- “Validate my cohort analysis approach and suggest improvements”


## **Why do Agents matter?**


AI Agents make it easier to manage the work that typically takes hours of manual effort. They combine intelligent planning, and automated execution into a single, repeatable workflow.


For example:


- A steward can ask an Agent to scan Snowflake tables for PII and automatically tag them for compliance.
- An analyst can request dbt models with documentation and lineage updates in one step.
- A manager can schedule Agents to catalog new tables every month so metadata stays current without extra effort.


Agents extend the value of existing Secoda features by turning natural language requests into actions. They give every team a reliable way to keep[governance](https://www.secoda.co/blog/data-governance-the-ultimate-guide) consistent as data grows.


## **The future of governance**


AI Agents shift governance from reactive cleanup to proactive, automated workflows. They reduce manual overhead for data producers, give consumers reliable and compliant answers, and keep visibility and control with your team.


AI Agents are a natural next step in modern data governance: proactive, efficient, and designed to scale as your organization grows.


Learn more about how to get started with[AI Agents](https://docs.secoda.co/features/ai-assistant/secoda-ai-agents) .
