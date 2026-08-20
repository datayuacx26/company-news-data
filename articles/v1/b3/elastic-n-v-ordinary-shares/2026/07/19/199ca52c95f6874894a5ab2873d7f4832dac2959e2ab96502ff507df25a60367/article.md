---
schema_version: "1.0.0"
document_id: "199ca52c95f6874894a5ab2873d7f4832dac2959e2ab96502ff507df25a60367"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026"
published_at: "2026-07-31T23:59:59+00:00"
first_seen_at: "2026-07-31T17:52:30.959585+00:00"
fetched_at: "2026-08-01T00:00:00.779085+00:00"
content_hash: "sha256:90042790e3be80cc6b5261e7d31cf6d34d01896291d1ccdcf40024363910f2af"
---

# Elastic goes all-in on Hacker Summer Camp at Black Hat and DEF CON in Las Vegas

31 July 2026 •


[Jackie McGuire](https://www.elastic.co/security-labs/author/jackie-mcguire)


# Elastic goes all-in on Hacker Summer Camp at Black Hat and DEF CON in Las Vegas


Attack Discovery turns raw alerts into validated threats and Elastic Defend closes vulnerable driver gaps as fast as they're disclosed. Watch it all run against real attacks at the booth.


7 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates) ,


[Detection Engineering](https://www.elastic.co/security-labs/category/detection-engineering)


At Elastic, we know that the best way to build security tools is to bring them to the community, have security pros use them, and let them tell us what features and functionality matter and why. This year, we’re excited to do this at Black Hat and DEFCON, the weeklong security marathon affectionately known as Hacker Summer Camp. Our smartest technical experts and practitioners will be at Black Hat showing off our latest innovations, sponsoring and hosting events to help security experts and leaders connect, and at DEFCON’s Blue Team Village with our new Capture the Flag challenge to help defenders sharpen their investigation skills.


This community-powered innovation is evident in everything we do. Elastic is a security tool built by security users, for security users. We’ve sat in the seat. We’ve worked the queue at 2 a.m. We’ve chased an alert that turned out to be nothing and missed the one that turned out to be everything. What we're continually building and improving is the security operations center (SOC) we wished we'd had back then. This means agents that carry the machine-speed work, leave critical judgment to analysts, and a platform that connects the two.


With Elastic, machine speed and human judgment work together in a single loop. Stopping more at the endpoint reduces the number of alerts. Those that remain surface the real threats, and you can validate them before they reach a queue. The work underneath is increasingly automated. Each piece makes the next one lighter, and none of it asks you to hand judgment over to a black box.


##


Alert Zero: From alert queue to validated threats


Every SOC is chasing a queue worked down to what actually matters, the SOC's version of “inbox zero.” When we built our suite of tools, our goal was[Alert Zero](https://www.elastic.co/security-labs/agentic-soc-alert-triage-alertzero) , a state that always felt out of reach. It’s a goal that teams move toward, with agents and analysts working together. It doesn’t mean zero alerts or replacing the analysts.


###


How Attack Discovery investigates alerts like an analyst


Attack Discovery has always pulled related alerts together into a single view of an attack. Now it goes further, working through them the way a human analyst would:


1. Threat-hunts raw events beyond the initial alerts.
2. Checks entity risk for the users and hosts involved.
3. Corroborates findings across other data sources.
4. Classifies the event as a validated attack.


Your team gets a short list of validated attacks to work, instead of a wall of raw alerts to triage.


###


Closing detection gaps with auto-drafted rules


When Attack Discovery finds something that your rules missed, it drafts a detection rule to close the gap and hands it to an analyst to approve, helping to make the entire workflow more efficient and to reduce the source of false positives.


Security teams need the *how,* not just the *what* , and Attack Discovery shows its work, so you can see how it got to each answer and recommended action. Every step of the reasoning is visible, so an analyst knows why an alert became an attack. You can run it however fits your team, whether you kick it off yourself or set a recurring cadence. You can even trigger it from Elastic Workflows. A separate alert analysis workflow addresses the volume from the other side, differentiating between likely false and true positives, so analysts lose fewer hours to low-fidelity alerts, and leaving Attack Discovery a cleaner set to investigate.


##


Elastic Defend endpoint protection: vulnerable driver coverage and Windows on ARM


Fewer alerts reach the queue when more threats are stopped on the device, so prevention starts at the endpoint.


###


Vulnerable driver coverage that keeps pace with disclosure


Elastic Defend[now gets ahead of vulnerable drivers](https://www.elastic.co/security-labs/vulnerable-driver-detection-elastic-defend-byovd) . Attackers exploit these by bringing a signed, trusted driver with a known flaw and using it to reach the kernel, and coverage for a new one has traditionally arrived on a release cycle. Our[threat research team](https://www.elastic.co/security-labs) monitors public disclosure sources, like VirusTotal, loldrivers.io, and Microsoft's blocklist. Through an always-on process, Elastic automatically generates and instantly deploys YARA rules as new drivers are disclosed, so protection keeps pace instead of waiting on a release. That speed matters when AI-driven attacks can move from one machine to the next in under a minute, faster than any response workflow can react.


###


Full endpoint protection for Windows on ARM


Windows on ARM is now fully covered in Defend, which brings Surface and other ARM-based laptops into the same protection as the rest of your fleet. Teams can roll this feature out across every endpoint without paying per device to do so. A new endpoint troubleshooting skill also rounds out this capability, automatically flagging policy and performance issues so your team spends less time chasing them.


##


Elastic Workflows: SOC automation you can describe in plain language


###


Build automations in plain language with version control


Elastic Workflows makes automations faster to build and shows you exactly what a workflow will do before it runs. The latest updates start with[plain-language authoring](https://www.elastic.co/search-labs/blog/ai-workflow-automation-natural-language) , so you can describe the automation you want and have it generated for you. Versioning then tracks every change, so you can compare any two versions and roll back to a working one in a click. You always know who changed what and when. Visual Mode shows a workflow as a graph, with its triggers, steps, branches, and logic visible at a glance next to the YAML. Drag-and-drop editing is coming next.


###


Human-in-the-loop approvals routed to Slack


Automation you can trust starts with understanding what Workflows will do and when it will ask for help. When a workflow reaches a decision that needs a person or an approval or other input, it pauses and routes the request to a tool your team already uses, like Slack. Automation handles the routine, while your team stays on top of the decisions that need judgment. Workflows runs natively within the Elasticsearch platform extending across search, observability, and security, so it runs where your security data already lives, rather than stitched as a layer on top.


Together, our drive to Alert Zero, enhanced endpoint protection, and automation where your data lives reinforce each other. Stronger prevention keeps alerts from being raised in the first place, and the ones that remain arrive validated instead of raw. Automation underneath keeps prevention and investigation moving at machine speed, while your analysts stay on the decisions that need a human. That’s what the agentic SOC looks like when it’s built to help the people in it rather than replace them.


*"Security teams are not losing because they lack tools; they're losing because the tools generate more work than the team can absorb," said **Mike Nichols, general manager Security, Elastic.** "Elastic Security is built by people who've sat in the SOC and worked the queue. These updates go after one of the biggest sources of analyst burnout, which are alerts that shouldn't be alerts in the first place. We know every barrier is a liability and we’re building an open, transparent platform that breaks down those barriers and makes it easier for teams to customize and manage the security stack they need to protect their organizations."*


##


Find Elastic Security at Black Hat and DEF CON 2026


At the booth, you can see all of this working on real attacks:


- Agents helping cut false positives on the way to Alert Zero, showing their reasoning at every step.
- Security information and event management (SIEM) built for the agentic SOC.
- Extended detection and response (XDR) across cloud, Kubernetes, and endpoint.
- Native automation with Elastic Workflows.
- Endpoint defense that holds across Linux, macOS, and Windows.
- Threat research from Elastic Security Labs that ships in the platform.


Bring your hardest questions.


Elastic is showing up across the week, at Black Hat and around the community.


- **Sober Speakeasy** with Sober in Cyber, an alcohol-free networking evening at the Mob Museum's Underground Speakeasy. Tuesday, August 4, 2026, 7:00–9:30 p.m. PT. All infosec professionals are welcome, sober or sober-curious.
- **Blitz & Defend** , an executive evening with Elastic and AWS at Allegiant Stadium, a behind-the-scenes tour, and reception in the Raiders locker room. Wednesday, August 5, 2026, 6:30-8:30 p.m. PT.
- **GDIT Sip & Cipher** , a cybersecurity networking reception at Toca Madera, with Elastic among the sponsors. Wednesday, August 5, 2026, 6:00–9:00 p.m. PT.
- **InnovatHERs: Women Shaping Tomorrow** , a breakfast with Women in CyberSecurity (WiCyS) to celebrate and connect women in the field. Thursday, August 6, 2026, 7:00–9:00 a.m. PT.
- **Find us at DEF CON, too:** Security people go to Black Hat because they have to. They go to DEF CON because they want to. That's why we're proud to be a Blue Tier sponsor of Blue Team Village at DEF CON this year, the highest-traffic village and home to the SOC, Digital Forensics and Incident Response (DFIR), and incident response communities. It's where defenders come to sharpen their craft, run capture-the-flag, and swap real-world detection and response tactics. For us, showing up there is about being where our people already are, because Elastic Security is built by the same community that fills the room. Come find us.


#### Jump to section


- [Alert Zero: From alert queue to validated threats](https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026#alert-zero-from-alert-queue-to-validated-threats)
- [How Attack Discovery investigates alerts like an analyst](https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026#how-attack-discovery-investigates-alerts-like-an-analyst)
- [Closing detection gaps with auto-drafted rules](https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026#closing-detection-gaps-with-auto-drafted-rules)
- [Elastic Defend endpoint protection: vulnerable driver coverage and Windows on ARM](https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026#elastic-defend-endpoint-protection-vulnerable-driver-coverage-and-windows-on-arm)
- [Vulnerable driver coverage that keeps pace with disclosure](https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026#vulnerable-driver-coverage-that-keeps-pace-with-disclosure)
- [Full endpoint protection for Windows on ARM](https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026#full-endpoint-protection-for-windows-on-arm)
- [Elastic Workflows: SOC automation you can describe in plain language](https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026#elastic-workflows-soc-automation-you-can-describe-in-plain-language)
- [Build automations in plain language with version control](https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026#build-automations-in-plain-language-with-version-control)
- [Human-in-the-loop approvals routed to Slack](https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026#human-in-the-loop-approvals-routed-to-slack)
- [Find Elastic Security at Black Hat and DEF CON 2026](https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026#find-elastic-security-at-black-hat-and-def-con-2026)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Elastic%20goes%20all-in%20on%20Hacker%20Summer%20Camp%20at%20Black%20Hat%20and%20DEF%20CON%20in%20Las%20Vegas&url=https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026&title=Elastic%20goes%20all-in%20on%20Hacker%20Summer%20Camp%20at%20Black%20Hat%20and%20DEF%20CON%20in%20Las%20Vegas)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/elastic-security-black-hat-defcon-2026&title=Elastic%20goes%20all-in%20on%20Hacker%20Summer%20Camp%20at%20Black%20Hat%20and%20DEF%20CON%20in%20Las%20Vegas)
