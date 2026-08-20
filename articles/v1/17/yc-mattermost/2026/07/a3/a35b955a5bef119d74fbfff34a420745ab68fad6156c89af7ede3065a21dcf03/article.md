---
schema_version: "1.0.0"
document_id: "a35b955a5bef119d74fbfff34a420745ab68fad6156c89af7ede3065a21dcf03"
company_key: "yc-mattermost"
company: "Mattermost"
source_id: "yc-mattermost-rss-3d807a20d23e"
canonical_url: "https://mattermost.com/blog/zero-trust-stops-at-the-login-screen-adversaries-know-it/"
published_at: "2026-07-03T13:00:00+00:00"
first_seen_at: "2026-07-20T23:24:03.492829+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:70603703d1a8ef849959c09f95dfcaa24e528eb292887d5df80c5fb06dab05f7"
---

# Zero Trust Stops at the Login Screen & Adversaries Know It

## Key Takeaways


- Zero Trust frameworks authenticate identity at login but rarely re-verify device posture, role, or risk signals once a collaboration session is underway.
- Neither the CISA Zero Trust Maturity Model nor the DoD Zero Trust Strategy explicitly names the collaboration layer as an enforcement surface.
- Policy drift accumulates quietly: departed contractors, role changes, and non-compliant devices can retain access long after conditions change.
- The Continuous Access Evaluation Profile (CAEP), finalized in 2025, defines how identity providers and collaboration platforms can share real-time session signals to close this gap.
- Most collaboration audit logs capture what was accessed but not the device, network, or policy context under which access occurred.


Most organizations building Zero Trust programs operate on a reasonable assumption that once identity is verified, endpoints are managed, and network access is controlled, the architecture is working. But that assumption ignores where a lot of operational work actually happens.


Employees are not making decisions in identity providers. They are not sharing sensitive files through network segmentation policies. They are doing all of it in messaging platforms and collaboration tools that most Zero Trust architectures treat as endpoints to be authenticated rather than enforcement surfaces to be governed. That distinction represents a significant and largely unaddressed security gap.


## Why Zero Trust Stops at Login: **Authentication Is Not Enforcement**


National Institute of Standards and Technology (NIST)[Special Publication 800-207](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf) establishes a foundational requirement: access to enterprise resources must be granted per session, evaluated continuously, and never assumed based on prior authentication. The standard is explicit that trust is not a state that persists but, rather, a condition that must be continuously verified. Collaboration platforms, by design, challenge that principle.


The operational pattern is familiar, with a user authenticating, satisfying MFA, passing through organizational identity controls, and entering an active session. That session then persists — often for hours, sometimes longer — without any reevaluation of the conditions under which access was granted. Device posture may degrade. The user may move from a managed corporate network to a personal hot spot. Role or access status may change. None of that triggers a policy response inside the collaboration environment, so the access holds and the session continues.


Security architects understand this at an abstract level. The practical problem is that many treat the security of collaboration tools as resolved once SSO and MFA are configured. While those controls establish identity at the point of entry, they do not control what happens after the door opens. Authentication at the door is not the same as enforcement at the desk.


## **Where CISA and DoD Zero Trust Models Fall Short on Collaboration**


