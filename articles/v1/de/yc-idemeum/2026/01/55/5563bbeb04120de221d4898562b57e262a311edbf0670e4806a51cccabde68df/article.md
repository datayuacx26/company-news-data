---
schema_version: "1.0.0"
document_id: "5563bbeb04120de221d4898562b57e262a311edbf0670e4806a51cccabde68df"
company_key: "yc-idemeum"
company: "idemeum"
source_id: "yc-idemeum-news-import-e4b4d27057e9"
canonical_url: "https://idemeum.com/blog/the-malware-problem"
published_at: "2026-01-19T00:00:00+00:00"
first_seen_at: "2026-07-25T09:02:12.338313+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:9fef82e0cdcbf8188d3b2438a6fd8505a88f494f74d1582c801b245085a104c1"
---

# The malware problem

## The malware crisis


One topic keeps coming up again and again in security space - we have a major problem in how we handle malware defense.


Despite deploying dozens of tools over the years, security teams are struggling to stop malware. And it’s not surprising: global ransomware attacks against critical industries[surged by 34% in 2025](https://industrialcyber.co/reports/half-of-2025-ransomware-attacks-hit-critical-sectors-as-manufacturing-healthcare-and-energy-top-global-targets/) . Adversaries are no longer leveraging artificial intelligence (AI) just for productivity gains, they are deploying[novel AI-enabled malware in active operations](https://cloud.google.com/blog/topics/threat-intelligence/threat-actor-usage-of-ai-tools) . Legacy defenses fall behind, and[85% of organizations](https://www.crowdstrike.com/en-us/press-releases/ransomware-report-ai-attacks-outpacing-defenses/) report traditional detection is becoming obsolete against AI-enhanced attacks. With AI these trends are likely to continue, and there is no easy way to catch[millions of malware files generated daily](https://www.kaspersky.com/about/press-releases/the-number-of-the-year-kaspersky-detected-half-a-million-malicious-files-daily-in-2025) .


We saw this problem first hand when leading product and engineering at Meta, FireEye, and VMware. There was always playing a catch up game with attackers by curating Threat Intelligence feeds faster, collecting malware samples, reverse engineering them to extract indicators, and detonating samples for dynamic analysis artifacts. It always felt like we were one step behind.


## The cost of getting it wrong


Some companies only have the bandwidth and budget to deploy something like an antivirus - knowing full well that there will be dangerous targeted attacks and malware that will bypass this defense.


Others deploy an EDR or XDR, but these tools also hit the limits of detection-based security. EDR and XDR platforms detect attacks only after malicious activity has already begun, which means payloads, fileless malware, and credential-based intrusions can execute before a response occurs. What is more, modern intrusions routinely subvert detection altogether by leveraging fileless malware and Living-off-the-Land (LOTL) techniques. Once deployed, malicious payloads can take seconds to profile and copy sensitive data or encrypt networks with ransomware.


Defense against cyberattacks is an arms race. The more advanced the attack, the more advanced the detection method must become. The more motivated the attacker, the more bespoke the attack is written against the individual victim. Detection products marketed to respond against common attack patterns cannot protect against hyper-customized attacks from motivated adversaries.


In summary, detection-based security falls short.


## Where do we go from here


EDR and XDR remain a necessary component of a modern cybersecurity stack for their invaluable incident detection and forensics capabilities. They provide deep endpoint visibility by recording processes, memory, and network behaviors to identify and contain anomalies that slip past policy boundaries. Their insights are aggregated with telemetry from identity, cloud, email, and network sources to reveal coordinated attacks across an enterprise.


Organizations cannot rely solely on detection. Prevention must be enforced through allowlisting controls to stop attacks before they can execute. Detection technologies must be combined with application allowlisting to build a comprehensive security ecosystem. Together, the stack not only blocks untrusted actions before they can cause harm but also detects and correlates suspicious events, showing where administrators can add defenses or improve user behavior. Prevent attacks first, then fortify initial attack vectors through the visibility and forensics offered by EDR and XDR.


At idemeum we are dedicated to changing how application control can stop malware. Our goal is to make allowlisting simple and leverage AI to reduce the friction and time spent on managing the app control operations.
