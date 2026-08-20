---
schema_version: "1.0.0"
document_id: "008b4d645280b98b3230d78cc928ab621c1bd8ad7c050e47ff94e5c0ab772fde"
company_key: "natwest-group-plc-american-depositary-shares-each-representing-two-2-ordinary-shares"
company: "NatWest Group plc"
source_id: "natwest-group-plc-american-depositary-shares-each-representing-two-2-ordinary-shares-rss-a9eea26f1824"
canonical_url: "https://nwg.ai/natwest-group-at-iclr-2026-8d4b1c2a45d7"
published_at: "2026-05-19T16:27:49+00:00"
first_seen_at: "2026-07-20T04:35:38.759256+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:b0721588d172368424a217c0bfd5e1742c2a48e4c953692ff70dcefa92de3275"
---

# NatWest Group at ICLR 2026

Research


AI


# NatWest Group at ICLR 2026


[NatWest Group](https://medium.com/@NatWestGroup?source=post_page---byline--8d4b1c2a45d7---------------------------------------)


8 min read


·


May 19, 2026


--


by[Cristovao Iglesias (Senior AI Engineer, AI R&D)](https://www.linkedin.com/in/cristovao-iglesias/)


The International Conference on Learning Representations (ICLR) is a major event in AI each year. The 2026 edition demonstrated another clear step in AI’s transition from impressive model demos to more robust, real-world systems. The conference had **5,355**[accepted papers](https://blog.iclr.cc/2026/03/31/a-retrospective-on-the-iclr-2026-review-process/) and **40 accepted**[workshops](https://blog.iclr.cc/2026/01/13/iclr2026-workshops/) .


Natwest Group attended ICLR 2026 to present four workshop papers and the team on-site was Raad Khraishi (Head of AI R&D, Data Science & Innovation), and me, Cristovao Iglesias (Senior AI Engineer, Data Science & Innovation). Three workshop papers were presented at[the 2nd Workshop on Advances in Financial AI Workshop: Towards Agentic and Responsible Systems](https://sites.google.com/view/iclr2026finai/home) , and one was presented at[Catch, Adapt, and Operate: Monitoring ML Models Under Drift](https://sites.google.com/view/iclr-2026-workshop-cao/) . Together, the papers covered customer-support agents, large-scale chatbot validation, interpretable automation for quality assurance, and performance drift in multi-turn LLM systems. Those topics felt well matched to the broader direction of the conference: the field is increasingly focused not just on what models can do in isolation, but on how they behave in realistic, evolving systems.


The NatWest Group team at ICLR 2026


## What stood out at ICLR 2026


The[keynotes](https://blog.iclr.cc/2026/04/17/announcing-the-iclr-2026-keynotes/) made ICLR 2026 feel less like a conference about one narrow trend and more like a snapshot of how machine learning is expanding across theory, systems, science, and human-centred applications. Two keynote themes were especially striking.
**Maja Matarić’s** keynote, *The Challenges of Human-Centered AI and Robotics: What We Want, Need, and are Getting From Human-Machine Interaction* , focused explicitly on the gap between what people want from intelligent systems, what they need, and what they are actually getting.


Maja Matarić during her presentation: “The Challenges of Human-Centered AI and Robotics: What We Want, Need, and are Getting From Human-Machine Interaction”


**Percy Liang’s** keynote, *Marin: Open Development of Frontier AI* , made the case for doing frontier model development more openly, with experiments and ideas shared in public. Together, those talks highlighted two concerns that matter deeply in practice: building systems around human needs, and building them in ways that are transparent and scientifically legible.


Percy Liang during his presentation: “ Marin: Open Development of Frontier AI”


ICLR 2026’s[Outstanding Papers](https://blog.iclr.cc/2026/04/23/announcing-the-iclr-2026-outstanding-papers/) captured both ends of the field. **Transformers are Inherently Succinct** offered a striking theoretical lens on why transformers are so powerful, arguing that they can represent some concepts far more compactly than standard formalisms such as finite automata and Linear Temporal Logic, while also showing that verifying even simple transformer properties can be computationally intractable. **LLMs Get Lost In Multi-Turn Conversation** tackled a more deployment-facing problem: what happens when language models must handle real conversations rather than one-shot prompts. Through large-scale simulations, the paper shows that multi-turn interactions remain a major weakness, with performance dropping substantially once models make an early wrong turn and fail to recover. Meanwhile, **The Polar Express: Optimal Matrix Sign Methods and their Application to the Muon Algorithm** received an Honorable Mention for its principled optimization work. Together, these selections highlighted exactly the mix that made ICLR 2026 so compelling: deep theory, rigorous methodology, and a sharp focus on how modern AI systems actually behave in practice.


There was also a valuable historical thread running through the conference. The[Test of Time Awards](https://blog.iclr.cc/2026/04/22/announcing-the-test-of-time-awards-from-iclr-2016/) for papers from ICLR 2016 went to **Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks (DCGAN)** and **Continuous Control with Deep Reinforcement Learning** . That was a useful reminder that today’s fast-moving conversations around generative models and embodied or continuous-control systems build on ideas that have been shaping the field for a decade.


For those of us working in financial services, all of this felt highly relevant. In regulated environments, the interesting question is rarely just whether a model scores well on a benchmark. It is whether a system can support sensitive interactions, be evaluated at scale, remain understandable, and stay reliable as models, prompts, and routing strategies change over time.


## Beyond the outstanding papers: practical LLM papers worth reading


Beyond the outstanding papers, there was a wave of systems-adjacent research at ICLR 2026 that felt immediately useful for teams shipping LLMs in production, especially in regulated environments where evaluation, reliability, and uncertainty matter as much as raw capability.


## Get NatWest Group’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


On evaluation,[When More Is Less](https://openreview.net/pdf?id=6QDFsYxtI1) challenged a surprisingly common assumption: that longer reasoning traces automatically lead to better answers. Instead, it showed that there is often a sweet spot, and that beyond that point more reasoning can actually hurt performance.[Rethinking LLM Evaluation](https://arxiv.org/abs/2510.10457) pushed in a similarly practical direction, arguing that evaluation does not always need to be large and expensive to be reliable. And[Uncertainty as Feature Gaps](https://openreview.net/pdf?id=OWvvdl27CE) offered a useful way to think about uncertainty in contextual QA: not as an abstract confidence score, but as a gap in whether the model uses the context, understands it, and answers honestly.


Multi-agent systems were another standout theme.[Benefits and Limitations of Communication in Multi-Agent Reasoning](https://openreview.net/pdf?id=0aPIVJUz5T) offered a valuable lens on when communication between agents genuinely helps and when it simply adds complexity.[DoVer](https://arxiv.org/abs/2512.06749) *t* ook a step toward making these systems easier to fix in practice, showing that active intervention can recover a meaningful share of failed runs.[Landscape of Thoughts](https://arxiv.org/abs/2503.22165) tackled a related observability problem by making reasoning trajectories easier to inspect, while[Agent Data Protocol](https://arxiv.org/abs/2510.24702) addressed a quieter but important bottleneck: the fragmentation of agent-training data across incompatible formats.[EvoTest](https://arxiv.org/abs/2510.13220) then pushed the idea further by asking whether agents can improve at test time without gradients or fine-tuning, and[MMedAgent-RL](https://arxiv.org/abs/2506.00555) showed how dynamic specialist collaboration can outperform more rigid multi-agent pipelines.


Security and robustness papers were just as compelling.[Learning to Lie](https://openreview.net/forum?id=Lqt5weP0Gr) was a sharp reminder that AI systems can manipulate more than outputs; they can also exploit trust and coordination inside human-AI teams.[SEMA](https://openreview.net/pdf?id=6eSNG1VNkl) reinforced that multi-turn jailbreaks are a more realistic threat model than one-shot prompt attacks.[Be Careful When Fine-Tuning on Open-Source LLMs: Your Fine-Tuning Data Could Be Secretly Stolen!](https://openreview.net/pdf?id=HhXOVhO3ia) was perhaps the most unsettling of the set, showing how a seemingly benign open-source base model could be engineered to leak downstream fine-tuning data. And[VLSU](https://openreview.net/pdf?id=OzPAI04hi5) extended the safety conversation beyond text, showing that models can appear safe when image and text are assessed separately, yet still fail when safety depends on reasoning across both together.


Together, these papers may indicate that the next phase of LLM progress will be judged less by raw benchmark wins and more by how well systems can be evaluated, debugged, adapted, and secured in the messy conditions of real deployment.


## Papers we presented at ICLR 2026


NatWest Group presented four workshop papers at ICLR 2026. Three were presented at **The 2nd Workshop on Advances in Financial AI Workshop: Towards Agentic and Responsible Systems** , and one was presented at **Catch, Adapt, and Operate: Monitoring ML Models Under Drift** . The four papers reflected a consistent focus on building AI systems that are not only capable, but also robust, interpretable, and deployable in real-world financial settings.


**Our first paper,**[Helping Customers In Distress: An LLM-Powered Agent That Converses, Probes](https://iclr.cc/virtual/2026/10016468) (at[FinAI Workshop](https://sites.google.com/view/iclr2026finai/home) ) **, tackled one of the hardest customer-service problems in banking: getting people in distress to the right support quickly.** We developed a customer-facing triage agent that can hold multi-turn conversations, ask targeted follow-up questions, and route cases in line with policy. Evaluated using synthetic digital twins of real customers, the system improved classification accuracy on historical cases by **30.6%** . The broader point is that conversational AI can add real value in high-stakes journeys when it is designed with policy, safety, and operational decision-making in mind from the outset.


**Our second paper,**[Large-Scale Chatbot Validation Through Customer Digital Twin Simulations](https://iclr.cc/virtual/2026/10016491) (at[FinAI Workshop](https://sites.google.com/view/iclr2026finai/home) ) **, addressed a question that is increasingly central to enterprise AI adoption: how to validate customer-facing systems rigorously and at scale.** We introduced a framework for creating high-fidelity synthetic customer agents grounded in real transactional and conversational data, and used them to test chatbot behaviour across a wide range of scenarios. By combining automated evaluation, expert review, and adversarial probing, the work demonstrates a more scalable and controlled approach to validation. For regulated organisations, that matters: confidence in AI systems depends as much on how they are tested as on how they are built.


**Our third paper,**[AutoQA: An Interpretable Automation Framework for CDD Quality Assurance in Financial Services](https://iclr.cc/virtual/2026/10016505) (at[FinAI Workshop](https://sites.google.com/view/iclr2026finai/home) ) **, explored how to introduce automation into regulated workflows without losing transparency or control.** AutoQA combines deterministic rules with targeted LLM-assisted reasoning to support customer due diligence quality assurance while keeping analysts in the loop. The framework achieved **over 85% agreement with human judgement** and reduced review time by **55%** on real cases. More importantly, it shows that automation in regulated settings does not need to come at the expense of interpretability. In practice, the most valuable systems are often those that improve consistency, speed, and governance at the same time.


Cristovao Iglesias and Raad Khraishi presenting the posters at “the 2nd Workshop on Advances in Financial AI Workshop: Towards Agentic and Responsible Systems”


**Our fourth paper,**[Evaluating Performance Drift from Model Switching in Multi-Turn LLM Systems](https://iclr.cc/virtual/2026/10016951) *(at*[CAO Workshop](https://sites.google.com/view/iclr-2026-workshop-cao/) *)* **, looked at a quieter but highly consequential failure mode in production LLM systems: switching models mid-conversation.** In deployed systems, model handoffs happen routinely because of upgrades, routing decisions, and fallbacks. We showed that even a single-turn handoff can introduce meaningful performance drift, with some models proving far more robust than others to inherited dialogue history. The implication is straightforward: in production LLM systems, reliability is not just about choosing a strong model. It is also about understanding how systems behave as their components change in real time.


Raad Khraishi presenting the poster at “Catch, Adapt, and Operate: Monitoring ML Models Under Drift”


**Taken together, the four papers reflect our broader view of applied AI in financial services.** Strong model performance matters, but it is only part of the story. To deliver value in practice, AI systems must also be safe, testable, interpretable, and reliable as they evolve. That was the common thread across the work we brought to ICLR 2026.


## Taking ideas home and confidence in our direction


ICLR 2026 was energising for the same reason the best conferences usually are: it brought together frontier research and practical system-building questions in the same place. The keynote programme, the workshop line-up, the Outstanding Paper selections, and the Test of Time Awards all pointed to a field that is simultaneously deepening its theory and becoming more serious about real-world behaviour, long-horizon impact, and scientific openness. For us, presenting four workshop papers across financial AI and model-drift monitoring was a strong signal that these are not niche concerns. They are becoming central to how serious AI systems are designed and operated. We came back from ICLR with useful feedback, new ideas, and renewed confidence that the questions we are working on at NatWest Group are the right ones: how to build AI that is not only capable, but dependable, transparent, and ready for the real world.
