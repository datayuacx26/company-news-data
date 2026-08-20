---
schema_version: "1.0.0"
document_id: "7d7fdffd4d3b56e2f7b30ba72a6ab82508ee1252cf1c1b7b2b46811fb1243f14"
company_key: "ouster-inc-common-stock"
company: "Ouster Inc."
source_id: "ouster-inc-common-stock-news-import-d3df9a0809e0"
canonical_url: "https://ouster.com/insights/blog/inha-liornet-lidar-in-the-snow"
published_at: null
first_seen_at: "2026-07-24T08:00:35.952779+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:ae2c8544402997568c6051eacaeb82d0d8bcd7d5a67968521e2bb5e4296a49f4"
---

# Inha University: Winter Autonomy with Ouster Digital Lidar

For autonomous vehicles, falling snow has always been a major headache. As flakes drift through a lidar sensor’s field of view, they create thousands of spurious points. To a traditional perception system, these look like phantom obstacles, effectively blinding the vehicle and causing[SLAM](https://ouster.com/insights/blog/introduction-to-slam-simultaneous-localization-and-mapping) (Simultaneous Localization and Mapping) algorithms to fail.


To solve this, researchers from Inha University (in collaboration with the Ministry of National Defense, Korea and KAIST) developed a breakthrough deep-learning framework: LIORNet.


## The Challenge: The Impossible Label


Training AI to recognize snow has historically hit a major bottleneck in annotation. You cannot ask a human to manually label 10,000 tiny, moving white dots in a high-speed 3D scan.


Furthermore, traditional filters often act like a blunt instrument. They are either too aggressive, deleting actual buildings and road signs along with the snow, or too weak, leaving enough noise to trigger emergency braking events.
