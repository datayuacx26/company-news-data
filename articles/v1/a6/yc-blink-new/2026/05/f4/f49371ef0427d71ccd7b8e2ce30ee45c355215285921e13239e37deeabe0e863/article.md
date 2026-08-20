---
schema_version: "1.0.0"
document_id: "f49371ef0427d71ccd7b8e2ce30ee45c355215285921e13239e37deeabe0e863"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-2026-4-26-whats-new"
published_at: "2026-05-01T13:02:09+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:46.128682+00:00"
content_hash: "sha256:8309952e1e9277115c11fcf7a9fb5e70e81b1490b13082282b22badf356da5cd"
---

# OpenClaw v2026.4.26: Realtime Voice, Smarter Image Defaults, and Command Security

## 3. Subagent Command Security Is Now Enforced Correctly


This is the fix that matters most for anyone running agents in production.


**The problem:** Prior to v2026.4.26,` subagents.allowAgents` was only checked for *automatic* same-agent spawn calls. If an agent made an explicit` sessions_spawn(agentId=...)` call targeting itself, the check was skipped entirely — the agent self-targeted without going through the allowlist gate.


This created a real gap: a malicious prompt or misconfigured agent could instruct a subagent to spawn itself with elevated permissions, bypassing the allow/deny rules an operator had set.


**The fix:**` subagents.allowAgents` is now enforced for *all* explicit` sessions_spawn(agentId=...)` calls, including self-targets. The PR notes this as a` Fixes #72827` change, credited to @oiGaDio.


Also in this release:


- **Gateway/device tokens** : rotated bearer tokens are no longer echoed back in` device.token.rotate` responses. A subtle but meaningful change — token rotation responses previously leaked the new token in cleartext to any eavesdropper on the response channel. Now only the device that triggered the rotation receives it via the expected handoff path.
- **Exec command loop detection** : tool-loop detection history is now scoped to the active run, so heartbeat cycles don't inherit stale repeated-call counts from previous runs. Agents that run scheduled heartbeats (like the[Soul Heartbeat](https://blink.new/blog/openclaw-soul-heartbeat-setup) pattern) were sometimes triggering false-positive command blocks.
- **Exec timeouts** :` tools.exec.timeoutSec` now applies correctly to background,` yieldMs` , and remote node commands. Commands no longer run indefinitely when a timeout is configured.


Security in action — the developer stands behind a glowing green shield blocking threat particles, confident protective stance in a dark server room


Blink


## How to Upgrade


If you're running OpenClaw manually:


```text
npx   @openclaw/cli@latest   update
```


Or with a global install:


```text
npm   update   -g   @openclaw/cli
openclaw   update
```


After upgrading, run` openclaw doctor` to catch any config issues introduced by the plugin manifest changes in this release. The plugin config load/write helpers have been deprecated in favor of runtime snapshots —` openclaw doctor --fix` will migrate stale configs automatically.


If you're upgrading from v2026.4.24, note that the bundled Claude importer (` openclaw migrate` ) is new in .26. It can import Claude Code and Claude Desktop instructions, MCP servers, skills, and command prompts in one pass.


## Blink Claw Users: Nothing to Do


If you're running your agent on[Blink Claw](https://blink.new/claw) , v2026.4.26 is already live on your machine. You didn't have to track the release, test the upgrade, or run` openclaw doctor` . Security patches like the subagent command fix in this release were applied to every managed agent automatically — no action required.


Blink Claw includes:


- **$22/mo all-in** — LLM costs included via our 200+ model router. No separate Anthropic or OpenAI bills to manage.
- **No Docker, no VPS** — the entire OpenClaw stack runs managed. You configure your agent, not your infrastructure.
- **24/7 uptime** — your agent runs whether or not your laptop is open.
- **Automatic security patches** — updates like the subagent command fix above are applied to your agent without you tracking CVEs or version numbers.


For teams building agents that use the new realtime voice features, Blink Claw's managed gateway handles the browser relay configuration that previously required manual setup. For agents using the[best OpenClaw skills](https://blink.new/blog/best-openclaw-skills) , the improved exec loop detection in this release means scheduled skill runs are less likely to trigger false-positive blocks.


## What's Next


The v2026.4.27 and v2026.4.28 releases are already staged in the changelog. Key items to watch: further Google Meet stability work, more Cron improvements for isolated run error classification, and continued plugin startup performance work from @shakkernerd.


## FAQ


**Do I need to reconfigure Talk/TTS after upgrading to v2026.4.26?** No. The new browser realtime transport contract is additive. Your existing TTS config continues working. You'll see the new Google Live browser session option available in the Control UI if your gateway configuration supports it.


**Does the subagent security fix require any config changes?** No. The fix is in the enforcement logic, not in the config schema. If you already had` subagents.allowAgents` set, it now applies correctly to self-spawn calls. If you didn't have it set, agent behavior is unchanged — the fix only closes the gap when the allowlist is actively configured.


**Will upgrading break my plugins?** The plugin config helpers deprecated in this release still work — they're deprecated, not removed. Run` openclaw doctor --fix` after upgrading to migrate automatically. Most users won't notice any difference.


**Is the Docker CA certificate fix relevant to me?** Only if you're running OpenClaw in a containerized setup (the bundled slim Docker image). Fixes #72787 installs the CA certificate bundle so HTTPS calls from containerized gateways work correctly. Non-Docker installs are unaffected.


**Can Blink Claw agents use the new realtime voice features?** Yes. The realtime transport infrastructure is available to all Blink Claw agents. Reach out via Discord or support if you're building a voice-first agent and want guidance on the configuration.
