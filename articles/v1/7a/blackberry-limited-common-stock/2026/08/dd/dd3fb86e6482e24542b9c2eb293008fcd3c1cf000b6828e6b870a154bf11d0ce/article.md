---
schema_version: "1.0.0"
document_id: "dd3fb86e6482e24542b9c2eb293008fcd3c1cf000b6828e6b870a154bf11d0ce"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/why-ci-security-guidance-fails-small-utility-operators"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-14T14:39:24.586950+00:00"
fetched_at: "2026-08-14T14:39:25.365499+00:00"
content_hash: "sha256:bbcacf2a1085164084867752f28ee4c602558fda367855a901983369e35b41f6"
---

# Why CI Security Guidance Keeps Failing Small Utility Operators

# Why CI Security Guidance Keeps Failing Small Utility Operators


Advice written for enterprises with security operations centers lands on utilities with three-person teams. Sequencing is the fix.


Aug 14, 2026


·


Blog


·


Ramon Pinero, Vice President and General Manager, BlackBerry AtHoc


Most critical infrastructure security guidance is written for large enterprises with dedicated security operations centers and substantial budgets. Small utilities, often operating with skeleton crews and constrained resources, cannot implement these frameworks as written. The result is not partial compliance; it is operational paralysis that leads to persistent vulnerability.


Small water utilities, rural electric cooperatives, and regional wastewater operators sit at the foundation of national infrastructure, serving millions of people. Yet by most practical measures, they are underserved by the very security frameworks designed to protect them.


