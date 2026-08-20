---
schema_version: "1.0.0"
document_id: "305017eac615ef0054158b037d2782e2a72d36e220181697e6b3cf262c362ccf"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/pt-metasploit-wrap-up-07-03-2026"
published_at: "2026-07-03T22:11:22+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:289bda9cf4de927682f4c6867c7bc591bdde5f378f35abf05d5087f0c2b22a91"
---

# Weekly Metasploit Update: Modules for SMB-to-Meterpreter, Peyara Remote Mouse RCE exploit, and more

## It's Time to Upgrade Your SMB Session


This week, Metasploit contributor Dean Welch has added an SMB to Meterpreter session upgrade module. It uses PsExec to facilitate the upgrade. Users can load the module with use windows/manage/smb_to_meterpreter


and specify the session number they wish to upgrade. This functionality is also available with the command sessions -u <session_id>


. This work is part of an overarching effort to enable a variety of session types to be upgraded to Meterpreter when possible.


## New module content (3)


### Peyara Remote Mouse 1.0.1 Unauthenticated Remote Code Execution


Author: tmrswrr


Type: Exploit


Pull request:[#21491](https://github.com/rapid7/metasploit-framework/pull/21491) contributed by[capture0x](https://github.com/capture0x)


Path: windows/misc/peyara_remote_mouse_rce


Description: Adds an exploit module for Peyara Remote Mouse v1.0.1 unauthenticated RCE.


### Linux Execute Command


Authors: bcoles[\[email protected\]](https://www.rapid7.com/cdn-cgi/l/email-protection#c4a6a7aba8a1b784a3a9a5ada8eaa7aba9) and modexp


Type: Payload (Single)


Pull request:[#21239](https://github.com/rapid7/metasploit-framework/pull/21239) contributed by[bcoles](https://github.com/bcoles)


Path: linux/loongarch64/exec


Description: Adds a new linux/loongarch64/exec command payload.


### SMB to Meterpreter Upgrade via PsExec


Author: Dean Welch


Type: Post


Pull request:[#21581](https://github.com/rapid7/metasploit-framework/pull/21581) contributed by[dwelch-r7](https://github.com/dwelch-r7)


Path: windows/manage/smb_to_meterpreter


Description: Adds the ability to upgrade authenticated SMB sessions to Meterpreter sessions using PsExec techniques.


## Enhancements and features (1)


- [#21527](https://github.com/rapid7/metasploit-framework/pull/21527) from[zeroSteiner](https://github.com/zeroSteiner) - Adds authentication support to the MCP server's HTTP transport by default.


## Bugs fixed (2)


- [#21618](https://github.com/rapid7/metasploit-framework/pull/21618) from[zeroSteiner](https://github.com/zeroSteiner) - Fixes a crash when running the scanner/discovery/udp_sweep


module on Windows environments.
- [#21624](https://github.com/rapid7/metasploit-framework/pull/21624) from[adfoster-r7](https://github.com/adfoster-r7) - Fixes a bug with SSH session's debug information showing the incorrect value localuser @


instead of ssh_user @ ssh_ip


.


## Documentation


You can find the latest Metasploit documentation on our docsite at[docs.metasploit.com](https://docs.metasploit.com/) .


## Get it


As always, you can update to the latest Metasploit Framework with msfupdate and you can get more details on the changes since the last blog post from GitHub:


- [Pull Requests 6.4.141...6.4.142](https://github.com/rapid7/metasploit-framework/pulls?q=is:pr+merged:%222026-06-24T23%3A18%3A10Z..2026-07-01T09%3A42%3A42Z%22)
- [Full diff 6.4.141...6.4.142](https://github.com/rapid7/metasploit-framework/compare/6.4.141...6.4.142)


If you are a git user, you can clone the[Metasploit Framework repo](https://github.com/rapid7/metasploit-framework) (master branch) for the latest. To install fresh without using git, you can use the open-source-only[Nightly Installers](https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers) or the commercial edition[Metasploit Pro](https://www.rapid7.com/products/metasploit/download/)
