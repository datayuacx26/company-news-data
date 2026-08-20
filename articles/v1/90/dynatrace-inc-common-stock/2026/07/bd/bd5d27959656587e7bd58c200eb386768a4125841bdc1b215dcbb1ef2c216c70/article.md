---
schema_version: "1.0.0"
document_id: "bd5d27959656587e7bd58c200eb386768a4125841bdc1b215dcbb1ef2c216c70"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/malicious-package-detection-with-runtime-vulnerability-analytics/"
published_at: "2026-07-31T18:40:19+00:00"
first_seen_at: "2026-08-01T01:30:35.056392+00:00"
fetched_at: "2026-08-01T01:30:36.549567+00:00"
content_hash: "sha256:9469cb3101d7eec2028088c878d6e12be81dfbede579db0732480b32ae828479"
---

# Malicious package detection with Runtime Vulnerability Analytics: Identify threats at runtime

Dynatrace malicious package detection identifies intentionally harmful software dependencies at runtime, supporting teams to detect and remove active attacks.


Software supply-chain attacks have moved from edge case to everyday reality. Attackers are no longer content to wait for a vulnerability they can exploit in one of your dependencies — increasingly, they publish packages that are malicious by design and rely on unsuspecting developers to pull them in and run them. By extending coverage of malicious package detection to the Vulnerability Feed, Dynatrace Runtime Vulnerability Analytics (RVA) helps you prioritize and act on threats based on what is actually running in your environment.


## A vulnerability is a risk. A malicious package is an attack.


It’s worth being precise about the difference between a **vulnerability** and an **attack** , because the distinction changes how you respond.


A vulnerability is a weakness. It only becomes a real threat when an attacker actively exploits it — which means there’s often time to react. On the other hand, the moment a malicious package is installed and its code runs, it does whatever the attacker has instructed it to do: steal credentials, open a backdoor, exfiltrate data, or hijack computing resources. There is no weakness to exploit — running the package is the exploit.


This distinction matters for prioritization. A malicious package in your environment isn’t a risk to assess — it’s an active threat that must be removed.


### How a malicious package differs from a vulnerability


**Vulnerability** **Malicious package**


**Description** Software weakness Intentional attack


**Trigger** Requires exploitation Executes immediately


**Risk type** Potential risk Active threat


**Required r** **esponse** Assess and prioritize Remove immediately


## How does Dynatrace detect malicious packages?


Dynatrace is expanding its coverage of malicious package records in the Dynatrace Vulnerability Feed. Alongside your existing vulnerability data, curated data from[OSV.dev](https://osv.dev/) and the[OpenSSF Malicious Packages](https://github.com/ossf/malicious-packages) project are also integrated into the analysis.


Package coverage spans the same six programming ecosystems covered by the Dynatrace vulnerability feed:


- Java
- JavaScript
- Python
- Go
- .NET
- PHP


## What information does Dynatrace provide for malicious packages?


Every malicious package record is built to fit naturally into the workflows you already use:


- **Clear identification** — titles always begin with **Malicious code** so you can tell at a glance whether you’re looking at a known vulnerability or a malicious component.
- **Standard classification** — each record carries the string **CWE-506 (Embedded Malicious Code),** the standard category for software containing intentionally malicious code.
- **High-priority scoring** — malicious packages are treated as critical-severity with a CVSS 4.0 score of 9.3 by default, so they rise to the top of your list. The CVSS 4.0 vector is designed to most closely reflect the behavior of the malicious component.
- **Consistent structure** — records follow the same format as vulnerability records, enabling reuse of:


- **Dashboards**
- **Filters**
- **Remediation workflows**


## Runtime context is the real differentiator for malicious package detection


Knowing a malicious package exists somewhere in a registry is useful. Knowing it’s running in your environment right now is what allows you to act with confidence.


By expanding coverage of malicious packages in RVA, you don’t just get another threat list — you get malicious packages evaluated against what’s actually loaded and executing in your applications (Figure 1).


Figure 1. Malicious package detection in the Dynatrace Vulnerabilities app


That means your teams can cut through the noise and focus on genuine exposure rather than working through theoretical inventories, and they can triage and remediate in the same place where they already manage vulnerabilities.


## What you gain with malicious package detection


- **Detect active threats, not just risks** — close a coverage gap left by traditional vulnerability scanning, which focuses only on weaknesses.
- **Use a single, unified view** — malicious packages and vulnerabilities live side by side, with no additional tools or processes required.
- **Prioritize based on runtime** — focus remediation effort on the components actually running in production.
- **Gain broad ecosystem coverage** — protect applications across the six most widely used language ecosystems out of the box.
- **Rely on trusted, publicly-verifiable data** — leverage transparent intelligence from OSV.dev rather than a closed, proprietary list.
- **Require no additional setup** — malicious package detection is available out of the box; no configuration required.


## Availability of malicious package detection


Malicious package detection is delivered as part of the Dynatrace Vulnerability Feed and appears automatically in the[Vulnerabilities app](https://www.dynatrace.com/hub/detail/vulnerabilities/) — no additional configuration required.


- **If you’re on Dynatrace SaaS** , this change will happen automatically when version 1.343 is rolled out in July 2026.
- **If you’re on Dynatrace Managed** , an update to version 1.343 (or later) is required to use the expanded malicious package detection.


## Closing the supply chain vulnerability gap


Software supply-chain attacks increasingly rely on malicious packages published directly to public repositories. These attacks exploit the time gap between when a package is published and when it is detected.


With malicious package detection in Runtime Vulnerability Analytics, that vulnerability gap closes.


You no longer rely on discovering threats after the fact. Instead, malicious components are surfaced exactly where your team is already looking — within runtime context — supporting faster remediation decisions for identified malicious packages.


## Not a Dynatrace customer yet?


**Try the Dynatrace malicious package detection in our Playground** and see how threat-aware, curated vulnerability intelligence helps you cut through noise and focus on real risk.


Explore the product hands-on in a live environment, and discover how Dynatrace can accelerate vulnerability prioritization and remediation across your stack — **start in the Playground today.**


[Explore on the Dynatrace Playground](https://www.dynatrace.com/signup/playground/application-security/)
