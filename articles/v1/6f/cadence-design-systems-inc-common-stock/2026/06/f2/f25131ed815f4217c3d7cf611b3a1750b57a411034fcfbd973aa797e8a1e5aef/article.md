---
schema_version: "1.0.0"
document_id: "f25131ed815f4217c3d7cf611b3a1750b57a411034fcfbd973aa797e8a1e5aef"
company_key: "cadence-design-systems-inc-common-stock"
company: "Cadence Design Systems Inc."
source_id: "cadence-design-systems-inc-common-stock-news-import-fe14fc9857d2"
canonical_url: "https://community.cadence.com/cadence_blogs_8/b/corporate-news/posts/honda-cadence-physical-ai-part-1-what-does-physical-ai-really-mean"
published_at: "2026-06-15T23:00:00+00:00"
first_seen_at: "2026-07-24T23:33:23.620609+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:74e70ecb8fc5710ae482a4125d33f772a356d40f883e250113e458e6213e669a"
---

# Honda + Cadence = Physical AI (part 1): What Does “Physical AI” Really Mean?

# Honda + Cadence = Physical AI (part 1): What Does “Physical AI” Really Mean?


15 Jun 2026


• 7 minute read


-
-
-
-


Hello everyone, I'm Atsushi Ogawa, Center Head of HGR.


The more widely the term "physical AI" spreads, the simpler the question becomes: **Can AI truly move in the real world?**


Mr. Anirudh Devgan, CEO of Cadence, described physical AI as a "three-layer cake." At the top is agentic AI. In the middle is the "ground truth" of physics, chemistry, and biology. And at the foundation are silicon and data. No single layer alone can move reality.


At the same time, the field presents even more difficult barriers: the moment perception must switch from vision to touch, and the individual differences that simulation alone cannot fully capture. Together with Takahide Yoshiike, who leads the Frontier Robotics Domain at HGR, we explore what the "physical" in physical AI truly means.


## Physical AI Is a "Three-Layer Cake"—AI Alone Cannot Move the Real World


**Ogawa:** Devgan-san, thank you for joining us today. I'd like to begin with the term "physical AI," which we are hearing everywhere these days. It was also a major theme at CES this year. How do you understand this term?


**Devgan:** I've believed in physical AI for at least five years. The reason is that AI is an enormous market, but with 700 to 800 billion dollars being invested annually, it needs a "killer application" that can justify that level of investment. I have always thought physical AI would be one of those applications.


**Ogawa:** What do you mean by a "killer application"?


**Devgan:** It is helpful to think of AI as a three-layer cake. The top layer is agentic AI—LLMs and world models. The middle layer is ground truth, meaning knowledge of how things actually move and behave: physics, chemistry, biology. And the bottom layer is silicon and data. The important point is that unless you are a two-year-old, you eat the whole cake together. You don't eat just one layer. Optimizing only one layer is not enough.


**Ogawa:** I see. Recently, many young researchers tend to think that AI alone is all you need.


**Devgan:** Exactly. People who have just graduated from school often think AI is enough. But engineers who graduated 30 years ago say, "You cannot build anything unless you understand the essence of how things work." Both are right. You need both.


**Ogawa:** Absolutely. In terms of knowledge of the physical world, my part is physics, and your part is AI. Physical AI exists where those two overlap. What kinds of application areas do you see in this "three-layer cake"?


**Devgan:** The first slice is "data center AI"—software applications like ChatGPT, already a market worth hundreds of billions of dollars. That is already happening. The second slice is physical AI: automobiles, drones, and robots. The automotive market alone is now over three trillion dollars. Drones are transforming the defense industry. Industrial robots are still not very intelligent, but some reports say humanoid robots could become the largest product category in history. Taken together, tens of trillions of dollars' worth of markets could be transformed by physical AI.


**Ogawa:** Speaking of robotics, Honda has worked on ASIMO for many years.


**Devgan:** I really loved ASIMO. I was watching it even ten years ago. I thought it was an amazing robot.


**Ogawa:** Thank you. Actually, Yoshiike-san and his team built ASIMO.


**Devgan:** Really? That is incredible.


## From Vision to Touch—The Challenge of Switching Modalities in the Field


**Ogawa:** If physical AI is so important, where do you see the biggest challenges today, Yoshiike-san?


**Yoshiike:** One of the challenges we face is "switching modalities." For example, imagine a robot inserting a wire-harness connector behind a wall. The moment its field of view is blocked, it has to switch its mode of perception from vision to touch.


