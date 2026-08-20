---
schema_version: "1.0.0"
document_id: "daae2f81a0b20b552873f36f52c7d2e904253862e4c6f60154d3dd28e22b5cc1"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/april-2026-security-beat-same-actors-new-targets"
published_at: "2026-06-04T16:16:30+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:f5d746ebc23b52f5c379a902f28247d7519aec89d4b683c479079613384adb4c"
---

# April 2026 Security Beat: Same Actors, New Targets

## April was undoubtedly a rocky month in security.


$635M was lost across 28 crypto incidents. The Axios npm package was compromised on day one, exposing an estimated 600,000 installs in three hours. Vercel was breached through a third party. Three major CVEs under active exploitation.


Here's the month in security 👇


‍


### **Crypto: $635.24M lost across 28 incidents**


Category Loss Incidents


Smart Contract $328.14M 23


Social Engineering $286.60M 3


Infrastructure / Custody $20.50M 2


**Total** **$635.24M** **28**


‍


### **Two events did most of the damage.**


**Kelp, April 18: ~$293M.** LayerZero OFT bridge exploit on Ethereum and Arbitrum. Source:[Chainalysis](https://www.chainalysis.com/blog/kelpdao-bridge-exploit-april-2026/) .


**Drift Protocol, April 1: $285M.** DPRK-linked operation involving a six-month patient infiltration:


- Attackers built a fully documented identity as a quantitative trading firm (website, LinkedIn profiles, trading history) starting in Fall 2025.
- Approached Drift contributors at multiple crypto conferences in person. Maintained ongoing contact via Telegram, working sessions, and additional in-person meetings at global events.
- Onboarded a vault on Drift and deposited >$1M of their own capital. Ran real trades. Participated in detailed strategy and product discussions with multiple contributors.
- Exploited Solana's durable nonce feature to get at least two Drift Security Council members to blind-sign transactions they did not fully understand. Those transactions silently transferred admin control to the attackers.
- The face-to-face operatives are not believed to be North Korean nationals. DPRK groups deploy third-party intermediaries with verifiable professional backgrounds for in-person work.
- Drift attributes the operation with medium-high confidence to the same actors behind the $50M Radiant Capital hack (Mandiant tracking: UNC4736 / AppleJeus / Citrine Sleet).


Sources:[Chainalysis](https://www.chainalysis.com/blog/lessons-from-the-drift-hack/) |[CoinDesk](https://www.coindesk.com/markets/2026/04/05/drift-says-usd270-million-exploit-was-a-six-month-north-korean-intelligence-operation) |[The Block](https://www.theblock.co/post/396361/drift-links-280-million-exploit-to-six-month-social-engineering-op-run-by-suspected-north-korean-actors) .


Excluding Kelp and Drift, the remaining 26 incidents totaled $57.24M.


‍


### **The Axios supply chain compromise**


**On March 31, The lead Axios npm maintainer was socially engineered. They hijacked his npm and GitHub accounts, and pushed two malicious versions of the Axios package (v1.14.1 and v0.30.4).** Axios regularly pulls 80–100M downloads per week. The package was live and malicious for roughly three hours, where an estimated ~600,000 installs landed in that window. Huntress observed at least 135 endpoints across all operating systems beaconing to the attacker's command and control infrastructure.


**For crypto teams, this is direct exposure.** Axios sits in almost any JavaScript codebase that touches HTTP, covering most crypto frontends, signing servers, oracle relayers, indexers, and devops scripts.


Sources:[Huntress](https://www.huntress.com/blog/axios-npm-compromise) ,[SecurityScorecard](https://securityscorecard.com/blog/lazarus-group-targets-developers-through-npm-packages-and-supply-chain-attacks/) ,[SANS](https://www.sans.org/blog/what-we-learned-axios-npm-supply-chain-compromise-emergency-briefing) .


‍


### **Vercel breach via Context.ai**


**On April 19, Vercel disclosed an intrusion that started with a compromise at Context.ai, a third-party tool integrated into Vercel's stack.** Attackers claim they grabbed access keys, source code, API keys, internal deployment credentials, and database contents.


Vercel hosts a large share of crypto frontends. If your team deploys to Vercel and Vercel uses Context.ai, make sure to check if you were affected.


Sources:[https://vercel.com/kb/bulletin/vercel-april-2026-security-incident](https://vercel.com/kb/bulletin/vercel-april-2026-security-incident)


‍


### **Active zero-days CISA wants you to patch**


Three CVEs from April that matter for any team running enterprise infrastructure:


**CVE-2026-31431, Linux kernel ("Copy Fail").** CVSS 7.8, but the score understates the severity. Local privilege escalation in the kernel's algif_aead module (AF_ALG userspace crypto API). A 732-byte Python script lets an unprivileged local user gain root on virtually any Linux distribution shipped since 2017 (Ubuntu, RHEL, Amazon Linux, SUSE, Debian, Fedora, Arch). The exploit modifies the kernel's page cache copy of a setuid binary without touching the file on disk, so standard disk forensics will not detect the alteration. High container breakout and multi-tenant compromise risk on cloud infrastructure. Patches are out from major distros; AF_ALG socket creation can be blocked as an interim mitigation.[Sysdig](https://www.sysdig.com/blog/cve-2026-31431-copy-fail-linux-kernel-flaw-lets-local-users-gain-root-in-seconds) |[Microsoft](https://www.microsoft.com/en-us/security/blog/2026/05/01/cve-2026-31431-copy-fail-vulnerability-enables-linux-root-privilege-escalation/) |[Help Net Security](https://www.helpnetsecurity.com/2026/04/30/copyfail-linux-lpe-vulnerability-cve-2026-31431/) .


**CVE-2026-35616, Fortinet FortiClient EMS.** CVSS 9.1. Improper access control in the EMS API lets attackers bypass authentication and run code on the server with no valid credentials or user interaction. WatchTowr's sensors caught active exploitation on March 31, before Fortinet's advisory dropped on April 4. CISA added it to the KEV catalog April 6.[watchTowr](https://watchtowr.com/resources/fortinet-forticlient-ems-zero-day-cve-2026-35616-active-exploitation-underway/) |[CyberScoop](https://cyberscoop.com/fortinet-forticlient-ems-zero-day-cve-2026-35616-hotfix-known-exploited/) .


**CVE-2026-32202, Microsoft Windows Shell.** Spoofing vulnerability with confirmed active exploitation. CISA mandated federal patching by May 12.[LufSec writeup](https://blog.lufsec.com/cisa-alerts-on-cve-2026-32202-windows-shell-zero-day-under-active-exploitation/) .


CISA added eight new vulnerabilities to the KEV catalog in April, with deadlines spanning April and May.[The Hacker News summary](https://thehackernews.com/2026/04/cisa-adds-8-exploited-flaws-to-kev-sets.html) .


‍


### **AI agent security: the threat surface nobody is paying attention to yet**


**Google reported a 32% increase in malicious indirect prompt injection detections between November 2025 and February 2026.** The attack hides instructions inside content that an AI agent will read (web pages, PR descriptions, emails, documents) and waits for the agent to obey them.


Three vulnerabilities from the past few months that any team using AI agents should know about:


- **CVE-2025-53773, GitHub Copilot.** CVSS 9.6. Hidden prompt injection in pull request descriptions enabled remote code execution. Copilot is in active use across most professional dev teams.
- **EchoLeak, Microsoft 365 Copilot.** Zero-click prompt injection that silently exfiltrates enterprise data. No user action required.
- **Comment and Control, AI code review agents from Anthropic, Google, and Microsoft.** Malicious instructions hidden in PR titles bypassed the agent's instructions and triggered unintended actions.


OWASP still ranks prompt injection as the #1 LLM vulnerability (LLM01). Any team using AI for code review, diff summaries, or processing external content has a new piece of attack surface to defend. Check out[OWASP’s top 10 for Agentic Applications](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/) .


‍


### **The pattern across the month**


Three things ran through April:


1. **Privileged-access compromises beat code exploits in dollars and impact.** The attack surface this month has been through identity, credentials, and trust.
2. **Third parties are the entry point.** The perimeter you're defending needs to include every dependency, vendor, and contractor that touches your stack.
3. **DPRK is operating on every front.**


‍


### **Recommendations**


For protocol teams, exchanges, and any company with funds onchain or admin keys:


1. **Build admin custody so one compromise doesn't end the protocol.** No single EOA should be holding ADMIN_ROLE. Multisigs should have geographically distributed signers. Timelocks on every privileged action. If a successful admin compromise can move funds inside an hour, your custody architecture has a structural problem.
2. **Audit your dependency tree as carefully as you audit your code.** Pin npm versions. Enforce lockfile integrity in CI. Add a reputable supply chain security platform to your build pipeline, such as Aikido Security, Socket, or Snyk. Set up alerts for new versions of critical dependencies before they land in your build. Axios was malicious for only three hours, and an estimated 600,000 installs happened in that window.
3. **Assume Lazarus is targeting your engineers right now.** They run months-long operations against named engineers at protocols they've picked. Train your team on the actual playbook: fake recruiters on LinkedIn, fake job interviews that ship malicious npm packages or Zoom downloads, fake bug bounty submissions with PoC code that runs on the reviewer's machine. Make this training mandatory and quarterly.
4. **Patch CVE-2026-31431** (Linux kernel "Copy Fail"), Patch CVE-2026-35616 (Fortinet) and CVE-2026-32202 (Windows Shell) now. All three are in active exploitation. They're how attackers reach your team, your servers, and your keys before they ever look at your code.
5. **Audit your AI tooling.** It's a new attack surface, and almost nobody is treating it that way. Any AI that reviews code, summarizes diffs, or processes external content should be sandboxed. Don't let it execute commands. Don't treat its outputs as authoritative. Read[OWASP’s Top 10](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/) and put one person on your team in charge of AI agent risk.
6. **Map your third-party trust graph.** List every vendor with access keys, deployment permissions, or read access to your code. For each one, answer: if they get compromised, what can the attacker do next? If you can't answer these, you're in the position Vercel was in before April 19.


‍


###### **Disclaimer**


This report aggregates publicly reported information as of the publication date and may be revised as investigations evolve and post-mortems are released. Recommendations are general guidance. Verify against primary sources before acting on any specific claim.


‍


###### **About This Series**


Quantstamp publishes the Security Beat monthly. We've conducted 1,300+ audits and secured $500B+ in digital assets across 250+ clients, including Ethereum Foundation, Aave, Polymarket, Ethena, Visa, OpenSea, Maker, Curve, Compound, and Lido. If you’d like to chat about anything security or request an audit, check out[quantstamp.com](https://quantstamp.com/) .


‍