The Cybersecurity and Infrastructure Security Agency’s (CISA)[Zero Trust Maturity Model](https://www.cisa.gov/zero-trust-maturity-model) structures enterprise Zero Trust programs across five pillars: Identity, Devices, Networks, Applications and Workloads, and Data.[The Department of Defense (DoD) Zero Trust Strategy](https://dodcio.defense.gov/Portals/0/Documents/Library/DoD-ZTStrategy.pdf) — mandating full implementation across all DoD components and Defense Industrial Base partners by fiscal year 2027 — is paired with the DoD Zero Trust Reference Architecture, which extends that to seven pillars and 152 discrete capabilities.


Unfortunately, neither framework explicitly identifies the collaboration layer as an enforcement surface, meaning that security gap is not formally addressed by these powerful strategic documents.


In practice, most collaboration environments were designed around the simplest organizational constraint: are you an employee? Access is provisioned at onboarding and adjusted through manual processes when roles change — if those processes are triggered at all. The dynamic, runtime policy enforcement that Zero Trust frameworks demand everywhere else in the stack simply does not reach into active collaboration sessions.


- A user whose device falls out of compliance continues participating in sensitive channels.
- Someone placed on administrative leave or who changes teams retains channel membership until an administrator manually removes them.
- A contractor whose engagement has ended keeps access to shared workspaces because no automated process applied that status change to the collaboration environment.


In each case, the identity system may reflect updated status. The collaboration platform does not.


This is natural policy drift, as user’s effective access becomes the accumulation of all access they’ve had to date. Even worse, it accumulates quietly, across hundreds of users and dozens of channels with access and sensitive information flowing to unexpected places. Unless the collaboration layer revalidates access continuously, this is a series of incidents waiting to happen.


## ****The Cost of Unmonitored Collaboration Sessions****


Collaboration environments are where organizations are most exposed — not because they are inherently insecure, but because of their design, what moves through them, and how fast they are.


Operational decision making happens on these channels. Incident response is coordinated there. Collaboration infrastructure regularly carries:


- Personnel and financial data
- Strategic plans
- Technical configurations
- Real-time situational awareness


All of it often moves faster than formal review processes can follow. When contextual access controls stop at login, all of it sits behind a door that was verified once and has been open ever since.


[The Verizon 2026 Data Breach Investigations Report](https://www.verizon.com/business/resources/reports/dbir/) — the largest dataset in the report’s history, drawing on more than 31,000 security incidents and 22,000 confirmed breaches — found that 62% of breaches involved the human element, including errors, social engineering, and decisions made without awareness of the risk. Those same users are active in collaboration environments every day, sharing files, forwarding credentials, and making access decisions that no runtime policy engine is monitoring.


The[Ponemon Institute’s 2026 Cost of Insider Risks Global Report](https://ponemon.dtex.ai/) put the financial exposure in sharper relief as they reported that the average annualized cost of insider risk reached $19.5 million in 2025; a 20% increase over two years. This uptick was primarily driven by negligent employees, credential theft, and malicious insiders exploiting access they should no longer hold. Even as containment times have improved — dropping to an average of 67 days — that window remains open for nearly ten weeks during which collaboration environments continue to expose sensitive operational information to users whose context has materially changed. Policy drift is the mechanism. Insider risk is the cost.


## **How CAEP and Policy Enforcement Points Close the Gap**


### What Is Continuous Access Evaluation (CAEP)?


[Continuous Access Evaluation Profile (CAEP)](https://openid.net/specs/openid-caep-specification-1_0.html) is an OpenID Foundation standard, finalized in 2025 under the Shared Signals Framework, that lets identity providers, device managers, and collaboration platforms share real-time session signals so access decisions update mid-session instead of only at login.


In practice, CAEP communicates specific real-time session changes — device compliance shifts, token revocations, location policy violations — so those changes can trigger an access update without waiting for the user to log in again.


CAEP defines the architecture that Zero Trust-aligned collaboration platforms are built to align toward — one in which the identity provider, the device management system, and the collaboration environment exchange shared security signals continuously and update access decisions as conditions change. An architecture conforming to this model responds immediately when a device falls out of compliance or a user’s role changes, reflecting updated access in real time rather than at next login. Runtime contextual awareness requires integration with the same controls governing Zero Trust decisions elsewhere in the enterprise, but that capability does not exist in most platforms.


### **What Is a Policy Enforcement Point?**


A Policy Enforcement Point (PEP) is the component in a Zero Trust architecture that translates policy decisions into actual access outcomes for a specific resource.[NIST 800-207](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf) treats PEPs as mature for networks and applications, but largely absent for active collaboration sessions.


In practice, PEPs are increasingly mature for network access, endpoint behavior, and application authentication. For active collaboration sessions, they remain largely absent.


The consequence is not theoretical. The runtime enforcement model security teams believe they have built stops at the edge of the collaboration environment. What happens inside the world, who sees what, when, under what conditions — is governed by provisioning decisions made days, weeks, or months earlier. And when that access is audited, the logs that most collaboration platforms produce capture activity without capturing context. They log what was accessed, but not under what posture, from what device, or under what policy conditions. The[audit trail](https://mattermost.com/blog/compliance-by-design-18-tips-to-implement-tamper-proof-audit-logs/) exists but it cannot answer the questions a Zero Trust reviews demands.


## ****What a Zero Trust Collaboration Platform Actually Requires****


A collaboration platform that genuinely participates in a Zero Trust architecture does not just mean supporting SSO. That is table stakes. The relevant question is whether the platform behaves as a policy enforcement surface receiving contextual signals from the broader security architecture and responding to them during active sessions, not only at authentication.


In operational terms, that means:


- Sessions that begin under compliant conditions can be modified or terminated when policy evaluates updated user and device attributes.
- Channel membership and message visibility are governed by current user and device context, not a provisioning state from ninety days ago.
- File access permissions reflect current role and clearance, evaluated dynamically.


The audit log the platform produces is contextually rich enough to support Zero Trust attestation, including capturing not just what occurred but the identity and network context under which it occurred.


This is the standard for the rest of the Zero Trust architecture, and the collaboration layer should not be exempt from it. Organizations that treat it as exempt are operating with a structural gap between the Zero Trust posture they believe they have and the one they do.


Identifying where that gap exists in each environment is the starting point, but closing it requires platforms willing to function as active participants in runtime enforcement, not communication utilities that authenticate at the door and disengage from governance thereafter.


## **Enforcement Ends Where Collaboration Begins — Until It Doesn’t**


Zero Trust rests on the single conviction that trust is the exploit. That conviction does not become optional inside a messaging platform or pause during an incident response channel or strategic planning thread. It applies everywhere operational work happens and information flows, which inherently includes the collaboration layer.


Enforcement that stops at the login screen is just perimeter security under a new name; not Zero Trust. Security and IT leaders who have invested significantly in Zero Trust programs should pressure-test whether their collaboration environments meet the same runtime enforcement standard applied elsewhere in the stack.


*Mattermost is built to close the collaboration layer enforcement gap, with runtime policy enforcement, ABAC-based access controls, and audit logging designed to integrate with your Zero Trust stack.[Register for a 1-hour preview demo to see Mattermost in action →](https://mattermost.com/sign-up/)*


## Read More Collaboration Articles


## Open source news, right in your inbox


## Thanks for subscribing!
