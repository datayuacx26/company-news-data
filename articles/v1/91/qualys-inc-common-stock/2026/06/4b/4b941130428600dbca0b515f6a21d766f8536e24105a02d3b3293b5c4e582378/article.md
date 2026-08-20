---
schema_version: "1.0.0"
document_id: "4b941130428600dbca0b515f6a21d766f8536e24105a02d3b3293b5c4e582378"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-d1030f25037f"
canonical_url: "https://blog.qualys.com/product-tech/2026/06/10/turning-millions-of-risks-into-one-actionable-list"
published_at: "2026-06-10T15:00:00+00:00"
first_seen_at: "2026-07-20T03:32:58.378885+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:c071310dd4081cb59e48f0d04bd6ebafbfce8bafcc5bb87e6096ec0772d98647"
---

# Turning Millions of Risks Into One Actionable List

#### Table of Contents


- The Problem Nobody Wanted to Say Out Loud
- How TruConfirm Actually Works
- Another Core Part of the Question
- Where Agent Val Comes In
- What the Numbers Look Like


Every security leader walks into Monday morning with the same question. The findings are there. The dashboards are running. But out of the thousands of critical vulnerabilities on that list, which ones can an attacker actually use against this organization today? Not in theory. Not in a lab. In production, with the controls that are actually in place.


For a long time, that question did not have a clean answer. Scanners tell you which software is present and match it against known vulnerabilities. They are almost entirely blind to whether any of those vulnerabilities can actually be reached and exploited, given everything else in the environment. A vulnerability scored critical might be sitting behind a WAF rule, a segmentation policy, or an EDR configuration that takes the real risk to near zero. Or it might be wide open. The scanner cannot tell the difference.


That gap is what TruConfirm, TruLens, and Agent Val were built to close.


#### Read More


### Find out how Qualys solutions prepare you for the post-Mythos era of risk.


