---
schema_version: "1.0.0"
document_id: "6592d24dee17e379e2ea3ebead2df95dc471015954ff020244018ded832a8f0d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-v2026-5-25"
published_at: "2026-05-28T00:14:09+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:03.604597+00:00"
content_hash: "sha256:e3d3cf1157ba0d0beb53f0a244659c21a14fee14264edd1e7c27b1c5a12ae8d5"
---

# OpenClaw v2026.5.25: iMessage Fixes, Alpine Linux, and MCP Hang Protection

## Alpine Linux: Finally Works Again


Docker-based OpenClaw deployments on Alpine Linux were quietly broken. The installer fell through to glibc Node tarballs, which fail on Alpine's musl libc — specifically when loading` node:sqlite` . The errors were hard to trace and had no clear path to resolution.


v2026.5.25 switches the Alpine install path to use` apk` — Alpine's native package manager. Node.js, npm, and Git now install from the correct package source and work as expected on musl systems.


Alpine is the default base image for many production container setups because of its small footprint and security posture. A broken installer was a silent blocker for every team trying to run OpenClaw in container-first production environments. This fix makes Alpine a first-class target again.


If you run **[Blink Claw](https://blink.new/claw)** — Blink's managed OpenClaw hosting — this fix was applied automatically. No Dockerfile changes, no version tracking, no maintenance window. Security patches and version upgrades ship to Blink Claw without any manual action on your part.


## iMessage: Two Fixes for Real Users


The v2026.5.25-beta.1 release adds two iMessage-specific fixes that address reported community problems directly.


**Attachment routing** : Attachments stored under` ~/Library/Messages/Attachments` , including wildcard-rooted paths, were rejected by the path policy instead of flowing through the inbound attachment pipeline. This silently dropped photo shares and file attachments in iMessage agent workflows. If your iMessage-attached files weren't reaching your OpenClaw agent, this is why.


**Duplicate watcher deduplication** : When` channels.imessage.accounts` listed both a default account and a named account pointing at the same Messages source, OpenClaw spawned duplicate` imsg rpc` processes. Each process sent doubled inbound replies. The fix deduplicates at startup while keeping both accounts usable for outbound sends.


Both fixes reflect how far the ecosystem has matured. OpenClaw users now run multi-account, group-chat, and attachment-heavy iMessage workflows — setups that clearly weren't the original use case. The platform is growing into its user base.


## More Changes Worth Knowing


**OpenRouter context limits** : OpenClaw was using incorrect context window sizes for OpenRouter-routed models because it didn't read endpoint-specific` top_provider` metadata. This caused silent truncation and hard-to-diagnose API errors on longer sessions. The fix reads actual per-endpoint context sizes.


**Config and secrets** :` exec` SecretRef IDs can now include` #` selectors. AWS-style` secret#json_key` patterns now validate correctly throughout the config — removing a real blocker for teams pulling structured secrets from AWS Secrets Manager.


**Performance caching** : Manifest-backed CLI provider descriptors and fallback provider resolution are now cached across plugin reloads. This extends the performance work that began with the[4,100× /models speedup in v2026.5.22](https://blink.new/blog/openclaw-v2026-5-22-release) , reducing overhead on hot paths without trading staleness across plugin reloads.


## How to Upgrade


Self-hosted users run one command:


```text
npm   update   -g   openclaw
```


Run OpenClaw without the hassle —[Blink Claw](https://blink.new/claw) handles everything from $22/mo. LLM costs are included via 200+ model router. No Docker needed, no VPS setup. Security patches applied automatically — you never track CVEs.


Full commit details live in the[v2026.5.25 release notes on GitHub](https://github.com/openclaw/openclaw/releases/tag/v2026.5.25) . The previous release covered[v2026.5.22's meeting notes plugin and 4,100× model speedup](https://blink.new/blog/openclaw-v2026-5-22-release) .


No. The timeout only bounds servers that fail to respond within the discovery window. Healthy MCP servers respond immediately and see no change in behavior. Sessions where all MCP servers are healthy perform identically to before v2026.5.25.


No. The fix changes how the installer resolves packages on Alpine. Existing working installs are not affected. Only fresh installs on Alpine Linux benefit — they now complete successfully instead of failing on` node:sqlite` .


Yes. The attachment routing fix and duplicate watcher deduplication ship in v2026.5.25-beta.1. The stable v2026.5.25 release contains MCP hang protection, Alpine installer, OpenRouter context corrections, and provider caching. Install the latest available tag to get all fixes.


No. Blink Claw auto-applies every OpenClaw update as it ships — no` npm update` required, no version tracking, no maintenance overhead. Run OpenClaw without the hassle —[Blink Claw](https://blink.new/claw) handles everything from $22/mo, LLM costs included.


Run` openclaw --debug` and look for timeout logs during the MCP discovery phase. Each server's response time appears in the log output. Review your[OpenClaw skills](https://blink.new/blog/best-openclaw-skills) and MCP server configurations to identify slow or unresponsive connections.
