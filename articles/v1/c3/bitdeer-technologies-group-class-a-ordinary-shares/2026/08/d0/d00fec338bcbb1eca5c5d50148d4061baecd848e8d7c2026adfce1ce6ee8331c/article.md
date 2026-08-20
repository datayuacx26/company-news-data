---
schema_version: "1.0.0"
document_id: "d00fec338bcbb1eca5c5d50148d4061baecd848e8d7c2026adfce1ce6ee8331c"
company_key: "bitdeer-technologies-group-class-a-ordinary-shares"
company: "Bitdeer Technologies Group"
source_id: "bitdeer-technologies-group-class-a-ordinary-shares-rss-2127a671e698"
canonical_url: "https://www.bitdeer.ai/en/blog/nvidia-nemotron-3-5-lightning/"
published_at: "2026-08-11T12:59:07+00:00"
first_seen_at: "2026-08-11T13:54:21.251692+00:00"
fetched_at: "2026-08-11T13:54:22.342037+00:00"
content_hash: "sha256:f284b03afd2298f27ee0028d735877d80a45b19532e220d13c91deef0a7a9ef9"
---

# Day 0 Availability: Power Always-On Agents with NVIDIA Nemotron 3.5 Lightning on Bitdeer AI Model Studio

When an always-on agent stalls or runs up a surprising bill, the instinct is to reach for a bigger, smarter model. But most agent work isn’t frontier reasoning. Always-on agents complete complex, multi-step tasks by gathering context, observing their environment, reasoning over what they know, and acting and every one of those steps calls a language model. Many of those steps are high-volume, repetitive, and domain-specific: exactly the work a smaller, specialized, controllable model can do faster and cheaper than a frontier model.


That is the shift behind systems of models: instead of routing every step to one large model, agents route each step to the right model for the task, frontier capability where it’s needed for orchestration and hard reasoning, and specialized models where the work is high-volume and repetitive.


