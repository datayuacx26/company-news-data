---
schema_version: "1.0.0"
document_id: "ac9804a55f224bea9f3e7f7133ef6a6dcbb88ee0ed4d9d0259ed5a2e71b5f370"
company_key: "yc-anjuna"
company: "Anjuna"
source_id: "yc-anjuna-news-import-d3424cd26fcb"
canonical_url: "https://www.anjuna.io/blog/cloud-is-sovereign-until-it-isnt"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-08-07T03:10:09.947425+00:00"
fetched_at: "2026-08-07T03:10:10.572267+00:00"
content_hash: "sha256:a8e7b592da3c9c4534111cbae44f0cfd5f8678ac3366233887d59b56dcafc7a7"
---

# A Cloud Is Sovereign, Until It Isn't

*What sovereignty entails in the age of Mythos*


**TL;DR** Sovereign cloud has always rested on a bet: that the gap between a vulnerability being found and being patched is wide enough for defenders to act inside it. Frontier models are closing that gap to hours, sometimes minutes, with a trend towards zero. The recent wave of SaaS-to-SaaS breaches shows attackers no longer need to break a cloud open. They walk in through a trusted integration and then pivot. Software-only controls, built on the assumption of a patchable world, cannot be retrofitted to a world where the patch window is the attack window. Sovereignty now has to start in the silicon, not in a policy document.


## What *sovereignty* actually means here


Two different claims get bundled under "sovereign cloud":


- **Jurisdictional sovereignty** :your data stays in a given legal territory, subject to that territory's courts and regulators, not a foreign government's subpoena power. Physical operations are with geographically aligned staff and systems related to the jurisdiction in question.
- **Operational sovereignty** : no party outside your control, including the cloud provider's own administrators, can read or alter your data or workload, regardless of where it sits.


The discussion in this blog is about the second one.


Residency policy can satisfy the first claim with contracts and geography. It cannot satisfy the second, because the second is a claim about who can technically access the system, and that has always been enforced in software such as identity and access management (IAM), encryption keys, and network boundaries, all of which run on infrastructure the cloud operator, and by extension anyone who compromises that infrastructure, can see. That gap between the sovereignty a vendor promises and the sovereignty a software stack can actually enforce is the subject of the following discussion.


## The bet we've been making


Every operational sovereignty claim made by providers — customer-managed keys, "your data never leaves your tenant," "we can't see your data" — is built on a trust model with four unstated assumptions:


- The cloud operator's insiders won't misuse access.
- Attackers who get in will be slower than the vendor's ability to patch.
- The supply chain of integrations, agents, and third-party services is a minor attack surface.
- A software boundary (IAM, encryption-at-rest, network segmentation) is sufficient to enforce the promise.


None of these assumptions were ever fully true. They were true enough in the past for a period of time that the economics of software-only security worked. That arithmetic is now breaking, and the recent record shows it breaking in public.


## Three breaches, one pattern


The following cases in the last two years produced a clear pattern. Different root causes, same shape: a trusted credential or integration became the path across a boundary that customers assumed was sealed.


*Figure 1: The model-accelerated kill chain, from vulnerability discovery to cross-cloud exfiltration, annotated with the three cases above.*


‍


The common failure isn't a missing patch. It's that a software-enforced trust boundary, once one side of it is trusted implicitly, offers no second line of defense. Once the token, the session, or the integration is trusted, everything reachable from that trust point is exposed. Lateral movement isn't a bonus objective for attackers anymore. In a hyperconnected SaaS mesh, it's the default outcome of any single foothold.


It is worth being precise about what kind of failure each of these cases experienced, because it matters for how to fix them. Snowflake and Salesloft Drift were **identity failures** : a valid credential or token made the attacker indistinguishable from an authorized caller, and the application did exactly what it was designed to do — decrypt and serve data to an authenticated request. No infrastructure control, hardware or otherwise, stops an application from honoring a token it was built to trust. Vercel–Context.ai is different: the exposure was secrets sitting in plaintext at rest, readable by anyone with internal platform access, which is a **data-handling design choice** , not an authentication failure. That distinction is the hinge for the rest of this discussion. We believe that hardware trust boundaries address the second kind of failure directly, and only address the first kind if the architecture around identity changes too.


## Mythos changes the clock


These failure patterns predate frontier models. What changes now is the speed at which the first foothold gets created through vulnerability discovery, not just credential theft.


