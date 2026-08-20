---
schema_version: "1.0.0"
document_id: "eab91884c2f2e056a73605537002ae8ba4595603805d8ae5144c0ae3da0accb3"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/what-the-warp-terminal-sends-home/"
published_at: "2026-04-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:15:51.462388+00:00"
content_hash: "sha256:7c11ddd41ac2ee3faa93e31cd8b01c1f85bc9bb6eb811fe59b4a4a0175b183d9"
---

# What the Warp Terminal Sends Home

The terminal is a developer’s most trusted tool. It sees your source code, your secrets, the commands you run in production. When a terminal adds AI and cloud features, it’s worth asking what it’s doing on the network.


The[Warp Terminal](https://warp.dev/) was the first terminal emulator I came across that tried to be more than just a simple CLI wrapper. Most terminals add quality of life features like tabs, search, and color themes. Warp sought to give devs more power by adding AI on top of the terminal experience, and to that end they seem to have done a pretty great job. The foundation is solid and fast, and the AI tools seem genuinely helpful for those who want their entire CLI experience to be wrapped in a Claude-Code-esque harness. I don’t personally use Warp, but I get the appeal.


But Warp is a company, which means they are incentivized to collect data. And they are using AI, which means you’re trading privacy, and maybe security, for convenience.


🎯 Key Takeaways


- Warp sends every command you run, with its output and working directory, to` /analytics/block` , tied to your account. Secret redaction exists but can’t reliably catch every credential pattern in shell output.
- Every AI interaction ships your entire AGENTS.md and all your` ~/.claude/skills/` definitions (24+ in our capture, including internal tool integrations) as a protobuf blob.
- AI routing is server-side. In one conversation the prompts went to GPT-5.2 and GLM 5 (Zhipu AI, a Chinese company), and the client never talks to OpenAI or Anthropic directly.
- Warp’s core API calls bypass HTTP_PROXY and use NoKeyLog, so normal inspection tools can’t see them. We got in with transparent TLS interception plus a process-filtered packet capture.


## Every Command Gets Logged


Every command you run is sent to Warp’s servers via the` /analytics/block` endpoint:


```text
{
"command"  :   "whoami"  ,
"output"  :   "josh"  ,
"exit_code"  :   0  ,
"predicted_command"  :   null  ,
"pwd"  :   "/Users/josh"  ,
"shell"  :   "zsh"  ,
"username"  :   "josh"  ,
"hostname"  :   "Mac.lan"  ,
"os"  :   "MacOS"  ,
"session_id"  :   177565946126758  ,
"block_id"  :   "precmd-177565946126758-1"  ,
"git_branch"  :   null  ,
"start_ts"  :   "2026-04-08T10:44:40.380284-04:00"  ,
"completed_ts"  :   "2026-04-08T10:44:40.438909-04:00"  ,
"was_autosuggestion_accepted"  :   null  ,
"was_autosuggestion_from_ai"  :   null
}
```


The payload includes your command, output, working directory, and several other details. Plus whether the command was predicted by the warp AI and whether you accepted it. This means that commands that include credentials, like` export AWS_SECRET_ACCESS_KEY=...` ,` curl -H "Authorization: Bearer ..."` ,` ssh user@production-db` , etc. go through the same endpoint, associated with your account. Warp does say that they “unconditionally applies[Secret Redaction](https://docs.warp.dev/support-and-community/privacy-security-and-licensing/secret-redaction) in all AI interactions to ensure that any sensitive data is never collected or sent to third parties”, but in reality credential patterns in shell output are too varied and context-dependent to catch reliably. This is a common trade off when using cloud-backed AI tools and means vendor trust is critical.


## What Warp Sends Before You Type Anything


- A Firebase token refresh to` securetoken.googleapis.com` , creating a Google login record for the session
- A version check
- Multiple GraphQL operations: referral status, 30 days of AI conversation history, GitHub connection status (` UserGithubInfo` , called several times per session), the complete AI model catalog, and your rate limits and credit balance
- A persistent WebSocket to` rtc.app.warp.dev` that stays open for the entire session, pinging every 3 seconds via the` graphql-transport-ws` protocol, even during local terminal use
- RudderStack analytics keyed to both a` userId` and an` anonymousId` , routed to Amplitude; events include experiment enrollment assignments and other behavioral telemetry sent at session start and throughout the session


## The AI Request That Sends Everything


When you use Warp’s AI features (agent mode, Cmd+I, or autocomplete), the client sends a protobuf request to` /ai/multi-agent` or` /ai/passive-suggestions` . Not JSON: even if you intercept the traffic, the payloads aren’t human-readable without a decoder.


We decoded the protobuf. A single` /ai/multi-agent` request (around 10KB) contained:


**Your prompt.** In our test case, “What does this app do?”


**Your entire AGENTS.md file** : build commands, project structure, coding conventions, deployment procedures. Sent as a binary blob on every request.


**All of your AI skill definitions** : every file from` ~/.claude/skills/` with full filesystem paths and descriptions. Our capture included 24+ skills covering internal infrastructure tools (Jira, Datadog, Grafana, GitLab CI), personal automation, and employer-specific integrations. The file paths are specific:


**Terminal command history** with timestamps. Working directory, home directory, git branch, shell version, OS.


The` /ai/passive-suggestions` endpoint sends the same 10KB skill payload for non-agent autocomplete.


Every AI interaction, not just explicit agent mode, sends your company’s internal tooling configuration to Warp’s servers.


## Which Models Actually Handle Your Prompts


Warp’s UI lets you select from a list of AI models. The` GetConversationUsage` response reveals which models actually handled your conversations, and they don’t always match.


A single conversation titled “Clarify App Functionality” used:


- **GPT-5.2 (low reasoning)** : 140,785 tokens as` primary_agent`
- **GLM 5** : 22,210 tokens as` primary_agent`


GPT you probably know. GLM 5 is less common and is developed by Zhipu AI, a Chinese company.


The full catalog shows roughly 25 models from four providers. In “auto” mode, Warp’s servers route your prompts based on cost/quality/speed scores. The decision is entirely server-side, and the client never contacts` api.anthropic.com` or` api.openai.com` directly. All AI traffic routes through` app.warp.dev` .


## How We Found Any of This


Our first instinct was to wrap the Warp binary with[proxymock](https://speedscale.com/proxymock/) , our CLI traffic capture tool:


```text
proxymock   record   --   /Applications/Warp.app/Contents/MacOS/stable
```


After recording,` proxymock inspect` opens a TUI to explore the captured traffic:


` proxymock` captured GraphQL queries, Firebase authentication, and RudderStack analytics. But a parallel` tcpdump` capture showed traffic to` app.warp.dev` that never appeared in proxymock at all. Warp’s core API calls intentionally bypass` HTTP_PROXY` entirely, so standard inspection tools don’t see them.


It’s interesting, though maybe not surprising, to see them using this obfuscation technique, especially for only the agent traffic. The` proxymock` tool doesn’t bother trying to get around this because our customers know their traffic is being recorded. That’s the whole point!


If you’re curious how production-grade traffic capture works under the hood, we wrote a[deep dive on building a traffic capture system](https://speedscale.com/blog/how-to-build-a-traffic-capture-system/) .


A process-filtered packet capture confirmed the traffic:


```text
sudo   tcpdump   -i   pktap,all   -Q   "proc=stable or eproc=stable"   \
-w   capture.pcapng   "tcp port 443"
```


We could see the connections but not the contents (TLS-encrypted, no keys available). Warp uses rustls 0.23.31 with` NoKeyLog` configured, a compile-time decision that disables` SSLKEYLOGFILE` . Even with raw packets, you can’t decrypt them.


The only approach that worked was transparent TLS interception: DNS redirection via` /etc/hosts` to route` app.warp.dev` to localhost, a local service forwarding traffic to the real server, and a trusted CA certificate installed in the macOS system keychain. Warp loads native root CAs and has no certificate pinning, so this works, but it requires root access and deliberate setup.


## Why It’s Hard to Inspect


Individually, none of these is a red flag but together they make it difficult to independently verify what Warp sends:


**Server-side LLM routing.** All AI traffic goes through` app.warp.dev` . The client never contacts an LLM provider directly, so you can’t identify AI traffic by destination.


**HTTP_PROXY bypass.** Warp’s core API calls ignore the` HTTP_PROXY` environment variable, which means standard traffic inspection tools like Charles, Proxyman, and` proxymock` don’t see them.


**NoKeyLog.**` NoKeyLog` explicitly set disables` SSLKEYLOGFILE` . Even with a raw packet capture, you can’t decrypt the content. (We’ve written about[debugging encrypted microservice traffic](https://speedscale.com/blog/you-cant-fix-what-you-cant-see-debugging-encrypted-microservice-traffic-with-speedscales-ebpf-collector/) if you want to go deeper on this problem.)


**Protobuf encoding.** All` /ai/*` endpoints use` application/x-protobuf` instead of JSON. Intercepted traffic requires a decoder to read.


## To Warp’s Credit


- **No silent file exfiltration.** Warp doesn’t read source files from your project tree and upload them. What gets sent is what you explicitly share through AI features.
- **Sentry error reporting is standard** , though the 170KB payloads containing 100 UI action breadcrumbs and full app config dumps are larger than typical.
- **The terminal works offline.** Core functionality doesn’t require a network connection.
- **Warp is transparent about their telemetry.** They publish an[exhaustive telemetry table](https://docs.warp.dev/support-and-community/privacy-security-and-licensing/privacy#exhaustive-telemetry-table) documenting every event they collect, and the settings menu includes a “View network logging” link that shows telemetry requests in real-time as they’re sent. Non-AI telemetry is also opt-out via Settings > Privacy > “Help improve Warp.”


## The Tradeoff


Warp is a good terminal with genuinely useful AI features. But you should know that by default your skill definitions, repo configuration, and command history get sent to Warp’s servers.


Warp exposes a “Help improve Warp” toggle in Settings > Privacy, which maps to the` isTelemetryEnabled` flag on their backend. In our capture, that flag was` true` and the` /analytics/block` endpoint was actively logging commands. We also verified that this telemetry data is not sent with this setting toggled off. The AI features themselves will always send your context regardless of this setting, which is expected.


The granularity of data Warp collects is genuinely impressive. It’s a testament to their engineering and probably a good indicator of the quality of the rest of the product, but as a user it’s something you need to know about. Warp documents their telemetry extensively, as covered above.


If you want to see what your own tools are sending home,[proxymock](https://speedscale.com/proxymock/) can wrap any process and give you a searchable view of its HTTP traffic in seconds.


## Common questions


### Does the Warp terminal send my commands to its servers?


Yes. Every command you run, along with its output, exit code, working directory, and shell details, is sent to Warp’s` /analytics/block` endpoint and associated with your account. Warp applies secret redaction, but credential patterns in shell output are too varied to catch reliably, so vendor trust is part of the deal.


### What does Warp send when you use its AI features?


A protobuf request to` /ai/multi-agent` or` /ai/passive-suggestions` that includes your prompt, your entire AGENTS.md file, and every skill definition in` ~/.claude/skills/` with full file paths. Our capture included 24+ skills covering internal infrastructure tools. This happens on AI autocomplete too, not just explicit agent mode.


### Which AI models does Warp actually use?


Routing is server-side and doesn’t always match the model you pick in the UI. One conversation in our test used GPT-5.2 and GLM 5, a model from Zhipu AI in China. The catalog has about 25 models from four providers, and the client never contacts OpenAI or Anthropic directly. Everything routes through app.warp.dev.


### Why is Warp’s network traffic hard to inspect?


Its core API calls bypass the HTTP_PROXY variable, so tools like Charles, Proxyman, and proxymock don’t see them. It uses rustls with NoKeyLog, which disables SSLKEYLOGFILE so raw packet captures can’t be decrypted, and the AI endpoints use protobuf instead of JSON. We got in with transparent TLS interception via /etc/hosts redirection and a trusted local CA.


---


*This analysis was conducted on Warp v0.2026.04.01.08.39.stable_02 running on macOS 26.3.1 in April 2026. Warp’s behavior may change in future versions. If we’ve mischaracterized anything, we’ll update this post.*


*[Speedscale](https://speedscale.com/) builds traffic replay and API simulation tools for development teams.[proxymock](https://docs.speedscale.com/proxymock/) is our open-source traffic capture tool.*
