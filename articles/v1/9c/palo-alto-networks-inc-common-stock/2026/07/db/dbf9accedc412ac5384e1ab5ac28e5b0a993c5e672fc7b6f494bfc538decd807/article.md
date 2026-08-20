---
schema_version: "1.0.0"
document_id: "dbf9accedc412ac5384e1ab5ac28e5b0a993c5e672fc7b6f494bfc538decd807"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/xcsset-v40-malware-analysis/"
published_at: "2026-07-31T10:00:18+00:00"
first_seen_at: "2026-07-31T17:05:09.488739+00:00"
fetched_at: "2026-07-31T17:05:10.966192+00:00"
content_hash: "sha256:181e4817a8dbb8fb9df8be7f2ac010e578e3577011a41744fca9fb15a80abef2"
---

# The Xcode Assassin Returns: A Deep Dive Into the Latest XCSSET Version

## Executive Summary


After months of dormancy, the attackers behind the XCSSET malware released version 40 (v40), targeting the macOS ecosystem. This version’s advanced architecture hides its core logic in memory space, reducing its digital footprint.


V40 further enhances its detection evasion capabilities by combining polymorphic payload generation with fileless persistence and dynamic in-memory execution, while weakening a number of security mechanisms on the affected machine.


Since early April 2026, the malware has spread through supply chain attacks by hiding itself in the Xcode projects of dozens of legitimate applications with thousands of active users. Xcode is Apple’s integrated development environment (IDE) for building apps for its various operating systems.


XCSSET’s author enhanced the threat’s ability to spread through open-source projects on GitHub and upgraded its worming capabilities. It can now infect all existing Xcode projects on a compromised system for maximum impact.


The author used a multi-layered cipher shift to conceal the threat’s internal functions. In response, our researchers leveraged advanced AI and pattern-matching algorithms to de-obfuscate the malware's logic.


This article:


- Explores XCSSET’s updated stealth practices
- Examines the new operational modules
- Reveals findings regarding the attackers' rotating command-and-control (C2) infrastructure
- Provides mitigation strategies to detect and prevent this threat


Palo Alto Networks customers are better protected from the threats discussed above through the following products and services:


