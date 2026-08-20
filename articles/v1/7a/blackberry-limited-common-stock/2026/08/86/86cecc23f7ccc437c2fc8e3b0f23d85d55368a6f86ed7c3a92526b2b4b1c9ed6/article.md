---
schema_version: "1.0.0"
document_id: "86cecc23f7ccc437c2fc8e3b0f23d85d55368a6f86ed7c3a92526b2b4b1c9ed6"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/moving-past-slogan-to-operational-requirements"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-14T01:22:00.473407+00:00"
fetched_at: "2026-08-14T01:22:02.638578+00:00"
content_hash: "sha256:1a26446ca805467668cd70aea73c10592ba4aced3e7a83728e460559bfe4219b"
---

# Assume Compromise, Then What? Moving Past the Slogan to Operational Requirements

# Assume Compromise, Then What? Moving Past the Slogan to Operational Requirements


If you actually believed an adversary was inside your environment, your communications would change first.


Aug 13, 2026


·


Blog


·


Ramon Pinero, Vice President and General Manager, BlackBerry AtHoc


"Assume compromise" has completed its lifecycle from insight to slogan. Every vendor writing about critical infrastructure security now leads with it. CI Fortify commentary repeats it so uniformly that the phrase has stopped carrying information: it no longer distinguishes one approach from another and it no longer tells an operator what to do differently on Monday.


The way to recover the value of the idea is to take it literally. Suppose you genuinely believed, today, that a capable adversary had some degree of access to your environment. Not hypothetically as a design principle, but actually. What changes first? The honest answer is not your firewall rules. It is your communications.


This isn't hypothetical framing.


[CISA's CI Fortify guidance](https://www.cisa.gov/topics/industrial-control-systems/ci-fortify)


defines isolation as "operating without reliable telecommunications, internet vendors, service providers, and upstream dependencies" — the assumption of compromise, written into federal guidance as an operating condition rather than a posture.


## The First Casualty of Assumed Compromise Is Trust in Your Own Channels


An adversary with access can read what you send over the channels they have reached. Under a genuine assumption of compromise, your email, your corporate chat, and potentially your voice traffic are all disclosure risks precisely when disclosure matters most: while you are discussing what you know, what you plan to do about it, and where your defenses are thin.


This is why out-of-band coordination is an established control rather than a preference.


[MITRE ATT&CK lists it as mitigation M1060](https://attack.mitre.org/mitigations/M1060/)


, directing defenders to "establish secure out-of-band communication channels to ensure the continuity of critical communications during security incidents, data integrity attacks, or in-network communication failures." Serious response playbooks stand these channels up before doing anything else. Yet the sector's CI Fortify content invokes assumed compromise constantly and rarely follows the premise to this, its most immediate operational consequence.


Taking "assume compromise" seriously means assuming the adversary can read your coordination. The first requirement it generates is a communications channel the adversary is not on.


#### **The Checklist the Premise Actually Generates**


Followed to its conclusions, assumed compromise produces a concrete set of operational requirements.


1.


**An out-of-band coordination channel.** End-to-end encrypted voice and messaging, independent of the corporate identity stack and network path, available to leadership and responders before an incident rather than improvised during one. BlackBerry


®


SecuSUITE


®


provides this layer.


2.


**Alerting that does not depend on compromised infrastructure.** The ability to reach your full response population across multiple channels with delivery confirmation, even if email and corporate telephony are down or untrusted. BlackBerry


®


AtHoc


®


provides mass notification and personnel accountability and is FedRAMP Class D (High) certified, re-certified this year — which matters when the fallback layer itself must not become the soft target.


3.


**Trusted endpoints for the response itself.** Managed, hardened devices for the people running the response because a fallback channel accessed from a compromised laptop is not a fallback. BlackBerry


®


UEM keeps that small population of devices patched, hardened, and recoverable.


4.


**Rehearsal.** The assumption is only operational if teams have actually exercised working through it.


Notice what this checklist is not. It is not a replacement for segmentation, detection, or recovery capability, and it is not another way of saying buy everything. It is the specific, bounded set of capabilities that the sector's favorite slogan logically requires and that its content systematically omits.


## Why the Omission Persists


Vendors write about the problems their products solve. Most vendors shaping this conversation sell network security, OT visibility, or access control — capable products solving the parts of the problem their diagrams contain. The coordination layer sits outside every one of those diagrams which is exactly why it goes unfunded.


Assume compromise. Then act like it: put your coordination on a channel the adversary cannot reach, make sure you can still summon your people, and put the response on hardware you trust. That is what the slogan means when it means anything at all.


**Related Reading:**
***[What CI Fortify Asks Utilities to Spend — And the Capability It Never Names](https://www.blackberry.com/en/secure-communications/insights/blog/what-ci-fortify-asks-utilities-to-spend)***
***[When the Phones Go Down: The Coordination Gap Nobody Plans For](https://www.blackberry.com/en/secure-communications/insights/blog/when-phones-go-down)***
***[Lessons from the July Water Utility Attacks: What the Joint Advisory Tells Operators to Do Next](https://www.blackberry.com/en/secure-communications/insights/blog/lessons-from-july-water-utility-attacks)***
***[Why OT Isolation Planning Needs Communications Continuity](https://www.blackberry.com/en/secure-communications/insights/blog/why-ot-isolation-planning-needs-communications-continuity)***


Get updates about the latest in-depth knowledge for secure communications.
