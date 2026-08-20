---
schema_version: "1.0.0"
document_id: "c96c63c6c26bab2d285f43624c02e0f47e3cc96c66daad2447d81f537ef3efa9"
company_key: "yc-secoda"
company: "Secoda"
source_id: "yc-secoda-news-import-8239d8ce1f4c"
canonical_url: "https://www.secoda.co/blog/implementing-scalable-ai-workshop"
published_at: "2025-08-26T00:00:00+00:00"
first_seen_at: "2026-07-24T00:59:20.364488+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:4cec8c38580791b0fb6cbe236160d59632cfbc0578c9db66175a90f33207e336"
---

# Implementing scalable AI: A complete guide to using Secoda AI

In this workshop, we covered how[Secoda AI](https://www.secoda.co/secoda-ai) helps teams move faster and more independently, with real-world examples of how customers are using it today. Whether you're a technical user or a business stakeholder, the value of context-aware AI is already at your fingertips, and in this session, we showed how to unlock it.


Below is a quick recap of what we covered, with a spotlight on standout moments from the live Q&A at the end.


##### Check out the full recording[here](https://www.youtube.com/watch?v=rv4r6rP0y40) .


## Automating documentation with Secoda AI


Manual documentation is one of the most time-consuming tasks for data teams. With Secoda AI, we showed how this process becomes significantly more efficient and scalable.


#### Auto-documenting metadata in the Catalog and resource side sheet


With Secoda AI, users can auto-generate documentation for data resources, either individually or in bulk, using the one-click AI description editor. Because Secoda AI is available directly in the resource side sheet, users can also generate documentation on demand using prompts. Just describe what you need (e.g. tailored to a specific audience or with certain requirements), and paste the generated text into the Documentation tab for that resource.


#### Glossary term suggestions


**** Secoda AI identifies gaps in your glossary based on your[data catalog](https://www.secoda.co/data-catalog) and usage. It suggests high-priority terms, like customer lifetime value or data lineage, and pre-populates definitions to speed up knowledge standardization across teams.


#### Generating SQL


Secoda AI helps users write and troubleshoot SQL, recommend join strategies, and even suggest business use cases for those queries. This lowers the barrier for less technical users and saves time for analysts by eliminating repetitive SQL tasks.


## Performing analysis with Secoda AI


We walked through how teams can use plain language to run analyses,[visualize data](https://www.secoda.co/blog/instant-insights-ai-generated-charts) , and extract insights, without building a dashboard or writing code. This capability is one of the most powerful applications of Secoda AI and is especially valuable for teams that need quick answers without heavy reliance on data analysts.


During the demo, we showed how a user can simply ask, *“What is the distribution of order sizes this year versus last year?”* Secoda AI automatically interpreted the request, wrote the SQL, queried the data, and returned a clean chart showing year-over-year comparisons. It also provides a narrative summary that highlights trends and outliers.


What makes Secoda AI stand out is its ability to remember the previous question and build on it. For example, you can follow up with:


*“Does order value increase with order size?” “Can you segment by loyalty status?”*


AI understands the ongoing thread of analysis and adjusts the query accordingly, saving users from rephrasing or restating context.


Instead of relying on pre-built dashboards or waiting for analysts to create new views, business users can explore trends, test hypotheses, and validate assumptions directly from the workspace. And since everything runs within the guardrails of Secoda (respecting[data access permissions](https://www.secoda.co/role-based-access-controls) and context), it’s safe and reliable.


Another major advantage is the way AI communicates results. Not only does Secoda AI show a visual, it also provides a written explanation of the findings, highlighting key takeaways and often suggesting what to do next. This empowers non-technical users to make sense of the data without digging through dashboards or raw query results.


This approach allows teams to shift from reactive reporting to proactive, conversational analysis. Whether you’re in product, marketing, finance, or sales, Secoda AI gives you the power to get answers on your own terms, and fast.


## Customizing AI settings


One of the most common questions we hear from customers is: *“How do we ensure AI is accurate, relevant, and secure for our team?”* That’s where Secoda’s customizable AI settings come into play. In this part of the webinar, we walked through how workspace admins can fine-tune Secoda AI to match team structure, data access policies, and use case requirements.


**Granular controls for safe access** Admins can restrict what Secoda AI has access to by limiting its scope to specific schemas, tags, or domains. For example, you might choose to exclude HR or finance tables from AI responses, or configure AI to only reference production data.


**Toggle specific AI features** You’re in control of what capabilities are available to your users:


- **Run SQL** : Turn this off if you want users to use AI for tasks like documentation and Q&A but not for running live queries and analysis.
- **AI**[memory](https://www.secoda.co/blog/smarter-conversations-advanced-memory-secoda-ai) : Toggle whether AI should remember past interactions for more personalized responses.
- **GetSecodaDocs** : Control whether AI can pull in responses from Secoda’s product documentation.


**Custom instructions for tailored responses** Admins can write their own prompts that tell AI how to behave. That could mean using a specific tone (e.g., concise, technical, beginner-friendly), referencing preferred documentation sources, and much more (feel free to get creative!). This ensures[AI output](https://www.secoda.co/blog/ultimate-guide-ai-outputs) is consistent with your team’s voice and standards.


**Assigning AI personas by team** Admins can assign different AI personas to specific groups. For instance:


- Data engineers may prefer a technical AI that uses SQL and metadata
- Business users might be better served by an AI that prioritizes plain-language summaries and documentation


This feature allows each team to experience Secoda AI in a way that’s aligned with their goals and experience level.


By leveraging these settings, teams can strike the right balance between enabling self-serve access to data and protecting sensitive information. These controls help foster trust in AI while allowing teams to experiment and scale their usage confidently.


## Adoption tips


Rolling out AI across an organization is as much about people as it is about features. In this section of the webinar, we shared strategies that have helped our customers successfully adopt Secoda AI and scale it across teams.


**Start small to scale fast** Begin with one high-impact team or workflow. This helps you demonstrate value quickly and build internal momentum before expanding to broader use cases.


**Assign AI personas early** Set your teams up for success by assigning the right AI persona based on their technical comfort level and day-to-day responsibilities. For example, customer success teams may benefit from more conversational, non-technical output, while analytics teams may want SQL-rich responses.


**Integrate AI into existing workflows** We recommend embedding Secoda AI into the tools your team already uses, especially Slack. The Slack integration makes it easy for business users to ask questions, get summaries, and share answers with minimal friction. This approach helps drive repeat usage without requiring a behavior change.


**Celebrate and socialize early wins** Share success stories internally when a team member saves time, finds answers faster, or streamlines a repetitive task with AI. Highlighting these wins reinforces the value of Secoda AI and encourages others to try it out.


**Track usage and iterate** Use the AI usage analytics dashboards in Secoda to monitor adoption, identify gaps, and tailor enablement. Teams that proactively iterate based on feedback and usage patterns tend to see faster and broader adoption.


## Secoda AI Q&A highlights


We had a packed Q&A with excellent audience questions. Here are some of the most valuable exchanges:


**Q: Can I turn off SQL generation while keeping other Secoda AI features on?** Yes, Run SQL can be toggled off independently, so you can use Secoda AI for documentation, glossary work, and questions without enabling query execution.


**Q: What context does Secoda AI use to generate column descriptions?** Secoda AI pulls from your entire workspace including catalog, lineage, documentation, glossary, monitors, saved queries, questions/answers, your governance settings and even your usage patterns. You can also add custom instructions to guide tone or reference specific sources like SQL logic or glossary entries.


**Q: Can AI analyze data quality and detect issues?** Yes, Secoda AI can check for active monitors or incidents tied to specific tables and surface quality concerns. This allows users to ask, “How is this table doing?” and get a contextual, trustworthy response. Secoda AI also proactively suggests new monitors for assets that appear to be untracked or at risk, helping teams stay ahead of potential issues before they escalate.


**Q: How does Secoda prevent AI from hallucinating or returning incorrect results?** Secoda AI prevents hallucination by grounding every response in your organization’s real metadata and, when enabled, live data through SQL execution. It uses a retrieval-augmented generation (RAG) approach, meaning it pulls from your actual data catalog and documentation rather than relying on generic training data. Strict access controls ensure users only receive information they’re authorized to see, while admins can enforce additional filters and custom instructions to maintain accuracy.


Secoda also offers built-in evaluations of AI accuracy, visibility into usage analytics, and the ability to QA AI-generated questions. Human-in-the-loop workflows and narrower scopes, such as limiting AI access to specific resources, further improve the quality, trustworthiness, and control of AI outputs.


For a deeper look at how we’ve engineered a context-aware, trustworthy LLM experience, check out our[technical guide to Secoda AI](https://www.secoda.co/blog/ultimate-technical-guide-secoda-ai) .


## Final thoughts


Secoda AI is already helping teams get more out of their data by making documentation faster, improving collaboration, and enabling self-serve access in a way that sticks. But we know that adopting new capabilities takes more than just shipping features. It takes education, examples, and space to ask questions. That’s exactly why we kicked off this AI workshop series.


We’ll be hosting more sessions like this moving forward, so stay tuned. And if you have any questions about how to get started or how to get more out of Secoda AI, don’t hesitate to reach out to **[\[email protected\]](https://www.secoda.co/cdn-cgi/l/email-protection)** .
