---
schema_version: "1.0.0"
document_id: "19a0b5939c4f56ff500e441cf5a8181980ec9d372feb7c9eb50523e9baa98ea4"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-32a3c94cba04"
canonical_url: "https://building.nubank.com/building-ai-agents-for-131-million-customers/"
published_at: "2026-03-23T16:43:15+00:00"
first_seen_at: "2026-07-20T04:35:52.604812+00:00"
fetched_at: "2026-07-28T21:56:54.694470+00:00"
content_hash: "sha256:eac123db34b79291a7903fcfffbcb736b88e10581f9925467c3331a3b535526d"
---

# Building AI agents for 131 million customers

*Written by: :[Aman Gupta](https://www.linkedin.com/in/aman-gupta1/) ,[Daniel Braithwaite](https://www.linkedin.com/in/daniel-braithwaite-phd-78ab58105/)*


In the high-stakes arena of global finance, the margin for error is zero. At Nu, we serve 131 million customers across Brazil, Mexico, and Colombia. For a neo-bank of this scale, the “chatbot” era is over. Our mission is to move beyond simple FAQ retrieval and toward autonomous systems capable of handling complex debt renegotiation, card logistics, and fraud prevention—all while maintaining the absolute trust of a customer base that expects instant, empathetic, and factually perfect support.


My perspective is shaped by over a decade at Apple, Amazon, and LinkedIn, along with a deep commitment to the research community, having published at NeurIPS, ICML, and ICLR. As the creator of **Quant Ease** —the first quantization algorithm at scale utilizing coordinate descent—and a researcher in LLM preference alignment, I’ve learned that the “magic” of AI agents is actually an infrastructure problem. To build agents that work at a hundred-million-user scale, you must abandon prompt “wizardry” in favor of architectural rigor.


Here are the five hardest lessons we’ve learned building production-grade AI agents.


## 1. The “Evals-First” Approach: From NPS to TNPS


Traditional metrics are a death trap for AI development. Most organizations rely on the Net Promoter Score (NPS) to judge success, but for high-volume support, NPS is a nightmare. Response rates are notoriously low, feedback is delayed by days, and the sample size lacks the statistical power required for rapid A/B testing.


To scale, you must pivot to **TNPS (Transactional Net Promoter Score)** —measuring the specific AI interaction immediately—and supplement it with an “LLM-as-a-judge” framework. Instead of waiting for a survey that may never come, we use a secondary, auditor LLM to evaluate every single conversation against binary criteria:


- **Correctness:** Did the agent’s response contradict our internal knowledge base?
- **Conciseness:** Did the agent provide a high-signal answer or a “wall of text”?
- **Preference Alignment:** Did the agent adhere to the specific “tone and style” versioning for that market?


By moving from a tiny sample of surveys to a 100% audit of conversations, we can iterate on variants daily. As the saying goes: *“If you cannot measure it, you cannot improve it. So focus on measurement—be fanatical about it.”*


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## 2. Redefining the Agent: The React Paradigm


The term “agent” is currently drowning in industry fluff. In a production environment, an agent is not a text-generator; it is an autonomous system following the **React (Reasoning and Acting)** paradigm. It is a brain with a toolbelt that solves tasks end-to-end.


Architecturally, we define the agent through three distinct layers:


- **The Prompt (The Brain):** The reasoning engine that decides the next step.
- **The Tools (The Hands/APIs):** The deterministic actions, such as check_delivery_status or trigger_debt_renegotiation.
- **The Data (The Memory):** The RAG (Retrieval-Augmented Generation) layer and the session context that grounds the model in the customer’s specific history.


If your system cannot trigger a real-world action—like re-ordering a lost card or updating a credit limit—it is a search interface, not an agent.


## 3. Stop Writing Prompts: The Era of Prompt Optimization


One of our most counter-intuitive findings is that humans are fundamentally bad at writing prompts for production. Manual prompting is “brittle” and model-dependent. We’ve observed a phenomenon where transitioning to next-generation frontier models (like moving from GPT-4 to newer, more capable reasoning models) can actually break existing agents. This is because smarter models **“assume a lot less”** —they lose the implicit helpfulness (and the implicit assumptions) your prompt was relying on.


To solve this, we utilize **Prompt Optimization** and **Semantic Versioning** . We treat prompts like code, segmenting them into modules (e.g., Tone, Tooling, Safety) that evolve independently. We leverage optimization frameworks like **DSPy** and Berkeley’s **Japa** optimizer. The workflow shifts the human’s role:


1. **Human Defines the Goal:** You provide the input/output success criteria.
2. **Human Labels the Samples:** You provide 50–100 high-quality traces.
3. **The Machine Optimizes:** The optimizer (like Japa) iterates on the prompt text to find the most efficient instruction set for that specific model version.


If you are writing five-page prompts by hand, you are building a system that will break the moment the model provider updates their weights.


## 4. Don’t Fine-Tune Unless You’re Chasing the Last 5%


There is a common urge to rush into Supervised Fine-Tuning (SFT) or Reinforcement Learning (RL). While my research focuses on preference alignment, my strategic advice is the opposite: **Do not fine-tune until you have exhausted the capabilities of Frontier Models.**


Your real competitive moat is not in the model weights; it is in your **Data and Action layers.** Fine-tuning introduces significant architectural risks:


- **Model Drift:** Improving a model on a narrow task often degrades its general reasoning.
- **Capacity Issues:** Smaller models “forget” safety guardrails or instructions not explicitly present in the training set.
- **Maintenance Overhead:** You are now responsible for the entire lifecycle of that model’s performance.


Unless you are optimizing for the final 5% of accuracy or specific latency requirements on high-volume, narrow tasks, your engineering hours are better spent building robust tools.


## 5. Move Logic to Tools: LLM Reasoning is Not Business Logic


Hallucinations are often a symptom of asking the LLM to do deterministic work. If a process has a fixed sequence, do not ask the LLM to “reason” through it.


Consider our **Card Delivery** case study. Initially, we tried to let the LLM chain three tools: check status \\rightarrow verify address \\rightarrow trigger re-order. The LLM would frequently hallucinate the sequence or fail mid-chain. We realized we were being “idiots” by making the LLM do the work of a script.


The solution was to build a **Composite Tool** : a single, deterministic API that handles the entire three-step logic internally. The LLM only needs to decide to “Fix Card Delivery.”


“If it’s deterministic, don’t make the LLM do the work.”


By offloading business logic into the tools, you reduce the reasoning load on the LLM, slash hallucination rates, and ensure the agent remains a reliable interface for your backend services.


## The Blueprint: Automated Simulation and Bug-Bashing


Before an agent variant reaches a single customer, it enters a “Bug-Bashing” environment. We have moved away from manual QA toward **Automated Simulation and Red-Teaming.** Using our internal dashboard and trace system, we simulate thousands of conversations to “attack” the agent.


We monitor the “traces”—the full interaction chain of LLM messages, customer inputs, and tool calls. We use simulation partners to stress-test the agent in ways a human tester might not anticipate, ensuring the agent won’t leak PII or deviate from its financial mandate before it goes live.


## Conclusion: The Future of Autonomous Finance


AI agents are not here to replace our human “X-Peers”; they are here to elevate them. By resolving high-volume, predictable cases like FAQ retrieval and card tracking, agents empower our experts to dedicate their expertise to the complex challenges that require deep financial empathy.


This transition, however, demands more than just better models—it requires a fundamental shift in mindset. In banking, where trust takes years to build but seconds to lose, the “magic” of autonomy must be anchored by architectural rigor. Success in this new era belongs to those who treat customer trust as their most valuable asset, ensuring that every automated interaction is governed by a fanatical, systematic measurement of excellence.
