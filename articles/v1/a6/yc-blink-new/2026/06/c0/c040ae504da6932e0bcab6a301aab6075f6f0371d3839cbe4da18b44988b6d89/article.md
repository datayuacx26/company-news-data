---
schema_version: "1.0.0"
document_id: "c040ae504da6932e0bcab6a301aab6075f6f0371d3839cbe4da18b44988b6d89"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-skill-workshop-guide"
published_at: "2026-06-10T00:22:44+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:791aa0464848d3b0d28b716e2fb82a308203bdb89f1d2c102356ea14878535bb"
---

# OpenClaw Skill Workshop Guide: Create Custom Skills with Governance

## The 3-Step Governed Workflow


**Step 1: Proposal.** The agent (or you via CLI) creates a pending proposal — a` PROPOSAL.md` containing the proposed skill content, support files, scanner results, and hashes. Nothing writes to your active workspace skills yet.


**Step 2: Review.** Inspect the proposal. Revise it as many times as needed. Each revision increments the version field and updates the timestamp. Reject it with a reason, quarantine it for security review, or approve it.


**Step 3: Apply (or Reject/Quarantine).** Apply transitions the proposal to` applied` and writes the` SKILL.md` to your workspace. The agent uses it immediately — no restart required.


The five proposal states: **pending** (entry point), **applied** (approved and live), **rejected** (declined with reason), **quarantined** (held for review), **stale** (live skill changed since proposal creation — create a fresh proposal).


## How to Use the skill_workshop Agent Tool


The simplest path is natural language in your agent session:


**Create a new skill:**


```text
Make a skill called morning-catchup that runs my Monday inbox routine.


```


The agent calls` skill_workshop` with` action: create` and returns a proposal ID in chat.


**Review and revise:**


```text
Show me the morning-catchup proposal.
Revise it to also flag anything marked urgent.
Apply the morning-catchup proposal.


```


**Via CLI:**


```text
# Create a proposal
openclaw   skills   workshop   propose-create   \
--name   morning-catchup   \
--description   "Daily inbox catch-up: triage, archive, surface, draft, plan"   \
--proposal   ./PROPOSAL.md


# List all proposals
openclaw   skills   workshop   list


# Inspect a specific proposal
openclaw   skills   workshop   inspect   <  proposal-i  d  >


# Apply, reject, or quarantine
openclaw   skills   workshop   apply   <  proposal-i  d  >
openclaw   skills   workshop   reject   <  proposal-i  d  >   --reason   "Duplicate of daily-briefing skill"
openclaw   skills   workshop   quarantine   <  proposal-i  d  >   --reason   "Needs security review"
```


Reviewing a skill proposal in OpenClaw Skill Workshop — pending, approved, and rejected states visible in the Control UI


Blink


## Creating Your First Governed Skill: Full Example


**1. Write the proposal file** (` PROPOSAL.md` ):


```text
---
name: morning-catchup
description: Daily inbox catch-up — triage, archive, surface action items, draft replies, plan the day.
status: proposal
version: v1
date: 2026-06-03
---


## What this skill does


When invoked, run the morning catch-up routine:
1.   Check email inbox — triage, archive, flag action items
2.   Surface the 3 most important tasks for today
3.   Draft replies for messages marked "needs response"
4.   Output a 5-line morning brief
```


The` status: proposal` field is proposal-only. The` apply` command removes it automatically. Do not include it in the applied` SKILL.md` — it signals an unapplied draft and will confuse the agent.


**2. Submit the proposal:**


```text
openclaw   skills   workshop   propose-create   \
--name   morning-catchup   \
--proposal   ./PROPOSAL.md
```


**3. For complex skills with support files** , use` --proposal-dir` :


```text
openclaw   skills   workshop   propose-create   \
--name   weekly-update   \
--description   "Friday wrap-up: stats, highlights, next week's top three"   \
--proposal-dir   ./weekly-update-proposal/
```


The directory must contain` PROPOSAL.md` . Support files must live under:` assets/` ,` examples/` ,` references/` ,` scripts/` , or` templates/` . **Limits** : 64 support files, 256 KB each, 2 MB total, 40,000 bytes for the proposal body.


**4. Review and approve:**


```text
openclaw   skills   workshop   inspect   <  proposal-i  d  >
openclaw   skills   workshop   apply   <  proposal-i  d  >
```


## The #1 Gotcha: Skill Workshop Hidden by the Coding Profile


