---
schema_version: "1.0.0"
document_id: "eabe3344196326b9719754849507d296357e0feb62426ba7915011ade53fa90b"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/introducing-archil"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:5fc49d14c345828bf376ff4b799eac6e7d91a25a86620d26259e9bad563d4a45"
---

# Introducing Archil

I'm excited to share that Regatta Storage has rebranded as "Archil," reflecting the company's expanded focus beyond storage to comprehensive data management. Archil is developing next-generation volume storage for cloud applications, delivering performance improvements and significant cost reductions compared to existing solutions.


## Background


I bring ten years of cloud storage industry experience, including leadership roles at Amazon's Elastic File System and Netflix, working with numerous data teams. A critical gap emerged: while S3 became standard for inactive data storage, the technology to store the active data that instances were currently using hadn't progressed much in 15 years.


Traditional EBS volumes require developers to provision storage upfront, creating challenges for applications with fluctuating or unknown storage needs. As applications scale across multiple instances, EBS limitations become apparent—volumes attach to single instances only, preventing data sharing.


## Archil Volumes


We developed Archil volumes—infinitely scaleable, shareable storage providing instant access to massive datasets from S3. Key advantages include:


- **Automatic scaling** to petabyte-scale without over-provisioning
- **Multi-instance access** with consistent data views
- **Direct S3 integration** eliminating download requirements
- **POSIX compatibility** requiring zero code changes


We're just getting started. Stay tuned for more updates as we continue to build the future of cloud storage infrastructure.
