---
schema_version: "1.0.0"
document_id: "534c4726624cda34512b9eaa726856b52de8b9467614b540c435828afe822200"
company_key: "varonis-systems-inc-common-stock"
company: "Varonis Systems Inc."
source_id: "varonis-systems-inc-common-stock-rss-915499d71e96"
canonical_url: "https://www.varonis.com/blog/dolphin-x-stealer"
published_at: "2026-07-22T13:00:01+00:00"
first_seen_at: "2026-07-24T05:47:29.345543+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:f5f9861e3dfe7ee8231f6b6b70505ce74fdeb84f452ffda362f2fd86526d0520"
---

# Dolphin X Stealer Targets 300+ Apps and Profiles Users with AI

Varonis Threat Labs discovered Dolphin X advertised on a cybercrime forum by a vendor using the alias “Kontraktnik.”


The listing claims the malware can target more than 300 applications and reaches far beyond browser passwords. Its collection capabilities include cryptocurrency wallets, .env files, SSH keys, cloud tokens, and other DevOps credentials.


A single archive can contain data from nine browsers, more than 100 wallet extensions, 65 desktop wallets, 10 password managers, and 30 cloud command-line tools. This gives the malware potential access to everything from a victim’s personal accounts to the credentials used to manage their employer’s cloud environment.


Another feature, called the “AI Profiler,” scores infected users based on their application usage, browsing activity, and installed software. Attackers receive the rankings in a daily summary, helping them identify high-value victims and decide where to focus next.


We obtained Dolphin X’s operator panel and analyzed it in an isolated lab, beginning with how the agent is built.


Forum post advertising Dolphin X as an all-in-one RAT.


Forum post advertising Dolphin X as an all-in-one RAT.


## Remote build and mutation


The panel is a desktop client built around a configuration wizard, but nothing compiles on the operator's machine. The operator sets the agent's C2 address, installation path, persistence, and evasion options, then the client submits the configuration to backend.thedolphinx\[.\]top:8443, where compilation happens.


Build configuration submitted to the remote server.


Build configuration submitted to the remote server.


Routing every build through the vendor's server also gives the seller a place to modify each binary before it returns. The panel exposes this as an opt-in mutation engine with three tiers, the deeper two locked behind the PRO plan. The engine is off by default, and the panel itself notes that off means the same hash across all builds.


Server-side mutation options are off by default and partly gated.


Server-side mutation options are off by default and partly gated.


Dolphin X’s tiers are structured as follows:


1. The top tier claims to rewrite control flow, substitute instructions, and re-encrypt embedded strings with a fresh key, making stable byte sequences harder to identify.
2.


The middle tier advertises shuffling the import table, which would change the binary's import hash between builds.


3.


The lowest tier advertises rewriting PE timestamps, the Rich header, and section padding. These are the byte regions that brittle YARA rules and hash-based blocklists tend to anchor on.


## Collection scope and AI profiling


The panel lists 329 features across ten categories. For defenders, however, the most important figure is the collection scope: more than 300 application targets appear under the credential-looter category. These targets range from browser logins and cryptocurrency wallets to SSH keys and cloud tokens, with the collected data staged in a single archive.


Feature panel, with the 300+ collection targets under the credential looter category.


On a developer’s machine, .env files and SSH directories often contain over-scoped, long-lived credentials that can provide access to cloud consoles, build pipelines, and production data.


Beyond credential collection, the panel includes a surveillance tab containing the AI Profiler. The seller describes it as an “AI behavioral profiler with app usage tracking, risk score, and daily summary.”


In practice, the feature appears designed to help operators triage victims.


A cybercriminal may control thousands of infected machines, far more than they could review manually. The profiler acts like an automated warehouse sorter, scoring and tagging each victim before returning a ranked list of the most valuable machines to investigate first. The risk score and daily summary determine that order.


Surveillance tab exposing the AI Profiler options.


Surveillance tab exposing the AI Profiler options.


## Defenses and conclusion


Dolphin X’s collection scope reaches well beyond browser passwords to SSH keys, cloud tokens, and DevOps credentials. On the wrong machine, a single infection could expose access to an entire production environment.


Its use of AI is also interesting because it shows us how AI is being integrated into more cybercrime tooling. We saw this with[SpamGPT](https://www.varonis.com/blog/spamgpt?hsLang=en) and[Bluekit](https://www.varonis.com/blog/bluekit?hsLang=en) , and now with Dolphin X as well.


The practical response for security teams comes down to two things:


1.


Keep long-lived credentials off disk wherever possible, especially out of project directories and local credential stores. Infostealers are designed to grab everything in one pass, so anything stored locally should be treated as potentially exposed.


2.


Focus detection on behavior rather than file signatures. For example, explorer.exe running under a non-default desktop is a strong indicator of an HVNC session, regardless of how the malware binary is packed or what hash it uses.


## Indicators of compromise


The controlled panel run and its traffic capture exposed several operator-side indicators.


Indicator


Type


Observed Role


backend.thedolphinx\[.\]top:8443


Host and port


Licensing, telemetry, and remote-build service


thedolphinx\[.\]top


Domain


Parent domain for the vendor backend


726e7fe23560fe03ea36163d5f510b494f41a78bf811c92ff219f64b4bfe2be0


SHA-256


Dolphin X operator panel client executable


## MITRE ATT&CK techniques


The mapping below is a detection reference for this class of tool. Every row is a capability the listing advertises or the panel exposes, so it points to where to look.


Technique


ATT&CK ID


Advertised Use


Credentials from Web Browsers


T1555.003


Nine Chromium and Gecko browsers, with DPAPI decryption


Credentials from Password Stores


T1555


MetaMask vault via PBKDF2 and Exodus seed via DPAPI


Credentials in Files


T1552.001


SSH keys, .env files and more than 30 cloud CLI tokens


Archive Collected Data


T1560


Collected credentials staged into a single archive


Process Injection


T1055


Injection into browser and wallet processes


Bypass User Account Control


T1548.002


Eight launcher-host UAC bypasses


Registry Run Keys / Startup Folder


T1547.001


Persistence through Registry Run Keys or the Windows Startup folder


Scheduled Task


T1053.005


Persistence through scheduled tasks


Disable or Modify Tools


T1562.001


AMSI and ETW patching


Native API


T1106


Direct syscalls intended to bypass user-mode API hooks


Obfuscated Files or Information


T1027


Three-tier opt-in mutation engine, applied at build time


Proxy


T1090


SOCKS5 reverse-proxy functionality


*Note: We analyzed the operator panel and its network traffic, not a sample running on an infected machine. Unless stated otherwise, agent capabilities described above are exposed by the builder or claimed in the vendor’s documentation rather than independently confirmed in execution.*


Stay up to date on the threat landscape by following[Varonis Threat Labs](https://www.varonis.com/varonis-threat-labs?hsLang=en) .
