---
schema_version: "1.0.0"
document_id: "42e0987967557a5e4a0304ab58eefcac07222e002d4eaf628cf31e8bd985ce24"
company_key: "n-able-inc-common-stock"
company: "N-able Inc."
source_id: "n-able-inc-common-stock-rss-2157b28f25ac"
canonical_url: "https://www.n-able.com/blog/active-directory-security"
published_at: "2026-07-08T13:04:12+00:00"
first_seen_at: "2026-07-24T11:53:52.327614+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:b161aea46add259aba7b6f148d715d21a4b7c9d0f3e707fe019926e26e55d43e"
---

# Active Directory Security: 8 Proven Best Practices

Here’s the uncomfortable part most teams learn the hard way: an attacker who compromises one ordinary domain user account can ask for service tickets tied to every privileged account in the environment, crack the hashes offline on their own time, and walk into domain admin without ever guessing a password. The path is well-worn, and the tools to run it are free.


That’s what makes Active Directory security worth real attention. It’s the mix of preventative controls, detective monitoring, and recovery capabilities that makes those attack chains harder to finish and faster to contain when they start. Active Directory (AD) sits at the center of authentication and authorization, so when it falls, a lot falls with it. The good news is that the attack paths are known, which means layered defense actually works.


The rest of this piece walks through why AD security matters for cyber-resilience, the vulnerabilities attackers keep coming back to, eight hardening practices that answer them, and how platform coverage turns all of that into something your team can actually run every day.


## **Why Active Directory security is critical for your cyber-resilience**


AD compromise doesn’t stay contained. It cascades. One domain controller breach opens paths into SaaS applications, cloud storage, and federated services, usually through token theft, ADFS key extraction, or AD Connect abuse. That blast radius is the whole reason AD hardening belongs near the top of the priority list, and it starts with understanding what the other side is actually doing.


## **Common Active Directory security vulnerabilities**


Attackers tend to follow a predictable chain: initial access, credential harvesting, lateral movement, privilege escalation, and eventually domain dominance. Along the way, they lean on misconfigurations, weak service accounts, and legacy protocols. A handful of techniques show up again and again in real incidents:


- **Kerberoasting and AS-REP Roasting:** Any domain-authenticated user can ask for Kerberos service tickets tied to accounts with Service Principal Names (SPNs). Those tickets are encrypted with a key derived from the account’s password, and attackers crack the key offline at their leisure. AS-REP Roasting is the nastier cousin, needing zero credentials when accounts have Kerberos pre-authentication disabled.
- **DCSync:** Abuses AD’s Directory Replication Service Remote Protocol to pull every password hash from a domain controller. Attackers get the Replicating Directory Changes rights they need through permission misconfigurations or existing Tier 0 access.
- **Pass-the-Hash and Pass-the-Ticket:** Here, attackers authenticate with stolen NT LAN Manager (NTLM) hashes or Kerberos tickets instead of plaintext passwords. Strong passwords don’t help, because the hash itself is the credential.
- **Golden Ticket attacks:** Once the KRBTGT account’s password hash is in hand, usually via DCSync, attackers forge Kerberos tickets valid for any account in the domain with whatever lifetime they choose. Cleaning up means resetting KRBTGT twice with forced replication, which is disruptive by design.
- **Group Policy Object (GPO) abuse:** Write access to GPOs lets attackers push malicious configurations to every computer in scope at once. Most teams over-permission GPOs early and rarely audit them afterward.


None of this is fringe. The joint Cybersecurity and Infrastructure Security Agency (CISA) advisory on[Akira ransomware](https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-109a) describes attackers dumping the AD database, password spraying against AD accounts, and enumerating users and groups through AD query tools. With that attacker playbook in mind, here’s where to focus the defense.


## **8 best practices for hardening Active Directory**


Hardening AD is layered work. Each of these eight practices answers a distinct attacker technique, and together they reduce credential exposure, limit privilege paths, tighten legacy settings, and make recovery possible when something slips through.


### **Enforce least-privilege access and tiering**


Every account, whether user, admin, or service, should hold only the permissions its job actually needs. At scale, that means separating privileges by how sensitive the asset is: Tier 0 for domain controllers and identity infrastructure, Tier 1 for application servers, Tier 2 for workstations.


Under that model, domain admin credentials never touch Tier 1 or Tier 2, with Authentication Policies and Silos (or User Rights Assignment via GPO) enforcing the separation.


Tier 0 admins belong to privileged groups like Domain Admins, Enterprise Admins, and Schema Admins. Those memberships need regular reviews, and just-in-time elevation should replace standing access wherever you can manage it.


One tier down, local admin accounts need their own control. Microsoft LAPS rotates unique passwords per machine and shuts down pass-the-hash pivots that otherwise flatten the whole tiering model. And the model only holds if the top tier is genuinely locked down.


### **Harden domain controllers as Tier 0 assets**


Domain controllers hold the highest-value credentials in the environment, so shrinking their attack surface is non-negotiable.


