---
schema_version: "1.0.0"
document_id: "16b6fc3a175f171e1c688189a7f755d39fa23f83f8451ffe05f025e1acf6623b"
company_key: "yc-malloc-inc"
company: "Malloc Inc"
source_id: "yc-malloc-inc-rss-36b506f8e239"
canonical_url: "https://blog.mallocprivacy.com/malloc-privacy-weekly-0130c7f5bf51"
published_at: "2026-07-09T04:31:36+00:00"
first_seen_at: "2026-07-24T10:30:15.411511+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:9e2858c998a871a848b35b4f5be9734b76901313b5f3b25888a264b18c525076"
---

# Malloc Privacy Weekly

# Malloc Privacy Weekly


[Malloc](https://medium.com/@Ayu_Malloc?source=post_page---byline--0130c7f5bf51---------------------------------------)


11 min read


·


Jul 9, 2026


--


**Welcome to this week’s edition of Malloc Privacy Weekly.** From repeated zero‑click Pegasus infections targeting a European Parliament investigator to newly uncovered flaws in AirDrop and Quick Share affecting billions of devices, this week’s headlines underline how quickly everyday platforms can become high‑impact surveillance and attack surfaces. Below, we break down what happened, why it matters for privacy and security, and the practical steps you can take to reduce your exposure.


**Former Greek investigative journalist and Member of the European Parliament Stelios Kouloglou** suffered repeated **zero-click Pegasus spyware infections** on his **iPhone** in **October 2022** while hospitalized for surgery and again in **March 2023** during **PEGA committee** travel and deliberations in Brussels. Serving as a **substitute member** on the European Parliament’s inquiry into **Pegasus and equivalent spyware abuses** by EU states including **Greece, Poland, Hungary, Cyprus** and **Spain** , Kouloglou’s device was compromised via a sophisticated **PWNYOURHOME exploit** abusing an unpatched **Apple HomeKit vulnerability** , potentially exposing **confidential committee documents** , private messages, location data and personal conversations to the same **NSO Group Pegasus operator** previously linked to hacks on European journalists. This brazen targeting of a committee investigator by the very surveillance tool under scrutiny has ignited fresh outrage, with Kouloglou vowing to **sue NSO Group** and civil society groups urging the **European Commission** to finally implement the **PEGA committee’s 2023 recommendations** for regulating commercial spyware and safeguarding democratic oversight across the bloc.


Security researchers from the **CISPA Helmholtz Center for Information Security** performed the first systematic reverse-engineering and protocol-aware fuzzing of the proprietary **Apple AirDrop** and **Google/Samsung Quick Share** proximity file-sharing protocols relied upon by more than **five billion active devices** across iOS, macOS, Android, and Windows. Their work identified **six vulnerabilities** arising from the protocols’ acceptance of unauthenticated nearby wireless connections and processing of complex serialized data in privileged components: three **denial-of-service flaws** in AirDrop, including a Swift fatalError crash in the sharingd daemon that disrupts multiple **Continuity features** such as AirPlay and Handoff, an unbounded recursion issue in Foundation’s XML property list parser with broad impact across Apple platforms and apps, and a NULL pointer dereference in Network.framework’s HTTP/1.1 parser; plus three Quick Share issues involving pre-authentication message dispatch before UKEY2 handshake completion on Samsung devices, encryption enforcement gaps allowing plaintext processing of several post-handshake frame types, and a **heap use-after-free** race condition in Google’s Windows client with plausible remote code execution potential under connection collision scenarios.


To learn more about these developments and other news, read the article below.


Press enter or click to view image in full size


Malloc Privacy Weekly


## European Parliament Investigator Stelios Kouloglou Targeted in “Zero-Click” Pegasus Spyware Attacks Amid Investigation into Surveillance Abuses


During his tenure on the **European Parliament’s PEGA committee** , which was actively investigating government abuses of surveillance tools, Greek journalist and former politician **Stelios Kouloglou** had his iPhone compromised by the **Pegasus spyware** in 2022 and 2023. Security researchers at **The Citizen Lab** confirmed that Kouloglou was targeted at least three times — once in October 2022 while hospitalized, and twice in March 2023 during committee travels — using a **“zero-click” security vulnerability** in Apple’s smart home software to covertly extract private correspondence, photos, and location data. This unprecedented breach marks the first time a member of the investigatory panel has been publicly identified as a victim, prompting lawmakers to label the intrusion a **direct attack on the rule of law** and demand strict limits on spyware from the **European Commission** . Although the specific state sponsor behind the hacks remains unidentified, the Pegasus operator utilized an email address linked to previous cyber campaigns against European journalists, leading Kouloglou to announce plans to **sue the Israeli-based NSO Group** to defend democratic integrity and human rights.


Source:[Tech Crunch](https://techcrunch.com/2026/07/02/politician-who-investigated-spyware-abuses-had-his-phone-hacked-with-pegasus-spyware/)


**How Malloc can help? 👉** Malloc keeps you safe and private online by detecting and blocking spyware, malware, and other malicious domains in real time. It does this using advanced spyware indicators, monitoring your app behavior and data transmission in real time. Stay connected to Malloc VPN at all times for maximum security and set the Android auto-scan feature to perform Malloc’s security scans daily.


## Scammers Exploit Brand Trust via Social Media Ads and Fake Google Play Pages to Deploy Deceptive Gambling Progressive Web Apps


A sophisticated, affiliate-driven cyber campaign is leveraging highly polished **social media ads** across platforms like Facebook, Instagram, Threads, and TikTok — incorporating AI-generated videos and forged user interfaces — to falsely announce “official” casino partnerships with trusted household names like **Tesco, Amazon, Monzo, and Revolut** . When users click these promotional ads, they are routed to highly convincing **counterfeit Google Play Store landing pages** hosted on innocuous-looking domains, which bypass automated security filters. Instead of downloading a vetted, native application from an official store, clicking “Install” triggers a browser prompt to add a **Progressive Web App (PWA)** to the user’s home screen. Once installed, these PWAs act as thin, branded wrappers that obscure the browser interface and load third-party **online gambling and slot websites** . Driven by lucrative affiliate programs offering **cost-per-acquisition payouts of $50 to $350** per depositing player, these malicious actors expose victims to immediate **financial losses** while eroding public trust in legitimate financial and retail institutions.


Source:[GB Hackers](https://gbhackers.com/fake-google-play-store-pages/)


**How Malloc can help? 👉** Malloc provides strong protection against advertisement fraud and advertisement bombing attacks by blocking third-party advertisement domains locally device-wide through its on-device VPN and local tracker blocklists. It further provides enhanced protection against fraud and malware through its anti-phishing protection and malicious app scans. Malloc VPN, via its IP address obfuscation and encrypted tunnel, also helps secure user privacy by limiting the user details sent to advertisers.


## Glitch SPY RAT Exploits Android Accessibility Services in Malicious Campaign


An emerging, highly modular Android remote-access trojan (RAT) platform known as **Glitch SPY** is being distributed through a **fraudulent Polish apartment-rental website** using the **Brokewell Android Loader** to trick users into sideloading a malicious application. Once installed, the malware coerces victims into enabling the **Android Accessibility Service** , granting the threat actors **near-complete device control** to perform hands-on financial fraud, execute complex gestures, and auto-approve permission prompts in real-time. This sophisticated malware family communicates with a command-and-control server to support over **70 remote actions** — including **live screen streaming** , location tracking, and camera capture — while utilizing a **crypto-clipper** to hijack digital wallet transactions and a **remote-browser capability (WebView)** to bypass detection during account takeovers. Powered by an exposed, customizable builder architecture featuring AES/GCM file encryption and secure-deletion routines, this campaign represents a highly adaptable **resalable platform** capable of targeting diverse global markets beyond its current European focus.


Source:[GB Hackers](https://gbhackers.com/glitch-spy-rat-abuses-android/)


**How Malloc can help? 👉** Malloc can detect the misuse of Android accessibility services exploited by Android malware and spyware, alerting users promptly to help them stay protected. Malloc’s device security and malicious app scan detects malicious apps and malware, alerting users. Malloc further ensures user security and privacy through its VPN, which blocks malicious domains used by malware locally.


## CISPA Researchers Uncover Critical Protocol Flaws in Apple AirDrop and Google/Samsung Quick Share Threatening Five Billion Devices with Wireless Attacks and Code Execution


Security researchers have successfully reverse-engineered the proprietary proximity file-sharing protocols of **Apple AirDrop** and **Google/Samsung Quick Share** , uncovering **six previously undocumented vulnerabilities** that collectively expose over **five billion active devices** to pre-authentication wireless exploits. By developing a custom protocol-aware fuzzing framework called **AIRFUZZ** , the team discovered **three major flaws in Apple’s ecosystem** — including a fatal error routing bug in the sharingd daemon, a recursion-limit weakness in the XML property list parser, and an HTTP parser null pointer dereference — which can trigger widespread **denial-of-service conditions** disabling essential Continuity features across macOS and iOS. Concurrently, they identified **three vulnerabilities in Quick Share** affecting Samsung and Windows devices, where missing authentication handshakes and bypassed encryption allow unauthenticated interaction, while a critical **heap use-after-free race condition** in Google’s Windows client could potentially be exploited for **remote code execution** . While **Apple, Google, and Samsung work to deploy fixes** , researchers advise implementing stronger error handling, enforcing strict recursion limits, and mandating central encryption to secure these widely deployed wireless sharing services.


Source:[Cyber Insider](https://cyberinsider.com/apple-airdrop-and-android-quick-share-flaws-expose-users-to-wireless-attacks/)


## Get Malloc’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**How Malloc can help? 👉** Malloc offers complete 360° protection against all kinds of mobile threats — it blocks spyware connections in real time, thwarts phishing attempts, blocks trackers and insecure HTTP websites, and detects permission misuse by apps. On Android, it also blocks microphone access, detects microphone and camera usage by rogue apps, stalkerware, and malware, and alerts users about such activity. Malloc VPN, via its IP address obfuscation and encrypted tunnel, helps secure user internet activity. Thus, it keeps you protected against the ever-increasing number of mobile security threats.


## Sophisticated Phishing Campaign Abuses CRM Platforms and Google OAuth Impersonation to Target Marketing Professionals with Fake Big-Brand Job Interviews


A highly coordinated **phishing campaign** is actively targeting **marketing professionals** by impersonating **over 30 well-known global brands** — including Adobe, Netflix, Coca-Cola, and OpenAI — with fake job opportunities. Discovered by cybersecurity researcher Will Thomas, the operation leverages the names and photos of **real corporate recruiters** to establish trust and lure victims. The threat actors abuse legitimate cloud-based HR and CRM platforms, specifically **PeopleForce, Salesforce Marketing Cloud, and Wise Agent** , utilizing a complex chain of **nested redirects** to bypass email security filters. When targets attempt to schedule an interview, they are routed to malicious landing pages hosting a deceptive **“browser-in-the-browser” (BitB) attack** . This technique uses advanced HTML and CSS to render a **highly convincing fake Google sign-in popup** that steals the victims’ **Google account credentials** directly, allowing the attackers to hijack their personal and professional profiles.


Source:[Bleeping Computer](https://www.bleepingcomputer.com/news/security/phishing-poses-as-big-brand-job-interview-to-steal-google-accounts/)


**How Malloc can help? 👉** Malloc has anti-phishing protection and blocks all kinds of malware and spyware domains locally using its on-device VPN. It also blocks trackers and insecure HTTP traffic device-wide. Stay connected to Malloc VPN at all times for maximum privacy. Malloc VPN, via its IP address obfuscation and encrypted tunnel, also helps secure user privacy by limiting the user details sent to advertisers.


## Cybercriminals Weaponize Legitimate Chinese DCloud Uni-App Framework to Orchestrate Massive Global Cryptocurrency Fraud, WhatsApp Phishing, and Fake Investment Schemes


Cybercriminals have extensively repurposed a legitimate, open-source Chinese cross-platform development toolkit called **DCloud Uni-App** to establish a massive, transnational scam infrastructure underpinning over **236,000 fraudulent second-level domains** . Following the exposure of the **2024 RainbowEx scandal** in Argentina, which defrauded thousands of victims through a fake cryptocurrency platform, researchers uncovered a highly organized, multilingual criminal network that continues to grow rapidly. This malicious ecosystem utilizes the development framework to build highly deceptive, fake investment portals impersonating major stock and cryptocurrency exchanges, run physical-world fraudulent schemes like passive-income scooter rentals, and deploy highly convincing **WhatsApp phishing templates** alongside active **crypto wallet drainers** to harvest credentials and steal digital assets.


Source:[Cyber Security News](https://cybersecuritynews.com/dcloud-uni-app-scam-network-powers-rainbowex-style-crypto-fraud/)


**How Malloc can help? 👉** Malloc blocks malicious URLs, including links to spyware, malware, and phishing domains, as well as crypto-mining sites and inappropriate content. It prevents malicious apps from connecting to command-and-control servers and blocks advertisement domains and trackers to protect against ad fraud across all apps and browsers. By utilizing a powerful on-device VPN, Malloc acts as a robust on-device firewall that monitors network traffic while preserving user privacy and safeguarding your digital identity whenever you connect to the internet.


## Google Strengthens Android 17 Security by Implementing Strict Lock Screen PIN Guessing Limits and Duplicate-Attempt Detection to Combat Brute-Force Attacks


Google is significantly upgrading device security in its upcoming **Android 17 update** by implementing **strict lock screen rate-limiting** to prevent attackers from brute-forcing user PINs. Moving away from older systems that allowed up to 1,800 attempts over five years, the new software strictly **limits users to 20 incorrect guesses** before completely blocking further attempts, which directly combats hackers who exploit predictable patterns or personal information like birthdates. To prevent penalizing legitimate users who make repetitive typos, the system introduces **duplicate-guess detection** so that identical consecutive incorrect entries do not increase the failure count. Additionally, the update improves user experience during lockouts by replacing confusing second-based countdowns with **clear minute-based lockout timers** and displaying a **recovery shortlink** to help users easily regain access to their devices using secondary hardware.


Source:[Cyber News](https://cybernews.com/security/android-pin-unlock-attempts-limit/)


**How Malloc can help? 👉** The Malloc app for Android, along with Android’s own default protection, is a great combination for users’ all-around privacy and security. Malloc’s numerous powerful security and privacy features, such as the device-wide tracker blocker, HTTP blocking, phishing and malicious domain protection, along with its Xray VLSS + Reality Protocol + Post Quantum Encryption powered VPN, device security scan, malicious app scan, download scan, and microphone and camera usage alerts and protection, work in conjunction with Android’s own protection features to safeguard users against all kinds of mobile threats, ranging from common scams to sophisticated state-sponsored spyware attacks.


## European Union’s Highest Court Upholds €4.125 Billion Antitrust Penalty Against Google and Parent Company Alphabet for Restricting Android Competitors to Protect Search Dominance


In a definitive legal resolution, the **Court of Justice of the European Union** has officially rejected the final appeal by **Google and its parent company, Alphabet** , solidifying a massive **€4.125 billion ($4.8 billion) antitrust fine** originally initiated by the **European Commission** . The ruling confirms that the tech giant engaged in a systematic **anticompetitive strategy** by abusing **Android’s dominant market position** to secure preferential placement for its own services. To achieve this, Google imposed strict **contractual restrictions on device manufacturers and mobile operators** , which included **mandatory pre-installation of Google Search and Chrome** to access the Google Play Store, alongside **anti-fragmentation agreements** that blocked competing Android forks and preserved high barriers to entry. Although Google defended its model by highlighting its massive investments in keeping the platform free and open, and noted it adapted its agreements years ago, the high court determined that these restrictive licensing practices successfully exploited **consumer status quo bias** and limited fair market competition.


Source:[Cyber Insider](https://cyberinsider.com/google-loses-final-appeal-against-e4-1-billion-android-antitrust-fine/#comments)


**How Malloc can help? 👉** As Google continues to monopolize the market and collect more and more user data while reducing privacy-friendly alternatives, users need to take steps to secure their devices. Malloc’s on-device system-wide VPN uses local block lists to block trackers and helps reduce first- and third-party data collection by blocking tracking domains. Malloc VPN also helps keep users private and secure by hiding their IP address and encrypting internet traffic in transit. Because Google cannot be trusted with user data and securing user privacy, trustworthy privacy tools like Malloc help keep users private.


## US Government Offers $10 Million Reward to Track Russian Intelligence Cyber Groups (UNC5792 & UNC4221) Targeting US Officials and Journalists via Malicious Signal and WhatsApp Phishing Campaigns


A highly active Russian state-sponsored cyber espionage operation has compromised thousands of Signal and WhatsApp accounts belonging to **high-value targets** , including **current and former US government officials, military leaders, political figures, and investigative journalists** . Conducted by two Russian intelligence-linked groups, **UNC5792** (associated with the FSB Border Guards) and **UNC4221** (working on behalf of Russian military services), the campaign utilizes **sophisticated phishing messages** masquerading as automated support services or altering legitimate group invite pages. These deceptive prompts trick victims into **linking attacker-controlled devices** to their accounts or **surrendering their secret backup recovery keys** , allowing hackers to bypass end-to-end encryption to access past and future communications. In response to this widespread threat, the **US State Department is offering a reward of up to $10 million** through its Rewards for Justice program for information leading to the identification or location of the cybercriminals, while federal authorities urge users to **generate new backup keys** if compromised and to never share verification codes or click urgent support links.


Source:[Ars Technica](https://arstechnica.com/information-technology/2026/06/us-offers-10-million-for-info-on-group-behind-signal-and-whatsapp-hacking-spree/)


**How Malloc can help? 👉** Malloc’s extensive features help reduce risks like phishing and spyware infections to devices that come with using messaging apps like Signal etc. Malloc VPN is a great supplement to existing end-to-end encryption (E2EE) in secure messaging apps and RCS messaging. It hides users’ IP addresses, which are unique and accessible to hackers, ISPs, and governments, and encrypts internet traffic, offering an additional layer of protection.
