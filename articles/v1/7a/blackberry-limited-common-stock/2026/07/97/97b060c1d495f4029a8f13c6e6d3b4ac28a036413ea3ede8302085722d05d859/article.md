---
schema_version: "1.0.0"
document_id: "97b060c1d495f4029a8f13c6e6d3b4ac28a036413ea3ede8302085722d05d859"
company_key: "blackberry-limited-common-stock"
company: "BlackBerry Limited"
source_id: "blackberry-limited-common-stock-news-import-2149b884f544"
canonical_url: "https://www.blackberry.com/en/secure-communications/insights/blog/email-spreadsheets-wont-survive-mythos-era"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-23T03:42:51.442589+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:49f0d6ad25d8182ce370602676aaf72924059d89ad4bbb34051eb125f4fdb994"
---

# Email and Spreadsheets Won't Survive the Mythos Era

# Email and Spreadsheets Won't Survive the Mythos Era


Why federated organizations are increasingly vulnerable to coordination failure — and what the July 2024 outage revealed.


Jul 14, 2026


·


Blog


·


Secure Communications


## The Problem Isn't Detection


In April,


[Anthropic disclosed Claude Mythos and chose not to release it](https://www.nbcnews.com/tech/security/anthropic-project-glasswing-mythos-preview-claude-gets-limited-release-rcna267234)


.


The decision itself was notable. The reason was more so.


According to Anthropic's published evaluations, Mythos can autonomously discover and exploit zero-day vulnerabilities across every major operating system and web browser. In controlled testing, it converted browser vulnerabilities into working exploits 72 percent of the time—compared with near-zero success rates for the prior generation.


The broader trend points in the same direction. Independent threat intelligence shows[average attacker breakout time at 29 minutes in 2026](https://www.crowdstrike.com/en-us/global-threat-report/) , a 65 percent improvement from 2024, while AI-assisted attacks have surged 89 percent year over year.


Most discussion has focused on what these numbers mean for detection. Fair enough. Detection matters.


But detection is no longer the hardest problem.


The bigger challenge is operational: how organizations turn an alert into coordinated action across dozens, hundreds, or even thousands of people. That layer—the people, decisions, communications, and dependencies wrapped around incident response—has quietly been held together by email, spreadsheets, and recurring status calls for years.


Those tools aren't disappearing. The assumptions that made them workable are.


## The Coordination Gap


During a major incident, the process is usually familiar. An alert is raised. Emails start flying. A spreadsheet appears. A standing bridge call gets scheduled.


Before long, the response exists in four places at once, and nobody is entirely certain which version reflects reality.


We've seen global response efforts hinge on a single Excel file. Three people updated it. Twenty stared at it. Everyone hoped it was accurate.


None of this reflects a lack of competence. These processes evolved because they were practical. They worked well enough for the environment organizations faced at the time.


When remediation timelines stretched across days or weeks, the friction was manageable. A status survey that took six hours to compile was annoying, not existential. Organizations had enough time to absorb delays, reconcile conflicting updates, and gradually build a common operating picture.


That math is changing.


When attackers can move from compromise to privilege escalation in minutes, a coordination cycle measured in hours stops being a coordination cycle. It becomes part of the exposure.


The issue isn't the vulnerability itself. It's the gap between operational tempo and organizational response.


A process designed around email distribution lists, manual status collection, and periodic meetings was never built for a threat environment moving at machine speed. The same vulnerabilities that once allowed for multi-day remediation windows may now be weaponized before the third recipient has opened the email.


This isn't a hypothetical future scenario. The people building and studying these systems are describing exactly this shift.


At the same time, Mythos didn't create the trend. It simply makes the trajectory harder to ignore.


## The Warning We Already Received


Defenders already got a glimpse of this future in July 2024.


The[CrowdStrike-triggered global outage](https://www.reuters.com/technology/cybersecurity/crowdstrike-update-that-caused-global-outage-likely-skipped-checks-experts-say-2024-07-20/) is often remembered as a faulty software update with unusually broad consequences. What it really exposed was something more fundamental: many organizations were forced to coordinate complex recovery efforts while portions of their normal communications and IT infrastructure were unavailable.


For hours—and in some cases days—distributed teams were making critical decisions without access to systems they typically depended on.


Some organizations adapted surprisingly well. They already had communications architectures capable of reaching people through channels independent of the systems under failure. They gathered structured status updates instead of relying on free-form email replies. They sustained coordination throughout the recovery rather than rebuilding situational awareness at the start of every call.


Others struggled because their coordination model depended on the very systems that had become unavailable.


The lesson had little to do with CrowdStrike specifically.


It was a warning about what happens when operational coordination becomes the weakest link.


## Where Coordination Breaks Down First


Federated organizations face the sharpest version of this challenge.


National governments with autonomous jurisdictions. Healthcare networks made up of independent institutions. Financial services groups with affiliated entities. Multinational organizations that operate through semi-independent regional structures.


These environments share a common characteristic: authority is distributed.


Headquarters may have visibility but limited control. Individual entities may have authority but limited visibility beyond their own boundaries. Informal mechanisms often fill the gaps between them, which is precisely why email threads, spreadsheets, and recurring calls become the default coordination layer.


In a faster threat environment, those gaps become harder to tolerate.


## Building for the Mythos Era


The requirements for addressing them aren't particularly mysterious.


First, the coordination layer cannot depend on the systems under attack.


Second, status reporting must be structured. Broadcast messages create ambiguity, and ambiguity becomes expensive when every minute matters.


Third, executive and government-level decision makers need trusted communications that remain available even when primary infrastructure cannot be trusted.


None of this can be assembled in the middle of an incident. The architecture has to exist before the event begins.


Regulatory frameworks such as NIS2 already point in this direction through requirements related to coordinated response, resilience, and continuity of essential services. In many respects, the regulations are catching up to conditions organizations are already experiencing.


The risk is assuming compliance alone is enough.


Some organizations will meet every reporting requirement and still discover that their coordination model breaks under pressure. An architecture can satisfy an audit and fail an incident at the same time.


That distinction matters more than it used to.


### The Real Question


Email and spreadsheets haven't suddenly become bad tools. They've become slow ones.


For years, organizations could compensate for that limitation because the threat environment moved slowly enough to accommodate it.


Increasingly, that assumption no longer holds.


CrowdStrike revealed what happens when coordination becomes difficult. Mythos points toward a future where coordination becomes the determining factor.


The question is no longer whether organizations can detect an attack.


The question is whether they can coordinate a response before the attacker achieves their objective.


Get updates about the latest in-depth knowledge for secure communications.
