---
schema_version: "1.0.0"
document_id: "129ea0cee970de4f11a6789d76794110addedb4ce7f1093b459b349877904afc"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-rss-b2c253ea0afd"
canonical_url: "https://www.gendigital.com/blog/insights/research/inside-the-jdownloader-supply-chain-attack"
published_at: "2026-05-11T15:11:33+00:00"
first_seen_at: "2026-07-20T03:32:39.128259+00:00"
fetched_at: "2026-07-28T22:15:07.431097+00:00"
content_hash: "sha256:5ab9574cb142394999e834216e4ebe806a51c983f829501b86c576c2a2bd9a49"
---

# Inside the JDownloader Supply-Chain Attack: An r77 Rootkit Bot That Kills Your Antivirus

By[Threat Research Team](https://www.gendigital.com/blog/authors/threat-research-team)


Attackers replaced selected official download links with malicious installers that deployed a Python bot, r77 rootkit components, and Windows policy-based defense evasion.


On May 6, 2026, attackers compromised the official JDownloader website and swapped download links to serve trojanized installers. The JDownloader developers[confirmed the breach](https://www.reddit.com/r/jdownloader/comments/1t6goqe/is_the_website_hacked/) within hours, restored clean files, and disclosed a timeline, but acknowledged they didn't know what the malicious installers actually do. We took the malware apart to answer that question.


What we found goes well beyond a typical trojanized installer. The payload is a multi-component attack framework: a Python bot protected by PyArmor v9, an r77 rootkit stager with AMSI bypass, and a Windows Defender Application Control (WDAC) policy that blocks 50 security executables from running, including Avast, AVG, Avira, Windows Defender, HitmanPro, and Kaspersky Virus Removal Tool. We reversed the full C2 protocol from the obfuscated Python bytecode and recovered the campaign configuration from the dropper's encrypted strings.


## The infection chain


The trojanized installer is unsigned. Windows SmartScreen blocks it, and users who ran it had to manually bypass the warning. Inside, it wraps the legitimate JDownloader installer alongside an XOR-encrypted second-stage PE:


Stage 2 acts as the campaign's quartermaster. It deploys five components from XOR-encrypted resources embedded in its PE (key:` fywo` ):


Component Purpose


**PyArmor bot** The main payload, a Python script with obfuscated bytecode


**PyArmor runtime** DLL that decrypts the bot's bytecode at execution time


**r77 rootkit stager** Native x86 PE that hides the bot's process, files, and registry keys


**WDAC Code Integrity policy** Blocks 50 AV/EDR executables from running after reboot


**Python 3.14.3 installer** The legitimate Python runtime (needed to run the bot)


Before doing any of this, Stage 2 calls` WaitForSingleObject` with a 300-second timeout — three times in sequence. This 5-minute-per-call delay is designed to outlast automated sandboxes, which typically give samples 2-5 minutes of execution time. One sandbox vendor recorded an 8-minute total wait before any malicious behavior appeared.


## Killing your antivirus before it can fight back


The most operationally significant component is the WDAC Code Integrity policy.


Windows Defender Application Control (WDAC) policies tell the operating system which executables are allowed to run. The attackers deploy a deny-list policy to` %WINDIR%\\System32\\CodeIntegrity\\SIPolicy.p7b` that blocks 50 executable paths across six security vendors plus core Windows security services:


Target Blocked executables


**Avast** AvastSvc.exe, AvastUI.exe, afwServ.exe, aswEngSrv.exe, aswidsagent.exe, aswToolsSvc.exe, wsc_proxy.exe, Icarus components, Overseer


**AVG** All executables under Program Files\\AVG\\Antivirus


**Avira** Security, VPN, Fallback Updater, Optimizer Host, Endpoint Protection SDK


**Windows Defender** MsMpEng.exe, MpCmdRun.exe, MpDefenderCoreService.exe


**Windows Security** SecurityHealthHost.exe, SecurityHealthService.exe, SecurityHealthSystray.exe


**Windows Update** MoUsoCoreWorker.exe, wuauclt.exe, wusa.exe


**Others** HitmanPro, Kaspersky Virus Removal Tool, Farbar Recovery Scan Tool


After the next reboot, none of these executables can start. The AV service is effectively dead. It won't run, it can't scan, and Windows Update can't push signature updates. One victim in the Reddit thread reported that Windows Defender disappeared entirely after running the trojanized installer.


This is a known but uncommon TTP (MITRE T1562.001). It requires the installer to run elevated, which the SmartScreen bypass dialog would have prompted for. Users who clicked through both warnings — SmartScreen and UAC — gave the malware everything it needed.


##


## How the dropper provisions the bot


Stage 2's strings are all XOR-obfuscated with the same` fywo` key used for its resources. After deobfuscation, we recovered 122 strings including registry paths, API names, and encrypted configuration values.


The dropper writes the bot's initial configuration to` HKCU\\SOFTWARE\\Python` . Both the registry value names and data are RC4-encrypted and hex-encoded — making the registry entries look like random hex strings to casual inspection. We recovered two configuration values:


- **CampaignID** a unique identifier for this campaign, used during bot registration with the C2 server.
- **DDRList** six dead-drop resolver URLs that the bot uses to discover its C2 servers (detailed in the next section).


Notably, the dropper does not write a C2 server list directly. The bot discovers its command-and-control addresses exclusively through the dead-drop resolver mechanism — a design choice that makes the infrastructure resilient to takedowns.


## Dead-drop resolvers: how the bot finds its C2


A dead-drop resolver (DDR) is an intermediary, a legitimate web page where the operator publishes encrypted C2 addresses. The bot fetches the page, extracts and decrypts the content, and connects to whatever C2 servers it finds. If the operator needs to rotate infrastructure, they update the DDR page and every infected machine follows automatically.


The bot reads its DDR list from the registry, fetches each URL, and RC4-decrypts the page content. Valid payloads start with the prefix` VAL:` followed by a semicolon-separated list of C2 URLs.


We extracted all six dead-drop resolver URLs from the dropper and probed them:


URL Status Decrypted content


` telegra\[.\]ph/tr02-05-02` Live C2 list: parkspringshotel\[.\]com + auraguest\[.\]lk


` telegra\[.\]ph/ki06-05-02` Live` VAL:XX` — placeholder for future C2 rotation


` rentry\[.\]co/zzk7opdu` Live Same C2 list


` rentry\[.\]org/wayc2qqz` Live` VAL:XX` — placeholder


` codeberg\[.\]org/etienne74/guix/...` 404 Removed or never created


\`2wxjoz6lkdxkniksujbdknkugweemx7uvxdr4ejgyipce4tnvktvbvqd.onion/t\` Not probed Tor hidden service DDR


The operator has two live C2 servers and four reserve dead-drop pages — two stubs ready for C2 rotation and one Tor fallback. If the current C2 infrastructure gets burned, the operator updates a Telegraph or Rentry page and every infected machine picks up the new address on its next poll cycle.


Beyond the dead-drop mechanism, the bot has a domain generation algorithm (DGA) as a last-resort fallback. It combines four seeds (` XNerWs4I` ,` e55gegnW` ,` RjWN0Y9m` ,` AemOU1co` ) with 14 TLDs and an ISO-week-based rotation to produce 56 candidate domains per week. We probed all current DGA domains — none are registered. The operator is running purely on dead-drop infrastructure for now, but the DGA ensures the botnet can survive even if all resolver pages and C2 servers are taken down simultaneously.


## The C2 protocol: RSA-OAEP wrapping AES-GCM


We reversed the bot's full communication protocol from the PyArmor-protected bytecode. It uses a layered encryption scheme designed to prevent passive interception and session tampering:


The bot wraps a fresh AES-256 key with the hardcoded RSA-2048 public key (OAEP, SHA-256) and sends it to the C2. The server proves possession of the private key by returning` "ok"` encrypted with AES-GCM. From that point, every message in both directions is AES-256-GCM encrypted with a per-direction sequence counter as authenticated associated data — preventing replay and reordering.


After the handshake, the bot registers with the campaign ID provisioned by the dropper. The server returns a` bot_id` and` bot_pass` that the bot stores in the registry for subsequent polling. The bot's primary capability is arbitrary Python code execution — the attacker can ship any Python code and the bot will run it and return the output.


All identified C2 and staging servers are compromised[QloApps](https://qloapps.com/) hotel reservation websites:


Domain Role IP PHP Location


` parkspringshotel\[.\]com` C2 panel 172.96.172\[.\]91 8.3.30 Ghana


` auraguest\[.\]lk` C2 panel 209.133.215\[.\]178 8.3.30 Sri Lanka


` checkinnhotels\[.\]com` Linux payload staging 66.29.137\[.\]25 7.4.33 —


The two C2 servers share identical JARM fingerprints and run the same PHP version, confirming the same C2 panel software is deployed on both. The third domain,` checkinnhotels\[.\]com` , was used to host the Linux payload disguised as an SVG file ([BleepingComputer](https://www.bleepingcomputer.com/news/security/jdownloader-site-hacked-to-replace-installers-with-python-rat-malware/) ). All three run QloApps — an open-source hotel booking CMS — suggesting the attacker specifically targets this software to build their infrastructure. None of the domains had malicious detections on VirusTotal at the time of discovery. We confirmed both C2 servers remain operationally live as of May 11.


##


## The r77 rootkit stager


The rootkit stager is a 176 KB native x86 PE based on the open-source[r77 rootkit](https://github.com/bytecode77/r77-rootkit) . It contains an embedded .NET assembly that performs four operations:


1. **AMSI bypass** — a PowerShell cradle patches` AmsiScanBuffer` in memory, disabling Windows' Antimalware Scan Interface before any script payload runs.
2. **Reflective DLL injection** — injects the r77 hooking DLLs (32-bit and 64-bit variants) into` winlogon.exe` . These DLLs hook Windows API functions to hide any process, file, or registry key whose name starts with the` $77` prefix.
3. **Registry persistence** — stores the stager assembly as a binary blob in` HKLM\\SOFTWARE\\$77stager` and reloads it at every boot via .NET reflection.
4. **Service creation** — installs a Windows service (` $77svc` ) to ensure the rootkit survives reboots independently of the registry mechanism.


Once r77 is active, the` $77` prefix acts as an invisibility cloak. The Python bot calls` r77_hide_self()` and` r77_hide_registry_path()` to rename its own process and registry hive with this prefix, making them invisible to Task Manager,` regedit` , and most security tools.


##


## The Python bot under PyArmor v9


The final payload is a Python script protected by[PyArmor](https://pyarmor.dashingsoft.com/) v9, a commercial bytecode obfuscation tool. PyArmor encrypts each function's bytecode individually and decrypts it at runtime through a native DLL — making static analysis significantly harder than standard Python malware.


We built a custom tool to decrypt and disassemble the protected bytecode, recovering 30 functions totaling 7,326 lines of disassembly. The bot is cross-platform (Windows and Linux), uses gevent for asynchronous I/O, and handles the full lifecycle: configuration management, C2 discovery via dead-drop resolvers and DGA, encrypted communication, task execution, and rootkit integration.


The bot's primary capability is executing arbitrary Python code received from the C2 server. Combined with the Python 3.14 runtime installed by the dropper, this gives the operator the ability to deploy any follow-on payload — keyloggers, credential stealers, lateral movement tools — without needing to push new binaries to the victim.


##


## Impact and recommendations


Users who downloaded JDownloader from the official website between May 6 and May 7, 2026, using the "Download Alternative Installer" links (Windows) or the Linux shell installer link are potentially compromised. The[official incident report](https://jdownloader.org/incident_8.5.2026.html) lists eight malicious installer hashes across x86/amd64 and multiple JRE versions. The attackers exploited an unpatched CMS vulnerability to swap download links — the actual installer files hosted externally were never modified. In-app updates, macOS downloads, and package manager installs (Flatpak, Winget, Snap) were not affected.


The Linux attack chain, \[first reported by BleepingComputer\](https://www.bleepingcomputer.com/news/security/jdownloader-site-hacked-to-replace-installers-with-python-rat-malware/), follows a different delivery path. We downloaded the trojanized shell installer and its payload to verify the claims and dig deeper.d. The modified install4j script requires root, checks for x86_64 architecture, and downloads a 20 MB tar.gz from` checkinnhotels\[.\]com/img/logo1.svg` (disguised with` Content-Type: image/svg+xml` — still live as of May 11). The archive contains two files: a 3 KB SUID-root helper (` systemd-exec` ) and a 21 MB PyInstaller bundle (` pkg` ). The helper is a minimal ELF that calls` setuid(0)` ,` setgid(0)` , and` execvp()` — installed to` /usr/bin/systemd-exec` with` chmod 4755` . Persistence is created via` /etc/profile.d/systemd.sh` , which launches the payload masquerading as` /usr/libexec/upowerd` .


We decrypted the PyArmor-protected Python bytecode from the Linux payload and confirmed it is **the same bot source code** as the Windows variant — identical RSA public key, DGA seeds, RC4 keys, C2 protocol, and DDR handler logic. The same campaign ID (` NdcHsJPM` ) and all six DDR URLs are written to` ~/.local/share/` as RC4-encrypted files (mirroring the Windows registry approach). The only difference is the Python runtime: the Linux bundle uses Python 3.12 (packaged via PyInstaller) versus Python 3.14 on Windows.


The JDownloader developers restored clean downloads on May 9 and published a detailed incident timeline. Users who already had JDownloader installed and received updates through the application's built-in updater were not affected — only fresh downloads from the website during the compromise window were trojanized.


**If you ran the trojanized installer:**


- The WDAC policy may have disabled your antivirus. Check` %WINDIR%\\System32\\CodeIntegrity\\SIPolicy.p7b` — if it exists and you didn't create it, delete it and reboot.
- The registry keys` HKCU\\SOFTWARE\\Python` (bot config) and` HKLM\\SOFTWARE\\$77stager` (rootkit) are strong indicators of compromise — but if r77 is active, its API hooks hide all` $77` -prefixed registry keys and the bot's own hive from standard tools like` regedit` . To inspect these, boot from a Windows recovery environment or mount the registry hives offline.
- A clean OS reinstall is the safest remediation. The r77 rootkit's reflective injection into` winlogon.exe` and AMSI bypass make partial cleanup unreliable.


**For defenders:**


- The bot uses the default Python User-Agent` Python-urllib/3.14` . While not unique on its own, it can help correlate connections to the C2 URLs and DDR pages listed below.
- Monitor for DNS queries to the DGA pattern: 15-character lowercase alphanumeric hostnames on TLDs including` .top` ,` .cfd` ,` .sbs` ,` .cyou` ,` .icu` ,` .casa` .
- The mutex name` Global\\0C3C1D37` can serve as a host-based indicator.
- Block the dead-drop resolver pages:` telegra\[.\]ph/tr02-05-02` ,` telegra\[.\]ph/ki06-05-02` ,` rentry\[.\]co/zzk7opdu` ,` rentry\[.\]org/wayc2qqz` .


##


## Indicators of Compromise


Full IOC list available on[GitHub](https://github.com/avast/ioc) .


### Hashes (SHA-256)


Artifact Hash


Stage 1 — Windows (analyzed variant)` 5a6636ce490789d7f26aaa86e50bd65c7330f8e6a7c32418740c1d009fb12ef3`


Stage 1 — Linux shell installer` 6d975c05ef7a164707fa359284a31bfe0b1681fe0319819cb9e2c4eec2a1a8af`


Stage 2 (C++ dropper)` 77a60b5c443f011dc67ace877f5b2ad7773501f3d82481db7f4a5238cf895f80`


Stage 3 — Windows (PyArmor bot)` 5fdbee7aa7ba6a5026855a35a9fe075967341017d3cb932e736a12dd00ed590a`


PyArmor runtime DLL` 33318499489cdb82543c0bfea699b98f5928c7a360966df6e958a9cbc2eab3fe`


r77 rootkit stager` 5c887054cb1dce077943afa955db43306f66795f7cbda8233d8ba25230a23d41`


WDAC CI policy` 5ee86c177dc5bdba05e3bdc67b07115c66097f825fff257bf0d4a999bbb8a1ea`


Linux payload archive (logo1.svg)` 6550672cac21e036882921dd934ee06552dc74d3b0a9e1ddc26f952855e11371`


Linux SUID helper (systemd-exec)` 25744e90bfa44cbcbf1f3d3c3cb90dd79dd32a6e359df9d2660ff251d6d03b46`


Linux PyInstaller bot (pkg)` bf47585bd0b39f0731f044b37a95eb7e311ad31b23b50306a113a3aa777dbfab`


Six additional Windows installer variants are listed in the[official incident report](https://jdownloader.org/incident_8.5.2026.html) . Full hash list on[GitHub](https://github.com/avast/ioc/tree/master/JDownloaderRAT) .


### Network indicators


Type Value Context


C2 URL` parkspringshotel\[.\]com/m/Lu6aeloo.php` Active C2


C2 URL` auraguest\[.\]lk/m/douV2quu.php` Active C2


Staging` checkinnhotels\[.\]com/img/logo1.svg` Linux payload (still live)


C2 IP` 172.96.172\[.\]91` parkspringshotel.com


C2 IP` 209.133.215\[.\]178` auraguest\[.\]lk


Staging IP` 66.29.137\[.\]25` checkinnhotels.com


Dead-drop resolver` telegra\[.\]ph/tr02-05-02` Live, serves encrypted C2 list


Dead-drop resolver` rentry\[.\]co/zzk7opdu` Live, serves encrypted C2 list


Dead-drop resolver` telegra\[.\]ph/ki06-05-02` Stub, reserved for C2 rotation


Dead-drop resolver` rentry\[.\]org/wayc2qqz` Stub, reserved for C2 rotation


### Host indicators


Type Value


Mutex` Global\\0C3C1D37`


Registry (bot config)` HKCU\\SOFTWARE\\Python\\*` (hex-encoded RC4-encrypted value names)


Registry (r77)` HKLM\\SOFTWARE\\$77stager` ,` $77dll32` ,` $77dll64` ,` $77svc`


WDAC policy` %WINDIR%\\System32\\CodeIntegrity\\SIPolicy.p7b` (GUID` {31351756-3F24-4963-8380-4E7602335AAE}` )


Bot process` pythonw.exe` (Python 3.14, User-Agent` Python-urllib/3.14` )


Tor drop path` C:\\ProgramData\\tor\\tor\\tor.exe`


Linux SUID binary` /usr/bin/systemd-exec`


Linux payload` /root/.local/share/.pkg`


Linux persistence` /etc/profile.d/systemd.sh`


Linux masquerade` /usr/libexec/upowerd`


### MITRE ATT&CK


Technique ID


Supply Chain Compromise: Compromise Software Supply Chain T1195.002


Obfuscated Files or Information T1027


Impair Defenses: Disable or Modify Tools T1562.001


Process Injection: Dynamic-link Library Injection T1055.001


Web Service: Dead Drop Resolver T1102.001


Dynamic Resolution: Domain Generation Algorithms T1568.002


Encrypted Channel: Asymmetric Cryptography T1573.002


Rootkit T1014


Create or Modify System Process: Windows Service T1543.003


Modify Registry T1112


For customers using Norton, Avast, AVG, Avira or any other security product, the most important question is whether the malicious installer was executed. If you downloaded an affected installer but did not run it, delete the file, download a fresh installer from the official JDownloader site, and run a full scan with up-to-date protection. If you ran the installer, or cannot rule out that you ran it, treat the system as compromised. JDownloader recommends a clean operating system reinstall in this situation, and we agree that this is the safest baseline. The Windows payload includes rootkit behavior, persistence, encrypted configuration and policy-based defense evasion, so a normal scan may reduce risk but cannot guarantee that every persistence mechanism has been removed. Until the system has been rebuilt or inspected from a trusted recovery environment, avoid using it for sensitive activity. Do not log in to email, banking, work accounts, password managers or other important services from that machine. Change important passwords from another trusted device, and monitor accounts for unusual activity.


More on this topic


[Fake invoices are moving from inboxes to shopping apps – From Research](https://www.gendigital.com/blog/insights/research/fake-invoices-shopping-apps)


[Inside Vidar’s ABE Bypass: From Memory Scanning to APC Injections – From Research](https://www.gendigital.com/blog/insights/research/inside-vidar-abe-bypass)


[Your flight was cancelled. Is the refund message real? – From Research](https://www.gendigital.com/blog/insights/research/flight-cancelled-refund-message-scam)


[Threat Research Team](https://www.gendigital.com/blog/authors/threat-research-team)


Threat Research Team


A group of elite researchers who like to stay under the radar.


Follow us for more
