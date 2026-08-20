---
schema_version: "1.0.0"
document_id: "44346a7bf7dec3b8e33577717c3e6d38c760be5a0379f099a6cbf2bd188e2c42"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:77235c18b6022a75cab986ad5eb5e11aabb16f2c1a58a32e1fd1e1fbf564445a"
---

# Lost in relocation: analysis of a new loader distributing CASTLESTEALER

19 June 2026 •


[Daniel Stepanic](https://www.elastic.co/security-labs/author/daniel-stepanic) •


[Jia Yu Chan](https://www.elastic.co/security-labs/author/jia-yu-chan)


# Lost in relocation: analysis of a new loader distributing CASTLESTEALER


Find out how a new obfuscated loader evades static detection using .reloc section abuse, five anti-VM/language checks and MBA obfuscation to deliver infostealer malware via Google Ads.


8 min read


[Malware Analysis](https://www.elastic.co/security-labs/category/malware-analysis)


A previously undocumented Windows loader tracked as OXLOADER is delivering the CASTLESTEALER infostealer via malicious Google Ads, with low detection rates across static engines and sandbox detonations. The loader uses several obfuscation layers (control-flow flattening, opaque predicates, mixed Boolean-Arithmetic), self-modifying decryption stubs, and abuses the Windows` .reloc` section to stage shellcode.


Elastic Security Labs identified OXLOADER in an active campaign targeting one of our customers; CIS-region and Russian-language exclusions point to a financially motivated, Russian-speaking threat actor. We have found no prior public reporting on this family.


##


Key takeaways


- Elastic Security Labs discovers new loader (OXLOADER)
- OXLOADER observed in campaigns distributing CASTLESTEALER via malicious Google Ads
- CIS-region exclusion and Russian language checks suggest a Russian-speaking, financially motivated threat actor
- Low detection rates across static engines and sandbox detonations
- Elastic Defend stops the entire attack chain using advanced prevention capabilities


#


How malvertising delivered OXLOADER to victims


OXLOADER is distributed via malicious Google Ads impersonating Node.js. Victims are redirected through an intermediary domain to a Storj-hosted batch script, which downloads and executes OXLOADER.


The infection began when the user searched for an` lts version of node.js` and clicked a sponsored result leading to` node-js\[.\]prentiva99\[.\]info` , a[malicious landing page](https://gist.github.com/jiayuchann/fc37e3c047ebd987619e440c44b465ad) designed to impersonate a legitimate Node.js deployment platform. The threat actor operated a Google Ads campaign targeting US-based victims; the ad was last shown on Apr 23, 2026, and the site is now offline. The advertiser was registered under the verified name` ВОЛОДИМИР ТЕРЕЩЕНКО` , based in Ukraine. Whether this reflects the actual operator, a front account, or a purchased identity remains unclear. On May 14, 2026, the advertiser along with their associated ad campaigns were removed from Google entirely.


Upon interaction, the user was redirected through` app\[.\]miloyannopoulos\[.\]com/download?subid1=download` , which responded with a` 302 Found` to the payload URL` link\[.\]storjshare\[.\]io/raw/jux4e4ky5mruo4jkxsssp42sau4q/ruslan/BATPackageBuilderSetup.bat` . This delivered a Windows batch script, hosted on[Storj’s](https://www.storj.io/) legitimate link-sharing service, which the threat actor abused to evade domain-based reputation filtering.


The batch script displays a fake software installation wizard UI, immediately downloads the next-stage executable from the Storj URL` link.storjshare\[.\]io/raw/jwwvr4oskkkjsgevt774ta62ehya/ruslan/aBsvwbdas.exe` via PowerShell, and launches it with` -Verb RunAs` to trigger a UAC elevation prompt.


Following execution of the Batch script, Elastic Defend detected malicious behavior (policy was set to detect only), triggering multiple behavioral rules including` Microsoft Common Language Runtime Loaded from Suspicious Memory` , hinting at a .NET-based payload consistent with` CASTLESTEALER` .


The following is the execution graph of the attack chain from payload download to` CASTLESTEALER` deployment.


#


OXLOADER malware loader: technical analysis


The first OXLOADER sample our team analyzed masquerades as the popular tool,[API Monitor](http://www.rohitab.com/apimonitor) from[rohitab.com](http://rohitab.com/) . Due to the heavy presence of legitimate code and code-hiding techniques, this loader is able to fly under the radar against static file analyzers.


##


How OXLOADER unpacks itself at runtime


The malware begins executing during the CRT initializer phase, before any user code is run. The CRT function` cinit()` invokes` initterm()` , which walks the C++ initializer table (` __xc_a` →` __xc_z` ) calling each entry. The malware developer has hijacked one of these entries, pointing to a function that makes a` RegisterClipboardFormatW()` call before tail-jumping into the first decryption stub.


The loader uses self-modifying techniques with several decryption stubs to unroll itself. Below is an example of a decryption stub being patched in during runtime, before jumping to it.


After the patching has taken place, the loader decrypts a 28,233-byte region. Each byte is decrypted with a single-byte XOR key that updates after every iteration: the just-decrypted plaintext byte is added to the key, which is then used to decrypt the next byte. This similar decryption routine runs three times in total, each over a different region.


##


Obfuscation techniques used to evade static detection


OXLOADER breaks automated function-boundary detection in binary analysis tooling such as IDA Pro using four layered obfuscation techniques: control-flow flattening (CFF), mixed Boolean-Arithmetic (MBA), opaque predicates, and function chunking across non-contiguous code regions. Functions are stitched together with unconditional jumps, and some regions are reached through indirect jumps whose target addresses are computed at runtime via MBA arithmetic. The result is that IDA Pro cannot reliably reconstruct function boundaries, requiring manual fixes.


The loader decrypts various strings at runtime using the following string decryption algorithm:


```text
uint32_t obf_xor_a1_with_a2_plus_33FDA(uint32_t a1, uint32_t a2) {
return a1 ^ (a2 + 0x33FDA);
}
```


After the code is fully unpacked/decrypted, the malware combines this string decryption function with an Adler-32 API hashing[algorithm](https://github.com/OALabs/hashdb/blob/main/algorithms/dualacc_modfff1.py) to dynamically resolve its imports.


##


How does OXLOADER evade sandbox and VM detection?


After resolving its APIs, OXLOADER performs various checks to ensure the machine executes in a clean environment, avoiding execution in sandbox environments.


Check Method Threshold


Emulation` WNetAddConnection2W` with malformed resource Expects` ERROR_BAD_NAME` (0x43)


CPU count Process environment check ≥ 3 CPUs


RAM` GlobalMemoryStatusEx` ≥ 3 GB physical memory


Display refresh rate WMI` Win32_VideoController` query ≥ 20 Hz


Geographic region` GetUserGeoID` Excludes CIS GEOIDs


The first check attempts to connect to a deliberately malformed network resource (` *72s@1s` ) using` mpr!WNetAddConnection2W` . This technique appears to defeat emulation/sandboxes that may hook or return a successful connection unconditionally. The malware developer verifies this call by accessing the TEB directly to retrieve the` LastErrorValue` . The loader expects this error code to be` ERROR_BAD_NAME (0x43)` , if the error code is anything other than this value, the malware takes the failure branch and stops execution.


The second check is an anti-sandbox test based on the processor count: the loader requires the host to have at least 3 CPUs to continue. Many sandboxes and analysis VMs are provisioned with one or two CPUs to conserve resources, so this threshold filters out these automated analysis environments.


The next check uses` GlobalMemoryStatusEx()` to verify that the host has at least 3 GB of available physical memory.


A further check uses WMI to query the system display's refresh rate, executing the WQL statement` SELECT CurrentRefreshRate FROM Win32_VideoController` and comparing the returned value (in Hertz) against a threshold of 20. Physical monitors usually report around 60 Hz or higher, while headless and default-virtualized configurations typically report 0 or 1, and values below 20 cause the loader to abort.


##


Geographic and language exclusions


The final two checks halt execution if the host is located in a[CIS](https://en.wikipedia.org/wiki/Commonwealth_of_Independent_States) country or configured for the Russian language. The first check in this category uses` GetUserGeoID` to retrieve the system’s geographic region and compares it against a hardcoded list of CIS country GEOIDs.


The second check uses` GetUserDefaultUILanguage` and matches against LANGID (` 0x419 - Russian, Russia` ), the standard configuration for a Russian language Windows installation.


##


Shellcode staging via .reloc section and OCX file


After all the checks have passed, the malware makes a copy of the Windows DirectUI Engine DLL (` C:\\Windows\\System32\\dui70.dll` ), storing it in a temporary location using a randomly generated name with the` .ocx` extension (` PFHemkxVk.ocx` ). This extension choice inspired the OXLOADER family name.


OXLOADER then creates a new section named (` .xtext` ) in this target DLL (` PFHemkxVk.ocx` ).


This new section (` .xtext` ) is configured with RWX (read/write/execute) protections in preparation to store and execute the next stage of the malicious code.


This updated DLL (` PFHemkxVk.ocx` ) is then loaded into the existing loader process via` LoadLibraryA` .


In a normal Windows executable, the` .reloc` section contains a table of` IMAGE_BASE_RELOCATION` blocks that the Windows loader applies to patch absolute addresses when the image is loaded at an address other than its preferred base. In this sample, the malware developer is using
the` .reloc` section to house malicious code instead of the base relocation entries. This is a strong static-analysis red flag: legitimate toolchains do not emit code into the` .reloc` section.


This shellcode from the` .reloc` section is then copied to the newly created section (` .xtext` ) in the OCX file, and the loader then calls this code.


As observed previously, there is another decryption stub used to unpack this next stage.


##


In-memory infostealer delivery via DonutLoader and .NET assembly


This next-stage shellcode is a payload configured from[DonutLoader](https://github.com/TheWover/donut) , an open-source shellcode generator used to wrap .NET assemblies, DLLs, and EXEs into position-independent shellcode (PIC) for in-memory execution. During unpacking, the shellcode decrypts the loader's embedded configuration and execution context using the Chaskey-LTS block cipher in CTR mode with the key (` 6E0A1F8F77F7011561F6F9CA96B71B8F` ) and IV (` 956C6128E9362E075F8D006C93616A66` ).


After the decryption, the payload is decompressed via aPLib then bootstrapped through DonutLoader’s` RunPE()`[function](https://github.com/TheWover/donut/blob/47758d787209dd1744f58c140102ac91b649df16/loader/inmem_pe.c#L57) . The final payload is a newly discovered information stealer by[Huntress](https://www.huntress.com/blog/clickfix-castleloader-backgroundfix) called CASTLESTEALER. This attribution is based on the same AES key used for C2 communications found between` 0xDEADBEEF` markers in a previous[sample](https://www.virustotal.com/gui/file/ed391a16389234f9ebb6727711baaf3e068d7f77c465708fa3e8b7d0565d7fb9) .


##


Second OXLOADER variant: same loader, different masqueraded program


A second OXLOADER variant masquerades as a Node.js installer rather than API Monitor, but uses the identical loader mechanism.


On May 13, 2026, we discovered that the redirector endpoint` app.miloyannopoulos\[.\]com/download` responded with one of two` Location` header fields, chosen at random:


- ` https://link.storjshare\[.\]io/raw/jv5uebuqwzfpmtahj34q753ptykq/node/BATPackageBulderSetup.bat`
- ` https://link.storjshare\[.\]io/raw/jvsmdybqmvwep2oawbobp6ub7aza/node/node-v24.15.0-x64-86.exe`


The Batch installation script (` BATPackageBulderSetup.bat` , with a typo for “Builder”) remained mostly identical. The only difference was that the Storj payload link now pointed to a different binary named` node-v24.15.0-x64-86.exe` .


The payload attempts to masquerade itself as benign CMake code while retaining “node” in the filename, likely to keep the lure theme intact. We believe the earlier “API Monitor” sample was likely a distribution error by the operator.


Upon execution, we noticed the same pattern: indirect jumps used for in-memory self-decryption, followed by the loading of` mpr.dll` and a call to` WNetAddConnection2W` . We confirmed that it is the identical loader mechanism discussed previously, and this sample also loads` CASTLESTEALER` .


Below is a snippet where the self-decryption occurs. Dummy CMake-related strings appear to be passed as arguments to a function call, which patches a small decryption stub into memory immediately after the call site. Execution then jumps to the stub to decrypt subsequent instructions, and this process repeats 2 more times until the main malware body is decrypted.


#


Conclusion: why defenders should track OXLOADER


OXLOADER is in an early operational phase, but the engineering behind it suggests this family is worth watching. The code obfuscation, anti-VM measures, benign-looking code used to masquerade its binaries, and unique staging techniques reflect deliberate engineering choices to evade analysis. That investment is paying off, resulting in low detection rates across static engines and detonation runs, giving OXLOADER a window to operate before it gets hunted down.


##


REF8372 through MITRE ATT&CK


Elastic uses the[MITRE ATT&CK](https://attack.mitre.org/) framework to document common tactics, techniques, and procedures that threats use against enterprise networks.


###


Tactics


Tactics represent the why of a technique or sub-technique. It is the adversary’s tactical goal: the reason for performing an action.


- [Resource Development](https://attack.mitre.org/tactics/TA0042)
- [Initial Access](https://attack.mitre.org/tactics/TA0001)
- [Execution](https://attack.mitre.org/tactics/TA0002)
- [Defense Evasion](https://attack.mitre.org/tactics/TA0005)
- [Discovery](https://attack.mitre.org/tactics/TA0007)
- [Command and Control](https://attack.mitre.org/tactics/TA0011)
- [Collection](https://attack.mitre.org/tactics/TA0009)
- [Exfiltration](https://attack.mitre.org/tactics/TA0010)


###


Techniques


Techniques represent how an adversary achieves a tactical goal by performing an action.


- [Acquire Infrastructure: Malvertising](https://attack.mitre.org/techniques/T1583/008/)
- [Stage Capabilities: Upload Malware](https://attack.mitre.org/techniques/T1608/001/)
- [User Execution: Malicious File](https://attack.mitre.org/techniques/T1204/002/)
- [Command and Scripting Interpreter: PowerShell](https://attack.mitre.org/techniques/T1059/001/)
- [Abuse Elevation Control Mechanism: Bypass User Account Control](https://attack.mitre.org/techniques/T1548/002/)
- [Obfuscated Files or Information: Encrypted/Encoded File](https://attack.mitre.org/techniques/T1027/013/)
- [Deobfuscate/Decode Files or Information](https://attack.mitre.org/techniques/T1140/)
- [Obfuscated Files or Information: Embedded Payloads](https://attack.mitre.org/techniques/T1027/009/)
- [Masquerading: Match Legitimate Name or Location](https://attack.mitre.org/techniques/T1036/005/)
- [Hijack Execution Flow: DLL Side-Loading](https://attack.mitre.org/techniques/T1574/002/)
- [Virtualization/Sandbox Evasion: System Checks](https://attack.mitre.org/techniques/T1497/001/)
- [System Location Discovery: System Language Discovery](https://attack.mitre.org/techniques/T1614/001/)
- [Reflective Code Loading](https://attack.mitre.org/techniques/T1620/)


##


Remediating REF8372


###


Prevention


- [Potential Browser Information Discovery](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/discovery_potential_browser_information_discovery.toml)
- [Potential Evasion with Hardware Breakpoints](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_potential_evasion_with_hardware_breakpoints.toml)
- [Suspicious Thread Context Manipulation](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_suspicious_thread_context_manipulation.toml)
- [VirtualAlloc API Call from an Unsigned DLL](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_virtualalloc_api_call_from_an_unsigned_dll.toml)
- [Suspicious Remote Memory Allocation](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_suspicious_remote_memory_allocation.toml)
- [Execution from Suspicious Stack Trailing Bytes](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/command_and_control_execution_from_suspicious_stack_trailing_bytes.toml)
- [Microsoft Common Language Runtime Loaded from Suspicious Memory](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_microsoft_common_language_runtime_loaded_from_suspicious_memory.toml)
- [Suspicious PowerShell Execution](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/execution_suspicious_powershell_execution.toml)
- [Module Stomping from a Copied Library](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/windows/defense_evasion_module_stomping_from_a_copied_library.toml)


####


YARA


Elastic Security has created YARA rules to identify this activity.


- [Windows.Trojan.OxLoader](https://github.com/elastic/protections-artifacts/blob/main/yara/rules/Windows_Trojan_OxLoader.yar)
- [Windows.Trojan.CastleStealer](https://github.com/elastic/protections-artifacts/blob/main/yara/rules/Windows_Trojan_CastleStealer.yar)


##


Observations


The following observables were discussed in this research.


Observable Type Name Reference


` node-js\\\[.\\\]prentiva99\\\[.\\\]info` domain-name Malvertising landing page


` app\\\[.\\\]miloyannopoulos\\\[.\\\]com` domain-name Malvertising Redirector


` fdfc7831e5c24cfa80152860dfe8c056ba079f7df1393bf6bb7b18ed974eda37` SHA-256` BATPackageBuilderSetup.bat` OXLOADER downloader & launcher


` de4f51649ec1a33071854aefe93ffb3fc225e19f802d8dd914676dd5dfef2615` SHA-256` BATPackageBulderSetup.bat` OXLOADER downloader & launcher


` 9a9939dff297997732aaade9b243d695632cbd64033c5fbcb9de3d09b7e6c28d` SHA-256` apimonitor-x64.exe` OXLOADER


` c85f2765a6c3c3f3907c17e57df12f8f68826f74bff3bbfd272af50666d065fe` SHA-256` node-v24.15.0-x64-86.exe` OXLOADER


` 4ec9d9d4d10ad78fc6d7bda7cb17d52984878ccd2dd4302fd1cef152313b9741` SHA-256 CASTLESTEALER


` 39019279686c820c3af5684012a0085a7e2109f612c9fab886dd0577ace5b5c6` SHA-256 CASTLESTEALER


` 89.124.95\\\[.\\\]161` ipv4 CASTLESTEALER C2


` 89.124.115\\\[.\\\]82` ipv4 CASTLESTEALER C2


#### Jump to section


- [Key takeaways](https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer#key-takeaways)
- [How OXLOADER unpacks itself at runtime](https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer#how-oxloader-unpacks-itself-at-runtime)
- [Obfuscation techniques used to evade static detection](https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer#obfuscation-techniques-used-to-evade-static-detection)
- [How does OXLOADER evade sandbox and VM detection?](https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer#how-does-oxloader-evade-sandbox-and-vm-detection)
- [Geographic and language exclusions](https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer#geographic-and-language-exclusions)
- [Shellcode staging via .reloc section and OCX file](https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer#shellcode-staging-via-reloc-section-and-ocx-file)
- [In-memory infostealer delivery via DonutLoader and .NET assembly](https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer#in-memory-infostealer-delivery-via-donutloader-and-net-assembly)
- [Second OXLOADER variant: same loader, different masqueraded program](https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer#second-oxloader-variant-same-loader-different-masqueraded-program)
- [REF8372 through MITRE ATT\\&CK](https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer#ref8372-through-mitre-attck)
- [Tactics](https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer#tactics)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Lost%20in%20relocation:%20analysis%20of%20a%20new%20loader%20distributing%20CASTLESTEALER&url=https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer&title=Lost%20in%20relocation:%20analysis%20of%20a%20new%20loader%20distributing%20CASTLESTEALER)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/oxloader-malware-loader-infostealer&title=Lost%20in%20relocation:%20analysis%20of%20a%20new%20loader%20distributing%20CASTLESTEALER)
