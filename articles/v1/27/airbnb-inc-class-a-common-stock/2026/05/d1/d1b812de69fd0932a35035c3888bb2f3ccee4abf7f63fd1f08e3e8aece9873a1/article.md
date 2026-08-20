---
schema_version: "1.0.0"
document_id: "d1b812de69fd0932a35035c3888bb2f3ccee4abf7f63fd1f08e3e8aece9873a1"
company_key: "airbnb-inc-class-a-common-stock"
company: "Airbnb Inc."
source_id: "airbnb-inc-class-a-common-stock-rss-af0970d10a00"
canonical_url: "https://airbnb.tech/infrastructure/monitoring-reliably-at-scale/"
published_at: "2026-05-07T22:15:41+00:00"
first_seen_at: "2026-07-20T03:32:35.357269+00:00"
fetched_at: "2026-07-28T20:51:12.047673+00:00"
content_hash: "sha256:a7a6d54f60dfc55af5bc63e786c0d591230eceb47f5eb634246bf7df53f54a4d"
---

# Monitoring reliably at scale

**By** :[Abdurrahman J. Allawala](https://www.linkedin.com/in/aallawala/)


## Introduction


When an incident hits, teams lean on observability to answer the only questions that matter: *what’s broken, and why?* Monitoring systems are designed to help you answer these questions, and they usually do.


But what happens when your observability stack is dependent on the same systems that are failing? In that moment, the dashboards go dark, alerts stop firing, and the tools meant to guide recovery become part of the outage.


This is an increasingly common challenge as organizations consolidate onto shared platforms like Kubernetes, service meshes, and other common infrastructure components. At Airbnb, where thousands of services rely on shared infrastructure to deliver a reliable experience for guests and hosts, we traced a key reliability risk back to a circular dependency: our metrics pipeline was built on the same systems it was meant to observe.


Reliability is foundational to how we build trust and deliver exceptional experiences for our global community. Airbnb’s core values include being a responsible, reliable host to millions of guests and hosts around the world. Breaking that dependency chain became essential to operating responsibly at scale and sustaining trust.


In this blog post, we’ll walk through how we identified and eliminated dependencies in Airbnb’s metrics platform. We’ll start with why circular dependencies pose such a risk to observability, then dive into the specific design patterns we used to break them across compute, networking, and meta-monitoring. The takeaway is a set of practical approaches you can apply to make your observability stack more reliable than the systems it observes.


## The hidden risk: Circular dependencies in observability


Reliable observability isn’t just about collecting data — it’s about ensuring that the observability stack itself is more reliable than the systems it monitors.


At Airbnb, platform teams provide shared infrastructure that lets developers deploy, operate, and run services with minimal friction. Our team relied heavily on those foundations — and that’s where we hit a subtle snag: our observability stack depended on the same systems it was supposed to monitor. In other words, we had circular dependencies.


Fixing this wasn’t optional. Left unsolved, it undercuts a core purpose of observability: enabling engineers to diagnose root causes when something goes wrong. If the system that detects outages relies on the very infrastructure that is failing, visibility disappears exactly when it’s needed most.


Our solution was simple in principle: give every internal customer a redundant, highly available path for collecting metrics. Just as important, we drew a clear line around our responsibilities: we didn’t try to guarantee the availability of metrics for systems that weren’t our internal customers, and we didn’t design for redundancy across those external fault domains.


### Isolating compute: Dedicated clusters without the overhead


Among the many stakeholders of our metrics platform, the Airbnb Cloud team was one of the most critical. Airbnb operates Kubernetes clusters for everything from personal dev environments to the production workloads powering Airbnb.com, and our system had to meet the Cloud team’s expectations for metrics availability.


When considering where to run our observability components, we effectively faced two extremes:


1. Run Observability on shared production clusters.
This minimized our operational overhead, but tightly coupled us to the very applications we needed to monitor.
2. Operate our own Kubernetes clusters.
This provided full isolation but required deep operational expertise and ongoing maintenance — work the small but mighty Observability team wasn’t eager to take on.


Neither extreme worked. The first introduced circular dependencies, jeopardizing our availability targets; the second imposed undue operational burdens.


Our “just right” solution was to isolate our workloads onto dedicated Kubernetes clusters. These clusters aren’t shared with product or infrastructure applications, but they’re still administered and maintained by the Cloud team. This preserved Kubernetes as a managed foundation while reducing shared failure domains.


To keep this setup reliable, we coordinate changes with the Cloud team so that only one major change lands at a time, and so that changes are validated on lower-priority clusters before reaching operational clusters. This struck the balance we needed: high availability for our internal customers with minimal operational overhead for our team.


### Rethinking networking: Breaking free from the service mesh


Observability data is uniquely high-volume. At Airbnb’s scale, we send orders of magnitude more observability traffic than business traffic, which makes networking a key foundation of our observability stack.


Airbnb uses Istio as its service mesh, and while Istio is excellent for many infrastructure benefits, it wasn’t the right fit for our observability workloads. The immediate challenge was obvious: we couldn’t rely on the same data plane for monitoring product and infrastructure applications as for business traffic. That would create a circular dependency — metrics for the data plane would depend on that same data plane to be delivered.


The second issue was more subtle: observability traffic behaves very differently from typical business traffic. It’s far larger and (arguably) demands higher availability and priority. Our service mesh was originally designed around business workloads, not a world where every service continuously pushes telemetry to a central store. Sharing the same transport channel meant that as usage grew, congestion could make metrics unavailable, eroding critical debuggability for both platform engineers and product developers. Worse, telemetry spikes could also consume shared capacity and degrade or disrupt application traffic, directly impacting Airbnb.com availability.


The first major step was to rethink the network path altogether. To break free from the service mesh, we built a custom Layer 7 network ingress layer based on Envoy that load-balances traffic and routes read and write requests to the right backends. Running this proxy independent of the shared compute layer added fault tolerance and shielded our ingest path from service-mesh failures.


You might wonder why we chose to manage our own networking layer but not our own compute layer. For compute, Kubernetes was already a mature, managed foundation operated by the Cloud team, and adding dedicated clusters for observability was a relatively small increment to their existing footprint. The networking layer was different: our service mesh couldn’t cleanly isolate and prioritize observability traffic from business traffic at our scale, and the features we needed — strict prioritization, isolation, and custom routing for telemetry — sat squarely within our team’s domain. Owning this layer gave us the control we wanted and, compared to running Kubernetes ourselves, it was a much more straightforward surface to operate.


Decoupling from the service mesh also unlocked important new capabilities. Airbnb runs over 1,000 services, each mapped to its own tenant in a single, global user space. Our custom load-balancing tier makes this practical: we map each service name to a specific cluster backend, and every request must include a tenant header that informs routing. This header-based routing was a key motivation, relieving clients of complex configuration and ensuring even, predictable load distribution.


Finally, managing our own network layer gives us the flexibility to implement powerful custom features. For example, we can mirror metrics to alternate destinations for testing or enforce fine-grained access controls, which is critical when working with external vendors or specialized use cases. This extensibility continues to pay dividends as the platform evolves.


## Monitoring the monitors


Once we’d built resilience into the core layers of our metrics infrastructure, a natural question followed: **how do we know when the metrics engine itself is having issues?** To answer that, we added another layer whose purpose is to monitor the monitors — a concept often called **meta-monitoring** .


At Airbnb, we run a separate set of Prometheus instances dedicated to monitoring our observability stack. These Prometheus servers alert us when a component misbehaves. To avoid correlated failures, they run on Kubernetes nodes isolated from the observability stack and in different availability zones. Each Prometheus instance is part of a high‑availability set, as are the corresponding Alertmanagers, and we ensure no Prometheus–Alertmanager pair can land on the same shared infrastructure, further reducing shared fault domains.


This naturally raises the next question: **how do we know if the meta-monitoring layer is down?** Spinning up yet another monitoring stack would just lead to an infinite regress.


Instead, we use a **Dead Man’s Switch** — a mechanism that sends a steady signal. The recipient of the signal can assume something is wrong when the signal disappears. In our setup, we maintain an alerting rule that always fires as long as Prometheus is scraping correctly. Alertmanager continuously sends these alerts to an external AWS SNS topic, and a CloudWatch alarm monitors the rate of incoming messages. If they stop — because Prometheus is down, scraping has stalled, Alertmanager can’t send, or something else has degraded — the CloudWatch alarm triggers and on-call is paged.


Together, these components form a robust signal chain that surfaces issues in the meta-monitoring layer itself, helping ensure that our “monitoring of the monitors” is protected against silent failures.


## Conclusion


Reliable monitoring requires designing for uncomfortable moments such as partial outages, degraded networks, and failing dependencies — exactly the conditions where traditional observability architectures often go blind. In this post, we covered how we strengthened reliability by eliminating circular dependencies: running observability workloads on dedicated (but managed) Kubernetes clusters, separating telemetry transport from the service mesh with a purpose-built proxy tier, and adding meta-monitoring backed by a dead man’s switch to avoid silent failure.


These approaches generalize well beyond Airbnb. Any organization can improve monitoring reliability by mapping critical dependencies, intentionally isolating failure domains, and ensuring there is always an independent path for the signals that drive paging and incident response. The specific technologies will vary, but the principle holds: treat monitoring as a production system whose availability must exceed that of what it observes. Doing so preserves visibility during incidents, speeds recovery, and helps teams operate services with the confidence their users — and their business — depend on.


If this type of work interests you, check out some of our[related positions](https://careers.airbnb.com/) .


## Acknowledgments


Thank you to the Observability team — Callum Jones, Eugene Ma, Natasha Aleksandrova, Rishabh Kumar, Rong Hu, Wei Song, and Yann Ramin — and our partners across the company who helped make this a reality.


We also want to thank Suman Karumuri and Xuan Lu for their support in authoring this post during their time at Airbnb.


*All product names, logos, and brands are property of their respective owners. All company, product and service names used in this website are for identification purposes only. Use of these names, logos, and brands does not imply endorsement.*
