---
schema_version: "1.0.0"
document_id: "859e515e4a391035cfa99102e98656cc76869928d14113f099dccaea86efc48a"
company_key: "yc-mattermost"
company: "Mattermost"
source_id: "yc-mattermost-rss-3d807a20d23e"
canonical_url: "https://mattermost.com/blog/the-collaboration-problem-zero-trust-still-hasnt-solved/"
published_at: "2026-07-01T13:00:00+00:00"
first_seen_at: "2026-07-20T23:24:03.492829+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:b8c1002f540152c0839da5edd7d884ddc9c9a1c1021e34595311863a8b91762a"
---

# The Collaboration Problem Zero Trust Still Hasn’t Solved

## Key Takeaways


- Enterprise collaboration platforms typically treat every login session as equivalent, regardless of device, location, network, or time of access.
- NIST SP 1800-35 and CISA’s Zero Trust Maturity Model both call for continuous, context-aware policy enforcement at every access point, not just initial login.
- Persistent session states and static permission sets create a collaboration layer blind spot that leaves messaging platforms outside core Zero Trust enforcement.
- The 2025 Ponemon Institute Cost of Insider Risks Global Report found that 55% of insider incidents stem from negligent or mistaken users.
- Runtime-aware collaboration requires session-level context evaluation, automated permission propagation, attribute-based channel access, and contextual audit logging.


Consider a scenario that plays out routinely across enterprise environments. A senior analyst accesses the company’s collaboration platform on Tuesday morning — managed corporate device, internal network, multi-factor authentication completed, familiar location, clean device posture. Every contextual signal aligns with what the access policy expects.


Three days later, that same analyst logs into the same platform at 2am on Saturday. The device is personal. The network is hotel Wi-Fi in a different time zone. The credentials and authentication method are identical, but the operational context surrounding them is fundamentally different.


Most enterprise messaging platforms treat those two sessions the same, and that uniform treatment is a measurable gap in Zero Trust enforcement that remains one of the most underexamined problems facing security architects and IT leaders today.


## What Does Zero Trust Require From the Collaboration Layer?


Zero Trust requires collaboration platforms to reevaluate access continuously, not just at login, using signals like device posture, network trust, location, and time. NIST SP 1800-35 and CISA’s Zero Trust Maturity Model both call for continuous, context-aware enforcement at every access point, including messaging and chat.


