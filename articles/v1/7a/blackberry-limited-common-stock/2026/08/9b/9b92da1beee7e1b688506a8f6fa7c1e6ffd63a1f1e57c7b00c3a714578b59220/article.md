---
schema_version: "1.0.0"
document_id: "9b92da1beee7e1b688506a8f6fa7c1e6ffd63a1f1e57c7b00c3a714578b59220"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/when-phones-go-down"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-12T07:13:31.135318+00:00"
fetched_at: "2026-08-12T07:13:31.943373+00:00"
content_hash: "sha256:e36155f32354e66d760758a2f396c0d72514cc78ea13d2aa5c8301498dc64a4d"
---

# When the Phones Go Down: The Coordination Gap Nobody Plans For

# When the Phones Go Down: The Coordination Gap Nobody Plans For


Isolation is step one of incident response. Coordination after isolation is the gap most playbooks leave unaddressed.


Aug 11, 2026


·


Blog


·


Jay Goodman


Every incident response playbook covers isolation. Segment the network. Cut external access. Contain the blast radius. These are the right instincts.


But isolation creates a second problem that most playbooks treat as someone else's responsibility: once you have severed the affected environment, how does your team coordinate? If your phones run through the network you just isolated, or if the adversary disrupted telecommunications before you detected the intrusion, your response team is operating in silence at the moment it needs to move fastest.


CISA's infrastructure disruption scenarios explicitly contemplate adversary action against telecommunications as a force-multiplying tactic, designed not just to degrade communications, but to delay, fragment, and misdirect the response. The July 2026 water utility attacks reinforced what those scenarios describe. Utilities that had pre-established out-of-band coordination recovered faster. Those that had not faced extended disruption, with no reliable way to coordinate across sites once digital systems were compromised.


Here’s what an out-of-band coordination layer must include, how it must perform under the conditions of the first six hours, and how to verify it works *before* an incident demands proof.


## Why the First Six Hours Are the Most Dangerous


The period between initial detection and full team mobilization carries the highest risk. Decisions made in that window shape the entire response.


Three conditions converge in that window.


1.


**The scope of the incident is unknown.** Determining whether you face a limited intrusion or a coordinated campaign requires communication across sites, roles, and agencies, and it requires confidence that those communications are not themselves compromised.


2.


**Normal communications infrastructure is suspect.** If an adversary has been inside the network long enough to reach operational technology systems, internal messaging, email, and voice-over-IP cannot be assumed clean. Using a compromised channel to coordinate a response to a compromise is a documented adversary technique for slowing and misdirecting recovery.


3.


**External coordination demands increase sharply** . Regulatory notification timelines begin. Mutual aid partners require situational awareness. Law enforcement and federal agencies need accurate, timely information to support response. All of this must happen through channels that are available and trusted simultaneously.


Most playbooks address the first condition. Far fewer address the second and third with the same rigor applied to technical containment.


## What "Out of Band" Means in Practice


The term "out of band" has been used loosely enough in security planning that it has lost precision. A genuine out-of-band coordination capability has three defining characteristics.


1.


**It operates independently of the infrastructure under incident.** A channel that routes through the same network being isolated is not out of band, regardless of how it is labeled. True independence means separate physical paths, separate identity verification, and separate authentication infrastructure.


2.


**It is available before the incident is detected.** Crisis is the wrong time to discover who has access, which devices are configured, or whether enrollment is complete. Device readiness, policy configuration, and enrollment must be completed and tested in advance.


3.


**** **It is trusted.** During an active incident, the identity of every participant in a coordination session matters. An adversary with knowledge of your response team structure can exploit an unverified channel to introduce false information or redirect resources. Every participant must be continuously verified against a known, controlled identity store.


Independence, availability, and trust define the minimum design standard for a fallback communications layer.


## Three Design Requirements for a Viable Fallback Communications Layer


### Requirement One: Sovereign, Network-Independent Delivery


The fallback layer must operate when primary network infrastructure is unavailable, degraded, or actively hostile. Its delivery mechanism cannot share the same telecommunications paths as standard operations, and it cannot depend on internal servers that may be isolated, compromised, or out of reach.


Practical options include pre-provisioned mobile devices with independent data paths, encrypted voice and messaging applications that function without reliance on internal servers, and deployment configurations that maintain availability when primary infrastructure is unreachable. Cellular-based delivery provides geographic resilience and does not depend on your primary network. For environments with the highest security requirements, air-gapped or on-premises deployments ensure the coordination platform remains beyond the reach of an adversary who has accessed your primary environment.


When evaluating solutions for this requirement, prioritize platforms that are purpose-built for high-security environments. Look for full end-to-end encryption with metadata shielding for both voice and messaging, support for on-premises or air-gapped deployment, and architecture that keeps the coordination channel fully independent from your primary network. When the primary network is under incident, the out-of-band channel must remain available and unaffected, regardless of what is happening on the primary infrastructure.


### Requirement Two: Continuous Identity Verification


During an incident, the identity of every person on a coordination call is an operational question, not a procedural one. Response team structures are not secrets. An adversary with sufficient knowledge of your organization can exploit an unverified channel to impersonate personnel, introduce false situational information, or redirect response resources at a critical moment. The channel that carries your recovery decisions must be able to confirm who is on it.


The fallback layer must enforce continuous identity verification, not just at login but throughout every session. Every participant must be validated against an authoritative, current identity store that your organization controls. Devices must be enrolled, managed, and confirmed as trusted before they can join a crisis session, and any device that cannot be verified should be denied access automatically.


Mobile device management for government environments must do more than enforce policy during normal operations. The platform governing crisis coordination devices must verify current security posture before granting access, bind each session to a confirmed user identity, and support remote management or decommissioning of any device that is lost, stolen, or compromised during an active incident. That governance layer, applied consistently across every device in the coordination environment, is what makes continuous identity verification operationally reliable rather than aspirational.


