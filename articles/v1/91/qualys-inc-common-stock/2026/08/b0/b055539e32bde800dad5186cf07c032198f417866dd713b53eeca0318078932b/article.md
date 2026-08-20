---
schema_version: "1.0.0"
document_id: "b055539e32bde800dad5186cf07c032198f417866dd713b53eeca0318078932b"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-d1030f25037f"
canonical_url: "https://blog.qualys.com/product-tech/2026/08/18/cve-2026-68820-kev-bod-26-04-requirements"
published_at: "2026-08-18T18:06:38+00:00"
first_seen_at: "2026-08-18T21:31:01.825892+00:00"
fetched_at: "2026-08-18T21:31:03.025998+00:00"
content_hash: "sha256:49e84244bd11f2b7cb57dbe362d7dd8bb1f7ead720aad31fbbce0388f161f822"
---

# CVE-2026-68820 is in KEV. Here Is What CISA BOD 26-04 Actually Requires Now

#### Table of Contents


- CISA BOD 26-04 Timeline
- How CVE-2026-68820 Can Be Exploited
- Patch Limitation: The Need to Reboot
- Our Recommendation: Deploy the Patch Now
- How We Calculate Patch Reliability
- Frequently Asked Questions (FAQs)


---


#### Executive Summary


*CVE-2026-68820 is an actively exploited Windows vulnerability listed in CISA’s Known Exploited Vulnerabilities (KEV) Catalog, with a remediation deadline as suggested by CISA BOD 26-04. CISA BOD 26-04 introduces risk-based remediation timelines ranging from 3 to 14 days, increasing the pressure on teams to move quickly from patch availability to verified remediation. Installing the patch alone does not complete remediation, as the fix replaces a kernel driver and requires affected endpoints to reboot. Qualys AI-Powered Patch Reliability Scoring rates the KB5121003 and KB5120249 updates high for reliability, while Qualys TruRisk Eliminate helps teams deploy the update, enforce the required reboot, and verify that remediation is complete within the required timeline.*


---


[Qualys TruRisk Eliminate](https://www.qualys.com/apps/trurisk-eliminate) helps teams respond to patches that can’t wait, deadlines that don’t bend, and environments where standard staged rollouts collapse under the timeline. It deploys the cumulative update, enforces the required reboot, and reports remediation state.


**CVE-2026-68820 shows why this matters.**


Microsoft published the fix for[CVE-2026-68820](https://cveawg.mitre.org/api/cve/CVE-2026-68820) on August 11, and[CISA added it to the Known Exploited Vulnerabilities (KEV) Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) the same day, with a remediation deadline as suggested by CISA BOD 26-04. **That **gives** IT Ops 3 to 14 days to test the patch, deploy it across affected endpoints, complete the required reboots, and verify that remediation is complete.**


## **CISA BOD 26-04 Timeline**


CISA designates CVE-2026-68820 as:


### **What This Means in Practice**


**Internal/non-exposed Windows endpoints:** 14-day deadline


- **Due date: August 25, 2026**


**Publicly exposed Windows systems (e.g., internet-facing):** 3-day deadline


- **Due date: August 14, 2026**


## **How CVE-2026-68820 Can Be Exploited**


CVE-2026-68820 is a use-after-free vulnerability in the Windows Ancillary Function Driver for WinSock (afd.sys). It sits on every Windows endpoint and is reachable from any low-privilege account. An attacker with an existing foothold runs a crafted application, wins the race, and takes SYSTEM. No user interaction required. Microsoft rates it Important at CVSS 7.0, and it was already under active exploitation when the patch shipped.


## **Patch Limitation: The Need to Reboot**


The fix is included in the August cumulative update: **KB5121003** for Windows 11, bringing builds to 26200.9168 and 26100.9168, and **KB5120249** for Windows 10 under Extended Security Updates.


Microsoft identifies no workaround, which is expected given that afd.sys cannot be disabled or firewalled.


Because the fix requires replacing a kernel driver, the vulnerable driver remains active until the endpoint restarts. Installing the update alone, therefore, does not complete remediation. Endpoints with the patch installed but a reboot pending remain exposed, making restart completion critical to meeting the CISA BOD remediation deadline.


## **Our Recommendation: Deploy the Patch Now**


Our recommendation is to deploy our high-reliability patches, **KB5121003 and KB5120249, immediately, then reboot the affected endpoints to complete remediation.**


## **How We Calculate Patch Reliability**


Qualys AI-Powered Patch Reliability Scoring predicts whether a patch will deploy cleanly in your environment before you deploy it.


It combines two signals: **global public sentiment** , where LLMs continuously analyze large-scale feedback from across the internet, including technical discussions, release-related feedback, and other real-world indicators that emerge after a patch ships, and **Qualys telemetry on patch rollback rates and vulnerability reopen rates** . These two signals are combined into one reliability score.


---


**Start your 30-day trial of Qualys TruRisk Eliminate and learn how it helps deploy patches at speed.**


[Start Free Trial](https://www.qualys.com/free-trial-new/trurisk-eliminate/)


---


## Frequently Asked Questions (FAQs)


### **What is CVE-2026-68820?**


It is a use-after-free race condition in afd.sys (the Windows Sockets API kernel driver) that allows a low-privilege local attacker to escalate to SYSTEM without requiring user interaction.


### **Why is the remediation deadline so aggressive?**


CISA added the CVE to the KEV catalog on the same day the patch was released. Under BOD 26-04 logic, publicly exposed systems have a 3-day deadline, and internal systems have a 14-day deadline.


### **Why isn’t installing the patch enough?**


The fix replaces a kernel driver. The vulnerable driver remains loaded and active until the system is rebooted. A “patched but not rebooted” endpoint is still fully exposed.


### **How can Qualys help meet the deadline?**


TruRisk Eliminate can deploy the required cumulative update and enforce the reboot in a single job. It also reports the true remediation state (including reboot completion) so teams can verify the KEV deadline has been met.
