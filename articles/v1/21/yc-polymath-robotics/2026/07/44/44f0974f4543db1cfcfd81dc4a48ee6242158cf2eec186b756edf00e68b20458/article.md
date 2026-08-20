---
schema_version: "1.0.0"
document_id: "44f0974f4543db1cfcfd81dc4a48ee6242158cf2eec186b756edf00e68b20458"
company_key: "yc-polymath-robotics"
company: "Polymath Robotics"
source_id: "yc-polymath-robotics-news-import-0eb697031937"
canonical_url: "https://www.polymathrobotics.com/blog/meet-2-new-modules-follow-the-leader-and-sim-to-reality-pipeline"
published_at: null
first_seen_at: "2026-07-24T03:33:00.269851+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:55339927e30af7ae43dc24eafd556c87f3bf56c58dac19ef8318dbab8937835b"
---

# Polymath Robotics Blog | Meet 2 New Modules: Follow the Leader and Sim-to-Reality Pipeline

###### *At Polymath Robotics, we're dedicated to streamlining off-highway vehicle automation. We’re rolling out 40 autonomous navigation modules across 20 weeks to simplify your autonomy stack. You can use each module independently or as a unified autonomy solution. This week, we're excited to unveil two new modules in our lineup: Follow the Leader and Sim-to-Reality Pipeline.*


‍


##### It’s too hard to build autonomous vehicles


**That’s why we built Polymath Robotics –** the autonomous navigation toolkit for off-highway vehicles.


Our aim is clear: to simplify off-highway vehicle automation. Typically, Polymath slashes off-highway autonomy project timelines by over a year (the equivalent effort of 5-10 engineers). With Polymath, building off-highway robots becomes hassle-free, avoiding unnecessary complexities.


In our effort to simplify automating off-highway vehicles even further, we're unveiling 40 Autonomous Navigation Modules within the next 20 weeks. So far we’ve released 10 different modules![Last time](https://www.polymathrobotics.com/blog/meet-2-new-modules-obstacle-detection-local-planning) we debuted Obstacle Detection and Local Planning.


***Today we’re excited to launch our next 2 modules: Follow the Leader and Sim-to-Reality Pipeline***


‍


##### Module #13: Follow the Leader


**Why we built this:** Some vehicles need to “follow” others as part of their daily work. For example, in agriculture, there is often a vehicle which “follows” a harvester to catch the harvested plants.


**Introducing our Follow the Leader module –** a simple, tuneable framework for an autonomous vehicle to follow another moving vehicle (manned or unmanned).


**How it works:** Follow the Leader converts the ‘leader’ vehicle’s GPS output into a series of waypoints for the ‘follower’ vehicle to follow. The follower vehicle converts that information into a smooth path, using onboard Obstacle Detection and Local Planning to maneuver its environment intelligently. Unlike other systems, the follower vehicle requires only the location of the lead vehicle to intelligently follow its path.


*What makes our Follow the Leader module so powerful?*


- **Vehicle orchestration:** Easily get your vehicle to follow another at whatever offset you specify. You can adjust the offset or speed based on the lead vehicle’s activity.
- **More work for less:** Conduct multiple vehicles to do work “in formation” - allowing more work to get done in less time.
- **Ready out of the box:** Follow the Leader is easy to install as an individual module on your existing autonomy stack, or as part of our full autonomy solution.


‍


This vehicle uses our Follow the Leader module to "follow" a manned lead vehicle (you can see the lead vehicle as a pink and blue dot under the vehicle icon). In this case the follower vehicle actually drives slightly "ahead" of the lead vehicle.


‍


##### Module #14: Sim-to-Reality Pipeline


**The problem:** Most simulation engines used in robotics projects lack accuracy in replicating a vehicle's physics and kinematics. Despite looking visually impressive, these simulations fail to predict how the vehicle will truly behave. Consequently, the simulated behavior doesn't match the real vehicle, leading to extra work to bridge the gap.


**That’s why we built the Sim-to-Reality Pipeline module –** the easy way to develop in simulation using the same system intended for the real vehicle.


**How it works:** We've prioritized making the vehicle move and behave correctly rather than focusing on fancy visuals. This system lets you test commands, like setting waypoints using our API, in the simulation. Then, you can install the exact same system on the real vehicle, making development faster and simpler than before.


What makes our Sim-to-Reality Pipeline module so powerful?


- **Develop for the real vehicle in simulation:** Sim-to-Reality Pipeline lets you jumpstart development even if the physical vehicle isn’t ready.
- **Integrate with our system before the field:** you can program customer behaviors using our API and get a sense for how your real vehicle will respond, even before the physical vehicle is available for testing.
- **Easy to incorporate:** like all our modules, you can use Sim-to-Reality Pipeline on its own, or as part of our full autonomous navigation system.


‍


^^ The integration above shows how we built an integration with Samsara in simulation first, but then ported the same system over to our real-life test vehicle


‍


##### **What’s Next**


In the first week of 2024, we’ll reveal two more of our 40 Autonomous Navigation Modules. As always, each module easily fits into your existing projects or combines with other Polymath modules to create a complete autonomous navigation system.


We’re excited to hear how you plan to use these modules to speed up your off-highway autonomy projects.[Contact Sales](https://www.polymathrobotics.com/contact) today.


‍
