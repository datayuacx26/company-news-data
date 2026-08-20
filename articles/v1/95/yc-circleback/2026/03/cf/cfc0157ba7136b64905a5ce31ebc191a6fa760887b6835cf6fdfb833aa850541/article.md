---
schema_version: "1.0.0"
document_id: "cfc0157ba7136b64905a5ce31ebc191a6fa760887b6835cf6fdfb833aa850541"
company_key: "yc-circleback"
company: "Circleback"
source_id: "yc-circleback-news-import-b22aa3eada49"
canonical_url: "https://circleback.ai/blog/how-to-connect-circleback-mcp"
published_at: "2026-03-11T00:00:00+00:00"
first_seen_at: "2026-07-24T01:33:40.934837+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:c1f250d47f19f1b0097ea31fdd090e690f543f37ead72d6ea1e199bae5783f99"
---

# How to Connect Circleback to Claude, ChatGPT, and Cursor with MCP

Circleback's MCP connector gives AI tools like Claude, ChatGPT, and Cursor access to your meetings, calendar, and email through a single connection. Once set up, you can ask your AI assistant questions about past conversations, upcoming meetings, email threads, action items, and the people and companies you work with — all without leaving the AI tool.


If you're unfamiliar with MCP,[here's what it is and why it matters](https://circleback.ai/blog/what-is-mcp-meeting-tool) . The short version: it's the open standard that lets AI tools connect to your data sources, and Circleback's is the only meeting tool MCP server that spans meetings, calendar, and email in one connector.


## What you get access to


Once connected, your AI assistant can search and retrieve:


**Meeting data** — summaries, transcripts, action items, attendees, and key topics from any meeting Circleback has processed. This works regardless of how the meeting was recorded (bot, desktop, or mobile).


**Calendar events** — upcoming and past calendar entries, including attendees, times, and descriptions. Useful for meeting prep and scheduling context.


**Email** — email threads connected to your Circleback account, providing relationship context that spans beyond meetings. This is unique to Circleback's MCP implementation — no other meeting tool includes email in their MCP server.


**People and companies** — contact information and interaction history across meetings and email. Ask about a specific person and get a consolidated view of your relationship.


All data respects your existing Circleback permissions. You only see meetings and emails you're authorized to access.


## Setup by platform


### Claude (Web, Desktop, and Mobile)


This is the simplest path — Circleback is in Claude's official connectors directory.


1. Open Claude and go to **Settings** (or click the connector icon in a new conversation)
2. Find **Connectors** and search for **Circleback**
3. Click **Connect** and authenticate with your Circleback account
4. Done — Circleback context is now available in any Claude conversation


You can also connect directly at[claude.com/connectors/circleback](https://claude.com/connectors/circleback) .


Once connected, start any Claude conversation and ask a question that requires meeting context. Claude will automatically pull from your Circleback data when relevant.


### ChatGPT


1. Go to ChatGPT and look for the Circleback integration in ChatGPT's app/connector settings
2. Click **Connect** and authenticate with your Circleback account
3. Circleback data is now available in ChatGPT conversations


### Cursor


For developers using Cursor as their IDE:


1. Open Cursor Settings
2. Navigate to the MCP section
3. Add the Circleback MCP server URL:` https://circleback.ai/api/mcp`
4. Authenticate with your Circleback account when prompted


Once connected, you can ask Cursor's AI assistant about meeting context while coding — useful when you need to reference what was discussed in a product meeting or pull up technical decisions from past conversations.


### Claude Code


For developers using Claude Code in the terminal:


1. Run the installation command from[circleback.ai/docs/mcp](https://circleback.ai/docs/mcp)
2. Authenticate when prompted
3. Circleback tools are now available in your Claude Code sessions


### Raycast


1. Open Raycast AI Chat
2. Type` @circleback` to reference the Circleback MCP connector
3. Authenticate on first use


### Codex


Available through both the IDE Extension and CLI. Follow the setup instructions at[circleback.ai/docs/mcp](https://circleback.ai/docs/mcp) .


## What to ask


Once connected, the AI assistant can answer any question that draws on your meeting, calendar, or email data. Here are prompts that demonstrate what's possible:


### Meeting recall and search


- "What did we discuss with \[company\] last week?"
- "What action items came out of our team standup this morning?"
- "When was the last time we talked about pricing with \[client\]?"
- "What decisions were made in the Q1 planning meeting?"
- "Summarize all meetings I had with \[person\] in the last month"


### Cross-meeting patterns


- "What objections came up most often in sales calls this month?"
- "What feature requests have customers mentioned across calls this quarter?"
- "Which deals have stalled based on recent call activity?"
- "What themes keep coming up in our leadership meetings?"


### Meeting prep


- "What should I know before my call with \[company\] tomorrow?"
- "What did we cover the last time I met with \[person\]?"
- "Are there any open action items from our previous meetings with \[client\]?"
- "What has \[person\] mentioned they're working on recently?"


### Email + meeting context


- "What's the full history of our interactions with \[company\] — calls and emails?"
- "Did \[person\] follow up on what they committed to in our last meeting?"
- "What pricing discussions have happened with \[client\] across email and meetings?"
- "Draft a follow-up email to \[person\] based on what we discussed yesterday"


### Calendar context


- "What meetings do I have tomorrow and what should I prep for each?"
- "Who am I meeting with this week from \[company\]?"
- "When's my next meeting with \[person\]?"


The key difference between Circleback's MCP and competitors' implementations is that last category — questions that span both meetings and email. Because Circleback's connector includes email data, the AI assistant can answer questions about your full relationship with someone, not just what happened on calls.


## Tips for getting the most out of it


**Be specific about time ranges.** "What did we discuss with Acme?" will return results, but "What did we discuss with Acme in the last two weeks?" returns more focused, useful answers.


**Use it for meeting prep.** Before any external meeting, ask your AI assistant to brief you on previous interactions with that person or company. This works best when you've been on Circleback long enough to have accumulated meeting history.


**Combine it with automations.** Circleback's[automation builder](https://circleback.ai/blog/circleback-automations-you-should-build) routes structured meeting outputs to Slack, your CRM, and project tools automatically. MCP gives you the on-demand, conversational layer on top of that. Automations handle the predictable workflows; MCP handles the ad hoc questions.


**Ask follow-up questions.** MCP connections persist within a conversation. Ask a broad question, then drill down: "What did we discuss with Acme last week?" followed by "What specifically did they say about the timeline?" The AI assistant maintains context across turns.


**Use it across platforms for different purposes.** Claude for research and analysis. ChatGPT for drafting. Cursor for pulling meeting context into code. The same underlying meeting data, accessed through whichever AI tool fits the task.


## Frequently asked questions


**Does this require a Circleback paid plan?** MCP connectivity is available on Circleback paid plans. Check[circleback.ai/pricing](https://circleback.ai/pricing) for current plan details.


**Does this require a paid Claude or ChatGPT plan?** For Claude, you need a Pro, Max, Team, or Enterprise subscription to use connectors. For ChatGPT, check OpenAI's current plan requirements for MCP support.


**Is my data shared with Anthropic or OpenAI?** Your meeting data is queried through MCP for individual responses — it is not stored by the AI platform or used for model training. Circleback's MCP server controls what data is returned, and your existing Circleback permissions apply. Both Anthropic and OpenAI have stated that connector data is not used for training.


**Can my teammates see my meetings through MCP?** No. The MCP connector authenticates with your individual Circleback account. You only see meetings and data that you have access to in Circleback. Workspace-level meeting data follows your existing permission settings.


**Do I need to reconnect every time I use it?** No. Once you connect Circleback's MCP to your AI tool, the connection persists. Authentication is handled via OAuth and refreshes automatically. You connect once and it stays connected.


**Can I disconnect later?** Yes. You can revoke the MCP connection at any time from your Circleback settings or from the AI tool's connector settings.


*Circleback's MCP connector spans meetings, calendar, and email — the broadest data scope of any meeting tool's MCP implementation.*[Connect it now.](https://circleback.ai/docs/mcp)
