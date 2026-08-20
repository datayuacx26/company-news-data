---
schema_version: "1.0.0"
document_id: "2e0e11af12aabaa2f3577c579e39402ca601d8b566e7d225452f38bf2ec7ba1e"
company_key: "yc-optery"
company: "Optery"
source_id: "yc-optery-rss-6e764af78e78"
canonical_url: "https://www.optery.com/social-engineering-dbir-silent-ransom-group-ai-lures/"
published_at: "2026-06-18T16:03:19+00:00"
first_seen_at: "2026-07-20T23:20:48.544663+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:2e28d7ce0ce700f85df965b9e465261253a557423f0384a26e27a2649c5eed14"
---

# Social Engineering Looms Large: DBIR Data, Silent Ransom Group, and AI-Branded Lures

Welcome to *The Optery Dispatch* — a newsletter delivering the latest insights on threat intelligence and proactive cybersecurity strategy. In Issue #14, published June 18, 2026, we cover:


- 2026 Verizon DBIR: Has vulnerability exploitation really become the top attack vector for initial access, or is it still social engineering?
- UNC3753 / Silent Ransom Group: Conti-linked actors use vishing, IT impersonation, screen sharing, and physical intrusions to steal data.
- AI Brand Phishing: Attackers impersonate Claude, ChatGPT, and other AI tools to steal credentials, tokens, and financial information.


## **Social Engineering’s Role in Breaches: Behind the Numbers of the 2026 Verizon DBIR**


**Despite the DBIR’s official rankings showing vulnerability exploitation as the #1 vector for initial access, the numbers show social engineering almost certainly still dominates**


Last year, we analyzed Verizon’s 2025 Data Breach Investigations Report and found that, although phishing ranked third in the official initial access chart, a closer reading of the DBIR’s data, analysis, and clarifying statements told a different story.