Today, NVIDIA Nemotron 3.5 Lightning, the fastest open model in its class for always-on agents, is available on[Bitdeer AI Model Studio](https://account.bitdeer.com/en/sign_in?method=3&service=https%3A%2F%2Fwww.bitdeer.ai%2Fauth&ref=bitdeer.ai) , bringing a fully customizable open model for powering production always-on AI agents to our serverless inference platform, built for secure, scalable enterprise AI. You can deploy it immediately, without managing the underlying infrastructure.


## **What is NVIDIA Nemotron 3.5 Lightning?**


NVIDIA Nemotron 3.5 Lightning is a 30B Mixture-of-Experts (MoE) model with 3B active parameters, distilled from NVIDIA’s frontier[Nemotron 3 Ultra](https://developer.nvidia.com/topics/ai/nemotron?ncid=pa-srch-goog-599191&_bt=797127771541&_bk=nemotron+ultra&_bm=p&_bn=g&_bg=194751055082&gad_source=1&gad_campaignid=23551395576&gbraid=0AAAAAD4XAoFeQinguIcJuX2VkkBLuCzca&gclid=Cj0KCQjwm8bTBhDWARIsAC9Hi8kqOfcaDF154scJbLfOspJdPcr7RvDUPLtevICET8GbBWdj63Iz4_UaArFXEALw_wcB&ref=bitdeer.ai#:~:text=speech%2C%20and%20safety.-,Nemotron%203%20Ultra%20550B%20A55B,-Ideal%20for%20multi) model. It is a fully customizable open model designed to power production of always-on AI agents, giving organizations control to own the model, post-train it for specialized tasks, and deploy it wherever their agents run.


Developed with the[Nemotron Coalition](https://investor.nvidia.com/news/press-release-details/2026/NVIDIA-Launches-Nemotron-Coalition-of-Leading-Global-AI-Labs-to-Advance-Open-Frontier-Models/default.aspx?ref=bitdeer.ai) and built for popular agent harnesses, Nemotron 3.5 Lightning is designed to deliver leading accuracy for coding, tool calling, instruction following, and multi-turn workflows, so agents can complete specialized tasks faster. It is built to slot into a system of models: routers such as NVIDIA NeMo Switchyard can intelligently direct each step of an agent workflow to the best available model in a chosen pool, selecting Nemotron 3.5 Lightning when its accuracy, speed, and deployment flexibility make it the right fit for high-volume specialized work.


The model is built around three core strengths:


- **Model control.** An open model, trained with open datasets, that enterprises can control, customize, and deploy at the edge, locally, and in the datacenter, while managing model behavior, data handling, and agent workflows.
- **Customizable for high accuracy.** Trained for popular agent harnesses for strong out-of-the-box accuracy on agentic tasks, and post-trainable for specialized workflows to improve accuracy in specific enterprise domains.
- **Fast task completion.** Up to 4x higher throughput and efficient token rollout help always-on sub-agents and personal agents complete more steps and finish specialized tasks faster while lowering inference cost.


## **Key Specifications**


Property


Details


Model


NVIDIA Nemotron 3.5 Lightning


Architecture


Hybrid Mixture-of-Experts (MoE), distilled from NVIDIA Nemotron 3 Ultra


Model size


30B total parameters · 3B active parameters


Generation


Multi-token prediction


Context length


Up to 1M tokens


Modalities


Text input, text output


Precision


Early access: BF16 · General availability: NVFP4


Openness


Open model weights and open training datasets; customizable and post-trainable


NVIDIA technology


Built for popular agent harnesses; routable via


[NVIDIA NeMo Switchyard](https://github.com/nvidia-NeMo/switchyard?ref=bitdeer.ai)


Supported GPUs


NVIDIA H100, H200, A100, L40S, GB200/B200, GB300/B300, RTX Pro 6000, and DGX Spark (B10)


## **Why “Systems of Models” Now Decides Agent Economics**


As enterprises move always-on agents into production, a familiar set of tradeoffs comes into focus:


- **Capability vs. cost.** Routing every step to a frontier model is expensive when most steps are high-volume and repetitive.
- **Capability vs. speed.** Larger models add latency to every turn, and always-on agents run many turns.
- **Generalist vs. specialist.** A single general model rarely matches a model post-trained for a specific enterprise domain.
- **Openness vs. production readiness.** Open models can be owned and customized, but teams still need tested, enterprise-grade servers to run them at scale.


Nemotron 3.5 Lightning is designed to address these tradeoffs directly:


**Own and control the model.** As an open model trained with open datasets, Nemotron 3.5 Lightning lets enterprises control model behavior, data handling, and agent workflows and deploy where their agents run, from the edge to the datacenter and cloud.


**Specialize for accuracy.** Trained for popular agent harnesses, the model delivers strong out-of-the-box accuracy on agentic tasks and can be post-trained for specialized workflows to raise accuracy in specific enterprise domains, so a small model can outperform a general one on the work that matters to you.


**Finish faster.** High token-generation throughput keeps per-step latency and cost low, letting always-on agents run more steps and complete specialized tasks faster. According to NVIDIA’s preliminary benchmarks, Nemotron 3.5 Lightning delivers up to 4x higher throughput than comparable open models on agentic multi-turn workloads, enabling agents to complete specialized tasks faster.


## **Enterprise Use Cases**


**Always-on personal and productivity agents.** Power long-running assistants for everyday tasks—managing email, calendars, projects, and bookings, where high-volume, repetitive steps benefit from a fast, controllable model.


**Financial services workflows.** Run specialized agents for tasks such as extracting data from documents, checking policy rules, monitoring risk signals, and preparing structured summaries.


**Cybersecurity operations.** Power specialized security agents that enrich alerts, classify incidents, query logs, validate controls, correlate indicators, and prepare structured findings for analysts.


**Telecom.** Support autonomous networks and proactive customer care triaging network alarms, optimizing configurations, and answering billing questions.


**Retail.** Enrich product catalogs, resolve inventory and fulfillment exceptions, assist product discovery, and answer order, return, or loyalty questions.


## **Run Nemotron 3.5 Lightning via API on Bitdeer AI Model Studio**


You can run Nemotron 3.5 Lightning on[Bitdeer AI Model Studio](https://account.bitdeer.com/en/sign_in?method=3&service=https%3A%2F%2Fwww.bitdeer.ai%2Fauth&ref=bitdeer.ai) , our serverless inference platform designed to make access to advanced foundation models simple and scalable. With a unified API, Model Studio lets developers and enterprises start using models quickly without managing underlying infrastructure, reducing deployment complexity and time to value, so you can slot a specialized model into a system-of-models agent architecture without standing up new serving stacks.


Bitdeer AI is a preferred NVIDIA Cloud Partner, certified to ISO/IEC 27001:2022 and SOC2 Type I & Type II, providing the secure, compliant, high-performance, enterprise-grade infrastructure that production agentic AI deployments require.


## **Get Started**


1. Log in to[Bitdeer AI Model Studio](https://account.bitdeer.com/en/sign_in?method=3&service=https%3A%2F%2Fwww.bitdeer.ai%2Fauth&ref=bitdeer.ai) .
2. Locate **NVIDIA Nemotron 3.5 Lightning** in the model list.


1. Generate an API key and start making API calls.


> curl -v --location '[https://api-inference.bitdeer.ai/v1/chat/completions](https://api-inference.bitdeer.ai/v1/chat/completions?ref=bitdeer.ai) ' --data '{"model":"nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16","messages":\[{"role":"system","content":"You are a knowledgeable assistant. Provide concise and clear explanations to scientific questions."},{"role":"user","content":"Can you explain the theory of evolution in simple terms?"}\],"max_tokens":4096,"top_p":1.0,"temperature":1.0,"frequency_penalty":0.0,"presence_penalty":0.0,"seed":0,"stream":false}' --header 'Authorization: Bearer <API_KEY>'


## **Conclusion**


Always-on agents don’t need one big model for every step, they need the right model for each step. NVIDIA Nemotron 3.5 Lightning brings a fully customizable open model to that architecture: a 30B MoE with 3B active parameters, distilled from a frontier model, built for popular agent harnesses, and designed for high throughput on specialized, high-volume work. With Day-0 availability on Bitdeer AI Model Studio, you can start routing your agents’ specialized steps to a model you can own, customize, and run at scale today with higher throughput, lower latency, lower inference cost, and full control over your data and workflows.
