---
schema_version: "1.0.0"
document_id: "da9cee41661457c8fc3b056f9cd16e1fde58e934e007e4f03e6e03a3c890daa0"
company_key: "criteo-s-a-american-depositary-shares"
company: "Criteo S.A."
source_id: "criteo-s-a-american-depositary-shares-rss-02db2411825d"
canonical_url: "https://medium.com/criteo-engineering/how-we-cut-haproxy-fleet-gcp-cost-in-half-by-moving-from-n2d-to-c4d-bbdafed3265f"
published_at: "2026-05-07T06:26:01+00:00"
first_seen_at: "2026-07-20T23:17:33.645392+00:00"
fetched_at: "2026-08-20T03:05:58.983824+00:00"
content_hash: "sha256:16c2d800a80ecec439a4f9f0db40ec227467c760dfd01e3f7b182d0cc30b6561"
---

# How We Cut HAProxy Fleet GCP Cost in Half by Moving from N2D to C4D

#### Author:[Stanislav Glukhov](https://www.linkedin.com/in/stanislav-glukhov/)


When you run a large production footprint in Google Cloud, changing a VM family is never just a hardware refresh. In our case, HAProxy sits on a critical path of the platform, serving as part of the traffic layer that hundreds of downstream systems quietly depend on every day. That means even a seemingly straightforward migration from one instance type to another has to be treated as a reliability exercise first and an infrastructure optimization second.


We decided to migrate our HAProxy fleet from N2D to C4D, not because the old setup was failing, but because at our scale, “good enough” is rarely the end of the story. With hundreds of VMs in production, small improvements in per-instance efficiency, network handling or latency behavior can compound into meaningful operational gains. At the same time, even a minor regression can have an outsized blast radius. This made the migration worth doing, but only with careful validation and a clear understanding of what success would actually mean.


What made this project especially interesting is that the value was not limited to raw benchmark improvements. We expected some performance benefits from moving to a newer instance family, but along the way we also found several less obvious advantages: better operational headroom, more predictable behavior under load, and benefits that only become visible when you operate proxy infrastructure at scale. This article is about that migration, how we approached it, and what we learned beyond the usual “new machine type is faster” narrative.


> “Even a seemingly straightforward migration from one instance type to another has to be treated as a reliability exercise first and an infrastructure optimization second”


### HAPROXY SETUP


In our platform, HAProxy is a core part of the traffic layer sitting between upstream traffic sources and a large pool of backend services in Google Cloud. It is not just a simple proxy tier: it serves millions of requests per second overall, with individual instances commonly handling around 100k–200k QPS. At that scale, even small changes in instance behavior can have a visible impact on latency, resilience and how well the platform absorbs traffic spikes or infrastructure events.


One of the main reasons this layer matters so much for us is that it gives us more flexibility than relying only on Google Cloud Load Balancing. It is also significantly less expensive, which is important when you run a fleet of this size. But the biggest advantage is control: with HAProxy, we can tune traffic distribution much more precisely and align balancing behavior with how our backend fleet actually behaves in production.


High-level traffic paths


That flexibility is especially valuable in our environment because a large part of our backend fleet runs on Spot VMs and those machines are not homogeneous. We use different instance types and sizes depending on availability, price and what capacity we can actually get in each region. As a result, backend capacity is not a flat pool of interchangeable nodes: some machines are stronger than others and the fleet composition can change quickly when Spot capacity becomes constrained.


This is where HAProxy gives us an advantage beyond cloud load balancing. Instead of treating every backend as equal, we can assign different balancing weights and distribute traffic according to the real serving capacity of each machine. To make those weights meaningful, we use our own internal performance measurements for the instance types we run rather than relying only on specs. That becomes especially useful during Spot shortages, when we need to replace one machine family with another and keep the service stable while the backend mix is changing.


### PLATFORM COMPARISON


N2D and C4D are not just two price points in the same catalog, they come from different product generations and were introduced for different goals. N2D was introduced as Google Cloud’s AMD-based general-purpose family, aimed at giving customers a lower-cost alternative for common compute workloads while still supporting large VM sizes and flexible memory ratios. The Milan-based version of N2D was announced in September 2021 as an update to the earlier N2D generation, bringing 3rd Gen AMD EPYC Milan into that same cost-oriented family. Google documents N2D as a general-purpose series with up to 224 vCPUs and up to 896 GB of memory.