For most of the last decade, the vulnerability lifecycle looked like this: a researcher spends two to six months auditing a target, builds a proof of concept, and reports it. Vendors get real lead time to patch before public disclosure. That lead time is the entire premise of coordinated disclosure and of "patch within 30/60/90 days" service level agreements (SLAs).


Anthropic's own Frontier Red Team and independent researchers have been documenting the collapse of that lead time through 2026:


- “[Anthropic is finding bugs faster than Microsoft can fix them](https://arstechnica.com/security/2026/07/anthropic-is-finding-bugs-faster-than-microsoft-can-fix-them/) ” reveals how Mythos and models like it have fundamentally shifted the risk balance in the attackers favor by exposing more vulnerabilities than can be timely patched.
- Palo Alto Networks disclosed 26 CVEs in a single Patch Wednesday cycle, roughly five times its usual volume, after testing frontier models including Claude Mythos, Claude Opus, and GPT-5.5-Cyber against its own codebase.
- Google's Threat Intelligence Group confirmed in May 2026 the first publicly documented case of a cybercrime group using an AI model to discover and weaponize a zero-day exploit — a two-factor authentication (2FA) bypass rooted in a hardcoded trust assumption, the kind of contextual logic flaw that fuzzers routinely miss and reasoning models routinely catch.
- Project Glasswing partners including AWS, Apple, Cisco, CrowdStrike, Google, Microsoft, NVIDIA, and Palo Alto Networks have used Mythos to scan critical software and found high-severity bugs across every major operating system and browser.
- CrowdStrike's 2026 Global Threat Report puts numbers on the attacker side of the ledger: an 89% year-over-year rise in AI-enabled adversary operations, a 42% increase in zero-days exploited before public disclosure, and an average eCrime breakout time of 29 minutes, which is down from 48 minutes the year prior, with the fastest observed breakout at 27 seconds and exfiltration beginning within 4 minutes of initial access in one documented intrusion.
- Earlier academic work from the University of Illinois Urbana-Champaign (UIUC) had already shown Chat GPT-4 could autonomously exploit 87% of one-day vulnerabilities from a public common vulnerabilities and exposure (CVE) description alone. Frontier reasoning models extend that from patched CVEs to undisclosed ones.


‍


*Figure 2: The zero-day clock: attacker discovery-to-exploit time compressing toward minutes while vendor and operator patch cycles remain measured in weeks. (Source: zerodayclock.com)*


‍


The defensive framing matters here and it deserves fair treatment: this same capability is disclosing and fixing far more vulnerabilities than it's creating exploits for, and every major AI lab and most named partners above are using it to find and patch bugs before criminals do. That is real, and it is good. But it does not change the structural problem for any single enterprise sitting downstream of a vendor's patch calendar: the discovery side of the clock is now running at machine speed, and the response side — vendor patch build, change control, staged rollout, validation — is still running at organizational speed. The gap between the two arcs is the exposure window, and it is not shrinking to match.


Lastly, and few are talking about it, the sheer volume of patches is spiking dramatically as AI finds more vulnerabilities, and vendors race to update what they can. This causes another problem for CISOs and CTOs: how to test and apply an increasingly high-dimensional matrix of interrelated updates that may interact, change system behavior, and impact compliance outcomes. It's not just releasing patches. They have to be applied without operational side effects.


## Why the patch model breaks structurally, not just slowly


The old model tolerated misalignment because the misalignment was small and roughly constant: weeks of vendor lead time against a threat that took months to develop. Frontier-model-assisted discovery does two things that a "just patch faster" response can't absorb:


- **It multiplies in volume:** Twenty-six CVEs in a cycle that used to produce five isn't five times the work. It's a queue that breaks triage, change-control, and regression-testing capacity simultaneously across every team drawing from the same patch calendar.
- **It shrinks lead time toward zero:** When discovery-to-exploit collapses from months to hours, coordinated disclosure loses its central assumption: that vendors get a meaningful headstart. Some fraction of these bugs will always be found first by an adversary, not a partner, and used before a patch exists - a true zero-day in the original sense, at a scale and cadence the ecosystem has never had to absorb.


This is the argument for a different foundation, not a faster patch pipeline: when the patching window cannot match the vulnerability window reliably, the security model cannot trust the software layer it depends on by the time an attacker gets to it. It has to assume compromise of the OS, the hypervisor, and the cloud operator's own administrative access, and hold the line anyway.


That can't be bolted on after the fact. A hardware trust boundary changes what the application, storage, and network layers are built to assume from day one: where keys live, what an administrator can see, what an attacker with root or hypervisor access can actually reach. Retrofitting that assumption into a system architected around software-only trust means re-architecting the system, not patching it.


## New trust anchors: confidential computing as the hardware floor


This is where the trust model has to move: from "we control the software stack, so we control the data" to "the hardware enforces the boundary regardless of who controls the software stack."


**Confidential computing** — the hardware-based trusted execution environments ( *Intel TDX, AMD SEV-SNP, AWS Nitro Enclaves* , etc.) with cryptographic attestation — is the mechanism that does this today. The technology provides the following fixes:


- **Insider and administrative risk:** data and code are encrypted in-use, not just at-rest and in-transit. A cloud provider's own administrators, any process outside the enclave, or a compromised hypervisor cannot read memory contents. This is the direct fix for the Vercel–Context.ai failure mode: if the secrets had only ever existed in plaintext inside an attested enclave, internal platform access wouldn't have been sufficient to read them.
- **Host and co-tenant compromise:** attestation lets a workload cryptographically prove what code is running and that the environment hasn't been tampered with, before it's trusted with keys or data. Root on the host, via an OS or hypervisor zero-day, stops being equivalent to root on the data.
- **What it does not fix on its own:** confidential computing does not stop Snowflake or Salesloft Drift as they actually happened. Those were valid-credential attacks, and a TEE will correctly decrypt data for a request it's designed to trust. That's the application working as intended, not a hardware failure. Closing that gap needs the identity layer to change alongside the hardware layer by binding key or sensitive data release to attested workload identity rather than network identity or a long-lived OAuth token.sThis way, even a stolen token only reaches what that specific attested workload's policy allows, not everything the token's scope nominally permits.


What about the TEE itself? Below are the honest limits of confidential computing, stated plainly, for consideration in your security model:


- TEEs are not bug-free. SGX and SEV-SNP have both had disclosed side-channel and microcode vulnerabilities that required firmware patches. The claim isn't that hardware is immune to zero-days. It's that the trusted computing base shrinks from an entire OS and hypervisor to a much smaller, more auditable boundary, which is easier to keep current, and harder to reach in the first place.
- The engineering work is not “enable confidential computing” as a checkbox. It's building the attestation verification pipeline such as who verifies the attestation, against what known-good measurements, how often those measurements are rotated, etc. and the key-release policy that decides what an attested workload is actually allowed to unlock. Skipping that and treating confidential computing as a flag you flip is how organizations end up with a hardware boundary that's real but unused, because nothing upstream is checking it.
- This sits underneath IAM, network segmentation, and patch programs, but it doesn't replace them. Identity hygiene (short-lived tokens, scoped OAuth grants, MFA everywhere) still does the work confidential computing structurally cannot.


## The time is now, not after the next breach


Sovereign cloud (residency plus encryption-at-rest plus a customer-managed key and network controls) as a checkbox was built for a threat model where the exploitable gap was measured in months. That gap is closing toward zero for a meaningful share of vulnerabilities, and the SaaS breach record of the last two years shows attackers already don't need a zero-day at all when a trusted OAuth token will do. Both paths lead to the same place: a trust boundary enforced only in software is a boundary that holds until someone finds the one flaw, human or machine, that lets them walk around it.


Confidential computing is the foundational layer that makes sovereignty a hardware-verifiable property instead of a contractual promise. Building it now into your architecture while it is still being designed for AI-era workloads is materially cheaper — and entirely practical — and more defensible than retrofitting it after the fact, which, in this threat model, may not leave much time at all.


Building sovereignty into the hardware layer doesn't have to wait for the next breach.[Start a free 30-day trial of Anjuna Seaglass](https://www.anjuna.io/anjuna-free-trial) on AWS, Azure, or Google Cloud, and see what a hardware-verifiable trust boundary looks like in practice.


### Sources


- CrowdStrike, 2026 Global Threat Report
- Google Threat Intelligence Group, May 2026
- Palo Alto Networks / Unit 42 — frontier model vulnerability discovery findings and Patch Wednesday CVE volume; Salesloft Drift breach analysis
- Anthropic, Frontier Red Team, "Evaluating and mitigating the growing risk of LLM-discovered 0-days," Feb 2026
- WTW, AppOmni, Token Security, Anomali — Salesloft Drift / Salesforce (UNC6395) breach reporting, 2025-2026
- Trend Micro, Cloud Security Alliance Labs — Vercel–Context.ai OAuth supply chain breach, April 2026
- University of Illinois Urbana-Champaign research on GPT-4 autonomous exploitation of one-day vulnerabilities
