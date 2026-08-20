---
schema_version: "1.0.0"
document_id: "18cfe272f21f9f73407052977a50eaf114046b02065651163455456c6448784f"
company_key: "qualys-inc-common-stock"
company: "Qualys Inc."
source_id: "qualys-inc-common-stock-rss-b23fdbdd1cee"
canonical_url: "https://blog.qualys.com/product-tech/2026/08/03/truconfirm-cloud-agent-endpoint-exploit-validation"
published_at: "2026-08-03T15:45:19+00:00"
first_seen_at: "2026-08-03T17:28:27.184067+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:d79850f9a560234b7d3a4abdd87e69cd2ef718985ff5fdc6197c41b72f17f884"
---

# Agent Val Now Validates the Entire Attack Surface: From Network to Host

#### Table of Contents


- The CVE Classes That Live Beyond the Networks Reach
- One TruConfirm, Two Engines, Complete Coverage
- How Cloud Agent-Based TruConfirm Works
- ETM, TruRisk, and Agent Val: Evidence-Driven Risk Operations
- What This Means for Security Teams
- From Possibility to Proof Across the Entire Attack Surface
- FAQ


### Powered by TruConfirm — Exploit Validation That Now Runs on the Network and the Host


---


#### Executive Summary


*Qualys TruConfirm now validates exploitability across the entire attack surface, not just the network. Cloud Agent-Based TruConfirm brings the same proof-based validation model to the endpoint, closing the gap on local, kernel, browser, and post-authentication CVEs that network scanning alone could never reach.*


---


AI-assisted vulnerability research has compressed the time between disclosure and active exploitation, leaving security teams with more findings than they can possibly investigate or remediate. The core question is no longer, “What is vulnerable?” Rather, it is, “Which of these findings are actually exploitable in my environment, against my controls, right now?”