[Read More](https://www.qualys.com/post-mythos-autonomous-remediation)


## The Problem Nobody Wanted to Say Out Loud


[Less than one percent of critical vulnerabilities](https://www.qualys.com/forms/whitepapers/the-broken-physics-of-remediation) in the average enterprise are actually weaponized. Everything else is theoretical risk — real in the abstract, unreachable in practice.


That is a significant claim, and it has significant consequences. If it is true, then the vast majority of the work most security teams do — the triage, the ticketing, the remediation cycles — is being spent on exposures that were never going to result in a breach. The queue is not a prioritized list of genuine threats. It is mostly noise, and the industry has spent years learning to take that noise seriously.


In 2025, around 48,000 new CVEs were disclosed. Roughly 52% of CVE exploits bypassed default web application firewall protections in independent testing — the safety net many organisations relied on was not catching what mattered.[The average time-to-exploit is now at negative seven days](https://cloud.google.com/security/resources/m-trends) , meaning vulnerabilities are being weaponized before patches exist. With Claude Mythos Preview now capable of chaining multiple vulnerabilities into working exploits within hours of disclosure, that window will only compress further.


***If less than 1% of critical vulnerabilities are weaponized, then 99% of the work the industry has been doing has been chasing things that were never going to hurt anyone.***


## How TruConfirm Actually Works


[TruConfirm](https://www.qualys.com/apps/truconfirm) is built on a deceptively simple idea. Instead of running attacker payloads, it replicates the behavior of an attacker using benign equivalents. If a vulnerability allows an attacker to create an out-of-band connection back to their server, TruConfirm sends a payload that asks the target to reach a Qualys-controlled endpoint. If it reaches out, the path is proven open. Nothing malicious has been sent. The mechanism is the one an attacker would use. The risk is gone.


Building safe exploit checks turned out to be the hardest engineering work in the project. Public exploits are straightforward. Safe versions are not. The threat research team had to take public exploit code, reverse-engineer it, and strip out everything that could damage a system while preserving the signal that proves the vulnerability is real. That work took two years and required hiring a team of world-class White Hat engineers dedicated to nothing else. It is, in many ways, the core intellectual property of the product.


A few principles govern how TruConfirm behaves. It leaves zero footprint — no agents, no persistence. It does not escalate privileges even where the vulnerability would allow it. When testing an Outlook RCE, it spins up a separate thread so the user sees nothing. Sensitive data in target responses gets stripped immediately. The answer that comes back is binary: exploitable, blocked, or unreachable.


There was one more problem nobody anticipated. Most customers allowlist their Qualys scanners — fine for vulnerability assessment, but it defeats the purpose of TruConfirm. A scan arriving from a known trusted IP is not a realistic test of whether an attacker could get through. So we built a separate scanner pool with IPs that customers have not allowlisted. From the target’s perspective, the traffic looks like it is coming from outside. For these purposes, it needs to.


## Another Core Part of the Question


TruConfirm answers whether a vulnerability is exploitable here.[TruLens](https://blog.qualys.com/product-tech/2025/10/15/introducing-trulens-for-enterprise-trurisk-management-unified-threat-intelligence) answers a different question: is anyone actually trying to use it against organizations like yours?


That distinction matters more than it might first appear. CVSS tells you what could happen in a worst-case scenario. It does not tell you whether anyone is attempting that worst-case scenario against your industry with the tools they currently have right now.


TruLens tracks more than 700 active threat actors — when they are active, what malware they use, what initial access techniques they prefer, and which industries they target. The intelligence comes from dark web chatter, deep web forums, third-party feeds, and Qualys’s own campaign analysis. When SolarWinds happened, the team mapped the five steps that played out. When the next Lazarus campaign surfaces, the playbook is ready before most teams know to look.


That changes the question security teams are actually answering. Instead of asking whether a CVE is critical, the question becomes whether anyone with the means and motive to hit this industry is actively weaponizing it today. Most of the time, the answer is no. Sometimes the answer is yes, and everything else stops.


Building that intelligence hit an unexpected problem: almost every commercial threat intelligence feed is calibrated to the United States, where honeypots have historically been deployed. For customers in India and other regions, the threat picture they were getting was not their threat picture. The fix was unglamorous — deploying honeypots in each region and building geo-specific intelligence from local telemetry. It does not make headlines, but it determines whether the product is useful in multiple geographies.


Threat actors themselves are harder to track than they appear. Lazarus is not one group — it splinters into 15 sub-units with different names and targets. Groups collaborate, sell access, share tooling. Tracking actors as stable identities was producing a misleading picture. The team shifted to tracing campaigns instead, working backwards through dark web claims, leaked tooling, and SEC filings to rebuild the full chain from reconnaissance through impact.


## Where Agent Val Comes In


Everything described so far works well when a human runs it. The harder question is who has time to run it across tens of thousands of CVEs and hundreds of thousands of assets, every day.


Nobody does.[Agent Val](https://blog.qualys.com/product-tech/2026/03/23/meet-agent-val-closing-the-validation-gap-in-exposure-management-at-machine-speed-with-agentic-ai) is the answer to that. It is an agentic layer purpose-built for safe exploit prioritization, validation, and remediation at scale. It decides which CVEs on which assets need to be validated first, launches the TruConfirm scan, selects the right scanner pool, updates the TruRisk score based on the result, and connects to the remediation workflow to elect an optimal remediation path: patch, compensating control, or mitigate now and patch later, based on factors such as the AI-based patch reliability score drawn from telemetry across millions of managed assets.


It runs in three modes. Fully automatic. Semi-automatic, where it shows what it is about to do and waits for approval — which is where most customers start. And manual, for teams that want to direct each step themselves. Agent Val does not run inside the customer environment. It runs on the FedRAMP-compliant Qualys platform and inherits the role-based access of the user running it. It cannot do anything the user could not do. That is by design.


## What the Numbers Look Like


One Fortune 50 company came to Qualys carrying 62.5 million live findings. After TruRisk and TruLens prioritization, that came down to roughly four percent. After overlaying critical asset context, one percent. After TruConfirm validation, the actionable list shrank again — to only the vulnerabilities genuinely exploitable in their specific environment. The cost of remediation dropped from $3.12 million for the full prioritized set to $31,000 for the confirmed exploitable set.


The point is not the math. It is what the math does for the team. Instead of arguing about which 600,000 findings to chase first, they have a list small enough to act on, meaningful enough to meet the core responsibility of keeping the organization secure, and evidence strong enough to defend in a board meeting.


TruConfirm is included in Enterprise TruRisk Management (ETM), not priced as a separate add-on. Exploit validation should not be a premium feature. It should be the baseline. The list of 60,000 critical findings was never the artifact that mattered. The list of 15 is. Getting there is what TruConfirm, TruLens, and Agent Val are built to do.


---


**Sign up for a demo of Qualys ETM to see TruConfirm, TruLens, and Agent Val in action.**


[Sign Up Now](https://www.qualys.com/demo/enterprise-trurisk-management)


---
