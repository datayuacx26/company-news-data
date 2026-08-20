---
schema_version: "1.0.0"
document_id: "6d208bfe06ca8ffa35486a6edf02ee1c4b9947233a05dab2118877fea049630a"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/how-to-add-mcp-to-claude-code-2026/"
published_at: "2026-08-19T11:57:29+00:00"
first_seen_at: "2026-08-19T11:59:57.232966+00:00"
fetched_at: "2026-08-19T11:59:59.042415+00:00"
content_hash: "sha256:d4d9d3793e1eb0a1ac346700d28bda3b47fa9c5bff7275793b1f09f3917fe06b"
---

# How to Add MCP to Claude Code [2026]

To add an MCP server to Claude Code, run` claude mcp add --transport http <name> <url>` in your terminal, then run` claude mcp list` to check that it reports` ✔ Connected` . That is the happy path. The rest covers local stdio servers, the three configuration scopes and which one you want, what belongs in` .mcp.json` , authentication, and the failures that eat an afternoon.


Every command here was checked against[Anthropic's MCP documentation](https://code.claude.com/docs/en/mcp?ref=scrimba.com) on 6 August 2026. Claude Code moves fast, so a guide that does not say when it was verified is one you should not trust. The old` docs.claude.com/en/docs/claude-code/` pages now redirect to` code.claude.com/docs/en/` , which is why so many tutorials on this topic link somewhere that bounces.


## What Is MCP, in One Paragraph?


The **Model Context Protocol** is an open standard for connecting AI tools to external systems. An **MCP server** exposes tools over that standard, and Claude Code is the client that calls them.


The trigger for adding one is simple: you keep copying data into the chat from somewhere else. An issue tracker, a dashboard, a database, a design file. Connect the server once and Claude reads that system directly instead of working from whatever you remembered to paste.


This guide assumes Claude Code is installed and authenticated. If it is not, start with our[guide to using Claude Code](https://scrimba.com/articles/how-to-use-claude-code/) . For the protocol itself rather than the setup, our roundup of[MCP tutorials and courses](https://scrimba.com/articles/best-mcp-tutorials-and-courses/) goes deeper than this page will.


## Add Your First MCP Server


The worked example here is **Scrimba Explain** , a real HTTP MCP server rather than a placeholder URL. You ask your coding agent a question about your codebase and Explain turns the answer into a narrated video walkthrough. It works with Claude Code, with Codex and ChatGPT, and with any agent that supports MCP. Scrimba ships it free during open beta.


Run this in your shell, *not* inside a` claude` session. The general form comes first, then the live example:


```text
claude mcp add --transport http <name> <url>


claude mcp add --transport http explain https://scrimba.com/explain/mcp


```


The four parts of that command:


- ` claude mcp add` registers a server with Claude Code.
- ` --transport http` says the server lives at a URL rather than running as a local process.
- ` explain` is a name you invent, used to label the server's tools and to identify it in later commands.
- The URL is the endpoint, here` https://scrimba.com/explain/mcp` .


Claude Code confirms with an` Added` line and names the file it wrote. That means the configuration was saved. It says nothing about whether the server responded, whether your credentials work, or whether the URL is right.


Scrimba's own FAQ notes that Explain, like any AI tool, can make mistakes, so double-check anything important.


## Verify the Server Is Actually Connected


Verification is a separate step, and skipping it is why people conclude MCP is broken when their configuration never connected.


```text
claude mcp list


```


Each server comes back with one of six statuses:


Status What it means


` ✔ Connected` Working. This is what you want.


` ! Connected · tools fetch failed` Connected but could not list tools. Run` claude mcp get <name>` for detail.


` ! Needs authentication` Reachable, but wants a browser sign in or a token.


` ✘ Failed to connect` No usable response. See troubleshooting below.


` ✘ Connection error` The connection attempt threw. Same section.


` ⏸ Pending approval` A project scoped server you have not approved yet.


Some older Windows consoles cannot render those glyphs and print` √` and` ×` instead. For one server,` claude mcp get explain` prints the full configuration including its scope. Inside a session,` /mcp` opens a panel with the same statuses, plus reconnect and authenticate actions.


Then use it. Start` claude` and name the server in your first prompt, so the work goes through it rather than some other tool. Claude asks permission the first time it calls a new tool. Approve it, then check that the tool call carries the server name.


> An` Added` confirmation is a write receipt, not a connection. Until` claude mcp list` says connected, nothing has been tested.


## Add a Local stdio Server


A **stdio server** is a program Claude Code starts as a subprocess on your machine rather than a service it reaches over a URL. Reach for stdio when the tool needs local resources: your filesystem, a browser, a database socket. No` --transport` flag is needed, since stdio is the default:


```text
claude mcp add playwright \
-- npx -y @playwright/mcp@latest


claude mcp add --env AIRTABLE_API_KEY=YOUR_KEY --transport stdio airtable \
-- npx -y airtable-mcp-server


```


Two rules separate a command that works from one that does not.


First, the` --` separator. Everything after it is the command Claude Code runs, passed through untouched. Without it, Claude Code parses your server's flags as its own, so a server taking something like` --port 8080` fails in a way that looks like a bug. It is not. It is a missing separator.


Second, flag ordering around` --env` . That flag accepts multiple` KEY=value` pairs, so if the server name comes directly after it, the CLI reads your name as another pair and rejects it. Put another option in between, as the Airtable example does.


A stdio server's first` claude mcp list` can show` ✘ Failed to connect` while` npx` is still downloading the package. Wait a few seconds and run it again. If this terminal territory is new, our[command line basics guide](https://scrimba.com/articles/command-line-basics-for-web-developers/) covers what it assumes.


## The Three Config Scopes and When Each Is Right


**Scope** decides which projects a server loads in and whether your team gets it too. It causes the most confusion, because the default is narrower than most people assume.


Scope Flag Loads in Shared with team Stored in


local` --scope local` (default) Current project only No` ~/.claude.json` , under this project's path


project` --scope project` Current project only Yes, via version control` .mcp.json` in the project root


user` --scope user` All your projects No` ~/.claude.json` , top level` mcpServers`


The table says where things land. Here is the judgment it cannot carry:


1. **local** for anything holding a credential you would not commit, and for every experiment.
2. **project** for servers the whole team needs. The file is the shared artifact, so the decision travels with the repository.
3. **user** for the two or three servers you want everywhere without thinking about them.


Scope is fixed when you add a server, so changing it means removing the entry and adding it again:


```text
claude mcp remove explain --scope local


claude mcp add --scope user --transport http explain https://scrimba.com/explain/mcp


```


When the same name is defined in more than one place, Claude Code connects once and uses the highest precedence definition: local first, then project, then user, then plugin provided servers, then claude.ai connectors. The winning entry is used *whole* , never merged, so a URL in one scope and a header in another will not combine into a working server. Plugins can bundle their own servers, which is why one you never added can appear in` /mcp` .


## What Belongs in` .mcp.json` , and What Does Not


Project scope writes a file at your repository root, and it doubles as configuration as code. Both server shapes use the same structure:


```text
{
"mcpServers": {
"explain": {
"type": "http",
"url": "https://scrimba.com/explain/mcp"
},
"playwright": {
"type": "stdio",
"command": "npx",
"args": ["-y", "@playwright/mcp@latest"]
}
}
}


```


The` type` field is not optional. Claude Code reads a typeless entry as a stdio server, so an entry with a` url` and no` type` gets skipped with a message telling you to add one. If a vendor's documentation gives you` streamable-http` , paste it as is: that is the specification's name for the same transport, and Claude Code takes it as an alias for` http` .


A raw token does not belong in this file, because you are about to commit it. Use environment variable expansion instead, which works in` command` ,` args` ,` env` ,` url` , and` headers` :


```text
{
"mcpServers": {
"api-server": {
"type": "http",
"url": "${API_BASE_URL:-https://api.example.com}/mcp",
"headers": {
"Authorization": "Bearer ${API_KEY}"
}
}
}
}


```


Both forms work:` ${VAR}` for a plain substitution and` ${VAR:-default}` for a fallback. If a variable is unset and has no default, the file still loads. Claude Code warns in` claude mcp list` and uses the literal` ${VAR}` text, so the failure surfaces at connect time rather than load time.


Commit the file. When a teammate clones the repository, Claude Code asks them to approve the servers before anything runs, which is exactly right: a repository you downloaded should not start processes without a person saying yes. Edit the file, then restart the session, because Claude Code reads it at session start.


## Authenticating Servers That Need It


Remote servers come in two authentication shapes, and telling them apart saves time.


**OAuth sign in.** Add the server and` claude mcp list` reports` ! Needs authentication` . Run` /mcp` in a session, select the server, choose Authenticate, and finish in the browser. Tokens are stored and refreshed for you. You can also do it from the shell:


```text
claude mcp login sentry


claude mcp logout sentry


```


On a machine with no browser, such as an SSH session,` --no-browser` prints the authorization URL instead.


**Static token in a header.** Some services want a token rather than a sign in flow, passed at add time:


```text
claude mcp add --transport http github https://api.githubcopilot.com/mcp/ \
--header "Authorization: Bearer YOUR_GITHUB_PAT"


```


Here is the caveat that costs people an hour.` claude mcp add` saves the configuration without validating the credential, so a placeholder token produces a clean` Added` line and a server that fails later.


The distinction worth memorizing: if you configured an` Authorization` header and the server rejects it, Claude Code reports the server as *failed* , not as *needing authentication* , and it does not fall back to OAuth. A` failed` status on a server you thought was authenticated usually means a bad token, not a bad URL. Client secrets go to your system keychain or a credentials file rather than into your configuration.


## Troubleshooting


Check the status first, then match the symptom. Editing configuration before reading` claude mcp list` or` /mcp` is how an afternoon disappears.


### ` /mcp` says no MCP servers are configured


Almost always scope. Local servers are tied to the project you added them from, meaning the repository root, or the exact directory if you were not in a git repository. Add it again from where you are now, or use` --scope user` .


The other cause is a configuration file at a path Claude Code never reads: not` ~/.claude/mcp.json` , not` ~/.claude/.mcp.json` , not` ~/.claude/config/mcp.json` , not` %APPDATA%\\Claude\\mcp.json` . There are two correct locations,` ~/.claude.json` and` .mcp.json` at the project root.


### Status shows` Failed to connect` or` Connection error`


For a remote server, prove the URL is reachable before touching anything else:


```text
curl -I https://scrimba.com/explain/mcp


```


Read the result properly. A` 404` or` 405` means the server is up and does not answer that request type, which is normal for MCP endpoints that only take POST. A` 401` or` 403` means it is up and you need to authenticate. No response points at the URL or your network. If Claude Code says the endpoint was not found, the message shows only the origin, so run` claude mcp get <name>` and compare the full path against the vendor's documented endpoint.


For a stdio server, run the configured command yourself. If it starts and sits waiting for input, the server is fine and your entry is wrong, most often a missing` --` separator. If it errors, the message names what is missing, usually Node.js or a browser.


### Connection timed out at startup


The startup timeout defaults to 30 seconds, and a stdio server's first run can exceed that while` npx` downloads a package. Raise it, in milliseconds:


```text
MCP_TIMEOUT=60000 claude


```


### ` already exists`


A server with that name already exists at that scope. Remove it or pick a different name. If the name exists at more than one scope,` remove` says so, and` --scope` picks the copy to delete.


### The server connects but no tools appear


Open` /mcp` and select the server to see its tool list. An empty list almost always means a missing environment variable, usually an API key. Pass it with` --env KEY=value` when adding, or in the` env` field of the entry.


### Changes to` .mcp.json` do nothing


The file is read at session start, so restart. If the servers still do not appear, open` /mcp` and look for a parse warning, since malformed entries are skipped rather than announced loudly. If you rejected the server at its approval prompt earlier, reset with` claude mcp reset-project-choices` .


### Transport mismatch


Two versions of one mistake: the typeless` url` entry described above, and adding an SSE endpoint as HTTP. SSE is deprecated and HTTP is the right default, but some services still expose only SSE, and there` --transport sse` is the answer rather than a workaround.


## A Few Things Worth Knowing Once Servers Are Working


Tool search is on by default, so definitions load on demand rather than up front and adding several servers no longer eats your context window. Removing servers you have stopped using is still worth doing, since each one's tool names load into every session.


Three commands round out the set:` claude mcp add-json` takes a JSON blob when a vendor hands you one instead of a URL,` claude mcp add-from-claude-desktop` imports your Claude Desktop configuration, and` claude mcp serve` runs Claude Code itself as a stdio MCP server.


None of this locks you into one agent, which is worth remembering when you evaluate a server. The same URL works in other MCP clients, including[OpenAI Codex](https://scrimba.com/articles/how-to-use-openai-codex/) . For structured practice, Scrimba's[AI Engineer Path](https://scrimba.com/the-ai-engineer-path-c02v?ref=scrimba.com) includes a Model Context Protocol module alongside Agents, Context Engineering, and the Vercel AI SDK, at $24.50 per month billed annually. Location based, student, and promotional discounts are available, and free courses include certificates. Scrimba also maintains a list of[Claude Code tutorials and courses](https://scrimba.com/articles/best-claude-code-tutorials-and-courses-in-2026/) .


## Frequently Asked Questions


### How do I add an MCP server to Claude Code?


Run` claude mcp add --transport http <name> <url>` in your terminal for a remote server, or` claude mcp add <name>` followed by the separator and the command for a local stdio server. Then run` claude mcp list` and confirm the server reports as connected before you rely on it.


### Where does Claude Code store MCP server configuration?


In two files. Local and user scoped servers live in` ~/.claude.json` , local under the current project's path and user under the top level` mcpServers` key. Project scoped servers live in` .mcp.json` at your repository root. Claude Code does not read any other path.


### What is the difference between local, project, and user scope?


Local is the default and keeps a server private to you in one project. Project writes to` .mcp.json` so everyone who clones the repository gets it. User stores the server in your home configuration and loads it in every project you open, still private to you.


### Why is my MCP server not showing up in Claude Code?


Usually because it was added at local scope from a different project, so it does not load where you are now. Add it again from the current project or use` --scope user` . The other common cause is a configuration file written to a path Claude Code does not read.


### Do I need to restart Claude Code after editing` .mcp.json` ?


Yes. Claude Code reads` .mcp.json` at session start, so edits made during a session have no effect until you exit and start again. If servers still do not appear after a restart, open the` /mcp` panel and look for a parse warning on the entry you changed.


## Key Takeaways


- Adding a remote MCP server is one command,` claude mcp add --transport http <name> <url>` , followed by` claude mcp list` to verify.
- The default scope is local, so the server loads only in the project you added it from. That causes most missing server reports.
- For stdio servers, everything after the` --` separator is passed to the server untouched. Omitting it is the most common malformed command here.
- An entry in` .mcp.json` with a` url` and no` type` is read as a stdio server and skipped.
- An` Added` confirmation means the configuration was written, not that the server connected or that your credentials work.


## Sources


- Anthropic. "Connect Claude Code to tools via MCP."[https://code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp?ref=scrimba.com)
- Anthropic. "Connect to MCP servers."[https://code.claude.com/docs/en/mcp-quickstart](https://code.claude.com/docs/en/mcp-quickstart?ref=scrimba.com)
- Model Context Protocol. "Introduction."[https://modelcontextprotocol.io/introduction](https://modelcontextprotocol.io/introduction?ref=scrimba.com)
- Scrimba. "Explain." Self-reported product information.[https://explain.new/](https://explain.new/?ref=scrimba.com)
