---
schema_version: "1.0.0"
document_id: "1b09e8d4af7c30682b2c4a0c9ead8167fc2b5af227a7cb037e8f84ab063c6b79"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-d41d574f701a"
canonical_url: "https://www.salesforce.com/news/stories/cutting-inference-spend-by-right-sizing-models/"
published_at: "2026-07-08T14:50:58+00:00"
first_seen_at: "2026-07-20T03:33:03.725024+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:af81c7ad291f3d71555c3471ed7adf72cbd40ec6136d6dc55c9cf1ec7cfa9cac"
---

# How We Cut Inference Spend by Right-Sizing Our Models

The headlines say enterprises can’t control their inference spend — the sometimes-surprising amount they pay frontier model companies to use their LLMs.[Uber](https://fortune.com/2026/05/26/uber-coo-ai-spending-tokens-claude-code/) burned through its entire AI budget in four months.[GitHub’s](https://www.businessinsider.com/github-copilot-token-uage-pricing-change-reaction-2026-6) usage-based pricing drew an instant backlash when customers realized how much more they would have to spend.


Now, companies are cutting inference costs by routing to cheaper rented models, and engineers are optimizing their existing models to reduce the bill. But cost is just the most visible symptom of a much larger problem. When a single rented model runs every job for your agent, from intent detection to safety to citation to policy, it’s a chokepoint for your whole business, not just your invoices. The latency sits at the model provider’s mercy, the roadmap depends on their release schedule, and the safety logic lives in a black box. Obsessing over token spend is a local minimum. The real engineering problem is architectural agency.


We know that pressure firsthand. Eighteen months ago,[Agentforce](https://www.salesforce.com/agentforce/) also ran entirely on a single rented model, and our token bill grew linearly with traffic. We could have passed the rising bill on to our customers, but that would have bought us a year at most. The bill would keep rising as long as we were renting the whole engine. So instead of repricing, we opted to rebuild. Instead of handing everything to one model, we broke out different tasks and tuned specific open-source models to accomplish them.


Today, these precision models run a growing share of the stack, and they’re already cutting costs, running more effectively, and giving us more control over our products and roadmap. And as more advanced open-source models are released, their performance should only improve. A frontier foundation model still handles the core muti-step reasoning, but now that reasoning is governed by the harness we’ve built around it — and that means that we, and our customers, can swap the foundation models freely, instead of being dependent on any single vendor.


## What the harness showed us


When a single frontier model handled every request, it had to do every job at once: reading what the user wanted, checking the request was safe, deciding which tool to call, grounding the answer in real data, and judging whether the answer held up. A general-purpose reasoning model can do all of that. It just does each piece more slowly, more expensively, and less precisely than a model trained specifically to do each job.


But none of this started with a list of jobs we knew we were optimizing for. It started with the harness. For nearly three decades we’ve been the system of record for enterprise work — we know what a billing dispute sounds like, what trace a service escalation leaves, and what qualifying a lead actually involves. That’s why we could build an agentic harness — grounded in real customer workflows, data, permissions, and business logic, and get it running across hundreds of thousands of use cases across service, sales, and commerce. The scale is crucial here, because every inference left a trace in our telemetry, and working backward from those traces lets us see the common patterns across agentic enterprise work.


The same seams kept appearing. Almost every job moved through the same handful of steps: screen the request for attacks, work out what the user intended, ground the answer in real data, check that the answer held up.


So we pulled each job out and tuned an open-source model to do it — targeted models, each handling the single job it was built for. We weren’t training from scratch. We were tuning on the patterns of millions of jobs the harness had already processed. Here’s a look at the models we run:


The job the agent has to do The model we run What it does St **atus**


Read the request and route it HyperClassifier Runs on every request before anything else, so speed is everything — about 50ms vs. roughly a second for a general model. GA Spring ’26


Check the request isn’t an attack Prompt injection defense (PID) Trained on the real prompt-injection attack surface instead of improvising a defense each time. Rolling out — GA Summer ’26


Screen every output for harmful content Toxicity model Scores every LLM response for violent, sexually explicit, hateful, and physically harmful content before it reaches the end use Generally available (GA)


Judge whether the answer holds up TextEval An independent evaluator checks grounding, citation, response quality, task resolution, and instruction adherence Generally available (GA)


Rank what retrieval pulls back TextRerank Built for high-throughput reranking, the high-volume step a generalist makes slow and costly. Generally available (GA)


*Agentforce Voice additionally uses Deepgram (speech-to-text) and ElevenLabs (text-to-speech), deployed within Hyperforce infrastructure so audio never leaves Salesforce’s trust boundary. Voice turn detection is handled by the HyperClassifier above.*


Now let’s dive deep into the job to be done of each model, where we started, and how we’re solving the problem today.


#### 1. Reading the request: Intent detection and routing


The first thing an[AI agent](https://www.salesforce.com/agentforce/ai-agents/) has to do is understand what the user is actually asking and route it to the tool for the job. A service request about a billing dispute is inherently different from a request to update a contact record. The general model treated all of these as reasoning tasks — which means it used the same slow, expensive process to handle each of them.


We replaced it with the[HyperClassifier](https://engineering.salesforce.com/solving-real-time-ai-classification-for-agentforce-how-single-token-prediction-delivers-30x-faster-agent-responses/) , a purpose-built classifier we fine-tuned on GPT-OSS-20B (OpenAI’s open-source Mixture-of-Experts architecture, quantized to run fast) and operate entirely within the Einstein Trust Layer. It reads the request, classifies the topic across up to 200 possible labels, and routes to the right subagent. It returns an answer in roughly 26 milliseconds — compared to around 1,446 milliseconds for a general frontier model. That’s not a rounding error: it’s roughly 55 times faster, and it runs on every single request, cutting user wait times substantially.


The HyperClassifier isn’t just faster than general-purpose modes, it’s more accurate and secure. Safety topic accuracy improved from 95% to 99%. Drift — the tendency to get pulled off-topic in a multi-turn conversation — dropped from approximately 20% to 10%. And because it only analyzes utterances and intent signals — and not the fully grounded prompt with CRM data — it can’t accidentally act on or share the most sensitive business context.


The HyperClassifier became generally available in Spring ’26 and is now the default intent detection and routing model for[Agentforce Service](https://www.salesforce.com/service/) and Employee Agent templates. The same model also powers semantic endpointing in[Agentforce Voice](https://www.salesforce.com/agentforce/voice/) : detecting, in real time, when a speaker has finished a thought rather than just pausing. That distinction — between a completed intent and a mid-sentence hesitation — is what makes a voice conversation feel like a natural exchange rather than a system waiting for silence.


#### 2. Defending the request: Prompt-injection screening


Before the agent acts on anything, it has to know the request is not an attack. An email body, a form field, a chat message — any of those can carry a crafted instruction designed to hijack the agent’s next action.


We built a dedicated injection-detection model for this, trained specifically on the most likely attack surfaces — CRM field values, inbound email bodies, form inputs, and transcript content — rather than improvising a defense from first principles. The model is embedded directly in the reasoning engine as a second independent detection layer alongside heuristic defenses. It classifies six distinct attack vectors: role-play and persona manipulation, prompt leakage, privilege escalation, encoding attacks (Base64, hex), privacy attacks, and malicious code generation. Every prompt receives an injection score, which is logged in an audit trail, and we are actively developing blocking mode, which will halt any detected attacks before sending them on to the reasoner.


#### 3. Screening every output: protection for toxic or unsafe content


After an LLM returns a response, it reviews it to make sure it doesn’t include any violent, sexually explicit, hateful, or physically harmful content. There’s just one problem: like humans, LLMs can have a blind spot when it comes to judging their own work. They tend to give themselves the benefit of the doubt. So we launched a specialized model to screen every response, assign it a toxicity score, and log it to the Einstein Trust Layer Audit Trail.


#### 4. Judging the answer: citation, task resolution, and instruction adherence


Once the agent has reasoned, the work still has to be checked — is the answer grounded in real data, does it cite a traceable source, did the agent follow its instructions, was the task actually resolved? A general model tends to grade its own work generously, the same blind spot we saw with toxicity screening. So we hand the judging to a separate model.


TextEval does that job. It’s a 20-billion-parameter model fine-tuned from GPT-OSS-20B, and it evaluates the response across several dimensions at once: whether answers are grounded in the customer’s own data, identifying sources and generating citations to support the response, whether the agent adhered to its instructions, and whether the task was resolved. The check is built into the runtime and can’t be modified by admins — a system-level guarantee, not a configuration. The result is measurable: instruction adherence and task-resolution accuracy run consistently higher than when a general model judges its own output.


#### 5. Reranking search results: Owned reranker


When the agent retrieves knowledge to ground an answer, the order of results matters. A poorly-ranked list of articles sends the LLM toward the wrong answer, even when the right answer is somewhere lower in the results. We run our own text ranking model for high-throughput retrieval reranking — deciding which retrieved content rises to the top before anything reaches the reasoning engine. It migrated from third-party hosting to Amazon Bedrock with zero customer-facing downtime and latency parity, and now runs across regions including a multilingual reranker for global deployments.


## Intelligence was never the gap


Frontier models are extraordinary, and we’re not betting against them — today, a frontier model handles the core reasoning in our stack, and our architecture lets our customers plug in the frontier model of their choice. But raw intelligence was never the thing standing between an enterprise and a resolved case.


More intelligence doesn’t close that gap, but knowing how enterprise companies actually work does. Those patterns took us decades to learn, and eighteen months to build into our agentic infrastructure, including our harness, models, and agentic apps. The result isn’t a smaller stack, or a cheaper one. It’s a precise one: the right intelligence for each job, and no more. Precision over power: that’s the real frontier.
