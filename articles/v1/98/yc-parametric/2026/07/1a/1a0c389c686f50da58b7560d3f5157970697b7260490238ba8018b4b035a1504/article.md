---
schema_version: "1.0.0"
document_id: "1a0c389c686f50da58b7560d3f5157970697b7260490238ba8018b4b035a1504"
company_key: "yc-parametric"
company: "Parametric"
source_id: "yc-parametric-news-import-eb04a40d8d59"
canonical_url: "https://parametric.company/blog/evaluation-precedes-capabilities"
published_at: null
first_seen_at: "2026-07-24T00:39:21.525691+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:babd0b621f990d2768fd9a3acfb34740ac08146819b8466240e8edd21eae05cf"
---

# Evaluation Precedes Capabilities

Over the next few years, the number of general-purpose robots in the wild will grow from hundreds to thousands, and eventually to millions.


Advancements in robotics are held back by multidisciplinary research questions, in which hardware, data, and modeling are tightly coupled. Changing one constrains the others, so answering these questions today requires a surgical approach: we change one variable, then re-evaluate the capabilities of the entire system.


We currently spend a lot of effort hand-crafting these surgical edits, but as the industry matures, we’ll get better at leveraging compute to automate these changes — shifting our effort toward defining the outcomes we want rather than the mechanisms. We’ll use evals to measure the gap between the outcomes we get and the outcomes we want.


As robots grow more capable, they’ll share many of our spaces, and we’ll want to personalize them to varying degrees. Evaluation will measure not just what robots can do, but how they do it, against human preference. Ultimately we’ll use measurement to steer these systems to align with our best interests.


## So where are the robots?


In San Francisco it’s not difficult to find people that believe we’ll have ASI next year


†


. Yet not a single robot today can autonomously .


It’s often echoed that a lack of data volume


explains the gaps between our chatbots and robots. It’s true that we don’t have an internet-scale corpus of robot data, but this flattens the picture. More precisely, today’s frontier robotics labs are racing to scale the *correct* type of data. This is an open research question that spans a tight coupling between hardware, data, and modeling.


For example, a decision to use three-finger grippers constrains the data that’s collected off your hardware, which in turn constrains your model’s capabilities. Every lab is actively debating the trade-offs of decisions that cut across domains, like the following:


- Should we be scaling video data, egocentric data, , or teleoperation? In what mixture?
- Should our robot have 2, 3, or 5 fingers? Can we collect UMI data for 5-fingered hands? Can we teleoperate them?
- Do we need motor torque in the training recipe, or can we use motor positioning and translate position to torque mechanically (e.g. with )?
-


Furthermore, robotics companies are racing each other in fierce competition. The speed at which a company answers these questions — and scales the *correct* types of data — determines its speed of growth and whether it lives or dies. Each question poses a bet on what to try next, and each bet must be validated against what the robot can do. We can think of this race for data as a search and evaluation problem.


## Accelerating Search


The Bitter Lesson


succinctly describes progress in the field of artificial intelligence: bet against scalable learned algorithms and you’ll eventually lose. In robotics this lesson extends to search itself. We’re becoming increasingly better at employing scalable learned methods to explore research questions, steered by the outcomes we want.


This progression may be understood as a gradual shift of complex technical work from humans to machines. Symbolic AI


, among the earliest “attempts” of AI, involved humans defining large collections of rules. Supervised learning offloaded the task of hand-writing these rules to a learned algorithm, but required humans to hand-label data meticulously. Deep reinforcement learning shifted human effort from labeling examples to defining the reward functions and environment, whether through human preference () or hand-specified verifiers (). Today, agents are climbing benchmarks written in natural language, e.g. autoresearch


, but the trajectory points further: state a goal, and let the agent experiment on training recipes, reward functions, and environments until it reaches that goal.


Foundational robotics models lag behind contemporary language models. But as frontier labs close in on automated researchers, and the robotics industry builds out more environments for verifiable reward, more of the search falls to autonomous systems. Eventually what’s left is the task of defining the outcomes we want, and the evaluation criteria via which we specify those.


## Where does this take us?


As robotics research advances we’ll see an accelerated progression of capabilities. Take the humble laundromat:


- Today, robots in a laundromat can fold a t-shirt, medium towel, or shorts.
- Soon that will expand to all clothing, even fitted sheets.
- Next, we’ll see learned systems loading and unloading machines, alongside handling the folding.
- This will evolve to a point where a robot handles dirty laundry on one end, and produces clean folded laundry on the other.


In a few years, robots will run our laundromats, clean our homes, cook our food, and handle many other aspects of physical labor for us. But capabilities won’t plateau.


Instead, we’ll continually re-evaluate the frontier of expectations we have for these systems. We’ll desire longer time horizons on task execution, and faster performance. We’ll also expect personalities, and behaviors matched to our human preferences. We’ll want capabilities today that are difficult to imagine, but become apparent as robots become larger parts of our lives.


Constant in all of this will be a gap between what we desire from our robots, and what we have. It’s quite possible this gap never fully saturates, and instead becomes an ever-moving target. Evaluation must adapt to continually measure this gap, and ultimately it will be via this measurement that we steer these systems towards what we want.