### Requirement Three: Unified Operational Awareness


Crisis coordination demands more than messaging. It requires shared situational awareness across teams that may be geographically dispersed, working from different information sets, and responding to conditions that are actively evolving. Without a unified picture, incident commanders make decisions based on what they knew at the start of the incident, not what is happening in the field.


The fallback layer must support more than point-to-point messaging. It must provide a shared operational picture: who is responding, where, with what resources, and against what confirmed or suspected conditions. Alerts must reach the right people through the right channels, including personnel who may not be at their primary workstation. Field intelligence must flow back to incident commanders in real time, structured and timely enough to inform decisions rather than add to the noise.


When evaluating platforms for this requirement, look for solutions that unify multi-channel alerting, personnel status tracking, geofencing, and bidirectional field communication into a single operational view. The goal is to give incident commanders real-time awareness of who has responded, who has not, and what conditions look like on the ground, all without depending on the primary communications infrastructure that may be compromised. Platforms that support mandatory structured responses with acknowledgment tracking are particularly valuable: they convert alerts into accountable action rather than broadcasts that may or may not have been seen.


An effective fallback architecture addresses all three requirements together: independent delivery that survives primary network failure, continuous identity verification that confirms every participant throughout a session, and unified operational awareness that gives incident commanders a real-time picture across all responding teams.


## The First Six Hours: A Scenario Walkthrough


### Hour One: Detection and Initial Containment


Operators notice discrepancies in ladder logic across several sites. Investigation reveals modified project files on programmable logic controllers. The incident commander activates the response plan and initiates isolation of affected systems.


If the response team's primary coordination channel routes through the network being isolated, communication drops at this moment. If the team has pre-provisioned out-of-band devices, coordination continues without interruption. A multi-channel alert reaches all designated responders, confirming activation and directing personnel to the out-of-band channel. Device posture is verified for each responder before session access is granted.


### Hours Two and Three: Scope Assessment and External Notification


The response team works to determine the scope of the incident across multiple sites. Cross-site coordination is essential, and it must occur on a channel that is not itself at risk.


The out-of-band layer supports this with encrypted voice and messaging that operates independently of the affected network. Simultaneously, the incident commander initiates external notifications to regulatory bodies and federal agencies through pre-configured out-of-band workflows. Notification timelines are met without requiring manual tracking under pressure.


### Hours Four through Six: Eviction, Recovery, and Sustained Coordination


As the team moves from containment to eviction and recovery, the coordination requirement shifts from rapid mobilization to sustained, structured communication. Field personnel report status through the out-of-band layer, using whatever structured reporting format your incident plan specifies. Incident commanders maintain a unified operational picture by consolidating field reports, tracking personnel status, and updating the shared situational view as conditions evolve.


Throughout this phase, the integrity of the coordination channel depends on maintaining continuous identity verification and device posture controls. No participant should be able to join a session without confirmed, verified identity. Any device that fails a posture check should lose access automatically and immediately.


## A One-Hour Tabletop Exercise: Communications Under Disruption


### Scenario Setup (10 minutes)


Brief participants on the following conditions: your primary internal network has been isolated under active incident response. Primary email, voice-over-IP, and internal messaging are unavailable. A regulatory notification obligation with a four-hour timeline is active. Your out-of-band communications capability is live.


### Exercise Phase One: Initial Coordination (15 minutes)


Activate the out-of-band channel and verify that every designated responder can be reached. Document who reached the channel within five minutes, who required assistance, and who was unreachable. Note any devices that failed posture verification.


Key questions: Who holds authority to activate the out-of-band channel? Is the activation procedure documented and accessible without network access? Can every designated responder authenticate from their current location?


### Exercise Phase Two: Situational Awareness (15 minutes)


Require the incident commander to establish a common operational picture using only the out-of-band layer. Field personnel report status. The incident commander synthesizes a current status for a simulated executive briefing.


Key questions: How long does it take to establish a shared operational picture? What information gaps remain after 15 minutes? What manual steps are required that would be automated in normal operations?


### Exercise Phase Three: External Coordination (15 minutes)


Simulate regulatory notification and coordination with federal agencies using only the out-of-band layer and pre-configured workflows.


Key questions: Are notification templates pre-loaded and accessible without network access? Does the team know the correct contacts and timelines? Is there a designated communications lead for external coordination?


### Debrief (5 minutes)


Document three specific gaps and assign ownership for remediation. Schedule a repeat exercise within 90 days.


## Building Communications Continuity that Holds Under Pressure


The first time a fallback communications system is tested should not be during a live incident. A sovereign, continuously verified out-of-band communications architecture, built on purpose-built sovereign platforms, provides the coordination layer that isolation alone cannot.


The challenge is rarely isolation itself. It's maintaining coordination after isolation has severed the systems people normally rely on. Closing it requires deliberate design, advance provisioning, and verified testing before the phones go down.


A useful starting point: if the primary network became unavailable today, how would your response team coordinate, and how confident are you that those channels would work? That single question will surface most of the gaps this post describes. Use the tabletop exercise above as the starting point, document what you find, assign ownership, and retest within 90 days.


**Related Reading:**
***[Lessons from the July Water Utility Attacks: What the Joint Advisory Tells Operators to Do Next](https://www.blackberry.com/en/secure-communications/insights/blog/lessons-from-july-water-utility-attacks)***
***[Why OT Isolation Planning Needs Communications Continuity](https://blackberrysc-stg.netlify.app/en/secure-communications/insights/blog/why-ot-isolation-planning-needs-communications-continuity)***


Get updates about the latest in-depth knowledge for secure communications.