As we[wrote at the time](https://www.optery.com/phishings-role-in-breaches-behind-the-numbers-of-verizons-2025-dbir/) , phishing was likely to have played a much larger, though hidden, role in breaches than the official rankings suggested. Credential abuse often begins with phishing/social engineering or infostealers deployed via phishing. Malware interaction usually depends on the victim being tricked into opening or installing malicious content. And even when phishing or another form of social engineering is not named as the initial access vector, it is frequently the first vector of compromise, whether used directly by attackers or earlier in the chain by access brokers harvesting credentials for later sale or use. Our analysis concluded that phishing remained the most consequential attack vector.


The 2026 DBIR presents a similar issue where the true role of social engineering in initial access is obscured in the official attack vector rankings.


At first glance, Verizon reports that vulnerability exploitation is now the most common known initial access vector, rising to 31% of breaches. Credential abuse, the previous leader, dropped to 13%. Phishing remained at 16%, and Pretexting, newly added to Verizon’s tracked initial access vectors this year, reached 6%.


But a closer look complicates the headline.


If phishing and pretexting are grouped together as social engineering, they total 22%. If credential abuse is added, because stolen credentials are most often obtained through phishing, pretexting, infostealers deployed via social engineering, ClickFix-style lures, fake login pages, help desk manipulation, and other social engineering techniques, the social-engineering-enabled total rises to 35%.


That is higher than vulnerability exploitation at 31%. Even if only 75% of credential-abuse cases originated from social engineering, phishing, pretexting, and social-engineering-enabled credential abuse would total 31.75%, still edging out vulnerability exploitation.


The 35% figure should be understood as an upper-bound estimate. Not every stolen credential can be definitively traced back to social engineering, and there is not always visibility into how credentials were first obtained.


The DBIR’s initial access chart reflects the first action Verizon could classify, not necessarily the true first source of compromise. When an attacker logs in with valid credentials, the visible initial access vector may be “credential abuse,” even if the credential was originally stolen through a social engineering campaign outside the victim organization’s visibility.


The 2026 DBIR says Phishing and Pretexting frequently overlap with credential abuse, and that adding Pretexting to the tracked initial access vector list helped lower credential abuse’s official percentage.


While vulnerability exploitation is clearly a top method being used by attackers for initial access, the combined phishing, pretexting, and credential abuse numbers, along with the well-established overlap among those categories, cast doubt on the claim that vulnerability exploitation has overtaken social engineering in reality.


For defenders, the lesson is straightforward: patching systems is critical, but so is reducing the information attackers use to identify targets, impersonate trusted parties, manipulate employees, steal credentials, and make social engineering attacks work.


Data broker and people-search profiles expose the personal and professional details attackers use to craft believable lures, reach employees through personal channels, impersonate trusted parties, and pressure help desks or high-risk teams. Removing that exposed data reduces the raw material social engineers rely on and significantly lowers the volume of incidents security teams have to manage.


The 2026 DBIR may place vulnerability exploitation at the top of the official initial access chart. But behind the numbers, social engineering and credential theft still look like the more consequential source of organizational compromise.


Read the full report:[2026 Data Breach Investigations Report (DBIR) | Verizon](https://www.verizon.com/business/resources/reports/dbir/)


## ****A Conti offshoot is using vishing, IT impersonation, and physical intrusions to target organizations****


**UNC3753 blends email pretexts, live phone calls, screen-sharing deception, and in-person intrusions to steal sensitive data**


Google Mandiant and Google Threat Intelligence Group reported that UNC3753, also known as Silent Ransom Group, Chatty Spider, and Luna Moth, targeted dozens of U.S. professional, legal, and financial services organizations between January and May 2026 using vishing, screen-sharing deception, and in some cases, physical intrusions.


The group typically begins with benign invoice-themed emails that contain no malicious links or attachments. The goal is to create a believable pretext for a follow-up phone call. Attackers then pose as internal IT support and convince employees to join Zoom, Microsoft Teams, Quick Assist, or other screen-sharing sessions, often under the guise of helping with a security issue or data migration project.


Once connected, the attackers guide victims into installing legitimate remote access tools, search corporate and cloud directories, and exfiltrate sensitive data such as legal agreements, financial records, tax documents, audits, Social Security numbers, and other PII.


In some incidents, the campaign moved offline. According to the FBI, Silent Ransom Group actors have posed as IT technicians and entered corporate offices in person to steal data using external drives or USB devices.


UNC3753 and a related threat cluster are assessed to be offshoots of the former Conti ransomware gang.[Conti operators have previously been documented using commercial people-search and business-intelligence sources such as ZoomInfo and RocketReach to profile targets.](https://documents.trendmicro.com/assets/white_papers/wp-what-decision-makers-need-to-know-about-ransomware-risk.pdf)


Given that history, it is reasonable to expect that data brokers likely support targeting in campaigns like this. Minimizing exposed employee data online should be considered a critical mitigation against this threat actor.


Learn more:[UNC3753 Used Vishing and Physical Intrusions in U.S. Data Theft Extortion Campaign](https://thehackernews.com/2026/06/unc3753-used-vishing-and-physical.html)


## ******Threat actors are using AI brands to phish employees at businesses and organizations******


**Trusted AI tools are becoming believable lures for account enforcement, payment-update, and fake software campaigns**


Microsoft Threat Intelligence reported that attackers are increasingly impersonating trusted AI brands, including Anthropic’s Claude and ChatGPT, in phishing campaigns designed to steal credentials, authentication tokens, and financial information.


One such example was a Claude-themed phishing campaign observed between April 20 and April 22, 2026. Microsoft reported that the campaign sent phishing emails to users at more than 2,000 organizations, primarily in the United States, the United Kingdom, and India. The affected industries included information technology and financial services, along with other business entities.


The emails impersonated Anthropic and Claude using account-enforcement messaging. Recipients were told their account had violated acceptable use policies and were pushed into a fake appeal process. The campaign used a PDF attachment, Claude-branded landing pages, and likely an adversary-in-the-middle flow designed to intercept credentials and authentication tokens.


Microsoft also reported a ChatGPT-themed phishing campaign that used urgent subscription-payment messaging to send recipients to fake payment-update pages. The affected industries included higher education and professional services, and the phishing pages collected personal and financial information, including names, addresses, and credit card details.


Other AI-themed campaigns described by Microsoft relied on malicious downloads, fake AI tools, GitHub repositories, search results, and malvertising.


In the cases where attackers are sending lures to employees at specific organizations, exposed personal and professional data is required to identify targets, validate contact details, and tailor messages.


Minimizing exposed employee data across data broker sites and other sources is essential for reducing the risk of being targeted in these campaigns.


Learn more:[AI brands as bait: How threat actors are using the AI hype in social engineering](https://www.microsoft.com/en-us/security/blog/2026/06/08/ai-brands-as-bait-how-threat-actors-are-using-the-ai-hype-in-social-engineering/)


**Thanks for reading! Want us to write about something specific?**[Submit a topic or idea.](https://share.hsforms.com/1WaLDYEywQqKs9wqQESC_nwnwt3v)


If you’re looking to reduce your organization’s exposed PII and dramatically lower the volume of phishing, voice and messaging scams, credential theft attempts, and other PII-based threats your team has to defend against, Optery can help. We find and remove dozens more exposed profiles per person on average than competing services, and we prove it with before-and-after screenshots.


Get started here:[Optery for Business](https://www.optery.com/get-a-optery-for-business-demo/)


**Subscribe to receive future editions of[The Optery Dispatch](https://www.optery.com/dispatch-newsletter/)**
