---
schema_version: "1.0.0"
document_id: "49df9d5d7448302061b835e92db4e59fb0747dd070c8b1c7dedea1dca26a3822"
company_key: "varonis-systems-inc-common-stock"
company: "Varonis Systems Inc."
source_id: "varonis-systems-inc-common-stock-rss-915499d71e96"
canonical_url: "https://www.varonis.com/blog/data-scanning-performance"
published_at: "2026-07-31T16:51:15+00:00"
first_seen_at: "2026-08-01T00:48:32.287340+00:00"
fetched_at: "2026-08-01T00:48:34.640105+00:00"
content_hash: "sha256:9e4358b29352379660ab988cad98291362d2d0e8470b1eba29c81d07951d80e8"
---

# Data Security Scanning Performance: Why Full Coverage Doesn't Mean Slow Scans

## Key takeaways


- The real limit on scan speed is usually API rate limits set by the cloud provider being scanned.
- Dynamic Data Concentration and Smart Scan work together to cut redundant scanning and surface high-risk data first.
- Automated remediation is what actually closes exposure at scale — in one environment, 3 million overexposed records dropped to 2,000 in two days.


Most conversations about[data security scanning](https://www.varonis.com/platform/data-discovery-and-classification?hsLang=en) tend to start with one question: "How *quickly* can this data security platform scan our data?" It's a fair ask. Long scan times delay visibility and, therefore, the actions an organization can begin taking to reduce risk.


However, the real measure of performance is how much ground a platform covers, how quickly it surfaces the risk that matters most, and whether it features automated remediation to address what it finds.


## The Varonis Data Security Platform: scanning capacity


The[Varonis Data Security Platform (DSP)](https://www.varonis.com/platform/data-discovery-and-classification?hsLang=en) measures scanning capacity in scan units, a standard measure that maps directly to compute resources. Performance scales predictably as more units are added, so doubling capacity means doubling throughput without needing to guess at the underlying CPU and memory requirements.


The best practice is to start small, monitor scan duration, and add units only if performance doesn't meet requirements. That gives an organization a predictable way to size its environment instead of overprovisioning a guess.


It's worth noting that, at Varonis, customers don't manage this math themselves. Varonis handles capacity planning and scaling in the background, so organizations see the result without needing to calculate compute requirements or provision infrastructure.


### The real bottleneck is usually API throttling


Scan units are only half the story. What happens when the data itself is complex, repetitive, or sitting behind a service with its own rules about access? That's where the real gains, and the real limits, show up.


Cloud services such as Google Workspace, AWS, and Box each limit the number of API calls that can be made in a given window to protect their own systems and keep things fair across all customers. Hit that ceiling, and requests get delayed or retried. Scan duration increases, no matter how much compute is idle on the scanning side.


- **What that looks like in practice:** A Google Workspace scan needs several API calls per file, including to download content, pull metadata, and check permissions. At a typical pace, a single collector generates roughly 342 API calls a minute. If an organization's rate limit with Google is 1,000 calls a minute, that ceiling becomes the real constraint once scanning scales past about three units. A fourth or fifth unit doesn't add speed at that point. It just runs into throttling.


This is exactly why "add more infrastructure" isn't a universal fix, and why Varonis surfaces throttling directly in the product instead of leaving a security team to guess why a scan slowed down.


Get started with our world-famous **Data Risk Assessment.**


[Get your assessment](https://info.varonis.com/en/data-risk-assessment?hsLang=en)


## Where data is being scanned also matters


The other important piece is where a scan actually runs. Some DSPs scan cloud-to-cloud, authenticating with an API key and pulling data across the internet into the vendor's cloud environment for scanning. Because there's no infrastructure to stand up, it can be a fast way to get started — but moving that data comes with real costs.


Data must cross the network to leave the customer's environment, so scanning is bound by WAN bandwidth rather than compute. It also adds egress costs, since cloud providers charge to move data out. And it means sensitive data is leaving the environment where it originated, which raises data privacy questions that matter in regulated industries.


Varonis supports cloud-to-cloud scanning, but many customers prefer a private data collector that runs inside the customer's own cloud environment. The data being classified never leaves that environment, and only metadata returns to Varonis. That removes WAN bandwidth as a bottleneck, avoids egress fees, and keeps sensitive data within boundaries the customer already controls.


Plus, once it's deployed, the collector runs on its own. In other words, there's no ongoing tuning or manual routing decisions to make — the security and performance benefits of scanning in place are automatic.


## Turning data repetition into an efficiency advantage


Some of the largest data stores are also the most repetitive. Picture an organization that enables AWS CloudTrail for every account, region, and service. The logs pile up by the millions, and nearly all share the same schema and sensitivity profile.


A scanner that treats each of those files as brand new spends most of its time reconfirming what it already knows. **Dynamic Data Concentration (DDC)** takes a different approach. It recognizes the repeating pattern, scans enough files to be confident in the classification, and applies that finding across the rest, without reopening files that are already classified.


But let's be clear: this isn't sampling. Sampling scans a subset and extrapolates, risking the omission of rare, sensitive data hidden in the part that went unscanned. DDC still scans the full store. It just stops repeating work it's already done. In an environment where a quarter of the files qualify, that alone can lift effective throughput.


## Being upfront about the tradeoffs


A few capabilities expand what a scan can see, at a cost worth knowing about upfront:


- Optical character recognition reads text from scanned documents and images, closing a real blind spot, but it takes significantly more computing than native text.
- Cloud Relay routes requests through a local collector to meet outbound-only connectivity requirements, adding a network hop and increased collector load that a direct connection wouldn't.
- Rich permission structures and detailed metadata take longer to process, but they're also what make the resulting risk analysis trustworthy.


None of these is a reason to skip a capability. Instead, they are reasons to make the tradeoff deliberately, rather than being surprised by it later.


### Speed to insight matters as much as speed to finish


Finishing a full scan quickly is nice, but finding the riskiest data sooner is better. **Smart Scan** prioritizes high-risk data, so the findings that matter most surface early. That means remediation can start before the scan finishes. Paired with DDC, the two work together comprehensively: Smart Scan shortens the time to the findings that matter, and DDC shortens the overall scan takes.


Both run automatically, with no configuration required. That means there's no threshold to set for what counts as repetitive and no priority list to build for what counts as high-risk. Varonis makes those calls in the background, and the customer simply sees a faster scan and the riskiest findings first.


### Speed to risk reduction matters most of all


Ultimately, fast scans and insights only matter if the risk they surface gets addressed at equal speed. Without automation, a scan that surfaces millions of overexposed records just creates a backlog. Now factor in[AI agents](https://www.varonis.com/blog/agentic-ai-security-risk?hsLang=en) , which can create new exposures faster than any security team can reasonably address, and manual remediation stops working.


Varonis addresses this with automated remediation: policy-driven actions that can close exposure across an entire environment, with scope defined by the organization rather than by the number of people available to click through tickets.


The reality is stark. In one environment, this is what automated remediation looked like:


- Remediate 900,000 sensitive files within one day of a policy going into effect.
- Reduce nearly 64,000 exposed folders to zero within one day.
- Remove 3 million overexposed records within two days.


The scan detects the exposure, but it's automated remediation that actually reduces the risk at scale. A platform that's fast at one and slow at the other has only solved half the problem.


Here are just some examples of Varonis customers who achieved rapid time-to-remediation (measured in days) with our automations.


Industry


Remediation Category


Risk Type / Before State


After Qty


Reduction %


Timeframe (Days)


Healthcare


Permissions Revocation


500,000 sensitive records exposed org-wide


800


99.8%


10


Healthcare


Sensitive Data Relocation


911,000 sensitive patient records in unsanctioned data store


0


100.0%


1


Healthcare


Sharing Link Cleanup


542,000 stale collaboration links exposing patient records org-wide


0


96.0%


4


Retail


Group Membership Removal


3,000,000 sensitive records exposed org-wide


2,200


99.9%


2


Retail


MFA / Policy Enforcement


2,100 users with MFA policy enforcement


0


100.0%


22


Public Sector


Data Labeling / Classification


63,400 sensitive records with a public sensitivity label


0


100.0%


1


Public Sector


Public Link Cleanup


337,000 sensitive records exposed publicly


980


99.7%


4


Construction


Service Account Hardening


88 admin accounts with SPN


0


100.0%


4


Construction


Data Deletion / Quarantine


200,000 stale sensitive records exposed to guest users


0


100.0%


7


Energy / Utilities


AD / Identity Cleanup


718 ghost users


718


100.0%


0


## From the first day to full scale


Getting Varonis up and running takes minutes, not days — that's[the deployment story](https://www.varonis.com/blog/fast-and-easy-agentless-cloud-deployment?hsLang=en) . But there's an important second chapter: what happens once data sources are connected and scanning runs continuously, at whatever scale the environment grows to. Fast onboarding helps an organization quickly find its first solution. Efficient, transparent scanning with automated remediation is what keeps that pace up as the data estate expands.


## Frequently asked questions


### **Why is data security scanning sometimes slow?**


Scan performance depends on data complexity, network conditions, and API rate limits set by the cloud provider being scanned. In many environments, those external limits constrain throughput more than compute does.


### **Does Varonis scan all data, or sample it?**


Varonis scans the full data estate. Dynamic Data Concentration (DDC) reduces redundant reads across repetitive files while ensuring every file is accounted for. Nothing is skipped or extrapolated statistically.


### **Does adding more scan units always make scans faster?**


Not past a certain point. API rate limits imposed by the source platform can become the binding constraint before compute does. Varonis surfaces throttling indicators directly, so organizations can find the actual bottleneck before adding infrastructure that won't help.
