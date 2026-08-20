---
schema_version: "1.0.0"
document_id: "cdba3f1dcc6abf39d1b9bcd7ef28522e7363e36b8982207a1733feb00b91719a"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/ai-agents-examples"
published_at: "2025-08-28T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:285d29cafa79cbe85596bb55ae85b41801186c1060899b963d3e46bc717eb37d"
---

# 10 AI agents examples from top companies

AI agents are no longer a futuristic concept – they're actively reshaping how businesses operate today. Unlike simple chatbots, AI agents are sophisticated systems that can understand context, plan multi-step actions, and execute complex workflows with minimal human oversight.


From financial data analysis to customer support, leading companies across industries are deploying AI agents to automate routine tasks, enhance productivity, and scale operations that previously required extensive human intervention. Here are 10 examples of how organizations are leveraging AI agents to transform their business operations.


##### \[fs-toc-omit\]650+ real-world AI use cases


> These AI agents examples were selected from our database of AI use cases. You can explore the[full list here.](https://www.evidentlyai.com/ml-system-design)


## 💻 Uber's financial data agent


Uber built[Finch](https://www.uber.com/en-IN/blog/unlocking-financial-insights-with-finch/) , a conversational AI agent that streamlines financial data retrieval to give analysts faster access to the information they need. Integrated directly into Slack, Finch removes the need for manual SQL queries by transforming natural language into structured data.


The system is designed as a multi-agent architecture: when a finance team member asks a question in Slack, a Supervisor Agent routes the query to appropriate sub-agents like the SQL Writer Agent. These agents query metadata indexes, construct SQL queries, and deliver formatted results back to Slack with real-time status updates throughout the process.


To maintain accuracy, Uber employs rigorous testing, including:


- Agent accuracy to evaluate each sub-agent against a “golden” set of expected responses.
- Supervisor agent routing validation to test the choice of tools.
- End-to-end validation with simulated queries to ensure system-wide reliability.
- Regression testing to detect performance drifts before deploying updates.


*Finch data agent context-building flow. Source:*[Unlocking Financial Insights with Finch: Uber’s Conversational AI Data Agent](https://www.uber.com/en-IN/blog/unlocking-financial-insights-with-finch/)


## 🚚 Delivery Hero's product knowledge base builder


Delivery Hero[uses](https://tech.deliveryhero.com/blog/how-delivery-hero-uses-agentic-ai-for-building-a-product-knowledge-base/) AI to manage large product catalogs with accurate data. AI agents help extract product attributes and generate titles to build a product knowledge base.


The system consists of two core LLM components orchestrated in sequence:


- Attribute Extraction agent receives vendor product titles and images to extract 22 predefined attribute types like brand, flavor, volume, etc.
- Title Generation agent creates standardized product titles following Delivery Hero's quality control format.


To maintain high quality of outputs, the team uses the Confidence Scoring system. It processes output logits from the LLMs and converts them into probability scores, automatically flagging outputs below predefined thresholds for human review to maintain quality standards.


*Delivery Hero’s AI agent implementation. Source:*[How Delivery Hero Uses Agentic AI for Building a Product Knowledge Base](https://tech.deliveryhero.com/blog/how-delivery-hero-uses-agentic-ai-for-building-a-product-knowledge-base/)


## 🦾 Anthropic's web research agent


Anthropic[launched](https://www.anthropic.com/engineering/multi-agent-research-system) a new Research feature that involves multiple Claude agents to explore complex topics.


It uses a multi-agent architecture with an orchestrator-worker pattern, where a lead agent plans the research process and creates parallel subagents to do the search. The subagents act as intelligent filters, iteratively using search tools to gather information and return results to the lead agent, who generates a final answer.


To evaluate the quality of the outputs, Anthropic uses an LLM judge that assesses factual accuracy, citation accuracy, completeness, source quality, and tool use efficiency. The judge outputs scores from 0.0-1.0 and a pass-fail grade. Human evaluation is reserved for catching edge cases that automated testing might miss.


*The multi-agent architecture of Anthropic’s Research feature. Source:*[How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)


## 📚 Dropbox's knowledge worker productivity agent


Dropbox uses AI agents to improve its search and knowledge management platform,[Dropbox Dash](https://dropbox.tech/machine-learning/building-dash-rag-multi-step-ai-agents-business-users) . It can summarize, answer questions, surface insights, and generate drafts. The company views AI agents that power Dash as multi-step orchestration systems that can dynamically break down user queries, execute them, and generate responses.


The orchestration system includes two stages: planning and execution. For example, when a user posts a request "Show me the notes for tomorrow's all-hands meeting," the agent walks through the following steps:


- Resolve dates and times for the phrase “tomorrow.”
- Identifies relevant meetings through search.
- Retrieves associated documents.
- Validates the execution logic.
- Delivers the list of documents to the user.


*Agents as multi-step orchestration at Dropbox. Source:*[Building Dash: How RAG and AI agents help us meet the needs of businesses](https://dropbox.tech/machine-learning/building-dash-rag-multi-step-ai-agents-business-users)


## 🔡 Airtable's field agents for content summarization


Airtable built[Field Agents](https://medium.com/airtable-eng/how-we-built-ai-agents-at-airtable-70838d73cc43) , AI-powered fields that autonomously gather insights and create content within Airtable bases. These agents can reason, plan, and orchestrate actions to accomplish complex tasks while summarizing content across databases and minimizing information loss.


Built as an asynchronous event-driven state machine, the system includes three main components: a context manager that maintains accessible information, a tool dispatcher that exposes and executes predefined actions, and a decision engine that determines next steps based on available context.


Users can engage with the agent, provide feedback, and ask follow-up questions through a conversational interface.


*Airtable’s agent state machine. Source:*[How we built AI agents at Airtable](https://medium.com/airtable-eng/how-we-built-ai-agents-at-airtable-70838d73cc43)


## 📦 Ramp's transaction-to-merchant matching agent


A fintech company, Ramp,[developed](https://engineering.ramp.com/post/fixing-merchant-classifications-with-ai) an AI agent to solve the merchant classification problem that previously required hours of manual work from customer support, finance, and engineering teams. The agent comprises an LLM backed by embeddings and rapid OLAP queries, multimodal RAG, and carefully constructed guardrails.


The system can resolve incorrect merchant reports in under 10 seconds instead of hours, with performance monitoring showing proper handling of nearly all cases. To ensure the system is safe, the LLM can only take approved actions, with post-processing guardrails to catch potential hallucinations.


*Mapping a user request to an action. Source:*[How Ramp Fixes Merchant Matches with AI](https://engineering.ramp.com/post/fixing-merchant-classifications-with-ai)


## 🧑‍💼 Netguru's sales agent


Netguru, a software development company, created[Omega](https://www.netguru.com/blog/how-we-built-an-ai-agent) , an AI agent designed to streamline sales workflows. The solution is based on multi-agent orchestration with specialized roles: the SalesAgent analyzes requests and determines next steps, the PrimaryAgent executes tasks, and the CriticAgent reviews outcomes and provides feedback.


Omega prepares expert call agendas, summarizes sales conversations, navigates project documentation, generates proposal feature lists, and tracks deal momentum – all integrated across Slack, CRMs, Apollo, and Drive to deliver actionable insights.


*Netguru’s sales AI agent production process. Source:*[From Slack Bot to Sales Brain: How We Built Our AI Agent](https://www.netguru.com/blog/how-we-built-an-ai-agent)


## 🚀 Moveworks' employee productivity agent


A tech company Moveworks built[Brief Me](https://www.moveworks.com/us/en/resources/blog/how-we-built-brief-me-for-productivity) , a productivity feature within their Copilot that enables employees to upload PDF, Word, and PPT files into chat and interact with content. Effectively, it’s like "talking" to the files. The AI agents within Brief Me handle complex content generation tasks, including summarization, Q&A, comparisons, and insight gathering, allowing users to bring their own data sources in real-time.


*Moveworks’ AI agent architecture. Source:*[What’s behind Brief Me? An exploration of its agentic engineering system](https://www.moveworks.com/us/en/resources/blog/how-we-built-brief-me-for-productivity)


## ✍️ Salesforce's Text-to-SQL agent


Salesforce democratizes data access through[Horizon Agent](https://www.salesforce.com/blog/text-to-sql-agent/) , an internal text-to-SQL Slack agent. It processes everyday language questions and returns SQL queries, answers, and context for confident decision-making. The system retrieves relevant business context and dataset information, submits enriched questions to LLMs, and provides explanations to increase user trust while supporting conversational follow-ups.


*Example interaction with Horizon Agent. Source:*[How We Built a Text-To-SQL AI Agent to Get Instant Answers From Our Data](https://www.salesforce.com/blog/text-to-sql-agent/)


## 🎵 Intercom's voice AI agent


Intercom developed[Fin Voice](https://www.youtube.com/watch?v=HOYLZ7IVgJo) for phone support. The voice AI agent handles customer calls, answers questions, and escalates to human agents when needed. The system integrates a complete voice stack including transcription, language models, text-to-speech, retrieval-augmented generation, and telephony, while addressing enterprise challenges like latency, voice quality, and answer accuracy within existing support workflows.


*Conversation summary with Fin Voice. Source:*[\[VIDEO\] Shipping an Enterprise Voice AI Agent in 100 Days](https://www.youtube.com/watch?v=HOYLZ7IVgJo)


## Evaluate AI agents with Evidently


These examples demonstrate that AI agents are moving beyond experimental implementations to become essential tools for businesses. If you are building complex systems like AI agents, you need[evaluations](https://www.evidentlyai.com/llm-guide/llm-evaluation) to make sure they work as expected – both during development and in production.


That’s why we built[Evidently](https://www.evidentlyai.com/) . Our open-source library, with over 25 million downloads, makes it easy to test and evaluate LLM-powered applications, including AI agents.


We also provide[Evidently Cloud](https://www.evidentlyai.com/register) , a no-code workspace for teams to collaborate on AI quality, testing, and monitoring and run complex evaluation workflows. You can generate synthetic data, create evaluation scenarios, run adversarial tests, and track performance – all in one place.


Ready to test your AI agent?[Sign up for free](https://www.evidentlyai.com/register) or[schedule a demo](https://www.evidentlyai.com/get-demo) to see Evidently Cloud in action. We're here to help you build with confidence!
