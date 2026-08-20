---
schema_version: "1.0.0"
document_id: "e784319ee561a05d4fdb19fa6d6e3e1f307fae4edc5a764373a4cdd534c175d0"
company_key: "workday-inc-class-a-common-stock"
company: "Workday Inc."
source_id: "workday-inc-class-a-common-stock-rss-1edd291cea4c"
canonical_url: "https://medium.com/workday-engineering/the-reliability-illusion-why-your-ai-partner-needs-a-specific-job-description-1fec4b6575fb"
published_at: "2026-03-30T23:29:07+00:00"
first_seen_at: "2026-07-20T04:35:52.231186+00:00"
fetched_at: "2026-07-28T22:16:23.597827+00:00"
content_hash: "sha256:036b4507cd511f18508cb25bb3d3a50191734f4b6bf2dc77e3a9b20ac9d5d932"
---

# The Reliability Illusion: Why Your “AI Partner” Needs a Specific Job Description

# The Reliability Illusion: Why Your “AI Partner” Needs a Specific Job Description


[Murtuza N. Shergadwala](https://medium.com/@murtuza.shergadwala?source=post_page---byline--1fec4b6575fb---------------------------------------)


10 min read


·


Mar 30, 2026


--


*This article explores new peer-reviewed research from the Responsible AI team at Workday on the “Stability Trap”: a phenomenon where our digital AI partners provide consistent answers but rely on hallucinated reasoning for deterministic tasks. By unpacking the “Anthropomorphic Fallacy”, that is, our tendency to project human cognitive baselines onto probabilistic models and introducing a new Hybrid Partnership Protocol, we provide a blueprint for agentic workflow architects. Learn how to build reliable, tool-equipped AI agents that eliminate costly rework and ensure true enterprise trust.*


By Murtuza N. Shergadwala with contributions from Veena Calambur, Kelly Trindel, Jason Vantomme, Jenn Dearth, Shalini Kodali, and Kathy Pham.


The narrative around Generative AI has now shifted towards Agentic AI. We are no longer just talking about productivity hacks or simple chatbots. As recent research from[McKinsey](https://www.mckinsey.com/mgi/our-research/agents-robots-and-us-skill-partnerships-in-the-age-of-ai) and[Workday](https://forms.workday.com/en-us/reports/beyond-productivity-ai-value/form.html) suggests, we are entering the era of **“Skill Partnerships.”**


The vision is compelling: Organizations are moving “beyond productivity” to fundamentally redesign workflows where humans and AI work side-by-side. In this future, AI isn’t just a tool; it is a “digital partner” expected to handle complex, multi-step tasks.


**However, this transition is facing a reality check.** Workday’s latest study, *Beyond Productivity* , reveals a “hidden productivity drag”: nearly 40% of the time saved by AI is currently lost to **rework** with humans fixing the machine’s output.


Why is this “tax” on talent so high? Because we are mismanaging the partnership. Effective collaboration relies on knowing your partner’s strengths as well as their limitations. You wouldn’t ask a brilliant creative director to audit your tax returns, nor would you ask a forensic accountant to brainstorm marketing slogans.


**This is the core insight of our**[recent peer-reviewed research](https://arxiv.org/abs/2601.11783) **** (accepted for the proceedings of the ACM Fairness, Accountability, and Transparency (FAccT’26) conference) **:** To build successful partnerships with AI and eliminate this rework drag, we must stop treating AI as a “do-it-all” generalist. We must start understanding the specific boundaries where it excels versus where it struggles. For AI development teams, understanding these nuances is essential to agent design: it helps determine which tasks you give to the model, how you scope its responsibilities, and where you need to shape the workflow differently to avoid end-user frustration and rework.


## **The Study: Stress-Testing the “LLM Judge”**


**The Challenge: How to Audit a Digital Partner** In a manual world, a manager would review a junior employee’s work. But in an enterprise deploying thousands of AI interactions daily, human review is unscalable. To solve this, the industry has adopted the **“LLM-as-a-Judge”** paradigm: using strong, reasoning-capable models (like Gemini 3.0 Pro or Claude 3.5 Sonnet) to act as automated auditors. Ideally, this provides a scalable, consistent mechanism to verify that your “Digital Partner” is following the rules.


But this creates a new risk: If we rely on an AI Judge to verify an AI application, we must ensure the judge itself is reliable.


To understand these boundaries, we conducted a rigorous stability analysis using our **Scoped Instruction Decomposition (SID) Framework** .


Press enter or click to view image in full size


A pictorial view of the Scoped Instruction Decomposition Framework starting with instruction scoping to rubric creation to testing the rubric for stability to then evaluating stability by separating verdicts and accompanying reasoning.


**The Setup:** We created **hypothetical test scenarios** mimicking standard HR tasks (e.g., generating performance reviews or career vision statements) to serve as our sample AI applications. *Note: These were controlled experimental artifacts, not deployed products.*


**The Goal:** We used “LLM-as-a-Judge” to evaluate whether these AI applications followed the prompt instructions. We measured two things:


1. **Verdict Stability:** Did the judge consistently vote “Pass” or “Fail”?
2. **Reasoning Stability:** Did the judge consistently cite the same *evidence* (e.g., specific word counts or text snippets) to justify that vote?


**The Result:** Our analysis revealed a stark dichotomy between the *verdict* and the *reasoning* , particularly depending on the **nature of the task** .


1. **The Illusion (High Verdict Stability):** On the surface, the judges appeared incredibly robust. Across both objective and subjective tasks, the judges achieved **>99% Agreement** . They were remarkably consistent in saying, “Yes, this output follows the rules.”
2. **The Reality (The Reasoning Gap):** However, when we looked at the *justifications* for those verdicts, a misalignment trap emerged specifically for **quantitative tasks** .


- When checking **Subjective** nuance (e.g., *“Is the tone motivational?”* ), the models were generally stable.
- When checking **Objective** quantities (e.g., *“Is the word count under 200?”* ) **Reasoning Stability dropped to as low as ~19%** . In these quantitative cases, the model often hallucinated the math, citing “185 words” in one run and “192 words” in the next, to retroactively justify a “Pass.”


## The Implication: Why This Matters


It is important to note that our study focused on **Generative AI** (one-shot tasks) meaning a single inference step that produces a direct output without multi-step execution or autonomous self-correction. But these findings have profound implications for the emerging field of **Agentic AI** . Agents are different from one-shot tasks because they *plan* . They rely on internal “Reasoning Traces” (Chain-of-Thought) to decide their next move. Because we are increasingly deploying them to manage complex, autonomous workflows, it becomes incredibly easy to view these reasoning traces as a human-like thought process. This tendency to interact with them as if they are independent human colleagues leads us directly into a dangerous cognitive trap.


## The Anthropomorphic Fallacy: Why We Let This Happen


Why do we set our AI partners up to fail like this? Because we fall into the trap of projecting human-like capabilities onto an architecture that “thinks” in a fundamentally different way. Users unconsciously assume that intelligence is a single, vertical ladder.


> If an entity is smart enough to do “Level 10” tasks (like creative reasoning or empathy), we assume it has already mastered “Level 1” tasks (like counting words or basic addition). This is the Anthropomorphic Fallacy.


The critical insight here is that AI agents are not people. They simply do not think like us, and we cannot expect the same cognitive baseline from them that we expect from human partners. Millions of years of human evolution wire us to anthropomorphize the entities we interact with, making this an incredibly difficult and counterintuitive habit to break. As we learn to live and work alongside agents, this fallacy is bound to manifest in all kinds of unexpected and potentially risky ways. If we are committed to building ethical, responsible, and trustworthy AI, we must actively educate ourselves and our teams about these fundamental differences.


To avoid this, we must align on what this technology actually is. It’s key to understand that current state-of-the-art AI can perform impressive reasoning-like tasks that fundamentally operate on the probabilities of the next “right” tokens in a sequence of characters. Despite all the advancements in pre- and post-training and the emergence of reasoning abilities, it is, at its core, a probabilistic next-token predictor. When you ask an LLM judge “Does this text have 155 words?”, it isn’t performing arithmetic; it is simply predicting the probability of a number that seems plausible in that context. In fact, in the early days, before specialized tuning and external tools were standard, the model was so reliant on raw token frequency that it might sometimes predict that “2+2=5” was a more plausible sequence of tokens than “2+2=4,” clearly revealing its non-deterministic nature.


## The Reality Check: A Hypothetical Finance Agent


**Note: This example is purely illustrative and not a Workday product. The focus is on the principle that the failure occurs when an agent is designed to rely on raw inference for deterministic tasks, rather than being equipped with a tool, which is the central finding of our research.**


While a hallucinated word count is annoying, the stakes rise exponentially in domain-specific workflows. **Consider a hypothetical Finance Agent** tasked with auto-approving expense reports. It might generate the following internal reasoning trace:


> *“I have analyzed the receipts. The total spend is $9,850. The auto-approval threshold is $10,000. Since the spend is within limits, I will trigger the payment API.”*


Press enter or click to view image in full size


An infographic of the stability trap using the hypothetical example where wrong processes can get triggered due to simple threshold calculation errors in reasoning traces without tools.


If the underlying model exhibits **Reasoning Instability** — for instance, if the actual sum of the receipts was $10,100 but the model hallucinated the arithmetic — the Agent’s planning capability collapses. It believes it has met a compliance constraint when it hasn’t, leading to a cascading failure: an unauthorized payment is released, creating a financial audit risk downstream. Reasoning instability becomes a real risk when reasoning is used as a system state or control, such as when agents rely on it to plan actions or trigger downstream processes, rather than when it is merely explanatory.


## Real-World Application: Empowering Agents with Purpose-Built Tools


While the hypothetical Finance Agent illustrates the danger of relying on raw LLM inference for precise tasks, it also points us directly to the architectural fix: equipping the agent with the right tools.


Instead of asking a general-purpose LLM to “predict” if an expense is anomalous using next-token generation, a well-designed agentic workflow delegates this to specialized, highly stable models. This is precisely how **Workday’s Expense Protect (EP)** operates as a built-in control for[Workday Expense Management.](https://www.workday.com/content/dam/web/en-us/documents/datasheets/datasheet-workday-expenses.pdf)


Rather than relying on an LLM’s unstable reasoning traces, a financial agent can invoke EP as a reliable, embedded tool to generate consistent risk signals before reimbursement occurs. EP leverages native, purpose-built machine learning to assess risk through specific lenses:


- **Risk Scoring (0–100):** Every expense line is assigned a numeric score indicating its relative risk compared to historical entries.
- **Amount Anomaly & Mismatch Detection:** Models detect outliers based on location and job profile, while also flagging mismatches between user-selected categories and extracted merchant data.
- **Duplicate Detection:** Analyzes patterns across out-of-pocket and corporate card feeds to catch repeat submissions.


While EP’s underlying anomaly detection calculates the probability of risk, its output is functionally stable and purpose-built for financial integrity. When an agent integrates with a tool like EP, it doesn’t have to guess at policy adherence or hallucinate arithmetic; it simply waits for EP’s inference and confidently routes the workflow based on reliable risk thresholds.


## The Solution: Redefining Roles and the Hybrid Partnership Protocol


Responsible AI isn’t just about guardrails; it’s about **Role Definition** . This means building systems where AI operates on clearly defined guardrails, and humans understand when to rely on those guardrails and when to intervene.


In the era of Skill Partnerships, as AI agents take on execution tasks, people must take on more responsibility for designing, supervising, and refining those workflows. We are no longer just *doing* the work; we are directing the digital workforce that *does* the work. In practice, this transition from Generative to Agentic AI, shifts the role of product teams from prompt writers to workflow architects.


## The Way Out: Setting Your Digital Partner Up for Success


Agent failures don’t mean the technology is broken; they mean we must be deliberate in how we design the agentic systems and workflows that utilize it. Currently, there is a critical skills gap: employees are being asked to direct powerful AI-enabled workflows without having been trained to do so. We must train teams not just in prompt engineering, but in AI Management — the skill of knowing how to delegate effectively.


An effective “AI Manager” must understand this boundary: they must know better than to ask an LLM to perform arithmetic via text prediction when the task requires deterministic accuracy. For agent developers, the takeaway is clear: you must treat semantic reasoning and deterministic checks as fundamentally different skill sets.


## The Hybrid Partnership Protocol


Based on our findings, we propose a Hybrid Partnership Protocol that **guides the design of agentic systems — delegating work to the AI partner based on its competencies and surrounding it with the right tools** :


### **Tier 1: Equip AI Agents with the right set of tools (The Deterministic Tier)**


- **The Nuance:** It’s not that we can’t ask our AI partners to do math — it’s that we must stop asking them to *predict* the answer using their internal weights.
- **The Task:** Counting words, calculating financial sums, validating JSON schemas.
- **The Fix:** Instruct the AI to **invoke tools** . Instead of asking the model to “guess” the word count, the Instruction Architect must verify that the agent is equipped with and using a **Calculator** or a **Python Sandbox** .
- **The Human Role:** The human becomes a reviewer of *method* , not just output. The audit question shifts from *“Is the number right?”* to *“Did the agent use the Calculator tool to derive this number, or did it try to do it by predicting tokens?”*


### **Tier 2: Guide AI Agents to do what they do best: language reasoning (The Semantic Tier)**


- **The Nuance:** Reserve the model’s raw inference capabilities for tasks that require reading comprehension, tonal analysis, and pattern matching.
- **The Task:** *“Is this feedback empathetic?”* , *“Does this summary capture the key project details?”* , *“Is the narrative cohesive?”*
- **The Finding:** Our study showed modern models are exceptionally stable at these tasks. This is where your digital partner shines without the need for external tools.


If we ask an Agent to perform tasks where it is statistically unreliable without these deterministic supports, we are setting our partner up for failure.


## Conclusion: Trust Through Architecture


The future of enterprise software is indeed an AI partnership. But as builders, we have a responsibility to set these digital partners up for success.


By understanding and considering in the design of agentic tools the exact limits of LLM cognition, specifically, the stark difference between semantic reasoning (where they excel) and arithmetic precision (where they hallucinate), we can design workflows that are robust, reliable, and ready for the agentic future.


We don’t need to fear the “Stability Trap.” As engineers, PMs, and Instruction Architects, our job is not to try and fix the model’s fundamental probabilistic nature, but to design the right environment around it. By knowing when to let the model use its semantic intuition, and when to build strict, deterministic rails for it to run on, we move beyond the illusion of reliability and build enterprise systems that truly work.
