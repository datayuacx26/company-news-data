---
schema_version: "1.0.0"
document_id: "83dff17d5d65469b76d762de7d715ed1c6db1cf4ef047e7a96dfdaba62b8fa1b"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/what-ci-fortify-asks-utilities-to-spend"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T22:39:49.648663+00:00"
fetched_at: "2026-08-12T22:39:53.475522+00:00"
content_hash: "sha256:41742e202c7440775066a6458d47e8cec815d2b3686eae798cc2fb6c52fc937c"
---

# What CI Fortify Asks Utilities to Spend — And the Capability It Never Names

# What CI Fortify Asks Utilities to Spend — And the Capability It Never Names


CISA tells operators to plan for life without reliable telecommunications. It doesn't say how you'd coordinate once that happens.


Aug 12, 2026


·


Blog


·


Ramon Pinero, Vice President and General Manager, BlackBerry AtHoc


The guidance arrived in the middle of the attack.


Over the weekend of July 26


– 27, 2026, a coordinated cyberattack disrupted more than 30 community water systems in Minnesota.


The FBI reported that water and wastewater utilities in at least seven states experienced incidents after July 27, some of which degraded water operations (


[FBI Public Service Announcement I-073026-PSA](https://www.ic3.gov/PSA/2026/PSA260730.pdf)


). On July 30, CISA issued an alert on a significant increase in threat activity targeting programmable logic controllers in the water sector.


Two days before that alert, CISA — with the Australian Signals Directorate's Australian Cyber Security Centre, the FBI, and international partners — published


*[CI Fortify – Advice for Isolating Vital Systems](https://www.cisa.gov/resources-tools/resources/ci-fortify-advice-isolating-vital-systems)* . Operators were reading isolation guidance while their neighbors were living through the scenario it describes.


So the cost question is not academic, and it deserves a straight answer: what does following this guidance actually require a small utility to spend?


## **The Capital Cost Is Lower than the Sector Fears**


There's a widespread assumption that CI Fortify means redundant infrastructure — a parallel environment standing by, waiting to take over. That assumption is expensive, discouraging, and wrong.


CISA names two capabilities. Isolation: "proactively disconnecting from third-party dependencies and operating without reliable telecommunications, internet vendors, service providers, and upstream dependencies." Recovery: "rapidly restoring vital compromised systems while isolated," where "a key part is testing recovery plans and practicing local and manual operations."


Restoring compromised systems and practicing manual operations is the low-capital path, not the high one. The July 28 guidance reinforces it — the work it details is identifying critical systems, mapping connections, and implementing effective separation points.


Inventorying assets, mapping dependencies, and testing recovery procedures. A utility with three operators can begin most of it without a budget cycle.


That is genuinely good news, though it is not always reflected in how the guidance is discussed.


## **The Real Cost Sits in a Line Item Nobody Has**


Read CISA's definition of isolation once more:


***operating without reliable telecommunications*** **.**


The guidance instructs operators to plan for their communications to be unavailable. It then says nothing about how a utility coordinates a response once they are.


That silence is the gap. Not an expensive requirement — an unaddressed one. And it compounds immediately, because the second capability CISA asks for is practicing local and manual operations. Manual operations are not a network activity. They involve people: operators called in at 2 a.m., shift coverage extended, contractors and integrators reached, mutual aid partners engaged, a primacy agency notified, a community told what's happening. Every one of those depends on reaching human beings over channels CISA has just told you to assume are unreliable or untrusted.


Isolation also doesn't isolate a utility from its obligations.


During an extended isolation period, utilities still require chemical deliveries to be ordered and paid for , certified lab work and compliance sampling filed with the state, parts and purchase orders, and payroll. None of that is operational technology. All of it is coordination with people outside the fence — and all of it is required for water to keep flowing safely and legally.


A small utility that has mapped every dependency and rehearsed manual operation still fails if its four responders can't reach each other and confirm who is engaged.


**In a small organization, coordination failure is total failure.** An enterprise loses an hour and absorbs it. A utility loses the response itself.


## **The Coordination Layer, Concretely**


Closing the gap CI Fortify leaves open takes three capabilities, and none of them touch the control network.


**Mass notification and personnel accountability.** BlackBerry


® AtHoc


® reaches staff, contractors, integrators, and mutual aid partners across multiple channels with delivery confirmation — and accounts for who responded. This is the operational form of "practicing local and manual operations": you cannot run a plant by hand without assembling the people who know how, and a manual call tree does not survive a 2 a.m. multi-site event. BlackBerry AtHoc is FedRAMP Class D (High) authorized, re-authorized this year, which matters when the coordination layer itself must not become the soft target.


**Encrypted voice and messaging.** BlackBerry


® SecuSUITE


® provides communications independent of the corporate identity stack and network path, so leadership and operations can coordinate candidly during containment even when the infrastructure carrying normal traffic is degraded or presumed compromised. When CISA's own definition of isolation assumes telecommunications are unreliable, an independent channel isn't a precaution — it's the stated operating condition.


**Managed, trusted endpoints.** BlackBerry


® UEM keeps the small population of response-critical devices patched, hardened, and recoverable. A fallback channel accessed from a compromised laptop is not a fallback.


This is additive. It does not require re-architecting the control network, retaining specialist staff, or committing to a multi-year program — and it produces value on ordinary days, not only during an event.


## **Why We're Saying It**


BlackBerry sells secure communications, so we have an obvious interest in where this argument lands. Fair. But the point stands on the primary text: CISA has directed critical infrastructure operators to prepare to operate without reliable telecommunications and to practice manual operations, and the guidance does not address how coordination survives either condition. Most vendors shaping this conversation sell network security, OT visibility, or access control — capable products solving the parts of the problem their diagrams contain. The coordination layer sits outside every one of those diagrams, which is exactly why it goes unfunded.


CI Fortify sets the right destination. Utilities implementing it should ask one additional question the guidance doesn't pose:


*when we disconnect, how do our people reach each other?*


**Related Reading:**
***[When the Phones Go Down: The Coordination Gap Nobody Plans For](https://www.blackberry.com/en/secure-communications/insights/blog/when-phones-go-down)***
***[Lessons from the July Water Utility Attacks: What the Joint Advisory Tells Operators to Do Next](https://www.blackberry.com/en/secure-communications/insights/blog/lessons-from-july-water-utility-attacks)***
***[Why OT Isolation Planning Needs Communications Continuity](https://www.blackberry.com/en/secure-communications/insights/blog/why-ot-isolation-planning-needs-communications-continuity)***


Get updates about the latest in-depth knowledge for secure communications.
