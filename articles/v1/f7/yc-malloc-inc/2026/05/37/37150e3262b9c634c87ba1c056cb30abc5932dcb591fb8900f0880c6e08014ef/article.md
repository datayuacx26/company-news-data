---
schema_version: "1.0.0"
document_id: "37150e3262b9c634c87ba1c056cb30abc5932dcb591fb8900f0880c6e08014ef"
company_key: "yc-malloc-inc"
company: "Malloc Inc"
source_id: "yc-malloc-inc-rss-36b506f8e239"
canonical_url: "https://blog.mallocprivacy.com/malloc-privacy-weekly-4828c75ed1f9"
published_at: "2026-05-11T11:05:39+00:00"
first_seen_at: "2026-07-24T10:30:15.411511+00:00"
fetched_at: "2026-07-28T20:51:07.653203+00:00"
content_hash: "sha256:bdd43af47d041aacda72396fc1cf6118544fe334b37babc229e69697bde7c7f8"
---

# Malloc Privacy Weekly

# Malloc Privacy Weekly


[Malloc](https://medium.com/@Ayu_Malloc?source=post_page---byline--4828c75ed1f9---------------------------------------)


10 min read


·


May 11, 2026


--


In this week’s edition of **Malloc Privacy Weekly** , we highlight the most impactful **mobile cybersecurity updates** . Our report covers **Android spyware** such as **BirdCall** and **Cerberus stalkerware** , the abuse of **Telegram Mini Apps** to distribute **malware** , and the **CallPhantom scam** . We also examine **Windows Phone Link exploits** used to steal **private information** , ongoing **stealthy banking trojan campaigns** , and critical **OS vulnerabilities** . Additionally, we discuss the rise of **botnets** , **state-backed official apps** that compromise **user privacy** , and **critical security flaws** in platforms like **WhatsApp** that enable **malicious URL execution** .


**North Korean state-sponsored hackers** from the **ScarCruft** group, also known as **APT37** or **Ricochet Chollima** , executed a **supply-chain attack** by compromising the **sqgame.net** gaming platform, which offers traditional **Yanbian-themed card and board games** to **ethnic Koreans** living in the **Yanbian Korean Autonomous Prefecture** in **China** near the North Korean border — a key transit point for **defectors** and refugees. The attackers trojanized **Android APKs** (such as those for popular titles like Yanbian Red Ten) and Windows update components to distribute a previously undocumented **Android variant of the BirdCall backdoor** , developed since around **October 2024** with at least seven versions up to mid-2025 (internally named “zhuagou”), alongside its known Windows version that evolved from **RokRAT** . This **advanced spyware** gathers extensive **surveillance data** such as **contacts, call logs, SMS messages, device details** including IMEI, MAC address, rooted status, and geolocation, **periodic screenshots** , **evening audio recordings** from the microphone between 7 pm and 10 pm local time, and targeted files of interest like documents and private keys, while maintaining persistence by playing a silent audio loop and exfiltrating everything via legitimate **cloud storage services** .


**Fraudulent Android applications** operating under the **CallPhantom** campaign infiltrated the **Google Play Store** with **28 deceptive apps** that collectively amassed more than **7.3 million downloads** — including one title alone exceeding **3 million installs** — before Google swiftly removed them all following an official report in December 2025. These scams, which first surfaced through user complaints on Reddit as early as November 2025, primarily preyed on curious individuals in **India** and the wider **Asia-Pacific region** by promising the impossible: instant access to **call histories, SMS records, and even WhatsApp call logs** for **any phone number** , often with India’s **+91 country code** preselected to heighten relevance. Victims were lured into paid **subscriptions** costing anywhere from about **$6 to $80** via official **Google Play billing** , **UPI** services such as **Google Pay, PhonePe, and Paytm** , or even direct **payment card forms** embedded in the apps — methods that sometimes violated store policies and complicated refunds. In reality, the apps featured **simple user interfaces** that requested **no sensitive permissions** whatsoever because they contained **zero actual functionality** to retrieve real data; instead, they delivered only **randomly generated or hardcoded fabricated information** directly from their source code, sometimes tricking users further with deceptive exit notifications claiming details had been emailed. One app even masqueraded under the misleading developer name **“Indian gov.in”** to falsely imply official legitimacy, underscoring how the scheme exploited basic human curiosity while leaving affected users to seek refunds directly from banks or third-party providers for non-Google transactions.


To learn more about these developments and other news, read the article below.


Press enter or click to view image in full size


Malloc Privacy Weekly


## APT37 hacks gaming platform to spread new BirdCall Android spyware


The **North Korean state-sponsored threat actor APT37** , also known as **ScarCruft** , has launched a sophisticated **supply-chain attack** by compromising a gaming platform to distribute malware. By embedding **trojanized components** into legitimate Windows and Android games, the group has deployed a **new mobile variant of the BirdCall spyware** alongside existing backdoors like RokRAT. This campaign specifically targets individuals in the **Yanbian region** for espionage, utilizing malicious capabilities such as **audio recording** , **credential theft** , and **data exfiltration** from mobile devices. To avoid detection, the spyware leverages **legitimate cloud storage services** for its command-and-control infrastructure, blending its activities with normal network activity while maintaining **cross-platform surveillance** operations.


Source:[Cyber Insider](https://cyberinsider.com/apt37-hacks-gaming-platform-to-spread-new-birdcall-android-spyware/)


**How Malloc can help? 👉** Malloc keeps you safe and private online by detecting and blocking spyware, malware, and other malicious domains in real time. It does this using advanced spyware indicators, monitoring your app behavior and data transmission in real time. Stay connected to Malloc VPN at all times for maximum security and set the Android auto-scan feature to perform Malloc’s security scans daily.


## Telegram Mini Apps abused for crypto scams, Android malware delivery


Cybercriminals are exploiting the **Telegram Mini App** feature through a sophisticated fraud platform known as **FEMITBOT** to execute wide-scale **cryptocurrency scams** and distribute **Android malware** . By utilizing **embedded WebViews** and **automated bots** , attackers create highly convincing, app-like experiences that impersonate **well-known global brands** to deceive users directly within the messaging interface. These campaigns often display **fraudulent account balances** and use **urgency tactics** to lure victims into making deposits or performing referral tasks, while simultaneously tricking them into **sideloading malicious APK files** . The underlying infrastructure leverages a **shared backend** and **tracking pixels** to optimize their phishing efforts and exfiltrate data, posing a significant risk to users who engage with **unverified financial bots** or download software outside of official app stores.


Source:[Bleeping Computer](https://www.bleepingcomputer.com/news/security/telegram-mini-apps-abused-for-crypto-scams-android-malware-delivery/)


**How Malloc can help? 👉** Malloc keeps you safe from malware, spyware, and rogue apps via its device security scan, malicious app scan, and downloaded files scan. It also detects and alerts users to the use of Android accessibility services used by malware and spyware, including banking trojans and other Remote Access Trojans.


## Cerberus Stalkerware on Google Play Leverages Accessibility Abuse and Firebase for Remote Control


A dangerous Android **stalkerware** family known as **Cerberus** has been discovered hiding on the **Google Play Store** under the guise of a legitimate **anti-theft tool** . Operating via a subscription model, this malicious software enables abusers to **silently track locations** , **record audio** , **take photos** , and even **remotely wipe devices** without the victim’s knowledge. The software leverages **Google’s Firebase infrastructure** for command-and-control operations and exploits **Android’s accessibility services** to bypass security restrictions and perform **automated touch gestures** . Notably, it can simulate a **fake device shutdown** , keeping the camera and microphone active while the screen appears dark. Because it reports permission changes to the controller in **real-time** , survivors are cautioned that attempting to inspect or remove the app may immediately alert the abuser.


Source:[Cybersecurity News](https://cybersecuritynews.com/cerberus-stalkerware-on-google-play-leverages-accessibility/)


**How Malloc can help? 👉** Malloc keeps you safe and private online by detecting and blocking spyware, malware, and other malicious domains in real time. It does this using advanced spyware indicators, monitoring your app behavior and data transmission, and alerts users when suspicious activity is detected on their device. Malloc can detect the abuse of Android accessibility services exploited by malware and spyware, alerting users promptly to help them stay protected.


## CallPhantom Android scam reached 7.3 million downloads on Google Play


A widespread fraud campaign known as **CallPhantom** successfully deceived over **7.3 million users** by distributing **28 fraudulent apps** through the **Google Play Store** that promised unauthorized access to **call histories** , **SMS records** , and **WhatsApp logs** . Primarily targeting users in **India and the Asia-Pacific region** , the scheme exploited a “curiosity gap” to sell **fabricated, randomly generated data** drawn from hardcoded lists rather than actual communications. To collect funds, the operators utilized a combination of **official Google Play billing** , **third-party UPI links** , and **embedded payment forms** , allowing them to bypass standard security policies and make **refunds nearly impossible** for those who paid outside of Google’s official system. Although the apps have been removed, the incident highlights the persistent threat of **social engineering** and the use of **Firebase Cloud Messaging** to coordinate large-scale mobile scams.


Source:[Help Net Security](https://www.helpnetsecurity.com/2026/05/07/callphantom-android-scam-google-play/)


## Windows Phone Link Exploited by CloudZ RAT to Steal Credentials and OTPs


The **CloudZ remote access tool (RAT)** and a specialized **Pheno plugin** are being used in a novel attack campaign to **hijack the Microsoft Phone Link** application built into **Windows 10 and 11** . By exploiting this legitimate cross-device syncing feature, threat actors can **monitor active processes** and access the **SQLite database** where synchronized phone data is stored, allowing them to **intercept SMS messages** and **one-time passwords (OTPs)** . This technique is particularly dangerous because it enables the **theft of credentials** and the **bypass of two-factor authentication** without requiring the attacker to deploy malware on the mobile device itself. The intrusion typically begins with a **fake ConnectWise ScreenConnect executable** that uses a **.NET loader** and **PowerShell scripts** to establish **persistence** and **exfiltrate sensitive information** to a remote command-and-control server.


## Get Malloc’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Source:[The Hacker News](https://thehackernews.com/2026/05/windows-phone-link-exploited-by-cloudz.html)


**How Malloc can help? 👉** Malloc offers complete protection against all kinds of mobile threats — it blocks spyware connections in real time, thwarts phishing attempts, blocks trackers and insecure HTTP websites, and detects permission misuse by apps. Malloc VPN helps protect your mobile device when connected to unsafe networks, keeping you protected against the ever-increasing number of mobile security threats.


## Official White House app is one command away from tracking your precise location


The official **White House Android app** has been identified as containing highly invasive **tracking and data-profiling** capabilities that extend far beyond its stated purpose. Technical analysis reveals the software is capable of monitoring a user’s **precise GPS location** as frequently as every **4.5 minutes** and transmitting that data to **non-governmental infrastructure** . Furthermore, the application can **inject custom code** into third-party websites to forcefully remove **cookie consent popups** , **GDPR privacy banners** , and **digital paywalls** , potentially stripping away legal privacy protections. These risks are compounded by a lack of **certificate pinning** and the use of **unsecured external code sources** , which could leave users vulnerable to **arbitrary code execution** and unauthorized surveillance.


Source:[Tech Radar](https://www.techradar.com/pro/security/researcher-reveals-official-white-house-app-is-one-command-away-from-tracking-your-precise-location-every-4-5-minutes-app-can-also-inject-code-to-dodge-cookie-consent-gdpr-banners-and-paywalls)


**How Malloc can help? 👉** Our app, Malloc, available on[iOS](https://www.mallocprivacy.com/mobile-security/ios/) and[Android](https://www.mallocprivacy.com/mobile-security/android/) , comes with a great set of privacy-preserving and enhancing features that can be beneficial in identifying and removing malware, blocking trackers, providing a VPN, and monitoring the mic and camera to detect background eavesdropping by state sponsored Spyware apps, along with many other features.


## Millions at risk as Android trojans use devious trick to ‘magically’ disappear once installed


Four sophisticated **Android banking trojan campaigns** — RecruitRat, SaferRat, Astrinox, and Massiv — are targeting millions of users by posing as **legitimate job portals** , **streaming services** , or software updates to encourage **malicious sideloading** . These threats utilize advanced **stealth techniques** , such as **replacing app icons with blank images** to “vanish” from the device and **abusing accessibility permissions** to prevent uninstallation. Once active, the malware can **stream live screen content** , deploy **deceptive overlays** to harvest login credentials, and **intercept one-time passwords** across hundreds of financial and social media applications. Orchestrated via **encrypted command-and-control servers** , these campaigns bypass traditional security measures to facilitate **large-scale digital fraud** and unauthorized account access.


Source:[Tech Radar](https://www.techradar.com/pro/security/it-just-vanished-millions-at-risk-as-android-trojans-use-devious-trick-to-magically-disappear-once-installed)


**How Malloc can help? 👉** Malloc App on Android detects and warns against spyware and malware, blocks phishing and malicious domains, blocks trackers and insecure HTTP traffic, and encrypts and hides user internet traffic through its VPN. It also detects and alerts users to the use of Android accessibility services used by malware and spyware, including banking trojans and other Remote Access Trojans. Always stay connected to Malloc VPN and enable auto-scan for complete protection against threats.


## Botnet Hijacks ADB-Exposed Android Devices to Target Minecraft Servers


The **xlabs_v1 botnet** , a **Mirai-derived** malware, is actively compromising **Android devices** , such as **Smart TVs and routers** , by exploiting exposed **Android Debug Bridge (ADB)** ports on **TCP 5555** . This operation creates a distributed **DDoS-for-hire service** specifically designed to disrupt **Minecraft servers** and other **gaming infrastructure** through **21 different flood modes** , including protocols like **RakNet** . To monetize the network, the malware performs **bandwidth-profiling** via **Speedtest endpoints** , categorizing infected hosts into tiers based on their **upstream capacity** . By utilizing **stealth techniques** like process renaming and **ChaCha20-encrypted** communications with its **command-and-control (C2)** infrastructure, the botnet maintains a persistent presence across millions of unauthenticated devices to launch high-bandwidth **volumetric attacks** .


Source:[GB Hackers](https://gbhackers.com/adb-exposed-android-devices/)


**How Malloc can help? 👉** Malloc’s extensive feature list helps provide complete protection against all kinds of mobile threats. It detects and warns against spyware and malware, blocks phishing and malicious domains, blocks trackers and insecure HTTP traffic, and encrypts and hides user internet traffic through its VPN. Malloc can detect the misuse of Android accessibility services exploited by Android malware and spyware, and alerts users. Always stay connected to Malloc VPN and enable auto-scan for complete protection against threats.


## Critical Android Zero-Click Vulnerability Enables Remote Shell Access


A highly critical **zero-click vulnerability** tracked as **CVE-2026–0073** has been identified within the **Android System component** , specifically impacting the **adbd (Android Debug Bridge Daemon)** . This flaw allows an attacker in **physical proximity** or on the **same local network** to achieve **remote shell access** and execute **arbitrary code** without any user interaction or elevated privileges. Affecting **Android versions 14, 15, and 16** , the exploit bypasses standard **sandboxing protections** , potentially exposing sensitive data. To mitigate this **Remote Code Execution (RCE)** risk, users must update their devices to the **May 2026 security patch level** (2026–05–01), which is being distributed via **Google Play system updates** to ensure rapid deployment across the mobile ecosystem.


Source:[GB Hackers](https://gbhackers.com/critical-android-zero-click-vulnerability/)


**How Malloc can help? 👉** While it’s up to the vendors to patch critical vulnerabilities, and users should keep their devices updated, Malloc can further enhance your security and privacy through its extensive features. Malloc detects and blocks spyware domains in real-time, blocks trackers and insecure HTTP traffic, and also protects against malicious domains and phishing attacks.


## WhatsApp Security Flaw Enables Malicious URL Execution Through Instagram Reels


**Meta** has patched two significant **security vulnerabilities** in **WhatsApp** affecting **iOS, Android, and Windows** platforms. The most severe flaw, **CVE-2026–23866** , exploits unvalidated **AI-rich response messages** in **Instagram Reels** to trigger **arbitrary URLs** , which can lead to **unauthorized command execution** or the launching of external applications. Additionally, **CVE-2026–23863** targets **WhatsApp for Windows** by utilizing **NUL byte manipulation** to **spoof file extensions** , tricking users into running **malicious executables** disguised as harmless documents. To mitigate these risks, users must immediately update to the **latest software versions** to ensure protection against these **remote execution** and **attachment spoofing** techniques.


Source:[GB Hackers](https://gbhackers.com/whatsapp-security-flaw-enables-malicious-url-execution/)


**How Malloc can help? 👉** Malloc blocks malicious URLs, including links to spyware, malware, and phishing domains, as well as crypto-mining sites and inappropriate content. It prevents malicious apps from connecting to command-and-control servers and blocks advertisement domains and trackers to protect against ad fraud across all apps and browsers. By utilizing a powerful on-device VPN, Malloc acts as a robust on-device firewall that monitors network traffic while preserving user privacy and safeguarding your digital identity whenever you connect to the internet.
