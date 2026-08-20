---
schema_version: "1.0.0"
document_id: "cd1d8980d5b5a81b7401ac9e2e58c49b3d6de953084fc8ccf6573da261977f1d"
company_key: "yc-polymath-robotics"
company: "Polymath Robotics"
source_id: "yc-polymath-robotics-news-import-0eb697031937"
canonical_url: "https://www.polymathrobotics.com/blog/meet-2-new-modules-obstacle-detection-local-planning"
published_at: null
first_seen_at: "2026-07-24T03:33:00.269851+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:8ba5751a476581927bd5ae4b45ca92d42a062641f11f64a19f8e3914bade7460"
---

# Polymath Robotics Blog | Meet 2 New Modules: Obstacle Detection + Local Planning

###### *At Polymath Robotics, we make it radically simple to automate off-highway vehicles. We’re releasing 40 autonomous navigation modules over 20 weeks, making it easier than ever to build your off-highway autonomy stack. Each module can be used on its own, or together as a full autonomy solution. This week we’re launching two new modules: Obstacle Detection and Local Planning.*


‍


##### It’s too hard to build autonomous vehicles


**That’s why we built Polymath Robotics –** the autonomous navigation toolkit for off-highway vehicles.


Our aim is simple: to streamline the process of automating off-highway vehicles. On average, Polymath reduces the timeline of off-highway autonomy projects by more than a year, or up to 5-10 engineers' worth of effort. With Polymath, you can build off-highway robots without getting mired in complexity.


In our effort to simplify automating off-highway vehicles even further, we're unveiling 40 Autonomous Navigation Modules within the next 20 weeks. So far we’ve released 10 different modules![Last week](https://www.polymathrobotics.com/blog/meet-2-new-modules-remote-e-stop-diff-drive-configuration) we debuted Remote Emergency Stop ("E-Stop")and Diff Drive Configuration.


***Today we’re excited to launch our next 2 modules: Obstacle Detection and Local Planning.***


‍


##### Module #11: Obstacle Detection


**Why we built this:** Humans naturally recognize obstacles in their path, but robots need a clear system to distinguish between objects that are "safe" or "unsafe" for interaction.


**Introducing our Obstacle Detection module –** a simple, tuneable framework for your vehicle to identify obstacles nearby.


**How it works:** Obstacle Detection takes raw point clouds and clusters them to identify an “obstacle.” The machine then defines obstacles based on those clusters’ height, distance, and movement. For instance, it won't try to drive over anything taller than one-fourth the height of its wheels. We can adjust these settings for different vehicles and environments. Additionally, we can use machine learning to classify obstacles more accurately when needed.


*What makes our Obstacle Detection module so powerful?*


- **Safer operations:** Help your vehicle chart a safer path by detecting obstacles in its way.
- **Flexibility:** We can easily tweak the Obstacle Detection parameters, allowing you to change the machine's operational environment without worrying about reprogramming the entire algorithm.
- **Ready out of the box:** Obstacle Detection is easy to install as an individual module on your existing autonomy stack, or as part of our full autonomy solution.


‍


Our Obstacle Detection module takes raw point clouds from the vehicle's sensors and clusters them to identify an "obstacle" *(in this case, a trash can seen in the camera view on the top left)*


‍


##### Module #12: Local Planning


**Why we built this:** Robots use both global and local planning. Global planning is akin to using Google Maps for directions, while local planning is similar to seeing a bunch of cones in a parking lot and charting a path around them.


**The problem:** Local planning involves making quick decisions in unpredictable settings and can be difficult to develop without consuming excessive computational resources on the machine.


**That’s why we built the Local Planning module –** the easy way to transform raw point cloud data into a unified cost map for efficient, safe robot navigation.


**How it works:** After obstacles are detected, the Local Planning module maps out the optimal path to the destination. It does this by making a "cost map" where obstacles are seen as having a higher cost. Then, it calculates the most direct route that avoids these costly obstacles. Additionally, Local Planning allows you to assign higher costs to specific obstacles like humans, ensuring that the machine maintains a greater distance from them.


What makes our Local Planning module so powerful?


- **Mapless navigation:** Local Planning does not require pre-mapping of its environment for the machine to navigate safely from A to B.
- **No internet connection needed:** Local Planning can find the best path using the computing power onboard the machine, right in its surroundings, without needing an internet connection.
- **Easy to incorporate:** like all our modules, you can use Local Planning on its own, or as part of our full autonomous navigation system.


‍


##### **What’s Next**


Next week, we’ll reveal two more of our 40 Autonomous Navigation Modules. As always, each module easily fits into your existing projects or combines with other Polymath modules to create a complete autonomous navigation system.


We’re excited to hear how you plan to use these modules to speed up your off-highway autonomy projects.[Contact Sales](https://www.polymathrobotics.com/contact) today.


‍
