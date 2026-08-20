---
schema_version: "1.0.0"
document_id: "bd49e3be4b6ceeae7eb535827612f05fbf25a43ff23a1a882e7335f146c4594d"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/lessons-running-inference-workloads"
published_at: "2026-07-02T10:00:00.013+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:d96b3a804e30c8b512ecde0172fac6159bbbb19613b944b665f6bb893a673e58"
---

# Built for Mass Scale: Hard-Won Lessons from Teams Running High Volume Inference Workloads in Production

Moving AI from a flashy demo to a high-volume production environment is a transition filled with hidden technical debt and infrastructure challenges. There’s a difference between calling the OpenAI API in a weekend prototype and serving 50,000 concurrent users who need sub-200ms latency, graceful fallbacks, and reliable output every single time. It is rarely a “model problem.” Instead, it is a problem of decisions, trade-offs, and architecture.


At[DigitalOcean Deploy 2026](https://www.digitalocean.com/deploy) , we hosted a panel of engineering leaders from **Workato** , **Hippocratic AI** , and **ISMG.** Moderated by Karnik Modi, DigitalOcean’s Senior Manager of Engineering, panelists shared the lessons they’ve learned while running inference workloads at scale.


The session focused on managing P99 latency spikes in real-time interactions, restricting agent permissions to prevent “admin” vulnerabilities, and ensuring infrastructure is policy-aware before production traffic hits. These insights move beyond model performance to address the orchestration and security guardrails required for reliable, mass-scale AI.


**Watch the full recorded session from Deploy 2026** :


## The Built for Mass Scale Panelists


Each panelist represents a company operating at the frontier of production AI, where the gap between a working prototype and a reliable system serving real users is the entire challenge. From orchestrating autonomous agents across thousands of enterprise applications to running real-time clinical voice conversations where latency is a patient-safety issue to deploying AI-powered intelligence across a global cybersecurity media network, these teams have confronted the infrastructure, governance, and architectural decisions that only surface at scale.


#### Oscar Wu — AI Research Technical Lead, Workato Research Lab


Workato is an enterprise integration platform that connects over 14,000 applications and has orchestrated more than one trillion automated tasks, and its AI focus has shifted to agentic orchestration‚—building, deploying, and governing autonomous AI agents that can reason, act, and execute multi-step workflows across enterprise systems without writing code. At production scale, Workato’s AI Research Lab confronts the hard problems of agent governance, tool selection accuracy across large tool inventories, and keeping inference fast and cost-efficient under sustained load.[Workato’s AI Research Lab runs its inference workloads](https://www.digitalocean.com/blog/workato-nvidia-technical-deep-dive-agentic-inference-cloud) on DigitalOcean’s AI-Native Cloud, where it achieved 67% lower inference costs and 77% faster time-to-first-token on NVIDIA Hopper GPUs.


#### Debo Datta — Co-founder, Hippocratic AI


Hippocratic AI builds safety-focused generative AI voice agents for healthcare, handling patient-facing tasks like post-discharge follow-up, chronic care management, medication review, and clinical trial coordination. Because each clinical conversation can span hundreds of turns in real time, the company’s core infrastructure challenge is maintaining sub-second latency at scale so that voice interactions remain empathetic and natural rather than robotic. Hippocratic AI was a[design partner for DigitalOcean’s inference engine](https://www.digitalocean.com/blog/powering-the-inference-era) and runs its patient-facing workloads on the platform, powering over 20 million patient interactions with 40% lower latency.


#### Dan Grosu — CTO/CISO, ISMG


[ISMG](https://ismg.io/) (Information Security Media Group) operates 38 media properties focused on cybersecurity, IT, and AI, serving security professionals across sectors like banking, healthcare, and government worldwide. The company has built its own enterprise AI platform, Apollo, which processes thousands of pages of interview transcripts and conference sessions to produce cybersecurity market intelligence and strategic assessments.


## AI Has Gone From “Secret Sauce” to Standard Infrastructure


The panelists agreed that the conversation around AI has changed over the last 18 months. It’s not enough to simply “have AI”; the focus is now on whether your stack can actually support it at scale.


> “AI is no longer a competitive edge. It’s competitive infra. The question has shifted from ‘Should we ship AI features?’ to ‘Is our enterprise stack actually going to work with AI agents?’” — **Oscar Wu, Workato**


When AI becomes the infrastructure, the GPU bill that powers it stops being an experimental line item and starts being a core operating cost. For companies like Hippocratic AI, which focuses on healthcare, AI is the core product, but it functions as a significant cost center due to the massive GPU requirements. The challenge is turning that cost into a safe, reliable revenue generator.


## What Works at Ten Requests Fails at a Million


Everything looks efficient in a controlled environment. However, the panelists identified specific “bottlenecks” that only appear once real production traffic hits.


## The Agentic Identity Crisis


When an AI agent has access to 5 tools, it works perfectly. When it has access to over 50, it starts to falter. Oscar noted that agents often begin picking the wrong tools because names sound similar or they lack a governed policy for execution.


## The Latency Trap


At Hippocratic AI, latency is more than a technical metric—it’s a patient safety concern. Their safety-focused LLM handles multi-turn clinical conversations where delays erode the empathy and trust required for effective care.


> “When volumes are low, you don’t really test the limits. Only when you scale do you notice the P99 latency. If you’re on a clinical phone call with 200 turns and your latency isn’t great, you are experiencing a slowdown in every single call.” — **Debo Datta, Hippocratic AI**


## Plan for the Architecture That Hasn’t Shipped Yet


Scaling AI inference is a constant cycle of redesigning your stack. As new architectures emerge, the winners will be teams whose foundations are agent-ready, policy-aware, and structured for trust.


> “Let AI surprise you. If you have your data organized, you are in the prime position to leverage AI to great success.” — **Dan Grosu, ISMG**


## Tighten Agent Permissions to Shrink the Blast Radius


As inference becomes more distributed, the security stakes rise. Dan from ISMG highlighted that while AI is a “superpower” for shipping faster, it also creates new liabilities. Every prompt sent out is a potential security risk if the infrastructure isn’t properly managed.


Oscar expanded on this point: “Don’t let your AI agents be admins. Even your interns aren’t admins. You need to treat the agent as a per-action delegate of the user with time-scoped, time-bound access.”


Dan noted that the “blast radius” of a security failure gets wider as AI integrates with more systems. Their move to reliable providers like DigitalOcean was driven by the need for traceability, logs, and reassurance that the “levers” were still under human control.


## The Riskiest AI Strategy Is No AI Strategy


Many leaders are still hesitant to integrate AI deeply into their existing stacks. The panel warned that this “wait and see” approach often leads to an insurmountable gap later on.


> “AI isn’t going to hide your messy enterprise stack. It’s just going to amplify it. The risk isn’t being six months late on a chatbot; it’s being two years late on the operating model that lets your AI safely do real work.” — **Oscar Wu, Workato**


Dan added that the most successful companies will be those who structured their data and workflows *before* they needed the AI. If your data is organized, AI can provide an immediate return on investment. Dan and his team have put together agents to create an “agent-based firewall” to stop complex security threats in real time.


---


Whether it’s Workato governing agents across thousands of enterprise apps, Hippocratic AI holding sub-second latency on clinical voice calls, or ISMG deploying agent-based firewalls against live threats, the common thread is the same: scaling inference is an infrastructure problem, not a model problem.


That is exactly the problem DigitalOcean’s AI-Native Cloud was built to solve: a single platform that integrates inference, compute, data, and agent runtime so teams can focus on shipping rather than stitching together vendors. If any of these lessons hit close to home, the stack is ready when you are.


→[Get started with DigitalOcean’s AI-Native Cloud](https://cloud.digitalocean.com/registrations/new)


### About the author


Hasan Nabulsi


Author


Content Marketing Manager


[See author profile](https://www.digitalocean.com/community/users/hnabulsi)


Hasan is a seasoned content marketer, having worked in the SaaS space for nearly a decade. He remains at the forefront of the latest trends in content marketing, AI, and the inference cloud space.


[See author profile](https://www.digitalocean.com/community/users/hnabulsi)
