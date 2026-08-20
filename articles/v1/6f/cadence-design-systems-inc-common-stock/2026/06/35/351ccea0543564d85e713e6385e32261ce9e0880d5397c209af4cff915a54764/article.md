---
schema_version: "1.0.0"
document_id: "351ccea0543564d85e713e6385e32261ce9e0880d5397c209af4cff915a54764"
company_key: "cadence-design-systems-inc-common-stock"
company: "Cadence Design Systems Inc."
source_id: "cadence-design-systems-inc-common-stock-news-import-fe14fc9857d2"
canonical_url: "https://community.cadence.com/cadence_blogs_8/b/corporate-news/posts/honda-cadence-physical-ai-part-2-where-physical-ai-will-be-won"
published_at: "2026-06-22T16:00:00+00:00"
first_seen_at: "2026-07-24T23:33:23.620609+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:aeff90508561f719b698e3d68c282f84ec4c2f96c998da9274dd5a48f7445cf3"
---

# Honda + Cadence = Physical AI (part 2): Where Physical AI Will Be Won

# Honda + Cadence = Physical AI (part 2): Where Physical AI Will Be Won


22 Jun 2026


• 8 minute read


-
-
-
-


Hello everyone, I'm Atsushi Ogawa, Center Head of HGR.


The real challenge of physical AI ultimately comes back to implementation and verification. No matter how precise a model may be, if it cannot be put into a usable form in the field, it remains only a possibility.


In this second part, the discussion continues with Takahide Yoshiike, who leads the Frontier Robotics Domain, and Mr. Anirudh Devgan, President and CEO of Cadence.


"The moment the error rate dropped below 0.2%, the entire organization moved." From a world where re-establishing correlation was taken for granted, to a world where correlation with wind tunnels may no longer be necessary—we explore the critical point at which physical AI begins to change even what becomes unnecessary.


## Accelerated by GPUs, Optimized by AI—Tools That Expand the Design Space


**Ogawa:** In the first part, we discussed how physical AI is ultimately a challenge of implementation, and we heard about field-level issues such as touch, simulation, and distributed architecture. In this second part, I'd like to go deeper into how those challenges can be solved. To begin, what approach is Cadence taking toward simulation for physical AI?


**Devgan:** In one phrase, our approach is to integrate all layers of the "three-layer cake" we discussed earlier. Cadence has traditionally been more of a software-oriented company. But to truly address physical AI, we realized that we needed the middle layer—ground truth, physics itself. That is why we acquired Hexagon D&E, Beta, and Cascade.* We needed to have real physical models.


**Ogawa:** I see. And that is combined with Cadence's existing strength in silicon design.


**Devgan:** Exactly. On top of that, we add GPU acceleration through the Millennium supercomputers, and then use AI agents to optimize design. First, we integrate multiphysics analysis into one platform, accelerate it with GPUs, and then have AI explore the design space. Completing that flow is our challenge.


## The Day Wind Tunnels May No Longer Be Needed—A World Without Re-Establishing Correlation


**Ogawa:** Simulation combined with GPUs has truly changed things. GPUs existed ten years ago, but simulation software could not fully draw out their capabilities. Now that has become possible. The process through which we switched from the analysis software we had used for many years to Cascade symbolizes exactly that. We knew Cascade was clearly more accurate. But it was not easy to switch.


**Devgan:** Why was that?


**Ogawa:** Because millions, even tens of millions, of calculations had been accumulated in correlation with the old software. Changing the software meant re-establishing all of that correlation. That cost is greater than most people imagine.


**Devgan:** The exact same thing has happened in the semiconductor world.


**Ogawa:** But then, at one point, the error rate dropped dramatically. The moment it went below 0.2%, the entire organization moved. The atmosphere changed to: "Maybe we no longer even need to re-establish correlation with the wind tunnel."


**Devgan:** Not needing correlation with the wind tunnel—that is remarkable.


**Ogawa:** There is a threshold for this. The meaning of improving from 3% to 2% is completely different from improving from 0.5% to 0.1%. If one engineer reduces error from 3% to 2%, people may simply say, "That makes sense." But the evaluation of someone who improves it from 0.5% to 0.1% is entirely different. Only when you reach that level of accuracy does organizational behavior begin to change.


**Devgan:** That is also what happened in semiconductors. In the past, engineers did not trust simulation. But after many years of proving its accuracy, now almost all silicon customers no longer build physical prototypes. Everything is designed and verified on computers. Once that became possible, optimization increased explosively. Because people could trust the accuracy, they moved from asking, "Is this design acceptable?" to asking, "How can we make it the best?"


**Ogawa:** As accuracy improves, the way it is used changes.


**Devgan:** Exactly. What we are working on now is putting AI optimization on top of Cascade and MSC.


**Ogawa:** How is AI optimization different from conventional optimization?


**Devgan:** Mathematical optimization is inherently local. Methods like gradient descent search around the current point step by step, so they can only see a small part of a vast design space. It is difficult to find the global optimum. But AI can operate based on a model, so it can explore the entire design space globally. That is the real beauty of it.


**Ogawa:** You said the same thing before, and I thought it was exactly right. In large organizations, numerical optimization tends to be gradient-based, which makes it easy to fall into local solutions. AI can go beyond that.


