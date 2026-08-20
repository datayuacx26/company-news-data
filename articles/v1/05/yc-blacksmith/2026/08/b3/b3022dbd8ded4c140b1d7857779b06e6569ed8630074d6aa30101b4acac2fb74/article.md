---
schema_version: "1.0.0"
document_id: "b3022dbd8ded4c140b1d7857779b06e6569ed8630074d6aa30101b4acac2fb74"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/us-west-regional-failure-august-13-2026-postmortem"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T23:41:23.162103+00:00"
fetched_at: "2026-08-19T23:41:24.899375+00:00"
content_hash: "sha256:12dc8797bd9b5ae43dd9f27518985956d7be81a6858f94e214f07b057ba1e348"
---

# US west region outage due to facility cooling loss on August 13, 2026

On August 13, 2026, the facility hosting our Phoenix region lost cooling due to utility fluctuations and grid outages in the city. Beginning at 09:30 UTC, most of our compute in the region went offline and the region lost all storage nodes that power our caching and sticky disk features. GitHub Actions job adoption in Phoenix returned to near-normal by 21:45 UTC that day, but Actions Cache, Docker layer cache, Bazel remote cache, and sticky disk interactions remained impacted until 18:09 UTC on August 14. Our services in other regions remained available throughout, though customers saw longer than normal queue times as we moved workloads out of Phoenix into those regions.


This post explains the cause, the impact customers felt, and the changes we are making so that upstream failures like this one are absorbed by our infrastructure with as little customer impact as possible.


## What happened


The facility lost cooling after back-to-back utility interruptions overnight took down all four of its chillers. Temperatures inside the hall began climbing at around 06:00 UTC. At 09:37 UTC our monitoring alerted on NVMe drive temperatures in Phoenix crossing 85 °C, up from a normal 50–60 °C. The rise was uniform across every node in the region, not confined to individual machines.


NVMe temperatures, top 20 (UTC). Overnight they sat in the 50s to 60s, then climbed together from ~06:00 UTC and pinned near 85 °C


‍


Our storage clusters failed first: nodes hosting our MinIO and Ceph clusters overheated and lost quorum, taking Actions Cache, Docker layer cache, Bazel remote cache, and sticky disks offline across the region. At this time our assigner service was still routing jobs to Phoenix. Jobs were hanging on any interactions with our offline caching services rather than failing fast. As a temporary workaround we disabled caching in our Phoenix region so that job execution could proceed without interacting with our caches.


Hosts down across the fleet (UTC)


‍


Hosts responsible for adopting GitHub Actions jobs began failing through the morning as temperatures kept rising, peaking at roughly 2,400 unavailable before machines started recovering on their own as the hall cooled.


Rising temperatures also took out the facility's remote management network, so we could not power-cycle our machines until 00:00 UTC on August 14. The static IP gateways serving network traffic for some of our customers in Phoenix went offline in the same period. Until remote power returned, moving workloads to other regions was the only mitigation available to us, and it pushed that load onto regions already running their own traffic.


#### Customer impact


The impact our customers experienced varied with how much they relied on Phoenix. Static IPs, caching, and sticky disks were unavailable in the region throughout, and the workload shifts required to keep jobs running increased queue times in every region.


Unscheduled backlog by region (UTC)


‍


At peak, 62.3k jobs were waiting for a runner in Phoenix and 27.3k in Ashburn, the latter driven by the workloads we moved out of Phoenix. The Netherlands and Germany queued as well, at lower volumes.


- **Static IPs:** Customers with static IPs in Phoenix could not run until those IPs were re-registered in Ashburn by 14:30 UTC on August 13. We advised some organizations to fall back to GitHub-hosted runners in the meantime.
- **Caching and sticky disks:** Actions Cache, Docker layer cache, Bazel remote cache, and sticky disks were unavailable in Phoenix from 11:00 UTC on August 13 until 18:09 UTC on August 14. Jobs in the region ran cold, and a small amount of recently written cache data was not recoverable.
- **Queue times:** Waits rose in every region as we shifted workloads out of Phoenix. 16 and 32 vCPU runners were the most affected and frequently went unscheduled for hours; jobs assigned to our compute region in Germany saw wait times of up to three hours.


## How we resolved it


With remote power control unavailable, we could not recover Phoenix directly for most of August 13. Our primary mitigation was to move workloads to other regions, which meant re-registering static IPs in Ashburn and rebuilding sticky disks from scratch in the receiving region.


Hosts began recovering on their own as the hall cooled, and job pickup in Phoenix returned to near-normal by 21:45 UTC on August 13. Remote access returned at 00:00 UTC on August 14, which let us power-cycle storage, bring MinIO and Ceph back online, and reboot the 1,011 hosts that had not recovered. Sticky disks, Docker caching, and incremental builders were restored at 18:09 UTC on August 14.


## What we plan to do


We have the facility's root cause analysis and have raised our concerns with them directly. None of the changes below depend on it. Our job is to make a single facility's failure survivable regardless of cause.


- **Reduce how much capacity depends on any single facility.** This work is already underway: new capacity is landing in multiple facilities, growth in Phoenix is going into a different building than the one that failed, and Ashburn is split across facilities from the start.
- **Degrade instead of hanging when storage is unavailable.** When a region's storage is down, sticky-disk and cache operations should return immediately so the job runs without cache, rather than blocking until it times out. We are adding circuit breakers so this happens automatically, instead of requiring us to disable caching by hand as we did here.
- **Change our controls without a deploy.** Several of the settings we wanted to adjust during the incident could only be changed by redeploying services, reducing our rate of iteration significantly. We are moving those settings to runtime configurations so mitigation doesn't wait on a release.


Upstream outages are Blacksmith outages. Our customers chose us so they would not have to think about CI infrastructure, and for most of August 13 they had no choice but to think about it. We're sorry for the disruption, and for the engineering time our customers spent working around us instead of on their own work.