Start with what doesn’t belong on a DC: web browsers, general internet access, unnecessary services, and the Print Spooler service that opens the door to PrintNightmare and coerced-auth attacks. What stays gets tightened through GPO-applied security baselines, application allowlists, and just-in-time (JIT) access for privileged groups.


Passwords matter too, with real strength: 14+ characters, complexity, 24-password history, no reversible encryption, and fine-grained policies for privileged accounts. Operators should work from Privileged Access Workstations (PAWs), never from the same machine used for email or browsing. Even with DCs locked down this tightly, the accounts that touch them still need a second layer of identity proof.


### **Enforce MFA for all privileged accounts**


Multi-factor authentication (MFA) protects an account even when the password is already gone. Every human account with privileged access in AD needs MFA, with no carve-outs.


For Tier 0 admins specifically, phishing-resistant MFA (FIDO2 keys or smart cards) is the real bar. TOTP and push can be phished through adversary-in-the-middle kits, which are widely available and surprisingly effective.


That approach anchors a broader[identity threat response](https://www.n-able.com/blog/itdr-why-its-essential-and-how-to-protect-your-organization) plan. Service accounts sit outside this rule, since they run as automated processes and need a different kind of control.


### **Manage service accounts with dedicated controls**


Service accounts are prime targets: elevated privileges, rarely rotated passwords, and automated usage that masks anything unauthorized.


The first step is inventorying every service, scheduled task, and batch job against its account and permissions. Nothing gets protected that isn’t known. From there, static passwords can be eliminated by converting accounts to[Group Managed Service Accounts](https://www.n-able.com/features/all-in-one-software-for-msps) (gMSAs) that rotate automatically.


For accounts that have to stay on passwords, honeypot SPNs earn their keep by triggering[SIEM](https://www.n-able.com/cyber-encyclopedia/what-is-security-information-and-event-management-siem) alerts on decoy ticket requests, so Kerberoasting attempts get caught early. Identity hardening only goes so far, though, while legacy protocols keep handing attackers easier paths to the same credentials.


### **Disable legacy protocols**


NTLM, Link-Local Multicast Name Resolution (LLMNR), NetBIOS Name Service, and SMBv1 are legacy protocols attackers exploit for credential interception and relay attacks. Each has a retirement path worth running:


- Audit and disable NTLM at the domain level via Group Policy
- Enable Lightweight Directory Access Protocol (LDAP) signing and channel binding
- Disable LLMNR via GPO
- Disable NetBIOS over TCP/IP
- Turn off SMBv1


Endpoints carrying these protocols also need the discipline covered in our[patch management](https://www.n-able.com/solutions/patch-management) guide and[endpoint security](https://www.n-able.com/solutions/endpoint-security) approach, since stale software reopens the same exposure. Even with that hardening in place, detection still depends on visibility into live network activity.


### **Configure audit policy and monitor for attack indicators**


Without auditing, attackers operate undetected for weeks. Advanced Audit Policy subcategories replace legacy categories for granular control over what actually gets logged.


A couple of events matter more than most. Event ID 4768 with pre-authentication type 0 flags AS-REP Roasting. Event ID 4662 for DS-Replication-Get-Changes on non-DC sources signals DCSync. Without monitoring on those events, credential theft happens silently, which is exactly where[continuous threat monitoring](https://www.n-able.com/products/threat-hunting) closes the gap. One area that rarely gets the same discipline is certificate infrastructure, which attackers have turned into a reliable privilege escalation path.


### **Harden Active Directory Certificate Services**


Active Directory Certificate Services (ADCS) misconfigurations open a direct AD attack path, and most of the exploitable ones live on certificate templates.


Priority actions are tight in scope: review template permissions, restrict enrollment rights, and disable the CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT flag that enables ESC1.


Template hardening addresses enrollment abuse, but the ADCS servers themselves still need protection. Disable NTLM on those servers, enable Extended Protection for Authentication (EPA) against NTLM relay on HTTP enrollment endpoints, and treat ADCS servers as Tier 0 assets alongside the DCs. The[joint AD hardening guidance](https://www.cyber.gov.au/resources-business-and-government/maintaining-devices-and-systems/system-hardening-and-administration/system-hardening/detecting-and-mitigating-active-directory-compromises) from CISA and allied agencies covers these techniques in depth. Even with every preventive control in place, the last question is whether the environment can actually come back after a successful attack.


### **Plan AD backup and recovery before an incident**


AD backup and recovery only works when it’s planned before the incident, not after. Immutable, tested backups of server infrastructure, including system-state backups of DCs that support authoritative restore, need verification on a regular schedule. That verification is the part that matters most when ransomware encrypts AD, because recovery speed decides whether teams face a manageable incident or an extended outage with real cost and downtime impact.


Documented[backup recovery](https://www.n-able.com/products/cove-data-protection) and ransomware recovery processes turn that planning into routine that holds under pressure. Pulling all eight of these practices together takes a platform that covers hardening, detection, and recovery without handoffs between disconnected tools.


## **How N‑able strengthens your Active Directory security**


N‑able solutions cover the full attack lifecycle with products matched to each phase: exposure reduction before an attack, visibility during one, and recovery when prevention fails.


### **Before an attack**


[N‑able N‑central](https://www.n-able.com/products/n-central-rmm) handles patching and endpoint hardening that reduces the exposure attackers rely on to reach domain controllers. From one console, missing patches on AD-joined endpoints, outdated third-party software, and Common Vulnerability Scoring System (CVSS) findings get surfaced, prioritized, and queued for deployment.


Patching closes known weaknesses, but unknown endpoint behavior still needs watching. That’s where[N‑able EDR](https://www.n-able.com/products/endpoint-detection-and-response) , powered by SentinelOne, looks for credential-dumping, lateral movement, and privilege escalation, containing a host autonomously before an analyst even picks up the alert. Ahead of that sits the network layer, where[N‑able DNS Filtering](https://www.n-able.com/products/dns-filtering) blocks command-and-control traffic and phishing domains attackers use to establish a foothold. Prevention reduces the attack surface, but motivated attackers still find ways through, which is where detection and response take over.


### **During an attack**


[Adlumin MDR/XDR](https://www.n-able.com/products/adlumin/mdr) catches the behaviors that define AD-targeting attacks: a user requesting dozens of service tickets, a workstation initiating replication traffic, or an account authenticating from an unexpected geography. Those signals correlate across endpoints, identity providers, and network sources, feeding playbooks that cut off compromised accounts or quarantine hosts mid-attack.


Automation handles first response, and a 24/7 Security Operations Center (SOC) then validates each incident, contains it, and closes the path against recurrence. That platform underpins N‑able’s[managed detection](https://www.n-able.com/products/adlumin) and[identity protection](https://www.n-able.com/solutions/identity-protection) coverage. When detection and response don’t stop the damage in time, recovery determines how costly the incident actually becomes.


### **After an attack**


[Cove Data Protection](https://www.n-able.com/products/cove-data-protection) shortens domain controller recovery from days to hours. Those recovery points stay protected because Cove’s backups sit in cloud storage isolated from production and locked against modification, so ransomware that encrypted production can’t touch them.


From that protected backup, Cove supports system-state recovery that enables authoritative and non-authoritative DC restores, plus file-level and full-system recovery for servers and workstations, with automated boot verification along the way. Coverage across all three phases puts the right controls in place, and tuning them against ongoing change completes the posture.


## **Build an Active Directory security posture that survives compromise**


Strong AD security comes from ongoing practice that evolves with the environment. These eight practices give you a foundation, and that foundation only holds as accounts change, permissions drift, and attacker techniques evolve. Maintaining the posture means auditing regularly, patching aggressively, monitoring for indicators, and keeping recovery tested.


To strengthen AD security across your environment,[contact us](https://www.n-able.com/?page_id=29988) to see how the platform fits your operations.


## **Frequently Asked Questions**


These questions come up often when teams evaluate how AD gets attacked and where hardening has the biggest impact.


### **What makes Active Directory such a high-value target for attackers?**


AD controls authentication and authorization for the entire environment, so compromising it gives attackers access to every joined system, application, and cloud resource connected through federation. A single domain controller breach can cascade into full domain dominance and ransomware deployment across the network.


### **How does Kerberoasting work, and why is it difficult to detect?**


Kerberoasting lets any authenticated domain user request service tickets for accounts with Service Principal Names set, where the ticket is encrypted with a key derived from that account’s password, then crack the key offline using brute-force tools. The offline cracking phase generates zero network noise and bypasses account lockout policies entirely.


### **Why is disabling NTLM important for AD security?**


NTLM is a legacy authentication protocol vulnerable to pass-the-hash and relay attacks that let attackers authenticate without ever knowing the plaintext password. The migration path centers on Kerberos-first authentication and related hardening steps such as LDAP signing.


### **How often does the KRBTGT password need to be reset?**


Resetting the KRBTGT password invalidates forged Golden Tickets, and the reset needs to happen twice with forced replication between resets to fully take effect. Rotation cadence varies by environment, risk profile, and change management capacity.


### **Can on-premises AD compromise affect cloud environments?**


Yes, attackers who compromise on-premises AD can pivot into cloud infrastructure through AD Connect synchronization or by forging authentication tokens via ADFS servers. The hardening steps for hybrid environments reflect that same escalation path from on-premises identity systems into cloud access.


©


N‑able Solutions ULC and N‑able Technologies Ltd. All rights reserved.


This document is provided for informational purposes only and should not be relied upon as legal advice. N‑able makes no warranty, express or implied, or assumes any legal liability or responsibility for the accuracy, completeness, or usefulness of any information contained herein.


The N-ABLE, N-CENTRAL, and other N‑able trademarks and logos are the exclusive property of N‑able Solutions ULC and N‑able Technologies Ltd. and may be common law marks, are registered, or are pending registration with the U.S. Patent and Trademark Office and with other countries. All other trademarks mentioned herein are used for identification purposes only and are trademarks (and may be registered trademarks) of their respective companies.
