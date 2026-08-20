---
schema_version: "1.0.0"
document_id: "a1e168892d7a95732820638b44893d8e58514725c2398d706d0df7d6f1d6588d"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-b23fdbdd1cee"
canonical_url: "https://blog.qualys.com/qualys-insights/2026/07/08/qualys-joins-chainguard-athena-turning-coalition-scale-findings-into-validated-action"
published_at: "2026-07-08T22:15:08+00:00"
first_seen_at: "2026-07-20T03:31:07.801512+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:0d6add888a00501b31cc6dfab7dc6d5dafc1f0993d8ba438b7e7364fee7b7691"
---

# When AI-Accelerated Discovery Outruns Patching, Exploitability Proof Decides What Gets Fixed First

#### Table of Contents


- The volume problem is now a velocity problem
- Severity is not exploitability
- What Qualys brings to the coalition
- Validation is the bridge


### Why Qualys joined the Athena coalition, and what it means for how you prioritize risk.


Qualys is proud to have joined[Athena, the industry coalition Chainguard launched](https://www.prnewswire.com/news-releases/chainguard-adds-new-members-to-athena-coalition-as-coordinated-open-source-defense-scales-302818937.html) to coordinate the defense of open source software. It’s a cause we strongly support, because the problem Athena is working to solve is the same one the[Qualys Threat Research Unit](https://www.qualys.com/tru) (TRU) has been measuring for the past four years. And the piece we bring to the table is the one thing most security teams tell us they are missing.


For customers, this is not about another vulnerability feed. It is about knowing which exposures can actually be exploited, which controls are already working, and which fixes must move first. Athena makes the ecosystem faster at finding and fixing. Qualys helps customers understand which of those findings can hurt them and what to do about them first.


## **The volume problem is now a velocity problem**


Frontier AI models are finding novel vulnerabilities in open source at machine speed. In its first three weeks, Athena processed tens of thousands of findings, 42 percent of them critical or high severity. The real exposure is larger than that 42 percent suggests, because the same models that find individual bugs can chain a medium-severity authentication bypass with a medium-severity file write and arrive at unauthenticated remote code execution that no single CVSS score would have flagged. Our research has documented well over a hundred exploit chains observed in real-world attacks, and in several of the most heavily exploited ones, the vulnerability that unlocks the whole path scores below 7.0 on its own.


Meanwhile, the defender’s clock has run out.[Mandiant’s M-Trends](https://cloud.google.com/security/resources/m-trends) data puts the mean time-to-exploit at negative seven days, meaning vulnerabilities are being weaponized before a patch exists.[Our own analysis](https://www.qualys.com/forms/whitepapers/the-broken-physics-of-remediation) of more than a billion CISA KEV remediation records across 10,000+ organizations shows the problem clearly: from 2022 to 2025, closed vulnerability events grew 6.5x, from roughly 73 million to 473 million. But outcomes did not improve. The share of critical KEV vulnerabilities still open at Day 7 rose from 56 percent to 63 percent. Teams are closing more issues than ever, but the backlog is compounding faster than the current model can absorb.


More findings do not automatically mean better security. If discovery accelerates and nothing else changes, the backlog is what accelerates.


#### Read More


### Download the Broken Physics of Remediation to see what data from 10,000 orgs reveals about remediation outcomes.


[Read More](https://www.qualys.com/forms/whitepapers/the-broken-physics-of-remediation)


## **Severity is not exploitability**


The uncomfortable fact underneath the backlog is that fewer than one percent of vulnerabilities labeled critical are ever actually exploited in the wild. Most of what fills a remediation queue is theoretical. The version matches a CVE, so the scanner flags it. Whether the vulnerable code path is reachable, whether the service is exposed, whether a WAF or segmentation already blocks the vector, whether it chains with anything else the environment holds, none of that is in the score.


Teams end up spending engineering cycles on vulnerabilities that were never exploitable, while the small fraction that are genuinely exploitable wait in the same queue. And a growing share of real exposure never enters the queue at all because it has no CVE. Seven percent of Athena’s early findings sit in packages more than five years old, many of them silently fixed upstream with no identifier for a scanner to key on. Attackers do not wait for CVEs. Defenders should not depend on them either. This is a gap we have been closing for years, which is why Qualys today ships more than 17,000 detection signatures for vulnerabilities that have no CVE assigned.


The question a security leader needs answered is not whether a version is vulnerable in theory. It is whether an attacker can exploit it in this environment right now, and what to do about it before a patch is available or can be deployed.


## **What Qualys brings to the coalition**


Answering that question is what[Qualys TruConfirm](https://www.qualys.com/apps/truconfirm) was built for, and it is why we are joining Athena as a cyber partner. As Athena’s disclosure process advances, Qualys can help convert early vulnerability intelligence into validated customer action through safe exploitability validation, exposure context, and prioritization.


TruConfirm confirms whether a vulnerability is actually exploitable on a specific asset by exercising the same path an attacker would, but with a benign, non-destructive action in place of a payload. A controlled callback. A cryptographic hash the target can only produce if the injected code ran. A read-only response signature. The goal is controlled proof without disruptive payloads, persistent changes, or unnecessary production impact. The result is proof, rather than probability, that a remediation team can act on without revalidating, and an auditor can inspect.


Proof is what turns a coalition-scale feed into a customer-scale decision. When Athena surfaces a vulnerability under embargo, our customers do not get another alert. They get a confirmed answer. Either this is exploitable on these assets, with the compensating control that holds until the patch lands, or this is present but not reachable and can wait. When Athena publishes an OSV record for a silent fix that never got a CVE, our customers see whether they are actually running the exposed version and whether the path to it is open, instead of being blind because their tooling only speaks CVE.


## **Validation is the bridge**


Validation does not replace patching. It sequences and prioritizes patching. When time-to-exploit is negative, and patch deployment still takes weeks, the only defensible posture is to know precisely which exposures are live, apply the compensating control that buys time, patch on a schedule the business can absorb, and then verify the exposure is actually closed. Find, validate, prioritize, remediate, verify. Validation is the step that turns the other four from a list of tickets into a ranked plan a CISO can stand behind.


Athena makes the ecosystem faster at finding and fixing. Qualys makes customers faster at validating, prioritizing, and acting. Coordinated defense needs both. Intelligence without environmental context is another feed to triage. Intelligence validated against your environment becomes a decision.


We will have more to share as the first live wave of the cyber partner program runs. If you want to understand how safe exploitability validation works under the hood, the[TruConfirm technical architecture paper](https://assets.qualys.com/asset/aaf17046-a0ff-4794-a82a-21d88bc67cb6/Qualys-White-Paper-TruConfirm-Technical.pdf) walks through the detection phases and safety principles in detail. If you want the data behind the remediation gap,[The Broken Physics of Remediation](https://www.qualys.com/forms/whitepapers/the-broken-physics-of-remediation) is the full study. The era of treating every scanner finding as equally urgent is over. The next advantage belongs to teams that can prove real exploitability, focus remediation where it matters most, and reduce exposure before attackers
