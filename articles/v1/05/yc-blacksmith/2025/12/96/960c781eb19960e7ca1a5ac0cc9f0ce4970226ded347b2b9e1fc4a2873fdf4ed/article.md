---
schema_version: "1.0.0"
document_id: "960c781eb19960e7ca1a5ac0cc9f0ce4970226ded347b2b9e1fc4a2873fdf4ed"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/actions-pricing"
published_at: "2025-12-16T00:00:00+00:00"
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T22:24:57.117171+00:00"
content_hash: "sha256:0cfc239bba3f314c2ee1061b7cf5e3790de0abc53c8b8d20c30022215641599a"
---

# The GitHub Actions control plane is no longer free

## **What's happening?**


GitHub just[announced changes](https://resources.github.com/actions/2026-pricing-changes-for-github-actions/) to Actions pricing. Previously, GitHub Actions had a free control plane. That meant if you used GitHub Actions but ran jobs outside of GitHub-hosted runners, whether that’s on Blacksmith, on your own machines, or in your own AWS account, you paid nothing to GitHub for those minutes; you only paid for the compute.


With this change, GitHub is introducing a $0.002 per-minute platform fee for *all* GitHub Actions usage.


In practice, this means CI costs now have two components:


- Compute costs (whoever runs your runners)
- A flat GitHub platform fee, charged per minute of Actions usage


These changes go into effect on March 1st, 2026.


## Our perspective on why they’re making these changes


GitHub Actions has long had a graduation churn problem. As companies grow, their CI workloads become larger, more complex, and more expensive. At a certain scale, GitHub-hosted runners become both slow and costly, pushing teams to self-host or move to third-party runners like Blacksmith.


Until now, that shift had an important side effect: companies could continue using the GitHub Actions control plane while paying GitHub nothing for CI execution. GitHub provided scheduling, orchestration, and workflow automation, but captured no revenue from some of its largest and fastest-growing customers.


The new per-minute platform fee changes that. It directly monetizes the Actions control plane and establishes a floor on what GitHub earns from CI, regardless of where jobs run. In effect, self-hosting is no longer free.


At the same time, GitHub reduced the price of GitHub-hosted runners. This isn’t accidental. Lower hosted runner prices make GitHub-hosted runners more attractive, while the platform fee introduces a new, unavoidable cost for self-hosting.


From GitHub’s perspective, this is a rational move. Most Actions usage is concentrated on smaller runners, so the hosted runner price cuts likely don’t materially impact revenue. More importantly, GitHub is trading lower-margin compute revenue for higher-margin platform revenue.


Hosted runners are fundamentally a compute business. The platform fee, by contrast, monetizes software without scaling infrastructure costs linearly. As CI usage grows, that revenue scales with significantly better unit economics.


In the past, our customers have asked us how GitHub views third-party runners long-term. The platform fee largely answers that: GitHub now monetizes Actions usage regardless of where jobs run, aligning third-party runners like Blacksmith as ecosystem partners rather than workarounds.


## **Per-minute CI pricing and its impact on self-hosting**


Before this change, self-hosting was a way to avoid paying GitHub entirely. That’s no longer true. Now, self-hosting retains the operational burden of running CI infrastructure while still incurring per-minute charges on GitHub.


At that point, the primary variable you can still control is how many minutes your CI jobs consume. One approach is to run CI on infrastructure designed to minimize wall-clock time and eliminate redundant work. That’s the problem Blacksmith focuses on. In practice, this shows up in a few areas:


- Faster machines. Blacksmith runs CI jobs on CPUs that have 50%+ higher single-core performance than GitHub-hosted runners. As part of ongoing fleet upgrades, we’re deploying even newer instances that add an additional 15–25% improvement, further reducing runtime for CPU-bound workloads.
- Reusing work across runs.[Docker layer caches](https://docs.blacksmith.sh/blacksmith-caching/docker-builds) are persisted across CI runs, allowing unchanged layers to be reused rather than rebuilt, cutting Docker build times from tens of minutes to seconds for many customers.
- Container caching (beta). Service containers can be pre-hydrated on runners, removing repeated[image pulls and extraction](https://docs.blacksmith.sh/blacksmith-caching/docker-container-caching) from the job startup path.


With a per-minute platform fee, CI performance and cost are tightly coupled. The remaining lever is reducing CI time and total Actions.


‍
