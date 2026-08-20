---
schema_version: "1.0.0"
document_id: "f2d28d28de2647a4c9b123e1d956822c5befc5c51a113649d5b74f5510238de7"
company_key: "yc-dailybot"
company: "DailyBot"
source_id: "yc-dailybot-news-import-dcb7a9078b70"
canonical_url: "https://www.dailybot.com/changelog/june-updates/"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-25T01:24:28.931275+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:c1afc16f9615c51196312562d23d7f4aa07d4e017dcfd6b0ceb1072e5bd17ff1"
---

# Dailybot | June updates: Dailybot CLI and form drafts · Changelog

In June we shipped stronger CLI and agent-skill workflows, plus form draft recovery so progress doesn’t disappear.


## Dailybot CLI and agent skills


The[Dailybot CLI](https://www.dailybot.com/cli) can now send and edit bot messages on Slack, Microsoft Teams, Discord, and Google Chat — from the terminal, with threaded replies and in-place edits. The[Agent Skill Pack](https://www.dailybot.com/skills) adds a matching chat skill, and lifecycle hooks let agents report progress on their own after a one-time` dailybot login` . Full docs are on the[developer portal](https://www.dailybot.com/developers/cli) .


We also refreshed the[Claude Code connection](https://www.dailybot.com/connections/plugin-for-claude) so coding agents report progress, pick up team messages, and stay visible in standups through the Dailybot Agent Skill.


The[DeepWorkPlan skill](https://www.dailybot.com/skills/deepworkplan) reached v2.15 with a new author sub-skill, so agents can create reusable skills and personas directly from your IDE.


## Form drafts that stick around


Forms now keep unfinished responses the same way check-ins already did. Close the tab, refresh, or lose connection mid-form — when you come back, your answers are still there.


If you intentionally exit with work still in progress, Dailybot asks you to confirm before you leave, so a mis-click doesn’t wipe everything. Once you submit successfully, the draft is cleared so the next open starts blank.
