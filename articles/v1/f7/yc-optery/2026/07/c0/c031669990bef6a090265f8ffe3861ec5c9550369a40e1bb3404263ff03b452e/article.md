---
schema_version: "1.0.0"
document_id: "c031669990bef6a090265f8ffe3861ec5c9550369a40e1bb3404263ff03b452e"
company_key: "yc-optery"
company: "Optery"
source_id: "yc-optery-rss-6e764af78e78"
canonical_url: "https://www.optery.com/multi-channel-phishing-marketer-targeted-job-lures-fake-interpol-emails/"
published_at: "2026-07-23T13:51:00+00:00"
first_seen_at: "2026-07-23T14:07:09.975986+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:45006a5d944fae6d9575d2d1b31aeb3beffb9a4259718080a8cbbac15e87f7d8"
---

# Multi-Channel Phishing, Marketer-Targeted Job Lures, and Fake Interpol Emails

Welcome to *The Optery Dispatch* — a newsletter delivering the latest insights on threat intelligence and proactive cybersecurity strategy. In Issue #15, published July 21, 2026, we cover:


- Social engineering is spreading across workplace channels, exposing the limits of email-only defenses.
- A campaign targeting marketers shows how attackers combine role-aware job lures, trusted platforms, nested redirects, and fake sign-in pages to make credential phishing harder to detect.
- Fake Interpol investigation emails show how fear, urgency, and authority can be used to push small businesses into opening ransomware hidden inside layered archive files.


## **The Multi-Channel Social Engineering Threat**


**Phishing is no longer only an email security problem**


ITPro reports that phishing is expanding across Microsoft Teams, Slack, Zoom, cloud platforms, calendar invites, and other workplace tools. KnowBe4 found that Microsoft Teams-based phishing attacks rose 41% between October 2025 and March 2026, showing how attackers are moving into trusted collaboration channels where employees are used to responding quickly. The article notes that this shift requires organizations to stop treating phishing as only an email security problem.


The article focuses on stronger authentication, user training, endpoint controls, and better visibility across collaboration tools and browsers. Those are important controls, but one major prevention layer is missing: reducing the exposed personal data attackers use to identify, reach, and profile employees in the first place.


As we noted in our[2026 Enterprise Social Engineering Report](https://www.optery.com/2026-optery-enterprise-social-engineering-survey-report/) , unlike channel-specific controls, the protective benefit of reducing publicly exposed employee data is not limited to a single channel. Personal data removal helps address targeted social engineering across all channels because it reduces the data attackers need regardless of delivery method.


Read more:[Multi-channel phishing attacks: How to manage the risk | IT Pro](https://www.itpro.com/security/multi-channel-phishing-attacks-how-to-manage-the-risk)


## ****Attackers Target Marketers With Big-Brand Job Lures****


**A role-aware social engineering campaign used trusted platforms and nested redirects to make phishing links harder to detect**


Dark Reading reports that attackers are targeting marketing professionals with fake recruiting messages impersonating major brands including Coca-Cola, Louis Vuitton, McKinsey & Company, Netflix, OpenAI, and FIFA. The campaign uses legitimate platforms, fake job interview links, nested redirects, and fake Google sign-in windows to evade detection and steal credentials.


The nested redirect tactic is especially challenging to defend against. Instead of sending victims directly to a phishing site, the campaign routes them through multiple legitimate services. Victims were first sent through ExactTarget, a Salesforce subsidiary, then redirected to Wise Agent, a real estate-focused CRM platform, and finally sent to a phishing site hosted on Netlify. Because the first links point to legitimate services, the malicious destination can be harder for victims and basic email or web filters to detect.


Nested redirects through legitimate services are designed to build trust with the victim, bypass basic filters that only inspect the first linked domain, and allow attackers to rotate parts of the redirect chain when one link is detected or blocked. A Salesforce spokesperson described abuse of legitimate services to route malicious traffic as an industry-wide challenge.


This campaign is a clear example of role-aware social engineering built to evade standard defenses. The messages addressed individuals by name and targeted people working in a relevant field, indicating the attackers had already done research before making contact.


When attackers can combine role-specific lures with techniques that make malicious links harder to detect, reducing the available targeting data becomes even more important. For organizations looking for a reason to include marketers in their data removal efforts, this campaign provides one.


Read more:[Big Brand Jobs Scam Targets Marketing Pros’ Google Accounts](https://www.darkreading.com/cyberattacks-data-breaches/big-brand-jobs-scam-marketing-pros-google-accounts)


## ******Fake Interpol Emails Target Small Businesses With Ransomware******


**Authority-based social engineering campaign targets small businesses across multiple industries**


Bitdefender researchers identified a phishing campaign targeting small businesses in the United States, Europe, Asia, and the Middle East with fake investigation emails impersonating Interpol. The emails claim to contain evidence of suspicious company activity and pressure recipients to open a password-protected archive hosted on Proton Drive. Instead of evidence, the file contains ransomware hidden within multiple archive layers.


Once executed, the malware seeks to encrypt files across available drives and displays a ransom note instructing victims to contact the attackers through Tox, an encrypted chat channel. Bitdefender notes that the ransomware appears to be custom-built but relatively simple, with hardcoded values and fewer features than those typically associated with large ransomware operations.


The fake Interpol notice uses fear, urgency, and law enforcement authority to push recipients into opening the malicious file before they verify the message. Small businesses are particularly at risk because many lack dedicated IT and cybersecurity resources.


Bitdefender observed the campaign targeting organizations across multiple industries, including food and agriculture, legal services, pharmaceuticals, media, technology, and finance.


Bitdefender recommends practical defenses including verifying unsolicited correspondence before acting, showing file extensions on Windows devices, enabling multi-factor authentication, keeping systems and software updated, training employees to recognize fear- and urgency-based scams, maintaining secure backups, and using layered security designed for small businesses.


Those controls help reduce the risk of execution, account compromise, and ransomware disruption. In addition to these, small businesses that employ personal data removal are less likely to be targeted by this and other social engineering campaigns.


Read more:[Fake Interpol Investigation Emails Spread Ransomware](https://www.bitdefender.com/en-us/blog/hotforsecurity/fake-interpol-emails-serve-ransomware?utm_source=chatgpt.com)


**Thanks for reading! Want us to write about something specific?**[Submit a topic or idea.](https://share.hsforms.com/1WaLDYEywQqKs9wqQESC_nwnwt3v)


If you’re looking to reduce your organization’s exposed PII and dramatically lower the volume of phishing, voice and messaging scams, credential theft attempts, and other PII-based threats your team has to defend against, Optery can help. We find and remove dozens more exposed profiles per person on average than competing services, and we prove it with before-and-after screenshots.


Get started here:[Optery for Business](https://www.optery.com/get-a-optery-for-business-demo/)


**Subscribe to receive future editions of[The Optery Dispatch](https://www.optery.com/dispatch-newsletter/)**
