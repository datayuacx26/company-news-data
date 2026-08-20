---
schema_version: "1.0.0"
document_id: "48fe0e7df13edcceecf98eeb0f77d1abe90d1f34e5a3a16abd5e406f217bdfd7"
company_key: "yc-polymath-robotics"
company: "Polymath Robotics"
source_id: "yc-polymath-robotics-news-import-0eb697031937"
canonical_url: "https://www.polymathrobotics.com/blog/2-new-modules-geofence-and-ackermann-configuration"
published_at: null
first_seen_at: "2026-07-24T03:33:00.269851+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:d2ee758d1f5af858f6a88de5aae000963840d7f4c87adfdb01832e871c53635d"
---

# Polymath Robotics Blog | 2 New Modules: Geofence and Ackermann Configuration

###### *At Polymath Robotics, we make it radically simple to automate off-highway vehicles. We’re releasing 40 autonomy modules over 20 weeks to make it easier than ever to build your autonomous navigation stack. Each module can be used on its own, or together as a full autonomy solution. This week we’re launching two new modules: Geofence and Ackermann Configuration.*


‍


##### It’s too hard to build autonomous vehicles


**That’s why we built Polymath Robotics –** the autonomous navigation toolkit for off-highway vehicles.


Our goal is clear: to streamline the process of automating off-highway vehicles. On average, Polymath reduces the timeline of off-highway autonomy projects by more than a year, akin to 5-10 engineers. With Polymath, you can build off-highway robots without getting overwhelmed by the complexities of robotics.


In our effort to simplify automating off-highway vehicles even further, we're unveiling 40 Autonomous Navigation Modules within the next 20 weeks.


Last week we[released another two modules](http://www.polymathrobotics.com/blog/meet-2-new-modules-robot-metrics-infrastructure-drive-by-wire-system-abstraction) : Drive-by-Wire System Abstraction and Robot Metrics Infrastructure.


***Today we’re excited to launch our next 2 modules: Geofence and Ackermann Configuration.***


‍


##### **Module #7:** Geofence


**The problem:** Most off-highway vehicles have dedicated spaces for operation – like a field, a construction site, or a mine. Their operations need to be contained to those sites. You don’t, for example, want those vehicles to think a nearby road or pedestrian walkway is a viable path.


**Introducing our Geofence module –** easily set boundaries for your autonomous off-highway vehicle’s operations.


**How it works:** The Geofence module simplifies the process of creating safe operation boundaries by allowing automatic setup using GPS coordinates. When a vehicle nears the Geofence, it gradually reduces slows and eventually halts, preventing it from crossing the defined boundary.


‍


*What makes Geofence a core part of the autonomous navigation stack?*


- **Safer operations:** Setting an operational boundary ensures that your vehicle will never be where it’s not supposed to be.
- **Versatility:** The module can receive any set of GPS coordinates and turn them into a polygon boundary.
- **Intelligent behavior:** The vehicle won’t stop suddenly at the Geofence, but rather will slow down gradually and come to a safe stop.


‍


Our Geofence module can receive any set of GPS waypoints and create a polygon boundary for safe operation. This image shows a coverage mapper generating a path for the vehicle within a specified Geofence.


‍


##### **Module #8:** Ackermann Configuration


**Why we built this:** Ackermann steering is common in large off-highway vehicles like tractors, wheel loaders, and dump trucks. Our very first vehicle, a small demo tractor, was Ackermann steering.


**The problem:** The standard Ackermann package in ROS assumes that, like a car, all the wheels are the same size. But many off-highway vehicles introduce a complexity: bigger back wheels! This means you need to rethink the kinematics (or motion profile) of these vehicles and reprogram your controls systems around them.


**That’s why we built the Ackermann Configuration module –** the easier way to tailor your controls & path planning for Ackermann kinematics with different wheel sizes.


**How it works:** We’ve built on top of the standard Ackermann template in ROS, so you can plug in your vehicle’s turning radius and wheel sizes and the system will automatically calculate its unique motion profile.


What makes our Ackermann Configuration so powerful?


- **Skip the kinematic headache:** Polymath’s Ackermann Configuration makes it faster and easier to configure your system around your vehicle’s unique kinematics.
- **Robust and flexible:** the module can be easily applied to Ackermann vehicles of any dimension.
- **Easy to incorporate:** like all our modules, you can use Ackermann Configuration on its own, or as part of our full autonomous navigation system.


The clip below shows our demo tractor using our Ackermann Configuration for accurate steering, path planning, and whole vehicle control.


‍


Our demo tractor using our Ackermann Configuration module for accurate steering, path planning, and whole vehicle control.


‍


##### What’s Next


Next week, we’ll reveal two more of our 40 Autonomous Navigation Modules. As always, each module easily fits into your existing projects or combines with other Polymath modules to create a complete autonomous navigation system.


We’re excited to hear how you plan to use these modules to speed up your off-highway autonomy projects.[Contact Sales](https://www.polymathrobotics.com/contact) today.


‍
