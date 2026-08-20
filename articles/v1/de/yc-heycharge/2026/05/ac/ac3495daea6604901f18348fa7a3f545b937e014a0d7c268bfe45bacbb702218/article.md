---
schema_version: "1.0.0"
document_id: "ac3495daea6604901f18348fa7a3f545b937e014a0d7c268bfe45bacbb702218"
company_key: "yc-heycharge"
company: "HeyCharge"
source_id: "yc-heycharge-rss-50b4f64ce74c"
canonical_url: "https://www.heycharge.com/news/ev-charging-without-network-access/"
published_at: "2026-05-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.777661+00:00"
fetched_at: "2026-07-28T22:09:21.780117+00:00"
content_hash: "sha256:1d7af90a8923084cded93c35e59332107e69fc95a0f7b92ffabbbc1898e5e94a"
---

# EV Charging That Doesn't Touch Your Network: The Cybersecurity Case for Offline-First Architecture

**Short answer.** For high-security sites — federal facilities, defense contractors, R&D labs, semiconductor fabs, critical-infrastructure operators — the IT-security objection to deploying EV charging usually isn’t price or reliability. It is that conventional EV chargers are unvetted IoT endpoints that demand either a place on the corporate network or a cellular SIM running a rogue radio inside the facility perimeter. Both are non-starters. The standard mitigations (VLAN segmentation, cellular SIMs, air-gapped chargers) each fail on at least one risk axis. HeyCharge’s offline-first architecture removes both attack vectors at the same time: no on-prem network connection, no SIM, no internet connectivity at the charger at all. The charger is not reachable from the internet because it isn’t connected to anything that touches it. This is the architectural change that turns “no IoT on my network” from a policy collision into a non-issue. We are actively engaging US and German government bodies on exactly this fit.


This article is for IT, security, and procurement leaders evaluating EV charging at sites where the IT-security review is the gating step rather than a formality. It explains why conventional EV charging architectures fail that review, why the standard mitigations don’t actually mitigate, how offline-first changes the picture, and what an honest IT/security review of the offline-first architecture finds.


---


## Quick comparison


Conventional


Cellular charger Conventional


Network-attached charger Offline-first


Adds device to corporate LAN No


Yes


No


Adds cellular radio inside facility Yes


No


No


Reachable from the internet Yes


Indirect via LAN


No


Fits sensitive / classified-adjacent sites No


No


Yes


---


## The IT-security objection


EV charging at office parking, fleet depots, and visitor lots is increasingly required by procurement policy, sustainability mandates, and fleet electrification timelines. For most operators, the charger procurement decision is a financial and operational one. For a class of customer the EV charging industry has largely overlooked, it is a security one.


These are organizations whose IT teams hold a default position of *“no untrusted devices on our network, ever.”* They are federal civilian agencies and defense-adjacent commercial sites. They are research labs whose intellectual property is the entire reason the building exists. They are semiconductor fabs where the cost of a downtime event is measured in wafers per minute. They are utilities, hospitals, ports, and airports — sites where the regulatory consequence of a network compromise is significant on its own and the operational consequence is much worse.


For these customers, an EV charger is not just an asset. It is a request — a demand, really — that an unvetted, unmanaged piece of OT/IT equipment be granted access to either the corporate LAN or RF spectrum inside the perimeter. That request collides with the most basic IT-security policy these organizations have. The default answer is no.


The result is that EV charging at these sites either does not happen, happens slowly and expensively after a multi-year integration project, or happens via a workaround that the IT team is privately uncomfortable with. None of those are good outcomes.


## How conventional EV charging fails the IT review


### Failure mode 1: the charger as another untrusted IoT endpoint


A network-attached EV charger is, from the IT department’s perspective, indistinguishable from any other IoT device — a vendor-supplied embedded computer running a vendor-supplied firmware stack, expecting outbound internet access for cloud management, OCPP back-end communication, billing reconciliation, and over-the-air firmware updates. The vendor expects to ship security patches when convenient. The vendor expects the device to be reachable from the internet at session start to authenticate the driver, settle the payment, push the meter values, and return the receipt.


For a site with even a moderately mature security posture, that is a sequence of escalating violations:


- The device is on the corporate LAN.
- The device has outbound internet access.
- The device receives unsupervised firmware updates from a third-party cloud.
- The device’s vendor has a published vulnerability history that will keep growing.
- The device and its peers form a botnet target if compromised — Mirai- and Mozi-style malware specifically targets unattended IoT endpoints with stable network presence.


