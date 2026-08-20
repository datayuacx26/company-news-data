---
schema_version: "1.0.0"
document_id: "9d171d1f8d95ab350be4d3b4865da6c5fa67be5e101d2f409bb072299bd44da6"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection"
published_at: "2026-08-07T23:59:59+00:00"
first_seen_at: "2026-08-07T18:12:01.041968+00:00"
fetched_at: "2026-08-08T00:00:01.391582+00:00"
content_hash: "sha256:0325b72597ddc63ea24f6f0a73044dbc19e5f3b41f9b68ba2e61e38e57c1d76c"
---

# Living off the coding agent: Two tales of tunnels and LaunchAgents

7 August 2026 •


[Mika Ayenson, PhD](https://www.elastic.co/security-labs/author/mika-ayenson) •


[Jia Yu Chan](https://www.elastic.co/security-labs/author/jia-yu-chan)


# Living off the coding agent: Two tales of tunnels and LaunchAgents


A macOS case study on Claude Code ancestry, free tunnel brokers, and the challenge of validating GenAI-adjacent alerts.


10 min read


[Generative AI](https://www.elastic.co/security-labs/category/generative-ai)


Coding agents such as Claude Code and Cursor are vendor-signed, used all day on developer laptops, and routinely open shells, call APIs, edit files, and install helpers. That makes GenAI-adjacent alerts challenging to investigate. The parent looks trusted, while the children can still look a lot like classic high-severity activity.


This article walks through one of those windows. On a macOS endpoint, Elastic Security endpoint telemetry showed shells under Claude Code that scripted a login to an ephemeral tunnel hostname, pulled application metrics, stood up a Cloudflare quick tunnel, and installed LaunchAgent persistence. Immediate children were often shells (` zsh` ) and helpers under that ancestry, not Claude executing every binary itself.


The outcomes Endpoint cares about are still there. Credentials show up on the wire, along with reverse tunnels and LaunchAgents. The destinations and command shapes also line up with known free tunnel providers (` localhost\[.\]run` /` lhr\[.\]life` , trycloudflare, ngrok, etc.), a local loopback app pattern, and ordinary SaaS APIs. Put together, the same telemetry can read like a remote admin of a local dashboard with agent help, which is exactly why these alerts are hard to triage.


Below we walk the multi-day suspicious activity and the 23 Jul window stage by stage, name the known dual-use tools, show which production rules fired, and describe challenges detection engineers and analysts encounter when investigating these alerts.


###


Key takeaways


- Agent-parented reverse tunnels and LaunchAgents can expose a local admin app to the internet. Treat that as high severity even when it looks like vibe-coded ops, not confirmed malware.
- Known free tunnel brokers (` localhost\[.\]run` /` lhr\[.\]life` , Cloudflare Quick Tunnels, ngrok) show up next to credentialed HTTP and LaunchAgents in the same session.
- The hard part for detection engineers is validation. Trusted coding-agent parents, dual-use destinations, and high-severity outcomes can all arrive in one alert pile.


##


Coding agent activity analysis


Two hunt paths landed on the same host on July 23, 2026. One started from VirusTotal reputation on a domain contacted under the agent lineage. The other started from correlated endpoint behavior alerts, including the production rule` Persistence via GenAI Tool` . Together they rebuilt one sequence rather than a pile of unrelated noisy events.


That was not the first Elastic Defend signal on the host. Telemetry already showed` event.code: malicious_file` and behavior activity from July 20, 2026 onward. Most of it was tunnel and VPN-class binaries, noisy GenAI-adjacent alerts, followed by credential posts and LaunchAgent persistence the hunts highlighted.


###


macOS developer endpoint: Where this showed up


The activity sits on a macOS developer endpoint where Claude Code (and in related cases Cursor) was already installed and in use. The practical class is a workstation that trusts signed coding agents to open shells, edit files, call APIs, and install helpers.


###


How the Claude Code session staged the chain


From telemetry, the session needed to run tooling with fewer approval prompts, reach a tunnel-published URL, expose a local service without inbound firewall changes, keep that path alive, and check related processes. Those steps show up as ordinary process, file, and network event types under a coding-agent parent.


Known dual-use tools in this window include localhost\[.\]run (free SSH tunnels on` *.lhr\[.\]life` ), Cloudflare Quick Tunnels (` *.trycloudflare\[.\]com` /` api.trycloudflare\[.\]com` ), and an ngrok binary. VirusTotal flags on broker apexes (root domain) remain useful hunt signals because those services get abused. In this session, the tunnels sit next to publish-localhost behavior and follow-on requests to` /login` and` /api/summary` on the public URL.


On this endpoint, the observed goals were as follows.


1. Run tooling through an approved coding agent with fewer human approval prompts
2. Authenticate over HTTP(S) to a tunnel-published URL and pull application metrics
3. Expose a local service to the internet without opening inbound firewall ports
4. Keep a reverse tunnel alive across logout or reboot, polling that the public link stays up
5. Check related processes while that access was being built


Each goal shows up under a coding-agent parent. Immediate children were usually shells (` zsh` ) under Claude Code, not Claude executing every binary itself. The raw alert shape still looks like living-off-the-agent activity. The destinations and command shapes give analysts more to work with when they open the case.


In the above image, a Claude Code session (Stage 1) parented credentialed HTTP against ephemeral tunnel hostnames and a metrics pull (Stage 2), a Cloudflare quick tunnel that published localhost (Stage 3), and LaunchAgent material that kept access alive (Stage 4). Stage 2 is where investigation gets interesting, with credentials on a free-tunnel URL under agent ancestry.


###


Related coding-agent parented cases


The tunnel and LaunchAgent window is one full chain. Separately, we also reviewed shorter cases on other hosts where a coding agent is still the execution parent.


- Variant A: Claude Code as effective parent, with Apple-signed Python and osascript dropping and re-running JavaScript under /tmp (LOLBin staging).
- Variant B: A Cursor agent session that tried a decrypted keychain dump filtered toward Linear and Model Context Protocol (MCP) OAuth material; endpoint controls blocked the dump in the case we reviewed ([T1555.001](https://attack.mitre.org/techniques/T1555/001/) ).
- Variant C: Claude Code with permission bypass pulled an unsigned binary over plaintext HTTP, tried quarantine stripping and ad-hoc re-signing, and probed` ~/.zshenv` as a persistence canary.


##


Observed activity flow


###


Days leading into the window


Before the alerts on the July 23 session, the same host had already been noisy in
Defend for several days:


When (UTC) Activity Why it matters


**July 20, 2026** malicious_file on tunnel/VPN-class binaries (` pritunl-client` , later` wireguard-go` ) plus discovery-oriented SIEM alerts Tunnel tooling lands on the host days before the Claude Code session


**July 21, 2026** Production Suspicious Python Script Execution and Network Connection:` zsh -> python3 /tmp/mcp_clean_landers.py -> <redacted domain>` . Same day:` cat` touched Claude project memory under` ~/.claude/projects/*/memory/MEMORY.md` Scripted analytics-style activity under a shell two days before the LaunchAgent window; durable agent memory touch outside usual Claude writers


**July 22, 2026** Spike in[GenAI or MCP Server Child Process Execution](https://github.com/elastic/detection-rules/blob/main/rules_building_block/execution_mcp_server_child_process.toml) Agent children get louder (often benign tooling); useful as timing and ancestry context


**July 23, 2026** Credentialized HTTP to` lhr\[.\]life` / trycloudflare,` cloudflared` , LaunchAgents, plus malicious_file on ngrok (earliest under` zsh` , later also under` launchd` ) The multi-stage chain below; second tunnel binary appears the same day


Malware classifications on commercial VPN clients can be dual-use noise. What matters here is the sequence: tunnel binaries and rising agent-child volume for days, then agent-parented credential posts, reverse tunnels, and LaunchAgent persistence in one window.


On July 21, 2026, we observed a process tree` zsh -> python3 /tmp/mcp_clean_landers.py` with an outbound HTTPS connection (` analytics.<redacted>.dev` ). From the script shape, the calls appear related to a plaintext MCP-style analytics client request under a shell where its request bodies are JSON-RPC method calls to retrieve data, and not data being sent out.


The same calendar day also showed a write to Claude project memory (` MEMORY.md` under` ~/.claude/projects/*/memory` ) by a generic utility` cat` , not the Claude process. One plain reading is agent self-update of durable context (for example` cat > MEMORY.md` ). The touch can sit hours away from other activity in the window.


###


During the July 23rd session


The July 23rd window starts with Claude Code already usable as a vendor-signed parent. Sessions in this window included permission-bypass flags such as` --allow-dangerously-skip-permissions` , so later tool calls needed fewer human clicks. Developers turn modes like that on for speed. From the endpoint's point of view, the parent still looks like a normal vendor binary. What matters is the flag set plus what the children do afterward.


Stage 2 traffic included hosts under` lhr\[.\]life` and Cloudflare Quick Tunnel names (` *.trycloudflare\[.\]com` ).` lhr\[.\]life` is the free subdomain space for` localhost\[.\]run` , an SSH-based free tunnel provider in the same class as Cloudflare Quick Tunnels and ngrok. Next to credential posts under Claude Code, these look like access plumbing for a published local service.


####


Credentialized HTTP(S) under the agent


With a shell under the agent, an outbound curl came next.


```text
# Shape only (credentials redacted)
URL="https://<id>.lhr[.]life"
# poll /login until HTTP 200 (проверка = "check")
curl -X POST "$URL/login" -d 'user=...&password=...'
curl "$URL/api/summary?from=...&to=..." | jq '{spend}'
```


This parent command line is a readiness loop. It polls` /login` until HTTP` 200` , authenticates, then pulls a` {spend} summary. Session files under` /tmp` and status strings like проверка ("check") show up in the same wrapper. Response bodies were discarded (` -o /dev/null` ) during auth checks. The public host is a` localhost\[.\]run\` free-tunnel URL.


```text
curl -X POST 'https://<name>.trycloudflare[.]com/login' -d 'user=...&password=...'
curl 'https://<name>.trycloudflare[.]com/...' | jq '{spend, ads}'
```


Why this reads as suspicious at first glance: a credential is POSTed in plaintext over an anonymized free-tunnel URL under coding-agent ancestry, the loop-until-200 pattern resembles C2 check-in, and the Cyrillic status strings add apparent attribution weight. Same actions can be self-testing a published local app or credentialed access to an exposed service.


The sequence authenticates to a tunnel-published app URL and pulls metrics such as` spend` and` ads` . Credentials in process command lines under coding-agent ancestry still matter for Endpoint and for anyone opening the alert.


What we can show cleanly is agent-parented credentialized HTTP to a known free tunnel broker, plus reverse-tunnel and LaunchAgent follow-ons that can expose a local admin app. VT hits on the broker apex, and odd apex pages sit in that same dual-use bucket. They are part of the case file, not a final judgment. Production coverage that fired here includes` Unusual Network Connection to Suspicious Top Level Domain` .


####


Publish localhost with a reverse tunnel


The same session started` cloudflared` against a local port through Cloudflare’s quick-tunnel control plane.


```text
# Reconstructed (no process.args captured), a typical quick-tunnel invocation looks like:
cloudflared tunnel --url http://localhost:<port>
# control-plane contact includes api.trycloudflare[.]com
```


The usual mechanics here are ingress. The local process opens an outbound control channel to the broker, the broker publishes a public URL, and remote clients reach a service on the laptop without classic port forwarding.` cloudflared` talking to` api.trycloudflare\[.\]com` is the control plane for that hole-punch. Together with the credentialized HTTP stage, the session publishes localhost and then exercises the public URL. The matching production alert was` Unusual Network Connection to Suspicious Web Service` .


The same day, Defend also raised a` malicious_file` event on an ngrok binary in the project tree. The earliest hit in the window sat under a` zsh` parent; later activity also appeared under` launchd` . Another tunnel front beside` cloudflared` . VirusTotal labels the[sample](https://www.virustotal.com/gui/file/d335a47a04dde726897a2e753187e9f48fbf40616c48be3266954c7b9175a09e) as` adware/hacktool ngrok` .


####


LaunchAgents for the tunnel and a watchdog


Under the same agent activity, LaunchAgent material was written and loaded: a watchdog agent configured with` PlistBuddy` and reloaded with` launchctl bootstrap` , alongside tunnel-keeper style agents in the same` com.<vendor>.<app>.* family` (server, ngrok, lhr, tunnel, watchdog). Program args and naming can point at keeping a local dashboard and its tunnels alive across sessions.


```text
PlistBuddy -c "Set :StartInterval 60" ~/Library/LaunchAgents/com.<vendor>.<app>.watchdog.plist
launchctl bootout gui/$(id -u)/com.<vendor>.<app>.watchdog


launchctl bootstrap gui/$(id -u)
~/Library/LaunchAgents/com.<vendor>.<app>.watchdog.plist


# then: curl -s -o /dev/null -w "…: %{http_code}"
https://<id>.trycloudflare[.]com/login   (liveness)
```


Shells and temp files disappear when the session ends. LaunchAgents with KeepAlive and a liveness loop do not. Installing LaunchAgents that keep a reverse tunnel (or the app behind it) alive should stay loud even when the parent is a trusted coding agent. This stage is where production rule` Persistence via GenAI Tool` fired.


####


Process discovery in the same session


The session also ran process discovery helpers, for example` pgrep` patterns tied to related workloads. By itself, that is low severity. In sequence with the stages above, it can look like checking that related services were still up, rather than hunting defensive tools. Related production coverage included` Process Discovery via Built-In Applications` .


The same host also triggered the` GenAI or MCP Server Child Process Execution` rule. That building block rule tracks GenAI parents spawning children, including routine tooling such as` git status` , but also more alarming commands like` security find-generic-password -a <user> -w -s "Claude Code-credentials"` , a Keychain read that prints Claude Code’s OAuth token to STDOUT; but this is[documented](https://github.com/anthropics/claude-code/issues/81707) as expected Claude Code behavior.


###


Coding-agent parented activity through MITRE ATT&CK


Elastic uses the MITRE ATT&CK framework to document common tactics, techniques, and procedures. The mapping below describes technique shape in telemetry so hunters can pivot across similar activity.


####


Tactics


Tactics represent the why of a technique or sub-technique. They are the tactical goal for performing an action.


- [Execution](https://attack.mitre.org/tactics/TA0002/)
- [Persistence](https://attack.mitre.org/tactics/TA0003/)
- [Defense Evasion](https://attack.mitre.org/tactics/TA0005/)
- [Credential Access](https://attack.mitre.org/tactics/TA0006/)
- [Discovery](https://attack.mitre.org/tactics/TA0007/)
- [Command and Control](https://attack.mitre.org/tactics/TA0011/)
- [Exfiltration](https://attack.mitre.org/tactics/TA0010/)


####


Techniques


Techniques represent how a tactical goal is achieved.


- [Command and Scripting Interpreter](https://attack.mitre.org/techniques/T1059/)
- [Application Layer Protocol: Web Protocols](https://attack.mitre.org/techniques/T1071/001/)
- [Exfiltration Over Web Service](https://attack.mitre.org/techniques/T1567/)
- [Protocol Tunneling](https://attack.mitre.org/techniques/T1572/)
- [Create or Modify System Process: Launch Agent](https://attack.mitre.org/techniques/T1543/001/)
- [Process Discovery](https://attack.mitre.org/techniques/T1057/)
- [Credentials from Password Stores: Keychain](https://attack.mitre.org/techniques/T1555/001/) (variant B)
- [Ingress Tool Transfer](https://attack.mitre.org/techniques/T1105/) (variant C)


##


Detecting coding-agent parented chains


###


Detection


The following detection rules and building-block alerts were observed throughout analysis of this host window (diagnostic-only alerts are omitted):


- [Unusual Network Connection to Suspicious Top Level Domain](https://github.com/elastic/detection-rules/blob/main/rules/macos/command_and_control_unusual_connection_to_suspicious_top_level_domain.toml)
- [Unusual Network Connection to Suspicious Web Service](https://github.com/elastic/detection-rules/blob/main/rules/macos/command_and_control_unusual_network_connection_to_suspicious_web_service.toml)
- [GenAI or MCP Server Child Process Execution](https://github.com/elastic/detection-rules/blob/main/rules_building_block/execution_mcp_server_child_process.toml) (building-block volume / ancestry)
- [Process Discovery via Built-In Applications](https://github.com/elastic/detection-rules/blob/main/rules_building_block/discovery_process_discovery_via_builtin_tools.toml)


No single production rule owns the full sequence. SIEM network rules catch reputation-bad or dual-use egress. Building blocks supply ancestry and discovery context. Correlate them with endpoint outcomes, especially when the parent is a trusted coding agent. Practical handling for windows like this:


1. Keep the high-severity outcomes loud. Credentialized HTTP, reverse tunnels, and LaunchAgents under coding-agent ancestry should not be auto-closed because Claude or Cursor is in the tree.
2. Name the destination class early.` localhost\[.\]run` , Cloudflare Quick Tunnels, and ngrok hosts are dual-use. Record that before leaning on rare-TLD or VirusTotal labels alone.
3. Prefer session context over single-event reputation. Readiness loops, publish-localhost followed by` /login` and` /api/summary` , and LaunchAgents that keep those tunnels up are stronger pivots than an apex page that says "no tunnel here."
4. Separate volume from outcome. GenAI child-process building blocks are ancestry and timing context until secrets, tunnels, or persistence show up.
5. Document both readings when the evidence supports them. The same chain can be a remote admin of a local dashboard or something more serious.


###


Prevention


The following Elastic Defend behavior prevention events were observed:


- [Persistence via GenAI Tool](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/cross-platform/persistence_persistence_via_genai_tool.toml)
- [Suspicious Python Script Execution and Network Connection](https://github.com/elastic/protections-artifacts/blob/main/behavior/rules/macos/execution_suspicious_python_script_execution_and_network_connection.toml)


##


Conclusion


This window teeters between two readings that share the same telemetry shape. On one side, agent-parented credentialized HTTP, reverse tunnels, and LaunchAgents are exactly the outcomes Endpoint is supposed to surface. On the other, the destinations and command shapes line up with known free tunnel brokers and remote exercise of a local admin app. Reputation labels and unbound broker apex pages do not break that tie by themselves.


That ambiguity is the GenAI detection-engineering problem, and it will get harder as coding agents stay in the trusted parent path for more of the work developers already do with shells, APIs, and helpers. The useful response is not to suppress the high-severity outcomes, and not to force every dual-use tunnel into a confirmed intrusion narrative. Keep the alerts, name the tool class, rebuild the session, and write down what the evidence can and cannot support.


##


References


The following were referenced throughout the above research:


- [Elastic Advances LLM Security with Standardized Fields and Integrations](https://www.elastic.co/security-labs/elastic-advances-llm-security)
- [Embedding Security in LLM Workflows: Elastic's Proactive Approach](https://www.elastic.co/security-labs/embedding-security-in-llm-workflows)


#### Jump to section


- [Key takeaways](https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection#key-takeaways)
- [Coding agent activity analysis](https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection#coding-agent-activity-analysis)
- [macOS developer endpoint: Where this showed up](https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection#macos-developer-endpoint-where-this-showed-up)
- [How the Claude Code session staged the chain](https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection#how-the-claude-code-session-staged-the-chain)
- [Related coding-agent parented cases](https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection#related-coding-agent-parented-cases)
- [Observed activity flow](https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection#observed-activity-flow)
- [Days leading into the window](https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection#days-leading-into-the-window)
- [During the July 23rd session](https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection#during-the-july-23rd-session)
- [Credentialized HTTP(S) under the agent](https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection#credentialized-https-under-the-agent)
- [Publish localhost with a reverse tunnel](https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection#publish-localhost-with-a-reverse-tunnel)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Living%20off%20the%20coding%20agent:%20Two%20tales%20of%20tunnels%20and%20LaunchAgents&url=https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection&title=Living%20off%20the%20coding%20agent:%20Two%20tales%20of%20tunnels%20and%20LaunchAgents)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/coding-agent-launchagent-tunnel-detection&title=Living%20off%20the%20coding%20agent:%20Two%20tales%20of%20tunnels%20and%20LaunchAgents)
