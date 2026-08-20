---
schema_version: "1.0.0"
document_id: "5be6d1863e78a0062670ee38c5c88bafe761a92f780a8d5ed345019dff153364"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/why-ot-isolation-planning-needs-communications-continuity"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T19:23:46.300773+00:00"
fetched_at: "2026-08-04T20:28:10.240209+00:00"
content_hash: "sha256:6956fd4e9c53ed9df4c00f6199da56cdf9b376a089ace245c3ead3f805ca5820"
---

# Why OT Isolation Planning Needs Communications Continuity

# Why OT Isolation Planning Needs Communications Continuity


When regulators direct you to disconnect, your organization must still lead. OT isolation is a technical capability; sustaining coordination through it is an operational one.


Aug 4, 2026


·


Blog


·


Secure Communications


The joint[CI Fortify guidance published by CISA and Australia's Cyber Security Centre on July 28, 2026](https://www.cyber.gov.au/business-government/secure-design/operational-technology-environments/ci-fortify/ci-fortify-advice-for-isolating-vital-systems) gives critical infrastructure operators a clear directive: build the capability to isolate vital OT and OT-enabling systems from all other networks. The technical case is sound. Isolation limits an adversary's ability to pivot, escalate, and cause cascading damage across interconnected systems.


OT isolation focuses on systems such as HMIs, PLCs, SCADA environments, engineering workstations, and supporting OT networks. But its operational effects extend further. Once isolation begins, response teams, leadership, field personnel, and external partners still need trusted ways to coordinate decisions, share status, escalate issues, and maintain continuity.


For many organizations, this creates a communications continuity requirement: a trusted coordination layer that remains available when primary infrastructure is unavailable, compromised, or intentionally disconnected. BlackBerry® Secure Communications solutions can support that layer as a complement to OT isolation planning.


The joint CI Fortify guidance reflects a broader shift in how regulators and security authorities view OT risk. Isolation is no longer treated as a contingency measure reserved for worst-case scenarios; it is becoming a core operational discipline.


The guidance is specific about what organizations must prepare:


-


Identify your vital systems


-


Map your connections


-


Build physical isolation points


-


Test your graduated isolation plan


-


Monitor effectiveness throughout


What it does not resolve in detail is the question leaders face the moment isolation begins: *How do people coordinate, communicate, and maintain command when normal systems are unavailable or no longer trusted?*


That question points to a weakness network diagrams and firewall configurations cannot close on their own. It is also more common than many organizations acknowledge.


## The Confidence-Capability Gap in Crisis Response


According to the[BlackBerry 2026 State of Secure Communications study](https://www.blackberry.com/en/secure-communications/business/secure-communications-2026-report)


, 90 percent of security leaders report confidence in their crisis management capabilities. Yet only 49 percent have a unified platform to coordinate crisis response.


The findings expose a structural vulnerability, not a minor discrepancy.


When OT environments are isolated, automated data flows and some IT-enabled coordination paths may be disrupted. Manual processes take over, and response teams need alternate ways to coordinate decisions, share status, report field conditions, and manage external dependencies. As CI Fortify notes, organizations must plan to operate in this state “for an extended period.”


Confidence without capability is not resilience. It is risk that has not yet been tested.


## What CI Fortify Requires Beyond the Network Plan


CI Fortify lays out a multi-step path to isolation: identify vital systems and critical customers, map connections between critical and non-critical networks, build physical separation and isolation controls, and create a graduated plan that can be executed as the threat environment deteriorates.


Each step is technical, but the guidance also acknowledges the human and operational work required to execute it.


Consider this passage: organizations must address "removing external dependencies for isolation or initial recovery, verifying service level agreements for prioritized support, and pre-establishing emergency communication for normal staff, as well as any remaining third-party dependencies."


The phrase *pre-establishing emergency communication* matters because it shows that isolation planning cannot stop at network architecture. CI Fortify does not prescribe a secure communications platform, but it does require organizations to decide how people will coordinate when normal channels are unavailable, compromised, or no longer trusted.


The guidance also notes that state-sponsored cyber actors target critical infrastructure “to pre-position for disruptive and destructive effects in the event of crisis or conflict.” In other words, the adversary may already be inside the perimeter, observing dependencies and communication patterns before isolation begins.


## How to Sustain Coordination Through an OT Isolation Event


The steps below translate CI Fortify’s technical directives into operational practice: what to plan and test before an isolation event, and how to sustain coordination, device governance, and secure communications once it begins.


### Step 1: Establish Out-of-Band Crisis Coordination


Before an isolation event occurs, designate an out-of-band coordination platform that does not traverse the networks you plan to isolate. Pre-register the personnel, roles, and external contacts who must be reachable, and configure alerting workflows so isolation automatically reaches the right people through the right channels. ***BlackBerry*** ***®*** ***AtHoc*** ***®*** ***supports this through simultaneous multi-channel delivery across SMS, email, voice, desktop notifications, and mobile applications.***


Define escalation paths in advance: who receives the initial alert, who is notified if acknowledgment is not received within a set threshold, and who has authority to escalate to cross-agency contacts. Enable bidirectional reporting so field teams can confirm status, report conditions, and flag emerging issues back to command. Test these workflows before an incident and document the sequence in your offline isolation plan.


Verify that every active isolation role has a registered, tested out-of-band contact path. For distributed sites, confirm reachability across locations before an event. Document who holds authority at each stage of the isolation sequence and ensure those individuals are enrolled, verified, and reachable through channels outside the networks being taken offline.


Apply the same rigor to external dependencies. Before an event, identify every organization your operations depend on during isolation, including peer utilities, scheduler operators, emergency services, regulators, and critical third-party vendors. Confirm out-of-band contact paths, designate named points of contact, and document escalation protocols that do not rely on the networks you intend to isolate. ***BlackBerry AtHoc maintains these cross-organization communication pathways outside a compromised or isolated network perimeter, so coordination can continue even when internal infrastructure is fully offline.***


### Step 2: Maintain Device Compliance and Access Control Through Isolation


***Before isolation begins, configure a secure MDM solution such as BlackBerry*** ***®*** ***UEM to enforce baseline device compliance policies regardless of network state.*** Define which devices are authorized to access sensitive systems, enroll all personnel with active isolation roles, confirm their devices meet your organization’s security requirements, and revoke access for any device that falls out of compliance. Establish a remote wipe protocol for lost, stolen, or compromised devices, and store policy configurations and device enrollment records in a format accessible offline.


CI Fortify warns that isolated systems may fall out of patch and that removable media used for updates may not receive adequate screening. Configure your UEM solution to flag devices that have not received updates within a defined threshold, document how removable media will be validated before use, and assign a named role to monitor patch status throughout the event.


In hybrid or partially isolated environments, apply conditional access policies that enforce the same compliance baseline across the device estate. Confirm that UEM policy enforcement does not depend on isolated network segments, block unverified or non-compliant devices from reaching sensitive systems, document which access policies apply at each stage of a graduated isolation event, and assign a named role to monitor compliance status.


### Step 3: Secure Voice and Messaging Channels Before Isolation Begins


Start by auditing the voice and messaging channels your operations and response teams rely on, then identify which ones depend on carrier-provided, externally managed, or otherwise non-sovereign infrastructure. CI Fortify urges organizations to account for external dependencies that could undermine isolation or recovery; communications planning should follow the same discipline.


For each channel, assess whether it can be rerouted through an out-of-band path that does not depend on the networks being isolated, or whether it should be replaced with a sovereign-controlled alternative for the duration of the event. Document which personnel depend on those channels, assign alternatives before an incident occurs, and confirm those alternatives are enrolled, tested, and reachable. Store this mapping in your offline isolation plan so it remains accessible when primary infrastructure is unavailable.


Where a channel depends on carrier-provided or externally managed infrastructure, replace it with a sovereign-controlled alternative that can operate independently of the networks being isolated. It should provide end-to-end encrypted voice and messaging, avoid infrastructure your organization does not control, and meet the certification standards relevant to your operating environment. ***BlackBerry*** ***®*** ***SecuSUITE*** ***®*** ***meets NSA CSfC and NATO Restricted certification requirements and operates on infrastructure under your organization’s direct control, making it a validated option for this role.*** Enroll personnel with active coordination responsibilities, confirm reachability through your out-of-band network path, and test the platform before an incident requires it.


Before the isolation event concludes, assess what communication patterns may reveal. Even without message content, adversaries can use call logs, participant identities, timing patterns, and communication frequency to infer organizational structure, decision-making rhythms, and operational priorities. ***BlackBerry SecuSUITE helps shield this metadata throughout the event by concealing call records, masking participant identities, and suppressing traffic pattern visibility.*** Assign a named role to verify that metadata protection controls are active before secure voice and messaging channels are brought online and remain in force across every communication session.


## Putting the CI Fortify Guidance Into Practice


Many organizations have invested heavily in detection, perimeter hardening, segmentation, and incident response tooling. Communications continuity — the human coordination infrastructure that helps teams operate through isolation — has received less structured attention.


That is not a failure of intent. Crisis communications planning is harder to scope, harder to test, and harder to assign ownership than network architecture work. The result is that technical isolation capability can outpace the operational readiness to sustain it.


CI Fortify shifts the baseline expectation for critical infrastructure operators. When CISA and ASD jointly direct organizations to build isolation capability, they are signaling that the threat environment has advanced to a point where isolation is no longer a scenario to plan for in theory. It is an operational state to prepare for in practice.


Organizations that have not yet stress-tested their coordination and communications capability against a realistic OT isolation scenario should treat this guidance as a prompt to do so.


Treat communications continuity as a supporting component of your isolation plan, not a follow-on consideration. Map coordination dependencies with the same rigor you apply to network architecture, and resolve them before an incident requires you to depend on them.


For many organizations, that means planning three supporting capabilities in concert: a pre-established coordination channel that remains available independent of isolated networks, a governance mechanism that keeps response devices verified and access controlled throughout the event, and a trusted voice and messaging path that adversaries cannot easily monitor or disrupt. Each capability should be planned, enrolled, and tested before an isolation event occurs. None should be improvised under pressure.


## Translating the CI Fortify Guidance Into Operational Action


-


**Establish out-of-band communication channels** that do not traverse the networks you intend to isolate. BlackBerry AtHoc and BlackBerry SecuSUITE can support this capability across multiple communication modes.


-


**Pre-establish emergency contacts and communication protocols** for staff, peer operators, and third-party dependencies before an incident forces you to improvise.


-


**Enforce device compliance and access controls** throughout the isolation period using BlackBerry UEM to prevent compromised endpoints from becoming a secondary attack vector.


-


**Conduct isolation plan testing** that includes communications and coordination scenarios, not only technical network separation exercises.


The CI Fortify guidance recommends testing isolation plans regularly and storing secure offline versions. Your communications continuity plan deserves the same discipline.


## Communications Continuity Makes OT Isolation Operationally Sustainable


Isolation is a defensive posture. Coordination is what makes it operationally sustainable.


The gap between the 90 percent of leaders who feel confident in crisis management and the 49 percent who have the platform to execute it is not a knowledge gap. It is a capability gap, and it becomes visible precisely when the pressure is highest.


BlackBerry AtHoc, BlackBerry UEM, and BlackBerry SecuSUITE can help organizations coordinate crisis response, maintain endpoint governance, and protect trusted voice and messaging during an isolation event.


Organizations that treat communications continuity as an afterthought to OT isolation are solving only part of the problem. The ability to disconnect matters most when teams can coordinate through the disconnect. As isolation moves from contingency planning into standard operational practice, the communications layer deserves the same rigor, governance, and readiness discipline as the network architecture it supports.


Get updates about the latest in-depth knowledge for secure communications.
