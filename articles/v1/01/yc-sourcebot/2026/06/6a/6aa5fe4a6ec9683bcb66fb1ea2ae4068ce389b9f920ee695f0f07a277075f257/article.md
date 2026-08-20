---
schema_version: "1.0.0"
document_id: "6aa5fe4a6ec9683bcb66fb1ea2ae4068ce389b9f920ee695f0f07a277075f257"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/v5"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:0c713ef61b9a32d0e12be918c0318957b8078c3a43dd73cfdc55055a7765df75"
---

# Sourcebot v5

Sourcebot v5 is our biggest release yet. Ask Sourcebot now reaches beyond your code to the tools you work with everyday, all inside an app we've rebuilt from the ground up.


Here's what's new:


- **Ask connectors** : bring Jira, Slack, Linear, and more into the Ask agent.
- **App redesign** : a persistent sidebar that keeps your recent repos and chats one click away.
- **Git blame and history** : see who changed what, and when, right in the app.


## Ask connectors


Context is often scattered across dozens of platforms - Jira tickets, Slack threads, Confluence docs, etc.


Ask Sourcebot can now connect to your tools via MCP, giving it access to data and actions beyond just your code. Bring external context to help debug issues, draft specifications, and create bug reports.


For example:


- Pull the top 3 highest severity bugs from **Jira** , investigate the root cause, and file your findings back as comments.
- Turn a **Linear** ticket or **Slack** thread into an implementation spec, grounded in how your code actually works.
- Create a document in **Confluence** for how this system works today.


Admins can control access by allowlisting connectors, tool permissions, and OAuth scopes. See the[docs](https://docs.sourcebot.dev/docs/features/ask/connectors) for setup instructions.


## App redesign


We've rebuilt the look and feel of Sourcebot around a persistent sidebar.


The sidebar stays with you everywhere in the app, so your recent repositories and chats are always one click away. Jump back into a conversation, switch to a repo you were just browsing, or start something new without losing your place.


## Git blame and history


We've integrated git blame and history directly into the code browser:


- When viewing a file, click the **blame** view to see the commit info per line.
- Clicking on a commit will open the **diff preview** preview.
- When viewing a file or directory, the **History** tab on the bottom bar previews the commit history.
- Clicking the "View full history" tab will reveal the full commit history, with options to filter by author or by date.
