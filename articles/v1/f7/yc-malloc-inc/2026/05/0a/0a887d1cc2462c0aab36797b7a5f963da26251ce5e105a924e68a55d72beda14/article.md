---
schema_version: "1.0.0"
document_id: "0a887d1cc2462c0aab36797b7a5f963da26251ce5e105a924e68a55d72beda14"
company_key: "yc-malloc-inc"
company: "Malloc Inc"
source_id: "yc-malloc-inc-rss-36b506f8e239"
canonical_url: "https://blog.mallocprivacy.com/malloc-privacy-weekly-24bce666f0ff"
published_at: "2026-05-20T11:15:22+00:00"
first_seen_at: "2026-07-24T10:30:15.411511+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:b23788a076b63833e4dbc1700e6891d433028084c2c2cd5c6ea1efdb16bbb856"
---

# Malloc Privacy Weekly

# Malloc Privacy Weekly


[Malloc](https://medium.com/@Ayu_Malloc?source=post_page---byline--24bce666f0ff---------------------------------------)


11 min read


·


May 20, 2026


--


Welcome to the latest edition of **Malloc Privacy Weekly** . We bring you the most essential **mobile cybersecurity updates** to help you stay **safe and private online** . This week’s news is significant, covering the **TrickMo Android Banking Trojan** , Android’s new **Intrusion Logging** , Google’s updated **reCAPTCHA system** , and the **Android 16 VPN Bypass vulnerability** . Additionally, we explore **End-to-End Encrypted RCS Messaging** , **messaging app profile leaks** , **AI-powered scams** , the **privacy threats** stemming from **generative AI applications** , the **end of end-to-end encryption for Instagram DMs** , and the escalating **safety risks of on-body tech** .


**Recent advancements in mobile threats** have introduced an evolved **TrickMo Android banking trojan** that operates as a highly effective **Device Take Over (DTO) malware** , specifically targeting **banking, fintech, cryptocurrency wallets, and authenticator applications** on Android devices, with active campaigns focused on users in **France, Italy, and Austria** . Typically distributed via deceptive **TikTok-themed** apps and streaming lures spread over social media platforms, the malware tricks victims into granting **accessibility service** permissions, unlocking full remote interactive control that includes **credential phishing** through realistic fullscreen overlays, **keylogging** with context awareness, **live screen recording and streaming** , **real-time SMS and notification interception** to silently suppress **one-time passwords (OTPs)** , and bidirectional gesture replay for complete device manipulation. In a deliberate platform redesign prioritizing **stealth, resilience, and flexibility** , the malware has shifted its primary **command-and-control (C2) communications** entirely onto **The Open Network (TON)** , which is a legitimate decentralized peer-to-peer overlay originally tied to Telegram to bypass traditional DNS takedowns and blend indistinguishably with normal traffic. Enhanced **network reconnaissance** combined with **SSH tunneling** and an **authenticated SOCKS5 proxy** transform infected handsets into programmable **network pivots** and traffic-exit nodes that route malicious activity through the victim’s own IP address, effectively defeating IP-based fraud detection on banking and e-commerce services.


Google has introduced **Intrusion Logging** , a pioneering opt-in security feature integrated into **Android’s Advanced Protection Mode** that empowers **high-risk users** such as **journalists, activists, human rights defenders, and dissidents** to detect and investigate sophisticated **spyware attacks** and law enforcement **forensic tool intrusions** . Rolling out now to **Google Pixel devices** running the **Android 16 December update and newer** , with broader vendor support expected soon, the capability generates daily **tamper-resistant forensic logs** that capture critical events like device unlocks, app installations and uninstallations, network connections to suspicious servers, **Android Debug Bridge** access, and attempts to delete evidence then stores them **end-to-end encrypted** in the user’s Google account for up to **12 months** , ensuring spyware cannot erase the records while keeping the data fully under user control for sharing with trusted researchers. Developed in partnership with **Amnesty International** and **Reporters Without Borders** , this industry-first tool addresses longstanding gaps in Android forensics by providing persistent, privacy-preserving evidence that was previously overwritten or insufficient, delivering a major advancement in mobile threat investigation comparable to protections offered by Apple’s Lockdown Mode.


To learn more about these developments and other news, read the article below.


Press enter or click to view image in full size


Malloc Privacy Weekly


## TrickMo Android Banking Trojan Evolves: New Variant Leverages TON Blockchain for Stealth C2 and Turns Infected Devices Into Network Pivots


The **TrickMo Android banking trojan** has evolved into a sophisticated **device takeover malware** that leverages **The Open Network (TON) blockchain** for **stealthy command-and-control (C2) communications** . By utilizing **decentralized .ADNL addresses** and an **embedded local proxy** , this new variant effectively evades **traditional DNS takedowns** and network-edge detection, as its encrypted traffic blends seamlessly with legitimate decentralized activity. Primarily targeting **banking and cryptocurrency wallets** in Europe through apps masquerading as popular social media or system services, the malware retains core offensive capabilities such as **keylogging, screen recording, and SMS interception for OTP hijacking** . However, its latest update introduces advanced networking tools — including **SSH tunneling and SOCKS5 proxying** — which transform infected smartphones into **programmable network pivots** and **traffic-exit nodes** . These features allow attackers to perform **remote network reconnaissance** and route malicious traffic through the **victim’s own internal network environment** , effectively bypassing **fraud-detection systems** that rely on IP-based signatures while maintaining a persistent foothold for future exploitation.


Source: 1.[Bleeping Computer](https://www.bleepingcomputer.com/news/security/trickmo-android-banker-adopts-ton-blockchain-for-covert-comms/) 2.[The Hacker News](https://thehackernews.com/2026/05/new-trickmo-variant-uses-ton-c2-and.html)


**How Malloc can help? 👉** Malloc keeps you safe from malware, spyware, and rogue apps via its device security scan, malicious app scan, and downloaded files scan. It also detects and alerts users to the use of Android accessibility services used by malware and spyware, including banking trojans and other Remote Access Trojans.


## Android Adds Intrusion Logging for Sophisticated Spyware Forensics


Starting with the Android 16 update, the mobile ecosystem introduces **Intrusion Logging** , a specialized **opt-in forensic tool** designed to combat **sophisticated spyware** by recording comprehensive **device and network activity** . These logs, which capture **app process starts** , **DNS lookups** , **IP connections** , and **file transfers** , are **end-to-end encrypted** and stored on secure servers, ensuring that even **persistent malware** cannot access, manipulate, or delete the data. Beyond forensics, the platform is integrating **verified financial calls** to prevent **scammer impersonation** , **Live Threat Detection** to identify malicious behaviors like **accessibility overlays** , and **SMS OTP protection** to block credential theft. These enhancements, alongside **post-quantum cryptography** and **AI-driven scam detection** , provide **high-risk individuals** and general users with a resilient defense against **mobile data extraction** and **automated financial fraud** .


Source:[The Hacker News](https://thehackernews.com/2026/05/android-adds-intrusion-logging-for.html)


**How Malloc can help? 👉** The Malloc app for Android, along with Android’s own default protection, is a great combination for users’ all-around privacy and security. Malloc’s numerous powerful security and privacy features, such as the device-wide tracker blocker, HTTP blocking, phishing and malicious domain protection, along with its WireGuard-based encrypted VPN, device security scan, malicious app scan, download scan, and microphone and camera usage alerts and protection, work in conjunction with Android’s own protection features to safeguard users against all kinds of mobile threats, ranging from common scams to sophisticated state-sponsored spyware attacks.


## Google’s new reCAPTCHA system restricts access to the open web


The transition to the **Cloud Fraud Defense** platform marks a significant shift in **online verification** , as **reCAPTCHA** now increasingly utilizes **QR code challenges** that mandate the presence of **Google Play Services** or Apple’s **attestation frameworks** . While marketed as an **anti-abuse tool** , this move has drawn sharp criticism for effectively implementing **hardware-based attestation** , which restricts access to the **open web** for users of **privacy-focused operating systems** like **GrapheneOS** and **CalyxOS** . By equating the lack of **proprietary system services** with **suspicious traffic** , the update essentially creates a **digital lock-out** for individuals who prioritize **data sovereignty** , including **journalists and activists** . This strategy is being viewed as a functional revival of the controversial **Web Environment Integrity** proposal, positioning tech giants as **gatekeepers** who can dictate which **hardware and software environments** are deemed “trustworthy” enough to browse the internet.


Source:[Cyber Insider](https://cyberinsider.com/googles-new-recaptcha-system-restricts-access-to-the-open-web/)


**How Malloc can help? 👉** As Google continues to monopolize the market and collect more and more user data while reducing privacy-friendly alternatives, users need to take steps to secure their devices. Malloc’s on-device system-wide VPN uses local block lists to block trackers and helps reduce first- and third-party data collection by blocking tracking domains. Malloc VPN also helps keep users private and secure by hiding their IP address and encrypting internet traffic in transit. Because Google cannot be trusted with user data and securing user privacy, trustworthy privacy tools like Malloc help keep users private.


## Android 16 VPN Bypass Lets Malicious Apps Reveal Users Real IP Address


A critical privacy vulnerability in **Android 16** , known as the **Tiny UDP Cannon** , allows malicious applications with **standard internet permissions** to **bypass VPN protections** and expose a user’s **real IP address** . This flaw centers on a design weakness within the **ConnectivityManager service** , specifically the **registerQuicConnectionClosePayload** method, which permits an app to register data with the high-privilege **system_server** process. Because this system process is not bound by **VPN routing rules** , it transmits the attacker-controlled payload directly over the **physical network interface** (such as Wi-Fi) once the app session ends, effectively circumventing **Always-On VPN** and **lockdown security policies** . Despite the potential for **unauthorized user tracking** and **data exfiltration** outside encrypted tunnels, the issue has been classified as **“Won’t Fix”** by developers, leaving users to rely on manual **ADB-based mitigations** to disable the vulnerable QUIC feature and maintain their **network anonymity** .


Source:[Cyber Security News](https://cybersecuritynews.com/android-16-vpn-bypass/)


**How Malloc can help? 👉** One should only use a VPN from trusted and secure providers who follow industry best practices. Malloc VPN has the most security features of any VPN, is Google Play Verified for meeting the security standards of the App Defense Alliance, and uses the latest Wireguard protocol for fast speeds and secure connections. It also blocks trackers and ads device-wide, insecure HTTP connections, malware, and phishing domains. Stay protected with Malloc!


## Apple and Google Roll Out End-to-End Encrypted RCS Messaging, But Your iPhone-to-Android Texts Still Aren’t Truly Secure by Default


The release of **iOS 26.5** marks a pivotal shift in **mobile security** by introducing **default end-to-end encryption** for **RCS messaging** between **iPhone and Android** devices, a result of **cross-industry collaboration** between **Apple, Google, and the GSMA** . This integration aims to replace the aging **SMS** standard with a more secure, feature-rich protocol that includes **high-resolution media sharing** , **typing indicators** , and **read receipts** , all while protecting content from unauthorized interception through a verified **lock icon** indicator. While this advancement significantly strengthens **cross-platform privacy** , it is not yet a universal “default” in practice, as its functionality relies heavily on **carrier support** and the use of **updated messaging clients** on both ends; without these conditions, conversations may still fall back to **unencrypted formats** . Beyond these communication upgrades, the software update is a critical **security patch** that addresses over **50 vulnerabilities** , protecting users from **data leaks** , **denial-of-service attacks** , and potential **remote exploitation** across various system components.


## Get Malloc’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Source: 1.[The Hacker News](https://thehackernews.com/2026/05/ios-265-brings-default-end-to-end.html) 2.[Forbes](https://www.forbes.com/sites/zakdoffman/2026/05/13/no-your-iphone-and-android-texts-are-not-secure-by-default/)


**How Malloc can help? 👉** Malloc VPN, which is fast and secure, is also a great supplement to existing end-to-end encryption (E2EE) in secure messaging apps and RCS messaging. It hides a user’s IP address, which is unique and accessible to hackers, ISPs, and governments, and encrypts internet traffic, offering an additional layer of protection.


## 1.2M messaging app profiles leaked online


A major security breach involving a popular messaging platform has exposed approximately **1.2 million user profiles** , revealing sensitive details such as **phone numbers, display names, and profile avatars** . While the actual **chat contents remained encrypted** using OpenSSL, the leak of an unsecured **MongoDB database** highlights a critical vulnerability where **unprotected metadata** can be used by threat actors for **phishing, spam campaigns, and detailed user profiling** . This incident underscores that **end-to-end encryption** is insufficient for total privacy if **infrastructure security** is neglected, as the exposure of **device tokens and activity indicators** provides enough information for malicious actors to undermine user trust and facilitate **targeted cyberattacks** .


Source:[Cyber News](https://cybernews.com/security/tokee-messaging-app-data-leak/)


**How Malloc can help? 👉** Malloc helps secure user privacy and security through its powerful privacy preserving features features. It has an on-device VPN that blocks trackers, malicious and phishing domains, HTTP requests, and hides IP and encrypts traffic. Its Malicious App scan and security scan check for malware and permission misuse by apps. This helps keep users secure in case of a social engineering attack using breached data and helps prevent excessive data collection.


## AI-Powered Scams Are Getting Deadlier: Over Half of Americans Targeted Monthly as Fraudsters Weaponize Artificial Intelligence for Bigger Payouts


The **scam economy** has entered a dangerous new phase where **generative AI** acts as a primary catalyst for **highly efficient fraud** , drastically reducing the time required for criminals to launch sophisticated attacks from hours to mere minutes. Currently, **over half of consumers** encounter scam attempts at least monthly, with the rate of **monetary loss** doubling as attackers pivot toward high-value targets like **fake invoice schemes** , **investment fraud** , and **banking scams** . While **younger demographics** face a higher frequency of attempts due to intense digital activity, **older generations** remain the most vulnerable to significant financial devastation once targeted. This rapid evolution is fueled by the use of **synthetic voices** , **deepfakes** , and the manipulation of **AI chatbots** to disseminate fraudulent contact information, creating a digital environment where even the most confident individuals struggle to **distinguish reality from deception** . As a result, there is an overwhelming public demand for **telecom providers** and digital services to move beyond traditional security toward building **true resilience** and integrated protections within the consumer experience.


Source: 1.[TechRadar](https://www.techradar.com/pro/security/scams-are-getting-so-much-more-efficient-new-study-warns-over-half-of-americans-hit-by-fraud-in-2025-and-the-figure-is-only-going-to-get-worse) 2.[Help Net Security](https://www.helpnetsecurity.com/2026/05/11/f-secure-ai-scam-risks-report/)


**How Malloc can help? 👉** With the rise of organized cybercrime, sophisticated scams, and personalized fraud industries in various parts of the globe leveraging social engineering and leaked or publicly available user data to defraud users, Malloc emerges as a powerful personal cybersecurity and anti-fraud solution. Malloc has powerful and reliable features, such as device-wide tracker blocking and a fast, secure VPN, not only to help prevent user data leaks to data brokers but also to leverage its multiple layers of defense — including a real-time firewall and antivirus on users’ devices, along with many additional features — to protect them from all threats.


## Your Most Private Conversations Are Being Tracked: Mental Health Apps and AI Chatbots Quietly Expose Sensitive User Data to Third-Party Advertisers


Recent research indicates that **mental health and generative AI applications** frequently operate without the **privacy standards and confidentiality** users expect, often functioning on **data-driven business models** that prioritize data collection over security. These platforms often embed **undisclosed third-party trackers** from major tech firms to monitor **behavioral signals and usage patterns** , which can lead to the **re-identification of users** or the exposure of **sensitive emotional and professional data** . Furthermore, many services share **conversation permalinks and metadata** with external entities, utilize personal entries to **train large language models** , and request **excessive device permissions** — such as camera and microphone access — without clear justification or transparent disclosure. Because many of these digital tools fall outside **stringent regulatory protections like HIPAA** , the resulting datasets are frequently vulnerable to **data brokers** , making it difficult for individuals to **effectively delete their information** or maintain true **anonymity** in what they mistakenly perceive as a safe, private space.


Source: 1.[Help Net Security](https://www.helpnetsecurity.com/2026/05/08/android-mental-health-apps-privacy-risks/) 2.[Tech Explore](https://techxplore.com/news/2026-05-conversations-ai-private.html)


**How Malloc can help? 👉** Users must be careful when downloading and using AI and other apps that collect significant amounts of personal data. While app developers are responsible for following standard security practices, Malloc further secures personal data through its VPN by hiding IP locations, blocking trackers and HTTP traffic, and detecting permission misuse by apps that collect sensitive data, subsequently alerting users.


## Instagram removed end-to-end encryption for DMs


As of May 2026, **Instagram** has officially discontinued **end-to-end encryption (E2EE)** for direct messages, granting **Meta** the technical ability to access and monitor previously private conversations. This significant policy change is driven by a combination of **low user adoption** and the necessity to comply with the **Take It Down Act** , a federal law requiring platforms to identify and remove **non-consensual intimate images** and **AI-generated deepfakes** within a strict 48-hour window. To preserve their data, users are advised to **manually download message backups** and store them in **secure local storage** rather than unencrypted cloud services, where the data remains vulnerable. This transition underscores a growing global trend where **regulatory compliance** and **safety moderation** mandates are prioritized over **individual digital privacy** , fundamentally altering the landscape of secure communication on major social media platforms.


Source:[Security Affairs](https://securityaffairs.com/191941/security/instagram-removed-end-to-end-encryption-for-dms-what-should-users-do.html)


**How Malloc can help? 👉** Malloc VPN, which is fast and secure, is also a great supplement to existing end-to-end encryption (E2EE) in messaging apps and RCS messaging, and, to an extent, it helps protect users in the absence of E2EE. It hides a user’s IP address, which is unique and accessible to hackers, ISPs, and governments, and encrypts internet traffic, offering an additional layer of protection.


## On-body tech could expose users to new privacy and safety risks


The transition from traditional computing peripherals to **on-body interaction techniques** — such as **wearables, smart glasses, and smart textiles** — introduces unprecedented **privacy and safety risks** due to the continuous collection of **high-resolution biometric and behavioral data** . Because these technologies operate in close proximity to the human form, **privacy violations** can directly translate into physical and psychological harm, ranging from **emotional manipulation** and **unauthorized data interference** to extreme scenarios like **“ransomware for the body”** or hardware-induced injuries. To mitigate these threats, it is essential to adopt **security- and privacy-by-design guidelines** that prioritize **minimal data collection** , **transparency for users and bystanders** , and **robust ethical frameworks** to prevent individuals from losing **bodily control** or autonomy in an increasingly immersive digital environment.


Source:[Tech Explore](https://techxplore.com/news/2026-05-body-tech-expose-users-privacy.html)


**How Malloc can help? 👉** Stay tuned as we continue to add more powerful features to the Malloc App to protect our users against the evolving threat landscape, including the technologies that would power future privacy, security, and mass, as well as individual, surveillance threats. Stay protected with Malloc!
