---
schema_version: "1.0.0"
document_id: "5848cd9319606bca4c1cf47400b44b650f4b1edc192d468a6bf1c034d23683db"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-whats-new-may-2026"
published_at: "2026-05-07T12:15:00+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:16.827560+00:00"
content_hash: "sha256:9b0fa06be0d6a2576123dbe46e95a2e4cd5db94fa60f5b1be4da68aeab6762bf"
---

# What's New in OpenClaw May 2026: v2026.5.3, v2026.5.4, and v2026.5.5

## v2026.5.4: Google Meet Gets a Real Voice Bridge


The headliner in v2026.5.4 is **Twilio dial-in for Google Meet** . Previously, Meet voice agents used a basic TwiML fallback path. Now, Twilio dial-in joins the realtime Gemini voice bridge — with paced audio streaming, backpressure-aware buffering, and barge-in queue clearing.


The practical result: Meet participants get a much snappier OpenClaw voice agent. The agent no longer lags when multiple participants speak at once.


v2026.5.4 also ships significant **Gateway startup performance** improvements. The startup path now lazy-loads plugin and runtime discovery, cron, schema, sessions, and model metadata — only loading what's needed. For larger OpenClaw installs with many plugins, this noticeably reduces startup time and peak memory pressure.


The Control UI got a meaningful update for cron jobs: the **New Job sidebar is now collapsible** , recovering screen space when you just need to see the jobs list.


**For OpenRouter users:** v2026.5.4 adds opt-in response caching via` X-OpenRouter-Cache` headers. It also expands the app-attribution categories OpenClaw sends to OpenRouter — now including coding, programming, writing, chat, and personal-agent usage. If you're on OpenRouter and care about provider attribution, update.


## v2026.5.5: Cross-Platform Bug Fixes


v2026.5.5 is the largest of the three by fix count. The[full release notes on GitHub](https://github.com/openclaw/openclaw/releases/tag/v2026.5.5) list 50+ fixes. Here are the ones that actually affect day-to-day use.


**xAI / Grok fixes:** OpenClaw was sending OpenAI-style reasoning effort controls to native Grok Responses models. This caused` xai/grok-4.3` to fail in live Docker and Gateway runs with an` Invalid reasoning effort` error. Fixed. The bundled xAI thinking profile is also now clamped to` off` by default.


**LINE channel fix:** A misconfigured` dmPolicy: "open"` without a wildcard` allowFrom` was being acknowledged silently, then blocked before inbound processing. Now it fails validation early with a clear error instead of disappearing into the void.


**iOS pairing improvement:** Manual` ws://` connects for private LAN and` .local` gateways now work, while Tailscale and public routes stay on` wss://` . Also fixes stale bootstrap token conflicts in mixed-auth reconnects (issue #47887, originally reported in 2023).


**Docker security hardening:** The bundled` docker-compose.yml` now drops` NET_RAW` and` NET_ADMIN` capabilities and enables` no-new-privileges` . If you run OpenClaw in Docker, pull the updated compose file.


**Telegram/Codex:** Progress drafts with tool-only outputs were duplicating item lines. Fixed. Progress now renders once per tool.


**Discord heartbeat:** Heartbeat ACK timeouts were being measured from gateway startup, not from the actual heartbeat send. This caused false reconnect loops while the channel was still awaiting readiness. Fixed (#77668).


## The Hotfix: v2026.5.6


v2026.5.5 introduced a` doctor --fix` repair that rewrote` openai-codex/*` routes to` openai/*` . This was correct for most users — but broke GPT-5.5 setups running the OAuth route via the Codex plugin.


v2026.5.6 ships within hours. It reverts that specific repair. If v2026.5.5 changed your default model, run:


```text
openclaw   models   set   openai-codex/gpt-5.5
openclaw   config   validate
```


This switches the default agent back to the Codex OAuth PI route.


## How to Upgrade (Self-Hosted)


If you're running OpenClaw on your own server or Mac:


```text
# Check current version
openclaw   --version


# Update to latest stable
openclaw   update


# Or via Homebrew (macOS)
brew   upgrade   openclaw-cli


# Run doctor after every major update
openclaw   doctor   --fix
```


After v2026.5.5 or v2026.5.6: run` openclaw config validate` to catch any route migrations that need manual attention.


**If you use Docker:** pull the updated compose file and rebuild. The` NET_RAW` /` NET_ADMIN` capability drops require the new compose config to take effect.


Self-hosters manage updates manually — Blink Claw handles them automatically the moment they land


Blink


## What This Means for Self-Hosters vs Blink Claw


Four releases in three days is a lot to track. v2026.5.6 shipped as a same-day hotfix for v2026.5.5. If you self-host, you need to:


- Monitor the[GitHub releases page](https://github.com/openclaw/openclaw/releases)
- Run` openclaw update` and` openclaw doctor --fix` after each release
- Check your Docker compose file after security updates
- Manually verify route migrations (like the Codex path change in v2026.5.5/v2026.5.6)
- Track breaking changes across plugin updates


If you're running OpenClaw on a personal Mac or VPS, that's 30-60 minutes of ops work every time a release lands. At this cadence, that's multiple times per week.


[Blink Claw](https://blink.new/claw) applies every update automatically. When v2026.5.5 shipped at 9am and v2026.5.6 followed at 5:51pm with the hotfix, Blink Claw users were on the fixed version without touching a terminal. Security patches, plugin updates, config migrations — all handled. Your agent runs 24/7 from one of 30+ data center regions, not from your laptop.


Blink Claw starts at $22/mo, all-in — LLM costs included via a 200+ model router.


## Frequently Asked Questions


Only if you use the Codex plugin with OAuth authentication and GPT-5.5. v2026.5.5's` doctor --fix` may have rewritten your route to` openai/*` instead of` openai-codex/*` . Run` openclaw config validate` to check. If your default model changed, follow the recovery steps in the v2026.5.6 release notes.


No. It ships as a bundled plugin but follows the default-deny path policy. You must configure allowed paths per node under` plugins.entries.file-transfer.config.nodes` before agents can use the` file_fetch` ,` dir_list` ,` dir_fetch` , or` file_write` tools.


The v2026.5.4 beta included the heartbeat ACK fix, but the stable fix shipped in v2026.5.5 (issue #77668). If you're on v2026.5.4 stable and seeing loops, update to v2026.5.5 or v2026.5.6. Run` openclaw channels status` to confirm your Discord channel shows as healthy.


Yes, if you use the bundled docker-compose.yml. The updated compose file drops` NET_RAW` and` NET_ADMIN` capabilities and enables` no-new-privileges` . Pull the updated compose file from the[OpenClaw repo](https://github.com/openclaw/openclaw) and rebuild your containers. Existing containers don't get these defaults automatically.


It's opt-in. When you route requests through OpenRouter, OpenClaw can now send` X-OpenRouter-Cache` and` X-OpenRouter-Cache-TTL` headers on verified OpenRouter routes. This enables server-side caching for repeated prompts, which reduces latency and cost on workloads with repetitive system prompts. See the OpenRouter docs for supported models and TTL limits.


For prior release context, see the[OpenClaw getting started guide](https://blink.new/blog/openclaw-getting-started) and the[Blink Claw vs clawctl comparison](https://blink.new/blog/blink-claw-vs-clawctl) .
