---
schema_version: "1.0.0"
document_id: "b14867bccb61ce209bb4996d5ecf4149b4e2f7b10508354de8a15341846c3e82"
company_key: "netskope-inc-class-a-common-stock"
company: "Netskope Inc."
source_id: "netskope-inc-class-a-common-stock-rss-c0a3e1ef9778"
canonical_url: "https://www.netskope.com/blog/world-cup-retrospective-analyzing-the-surge-in-cyber-threats"
published_at: "2026-07-22T16:48:22+00:00"
first_seen_at: "2026-08-20T03:53:11.718071+00:00"
fetched_at: "2026-08-20T03:53:13.170789+00:00"
content_hash: "sha256:7100c6c6e84f6f13ee292e8106b63a46cef8252873c8a6ad2c536aef7b56a985"
---

# World Cup Retrospective: Analyzing the Surge in Cyber Threats

High-profile events consistently attract opportunistic attackers, and the 2026 FIFA World Cup was a prime example of this trend. Because the tournament generated massive global interest and traffic, it created a perfect environment for hackers to exploit unsuspecting users. They used various techniques, from simple social engineering to complex malware delivery schemes, tailored to tap into the surge of traffic from users looking for anything related to the World Cup. Netskope observed a spike in users attempting to visit malicious content with World Cup themes corresponding to the start of the tournament and continuing through the tournament’s end. At its peak, the number of users attempting to access malicious World Cup content was nearly 6-times the pre-tournament average. Over the course of the tournament, Netskope detected and stopped more than 28,000 World Cup-themed threats spanning more than 1,000 organizations worldwide.


Figure 1: Line graph showing the number of alerts and users from March to July 20, 2026


## Examples


The following are three examples of the most common threat types throughout the tournament.


**Phishing / Credential Scams**


To trick users into providing their credentials, attackers used a wide range of social engineering tactics. Perhaps the most interesting were fraudulent job postings, which required victims to log in to apply and included fake pages to join a videoconference for the interview, as shown in the image below. The ultimate objective of these campaigns was to harvest account credentials.


Image 1: A fake meeting page from FIFA from a fake job hiring domain


**Fake streaming websites**


As fans scrambled for ways to view matches, many inevitably sought out unauthorized platforms offering free access. Adversaries capitalized on this demand by launching fake streaming portals designed to siphon traffic for secondary motives or implement deceptive payment gateways that trick enthusiasts into purchasing bogus subscriptions. Attackers successfully used SEO techniques to get their fake streaming sites listed favorably on popular search engines, which were the main drivers of traffic.


Image 2: Fake streaming portal for FIFA games Image 3: After clicking the play button, it would prompt for payment


**File-based malware**


Attackers even spread malware that promised to help its victims secure World Cup tickets or stream the matches. The payloads were typically commodity infostealers that would harvest credentials and other sensitive data from their victim’s computers.


Image 4: Virustotal analysis for a “WorldCup_Tickets_Viewer” file


## Conclusion


High-profile events such as the World Cup serve as a lucrative bait for attackers, because a single bait can attract a wide variety of victims across the world. Attackers use the event as bait to drive traffic to malicious websites, facilitate financial scams, and spread malware. The activities observed during the World Cup serve as a reminder to remain vigilant against offers that seem too good to be true: free tickets, free streams, dream jobs, etc. Organizations can help ensure a safer browsing experience during future high-interest global events by deploying robust security guardrails that inspect web content in real time to block potential threats.