The guidance exists: CISA publishes it, NERC-CIP mandates portions of it, and sector-specific agencies issue regular advisories. The most recent addition is[CI Fortify](https://www.blackberry.com/en/secure-communications/business/ci-fortify) , the initiative CISA launched in May 2026 and extended on July 28 with joint guidance issued alongside the Australian Signals Directorate’s Australian Cyber Security Centre, the FBI, and international partners. It asks operators to build two capabilities: isolation, defined as “proactively disconnecting from third-party dependencies and operating without reliable telecommunications, internet vendors, service providers, and upstream dependencies,” and recovery, where “a key part is testing recovery plans and practicing local and manual operations.”


That updated guidance arrived days after a coordinated cyberattack disrupted more than 30 community water systems in Minnesota over the weekend of July 26–27, 2026. The FBI reported incidents at water and wastewater utilities in at least twelve states after July 27, some of which degraded water operations.


The issue is not the quality of any of this documentation; it is that much of it assumes an organizational model most small utility operators do not have: dedicated security analysts, tiered incident response teams, formal change management processes, and budget lines for continuous monitoring tools.


A community water system serving fewer than 10,000 people,[roughly 91 percent of the approximately 50,000 community water systems in the country](https://www.foodmanufacturing.com/facility/news/22972394/partnership-aids-rural-water-security) , may employ only two or three people in IT-adjacent roles, most of whom also carry operational responsibilities. That organization cannot staff a 24-hour security operations center or dedicate a full-time employee to threat intelligence analysis. When a federal advisory arrives with dozens of mitigation actions and no sequencing, the practical result is often inaction: not noncompliance born of negligence, but paralysis created by an impossible implementation burden.


# The Enterprise Assumption Problem


Frameworks like NIST CSF, ISO 27001, and sector-specific guidance from CISA generally assume a mature enterprise: layered teams, defined roles, documented processes, and budget authority for specialized tooling. That model describes a Fortune 500 company or a large municipal utility, not most operators responsible for critical infrastructure at the community level.


Full NIST CSF implementation requires an organization to identify assets comprehensively, protect them through access controls and training, detect anomalies through continuous monitoring, respond through a documented incident response plan, and recover through tested business continuity procedures. Each function creates additional sub-functions, roles, tools, and processes.


For a three-person operations team managing both IT and operational technology environments, implementing these functions concurrently is not feasible. The guidance does not tell operators what to do first if they can only address one function this quarter. It does not distinguish between a control that materially reduces risk and one that delivers only marginal improvement. It presents a comprehensive architecture, then leaves sequencing to the reader.


This is a structural failure more than a documentation gap. The organizations with the least capacity to interpret and prioritize complex guidance are the ones receiving the least interpretive support from it.


# The Paralysis of Unsequenced Mitigation Lists


CISA advisories and sector alerts frequently include mitigation lists of 20 to 50 items. Each item may be technically valid but together they create an implementation backlog that a small operator cannot clear in a year, let alone in the short window between advisory publication and active exploitation.


Unsequenced lists create a specific cognitive problem: they offer no entry point. When every item is presented as equally important, operators often start with what is familiar or logistically convenient rather than what is operationally critical. Patch management gets attention because it is routine. Network segmentation between IT and OT environments gets deferred because it requires planning and potential downtime. Multi-factor authentication rollout slips because it requires user coordination.


The result is a security posture that looks active while effort concentrates in lower-impact areas and high-value attack surfaces remain exposed. Adversaries targeting critical infrastructure do not need the most recently unpatched system. They need the segmentation gap, the unmonitored remote access channel, or the default credentials that remained unchanged because the advisory mentioning them was item 34 of 47.


# Coordination Failure Is Total Failure in a Small Organization


Sequencing for a small utility has to be ranked by two scarce resources at once: budget and staff attention. It also has to respect a structural fact about incidents that enterprise-oriented guidance can afford to ignore.


Large organizations can absorb coordination failures. If one team does not communicate a security event to another, the second team may still detect and respond independently. Redundancy provides resilience.


Small utilities do not have that redundancy. A utility whose entire response capacity is four individuals loses the response itself if those four cannot reach each other, confirm who is engaged, and speak candidly about what to do. If the one person who manages SCADA systems is unavailable during an incident, and no one else understands the OT environment well enough to isolate affected systems, coordination failure becomes operational failure.


CI Fortify makes this sharper rather than softer.[CISA’s own definition of isolation instructs operators to plan for operating without reliable telecommunications](https://www.blackberry.com/en/secure-communications/insights/blog/why-ot-isolation-planning-needs-communications-continuity) and the guidance says nothing about how a utility coordinates a response once that condition holds. Its second capability compounds the problem: practicing local and manual operations is not a network activity. It is people. Operators called in at 2 a.m., shift coverage extended, contractors and integrators reached, mutual aid partners engaged, a primacy agency notified, a community told what is happening.


Isolation also does not isolate a utility from its obligations. During an extended isolation period, utilities still require chemical deliveries to be ordered and paid for, certified lab work and compliance sampling filed with the state, parts and purchase orders, and payroll. None of that is operational technology. All of it is coordination with people outside the fence and all of it is required for water to keep flowing safely and legally.


Effective incident response in a small organization therefore requires radical simplicity. The response plan should fit on two pages and identify who calls whom, in what order, through which communication channel, when a specific trigger condition is met. It should be tested at least once a year through a tabletop exercise with the people who would actually execute it. It should be updated whenever staff or contact information changes.


Complexity in a response plan creates liability under the exact conditions the plan is meant to address: pressure, confusion, and time constraint.


# Detection Is Being Solved. Coordination Is Still Yours.


The staffing problem described above is finally getting sector-level attention. On August 7, 2026,[DEF CON Franklin and the National Rural Water Association launched the Water Watch Center](https://www.nextgov.com/cybersecurity/2026/08/new-water-watch-center-launched-help-small-utilities-stop-cyberattacks/415288/) , pairing an initial set of five managed detection and response providers with water utilities serving fewer than 10,000 people at no cost to the utility.


This is the most credible answer yet to the small-utility staffing gap, but it should be read precisely: it supplies detection capacity to operators who have none.


Detection tells you it happened. Coordination is how you respond. The Water Watch Center closes the first gap for small utilities; the coordination layer is still theirs to solve. What a three-person utility does in the hour after that provider calls — who picks up, who is reachable, who can authorize a shutdown, who notifies the state — is a separate problem.


# A First-Quarter Deployment Path


The same sequencing logic can be turned into a first-quarter plan. The goal is visible progress against the risks most likely to produce operational disruption rather than completion of every security requirement. Start with the layer whose absence nullifies every other investment during an actual event.


**Days 1–30:** stand up alerting with delivery confirmation. Replace the manual call tree that fails at 2 a.m. with mass notification that reaches staff, contractors, integrators, and mutual aid contacts across multiple channels, and that accounts for who actually responded. This is the operational form of “practicing local and manual operations”; you cannot run a plant by hand without first assembling the people who know how. BlackBerry® AtHoc® provides this capability and is FedRAMP Class D (High) certified, re-certified this year, which matters when the coordination layer itself must not become the soft target.


**Days 31–60** : establish an out-of-band coordination channel. Put leadership and operations on encrypted, authenticated voice and messaging that is independent of the corporate identity stack and network path, so incident coordination never rides on channels that are degraded or presumed compromised. BlackBerry® SecuSUITE® provides this layer. Out-of-band coordination is an established control, not a preference:[MITRE ATT&CK lists it as mitigation M1060](https://attack.mitre.org/mitigations/M1060/) , directing defenders to “establish secure out-of-band communication channels to ensure the continuity of critical communications during security incidents, data integrity attacks, or in-network communication failures.”


**Days 61–90:** bring response-critical devices under management. Identify the small population of phones and laptops that would carry the response and ensure they are patched, hardened, and recoverable. BlackBerry® UEM manages that population. A fallback channel accessed from a compromised laptop is not a fallback.


None of these moves requires touching the control network, retaining specialist staff, or committing to a multi-year program. Each produces measurable readiness within weeks, which matters for boards and rate cases as much as for security: a utility can demonstrate to its regulators and its community that the most recent federal advisory changed something concrete about its posture. And each pays off in ordinary operations, not only during a crisis.


Broader guidance still matters. Credential hygiene, network segmentation, and multi-factor authentication all belong in a mature program, and operators should work toward them. But they are the second quarter’s work, not the first, because a utility that has hardened every system and cannot coordinate its own people has bought controls it will not be able to use during the event they were purchased for.


# A Note to the Sector’s Advisors


Associations, state primacy agencies, and consultants who translate federal guidance for small systems have more influence over sector security than any vendor. The most valuable thing that influence can produce right now is honest prioritization: telling a resource-constrained utility not just what the full mitigation set contains, but what to do first, second, and third, and why.


Guidance that refuses to sequence is guidance that quietly delegates the hardest decision to the least resourced party.


# Security Guidance Must Serve the Operators Who Need It Most


The guidance that currently exists is not wrong. But it is incomplete in ways that matter most to the organizations least equipped to compensate for its gaps.


CI Fortify sets the right destination. Effective critical infrastructure security at the community level requires guidance that sequences controls by operational impact, distinguishes between what is ideal and what is sufficient, and accounts for the staffing and budget realities small operators actually face.


Until that guidance exists in a more accessible, operationally grounded form, small utility operators should resist the paralysis that unsequenced mitigation lists produce. Sequence by impact per dollar spent and per staff-hour invested. Start with the layer that keeps your people coordinated. Build from there. And ask the question CI Fortify does not pose: *when we disconnect, how do our people reach each other?*


**Related Reading:**


-


***[Assume Compromise, Then What? Moving Past the Slogan to Operational Requirements](https://www.blackberry.com/en/secure-communications/insights/blog/moving-past-slogan-to-operational-requirements)***


-


***[What CI Fortify Asks Utilities to Spend — And the Capability It Never Names](https://www.blackberry.com/en/secure-communications/insights/blog/what-ci-fortify-asks-utilities-to-spend)***


-


***[When the Phones Go Down: The Coordination Gap Nobody Plans For](https://www.blackberry.com/en/secure-communications/insights/blog/when-phones-go-down)***


-


***[Lessons from the July Water Utility Attacks: What the Joint Advisory Tells Operators to Do Next](https://www.blackberry.com/en/secure-communications/insights/blog/lessons-from-july-water-utility-attacks)***


-


***[Why OT Isolation Planning Needs Communications Continuity](https://www.blackberry.com/en/secure-communications/insights/blog/why-ot-isolation-planning-needs-communications-continuity)***


Get updates about the latest in-depth knowledge for secure communications.