C4D was introduced much later, in June 2025, for a different purpose. Google presents it as a high-performance VM family for demanding business workloads, built on 5th Gen AMD EPYC Turin and Google’s Titanium platform. In this context, Titanium matters as much as the CPU generation itself: Google describes it as dedicated offload hardware for networking, storage and virtualization, which reduces hypervisor overhead on host CPUs and is meant to deliver more predictable performance. Google also positions C4D above prior generations in raw capability, advertising up to 80% higher web-serving throughput and up to 30% better general compute performance versus the previous generation.[C4D VMs: Unparalleled performance for business workloads | Google Cloud Blog](https://cloud.google.com/blog/products/compute/c4d-vms-unparalleled-performance-for-business-workloads?utm_source=chatgpt.com)


For us, the architectural difference mattered less as a marketing distinction and more as an operational one. N2D was built to be a strong price-performance general-purpose option, which is exactly why it had been the default choice for years. C4D, by contrast, was launched for workloads where higher and more stable performance is worth paying for. That makes the comparison less about “old versus new” and more about whether a general-purpose Milan platform is good enough for the job, or whether a Turin + Titanium platform gives enough additional throughput and consistency to justify its higher cost.


### NOISY NEIGHBOR


In cloud environments, the noisy-neighbor effect is the name for performance interference caused by resource sharing on the same physical host. Even with virtualization and tenant isolation, not every underlying resource is perfectly dedicated. Different workloads can still compete for shared CPU caches, memory bandwidth, NUMA-local resources, interrupt handling capacity, NIC queues, storage paths and hypervisor attention. This usually does not appear as a hard failure. More often, it shows up as lower consistency: jitter, uneven latency and visible differences between instances that look identical on paper.


Our measurements are consistent with that kind of effect on N2D and it is especially visible for a latency-sensitive tier such as HAProxy. In practice, we observed performance differences of around 20% between nominally identical machines and performance on the same machine could also vary over time. For a high-QPS proxy layer, that means you cannot safely size the fleet around the average machine in the pool. Operationally, you have to orient load toward the weakest machines, because they define the safe capacity limit. That reduces usable headroom per instance and forces you to run more machines overall than you would need on a more stable platform.


For example, in the test below, we sent the maximum possible request volume to both the N2D servers and C2D, the previous generation of AMD instances that already provided much more stable performance. The difference was visible immediately. N2D performance fluctuated significantly throughout the day, even under the same test conditions, while C2D remained much more consistent over time.


N2D performanceC2D performance


> “Operationally, you have to orient load toward the weakest machines, because they define the safe capacity limit”


We do not observe the same behavior on C4D, and that is not surprising. Google explicitly presents C4D as a platform designed for stronger consistency, describing it as aligned with the underlying NUMA architecture to provide optimal, reliable, and consistent performance. In practice, C4D combines a newer Turin-generation CPU, higher memory bandwidth, and a more advanced platform stack, which likely reduces transient contention enough that it no longer becomes visible in our HAProxy workload. So while noisy-neighbor effects are theoretically possible on any shared platform, in our case, they are clearly visible on N2D and effectively negligible on C4D.


### SECRET SAUCE


One of the most valuable outcomes of this migration was that it improved much more than raw instance performance. During a broader HAProxy configuration optimization initiative, we had already learned that for this type of workload, hardware and network driver behavior matter almost as much as the proxy configuration itself. In a system serving traffic over Ethernet at very high rates, some share of CPU is always spent on IRQ processing. One of the first optimizations we applied was to dedicate a subset of cores to IRQ handling only. That helped, but it also made it clear that the default networking settings provided by the platform are designed for general-purpose workloads, not for a proxy layer operating at our scale.


A particularly useful optimization was reducing how frequently the system switches between kernel space and user space by increasing NIC interrupt coalescing values. In practice, this meant allowing the kernel to batch work more aggressively instead of interrupting the CPU as often. The change was simple:


```text
ethtool -C ens3 rx-usecs 256 tx-usecs 256  # instead of the default 20 / 50
```


Our preliminary investigation showed that this could increase ICMP response latency by around 0.512 microseconds, which was negligible for our use case, while reducing the user-space share of CPU load quite significantly. In other words, the machines became not just faster, but calmer: less CPU time was wasted on constant interruption and context switching and more of it was available for actual HAProxy work.


The non-obvious advantage was that this optimization turned out to depend not only on HAProxy tuning, but also on the NIC implementation and queue format exposed by the instance family. Older environments gave us either legacy VirtIO networking or an older generation of gVNIC behavior. Newer machines, by contrast, came with gVNIC by default and more importantly, with a more modern queue model that exposed more granular driver controls. That difference mattered because the rx-usecs setting is supported only when the NIC and driver stack expose the right capabilities. In our older fleet, that path was effectively blocked even when gVNIC was enabled.


The reason it failed on N2D was not that the idea was wrong, but that the platform simply did not support the level of NIC tuning we needed. On N2D, the gVNIC driver runs with the older GQI QPL queue format:


```text
sudo dmesg | grep gve  [    4.388590] gve 0000:00:04.0: Driver is running with GQI QPL queue format.
```


That queue format does not support reading or changing coalescing parameters through ethtool, which meant we could not tune rx-usecs and tx-usecs there. On more modern instances, the driver uses DQO RDA instead:


```text
sudo dmesg | grep gve  [    2.224848] gve 0000:00:03.0: Driver is running with DQO RDA queue format.
```


C4D Haproxy node before coalesce tuningC4D Haproxy node after coalesce tuning


> “The machines became not just faster, but calmer: less CPU time was wasted on constant interruption and context switching and more of it was available for actual HAProxy work”


That newer mode allows tuning of network parameters and in practice it gave us a meaningful extra lever for optimization. Once we could actually apply those settings, we saw the soft-IRQ portion of CPU load drop by roughly 10%. This was one of the clearest examples of a non-obvious migration benefit: the gain was not just that the newer instances were faster, but that they unlocked a class of low-level network tuning that had simply not been available to us before.


At the same time, this is only one part of a much broader story. We have done a long and detailed body of work on adapting HAProxy to cloud environments and that topic deserves a dedicated article of its own. Much of this initiative has been driven by our SRE professionals,[Dmytro Hopkalo](https://medium.com/u/df19499e717a) and Damien Claisse, who have invested significant effort into understanding and improving HAProxy behavior under real cloud constraints. They will be writing a separate article focused specifically on HAProxy setup and cloud adaptation shortly after this one.


### AND HOW IT GOES


We approached the migration conservatively. Our HAProxy fleet is managed through Google Instance Groups, so instead of doing a big-bang replacement, we migrated environments gradually, moving traffic from N2D-based groups to C4D-based groups step by step. After each change, we let the environment run for several days and watched the metrics that actually mattered for this layer: latency, CPU behavior, stability during peaks and the overall predictability of the fleet. For a proxy tier on the critical path, that kind of slow validation under real traffic was far more important than any synthetic benchmark.


On paper, the economics looked challenging. We use highcpu shapes for HAProxy, but the C4D equivalents come with much more memory than N2D. For example, n2d-highcpu-16 has 16 GB RAM, while c4d-highcpu-16 has about 30 GB. So part of the C4D premium was not just newer CPUs, but also paying for memory we did not necessarily need in the same ratio. In our case, the closest C4D equivalent ended up being about 88% more expensive per instance than N2D. That meant the migration only made sense if the performance gain was significant enough to let us run materially fewer resources.


#### Some Cost Optimization Examples


In production, that is exactly what happened. Because C4D gave us more usable performance per machine, more stable behavior, and better tuning options, we were able to right-size environments much more aggressively using c4d-highcpu-4, c4d-highcpu-8, and c4d-highcpu-16 according to real traffic patterns. We also moved to HyperDisk Balanced, which improved I/O behavior and simplified the platform. Above are the results for some environments in Japan and the Netherlands, where we achieved a significant reduction in vCPUs and instance cost. Overall, the effective savings were around 50%, despite C4D being substantially more expensive per instance on paper.


Just as importantly, we achieved all of this with zero downtime. The migration was completed without service interruption, which is exactly the standard you want for a high-QPS HAProxy layer. The only serious complication was capacity availability: during Black Friday 2025, we had difficulties with provisioning of C4D highcpu instances in the Netherlands for almost two months. That was a useful reminder that infrastructure migrations are not only about performance and cost. They also depend on regional capacity and keeping a realistic fallback plan. Looking back, the migration was worth it not because one benchmark was higher, but because the new platform changed the operational profile of the fleet: fewer vCPUs, fewer instances, lower cost, better tuning control and more predictable performance.


---


[How We Cut HAProxy Fleet GCP Cost in Half by Moving from N2D to C4D](https://medium.com/criteo-engineering/how-we-cut-haproxy-fleet-gcp-cost-in-half-by-moving-from-n2d-to-c4d-bbdafed3265f) was originally published in[Criteo Tech Blog](https://medium.com/criteo-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
