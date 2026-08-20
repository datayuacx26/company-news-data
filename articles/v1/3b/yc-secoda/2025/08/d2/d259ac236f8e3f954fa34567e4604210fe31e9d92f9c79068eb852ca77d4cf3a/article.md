---
schema_version: "1.0.0"
document_id: "d259ac236f8e3f954fa34567e4604210fe31e9d92f9c79068eb852ca77d4cf3a"
company_key: "yc-secoda"
company: "Secoda"
source_id: "yc-secoda-news-import-8239d8ce1f4c"
canonical_url: "https://www.secoda.co/blog/workshop-recap-automating-data-governance-secoda-ai"
published_at: "2025-08-26T00:00:00+00:00"
first_seen_at: "2026-07-24T00:59:20.364488+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:00bccb8c515c50834c3dd45ca28ee59fe7485bd809a0d46488256519d21766cb"
---

# Workshop recap: Using AI to monitor, document, and govern your data

In our latest workshop, we explored how Secoda AI can take the guesswork out of data quality, documentation, and[governance](https://www.secoda.co/data-governance) . Instead of starting from a blank slate, Secoda AI uses specialized suggestion agents to scan your metadata, usage patterns, and governance rules to recommend exactly what to monitor, what to document, and how to strengthen compliance. From identifying untagged PII fields to suggesting high-impact glossary terms and scoring governance maturity, these workflows show how AI can help data teams move faster, reduce manual effort, and improve trust in their data.


#### Check out the full recording[here](https://youtube.com/live/c2aAo2aKwZk) .


## What is Secoda AI?


[Secoda AI](https://www.secoda.co/secoda-ai) is a purpose-built, context-aware AI agent that works directly with your metadata, lineage, and documentation. Unlike generic AI tools, it is grounded in your actual data stack using Retrieval-Augmented Generation to deliver answers based on what is really happening in your environment, without ever using customer data to train models.


You can customize how Secoda AI behaves based on your team’s needs, tailoring AI assistants to different roles like analysts, engineers, or business users so answers are contextually relevant, while controlling how the AI interacts with your data through permission rules that match your governance[policies](https://www.secoda.co/policies) .


Over time, Secoda AI learns from previous interactions to refine responses and better support your workflows. With role-aware personas, governance controls, and deep integration into your[catalog](https://www.secoda.co/data-catalog) , Secoda AI can search, generate SQL, document metrics, define terms, visualize insights, and tag assets, all while tailoring responses to each user’s role and ensuring compliance.


## Data quality, governance, and glossary automation with Secoda AI


In this demo, we walk through new AI suggestion agents in Secoda AI that automate three key workflows:[data quality monitoring](https://www.secoda.co/data-observability-monitoring) , glossary term creation, and governance. These agents act as high-quality starting points, scanning your metadata, usage patterns, and documentation to surface actionable recommendations in seconds.


You’ll see how Secoda AI:


- Finds and organizes relevant customer tables by purpose, complete with usage recommendations.
- Flags untagged PII fields and suggests fixes to improve compliance.
- [Recommends data quality monitors](https://www.secoda.co/blog/secoda-ai-suggestions) with prebuilt SQL, thresholds, and schedules.
- Identifies high-impact glossary terms to standardize language across your organization.
- Scores governance maturity and delivers a prioritized plan for improvement.


The result is less manual work, faster execution, and a clear path to better data quality, consistency, and compliance.


## **End-to-end sales analysis workflow with Secoda AI**


In this demo, we show how Secoda AI can handle complex, multi-step analysis workflows that bring together data discovery, analysis, visualization, and documentation in one continuous flow. What would normally take hours across multiple tools happens in minutes inside Secoda AI.


You’ll see how Secoda AI:


- **Discovers relevant assets** by scanning across all connected schemas, integrations, and documentation to find and organize historical sales data, complete with a recommended approach for which tables to join.
- **Runs deep product revenue analysis** for a selected time period, paired with visualizations and contextual insights.
- **Generates ready-to-use SQL** for quarterly revenue analysis, calculating metrics like quarter-over-quarter growth and average order value.
- **Provides a glossary term** for the new metric with a clear definition, calculation method, and real examples.
- **Builds interactive visualizations** for metrics like revenue-per-order by region, along with a written analysis of the drivers behind growth or contraction.


The outputs include both high-level summaries for quick decision-making and detailed breakdowns for further investigation.


This workflow demonstrates how teams can go beyond basic table lookups and use Secoda AI to reason through problems, gather insights, spot risks, and deliver a complete, documented data story.


## **Top 8 questions from the workshop Q&A**


We had a packed Q&A with excellent audience questions. Here are some of the most valuable exchanges:


### **1. How is AI memory handled?**


**A:** Secoda AI uses[Advanced Memory](https://www.secoda.co/blog/smarter-conversations-advanced-memory-secoda-ai) , a two-layer system that combines Personal Memory and Workspace Memory. Personal Memory is private and user-controlled, letting you save preferences, frequently used facts, or instructions that guide responses. Workspace Memory is shared across your organization and maintained automatically, storing useful patterns from conversations like common fixes or proven tool sequences. The AI filters out irrelevant information, and you can guide what gets saved using thumbs-up or thumbs-down reactions in chat. Memory helps Secoda AI provide faster, more accurate, and context-aware answers without changing how search results are ranked.


### **2. How can we reduce AI hallucinations?**


**A:** Secoda AI reduces hallucinations by anchoring every response in verified workspace content such as glossary terms, documentation, and query history, and when enabled, live data through SQL execution. Using retrieval-augmented generation (RAG), it pulls only from trusted sources in your actual data catalog,[lineage](https://www.secoda.co/data-lineage) , and documentation instead of relying on generic training data, avoiding speculative answers. Strict access controls ensure users only see authorized information, and admins can apply additional filters or custom instructions to maintain accuracy.


Secoda also runs post-generation validation. For SQL queries, it automatically checks syntax before returning results. For lineage or governance suggestions, it cross-checks against actual metadata to confirm accuracy. Teams can further improve results by keeping documentation fresh, removing conflicting Q&A entries, and setting clear role-based instructions for AI personas. Human-in-the-loop workflows and scoping AI to specific resources provide additional safeguards, ensuring outputs are accurate, trustworthy, and aligned with governance policies.


For a deeper look at how we’ve engineered a context-aware, trustworthy LLM experience, check out our[technical guide to Secoda AI](https://www.secoda.co/blog/ultimate-technical-guide-secoda-ai) .


### **3. Does Secoda AI learn differently for each user role?**


**A:** Yes. As mentioned above, Secoda’s dual memory system includes:


- **User memory** : Captures an individual’s preferences, frequently used datasets, and interaction history.
- **Workspace memory** : Stores glossary terms, documentation, and other institutional knowledge shared by everyone.


This means a salesperson and an engineer in the same workspace could get different responses to the same question if their user memories and access differ.


AI personas build on this by tailoring behaviors for certain roles, keeping answers aligned with the user’s function.


### **4. What are some best practices for building AI personas and workflows?**


**A:** Here’s a list of best practices to follow when beginning your journey with AI personas and workflows in Secoda:


- Start with 2–3 core personas for key user types (Data Lead, Analytics Engineer, Data Analyst, Business User).
- Give each persona clear instructions and restrict access to relevant datasets.
- Align personas with team structures and refine based on feedback.
- Automate high-impact workflows like documentation generation, PII tagging, natural language to SQL, and data quality monitoring.
- Integrate into daily tools such as Slack and the[Chrome extension](https://www.secoda.co/blog/ai-powered-chrome-extension) to reduce context switching.
- Maintain governance with behavioral guidelines, feedback loops, and performance metrics.


### **5. Do you have any tips for better prompting when asking complex analytical questions?**


**A:** To get the most accurate, useful results from Secoda AI:


1. **Enrich your workspace context:** Keep glossary terms and documentation up to date, configure monitors and automations, and maintain lineage so the AI understands data dependencies and relationships.
2. **Be explicit in your prompts:** Instead of *“Why did revenue drop?”* , try: *“Compare Q2 2024 vs Q1 2024 revenue using the sales_fact table and list the top 5 product categories contributing to the change.”* Clear parameters help the AI focus on relevant datasets and logic.
3. **Use the @ symbol to direct the AI to exact resources:** When you know the specific table, dashboard, or metric, mention it directly in your question for more accurate context. For example: *“Can you tell me more about how the @Sales Dashboard was created and which resources it uses?”*
4. **Use role-specific AI personas:** An *Analytics Engineer* persona might return optimized SQL with technical details, while a *Business Analyst* persona would return summaries with a business focus.
5. **Provide feedback to improve accuracy:** Use the 👍 and 👎 buttons to let Secoda AI know when an answer is correct or off-target. For example, if it references the wrong table, correct it so future responses are more accurate.
6. **Start fresh every few questions:** Open a new chat every 3–4 questions to reset the AI’s context and avoid confusion from earlier prompts.
7. **Keep questions concise and direct:** Short, specific questions typically yield the best responses. Avoid vague or overly complex queries that could dilute the AI’s focus.


### **6. Does Secoda AI use all connected integrations as context?**


**A:** Yes, but in a targeted way. Secoda integrates with warehouses, BI tools, and workflow systems, but it only retrieves the relevant slices of information needed to answer the specific question.


For example, if you ask, *“Which tables feed the revenue dashboard in Looker?”* Secoda will search lineage metadata and documentation from the connected BI tool and warehouse, not every piece of content in your workspace. This selective retrieval improves accuracy and efficiency.


### **7. Can Secoda AI be enhanced with custom MCP?**


**A:** Yes. Secoda can act as an[MCP (Model Context Protocol)](https://www.secoda.co/blog/model-context-protocol-secoda) server, letting you connect it to tools like Cursor or Claude and run Secoda capabilities, such as executing SQL queries or retrieving metadata, directly in those tools.


MCP client support is also in the works, which would let Secoda pull context from other MCP-compliant tools automatically. This could enable more seamless workflows, like fetching schema changes or pipeline statuses from other systems without manual prompting.
