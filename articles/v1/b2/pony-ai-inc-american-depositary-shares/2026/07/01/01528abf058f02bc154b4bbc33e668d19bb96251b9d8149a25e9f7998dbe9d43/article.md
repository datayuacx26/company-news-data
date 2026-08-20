---
schema_version: "1.0.0"
document_id: "01528abf058f02bc154b4bbc33e668d19bb96251b9d8149a25e9f7998dbe9d43"
company_key: "pony-ai-inc-american-depositary-shares"
company: "Pony AI Inc."
source_id: "pony-ai-inc-american-depositary-shares-rss-5e5976f990b5"
canonical_url: "https://blog.pony.ai/from-teaching-ai-to-working-with-it-tiancheng-lou-on-the-changing-role-of-engineers/"
published_at: "2026-07-16T03:35:35+00:00"
first_seen_at: "2026-07-26T07:17:13.019566+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:f9fa4dd25415fc6be49d1cb63ead7c184bf30ff7d7068384ade7747b1bee1e5b"
---

# From Teaching AI to Working With It: Tiancheng Lou on the Changing Role of Engineers

#


Artificial intelligence is moving beyond generating content and answering questions. Its next frontier is the physical world, where AI systems must perceive complex environments, make decisions in real time and act safely alongside people.


At LEAP East 2026 in Hong Kong, Pony.ai Founder and CTO Dr. Tiancheng Lou shared his latest thinking on physical AI in a keynote speech and panel discussion. Drawing on more than a decade of work in autonomous driving, he explored **how advances in foundation models, AI coding, reinforcement learning and world models are enabling AI systems to learn and improve in new ways** —and fundamentally redefining the role of the engineers who build them.


As Tiancheng explained, **the role of engineers is changing fundamentally** . In the past, engineers taught systems what to do step by step. Today, their role is to build training environments, define objectives and establish evaluation frameworks that allow AI to find better solutions on its own. Looking ahead, engineers will increasingly serve as assistants to AI—helping it learn from human experience, accelerate its own evolution and identify better solutions to more complex problems.


Human knowledge and experience remain indispensable, but the way they contribute is changing—from prescribing every action to creating the conditions for AI to learn effectively and supporting its responsible development and deployment.


## From one-shot answers to self-improving systems


The rapid improvement of foundation models is changing how AI systems are developed. In particular, coding has emerged as an important measure of model capability because it enables AI to do more than produce a single answer to a prompt.


With a clearly defined objective and evaluation criteria, an AI agent can write code, test the result, identify problems and iterate based on feedback. This “agent loop” turns AI from a tool that responds to individual instructions into a system capable of executing complex tasks, evaluating the results and continuously improving its own performance.


For Tiancheng, the implications extend well beyond software development. The same approach can be applied to the autonomous driving development process: define clear objectives and evaluation criteria, allow the system to practice and assess its performance in a controlled environment, and use each cycle of iteration to improve its driving capabilities.


## The lesson of AlphaZero: AI must go beyond imitation


Tiancheng traced this idea back to a defining moment in modern AI. When AlphaGo defeated Lee Sedol in 2016, it showed that AI could master a highly complex game. AlphaGo Zero went further: instead of relying on records of games played by humans, it learned entirely through self-play and ultimately surpassed previous versions of AlphaGo.


The lesson was not that human knowledge had become irrelevant. Human knowledge can provide a valuable starting point for AI development, but it can also place limits on how far a system is able to progress.


This distinction is especially important in autonomous driving. A system trained on large volumes of human driving data can quickly approach human-level performance. But in learning to drive like a human, it can also inherit poor human driving habits. For an AI driver expected to meet a higher safety standard, imitation alone is not enough.


> “The goal has to shift from driving like a human to driving well,” Tiancheng said.


That shift sits at the heart of Pony.ai’s development approach. Instead of relying solely on large volumes of human driving data, autonomous driving systems can learn through reinforcement learning in virtual environments. Engineers define what good driving looks like, while the AI explores how best to achieve that outcome across a vast range of situations.


## Building a virtual world for real-world driving


