---
schema_version: "1.0.0"
document_id: "53257f6a83bdb283a89a29317c8a9bf33c7c515f8984bdd6840ffafe76d32bc0"
company_key: "moody-s-corporation-common-stock"
company: "Moody's Corporation"
source_id: "moody-s-corporation-common-stock-news-import-112754bd8765"
canonical_url: "https://www.moodys.com/web/en/us/creditview/blog/the-false-dichotomy-of-build-and-buy.html"
published_at: "2026-03-10T10:53:00+00:00"
first_seen_at: "2026-07-22T04:52:53.039531+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:2669eb9342086e9fb42c0cb3de3a72354fdae05e69563dfc62c2a58d8df3fae4"
---

# The false dichotomy of build and buy in enterprise AI: What should businesses really be doing?

There is a quiet arms race happening now inside every major bank, insurer, and financial institution. AI teams are being formed, budgets are being approved, and internal platforms are being built at speed. But, many of these institutions are discovering that the hardest part of the job is not the AI. It’s the data underneath, and in many cases, someone else has done the work, and has probably done it better. The delay involved in rebuilding from scratch because an institution hasn’t accounted for that data, and how it’s implemented, is costing them more than the build ever will.


This is the flaw at the heart of the familiar build-versus-buy debate. The question itself assumes a binary choice, when the organizations making the most progress have moved past that framing entirely. AI generates text. Data is the engine that powers it. And understanding why that distinction matters requires looking at how the meaning of “build” has changed.


### **What does "build" mean now?**


AI has shifted fundamentally in the past decade. Ten years ago, building meant constructing a data infrastructure from the ground up, then dutifully maintaining it. Organizations pulled information from legacy systems that were never designed for interoperability. This data was then normalized, structured, and stored. Only then could machine learning models be layered on top, almost always for predictive analytics. The models attracted the attention, but the data engineering underneath consumed the vast majority of effort and budget.


Large language models (LLMs) altered that picture. Powerful pre-trained models became available through APIs. Open-source alternatives emerged. Competition among hyperscalers drove costs down and capabilities up. For the first time, a small team could prototype a compelling AI application in days rather than months, without needing the company's own data to get started.


The conversation inside banks changed accordingly. Instead of "we need to fix our data before we can do anything with AI," the question became "where are the use cases that can unlock value immediately?"


But AI-mature institutions – those that have moved beyond the prototype stage – soon discovered that the data problem hadn’t disappeared. It had migrated and magnified in importance. Generating fluent text is not the same as generating decision-grade intelligence. For work involving real risk, whether credit decisions, compliance assessments, underwriting opinions, or counterparty analyses, structured and contextualized data remains essential.


