---
schema_version: "1.0.0"
document_id: "647a0dc11c8a7603bba877873fd168bbf6ec36fd8f76fced67b9bcf30d2ed10f"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/russian-webmail-espionage/"
published_at: "2026-07-23T14:10:53+00:00"
first_seen_at: "2026-07-23T14:22:59.818071+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:a8921976746da6e93ccd081324beb8f864cd32d3174fa387f2b2544e89fe73bb"
---

# Russian Global Webmail Espionage

## Executive Summary


Unit 42 has observed a persistent cyberespionage campaign we track as CL-STA-1114. This activity cluster overlaps with activity from a Russian threat actor tracked by other vendors as Void Blizzard and LAUNDRY BEAR.


The attackers behind this campaign targeted Zimbra webmail in organizations in the following sectors:


- Governments
- Defense
- Transportation
- Financial organizations across the following regions:


- NATO member states
- Ukraine
- Commonwealth of Independent States (CIS) countries
- Africa


Unique to this campaign, the group leveraged zero-click phishing emails that exploit a vulnerability in the Zimbra Collaboration Suite (ZCS) webmail platform (CVE-2025-66376). The exploit automatically injects a malicious JavaScript payload without requiring recipient interaction. Once executed, the payload exfiltrates sensitive user data, including login credentials, email archives, and search histories. Threat actors continue to actively target unpatched ZCS instances using CVE-2025-66376.


Palo Alto Networks customers are better protected from the threats discussed above through the following products:


- [Cortex Advanced Email Security](https://www.paloaltonetworks.com/cortex/advanced-email-security)
- [Advanced URL Filtering](https://docs.paloaltonetworks.com/advanced-url-filtering) and[Advanced DNS Security](https://docs.paloaltonetworks.com/dns-security)


If you think you might have been compromised or have an urgent matter, contact the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) .


**Related Unit 42 Topics** **[Cyberespionage](https://unit42.paloaltonetworks.com/tag/cyber-espionage/) ,[Phishing](https://unit42.paloaltonetworks.com/tag/phishing/) ,[Data Exfiltration](https://unit42.paloaltonetworks.com/tag/data-exfiltration/)**


## Technical Analysis


The attackers behind CL-STA-1114 have been active[since at least 2024](https://www.microsoft.com/en-us/security/blog/2025/05/27/new-russia-affiliated-actor-void-blizzard-targets-critical-sectors-for-espionage/) , and this campaign targeting Zimbra servers started in July 2025. Initial access starts with a phishing email that contains either an HTML attachment or embedded HTML in the message text. This lure is designed to catch recipients' attention with news headlines.


Figure 1 shows an example of the lure used and a snippet of the underlying HTML code.


Figure 1. Example lure and a snippet of its underlying HTML content.


The HTML text contains an obfuscated division with a Base64-encoded script (highlighted in red in Figure 1). The obfuscated section creates an invisible Scalable Vector Graphics (SVG) element that, upon loading, decodes the Base64-encoded script into a JavaScript payload that it injects into the victim’s browser.


When executed, this JavaScript exfiltrates the victim’s Zimbra webmail data to a hard-coded command and control (C2) server. Exfiltrated data includes:


- CSRF tokens
- Email address and password
- Two-factor authentication (2FA) scratch codes
- System and environment details
- The victim’s last 90 days of email and search history


Over the course of this campaign, we observed minimal changes to the JavaScript payload.


Figure 2 illustrates the attack chain.


Figure 2. The attack chain.


Since we began tracking this campaign, there have been at least nine IP addresses and nine domains for the C2 servers. These servers were active for an average of 35.4 days. See the Indicators of Compromise (IoC) section for a list of the IP addresses and domains used in CL-STA-1114 activity.


## Conclusion


This campaign activity in CL-STA-1114 illustrates the persistent and evolving threat of state-sponsored cyberespionage. The attacker behind this activity targets widely used mail platforms like Zimbra, posing a risk to critical industries globally.


This research highlights the need for vigilance, proactive patching and advanced threat detection to protect organizations. Network administrators, defenders and security researchers should patch vulnerable systems and use the IoCs below to investigate and strengthen defenses against CL-STA-1114 and similar activity.


- The[Cortex Advanced Email Security](https://www.paloaltonetworks.com/cortex/advanced-email-security) module routes suspicious HTML attachments to[Advanced WildFire](https://www.paloaltonetworks.com/network-security/advanced-wildfire) for static and dynamic analysis. This ensures that by the time an endpoint opens the attachment, it has already been scanned, allowing the agent to immediately act on a known verdict.
- [Advanced URL Filtering](https://docs.paloaltonetworks.com/advanced-url-filtering) and[Advanced DNS Security](https://docs.paloaltonetworks.com/dns-security) identify known domains and URLs associated with this activity as malicious.


If you think you may have been compromised or have an urgent matter, get in touch with the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) or call:


- North America: Toll Free: +1 (866) 486-4842 (866.4.UNIT42)
- UK: +44.20.3743.3660
- Europe and Middle East: +31.20.299.3130
- Asia: +65.6983.8730
- Japan: +81.50.1790.0200
- Australia: +61.2.4062.7950
- India: 000 800 050 45107
- South Korea: +82.080.467.8774


Palo Alto Networks has shared these findings with our fellow Cyber Threat Alliance (CTA) members. CTA members use this intelligence to rapidly deploy protections to their customers and to systematically disrupt malicious cyber actors. Learn more about the[Cyber Threat Alliance](https://www.cyberthreatalliance.org/) .


## Indicators of Compromise


### IP addresses


- 37.120.247\[.\]228


- 64.226.124\[.\]190


- 104.248.134\[.\]194


- 185.86.79\[.\]95


- 193.238.152\[.\]66


- 194.156.103\[.\]193


- 216.252.238\[.\]18


- 216.252.238\[.\]64


- 216.252.238\[.\]104


### Domains


- analyticemailmeter\[.\]com


- emailanalytics\[.\]com\[.\]ua


- istc-cloud\[.\]com


- mailnalysis\[.\]com


- synacorzimbra\[.\]nl


- zimbra-metadata\[.\]com


- zimbrastat\[.\]com


- zimbrasoft\[.\]com\[.\]ua


- zmailanalytics\[.\]com


## Additional Resources


- [Operation GhostMail: Russian APT exploits Zimbra Webmail to Target Ukraine State Agency](https://www.seqrite.com/blog/operation-ghostmail-zimbra-xss-russian-apt-ukraine/) – Seqrite Blog
- [AIVD and MIVD identify new Russian cyber threat actor](https://www.aivd.nl/site/binaries/site-content/collections/documents/2025/05/27/aivd-en-mivd-onderkennen-nieuwe-russische-cyberactor/Advisory+AIVD+en+MIVD+Public+report+on+new+cyber+actor.pdf) – AIVD/MIVD
- [New Russia-affiliated actor Void Blizzard targets critical sectors for espionage](https://www.microsoft.com/en-us/security/blog/2025/05/27/new-russia-affiliated-actor-void-blizzard-targets-critical-sectors-for-espionage/) – Microsoft
