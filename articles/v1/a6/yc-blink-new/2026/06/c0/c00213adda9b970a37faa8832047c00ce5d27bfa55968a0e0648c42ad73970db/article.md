---
schema_version: "1.0.0"
document_id: "c00213adda9b970a37faa8832047c00ce5d27bfa55968a0e0648c42ad73970db"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/cursor-mcp-setup-guide"
published_at: "2026-06-12T13:03:02+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:75fe988d0313e68184c9e55853604acb3e150eac40d552501579ef6651374d5c"
---

# Cursor MCP Setup Guide: Connect Your Agent to Real Infrastructure

## Method 1: Cursor Settings UI


The UI path is the safest route for first-time setup. It opens the correct config file and confirms the server loaded correctly.


1


#### Open Settings


Press` Cmd + ,` (Mac) or` Ctrl + ,` (Windows/Linux) to open Cursor Settings.


2


#### Navigate to Tools & MCP


Click **Tools & MCP** in the left sidebar. Any currently configured servers appear here.


3


#### Add a new server


Click **New MCP Server** . Cursor opens your global` ~/.cursor/mcp.json` in the editor.


4


#### Paste your server config


Add your server block under the` mcpServers` key. See the JSON examples in Method 2 below.


5


#### Save and reload


Save the file. Toggle the server off and back on in the Tools & MCP panel to reload without a full restart.


The Settings UI edits your **global** config. To add a project-local server instead, open` .cursor/mcp.json` in your project root directly — Cursor has no UI picker for this distinction.


## Method 2: Edit mcp.json directly


Every server entry lives under the` mcpServers` key. Cursor supports three transport types. Pick the one that matches how your server runs.


### Local server (stdio)


stdio servers run as a subprocess on your machine. They're the most common type — any MCP npm package uses this format.


```text
{
"mcpServers"  : {
"filesystem"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-filesystem"  ,   "/path/to/dir"  ]
}
}
}
```


The` command` field is the binary to run. The` args` array passes arguments to it. Node.js 18 or higher is required — older versions fail silently.


### Remote server (Streamable HTTP or SSE)


Remote servers run on a cloud host or your team's infra. Connect over HTTPS and pass credentials in headers.


```text
{
"mcpServers"  : {
"remote-api"  : {
"url"  :   "https://mcp.example.com/mcp"  ,
"headers"  : {
"Authorization"  :   "Bearer YOUR_API_KEY"
}
}
}
}
```


Streamable HTTP is the current standard. SSE still works but is the older format — prefer HTTP when the server gives you a choice.


Cursor caps total MCP tools at 40 across all active servers. There's no hard limit on server count, but every server's exposed tools count against that 40. Trim to 4–6 servers you actively use and disable the rest in Settings > Tools & MCP.


## Method 3: One-click deep-links


Many MCP publishers include an **"Add to Cursor"** button directly on their documentation pages. Clicking it triggers a` cursor://` deep-link that pre-fills your` mcp.json` automatically.


This eliminates JSON syntax errors and puts the correct server name, command, and args in place. Check the server's docs before writing the config manually — the button is often right next to the install instructions.


The Blink plugin takes this a step further:` npx skills add blink-new/blink-plugin` installs and configures everything in one terminal command, no browser deep-link needed.


Cursor maintains an[official list of verified MCP servers](https://cursor.com/docs/mcp) in its documentation — a useful starting point for finding servers for databases, version control, and cloud services.


## Add Full-Stack Infrastructure to Cursor in 2 Commands


Every method above requires you to find servers, validate JSON, manage credentials, and maintain config files over time. For full-stack projects, that means separate accounts for database, auth, hosting, and deployment — each needing its own MCP entry, its own credentials, its own configuration.


Blink replaces all of that. Install the[14 skills](https://blink.new/docs/cloud/tools/skills) that give Cursor's agent database, auth, hosting, and deploy in two commands:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


No manual mcp.json editing. The CLI auto-configures MCP for you. Then ask Cursor:


> "Set up Blink Cloud in Cursor and build a full-stack app."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account, no Auth0 setup.


The before/after is stark: manually wiring three or four services, keeping credentials in sync, updating MCP entries when APIs change — versus two commands and a browser login. Every new team member who runs` npx skills add blink-new/blink-plugin` gets the full stack immediately.


[Learn more about Blink Cloud →](https://blink.new/cloud)


## Common issues and fixes


The most common cause: you edited the wrong file. Confirm you modified` ~/.cursor/mcp.json` for global config, or` .cursor/mcp.json` in your project root for project-local. After saving, toggle the server off and on in Settings > Tools & MCP. For newly added servers, a full Cursor restart is more reliable than toggling.


JSON does not allow trailing commas. Every server entry needs a comma between entries, but the last entry must have none. Paste your config into VS Code's JSON editor or jsonlint.com before saving — Cursor silently ignores a malformed file with no error message.


Cursor GUI apps inherit a restricted PATH that often excludes your shell's custom directories. Use the full binary path instead:` /usr/local/bin/npx` rather than` npx` . Alternatively, add your Node.js bin directory to your OS-level system PATH in System Preferences or Environment Variables. Node.js 18 or higher is required.


Confirm the URL uses` https://` not` http://` . Verify the server is running and publicly reachable. Check that your` Authorization` header matches the server's expected format — Bearer token, raw API key, and Basic auth all look different in the JSON. Test the endpoint with` curl` before debugging Cursor.


Some servers need explicit invocation. Type the tool name directly: "Use the \[tool_name\] tool to...". If you've loaded many servers and hit the 40-tool cap, Cursor stops loading tools once the limit is reached without any warning. Disable unused servers in the Tools & MCP panel to free up capacity.


The file must be at` .cursor/mcp.json` in your project root — not inside a subdirectory or nested` .cursor/` folder. Confirm the path by running` ls .cursor/` from your project root. In a monorepo, each workspace root needs its own` .cursor/mcp.json` .


## FAQ


Global (` ~/.cursor/mcp.json` ) applies to every Cursor workspace on your machine. Project (` .cursor/mcp.json` ) applies only to that repo and takes precedence when both files define the same server name. Commit the project file to share your MCP setup with teammates — they get it automatically on clone.


There's no hard limit on server count, but Cursor enforces a 40-tool cap across all active servers. Each server's exposed tools count against that limit. Keep 4–6 active servers maximum and toggle the rest off in Settings > Tools & MCP.


Use stdio for local servers running as npm processes on your machine. Use Streamable HTTP for remote hosted services — it's the current standard. SSE still works but is the older format; choose HTTP when the server docs give you a choice.


Yes. Put` .cursor/mcp.json` in your project root and commit it to source control. Every teammate who opens the project in Cursor gets the same server setup automatically. Don't hardcode secrets in the committed file — use environment variable references or a secrets manager, and document the required env vars in your README.


Not always. Toggle the server off and on in Settings > Tools & MCP to reload the config. For brand-new servers being added for the first time, a full Cursor restart is more reliable than toggling.


Running` npx skills add blink-new/blink-plugin` installs 14 skills and auto-configures MCP — no manual mcp.json editing. It replaces the need for separate database setup (no Supabase), auth configuration (no Auth0), and hosting management (no Vercel). Your Cursor agent manages the full stack through Blink Cloud.
