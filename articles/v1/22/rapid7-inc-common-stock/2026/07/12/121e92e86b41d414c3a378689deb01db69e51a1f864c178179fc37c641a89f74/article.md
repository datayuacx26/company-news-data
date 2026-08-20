---
schema_version: "1.0.0"
document_id: "121e92e86b41d414c3a378689deb01db69e51a1f864c178179fc37c641a89f74"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/pt-weekly-metasploit-update-exploits-for-flowiseai-csv-agent-and-macos-package-kit"
published_at: "2026-07-11T00:32:34+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:a949e9f274d70495c7663d4a3ad35780d88e4d4c37813022a4bf8ac715a459e1"
---

# Weekly Metasploit Update: Exploits for FlowiseAI CSV Agent and MacOS Package Kit

## More AI, more software, more bugs!


AI, it's all you hear about nowadays and everyone's got an opinion on it. Here at Metasploit, we care less about those opinions and more about the growing attack surface all this new software brings with it (yeehaw exploits!). Take for example the new Flowise CSV Agent Prompt Injection RCE brought to you by Takahiro Yokoyama and zdi-disclosures. Flowise is an open-source tool that lets you build AI apps and chatbots using a visual, drag-and-drop canvas and CVE-2026-41264 is an unauthenticated RCE run method of the CSV_Agents class in Flowise. The vulnerability exists due insufficient sandboxing and an incomplete list of disallowed inputs. It allows unauthenticated attackers to upload a .csv file containing arbitrary python code and execute it. One moment you're using AI to help draft and email and the next moment you're getting pwn'd, what a world we live in! Happy Friday and happy hacking everyone.


## New module content (3)


### Apache .htaccess Persistence


Authors: 4ravind-b, msutovsky-r7, and wireghoul


Type: Exploit


Pull request:[#21473](https://github.com/rapid7/metasploit-framework/pull/21473) contributed by[4ravind-b](https://github.com/4ravind-b)


Path: linux/persistence/apache_htaccess


Description: Adds a new persistence module, exploits/linux/persistence/apache_htaccess, that plants wireghoul's mod_cgi .htaccess web shell on a Linux Apache target.


### Flowise CSV Agent Prompt Injection RCE


Authors: Takahiro Yokoyama and zdi-disclosures


Type: Exploit


Pull request:[#21407](https://github.com/rapid7/metasploit-framework/pull/21407) contributed by[Takahiro-Yoko](https://github.com/Takahiro-Yoko)


Path: multi/http/flowise_auth_rce_cve_2026_41264


AttackerKB reference:[CVE-2026-41264](https://attackerkb.com/search?q=CVE-2026-41264&referrer=blog)


Description: This adds a new exploit module for FlowiseAI Flowise (CVE-2026-41264). The CSV Agent feature evaluates LLM-generated Python code without proper sandboxing, allowing a prompt injection to achieve arbitrary code execution as the user running the server. Flowise versions 1.3.0 through 3.0.13 are affected. The module requires an API key with chatflows:create permission but does not require Flowise authentication to trigger the underlying flaw.


### macOS PackageKit ZSH Environment Privilege Escalation


Authors: Mykola Grymalyuk and h00die


Type: Exploit


Pull request:[#21499](https://github.com/rapid7/metasploit-framework/pull/21499) contributed by[h00die](https://github.com/h00die)


Path: osx/local/packagekit_zshenv_privesc


AttackerKB reference:[CVE-2024-27822](https://attackerkb.com/search?q=CVE-2024-27822&referrer=blog)


Description: This adds a new local privilege escalation module for macOS targeting CVE-2024-27822 in PackageKit.framework. When a PKG installer script uses a ZSH shebang, PackageKit runs it as root while inheriting the installing user's environment, causing ZSH to source the user's ~/.zshenv with root privileges. The module plants a payload in ~/.zshenv that fires only when running as root, then opens a minimal PKG with Installer.app; once the user approves the installation prompt and authenticates, the payload executes as root and a root session is returned. Affected versions are macOS 14.4, 13.6.6, 12.7.4, and 11 and earlier; the issue is patched in 14.5, 13.6.7, and 12.7.5.


## Enhancements and features (5)


- [#21416](https://github.com/rapid7/metasploit-framework/pull/21416) from[g0tmi1k](https://github.com/g0tmi1k) - This updates the Exploit::Remote::Ftp mixin to improve target fingerprinting. It now leverages recog to fingerprint targets from their banners and adds ftp_fingerprint and ftp_list_directory methods to assist with target enumeration.
- [#21436](https://github.com/rapid7/metasploit-framework/pull/21436) from[g0tmi1k](https://github.com/g0tmi1k) - Improved UX for reloading of library files.
- [#21579](https://github.com/rapid7/metasploit-framework/pull/21579) from[zeroSteiner](https://github.com/zeroSteiner) - This adds a few extra fields to some MCP Server tools to align with recent RPC changes in the framework. The msf_service_info tool now has resource and parents fields, the msf_vulnerability_info tool now has a resource field, the msf_note_info tool now has a data field, and the msf_credential_info tool now has new realm_key and realm_value fields.
- [#21580](https://github.com/rapid7/metasploit-framework/pull/21580) from[Pushpenderrathore](https://github.com/Pushpenderrathore) - This adds a Certificate Signing Request (CSR) Trace to the CertificateTrace functionality. Users can now opt to see the CSR get printed when requesting certificates from AD CS.
- [#21637](https://github.com/rapid7/metasploit-framework/pull/21637) from[eve0805](https://github.com/eve0805) - This adds improved levels of granularity to the KerberosTicketTrace functionality. Users can now choose to print the full kerberos trace output, only the tickets or only the metadata.


## Bugs fixed (2)


- [#21588](https://github.com/rapid7/metasploit-framework/pull/21588) from[vinicius-batistella](https://github.com/vinicius-batistella) - Fix a bug in the format dispatcher where although we can generate AARCH64 windows exe files, we fail trying to do so because the dispatcher does not properly handle the request by the user.
- [#21651](https://github.com/rapid7/metasploit-framework/pull/21651) from[jheysel-r7](https://github.com/jheysel-r7) - This fixes a bug in the Role Based Constrained Delegation (RBCD) module that prevented Access Control Entries (ACEs) from being removed due to a type mismatch while comparing Security Identifiers (SIDs).


## Documentation


You can find the latest Metasploit documentation on our docsite at[docs.metasploit.com](https://docs.metasploit.com/) .


## Get it


As always, you can update to the latest Metasploit Framework with msfupdate and you can get more details on the changes since the last blog post from GitHub:


- [Pull Requests 6.4.142...6.4.143](https://github.com/rapid7/metasploit-framework/pulls?q=is:pr+merged:%222026-07-01T09%3A42%3A42Z..2026-07-08T13%3A32%3A18-07%3A00%22)
- [Full diff 6.4.142...6.4.143](https://github.com/rapid7/metasploit-framework/compare/6.4.142...6.4.143)


If you are a git user, you can clone the[Metasploit Framework repo](https://github.com/rapid7/metasploit-framework) (master branch) for the latest. To install fresh without using git, you can use the open-source-only[Nightly Installers](https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers) or the commercial edition[Metasploit Pro](https://www.rapid7.com/products/metasploit/download/)
