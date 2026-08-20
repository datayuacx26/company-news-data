---
schema_version: "1.0.0"
document_id: "392ce8fb40b150a9191ad1fac660302f50eac99e37745a756aea1c5551d7b81a"
company_key: "synaptics-incorporated-common-stock-0-001-par-value"
company: "Synaptics Incorporated Common Stock $0.001 Par Value"
source_id: "synaptics-incorporated-common-stock-0-001-par-value-rss-c69120407f43"
canonical_url: "https://www.synaptics.com/company/blog/the-missing-sense-why-physical-ai-can-see-everything-and-feel-nothing"
published_at: "2026-07-09T20:27:21+00:00"
first_seen_at: "2026-07-29T11:28:26.992739+00:00"
fetched_at: "2026-07-29T11:28:28.703298+00:00"
content_hash: "sha256:271021db40428a8a04295cb60d15c2c3d073a87969b945fc559203b1c9985097"
---

# The Missing Sense: Why Physical AI Can See Everything and Feel Nothing

[Home](https://www.synaptics.com/) /[Company](https://www.synaptics.com/company/overview) /[Blog](https://www.synaptics.com/company/blog) / The Missing Sense: Why Physical AI Can See Everything and Feel Nothing


# The Missing Sense: Why Physical AI Can See Everything and Feel Nothing


July 09, 2026[Satish Ganesan SVP & GM Edge Interface & Sensing Division, CSO](https://www.synaptics.com/company/blog?author=Satish%20Ganesan) Share this article


*Part 1 of a series on Physical AI, tactile sensing, and the road to dexterous machines.*


Reach into your pocket and find your keys without looking.


You just did something today’s robots still struggle to do reliably. Your eyes were somewhere else entirely, yet your fingers sorted coins from keys from lint, registered their weight and the cut edges, and closed around the right object with exactly enough force to lift it — not so little that it slipped, not so much that it bent. No camera was involved. That was touch, quietly doing the kind of work that sight simply cannot.


For the better part of a decade, the visible progress in AI has been a story about perception. Machines became far better at seeing, recognizing, and planning. Vision models can identify objects in cluttered scenes; large language models can take vague instructions like "clean up the kitchen" and break it into a plausible sequence of steps. Robotic systems can now combine perception, language, and planning in ways that would have seemed like science fiction only a few years ago.


And yet, ask a robot to pick up a paper cup, a grape, or a crumpled receipt, and the illusion breaks. It hesitates, then crushes the cup or drops the grape. The brain got smart far faster than the hands did.


### From perception to interaction


This is the pivot now underway in robotics. The field is shifting from perception — understanding the world — to interaction: physically changing it. Seeing the world is no longer the only hard part. Touching and manipulating it reliably, gently, and quickly is.


It helps to contrast two forms of autonomy. A self-driving car spends much of its effort avoiding contact; touch usually means something has gone wrong. A robotic hand has the opposite job. Its purpose is to make controlled contact — to grasp, hold, and manipulate — and to understand how the object pushes back. Vision can tell the system where an object is. Touch reveals how an object responds the moment the fingers close around it.


That "how" is the missing sense.


### Vision plans. Touch executes.


A phrase often used to crystallize this shift is that vision is for planning, while touch is for execution.


The distinction is more than a tidy slogan; it points at a physical fact about where each modality reaches its limits. A camera can survey a room, locate a mug, and decide on an approach. But the moment a gripper actually reaches the mug, vision reaches predictable limits. The fingers occlude the contact point the system needs to observe— the hand creates its own blind spot exactly where feedback matters most. The contact patch, where grip is won or lost, remains hidden from the lens. A camera also cannot see friction: it cannot tell whether the surface of that plastic cup is greasy, or how the load will shift the instant the liquid inside it moves. Sub-millimeter errors in grip or object placement that appear fine on camera can still result in a mug ending up on the floor. Touch provides the feedback vision cannot.


There is a counterintuitive payoff here. We tend to assume that giving a robot a sense of touch would make it more cautious — more careful, and therefore slower. The opposite is true. Consider the human benchmark: a person can feel an object begin to slip and correct their grip in a fraction of a second, before conscious thought catches up, without ever looking. A robot that can feel its grip does not have to creep through a task, stopping to check and re-check with vision at every step. It can move with greater confidence, adapting in real time as contact conditions change. Research in force-aware manipulation continues to show that adding force and touch to a manipulation system can improve both speed and accuracy. Touch does not slow robots down. It is what allows them to move faster with control.


### What touch actually has to deliver


So what does a sense of touch need to provide to be useful? At a practical level, it comes down to four things:


- **Contact confirmation:** Is the gripper actually touching the object — not a millimeter away, not closing on empty space?
- **Force:** How much force is it applying, and is that the right amount for the object being handled, such as a steel bracket or an egg?
- **Slip:** Is the object beginning to slip before the grasp fails and the object falls?
- **Ground truth:** Rich, labeled physical data on what real contact feels like, the kind of data AI models need to learn physical manipulation.


When those four elements work together, the downstream benefits become concrete: higher grasp success rates, fewer damaged objects from over-gripping or failed grasps, and a stream of high-quality training data that helps improve the next generation of manipulation models.


### Why this is suddenly urgent


The idea is not new. What has changed is the urgency. Humanoid robots are beginning to move from limited deployments toward forecasts that point to much larger volumes within the next several years, before even considering the millions of collaborative and industrial robots already operating in factories and warehouses. Many of those machines run into the same constraint: for Physical AI systems, the bottleneck to deployment is not only intelligence or mobility. It is dexterity. It is the ability to grasp, hold, and manipulate reliably. It is the missing sense.


That raises a familiar sensing question, just at a different scale.


When you place a finger on a phone screen, the system has to decide quickly and reliably whether that's a real touch or a water droplet, a resting palm, or just electrical noise. It has to do this consistently across temperature swings, moisture, and interference, on billions of devices, without the user ever noticing that decision was made. That is, at its core, the same problem a tactile robotic fingertip faces: extract a clean, trustworthy signal from a noisy physical world, and act on it fast enough to matter.


Synaptics has spent decades solving that problem.


### What's ahead in this series


This is the first post in a series about the missing sense — what it takes to give machines a sense of touch, and why it is becoming one of the defining challenges in Physical AI.


The human body offers a useful model. Touch is not processed as a single isolated signal; it is part of a distributed sensory system that detects, filters, and responds to physical information close to the point of contact. In that sense, the body already demonstrates the kind of neuromorphic, distributed intelligence Physical AI systems increasingly need.


In the posts ahead, we will explore the physics of slip and why detecting it early is critical; the engineering craft behind sensors that can survive the real world; the intelligence that needs to live at the edge, close to the fingertips; the reliability challenges the field still needs to solve; and the market opportunity taking shape as machines learn to feel.


Vision gave robots a view of the world. Touch is what will allow them to reach into it with control.


*Next in the series: **The Ghost in the Grip** — the science of slip and why detecting grasp failure early is central to robotic dexterity.*


#### Satish Ganesan


Satish Ganesan joined Synaptics in November 2019 and serves as SVP & GM of Edge Interface & Sensing Division and Chief Strategy Officer. Prior to Synaptics, Satish served as Chief Product Officer of Keyssa, a wireless startup focused on short range connectivity. Before that, he held several executive positions at Broadcom where he led IoT and mobile strategy at different times. Prior to Broadcom, he held various senior product marketing positions at Tilera and Broadcom. He spent his early career in various engineering roles at Xilinx, and holds more than 10 US patents. He received his Bachelors degree in Electrical and Electronics Engineering from BITS, India and MS in Computer Engineering from University of Cincinnati.


[Read more by Satish Ganesan](https://www.synaptics.com/company/blog?author=Satish%20Ganesan)


[AI](https://www.synaptics.com/company/blog?category=AI)[Edge Computing](https://www.synaptics.com/company/blog?category=Edge%20Computing)
