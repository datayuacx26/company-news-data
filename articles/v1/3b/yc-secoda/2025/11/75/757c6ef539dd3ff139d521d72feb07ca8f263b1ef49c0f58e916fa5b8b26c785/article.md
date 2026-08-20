---
schema_version: "1.0.0"
document_id: "757c6ef539dd3ff139d521d72feb07ca8f263b1ef49c0f58e916fa5b8b26c785"
company_key: "yc-secoda"
company: "Secoda"
source_id: "yc-secoda-news-import-8239d8ce1f4c"
canonical_url: "https://www.secoda.co/blog/workshop-recap-dashboards"
published_at: "2025-11-19T00:00:00+00:00"
first_seen_at: "2026-07-24T00:59:20.364488+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:12da09e9e28eebdd67534578945af9020c1e384cafd03f87bdc6148ab30a4163"
---

# Workshop recap: Build governed dashboards with Secoda AI

In our latest workshop, we showed how Secoda AI makes it easy to create and manage dashboards in minutes. Teams can build, edit, and share live dashboards directly from a chat without needing SQL or analyst support. Dashboards update automatically with connected data, keeping insights accurate and current. The result is a faster, simpler way to visualize key metrics and make decisions with confidence.


Whether it’s preparing a revenue overview before a meeting or tracking key data quality metrics week over week, Secoda AI makes dashboarding effortless and reliable.


### Check out the full recording[here](https://youtu.be/4P9L7z3wubA) .


## **Secoda AI overview**


