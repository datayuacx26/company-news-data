---
schema_version: "1.0.0"
document_id: "faf30e505c6e3a0f1b9cb04920627eb4278370d41c00c8191063116749d68dff"
company_key: "netscout-systems-inc-common-stock"
company: "NetScout Systems Inc."
source_id: "netscout-systems-inc-common-stock-rss-fc290aa3540c"
canonical_url: "https://www.netscout.com/blog/when-cloud-saas-ddos-mitigation-offerings-arent-enough"
published_at: "2026-03-04T13:47:52+00:00"
first_seen_at: "2026-07-20T23:22:05.095775+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:a9fe577559e01005b17325632d068584272d7470b14930b88b3e0e62ed44043c"
---

# When Cloud SaaS DDoS Mitigation Offerings Aren’t Enough

For a large Asia-based financial group, protecting customer-facing services wasn’t just a security priority—it was a regulatory mandate. The group’s environment spanned multiple subsidiaries with a mix of on‑premises data centers plus rapidly expanding public cloud workloads. The organization needed a single, standardized anti‑distributed denial-of-service (DDoS) approach that could work across the entire environment without violating strict government requirements that complicated the use of traditional software-as-a-service (SaaS) security services.


But compliance was only half the battle. The team also faced major visibility gaps: Native cloud DDoS options weren’t fully effective in their environment and didn’t provide the transparency and granular telemetry required by the group’s centralized security control center. In particular, the IT organization needed comprehensive event logs that could be fed into its security information and event management (SIEM) for unified monitoring—yet competing vendors often offered only sampled data, which didn’t meet internal oversight expectations. On the threat side, the group had to close protection gaps against sophisticated application-layer (Layer 7) and multivector attacks, including[DNS NXDOMAIN attacks](https://www.netscout.com/what-is-ddos/dns-nxdomain-flood) that some cloud-only and SaaS approaches struggled to mitigate.


The answer was a validated hybrid architecture: NETSCOUT[Arbor Edge Defense (AED)](https://www.netscout.com/product/arbor-edge-defense) for on‑prem protection, virtual AED for public cloud environments, and NETSCOUT Arbor Enterprise Manager (AEM) for centralized management. This approach enabled consistent policy management across a hybrid infrastructure while supporting compliance needs by allowing the group to manage security data independently. During the proof of concept, the solution stood out by successfully blocking multivector and DNS-specific attacks that competitors failed to stop. Just as importantly, it delivered full event logging (not sampling), aligning cleanly with SIEM integration requirements.


Post‑deployment, the group gained end‑to‑end visibility into DDoS events via centralized SIEM monitoring and improved operational independence from cloud-provider reporting limits. They also strengthened resilience in the cloud with automated health checks and failover—critical for maintaining continuous availability.


[Read the case study](https://www.netscout.com/resources/case-studies/hybrid-ddos-protection-with-netscout-arbor-edge-defense-in-the-public-cloud) **.**
