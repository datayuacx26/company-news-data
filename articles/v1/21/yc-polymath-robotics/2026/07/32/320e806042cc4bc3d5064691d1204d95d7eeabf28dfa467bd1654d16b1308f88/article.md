---
schema_version: "1.0.0"
document_id: "320e806042cc4bc3d5064691d1204d95d7eeabf28dfa467bd1654d16b1308f88"
company_key: "yc-polymath-robotics"
company: "Polymath Robotics"
source_id: "yc-polymath-robotics-news-import-0eb697031937"
canonical_url: "https://www.polymathrobotics.com/blog/ros2-graph-monitor"
published_at: null
first_seen_at: "2026-07-24T03:33:00.269851+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:0c22a10edc6d551238ad403e8933ca72fbf637c94e641456771e3961f99e0eb1"
---

# Polymath Robotics Blog | ROS2 Graph Monitor: A Continuous Snapshot of Your Robot’s State

For years, ROS 2 users have struggled to get a clear and complete snapshot of their system state at a moment in time. Today, we're excited to announce our open-source work on[ROS2 Graph Monitor](https://github.com/ros-tooling/graph-monitor/tree/main/rosgraph_monitor) in conjunction with[a Foxglove extension](https://github.com/polymathrobotics/foxglove_extensions/tree/main/ros2-graph) to help solve that problem:


### The problem we're solving


ROS2 is made up of nodes that connect to each other through topics, services, and actions. When something goes wrong, being able to see the exact connections at the moment of failure is critical.


But the existing options to do this introspection have always been clunky. You might run a script on the robot to spit out a static image of the graph, but that's not something you can easily query later or pair with a bag recording to make the debugging process easier. And due to the distributed nature of a ROS 2 application, this is not easy to do instantaneously, it is better solved by a long-running observer of the system.


If a graph-related issue happened in the field two weeks ago, you'd be out of luck.


### **How ROS Graph Monitor came to be**


On the Polymath team, we've wanted the capability to assess the health of the ROS2 graph for a long time. Emerson Knapp released an initial version with his[ROSCon 2024](https://vimeo.com/1024971769) talk on the subject (side note: that was how the Polymath team first connected with him!). Shortly after, we brought that work into the Polymath stack.


While we ran the rosgraph monitor continuously, it wasn’t until our engineer Dhruv Tyagi decided to build a ROS graph visualizer in Foxglove Studio that things started to click: we could pair Dhruv’s visualizer with a publisher that continuously outputs the graph structure. That way, you could record the graph state alongside other telemetry and inspect it any time.


### **How it works**


The system we’re discussing is made of two independent components. One component, the graph monitor, observes and publishes the ROS graph - every node, topic, parameter and connection - at a set interval and whenever something changes. With this information available, you can use the Foxglove panel to explore it visually.


Now, if you have a bag file from that weird day in the field, you can jump back in time and see exactly what the system looked like. You can spot which topics were active, how often they were publishing, and whether something drifted from its normal state.


### **Why it matters**


Say your costmap generation has degraded. You suspect your LiDAR is publishing at a reduced rate. With ROS Graph Monitor, you can pull up a snapshot from before and during the problem, see the drop from 20Hz to a much lower rate, and confirm the basic root cause without trawling through individual messages.


It's almost like running a blood panel for your robot. You are not looking at every individual reading, just the high-level state (and variances from expected values) that tells you where to focus.


### **Open source and ready to use**


The[ROS Graph Monitor](https://github.com/ros-tooling/graph-monitor/tree/main/rosgraph_monitor) is now released into ROS 2 distributions, and[the Foxglove extension](https://github.com/polymathrobotics/foxglove_extensions/tree/main/ros2-graph) is now open source. You can use them in your application today, and soon the extension will be available on the Foxglove public registry.


### **What's next**


There is an active backlog for both of these tools. Visualizing topic frequencies is high on the list, so you could glance at the graph and see which areas are “hot” and which are “cold.” There is also a lot of room for improvement in the rendering of the ROS graph in the panel.


These kinds of tools don't directly make a robot more reliable, but they make engineers far more effective. The faster you can see what is going on, the faster you can fix it.


‍
