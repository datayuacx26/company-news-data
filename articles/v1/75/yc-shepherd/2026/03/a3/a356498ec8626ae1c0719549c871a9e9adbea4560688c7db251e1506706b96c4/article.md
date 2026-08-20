---
schema_version: "1.0.0"
document_id: "a356498ec8626ae1c0719549c871a9e9adbea4560688c7db251e1506706b96c4"
company_key: "yc-shepherd"
company: "Shepherd"
source_id: "yc-shepherd-news-import-bb872756570a"
canonical_url: "https://shepherdinsurance.com/blog/the-road-to-autonomous-underwriting"
published_at: "2026-03-24T00:00:00+00:00"
first_seen_at: "2026-07-24T01:19:36.558879+00:00"
fetched_at: "2026-07-28T22:17:53.237254+00:00"
content_hash: "sha256:ad3f93cc033363c660f988ab8cd470b77bccdfd73e03dcb5e64847878a0b3b4f"
---

# Behind Our Series B: The Road to Autonomous Underwriting

Commercial insurance underwriting works essentially the same way it did 30 years ago. A broker emails a submission with 6–12 different files: PDFs, spreadsheets, and sometimes handwritten applications. An underwriter opens it, manually keys data into several quoting tools, cross-references loss runs, checks appetite guides, pulls up comparable accounts, and, eventually, produces a price. A good underwriter handles maybe twenty accounts a month this way. The entire workflow is built around human data entry, human pattern matching, and human bottlenecks.


This is not a technology problem waiting for a better interface. The commercial insurance industry processes information the way it always has: one document at a time, one human at a time, because nobody has built the intelligence layer that would let it work differently.


We have spent the last four years building that. Today, alongside our $42M Series B announcement, I want to explain what we are building, where we are on the journey, and why I believe autonomous underwriting is the future of commercial insurance.


### What we built first


Before we could build autonomous underwriting, we had to build something harder: an industry-leading system that understands complex commercial risk at scale.


In commercial insurance, no two submissions look the same. Every inbound opportunity arrives in a different format — emails, spreadsheets, PDFs, sometimes handwritten applications. Each opportunity needs to be parsed, structured, and organized before an underwriter can act on it. Speed of input became the bottleneck, and we focused our attention on measuring every step of the workflow, eliminating any drag or slowdowns. The result? Shepherd's core platform helps underwriters manage large sets of data in hours rather than weeks, and many more opportunities than traditional carriers.


That intake engine is the foundation of our data advantage, and it runs on every submission we see — not just the ones we write. Shepherd's highly selective appetite means we quote less than 40% of what we receive. But we extract and structure data from every single submission regardless of outcome. In four years, that process has produced large, statistically significant, industry-specific loss datasets that no other organization has. The datasets allow us to launch novel pricing strategies such as[Shepherd Savings.](https://www.shepherdinsurance.com/savings)


‍


### A framework for autonomous underwriting


When I think about our trajectory, I find the self-driving analogy genuinely useful — not as marketing, but as an engineering framework.


Self-driving cars progressed through well-defined levels of autonomy: from Level 0 (fully manual) through increasing degrees of machine capability, up to Level 5 (fully autonomous in all conditions). At each level, the system's operational domain — the set of conditions where it could be trusted to act without human intervention — expanded.


Underwriting follows the same curve.


The critical insight from self-driving that transfers directly: autonomy is earned, not declared. Waymo did not reach Level 4 by announcing it. They drove millions of miles. They built the sensor stack, the mapping layer, the decision engine, and the correction loop — and then they proved, mile by mile, that the system worked.


We are taking the same approach. Every time our system parses a submission, enriches a data set, or produces a recommendation that an underwriter accepts, that is one more autonomous mile driven. The metric that matters is not capability — it is demonstrated reliability.


### Where we are today


We see a clear two-year path to supervised autonomy. Large language models will continue to improve — they are getting better every week, and as a services company enhanced by AI, every model improvement accelerates us rather than threatens us. The distance is a data problem, a trust problem, and a systems problem. We need to demonstrate, across thousands of decisions, that the system produces reliable output. We need the correction loop — the feedback from underwriters who accept, reject, or modify AI recommendations — to compound into institutional knowledge that makes the next recommendation better.