**Ogawa:** From vision to touch.


**Yoshiike:** Exactly. We see this as a transition from "Arm Dexterity" to "Hand Dexterity." What HGR is focusing on in particular is the delicate motion control required at the fingertip and hand level.


****


**Ogawa:** And it has to learn that in real time. But if you try to do it through simulation, there are limits.


For example, if you try to simulate the action of grasping a handkerchief, the spatial resolution of the fabric becomes an issue. Every time you place the handkerchief down, its shape differs slightly. These individual differences make simulation even more difficult. In the end, there are many situations where AI has no choice but to learn from the real world in real time.


**Yoshiike:** Labeling tactile information is also a major problem. With vision, it is relatively easy to label things: "This is a cup," "This is a pair of scissors." But touch is different. We still do not have the vocabulary needed to accurately describe sensor input states in language.


**Devgan:** The requirement for real-time learning makes the problem even harder. If simulation cannot be used, then everything has to be learned on actual hardware. That costs both time and money.


**Ogawa:** But there is also an approach where we use both simulation and actual hardware, and correlate the two. Rather than relying completely on one or the other, we combine results from both to accelerate AI. I believe there is potential in that approach. But in order to establish that correlation correctly, you need people who understand physics. How is Cadence approaching physical AI?


**Devgan:** Our role is to make physical AI easier to design. We provide silicon solutions, better simulators, and AI-powered optimization tools. Honda builds actual products—actual robots, actual cars. You are on the side that changes the world. We are on the side that accelerates the design of those products. That is why this collaboration is so interesting.


**Ogawa:** Cadence has the ability to scale technology and provide it as tools. We understand the requirements of specific products. Combining our resources feels like a natural way forward.


**Devgan:** Let me shift the perspective a little. Many companies are now talking about physical AI. From HGR's point of view, where does your uniqueness or originality lie?


**Ogawa:** That connects directly to what Yoshiike-san just said. What we have to protect is "real things in real places"—quality, safety, and the joy of control. These are things Honda has built up over many years.


**Yoshiike:** From a technical standpoint, I think our accumulated knowledge lies in the fact that we understand the limits of simulation, we understand the nature of experimental error, and we have experience connecting the two. As I mentioned with the handkerchief example, even the same type of product can differ from one item to another. You can establish the right correlation only when you have people who understand those "fluctuations of physics."


**Ogawa:** Saying "establish correlation" sounds simple, but in reality, you can only do it when you know where the limits of simulation are and understand what is uncertain in physics. That is HGR's strength, and it is also the significance of our collaboration with Cadence.


## Explainability Becomes Safety—Distributed Architecture and Ground Truth


**Devgan:** May I ask one more question? What kind of architecture will be needed for control systems in autonomous driving and robotics?


**Yoshiike:** Robot control essentially requires a hierarchical distributed architecture. Motion control that must be processed at high frequency is distributed to the edge. Integrated decision-making is handled in upper layers. How to bring together multiple layers with different sampling times—that will be a key technology for future robot and vehicle systems.


**Ogawa:** ASIMO was the same. The control frequency for leg movement was so high that centralized processing could not keep up. So processing was distributed to the extremities.


**Devgan:** In that case, mixed-signal design, where analog and digital coexist, becomes even more important. To process AI at the edge with low power consumption, that approach will be essential.


**Ogawa:** And the more complex the system becomes, the more explainability becomes necessary for safety. This is also why we are particular about starting from a model-based approach in autonomous driving. Jumping directly into a fully autonomous planner is too risky, because you cannot explain where it might make a mistake.


In robotics and automobiles, we have to guarantee human life 100%. If the device is for enjoyment or efficiency, perhaps some degree of black box may be acceptable. But what we build is different. It must be designed in a way that explains why it moves the way it does.


**Devgan:** That is precisely why the "ground truth" of physics matters. A system becomes trustworthy only when the answer produced by AI is verified against physics.


**Ogawa:** Exactly. The "physical" in physical AI also carries the meaning of ensuring safety. In the second part, I would like to discuss the concrete integration of simulation and AI, and the outlook for the next five to ten years.


**Learn more about[Cadence Physical AI,](https://www.cadence.com/en_US/home/ai/design-for-ai/physical-ai.html) and stay tuned for part 2 of the Honda + Cadence series.**
