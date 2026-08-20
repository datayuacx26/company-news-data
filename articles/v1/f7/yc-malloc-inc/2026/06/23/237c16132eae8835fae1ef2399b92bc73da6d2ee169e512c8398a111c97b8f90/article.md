---
schema_version: "1.0.0"
document_id: "237c16132eae8835fae1ef2399b92bc73da6d2ee169e512c8398a111c97b8f90"
company_key: "yc-malloc-inc"
company: "Malloc Inc"
source_id: "yc-malloc-inc-rss-36b506f8e239"
canonical_url: "https://blog.mallocprivacy.com/malloc-privacy-weekly-7d721cb835a6"
published_at: "2026-06-24T11:05:32+00:00"
first_seen_at: "2026-07-24T10:30:15.411511+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:ad1172d8272105237d570eb4e040786981d90f4329c1c7fd32bff97bdf4e2828"
---

# Malloc Privacy Weekly

# Malloc Privacy Weekly


[Malloc](https://medium.com/@Ayu_Malloc?source=post_page---byline--7d721cb835a6---------------------------------------)


13 min read


·


Jun 24, 2026


--


In this latest edition of **Malloc’s Weekly Privacy** **Analysis** , we are covering the most critical threats that are wreaking havoc on the digital lives of citizens, businesses, and governments worldwide. We focus on mobile security and privacy, and have discussed in detail the threats and how users can safeguard themselves.


**WhatsApp has successfully disrupted a fresh spear-phishing campaign linked to the Israeli spyware firm NSO Group, the creators of the notorious Pegasus surveillance tool, and is now seeking to hold the company in contempt of court for violating a standing permanent injunction.** This latest activity, detected after user reports and involving **test accounts and groups** that WhatsApp promptly removed, relied on **malicious external links** rather than direct app vulnerabilities — tactics echoing prior NSO-associated one-click operations that trick users into visiting harmful sites. The company publicly shared three **indicator domains** — **fr24cast.com** , **ghazacast.com** , and **ikhwancast.com** (with “Ikhwan” evoking the Muslim Brotherhood and others nodding to Gaza or France 24 news) — to help users, researchers, and organizations check for exposure across messaging, email, or SMS.


**Cybercriminals** are running a **targeted phishing campaign** against **Signal users** , especially **journalists** , **anti-CCP activists** , and **human rights defenders** , by sending unsolicited messages that impersonate **Signal Support** and falsely warn of a **synchronization problem** risking **permanent data loss** . Victims are instructed to navigate within the app to **Settings > Backups > Configure > View Recovery Key** , copy the unique **64-character recovery key** , and paste it into the fraudulent chat. This key is the sole cryptographic credential for Signal’s **Secure Backups** feature (introduced September 2025), which stores an **end-to-end encrypted** , **zero-knowledge** archive of message history plus the most recent media on Signal’s servers; the key is generated locally on the user’s device and never reaches Signal, so the company itself cannot decrypt or restore the data. Possessing the key alone is not enough — attackers must also achieve **account takeover** , typically by separately stealing a **registration code** or exploiting other vectors to re-register the victim’s phone number on a device they control, after which they can download and decrypt the entire backup to access years of private conversations, photos, and documents.


To learn more about these developments and other news, read the article below.


Press enter or click to view image in full size


Malloc Privacy Weekly


## WhatsApp Seeks Contempt Order Against NSO Group Over New Phishing Campaign


In a major escalation against mercenary surveillance, **WhatsApp** has disrupted a new **spear-phishing spyware campaign** linked to the Israeli firm **NSO Group** and is petitioning a **US federal court** to hold the company in **contempt of court** for violating a **permanent injunction** . Unlike previous zero-click exploits, this recent activity bypassed internal platform vulnerabilities by utilizing malicious external links — specifically targeting users through domains like **fr24cast.com** , **ghazacast.com** , and **ikhwancast.com** that mimic news and political themes — to lure victims into clicking outside the application. This breach occurs despite NSO Group being on the **US trade Entity List** since 2021 and already legally barred from targeting WhatsApp’s infrastructure or its users following a landmark ruling on a 2019 cyberattack. In response to this persistent threat, the messaging platform has purged suspicious test accounts, reinforced that its default **end-to-end encryption** remains secure, and pledged financial support to the **Spyware Accountability Initiative** to bolster global civil society defenses against commercial espionage.


Source:[Hack Read](https://hackread.com/whatsapp-blocked-pegasus-spyware-campaign-nso/)


**How Malloc can help? 👉** Malloc keeps you safe and private online by detecting and blocking spyware, malware, and other malicious domains in real time. It does this using advanced spyware indicators, monitoring your app behavior and data transmission and alerts users when suspicious activity is detected on user device. Malloc can detect the misuse of Android accessibility services exploited by malware and spyware, alerting users promptly to help them stay protected.


## The Rise of Asin Android Spyware Targeting Arabic-Speaking Media and OSINT Researchers


Arabic-speaking users, particularly **journalists and open-source intelligence (OSINT) researchers** , are being targeted by a newly discovered **Android spyware codenamed Asin** . Distributed through coordinated campaigns utilizing **fraudulent websites and social media channels** on platforms like Facebook and Telegram, the spyware masquerades as **government news platforms, secure PDF readers, and interactive war-tracking maps** . To execute its payload, the malware relies on victims **manually installing malicious APKs** on modern devices — such as those running **Android 15** — and granting extensive system permissions, which enables the software to seamlessly combine **legitimate application utility with stealthy spyware capabilities** to conduct covert surveillance.


Source:[The Hacker News](https://thehackernews.com/2026/06/android-spyware-asin-targets-arabic.html?m=1)


**How Malloc can help? 👉** Malloc keeps you safe and private online by detecting and blocking spyware, malware, and other malicious domains in real time. It does this using advanced spyware indicators, monitoring your app behavior and data transmission in real time. Stay connected to Malloc VPN at all times for maximum security and set the Android auto-scan feature to perform Malloc’s security scans daily.


## Phishing Campaign Targeting Signal Backup Recovery Keys Exposes Historical User Data


A highly targeted **phishing campaign** is currently exploiting the platform’s encrypted cloud storage feature by sending fraudulent messages impersonating **Signal Support** to manipulate users — especially **journalists, activists, and human rights defenders** — into sharing their highly sensitive **64-character recovery keys** . Under the false pretense of resolving urgent synchronization issues to prevent permanent data loss, attackers attempt to steal these unique, locally generated keys, which represent the sole decryption tool for cloud-stored message archives under Signal’s **zero-knowledge security architecture** . Unlike previous account-hijacking attempts that only intercepted future communications, obtaining this recovery key allows threat actors who gain control of an account to download and decrypt **entire past message archives, photos, and documents** . To mitigate this critical threat, users must strictly adhere to **security best practices** by never sharing credentials, verifying account alerts directly within the official app, securing keys in **password managers** , enabling **Registration Lock** , and utilizing **disappearing messages** to minimize historical data exposure.


Source:[Cyber Insider](https://cyberinsider.com/signal-users-targeted-by-attackers-seeking-backup-recovery-keys/)


**How Malloc can help? 👉** Malloc has anti-phishing protection and blocks all kinds of malware and spyware domains locally using its on-device VPN. It also blocks trackers and insecure HTTP traffic device-wide. Stay connected to Malloc VPN at all times for maximum privacy. Malloc VPN, via its IP address obfuscation and encrypted tunnel, also helps secure user privacy by limiting the user details sent to advertisers.


## Stealthy MagicAd Malware Bypasses Android OS Constraints to Flood Devices with Persistent Advertisements


The newly discovered Android trojan, **MagicAd** , is a sophisticated malware strain disguised within numerous gaming and utility applications on legitimate platforms like **Xiaomi’s GetApps** and the **Samsung Galaxy Store** . To evade detection, the malware actively **inspects the host environment** for analysis tools, virtual machines, and blacklisted network addresses before hiding its application icon and deploying persistent background services. It systematically bypasses standard Android overlay permission restrictions by loading advertisements as **Translucent Activities** or leveraging deep system integrations, such as **Android Binder** and **system app intents** , to force-feed ads on Xiaomi and Vivo devices. Furthermore, **MagicAd** implements a universal exploit that plays a **silent, decrypted audio file** to hijack global media controls, allowing it to secretly launch ads across almost all Android platforms, including Amazon Fire TV. Benefiting from **aggressive persistence mechanisms** like task scheduling and virtual screens, this threat continuously self-restarts, making regular **unfamiliar app audits** , timely system updates, and robust **mobile security software** vital defense strategies.


Source:[Cyber Security News](https://cybersecuritynews.com/new-magicad-android-malware-flood-device/)


**How Malloc can help? 👉** Malloc provides strong protection against advertisement fraud and advertisement bombing attacks by blocking third-party advertisement domains locally device-wide through its on-device VPN and local tracker blocklists. It further provides enhanced protection against fraud and malware through its anti-phishing protection and malicious app scans. Malloc VPN, via its IP address obfuscation and encrypted tunnel, also helps secure user privacy by limiting the user details sent to advertisers.


## Sophisticated Android Banking Trojan “OverlayPhantom” Targets Global Financial and Cryptocurrency Application


A dangerous and highly sophisticated Android banking trojan known as **OverlayPhantom** has been actively targeting users across ten Western nations, compromising over **180 banking, financial services, and cryptocurrency applications** . Spreading via **malicious links** disguised as trusted platforms like ID Austria or TikTok, the malware initiates a **two-stage infection process** where a dropper app tricks victims into installing a fake system update. Once installed, it disguises itself as **Google Play Services** and exploits Android’s **Accessibility Service** to establish persistent control and communicate with a **Command and Control (C&C) server** . By leveraging multi-port communication and the **MediaProjection API** for live screen streaming, remote attackers can execute over **30 remote commands** , including simulating screen gestures, manipulating clipboard data, and launching unauthorized transactions. Furthermore, the trojan employs highly convincing **WebView HTML overlay attacks** that display counterfeit phishing screens over legitimate financial apps to silently harvest login credentials. To mitigate this severe threat, users must download applications exclusively from **official app stores** , strictly avoid granting **Accessibility Service permissions** to unfamiliar applications, enable **multi-factor authentication** , and keep their device operating systems updated with the latest security patches.


Source:[Cyber News](https://cybersecuritynews.com/android-banking-trojan-overlayphantom/)


**How Malloc can help? 👉** Malloc App warns against spyware and malware, blocks phishing and malicious domains, blocks trackers and insecure HTTP traffic, and encrypts and hides user internet traffic through its VPN. It also detects and alerts users to the use of Android accessibility services used by malware and spyware, including banking trojans and other Remote Access Trojans. Always stay connected to Malloc VPN and enable auto-scan for complete protection against threats.


## NFCShare Android Malware Steals Physical Card Data via Weaponized Banking Apps


A refined campaign of the **NFCShare Android banking trojan** is actively targeting European financial consumers by masquerading as legitimate banking applications to harvest physical credit and debit card information. Distributed through sophisticated **phishing sites** , shortened URLs, and compromised GitHub repositories, the malware tricks users into sideloading malicious **APK files** disguised as urgent security updates. Once active on a device, the trojan uses a fake **WebView user interface** to prompt victims to hold their payment cards close to the phone, utilizing the device’s native **Near Field Communication (NFC)** technology to scan and extract the **card’s primary account number (PAN), expiration date, and label** , while simultaneously capturing the user’s **four-digit PIN** . This harvested financial data is compiled and exfiltrated to a **WebSocket command-and-control (C2) server** to facilitate immediate card cloning and unauthorized fraud. To hinder security mitigation efforts, the malware incorporates **aggressive packaging tactics** , including **malformed ZIP paths** within its archive files, which intentionally crash automated analysis pipelines and bypass standard security detection systems while remaining fully executable on victim devices.


## Get Malloc’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Source:[GB Hackers](https://gbhackers.com/nfcshare-android-malware/)


**How Malloc can help? 👉** Malloc App provides strong protection against Phishing Threats via its on-device VPN, and helps protect users when connected to the internet via its remote VPN. It also has additional security features like HTTP traffic blocking, protection against malicious domains and trackers, and includes malicious app scanning, device security scanning, and download file scanning.


## Silent WhatsApp Account Takeovers via Zero-Click iOS 16 Exploit Chain


A highly sophisticated **zero-click exploit chain** targeting **iOS 16** users allows threat actors to silently hijack **WhatsApp accounts** to commit **financial fraud** without requiring any user interaction or displaying unauthorized sessions in the application’s “Linked Devices” interface. This compromise relies on two critical vulnerabilities: **CVE-2025–43300** , an out-of-bounds write flaw in Apple’s **ImageIO framework** triggered by malformed images, and **CVE-2025–55177** , which involves **insufficient authorization of linked-device synchronization messages** within WhatsApp. By exploiting this sequence, attackers extract **cryptographic session handshake material** from vulnerable devices (specifically iOS builds below 16.7.12 and WhatsApp versions before[2.25.21.73](https://2.25.21.73/) ) to run a parallel client that continuously **resynchronizes with servers** , effectively hiding the rogue session from the user. Once inside, attackers exploit visibility into recent chats to send **convincing, fraudulent payment and wire-transfer requests** to the victim’s contacts. Because traditional social engineering warning signs are completely bypassed by this zero-interaction threat, immediate mitigation requires **updating iOS and WhatsApp to the latest patched versions** , utilizing **chat locks** , or **reinstalling the application** to terminate active malicious sessions.


Source:[GB Hackers](https://gbhackers.com/new-zero-click-whatsapp-account-takeover-attack-targets-ios-16-users/)


**How Malloc can help? 👉** Malloc blocks malicious URLs, including links to spyware, malware, and phishing domains, as well as crypto-mining sites and inappropriate content. It prevents malicious apps from connecting to command-and-control servers and blocks advertisement domains and trackers to protect against ad fraud across all apps and browsers. By utilizing a powerful on-device VPN, Malloc acts as a robust on-device firewall that monitors network traffic while preserving user privacy and safeguarding your digital identity whenever you connect to the internet.


## Brazilian Food Delivery Giant iFood Facing Major Data Breach Controversy


The Brazilian food delivery platform **iFood** has confirmed a **December 2025 data breach** affecting **1.2 million users** , resulting in the unauthorized access of personal information including **names, phone numbers, physical addresses, and CPF taxpayer numbers** . While the company assured customers that sensitive credentials like **passwords and financial data remain secure** , a cybercriminal using the alias **bacen** has claimed to possess a much larger dataset of **43.8 million records** , threatening to leak it if a **ransom** is not paid. Although the platform disputes these massive figures, hacker claims persist that the confirmed leak and the larger database are separate security issues. Meanwhile, citing **Brazil’s General Data Protection Law (LGPD)** and regulatory criteria set by the **ANPD** , the company opted **not to formally notify affected users** , arguing the incident poses no relevant risk, despite cybersecurity warnings regarding the high utility of stolen **CPF numbers for identity fraud** .


Source:[Hacker Read](https://hackread.com/ifood-confirms-data-breach-brazil-users/)


**How Malloc can help? 👉** Malloc’s Malicious App Scan helps detect malicious and rogue apps, their permission misuse, and trackers, and warns users. Malloc helps remove these apps from the device to protect user privacy. It also comes with a VPN to block trackers and HTTP requests, hide IP locations, and encrypt traffic.


## Motorola Smart Feed Affiliate Hijacking Controversy


Recent findings revealed that Motorola’s preinstalled **Smart Feed app** was **silently hijacking launches of the Amazon Shopping app** to inject **unauthorized affiliate referral codes** and generate third-party commissions. When users launched Amazon from the app drawer, the system intercepted the action and routed the connection through a tracking domain affiliated with the advertising platform **Device Native** before redirecting back to Amazon with an affiliate ID. Although Motorola characterized the incident as an **unintended configuration error** in its app search experience and subsequently **released updates to disable the redirect** , the controversy highlights growing privacy concerns regarding **privileged on-device bloatware** and **stealth monetization tactics** employed by reputable manufacturers.


Source:[GB Hackers](https://gbhackers.com/motorola-app-allegedly-hijacks-amazon-app-activity/)


## Security Risks of Smart TV Apps Functioning as Residential Proxy Nodes


**Free applications on Samsung and LG smart TVs** are raising significant privacy and security concerns by silently converting home devices into **residential proxy nodes** using **Bright Data’s software development kit (SDK)** . This embedded SDK establishes a **persistent WebSocket connection** to route third-party web traffic — often used for **AI-related data collection and public web scraping** — through the user’s home network, masking unauthorized activity as legitimate residential traffic. Because smart TVs are continuously powered, connected to Wi-Fi, and rarely monitored by security tools, they serve as ideal targets for this bandwidth exploitation. While users often agree to vague consent prompts in exchange for free, ad-supported services, they unknowingly expose their **device state information** (such as CPU, memory, and bandwidth usage), consume their own internet data caps, and risk having **suspicious or malicious third-party web activity** trace directly back to their home IP addresses. To mitigate these security threats, users must carefully inspect installed applications, avoid ambiguous consent prompts, and actively **block associated proxy domains** at the **DNS or router level** .


Source:[Cyber Press](https://cyberpress.org/smart-tvs-become-proxies/)


**How Malloc can help? 👉** Malloc offers complete protection against all kinds of mobile threats — it blocks spyware connections in real time, thwarts phishing attempts, blocks trackers and insecure HTTP websites, and detects permission misuse by apps. Malloc VPN helps protect your mobile device when connected to unsafe networks, keeping you protected against the ever-increasing number of mobile security threats.


## RCS-Powered Fake Call Detection: Android’s New Defense Against AI Voice-Cloning and Spoofing Scams


To combat the rising threat of sophisticated **AI voice-cloning scams** and **phone number spoofing** targeting individuals and businesses, Android is rolling out a global **fake call detection feature** for devices running Android 12 or later. Operating silently in the background of the default calling app, this security tool acts as a **digital handshake** powered by end-to-end encrypted **Rich Communication Services (RCS) technology** . When a contact call is received, the system verifies its legitimacy by exchanging **silent confirmation signals** between the two devices; if this signal is missing or the caller’s device denies initiating the call, an **on-screen warning** immediately alerts the recipient to a potential impersonation attempt and advises them to hang up.


Source:[Help Net Security](https://www.helpnetsecurity.com/2026/06/03/android-fake-call-detection-feature/)


**How Malloc can help? 👉** Stay tuned as Malloc continues to add more features to protect users from vishing threats using advanced AI detection. Even then, Malloc offers strong protection against scams and harmful actions done by users over fraudulent calls, as well as protection against smishing, through its advanced phishing protection and by detecting and blocking spyware, malware, and other malicious domains in real time. Stay protected with Malloc!


## Proactive Anti-Theft Security: Apple’s New Multi-Signal Auto-Lock Feature for iPhones


To address the vulnerability of unlocked devices being snatched directly from users’ hands, a new **proactive theft prevention feature** is in development to **automatically lock iPhones in real time** during a snatch-and-grab event. While existing security tools only activate after a device is already stolen — leaving a dangerous window for thieves to exploit active sessions, access saved passwords, and modify account settings — this upcoming capability uses a **multi-signal detection engine** to stop unauthorized access instantly. By simultaneously analyzing **sudden accelerometer movements** indicative of a physical grab, the **abrupt physical separation from a paired Apple Watch** , and **unfamiliar location or network contexts** , the system can immediately determine if a theft is occurring. Once a grab is detected, the device instantly locks and **blocks access to critical functions** like Keychain credentials, Apple Pay, and Apple Account settings. This layered, hardware-integrated approach significantly **reduces false positives** during high-motion activities and successfully transforms mobile security from a reactive defense into an immediate, **sensor-driven protective response** .


Source:[Cyber Press](https://cyberpress.org/apple-developing-auto-lock-feature/)


**How Malloc can help? 👉** Malloc app for iPhones, along with the iPhone’s new protection features, is a great combination for users’ all-around privacy and security. Malloc’s numerous powerful security and privacy features, such as the device-wide tracker blocker, HTTP blocking, phishing and malicious domain protection, along with its XRay VLSS+ Reality Post + Quantum Encryption powered secure and fast VPN, device security scan, malicious app scan and download scan work in conjunction with the iPhone’s own protection features to safeguard users against all kinds of mobile threats, ranging from common scams to sophisticated state-sponsored spyware attacks.
