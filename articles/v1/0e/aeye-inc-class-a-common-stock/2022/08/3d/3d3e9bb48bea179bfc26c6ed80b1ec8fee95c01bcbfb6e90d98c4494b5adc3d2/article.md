---
schema_version: "1.0.0"
document_id: "3d3e9bb48bea179bfc26c6ed80b1ec8fee95c01bcbfb6e90d98c4494b5adc3d2"
company_key: "aeye-inc-class-a-common-stock"
company: "AEye Inc."
source_id: "aeye-inc-class-a-common-stock-rss-1927df7d28ee"
canonical_url: "https://www.aeye.ai/blog/car-mbs-the-leap-from-adas-to-autonomy/"
published_at: "2022-08-11T22:51:51+00:00"
first_seen_at: "2026-07-20T23:23:00.433930+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:2cb2a5b6f1f560ddeaad1fc838f2d3e1c1e70d4d3eb38978bf10443ea00596db"
---

# CAR MBS: The Leap from ADAS to Autonomy

-
-
-
-
-


Consumers are acclimating to ADAS safety features, as applications that used to be premium add-ons (think: Adaptive Cruise Control and Lane Centering/Lane Keeping) are becoming standard issue on mainstream vehicle models. But the transition from ADAS to autonomy is a complex one, requiring cars to acquire human-like intelligence to safely navigate roadways with little to no human intervention.


Last week’s CAR MBS panel, *Tapping Intelligence to Leap from ADAS to Autonomy* , tackled the competencies and roadblocks required to move from L2 to L4, with panelists from AEye, Toyota, Honda Research Institute, Amazon Web Services and AAA agreeing that the transition is more than a process evolution: safely deploying higher level autonomy is a step-change that requires a fresh look at architectures and education.


#### **An Architectural Shift**


While current architectures, which largely leverage camera and radar sensors, suffice for basic ADAS features, their shortcomings become clear as automakers add more nuanced and advanced safety features. AAA’s Director Automotive Engineering and Industry Relations[Greg Brannon](https://www.cargroup.org/person/greg-brannon/) offered a case in point with Lane Centering, noting the[safety issues](https://www.forbes.com/sites/tanyamohn/2021/07/22/driving-on-curves-using-automated-systems-can-pose-safety-challenges/?sh=2b562c7f1df4) that have arisen due to what he believes is heavy reliance on cameras and unreliable lane markings.


According to AEye’s Director of Product Management, Automotive, Indu Vijayan, as auto makers look to implement higher levels of autonomy, they must both expand their sensor suite, and integrate sensors with built-in intelligence to ensure redundancy and safety. Vijayan described AEye’s lidar, a situationally aware sensor that captures better information using less data, as providing the kind of edge intelligence needed to get to perception faster (to be within safe reaction times, where use cases with time-to-collision are lower.)


*“The same sensor capability should now be able to get more information from the scene and be able to send that information to the higher perception and path plan system. Having that kind of feedback loop, with understanding what’s happening in the situation, enables the sensor to give the most relevant data, without overloading the system with too much data, so that decisions can be made much faster, since accidents happen in seconds,” says Vijayan.*


Honda Research Institute (HRI)’s Lead Scientist,[Ehsan Moradi Pari](https://www.cargroup.org/person/ehsan-moradi-pari/) , spoke on the importance of edge intelligence, and training AI to handle a diversity of situations in order to minimize accidents: “ *If you look at the roadways and the city urban areas, you see scooters, you see e-bikes, and other mobility solutions that all have limited computation capabilities. It’s important to understand how edge computing can help minimize fatalities and crashes.”*


In addition, edge computing helps alleviate what AWS’ Global Head of Business Development and GTM for ADAS, Vijitha Chekuri, calls the “big data and compute problem”: Autonomous vehicles will generate as much as 40 terabytes of data an hour from cameras, radar, and other sensors—equivalent to an iPhone’s use over 3,000 years—and suck in massive amounts more to navigate roads, according to Morgan Stanley.


Artificial intelligence, data analytics, and edge computing are more important than ever to tame this data torrent. But what does this mean for development?


#### **Two Teams – Two Focuses**


Once seen as a building block to L4 and L5 autonomy, the panelists largely see ADAS as a separate but complementary endeavor, with disparate use cases, requiring different teams and architectures.


According to Toyota’s Vice President Integrated Vehicle Systems[Nick Sitarski](https://www.cargroup.org/person/nick-sitarski/) , *“Level 2 and Level 4 .. use a lot of the same sensors, but the biggest difference is that on a Level 2 system, the most capable sensor in a system is the human… In a Level 4 system, the initial complexity is really trying to recreate what the human is doing, and that’s just not easy.”*


At L4 and L5 autonomy, the vehicle takes over human responsibility while the Automated Driving System (ADS) is activated. At this stage, panelists agreed, the perception system has to solve the last 1% of corner cases – the toughest ones. This will require both a leap in architecture, and collaboration between ADAS and AV development teams.


AWS’ Chekuri explains, *“The last 5 or last 1% is the hardest to get to. So, when we look at the teams or customers working there, it’s very focused at L4 and L5, and ADAS is also an extremely focused effort, and it’s here and now…They are going to collaborate on the technology and they need to do some best practices and patterns that fit for ADAS, but (L4 teams) are focusing their efforts on solving that last 1%.”*


#### **The Software-Focused Future**


As cars evolve, at all levels, to become software-driven like our phones, the development process becomes even more critical. Consumers are already asking for Over the Air (OTA) upgrades, but, when updates and services happen continuously via software, how does that change the back end? AWS and AEye talked about the need to separate hardware and software in order to move fast, as well as the need to validate ahead.


According to AWS’ Chekuri, *“Not waiting until the hardware is ready to validate, doing the software in the loop validation ahead, and making those models available in the software to be as accurate as possible compared to your hardware…This area is going to get a lot of momentum going forward because it will take the costs out and speed up development.”*


Panelists agreed, while the importance of robust hardware remains, the focus on software will be a game-changer, with the need to validate earlier before services are deployed in the market.


#### **What about L3?**


In short, it’s complicated. While engineers like to do things in sequence, Level 3 requires a mix of technology proficiency, driver awareness and OEM culpability that most OEMs aren’t comfortable with. According to AAA’s Brannon, *“Most people would be writing on the road, reading a book or checking their email, and then re-engage in an emergency situation..It’s very challenging and there’s a lot of liability that comes with it. I don’t think anybody wants to be first.* *”*


And that’s where the development divide comes in. Many feel that automakers will push the envelope on Level 2 solutions, achieving L2+++ capabilities, and largely skip over Level 3 – instead jumping to L4 and L5 autonomous solutions, where the driver is not expected to engage.


#### **Education. Education. Education.**


With all this talk, and confusion, over levels of autonomy, panelists concurred on the importance of communication and transparency with consumers, saying it’s going to take “a village” to continue to educate people about these technologies.


According to Sitarski, “ *Right now in the mass market there are no autonomous vehicles… There’s lot of vehicles with assist systems and I think it’s critical that we’re transparent and honest with customers about what those systems are and what they’re capable of and what they’re not capable of.”*


Moradi Pari echoed *“That’s why we see ADAS as a bridge to learn, improve and evolve toward automated driving.”*
