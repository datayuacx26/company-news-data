---
schema_version: "1.0.0"
document_id: "c5c7d29de496e28a1f9a741027688c1e27ad183b4b2211ca2465b08ca0485da7"
company_key: "yc-observe-ai"
company: "Observe.AI"
source_id: "yc-observe-ai-news-import-a6122144423e"
canonical_url: "https://observe.ai/blog/beyond-the-prompt-what-building-enterprise-ai-taught-me-after-leaving-quality-assurance"
published_at: null
first_seen_at: "2026-07-22T06:47:49.415185+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:8d41f0ceec8a8bb97c42454d786826113e733bd352d7e64b400421f522656f45"
---

# Beyond the Prompt: What Building Enterprise AI Taught Me After Leaving Quality Assurance

Scroll through LinkedIn today, and you’d think building an AI agent is a weekend project: just write a clever prompt, ingest some data, and watch the magic happen.


I’ve spent the past several months as a Modern AI Engineer, building and deploying AI systems for enterprise Customer Experience teams at Observe.AI. I can tell you firsthand that narrative is an illusion.


Before joining the Forward Deployed Team (FDT), I worked as a Quality Engineer focused on platform experience. My job was straightforward: break the platform before the customer could, find edge cases, and ensure a seamless experience. My metric for success was a functionally perfect, deterministic system. A test either passed or it failed.


When I transitioned into a Forward Deployed Engineer (FDE) role, building and deploying probabilistic AI agents for large enterprise Customer Experience environments, it forced a complete mindset shift. The primary question was no longer, “Does this feature work?” It became, “Why are we building this, and what business outcome does it drive?”


My roots in Quality Assurance shaped how I approached problems. I believed that if I tested rigorously enough, I could engineer out uncertainty entirely.


AI taught me the opposite.


The LLM-first systems I work on have a different ethos altogether. The goal is not to eliminate ambiguity. It is to understand where ambiguity exists and build systems that can operate safely within it.


I initially thought my new role would mostly revolve around prompt engineering and building workflows. But when I stepped into the enterprise reality, I quickly realized that the model itself is just the tip of the iceberg.


###
Testing the Untestable: When Non-Determinism Becomes Your New Normal


In traditional software development, the rules of engagement were comforting to me. I would write a script, hunt for edge cases, and validate that clicking a specific button updated the database exactly as expected.


AI systems do not behave that way.


Two customers might ask for the same outcome using completely different phrasing, terminology, tone, and context. The system has to understand both. It has to interpret intent, apply the right business logic, and respond in a way that feels natural and compliant.


My QA background became the foundation for building enterprise guardrails. Instead of testing for a 404 error, I test for hallucinations, tone deviations, and context loss. Quality Assurance in the AI era is less about proving a system is always correct and more about defining safe boundaries for uncertainty.


But technical correctness is only half the challenge. The second battle is adoption through trust. An AI agent can feel like an intimidating black box to a Customer Experience supervisor. If it flags a false positive on a compliance check, the user may immediately reject the tool.


I’ve had to design human-in-the-loop interfaces that explain why the AI reached a certain conclusion. If you do not design for human psychology, even the most technically sound system can get shelved.


###
The 90% Below the Waterline


Once I cleared the trust hurdle, I ran straight into the enterprise reality: the model itself is often less than 10% of the actual deployment effort. The remaining 90% live below the waterline.


By that, I mean the real complexity is often unseen by customers and even by others on the team. Enterprise deployments are rarely smooth sailing. In my experience, every day surfaces a new challenge I did not see coming.


Getting an AI agent live requires me to master a massive behind-the-scenes ecosystem.


Environment Management: Managing entirely different data sets across Dev, UAT, and Production environments.


Integrations and Authentication: Securely connecting to external systems by passing client IDs, secret keys, and other authentication details.


Compliance and Privacy: Ensuring PII is protected and that call recordings adhere to applicable compliance requirements.


Root Cause Analysis: When an issue occurs, it is not always a quick bug fix. It often requires reviewing multiple calls, logs, transcripts, and system events just to find the root cause.


These agents play a critical role for my customers, and any issue I fail to catch can severely impact their business. The ecosystem is intricate, and it only becomes more interconnected as the technology scales. When things break, they rarely break in obvious ways.


When an issue arises in production, it is rarely a simple bug. It is a chaotic, multi-system investigation.


###
Deploying in a Language You Don't Speak


Nothing exposed the true complexity of AI engineering like the moment I had to deploy a voice agent in a language I do not speak.


During my pilot, local testers consistently reported that the agent sounded abrupt and impersonal. The transcripts looked correct, the intent classification was accurate, and all my backend metrics passed with flying colors. The math said it was working, but the human testers hated it.


‍


The issue was not the logic. It was the delivery.


‍


The culprit turned out to be our TTS tuning. Small adjustments to stability and similarity settings completely changed how users perceived the interaction. I also had to aggressively use word boosting in our STT settings to ensure the AI captured localized business terminology accurately.


‍


Because I did not speak the language natively, I had to rely on a tight feedback loop with local testers, translating their qualitative feedback on tone into quantitative tuning adjustments.


‍


### The Cross-Functional Orchestra


‍


None of this happens in a vacuum.


‍


Getting all of these pieces in place requires significant cross-functional coordination, and I cannot pull off a deployment like this in a silo. The best agent design is useless if it gets blocked by Legal, fails Security review, or never earns adoption from regional managers.


‍


Communication is not a soft skill here. It is the operational engine that keeps the project alive.


‍


### The Modern AI Engineer


‍


What I’ve learned is that the technical baseline- writing clean code, understanding data ingestion, and fine-tuning LLM behavior- is table stakes.


‍


What actually defines my work as a Modern AI Engineer is everything built on top of it: customer empathy, stakeholder alignment, ambiguity management, and the ability to translate messy human problems into reliable AI systems.


‍


My experience building enterprise AI has made one thing crystal clear: the hardest part is not generating the right answer. It is earning enough trust for the answer to actually be used in a real business process.


‍


That is the part no prompt can solve for me. And it is what makes this work as a Modern AI Engineer so endlessly challenging and so worth doing.


‍
