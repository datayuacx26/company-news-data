---
schema_version: "1.0.0"
document_id: "5672e4b7a1b2ee5dae00ba3a651deb77a1444ebc4aedd7ef140b6c2ed1ff1eb0"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-2026-5-20-release"
published_at: "2026-05-25T12:15:56+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:2e6fcfa8b6c126352e31d2eb3289e658aaf655cd7e8ce7ff2cd967e91a24b512"
---

# OpenClaw 2026.5.20: Discord Voice Follow, Policy Checks, and Safer Agent Operations

## Policy Plugin: Channel Conformance Checks


The bundled Policy plugin adds a formal way to enforce behavior rules across your agent's active channels.


Three capabilities ship in v2026.5.20. **Channel conformance checks** let you define a policy file and validate that your agent's responses match it — useful for compliance requirements or specific communication standards. **Doctor lint findings** surface policy violations during the standard` doctor` health check, so misconfigurations appear before they cause runtime failures. **Opt-in workspace repair** can automatically fix common violations when you run the repair command.


Teams running OpenClaw across multiple channels with compliance requirements will get the most value here. Solo personal agent deployments can ignore the plugin entirely — it is strictly opt-in and changes nothing if you don't configure it.


This is a foundation layer. Expect more conformance rules and automated repair capabilities in future releases.


## Safer Cron Operations


Three cron fixes in v2026.5.20 address failure modes that affect long-running agent deployments.


**Background cron turns no longer block human chat.** Previously, a scheduled run could hold the main session while processing. That prevented you from chatting with your agent during scheduled work. The fix routes background cron turns to a separate execution path — your agent stays responsive while handling scheduled tasks.


**Preferred final output on successful runs.** When a cron job completes successfully, the agent now delivers the preferred output format instead of a raw task completion message. Scheduled summaries and reports arrive formatted correctly.


**Bound pagination prevents hanging.** Some cron runs stalled while paginating through large datasets with no stop condition. Pagination is now bounded — long-running scheduled jobs exit cleanly every time.


OpenClaw 2026.5.20 cron improvements — background scheduled tasks no longer block human chat sessions


Blink


On a self-hosted setup, each of these fixes requires you to manually track the release, pull the update, and restart your agent. Blink Claw handles this automatically — patches and runtime updates apply without touching a config file. Your agent runs 24/7 across 30+ data center regions, from $22/mo all-in with LLM costs included.


## Under the Hood: Codex 0.132.0 and More


**Codex harness updated to @openai/codex 0.132.0.** The model-list documentation now reflects the current catalog — stale or missing models are refreshed.


**xAI device-code OAuth** enables remote and headless setups to authorize xAI without a localhost browser callback. The device-code flow sends you a short code to enter in any browser — useful for SSH sessions or server-side deployments where opening a local browser isn't possible.


**Credential security tightened.** The` doctor` command now warns when` openclaw.json` stores plaintext secret-bearing config fields. Credential loaders also refuse symlinked credential files — a potential attack surface that is now closed. Teams managing multiple agents should run` doctor` after upgrading to catch any plaintext credential warnings.


Additional fixes in this release:


- Stale completion announces resolved — agent no longer announces completion on turns it didn't finish
- Wildcard subagent allowlists now work correctly for all pattern formats
- Message-tool-only turns no longer trigger unexpected behavior
- Heartbeat events filtered from session logs that were cluttered by them
- Mac app: Peekaboo bridge updated to 3.2.1, copyright year updated to 2026


## How to Upgrade


Update via npm:


```text
npm   install   -g   openclaw@2026.5.20
```


Full package details are on[npm](https://www.npmjs.com/package/openclaw/v/2026.5.20) . Complete commit-level release notes are on[GitHub](https://github.com/openclaw/openclaw/releases/tag/v2026.5.20) .


## Run OpenClaw Without the Hassle


Managing your own OpenClaw installation means tracking every release, applying patches manually, keeping Docker up to date, and restarting agents when configs change. That operational overhead adds up.


[Blink Claw](https://blink.new/claw) runs OpenClaw for you — $22/mo all-in, LLM costs included via 200+ model router, no Docker needed. Your agent stays on the latest release automatically across 30+ data center regions. It runs 24/7, not just when your laptop is on.


For extending what your agent can do, the[OpenClaw skills guide](https://blink.new/blog/openclaw-skills-guide) covers building and configuring skills that work across any integration — Discord, Telegram, Slack, and beyond. Blink Claw handles everything automatically so you can focus on what your agent does.


Try Blink free — ship your first app today


Describe what you want to build. Get a working app with database, auth, and hosting in minutes.


[Start free](https://blink.new/)


## Frequently Asked Questions


Voice Follow is available immediately after upgrading to v2026.5.20. Configure your allowed voice channels in the Discord integration settings — the agent will only follow you into channels you've explicitly pre-authorized. Multi-user handoff and DAVE recovery work automatically once Voice Follow is active.


Yes — starting in v2026.5.20, realtime voice sessions include` IDENTITY.md` ,` USER.md` , and` SOUL.md` by default. To disable context injection, set` voice.realtime.bootstrapContextFiles: \[\]` in your config. The[SOUL.md setup guide](https://blink.new/blog/openclaw-soul-heartbeat-setup) covers all configuration options.


No — the Policy plugin is fully opt-in. Upgrading to v2026.5.20 does not activate it. It only does anything when you define a policy file and explicitly run conformance checks. Doctor lint findings appear in the standard` doctor` output once the plugin is configured.


No. The fix specifically addresses this: background cron turns now run on a separate execution path that doesn't touch the main session. You can chat with your agent normally while scheduled tasks process in the background — no blocking, no delays.
