---
schema_version: "1.0.0"
document_id: "802bfd78c6eb26776ee863210845ff30189c3f165bfbb4437608a1cb4b3ba9ed"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-22T19:57:23.739169+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:3288695153997c488e6d681c1ef992048e7cdfc403074e48bd8754cc28d8a01a"
---

# wp2shell hits WordPress: detecting pre-auth RCE from plugin drop to command execution

23 July 2026 •


[Ruben Groenewoud](https://www.elastic.co/security-labs/author/ruben-groenewoud) •


[Bryan Porras Blanch](https://www.elastic.co/security-labs/author/bryan-porras-blanch)


# wp2shell hits WordPress: detecting pre-auth RCE from plugin drop to command execution


We ran the wp2shell WordPress RCE chain end-to-end with Elastic Defend. Detection rule walkthrough, IOCs, and hunt guidance.


10 min read


[Detection Engineering](https://www.elastic.co/security-labs/category/detection-engineering) ,


[Threat Intelligence](https://www.elastic.co/security-labs/category/threat-intelligence)


On July 17, 2026,[Searchlight Cyber](https://slcyber.io/research-center/wp2shell-pre-authentication-rce-in-wordpress-core) disclosed` wp2shell` , a pre-authentication remote code execution chain in WordPress Core ([CVE-2026-63030](https://nvd.nist.gov/vuln/detail/CVE-2026-63030) ,[CVE-2026-60137](https://nvd.nist.gov/vuln/detail/CVE-2026-60137) ). Proof-of-concept tools hit GitHub within hours.[hashkitten](https://x.com/hash_kitten) published the chain after PoCs started circulating, including a write-up of[how the bug was found](https://slcyber.io/research-center/exploit-brokers-pay-500000-for-a-wordpress-rce-i-found-one-with-gpt5-6/) .


Scanning followed immediately, and we are already seeing the same host footprint in customer telemetry: PHP and web server runtimes spawning shells, plugin directories appearing under` wp-content/plugins/` , and access-log markers from stock tooling.


This post is the defender-facing follow-up. We ran the public[Icex0 PoC](https://github.com/Icex0/wp2shell-poc) end-to-end in a lab with Elastic Defend, traced the attack chain in Kibana, and mapped each stage to the rules that fire. If you are patching and triaging right now, that rule walkthrough is the core of the post. We close with IOCs for quick hunts.


This post covers:


- What wp2shell is, which versions are exposed, and where to read the full technical breakdown.
- The PoC wave and what live telemetry looks like.
- An end-to-end lab run: PoC execution, the attack chain on disk, and each detection rule that triggers (and why).
- Network and host IOCs to hunt while public tooling is still unchanged.


Patch to 7.0.2 or 6.9.5 first. Check exposure at[wp2shell.com](https://wp2shell.com/) and treat any internet-facing vulnerable instance as potentially compromised until patched.


##


Scope: public PoCs and observed Linux host behavior


This post follows publicly available PoCs and the behaviors they produce on a Linux host, in conjunction with telemetry we have observed. Attackers can rename plugins, rewrite payloads, or drop webshells outside the plugin directory (for example, under` wp-content/cache/` , as[SANS ISC](https://isc.sans.edu/forums/diary/WordPress+Exploitation+Underway+CVE202663030/33168/) documented). Hunt the IOCs while they are fresh, but rely on behavioral detection for durability.


##


What is wp2shell


` wp2shell` is a pre-authentication exploit chain against WordPress Core's REST` batch` endpoint (` /wp-json/batch/v1` , or` /?rest_route=/batch/v1` ). No plugins required.


WordPress branch Exposure Fixed in


` <= 6.8.5` Not affected by the full RCE chain n/a


` 6.8.0` to` 6.8.5` SQL injection only (no full RCE chain on this branch) Patch to a non-vulnerable release (6.8.6)


` 6.8.6` Patched / Not vulnerable (SQL injection fixed) Included in 6.8.6


` 6.9.0` to` 6.9.4` Full pre-auth RCE chain 6.9.5


` 7.0.0` to` 7.0.1` Full pre-auth RCE chain 7.0.2


The bug is a route confusion in WordPress batch handling. When sub-requests inside a batch call get out of sync, a request can be dispatched under the wrong REST handler. Public chains nest batches to bypass method restrictions, then reach a pre-auth SQL injection primitive through query parameters that should not apply on the route they land on.


From there, tooling differs: the[Icex0 PoC](https://github.com/Icex0/wp2shell-poc) escalates through a SQLi-to-administrator bridge and plugin upload; other actors drop a webshell straight to disk via SQL` INTO OUTFILE` . Both end the same way for defenders: attacker-controlled PHP on the host, then command execution through the web stack.


That is the short version. For the full chain, read[Searchlight Cyber's advisory](https://slcyber.io/research-center/wp2shell-pre-authentication-rce-in-wordpress-core) , their[methodology write-up](https://slcyber.io/research-center/exploit-brokers-pay-500000-for-a-wordpress-rce-i-found-one-with-gpt5-6/) , and the[Icex0 PoC README](https://github.com/Icex0/wp2shell-poc) .[SANS ISC](https://isc.sans.edu/forums/diary/WordPress+Exploitation+Underway+CVE202663030/33168/) captured an in-the-wild SQLi payload if you want raw HTTP context.


The activity maps to[Exploit Public-Facing Application (T1190)](https://attack.mitre.org/techniques/T1190/) ,[Server Software Component: Web Shell (T1505.003)](https://attack.mitre.org/techniques/T1505/003/) , and[Command and Scripting Interpreter (T1059)](https://attack.mitre.org/techniques/T1059/) .


##


wp2shell PoCs and what we're seeing in telemetry


Representative public repos appeared within hours of disclosure:


- [Icex0/wp2shell-poc](https://github.com/Icex0/wp2shell-poc)
- [0xsha/wp2shell](https://github.com/0xsha/wp2shell)
- [sergiointel/wp2shell-poc](https://github.com/sergiointel/wp2shell-poc)
- [dinosn/wp2shell-lab](https://github.com/dinosn/wp2shell-lab)


Most drive the same batch entry point. Honeypots and telemetry show a mix of vulnerability scanning and full exploitation. On hosts with Elastic Defend, the post-exploitation picture is consistent:


- ` httpd` ,` apache2` , or` php-fpm` spawning a shell and running short discovery commands (` sh -c id` is a common first confirmation).
- New paths under` wp-content/plugins/wp2shell_<hex>/` when the plugin-upload chain is used unchanged.
- Access-log markers such as` POST /?rest_route=/batch/v1` with` User-Agent: wp2shell` on stock PoCs.


That web-server-to-shell relationship is the durable detection opportunity. PoC-specific strings help for triage, not for long-term coverage.


##


wp2shell lab walkthrough: from PoC execution to Elastic alerts


The[Icex0 PoC](https://github.com/Icex0/wp2shell-poc) drives the batch SQLi-to-administrator bridge, uploads a plugin-backed webshell, and executes commands through it. Each` shell` invocation generates a fresh random plugin name (` wp2shell_<hex>` ), so repeated runs produce multiple plugin directories on the same host. That is what you see in the figures: one run may reference` wp2shell_6a5566a6` , another` wp2shell_79b06a80` or` wp2shell_360866a8` . The hex suffix is a fragile IOC; the parent process and command patterns are not.


Every successful run follows the same skeleton:


1. **Staging:**` apache2` (or another technology) writing plugin files
2. **Execution:**` apache2` (or another technology) spawning a shell from the plugin working directory.


Elastic Defend records both, and the SIEM rules stack on top as commands execute.


###


wp2shell staging: plugin upload file events on disk


Before any shell alert fires, the file timeline tells the story. The staging sequence is consistent across runs:


In the figure above, we can see the following sequence of events:


1. **Write probes:**` apache2` creates and deletes` temp-write-test-*` files under` wp-content/` to confirm the directory is writable.
2. **Upload staging:** A zip lands under` wp-content/uploads/` (for example` wp2shell_6a5566a6.zip` ), with a temporary PHP upload file under` /tmp/` .
3. **Unpack:** PHP is written under` wp-content/upgrade/wp2shell_<hex>/.../wp2shell_<hex>.php` .
4. **Install:** The plugin directory is renamed into` wp-content/plugins/wp2shell_<hex>/` .


All of this is` apache2` acting as the file writer, which is normal for a WordPress plugin upload but abnormal at this volume and with these filenames on a production site.


###


Alert overview: what fired during wp2shell exploitation


Running the PoC end-to-end on` wp2shell-lab` produced 12 alerts, all as` www-data` on the same host:


The alerts cover backdoor upload and the execution of suspicious and unusual commands. These rules are discussed below.


- [Payload Execution by Web Server](https://github.com/elastic/protections-artifacts/blob/a8602190dc5e6ace08493272ee953f9fefd9eae3/behavior/rules/linux/persistence_payload_execution_by_web_server.toml) (EDR)
- [PHP File Creation in WordPress Plugin Directory](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_php_file_creation_in_wordpress_plugin_dir.toml) (SIEM)
- [Suspicious Command Execution via Web Server](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_suspicious_command_execution.toml) (SIEM)
- [Unusual Command Execution via Web Server](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_unusual_command_execution.toml) (SIEM)


The alert table ties file paths to process activity. The two file-creation alerts reference plugin directories such as` wp-content/plugins/wp2shell_79b06a80` and` wp2shell_360866a8` . The command-execution alerts show` /usr/sbin/apache2` as the parent and` /usr/bin/dash` running` sh -c -- id` ,` whoami` , and` hostname` from those same plugin paths. File creation fires on the drop; endpoint prevention fires when the shell runs; SIEM rules accumulate as each command executes.


###


Process lineage: apache2 spawning a shell in the analyzer graph


The Elastic analyzer graph makes the` wp2shell` web-server-to-shell transition visible in one view:


The chain is` systemd` →` apache2` →` apache2` →` dash` →` hostname` . The` apache2` worker node shows the preceding file and network activity (8 file events, 7 network events in this session). The` dash` node is flagged as an analyzed event with 3 alerts attached and the process terminated by prevention. That is[Payload Execution by Web Server](https://github.com/elastic/protections-artifacts/blob/a8602190dc5e6ace08493272ee953f9fefd9eae3/behavior/rules/linux/persistence_payload_execution_by_web_server.toml) doing its job: stopping the shell the moment` apache2` crosses from serving HTTP to running a payload.


###


What commands does the wp2shell PoC run after exploitation?


Expanding the process and file event table shows everything the[Icex0 PoC](https://github.com/Icex0/wp2shell-poc) ran on the host, not just the first three discovery commands:


From` process.working_directory` set to` wp-content/plugins/wp2shell_<hex>/` ,` apache2` spawns` dash` with` sh -c -- id; whoami; hostname` . Each command runs as a child (` id` ,` whoami` ,` hostname` ) under the web server parent. That is the pattern we see in telemetry and in the alert overview above. The attacker can of course choose what commands to run here.


The[Icex0 PoC](https://github.com/Icex0/wp2shell-poc) removes its own plugin after execution:


```text
sh -c -- d=$(pwd); case "$d" in */wp-content/plugins/*) cd / && rm -rf "$d";; esac
```


That deletes the plugin directory and the staged zip. Responders should not treat a missing` wp2shell_*` folder as evidence the host is clean if process alerts already fired.


###


Attack discovery: correlating 25 wp2shell alerts into one incident


When you pivot from individual alerts to Attack discovery, the narrative pulls the session together. After multiple PoC runs and the extended recon pass, Elastic grouped 25 alerts on` wp2shell-lab` into a single incident:


Attack discovery summarizes the arc:


1. **Initial access:**` wp2shell` exploitation via the batch API.` apache2` creates multiple plugin directories under` wp-content/plugins/` (` wp2shell_78b05e60` ,` wp2shell_369fdaf8` , and others across repeated runs).
2. **Execution:**` apache2` spawns` dash` ; prevention alerts fire on shell execution. The PoC runs discovery commands and attempts self-cleanup.
3. **Persistence:** Additional plugin drops from repeated` shell` invocations (` wp2shell_4014c5b3` ,` wp2shell_e358ff3b` ,` wp2shell_8a5566a0` , and more).
4. **Discovery and privilege escalation:** From a fresh webshell, reconnaissance commands including SUID enumeration (` find / -perm -u=s -type f` ). This is where the extended lab pass adds alert volume beyond the initial` id` /` whoami` /` hostname` trio.


This demonstrates why we detect the behavior rather than the plugin name.


###


PHP File Creation in WordPress Plugin Directory


[PHP File Creation in WordPress Plugin Directory](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_php_file_creation_in_wordpress_plugin_dir.toml) fired several times in our primary run, on paths such as` wp-content/plugins/wp2shell_79b06a80` and` wp2shell_360866a8` . The rule keys on Elastic Defend file events where a web-related process creates PHP under a WordPress plugin path:


```text
file where event.type in ("creation", "change") and (
process.name in (
"nginx", "apache2", "httpd", "php-cgi", "php-fcgi",
"php-cgi.cagefs",  "sw-engine-fpm",
"wget", "curl", "bash", "dash", "sh", "tcsh", "csh",
"zsh", "ksh", "fish", "mksh", "busybox"
) or
process.name like ("php-fpm*", "lsphp*", "*.cgi", "*.fcgi")
) and
file.path like~ "*/wp-content/plugins/*"
```


It is the earliest structured signal in the alert set: the drop lands before the shell executes. The rule ships with a direct reference to the[Icex0 PoC](https://github.com/Icex0/wp2shell-poc) . It is scoped to` plugins/` only and will not catch the SANS-documented` INTO OUTFILE` variant under` wp-content/cache/` , which is why the shell-spawn rules below matter as a second layer.


###


Payload Execution by Web Server


[Payload Execution by Web Server](https://github.com/elastic/protections-artifacts/blob/a8602190dc5e6ace08493272ee953f9fefd9eae3/behavior/rules/linux/persistence_payload_execution_by_web_server.toml) is the Elastic Defend behavioral rule behind the two critical alerts in the overview. It matched when` /usr/sbin/apache2` spawned` /usr/bin/dash` to run suspicious commands. The rule treats web server parents (` apache2` ,` httpd` ,` nginx` ,` php-fpm*` , and others) launching a shell with high-risk command lines as payload execution. To avoid filling the whole blog with EQL queries, a snippet is displayed below:


```text
process where event.type == "start" and event.action == "exec" and (
process.parent.name in (
"nginx", "apache2", "httpd", "caddy", "mongrel_rails", "uwsgi", "daphne",
"httpd.worker", "flask", "php-cgi", "php-fcgi", "php-cgi.cagefs",
"lswsctrl", "varnishd", "uvicorn", "waitress-serve", "starman",
"frankenphp", "zabbix_server", "asterisk", "sw-engine-fpm"
) or
process.parent.name like ("php-fpm*", "gunicorn*", "*.cgi", "*.fcgi") or
[...]
[Additional web server technologies]
[...]
) and
process.name in (
"bash", "dash", "sh", "tcsh", "csh", "zsh", "ksh", "fish", "busybox"
) and
process.args in ("-c", "-cl", "-lc") and
process.command_line like~ (
[...]
[Suspicious command lines]
[...]
)
```


Discovery commands are explicitly in scope, alongside decoding pipelines, reverse shells, credential paths, and more. The full rule logic can be found[here](https://github.com/elastic/protections-artifacts/blob/a8602190dc5e6ace08493272ee953f9fefd9eae3/behavior/rules/linux/persistence_payload_execution_by_web_server.toml) . Where prevention is enabled, the rule terminates the process via` kill_process` , which is why the analyzer graph shows` dash` as a terminated analyzed event. This is the tightest endpoint signal for "web RCE just succeeded."


###


Suspicious Command Execution via Web Server


[Suspicious Command Execution via Web Server](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_suspicious_command_execution.toml) produced three alerts on the initial discovery commands and additional matches when we ran the extended recon pass. This rule is very similar to the previous rule, but does not have preventive actions, and therefore has fewer exclusions baked in, decreasing the risk of introducing false negatives.


###


Unusual Command Execution via Web Server


[Unusual Command Execution via Web Server](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_unusual_command_execution.toml) is a new terms rule: it uses statistics to understand which shell command lines each web-server parent normally runs on a host, then alerts on lines that break the baseline. It produced five alerts in our primary run, the highest count in the set.


Generally, a PoC invocation introduces command lines the host has never seen: compound discovery strings, recon one-liners with` uname` and` find` , and cleanup scripts that reference` wp-content/plugins/` . The rule casts a wider net than “Suspicious Command Execution”. When both fire on the same host within the same session, treat the cluster as high confidence.


###


Suspicious and Unusual Child Execution via Web Server


Two related SIEM rules cover cases where the web stack spawns non-shell children (interpreters, downloaders, reverse-shell helpers):


- [Suspicious Child Execution via Web Server](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_suspicious_child_execution.toml)
- [Unusual Child Execution via Web Server](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_unusual_child_execution.toml)


They did not appear in the 12-alert overview because the Icex0 chain goes through` dash` and standard discovery binaries, which the command-execution rules already cover. They still belong in the same rule set on WordPress hosts: modified exploits that invoke` curl | bash` ,` python -c` , or a reverse shell without passing through the discovery shortlist will land here instead.


###


Payload Downloaded via Curl or Wget by Web Server


Another rule worth mentioning is[Payload Downloaded via Curl or Wget by Web Server](https://github.com/elastic/protections-artifacts/blob/a8602190dc5e6ace08493272ee953f9fefd9eae3/behavior/rules/linux/persistence_payload_downloaded_via_curl_or_wget_by_web_server.toml) . This is an EDR rule (with killing actions) that detects` wget` /` curl` invocations via a` sh -c` sequence, from web server parents. This is a common technique to download additional tooling once RCE was achieved on a host. Although it did not fire on the PoC, it may fire in a real attack.


###


Which Elastic rules should I enable for wp2shell?


Enable these pre-built Linux rules on WordPress hosts:


- [PHP File Creation in WordPress Plugin Directory](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_php_file_creation_in_wordpress_plugin_dir.toml)
- [Suspicious Command Execution via Web Server](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_suspicious_command_execution.toml)
- [Unusual Command Execution via Web Server](https://github.com/elastic/detection-rules/blob/main/rules/linux/persistence_webserver_unusual_command_execution.toml)


Several building block rules are triggering on certain activity from the attack chain, ranging from file creation to reconnaissance commands, and are listed below:


- [Unusual File Creation by Web Server](https://github.com/elastic/detection-rules/blob/main/rules_building_block/persistence_web_server_sus_file_creation.toml)
- [Linux System Information Discovery](https://github.com/elastic/detection-rules/blob/e45a6518f6478eb91623feb1cfac63e4e2f01fd3/rules_building_block/discovery_linux_system_information_discovery.toml)
- [System Owner/User Discovery Linux](https://github.com/elastic/detection-rules/blob/e45a6518f6478eb91623feb1cfac63e4e2f01fd3/rules_building_block/discovery_linux_system_owner_user_discovery.toml)


Deploy Elastic Defend with prevention enabled on hosts that run WordPress. If your web stack uses a parent process not in the current lists, let us know so we can extend scope.


##


Hunting queries


Use these for quick hunts while tooling is still stock. Treat them as fragile: rename the plugin slug or change the User-Agent and most of them disappear. Behavioral rules above are the durable layer.


###


wp2shell network and access log indicators


- ` POST /?rest_route=/batch/v1` or` POST /wp-json/batch/v1` with a nested JSON` requests` array.
- ` User-Agent: wp2shell` ,` User-Agent: cve-2026-63030/1.0` , or` User-Agent: rezwp2shell` on batch traffic.
- HTTP 207 Multi-Status responses on batch requests (supporting evidence, not standalone).
- Follow-on requests to` /wp-admin/plugin-install.php` ,` /wp-admin/update.php?action=upload-plugin` , or` /wp-json/wp/v2/users?context=edit` .


###


wp2shell host artifacts


- Directories matching` wp2shell_<hex>` under` wp-content/plugins/` .
- New or modified` .php` under` wp-content/plugins/` or` wp-content/cache/` .
- Write-test files` temp-write-test-*` under` wp-content/` (seen in our lab run and in staging behavior).
- Unexpected WordPress administrator accounts created during the exploitation window.
- PHP webshell hashes published in open research (supplemental; prioritize process and file telemetry).


###


How to block wp2shell if you can't patch immediately


If you cannot patch immediately, block anonymous access to` /wp-json/batch/v1` and` /?rest_route=/batch/v1` at the edge, or disable anonymous REST access via a hardening plugin. Expect breakage of legitimate batch consumers; use only as a temporary measure until 7.0.2 or 6.9.5 is deployed.


##


wp2shell detection: patch first, then verify your coverage


` wp2shell` turns a REST batch parsing bug into pre-authenticated code execution on default WordPress installs. Public PoCs spread fast, and the host footprint is predictable: plugin files staged under` wp-content/` , then a web server parent spawning a shell that runs a malicious command.


We ran the[Icex0 chain](https://github.com/Icex0/wp2shell-poc) end-to-end in a lab and watched the detections line up with that sequence. PHP File Creation in WordPress Plugin Directory catches the drop. Payload Execution by Web Server fires first on the endpoint when` apache2` runs the payload. Suspicious and Unusual Command Execution via Web Server adds SIEM depth on the same shell activity. Additional rules are in place to detect real-world activity beyond the PoC. Patch first, hunt the IOCs while they last, and lean on that behavioral stack for coverage that survives PoC renames.


#### Jump to section


- [Scope: public PoCs and observed Linux host behavior](https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend#scope-public-pocs-and-observed-linux-host-behavior)
- [What is wp2shell](https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend#what-is-wp2shell)
- [wp2shell PoCs and what we're seeing in telemetry](https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend#wp2shell-pocs-and-what-were-seeing-in-telemetry)
- [wp2shell lab walkthrough: from PoC execution to Elastic alerts](https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend#wp2shell-lab-walkthrough-from-poc-execution-to-elastic-alerts)
- [wp2shell staging: plugin upload file events on disk](https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend#wp2shell-staging-plugin-upload-file-events-on-disk)
- [Alert overview: what fired during wp2shell exploitation](https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend#alert-overview-what-fired-during-wp2shell-exploitation)
- [Process lineage: apache2 spawning a shell in the analyzer graph](https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend#process-lineage-apache2-spawning-a-shell-in-the-analyzer-graph)
- [What commands does the wp2shell PoC run after exploitation?](https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend#what-commands-does-the-wp2shell-poc-run-after-exploitation)
- [Attack discovery: correlating 25 wp2shell alerts into one incident](https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend#attack-discovery-correlating-25-wp2shell-alerts-into-one-incident)
- [PHP File Creation in WordPress Plugin Directory](https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend#php-file-creation-in-wordpress-plugin-directory)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=wp2shell%20hits%20WordPress:%20detecting%20pre-auth%20RCE%20from%20plugin%20drop%20to%20command%20execution&url=https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend&title=wp2shell%20hits%20WordPress:%20detecting%20pre-auth%20RCE%20from%20plugin%20drop%20to%20command%20execution)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/wp2shell-wordpress-rce-detection-elastic-defend&title=wp2shell%20hits%20WordPress:%20detecting%20pre-auth%20RCE%20from%20plugin%20drop%20to%20command%20execution)
