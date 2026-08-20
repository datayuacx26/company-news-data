---
schema_version: "1.0.0"
document_id: "3c4704349e272f7edee88ae10e1089a0ae09ab44086d3c02ae57f385a4e7a8c7"
company_key: "ouster-inc-common-stock"
company: "Ouster Inc."
source_id: "ouster-inc-common-stock-news-import-d3df9a0809e0"
canonical_url: "https://ouster.com/insights/blog/3-2-firmware"
published_at: null
first_seen_at: "2026-07-24T08:00:35.952779+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:2e7587ef1020db0ead8b0a78450fc5265eb4a7f347865e5eda43d234b9b06a87"
---

# Firmware 3.2: New Features & Improved Data Quality

We are proud to announce the release of[Firmware 3.2](https://ouster.com/downloads) for Ouster sensors. This release introduces significant upgrades to edge processing capabilities, data quality, and ease of integration. Firmware 3.2 builds on the major performance improvements of our recent hardware generations with a new standalone 3D Zone Monitor, advanced blockage detection for dirty environments, synchronous IMU streams, and a modernized Web UI.


## Top Feature Updates


### 3D Zone Monitor


The most significant change in Firmware 3.2 is the introduction of a new 3D Zone Monitoring system. This feature supports basic on-sensor object occupancy and vacancy detection, allowing users to configure and monitor multiple 3D zones directly on the lidar sensor. The sensor monitors these zones without the need for extra compute or software.


- Define up to 128 3D zones of any shape or size, actively monitor 16 at a time
- Rapidly switch between active zones based on speed and steering
- Configurable zone trigger thresholds based on point count, frame count and mode (occupancy or vacancy)
- Get live zone statistics
- Functions independently or in conjunction with other point cloud operations


[Ouster SDK 0.16.0](https://static.ouster.dev/sdk-docs/reference/zone_monitor.html) (generally available) and newer versions provide full support to read/write zone configurations and visualize states over time.
