---
schema_version: "1.0.0"
document_id: "7cdf08053f1c686fa16f00daff104683e03b6c1036240cf24e8bd02219ba3a75"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-v2026-5-22-release"
published_at: "2026-05-24T12:25:06+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:4c329e6f10b262ee775992587d0da41f5b49fc05deb48dec89f7965b24a8c84a"
---

# OpenClaw v2026.5.22 + v2026.5.20: Meeting Notes, 4,100× Faster Models, and Smarter Agents

## The Meeting Notes Plugin: Live Capture From Discord Voice


The Meeting Notes plugin is the most ambitious addition in this release. It turns Discord voice channels into a live, structured note-taking source.


The plugin connects to a voice channel, monitors audio in real time, and generates structured notes as the conversation unfolds. Auto-start capture activates recording when a call begins — no manual trigger required.


For existing recordings, the plugin accepts manual transcript imports. Load or paste a transcript and OpenClaw processes it through the same extraction pipeline. Read-only CLI access lets you query and export notes from the command line without opening the full Control UI.


The plugin ships outside the core` openclaw` npm package — an external plugin install, separate from the core runtime. Install it through the OpenClaw plugin registry once your version reaches v2026.5.22.


The Meeting Notes plugin requires Discord voice channel access permissions. Check the[v2026.5.22 release notes](https://github.com/openclaw/openclaw/releases/tag/v2026.5.22) for the exact install command once the plugin registry entry is live.


## Performance: 4,100× Faster Model Listing


The /models endpoint now responds in 5ms instead of 20 seconds — a 4,100x improvement from provider auth-state pre-warming


Blink


*Before v2026.5.22, every /models call waited ~20s for provider auth to resolve. Pre-warming at startup drops that to 5ms.*


The` /models` gateway endpoint previously resolved at approximately 20 seconds. After v2026.5.22, it resolves at approximately 5ms.


That's a 4,100× improvement. The cause was straightforward: OpenClaw resolved provider auth-state lazily on every call. The fix pre-warms provider authentication at gateway startup. The first call hits a ready cache instead of waiting for a fresh auth handshake.


In daily use, this eliminates the startup delay every session experiences when loading available models. Sessions initialize faster. The model picker responds immediately. If you've watched the spinner for 20 seconds before your agent loaded — that spinner is gone.


## Sub-Agent Context Tightened — What You Need to Know


PR #85283 changes the default context delegated sub-agents receive at bootstrap.


Before this change, spawned workers inherited broad context. Persona files, identity definitions, user data, memory snapshots, and heartbeat files all passed to sub-agents by default. After this change, the default bootstrap limits context to **AGENTS.md** and **TOOLS.md** only.


Sub-agent context is now scoped to AGENTS.md and TOOLS.md by default, keeping persona and memory files out of delegated workers


Blink


*Persona, identity, user, memory, and heartbeat files are now excluded from delegated worker bootstraps by default.*


Sub-agents handling narrow tasks don't need full persona context. Limiting bootstrap context reduces token overhead and keeps sensitive configuration out of workers that don't require it.


If you rely on persona or memory context flowing to sub-agents — for example in[custom OpenClaw skills](https://blink.new/blog/openclaw-skills-guide) or[morning briefing workflows](https://blink.new/blog/openclaw-morning-briefing) — check whether those workflows depend on broad context inheritance. You may need to explicitly pass files that sub-agents previously received automatically.


## How to Upgrade


Self-hosted OpenClaw users run one command:


```text
npm   update   -g   openclaw
```


If you run OpenClaw through **[Blink Claw](https://blink.new/claw)** , no manual update is needed. Blink Claw auto-applies patches and version upgrades as they ship — no` npm update` required, no maintenance overhead. Comparing self-managed OpenClaw to a managed setup?[Blink Claw vs clawctl](https://blink.new/blog/blink-claw-vs-clawctl) covers the tradeoffs directly.


These two releases follow the[v2026.5.18 update](https://blink.new/blog/openclaw-v5-18-may-2026) that shipped earlier this month. Full commit details live in the[v2026.5.22 release notes on GitHub](https://github.com/openclaw/openclaw/releases/tag/v2026.5.22) .


It might, if your workflows rely on persona or memory files passing to delegated workers. Check any sub-agent pipelines that depend on contextual awareness beyond task definitions. You can explicitly pass additional context files to sub-agents to restore the previous behavior where needed.


No. The Meeting Notes plugin ships as an external plugin outside the core npm package. Install it separately through the OpenClaw plugin registry. Check the[release notes](https://github.com/openclaw/openclaw/releases/tag/v2026.5.22) for the install command once the registry entry is live.


No. The v2026.5.22 Grok integration reuses your existing xAI OAuth auth profile. Web search support and model aliases activate without creating a separate credential configuration.


No. Blink Claw auto-applies OpenClaw patches and version upgrades as they ship. Run OpenClaw without the hassle — Blink Claw handles everything from $22/mo at[blink.new/claw](https://blink.new/claw) , including security patches, so you stay current without any manual maintenance.
