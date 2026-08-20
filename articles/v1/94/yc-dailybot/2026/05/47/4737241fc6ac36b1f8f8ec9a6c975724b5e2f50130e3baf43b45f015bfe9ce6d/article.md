---
schema_version: "1.0.0"
document_id: "4737241fc6ac36b1f8f8ec9a6c975724b5e2f50130e3baf43b45f015bfe9ce6d"
company_key: "yc-dailybot"
company: "DailyBot"
source_id: "yc-dailybot-rss-59e03def7d75"
canonical_url: "https://www.dailybot.com/blog/dailybot-plugin-for-claude-and-skill-for-cursor/"
published_at: "2026-05-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:17.151433+00:00"
fetched_at: "2026-07-28T22:12:46.079122+00:00"
content_hash: "sha256:a2c7afae575a14dc7c4572d90e9af6527b35e6e6c7ef8fa743ff71c916282127"
---

# How we gave our coding agents a voice on the team

Three months ago, our engineering team started running Claude Code agents overnight. By morning, dozens of files had changed across multiple repos. The standup was blank. The manager asked what happened. Nobody knew — because the agents had no way to tell us.


We built the Dailybot plugin for Claude Code and the skill for Cursor to fix that. Both integrations connect AI coding agents to the same coordination layer the rest of the team already uses: Dailybot.


---


## The problem with invisible agents


AI coding agents are productive. They can reason through multi-step tasks, write tests, fix bugs, and ship features across long sessions. But all that work is invisible to the team unless someone manually writes it up.


In practice, that means one of two things happens. The developer who launched the agent writes a standup update summarizing what the agent did — adding overhead to a process that was supposed to save time. Or nobody writes anything, and the team loses visibility into hours of real work.


Neither outcome is acceptable when agents are doing meaningful, production-level engineering. The team needs to see what agents accomplish the same way they see what people accomplish: in the standup feed, in the coordination layer, in the daily rhythm of the team.


---


## What the plugin and skill do


The Dailybot plugin for Claude Code and the Dailybot skill for Cursor both solve the same problem from different editors. They connect the agent to Dailybot so it can:


-


**Report progress automatically.** When the agent finishes meaningful work — shipping a feature, fixing a bug, completing a multi-file refactor — it sends a standup-style update to Dailybot. Reports describe what was accomplished and why it matters, not which files changed. Trivial work like typo fixes or lockfile updates is skipped.


-


**Receive team messages.** At the start of every session, the agent checks for pending instructions, priorities, or feedback from the team. This turns Dailybot into a two-way channel: the team sends context, the agent picks it up and acts on it.


-


**Send emails.** The agent can send follow-up emails through Dailybot — useful for weekly summaries, notifications, or reports that need to reach people outside the chat platform.


-


**Announce health status.** The agent can tell the team whether it’s online, working, offline, or degraded. Health checks also deliver any pending messages.


The key design decision was making this automatic. The plugin detects when significant work is completed and sends the report without a manual command. When the agent finishes a turn with unreported work, it’s reminded to offer a report before stopping. Session messages are fetched at startup without the developer asking.


---


## How reports look in practice


We spent time calibrating the reporting style. Reports follow a standup format — written in first person, outcome-focused, free of file paths or git statistics. A few real examples:


> “Fixed a bug where users without a timezone set would see errors on their profile page.”


> “Built the notification preferences system — users can now configure which alerts they receive and through which channels.”


> “Shipped the new billing dashboard — managers can now view usage, invoices, and plan details in one place.”


The reports are designed so that a team member reading the standup feed cannot tell whether a human or an agent wrote them. That is the point. The work matters, not who did it.


---


## Co-authoring and the human behind the agent


One detail we got right early: co-authoring. When a developer is logged into the Dailybot CLI, every report the agent sends automatically credits that developer as a co-author. The agent’s work shows up in the developer’s daily standup — because they directed it.


This matters for two reasons. First, it keeps accountability clear. The human who launched the agent is responsible for the output. Second, it makes agent work visible in the same feed as human work, without creating a separate “agent” channel that nobody checks.


A developer running three agents in parallel is doing three times the work. The standup should reflect that.


---


## Getting started


Both integrations take under five minutes to set up.


**For Claude Code:**


```text
curl   -fsSL   https://cli.dailybot.com/install.sh   |   bash
dailybot   login
npx   skills   add   DailybotHQ/agent-skill
```


Since this post first went up, the Claude-specific plugin and the Cursor skill have been unified into a single cross-agent **Dailybot Agent Skill** . The install line above is the same one you’d run for Cursor, OpenAI Codex CLI, or Gemini CLI — one command, one skill pack, every AI CLI.


**For Cursor** , the skill is loaded from the project’s` .agents/skills/` directory and activated automatically when the workspace includes Dailybot configuration. The Dailybot CLI handles authentication the same way.


No credit card required. If you don’t have a Dailybot account, the agent can create a new organization from the terminal and generate a link for your team to connect Slack, Microsoft Teams, Discord, or Google Chat.


The slash commands are available for manual use when needed:


Command What it does


` /dailybot:report` Send a progress report to your team


` /dailybot:messages` Check for pending messages


` /dailybot:email` Send an email through Dailybot


` /dailybot:health` Announce agent status and pick up messages


---


## What this changes for teams


The real shift is not that agents can report. It is that agent work becomes part of the team’s coordination rhythm — the same standups, the same feed, the same visibility layer that human work already uses.


Before this plugin, a manager reviewing the morning standup saw half the picture. The human updates were there. The agent work was scattered across git logs, PR descriptions, and Slack threads. Now both show up in one place, written in the same format, with the same level of detail.


If your team is running coding agents and losing track of what they produce, the[Dailybot plugin for Claude Code](https://www.dailybot.com/connections/plugin-for-claude/) closes that gap.