This is[GitHub Issue #87570](https://github.com/openclaw/openclaw/issues/87570) , the most common Skill Workshop failure mode by far.


**Symptom** : The plugin loads correctly. Auto-capture runs. But` skill_workshop` is undefined inside your agent session. No error. Just silence.


**Root cause** : The` coding` tools profile does not include` group:plugins` . The` skill_workshop` tool is silently absent even when everything else looks healthy.


**Fix** :


```text
{
"tools"  : {
"profile"  :   "coding"  ,
"alsoAllow"  : [  "skill_workshop"  ]
}
}
```


This explicitly injects` skill_workshop` into the coding profile's allowed tools. Check this before opening a new issue.


## The Control UI: What Each Panel Shows


v2026.6.1 ships a new **Skill Workshop tab** in the Control UI with four panels:


- **Proposal list** : all proposals across all states, sortable by date and status
- **Today view** : proposals created or modified today — useful for active skill development
- **Revision dialog** : full history of every revision to a pending proposal
- **File preview** : searchable content of proposal and support files before you approve


The default` approvalPolicy: "pending"` prompts for human approval before the agent autonomously triggers` apply` ,` reject` , or` quarantine` . For CI or trusted sandboxes:


```text
{
"skillWorkshop"  : {
"approvalPolicy"  :   "auto"
}
}
```


## Configuration Reference


Setting Default What it controls


` autonomous.enabled`` false` Agent creates proposals from durable conversation signals


` approvalPolicy`` "pending"` Human approval prompt vs. auto-apply


` maxPending`` 50` Max pending + quarantined proposals per workspace


` maxSkillBytes`` 40,000` Max proposal body size in bytes


The description field hard-caps at 160 bytes regardless of` maxSkillBytes` . Set` autonomous.enabled: true` only when you want the agent to propose skills based on patterns it observes across sessions — not just explicit requests.


Deploying an approved skill to your OpenClaw workspace — the agent picks it up immediately, no restart required


Blink


## Blink Claw: Skip the Docker Upgrade Entirely


OpenClaw v2026.6.1 shipped June 1, 2026. On self-hosted setups, getting it means pulling a new Docker image, running migrations, and validating config. Or waiting until you remember to do it.


Blink Claw users got v2026.6.1 automatically. No Docker setup. No version tracking. No manual config check. Security patches apply automatically — you never track CVEs.


Blink Claw runs at $22/mo all-in — LLM costs included via 200+ model router. No separate API keys. Your agent runs 24/7, not just when your laptop is open.


[OpenClaw has 376,000+ GitHub stars](https://www.trendingtopics.eu/openclaw-numbers/) and 3.2 million active users worldwide. The ecosystem moves fast. Blink Claw keeps you current without the infrastructure overhead. Every v2026.x update ships to your agent automatically.


For more on OpenClaw skills, see the[OpenClaw skills guide](https://blink.new/blog/openclaw-skills-guide) and[best OpenClaw skills 2026](https://blink.new/blog/best-openclaw-skills-2026) . And if you're comparing the hosting options,[how to run OpenClaw without Docker](https://blink.new/blog/how-to-run-openclaw-without-docker) covers the full comparison.


The most common Skill Workshop issue. The` coding` tools profile does not include` group:plugins` , so` skill_workshop` is silently absent even when the plugin loads correctly. Fix: add` "alsoAllow": \["skill_workshop"\]` under your` tools` config. See[GitHub Issue #87570](https://github.com/openclaw/openclaw/issues/87570) for the full thread and discussion.


A proposal goes` stale` when the live skill changes after the proposal was created — the target hash no longer matches. Stale proposals cannot be revised or applied. Create a fresh proposal; the agent picks up the current skill state and incorporates it into the new proposal automatically.


Not yet. Skill Workshop only writes to` <workspace>/skills/<name>/` — not to` ~/.openclaw/skills/` , the shared root for orchestrator/worker setups.[GitHub Issue #74601](https://github.com/openclaw/openclaw/issues/74601) tracks cross-workspace promotion. Current workaround: manually copy the applied` SKILL.md` to the shared root after approval.


In CI environments or fully isolated sandboxes,` "auto"` lets the agent apply proposals without a human confirmation step. In production or shared workspace setups, keep` "pending"` — you want a human in the loop before new skills modify how your agent behaves in live sessions.


Yes. Blink Claw runs on OpenClaw and auto-updated to v2026.6.1 when it shipped June 1. Skill Workshop is available on every Blink Claw plan with no config changes and no Docker upgrades. Start at[blink.new/claw](https://blink.new/claw) from $22/mo all-in — LLM costs included.


` rejected` is a final decision — you declined the proposal with a logged reason.` quarantined` is a hold — the proposal stays in queue for deeper review and counts toward your` maxPending` limit (default 50). Use` quarantined` when a proposal needs a security or compliance check before making a final call.