For reinforcement learning to work in autonomous driving, the simulation environment must reproduce real-world driving scenarios as faithfully as possible. Vehicles do not simply identify lanes, traffic signals and other road users. They must interpret intent, respond to uncertainty and navigate complex interactions with drivers, cyclists and pedestrians.


The closer the virtual world is to reality, the more effectively an autonomous driving system can transfer what it learns into real-world operations. Reducing this “sim-to-real gap” therefore becomes a central technical challenge.


World models provide the foundation for this process. They allow autonomous driving systems to practice complex long-tail scenarios at scale, compare the outcomes of different decisions and improve automatically. The system does not have to wait for an extremely rare event to occur on public roads, nor does it need to assume the safety risks that would come with testing every possible response in the real world.


As the technology advances, AI can also take the lead across the self-improvement cycle: identifying weaknesses in the world model, determining what additional data and validation are needed, diagnosing root causes and proposing improvements that close the development loop.


This connects real-world operations, simulation, system evaluation and technical iteration in a continuous self-improving development loop. The same logic extends well beyond mobility. Humanoid robots, industrial robotic arms, drones and other AI systems operating in physical environments all need to learn and improve in safe, controlled virtual environments before they can be deployed at scale in the real world.


## Why society holds AI to a higher standard


The technical difficulty of autonomous driving is only part of the challenge. During the panel discussion, Tiancheng highlighted what he called the industry’s “double standard”: people generally tolerate occasional human error as an unavoidable part of life, but may view a single mistake by an AI system as evidence of a wider failure.


This higher standard is understandable. People increasingly see autonomous systems as infrastructure, and infrastructure is expected to deliver consistent and dependable performance. The answer is not to ask society to lower its expectations, but to develop AI systems capable of meeting them.


For robotaxis, that means working toward safety performance substantially better than that of human drivers. It also means validating performance with evidence and giving the public opportunities to experience the technology firsthand.


Trust cannot be created through claims alone. It is built gradually through safe operations, transparent standards and direct experience. Regulators have an important role to play by establishing evaluation and monitoring frameworks that can independently track progress. Companies, meanwhile, must demonstrate that autonomous vehicles can operate safely, efficiently and consistently in real urban environments.


## Technology is the foundation, not the full equation


As autonomous mobility moves from pilot programs toward commercial deployment, technical capability must be matched by viable economics, reliable operations and supportive regulation.


Tiancheng noted that safety comes first, but the commercial sustainability of robotaxis also depends on cost structure, fleet scale, operational efficiency and passenger experience. As deployments expand, greater fleet density can improve vehicle utilization and overall operating efficiency. At the same time, a robotaxi must provide a service that passengers genuinely value, including a comfortable ride, a private cabin and a convenient journey from pickup to destination.


Regulation remains one of the most important factors shaping the pace of deployment. Its role goes beyond granting permission to operate. A clear regulatory framework creates a structured path for companies and policymakers to evaluate the technology, learn from real-world performance and build public confidence over time.


The process is necessarily gradual. But as system performance improves and regulators gain more evidence from actual operations, each can accelerate the other: expanded deployment generates more experience and data, while greater confidence enables further deployment.


## The defining challenge for this generation of engineers


Tiancheng closed his keynote with a broader question: **what human capability will AI never replace?**


His answer was that this remains an ongoing process—one in which people continue to identify possible answers, only for AI to disprove them. As AI progresses from assisting people and executing tasks toward innovation and system building, the boundary between human and AI capabilities continues to shift. Over the past year alone, AI has increasingly demonstrated its ability to take on work once regarded as distinctly human, from programming and model training to driving parts of the research and development process.


But at least for today, one conclusion is becoming clearer: rather than treating humans and AI as separate competitors, we should find more effective ways for them to collaborate. **Humans and AI working together can often achieve more than either can alone.**


In autonomous driving, this collaboration combines engineering expertise and human responsibility with AI’s capacity for continuous iteration.


> The defining challenge for this generation of engineers is to discover the most effective ways for humans and AI to work together—and to use that collaboration to build AI systems that are not only more capable, but also safer, more reliable and more valuable to society.


That may be the next stage of physical AI: moving beyond imitation, enabling AI to improve continuously and bringing that capability responsibly into the real world.
