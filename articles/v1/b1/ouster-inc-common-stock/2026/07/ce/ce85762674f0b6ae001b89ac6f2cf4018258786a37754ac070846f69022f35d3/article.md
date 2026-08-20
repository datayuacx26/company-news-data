---
schema_version: "1.0.0"
document_id: "ce85762674f0b6ae001b89ac6f2cf4018258786a37754ac070846f69022f35d3"
company_key: "ouster-inc-common-stock"
company: "Ouster Inc."
source_id: "ouster-inc-common-stock-news-import-d3df9a0809e0"
canonical_url: "https://ouster.com/insights/blog/kth-joint-modeling-tgm-transportation"
published_at: null
first_seen_at: "2026-07-24T08:00:35.952779+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:065cc8c7135e726d787b66a6a3f60dc4ac9cefcf784f341a3cf8f9a54c8789b4"
---

# KTH: Joint Modeling of Static and Dynamic Occupancy for Intelligent Transportation

Building a map is the first step for any autonomous vehicle. In a controlled warehouse or an empty parking lot, this is relatively straightforward. But the real world is rarely static. Pedestrians cross streets, cars merge into lanes, and trams rumble through intersections.


For most Simultaneous Localization and Mapping (SLAM) algorithms, this movement is "noise." Traditional systems struggle to differentiate between a permanent wall and a temporary truck. The result? "Ghosting" or "smearing" in the map that causes the vehicle’s localization to drift, and in some cases, fail entirely.


To solve this, researchers at[KTH Royal Institute of Technology](https://www.kth.se/en) in Stockholm developed a new way for machines to understand motion. By combining Ouster digital lidar with a sophisticated probabilistic framework, they are helping autonomous systems see through the chaos of urban traffic.


### The Challenge: The "Ghost" in the Map


When a lidar sensor captures a moving object, standard SLAM algorithms often try to incorporate that object into the static map. If a car drives past your sensor, the algorithm might "smear" that car across the lane, creating a phantom obstacle that isn't actually there.


For an autonomous vehicle, these ghosts are dangerous. They degrade the accuracy of the map and make it impossible for the robot to know exactly where it is. To reach safety and reliability at scale for autonomous city driving, we need maps that can ignore the temporary and focus on the permanent.
