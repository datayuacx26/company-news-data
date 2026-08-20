---
schema_version: "1.0.0"
document_id: "7101dc3fee2a1e843523b95c39864d742040f3f70f3920797a2ac09089ecb721"
company_key: "cadence-design-systems-inc-common-stock"
company: "Cadence Design Systems Inc."
source_id: "cadence-design-systems-inc-common-stock-news-import-fe14fc9857d2"
canonical_url: "https://community.cadence.com/cadence_blogs_8/b/corporate-news/posts/honda-cadence-physical-ai-part-3-creating-a-chip-the-world-has-never-seen"
published_at: "2026-08-19T15:00:00+00:00"
first_seen_at: "2026-08-19T19:11:30.463410+00:00"
fetched_at: "2026-08-19T19:11:31.497266+00:00"
content_hash: "sha256:4f2258e91842a4fa155ed4d971f3bb271fd0ee93d2f0a4b635056967ed7b6afd"
---

# Honda + Cadence = Physical AI (part 3): Creating a Chip the World Has Never Seen

# Honda + Cadence = Physical AI (part 3): Creating a Chip the World Has Never Seen


19 Aug 2026


• 9 minute read


-
-
-
-


Hello everyone. I'm Kentaro Komori, an engineer in the Semiconductor Research at HGR.


In this article, I'd like to introduce Cadence that is our design partner and make the development of next-gen AI chip chips for physical AI.


In the age of physical AI, AI chips cannot compete on computing performance alone. Power, heat, cost, mass production, verification—only when all of these requirements are met can a technology truly reach the real world.


The first target we presented was this: "several quadrillion operations per second, with impossibly low power consumption." It was a specification completely outside the common sense of the industry. And yet, strangely enough, the conversation did not end there. From that point on, it became a journey of pursuing unprecedented performance together as one team.


Why did that happen? And what are we now reaching toward?


I sat down with Yoshikatsu Shiwa, who is working with me on this research at HGR, and Osman Javed, Vice President of Customer Solutions at Cadence, our partner in this effort.


## It Started with a Vision of "the Generation After Next"


**Shiwa:** Osman, thank you for joining us today. We've become quite used to visiting this office in San Jose, but today I'd like to talk about how we first met, and what kind of future we are looking toward as we move this research forward.


**Komori:** Semiconductor design usually starts by working backward from the next-generation product. But for us, that wasn't the starting point. We were looking even further ahead—at the generation after next—and asking, "If we work backward from there, what kind of chip will be needed?" That was the starting point for everything.


**


##### *From left: Yoshikatsu Shiwa, Osman Javed, Kentaro Komori*


**Osman:** I remember that first meeting very clearly. I was at Berkeley, in the middle of an Executive MBA class. Someone from the office contacted me and said, "Can you join right now?" So I stepped out of class, opened my laptop in a small room, and joined the meeting.


The target specification was several quadrillion operations per second, with power consumption that was "impossibly" low. Normally, that kind of conversation would end with, "That's not possible." But strangely, that didn't happen.


**Shiwa:** For us, the target and the application were clear from the beginning. But our know-how in semiconductor design was still not fully developed. That's why we needed to build both our knowledge and our capabilities together with the Cadence team.


**Osman:** Shiwa-san and Komori-san were thinking from the very beginning about how to differentiate in the market, while looking far into the future.


**Komori:** From our side, it felt like this: "We can see the destination, but we don't have a map." That's why, from the very beginning, we needed a partner who could imagine and draw that map with us.


**Osman:** The HGR team wasn't trying to create something excellent using only existing technology. Their attitude was to lead the technology and bring something into the world that does not yet exist. To put it another way, we are all swinging for a home run. Even if it means taking risks, we want to bring the best possible product to market. I think that is the basic instinct that unites this team.


## The Day We Could Describe SoC Design in the Language of Engines


**Komori:** The turning point was a two-day workshop. The moment Shiwa-san began drawing the engine development process on the whiteboard, Osman's expression changed.


**Osman:** I had always associated Honda with quality, durability, reliability, and excitement. My first motorcycle was a Honda 125cc when I was 16. My father also drove a Honda car. But when I actually met Honda team, I was surprised. I had not realized that Honda was so deeply committed to AI and autonomous systems.


In that discussion, we drew an engine development chart and an SoC development chart side by side on the whiteboard. The skills were different. The products were different. But the methodology and the process were surprisingly similar. As we translated engine terminology into SoC language, there were many moments where we realized, "We're talking about the same thing." That convinced me that there was a foundation for creating something together.


**Shiwa:** I remember that moment very clearly as well. It felt as if our language had completely synchronized with Osman and his team.


**Komori:** I felt that the flow from logic design to physical design in SoC development was very similar to specification design and detail design in engine development. I think that conversation was the true starting point of this collaboration.


**Osman:** That's why, to me, Honda is not simply a "customer" of Cadence. We are moving together with a One Team Approach.


This program has become a very deep partnership. A strong partnership cannot be built only on matching technologies or skills. As a team, and as people, we need to have things in common and be able to move toward the same goal. Standing in front of that whiteboard, I became convinced that we had that.


## **Tuning Co-Design into a Working System**


**Komori:** Even if the language aligns, design does not move forward unless there is a design process. So first, we created decision criteria. We divided the project into phases. We defined the criteria for judgment. We synchronized every week. We met face to face every quarter. Only when those things were in place could co-design truly move forward.