NIST SP 1800-35, the June 2025 practice guide built across 24 industry vendors and 19 real-world Zero Trust deployments, states explicitly that effective Zero Trust requires continuous, context-aware policy enforcement at every access point; not a one-time authentication check at login. CISA’s[Zero Trust Maturity Model](https://www.cisa.gov/resources-tools/resources/zero-trust-maturity-model) describes the target state as one where policies are “triggered by observed behaviors and real-time risk signals rather than static rules.”


The 9 a.m. Tuesday session and the 2 a.m. Saturday session differ across every one of those signals of device posture, network trust level, location, and time of access. A mature Zero Trust architecture is designed to recognize that difference and respond to it — producing the contextual signals that enforcement systems and audit tools use to evaluate whether access decisions reflect current risk. Unfortunately, the collaboration layer — where sensitive operational communication happens — frequently does not participate in that architecture at all.


According to[Gartner](https://www.gartner.com/en/newsroom/press-releases/2024-04-22-gartner-survey-reveals-63-percent-of-organizations-worldwide-have-implemented-a-zero-trust-strategy) , 63% of organizations worldwide have implemented a strategy. The research firm[also reported](https://www.gartner.com/en/newsroom/press-releases/2024-04-22-gartner-survey-reveals-63-percent-of-organizations-worldwide-have-implemented-a-zero-trust-strategy) that 75% of U.S. federal agencies will still fail to fully implement Zero Trust security policies through 2026 due to funding and expertise shortfalls. The gap between declaring a strategy and enforcing it consistently across every access point is where the real exposure lives — and collaboration sits squarely in that gap.


## What Is **The Collaboration Layer Blind Spot** in Zero Trust


The collaboration layer blind spot is the tendency of enterprise messaging platforms to treat every session identically, regardless of device, network, location, or time. Persistent session states and static permissions mean these platforms operate as isolated silos outside the broader Zero Trust enforcement architecture.


That blind spot is easiest to understand through a concrete example. An organization has invested meaningfully in[Zero Trust architecture](https://mattermost.com/blog/what-is-zero-trust-compliant-collaboration/) — conditional access policies govern identity verification, device posture is evaluated before network access is granted, and risk signals are aggregated continuously across the environment. It performs as intended for the applications the framework was designed to cover.


A user then opens the enterprise messaging platform. The 9 a.m. Tuesday session and the 2 a.m. Saturday session look identical. Same credentials verified, same MFA prompt passed, same persistent permissions applied. The platform does not reevaluate device posture, register that the network is unmanaged, or recognize that the time and location fall outside any established behavioral pattern.


This is the collaboration layer blind spot. These platforms maintain persistent session states and static permission sets that do not respond to changing context. They operate as isolated silos rather than integrated enforcement points within the broader Zero Trust architecture. The[2026 Verizon Data Breach Investigations Report](https://www.verizon.com/business/resources/reports/dbir/) documented more than 22,000 confirmed breaches — nearly double the prior year — with third-party access and session-layer exposure representing a growing share of that landscape.


The failure modes are not exotic. A contractor’s engagement concludes, but access to sensitive operational channels persists because the offboarding process did not extend to the collaboration platform. A team member moves to a different department but continues to see content from their previous role for weeks. An employee connects from an unmanaged personal device and encounters no differentiated access treatment. None of these involve malicious intent, but all of them represent static operational assumptions that no longer reflect reality.


The[2025 Ponemon Institute Cost of Insider Risks Global Report](https://ponemon.dtex.ai/) found that 55% of insider incidents originate from negligent or mistaken users, with annualized costs reaching $17.4 million per organization; a 13% increase in just three years. The threat vector is not a sophisticated adversary. It’s just a system that has not been told the context has changed.


The problem compounds as AI adoption accelerates, particularly when we consider that the 2026[Verizon DBIR](https://www.verizon.com/business/resources/reports/dbir/) flagged that employee use of unapproved shadow AI tools tripled to 45% of the workforce. When employees connect unsanctioned AI tools through collaboration platforms — uploading documents, pasting channel content, querying project data — information leaves the environment without an audit trail. The collaboration layer becomes an unintended exfiltration path, and the 2am Saturday session becomes the mechanism.


## **Why Do Static Collaboration Permissions Create Compliance Risk?**


> Static collaboration permissions create compliance risk because they produce permission drift, where access granted for legitimate reasons persists after the original context changes. Without contextual signals in audit logs, organizations cannot prove that access decisions reflected current risk, which weakens evidence during regulatory review.


Security leaders understand that operational and governance consequences deserve equal attention, particularly for the architects and IT leaders who own policy enforcement and compliance accountability. When collaboration platforms fail to reevaluate access in real time, that drift compounds quietly: no system is compromised and no policy is technically violated in a way the platform can detect.


Permission drift produces governance gaps that are difficult to close retroactively, and the audit question is not ***just*** whether a policy was violated, but whether the organization can demonstrate that contextual enforcement was in place at all. For organizations subject to regulatory oversight, the inability to ***prove*** that access decisions reflect current context — rather than static permission tables — represents a compliance exposure that can exceed the underlying security risk.


Audit challenges compound this, as a useful compliance record does not simply confirm that a user was authenticated. It records device type, network classification, time and location of access, and whether access decisions aligned with the user’s current attribute profile — the signal set a SIEM or UEBA tool needs to evaluate access patterns. Without contextual signals in the audit log, the 2 a.m. Saturday session is indistinguishable from the 9 a.m. Tuesday session in the record — and neither session can be used to demonstrate that enforcement occurred.


[IBM’s 2025 Cost of a Data Breach Report](https://www.ibm.com/reports/data-breach) provides relevant financial context, as malicious insider attacks carried the highest average cost at $4.92 million per incident, and credential-based compromise scenarios averaged 246 days to identify and contain. Those timelines extend precisely because the collaboration layer lacks the contextual logging that would surface anomalous access patterns earlier in the timeline.


**What Does Runtime-Aware Collaboration Actually Require?**


> Runtime-aware collaboration requires session-level context evaluation, automatic role and permission propagation tied to identity systems, attribute-based channel access, and contextual audit logging that captures session ID, IP address, and user agent. Each capability must function continuously, not as a one-time login check.


The solution is not adding multi-factor authentication to the collaboration platform. The 2 a.m. Saturday session already passed MFA because MFA validated the credential without evaluating the context surrounding that credential.


Runtime-aware collaboration means the platform participates actively in the access enforcement architecture rather than passively alongside it. Several capabilities must be genuinely present for that integration to hold.


- **Contextual access evaluation must occur at the session level, not only at login.** When the 2am Saturday session arrives — with its different device, network, location, and time context — these signals represent the risk context a Zero Trust architecture needs as policy inputs. Enforcement scope should adapt to those inputs: channel visibility narrowed and additional verification required where contextual policy supports it.
- **File access should require additional verification.**[CISA’s Zero Trust Maturity Model](https://www.cisa.gov/resources-tools/resources/zero-trust-maturity-model) identifies this dynamic, signal-driven policy adjustment as a defining characteristic of advanced implementation.
- **Role and permission changes must propagate automatically, tied to live identity signals rather than manual processes** . When an analyst changes roles, concludes a project, or separates from the organization, collaboration access should update automatically in concert with the identity management system, following the next synchronization with the identity provider. NIST SP 1800-35 is direct on this requirement: effective Zero Trust implementation demands continuous enforcement tied to live identity signals, not periodic reconciliation against static permission tables.
- **Attribute-based channel access — controlling what information is visible to a given user based on their current role and attribute context — must function as a first-order access control, not a configuration option.** An analyst connecting from an unmanaged device at 2 a.m. on a Saturday should encounter a meaningfully different operational environment than the same analyst on a managed device during business hours. The platform must enforce that difference without requiring manual intervention.
- **Contextual audit logging must capture the full signal set:** session ID, IP address, and user agent — the confirmed foundation of the contextual audit record. That is the baseline a compliance review requires and an incident investigation depends on. Without contextual signals in the audit log, the organization cannot prove what the system did during the 2 a.m. Saturday session, only that authentication succeeded.


## **The Same Identity Should Not Always Receive the Same Access**


Zero Trust, implemented correctly, closes the gap between what the policy states and what the system enforces across every access point. The collaboration layer has largely sat outside that enforcement loop by default rather than by design.


The 9 a.m. Tuesday session and the 2 a.m. Saturday session carry different device postures, different network trust levels, different locations, different times, and different access contexts. A Zero Trust architecture that treats them identically has not solved the problem it was designed to solve. It has left the gap precisely where sensitive communication happens most.


Organizations that are serious about Zero Trust maturity need to evaluate their collaboration platforms against the same standard applied to every other enforcement point in the architecture. Does it adapt access decisions to current context, enforce policy changes in real time, and produce audit records that can prove contextual enforcement occurred? If the answer is no, architecture has a measurable gap — and the 2 a.m. Saturday session is where that gap is most likely to matter.


*The 9am and 2am sessions don’t have to look identical to your collaboration platform. Mattermost enforces Zero Trust policy at the session layer — evaluating context continuously, not just at login.[Register for a 1-hour preview demo to see Mattermost in action](https://mattermost.com/sign-up/) .*


## Read More Collaboration Articles


## Open source news, right in your inbox


## Thanks for subscribing!
