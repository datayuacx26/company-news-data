---
schema_version: "1.0.0"
document_id: "a8e75ead0b3a6a4718b5b113edc35d3a7a62aea20722266b373184ee2e7191fc"
company_key: "yc-malloc-inc"
company: "Malloc Inc"
source_id: "yc-malloc-inc-rss-36b506f8e239"
canonical_url: "https://blog.mallocprivacy.com/malloc-privacy-weekly-91bea192a057"
published_at: "2026-06-02T05:07:49+00:00"
first_seen_at: "2026-07-24T10:30:15.411511+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:ea882296ce994fa63fd163435e2b5be377810f17dfd5277c29623f1a90d0ec7d"
---

# Malloc Privacy Weekly

# Malloc Privacy Weekly


[Malloc](https://medium.com/@Ayu_Malloc?source=post_page---byline--91bea192a057---------------------------------------)


15 min read


·


Jun 2, 2026


--


Welcome to the latest edition of **Malloc’s Weekly Global Mobile Privacy and Security Analysis** . We bring you the **latest updates** about **mobile security threats** and **digital privacy hazards** that are covertly targeting citizens, governments, and businesses to spy on them or harm them financially.


A sophisticated **Android malware campaign** has conducted extensive **carrier billing fraud** through **premium SMS abuse** , deploying nearly **250 malicious applications** that impersonate trusted platforms including **Facebook Messenger, Instagram Threads, TikTok, Minecraft, and Grand Theft Auto** and distribute primarily via social media channels and search engines outside official app stores. These applications employ precise **SIM operator validation** to target users of specific carriers in **Malaysia** (such as DiGi, Celcom, Maxis, and U Mobile), **Thailand** (including TrueMove H), **Romania** , and **Croatia** — with Malaysian victims comprising the majority — while displaying innocuous content on non-targeted devices to maintain persistence and evade detection. Once activated, the malware executes automated subscription workflows using hidden **WebView manipulation** , **JavaScript injection** , **SMS Retriever API** abuse for one-time password interception, delayed premium message scheduling, cookie theft for session maintenance, and real-time exfiltration of device data and billing details via **Telegram bots** and distributed command-and-control infrastructure, some of which remains operational. Active from March 2025 through mid-January 2026, this operation highlights the advanced automation and regional selectivity employed in modern **mobile threats** , resulting in unauthorized charges directly on victims’ phone bills without any visible user interaction.


**BTMOB** constitutes a stealthy **Android remote access trojan** that has evolved from the **SpySolr malware family** , initially documented in **February 2025** , and presents a significant threat through its advanced capabilities for **full device compromise** . Primarily affecting users in **Brazil and Latin America** yet with potential for broader global impact, the malware propagates via **phishing campaigns** employing deceptive websites that impersonate **streaming services** , **cryptocurrency platforms** , and even **government authorities** such as Argentina’s tax and customs agency, thereby directing victims to install malicious applications from counterfeit app repositories. Upon installation, **BTMOB** exploits **Android Accessibility Services** to secure elevated permissions silently, facilitating extensive functionalities including **sensitive data exfiltration** , **screenshot capture** , **screen and audio recording** , **keystroke logging** , **credential theft** , and **real-time remote control** of the infected device. Distributed as **malware-as-a-service** with an intuitive **no-code APK builder interface** that permits rapid customization of payloads and regional phishing lures without requiring programming expertise, the tool is commercially available for approximately a **$5,000 lifetime license** accompanied by monthly support fees, thereby democratizing sophisticated cyberattacks for less experienced adversaries and contributing to swift variant generation and proliferation through secondary markets.


To learn more about these developments and other news, read the article below.


Press enter or click to view image in full size


Malloc Privacy Weekly


## Android Malware Campaign Uses Fake Apps to Silently Subscribe Users to Premium Services


A 1 **0-month Android malware campaign** , dubbed **“Premium Deception”** by researchers, deployed nearly **250 fake apps** disguised as popular brands like **Facebook Messenger** , **Instagram Threads, TikTok, Minecraft,** and **Grand Theft Auto** to covertly enroll victims in premium carrier-billed services without their knowledge. Active from **March 2025** to **mid-January 2026** , the operation specifically targeted mobile users in **Malaysia** , **Thailand** , **Romania** , and **Croatia** , validating each device’s **SIM card operator code** against a **hardcoded list of carriers** including DiGi, Celcom, TrueMove H, Vodafone, and others before activating the **fraud workflow** . Researchers identified **three distinct malware variants** of escalating sophistication: the **most advanced variant disabled Wi-Fi to force cellular traffic** , loaded **hidden WebViews** , executed **JavaScript injection to automate the subscription process** , and **abused Google’s SMS Retriever API** to silently **intercept one-time passwords (OTPs)** and **Transaction Authentication Codes (TACs)** ; a **second variant** targeted **Thai users** with delayed **SMS scheduling at 60 and 90-second intervals** to **evade fraud detection** while **stealing session cookies** ; and a **third variant** incorporated **real-time Telegram reporting** to give operators **live visibility into infection rates** across distribution platforms. The campaign’s infrastructure featured a sophisticated **HTTP referrer tracking system** embedded in each malicious sample, enabling attackers to measure which fake app personas and social media distribution channels yielded the highest conversion rates, indicating a **well-organized commercial operation** focused on optimization. Security experts warned that this campaign **represents a shared failure** of controls across the entire mobile ecosystem, highlighting that **platform APIs** like the **SMS Retriever** and **WebView** components grant broad access without adequate transparency, while organizations’ continued reliance on **SMS-based MFA** remains a widely recognized **vulnerability** that attackers are now building sustained, professional operations around. Users are advised to **avoid sideloading apps** from **third-party stores** , **audit installed applications** , and **review mobile bills** for unexplained subscription charges.


Source: 1.[Infosecurity Magazine](https://www.infosecurity-magazine.com/news/android-carrier-billing-fraud-four/) ; 2.[HackRead](https://hackread.com/android-malware-subscribe-services-without-consent/) ; 3.[Dark Reading](https://www.darkreading.com/mobile-security/fake-android-apps-carrier-billing-fraud)


**How Malloc can help? 👉** Malloc keeps you safe from malware, spyware, and rogue apps via its device security scan, malicious app scan, and downloaded files scan. It also detects and alerts users to the use of Android accessibility services used by malware and spyware, including banking trojans and other Remote Access Trojans.


## BTMOB: The Android Malware-as-a-Service Threat Targeting Brazil and Latin America


A dangerous **Android remote access Trojan (RAT)** called **BTMOB** , believed to be an evolution of the **SpySolr malware family** , is being actively distributed through a **malware-as-a-service (MaaS)** model that significantly lowers the barrier to entry for cybercriminals seeking full **mobile device takeover** . Sold via **private Telegram channels** and openly advertised on the **clearweb** , **BTMOB** is available for a $700 monthly subscription $5,000 lifetime license, and comes equipped with a **no-code APK builder interface** that allows operators to **generate custom malicious Android apps** and **localized phishing lures** without any programming expertise. The malware’s capabilities extend well beyond typical banking Trojans, offering **data exfiltration** , **screenshot capture** , **financial transaction interception** , **device activity recording** , and **full remote control** , all while **abusing Android Accessibility Services** to gain **elevated permissions** without further user interaction. Currently most active in **Brazil** and **Latin America** , **BTMOB** is distributed through **phishing websites** impersonating **streaming services** , **cryptocurrency mining platforms** , and **fake Google Play storefronts** , with recent campaigns even **mimicking** an **Argentinian government agency** to lure victims. Security researchers warn that the **MaaS distribution model** combined with the **rapid payload generation capability** **undermines single-layered defenses** , and the tool risks **spreading into secondary markets through resale or sharing** within closed groups. Users are strongly advised to **install apps only from the official Google Play Store** , keep Play Protect enabled, **revoke unnecessary permissions** such as **Accessibility access** , and **treat unsolicited links with suspicion** , while enterprises should apply the same security rigor to mobile devices as they do to other machines and environments.


Source: 1.[Bleeping Computer](https://www.bleepingcomputer.com/news/security/btmob-android-malware-service-generates-custom-phishing-payloads/) ; 2.[Dark Reading](https://www.darkreading.com/cyberattacks-data-breaches/btmob-rat-brazil-latam-maas-model)


**How Malloc can help? 👉** Malloc keeps you safe and private online by detecting and blocking spyware, malware, and other malicious domains in real time. It does this using advanced spyware indicators, monitoring your app behavior and data transmission, and alerts users when suspicious activity is detected on their device. Malloc can detect the misuse of Android accessibility services exploited by malware and spyware, alerting users promptly to help them stay protected.


## Trapdoor: The Massive Android Ad Fraud Campaign That Generated 659 Million Fake Ad Requests Daily


A major **ad fraud and malvertising operation** called **Trapdoor** has been **uncovered** and **dismantled** by Researchers, revealing a **sophisticated scheme** that exploited **455 malicious Android apps** and **183 command-and-control (C2) domains** to **defraud** the **digital advertising ecosystem** on a **massive scale** . The campaign began on the **Google Play Store** , where attackers offered **seemingly harmless utility apps** — such as **PDF readers** and **device cleanup tools** — that functioned normally and raised no suspicion upon installation. However, shortly after being downloaded, these apps displayed **fake pop-up update alerts** that tricked users into **installing a secondary hidden app** , which then launched **invisible WebViews** , **loaded attacker-controlled HTML5 domains** , and **secretly requested ads** that **no real user ever saw** . At its peak, **Trapdoor** generated **659 million fraudulent ad bid requests per day** , with the associated apps accumulating over **24 million downloads** , and more than **three-fourths of the traffic originating from the United States** . The operation was notably **self-sustaining** : **revenue from hidden ad fraud** funded further **malvertising campaigns** , which in turn **drove more app installs** , creating a **continuous illicit revenue cycle.** The attackers employed **selective activation techniques** , triggering the **malicious payload** only for users acquired through their **ad campaigns** while **suppressing it for organic downloads** , alongside **anti-analysis** and **obfuscation methods** such as **impersonating legitimate SDKs** and **abusing install attribution tools** designed for legitimate marketers. Following responsible disclosure, Google removed all identified malicious apps from the Play Store, effectively neutralizing the operation, though researchers warned that **Trapdoor** exemplifies how determined **fraudsters are constantly evolving** by **co-opting legitimate tools** and **chaining together multiple fraud stages into a single** , **difficult-to-detect pipeline.**


Source: 1.[Techradar](https://www.techradar.com/pro/security/android-users-beware-this-huge-fraud-scam-campaign-hit-millions-of-victims-around-the-world-make-sure-youre-not-next) ; 2.[The Hacker News](https://thehackernews.com/2026/05/trapdoor-android-ad-fraud-scheme-hit.html)


**How Malloc can help? 👉** Malloc provides strong protection against advertisement fraud and advertisement bombing attacks by blocking third-party advertisement domains locally device-wide through its on-device VPN and local tracker blocklists. It further provides enhanced protection against fraud and malware through its anti-phishing protection and malicious app scans. Malloc VPN, via its IP address obfuscation and encrypted tunnel, also helps secure user privacy by limiting the user details sent to advertisers.


## Chinese Phishing-as-a-Service Operations Surge with Real-Time Credential Theft and AI-Powered Tactics


The **Chinese-language phishing-as-a-service (PhaaS)** landscape has rapidly expanded in size and sophistication, **challenging** the historical dominance of **Russian-speaking cybercriminal groups** , according to research by the **Google Threat Intelligence Group (GTIG)** , which identified at least a **dozen active PhaaS offerings** **operating in Chinese-language underground communities.** These threat actors have made a **fundamental shift** from **static password harvesting** to **real-time credential interception** , using **live administration panels** that allow attackers to **interact with victims** **during phishing activity** , **capturing one-time passcodes (OTPs)** as they are entered to **bypass multifactor authentication (MFA) protections almost instantly.** Nearly **all organizations impersonated by these services are no** n **-Chinese entities** , with **top targeted countries** including **Japan** , the **US** , **Australia** , **Hong Kong** , and the **United Arab Emirates** , and operators deliberately **avoid domestic targets** . Instead of **traditional SMS** , these **phishing operations** now **leverage encrypted messaging protocols** like **Rich Communication Services (RCS)** and **Apple iMessage** to **deliver phishing lures** , making **detection** by **infrastructure-level filters** **significantly harder** while **appearing more convincing to victims** . A **key monetization strategy** involves **exploiting digital wallet provisioning** , where attackers use **stolen credentials** and **OTPs** to **provision victims’ payment cards** into **digital wallets** on **attacker-controlled devices** , **enabling contactless payments** , **high-value transactions** , and **ATM withdrawals** . Platforms like **Darcula** , linked to **threat acto** r **UNC5814** , have **embraced AI-powered page generators** and **browser automation tools** such as **Puppeteer** to **clone legitimate websites** by replicating their HTML, CSS, JavaScript, and visual elements, rendering **each phishing page unique** and **making traditional signature-based detection increasingly ineffective.** Another platform, **YY Lai Yu** , demonstrates the highly **localized nature** of these campaigns, offering over **400 phishing templates** **covering** **Japanese brand** s and **extending beyond banking lures** into **loyalty points** , **rewards programs** , **transit services** , and **e-commerce platforms** , while **deploying anti-bot verification screens** to **hinder automated security analysis** . Many of these **sophisticated** **PhaaS platforms** also offer **comprehensive criminal suites** including the sale of **personally identifiable information** **(PII)** , **domain registration, money laundering, IMSI catchers, and stolen payment card trading,** with some operators openly advertising on **Telegram** and flaunting luxury lifestyles, revealing poor operational security (OpSec) — altogether demonstrating that the **China-based criminal ecosystem continues to evolve** , enabling even threat actors with limited technical skills to conduct **highly effective phishing operations** at scale.


Source: 1.[Infosecurity Magazine](https://www.infosecurity-magazine.com/news/chinese-phishing-live-credential/) ; 2.[Help Net Security](https://www.helpnetsecurity.com/2026/05/26/chinese-language-phishing-services/)


## Get Malloc’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**How Malloc can help? 👉** Malloc has anti-phishing protection and blocks all kinds of malware and spyware domains locally using its on-device VPN. It also blocks trackers and insecure HTTP traffic device-wide. Stay connected to Malloc VPN at all times for maximum privacy. Thus, Malloc’s VPN and tracker blocking protection, as well as anti-phishing protection, along with its anti-spyware and anti-malware protection protects against all kinds of threats when receiving malicious links through SMS.


## Telegram’s MTProto Protocol Exposes Users to Network-Level Tracking Despite Encryption


A technical review of **Telegram’s MTProto protocol** , authored by cryptographer Dr. Nadim Kobeissi and published in an 89-page report, reveals that the messaging platform **leaks persistent device identifiers** to **passive network observers** , enabling **user tracking across networks,** **locations** , and **sessions without needing to break Telegram’s encryption.** The **core vulnerability** lies in the **auth_key_id,** a 64-bit **identifier embedded in every Telegram message header** that remains **stable across app restarts, IP address changes, VPN use, and network switches** , effectively serving as a **long-lived tracking beacon.** The problem is compounded by the fact that **Telegram clients on Android** and **desktop** transmit **MTProto traffic over raw, unencrypted TCP connections rather than TLS or HTTPS** , even when using port 443, meaning **internet service providers, mobile carriers, enterprise network administrators, and state surveillance systems can observe these identifiers without decrypting any message content.** Critically, even **Telegram’s Secret Chats** and **Perfect Forward Secrecy protections** do **not mitigate this issue** because the exposure occurs at the **transport layer** , **below application-level encryption.** Privacy researcher Lukasz Olejnik warned users **not to rely on Telegram for sensitive communications,** while researchers recommended that **Telegram mandate TLS encryption for all MTProto connections and remove unencrypted TCP fallbacks entirely** . **Telegram rejected the findings** , arguing that auth_key_id “changes regularly” and does not reveal user information, and that any observer capable of monitoring this identifier would already have access to other metadata such as IP addresses and traffic patterns — a defense researchers and privacy advocates consider insufficient.


Source:[Cyber Insider](https://cyberinsider.com/telegrams-mtproto-protocol-leaks-persistent-identifiers-enabling-user-tracking/)


**How Malloc can help? 👉** Malloc VPN, which is fast and secure, is also a great supplement to existing end-to-end encryption (E2EE) in secure messaging apps and RCS messaging. It hides a user’s IP address, which is unique and accessible to hackers, ISPs, and governments, and encrypts internet traffic, offering an additional layer of protection.


## Apple’s Massive App Store Fraud Crackdown Prevents Over $11 Billion in Losses Over Six Years


**Apple** has significantly ramped up its **fraud prevention efforts** across the **App Store** , blocking more than **2.2 billion** **USD** in **fraudulent transactions** in **2025** alone and bringing the cumulative total over the **past six years** to over **11 billion USD** . The company’s **multilayered defense system** , which combines **human review** and **advanced machine learning technologies** , processed over **9.1 million app submissions** last year, **rejecting** more than **2 million problematic apps** for issues including **privacy violations, bait-and-switch tactics, cloned or misleading apps, and hidden or undocumented features** — with nearly **59,000 apps removed for deceptive practices,** almost triple the prior year’s figure. On the account security front, Apple’s systems **blocked 1.1 billion USD fraudulent customer account creations** , **deactivated 40.4 million user accounts suspected of fraud and abuse** , **terminated 193,000 developer accounts** , and **rejected over 138,000 developer enrollments tied to malicious actors.** The company also prevented more than **5.4 million stolen credit cards** from being used for **fraudulent purchases** and **banned nearly 2 million user** accounts from transacting again. Beyond the **official App Stor** e, which draws over **850 million weekly visitors** across **175 storefronts worldwide** , Apple detected and blocked **28,000 illegitimate apps on pirate storefronts** designed to distribute **malware** and **pirated software** , while also blocking **2.9 million attempts** in a **single month** to install apps from **unauthorized distribution channels.** Additionally, of the **1.3 billion ratings** and **reviews** submitted last year, nearly **195 million fraudulent reviews** were identified and **removed** , and Apple **blocked thousands of deceptive apps from appearing in search results** and **App Store charts** , underscoring the **enormous** and **ever-evolving scale** of **cybercriminal activity** targeting one of the world’s largest digital marketplaces and the critical importance of continuous, AI-driven enforcement to protect consumers and legitimate developers alike.


Source: 1.[Infosecurity Magazine](https://www.infosecurity-magazine.com/news/apple-blocked-2bn-app-store-fraud/) ; 2.[Bleeping Computer](https://www.bleepingcomputer.com/news/apple/apple-blocked-22-billion-in-fraudulent-app-store-transactions-in-2025/) ; 3.[Security Affairs](https://securityaffairs.com/192484/security/apple-blocks-over-2-million-apps-in-2025-fraud-crackdown.html)


**How Malloc can help? 👉** Malloc app for iPhones, along with the iPhone’s new protection features, is a great combination for users’ all-around privacy and security. Malloc’s numerous powerful security and privacy features, such as the device-wide tracker blocker, HTTP blocking, phishing and malicious domain protection, along with its WireGuard-based encrypted VPN, device security scan, malicious app scan and download scan work in conjunction with the iPhone’s own protection features to safeguard users against all kinds of mobile threats, ranging from common scams to sophisticated state-sponsored spyware attacks.


## Pentagon Under Fire for Failing to Protect US Troops from Smartphone Location Tracking by Foreign Adversaries


**US policymakers** are demanding urgent changes to **Pentagon smartphone security policies** after confirming for the first time that **hostile foreign adversaries** have been **purchasing commercial geolocation data** from s **martphone advertising profiles** to t **rack, target,** and **surveil American servicemembers in active war zones** in the **Middle East.** In a **bipartisan letter** to the Pentagon signed by Senator Ron Wyden, Representative Pat Harrigan, and 12 other **members of Congress** , lawmakers revealed that **US Central Command (USCENTCOM)** has received **multiple threat reports** confirming this **exploitation** , yet the **Department of Defense** still has **no policy requiring troops to disable geolocation capabilities** on their personal devices in operational areas, and even **Pentagon-issued smartphones fail to disable advertising IDs by default** . The issue has been known for nearly a **decade** , with government contractors briefing military leadership as early as 2016 on the **ease of tracking troops’ smartphones,** yet **DoD leadership failed** to act on the threat, which **enables adversaries to identify where US troops congregate** and **map their patterns of life** for **potential attacks using missiles** , **drones** , and **roadside bombs** . The lawmakers are now urging the Pentagon to adopt **common-sense cybersecurity safeguards,** including **disabling smartphone advertising IDs** and **replacing tracking-prone web browsers like Google Chrome** with privacy-focused alternatives, measures already recommended by the **National Security Agency (NSA)** and the **Cybersecurity** and I **nfrastructure Security Agency (CISA).**


**Source:**[Cybernews](https://cybernews.com/security/american-troops-phone-location-data-iran/)


**How Malloc can help? 👉** With the Adversary Governments buying data from data broker industry to spy on foreign users and troops, and AI-powered user data collection and analysis gaining momentum, online users have never been in greater danger. Malloc App will help secure users during these difficult times. Its on-device tracker blocker, which uses a local system-wide VPN, helps greatly reduce data collection. Its VPN hides users’ geolocation by hiding their IP address and encrypts their internet traffic in transit. Further, its security features help secure users in case of a targeted social engineering attack made possible through previously collected and sold user data.


## Tech Giants Fail to Remove Majority of Scam Ads Despite Consumer Complaints


Between **December 2025 and March 2026** , **European consumer organization BEUC** and **consumer groups from 13 countries** filed a total of **893 complaints against Meta, TikTok, and Google** for **hosting scam advertisements** that **breach European law** , yet the **platforms removed only 27% of the reported ads** , leaving a **staggering 73% still active** — **with 52% outright rejected or ignored** . These fraudulent advertisements continue to reach more than **200 million European consumers every month** , exposing them to **significant financial fraud and potential losses of hundreds to thousands of euros** . Under the **Digital Services Act (DSA)** , **tech companies are legally required to maintain accessible reporting systems** for **malicious ads** , **respond to complaints seriously** , **and address systemic risks on their platforms** , with **violations carrying fines of up to 6% of global annual turnover** . While Google claims it blocks over 99% of violating ads before they run, **Meta’s own internal documents** reveal that **fraudulent and misleading advertisements generate approximately $16 billion annually** — **roughly 10% of Meta’s total revenue** — with users being shown around **15 billion “higher risk” scam ads per day** , underscoring the **enormous financial incentive** these platforms have to tolerate deceptive advertising and highlighting the **urgent need for stronger enforcement** and **corporate accountability** **to protect consumers across Europe.**


**Source:**[Cybernews](https://cybernews.com/security/meta-tiktok-google-scam-ads-online/)


## Poland Leads European Shift to State-Controlled Messaging Platforms Amid Russian-Linked Cyberattacks on Officials


**Poland’s government** has **instructed public-sector officials** to **stop using Signal for sensitive communications** and transition to **domestically controlled encrypted messaging systems** , following a **wave of phishing attacks and social engineering campaigns attributed to Russian-backed APT groups targeting politicians, military personnel, and government employees** . The attacks did not break Signal’s encryption itself but instead **exploited legitimate account-management features through two primary methods:** **attackers impersonating Signal support staff to trick victims into sharing SMS verification codes and PINs for full account hijacking** , and **distributing malicious QR codes or links that secretly connect attacker-controlled devices to victims’ accounts** , **granting silent access to private chats** , **group conversations** , and **message histories.** Poland’s **Ministry of Digital Affairs,** under **Deputy Prime Minister Krzysztof Gawkowski,** is **recommending two nationally operated platforms as replacements** : **mSzyfr Messenger** , an **encrypted communication tool** managed by **NASK-PIB** available only to approved organizations, and **SKR-Z,** an **isolated classified communications network for handling restricted information** — **both fully hosted within Polish jurisdiction under national cybersecurity standards.** This decision reflects a broader **European trend toward digital sovereignty,** with **Germany’s Bundestag similarly encouraging lawmakers to adopt the Wire messaging platform** and **Dutch intelligence agencies AIVD and MIVD confirming that government officials were compromised through coordinated Signal-targeting campaigns.** Polish authorities are advising remaining **Signal users** to **enable Registration Lock** , **regularly review linked devices** , **never share verification codes** , **avoid scanning unsolicited QR codes,** and **use usernames instead of phone numbers,** while emphasizing that the **fundamental vulnerability lies not in the technology but in users being the easiest point of attack against state-backed cyber-espionage operations.**


Source: 1.[Cyber Insider](https://cyberinsider.com/poland-urges-officials-to-ditch-signal-for-state-run-messaging-apps/) ; 2.[Security Affairs](https://securityaffairs.com/192381/intelligence/poland-shifts-away-from-signal-following-cyberattacks-on-officials-accounts.html)


**How Malloc can help? 👉** Malloc VPN, which is fast and secure, is also a great supplement to existing end-to-end encryption (E2EE) in secure messaging apps and RCS messaging. It hides a user’s IP address, which is unique and accessible to hackers, ISPs, and governments, and encrypts internet traffic, offering an additional layer of protection. Malloc also blocks phishing attempts and account takeover attacks via its on-device VPN that works alongside the remote VPN to protect users from frauds and cyber espionage.
