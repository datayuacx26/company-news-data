---
schema_version: "1.0.0"
document_id: "ebe3ec41764deb2d6c145f133824d7cc3f0dc756b706c0ed1f3b0538953ae5d9"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/space-cybersecurity"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:faa9e758a3e0e18130ef046a9e6aa0bae0d15f3c1572f6b2b0b355176761faff"
---

# Space is now critical infrastructure, but we're not securing it like it is

# Space is now critical infrastructure, but we're not securing it like it is


By


[Marcial Villegas](https://www.elastic.co/blog/author/marcial-villegas)


July 14, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


In the early hours of February 24, 2022 — the same morning Russian tanks crossed into Ukraine — a piece of malware called AcidRain was already running. By the time anyone understood what was happening, 5,800 satellite modems across Europe had been permanently bricked. Wind farms lost remote management, emergency services went dark, and Ukrainian military units lost coordination capability in the critical opening hours of the invasion.


The remarkable thing about this attack is what it


*didn't*


do. It didn't require proximity to a satellite, didn't jam signals, and didn't knock anything out of orbit. It exploited a satellite ground station using the same firewalls, VPNs, and identity systems most enterprise IT teams manage every day; and from there, it reached out and silenced a constellation.


The defenders watching from their operations centers had no idea what they were seeing because their tools simply had no language for it.


## A new kind of infrastructure, an old kind of problem


Space has quietly become load-bearing infrastructure. Financial systems rely on GPS timing signals accurate to billionths of a second. Agricultural sensors beam crop data through low-earth-orbit constellations. Emergency responders coordinate through satellite links when terrestrial networks fail. Multiple nations now classify space as an operational warfighting domain alongside land, sea, air, and cyber.


Yet, the cybersecurity posture protecting this infrastructure hasn't kept pace with its criticality.


According to the


[Satellite Industry Association's 29th annual State of the Satellite Industry Report](https://sia.org/affordability-productivity-drive-historic-satellite-industry-growth-satellite-industry-association-releases-29th-annual-state-of-the-satellite-industry-report/) , produced by BryceTech and released in May 2026, at the end of 2020, roughly 3,400 satellites were operating in Earth’s orbit. By the end of 2025, that number had surpassed 14,000, representing a fourfold increase in five years. These aren't monolithic, purpose-built spacecraft. Modern space missions are software-defined, API-connected, and deeply integrated with cloud platforms and enterprise identity systems. They receive over-the-air updates and authenticate operators using the same identity providers your company uses for its corporate VPN.


In other words, the space domain has inherited all the attack surface of modern enterprise IT and then layered an entirely new attack surface on top — one that almost no security tooling was built to see.


## 4 domains, one attack path


Conceptual architecture for mission segments


To understand why space cybersecurity is so difficult, consider how a modern space mission actually works.


-


**User segment:**


Operators and administrators connecting through VPNs and identity providers, which is standard enterprise territory


-


**Ground segment:**


The mission operations center, antenna control units, cryptographic key management systems, and the APIs that tell the spacecraft what to do


-


**Link segment:**


Where RF uplink and downlink communications use protocols that predate the commercial internet and that no general-purpose security tool was designed to read


-


**Space segment:**


The spacecraft, generating a continuous stream of telemetry from its power systems, thermal controls, attitude determination, and propulsion


An adversary doesn't need to compromise all four segments to do damage; they only need one entry point. The


[Viasat attack](https://cyberconflicts.protect.ngo/law-and-policy/cases/viasat) shows the fragility of these interconnected layers: it didn't touch a single satellite; it ran entirely through the ground and user segments, exploiting a misconfigured VPN appliance and a trusted credential to find a path from familiar enterprise IT into specialized satellite command infrastructure. From there, it took less than a day to brick equipment across an entire continent.


In this multilayered, multidomain environment, anomaly detection is complex, going beyond detecting a single anomaly in one segment. Instead, teams need to detect and correlate weak signals across all four segments fast enough to prevent physical impact before a command-flooding attack completes, before an unauthorized firmware upload is accepted, and before a rogue ground station establishes contact during a pass window.


## The blindspot hiding in plain sight


Here's what makes this especially alarming: The data to detect these attacks often


*exists*


. The problem is that no security platform can read it.


When AcidRain was running, there were signals that included firmware-wipe sequences, network anomalies, changes to satellite command streams, and RF link degradation. But there were also questions that couldn't be answered like


*was the signal drop an attack, a system failure, or a scheduled maintenance window? Was the anomalous command legitimate or injected? Was the RF degradation from jamming or a solar weather event?*


Answering those questions requires correlating data across all four segments simultaneously. This data can include structured telemetry from the spacecraft, cryptographic authentication records from the command channel, RF signal quality metrics from ground receivers, antenna control logs, space weather data from NOAA, and maintenance records sitting in a ticketing system somewhere.


A traditional security platform sees the IT side of this picture clearly but goes dark the moment data crosses into the RF link and the spacecraft. This isn't a failure of the tools so much as a failure of the


*maps*


. Space telemetry uses protocols, including international standards like Consultative Committee for Space Data Systems (CCSDS), Space Data Link Security (SDLS), and Packet Utilization Standard (PUS), that are rigorously defined but never translated into a format security monitoring platforms understand. Nobody built a translation layer, so the data streams in, gets stored, and produces no actionable signals whatsoever.


Think of it like air traffic control radar that only covers the eastern half of the sky. You can warn about everything you see, but you have no idea what's approaching from the other side until it's already crossed.


## Context is the missing ingredient


There's a second problem that’s quieter but equally important.


Even when you can see an anomaly, you need to know what it


*means*


. A spike in bit error rate could be RF jamming, or it could simply be the spacecraft at a low elevation angle where atmospheric interference is expected. An unusual thruster firing could be an unauthorized maneuver or a scheduled orbital avoidance burn that someone forgot to log.


Answering these questions doesn't necessarily require more telemetry; it usually just requires


*context*


, such as the shift notes the previous operator team left at handoff, the mission planning documents showing scheduled maneuvers, the space weather report for that orbital window, and decades of anomaly history for this specific spacecraft and subsystem. This context exists and is scattered across wikis, ticketing systems, email threads, and shared drives, but it's almost never connected to the security workflow. When an analyst is triaging an incident in real time, they're consulting six different systems manually while the clock runs.


For space operations, that clock runs faster than most people realize. A satellite passes over a given ground station for perhaps 10 minutes per orbit, which means that if the attack window is measured in minutes, the triage window is measured in seconds. An analyst manually cross-referencing four separate systems loses those seconds every time.


*Mission context collapse*


is when a single incident spans identity systems, RF degradation, orbital anomalies, and spacecraft health simultaneously. Traditional security tools break down here because they were built for attacks that happen within a single operational domain. Space attacks don't respect those boundaries.


## The shape of a solution


The path forward isn't complicated to describe, even if so far it's been difficult to execute.


At the foundation, space cybersecurity needs a common schema: a standardized way of representing space mission data that security tools can actually read. This should not be a bespoke solution for one mission, but an open standard that any operator, constellation, government program, or commercial venture can adopt and contribute to. Space cybersecurity cannot mature as a discipline if every operator is solving the same foundational problems in isolation.


The framework for cataloguing the attack surface already exists. The Space Attack Research and Tactic Analysis (


[SPARTA](https://sparta.aerospace.org/) ) framework is like the MITRE ATT&CK framework for the space domains. Sponsored by DHS and developed by Aerospace Corp, SPARTA documents 260 attack techniques spanning every segment of a space mission, including RF jamming, command injection, firmware corruption, unauthorized uplink via rogue ground stations, and cryptographic key theft. It's a rigorous, well-maintained body of work, but before this year, exactly zero of those 260 techniques had a working detection rule that a space operator could import and actually run. That's the gap between a threat framework and operational security: a catalog that is respected and widely cited but for practical purposes is useless because the maps stop short of the territory.


Once a common schema exists, detection rules become shareable and threat intelligence becomes portable. When one operator figures out how to detect a novel command injection technique and describes it using the SPARTA schema, that detection is usable by every other operator running the same schema, so the community builds collective defenses instead of solving the same problems in isolation over and over.


## A discipline finding its footing


Space cybersecurity is a young discipline operating on an old problem, namely securing infrastructure that was designed for performance and reliability before it was designed for security. The protocols underpinning space communications were not written with adversaries in mind. And the ground systems commanding spacecraft were built by engineers solving the problem of getting a mission to work, not the problem of stopping someone else from interfering with it.


That's not a criticism so much as a description of where the discipline stands and a signpost for where it needs to go. The schemas that didn't exist are being built. The detection rules that didn't exist are being written. The frameworks that catalogued threats without enabling detection are being operationalized, and space cybersecurity is beginning to look less like an afterthought and more like the serious engineering discipline that critical infrastructure demands.


Space is the newest critical infrastructure domain, and while the adversaries already understand that, the defenders are catching up.


This is where Elastic fits in. Elastic's platform is built to unify fragmented data at scale, and space security is fundamentally a data unification problem: telemetry from spacecraft, logs from ground systems, RF metrics from antenna controllers, and unstructured context from operator notes and mission documents all need to be searchable together


*in real time*


by the people responsible for keeping a mission safe.


By extending the Elastic Common Schema into the space domain and mapping open detection content to the SPARTA framework, Elastic gives these signals a shared language; so, weak indicators scattered across all four segments can be read as one picture instead of four disconnected ones. That common schema is the foundation everything else builds on. It is what makes detections shareable, threat intelligence portable, and correlation fast enough to matter inside a pass window. This is where the same schema lets an agentic SOC — the model Elastic's GM of Security Mike Nichols lays out in


[The AI arms race in cybersecurity: Why your SOC needs to evolve now](https://www.elastic.co/blog/ai-cybersecurity-arms-race-agentic-soc) — act on those correlated signals at machine speed. The schema comes first. Everything else, including the speed to respond, follows from getting that foundation right.


This is the first in a series on operationalizing SPARTA and securing space infrastructure. More to come.


## Learn more about Elastic and cybersecurity


To learn more about how public sector organizations are approaching their cybersecurity strategies, download the


[Cybersecurity Guide for Public Sector](https://www.elastic.co/industries/public-sector/cybersecurity-guide-public-sector) .


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
