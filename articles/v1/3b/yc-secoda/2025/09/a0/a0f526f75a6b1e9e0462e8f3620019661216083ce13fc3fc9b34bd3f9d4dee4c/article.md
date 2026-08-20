---
schema_version: "1.0.0"
document_id: "a0f526f75a6b1e9e0462e8f3620019661216083ce13fc3fc9b34bd3f9d4dee4c"
company_key: "yc-secoda"
company: "Secoda"
source_id: "yc-secoda-news-import-8239d8ce1f4c"
canonical_url: "https://www.secoda.co/blog/secoda-product-news-september-2025"
published_at: "2025-09-16T00:00:00+00:00"
first_seen_at: "2026-07-22T12:57:57.585252+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:7b5722d3ae40b5c0146882c4f217391f7f679b8c277cfd726b2d736ba061938f"
---

# Letter from the CEO - September 2025

We’ve reimagined what[Secoda AI](https://www.secoda.co/secoda-ai) can do and the impact is clear. Traditional AI tools often struggle with business intelligence because they misinterpret queries, return irrelevant results, or fail to connect insights to actionable next steps. Secoda AI has solved these challenges. It’s faster, more accurate, and has become the AI assistant that both data and business teams can rely on. That’s why 76% of all usage today is for business intelligence and analytics. It understands the question, runs the right query, explains what it found, and recommends what to do next. Now powered by a multi-agent system, the experience feels more like working with an analyst than a generic LLM.


What’s even more exciting is seeing the exponential growth in usage since these improvements rolled out. Over the last few months, we've shipped dozens of enhancements, from AI-powered charts to advanced memory. That momentum has driven a clear shift in user behavior. AI usage across workspaces has surged with weekly prompt volume multiplying as more teams roll out Secoda AI for faster analysis and insights. To make that growth more tangible, we ran an[analysis](https://www.secoda.co/blog/secoda-ai-usage-report-summer) of how customers are using Secoda AI and grouped the types of questions into benchmarks you can explore in the full report here.


With the latest improvements, Secoda AI bridges the gaps in traditional self-serve analytics, making data work conversational and accessible.


## **The problem with self-serve today**


​​Self-serve analytics tools are great at static analysis. If you want to check daily sales, a dashboard is exactly what you need. But the moment your curiosity goes deeper, these tools fall short. Questions like “How many of the canceled orders were from first-time customers versus repeat customers, and how did that affect lifetime value projections?” or “Did yesterday’s sales dip correlate with website downtime or slower page load times?” aren’t so easily answered in traditional analytics tools.


They don’t think like people do. They don’t have a conversation with you or follow the thread of your questions. While they respect permissions and can surface reliable dashboards, they aren’t built to carry context forward, adapt to different roles, or refine answers as you probe further. What’s left is a lot of guesswork, slower decisions, and a growing list of unanswered questions.


## **A system that just works**


Secoda AI understands what you’re asking and does the full analysis end-to-end.


It checks your permissions, selects the right tables, writes and executes live SQL in your warehouse, and explains the results in plain language. Then it goes further, suggesting follow-up questions to deepen the analysis.


This is more than “chat with your data.” Where most tools stop at a single query response, Secoda AI understands business context, respects governance rules, and adapts to different roles. It acts like an analyst that can keep up with your curiosity, not just a chatbot pointing at data.


The best part? Teams are already using it in ways that go far beyond simple Q&A.


[Dialpad](https://www.secoda.co/customers/dialpad-uses-secoda-core-layer-ai-infrastructure) used Secoda AI to overhaul how they analyzed active deals and competitive trends.


Before, insights like “Who are we losing to?” or “What features do we need to build to win more deals?” required scattered data, manual cleanup, and hours of SQL work. Now, they can ask natural language questions about their documented opportunity tables directly in Secoda.


Secoda AI returns instant summaries, highlighting top competitors, common loss reasons, and product gaps, while recommending next steps. What once took days now takes seconds, and multiple teams can explore follow-up questions without analyst support.


As Jesu Joseph, Global Head of Data and Advanced Analytics, from Dialpad put it: *“We’re training the company to rely on dashboards for basic things. But for anything more sophisticated, use Secoda AI.”*


## **Under the hood: A multi-agent architecture**


Behind the scenes, Secoda runs on a coordinated[multi-agent AI architecture](https://www.secoda.co/blog/technical-guide-secodas-ai-multi-agent-system) designed to reflect how technical data teams solve problems.


Each AI agent has a specific role: writing SQL, checking governance policies, generating documentation, visualizing results, interpreting outputs, and more. A central orchestrator coordinates their work, breaking down complex questions and assigning each task to the right agent.


Ask a single question, and the system can:


- Find the right tables
- Run the analysis
- Flag gaps in governance
- Suggest monitors
- Update documentation
- Visualize insights
- Explain what it all means


Each step is powered by your metadata, access policies, and usage history. The system improves over time, adapting based on how your team interacts with it.


When a user asks a question, Secoda AI translates ambiguous language into structured queries. It runs those queries securely in your live warehouse and applies statistical methods to surface changes, trends, and anomalies. It explains the patterns in clear terms and recommends follow-up actions. It also brings in additional context from lineage, documentation, and previous questions to make the response more complete.


This layer of interpretation is what sets it apart. A chart showing a 12 percent drop in engagement is useful. But Secoda AI tells you when the drop started, what changed, which users were affected, and what to explore next.


It’s fast, but more importantly, it’s informed. It understands your environment, respects your governance policies, and aligns with how your team operates.


## **Introducing Secoda AI Agents**


If the multi-agent architecture is the intelligence behind Secoda AI, then[Secoda AI Agents](https://www.secoda.co/blog/secoda-ai-agents) are how you put that intelligence to work.


AI Agents give you the ability to configure custom agents that plan and execute complex, long-running tasks on your behalf. Instead of spending hours on repetitive cataloging, governance, or observability work, teams can describe what they need conversationally, and an Agent will take it from there.


For data producers, this means automation for the kinds of routine tasks that drain time and focus, bulk tagging, creating dbt models, setting up monitors, or maintaining lineage. For business users, it means going beyond simple Q&A to get curated, actionable results without needing SQL or system-level expertise.


The workflow is simple: You request an Agent in natural language, Secoda AI executes the task, delivers results or proposals back, and you decide what should be applied.


Here are a few examples in practice:


- **Tracing data issues** : An Agent can check upstream Snowflake tables, dbt transformations, and downstream Tableau dashboards to trace why a revenue dashboard is showing anomalies. It identifies the failure points, recommends fixes, and either executes them or notifies the right owners once approved.
- **Building models** : With a single request, an Agent can generate dbt models for customer health scores, update lineage, and surface the documentation automatically.


These Agents bridge the gap between data producers and consumers. They move work forward automatically, while keeping human oversight at the center of every step.


And while Agents shine at handling complex, multi-step workflows on demand, we also wanted to give teams a way to automate the recurring, everyday tasks that never stop piling up. That’s where AI Automation Blocks come in.


## **Introducing AI Automation Blocks**


To make metadata management easier to maintain and scale, we’re introducing[AI Automation Blocks](https://www.secoda.co/blog/ai-automation-block-metadata-updates) . With these blocks, teams can automate repetitive work like generating missing descriptions or tagging undocumented tables. They slot directly into your existing processes, so documentation and governance stay consistent without the manual upkeep.


The experience is built with control in mind. Each action combines custom inputs, flexible prompts, and previewable outputs. You can review changes, test them on a sample set, and then decide whether to approve individually or apply in bulk across your environment. What used to take hours now takes minutes, giving teams faster, reliable updates at scale.


AI Automation Blocks turn metadata maintenance from a distraction into a background process, keeping your catalog fresh, accurate, and dependable without constant oversight.


## **A complete system for AI adoption**


We built these three layers of Secoda AI because no single approach can solve the range of challenges data teams face. Some moments call for quick answers, others require complex workflows, and many need reliable background automation.


Aspect Agents (NEW)


Chats Automations


Purpose Execute long-running AI workflows Real-time Q&A + analysis


Rule-based updates + alerts


Interaction Request > plan > execute > response/proposals > approve Back-and-forth conversation


Background execution


Effort Medium High


Low


Best For Internal/external tasks (tagging, dbt, integrations) Exploring, problem-solving Bulk updates, governance


Output Analysis + progress + propsals Insights/answers


Metadata changes + notifications


Timing


On-demand or scheduled Real-time


Scheduled


This chart shows how we think about the layers of Secoda AI, with chats, Agents, and Automations working together. Each serves a distinct purpose, but they are designed to work best in combination.


- Chats give you real-time analysis and exploration, helping teams quickly answer questions or troubleshoot problems.
- Agents take on the more complex, long-running workflows by planning, executing, and surfacing proposals that move projects forward.
- AI Automation Blocks run quietly in the background, applying rules and updates at scale so your catalog stays accurate without constant intervention.


Together, they form a complete system. You can start with a chat to uncover an issue, spin up an Agent to address it, and rely on Automations to make sure the fix stays in place over time. It is a cycle of insight, action, and maintenance that helps teams scale governance, documentation, and analysis without trading off speed or trust.


## **Helping teams see what's possible with AI**


With the addition of Agents and AI Automation Blocks, we expect usage of Secoda AI to grow significantly across teams and workspaces. As more teams adopt these workflows, we wanted to give customers a way to understand how others are using Secoda AI and where they’re seeing the most impact.


We put together[this report](https://www.secoda.co/blog/secoda-ai-usage-report-summer) to help you answer questions like:


- What kinds of questions are being asked?
- How is AI being used across documentation, governance, monitoring, and analysis?
- Where are the quick wins happening?


Whether you’re just getting started or looking to expand usage, this report gives your team a window into what’s working, how others are applying Secoda AI, and where there may be untapped potential in your own workspace.


It’s one more way we’re making AI in Secoda more useful, more transparent, and easier to scale across your organization.


## **Final thoughts**


With a multi-agent system under the hood, AI-powered analysis that actually explains what’s happening, Secoda AI Agents that can plan and execute complex workflows, and Automation Blocks that eliminate repetitive work, Secoda AI is a trusted partner in everyday workflows. It’s fast and accurate, with the context and governance data teams need.


We’re excited about what’s next, and even more excited to see how your team puts it to work.