The work that practitioners now call[context engineering](https://www.moodys.com/web/en/us/creditview/blog/beyond-prompts-why-enterprise-ai-demands-context-engineering.html) , which encompasses document structuring for retrieval, metadata enrichment, knowledge graph construction, and vectorization, is, in essence, the old data readiness challenge in a new form. The 80/20 rule of data science, where roughly 80% of effort goes on data preparation and 20% on modelling, has not been repealed by generative AI. If anything, it has intensified.


There is a real risk here that deserves attention: without careful planning, a data science team can quietly become a data cleaning team, and that’s certainly not what any organization intends when it commits to building. If your highest paid engineers end up spending most of their time on the “plumbing,” you’re building a cost center, not a competitive advantage.


### **The anatomy of a hybrid approach**


We think this is where black and white build-versus-buy framing starts to break down, and where a more productive framework emerges.


Consider what goes into a credit decision:


A substantial portion of the underlying intelligence, perhaps 60 to 80%, is based off of data and insights not proprietary to a single financial institution. Credit ratings, financial data, entity structures, counterparty news: these are shared building blocks that specialized providers have spent years gathering, structuring, validating, and curating.


The remaining 20 to 40%, however, must come from within the institution. Client relationship history, cross-selling considerations, unique risk appetites, portfolio-specific constraints, and internal models. These are the judgements that reflect an institution's own experience and positioning. These judgements are the institution’s competitive advantage. Successful firms have adopted a Hybrid Model in lieu of solely building or solely buying. They are leveraging a simple, but powerful framework: partner for the foundations; build the last mile. This allows these firms to focus their internal resources on the proprietary logic unique to them.


This is not a compromise, nor is one party giving up control. This hybrid approach enables both the institution and the vendor to focus on their competitive advantage – driving value greater than the sum of its parts. When laid out like this, it becomes very clear that this isn’t a compromise between building and buying, but an architecture that allows each party to contribute what it does best.


In practice, institutions are already adopting AI in various ways, through diverse entry points, and at differing maturity stages. In an ideal state, the architecture should support that progression without requiring repeated reinvention.


### **Speed as a strategic consideration**


One factor that often tips the balance in these decisions is time, and specifically the cost of delay.


A large share of financial services technology spend is maintaining existing systems. In fact, according to[McKinsey](https://www.moodys.com/web/en/us/creditview/blog/For%20one%20leading%20asset%20management%20firm%20with%20more%20than%20$1%20trillion%20of%20AUM,%20roughly%2080%20percent%20of%20its%20technology%20spend%20went%20toward%20run-the-business%20projects.) , “for one leading asset management firm with more than $1 trillion of AUM, roughly 80% of its technology spend went toward run-the-business projects”. Every new internal build adds to that maintenance burden. When an institution commits to assembling foundational domain intelligence from scratch, the direct engineering cost is significant, but the opportunity cost may be larger still. Eighteen months spent rebuilding capabilities that are already available from a specialist is eighteen months during which competitors are deploying.


Hybrid AI strategies, where one partners with specialized vendors, alleviates this pressure and enables internal teams to can on areas of competitive differentiation. The institution stops maintaining parallel infrastructures for indistinguishable capabilities - redirecting efforts toward higher value-add work.


In the fast-moving world of GenAI, the "Build" cycle is often the "Delay" cycle. By the time an in-house team has mastered the nuances of vector database maintenance and agentic memory, the market leaders who adopted a hybrid approach will have already moved into production. As the cost of delay compounds, speed to market is itself a strategic, if not existential, imperative.


### **Designing for progress**


As we’ve stated before, AI maturity varies considerably across institutions. Some are refining internal copilots. Others are experimenting with agent-to-agent orchestration. Many are working through the practical realities of data readiness and governance.


Any workable strategy should acknowledge these different starting points. It separates foundational domain intelligence, which can be sourced from specialists, from proprietary logic, which must be built and owned internally. It allows external agents to operate within internal guardrails. And it expands gradually, guided by use cases and governance maturity rather than ideology.


The build-versus-buy debate will continue to surface. But in practice, it is increasingly a false dichotomy. The more productive question is: build what, buy what, and how do you connect them?


Institutions that answer that question well tend to move with greater deliberation, integrate more effectively, and maintain clearer control over the capabilities that truly set them apart. That layered approach, rather than any binary choice, is where durable advantage is emerging.


[Go to our Agentic Solutions](https://www.moodys.com/web/en/us/capabilities/gen-ai/agentic-solutions.html)


#### Learn more:


[Browse our hub on credit risk insights](https://www.moodys.com/web/en/us/insights/credit-risk.html?cid=web-ntrnlbnnr-19741)[Read more insights on artificial intelligence](https://www.moodys.com/web/en/us/insights/ai.html?cid=web-ntrnlbnnr-19741)


---


**About the authors:**


**Omar Khan** is a Senior Director of GenAI at Moody's, where he shapes Generative and Agentic AI solutions and partners with executives in Financial Services and Insurance to translate innovations in AI into measurable business impact. Leveraging Moody’s proprietary data, advanced technology and award-winning insights, Omar works with financial services institutions to deploy AI-powered workflow automations that support decision-making, reduce complexity, and perform with precision in regulated, high-stakes environments.


Omar has championed pragmatic environmentalism as a volunteer for Trees New York, The Big Reuse, NYC Parks, and as a member of Transportation Alternatives. He holds of Bachelor of Science in Finance from New York University - Stern School of Business


**Leonardo Orlando** is a seasoned innovator with over 20 years of experience driving business transformation across High Tech, Banking, Capital Markets, and Oil & Gas industries. Passionate about data and AI, he is known for blending cutting-edge technologies with strategic vision to solve complex challenges and accelerate value creation.


In his current role at Moody’s, Leonardo helps customers unlock the potential of generative AI by leveraging tailored solutions built on Moody’s curated data. These tools deliver measurable impact by addressing specific customer use cases. Leonardo prides himself on understanding customer pain points and aligning solutions to meet their needs.


Prior to Moody’s, Leonardo held leadership roles at Accenture, designing integrated AI solutions for finance, risk, and regulatory reporting, as well as industry-specific innovations that enhanced compliance, reduced costs, and created new revenue streams.