**Osman:** In the initial discussions, we organized the work from Phase 0 to Phase 4. Phase 0 was the analysis and visualization of AI-related IP—intellectual property and design assets. In Phases 1 and 2, we evaluated the IP and selected what could move forward. After that, we proceeded to the front end, back end, and tape-out, which is the final data output for manufacturing in phase 3 and 4.


The important point was not simply dividing the work into phases. We made clear the criteria for deciding whether to move to the next phase. As a result, we were able to move through three phases. Now we are entering the stage between Phase 3.5 and Phase 4. The Honda team is already on the side of driving the specifications. They are now able to define the hardware requirements themselves.


**Komori:** The physical design stage requires an extremely high level of technology. But when we wrote out the process and organized the important stages, we gradually began to see what needed to be done.


**Osman:** This project also involves partners other than Cadence and HGR. For example, Mythic is running a technically advanced project for HGR that no one has ever done before. In the packaging area as well, efforts are moving forward at a level that has never been achieved before.


Everything is discussed openly, and decisions are made at the right timing. I believe the reason these three companies can move in this way is because of the environment that Shiwa-san and Komori-san have created.


****


**Komori:** The way we proceed is simple. First, we complete the current phase and confirm whether all requirements have been met. Then, we discuss the architecture of the next SoC in depth, so that the next phase can begin immediately. And finally, what we must never forget is brainstorming: "Is there anything we can change to make this a better product?" We have built this up every week. Even at this very moment, that process is still moving forward.


**Shiwa:** We meet at least once a week. Development is moving at tremendous speed, so constant synchronization is essential. On top of that, we meet face to face once every quarter. I think that rhythm supports the deep synchronization among the three companies.


**Osman:** One thing I personally liked was that Honda meetings always begin with a schedule review. At first, that felt very fresh. Everyone's mind becomes aligned around the goal, the target, and the risks before the discussion begins. And that is updated every week.


With Shiwa-san and Komori-san, I don't think we have ever had what you would call a typical "business negotiation." It has always been Engineering First, Problem Solving First. Nothing else is needed.


## Refusing to Give Up on an AI Chip the World Has Never Seen


**Komori:** Honda's roots are in industrial products. That's why simulation can become a common language—and that was the point of connection with Cadence.


**Osman:** Honda is also a "super user" for Cadence. From architecture to SoC, package, PCB, thermal, structural, and aerodynamic analysis—everything applies. That is why working together sharpens both sides.


The people from Honda come from many different divisions, don't they? Normally, 90% of people would choose to continue doing "their current work" by making use of their existing careers. But you all decided to learn something new. That attitude is very strong.


**Shiwa:** As engineers, if we share the same mission of realizing a new technology that no one has ever achieved before, differences do not really matter.


**Komori:** We are betting on achieving both high performance and ultra-low power consumption at the same time.


**Shiwa:** Environment and safety—these are Honda's core values. Physical AI needs to acquire large amounts of data from various sensors and recognize, infer, and act in the real world. Moreover, this must be done on devices equipped with and processed by physicalAI, such as robots, without relying on data centers or the cloud. Motorcycles and robotics have limited space. They cannot carry huge batteries. That is why achieving both high performance and ultra-low power consumption is an absolute requirement.


**Komori:** If we can achieve high performance, low cost, and low power consumption together, the possibility of people around the world accessing Physical AI will expand. I believe that can become a solution for society.


**Osman:** I'm a Formula 1 fan, and what you are doing feels very similar to being told, "Build an F1 engine." Honestly, unless there is a little bit of craziness, you cannot move to the next stage. But you all turn that into reality through the speed of absorption and decision-making. That is what is amazing.


What this project is aiming for is "the world's most energy-efficient and high-performance Physical AI product." Power and intelligence matter, and in addition it requires spirit.


**Komori:** Co-design does not simply mean multiple companies designing something together. I think it means people with different expertise, cultures, and languages coming together under the same mission to update the very culture of design itself.


**Osman:** And beyond that, there is one more thing I am looking forward to. The HGR team has been built as an AI-native team. When a team works with AI from the very beginning, it is not bound by old ways of doing things, and it can grow at a different speed. In the next phase, we will bring AI-based tools and flows into their team and raise productivity even further. I believe that will be the next stage of this collaboration.


**Osman:** At first, we did not know whether it could be done. But we kept asking the question without giving up, and we kept trying different solutions. If you continue doing that, a path will always open. I think that persistence and determination are the strength of this team.


**Shiwa:** Never Give Up. That is our shared motto.


**Komori:** In the next phase, what we are aiming for is to bring the first silicon with high performance and high power efficiency into the world. Starting from there, we will build an ecosystem that includes the software needed for the final product. AI chip the world has never seen—we will make it real from here.


**Learn more about[Cadence Physical AI,](https://www.cadence.com/en_US/home/ai/design-for-ai/physical-ai.html) and read the other posts in the series:**


- **[Honda + Cadence = Physical AI (part 1): What Does “Physical AI” Really Mean?](https://community.cadence.com/cadence_blogs_8/b/corporate-news/posts/honda-cadence-physical-ai-part-1-what-does-physical-ai-really-mean)**
- **[Honda + Cadence = Physical AI (part 2): Where Physical AI Will Be Won](https://community.cadence.com/cadence_blogs_8/b/corporate-news/posts/honda-cadence-physical-ai-part-3-creating-a-chip-the-world-has-never-seen)**
