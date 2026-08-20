---
schema_version: "1.0.0"
document_id: "87158e97d54aaeedcd9635a3c3faba57a4a2b7f0d3bb1e7d7b88c49ff3a92043"
company_key: "yc-firecrawl"
company: "Firecrawl"
source_id: "yc-firecrawl-news-import-e3b10de50c72"
canonical_url: "https://www.firecrawl.dev/blog/secure-openclaw"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-07-21T20:06:32.273956+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:1413f8cf7e7c94be112d7d6b3fa2dc315aea5cd32a098b4509c16e394998e95e"
---

# Is OpenClaw Safe? 7 Real Vulnerabilities and How to Fix Them

## TL;DR: 7 OpenClaw risks and fixes


- **Gateway exposure:** Bind to loopback, use token auth instead of passwords
- **Prompt injection:** Route untrusted content through a read-only reader agent, require approval for high-risk actions
- **Malicious skills:** Check source code before installing, use ClawSec or SecureClaw to automate checking
- **Session leakage:** Set` dmScope` to` per-channel-peer` so platforms don't share context
- **Credential sprawl:** Limit filesystem access to workspace only, remove token patterns from logs
- **Local browser risk:** Use a dedicated browser profile, or move the browser off your machine with Firecrawl Browser Sandbox
- **Config drift:** Run a config-guard script that restores a known-good` openclaw.json` after every restart


---


OpenClaw has some real security problems.