- [Cortex XDR](https://www.paloaltonetworks.com/cortex/cortex-xdr?_gl=1*13pmp8e*_ga*NzQyNjM2NzkuMTY2NjY3OTczNw..*_ga_KS2MELEEFC*MTY2OTczNjA2MS4zMS4wLjE2Njk3MzYwNjEuNjAuMC4w) and[XSIAM](https://www.paloaltonetworks.com/resources/datasheets/cortex-xsiam-aag)
- [Advanced URL Filtering](https://docs.paloaltonetworks.com/advanced-url-filtering) and[Advanced DNS Security](https://docs.paloaltonetworks.com/dns-security)


If you think you might have been compromised or have an urgent matter, contact the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) .


**Related Unit 42 Topics** **[Supply Chain](https://unit42.paloaltonetworks.com/tag/supply-chain/) ,[Backdoor](https://unit42.paloaltonetworks.com/tag/backdoor/) ,[macOS](https://unit42.paloaltonetworks.com/tag/macos/)**


## Background


XCSSET is a modular macOS malware family that primarily targets software developers within the Apple ecosystem, spreading through[Xcode](https://developer.apple.com/xcode/) projects. Threats in this family download task-specific modules from a C2 server, giving it capabilities including:


- Browser hijacking
- Credential theft
- Clipboard monitoring
- Data exfiltration


XCSSET’s[initial discovery](https://www.trendmicro.com/en_us/research/20/h/xcsset-mac-malware--infects-xcode-projects--uses-0-days.html) was by Trend Micro in 2020. Security researchers at Microsoft analyzed and documented two subsequent versions in[March](https://www.microsoft.com/en-us/security/blog/2025/03/11/new-xcsset-malware-adds-new-obfuscation-persistence-techniques-to-infect-xcode-projects/) and[September](https://www.microsoft.com/en-us/security/blog/2025/09/25/xcsset-evolves-again-analyzing-the-latest-updates-to-xcssets-inventory/) 2025. These updates indicate that the attackers were enhancing their codebase.


In mid-April 2026, we started tracking a new version of XCSSET. We saw a secondary wave of attacks in early May 2026 that introduced an expanded suite of operational modules.


In this new version, we observed a heightened volume of attacks targeting developers across South Asia, which is consistent with Trend Micro's initial 2020 reporting,


While the threat actor has named this latest iteration XCSSET v40, the security community has historically identified only a handful of intermediary versions, none of which featured formal version labels.


## Infection Chain Analysis


In this section, we provide a high-level overview of XCSSET v40’s infection chain. The threat’s authors restructured its execution framework to be more stealthy and modular. We provide a complete step-by-step breakdown of each phase in Appendix A.


The malware injects an initial downloader script into benign project files in Xcode projects and vulnerable Git repositories. While the attack lifecycle begins with the infected codebase, the endpoint infection is triggered only when the developer builds that project locally.


The malware scrambles its payload generation at compile time, switching between nested layers of different encryption mechanisms. Figure 1 shows a benign infected Xcode project on GitHub with two separate XCSSET payloads.


Figure 1. Infected Xcode project on GitHub.


The XCSSET v40 infection chain consists of four distinct stages prior to final payload execution:


- The initial loader script establishes C2 communication
- The second stage collects basic fingerprinting information on the system and downloads further modules
- The third stage includes a temporary staging applet that is dropped onto the system to load the final stage into volatile memory space
- The fourth stage is the core module logic


The moment this memory-resident core module loop becomes active, the malware terminates its staging processes and deletes all installation files from the disk. The goal of the core-module (internally called boot


) is to execute and load additional, specialized modules into memory, such as keyloggers, clipboard hijackers or browser hijackers.


Figure 2 describes XCSSET v40’s infection chain.


Figure 2. XCSSET v40 full infection chain.


## New Module Breakdown


Our analysis of XCSSET v40 uncovered 17 distinct modules, each designed for a different goal. The modules were delivered via a dynamic C2 infrastructure and executed in memory.


We found that the operators have enhanced several of its legacy modules while introducing two new components. These include a Chrome hijacking backdoor and a Telegram trojanizer.


We provide the full list of XCSSET v40 modules in Appendix B.


### Chrome Hijacking Backdoor via Chrome DevTools Protocol (CDP) Protocol


The Chrome hijacking module controls the browser by misusing a legitimate Chromium feature, the CDP.


For the CDP-based hijacking to work, the malware must redirect how the user interacts with the browser. It does this by wrapping the benign Google Chrome binary in a malicious persistence script. When a victim launches Google Chrome, the wrapper executes a three-step chain:


- **The orchestrator check:** First, it restarts the main XCSSET orchestrator module (boot) every time Google Chrome is initialized, ensuring the malware's core process remains active
- **CDP execution:** It then launches the legitimate Google Chrome application with specific command-line arguments that activate the CDP on a pre-defined local port, exposing the browser's internal engine
- chrome_remote


**backdoor:** Finally, it drops and launches a specialized Chrome hijacking binary ( chrome_remote


). This binary connects to the opened CDP port, allowing the attackers to execute arbitrary JavaScript, manipulate active browser sessions and extract cookie tokens invisibly.


Figure 3 illustrates the module’s infection and execution chain.


Figure 3. Chrome-hijacking backdoor’s execution chain.


**Inside the chrome_remote Binary**


The chrome_remote


binary dropped by the browser hijacking module establishes a persistent WebSocket connection to the C2 server to pull down real-time JavaScript payloads. Leveraging CDP's ability to inject code before a page even loads allows the malware to force the browser to evaluate and execute these remote scripts on every new tab or document the user opens.


Once injected into a webpage, the malware’s dynamic scripts override critical browser APIs to manipulate the user's active session for the following goals:


- **Traffic interception:** Hooks placed on window.fetch


and XMLHttpRequest


monitor to exfiltrate sensitive data streams, credentials and API tokens
- **Crypto wallet manipulation:** Intercepting MetaMask's Ethereum provider allows the malware to alter cryptocurrency wallet addresses or manipulate decentralized application (dApp) transactions
- **Credential theft:** Overriding password-manager autofill fields captures credentials


This module is able to pivot from a browser hijack to full host-level compromise, operating within the context of the legitimate Google Chrome process. The binary monitors active tabs for specific browser console logging events. If the operator wants to run a local system command on the infected machine, they execute a standardized string such as a console.log


prefixed with a specific delimiter.


The chrome_remote


binary intercepts this console event, strips the delimiter and passes the remaining payload to the host's underlying shell handler ( exec.Command


). The resulting shell output is then packaged and routed back through the active CDP WebSocket to the C2 server, establishing a stealthy, fileless reverse shell.


We reported the information about this threat to Google. This behavior is protected against in Windows, and Google is currently working on expanding the same protections to macOS.


### Telegram Trojanizer


We identified a new Telegram Desktop trojanizer module in May 2026 that was absent from the April 2026 deployment. The delayed introduction of this module demonstrates that the threat actor was actively refining XCSSET v40 after it was already deployed in the wild.


This new module performs the following activities:


- Downloading a pre-built malicious Telegram.app


ZIP
- Wiping the legitimate copy
- Dropping the C2-supplied replacement in its place
- Ad hoc code-signing the fake Telegram app
- Issuing a kill


command to the original Telegram process so the victim relaunches the trojanized copy


This module was updated with a custom AES-encrypted configuration from a dedicated endpoint ( /w?tr


). We have observed this security mechanism in other modules in[earlier iterations](https://www.microsoft.com/en-us/security/blog/2025/09/25/xcsset-evolves-again-analyzing-the-latest-updates-to-xcssets-inventory/) of the XCSSET malware family.


The decrypted configuration is written to ~/.tr


, and a companion ~/.tr_map


file tracks state. Whenever the SHA-1 of .tr


changes, .tr_map


is cleared. Both files are then uploaded back to the C2 as base_tr_file.txt


and base_tr_map.txt


.


Because the configuration blob itself was not captured during our collection window, we could not verify its exact contents. However we assess that this is how XCSSET’s operators kept server-side track of which Telegram-related markers existed on each infected host.


This is not the first time XCSSET has been seen targeting Telegram. The original 2020 generation of XCSSET featured dedicated telegram / telegram_lite


data-stealing modules. The 2025 XCSSET iteration included the data_folders_finder


module that exfiltrated Telegram's chat history, cached files and local encryption keys.


The newest Telegram trojanizer represents a meaningful escalation in the attacker’s access to the app. Rather than a one-time copy of Telegram-related data, the attacker now replaces the application binary itself, giving them an in-process foothold.


## The Invisible Malware: New Tactics, Techniques and Procedures (TTPs) Breakdown


When analyzing v40, it became clear that XCSSET went through architectural changes and made core changes to its TTPs.


The attackers behind the malware enhanced its stealth practices to sabotage detection and thwart analysis, while also adding new persistence and data theft methods. This section highlights the recent TTPs observed in v40 illustrated in Figure 4, including:


- Multi-layered encryption
- Polymorphism
- New fileless persistence
- Impairing defenses
- Virtual machine (VM) evasion


Figure 4. New XCSSET v40 TTPs.


### Multi-Layered Polymorphism and Encryption


The architectural hallmark of XCSSET v40 is its defense-evasion framework, combining overlapping layers of polymorphism and a dual-key encryption scheme. Rather than relying on a single defensive trick, the malware implements a multi-tiered cryptographic gauntlet across its binaries, network payloads and internal source code. Figure 5 describes the XCSSET v40 evasion stack:


Figure 5. Layers of polymorphism and encryption in XCSSET v40.


#### Binary and Network-Level Polymorphism


The malware leverages polymorphism to rotate its digital fingerprints and evade detection. The loader binary, which is responsible for executing the core modules in memory, is recompiled on the C2 server every few hours. During analysis, we observed eight distinct hashes delivered to a single endpoint within a 24-hour window.


The functional modules streamed to the orchestrator are polymorphic. Each component is encrypted via AES-256-CBC using a per-build key and a randomized Initial Vector (IV) prepended to the ciphertext. Because the IV shifts with every single transmission, even two identical modules served seconds apart will result in two different encrypted blobs. Figure 6 illustrates the encrypted payload injection process into osascript


as detected in Cortex XDR.


Figure 6. Encrypted module payload injected to XCSSET v40’s loader.


#### Network Level Dual-Key Architecture


While previous versions of XCSSET protected their C2 communications using a single, hard-coded plaintext key, v40 introduces a dual-key architecture that separates inbound and outbound encryption.


Unlike its predecessors, XCSSET v40 embeds its inbound key within the compiled AppleScript loader. As a result of this compartmentalized key placement, defenders who retrieved the outbound key from network telemetry will not be able to decrypt and access the core logic of the malware.


#### Module Source-Code Obfuscation and String-Literal Ciphers


The malware applies a third layer of polymorphism at the structural code level. Every internal string literal is dynamically encoded using a per-module keyed Caesar cipher featuring a randomized 52-character alphabet and variable shift values. As a result, no two builds of the same module share common string signatures.


XCSSET v40’s developers also implemented a pre-compilation substitution cipher for all internal module, function and variable names. Because this obfuscation takes place on the C2 server before distribution, the decryption mapping is absent from the host endpoint. This absence means that analysts cannot reverse a local execution routine to reveal the original code structure.


Figure 7 includes a scrambled source-code module with decrypted string literals.


Figure 7. Encrypted function names in the boot module.


By leveraging advanced pattern matching and LLM assistance, we broke the identifier substitution cipher. This allowed us to trace the obfuscated module and function names back to their original, operator-assigned names. This allowed us to dive into the malware’s core logic.


XCSSET adopted new technologies to scale their operations. This can also be a reminder for the threat intelligence community that defenders can harness those same capabilities to neutralize this threat.


### New Fileless Persistence


Beyond introducing polymorphic capabilities, XCSSET v40 also added a new fileless persistence to its TTPs. In addition to its usual persistence through Git hooks, Launch Daemons and trojanized applications, v40 adopted another method that misuses the macOS defaults


configuration system.


Defaults


is the macOS counterpart to the Windows Registry, which is a built-in mechanism for managing user preferences and application settings.


Historically, macOS malware families like[NetWire](https://objective-see.org/blog/blog_0x45.html) and[FruitFly](https://objective-see.org/blog/blog_0x25.html) have misused the defaults utility to store state data. XCSSET v40 instead uses this utility to shift from predictable, disk-resident persistence to a fileless re-infection loop.


Rather than dropping additional scripts on disk between cycles, XCSSET v40 writes a Base64-encoded staging payload into a preferences domain it generates per host. Inside the domain, the malware writes the payload under keys that are meant to seem random, like mpirv_eahpi_apm


or ychax_muwch_ucy


. When a victim launches a trojanized or hijacked application, the threat runs a one-liner to retrieve and decode the payload:


The decoded blob re-infects the host, with the SRC


tag identifying which infection vector (e.g., hijacked browser, infected Xcode project or trojanized application) is responsible for triggering the re-arm.


Beyond standard persistence, XCSSET v40 uses the defaults


system during initial infection to store and query system information. Misusing defaults


as an operational configuration cache is uncommon in the macOS malware landscape.


### **Impairing Defenses**


XCSSET v40 also introduces significant defense-evasion techniques that were not observed in prior campaigns. In this multi-part effort to thwart Apple’s defenses, XCSSET v40:


- Disables the SoftwareUpdate


configuration channel
- Terminates cloud telemetry mechanisms
- Locks XProtect signature databases
- Resets the[Transparency, Consent and Control (TCC) framework’s](https://www.huntress.com/blog/full-transparency-controlling-apples-tcc) databases


#### Disabling the SoftwareUpdate


Configuration Channel


XCSSET v40 executes the following commands to hinder the machine’s ability to receive security updates:


Setting these values to false


prevents the endpoint from automatically retrieving updates to crucial macOS signature databases like:


- XProtect


- MRT


- TCC


This also prevents access to Apple's Rapid Security Response channel, which delivers emergency patches between full macOS releases.


#### Termination of Cloud Telemetry Mechanisms


XCSSET v40 runs a constant loop that hinders the endpoint’s ability to send security-related data through the CloudTelemetryService


process. This evasion method blocks the transmission of local security telemetry to Apple, ensuring that the operator's tooling is not sampled into subsequent XProtect signature releases.


#### Exclusive File Lock on the XProtect Signature Database


The malware spawns a Perl process that tries to acquire and hold access to the endpoint's YARA-rule database ( XPdb


). This exclusive file lock on the XProtect signature database ensures that if the endpoint does receive a security update, its content could not be written to disk.


#### TCC Database Reset Upon Denial of Permissions


Prior XCSSET versions terminated module execution when the user denied AppleEvents automation prompts. XCSSET v40 instead invokes tccutil reset


AppleEvents


, which clears the user's TCC decision database for the AppleEvents service. It then reloads a TCC prompt, masquerading as System Settings or Xcode to trick the user into re-granting automation permissions to the malware's bundle ID. The subsequent automation request is treated as a first-time prompt, redisplaying the consent dialog.


### Anti-VM Reporting


XCSSET v40 also attempts to avoid running on VMs. Upon execution of the stats


module (one of the first modules downloaded to the machine), the module generates a set of checks on the machine’s CPU and hardware metadata. This check is to determine whether or not the infected endpoint is a VM.


Once the module performs those checks, it calculates a final verdict (" Model Identifier suggests VM: false


", " Result: likely physical


") and ships the results over to the C2. Hosts reporting a virtual environment receive no further module deliveries, ensuring that automated sandboxes do not analyze XCSSET’s core logic.


## C2 Infrastructure Analysis


By analyzing XCSSET v40’s Uniform Resource Identifier (URI) structure and domain registration strategies, we were able to learn more about the timeline of the most recent campaign. We even found several operational security (OPSEC) failures that provided insights into the attacker’s strategies and capabilities.


### **Endpoint URL** B **reakdown**


XCSSET v40 shows a clear pattern of URL endpoint structure throughout the campaign, assigning distinct functionality to each URI endpoint as shown in Table 1.


**Path** **Method** **Purpose**


/d/<rotated_binary_name>


GET


<Base64- + AES-encrypted payload> Binary download (e.g., AppleScript loader, Chrome hijacker binary)


/a


GET


Loader and stager retrieval during initial infection


/s/<rotated_module_name>


GET


<Base64- + AES-encrypted payload> AppleScript module retrieval (executed in-memory)


/l


POST -d


<Base64 payload> Status and log reporting


/u


POST -F


m=<Base64 payload> File exfiltration


/p


POST -d t=…&u=…&s=…


Heartbeat


/w?<cmd>


GET


Server-side dynamic configuration retrieval (e.g., /w?cbp


for clipboard, /w?tr


for telegram)


/e


POST


Browser-hijack events


Table 1. XCSSET v40 URI endpoint breakdown.


### Domain Registration and Staging Strategy


XCSSET v40's C2 infrastructure reveals a distinct domain registration strategy. In early 2026, the attackers registered about 40 different domains in at least four short bursts across a small pool of IP addresses. The operator staged and aged these domains months before launching the attack wave, to bypass detection of newly registered domains.


Geographically, the attackers’ targeting parameters and naming conventions have also evolved. While the 2025 campaigns relied on \[.\]ru


(Russia) domains masquerading as legitimate content delivery networks (CDNs) and tech properties, the 2026 attack wave introduced \[.\]in


(India) names registered alongside identical \[.\]ru


siblings. This geographic infrastructure pivot aligns with recent victimology, matching our observations of XCSSET v40 targeting developers across South Asia.


#### OPSEC Failures


Despite mitigating detection risks by aging their domains, the attackers compromised their own campaign through poor OPSEC. Specifically, they cross-contaminated the IP addresses hosting those domains across different XCSSET campaigns.


Furthermore, all four operator IP addresses are linked by a single shared SSL thumbprint ( 6e480d648fa1b70612f5d198a66875e28847547d


), reused SSH keys and a shared self-signed remote desktop protocol (RDP) certificate.


## Mitigation Strategies


Defending against XCSSET v40 requires defenders to use real-time behavioral enforcement to flag runtime irregularities. Unit 42 suggests the following mitigations to detect and prevent this threat:


- Implement AI-enhanced process anomaly detection capable of flagging runtime irregularities, specifically monitoring for abnormal AppleScript instances
- Monitor browser launcher paths and block unauthorized file-write activity
- Identify and block the creation of abnormal local system defaults


domains and their modification through the defaults


utility
- Track ad hoc signed applications and untrusted local code signers, immediately isolating binaries that bypass native Apple Gatekeeper requirements
- Implement automated supply-chain dependency scanning to intercept poisoned open-source repositories before they are pulled into internal developer pipelines


## Conclusion


The latest XCSSET version demonstrates a persistent and specialized threat within the macOS landscape. Rather than relying on conventional delivery methods, the framework turns legitimate developer workstations into automated, self-propagating supply chain vectors.


The discovery and analysis of XCSSET v40 reveals a modular framework for exfiltrating data, subverting system security and performing persistent browser hijacking.


While the malware's historical reliance on AppleScript and bash stagers remains consistent, v40 introduces a significant technical evolution in defense evasion. By adopting a largely memory-resident and polymorphic architecture, XCSSET v40 leaves a minimal disk footprint.


Because adversaries are now using AI-enhanced pipelines to generate polymorphic code on the fly, defenders must shift to AI-driven behavioral analysis to identify unusual or suspicious process chains and flag anomalous use of built-in detection mechanisms.


### Palo Alto Networks Protection and Mitigation


Palo Alto Networks customers are better protected from the threats discussed above through the following products:


#### Cortex XDR and XSIAM


At the endpoint level,[Cortex XDR](https://www.paloaltonetworks.com/cortex/cortex-xdr?_gl=1*13pmp8e*_ga*NzQyNjM2NzkuMTY2NjY3OTczNw..*_ga_KS2MELEEFC*MTY2OTczNjA2MS4zMS4wLjE2Njk3MzYwNjEuNjAuMC4w) blocks XCSSET on macOS hosts using Behavioral Threat Protection (BTP) to terminate fileless, in-memory execution chains—including suspicious osascript calls, multi-pass base64/xxd decoders, and process spawning from infected .xcodeproj build phases—while[Advanced WildFire](https://docs.paloaltonetworks.com/wildfire) inspects and blocks payloads on disk.


At the Security Operations level,[Cortex XSIAM](https://www.paloaltonetworks.com/resources/datasheets/cortex-xsiam-aag) correlates these host-level detections with developer repository, network, and identity telemetry, providing SOC analysts with a unified attack narrative and automated playbooks to stop cross-environment supply-chain propagation.


#### Advanced URL Filtering and Advanced DNS Security


[Advanced URL Filtering](https://docs.paloaltonetworks.com/advanced-url-filtering) and[Advanced DNS Security](https://docs.paloaltonetworks.com/dns-security) identify known domains and URLs associated with this activity as malicious.


If you think you may have been compromised or have an urgent matter, get in touch with the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) or call:


- North America: Toll Free: +1 (866) 486-4842 (866.4.UNIT42)
- UK: +44.20.3743.3660
- Europe and Middle East: +31.20.299.3130
- Asia: +65.6983.8730
- Japan: +81.50.1790.0200
- Australia: +61.2.4062.7950
- India: 000 800 050 45107
- South Korea: +82.080.467.8774


Palo Alto Networks has shared these findings with our fellow Cyber Threat Alliance (CTA) members. CTA members use this intelligence to rapidly deploy protections to their customers and to systematically disrupt malicious cyber actors. Learn more about the[Cyber Threat Alliance](https://www.cyberthreatalliance.org/) .


## Indicators of Compromise


### XCSSET v40 C2 Domains


- accapple\[.\]ru


- adschecks\[.\]ru


- adschecks.ru


- adsmobi\[.\]ru


- adsmorein\[.\]in


- adsmoreme\[.\]in


- amdcdn\[.\]ru


- amzndev\[.\]in


- amzndev\[.\]ru


- amznprod\[.\]in


- applecdn\[.\]ru


- appledisk\[.\]ru


- appledns\[.\]ru


- applehosts\[.\]ru


- appletime\[.\]in


- bulksec\[.\]ru


- cdnamz\[.\]in


- cdnamz\[.\]ru


- cdnapple\[.\]in


- cdnatapple\[.\]ru


- cdnroute\[.\]ru


- checkcdn\[.\]ru


- chromeads\[.\]ru


- cnmag\[.\]ru


- devnetaps\[.\]ru


- dnsapple\[.\]ru


- dnsrelays\[.\]ru


- explorecdn\[.\]ru


- fiddlejoy\[.\]ru


- figmacat\[.\]ru


- figmanets\[.\]in


- funchats\[.\]ru


- gironetcdn\[.\]ru


- goalmate\[.\]ru


- googlenets\[.\]ru


- greencn\[.\]ru


- icloudsnet\[.\]ru


- imails\[.\]ru


- legalads\[.\]in


- littleads\[.\]in


- littledns\[.\]ru


- maganet\[.\]ru


- mindelgate\[.\]ru


- netapsdev\[.\]ru


- netcdnads\[.\]in


- netcdnamz\[.\]ru


- netcdndev\[.\]in


- netcorps\[.\]ru


- netsprot\[.\]in


- netsproto\[.\]in


- networkads\[.\]in


- rigacdn\[.\]in


- rigmajoys\[.\]in


- rigmanet\[.\]ru


- rigmanets\[.\]in


- sahusuzuki\[.\]in


- stuffdns\[.\]in


- testjoys\[.\]ru


- timewebnet\[.\]in


- vigmanet\[.\]ru


- whitead\[.\]in


- whiteads\[.\]ru


- wincdn\[.\]ru


- windsecure\[.\]ru


### **C2 URLs - Chrome CDP Helper Binary**


- hxxps\[:\]//amzndev\[.\]in/d/zw_sfp64


- hxxps\[:\]//amzndev\[.\]ru/d/zw_sfp64


- hxxps\[:\]//googlenets\[.\]ru/d/zw_sfp64


- hxxps\[:\]//netcdndev\[.\]in/d/zw_sfp64


- hxxps\[:\]//whitead\[.\]in/d/zw_sfp64


- hxxps\[:\]//whiteads\[.\]ru/d/zw_sfp64


### **XCSSET v40 C2 IP Addresses**


- 91.108.106\[.\]229


- 95.142.35\[.\]34


- 95.142.35\[.\]206


- 95.142.37\[.\]159


- 151.243.109\[.\]188


- 178.208.92\[.\]129


- 178.208.92\[.\]168


### XCSSET v40 SSL Thumbprint


- 6e480d648fa1b70612f5d198a66875e28847547d


## Additional Resources


- [XCSSET Mac Malware: Infects Xcode Projects, Uses 0Days](https://www.trendmicro.com/en_us/research/20/h/xcsset-mac-malware--infects-xcode-projects--uses-0-days.html) – Trend Micro
- [XCSSET evolves again: Analyzing the latest updates to XCSSET’s inventory](https://www.microsoft.com/en-us/security/blog/2025/09/25/xcsset-evolves-again-analyzing-the-latest-updates-to-xcssets-inventory/) – Microsoft
- [New XCSSET malware adds new obfuscation, persistence techniques to infect Xcode projects](https://www.microsoft.com/en-us/security/blog/2025/03/11/new-xcsset-malware-adds-new-obfuscation-persistence-techniques-to-infect-xcode-projects/) – Microsoft
- [NetWire Analysis](https://objective-see.org/blog/blog_0x45.html) – Objective-See
- [FruitFly Analysis](https://objective-see.org/blog/blog_0x25.html) – Objective-See
- [Chrome DevTools Protocol](https://developer.chrome.com/docs/devtools) – Developer


## Appendix A - XCSSET v40 Infection Lifecycle Breakdown


The infection lifecycle of XCSSET v40 can be categorized into four phases, as detailed below.


### **P** hase 1: Initial Compromise and Execution


The infection lifecycle begins when a developer opens a poisoned Xcode project, typically downloaded from GitHub or built internally:


- The moment the developer builds the project locally, a malicious run-script phase executes silently in the background
- The malware dynamically scrambles its payload generation at compile time, switching between nested layers of Hex- and Base64-encoding
- This decoded script initiates contact with the attacker’s C2 infrastructure by executing a curl


request to the /a


with basic execution context ( p=xcode_phase


) to retrieve the next stage


### Phase 2: Host Reconnaissance and Staging


- The retrieved staging payload runs a second, specialized curl


command that collects and exfiltrates primary host metadata
- The payload queries the operating system type ( uname -s


) and the current username ( whoami


), transmitting these details back to the C2 endpoint


### Phase 3: Loader Wrappers and Binaries


If the C2 approves the host profile, it returns a bash script obfuscated via a custom substitution cipher. This script handles the high-risk task of staging the main loader while covering its tracks:


- The bash script performs deeper hardware fingerprinting, matching the host's serial number against targeted profiles
- It then pulls the primary malware loader to /tmp/r


and compiles an accompanying AppleScript wrapper as /tmp/p.app


on the fly
- To eliminate forensic evidence, the loader wrapper is executed in memory by osascript


, which in turn downloads the main orchestrator module and its AppleScript loader
- After execution, the malware terminates osascript


and deletes both /tmp/r


and /tmp/p.app


from the disk to minimize its forensic footprint


### Phase 4: Orchestrator and Core Logic Modules


Once it erases its disk footprint, the malware transitions to a mostly fileless execution:


- The main orchestrator module named “boot” by the developers runs and retrieves additional module payloads from https://<C2>/s/<encoded_module_name>


- Finally, the orchestrator pipes the payloads to the AppleScript to decrypt and execute the modules in memory


## Appendix B - XCSSET V40 Module Breakdown


This appendix maps the 17 modules identified in XCSSET v40. We correlated the canonical XCSSET v40 module names recovered through our decryption efforts with the terminology used in the three prior public reports.


Please note that since XCSSET has gone through major architectural changes in v40, some modules’ logic may be expanded or split into different modules. It is also worth noting that previous reports of XCSSET did not decrypt the original module names, and therefore they appeared as jumbled strings.


**Module Name** **Previously Recorded Names** **Functionality**


boot


boot, bootstrap


Main orchestrator, module-dispatch loop


stats


vexyeqj, seizecj


Initial reconnaissance on the infected endpoints, exfiltrates existing browser extensions, performs anti-VM checks


clipboard_v2


bnk


Keyboard hijacker


payloader


payloader


Secondary module dispatcher, downloads dynamic configuration files, performs keyboard hijacking


replicator_finder


replicator, dfhsebxzod


Xcode project file infector


git_finder


pods_infect, jez, jey


Git pre-commit hook infector


zip_infect_finder


logic previously existed in dfhsebxzod


and replicator


modules Split out in v40 from replicator_finder


. Recursively traverses user directories to identify and infect Xcode projects present in .zip


archives.


data_folders_finder


finder, txzx_vostfdi, neq_cdyd_ilvcmwx


C2-driven folder finder and data exfiltrator


firefox_data


iewmilh_cdyd


Infostealer targeting Firefox


notes_app


cozfi_xhh


Apple Notes exfiltrator


settings_app


xmyyeqjx


LaunchDaemon-based persistence using a fake Settings.app, defense evasion by blocking XProtect features


finder_app


finder_app, vectfd_xhh


TCC permission misuse and reset, creates trojanized app that mimics Finder/ Xcode/ Terminal/ Reminders/ SimulatorTrampoline


persist


hfdieiz


, some of the logic previously existed in xmyyeqjx


.zshrc


and Dock-app based persistence


browser_remote


chrome_remote, firefox_remote, opera_remote, yandex_remote, brave_remote, edge_remote, 360_remote


(one module per browser, each downloads a backdoor masquerading as browser from the server; uses an exploit to hijack the actual browser) Unified browser-hijack dispatcher checks for existing browser on the endpoint and dispatches different hijacking modules


safari_remote


safari_remote


Browser hijacker


chrome_remote


new module (v40) Browser backdooring and hijack through CDP protocol
Note: Trend Micro’s 2020 report mentions a module named chrome_remote


, but v40’s module has different functionality


tdesktop


new module (v40) Telegram desktop trojanizer
