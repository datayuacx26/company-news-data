---
schema_version: "1.0.0"
document_id: "716c783d5b16f2f7a3c5265f0fe49c3c228ab42a75b8cfc610d69af6cb2ad1ec"
company_key: "yc-codewisp"
company: "CodeWisp"
source_id: "yc-codewisp-news-import-78afdd680d6b"
canonical_url: "https://codewisp.ai/blog/connect-your-codewisp-game-to-an-ai-agent-mcp"
published_at: "2026-07-22T14:36:37.748+00:00"
first_seen_at: "2026-07-26T11:57:45.914926+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:2b482992ffd93992914c026e512f0c36fbba84fd9de66d20288a3ae94439f10f"
---

# Connect your CodeWisp game to an AI agent (MCP)

[Back to Blogs](https://codewisp.ai/blog)


# Connect your CodeWisp game to an AI agent (MCP)


Your CodeWisp game now speaks **MCP** (Model Context Protocol). That means you can point an external AI agent — like Claude Desktop or Cursor — straight at a project and let it read, edit, validate, and run your game, using the same tools the CodeWisp editor uses. Here’s how to set it up in about two minutes.


###


What your agent can do


Once connected, the agent gets seven tools scoped to a single project:


- **read_project / list_files / read_file** — see your title and source files


- **validate_project** — run quick static checks (syntax, lint, script tags)


- **edit_project / write_file** — change the whole bundle or a single file


- **run_project** — get a runnable HTML build of the game


Every edit the agent makes is saved to your project and shows up in the editor’s message panel as an agent operation, so you always see what changed.


###


Step 1 — Open the connect dialog


1. Open your game in the CodeWisp editor.


1. In the top bar, click **Connect AI agent** .


The dialog mints a **project-scoped token** and shows you an **Endpoint** URL plus ready-to-paste config for Claude Desktop and Cursor. The token acts as you, on that one project only.


###


Step 2 — Paste the config into your agent


####


Claude Desktop


Open **Settings → Developer → Edit Config** , then add the` codewisp` server (copy the exact values from the dialog):


Save and **restart Claude Desktop** . The CodeWisp tools appear automatically.


####


Cursor


Copy the **Cursor config** from the dialog into` ~/.cursor/mcp.json` (same shape as above), then restart Cursor.


###


Step 3 — Try it


Ask your agent something like:


- “Read my CodeWisp project and summarize how it works.”


- “Make the player move faster, then validate the project.”


- “Add a pause screen and run it so I can see the HTML.”


The agent works on your real project — changes land in the editor immediately.


###


Keeping it secure


- A token is **scoped to one project** and acts as you on that project only.


- Need to cut off access? Open the same dialog and hit **Revoke & regenerate** — it invalidates every token issued for that project at once, and hands you a fresh one.


- Tokens expire on their own, so old configs stop working automatically.


That’s it — your game is now editable by any MCP-capable AI agent, with you in control of access.
