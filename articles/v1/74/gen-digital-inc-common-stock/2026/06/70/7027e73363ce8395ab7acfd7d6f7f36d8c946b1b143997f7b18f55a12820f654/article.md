---
schema_version: "1.0.0"
document_id: "7027e73363ce8395ab7acfd7d6f7f36d8c946b1b143997f7b18f55a12820f654"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-rss-b2c253ea0afd"
canonical_url: "https://www.gendigital.com/blog/insights/research/inside-vidar-abe-bypass"
published_at: "2026-06-18T21:16:37+00:00"
first_seen_at: "2026-07-20T03:32:39.128259+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:c666f639c9c6c217480b28b2b9648f732c179c127f0b351e6a1c30c09518521d"
---

# Inside Vidar’s ABE Bypass: From Memory Scanning to APC Injections

By[Vojtěch Krejsa](https://www.gendigital.com/blog/authors/vojtech-krejsa)


## Introduction


Infostealers are constantly evolving, and so are the techniques they use to bypass[Application-Bound Encryption (ABE)](https://security.googleblog.com/2024/07/improving-security-of-chrome-cookies-on.html) . In recent weeks, Vidar has been among the most actively developed stealers and, apart from[multiple updates to its string obfuscation](https://x.com/GenThreatLabs/status/2064836539952361976) and[a reworked approach to protecting its configuration](https://x.com/GenThreatLabs/status/2061462524604649840) , it has also introduced a novel technique for bypassing ABE. And while there have been many other changes in Vidar lately, with new versions dropping every week, in this blog post we focus solely on the ABE bypass and its technical aspects.


## The ABE Bypass


From a high-level perspective, Vidar’s approach to bypassing ABE is somewhat similar to that of Remus/Lumma, in that both try to extract the` v20_master_key` directly from the browser’s memory. What sets them apart, however, is how they achieve it. Since we’ve already covered the inner workings of ABE in earlier posts ([Remus](https://www.gendigital.com/blog/insights/research/remus-64bit-variant-of-lumma-stealer) ,[VoidStealer](https://www.gendigital.com/blog/insights/research/voidstealer-abe-bypass) ), we don’t revisit them here and focus instead on the bypass itself. For readers interested in the broader background, we encourage checking those earlier posts.


In short, the end goal of infostealers is to obtain the` v20_master_key` , as it alone is sufficient for decrypting any ABE-protected data tied to a specific application. This key can be obtained in one of two ways: either from disk, where it is protected by two layers of` CryptProtectData` (one applied in the context of the logged-in user and one as` NT AUTHORITY\\SYSTEM` ), or from the browser’s memory – the direction Vidar took.


In memory, however, the key is kept protected with` CryptProtectMemory` using the` CRYPTPROTECTMEMORY_SAME_PROCESS` flag. This ensures that even if an attacker manages to read the encrypted form of the key from memory, they cannot simply decrypt it (they would need to invoke the complementary` CryptUnprotectMemory` function directly from within the browser process). That leaves attackers with two problems to solve: how to find the encrypted key in memory, and how to get it decrypted.


## Locating the v20_master_key in Browser’s Memory


Vidar’s approach to locating the encrypted` v20_master_key` in memory involves scanning the entire memory of a running browser for a specific pattern. But that first requires a browser process to scan.


When the target browser is already running, Vidar reuses the existing process. However, it doesn’t read its memory directly. Instead, it creates a fork of it using` NtCreateProcessEx` with` SectionHandle` set to` NULL` , instructing the kernel to clone the address space of the process specified by the` ParentProcess` as a copy-on-write mapping. The resulting process has no threads and is never resumed – it exists purely as a static memory snapshot.


To perform the fork, Vidar first opens a handle to the target browser via` OpenProcess` with` PROCESS_QUERY_INFORMATION | PROCESS_CREATE_PROCESS | PROCESS_VM_READ` , falling back to` PROCESS_CREATE_PROCESS` if the initial call fails. It then passes this handle as the` ParentProcess` argument to` NtCreateProcessEx` , requesting` PROCESS_ALL_ACCESS` on the resulting child. If the fork fails, Vidar falls back to scanning the live browser process directly.


Figure 1: Vidar forking a browser process.


If no target browser process is found running, Vidar creates a new isolated desktop via` CreateDesktopA` , assigns it a name of the form` v20_%d` (where` %d` is replaced with the browser’s index in Vidar’s configuration), and launches the target browser in there. In the case of Vidar version 2.1, the browser is launched with the command line` --no-first-run --disable-gpu about:blank` , although the exact command line varies across versions. Since modern browsers use a multi-process architecture, there are typically multiple processes matching a given browser. Vidar enumerates all of them (up to 64) and applies the fork-and-scan procedure to each one independently.


Figure 2: Vidar launching a browser in a new desktop.


With a handle to the forked process in hand, Vidar proceeds to enumerate its virtual memory regions using` NtQueryVirtualMemory` . It iterates through the entire browser’s address space, collecting regions that match a specific profile: committed (` MEM_COMMIT` ), private (` MEM_PRIVATE` ), and either readable (` PAGE_READONLY` ) or read-write (` PAGE_READWRITE` ). Vidar records up to 4096 memory regions matching these requirements, along with their base addresses and sizes. **** The actual pattern scanning is then performed in parallel with up to 64 worker threads, depending on the system.


Figure 3: Vidar enumerating memory regions and collecting the promising ones.


Each thread scans its assigned regions using` NtReadVirtualMemory` , searching for a 32-byte pattern. When a match is found, the thread reads 8 bytes at offset +32 from the match, interprets them as a pointer, and validates that it points to committed, private, and readable memory (the expected memory state for the encrypted` v20_master_key` ).


Figure 4: Vidar scanning a memory region for a 32-byte pattern and validating it points to committed, private, and readable memory.


This naturally raises the question of what pattern Vidar searches for. The answer is a 32-byte signature:` 76 32 30 00 ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? ?? 03 00 00 00 00 01 ?? ?? ??` , where` ??` denotes a wildcard (Vidar uses the byte` AA` as the wildcard marker in its internal representation). Notably, the first four bytes correspond to the ASCII` v20\\x00` , which is a common prefix used for ABE-related data.


Figure 5: Vidar building the scan pattern.


We assess with high confidence that this pattern is intended to match a node structure within Chromium’s` Encryptor::KeyRing` , the structure that holds the keys used by the` Encryptor` class. Now, this part is admittedly a bit tricky as it relies on several assumptions we made about the internal memory layout, as well as on implementation-specific representations of various C++ standard library types` (std::map` ,` std::string` ,` std::vector` ,` std::optional` ), which is why we frame this as a high-confidence rather than a certainty.


Our reasoning is as follows:` KeyRing` is[defined](https://github.com/chromium/chromium/blob/cf5d502bcaedb22927e3829a18be1ff6d8b6f598/components/os_crypt/async/common/encryptor.h#L132) as` std::map</*tag=*/std::string, std::optional<Key>>` , and` std::map` is typically implemented as a red-black tree, where each tree node stores three pointers (left child, parent, and right child) followed by the key-value pair. In this case, the pair is in the form of` std::pair</*tag=*/std::string, std::optional<Key>>` , meaning that the` std::string` and the` Key` object might be laid out consecutively in memory within each node. For a` v20_master_key` entry, the tag corresponds to the string` v20` , which at 4 bytes including the null terminator fits within the small string optimization (SSO) buffer and is therefore stored inline rather than heap-allocated. The` Key` class that immediately follows[contains three member variables](https://github.com/chromium/chromium/blob/cf5d502bcaedb22927e3829a18be1ff6d8b6f598/components/os_crypt/async/common/encryptor.h#L93-L98) :` std::optional<mojom::Algorithm> algorithm_` ,` std::vector<uint8_t> key_` , and` bool encrypted_ = false` .


Putting it all together and applying it to a concrete example from a browser memory dump, we get the following layout:


Figure 6: An annotated browser memory dump of a single KeyRing node.


Please note that while the exact layout may contain minor inaccuracies on our part, the overall structure should be roughly correct. For easier orientation, the following diagram illustrates how the individual structures are nested within each other:


Figure 7: Visualization of how the structures are nested within a single KeyRing node.


At this point, it should be clear how the scan pattern maps onto the searched structure. The 32-byte match covers the` std::string` tag` v20\\x00` and the beginning of the` Key` object up to and including` algorithm_` . The pointer at offset +32 from the start of the match then lands precisely on` key_.begin` , the first member of` std::vector<uint8_t>` that holds the encrypted` v20_master_key` .


Matches from all threads are collected into a shared buffer and subjected to a majority-voting scheme: candidates consisting mostly of zeroes are discarded, the rest are pairwise-compared, and each is scored by the number of identical copies found. This voting is performed independently for each browser process, producing one best candidate per process.


## Decrypting the v20_master_key via APC Injections


For each browser process that produced a match, Vidar now holds a candidate address pointing to a potential encrypted` v20_master_key` . However, as already noted, since the key is protected with` CryptProtectMemory` using` CRYPTPROTECTMEMORY_SAME_PROCESS` , it can only be decrypted by invoking` CryptUnprotectMemory` from within the browser process itself. Vidar solves this by injecting an Asynchronous Procedure Call (APC) into the live browser process.


Vidar supports two APC injection methods, selected based on whether ESET or Bitdefender is detected on the system. If either of these products is present, Vidar uses the first method, which it refers to as “classic” (based on decrypted strings) – it creates a suspended thread in the target browser via` CreateRemoteThread` (with` NtTestAlert` as the thread start address and the` CREATE_SUSPENDED` flag), queues an APC using` NtQueueApcThread` , resumes the thread, and then waits for it to complete via` WaitForSingleObject` with a randomized timeout. When the thread resumes, it calls` NtTestAlert` , which flushes the APC queue and executes the injected routine.


Figure 8: Vidar’s APC method dispatcher.


Figure 9: Vidar’s “classic” APC injection.


If neither product is detected, it uses the “special” method – it finds an existing thread in the target browser using a combination of` CreateToolhelp32Snapshot` and` Thread32First/Thread32Next` , opens it via` NtOpenThread` , and calls` NtQueueApcThreadEx2` with the` QUEUE_USER_APC_FLAGS_SPECIAL_USER_APC` flag. Unlike regular APCs, special user APCs execute immediately. Therefore, the target thread does not need to be in an alertable wait state.


Figure 10: Vidar’s “special” APC injection.


In both cases, the APC routine to be executed is` CryptUnprotectMemory` , called with the following three arguments: the address of the encrypted` v20_master_key` candidate, its size (32 bytes), and` CRYPTPROTECTMEMORY_SAME_PROCESS` for the` flags` parameter.


Since` CryptUnprotectMemory` decrypts the data in place, once the APC executes, the key is decrypted directly in the live browser's memory. To read it, Vidar creates a second fork of the browser process and reads the key from the same address using` NtReadVirtualMemory` . It then compares the result against a snapshot of the encrypted key taken from the first fork before the APC call – if the data is unchanged, decryption failed, and Vidar moves on to the next browser process’s candidate.


If the data differs, Vidar uses the same second fork to perform an additional verification: it scans the forked process’s memory for the byte sequence` v20` and, treating each match as a potentially ABE-encrypted entry (cookie, password), attempts AES-256-GCM decryption with the key candidate using` BCryptDecrypt` (successful GCM tag verification confirms that the key is correct).


Whenever the data has changed (indicating that decryption of the key took place), Vidar re-encrypts the key in the browser’s memory by injecting another APC with` CryptProtectMemory` as the routine, preserving the browser’s state. If no candidate produced a successful decryption (i.e., the data remained unchanged after every APC call), Vidar takes a more aggressive approach: it terminates all browser instances, restarts the browser, and repeats the entire scan-decrypt-verify cycle.


By abusing APC injections instead of the more traditional approach of injecting code via` NtWriteVirtualMemory` that performs the decryption, Vidar trades one suspicious behavior for another. APC injections are arguably less common and may thus slip past some detection systems, though they are still well-known techniques monitored by AV and EDR solutions. But that is the nature of ABE bypasses – each has its own advantages and disadvantages, and that’s exactly why infostealers keep inventing new ones.


## Indicator of Compromise (IoC)


459daa809751e73f60fbbe4384a7d1653c36bb06945e4eb3635270924241100a (Vidar 2.1)


More on this topic


[Fake invoices are moving from inboxes to shopping apps – From Research](https://www.gendigital.com/blog/insights/research/fake-invoices-shopping-apps)


[Your flight was cancelled. Is the refund message real? – From Research](https://www.gendigital.com/blog/insights/research/flight-cancelled-refund-message-scam)


[Fake hiring pages abuse FIFA and other major brands to steal work credentials – From Research](https://www.gendigital.com/blog/insights/research/fake-hiring-pages-abuse-fifa)


[Vojtěch Krejsa](https://www.gendigital.com/blog/authors/vojtech-krejsa)


Threat Researcher at Gen


Follow us for more