[A January 2026 audit found 512 vulnerabilities](https://docs.openclaw.ai/gateway/security) , 8 at the highest severity level.[A Bitsight scan](https://www.bitsight.com/blog/openclaw-ai-security-risks-exposed-instances) of over 40,000 public instances found 63% were open to remote attack.[A Shodan search](https://www.kaspersky.com/blog/openclaw-vulnerabilities-exposed/55263/) found nearly 1,000 instances running with no login required.


The corporate reaction was fast. Massive's CEO banned OpenClaw from all company hardware on January 26. Valere's president followed three days later. China's CNCERT blocked it in government systems, and a Meta executive threatened to fire anyone who installed it. All of this happened within three weeks of the tool hitting 175,000 GitHub stars.


If you're running OpenClaw or planning to, this guide covers seven known security risks with proof and a specific fix for each one.


## What is OpenClaw?


OpenClaw is an open-source[AI agent](https://www.firecrawl.dev/blog/ai-agents) platform that connects to your email, messaging apps, browser, and shell to automate tasks on your behalf. It runs as a self-hosted server (the "gateway") that accepts commands from any connected client.


That flexibility is also its biggest weakness. Every tool you give OpenClaw becomes a possible attack path, and the default settings leave most of those paths open. For a deeper look at how[Firecrawl integrates with OpenClaw](https://www.firecrawl.dev/blog/openclaw-firecrawl-guide) for web scraping and browser tasks, see the Firecrawl section later in this guide.


## What's at stake when OpenClaw gets compromised?


OpenClaw is **not** a chatbot with guardrails.


It's an agent with direct tool access, and every app you connect becomes a possible way in. Connect it to email and calendar and your inbox becomes a live attack target. Harmful content in an email doesn't need to trick you — just the agent reading it for you.


The same risk applies to every messaging platform.


By default, OpenClaw doesn't separate session data between WhatsApp, Telegram, Slack, or Discord. What one conversation can access, another can too. Give it shell access and filesystem tools (which a full OpenClaw setup does by default) and the risk grows.


A single browser tab can take full control of the agent, without you doing anything.


VPS and dedicated hardware setups make this worse. Both are popular (including the Mac Mini setup and the $5/month VPS with a dedicated eSIM for WhatsApp), and both make your gateway open to the internet. OpenClaw's security model was designed for one trusted user.


It was not designed for shared use, and thinking otherwise breaks the assumption the whole model depends on.


## 7 OpenClaw security risks and how to fix them


None of this changes how capable OpenClaw is. It can automate across email, messaging, browser, and shell in ways no other tool matches right now. The risks below are real, but they're all fixable — and once you've applied them, you get the full power of the tool without the exposure. Each risk below has a CVE (Common Vulnerabilities and Exposures identifier), a public report, or both. They're listed from most to least serious, starting with the one that requires zero user action.


### 1. Gateway exposure


The gateway is OpenClaw's entry point.


It's the server that waits for connections from your browser, messaging apps, and any other client that talks to the agent.


In February 2026, security firm Oasis Security disclosed a vulnerability in OpenClaw's gateway that they named ClawJacked (CVE-2026-32025). (Not to be confused with CVE-2026-25253, a separate token exfiltration bug patched a month earlier, has also been called ClawJacked).


The ClawJacked vulnerability scored a CVSS 8.8 out of 10 (CVSS is the standard severity scale for security vulnerabilities). It shows how any webpage you visit can take over your OpenClaw agent through that gateway, with no clicks or permissions needed.


Here's how the attack works.


A webpage's JavaScript opens a WebSocket to localhost. A WebSocket is a live, two-way connection between a browser and a server. Browsers normally block cross-domain requests as a security measure, but connections to localhost are treated as local and trusted, so they go through without that check. The script then rapidly guesses the gateway password. OpenClaw's limit on login attempts doesn't apply to localhost connections, so the script can try passwords without any delay.


Once it gets the password, it adds itself as a trusted device, and localhost devices are automatically approved.


Researcher Jamieson O'Reilly showed what a successful attack gives the attacker: Anthropic API tokens, Telegram bot tokens, Slack accounts, months of chat history, and the ability to send messages and run commands with full admin access. @fmdz387's Shodan scan confirmed how common this is: instances running with no authentication, fully open to the internet.


A few default settings make the password-guessing step even easier.


OpenClaw accepts single-character passwords like "a," which means the password-guessing script from the attack above can crack them in seconds. The` allowInsecureAuth` option turns off all login restrictions, so there's nothing to guess. And if you're running a reverse proxy (a server that sits in front of OpenClaw and forwards requests to it), a setup mistake can make external requests look like they're coming from localhost, skipping login checks the same way the ClawJacked exploit does.


The fix goes in` ~/.openclaw/openclaw.json` , the main config file for your OpenClaw instance.


Set` gateway.bind` to` loopback` (the default), which means the gateway only accepts connections from your own machine. Add strong token-based login so the password-guessing attack has nothing to try:


```text
{
"gateway"  :   {
"bind"  :   "loopback"  ,
"auth"  :   {
"mode"  :   "token"  ,
"token"  :   "<your-random-token>"
}
}
}
```


Options like` lan` ,` tailnet` , and` custom` allow more devices to connect and need a good reason.


If you need to access OpenClaw from another device, Tailscale Serve creates an encrypted tunnel to your gateway without opening it to the internet. For VPS setups, SSH tunneling does the same thing:` ssh -L 18789:127.0.0.1:18789 user@your-vps` forwards your local port to the remote machine through an encrypted connection.


If you're running OpenClaw in Docker, be aware that Docker's port publishing bypasses your host firewall rules entirely, so firewall rules you've set up won't protect the gateway port. Docker has its own firewall layer called the DOCKER-USER chain, and you can add a rule there to block external traffic from reaching the gateway. To allow only your own machine to connect, run:


```text
# First, allow already-established connections through
iptables   -I   DOCKER-USER   -p   tcp   -m   conntrack   --ctstate   ESTABLISHED,RELATED   -j   ACCEPT
# Then block external traffic to the gateway port based on original source
iptables   -I   DOCKER-USER   -p   tcp   -m   conntrack   --ctorigdstport   18789   !   --ctorigsrc   127.0.0.1   -j   DROP
```


This tells Docker's firewall to drop any connection to port 18789 that doesn't come from localhost.


### 2. Prompt injection


OpenClaw's own security docs say that system prompt rules are suggestions, not rules the agent has to follow.


When the agent processes too much content, it starts dropping older instructions to fit the new content. Too many emails can push the system prompt out of the model's memory completely. Harmful content in emails, messages, or web pages doesn't need to get past a security check. It just needs to arrive when the context is full enough that the model forgets its rules.


The results are public.


Summer Yue, Meta's director of AI alignment, connected OpenClaw to her email and the agent started deleting everything in what she described as a "speed run." She had to physically run to her Mac Mini to stop it, and it ignored stop commands.[TechCrunch quoted her reaction](https://techcrunch.com/2026/02/23/openclaw-inbox-incident/) : "Rookie mistake tbh."


Other researchers ran planned attacks.


[Kukuy, CEO of Archestra.AI](https://x.com/Mkukkk/status/2015951362270310879) , sent an injection email that caused the agent to send a private crypto token. A separate experiment caused a bot to forward messages to an attacker address without asking for permission. Testing on a "fully updated" instance showed an[80% takeover rate](https://arxiv.org/html/2603.10387v1) through prompt injection.


The backend model plays into this:


- Claude defends against injection[83% of the time](https://arxiv.org/html/2603.10387v1)
- GPT at 58%
- DeepSeek at 17%


That 66-point difference is not random, given how expensive it is to run OpenClaw and most people choosing open-source models.


The reader agent pattern is the main fix.


In OpenClaw, the main agent is the one with access to your tools, files, and messaging platforms. If it reads a malicious email directly, that content sits in the same context window as its instructions, and can override them. The reader agent pattern adds a buffer: untrusted content goes to a separate, read-only agent with no tools attached, which summarizes it first. Only that summary reaches the main agent, so the original malicious content never touches the tool layer. Add human approval rules on top of that.


Split actions into two tiers.


Low-risk actions (summarize, classify, draft, extract) can run automatically and record what they did. High-risk actions (send messages, change records, install skills, shell commands, anything involving money or passwords) should require your approval before they run. Use` tools.exec.ask: "always"` to require approval for every command, or` tools.exec.security: "deny"` to block commands entirely.


### 3. Malicious skills / supply chain


A compromised skill can change how your agent behaves permanently.


Skills are third-party add-ons that add new abilities to your OpenClaw agent. When you install one, it runs as trusted code inside the gateway process itself. That means a malicious skill has the same access as OpenClaw's own code. It can rewrite` SOUL.md` , the file that defines your agent's personality and behavior.


Because` SOUL.md` stays after chat resets, a poisoned version stays after restarts.


If you install a skill with` npm` , its install scripts run automatically before you can review anything. ClawHub, the community skill marketplace, has no categories, filters, or review process. It's an open upload directory, not a reviewed registry.


If you install a skill from ClawHub without checking it first, you're likely to hit a bad one.


[The ClawHavoc campaign](https://www.trendmicro.com/en_us/research/26/b/openclaw-skills-used-to-distribute-atomic-macos-stealer.html) found 1,184 malicious skills on ClawHub and 2,200+ across GitHub, roughly one in five packages in the ecosystem. The attack method was` SKILL.md` files: markdown files the agent reads and follows as instructions, turning the agent into a middleman for fake setup steps. This technique, called ClickFix, installed AMOS (Atomic macOS Stealer), malware that steals browser credentials, keychain passwords, crypto wallets, SSH keys, and API tokens from macOS.


*A malicious SKILL.md file from the ClawHavoc campaign. The agent reads the markdown as trusted instructions and runs the embedded download command. Source: Trend Micro.*


[A Snyk scan](https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/) of around 4,000 skills found 36% with vulnerabilities and 76 that were malware.


Read the source code before turning on any skill, and pin to exact versions.


Don't trust ClawHub until you've checked a skill's source directly. For a broader look at the OpenClaw skill ecosystem, including a[skill vetter that scans for malicious patterns](https://www.firecrawl.dev/blog/openclaw-skills) , see Firecrawl's guide to OpenClaw skills. If you want to automate skill checking, three community tools exist:


- ClawSec checks whether skill files have been changed without your knowledge and alerts you if` SOUL.md` or` IDENTITY.md` change.
- SecureClaw runs a wider check: 56 checks across security and monitoring, based on AI security standards (OWASP ASI Top 10 and MITRE ATLAS).
- openclaw-security-monitor scans for known malware patterns, including AMOS stealer traces and connections to attacker-controlled servers.


### 4. Session data leakage


The default session config was designed for single-user setups.


By default, OpenClaw puts all DMs into one shared session that stays open, no matter which platform they come from. For solo use that's fine, but for any setup where more than one person can message the bot, it means API tokens loaded in one conversation are accessible from another. Files written during a Telegram session can be read from a Discord session.


Token exposure adds more risk.


Access tokens appear in URL query parameters by default, so anyone with access to browser history, server logs, or unencrypted traffic can see them. Group chats create another way in. Tools used in a group can access environment variables, API tokens, and config files. Indirect prompt injection can cause the agent to create a URL controlled by the attacker with private data in the URL.


Link previews in Telegram and Discord load that URL automatically, without anyone clicking it.


Fix session separation first. In OpenClaw, a session is the active memory and tool context for a conversation, including any loaded API tokens, file handles, or environment variables. By default, all direct messages share one session regardless of which platform they came from, meaning a Telegram conversation and a Discord conversation are drawing from the same pool of data.


Add this to` ~/.openclaw/openclaw.json` :


```text
{
"session"  :   {
"dmScope"  :   "per-channel-peer"
}
}
```


This gives each channel-and-sender pair its own separate session, so a conversation on Telegram can't read data from a conversation on Discord. For multi-account setups, use` per-account-channel-peer` to separate even further by account. For group chats, add these restrictions to the same` openclaw.json` file:


- Set` workspaceAccess: "none"` to block file access
- Deny` group:runtime` and` group:fs` permissions to prevent shell and filesystem access
- Deny` sessions_spawn` to stop the agent from creating new sessions on its own


For DM pairing, switch to` allowlist` mode so only senders you've approved can message the bot. The default` pairing` mode gives unknown senders a one-hour code to pair, which may be too open.


### 5. Credential sprawl


Any agent with shell access can read` .env` files.


That's just how shell access works. If your OpenClaw setup has filesystem tools turned on and stores API tokens or OAuth credentials in plain text files, those secrets are readable. Agents also paste full environment variables into logs, chat windows, or external servers during debugging sessions.


This is part of a well-documented pattern.[GitGuardian tracked 12.8 million secrets](https://www.gitguardian.com/state-of-secrets-sprawl-report-2024) leaked on public GitHub in 2023, up 28% from the year before. Most of those leaks came from the same root cause: credentials stored in plain text, showing up in logs, debug output, or chat history. OpenClaw's default behavior recreates that same condition locally, with an agent that has shell access and logs everything it does.


*12.8 million new secrets detected in public GitHub commits in 2023, up 28% year over year. Source: GitGuardian State of Secrets Sprawl 2024.*


The risk also covers OpenClaw's own files. Session logs at` ~/.openclaw/agents/<agentId>/sessions/*.jsonl` can include pasted secrets, file contents, and command results. Any process with filesystem access can read them. Log files at` /tmp/openclaw/openclaw-YYYY-MM-DD.log` have the same problem if` logging.redactSensitive` is off.


If your workspace root is too wide (a home directory, for example), it gives filesystem tools access to` ~/.openclaw` state and config.


To secure OpenClaw against credential leaks, the fix has two parts: limit what the agent can see, and clean what it logs.


First, limit filesystem access. Add this to` ~/.openclaw/openclaw.json` so the agent can only read files inside your project directory, not your entire home folder:


```text
{
"tools"  :   {
"fs"  :   {   "workspaceOnly"  :   true   }  ,
"exec"  :   {   "applyPatch"  :   {   "workspaceOnly"  :   true   } }
}  ,
"logging"  :   {
"redactSensitive"  :   "tools"  ,
"redactPatterns"  :   [  "fc-[A-Za-z0-9]+"  ,   "sk-[A-Za-z0-9]+"  ]
}
}
```


The` redactPatterns` entries tell OpenClaw to remove anything matching those patterns from logs. Add entries for your own token formats (Anthropic keys start with` sk-ant-` , OpenAI keys with` sk-` ). Restrict file permissions on OpenClaw's own config by running` chmod 600 ~/.openclaw/openclaw.json` and` chmod 700 ~/.openclaw/` so only your user account can read them.


For API tokens the agent needs to use (not just hide), give it read-only tokens with short expiration times instead of permanent admin tokens. Use separate credentials for each project so a hacked agent in one project can't access another.


### 6. Local browser risk


When OpenClaw drives a browser, it drives your browser.


That's the one with your logged-in sessions, saved credentials, and cookies. Any page the agent visits can reach anything that profile can reach. This applies to the standard OpenClaw setup for any task that uses web research or form filling. OpenClaw's browser also defaults to allowing requests to private network addresses (internal IPs like 192.168.x.x or 10.x.x.x).


An attacker who controls what the agent browses can use this to scan services on your local network.


If you're running OpenClaw in Docker, there's an added problem. Security recommendations say to remove all extra Linux permissions from the container, but Chromium needs extra permissions to run. You can't have a locked-down container and a local browser at the same time.


If you need a local browser, use a dedicated browser profile with sync and password managers disabled, so the agent never uses your real sessions. For a cleaner fix that moves the browser off your machine entirely, see the Firecrawl Browser Sandbox setup below.


### 7. Config drift


OpenClaw rewrites` openclaw.json` on startup, during` openclaw doctor` , and when the setup wizard runs, removing your custom settings in the process.


WhatsApp` allowFrom` entries, agent bindings, changed tokens: every security step from items 1 through 6 can be quietly removed on the next restart. This isn't a one-time problem. Any reboot, doctor run, or wizard run can erase your security settings without warning.


The fix is a config-guard script that checks` openclaw.json` against a known-good reference copy before each startup.


If it passes, it saves the reference copy. If it fails, it backs up the broken config and restores the good one. Add a cron job that watches for changes:


```text
# config-watch cron: runs every minute
*   *   *   *   *   /usr/local/bin/openclaw-config-guard.sh


# Docker entrypoint pattern:
# 1. Clean up any leftover SingletonLock files
# 2. Start OpenClaw
# 3. Wait 15 seconds for OpenClaw's startup rewrite to finish
# 4. Overwrite with golden config
# 5. Restart OpenClaw to pick up restored settings
```


The 15-second delay in the Docker entrypoint makes a difference.


It lets OpenClaw finish its startup rewrite, then replaces the result with your known-good version. If you want to verify your config after any change, run` openclaw security audit --deep` . It checks your setup against known risks and tells you what to fix. Add` --fix` to let it automatically fix safe issues.


## What about browser and web scraping risks?


Fixes 1 through 7 are all config fixes.


But OpenClaw also has a browser and web scraping layer that sits outside` openclaw.json` , and that layer has three problems that config changes alone can't fix.


### Why do JavaScript-heavy pages return garbage?


OpenClaw's built-in` web_fetch` makes a plain HTTP GET and tries to get content with Readability.


It does not run JavaScript. For any page that loads content with JavaScript (most modern sites), the agent gets nav links, cookie banners, and whatever static HTML was in the first page load.


[Firecrawl](https://www.firecrawl.dev/blog/openclaw-web-search) is the context API to search, scrape, and interact with the web at scale. It renders pages in a remote headless browser and returns clean, structured content.


It connects to OpenClaw's backup fetch system as a scraper that can run JavaScript. When Readability fails, OpenClaw sends the URL to Firecrawl if an API key is configured, which renders the page and returns the result. Add this to` ~/.openclaw/openclaw.json` :


```text
{
"tools"  :   {
"web"  :   {
"fetch"  :   {
"firecrawl"  :   {
"apiKey"  :   "fc-YOUR-API-KEY"  ,
"onlyMainContent"  :   true  ,
"maxAgeMs"  :   172800000
}
}
}
}
}
```


` onlyMainContent: true` removes page clutter (headers, footers, nav bars) from every response.` maxAgeMs: 172800000` sets a 48-hour cache period, which reduces repeated fetches when you search the same sites again. You can reduce to 86400000 (24h) for fresher results. See the[Firecrawl OpenClaw integration docs](https://docs.firecrawl.dev/developer-guides/openclaw) for the full setup reference.


For security-sensitive workflows where you need to guarantee no live outbound requests — for example, when an LLM might be deciding which URLs to scrape — Firecrawl's[Lockdown Mode](https://www.firecrawl.dev/blog/lockdown-mode-launch) restricts` /scrape` to cache-only results, so a prompt-injected URL can never reach an attacker-controlled server.


### Why does web search return titles but no content?


OpenClaw's native` web_search` returns a title, URL, and snippet per result.


To get actual page content, the agent has to call` web_fetch` on each URL one by one. That's where 403 errors and JavaScript-rendering failures add up, often without the agent telling you. Firecrawl's search skill returns full page content with search results in a single call.


As of April 2026, Firecrawl is now a default search provider in OpenClaw — no extra install step required if you have an API key configured.


For older versions, install the Firecrawl CLI with:


```text
npx   -y   firecrawl-cli@latest   init   --all
```


Unlike other[OpenClaw search providers](https://www.firecrawl.dev/blog/best-openclaw-search-providers) like Brave, Gemini, Perplexity, and Grok (which return titles, URLs, and snippets or AI-generated answers), Firecrawl returns search results and full scraped page content together in a single call.


### How does the local browser expose your sessions?


Fix #6 covered why running a local browser through OpenClaw is risky.


The agent uses your browser profile with all its saved sessions, cookies, and credentials. In Docker, locked-down containers and local Chromium are incompatible because of how their security works. The[Firecrawl Browser Sandbox](https://www.firecrawl.dev/blog/openclaw-firecrawl-guide) moves the browser off your machine entirely. Each session runs in a remote, temporary environment.


No local Chromium to install, no extra permissions, and no shared browser data between sessions. You learn more about when and why you should use AI sanboxes in our article on[AI sandboxes](https://www.firecrawl.dev/blog/ai-agent-sandbox) .


Local browser Firecrawl Browser Sandbox


Local Chromium required Yes No


Shares logged-in sessions Yes No (disposable per session)


SYS_ADMIN in Docker Required Not needed


Parallel sessions RAM-limited, often fails Real parallelism


Output to agent Raw DOM / screenshots Clean markdown output


To set it up, run:


```text
npx   -y   firecrawl-cli   init   --browser
```


See the[Browser Sandbox docs](https://docs.firecrawl.dev/features/browser) for session limits, TTL settings, and advanced configuration. You can try Firecrawl without an API key to get started, and sign up at[firecrawl.dev](https://www.firecrawl.dev/) for a key with higher rate limits and more credits.


## Conclusion


The seven risks in this guide are all known, all repeatable, and all fixable before your next session.


Start with gateway binding and token auth, which take five minutes and fix the most serious risk. Then set up session separation, credential separation, and the config-guard script. Add the reader agent pattern for any setup that uses email or messaging.


The Firecrawl integrations are optional but worth setting up for web research, since the browser sandbox removes local Chromium exposure. Apply the fixes in order, verify with` openclaw security audit --deep` , and you have a setup that matches the actual risks.


## References


- [OpenClaw Security Docs & Hardened Baseline JSON](https://docs.openclaw.ai/gateway/security)
- [ClawJacked Vulnerability (CVE-2026-32025)](https://www.oasis.security/blog/openclaw-vulnerability)
- [Kaspersky: OpenClaw Vulnerabilities Exposed](https://www.kaspersky.com/blog/openclaw-vulnerabilities-exposed/55263/) — covers O'Reilly, @fmdz387 Shodan scan, Kukuy injection demo
- [Kukuy Prompt Injection Demo (X post)](https://x.com/Mkukkk/status/2015951362270310879)
- [Bitsight: OpenClaw Exposed Instances](https://www.bitsight.com/blog/openclaw-ai-security-risks-exposed-instances)
- [80% Hijacking Success Rate (arXiv)](https://arxiv.org/html/2603.10387v1)
- [TechCrunch: OpenClaw Inbox Incident](https://techcrunch.com/2026/02/23/openclaw-inbox-incident/)
- [ClawHavoc Campaign (Trend Micro)](https://www.trendmicro.com/en_us/research/26/b/openclaw-skills-used-to-distribute-atomic-macos-stealer.html)
- [Snyk ToxicSkills Study](https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/)
- [ClawSec](https://github.com/prompt-security/clawsec)
- [SecureClaw (Adversa AI)](https://adversa.ai/)
- [openclaw-security-monitor](https://github.com/adibirzu/openclaw-security-monitor)
- [GitGuardian: State of Secrets Sprawl 2024](https://www.gitguardian.com/state-of-secrets-sprawl-report-2024)
- [Tailscale Serve](https://tailscale.com/kb/1312/serve)
- [Federico Viticci's OpenClaw Review (MacStories)](https://www.macstories.net/stories/clawdbot-showed-me-what-the-future-of-personal-ai-assistants-looks-like/)
