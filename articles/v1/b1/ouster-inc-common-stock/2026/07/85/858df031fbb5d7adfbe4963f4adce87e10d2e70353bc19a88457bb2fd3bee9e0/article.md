---
schema_version: "1.0.0"
document_id: "858df031fbb5d7adfbe4963f4adce87e10d2e70353bc19a88457bb2fd3bee9e0"
company_key: "ouster-inc-common-stock"
company: "Ouster Inc."
source_id: "ouster-inc-common-stock-news-import-d3df9a0809e0"
canonical_url: "https://ouster.com/insights/blog/freie-berlin-groundloc-mapping"
published_at: null
first_seen_at: "2026-07-24T08:00:35.952779+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:b20676bfc1affd6fc795f8a820213c28d430b0cf836c272c12e9a73da5075573"
---

# Freie Universität: High-Precision, Small-Footprint Mapping

Building a map is a relatively trivial task for autonomous systems operating in simple environments. While GPS provides a rough estimate, autonomy requires centimeter-level precision. Typically, this is achieved by comparing real-time lidar data against a pre-existing high-definition (HD) 3D map.


Storing the raw 3D data required to map an entire metropolitan area can consume terabytes of storage, overwhelming the onboard computers of mobile robots and driving up operational costs. Furthermore, in environments like tunnels, parking garages, or expansive highway bridges—where satellite signals fail and vertical landmarks are sparse—traditional localization systems often lose their way.


To solve this, researchers at Freie Universität Berlin have developed[GroundLoc](https://github.com/dcmlr/groundloc) , a lidar-only localization pipeline that proves you don't need a heavy map to achieve heavy-duty precision.


## The Challenge: Memory Limits


The primary bottleneck for large-scale autonomy is data density. Traditional HD maps rely on massive 3D point clouds that can consume upwards of 55 GB of data for every square kilometer of road. For a fleet operating across thousands of miles, this memory wall makes map updates and real-time retrieval nearly impossible.


Beyond storage, there is a structural challenge. Most localization algorithms rely on vertical features—like walls, poles, and building facades—to lock a vehicle’s position. But the real world is messy. A bus parked in front of a wall or a highway bridge with no nearby buildings can leave a robot with no landmarks to reference. To achieve reliable, all-weather autonomy, we need a way to localize that is both data-efficient and resilient to the dynamic chaos of urban traffic.
