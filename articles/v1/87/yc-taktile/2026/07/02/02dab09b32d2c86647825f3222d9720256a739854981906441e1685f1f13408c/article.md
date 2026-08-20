---
schema_version: "1.0.0"
document_id: "02dab09b32d2c86647825f3222d9720256a739854981906441e1685f1f13408c"
company_key: "yc-taktile"
company: "Taktile"
source_id: "yc-taktile-news-import-01317932feb4"
canonical_url: "https://engineering.taktile.com/blog/adapting-offensive-security-for-the-ai-agent-age/"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-24T03:38:54.472601+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:ddb9af99d4092e7310b380b748dd2dafa4d8b432ab96e2989bf6fb3456457eac"
---

# Adapting offensive security for the AI agent age

Jul 14, 2026 ·


3 min read


# Adapting offensive security for the AI agent age


---


Security has been one of the biggest sources of anxiety as AI agents get smarter and more capable. Frontier labs like Anthropic have resorted to conditioning access to their most capable models in an effort to prevent malicious actors from using them for nefarious cybersecurity activities.


At Taktile we’ve been using these new capabilities to make our product safer: from security vulnerability detection to remediation. We’ve also noticed more and more security products previously based on deterministic scanners make the jump to AI and leverage it to assess impact and find new vulnerabilities.


Some of these products risk adding to the existing noise, however. Engineering teams are already overwhelmed with output from multiple security scanners generating hundreds of tickets a week, and some of these tools are stapling a non-deterministic layer on top of a very old technology: static scanners.


This is, in my view, the wrong approach to leveraging AI.


An AI-automated penetration test can quickly turn into just another SAST if offered no guidance - yet it is precisely in reducing noise that AI is at its best.


With frontier labs shipping[goal-driven loops](https://code.claude.com/docs/en/goal) , I realized that treating security assessments like a Capture the Flag event was where the real value lay: shifting from SAST/DAST-powered security scanning to penetration testing.


## Pentesting versus Security Scanning


As an analogy, we can think of SAST/DAST scanning as[trawling](https://en.wikipedia.org/wiki/Trawling) and pentesting as[trolling](https://en.wikipedia.org/wiki/Trolling_(fishing)) . One will use a wide net to catch all sorts of fish in an area of ocean, but in that quest it will also cause unintended damage to the biosphere and drag trash aboard.


Penetration testing aims to find a way past your security perimeter, by any means necessary. If we can’t exfiltrate private information, then too bad. No “hardening findings”, no “defense in depth”, and no hypothetical findings. We just need a realistic path from an external threat, through internal system compromise, to real impact such as compromised PII.


With that in mind, we combine the power of goal-driven loops with the context available to us in the form of source code and architecture.


## Building an AI Hacker


We've adapted our resident AI Agent at Taktile, dubbed "Claudius" and made a spin-off personality called "Black Hoodie Claudius". Then, supplied it with a simple completion goal:


> *“ find one single high-severity security vulnerability. “*


In order for it to achieve that goal, we need to supply the agent with a definition of “high severity”: what roles are low and high privileged, what resources are critical, and we tighten the focus towards cross-tenant vulnerabilities.


The fundamental part of this approach is the cadence: a single vulnerability.


The beautiful thing about AI agents in security is that they can finally free security engineers and developers from assessing impact across hundreds of entries from a SAST/DAST by focusing their effort on a single vulnerability. We chose to run this weekly as it allows this vulnerability to be consumed by security engineers, developers and stakeholders, and ship a fix before the next one is found the following week.


In an age of fatigue caused by endless AI output that is cheap to produce but expensive to consume, we choose to use these tools as pure signal generators.
