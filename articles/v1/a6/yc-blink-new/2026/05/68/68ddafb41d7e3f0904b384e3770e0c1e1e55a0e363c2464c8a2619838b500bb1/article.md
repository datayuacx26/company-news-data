---
schema_version: "1.0.0"
document_id: "68ddafb41d7e3f0904b384e3770e0c1e1e55a0e363c2464c8a2619838b500bb1"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-v5-18-may-2026"
published_at: "2026-05-20T12:55:07+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:04675d0e4168d051e24b2a1bf799a31efbe138fd8d61d1b14a08c6ddafb3430a"
---

# OpenClaw v2026.5.18: What's New in May 2026 (Stable Rollup)

OpenClaw shipped two major releases in May 2026. v2026.5.12 arrived May 14th. v2026.5.18 followed May 18th as the stable rollup.


Both releases are significant. Material improvements to voice, messaging, plugin development, and security.


## v2026.5.12: Leaner, Faster, More Resilient (May 14)


**Leaner installs.** WhatsApp, Slack, Amazon Bedrock, Anthropic Vertex, and related providers moved out of the core runtime. OpenClaw installs no longer pull AWS SDK dependencies unless you actually use Bedrock. Fresh installs are meaningfully smaller.


**Telegram got much more resilient.** Polling now runs in an isolated worker with a durable local spool. A main event-loop stall can no longer freeze Telegram ingress. Formatted replies (HTML, Markdown links) survive lazy delivery paths.


**Plugin system hardened.** pnpm 11 support landed. Peer dependency preservation improved.


**Per-sender tool policies.** Operators can now restrict dangerous tools by requester identity globally, per-agent, per-group, or by channel.


### Key Numbers


- **5 provider packages** externalized from core (WhatsApp, Slack, Bedrock, Vertex, OpenShell sandbox)
- **3 Telegram fixes** : isolated polling, durable spooling, group media handling
- **pnpm 11** now supported for plugin installs


## v2026.5.18: The Stable Rollup (May 18)


This is the release to update to. It includes the 2026.5.14 and 2026.5.17 beta trains plus final fixes.


### Android Talk Mode to Realtime Gateway Relay


Android Talk Mode now routes through realtime Gateway relay voice sessions with streaming mic input, realtime audio playback, tool-result bridging, and on-screen transcripts. Voice sessions on Android now work the same way desktop sessions do.


### New Plugin SDK CLI


Developers building OpenClaw plugins get a new first-class development workflow:


- **openclaw plugins init** : scaffold a new plugin
- **openclaw plugins build** : bundle plugin with typed tool helpers
- **openclaw plugins validate** : check manifest, tool declarations


The new` defineToolPlugin` helper brings typed simple tool plugins with generated manifest metadata.


### New Skills


Three new skills shipped with v2026.5.18:


- **Meme Maker** : curated template search, local SVG/PNG rendering
- **Python Debugging** : pdb, breakpoint(), post-mortem inspection, debugpy remote attach
- **Node Inspector Debugging** : Node.js debugging, fused diagram generation
- **Obsidian skill** updated to the official Obsidian CLI


### Mac App Redesign


- Consistent card layouts across all Settings panes
- Cached navigation (switching tabs no longer blanks content)
- Dashboard, Chat, Canvas, and Settings shortcuts in the Dock icon menu


### Codex and OpenAI Improvements


GPT-5.1, GPT-5.2, and GPT-5.3 model refs now pass config validation. The` max_completion_tokens` parameter is now honored on inbound` /v1/chat/completions` requests.


### Security Hardening


- Exec approval realpath binding tightened
- Docker non-loopback fail-closed behavior added
- SSRF/private-network guardrails improved
- Sandbox credential-bearing binds blocked on Windows USERPROFILE


### Telegram Reliability


- Forum topic delivery preserved across requester-agent handoffs
- HTTP 421 Misdirected Request retried on fresh fallback transport
