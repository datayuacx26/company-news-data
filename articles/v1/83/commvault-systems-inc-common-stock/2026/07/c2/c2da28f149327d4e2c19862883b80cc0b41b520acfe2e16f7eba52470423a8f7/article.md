---
schema_version: "1.0.0"
document_id: "c2da28f149327d4e2c19862883b80cc0b41b520acfe2e16f7eba52470423a8f7"
company_key: "commvault-systems-inc-common-stock"
company: "Commvault Systems Inc."
source_id: "commvault-systems-inc-common-stock-news-import-d7ff9e033aa3"
canonical_url: "https://www.commvault.com/blogs/the-window-between-discovery-and-exploit-is-closing"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-25T00:29:47.384974+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:f495d6cb77553e0b1276f31099f9185ce7846aa5b8d3acdef6114ac6b16647a0"
---

# The Window Between Discovery and Exploit Is Closing

### Key Takeaways


- The rapid growth in vulnerabilities and AI-enabled discovery is shrinking the time between vulnerability disclosure and active exploitation.
- Regular, disciplined patching cycles help reduce overall exposure and prepare for new CVEs.
- Recovery is essential for resilience, but it must be paired with timely patching to remediate vulnerabilities.
- Organizations should use AI to accelerate vulnerability detection and remediation rather than allowing issues to accumulate in backlogs.
- Vendors who play a critical role provide fast, transparent vulnerability disclosures and clear remediation guidance for customers.


Last year,[industry reporting](https://www.cve.org/about/Metrics) put annual CVE volume in the tens of thousands, with NIST later noting record CVE growth and a[263% increase in submissions](https://www.nist.gov/news-events/news/2026/04/nist-updates-nvd-operations-address-record-cve-growth) between 2020 and 2025.


I hear what that does to a security team in real time, because I am on the calls when it happens. The old questions – what is our exposure, and how fast can we close it? – used to have room to breathe. Now they arrive faster than most teams can staff for them.


Most organizations have vulnerability response processes. Fewer have processes designed for this speed.


For years, the industry organized its response around individual vulnerabilities. A CVE would publish, severity scores would follow, enrichment would catch up, and teams would triage with some margin for judgment. That rhythm assumed a human pace of discovery, but that assumption no longer holds.


That volume is already outrunning the infrastructure built to track it. NIST has said the National Vulnerability Database is moving to a risk-based enrichment model because CVE submissions have grown faster than the program can fully process them.


AI is likely compounding pressure by helping threat actors exploit vulnerabilities, and defenders identify and validate vulnerabilities faster than legacy cataloging workflows can absorb. The window between when a vulnerability is discovered and when it is exploited is closing, and working exploit code can appear before a patch is widely deployed.


That breaks the old model. Structured vulnerability management still matters, but many programs are calibrated for a slower era: gather signal, rank risk, assign owners, then remediate. When discovery accelerates this sharply, even disciplined teams fall behind because the operating model cannot absorb the volume fast enough.


So, the unit of work must change. It no longer matters whether you patched a specific vulnerability but whether your organization can apply, verify, and recover at the speed the threat environment now demands.


### Patch on a Clock


Start with cadence. The most resilient operations I see have stopped treating patching as an interruption and started treating it as routine maintenance: scheduled weekly, visibly owned, and measured like any other operational commitment.


A predictable cadence helps shrink the standing window of exposure across the estate and remove the panic premium from any single disclosure. When patching happens every week, organizations are prepared for CVEs.


Cadence does not mean treating everything alike. A vulnerability under active exploitation, the kind that lands in[CISA’s Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) , still earns an immediate, out-of-band response. The weekly schedule handles the flood as routine, so true emergencies receive proper attention instead of competing with noise.


### Close the Vulnerability, Not Just the Gap


Here is the part recovery cannot fix by itself. If a vulnerability puts an asset at risk, restoring that asset without closing the vulnerability just resets the clock. The vulnerability is still there, waiting for the next attempt. Recovery matters, but it is not a substitute for closing the hole that let the threat actor in.


That means the real work needs to happen earlier, at the point where vulnerabilities are found and fixed. AI is changing that math on both sides. The same models that help an threat actor spot an exploitation can help a vendor find it first.


Commvault Engineering runs AI against our own codebase to scan for vulnerabilities before they ship, and we apply AI to help resolve what we find instead of routing it into a backlog. A vulnerability that sits in queue for weeks because a team ran out of bandwidth is still a vulnerability. Speed to detection means nothing without speed to resolution.


### Ask More of Your Vendors


When the window between discovery and exploit is measured in hours, customers cannot afford to learn about a vulnerability in their vendor’s product from a third party.


They need to hear it from the vendor, early, in plain language, with a direct answer to “Am I affected?” and “What do I do first?” Ask every critical vendor how quickly they disclose, how they notify affected customers, what evidence they provide for remediation, and how customers can validate that the exposure is closed. Vulnerability transparency is part of resilience.


The frontier AI era will not be won by whoever ships the fewest vulnerabilities. Every serious software company will disclose more. The advantage goes to whoever treats patching as a standing discipline and treats recovery as the discipline that makes a missed window survivable.


### FAQs


**Q: Why is the window between vulnerability discovery and exploitation getting shorter?**
**A:** AI is likely compounding pressure by helping threat actors exploit vulnerabilities, and defenders identify and validate vulnerabilities faster than legacy cataloging workflows can absorb. As a result, exploit code can become available before many organizations have had time to deploy patches.


**Q: Why are weekly patching cycles becoming more important?**
**A:** A consistent weekly patching schedule helps reduce the organization’s exposure to known vulnerabilities. It also allows security teams to focus immediate attention on actively exploited threats and prepare new CVEs.


**Q: Is disaster recovery enough to protect against cyberattacks?**
**A:** No. Recovery helps organizations restore operations after an incident, but restoring systems without addressing the underlying vulnerability leaves them exposed to future attacks. Effective resilience requires both rapid remediation and reliable recovery.


**Q: How can AI help improve vulnerability management?**
**A:** AI can help identify vulnerabilities earlier, prioritize remediation efforts, and accelerate the resolution process. This helps security and engineering teams respond more quickly instead of allowing vulnerabilities to remain unresolved in lengthy backlogs.


**Q: What should organizations ask their software vendors about vulnerability management?**
**A:** Organizations should ask how quickly vendors disclose vulnerabilities, how affected customers are notified, what remediation guidance is provided, and how customers can verify that the issue has been fully addressed. Transparent communication is an important part of cyber resilience.


[Rajiv Kottomtharayil](https://www.linkedin.com/in/rajiv-k-a8a123211/) *is Chief Products Officer at Commvault.*