The standard response is network segmentation: put the chargers on their own VLAN, allow only the specific outbound destinations required, deny everything else. This works only as long as the segmentation never drifts. It is well documented that multi-year IoT deployments accumulate config drift, and the consequence of any single mis-patch — a router config change, a firewall rule rotation, a forgotten service rule — is that the chargers’ VLAN now sees more of the network than it should, or the chargers now see more of the internet than the policy says. VLAN segmentation reduces blast radius. It does not eliminate the risk; it defers it to whenever the configuration next moves.


### Failure mode 2: the charger as a rogue cellular radio inside the perimeter


The standard alternative to *“put it on our network”* is *“give it a cellular SIM.”* Vendors package this as a feature: the charger ships with a 4G/5G modem, the operator pays the data fees, IT is not involved.


For a site that controls the RF spectrum inside its perimeter, this is worse, not better. A SIM-equipped charger is by definition a device with an active cellular radio capable of bidirectional traffic to a third-party network the site has no visibility into and no control over. The site has now installed an asset whose communication path is a black box. From a TEMPEST, classified-spaces, or simply-disciplined-IT standpoint, that is unacceptable. Many sensitive sites explicitly prohibit unmanaged cellular endpoints inside the perimeter for exactly this reason.


The second-order problem is coverage. Cellular signal in underground parking — the most common EV charging deployment environment — is generally absent. The charger that ships with a SIM but no signal is offline by accident, not by design. It cannot be relied on for anything.


### Failure mode 3: the firmware update path


Even the most security-conscious deployment has to update the charger firmware over its lifetime. For most EV chargers, this happens over the air: the vendor cloud pushes a signed firmware image, the charger reboots, the new firmware is in place. The IT department has, in effect, delegated a continuous root-of-trust update to a third party.


This is the supply-chain attack vector that has been the explicit subject of congressional testimony, BSI advisories, and the IoT Cybersecurity Improvement Act. The fact that a single compromised vendor cloud, or a single misissued signing key, can push attacker-controlled firmware to thousands of devices is not theoretical. It has happened. It will keep happening. For the customer profile we are describing, *“the vendor will push you firmware updates whenever they like”* is a non-starter.


## Why the standard mitigations fail


Three mitigations dominate the conversation when an IT/security team is asked to accept a connected EV charger. None of them solves the underlying problem.


**VLAN segmentation.** This is the most commonly proposed mitigation. The charger goes on its own VLAN, with explicit allow-listed destinations and deny-by-default for everything else. This is sound network practice and does limit blast radius. It does not change the fact that the device is on the corporate network, that its firmware is updated unsupervised, that it is reachable from the internet via its sanctioned outbound paths, and that any future configuration drift weakens the boundary. VLAN segmentation is harm reduction, not problem elimination.


**Cellular SIMs.** Solving the on-LAN problem by moving to cellular replaces one risk with another, often worse, one. As above, an active cellular endpoint inside a sensitive perimeter is itself a security event in many threat models. And in the deployment environments where EV charging is most needed — basements, underground garages, dense urban buildings, military and R&D campuses with deep RF shielding — cellular signal is not reliable enough to function as the primary communication channel anyway.


**Air-gapped chargers.** Some vendors offer fully offline chargers — no LAN, no SIM, no cloud. This satisfies the IT-security requirement at the cost of every operational property an actual charging operator needs: no remote authentication, no centralized billing, no fleet visibility, no firmware update path, no driver authorization tied to identity rather than RFID. For a single charger at a remote facility, this is acceptable. For an actual EV charging deployment of 50, 100, or 1,000 spots across multiple buildings, it is not.


The pattern across all three mitigations is the same: each forces a trade-off between security and operability. The operator either accepts a security compromise to keep the charger functional, or accepts an operational compromise to keep the charger secure. There is no architecture in the conventional cloud-connected EV charging stack that gives them both.


## The offline-first architecture


