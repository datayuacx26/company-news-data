---
schema_version: "1.0.0"
document_id: "dbf895306ff599dce649dd52e6e26d7dced6baacef101e1eeee634e263f2e762"
company_key: "yc-malloc-inc"
company: "Malloc Inc"
source_id: "yc-malloc-inc-rss-36b506f8e239"
canonical_url: "https://blog.mallocprivacy.com/malloc-privacy-weekly-c85fc904c1a4"
published_at: "2026-06-30T14:19:52+00:00"
first_seen_at: "2026-07-24T10:30:15.411511+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:2a378f3b9394ba3a90424bfb4988aea9ae80d22cbe3a3a85e3fdaf769fd227e9"
---

# Malloc Privacy Weekly

# Malloc Privacy Weekly


[Malloc](https://medium.com/@Ayu_Malloc?source=post_page---byline--c85fc904c1a4---------------------------------------)


12 min read


·


Jun 30, 2026


--


Welcome to this week’s edition of Malloc’s Weekly Global Privacy and Security Analysis. This week, we look at how forensic tools and “secure backup” features are being weaponized in the real world — from state-linked actors extracting data from seized devices, to targeted phishing campaigns designed to trick people into handing over recovery keys that unlock their encrypted histories. We also cover the latest mobile threats and platform shifts that quietly widen the attack surface, and what practical steps you can take right now to reduce tracking, phishing risk, and device compromise.


**Russian security forces used Cellebrite UFED forensic extraction tools to unlock and thoroughly analyze the iPhone 12 of leading Russian opposition activist Andrey Pivovarov while it remained in official custody after his arbitrary arrest at St. Petersburg airport on May 31, 2021** , enabling investigators to pull private communications from **WhatsApp, Telegram, and Viber** and run targeted keyword searches for his political contacts and activities linked to the pro-democracy **Open Russia** movement founded by **Mikhail Khodorkovsky** . Despite the Israeli company publicly ending sales and support to Russian authorities three months earlier in March 2021, the offline-capable hardware continued functioning, as confirmed by both detailed device log analysis showing a specific **Cellebrite Host ID** connection on June 17, 2021, and official Russian Interior Ministry forensic records explicitly documenting the tool’s deployment.


**Russian Intelligence Services (RIS)** cyber threat actors, publicly tracked as **UNC5792** (linked to **FSB Border Guards** officers) and **UNC4221** (tied to Russian military services), have escalated a sophisticated **phishing campaign** targeting high-value **Signal** users — including current and former U.S. and international government officials, military personnel, political figures, journalists, and key Ukrainian officials — by impersonating automated support accounts with fabricated claims of attacks from Iran and post-Soviet actors to push mandatory two-factor verification and **Signal Secure Backups** . Victims are tricked via precisely worded in-app-style messages into enabling encrypted cloud backups (which generate and display a unique **64-character Backup Recovery Key** never shared with Signal) and then pasting that key into the fake chat under the pretext of a sync issue or data-loss risk; with the key, attackers restore and decrypt the **end-to-end encrypted** archives on their own devices, gaining full access to the victim’s **historical messages** , private and group conversations, and recent media without breaking Signal’s cryptography, which remains intact as the attack relies entirely on social engineering.


To learn more about these developments and other news, read the article below.


Press enter or click to view image in full size


Malloc Privacy Weekly


## Russian State Exploits Cellebrite Forensic Tools to Prosecute Activist Andrey Pivovarov Despite Official Contract Cancellations


A forensic investigation by the Citizen Lab confirmed that Russian authorities utilized **Cellebrite’s UFED forensic extraction tools** to breach the iPhone 12 of prominent political activist **Andrey Pivovarov** following his arrest in May 2021. Despite Cellebrite publicly terminating its Russian contracts in March 2021, official documents from Russia’s Ministry of the Interior verify that state experts successfully extracted data from Pivovarov’s device to **hunt for information regarding his political activities and social graph** , including high-profile opposition figures. While the authorities failed to bypass the encryption on Pivovarov’s MacBook, the extracted mobile data may have enabled subsequent **transnational social-engineering cyber campaigns** against his associates by the FSB-linked hacking group COLDRIVER. This case underscores a broader, systemic failure where forensic technology providers struggle or fail to remotely deactivate legacy systems, allowing **autocratic regimes to repurpose forensic tools for political repression** and human rights abuses globally.


Source:[Citizen Lab](https://citizenlab.ca/research/russia-breaks-into-human-rights-activists-phone-with-cellebrite/)


**How Malloc can help? 👉** Malloc offers complete protection against all kinds of mobile threats — it blocks spyware connections in real time, thwarts phishing attempts, blocks trackers and insecure HTTP websites, and detects permission misuse by apps. Malloc VPN helps protect your mobile device when connected to unsafe networks, keeping you protected against the ever-increasing number of mobile security threats.


## Russian Intelligence Services Target High-Value Signal Users in Phishing Campaign to Steal Secure Backup Recovery Keys


The **FBI and CISA** have issued an updated warning regarding a sophisticated phishing campaign conducted by **Russian Intelligence Services (RIS)** , specifically targeting high-profile individuals such as **government officials, military personnel, and journalists** . In an evolution of their tactics, the attackers masquerade as **official Signal support accounts** and falsely claim that mandatory updates are required to prevent data loss. By tricking victims into enabling **Signal Secure Backups** and subsequently manipulating them into sharing their **Backup Recovery Keys** under the guise of fixing synchronization issues, the threat actors are successfully bypassing end-to-end encryption to **download and access victims’ historical private messages** on their own devices. Security agencies emphasize that creating a new account under the same phone number will not invalidate a compromised key; instead, users must **generate a new recovery key** through their settings to prevent further unauthorized backup downloads, while remaining vigilant that legitimate support teams will **never request verification codes or recovery keys** directly within the application.


Source:[Bleeping Computer](https://www.bleepingcomputer.com/news/security/fbi-russian-hackers-now-target-signal-backup-recovery-keys/)


**How Malloc can help? 👉** Malloc’s extensive features help reduce risks like phishing and spyware infections to devices that come with using messaging apps like Signal etc. Malloc VPN is a great supplement to existing end-to-end encryption (E2EE) in secure messaging apps and RCS messaging. It hides users’ IP addresses, which are unique and accessible to hackers, ISPs, and governments, and encrypts internet traffic, offering an additional layer of protection.


## Scammers Exploit Shop App with Fake Invoices to Mimic Real Brand Purchases


**Cybersecurity experts have identified an emerging threat where scammers exploit Shopify’s Shop app by placing fake invoices — impersonating trusted brands like Norton, McAfee, Apple, and PayPal — directly into users’ trusted order histories.** Because the legitimate app automatically consolidates purchases by scanning connected emails or utilizing merchant workflows, victims receive official-looking push notifications and receipts for high-value items they never bought, creating immediate panic. The fraudulent receipts contain embedded **fake support phone numbers** instead of real contact details. When worried users call these numbers to dispute the charge, they are routed to scammers who attempt to steal **sensitive payment credentials, one-time security codes, or trick them into installing remote access software** . Security researchers advise consumers to never use the contact details provided within suspicious app alerts, and instead **verify pending transactions directly through their bank accounts or official brand websites** .


Source:[Gen Digital](https://www.gendigital.com/blog/insights/research/fake-invoices-shopping-apps)


**How Malloc can help? 👉** Malloc provides strong protection against malware and rogue apps via its malicious app scan, device security scan, download file scan, and app installation scan. Malloc on Android blocks apps from accessing the mic and also detects camera usage. Malloc also blocks malicious domains and phishing domains, and blocks all spyware traffic in real time by using strong spyware indicators. It also blocks unsecured HTTP traffic device-wide, trackers, and its VPN hides IP location and encrypts device internet traffic.


## Google Play Store Compromised as Fake Document Reader App Infects Over 100,000 Android Devices with Anatsa Banking Trojan


A highly active cyber campaign has successfully bypassed Google Play Store security checks using a **fake document reader application** that has amassed **over 100,000 downloads** . Acting as a dropper, this malicious app quietly connects to a remote server to install the **Anatsa Android banking trojan** in the background. Once active, the malware abuses Android’s **Accessibility Services** to capture keystrokes, monitor screens, and overlay **fraudulent login pages** on top of legitimate financial applications. This enables attackers to steal sensitive **credentials and two-factor authentication codes** , ultimately initiating unauthorized fund transfers and intercepting SMS verification messages to bypass security measures undetected.


Source:[Cyber Press](https://cyberpress.org/fake-document-reader-malware/)


**How Malloc can help? 👉** Malloc keeps you safe and private online by detecting and blocking spyware, malware, and other malicious domains in real time. It does this using advanced spyware indicators, monitoring your app behavior and data transmission, and alerts users when suspicious activity is detected on their device. Malloc can detect the misuse of Android accessibility services exploited by malware and spyware, alerting users promptly to help them stay protected.


## Joint SSU and FBI Investigation Exposes Ongoing Russian Cyber Espionage Campaign Targeting Global Officials and Military Personnel via Messenger Exploitation


In a major collaborative effort, Ukraine’s **Security Service (SSU)** and the **U.S. FBI** have uncovered a systematic, long-term **cyber espionage operation** conducted by **Russian intelligence agencies** to infiltrate the messaging accounts of **government officials, military personnel, politicians, and activists** across Ukraine, Europe, and the United States. This ongoing campaign utilizes **social engineering tactics** , such as sending **impersonating SMS messages** from platform support bots during the vulnerable early morning hours, to deceive targets into revealing **credentials, verification codes, and PINs** . Crucially, the attackers have evolved their techniques to actively harvest **Signal Backup Recovery Keys** — which grant permanent access to a user’s entire message history — and use **compromised QR codes** to silently link unauthorized devices. To counter this widespread threat, security agencies urge users to implement robust **two-factor authentication with alphanumeric PINs** , regularly audit and **terminate unrecognized active sessions** , and strictly avoid scanning unknown QR codes or sharing recovery keys.


## Get Malloc’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Source:[Security Affairs](https://securityaffairs.com/194399/intelligence/ssu-)


**How Malloc can help? 👉** Malloc has released a new Phishing Protector App to protect users against SMS phishing threats. The regular Malloc App also offers protection against phishing via its on-device VPN, and helps protect users when connected to the internet via its remote VPN. It also has additional security features like HTTP traffic blocking, protection against malicious domains and trackers, and includes malicious app scanning, device security scanning, and download file scanning.


## Unofficial Android TV Boxes and Smart TV Apps Co-opted as Stealth Residential Proxies to Fuel AI Scraping and Cyber Fraud


A sprawling Android-based botnet known as **Popa** has compromised millions of **consumer TV boxes** and **smart TV apps** over the past four years, converting unsuspecting home internet connections into a massive **residential proxy network** . Security researchers have linked this infrastructure to **NetNut** , a proxy provider operated by the publicly traded Israeli firm **Alarum Technologies** , although the company disputes the characterization of their software development kit (SDK) as malware. By embedding these silent SDKs into popular **pirated streaming applications** , as well as legitimate-looking games and utilities on **Samsung and LG smart TV platforms** , developers secretly monetize user bandwidth without meaningful consent. This persistent communication layer routes internet traffic for **advertising fraud, account takeovers, and aggressive web-scraping campaigns** — frequently leveraged by AI companies to bypass datacenter blocks while training large language models. The sheer volume of this traffic routinely disrupts targeted websites, while the presence of these hidden proxies on employee devices poses severe **security, legal, and reputational risks to corporate and government networks** whose IP addresses are ultimately blamed for the malicious activity.


Source:[Krebs On Security](https://krebsonsecurity.com/2026/06/popa-botnet-linked-to-publicly-traded-israeli-firm/)


**How Malloc can help? 👉** Stay Tuned as Malloc continues to deliver and protect users against the ever-evolving threat landscape in cybersecurity by constantly adding new features and releasing new products to help secure the precious and vulnerable digital lives of users by establishing strong defenses and offenses against various tactics used by cybercriminals to harm and spy on them.


## Polish Cybercrime Bureau, FBI, and HSI Apprehend International SIM-Swapping Syndicate Responsible for Laundering Millions in Stolen Cryptocurrency


Polish law enforcement, in collaboration with the US FBI and Homeland Security Investigations, has dismantled an organized cybercrime syndicate by **arresting four individuals** who engaged in highly sophisticated **SIM-swapping attacks** to steal **millions of dollars in cryptocurrency** . Utilizing specialized software, social engineering, and breaches of telecommunications partners and employee email accounts, the threat actors — including one identified by blockchain researchers as Wojtek “Merry” Kulisz — successfully **cloned victims’ phone numbers** , bypassed security protocols to intercept SMS and email messages, and hijacked digital wallets and exchange accounts. The stolen assets, valued at over several tens of millions of Polish złoty (at least **$5 million USD** ), were laundered through a global financial network of bank accounts and digital wallets, treating this illicit scheme as a regular source of income. Currently held in pre-trial detention, the suspects face charges of hacking, money laundering, and participating in an organized criminal group, carrying a potential maximum penalty of **25 years in prison** .


Source:[Bleeping Computer](https://www.bleepingcomputer.com/news/security/poland-busts-sim-swapping-gang-tied-to-millions-in-crypto-theft/)


**How Malloc can help? 👉** Malloc has anti-phishing protection and blocks all kinds of malware and spyware domains locally using its on-device VPN. It also blocks trackers and insecure HTTP traffic device-wide. Stay connected to Malloc VPN at all times for maximum privacy. Malloc VPN, via its IP address obfuscation and encrypted tunnel, also helps secure user privacy by limiting the user details sent to advertisers.


## Paradigm Shift Exposes Unpatchable “usbliter8” Boot ROM Vulnerability in Apple’s A12 and A13 Chips, Enabling Physical Exploits and Potential Jailbreaks on Older iPhones


A newly uncovered **unpatchable Boot ROM vulnerability** dubbed **“usbliter8”** affects Apple’s **A12 and A13 chips** , which power older devices ranging from the **iPhone XS and XR up to the iPhone 11** . Discovered and publicly detailed alongside a proof of concept by Barcelona-based offensive cybersecurity firm **Paradigm Shift** , this flaw resides in the immutable, first-stage startup code of the device, meaning **it cannot be patched** by software updates and the only permanent mitigation is migrating to newer hardware. Exploiting the flaw **requires physical access** to connect a cable to the target device, allowing hackers and security researchers to bypass initial defense checks and potentially chain this bug with other vulnerabilities to develop an **iPhone jailbreak** or bypass security restrictions. While sophisticated government contractors and spyware firms like **Cellebrite and Magnet Forensics** likely utilize similar techniques to break into seized devices, the public release of usbliter8 serves as a rare catalyst for security researchers to study iOS defenses, even though additional exploits are still required to fully access encrypted user data.


Source:[Tech Crunch](https://techcrunch.com/2026/06/22/a-new-unpatchable-flaw-in-apple-chips-opens-the-door-to-an-iphone-jailbreak/)


**How Malloc can help? 👉** While it’s up to Apple to patch critical OS vulnerabilities, and users should keep their devices updated, Malloc can further enhance your security and privacy through its extensive features. Its Root Checker Scan, malware and spyware scan, and app permission scan help secure your device. Furthermore, Malloc VPN, its on-device Threat Protection feature, and tracker blocker help users stay secure and private.


## Samsung KNOX Kernel Vulnerability (CVE-2026–20971) Exposes Millions of Galaxy Devices to Potential Takeover via Race Condition Exploitation


Security researchers from **LucidBit Labs** discovered a critical **use-after-free (UAF) kernel vulnerability** tracked as **CVE-2026–20971** within **Samsung’s KNOX security suite** , specifically involving the interaction between the **PROCA and FIVE subsystems** designed to validate process integrity. The flaw is triggered by a **race condition** during process state transitions, where a tiny timing window allows an attacker executing an **untrusted application** to access previously freed memory, bypassing defenses like **Kernel Control Flow Integrity (KCFI)** to achieve **kernel memory corruption** and potentially complete **device takeover** . This vulnerability impacts **millions of Galaxy devices** — ranging from the **S9 to the S25 series** , alongside A-series models running **Android 13 through 16** across both Exynos and Qualcomm chipsets — highlighting the irony of defensive security architectures expanding the physical attack surface. Although Samsung successfully patched the flaw in its **January 2026 security update** , the incident serves as a crucial reminder for enterprise environments to **patch devices quickly** to prevent mobile endpoints from becoming initial access vectors into secure corporate networks.


Source:[Security Affairs](https://securityaffairs.com/194090/security/samsung-knox-kernel-uaf-exposes-millions-of-galaxy-devices.html)


**How Malloc can help? 👉** As Android OEMs continue working to patch critical OS vulnerabilities, users should ensure they download and install the latest security patches. Malloc can further enhance your security and privacy through its powerful features. Malloc detects and alerts users to microphone and camera usage, blocks trackers, HTTP traffic, and malicious domains, hides IP location, encrypts traffic, and scans for malware, spyware, and permission misuse by apps.


## Google Enforces September 30 Deadline for Android Developer Verification to Combat Malware Scams in Brazil, Indonesia, Singapore, and Thailand


Beginning **September 30, 2026** , **Google** will enforce a new security initiative requiring **Android developer verification** across certified devices in **Brazil, Indonesia, Singapore, and Thailand** , blocking normal installations of unregistered applications from both the **Google Play Store** and major third-party marketplaces like **Samsung, Xiaomi, OPPO, vivo, Honor, and Transsion** . Aimed at mitigating rampant **app-based malware and financial scams** in these highly targeted regions, the policy introduces the **Android Developer Verifier** service to validate creator identities via legal documentation and cryptographic signatures. While standard developers must pay a **one-time $25 fee** , Google has introduced a **free 20-device limited-distribution tier** for students and hobbyists, alongside a highly restricted, high-friction “advanced flow” for unverified sideloading. Despite these accommodations, the program faces intense pushback from over 70 organizations — including open-source repositories like **F-Droid** — who argue that placing a single corporation at the installation gateway threatens the existence of **privacy-focused, anonymous, and independent software distribution** globally.


Source:[The Hacker News](https://thehackernews.com/2026/06/google-sets-sept-30-deadline-for.html)


**How Malloc can help? 👉** As Google continues to monopolize the market and collect more and more user data while reducing privacy-friendly alternatives, users need to take steps to secure their devices. Malloc’s on-device system-wide VPN uses local block lists to block trackers and helps reduce first- and third-party data collection by blocking tracking domains. Malloc VPN also helps keep users private and secure by hiding their IP address and encrypting internet traffic in transit. Because Google cannot be trusted with user data and securing user privacy, trustworthy privacy tools like Malloc help keep users private.
