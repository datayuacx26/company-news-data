---
schema_version: "1.0.0"
document_id: "d3175690e89490c205dbe7a048e1a977dad3aee42d5f2ca8b347e1c63bcc38e6"
company_key: "yc-codeant-ai"
company: "CodeAnt AI"
source_id: "yc-codeant-ai-news-import-b68a5af7b5b5"
canonical_url: "https://codeant.ai/blogs/the-penetration-test-you-only-pay-for-when-it-finds-something"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-24T18:23:22.105025+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:5f9b8a820e15b923a74b6905c1daad01fee1319168190715fff9c2602e7ed0f2"
---

# The Penetration Test You Only Pay For When It Finds High & Critical Findings

Most[penetration testing](https://codeant.ai/pentesting) works the same way. You hire a firm, they bill you for the hours their researchers spend, and you pay upfront. Whether or not they find a critical issue, the invoice is the same.


That model is not built to find dangerous[vulnerabilities](https://codeant.ai/vulnerability-database) . It is built to deliver a signed report in time for a SOC 2 or HIPAA audit. Both sides optimize for the deadline, and security becomes the thing that happens on the way to the sign-off.


In 2026, that trade is no longer worth making. Attackers move faster and hit more targets, so the point of a pentest is to protect yourself, and the model should reward finding the issues that matter.


This piece covers what changed. Why the hours-based model is broken, what outcome-based testing rewards instead, and how[black box, white box, and gray box penetration testing](https://www.codeant.ai/blogs/black-box-vs-white-box-vs-gray-box-penetration-testing) actually work under continuous, code-aware testing.


> **What CodeAnt AI solves here:**[CodeAnt AI](https://www.codeant.ai/) runs continuous, code-aware pentesting and charges for outcomes rather than hours. It reads your code, cloud, and external surface, chains the findings into real attack paths, and you pay only for confirmed, critical, exploitable issues. It also one of those vendors offering 48-hour delivery.


## Why the Penetration Testing Business Model is Broken


The standard engagement prices researcher time. You agree on a number of hours, pay before the work starts, and the fee holds whether the test surfaces a critical data leak or nothing at all.


That structure quietly sets the wrong incentive. A firm paid for time has no reason to go deeper than the scope, and the natural goal becomes producing a clean, on-schedule report an auditor will accept.


So the whole exercise optimizes for speed to a signature. You get a document that satisfies a control, the vendor gets paid on time, and neither party is actually rewarded for end-to-end security. That is the gap outcome-based testing was built to close.


## Outcome-based penetration testing rewards findings over hours


[CodeAnt](https://codeant.ai/) changed the costing model when it entered pentesting in late 2025. You are not billed for how long researchers spend. You are billed on whether the test finds a high-end, critical, exploitable issue.


The rule is simple. If it finds a critical data leak and you see the value, you pay. If it does not, and you agree there is no critical exploitable exposure, there is no charge. Findings that only fill a report and add no real risk do not count.


That structure changes how the vendor behaves. When payment depends on finding a genuine critical leak, the incentive is to go deep on every single engagement, because a shallow test that finds nothing earns nothing.


The phrase for it on the[pentesting side](https://www.codeant.ai/pentesting) is "no working exploit, no payment." It is a different business model for the industry, and it lines the tester's incentive up with the only outcome that matters to you.


## Why Continuous Penetration Testing Replaced the Annual Engagement


Automated,[continuous pentesting](https://www.codeant.ai/blogs/best-continuous-pentest-tools-ci-cd) is not a convenience. It is a response to how fast the threat side moved.


Models keep getting more capable, and attackers use them to hit one company after another, extract data, and extort a payment. That pressure pushed testing cadence from annual to quarterly, monthly, and increasingly per feature.


The speed gap is stark. The average eCrime[breakout time at 29 minutes](https://www.crowdstrike.com/global-threat-report/) , and IBM put the average breach at[4.88 million dollars](https://www.ibm.com/reports/data-breach) . A test that runs four times a year cannot cover a surface that changes every week.


A purely manual test cannot keep that pace either. Validating a modern company means understanding how code ships, how developers push changes, which vulnerabilities and exposed secrets already exist, and how the network, cloud, and external surface actually look.


That only works when one tool builds connectivity across every layer, from the full SDLC through the cloud. That is the shape of[continuous penetration testing](https://www.codeant.ai/blogs/best-continuous-pentest-tools-ci-cd) . It continuously maps where the issues are, what threats they create, and how those threats chain into the attack paths that end in a data leak.


## Black Box, White Box, and Gray Box Penetration Testing


Access model decides what a test can find. The[three types of pentesting](https://www.codeant.ai/blogs/types-of-penetration-testing) approaches differ in how much the tester knows going in, and the strongest testing uses all three together.


### Black box penetration testing, the attacker's outside view


Black box is how an adversary sees you from the outside, with no access and no source code. If your network cannot be penetrated from there, no external attacker sees your code at all.


The process runs in stages:


1.


Reconnaissance. Map everything exposed about the company: open ports, CVEs on the network, IP ranges, vendor risk, and leaked credentials sitting on the open internet, plus which threat actors are actively probing you now


2.


Service discovery. Enumerate the services running on that surface


3.


Reachability. Work out what is actually reachable, because the exposed surface is usually far larger than expected


4.


Data exploitation. Chain the vulnerabilities, threats, and root causes together into attack paths that reach real data, a phase that runs for hours on its own


5.


Manual validation. Researchers re-validate every finding and push the chains further by hand


6.


Evidence. Collect the proof needed for SOC 2, ISO 27001, and HIPAA


In the house analogy, black box is the front door left open. If the door is open, anyone can walk in, which is what a penetrable network amounts to.


### White box penetration testing, the full-codebase threat model


White box goes through the entire codebase. It reads every route, every API call, and every database call, then builds a threat model from what it finds.


That model spans the code and everything around it: current application issues, third-party dependency risk, infrastructure weaknesses, exposed secrets, and SBOM concerns. It is then paired with the cloud, including every misconfiguration, a weak WAF layer, exposed network and storage layers, and whatever else is running there.


The real value is in the connections. White box threat-models how internal applications talk to the cloud and what happens in between, which surfaces things like an unauthenticated API call reaching a database directly with no rate limiting. Anyone who penetrates the network can see that call too, and that context simply is not visible from the outside.


This is why the strongest approach integrates into the whole SDLC, understanding the code from the moment a developer writes it, through pull requests and CI/CD, and refining the threat model on every commit. The[defensive and offensive sides](https://www.codeant.ai/blogs/defensive-vs-offensive-security) share that same view of the code.


In the house analogy, white box is being able to map the layout of rooms you are not allowed to enter. You can see where the locks are and where people sleep, all from inside the walls.


### Gray box penetration testing, outside reach with inside knowledge


Gray box combines the two. You hold the external attacker's view from black box and the internal threat model from white box, then you go outside the network and come back in using what you know.


A concrete example makes it clear. Suppose a public API call looks harmless, but you know from the code that it triggers a downstream internal API, one that is never exposed publicly and makes a database call. Standing outside as an adversary, you make the public call knowing the internal path exists, and you use it to reach and corrupt the database.


That is what protects a company from both external and internal threat actors. The best teams assume an attacker will eventually combine outside access with inside knowledge, and they test for exactly that.


In the house analogy, gray box is someone inside opening a window instead of the door. From the window, an adversary sees how everything is arranged, then uses that knowledge from outside to plan the way in.


## How CodeAnt Runs All Three On Shared Intelligence


Running black, white, and gray box well takes one system that sees every layer, because the three approaches only compound when they share the same intelligence.


That is the design.[CodeAnt](https://codeant.ai/) integrates across the[SDLC](https://www.codeant.ai/white-paper/how-artificial-intelligence-is-transforming-software-development) , the cloud, the network, and the external surface, so the codebase knowledge from white box feeds the outside-in attack in gray box, and the whole thing runs continuously rather than once a quarter.


The output is evidence, not a list of maybes. Findings arrive with a working proof of concept and the code path behind them, human researchers validate and extend the chains, and you get[audit-ready evidence](https://www.codeant.ai/blogs/ai-pentesting-compliance) for SOC 2, ISO 27001, and HIPAA. Because it is outcome-based, you pay for the critical issues it proves, and nothing else. It also one of those vendors offering 48-hour delivery.


## The Bottom Line On Outcome-Based Penetration Testing


The hours-based model rewards a signed report on schedule. That is why so many pentests optimize for the audit instead of the attacker, and why a clean engagement can still leave a critical data leak sitting in production.


[Outcome-based, continuous pentesting](https://codeant.ai/pentesting) rewards the opposite. The vendor only wins when it finds a genuine critical exposure, the test runs continuously instead of once a quarter, and black, white, and gray box all run on one shared view of your code, cloud, and surface.


That is the model[CodeAnt](https://codeant.ai/) runs, testing your surface from the outside with knowledge of your code from the inside, and charging only for the critical, exploitable issues it can prove. It also one of those vendors offering 48-hour delivery.


Ready to see what a real attacker could reach?[Launch a free black box scan](https://codeant.ai/pentesting) for one URL, then[book a walkthrough](https://codeant.ai/sales) to see code-aware white and gray box testing against your own stack.