That’s the question[Qualys TruConfirm](https://www.qualys.com/apps/truconfirm) was built to answer. Instead of asking whether a vulnerability could be exploitable, TruConfirm proves whether it has been. Since we[launched about a year ago](https://blog.qualys.com/product-tech/2025/10/15/introducing-truconfirm-for-enterprise-trurisk-management-automated-exposure-validation) , scanner-based TruConfirm has done exactly that at scale, safely validating exploitability in enterprise environments across:


- Coverage for more than **1,800** CVEs
- **8 million+** validations
- **100s** of enterprise organizations


With coverage expanding continuously as the[Qualys Threat Research Unit](https://www.qualys.com/tru) adds new payloads.


---


> “ *Every validation method has a vantage point. The network was only one of them — the endpoint was always the other half of the attack path.* “
>
>
> -Himanshu Kathpal, Vice President, Product Management, Platform and Technologies


---


Every validation method has a vantage point, though, and the network is only one of them. A meaningful share of the CVEs security teams worry about most don’t live on the network. They live on the endpoint. That foundation is not changing. Scanners remain a first-class validation engine and a core strength of[Enterprise TruRisk™ Management (ETM)](https://www.qualys.com/apps/enterprise-trurisk-management) . What is changing is the breadth of attack-surface coverage. TruConfirm now extends exploit validation beyond network-exposed assets to endpoint-resident vulnerabilities that traditionally could not be validated remotely.


Now, with this launch, we’re giving TruConfirm a second vantage point. **Cloud Agent-Based TruConfirm** brings the same proof-based validation model directly to the hosts, delivered through the Qualys Cloud Agent that’s already running across our customers’ environments. **** Cloud Agent-Based TruConfirm delivers host-level exploit validation for local, kernel, browser, and client-side CVEs that network-based validation cannot reach. This allows teams to now validate **the entire attack path, from entry to impact.**


## The CVE Classes That Live Beyond the Network’s Reach


Scanner-based TruConfirm earned its place as one of the most trusted capabilities inside Qualys Enterprise TruRisk Management (ETM). It excels at validating remotely exploitable vulnerabilities in networking hardware, perimeter services, and enterprise platforms that are reachable over the wire. That remains a core pillar of the product.


Some of the most damaging CVE classes, however, live deeper inside the attack path and structurally require local execution to validate. These include:


- **Local privilege escalation chains** that require an authenticated session on the host itself, working through kernel pools, driver IOCTLs, and privilege-token chains.
- **Browser and client application exploits** , including V8 type confusion, use-after-free bugs, and Office macro chains, that execute inside a process context that only exists on the endpoint.
- **Linux and Unix kernel-level vulnerabilities** that require local kernel execution to validate at all.
- **Post-authentication and insider-exploit classes** that need a privileged local session that a network scanner simply does not hold.
- **Blind local remote code execution exploits** that produce no network-observable signal. The only way to confirm it ran is a callback from inside the endpoint.


Without endpoint-level proof, these vulnerabilities often sit in the same queue as every other critical-rated CVE. Patch teams see identical scores yet have no evidence that distinguishes issues that can actually be executed from those that cannot. Effort goes toward risk that doesn’t exist in practice, while the CVEs that do matter wait their turn. And as AI-assisted vulnerability research accelerates the discovery of exactly these endpoint-resident classes, that visibility gap keeps widening for anyone without a way to validate on the endpoint itself. **Cloud Agent-Based TruConfirm** is built to close that validation gap. It brings proof-based validation into the endpoint itself and extends TruConfirm coverage from external-facing assets to the full organizational attack surface.


## One TruConfirm, Two Engines, Complete Coverage


In this evolution,[TruConfirm](https://www.qualys.com/apps/truconfirm) operates through two complementary engines that work as one capability.


- **Scanner-led TruConfirm** validates remote- and network-exposed vulnerabilities from the network perspective.
- **Cloud Agent-Based TruConfirm** validates endpoint-local and post-authentication vulnerabilities from the host perspective.


Because it leverages the Cloud Agent already deployed across our customers’ servers, laptops, cloud instances, and containers, there’s no new infrastructure to stand up, no credentials in flight, and nothing additional to install. Validation logic is delivered as a signed manifest, and the agent checks in on a regular cycle, roughly every 15 minutes, to pick up new TruConfirm assessments as they become available. Scanner-based TruConfirm continues to handle everything it does today: remote- and network-exposed CVEs, validated the same way, at the same scale. The endpoint side simply picks up where the network’s reach ends.


## How Cloud Agent-Based TruConfirm Works


Cloud Agent-Based TruConfirm runs the same three-stage workflow that defines TruConfirm’s safety and rigor. It simply executes that workflow on the endpoint where local exploits occur.


- **Step 1: Environment assessment.** Before anything else happens, the agent profiles the host: OS and kernel version, loaded drivers, running services, installed software and browser versions, patch status, user privilege context, and whether compensating controls like EDR or EPP are present and active. Validation proceeds only when conditions indicate that exploitation is actually feasible on that specific asset.
- **Step 2: Safe proof-of-concept execution.** A purpose-built, benign proof-of-concept runs locally to test whether a vulnerability can actually be exploited on that endpoint. No shellcode executes, no sensitive data is touched, and no persistent change is made to the system. This mirrors the safety discipline scanner-based TruConfirm has always applied, adapted for on-device execution.
- **Step 3: Deterministic proof generation.** The agent captures hard evidence, such as privilege-escalation outcomes, process lineage changes, kernel-state modifications, or controlled DNS and HTTP callbacks. A finding is only marked exploitable when observable proof backs it up. No assumptions, no probability scores, just evidence.


**Validated findings then flow directly into ETM.** Exploit-confirmed vulnerabilities rise in TruRisk priority, remediation queues reorder automatically, and dashboards reflect what’s actually exploitable rather than what a severity score assumed.


## ETM, TruRisk, and Agent Val: Evidence-Driven Risk Operations


Because TruConfirm is native to[Enterprise TruRisk Management](https://www.qualys.com/apps/enterprise-trurisk-management) , every validation signal flows directly into your risk operations. Exposure data from Qualys scanners and third-party sources is ingested into ETM. TruRisk applies threat intelligence and business context to build a prioritized view of risk.


TruConfirm then validates which of those exposures are exploitable on the network and on the endpoint, or confirms that existing controls are already blocking those paths. The result is a remediation queue ordered by confirmed exploitability rather than by severity alone.


**[Agent Val](https://blog.qualys.com/product-tech/2026/03/23/meet-agent-val-closing-the-validation-gap-in-exposure-management-at-machine-speed-with-agentic-ai)** , the agentic AI for safe exploit validation, amplifies this workflow. It continuously analyzes your environment to identify high-risk and trending CVEs. It drives TruConfirm assessments to the right assets. It orchestrates remediation on validated findings and then revalidates the same exploit path to prove it is closed. The loop becomes detect, validate, remediate, and revalidate, all grounded in live exploit evidence.


---


> **For leadership and boards, this translates into a measurable outcome** . Ten thousand critical findings compress into a few hundred validated exposures. Those exposures have been proven exploitable on specific assets in your environment against your controls. And you can show which of those exposures have been closed with proof.


---


## What This Means for Security Teams


- **Compensating controls finally get verified before an attacker tests them.** Organizations invest heavily in EDR, EPP, and other host-level controls, but historically only found out whether those controls actually held during an incident. The environment assessment stage surfaces what’s present and active ahead of time.
- **Endpoint-class CVEs stop being a standing blind spot.** Local privilege escalation, kernel exploits, browser engine bugs, and insider-access paths now have a validation route that didn’t exist before.
- **Audit and board reporting moves from counts to evidence.** Severity ratings and dashboard totals are no longer enough for risk conversations at the executive level. This endpoint-level validation produces auditable findings that show whether a vulnerability was actually exploitable in your environment, not just theoretically possible.
- **Patch backlogs stop outpacing team capacity.** As disclosure volume climbs, manually investigating every finding doesn’t scale. Non-exploitable CVEs are automatically ruled out, and confirmed exploits move to the top of the queue without a human having to triage them.
- **Network and endpoint validation are unified** . Security teams gain a single evidence-driven workflow for exploit validation regardless of where the vulnerability exists or how exploitation occurs.


What Complete Coverage Looks Like


- **1,800+ CVEs, 8 Million+ Validations:** Scanner-based TruConfirm has already proven exploitability at this scale across hundreds of enterprise environments — and that foundation doesn’t change.
- **From 10,000 to a Few Hundred:** Ten thousand critical findings compress into a few hundred validated exposures, proven exploitable on specific assets against your specific controls.
- **Zero New Infrastructure:** Cloud Agent-Based TruConfirm runs on the agent you’ve already deployed — no new install, no credentials in flight, checking in every 15 minutes.
- **Entry to Impact, Fully Covered:** Network and host, perimeter and endpoint — the entire attack path is now validated by one system.”


## From Possibility to Proof Across the Entire Attack Surface


Cloud Agent-Based TruConfirm does not replace scanner-based validation. It completes it. Together, they give you exploit validation for the entire attack surface. External facing assets. Internal endpoints. Network entry points and endpoint impact.


In an era where vulnerability discovery is accelerating, and attackers move faster than patch cycles, it is no longer enough to know what is vulnerable. You need to know what is exploitable in your environment right now. TruConfirm, powered by scanners, the Cloud Agent, and Agent Val, is built to answer that question with confidence.


### See TruConfirm in Action


Request a live demo and watch Cloud Agent-Based TruConfirm prove exploitability on a real endpoint—from environment check to callback evidence.


---


**Be among the first to validate the entire attack surface, network to host.**


[Request Demo](https://www.qualys.com/demo/enterprise-trurisk-management)


---


## **FAQ**


### **Q: What is Cloud Agent-Based TruConfirm?**


A: It is the extension of Qualys TruConfirm that runs on the Qualys Cloud Agent already deployed on hosts. It safely validates the exploitability of local, kernel, browser, and client-side CVEs that network-based validation cannot assess.


### **Q: Does this replace scanner-based TruConfirm?**


A: No. Scanner-based TruConfirm continues to validate remote and network-exposed vulnerabilities. Cloud Agent-Based TruConfirm adds the endpoint vantage point. Together they cover the full attack surface.


### **Q: What types of vulnerabilities can now be validated on the host?**


A: Local privilege escalation chains, browser and client application exploits, Linux/Unix kernel vulnerabilities, post-authentication paths, and blind local remote code execution exploits that produce no network-observable signal.
