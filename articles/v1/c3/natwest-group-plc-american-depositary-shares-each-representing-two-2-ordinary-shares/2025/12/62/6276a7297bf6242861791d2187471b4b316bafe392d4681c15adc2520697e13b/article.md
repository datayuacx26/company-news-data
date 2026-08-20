---
schema_version: "1.0.0"
document_id: "6276a7297bf6242861791d2187471b4b316bafe392d4681c15adc2520697e13b"
company_key: "natwest-group-plc-american-depositary-shares-each-representing-two-2-ordinary-shares"
company: "NatWest Group plc"
source_id: "natwest-group-plc-american-depositary-shares-each-representing-two-2-ordinary-shares-rss-a9eea26f1824"
canonical_url: "https://nwg.ai/natwest-group-at-neurips-2025-349f0cd56e80"
published_at: "2025-12-19T09:16:27+00:00"
first_seen_at: "2026-07-20T04:35:38.759256+00:00"
fetched_at: "2026-07-28T22:24:52.050374+00:00"
content_hash: "sha256:5f11dd20a0f202d593eaa37256dd7a61e160fed59adcf2eb55ae212dc634e719"
---

# NatWest Group at NeurIPS 2025

Neurips


Financial Services


Natwest


AI


Machine Learning


# NatWest Group at NeurIPS 2025


