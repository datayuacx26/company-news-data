---
schema_version: "1.0.0"
document_id: "1339b620d0fb809c963582e1cb8fcd54d699cff4bdc89ff2936d44b67a7fb6ab"
company_key: "yc-great-question"
company: "Great Question"
source_id: "yc-great-question-news-import-f3444748def0"
canonical_url: "https://greatquestion.co/blog/new-ai-user-research-tools"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-23T10:44:58.935973+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:4dbbcb09bcf762de50fbbf27fd7f807f0fc0adde93a17d535e39c4fb1183b467"
---

# New AI user research tools and features - updated monthly!

This is a running log of what shipped in AI user research: new tools, and new AI features in the UX research space. It's moving fast and we love hearing about what's new, and hope you do too.


*Last updated: July 2026.*


**TL;DR (July 2026).** New AI user research tools launch almost every week now, and the category is converging on three moves: AI-moderated interviews that run at scale, MCP connections that let assistants like Claude and ChatGPT reach your research data, and agentic repositories that answer questions on their own. What separates the tools is whether the AI can actually run a study and let you trace its output back to a real participant, or whether it can only read and summarize what already exists. Great Question sits on the run-it-and-prove-it side: its[MCP](https://greatquestion.co/features/mcp-integration) gives an AI tool 112+ actions across the full workflow and now lets your own agent run studies unattended, and its[Ask AI](https://greatquestion.co/features/ai) answers questions from your repository with the verbatim quote behind every claim.


## What we’re shipping at Great Question


We build in the open, so this section stays current with what’s live. Your AI can now run real research inside Great Question end to end, not just read about it: your own assistant connected over MCP, an agent that runs studies unattended, and the AI built into your repository.


### Your AI can run the whole study, not just read the repository


Most research tools now connect to Claude, ChatGPT, or Cursor or any other MCP-compatible tool. Almost all of those connections are read-only: your AI can pull a transcript or summarize a study someone else built. Ours does the work.


The Great Question[MCP](https://greatquestion.co/features/mcp-integration) now gives your AI 112+ things to do across the research workflow, the most comprehensive set of any research platform. Where most integrations let your AI look, ours lets it run the study, from a prompt to a finished, verified study:


- **Design studies.** Create[interviews](https://greatquestion.co/features/interviews) (1:1, round robin, or collective),[surveys](https://greatquestion.co/features/surveys) (12+ question types), and unmoderated tests ([prototype](https://greatquestion.co/features/unmoderated-prototype-testing) ,[card sort](https://greatquestion.co/features/card-sorting) ,[tree test](https://greatquestion.co/features/tree-testing) ) with incentives, participation caps, consent forms, screeners, and skip logic.
- **Manage your**[panel](https://greatquestion.co/features/panel) **.** Create, update, and delete candidate profiles, custom attributes, and saved segments, with consent and GDPR built in.
- **Recruit participants.** Shortlist candidates from your panel onto any study and send screener invitations in seconds.
- **Schedule and assign.** Set booking limits, availability, buffer time, and notice, and add moderators and observers to sessions.
- **Analyze sessions.** Run full-text search across speaker-attributed transcripts, highlights, and match scores, plus survey answers and prototype analytics like completion rate, top paths, and per-screen drop-off.
- **Synthesize insights.** Pull findings, stories, and curated[highlight reels](https://greatquestion.co/features/highlight-reels) , and export insights as a PDF.


That’s read and write across candidates, study design, scheduling, recruitment, analysis, and synthesis. Most platforms that offer an MCP at all offer it read-only, or through a third-party connector like Zapier with a fraction of the actions.


Capability Great Question Other platforms


Native MCP server Yes Limited


Search research sessions Yes Yes


List studies Yes No


Create interview studies Yes No


Create surveys Yes No


OAuth 2.0 security Yes Varies


Enterprise role permissions Yes Limited


When your AI hands you a synthesis, every insight traces back to the session highlight it came from. You don’t have to take the summary on faith. You can click through and check the moment a real customer said it. AI moderation and synthesis are only worth using if you can audit them, and most tools ask you to trust the output blind.


It runs inside Claude, ChatGPT, Cursor, Gemini, and Copilot. You connect once, and your existing role permissions carry through via[OAuth](https://greatquestion.co/features/security) , so nothing gets exposed that shouldn’t be.


[See it run a study live → book a demo](https://greatquestion.co/about/demo)


Want the full setup, the 112+ actions, and how teams roll it out?[Get the Ultimate Guide to UX Research with MCP →](https://greatquestion.co/resources/download-mcp-guide-see-our-ultimate-guide-to-mcp-for-ux-researchers)


### Point your own agent at it and let it run unattended


Connecting through Claude or ChatGPT keeps you in the loop, approving each step. Our biggest MCP update to date removes that requirement for teams that want it. You hand a custom agent a secure API key once, and after that it connects to Great Question on its own, in the background, whenever it needs to. It only ever sees what the person behind the key is allowed to see, exactly like a normal login, so nothing gets exposed that shouldn’t be.


This is built for the ResearchOps and technical teams wiring up their own agents, not for running prompts by hand in a chat. Two things teams already do with it: an agent that spots product questions in Slack, checks whether the answer already exists in Great Question, and drafts a ready-to-launch study for a researcher to approve; and an internal assistant that blends Great Question’s qualitative research with company-wide product analytics, so one question gets one answer drawn from both.


[See how the MCP works →](https://greatquestion.co/features/mcp-integration)


‍


### Your repository answers questions, with the evidence cited


The MCP is one half of agentic research. The other half is already inside your repository.[Ask AI](https://greatquestion.co/features/ai) takes a plain-language question (like “why do enterprise users churn in month two?”) and returns a structured answer pulled from every transcript, survey, and highlight in your account. Run it across the whole[repository](https://greatquestion.co/features/research-repository) for cross-study synthesis, or scope it to a single study or session.


Every claim is tied to the verbatim quote and the session it came from, so you can verify any finding in one click. If the answer isn’t in your research, Ask AI says “not found” instead of inventing one. Real names never surface, since it uses speaker handles only, and it’s covered by the same SOC 2, GDPR, and HIPAA controls as the rest of the platform.


Same principle as the MCP, now applied to the repository itself: an AI that acts on your research is only useful if you can trace what it says back to a real person who said it.


[See how Ask AI works →](https://greatquestion.co/features/research-repository)


### A new Claude skill for research, updated regularly


Alongside the MCP, we publish Claude skills that package a research workflow into one prompt. The[AI Research Toolkit](https://greatquestion.co/resources/ai-research-toolkit) now has 11: transcript cleanup, cross-study synthesis, screener building, a JTBD interview guide builder, a JTBD analyzer that extracts push/pull/anxiety/habit forces, and a survey designer that catches the leading and double-barreled questions.


It’s free, and it’s the fastest way to feel what agentic research is like before you connect anything.


[Get the AI Research Toolkit (11 Claude skills, free) →](https://greatquestion.co/resources/ai-research-toolkit)


## The rest of the category: AI research tool launches by month


We track what everyone else ships too, without spin, because a changelog that only mentions us wouldn’t be worth your bookmark.


### July 2026


*New in July 2026: MCP servers became table stakes, AI moderation kept advancing, and voice-first entrants kept launching.*


MCP servers became table stakes over the first half of 2026. Maze, Listen Labs, Dovetail, and Lookback have all shipped connections that let AI assistants reach their data, the same standard our own integration runs on. What separates these tools now is what the AI can do once it’s connected: run new research, or only read what’s already there.


On the moderation side, the arms race continued. Listen Labs, fresh off a large Series B in January, added multi-modal emotion detection to its AI interviews, and Maze expanded its AI Moderator into deeper evaluative research. Newer entrants like UserCall and Userology are building AI-voice-moderated interviews as their whole product. With all of them, the interview is only as good as your ability to trust and trace what came out of it.


### Earlier in 2026


*Q1–Q2 2026: heavy funding and fast feature shipping across the category: Listen Labs’ raise, plus a broad push into agentic repositories and scheduled AI agents.*


Listen Labs raised a $69M Series B in January and has shipped quickly since. Dovetail added AI Agents that run on a schedule and post summaries into Slack and Teams on their own, plus a run of chat and MCP updates through the spring. UserTesting added AI summaries for imported recordings, and Lookback quietly added AI-suggested findings and a bring-your-own-AI connection. Among the legacy repository tools, EnjoyHQ has gone quiet, with no notable releases since it moved under UserTesting.


Everyone agrees AI belongs in research now. The disagreement is about where the human stays in the loop, and how much you’re asked to trust an output you can’t trace to the person who said it.


## Common questions


**What is agentic user research?**
Agentic user research is when an AI does the research work, not just reads about it: designing studies, recruiting participants, running analysis, and synthesizing findings, either through a connected assistant like Claude or ChatGPT or inside the research platform itself. The test that matters is whether you can trace every AI output back to a real participant.


**Can AI run a full user research study end to end?**
Yes. Through the Great Question MCP, an assistant like Claude, ChatGPT, Cursor, Gemini, or Copilot can design a study, recruit from your panel, schedule sessions, analyze transcripts, and synthesize insights. That’s 112+ actions across the workflow, and a custom agent with an API key can now run those actions unattended in the background. Most research-tool integrations are read-only, so the AI can summarize existing work but can’t run new studies.


**Can an AI moderate a user interview?**
Yes. AI moderators now run interviews, prototype tests, and survey follow-ups, asking their own questions in real time. The newer ones are multimodal: instead of only reading typed answers, they watch the participant’s screen during a prototype test and probe when someone gets stuck. Listen Labs, Maze, UserCall, Userology, and Great Question all offer AI moderation as of 2026, and the more capable moderators can see the session, not just read the transcript.


**Which user research platforms have an MCP server?**
As of July 2026, Great Question, Maze, Listen Labs, Dovetail, and Lookback all offer MCP connections to AI assistants. Most are read-only, so the AI can pull data but can’t act on it. Great Question’s MCP is read and write across 112+ actions, so the assistant can run a study, not just read one.


**Is AI research synthesis trustworthy?**
Only if you can audit it. AI synthesis is worth using when every finding links back to the verbatim quote and session it came from, so you can click through and check what a real customer actually said. In Great Question, both the MCP and the repository’s Ask AI cite the source behind every claim, and say “not found” rather than inventing an answer.


**Running research with AI shouldn’t mean trusting a black box.**[See how it works → book a demo](https://greatquestion.co/about/demo)
