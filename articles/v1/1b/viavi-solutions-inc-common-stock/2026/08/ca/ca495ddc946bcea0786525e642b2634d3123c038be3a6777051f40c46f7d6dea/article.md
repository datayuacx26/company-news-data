---
schema_version: "1.0.0"
document_id: "ca495ddc946bcea0786525e642b2634d3123c038be3a6777051f40c46f7d6dea"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2026/08/07/chaos-by-design-why-we-need-to-break-ai-network-fabrics-on-purpose/"
published_at: "2026-08-07T11:35:52+00:00"
first_seen_at: "2026-08-07T12:50:06.605085+00:00"
fetched_at: "2026-08-07T12:50:08.073435+00:00"
content_hash: "sha256:c055cb3d755fa9b99616b4c859c1ded5bb3233a13a0292f35756edfbadfb198e"
---

# Chaos by Design: Why We Need to Break AI Network Fabrics on Purpose

**


# Chaos by Design: Why We Need to Break AI Network Fabrics on Purpose


- **Neil Holmquist
- ** August 7, 2026
- Like


*Notes from the front lines of AI fabric validation, just in time for*[OCP APAC Summit 2026](https://www.opencompute.org/summit/2026-ocp-apac-summit)


Here’s an uncomfortable truth about AI networking: the failures that determine whether a fabric is production-ready rarely appear under normal operating conditions.


Training clusters run for months. Collective operations complete successfully. Congestion clears and dashboards stay green. Everything appears healthy…until it doesn’t.


A burst storm arrives at exactly the wrong moment or a credit pool depletes under an untested traffic pattern. Suddenly, the fabric that spent a year in qualification collapses in production, often when it matters most.


The real issue isn’t that these failures occur. It’s that they’re entirely predictable. Traditional validation methods simply don’t recreate the conditions that expose them in a repeatable, measurable way.


### **The Open AI Networking Ecosystem Has Arrived**


For years, open AI networking was more roadmap discussion than deployment reality. That changed quickly.


UEC Specification 1.0 debuted in 2025. ESUN 1.0 released in March 2026.[Broadcom introduced Tomahawk Ultra](https://www.broadcom.com/company/news/product-releases/63341) silicon engineered for scale-up Ethernet and demonstrated UEC LLR/CBFC interoperability at OFC 2026. Synopsys is licensing ESUN IP, and VIAVI showcased Ultra Ethernet Transport (UET) interoperability with HPE Juniper at Interop Tokyo 2025.


This is no longer a discussion about future standards. Open AI networking is shipping today and increasingly finding its way into customer deployments.


As a result, the industry’s fundamental question has shifted. The question is no longer *“What should AI fabrics do?”* but *“How do we prove they will continue to do it under real-world conditions, failures, and scale?”*


### **Five Ways a “Healthy” Fabric Can Fail**


Across AI infrastructure deployments, the same classes of failures continue to surface.


- **Burst storms:** synchronized All-Reduce operations can overwhelm the fabric instantaneously in ways that everyday traffic rarely reproduces.
- **Credit starvation:** a large flow consumes available CBFC credits, starving other traffic sharing the same resources.
- **Asymmetric load:** uneven ECMP hashing concentrates traffic on certain paths while leaving others underutilized.
- **Retry cascades:** a single packet drop triggers retries, which generate additional congestion, causing further packet loss and even more retries.
- **Head-of-line blocking:** large flows occupy critical resources, delaying smaller latency-sensitive traffic and degrading overall application responsiveness.


None of these scenarios are unusual. All occur in production environments. What’s concerning is that most traditional lab qualification testing rarely exposes them consistently enough to diagnose and fix the underlying issues.


### **Inference Traffic Raises the Stakes**


If training traffic is challenging to validate, inference traffic is even more demanding.


Modern LLM inference introduces a mix of traffic patterns that place unique stress on the network. Prefill and decode phases exchange state through KV-cache transfers that can range from hundreds of megabytes to tens of gigabytes.


These large “elephant” transfers share infrastructure with latency-sensitive “mice” flows responsible for delivering tokens every few milliseconds.


When a KV-cache transfer consumes credits at the wrong moment, decode traffic suffers and inter-token latency increases. User experience degrades. p99.9 service-level objectives are missed.


This is not a theoretical edge case. It is a validation challenge that must be tested intentionally.


The impact becomes even more pronounced at scale. A tail-latency event that appears insignificant at small GPU counts becomes almost inevitable in large clusters.


At 64 GPUs, a 0.1% per-GPU tail-latency rate may appear occasionally. At 1,024 GPUs, that same rate becomes an expected occurrence at virtually every step.


As infrastructures scale, worst-case behavior becomes normal behavior.


### **Why NCCL Benchmarks Aren’t Enough**


Many organizations still rely heavily on NCCL-based benchmarks to validate AI fabrics.


NCCL testing certainly provides useful data, but it has important limitations. Because NCCL generates self-regulated traffic patterns, engineers cannot independently control burst characteristics, arrival timing, flow size distributions, incast conditions, and cross-flow interactions.


More importantly, NCCL benchmarks largely focus on application-level outcomes rather than fabric-level behavior. They measure throughput, but do not provide deep visibility into:


- Switch buffer occupancy
- Credit consumption and allocation
- Congestion development
- Flow interactions
- Tail-latency root causes


Yet these are precisely the areas where fabric failures originate. Today, the AI networking ecosystem still lacks widely accepted validation standards, including common acceptance test suites, standardized chaos scenarios, agreed p99.9 performance thresholds, and consistent qualification criteria.


As a result, every vendor is effectively defining “qualified” independently.


### **Closing the Validation Gap with Workload Emulation**


This is where[synthetic workload emulation](https://www.viavisolutions.com/en-us/solutions/ai-data-center-networking-test) becomes critical. Rather than waiting for production traffic to uncover weaknesses, engineers can intentionally create the conditions most likely to expose them.


[VIAVI TestCenter](https://www.viavisolutions.com/en-us/products/testcenter) was built to enable exactly this approach, generating realistic AI workload traffic patterns deterministically, repeatably, and at line rate without requiring a live GPU cluster.


With VIAVI TestCenter, teams can:


- Recreate burst storms, credit starvation, asymmetric ECMP load, retry cascades, and head-of-line blocking on demand
- Emulate KV-cache elephant-and-mice traffic interactions across varying context lengths and precision formats
- Measure p99.9 and tail-latency performance with visibility into switch buffers and credit behavior
- Compare firmware releases and hardware generations using identical, repeatable validation scenarios
- Validate multi-vendor interoperability before deployment by testing switches, NICs, and operating systems under controlled stress conditions


Because these workloads are synthetic, the methodology remains protocol-agnostic. The same approach can be applied across RoCEv2/DCQCN and emerging UEC/[UET](https://www.youtube.com/watch?v=co2iIPwGhvw) /CBFC environments, enabling meaningful comparisons.


The value is not simply generating stress. It is generating stress that can be **repeated, measured, and trusted.**


### **Moving from Reactive to Intentional Validation**


Open AI networking specifications have given the industry a blueprint for what AI fabrics should do. What remains missing is a shared, repeatable method for validating whether they continue to perform under the conditions that matter most. Workload emulation helps close that gap.


As ESUN and UEC deployments continue to accelerate, now is the time for the industry to align on repeatable validation methodologies rather than learning these lessons through production incidents.


Don’t wait for chaos to find your fabric. Instead, create it deliberately and test against it, measure it, and fix it. This is what we call “Chaos by Design.”


### **See Chaos by Design in Action**


See how VIAVI is helping validate next-generation AI fabrics with UET and workload emulation. Watch[our video](https://www.youtube.com/watch?v=co2iIPwGhvw) to see UET performance and interoperability in action.


In addition,[join VIAVI at OCP APAC Summit 2026](https://2026ocpapac.fnvirtual.app/a/schedule/) “Chaos by Design” session to learn how workload emulation helps expose hidden AI fabric failure modes before deployment.


- ** Categories:[AI](https://blog.viavisolutions.com/category/ai/) ,[High-Speed Ethernet](https://blog.viavisolutions.com/category/hse-ce/high-speed-ethernet/) ,[HSE and CE](https://blog.viavisolutions.com/category/hse-ce/)
- ** Tags:[AI](https://blog.viavisolutions.com/tag/ai/) ,[AI Networking](https://blog.viavisolutions.com/tag/ai-networking/) ,[TestCenter](https://blog.viavisolutions.com/tag/testcenter/) ,[Ultra Ethernet](https://blog.viavisolutions.com/tag/ultra-ethernet/)


-
-
-
-
-
-
