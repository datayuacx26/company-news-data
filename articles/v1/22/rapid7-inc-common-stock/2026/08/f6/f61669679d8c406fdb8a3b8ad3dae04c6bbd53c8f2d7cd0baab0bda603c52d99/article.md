---
schema_version: "1.0.0"
document_id: "f61669679d8c406fdb8a3b8ad3dae04c6bbd53c8f2d7cd0baab0bda603c52d99"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/pt-metasploit-wrap-up-lot-of-summer-shells-and-fit-http-profiles"
published_at: "2026-08-14T21:27:45+00:00"
first_seen_at: "2026-08-14T23:14:31.969643+00:00"
fetched_at: "2026-08-14T23:14:33.373498+00:00"
content_hash: "sha256:d0268d1f551f5938d4e03251131594d15b7d4d8bd71dba9a6239f91020eb4b54"
---

# Metasploit Wrap Up: Lot of summer shells and fit http profiles

This wrap-up brings a full-on shell parade. Thirteen shiny new modules landed, starting with a buffet of RCEs. WordPress WP2Shell, Ghost CMS, Joomla JCE, Langflow, OpenCATS, Pterodactyl Panel, SonicWall SMA1000, Ray Dashboard, a Pix-for-WooCommerce, and for those who like their exploits closer to the bare-metal, the Fragnesia Linux kernel LPE (CVE-2026-46300). Metasploit also got the glow-up of the summer with the new http malleable profiles, MCP functionality and linux multi fetch payloads (more details on the \[official 6.5 release blog post\](https://www.rapid7.com/blog/post/pt-metasploit-framework-6-5-released/)!). Windows on ARM confirm to be the new first-class citizenship thanks to brand-new AArch64 reverse-TCP shells (both inline and staged), so your Snapdragon boxes can join the party too. Last but not least, an important message: *Nyan Nyan Nyan Nyan Nyan Nyan.*


## New module content (13)


### Ray Dashboard Logs API Path Traversal


Author: Richard Howe <rhowe425>


Type: Auxiliary


Pull request:


[#21681](https://github.com/rapid7/metasploit-framework/pull/21681) contributed by


[rmhowe425](https://github.com/rmhowe425)


Path: \`gather/ray_dashboard_logs_api_path_traversal\`


Description: This adds an auxiliary module that leverages a path traversal vulnerability in Ray to list the contents of local directories. There is currently no CVE assigned to this vulnerability. Issuance is pending with MITRE.


### Pterodactyl Panel CVE-2025-49132 Remote Code Execution


Authors: 0xtensho and jheysel-r7


Type: Exploit


Pull request:


[#21452](https://github.com/rapid7/metasploit-framework/pull/21452) contributed by


[jheysel-r7](https://github.com/jheysel-r7)


Path: \`linux/http/pterodactyl_locales_locale_json\`


AttackerKB reference:


[CVE-2025-49132](https://attackerkb.com/search?q=CVE-2025-49132&referrer=blog)


Description: This adds a module which exploits a vulnerability in Pterodactyl Panel before version 1.11.11 that allows unauthenticated remote code execution through improper handling of locale file operations. The vulnerability, CVE-2025-49132, exists in the locale.json endpoint which allows path traversal and arbitrary file creation. This combination of capabilities results in remote code execution in the context of the user running the web server.


### SonicWall SMA1000 WorkPlace wsproxy SSRF Remote Command Execution


Authors: Deral Heiland, Rapid7 Vulnerability Research, and Ryan Emmons


Type: Exploit


Pull request: #


[21678](https://github.com/rapid7/metasploit-framework/pull/21678) contributed by dheiland-r7(https://github.com/dheiland-r7)


Path: \`linux/http/sonicwall_sma1000_wsproxy_rce\`


AttackerKB reference:


[CVE-2026-15409](https://attackerkb.com/search?q=CVE-2026-15409&referrer=blog)


Description: This adds a new exploit module for CVE-2026-15409, a Server-Side Request Forgery (SSRF) vulnerability in the SonicWall SMA1000 WorkPlace wsproxy service.


### Fragnesia LPE (CVE-2026-46300)


Authors: William Bowling and msutovsky-r7


Type: Exploit


Pull request:[#21456](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21456) contributed by[msutovsky-r7](https://github.com/msutovsky-r7)


Path: linux/local/cve_2026_46300_fragnesia


AttackerKB reference:[CVE-2026-46300](https://www.google.com/search?q=https://attackerkb.com/search%3Fq%3DCVE-2026-46300%26referrer%3Dblog)


Description: This adds a local module for the Fragnesia exploit which is a page-cache replacement vulnerability in the Linux kernel's XFRM (IPsec) subsystem, tracked as CVE-2026-46300.


### Ghost CMS Remote Code Execution


Authors: Cristian-Alexandru Staicu and Maksim Rogov


Type: Exploit


Pull request:[#21234](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21234) contributed by[vognik](https://github.com/vognik)


Path: multi/http/ghostcms_auth_rce_cve_2026_29053


AttackerKB reference:[CVE-2026-22594](https://www.google.com/search?q=https://attackerkb.com/search%3Fq%3DCVE-2026-22594%26referrer%3Dblog)


Description: This adds an exploit module for Ghost CMS (CVE-2026-29053) that achieves remote code execution by uploading a malicious theme. Ghost's theme renderer evaluates untrusted JSONPath expressions through the {{#get}} helper, letting the module inject and trigger arbitrary code once a theme is uploaded and activated. You'll need valid admin or staff credentials to authenticate.


### Joomla Content Editor Unauthenticated File Upload RCE


Authors: David Jardin, Uwe Flottemesch, and ispyispyispy


Type: Exploit


Pull request:[#21615](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21615) contributed by[15py15py15py](https://www.google.com/search?q=https://github.com/15py15py15py)


Path: multi/http/joomla_com_jce_unauth_file_upload_rce


AttackerKB reference:[CVE-2026-48907](https://www.google.com/search?q=https://attackerkb.com/search%3Fq%3DCVE-2026-48907%26referrer%3Dblog)


Description: This adds a new exploit module for CVE-2026-48907, an unauthenticated arbitrary profile creation vulnerability in the JCE (Joomla Content Editor) extension for Joomla!. The profiles.import task fails to enforce authentication, letting an attacker import a crafted profile that is written to disk as a PHP web shell, resulting in remote code execution when the tmp/ directory is directly accessible. All JCE versions up to and including 2.9.99.4 are affected, and no credentials are required.


### Langflow Unauth RCE


Authors: Diamorphine and Richard Howe


Type: Exploit


Pull request:[#21700](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21700) contributed by[rmhowe425](https://github.com/rmhowe425)


Path: multi/http/langflow_unauth_rce_cve_2026_33017


AttackerKB reference:[CVE-2026-33017](https://www.google.com/search?q=https://attackerkb.com/search%3Fq%3DCVE-2026-33017%26referrer%3Dblog)


Description: Adds a new multi/http/langflow_unauth_rce_cve_2026_33017 exploit module that exploits an unauth RCE vulnerability in the /api/v1/build_public_tmp/{flow_id}/flow endpoint in Langflow versions prior to 1.9.0.


### OpenCATS Installer PHP Code Injection


Authors: Chocapikk and stlthr4k3r


Type: Exploit


Pull request:[#21630](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21630) contributed by[stlthr4k3r](https://github.com/stlthr4k3r)


Path: multi/http/opencats_installer_rce


AttackerKB reference:[CVE-2026-27760](https://www.google.com/search?q=https://attackerkb.com/search%3Fq%3DCVE-2026-27760%26referrer%3Dblog)


Description: Adds an exploit module targeting CVE-2026-27760, a PHP code injection in OpenCATS.


### WordPress WP2Shell REST API Batch Route Confusion SQLi to RCE


Authors: Adam Kues, Crypto-Cat, TF1T, dtro, and haongo


Type: Exploit


Pull request:[#21686](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21686) contributed by[Crypto-Cat](https://github.com/Crypto-Cat)


Path: multi/http/wp_batch_desync_rce


AttackerKB reference:[CVE-2026-60137](https://www.google.com/search?q=https://attackerkb.com/search%3Fq%3DCVE-2026-60137%26referrer%3Dblog)


Description: This adds an exploit module to target WP2Shell, an unauthenticated pre-auth remote code execution vulnerability affecting WordPress core versions 6.9.0–6.9.4 and 7.0.0–7.0.1. The module chains a REST API route confusion flaw (CVE-2026-63030) with an SQL injection (CVE-2026-60137) to elevate privileges, deploy a payload via a custom plugin, and execute a remote session.


### WordPress Unauthenticated RCE via Pix for WooCommerce plugin


Authors: Alexis Lafontaine and Maksim Rogov


Type: Exploit


Pull request:[#21683](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21683) contributed by[vognik](https://github.com/vognik)


Path: multi/http/wp_plugin_pix_unauth_rce_cve_2026_3891


AttackerKB reference:[CVE-2026-3891](https://www.google.com/search?q=https://attackerkb.com/search%3Fq%3DCVE-2026-3891%26referrer%3Dblog)


Description: Adds CVE-2026-3891 WordPress Unauthenticated RCE Exploit module targeting Pix for WooCommerce plugin.


### Release Metasploit 6.5


Authors: OJ Reeves, Spencer McIntyre


Type: Payload (Single) Pull request:[#21728](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21728) contributed by[zeroSteiner](https://github.com/zeroSteiner)


Description: Adds support for a new MALLEABLEC2 option to Meterpreter HTTP(S) payloads. This feature enables users to load a standard profile into Meterpreter and change the shape of its HTTP(S) traffic. All Meterpreters, including Windows, Java, Python, PHP and Linux, have been updated with this functionality.


### Windows AArch64 Command Shell, Reverse TCP Inline


Author: vinicius-batistella


Type: Payload (Single)


Pull request:[#21589](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21589) contributed by[vinicius-batistella](https://github.com/vinicius-batistella)


Path: windows/aarch64/shell_reverse_tcp


Description: Adds Windows on ARM (AArch64) reverse-TCP command-shell payload.


### Windows AArch64 Command Shell, Windows AArch64 Reverse TCP Stager


Author: vinicius-batistella


Type: Payload (Stager)


Pull request:[#21744](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21744) contributed by[vinicius-batistella](https://github.com/vinicius-batistella)


Path: windows/aarch64/shell/reverse_tcp


Description: Adds Windows AARCH64 staged shell payloads.


## Enhancements and features (15)


- [#21379](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21379) from[g0tmi1k](https://github.com/g0tmi1k) - This improves the FTP login scanner by extending the reporting logic and adding extra checks
- [#21575](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21575) from[g0tmi1k](https://github.com/g0tmi1k) - Improves scanner/ftp/ftp_version to now report the service if host is up but we don't get an appropriate FTP banner
- [#21578](https://github.com/rapid7/metasploit-framework/pull/21578) from[arpan-pramanik](https://github.com/arpan-pramanik) - Fixes a bug where msfconsole crashes with an unhandled exception when attempting to exit if the msf database is missing
- [#21607](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21607) from[Pushpenderrathore](https://github.com/Pushpenderrathore) - This extends CertificateTrace peer cert tracing to support LDAP over TLS and RDP
- [#21608](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21608) from[Pushpenderrathore](https://github.com/Pushpenderrathore) - This adds CertificateTrace peer cert tracing to PostgreSQL over TLS, and fixes a long-standing gap where the Postgres mixin accepted an SSL datastore option but never passed it through to the underlying connection
- [#21622](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21622) from[zeroSteiner](https://github.com/zeroSteiner) - Adds Bearer Authentication to the MCP server plugin
- [#21638](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21638) from[eve0805](https://github.com/eve0805) - This reuses the existing Kerberos ticket trace formatting for offline Kerberos artifact workflows, so stored and converted tickets can be inspected with the same output style used during live Kerberos authentication
- [#21653](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21653) from[cdelafuente-r7](https://github.com/cdelafuente-r7) - This adds job tracking via run_uuid across all module types when executed through RPC endpoints to support Model Context Protocol (MCP) tool integrations
- [#21654](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21654) from[cdelafuente-r7](https://github.com/cdelafuente-r7) - Adds eight new MCP tools: that wrap the existing RPC endpoints for module and session control, backed by strict per-tool input validation with an opt-in flag for dangerous actions
- [#21667](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21667) from[jburgess-r7](https://github.com/jburgess-r7) - This updates the gogs_rebase_rce module with the vulnerability's newly assigned CVE, CVE-2026-52806
- [#21691](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21691) from[eve0805](https://github.com/eve0805) - This adds KerberosTicketTrace support to the auxiliary/admin/kerberos/forge_ticket module
- [#21697](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21697) from[bwatters-r7](https://github.com/bwatters-r7) - Adds explicit endianness to fetch multi payload query strings and encodes the fetch command in base64 for Python 3.8+ environments
- [#21728](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21728) from[zeroSteiner](https://github.com/zeroSteiner) - Adds support for a new MALLEABLEC2 option to Meterpreter HTTP(S) payloads
- [#21748](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21748) from[zeroSteiner](https://github.com/zeroSteiner) - Nyan Nyan Nyan Nyan Nyan Nyan!
- [#21768](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21768) from[adfoster-r7](https://github.com/adfoster-r7) - Updates the default authentication logic in the JSON RPC support to now require auth by default, either via the database with user credentials or an auth token


## Bugs fixed (15)


- [#21552](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21552) from[stzifkas](https://www.google.com/search?q=https://github.com/stzifkas) - Fix LHOST validation rejecting tunnel hostnames when DNS lookup fails
- [#21574](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21574) from[g0tmi1k](https://github.com/g0tmi1k) - Fixes the implementation of the FTP mixin to allow for reading multiple responses on the same TCP segment
- [#21609](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21609) from[dwelch-r7](https://github.com/dwelch-r7) - Fixed an issue where payload option validation was delayed until after running show options, ensuring options like LHOST and LPORT are validated immediately upon setting a payload or loading a module
- [#21647](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21647) from[jheysel-r7](https://github.com/jheysel-r7) - This updates the ntlm_relay_2_self module to automatically configure Resource-Based Constrained Delegation (RBCD) on the target machine account by setting its msDS-AllowedToActOnBehalfOfOtherIdentity attribute
- [#21659](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21659) from[dwelch-r7](https://github.com/dwelch-r7) - This improves port conflict handling for the MCP plugin by adding pre-flight availability checks, post-spawn verification, and proper state resets on failure
- [#21661](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21661) from[kx7m2qd](https://github.com/kx7m2qd) - Fixes an issue with Ctrl+C handling regarding the MCP plugin
- [#21663](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21663) from[zeroSteiner](https://github.com/zeroSteiner) - Updates the error handling in the auxiliary/admin/dcerpc/icpr_cert module
- [#21687](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21687) from[zeroSteiner](https://github.com/zeroSteiner) - This updates and centralizes the warning message that's displayed when a user sets a datastore option that is not valid in the current context
- [#21699](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21699) from[sjanusz-r7](https://github.com/sjanusz-r7) - Fixes a FrozenError issue when the DNS feature was enabled, which occurred when attempting to mutate frozen strings in place during DNS queries
- [#21701](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21701) from[cdelafuente-r7](https://github.com/cdelafuente-r7) - This fixes an issue where exploit error messages were captured by the job listener during execution but hidden from the console
- [#21711](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21711) from[cdelafuente-r7](https://github.com/cdelafuente-r7) - This fixes a Ruby 3 keyword argument parsing issue in run_simple, exploit_simple, and check_simple where passing a braceless hash literal caused an ArgumentError
- [#21714](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21714) from[l1ve709](https://github.com/l1ve709) - Fixes typos in various module docs
- [#21718](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21718) from[sjanusz-r7](https://github.com/sjanusz-r7) - Fixes a crash when attempting to run VNC sessions from Metasploit
- [#21722](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21722) from[dwelch-r7](https://github.com/dwelch-r7) - Fixed a regression to now again allow 0.0.0.0 as a valid listener LHOST address
- [#21759](https://www.google.com/search?q=https://github.com/rapid7/metasploit-framework/pull/21759) from[adfoster-r7](https://github.com/adfoster-r7) - Fixes a crash on multiple SMB modules when attempting to register an SMB service


## Documentation


You can find the latest Metasploit documentation on our docsite at[docs.metasploit.com](https://docs.metasploit.com/) .


## Get it


As always, you can update to the latest Metasploit Framework with msfupdate and you can get more details on the changes since the last blog post from GitHub:


- [Pull Requests 6.4.144...6.5.2](https://www.google.com/search?q=%5Bhttps%3A%2F%2Fgithub.com%2Frapid7%2Fmetasploit-framework%2Fpulls%3Fq%3Dis%3Apr%2Bmerged%5D%28https%3A%2F%2Fgithub.com%2Frapid7%2Fmetasploit-framework%2Fpulls%3Fq%3Dis%3Apr%2Bmerged%29%3A%222026-07-15T15%253A48%253A48-07%253A00..2026-08-13T15%253A07%253A46%252B01%253A00%22)
- [Full diff 6.4.144...6.5.2](https://www.google.com/search?q=%5Bhttps%3A%2F%2Fgithub.com%2Frapid7%2Fmetasploit-framework%2Fcompare%2F6.4.144...6.5.2%5D%28https%3A%2F%2Fgithub.com%2Frapid7%2Fmetasploit-framework%2Fcompare%2F6.4.144...6.5.2%29)


If you are a git user, you can clone the[Metasploit Framework repo](https://www.google.com/search?q=%5Bhttps%3A%2F%2Fgithub.com%2Frapid7%2Fmetasploit-framework%5D%28https%3A%2F%2Fgithub.com%2Frapid7%2Fmetasploit-framework%29) (master branch) for the latest. To install fresh without using git, you can use the open-source-only[Nightly Installers](https://www.google.com/search?q=%5Bhttps%3A%2F%2Fgithub.com%2Frapid7%2Fmetasploit-framework%2Fwiki%2FNightly-Installers%5D%28https%3A%2F%2Fgithub.com%2Frapid7%2Fmetasploit-framework%2Fwiki%2FNightly-Installers%29) or the commercial edition[Metasploit Pro](https://www.google.com/search?q=%5Bhttps%3A%2F%2Fwww.rapid7.com%2Fproducts%2Fmetasploit%2Fdownload%2F%5D%28https%3A%2F%2Fwww.rapid7.com%2Fproducts%2Fmetasploit%2Fdownload%2F%29) .
