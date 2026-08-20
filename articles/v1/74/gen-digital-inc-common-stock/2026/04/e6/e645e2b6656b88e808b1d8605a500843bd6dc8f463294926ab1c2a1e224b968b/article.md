---
schema_version: "1.0.0"
document_id: "e645e2b6656b88e808b1d8605a500843bd6dc8f463294926ab1c2a1e224b968b"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-news-import-4d91977bfa54"
canonical_url: "https://www.gendigital.com/blog/insights/research/remus-64bit-variant-of-lumma-stealer"
published_at: "2026-04-07T15:20:49+00:00"
first_seen_at: "2026-07-21T21:24:31.621864+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:8e63c8be6c6e74ee870332a49ac64fb751e2d016270f6fa66d7c366cc5c0005f"
---

# Remus: Unmasking The 64-bit Variant of the Infamous Lumma Stealer

By[Vojtěch Krejsa](https://www.gendigital.com/blog/authors/vojtech-krejsa)


,[Jan Rubín](https://www.gendigital.com/blog/authors/jan-rubin)


## Key points


- Gen Threat Labs has identified Remus, a new 64-bit infostealer we attribute to the infamous Lumma Stealer family – emerging in the wake of Lumma’s takedown and the doxxing of its alleged core members.
- In this technical blog post, we detail the compelling evidence tying Remus to Lumma across multiple dimensions.
- We also describe a previously undocumented Application-Bound Encryption bypass employed specifically by Remus and Lumma.
- The first Remus campaigns date back to February 2026, with the malware switching from Steam/Telegram dead drop resolvers to EtherHiding and employing new anti-analysis checks.


## Introduction


When the security industry talks about information stealers, Lumma Stealer, without a doubt, has become the notorious icon of this landscape. Not only could it count itself among the most sophisticated, technically advanced, and widespread stealers-as-a-service in the world, but it was also described in a variety of blog posts from basically everyone in the industry, including us.


In this analysis, we describe a new variant of Lumma Stealer that we call Remus, a new x64 build which we suspected might occur in the foreseeable future after the doxxing of Lumma authors from late August to October 2025. And the future is here.


Remus brings the same stealing arsenal to the table as we already know from Lumma, capable of stealing stored browser passwords, cookies, cryptocurrency, and much more. However, rather than revisiting Lumma's well-documented capabilities, we focus on the remarkable resemblance between Remus and Lumma, as well as the new techniques Remus introduces, including the use of EtherHiding to resolve C2s, replacing the traditional use of Steam and Telegram dead drop resolvers, and additional anti-analysis checks.


The attribution of Remus to Lumma is many-fold and described thoroughly throughout the blog post. The main indicators, to name a few, are the use of the same string obfuscation technique, AntiVM checks, direct syscall/sysenter handling, indirect control flow obfuscation, and, most importantly, an almost identical approach to bypassing AppBound Encryption, which we’ve only seen used explicitly by Lumma to date.


With that said, we can still see active Lumma campaigns all around the world, which makes Remus not a replacement, but a continuous evolution.


## Development timeline


Before diving into the technical details, it is worth noting that during our hunting efforts, we came across several test samples, which even carry a` testbuild` label embedded directly in the binaries (see Figure 1). We are internally referencing these builds as Tenzor (based on encrypted strings). These test builds are already 64-bit and structurally very close to Remus, suggesting they represent a transitional step, or perhaps even a testing ground, between Lumma and Remus.


Figure 1: The string testbuild present in a Tenzor sample. Reference sample: 0580ebf601989457f0708799b431fd4d9f5e59d98838282d72936099aa6636da.


This also brings us to the name itself. Both Tenzor and Remus have left a string artifact in the same part of the code, referencing the` # TENZOR LOG` and` # REMUS LOG` strings. Since we have been tracking only test builds with the Tenzor string and Remus is the variant that is being actively distributed in live campaigns, we took the liberty of naming the new x64 Lumma variant after the latter string – Remus.


Figure 2: Decrypted LOG strings in Tenzor (left) and Remus (right). Reference samples: 0580ebf601989457f0708799b431fd4d9f5e59d98838282d72936099aa6636da (Tenzor), dbf6facd28406361a6a81417b3ff5eb272ccc8dcc58a36bd5335a253ae4bf036 (Remus).


Notably, all the Tenzor samples carry a build date of September 16, 2025, in their stack-encrypted strings, which is months before the first Remus samples began appearing in the wild at the turn of January and February 2026. The build timing closely coincides with a period when Lumma Stealer suffered a major blow through a doxxing campaign that exposed alleged core members and severely disrupted its operations. This may suggest that some of Lumma’s authors split off, or that Lumma decided to rebrand under a new name in the midst of the doxxing fallout. Nevertheless, at Gen Threat Labs, we track both Remus and Tenzor as variants of Lumma Stealer. That said, the only samples bearing the Tenzor name are the ones that also carry the` testbuild` label. All others are identified as Remus.


Figure 3: Development timeline.


## Similarities between Remus and Lumma


When we first started analyzing Remus, something felt familiar, and for good reason. Many of the techniques and design patterns we encountered in Remus closely mirrored those we had already seen in Lumma Stealer. The main difference is that Lumma was 32-bit while Remus is 64-bit, which naturally introduced some variations. Yet, when we looked past the 32-bit versus 64-bit differences and stripped away the obfuscation layers, what remained was strikingly familiar.


In the following subsections, we highlight the most compelling evidence of a direct connection between Remus and Lumma. Note that this is not an exhaustive list. The overlaps are far more numerous, but we selected just a few that we believe speak for themselves.


### Forgotten dead drop resolver and string obfuscation


For a malware researcher, strings are among the most valuable resources during reverse engineering, and in the case of attributing Remus to Lumma, they proved no different.


Both Remus and Lumma use a virtually identical mechanism for string obfuscation. The encrypted string is first assembled on the stack using a series of various forms of the` mov` instruction, followed by a decryption loop that transforms the data byte by byte. The transformation is often further obscured by MBA (Mixed Boolean-Arithmetic) obfuscation and is either inlined or placed in a standalone function. In any case, each string is protected using a unique transformation, making it very difficult to build a universal static decryptor. As a result, emulating the decryption loops is likely the most effective approach for dealing with encrypted strings in both Remus and Lumma.


Figure 4: Decryption of the string “Processes.txt” in Remus (left) and Lumma (right). Reference samples: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69 (Remus), 0683f353cf3e101f721f1658e2a554ff7888ff9f2c32e23ceb3d23876864a264 (Lumma).


Notably, the decryption loops are often preceded by several` nop` instructions (Lumma) or a single` nop` instruction encoded using multiple bytes (Remus), likely inserted as padding during a custom compilation pass. We are aware that the compilers themselves can emit` nop` paddings. Still, in both Remus and Lumma, these sequences go beyond the typical, making it yet another indicator linking the two together.


After decrypting all strings in both Remus and Lumma and comparing them side by side, we found roughly 100 completely identical strings. Most of these, however, could be attributed to virtually any information stealer. The more telling discovery came when we looked at the strings that were not identical – most of them turned out to be semantically the same, just slightly refactored. This alone would already be a strong indicator.


However, what ultimately confirmed our theory was shifting the string comparison from Lumma vs. Remus to Lumma vs. Tenzor, as Tenzor effectively acts as a bridge between the other two – many of the strings we had previously matched only semantically between Remus and Lumma were still present in Tenzor in their original, unmodified form, directly matching Lumma’s. At the same time, Tenzor also contained very specific strings found exclusively in Remus but not in Lumma, such as` B9%????4rnO/@NQe?Nx*` , used as a wildcard mask in the ABE-bypass (which we will discuss later), firmly linking it to both sides.


Figure 5: Comparison of selected decrypted strings across Lumma, Tenzor, and Remus.


On top of that, among the decrypted Tenzor strings, we also found a Steam dead drop resolver` hxxps\[://\]steamcommunity\[.\]com/profiles/76561199861614181` , which turned out to be the exact same resolver present in multiple confirmed Lumma samples, namely:


- 002f714f93bed53f165129a820c2d5b72227f1cafac43be19e5e223ce219a5e1 (Lumma)
- 066c4ab954fc1270ee62c0d7c582c4c691e58e0ffef0c654bc204a46e440d16d (Lumma)
- 0683f353cf3e101f721f1658e2a554ff7888ff9f2c32e23ceb3d23876864a264 (Lumma)
- 0580ebf601989457f0708799b431fd4d9f5e59d98838282d72936099aa6636da (Tenzor)


Figure 6: Decrypted Steam dead drop resolver in Tenzor. Reference sample: 0580ebf601989457f0708799b431fd4d9f5e59d98838282d72936099aa6636da.


Figure 7: Decrypted Steam dead drop resolver in Lumma. Reference sample: 0683f353cf3e101f721f1658e2a554ff7888ff9f2c32e23ceb3d23876864a264.


### Application-bound encryption bypass


Another very strong indicator tying Remus and Lumma together is the way they bypass Application-Bound Encryption (ABE), as both employ a highly specific technique that, until now, we have only observed and have been actively tracking in Lumma Stealer.


The technique involves injecting a very short shellcode (less than 100 bytes) into the browser process to decrypt the` v20_master_key` . However, unlike most techniques relying on injection, Lumma as well as Remus do not use this shellcode to call` IElevator::Decrypt` with the key obtained from the` Local State` file (the` \[“os_crypt”\]\[“app_bound_encrypted_key”\]` JSON field). Instead, it locates the` v20_master_key` directly in the browser’s process memory, where it is stored in an encrypted form protected by` CryptProtectMemory` with the` CRYPTPROTECTMEMORY_SAME_PROCESS` flag. It then calls the complementary` CryptUnprotectMemory` with the same flag from within the browser’s context to decrypt it.


It is important to note that this in-memory encrypted form of the` v20_master_key` is protected with a **different key** than the one used to protect the key on disk – it is a separate protection layer that Chromium browsers use to safeguard the key while it resides in memory at runtime. However, since the key is protected with the` CRYPTPROTECTMEMORY_SAME_PROCESS` flag, decryption can only be performed by the same process that encrypted it, which is precisely why injection into the browser process is necessary in this specific bypass. For the curious, we discussed this topic in more detail in our recent[VoidStealer blog post](https://www.gendigital.com/blog/insights/research/voidstealer-abe-bypass) .


From this point on, we will walk through the ABE bypass as implemented in Remus, noting any differences from Lumma’s implementation where applicable. Where no differences are explicitly mentioned, the two implementations work either identically or very similarly.


So how does Remus find the protected` v20_master_key` ? It begins by walking the browser’s PEB module list, looking for two modules in a single pass:` dpapi.dll` and the browser DLL (e.g.,` chrome.dll` ).


When` dpapi.dll` is found, Remus manually parses its PE export directory, reading the DOS header, PE header, and export table through repeated` NtReadVirtualMemory` syscalls. For each exported function name, it computes a seeded CRC32 hash and compares it against the pre-computed hash corresponding to` CryptUnprotectMemory` . Essentially, a typical API-hashing, but in a remote process. Once a match is found, it resolves the function’s RVA to obtain its absolute address in the browser’s address space.


When the browser DLL is found during the same pass, Remus decrypts a hex pattern` 488d058bcc8d02488901488b024889415b488d41` , which was encrypted by the same string obfuscation technique described earlier and stores the value` 0xEFF87` (which corresponds to` 11101111111110000111` in binary) into a variable we labeled as` wildcard_mask` (see Figure 8).


Figure 8: Remus decrypting the hex pattern used in the ABE bypass. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69.


The hex pattern corresponds to a sequence of opcodes that Remus searches for within the browser DLL (e.g.,` chrome.dll` ), while the wildcard mask, read from the least significant bit, indicates which bytes must match exactly and which should be treated as wildcards. Lumma does the same but takes a different approach to wildcarding. Instead of a hex mask, it uses a 20-character string` B9%????4rnO/@NQe?Nx*` , where ? denotes a wildcard and any other character means the byte must match exactly. Notably, this exact string also appears in Tenzor builds, which directly ties Tenzor to the Lumma codebase.


Figure 9: Visualization of the ABE-bypass pattern wildcarding (for Remus).


Looking into` chrome.dll` , we can see that the pattern is designed to locate a` LEA` (Load Effective Address) instruction that loads a 32-bit displacement pointing to the` os_crypt_async::Encryptor` virtual function table (vftable). The reason for specifically targeting the` os_crypt_async::Encryptor` class is that it holds the protected` v20_master_key` .


Figure 10: The opcode pattern that Remus searches for within chrome.dll (the disassembly is from chrome.dll).


Once the pattern is found, Remus reads the 32-bit displacement at` pattern_address + 3` and computes the absolute address of the` os_crypt_async::Encryptor` vftable using the formula:` target_addr = pattern_addr + disp32 + 7` (since the` LEA` instruction is 7 bytes long and the displacement is relative to the next instruction). It then scans the browser’s memory for this 8-byte vftable pointer by enumerating committed, readable memory regions via` NtQueryVirtualMemory` and reading each one into a local buffer via` NtReadVirtualMemory` . Wherever a match is found, it marks the start of an` os_crypt_async::Encryptor` instance in memory, as the vftable pointer sits at the very beginning of the object. From there, extracting the protected` v20_master_key` is just a matter of walking the structure at known offsets.


Figure 11: Remus finding the pattern in the browser process. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69.


After resolving all the necessary addresses, Remus allocates a buffer in the browser process via` NtAllocateVirtualMemory` , which serves as both the copy destination and the in-place decryption target. It then constructs a 51-byte shellcode (shown in Figure 12), allocates a second buffer with the` PAGE_EXECUTE_READWRITE` protection constant for the shellcode itself, and writes it into the browser process via` NtWriteVirtualMemory` . The shellcode is then executed via` NtCreateThreadEx` , and Remus waits for its completion using` NtWaitForSingleObject` . Once the` v20_master_key` is decrypted, Remus simply reads it from the buffer it previously allocated (and, therefore, already knows its address) via` NtReadVirtualMemory` .


Figure 12: The shellcode skeleton that Remus constructs for injection into the browser process(es).


The injected shellcode differs slightly between Remus and Lumma, but both fundamentally do the same thing – copy the protected` v20_master_key` into a pre-allocated buffer and jump to` CryptUnprotectMemory` to decrypt it in place. Both construct the shellcode on the stack, patching in the resolved addresses before writing it into the browser process. The only real difference is that Remus produces a more compact variant (51 vs 62 bytes) by reusing registers. Concrete examples of the injected shellcodes are shown in Figure 13 (Remus) and Figure 14 (Lumma).


Figure 13: Example of Remus’s shellcode injected into Chrome. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69.


Figure 14: Example of Lumma’s shellcode injected into Chrome. Reference sample: b037fa1dd769891b538d9ca26131890c93e3458eec96c5354bdebe50d04a5b3d.


Another detail linking the two is that when no browser process is already running, or when injection into an existing one fails, both Remus and Lumma spawn a new one on a separate, hidden desktop, preventing any visible windows from appearing on the user’s screen. They first attempt to open an existing desktop using` OpenDesktopW` and fall back to creating a new one via` CreateDesktopW` if it does not yet exist. A new browser instance is then launched via` CreateProcessW` with` STARTUPINFOW.lpDesktop` set to this desktop. The only difference is that Lumma uses a hardcoded desktop name` ChromiumDev` (or previously also` ChromeBuildTools` ), while Remus generates a random 16-character alphanumeric string using a Mersenne Twister PRNG, a natural evolution rather than a change in approach.


Furthermore, both Remus and Lumma also employ SYSTEM token impersonation as an alternative method to bypass ABE. Interestingly, the two families differ in which method they prefer: Remus attempts SYSTEM elevation first and falls back to shellcode injection, while Lumma tries injection first and resorts to SYSTEM elevation only if it fails. That said, SYSTEM token impersonation for ABE bypass is a well-known technique widely used across many stealer families. What, however, is highly distinctive is the injection-based bypass described above, which to our knowledge has only been used by Lumma and now also Remus.


### AntiVM CPUID checks


Another identical technique shared by Remus and Lumma is their anti-VM check based on the` cpuid` instruction with` EAX` set to` 0x40000000` – the hypervisor vendor identification leaf. When executed inside a virtual machine, the processor returns the hypervisor’s vendor signature in` EBX:ECX:EDX` . Both Remus and Lumma specifically check the` ECX` portion (4 bytes) against five known hypervisor signatures: KVM (` KVMKVMKVM` ), QEMU/TCG (` TCGTCGTCGTCG` ), VMware (` VMwareVMware` ), VirtualBox (` VBoxVBoxVBox` ), and Xen (` XenVMMXenVMM` ). Notably, both check **the same substrings** of hypervisor names **in the same order** , and the checked substrings are obfuscated.


The decompiled code performing the check can be seen in Figure 15 (Remus) and Figure 16 (Lumma). For clarity, we omitted the decryption loops from the Lumma listing, but they are the same decryption loops we described earlier.


Figure 15: Remus’s AntiVM cpuid checks. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69.


Figure 16: Lumma’s AntiVM cpuid checks. Reference sample: 8b6b238ffa6e411229c6754ba99f7b990c49edfb2c34068ce0ac5564824d71ad.


### Crypter check


When executed without a protective layer, both Remus and Lumma display a warning dialog before proceeding with their malicious payloads (see Figures 17 and 18). The purpose of this mechanism is twofold: to discourage distributors from spreading the raw, unprotected executables, which are more easily detected by security products, and to prevent less skilled affiliates from accidentally infecting their own machines.


Figure 17: Lumma’s warning dialog triggered when executed without a protective layer.


Figure 18: Remus’s error dialog when executed without a protective layer.


This type of check is relatively rare. So far, only a handful of families have been observed employing it – namely,[Lumma](https://outpost24.com/blog/lummac2-anti-sandbox-technique-trigonometry-human-detection/) ,[Rhadamanthys](https://research.checkpoint.com/2025/rhadamanthys-0-9-x-walk-through-the-updates/) , Remus, and[more recently also AuraStealer](https://www.gendigital.com/blog/insights/research/defeating-aurastealer-obfuscation) , albeit AuraStealer takes a slightly different approach (rather than simply displaying a warning and waiting for the user to click` Yes` or` OK` , it additionally requires the user to enter a randomly generated code shown in the dialog).


However, despite the surface-level similarity, the underlying implementations differ.[Rhadamanthys displays its warning dialog](https://research.checkpoint.com/2025/rhadamanthys-0-9-x-walk-through-the-updates/) using` MessageBoxW` , whereas both Remus and Lumma invoke it through a direct` NtRaiseHardError` syscall.


### Direct syscalls/sysenters


Both Remus and Lumma make use of direct syscalls/sysenters. While their implementations differ in some details, mostly as a natural consequence of the 32-bit vs 64-bit architecture, the overall design is strikingly similar.


One of the first things both do upon execution is that they enumerate all` Nt` -prefixed exports from` ntdll.dll` and build a lookup table mapping their name hashes to the corresponding Syscall Service Numbers (SSNs). The process is the same in both cases: they walk the` ntdll.dll` export directory, hash each matching export name, and scan the first 32 bytes of each function’s prologue to extract the SSN from the` mov eax, <SSN>` instruction. The extracted SSN is then stored alongside the export’s name hash in a hash-to-SSN lookup table.


Figure 19: Beginning of the hash-to-SSN table constructed at runtime (built by both Remus and Lumma).


To invoke a specific syscall/sysenter, both perform a linear scan of the lookup table until they find an entry whose hash matches the requested function. The SSN from the matching entry is then passed to a central dispatcher, which we refer to as` lumma_sysenter` and` remus_syscall` . The calling conventions of those dispatcher functions are nearly identical: the first argument is the SSN, followed by the argument count and the variadic arguments themselves. The only difference is that Lumma expresses the argument count in bytes (` number_of_arguments × 4` ), whereas Remus passes the actual count directly.


Figure 20: Example of Remus’s syscall invocation. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69.


Figure 21: Example of Lumma’s sysenter invocation. Reference sample: 8b6b238ffa6e411229c6754ba99f7b990c49edfb2c34068ce0ac5564824d71ad.


The dispatchers themselves look different at first glance, but this is mostly a consequence of ABI differences between 32-bit and 64-bit code. The core logic is the same: arrange the syscall/sysenter arguments and invoke the kernel. Remus does so directly via the` syscall` instruction. Lumma, on the other hand, resolves a dispatcher address during the hash-to-SSN initialization through a fallback chain: it first attempts to resolve the` Wow64Transition` export from` ntdll.dll` using API-hashing, then tries the` TEB->WOW32Reserved` , and finally, if that value is` NULL` , falls back to a hardcoded` sysenter` stub. Each of these fallback mechanisms ultimately achieves the same outcome – a transition from user-mode 32-bit code into the kernel.


Figure 22: Remus’s syscall dispatcher. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69.


Figure 23: Lumma’s sysenter dispatcher. Reference sample: 8b6b238ffa6e411229c6754ba99f7b990c49edfb2c34068ce0ac5564824d71ad.


Separately from the 32-bit` sysenter` dispatch, Lumma also employs Heaven’s Gate, a technique that allows 64-bit code to be executed from within a 32-bit process on a 64-bit operating system. Lumma uses it specifically when interacting with 64-bit browsers (during the ABE bypass) to call native 64-bit` ntdll.dll` functions that have no NtWow64 equivalents – namely` NtCreateThreadEx` to start the injected thread,` NtFreeVirtualMemory` for clean up, and` NtQueryVirtualMemory` to query the browser’s memory layout. The remaining cross-process operations, such as memory reads, memory allocations, and shellcode writes, are handled through standard NtWow64 sysenter calls (` NtWow64ReadVirtualMemory64` ,` NtWow64AllocateVirtualMemory64` , and` NtWow64WriteVirtualMemory64` ). Since Heaven’s Gate is inherently a WoW64 technique applicable only to 32-bit processes running on 64-bit systems, and Remus is a 64-bit binary, it has no equivalent. Remus simply uses direct syscalls for all these operations.


### Shared code patterns


Another striking similarity is the layout of many functions, which shows a remarkable resemblance and, in some cases, is nearly identical (if we disregard the obfuscation and differences caused by 32-bit vs 64-bit code). While there are many such functions, we highlight two concrete examples. The first is the heap allocation wrapper, which Remus and Lumma implement in a virtually identical manner, as illustrated in Figures 24 and 25. Furthermore, the same holds for other memory helpers, such as the reallocation and deallocation wrappers.


Figure 24: Remus’s heap allocation wrapper function. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69.


Figure 25: Lumma’s heap allocation wrapper function. Reference sample: 8b6b238ffa6e411229c6754ba99f7b990c49edfb2c34068ce0ac5564824d71ad.


The second example is the clipboard-stealing routine (Figures 26 and 27). Both implementations follow the same sequence: they open the clipboard, retrieve its contents, convert it from UTF-16 to UTF-8, decrypt the output filename` Clipboard.txt` using a stack-based decryption loop, and finally append the result to the exfiltration archive. The only difference is that Remus wraps each API call in a thin stub, which, however, might just be a result of different compiler optimization.


Figure 26: Remus’s clipboard stealing function. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69.


Figure 27: Lumma’s clipboard stealing function. Reference sample: 8b6b238ffa6e411229c6754ba99f7b990c49edfb2c34068ce0ac5564824d71ad.


Notably, although the vast majority of API calls are obfuscated through API hashing, the clipboard functions` OpenClipboard` ,` GetClipboardData` , and` CloseClipboard` are among the very few unobfuscated imports in both binaries.


Equally notable is the entry point structure. Both Remus and Lumma begin executing their malicious logic directly from the PE entry point (the` start` function), with no runtime initialization whatsoever. In a typical C/C++ binary, the entry point calls the CRT startup routine, which sets up the heap, initializes global variables, registers exception handlers, processes the command line, and invokes static constructors before eventually calling` main` . However, neither Remus nor Lumma follows this pattern. Instead, both jump directly from the entry point into their core logic: resolving modules and APIs, performing anti-analysis checks, initializing C2 communication, and exfiltrating data, ending with a direct call to` ExitProcess(0)` . This suggests both were compiled without linking to the standard C runtime, likely a deliberate choice to minimize the binary size and reduce unnecessary dependencies.


The structural similarities extend further. Both binaries share the same section layout, with each section serving an identical semantic role. Even the C2 configuration follows the same pattern: in both cases, the encrypted blob, key, and nonce are statically embedded in the` .rdata` section and decrypted at runtime using ChaCha20.


To summarize, we strongly believe the degree of similarity between the two codebases is too deliberate to be a coincidence, pointing to a single, shared origin.


### Indirect control flow obfuscation


Finally, both Remus and Lumma make use of control flow obfuscation by replacing direct jumps with indirect ones, where the target is read from an offset stored in the` .data` section. As shown in Figure 28, this technique takes several forms. In its simplest case, the target address is resolved by a single pointer dereference (highlighted in yellow). In more complex cases, it extends to a full jump table, where a computed index selects among several target addresses stored consecutively in the` .data` section (highlighted in orange). Lastly, in some cases, the target address is first loaded into a register (highlighted in blue) and then jumped to via a register-based indirect jump (highlighted in pink).


Figure 28: Randomly selected disassembly code from Remus (left) and Lumma (right) highlighting the different forms of the discussed indirect control flow obfuscation.


Although functionally equivalent to normal jumps, the indirection complicates the control flow analysis in two ways. First, some of the indirect jump targets cannot be statically determined (for example, when the register holding the target address can be set in multiple different preceding basic blocks, each potentially loading a different target), leaving the control flow graph incomplete as the connections between basic blocks are lost. Second, some of these orphaned blocks end up being misidentified as standalone functions, further disrupting function boundary detection and leading to a broken, incomplete decompiler output. Notably, Figure 28 also provides another glimpse of the` nop` paddings discussed earlier.


Figure 29: A portion of the .data section in Remus (left) and Lumma (right) showing the consecutively stored target addresses referenced by the indirect jumps and jump tables.


Both binaries contain hundreds of these indirect dispatch points, and once again, if we set aside the minor differences arising from the 32-bit vs. 64-bit differences, the obfuscation is virtually identical. Although removing the obfuscation entirely requires more effort, the IDA’s decompiler output can be significantly improved by marking the referenced offsets as constants. For those interested, we covered this trick in our[AuraStealer blog post](https://www.gendigital.com/blog/insights/research/defeating-aurastealer-obfuscation) , along with other approaches for tackling control flow obfuscation.


## What’s new in Remus


Having covered the key similarities between Remus and Lumma, let’s now focus on what is new in Remus. The core architecture is clearly inherited from Lumma. Still, several changes go beyond a straightforward 32-bit-to-64-bit port. These range from refactored strings and more thorough device fingerprinting to a different exfiltration archive format, a switch from FNV-1a to CRC32 for API hashing, and an overhauled C2 communication that has been restructured to deliver dynamic configurations incrementally rather than in a single large blob.


The most significant changes, however, are the replacement of traditional Steam/Telegram dead drop resolvers with blockchain-based C2 resolution using EtherHiding, a persistence technique allowing attackers to store malware-related data (such as domains, payloads, encryption keys, among others) on blockchain as a smart contract, and the introduction of additional anti-analysis checks.


### Dead drop resolvers via EtherHiding


Both Remus and Lumma employ dead drop resolvers, a mechanism in which the malware does not contact its C2 server directly but instead retrieves the C2 address at runtime from an intermediary hosted on a legitimate platform. This makes the infrastructure significantly more resilient, as if a C2 domain is taken down, the dead drop can be adjusted to point to a new server without the need to distribute an updated binary. Moreover, the dead drop URLs typically reside on well-known, high-reputation services, making them more difficult for security vendors to block without collateral damage.


Where the two differ is in the choice of platform. Lumma relies on Steam profiles and Telegram channels, typically encoding C2 URLs using ROT-15, while Remus goes a step further, replacing these with Ethereum smart contracts. At runtime, it sends an` eth_call` JSON-RPC request to a hardcoded contract address via a public RPC endpoint and extracts the C2 URL from the hex-encoded response. Because blockchain data is decentralized and immutable, there is no platform operator to report abuse to, making the dead-drop effectively immune to takedowns.


Just like Lumma’s Steam profile URL, Remus stores the smart contract address separately from the static C2 configuration, as a stack-encrypted string. Notably, after the JSON response is retrieved, the C2 address is, at least for now, merely hex-encoded in the` result` field (no other obfuscation, such as ROT-15 in case of Lumma, is applied).


Figure 30: Remus resolving a C2 using EtherHiding. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69 (Remus).


Figure 31: Remus’s EtherHiding C2-resolution visualization. The Python script is only a helper to illustrate what happens under the hood – in practice, Remus sends an HTTP POST request.


### Additional anti-analysis checks


Remus introduces two additional anti-analysis mechanisms, both evaluated early during startup (before connecting to its C2). If either check fails, the malware silently terminates via` ExitProcess(0)` .


The first check targets sandbox and analysis tool DLLs. Remus walks the PEB's` InLoadOrderModuleList` , hashes the name of every loaded module using a CRC32 variant with a custom initialization constant, and compares the result against 11 pre-computed hashes stored in its` .rdata` section, each corresponding to a module commonly injected by analysis environments, including Avast sandbox (` snxhk.dll` ), Sandboxie (` sbiedll.dll` ), Comodo sandbox (` cmdvrt32.dll` ,` cmdvrt64.dll` ), and several others.


Figure 32: Remus’s CRC32-hashed array of forbidden DLLs. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69.


Figure 33: Remus’s anti-analysis check for forbidden DLLs. Reference sample: 64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69.


If no sandbox DLLs are detected, Remus proceeds by expanding the path` %UserProfile%\\Documents\\Outlook Files` , enumerating all` *.pst` files in that directory, and checking for the presence of` honey@pot.com.pst` , which would be treated as a sandbox environment.


## Summary


In this analysis, we described our findings regarding a new x64 variant of Lumma Stealer, which we call Remus. We focused on various similarities and differences between Lumma and Remus, thoroughly describing the technical aspects of both. We also provided a timeline, referencing the very first test builds of Remus (historically called Tenzor), which correlate with the extensive doxxing of Lumma authors from late August to October 2025.


The key aspects that allowed us to attribute Remus as a new x64 version of Lumma are the use of the same string obfuscation technique, AntiVM checks, direct syscall/sysenter handling, indirect control flow obfuscation, and, most importantly, an almost identical approach to bypassing AppBound Encryption, which we’ve only seen used by Lumma to date.


Remus, however, is not merely an x64 port of Lumma. It also introduces a couple of novel techniques in how it operates, including the use of EtherHiding instead of a traditional approach of using Steam/Telegram as dead drop resolvers, as well as additional anti-analysis checks to evade a wider range of security vendors.


## Indicators of Compromise (IoCs)


A complete IoC list is available on our[GitHub](https://github.com/gendigitalinc/ioc/tree/master/Remus) .


### C2 domains


hxxp\[://\]217\[.\]156\[.\]122\[.\]12:80
hxxp\[://\]217\[.\]156\[.\]122\[.\]57:80
hxxp\[://\]217\[.\]156\[.\]122\[.\]75:1378
hxxp\[://\]45\[.\]151\[.\]106\[.\]110:80
hxxp\[://\]80\[.\]97\[.\]160\[.\]155:80
hxxp\[://\]86\[.\]107\[.\]168\[.\]103:80
hxxp\[://\]94\[.\]231\[.\]205\[.\]229:28313
hxxp\[://\]adveryx\[.\]biz:6573
hxxp\[://\]backbou\[.\]biz:5902
hxxp\[://\]baxe\[.\]pics
hxxp\[://\]baxe\[.\]pics:48261
hxxp\[://\]borscer\[.\]biz:9592
hxxp\[://\]buccstanor\[.\]pics
hxxp\[://\]buccstanor\[.\]pics:28313
hxxp\[://\]buccstanor\[.\]pics:48261
hxxp\[://\]chalx\[.\]live:5902
hxxp\[://\]chromap\[.\]biz:4219
hxxp\[://\]coox\[.\]live:28313
hxxp\[://\]drymoge\[.\]biz:4192
hxxp\[://\]forestoaker\[.\]com:6290
hxxp\[://\]gluckcreek\[.\]online:48261
hxxp\[://\]intem\[.\]lat:9592
hxxp\[://\]interxo\[.\]biz:7481
hxxp\[://\]josegza\[.\]biz:8521
hxxp\[://\]krondez\[.\]com:28982
hxxp\[://\]lazzo\[.\]bet:3989
hxxp\[://\]managew\[.\]biz:5902
hxxp\[://\]navelum\[.\]biz:3201
hxxp\[://\]nitroca\[.\]biz:6782
hxxp\[://\]outcrol\[.\]biz:4895
hxxp\[://\]padaz\[.\]pics:4219
hxxp\[://\]parky\[.\]pics:3989
hxxp\[://\]prickaz\[.\]biz:2039
hxxp\[://\]remnane\[.\]biz:5692
hxxp\[://\]ropea\[.\]top:28313
hxxp\[://\]siltsoh\[.\]biz:7481
hxxp\[://\]texakgi\[.\]cloud:3849
hxxp\[://\]vinte\[.\]online
hxxp\[://\]vinte\[.\]online:28313
hxxp\[://\]woodena\[.\]biz:7821
hxxp\[://\]zadno\[.\]run:4219
hxxps\[://\]cheekiez\[.\]biz
hxxps\[://\]nobleckly\[.\]biz


### Remus SHA-256 (Non-exhaustive)


0a8f734f10400f7ae8fef591147e78dab6350089683be84c1cb6c82113cb1319
25e74a76f2f3601abcb20fd743a7e3cf3befd5a3838c7501af5d87d293233809
4428c3ffe2532f162f31d7573bbc1cca2299195421da3d8e8a3e535e9fc42b08
484e3ab5d425a97819f01dcc330e005dc444c51625bfdcd7ea9a3954018d1fc9
64db10e76b46be8db36e02993d36559bc3f86606c9ea955731872b716c8f0c69
788b56e9be2f1dd6a977dce0265f293ab42d3e8ffb287ab584e169fbf115da1f
8653d7158486aa10fc0078c3ca9318cd7ace05d4b3e6f3b1fb84ffb7a6a339ec
a4f111e5425690fcd384c62ecb5b57b0f645925572af3541748e01d810cd2b40
ab2e47720388fa201e242552f8d8b82363c6c52f6c63fa3fec9dce027cb12e77
bc11d036fe59abb3915f736307c56d2fd43e8127e46c31f926eeda864f4d66dc
c3f7cea80dbafaa90a88b28a6dfb1227caaf5c2a29f0ce06bf663d6ed2cfc079


### Tenzor SHA-256


0580ebf601989457f0708799b431fd4d9f5e59d98838282d72936099aa6636da
7a25253e6d8d9ccf62a67f8014cacb301daf9e40f1b68ecc7f354d6896d16960
cab7855ccfca19a06eea76e0e170f592dcc95906ecfa5436f5a11947e04e63d5
dfbeab30d14df9104a95de83ab4690308c653eb0c3706554687c45a77adc1385


More on this topic


[Defeating AuraStealer: Practical Deobfuscation Workflows for Modern Infostealers – From Research](https://www.gendigital.com/blog/insights/research/defeating-aurastealer-obfuscation)


[Alliances of convenience: How APTs are beginning to work together – From Research](https://www.gendigital.com/blog/insights/research/apt-cyber-alliances-2025)


[DPRK’s Playbook: Kimsuky’s HttpTroy and Lazarus’s New BLINDINGCAN Variant – From Research](https://www.gendigital.com/blog/insights/research/dprk-kimsuky-lazarus-analysis)


[Vojtěch Krejsa](https://www.gendigital.com/blog/authors/vojtech-krejsa)


Threat Researcher at Gen


[Jan Rubín](https://www.gendigital.com/blog/authors/jan-rubin)


Threat Research Team Lead


Follow us for more
