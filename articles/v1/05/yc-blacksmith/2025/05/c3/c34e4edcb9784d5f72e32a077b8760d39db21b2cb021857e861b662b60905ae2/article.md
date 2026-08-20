---
schema_version: "1.0.0"
document_id: "c34e4edcb9784d5f72e32a077b8760d39db21b2cb021857e861b662b60905ae2"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/blacksmith-seed-announcement"
published_at: "2025-05-01T00:00:00+00:00"
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:c6de5626fe0ebdede083ffec19c4f18892368d943ba821d11f300ca401951054"
---

# Blacksmith Raises $3.5M led by GV and Y Combinator to Build a High-Performance CI Cloud.

We can finally stop holding our breath and just say it: we've raised $3.5M for our seed round! This was led by[Erik Nordlander](https://www.linkedin.com/in/eriknordlander/) at Google Ventures (GV) and Y Combinator, along with some prolific angels like[Spencer Kimball](https://www.linkedin.com/in/spencerwkimball/) (CEO of Cockroach Labs),[Peter Mattis](https://www.linkedin.com/in/peter-mattis-46549144/) (CTO of Cockroach Labs),[Rich Aberman](https://www.linkedin.com/in/richaberman/) (Co-founder of WePay),[JJ Fliegelman](https://www.linkedin.com/in/jjfliegelman/) (Co-founder of WayUp),[Eli Brown](https://www.linkedin.com/in/elibrownye/) (Founder of Guilded) and[Theo Browne](https://x.com/theo) (Founder of T3 Chat). What started a little over a year ago has quickly evolved to power CI for over 600 organizations, including[Ashby](https://www.blacksmith.sh/customer-stories/ashby) ,[Finch](https://www.blacksmith.sh/customer-stories/finch) ,[Clerk](https://www.blacksmith.sh/customer-stories/clerk) , Veed, and Mintlify. And yet, this is only the beginning.


## **You’re building a CI Cloud?**


Today, developers know us as a drop-in replacement for GitHub Actions runners which is twice as fast at half the cost. Underneath it all, we’re building a CI cloud -- a hardware-software stack that is designed from first principles to provide best-in-class compute, observability, and security for CI workloads.


Before starting Blacksmith, we were engineers at Cockroach Labs, Faire, and Superblocks. Aayush and Maru worked on the core database team at Cockroach and were experts on CockroachDB’s data distribution and disaster recovery systems. JP worked at Faire, where he helped scale search infrastructure and was later a founding member of the ads team that he helped to grow to several million in ARR. While working on ads, JP realized how critical fast CI and deployments were, especially when deploying hot fixes. Every minute of downtime is a minute of lost revenue. From our experiences and talking to others in the industry, we saw firsthand how companies struggle with managing CI infra. If we had to distill what great CI infra looks like, it boils down to the following:


- Fast compute that is instantly available, without the overhead of a dedicated team to manage it
- A predictable pricing model that scales with usage
- Observability into issues and long term trends that block developers from getting their code merged


Building great CI infra is hard. CI workloads are fundamentally *spiky* and *short-lived* , causing[companies](https://wecode.wepay.com/posts/cicd-autoscale-one) of all[sizes](https://www.rippling.com/blog/how-rippling-used-spot-instances-to-save-and-scale-ci-cd) to struggle with orchestrating such workloads on hyperscalers like AWS (more on this below). Furthermore, we believe that with the rise of AI code-gen tools, we're going to see an explosion in the amount of tests developers write and that the compute requirements for CI are going to grow exponentially in the coming years.


## **Wait, but why can’t I self-host CI infra on AWS/GCP/Azure?**


Well, you could. But we don’t believe you should. The longer we’ve been in this business, the more we learn why self-hosting CI infra is arduous and painful.


> " *I love self-hosting CI infrastructure. GitHub's ARC is so easy to use* "


##### — No DevOps Engineer ever


There are several reasons why traditional hyperscalers are not a good fit for CI workloads. Among these are *slow provisioning times* and *forced bundling* .


Like we mentioned before, CI workloads tend to be short-lived and spiky. It's not uncommon for companies with even 30 engineers to go from using 0 vCPUs to needing 3000 vCPUs for CI in under a minute. The provisioning characteristics of traditional hyperscaler VMs (like EC2 instances) are simply not meant for such bursty workloads, and they can vary significantly based on factors like the time of day, or how new the instance type is. Companies that self-host typically use on-demand VMs and autoscaling on Kubernetes to maintain a warm pool that is ready to pick up CI jobs. But then they need to make tradeoffs around overprovisioning compute for their warm pool to absorb these sudden spikes or accepting that their developers must wait for nodes to be provisioned. This becomes a growing productivity bottleneck as a team grows, both on the dev infra team and the developers themselves.


Another key issue is that the hyperscalers force-bundle network-attached storage (EBS) to their VMs. EBS volumes, almost by definition, are a tradeoff away from performance and in favor of durability. AWS/GCP/Azure charge you more to provision instances with locally attached NVMe storage, and some of the newest instance types don’t even offer a variant with local storage. This is incompatible with the requirements of ephemeral CI jobs, where you want the fastest storage possible and are totally willing to sacrifice durability. Here's a striking comparison: a 2TB io2 EBS volume provisioned for 64000 IOPS on AWS[costs ~$3,850 per month](https://calculator.aws/#/estimate?id=3869c27d5964c770cdef42c8dd72b658ebec21ee) , whereas you can go buy a 2TB Samsung NVMe drive, that does over 1M IOPS, for[less than $200 outright](https://www.amazon.com/Samsung-Internal-Gaming-Editing-MZ-V9P2T0BW/dp/B0B9C4DKKG?th=1) .


We reasoned about these issues from first principles and found we could offer a much better experience by owning our own pool of bare-metal hardware that only runs CI jobs. We chose to build out a fleet of consumer gaming CPUs, as they have the highest single-core performance, which is the main bottleneck for CI jobs like builds, code compilation, and tests. Having full control over the hardware-software stack also lets us make tradeoffs, and optimizations for common CI use-cases, like mounting your Docker layer cache into your VMs so all your[Docker builds are incremental in CI](https://docs.blacksmith.sh/blacksmith-caching/docker-builds) .


## **What’s ahead for Blacksmith**


Long term, we envision CI needs through the lens of[Maslow's hierarchy](https://en.wikipedia.org/wiki/Maslow%27s_hierarchy_of_needs) , structured in three layers. At the bottom is a fast compute layer, upon which we build an observability layer that actually assists developers in merging code quicker by identifying pernicious performance regressions and flaky tests. At the top sits security, as we believe we're at the right layer to guard against supply chain attacks.


While we've already begun tackling some of these challenges, much work remains before we can fully ascend this hierarchy — and we can’t work our way to the top without more help. This recent funding helps us hire folks who are passionate about this mission and the opportunity this presents. We’re hiring across the board — check out[our careers page](https://jobs.ashbyhq.com/Blacksmith) .


We could not be more excited about what we have in stock this year and can’t wait to get it in the hands of our customers. Don't miss out — come[try it out for yourself today](https://app.blacksmith.sh/) !