That correction loop and deep domain expertise is, I believe, our deepest moat. Four years of structured underwriting data. Thousands of policies worth of institutional knowledge about how to price complex commercial risk. It deepens with every interaction. You cannot replicate this with a better model or a bigger training run. You replicate it by doing the work, account by account, for years.


### The shift: from input-driven to output-driven


There is a conceptual shift happening in how AI systems work that matters enormously for underwriting, and it is worth explaining clearly.


The first generation of AI tools in insurance, and in most industries, were input-driven. They waited for a human to ask a question. A chatbot retrieves information when prompted. A copilot suggests a next step when asked. The human drives; the AI assists.


The next generation is output-driven. An agent does not wait for a prompt. It acts on a trigger. A submission arrives in an inbox. The system reads it, parses it, enriches it, prices it, and presents a decision. The human reviews output, not input. The workflow inverts.


This is actually a different operating model. In an input-driven system, the human's throughput is the ceiling — the AI can only help as fast as the human can ask. In an output-driven system, the AI's throughput is the baseline, and the human adds judgment where it matters.


We learned this the hard way. Our AI platform began as a chat interface — ask a question, get an answer. It worked, but it did not transform the workflow. What mattered more than the chat experience was what we built underneath: the document parsing engine, the retrieval system, the citation layer, the platform data access. Underwriting intelligence. Once we had that, we could stop building tools that respond and start building systems that act.


The exploratory phase is over. We now know what to build.


### What this means for underwriters


Every discussion of AI in a professional services context eventually arrives at the replacement question. Let me address it directly.


The Greek root of "autonomy" is *autos* (self) and *nomos* (law) — self-governance. Autonomy does not mean no humans. It means humans with more agency. An autonomous underwriting system does not replace underwriters. It makes each underwriter superhuman.


Today, a skilled underwriter spends the majority of their time on manual data processing and frankly, wastes a lot of time on unqualified submissions. Maybe twenty percent of their time is spent on the work that actually requires their expertise: evaluating risk quality, structuring coverage, making judgment calls on edge cases, managing portfolio construction.


In an agentic underwriting world, those ratios invert. The system handles intake, enrichment, analysis, and pricing. The underwriter orchestrates strategy, reviews exceptions, and allocates capital. Twenty accounts a month becomes two hundred — not because the underwriter works ten times harder, but because they spend their time on the work that only an underwriter can do.


### What this means for engineers


If you are an engineer reading this, I want to make the case that **this is one of the most fascinating applied AI problems you could work on.**


First, the data problem is genuinely hard. We are not working with clean, curated datasets. We are ingesting real-world data from construction sites — drone footage, project management updates, safety reports — across multiple platforms with different schemas, reliability characteristics, and update cadences. Building pipelines that are robust enough to stake underwriting decisions on is a nontrivial engineering challenge.


Second, the explainability constraint shapes everything. Insurance is a regulated industry. We cannot use a model that produces the right answer but cannot explain why. Every recommendation needs a citation trail. Every pricing decision needs to show its work. The system must know its own boundaries — when it is confident and when it should defer to a human. This constraint makes the engineering harder and, I would argue, more interesting. Building systems that degrade gracefully is a different discipline than building systems that optimize for accuracy alone.


Third, every improvement in foundation models is a leap forward for us. We are not building a company that gets disrupted by the next model release. We are building the data layer, the integration layer, the correction loop, and the domain expertise that sit on top of whatever the best available model is. When the models improve, our system improves. That is a fundamentally different relationship with the AI frontier than most companies have.


And fourth, the feedback loop is real and fast. This is not a research lab. When the system produces a recommendation and an underwriter acts on it, we know within minutes whether the decision was good. The signal density in insurance underwriting — structured decisions, measurable outcomes, continuous feedback — is unusually high for an applied AI domain.


### The road ahead


We are just months away from the first fully agentic submission in commercial insurance. A submission arrives by email. The system reads it, enriches it, prices it, and returns a quote. No human intervention. For a defined, well-understood segment of risk, the system handles the entire workflow.


That is only the starting line. From there, the operational domain expands: more segments, more complexity, more nuance, the same way self-driving expanded.


A year from now, commercial underwriting will be unrecognizable.


*Shepherd is hiring engineers, underwriters, and operators who want to build at this frontier. If this is the kind of problem that excites you,*[reach out](https://www.shepherdinsurance.com/careers) *.*


‍