HeyCharge’s[SecureCharge platform](https://www.heycharge.com/technology) starts from a different assumption: the charger should never be on a network the operator’s IT team is responsible for, and should never have a cellular radio attached. Four properties move in lock-step from there.


**The charger has no internet connectivity, period.** No SIM. No Ethernet uplink. No WiFi connection to the building network. The charger is, by deliberate design, not reachable from the internet at all. This is the load-bearing architectural commitment everything else rests on.


**Authentication runs locally on the charger.** When a driver arrives, an authorization handshake completes between the driver’s phone (or a fleet-issued token) and the charger. The protocol is built on industry-standard asymmetric cryptography with hardware-backed secure key storage on the charger, and uses patented single-use, session-bound tokens that fail closed against replay and substitution. **BLE is the transport that carries the handshake, not the security mechanism** — the cryptographic guarantees are incremental to whatever Bluetooth Low Energy already provides at the link layer, not dependent on it. There is no remote attack vector because there is no remote connection.


**The driver’s phone is the courier.** Session metadata, billing data, audit logs, and configuration changes move between the charger and the HeyCharge platform asynchronously, riding on the driver’s phone via the HeyCharge app. Every session generates a signed record on the charger; that record is uploaded by the next driver phone that connects to it; the platform reconciles. This is the same store-and-forward pattern used in tactical military radios, satellite-relay logistics, and offline-first banking — well-understood engineering, not an improvisation.


**The supply-chain attack vector loses its delivery channel.** With no internet at the charger, there is no automated path through which a compromised vendor cloud could push attacker-controlled firmware. Operators who want supervised, scheduled updates can have a HeyCharge-authorized technician apply each one over the service port; operators who want OTA-style updates can have them ride the asynchronous BLE/courier channel. Either way, the customer stops having to care about what software the charger is running — the device is no longer an attack vector regardless of its firmware version.


What the four properties give you, taken together:


- There is no IP traffic between the charger and any network the operator owns.
- There is no IP traffic between the charger and any network at all.
- There is no cellular radio inside the facility perimeter.
- There is no internet-reachable firmware delivery channel that could be compromised at the vendor and propagated to the device.
- The session authentication channel is short-range, single-use, and cryptographically signed.


The charger is, in the most literal sense, not reachable from the internet at all.


## What the IT review actually finds


For the customer profile we have been describing, the IT review of an offline-first EV charging deployment is unusual mainly in how short it is.


**Communication paths.** The charger has exactly two: BLE to driver phones, and a service-port interface used only by an authorized technician. There is no third channel. The review confirms this against the device specification and the deployed configuration.


**Network exposure.** Zero. The device does not connect to the corporate LAN, the guest WiFi, the building automation system, or any cellular network. It does not have a routable address on any network the operator manages. There is no inbound surface, no outbound path.


**Firmware update governance.** Operators choose the update model. Sites that want firmware pinned can require a HeyCharge-authorized technician to apply each update on a scheduled, witnessed basis via the service port; sites that don’t can let updates ride the asynchronous BLE/courier channel. Either way, there is no internet-reachable update endpoint on the charger. The supply-chain risk that drives most IoT firmware concerns simply doesn’t have a delivery vehicle.


**Audit logs.** Every session generates a signed, timestamped record on the charger itself. Records are uploaded asynchronously via driver phones and reconciled on the platform side. For audit purposes, the charger-side record is the ground truth; platform-side reconciliation is a convenience layer. Operators who require local-only audit (no upstream replication) can configure the platform to discard reconciled records after a defined retention period.


**Supply chain.** The hardware bill of materials is documentable. The cryptographic primitives are industry-standard and reviewable. Penetration testing is feasible because the attack surface is small and well-defined: the cryptographic authentication protocol, the service port, and the signed firmware update flow.


The shape of the review converges on a small number of bounded questions, each of which has a definite answer. That is unusual for IoT.


## Regulatory fit


The current direction of cybersecurity regulation in both the US and the EU is converging on the same posture for IoT: known supply chain, bounded attack surface, supervised firmware update path, and minimum-necessary network exposure. The IoT Cybersecurity Improvement Act of 2020 codified this for federal procurement in the US. NIS2 and the EU Cyber Resilience Act extend the posture across the EU. The BSI’s TR-03183 work, and the ETSI EN 303 645 baseline, name the same controls. None of these frameworks require offline-first specifically. All of them get easier to comply with when the device in question simply does not have the network connectivity the regulation is concerned about. We are not making a compliance argument here — we are making an architectural argument that happens to also align well with what the regulators are converging on.


## Who should consider this


The offline-first architecture is operationally suited to most large EV charging deployments. It is *uniquely* suited to the deployments where conventional architectures fail the IT-security review:


- **Federal civilian and defense-adjacent commercial sites.** Sites subject to procurement policies prohibiting unvetted IoT, unmanaged cellular, or vendor-cloud-dependent infrastructure on the perimeter.
- **Defense R&D and classified-adjacent facilities.** Where the controlling principle is “no third-party network presence inside the boundary, no exceptions.”
- **Critical infrastructure operators.** Utilities, ports, airports, hospitals — sites where regulatory consequence of a network compromise is operationally and reputationally high.
- **Semiconductor fabs and other high-IP commercial environments.** Where the cost of an industrial-espionage event dwarfs the cost of the EV charging deployment.
- **Diplomatic, intelligence, and sensitive-private deployments.** Where the operating principle is *no published vendor cloud should know which chargers are at this address.*


The common thread is that “no untrusted IoT on my network” is the IT department’s default position, EV charging is a recently-required priority, and the two teams are in active conflict over how to deploy.


We are actively engaging both US and German government bodies on exactly this fit. We do not yet name customers. The point of this article is not to claim deployments — it is to describe the architectural fit so that the right teams can recognize themselves in it and reach out.


## FAQ


**Does the charger ever connect to the internet?** No. The charger has no SIM, no Ethernet uplink to the internet, and no WiFi connection to the building network. It is not reachable from the internet at all. Asynchronous data exchange with the HeyCharge platform happens via the driver’s phone, which acts as a courier between the charger and the cloud.


**How does the charger get software updates?** Whichever supervised path the operator wants. Sites that pin firmware can require an authorized HeyCharge technician to apply each update on a scheduled, witnessed basis via the service port. Sites that don’t can let updates ride the asynchronous BLE/courier channel. The architecturally important point is upstream of the update mechanism: there is no internet-reachable update endpoint on the charger, so the supply-chain attack vector that drives NIS2, the IoT Cybersecurity Improvement Act, and BSI advisories has no delivery channel here.


**Doesn’t this rely on Bluetooth’s security?** No. BLE is the transport layer, not the security mechanism. Authentication uses industry-standard asymmetric cryptography — signed challenges and patented single-use, session-bound tokens — with hardware-backed secure key storage on the charger. The cryptographic protocol is engineered so that whatever security BLE provides at the link layer is incremental, not load-bearing. The same protocol would run identically over any short-range carrier.


**What if Bluetooth itself is restricted at our site?** For sites that restrict driver phones from carrying RF radios, we support fleet-issued physical authentication tokens with the same cryptographic guarantees, used in place of the phone. The store-and-forward path then runs through dedicated courier devices that are visitor-policy-compatible.


**How do we audit usage and billing without a continuous network connection?** Every session generates a signed, timestamped record on the charger itself. Records are reconciled asynchronously when a driver phone or fleet token uploads them. For local-only audit, the charger-side record is canonical and platform-side reconciliation can be discarded after a configurable retention period. The audit trail is complete, signed, and verifiable.


**Can we run penetration testing against the deployment?** Yes. The attack surface is well-bounded — cryptographic authentication protocol, service port, signed firmware update flow — and we can provide the documentation a serious pen-test engagement needs. We’ve scoped this with US and German government counterparts in early-stage conversations; we don’t yet have a public pen-test report to share, and we’d welcome the engagement.


**Can you name a customer in this segment?** Not yet. We are in active conversation with US and German government bodies on exactly this fit. The same factors that make the architecture suitable for these customers — minimum vendor-cloud knowledge, no published deployment topology — also mean that named customer references are unlikely to ever exist in this segment. We expect this to be true of every credible vendor working in this space.


**What is the deployment timeline?** For sites in the customer profile above, we ask for a confidential briefing first. The deployment itself is materially faster than conventional EV charging because the IT-network and cellular-coverage steps that consume most of the project schedule are not required.


---


## The bottom line


For most operators, EV charging is a procurement decision driven by cost, reliability, and installation speed. For a real and growing class of customer — federal, defense-adjacent, R&D, critical infrastructure — it is a security decision. The conventional EV charging architecture forces these customers to accept either an unvetted IoT device on their corporate network or a rogue cellular radio inside their perimeter. Neither is acceptable. The mitigations don’t actually mitigate.


HeyCharge’s offline-first architecture removes both attack vectors at the same time. No on-prem network. No SIM. No vendor cloud reaching into the facility. The charger is not reachable from the internet at all. This is the architectural change that turns “no IoT on my network” from a policy collision into a non-issue.


If your team is in this position,[request a confidential security briefing](https://www.heycharge.com/contact?intent=security-briefing) . We will walk through the architecture, the threat-model assumptions, the audit-log path, and the procurement options under NDA. No public references, no deployment-topology disclosure, no attribution.


**Further reading:**


- [SecureCharge: how the offline architecture works at the protocol level](https://www.heycharge.com/technology) — for IT/security teams that want to start with the technical detail.
- [EV Charging in Underground Garages Without Internet](https://www.heycharge.com/news/ev-charging-underground-garages-without-internet/) — the deployment-environment companion piece for sites where conventional connectivity also fails for non-security reasons.


— *Chris Cardé, CEO & Co-Founder · Robert Lasowski, CTO & Co-Founder*