[Secoda AI](https://www.secoda.co/ai-data-analyst) is the built-in assistant that understands your data’s full context including metadata, lineage, and past interactions, to help teams move from searching for answers to actually getting work done. Whether you’re managing governance and quality issues or just need quick insights, Secoda AI is built to reduce friction and make every workflow more efficient.


In the session, we walked through how teams can use Secoda AI across the following core areas:


- **AI Chats:** Ask any question in plain English and explore your data in real time. Secoda AI can write SQL for you, run quick analyses, and help uncover insights.
- [Suggestions](https://www.secoda.co/blog/secoda-ai-suggestions) **:** Secoda AI can proactively flag missing documentation, data quality issues, and governance gaps, helping teams maintain a healthy data environment automatically.
- **Charting:** Need a quick visualization? Secoda AI can[generate charts](https://www.secoda.co/blog/instant-insights-ai-generated-charts) on demand, turning text-based queries into instant visuals for clearer decision-making.
- **Agents:** Automate recurring analyses or governance checks with scheduled AI chats.[Agents](https://www.secoda.co/blog/secoda-ai-agents) run on a set cadence, saving teams time while ensuring regular insights.
- **Automation Blocks:** Keep metadata up to date without manual effort.[Automation blocks](https://www.secoda.co/blog/ai-automation-block-metadata-updates) use AI to generate and maintain descriptions, tags, and classifications as data evolves.


Together, these capabilities help both data teams and business users work faster with trusted, well-documented, and transparent data.


## **Introducing Dashboards in Secoda AI**


Building on that foundation, we introduced[Dashboards](https://www.secoda.co/blog/create-dashboards-with-secoda-ai) as the latest addition to Secoda AI’s toolkit for automating analysis and sharing insights.


### **What they are**


Dashboards are live, AI-generated reports that anyone in your organization can create and refine directly in Secoda. They pull from connected sources, show metric lineage, and update automatically, with no BI tool sprawl or manual upkeep required.


### **How they work**


Start by asking Secoda AI a question in plain English, like “Show me a dashboard on revenue and returns over the last quarter.” Secoda AI writes the SQL, runs the queries, and builds a dashboard complete with visualizations, metric details, and documentation. Users can toggle between the chart, SQL, and table views, making it easy to verify results or make quick edits.


### **Why they matter**


Dashboards eliminate the bottleneck between data teams and business users. They’re transparent, governed, and collaborative, helping everyone work from a shared source of truth.


Example use cases include:


- Automated team dashboards for tracking revenue, churn, or product performance
- Governed BI reports with built-in lineage and ownership
- On-demand visuals for quick decision-making before meetings


## **Demo: Building Dashboards with Secoda AI**


In the demo, we showed how to create a Finance Overview Dashboard in minutes.


**Step 1: Ask Secoda AI to create the dashboard.** We prompted Secoda to generate a report showing orders, revenue, and returns. Behind the scenes, Secoda AI wrote the SQL, ran the queries, and produced a live dashboard.


**Step 2: Review and refine.** Before saving, we clicked into the dashboard to review the metrics. Each visualization can be edited and customized. You can change chart types, adjust axes, or re-run queries, all within Secoda.


**Step 3: Add context and governance.** Once finalized, dashboards include:


- A **Documentation tab** for adding notes and explanations
- A **Lineage view** tracing dependencies across tables and metrics
- A **Questions tab** for users to collaborate or request updates
- **Slack notifications** for dashboard updates
- **Metadata visibility** to see owners, tags, and related resources


The result: dashboards that are fast to build, accurate by design, and fully traceable.


## **What makes Secoda Dashboards different**


- **Speed and accessibility** : Build and iterate on dashboards instantly, no SQL required.
- **Transparency** : Every dashboard shows the chart, SQL, and table view side by side, so anyone can trace where numbers come from.
- **Governance-first** : Dashboards follow existing Secoda permissions and lineage tracking, ensuring consistency and compliance.
- **Collaboration-ready** : Share dashboards, tag teammates, and sync updates via Slack or email to keep everyone aligned automatically.


## **Introducing AI Analytics**


[AI Analytics](https://www.secoda.co/blog/ai-analytics) gives leaders visibility into how AI is used across their organization, quantifying its real impact.


With AI Analytics, you can track metrics such as:


- Total number of AI chats across your organization
- Time savings across workflows
- Trends in AI usage and adoption over time
- The most active users and use cases
- Feedback on AI-generated outputs


It’s a dedicated workspace dashboard that lets teams move from assumptions (“we think AI helps”) to measurable proof (“AI saved 40 hours this month on reporting”).


## **Top 5 questions from the workshop Q&A**


**Question 1:** How do permissions work? Do they come from the source data?
**Answer:** Permissions in Secoda AI dashboards inherit directly from the source data, following the same unified permission model used across all Secoda assets.


Here’s how it works:


1. **Entity-Based Permissions:** Dashboards and metrics follow the same hierarchy as other Secoda assets, integrated into the unified permission model.
2. **Permission Inheritance Order:**


- Custom entity permissions (if set)
- Parent entity permissions
- Collection permissions
- Team memberships (from the entity, parent entities, and integrations)


3. **SQL Query Access Control:** When AI generates metrics, it checks the user’s permissions on source tables. If the` strict-run-sql-permissions` feature flag is enabled, users can only query tables they already have access to.
4. **Lineage Connection:** Each metric automatically creates lineage links to its source tables (` task_create_bi_metric_lineage` ), ensuring the permission chain remains connected to the underlying data.


This layered model ensures that every AI-generated dashboard and metric in Secoda remains secure, governed, and fully traceable back to its source.


**Question 2:** What happens if I want to change the chart type?
**Answer:** You can change the chart type in two ways. First, use the point-and-click configuration options directly in the dashboard. Second, you can ask Secoda AI in the conversation to change the chart for you.


**Question 3:** How are time zones handled in dashboards?
**Answer:** Dashboards use the time zone of your data warehouse. Whatever time zone setting your warehouse uses will apply to the query results in Secoda.


**Question 4:** How specific do prompts need to be when creating dashboards?
**Answer:** Prompts don’t have to be very specific. You can give Secoda AI a general question or topic, and it will generate a high-level dashboard. If you want more detailed or focused results, you can include specific resources or metrics in your prompt.


**Question 5:** How do you actually create a dashboard?
**Answer:** You create dashboards through Secoda AI by asking conversationally. Once AI generates the dashboard and you’ve reviewed the results, you can save it to your workspace to share with your team. Otherwise, it stays in the chat.
