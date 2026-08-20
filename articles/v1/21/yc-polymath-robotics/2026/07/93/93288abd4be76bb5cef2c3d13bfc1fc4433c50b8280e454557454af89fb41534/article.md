---
schema_version: "1.0.0"
document_id: "93288abd4be76bb5cef2c3d13bfc1fc4433c50b8280e454557454af89fb41534"
company_key: "yc-polymath-robotics"
company: "Polymath Robotics"
source_id: "yc-polymath-robotics-news-import-0eb697031937"
canonical_url: "https://www.polymathrobotics.com/blog/meet-2-new-modules-remote-e-stop-diff-drive-configuration"
published_at: null
first_seen_at: "2026-07-24T03:33:00.269851+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:dbb8f99e105578b2213ee0f13423fd0708690c7899377c6989ca529ed2708e93"
---

# Polymath Robotics Blog | Meet 2 New Modules: Remote E-Stop & Diff Drive Configuration

###### *At Polymath Robotics, we make it radically simple to automate off-highway vehicles. We’re releasing 40 autonomous navigation modules over 20 weeks, making it easier than ever to build your off-highway autonomy stack. Each module can be used on its own, or together as a full autonomy solution. This week we’re launching two new modules: Remote Emergency Stop and Diff Drive Configuration.*


‍


##### It’s too hard to build autonomous vehicles


**That’s why we built Polymath Robotics –** the autonomous navigation toolkit for off-highway vehicles.


Our aim is simple: to streamline the process of automating off-highway vehicles. On average, Polymath reduces the timeline of off-highway autonomy projects by more than a year, or up to 5-10 engineers' worth of effort. With Polymath, you can build off-highway robots without getting mired in complexity.


In our effort to simplify automating off-highway vehicles even further, we're unveiling 40 Autonomous Navigation Modules within the next 20 weeks.


Last week we[released another two modules](http://www.polymathrobotics.com/blog/meet-2-new-modules-robot-metrics-infrastructure-drive-by-wire-system-abstraction) : Geofence and Ackermann Configuration.


***Today we’re excited to launch our next 2 modules: Remote Emergency Stop ("E-Stop")and Diff Drive Configuration.***


‍


##### Module #9: Remote E-Stop


**Why we built this:** while our system has built-in safety onboard, sometimes you need the vehicle to come to a stop for safety reasons.


**Introducing our Remote E-Stop module –** bring the vehicle to a safe stop, from anywhere in the world.


**How it works:** The E-Stop constantly listens for the heartbeat of the vehicle. If an E-Stop button is triggered, the vehicle will be commanded to come to an immediate stop.


‍


*What makes Remote E-Stop so powerful?*


- **Safer operations:** Safety is at the core of everything we do. The ability to stop the vehicle remotely anytime, anywhere helps ensure safer operations in dynamic industrial environments.
- **Versatility:** Our E-Stops work in teleoperation mode or full autonomy, so you always have full control over your vehicle’s safe operations.
- **Default to a human:** Once the vehicle is brought to a stop, it requires human oversight to start moving again - ensuring the vehicle doesn’t resume motion in unsafe situations.


‍


Our Remote E-Stop module can be configured to any physical e-stop switch and work with your existing autonomy stack.


##### Module #10: Diff Drive Configuration


**Why we built this:** Differential Drive steering, where the vehicle stays in place as it turns, is common in off-highway vehicles like bulldozers and excavators.


**The problem:** Bigger off-highway equipment with differential drive typically uses treads instead of wheels and has a different steering mechanism than smaller diff drive robots. These larger machines usually lack a zero-turn radius, making the standard differential drive packages in ROS inadequate for accurately representing their motion profiles.


**That’s why we built the Diff Drive Configuration module –** the easier way to tailor your controls & path planning for diff drive kinematics with differing turn radiuses.


**How it works:** We’ve built on top of the standard diff drive template in ROS, so you can plug in your vehicle’s turning radius and wheel (or track) sizes and the system will automatically configure itself to that unique motion profile.


What makes our Diff Drive Configuration so powerful?


- **Skip the kinematic headache:** Polymath’s Diff Drive Configuration makes it faster and easier to configure your system around your vehicle’s unique kinematics.
- **Robust and flexible:** the module can be easily applied to Diff Drive vehicles of any dimension.
- **Easy to incorporate:** like all our modules, you can use Diff Drive Configuration on its own, or as part of our full autonomous navigation system.


The clip below shows an autonomous bulldozer with track treads uses our Diff Drive Configuration for accurate steering, path planning, and whole vehicle control.


‍


##### **What’s Next**


Next week, we’ll reveal two more of our 40 Autonomous Navigation Modules. As always, each module easily fits into your existing projects or combines with other Polymath modules to create a complete autonomous navigation system.


We’re excited to hear how you plan to use these modules to speed up your off-highway autonomy projects.[Contact Sales](https://www.polymathrobotics.com/contact) today.
