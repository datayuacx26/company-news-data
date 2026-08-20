---
schema_version: "1.0.0"
document_id: "50f39849a80dc7469b759ea0f1732b51ff906cb8379d1115fce65973d8e03ca0"
company_key: "yc-feroot-security"
company: "Feroot Security"
source_id: "yc-feroot-security-news-import-5cc226b40d32"
canonical_url: "https://feroot.com/blog/appsflyers-javascript-sdk-has-been-compromised/"
published_at: "2026-03-11T23:52:02+00:00"
first_seen_at: "2026-07-22T07:46:19.024825+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:1b3ed8def8476b28e55f1b2edc34fcf50a4fa3982c8f602aa55827c6e7591ff7"
---

# AppsFlyer's JavaScript SDK Has Been Compromised - Feroot Security

## **Update (July 17, 2026): The North Korea Connection**


When we published this alert in March, the attacker was unknown. Four months later, attribution for this specific incident has still not been publicly confirmed. AppsFlyer described the root cause as a domain registrar incident, and no government agency or research team has officially named the group behind it.


What has become clear is whose playbook this is.


The code injected into the AppsFlyer SDK watched pages inside the browser for cryptocurrency wallet addresses and silently swapped them with attacker-controlled ones. That is the signature technique of North Korean state-sponsored operations:


- In February 2025, the FBI attributed the $1.5 billion Bybit theft, the largest cryptocurrency heist in history, to North Korea’s TraderTraitor group. That attack worked the same way this one did: malicious JavaScript injected into trusted web infrastructure, altering what happened inside the browser while everything looked normal to the user.
- North Korean groups have industrialized JavaScript supply chain attacks, publishing hundreds of malicious npm packages aimed at developers and cryptocurrency users to gain a foothold in trusted code.
- Stolen cryptocurrency is a state funding line. North Korea-linked hackers stole over $1 billion in crypto assets in 2023 alone, and the proceeds fund the regime’s weapons programs.
- The same week the AppsFlyer SDK was compromised, the US Treasury sanctioned a North Korean IT worker network that generated nearly $800 million in 2024 for the regime’s weapons programs.
- Three weeks after this attack, blockchain investigators at TRM Labs pointed to North Korea in the $285 million Drift Protocol drain.


Whether or not this incident is ever formally attributed, the takeaway does not change. The JavaScript running in your users’ browsers is now an active theater for nation-state operations, not just criminal fraud. Feroot’s research on foreign-controlled code executing in the browser has been used by the US Congress and Senate to understand exactly this class of supply chain risk. The AppsFlyer incident shows the same channel being used for direct theft at scale.


---


AppsFlyer’s JavaScript SDK has been compromised in an active supply chain attack. Websites loading the script are serving malicious code to their users without any changes to their own codebase.


## **What happened**


Attackers modified AppsFlyer’s SDK, the script websites load to track marketing attribution. Because the script is served from AppsFlyer’s infrastructure, any site that includes it automatically begins delivering the compromised version to visitors.


## **Who is affected**


Any website that loads the AppsFlyer JavaScript SDK. This includes e-commerce, fintech, healthcare, and SaaS platforms, many of which load the script across every page, including forms and checkout flows.


## **What the compromised script can access**


Scripts run inside the browser with direct access to the page. That includes form fields, keystrokes, authentication tokens, and any data a user submits before it reaches your server.


## **What to do now**


1. Confirm whether AppsFlyer loads on any of your pages: check all pages, not just marketing pages
2. Remove or block the script until AppsFlyer confirms a clean build
3. Review network traffic logs for the past 72 hours for unexpected outbound data calls
4. Notify your security and compliance teams if payment or health data was exposed, regulatory reporting obligations may apply


## **What this illustrates**


Third-party scripts run with the same access as your own code, but outside your change management and security review processes. A script that is clean today can be weaponized through a vendor update, a hijacked CDN, or a compromised build pipeline without any action on your part.


PCI DSS 4.0.1 Requirements 6.4.3 and 11.6.1 require continuous inventory and integrity monitoring of scripts on payment pages specifically because of this attack pattern.


*A deeper technical analysis of this attack, including indicators of compromise and script behavior patterns, will be published shortly.*


*Feroot Security monitors script behavior inside browsers in real time. If you want to know what scripts are running on your site and what they’re doing, you*[request a free scan here](https://www.feroot.com/demo/) .


### **Sources**


- [BleepingComputer: AppsFlyer Web SDK hijacked to spread crypto-stealing JavaScript code](https://www.bleepingcomputer.com/news/security/appsflyer-web-sdk-used-to-spread-crypto-stealer-javascript-code/)
- [ISMG BankInfoSecurity: Cryptohack Roundup covering the AppsFlyer breach and the OFAC sanctions on the North Korean IT worker network](https://www.bankinfosecurity.com/cryptohack-roundup-appsflyer-sdk-breach-enabled-theft-a-31060)
- [Rescana: AppsFlyer Web SDK supply chain attack analysis, attribution unconfirmed](https://www.rescana.com/post/appsflyer-web-sdk-supply-chain-attack-global-crypto-stealing-javascript-injection-and-mitigation-an/)
