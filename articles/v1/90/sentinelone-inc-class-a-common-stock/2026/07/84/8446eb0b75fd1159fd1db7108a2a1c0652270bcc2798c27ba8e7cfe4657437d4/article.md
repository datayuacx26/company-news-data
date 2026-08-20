---
schema_version: "1.0.0"
document_id: "8446eb0b75fd1159fd1db7108a2a1c0652270bcc2798c27ba8e7cfe4657437d4"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-28-8/"
published_at: "2026-07-10T16:27:02+00:00"
first_seen_at: "2026-07-20T23:17:13.754958+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:ba10da965fabd0cacd886f18cf17e180a7cd80d2bdbe251940e63357cf1706f5"
---

# The Good, the Bad and the Ugly in Cybersecurity – Week 28

## The Good | Authorities Apprehend Pro-Russian Hacktivist & Dismantle Global Fraud Networks


Spanish authorities, acting on intelligence provided by the FBI, have apprehended a suspected core member of the pro-Russian hacktivist syndicates CyberArmy of Russia Reborn (CARR) and Z-Pentest


.


While masquerading as ideologically motivated collectives, these groups have actively executed disruptive cyberattacks against[critical infrastructure](https://www.sentinelone.com/blog/securing-the-nations-critical-infrastructure-action-plans-to-defend-against-cyber-attacks/) , including food processing and water facilities across the United States and Europe. Investigators[allege](https://policia.es/_es/comunicacion_prensa_detalle.php?ID=16937) the arrested individual, residing in Palencia, provided extensive operational and logistical support to a Ukrainian hacker working for CARR, even attempting to facilitate their escape to Russia.


The individual is also suspected of coordinating cyber operations for the[NoName057(16)](https://www.sentinelone.com/labs/noname05716-the-pro-russian-hacktivist-group-targeting-nato/?utm_campaign=11908692948&utm_source=google-paid&utm_medium=paid-search&utm_content=568128260294&utm_term=undefined&gclid=undefined) group using encrypted messaging platforms. In a March 2026 raid, law enforcement officers seized multiple computers and successfully froze cryptocurrency wallets utilized to launder illicit proceeds generated from stolen data sales. The suspect currently faces ongoing criminal investigations for alleged collaboration with a recognized terrorist organization and severe computer damage.


In a massive global crackdown on[social engineering](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-social-engineering/) and financial fraud, international law enforcement agencies have


[arrested](https://www.interpol.int/News-and-Events/News/2026/Over-5-800-arrests-USD-293-million-intercepted-in-global-fraud-bust) 5,811 suspects and seized approximately $293 million in illicit assets


. Codenamed “Operation First Light 2026”, the coordinated initiative spanned 97 countries and specifically targeted[business email compromise](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/business-email-compromise-bec/) (BEC), investment scams, and money laundering syndicates operating between January and April. Interpol actively coordinated the extensive joint action, collaborating directly with regional policing bodies like ASEANAPOL, GCCPOL, and Europol to swiftly block over 31,000 fraudulent bank accounts and virtual wallets.


Eswatini police seized electronic devices, foreign currency, and realistic replicas of Brazilian police uniforms, signage, and equipment (Source: Interpol)


Investigators identified more than 142,000 victims worldwide and pinpointed an additional 15,600 suspects for future prosecution. This success builds upon recent international efforts, including[Operation Synergia II](https://www.interpol.int/en/News-and-Events/News/2024/INTERPOL-cyber-operation-takes-down-22-000-malicious-IP-addresses) , to dismantle the sprawling infrastructure supporting transnational cybercrime. Officials emphasize that robust, cross-border law enforcement cooperation remains essential to combat the escalating threat of organized cyber-enabled financial crimes globally.


## The Bad | Threat Actors Deploy Forg365 PhaaS to Hijack Microsoft Accounts


Cyber researchers have identified a new phishing-as-a-service (PhaaS) operation dubbed Forg365, which targets Microsoft 365 enterprise accounts


. Blending[adversary-in-the-middle](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-an-adversary-in-the-middle-aitm-attack/) (AiTM) techniques with device-code phishing, the platform provides an integrated dashboard to manage post-compromise activities.


Forg365 works by[incorporating](https://zerobec.com/blog/inside-forg365-telegram-distributed-sneaky2fa-style-phaas) AI to assist in generating customized[phishing](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/phishing-scams/) lures. By integrating AI directly into the control panel, Forg365 developers significantly lowered the financial cost needed to launch targeted campaigns. To remain undetected, its operators route messages through legitimate Amazon SES infrastructure while hosting their landing pages on Cloudflare.


The Forg365 panel (Source: ZeroBec)


The operation leverages the OAuth 2.0 device code authentication flow, originally designed for input-constrained devices. Attackers present victims with a deceptive verification page, tricking them into authorizing an attacker-controlled gadget rather than stealing their password directly


.


Once initial access is achieved, the platform ensures persistence through a specialized browser extension called ForgCookie. Compatible with Microsoft Edge, Google Chrome, and Brave, this extension operates silently to request account data, clear session cookies, and trigger a hidden OAuth flow to capture fresh tokens. Doing so grants attackers continuous access to the victim’s Microsoft services without requiring them to ever re-authenticate.


To protect its administration panel from being accessed by security defenders, Forg365 integrates robust anti-analysis features. The platform utilizes debugger traps, polymorphic code, and dynamic sandbox checks to evade detection, redirecting connections to innocuous websites whenever a VPN is detected.


Administrators can defend against these hijacking techniques by monitoring Microsoft Entra logs for unexpected device-code authentication events. Organizations can also restrict or entirely disable device-code flows unless absolutely necessary. In the event of a suspected compromise, security teams should revoke all OAuth grants and refresh session tokens to sever access.


## The Ugly | Rival Espionage Actors Breach & Spy On Pakistani Law Enforcement Networks


Between February 2024 and April 2026, suspected state-sponsored threat actors based in China and India separately converged on several Pakistani law enforcement organizations in unrelated cyberespionage campaigns


.


According to[SentinelLABS](https://www.sentinelone.com/labs/one-target-china-india-espionage-converge-on-pakistani-law-enforcement/) , operators heavily targeted the Balochistan Police, compromising critical network appliances and web servers. By infiltrating these critical systems, both nations actively sought independent visibility into Pakistan’s internal security posture and ongoing counter-militancy operations.


The China-nexus intrusions, leveraging[PlugX, ShadowPad](https://www.sentinelone.com/labs/moshen-dragons-triad-and-error-approach-abusing-security-software-to-sideload-plugx-and-shadowpad/?utm_campaign=11908692948&utm_source=google-paid&utm_medium=paid-search&utm_content=568128260294&utm_term=undefined&gclid=undefined) , and[Cobalt Strike](https://www.sentinelone.com/cybersecurity-101/threat-intelligence/what-is-cobalt-strike/) malware, were likely driven by Beijing’s concerns over the safety of Chinese nationals working on regional infrastructure projects within the China-Pakistan Economic Corridor (CPEC). Ongoing terrorist attacks have left the Chinese government dissatisfied with Pakistani protection and data from Balochistan Police would give the PRC direct insights.


Conversely, the India-nexus activity utilized[Remcos](https://www.sentinelone.com/blog/dbatloader-and-remcos-rat-sweep-eastern-europe/?utm_campaign=11908692948&utm_source=google-paid&utm_medium=paid-search&utm_content=568128260294&utm_term=undefined&gclid=undefined) backdoors to gather intelligence on the restive Balochistan province, a recurring flashpoint in the adversarial relationship between the two countries. Control over Balochistan Police networks means having invaluable visibility on how Pakistan manages their security posture as well as persistent access to civilian data.


Timeline of C2 traffic to Pakistani law enforcement organizations (Source: SentinelLABS)


One China-aligned threat actor specifically compromised the Balochistan Police Force’s Complaint Management System (CMS), a web application that actively serves both law enforcement personnel and Pakistani civilian users. The attackers uploaded custom malware implants disguised as routine portal updates, effectively weaponizing the digitalization of public policing services.


One payload masqueraded as a legitimate component of endpoint security software to evade initial detection and deploy an AsyncRAT client in order to establish persistent footholds into internal police networks while simultaneously surveilling citizens utilizing the platform.


This multi-actor convergence highlights how modernized policing infrastructure can serve as a high-value intelligence target for rival nations seeking comprehensive regional data


.
