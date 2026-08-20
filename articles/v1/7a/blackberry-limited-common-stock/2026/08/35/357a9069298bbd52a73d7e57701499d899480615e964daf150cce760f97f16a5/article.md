---
schema_version: "1.0.0"
document_id: "357a9069298bbd52a73d7e57701499d899480615e964daf150cce760f97f16a5"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/water-watch-center-what-sector-must-build"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T00:27:36.183116+00:00"
fetched_at: "2026-08-18T00:27:38.947359+00:00"
content_hash: "sha256:42835a576b77eb7573214a89e14140cb9cca19fd37bf738fb270d7565032ec65"
---

# Detection Is Not Coordination: What Small Utilities Still Need After Water Watch

# Detection Is Not Coordination: What Small Utilities Still Need After Water Watch


Detection is finally reaching the smallest utilities that need it most. Coordinating a response remains a challenge that local operators must solve themselves.


Aug 17, 2026


·


Blog


·


Jay Goodman


On August 7,


[DEF CON Franklin and the National Rural Water Association (NWRA) launched the Water Watch Center](https://www.nextgov.com/cybersecurity/2026/08/new-water-watch-center-launched-help-small-utilities-stop-cyberattacks/415288/) at DEF CON in Las Vegas.


Five managed detection and response providers will deliver services directly to utilities serving fewer than 10,000 people.


Those utilities account for 91 percent of the roughly 50,000 community water systems in the United States. They are also the systems that have historically been the hardest to reach with cybersecurity programs and guidance.


The launch deserves more than a passing mention. It follows two years of field work in which DEF CON Franklin recruited nearly 450 volunteer cyber experts to support water and wastewater utilities across seven states. When it became clear that volunteer efforts could not scale to the size of the sector, the work evolved into a service model.


The result is the first credible effort to deliver cybersecurity capability at scale to utilities with minimal staff and limited resources.


It answers an important question, but only half of one.


## What Detection Produces and What It Does Not


Managed detection and response solutions close a visibility gap. For a utility that has never had anyone watching its environment, that is a substantial gain. The output of the capability, though, is a notification. A notification is the beginning of a response, not the response itself.


Consider what the recipient must do at two in the morning:


-


Reach an operator who is not on shift


-


Reach an operations lead who is also the IT department


-


Assemble enough people to make a containment decision


-


Coordinate a transition to manual control across a plant and a distribution system


-


Establish who is on site and who is reachable.


-


Report status to the state primacy agency and to CISA and keep reporting for as long as the incident runs.


None of that is a detection function and none of it happens without channels that hold up while the enterprise network is under suspicion. Independent commentary on the launch has already noted that


[monitoring built for IT environments may not register direct interaction with an exposed controller](https://www.govinfosecurity.com/a-new-national-water-cyber-program-may-miss-real-gap-a-32509) and that MDR delivers for utilities without security staff only where it has visibility into the operational environment. That limitation is real, and it belongs to the OT security specialists. Set it aside for a moment because even detection with perfect operational visibility still ends at the notification.


## The Dependency Question CI Fortify Raises


In May, CISA published


[CI Fortify: Advice for Isolating Vital Systems](https://www.blackberry.com/en/secure-communications/business/ci-fortify)


with the Australian Signals Directorate, the United Kingdom's National Cyber Security Centre, and the Canadian Centre for Cyber Security. It makes two structural demands:


1.


**Isolation** : the ability to proactively disconnect vital operational technology and enabling systems from third-party dependencies including telecommunications, internet, and vendor connections


2.


**Recovery** : the ability to rebuild from known-good sources while still isolated. CISA's planning language points to sustaining essential services in that state for up to three months


Read the two initiatives next to each other and a design challenge emerges. Managed detection and response is delivered by a third party over the internet. Under CI Fortify's own planning assumption, it sits on the far side of the boundary the operator has been instructed to be able to draw.


Every capability delivered as a service faces the same challenge, and the useful time to answer it is before an isolation event rather than during one. An operator who understands that detection goes quiet during isolation can plan around it. An operator who has never asked will discover it at the worst possible moment.


That challenge applies to us as well. BlackBerry


®


AtHoc


®


is cloud-first by default, and the deployment model is what determines whether alerting and accountability survive a severed connection; on-premises and hybrid deployment are the reason the answer can be yes and a hosted-only configuration is not. Any vendor asserting continuity through isolation, this one included, should be required to name the deployment model behind the assertion.


So there is one question worth putting to every provider in the response chain, in the same words:


*what does this capability do when the internet connection is gone and what deployment model makes that answer true?*


## What the Next Dollar Buys


The funding picture moved in the same week. Two U.S. Senators introduced the


[Water Cyber Shield Act](https://thehill.com/homenews/senate/6020953-water-cyber-shield-act-schiff-klobuchar/) which would give EPA explicit authority to conduct cybersecurity assessments and set standards alongside CISA and NIST and would authorize $300 million annually through the Drinking Water and Clean Water State Revolving Funds. The bill is introduced rather than enacted and authorization is not appropriation. Even so, a sector accustomed to receiving unfunded mandates now holds a no-cost detection service and has a plausible funding mechanism in front of Congress.


That turns sequencing into a live question. If the first increment of capability arrived free and covered detection, the second increment should cover what detection hands off to: mass alerting with confirmed delivery and verified personnel accountability, carried on a coordination channel that does not ride the network under suspicion. For a utility whose entire response capacity is four people, coordination failure is response failure. There is no depth on the bench to absorb it.


There is a durability point here that nobody should be shy about making. Philanthropic seed funding and volunteer effort built something the federal government had not managed to build and neither one is a budget line. Capability a utility owns, operates, and exercises in daily work outlasts any grant cycle. The most valuable thing the sector's associations and state primacy agencies can do with the current wave of attention is tell their members what to fund second and why.


The Water Watch Center closed the sector's widest gap. The next gap is the one between a detection and a response.


**Related reading:**


-


***[Why CI Security Guidance Keeps Failing Small Utility Operators](https://www.blackberry.com/en/secure-communications/insights/blog/why-ci-security-guidance-fails-small-utility-operators)***


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