**Yoshiike:** That seems useful not only for design problems, but much more broadly as well.


**Ogawa:** Exactly. AI does not only accelerate design. AI can also improve the design process itself. You could call it "AI for physical AI"—using AI to design AI systems faster. Once that cycle starts turning, the speed of development will change dramatically.


**Devgan:** That is what we want to do together. By combining AI-driven optimization with accurate multiphysics simulation, we can close the gap between simulation and reality—what is often called the "Sim-to-Real Gap." About 2,000 people are currently involved in this project.


**Ogawa:** Two thousand people! That really shows the level of commitment.


## From Cloud to Edge—Power Constraints Will Define Architecture


**Ogawa:** Let me change the perspective a little. I'd also like to ask about future system architecture. As physical AI becomes more widespread, how will the relationship between cloud and edge change?


**Devgan:** I think it will change significantly. More AI processing will shift to the edge. The reason is power consumption. For physical AI, power constraints are extremely severe. A robot that needs to be charged every 15 minutes is not useful. So running AI at the edge with low power consumption will become the biggest engineering challenge.


****


**Ogawa:** Does that mean dedicated chips will become necessary?


**Devgan:** Yes. You cannot simply take a GPU designed for a data center and use it for physical AI. Automotive chips and robotics chips are different, and they also need to be optimized for each application. Algorithms, architecture, and even the combination of analog and digital will all need to be redesigned.


**Yoshiike:** When we talk about semiconductors, ASIMO is a good example. As we discussed in the first part, ASIMO used distributed ECUs. The control frequency for leg movement was so high that processing everything centrally would not have been fast enough. So processing was distributed to the extremities.


That design philosophy will fundamentally apply to future robots and automobiles as well. High-frequency processing should be distributed to the edge, while integrated decision-making should take place in upper layers. How to bring together multiple layers with different sampling times—that itself is one of the core questions in future system design.


**Ogawa:** It is the same idea as the human nervous system. Not everything is processed by the brain. Reflexes are processed at the spinal-cord level, which is why they are fast.


## The Next Five to Ten Years of Physical AI—Can We Run Through the Era When Correlation Is Still Necessary?


**Yoshiike:** Finally, how do you see the next five to ten years for HGR?


**Ogawa:** I believe the next five to ten years will be decisive for us. Right now, we are in an era when correlation between simulation and actual hardware is still necessary. In the future, as accuracy improves, that correlation may no longer be needed.


But paradoxically, only organizations that can lead during the era when correlation is necessary will survive even after correlation becomes unnecessary. That is because they understand the limits of simulation, understand the nature of error in physical experiments, and have experience connecting the two.


**Devgan:** And that will become Honda's strength.


**Ogawa:** Yes. When you grasp a handkerchief, its shape is slightly different every time. Knowing those "fluctuations of physics," knowing the errors caused by the environment, knowing the structure of experimental error—these accumulated experiences make it possible to establish the right correlation. That is not something you can build overnight.


**Devgan:** That accumulation is precisely Honda's asset built over decades. We have the ability to scale technology and turn it into tools. But you are the ones who know what actually happens in the field. That is why working together has meaning.


**Yoshiike:** From the perspective of robotics, the hand dexterity problem we are working on now—enabling robotic hands to move like human hands—will be decided over the next five to ten years. If simulation accuracy improves and AI optimization becomes usable, things that can currently only be tested with actual hardware will become testable on computers. We are standing at that turning point now.


**Ogawa:** Devgan-san, from your perspective, is there anything you would like to say to engineers as we move toward the era of physical AI?


**Devgan:** One thing I would like to mention is what is happening now at the top universities in the United States. Until five years ago, many of the best students chose only computer science. At Stanford, more than 60% of students were in CS. But now, a change is happening.


**Ogawa:** What kind of change?


**Devgan:** Programs that combine mechanical engineering and computer science are growing. Smart young people are beginning to realize that AI has value only when it is applied to a domain. They are studying for physical AI, for automobiles, for robotics. I think that is exactly what everyone at HGR embodies. Computer science plus AI plus mechanical engineering—people with that combination will be the most needed in the era ahead.


**Ogawa:** If we are talking only about language models, there are many similar players around the world. But physical AI goes beyond vision. It is a world where touch, sound, and diverse sensor information are all integrated. That is where we have a place.


**Devgan:** The word "physical" carries responsibility toward reality. Thank you very much for today. The message that left the strongest impression on me is this: people who understand physics will be the strongest in the age of AI. I believe our collaboration with Honda and HGR will change the industry.


**Ogawa:** Let's do it together.


**Learn more about[Cadence Physical AI,](https://www.cadence.com/en_US/home/ai/design-for-ai/physical-ai.html) and read[Honda + Cadence = Physical AI (part 1): What Does “Physical AI” Really Mean?](https://community.cadence.com/cadence_blogs_8/b/corporate-news/posts/honda-cadence-physical-ai-part-1-what-does-physical-ai-really-mean) .**


*Note: These are related technology groups that Cadence has brought under its umbrella through M&A. Hexagon D&E provides an industrial data and CAE ecosystem, including measurement and manufacturing; BETA CAE supports CAE workflows for vehicle development, including model generation and analysis processes; and Cascade provides simulation software for physical phenomena such as fluid dynamics and thermal behavior.