[NatWest Group](https://medium.com/@NatWestGroup?source=post_page---byline--349f0cd56e80---------------------------------------)


6 min read


·


Dec 19, 2025


--


by[Cristovao Iglesias (Senior AI Engineer, AI R&D)](https://www.linkedin.com/in/cristovao-iglesias/)


NeurIPS 2025 marked a milestone as a true “mega-event”, showcasing the AI field’s rapid evolution, deep industrial integration, and heightened societal awareness. This year’s conference even featured a dedicated workshop for the finance sector, reflecting the breadth of AI applications. The scale of participation was unprecedented: the main track received[21,575 valid submissions](https://blog.neurips.cc/2025/09/30/reflections-on-the-2025-review-process-from-the-program-committee-chairs/) , with 5,290 papers accepted.


NatWest Group returned to NeurIPS as a Silver sponsor (an upgrade from Bronze in 2024) affirming our commitment to advancing AI research and its responsible application in financial services. Our presence included a booth in the Exhibition Hall and an accepted workshop paper, showcasing NatWest’s role at the forefront of applied AI innovation in the finance sector. The team on-site was Raad Khraishi (Head of AI R&D), Euan Wielewski (Head of Applied AI) and me, Cristovao Iglesias (Senior AI Engineer). At the booth, we had engaging conversations with researchers and practitioners about applying AI in a regulated environment, covering topics such as deployment, evaluation, governance, and reliability. The interest didn’t stop there, we also had plenty of interest in internships and roles, and (unsurprisingly) the NatWest Group giveaways disappeared fast.


Notably, NatWest Group was the only UK banking institution listed as an official sponsor at NeurIPS 2025, where the finance sector had a strong presence: 31 finance-related organizations participated as sponsors, accounting for roughly ~24% of the 132 official sponsors. It’s a strong signal that cutting-edge ML is increasingly central to modern finance.


The NatWest Group team at NeurIPS 2025


## What stood out at NeurIPS 2025


A helpful lens on NeurIPS 2025 is the **Best Paper Awards** : four Best Papers (including one from the Datasets & Benchmarks track) and three Runner-Ups ([award announcement](https://blog.neurips.cc/2025/11/26/announcing-the-neurips-2025-best-paper-awards/?utm_source=chatgpt.com) ). Together, they map closely onto the themes that dominated the week: reasoning and evaluation, mechanistic scaling, LLM architecture, generalization vs. memorization, and theory.


On reasoning, the Runner-Up[Does Reinforcement Learning Really Incentivize Reasoning Capacity in LLMs Beyond the Base Model?](https://openreview.net/forum?id=4OsgYD7em5) challenged a popular assumption: RL with verifiable rewards can improve **sampling efficiency** , but doesn’t reliably create **new** reasoning capability. Scaling discussions were similarly mechanism-driven:[1000 Layer Networks for Self-Supervised RL](https://openreview.net/forum?id=s0JVsx3bx1&utm_source=chatgpt.com) showed depth can unlock stronger goal-reaching behavior, while[Superposition Yields Robust Neural Scaling](https://openreview.net/forum?id=knPz7gtjPW&utm_source=chatgpt.com) argued superposition helps explain scaling laws. On architecture,[Gated Attention for Large Language Models](https://openreview.net/forum?id=1b7whO4SfY) demonstrated that a simple head-specific gating change can improve stability, scaling properties, and long-context performance. For generative modeling,[Why Diffusion Models Don’t Memorize](https://openreview.net/forum?id=BSZqpqgqM0&utm_source=chatgpt.com) provided a clearer account of generalization vs. memorization via two predictable training timescales, offering a more grounded way to think about training duration, dataset size, and overfitting risk. Evaluation also stayed front and center, supported by efforts like the[LLM evaluation workshop](https://sites.google.com/view/llm-eval-workshop) — and[Artificial Hivemind](https://openreview.net/forum?id=saDOrrnNTz) highlighted both homogenization across models and miscalibration between automated judges, reward models, and diverse human preferences. Finally,[Optimal Mistake Bounds for Transductive Online Learning](https://openreview.net/forum?id=EoebmBe9fG) resolved a 30-year-old open problem and quantified how unlabeled data changes online mistake bounds.


## Beyond the best papers: practical LLM papers worth reading


Beyond the award papers ([and reflecting how dominant LLM work remains at NeurIPS](https://newsletter.languagemodels.co/p/the-illustrated-neurips-2025-a-visual) ), there was a wave of “systems-adjacent” research that feels immediately useful for teams shipping real products, especially in regulated settings like finance.


## Get NatWest Group’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Agent memory stood out:[A-Mem](https://openreview.net/pdf?id=FiM0M8gcct) proposes a Zettelkasten-style memory that links and evolves notes over time, while[CAM](https://openreview.net/pdf?id=ACSOnSHiWe) explores more structured, constructivist memory for recall. On evaluation and reliability,[AlignEval](https://openreview.net/pdf?id=OBaK9JSbHk) assesses models *as evaluators* , and[Bridge](https://openreview.net/pdf?id=bEP87LNTfX) formalizes gaps between human and LLM judgments. For uncertainty-aware interaction,[Conformal Information Pursuit](https://openreview.net/pdf?id=xAHozxfuUW) uses conformal sets to guide questioning, and[MemSim](https://openreview.net/forum?id=vAT2xlaWJY) offers a Bayesian simulator for evaluating memory in LLM personal assistants. Finally, on governance/efficiency/security,[DATE-LM](https://openreview.net/pdf?id=e2cD5xuHix) benchmarks training-data attribution,[KVLink](https://openreview.net/pdf?id=oDcAGSXZZP) reuses KV caches across overlapping contexts to cut compute,[Activation Control](https://openreview.net/pdf?id=XNo4yS9n1k) steers a small set of activations to elicit longer reasoning, and[VERA](https://openreview.net/pdf?id=XBD4OjCcIA) targets black-box jailbreaking for API-only models.


## Several talks provided particularly strong intellectual anchors for the week


The invited talks were sharp, opinionated, and focused on **what the field should do next** .[Melanie Mitchell](https://neurips.cc/virtual/2025/loc/san-diego/invited-talk/109607) emphasized the gap between strong benchmark results and genuine understanding;[Kyunghyun Cho](https://neurips.cc/virtual/2025/loc/san-diego/invited-talk/109605) reflected on “problem finding” in AI and shared a set of[open questions](https://drive.google.com/file/d/1nGHFAx4wSo30B9td5gSEUb7eyZy0Fe7u/view) ; and[Andrew Saxe](https://neurips.cc/virtual/2025/loc/san-diego/invited-talk/109602) offered a clear, technical perspective on learning dynamics and the role of depth.[Yejin Choi](https://neurips.cc/virtual/2025/loc/san-diego/invited-talk/109603) argued that scaling has produced impressive capabilities but also “jagged intelligence,” and called for more principled reasoning and evaluation, especially methods that help smaller models close the gap in specific domains.[Rich Sutton](https://neurips.cc/virtual/2025/loc/san-diego/invited-talk/109601) (2024 Turing Award) revisited the “bitter lesson” and presented the Options and Knowledge (OaK) architecture as a vision for continual, experience-driven learning and planning, while highlighting catastrophic forgetting as a key blocker for deep continual learning at scale.


Rich Sutton during his presentation: “The Oak Architecture: A Vision of Super Intelligence from Experience”


## The most urgent talk: trust and verification


[Zeynep Tufekci](https://neurips.cc/virtual/2025/loc/san-diego/invited-talk/109606) , argued that the biggest risks from generative AI may arrive long before anything resembling AGI. She warned that “artificial good-enough intelligence” is already sufficient to destabilize trust at scale by making persuasion, impersonation, and manipulation cheap, fast, and widely accessible, eroding the everyday signals we use to verify truth, authenticity, effort, and even whether a person is real. Her call to action was to shift attention from optimizing engagement toward building stronger proof infrastructure: cryptographic and provenance mechanisms for authentication.


For finance, this matters immediately, if authenticity and intent become cheap to fake, identity verification weakens, social engineering gets easier, compliance evidence becomes harder to validate, and the priority shifts from model capability to system integrity: provenance, audit trails, authentication, and robust controls.


Zeynep Tufekci during her presentation: “Are We Having the Wrong Nightmares About AI?”


## Papers we presented at NeurIPS 2025


NatWest Group participated in the[Generative AI in Finance workshop](https://sites.google.com/view/neurips-25-gen-ai-in-finance/home) with the paper:
[Operationalising LLMs for Compliance-Critical Letter Writing in Financial Services](https://neurips.cc/virtual/2025/loc/san-diego/132535) .


The paper describes a production LLM system supporting complaint-resolution letters: since launch (Nov 2024) it has generated tens of thousands of compliant, personalized letters, improved quality (+30%), reduced drafting time (-62%), and delivered measurable operational impact (including doubling the three-day resolution rate). We also describe the end-to-end architecture (continuous “LLM-as-judge” compliance monitoring, safeguards for a regulated setting, and a scalable deployment strategy) plus practical lessons on iteration, user engagement, and transparent evaluation.


NatWest Group’s Raad Khraishi presenting a poster at the Generative AI in Finance workshop


In addition to our workshop paper, Raad Khraishi and I presented our own **position paper** , written in a personal capacity and on personal time (expressing our own views), which was selected for **oral presentation** :[Real-Time Hyper-Personalized Generative AI Should Be Regulated to Prevent the Rise of “Digital Heroin”](https://neurips.cc/virtual/2025/loc/san-diego/poster/121952) .


In this paper, we argue that once generative models can produce content in real time and platforms optimize relentlessly for engagement, the combination of hyper-personalization and tight feedback loops risks creating a powerful compulsion engine (akin to “digital heroin”) with disproportionate harm for younger and vulnerable users. We propose acting early through audits/transparency, friction-by-design, age protections, and a proactive research agenda that brings technologists together with public-health and behavioural experts. The paper sparked strong discussion in both the oral and poster sessions, including conversations with[Yoshua Bengio](https://yoshuabengio.org/) on complementary mitigation approaches and the importance of involving domain experts such as psychologists alongside technologists and policymakers in the design of regulatory and mitigation frameworks.


Raad Khraishi presenting the position paper


## Taking ideas home and confidence in our direction


NeurIPS 2025 was a strong reminder of why this conference matters: it’s where foundational advances (reasoning, scaling, evaluation) meet the realities of building and operating AI systems in the real world. For us, the week was energising not only because of the conversations at the NatWest Group booth and our two presentations, but also because we repeatedly came across papers tackling problems very similar to the ones we are working on. That overlap is a good signal: we’re tackling the right challenges, in the right direction. We came back with concrete ideas to incorporate and renewed momentum to keep pushing forward on applied, trustworthy AI for financial services.
