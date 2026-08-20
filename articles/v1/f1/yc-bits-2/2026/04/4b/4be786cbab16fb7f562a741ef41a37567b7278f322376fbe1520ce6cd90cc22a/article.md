---
schema_version: "1.0.0"
document_id: "4be786cbab16fb7f562a741ef41a37567b7278f322376fbe1520ce6cd90cc22a"
company_key: "yc-bits-2"
company: "Bits"
source_id: "yc-bits-2-news-import-8ae56e6aae1c"
canonical_url: "https://klausai.com/blog/openclaw-mcp-servers-how-to-connect-your-agent-to-any-tool/"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-24T21:07:46.493176+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:0235dd52efbe7503ebaf5d5f39f866dc0feca6f99db3f36c5026befed10a48ae"
---

# OpenClaw + MCP Servers: How to Connect Your Agent to Any Tool

# OpenClaw + MCP Servers: How to Connect Your Agent to Any Tool


MCP has become the standard way AI agents talk to external tools. Every week I see another announcement: Slack has an MCP server, Postgres has one, GitHub has one. The protocol itself is well-documented. What is not well-documented is the part that matters: getting these servers to actually work with OpenClaw on a real VM.


I’ve been adding MCP servers to our customer VMs at Klaus and testing which ones are worth the setup. Some start cleanly. Some need environment variables nobody mentions in the README. Some crash silently and you spend an hour figuring out why your agent lost access to GitHub.


## What Is MCP and Why Does OpenClaw Support It?


MCP (Model Context Protocol) is an open standard that lets AI applications connect to external systems like databases, APIs, and developer tools through a universal interface.[Anthropic](https://www.anthropic.com/news/model-context-protocol) launched it in November 2024 as an open-source protocol with SDKs, a specification, and a set of pre-built servers for tools like Google Drive, Slack, GitHub, and Postgres.


The analogy from the[official docs](https://modelcontextprotocol.io/docs/getting-started/intro) is “USB-C for AI.” Instead of building a custom integration for every tool your agent needs, MCP gives you a universal connector. One protocol, many servers, any tool.


The adoption has been fast.[OpenAI adopted MCP](https://en.wikipedia.org/wiki/Model_Context_Protocol) in March 2025. Google DeepMind followed in April 2025. By December 2025, Anthropic transferred governance to the Agentic AI Foundation under the Linux Foundation, co-founded with Block and OpenAI. The[official MCP server repository](https://github.com/modelcontextprotocol/servers) has 83.7k GitHub stars.


Here’s what that means for OpenClaw: your agent can discover and call tools exposed by any MCP server, the same way it calls skills. OpenClaw supports three transport types for MCP connections ([docs](https://docs.openclaw.ai/cli/mcp) ):


Transport How it works When to use


**stdio** Runs the server as a local child process Most common. Used for servers installed via npm/pip on your machine.


**SSE/HTTP** Connects to a remote server over HTTP Cloud-hosted MCP servers. Needs an API key or bearer token.


**Streamable HTTP** Bidirectional streaming over HTTP Real-time data feeds. Less common, but growing.


Stdio is what you’ll use 90% of the time. The server runs on the same machine as OpenClaw, communicates over standard input/output, and your agent picks up the tools automatically.


## How to Add Your First MCP Server


Adding an MCP server takes two steps: define it in your config, and verify OpenClaw sees the tools.


### Option 1: Edit openclaw.json directly


Open your` openclaw.json` and add a` mcpServers` block. Here’s the minimal config to add the[Filesystem server](https://github.com/modelcontextprotocol/servers) , which gives your agent read/write access to files on your machine:


```text
{
"mcpServers"  : {
"filesystem"  : {
"command"  :   "npx"  ,
"args"  : [
"-y"  ,
"@modelcontextprotocol/server-filesystem"  ,
"/home/user/documents"
]
}
}
}
```


You can just ask Klaus to do this and he will understand.


The last argument (` /home/user/documents` ) restricts which directory the server can access. This matters. Without it, your agent could read and write anywhere on your filesystem.


### Option 2: Use the CLI


If you prefer not to edit JSON by hand, OpenClaw has CLI commands for managing MCP servers ([docs](https://docs.openclaw.ai/cli/mcp) ):


```text
# Add a server
openclaw   mcp   set   filesystem   '{"command":"npx","args":["-y","@modelcontextprotocol/server-filesystem","/home/user/documents"]}'


# List all configured servers
openclaw   mcp   list


# View a specific server's config
openclaw   mcp   show   filesystem


# Remove a server
openclaw   mcp   unset   filesystem
```


### Verifying it works


After adding the server, run` openclaw mcp list` to confirm it’s registered. Then start a conversation with your agent and ask it what tools are available. If the Filesystem server is running, your agent should list tools like` read_file` ,` write_file` ,` list_directory` , and` search_files` .


If nothing shows up, the most common issues are:


- **Node.js not installed** (or wrong version). Most MCP servers need Node 18+. Run` node --version` to check.
- **npx not in PATH.** If you installed Node via nvm, make sure nvm is loaded in the shell OpenClaw uses.
- **Server crashed on startup.** Run the npx command directly in your terminal to see if it throws errors. Silent crashes are the most frustrating debugging experience with MCP servers.


## Which MCP Servers Are Actually Worth Adding?


The five servers worth starting with are Filesystem, GitHub, Postgres, Memory, and Fetch. There are[7 official reference servers](https://github.com/modelcontextprotocol/servers) maintained by the MCP steering group, plus hundreds of community-built ones. You don’t need all of them. Here are the ones I’ve tested and found useful on our Klaus VMs.


Server What it does Setup complexity Notes


**Filesystem** Read, write, search files on disk Low (no API key) Restrict the directory path. Your agent will use this constantly.


**GitHub** Create issues, read repos, manage PRs Medium (needs personal access token) Set` GITHUB_PERSONAL_ACCESS_TOKEN` as an env var. Worth the setup if your workflow touches code.


**Postgres** Query databases, explore schemas Medium (needs connection string) Useful for agents that pull reports or manage data. Use a read-only connection for safety.


**Memory** Persistent knowledge graph across sessions Low (no API key) Stores facts between conversations. Complements OpenClaw’s[native memory](https://klausai.com/blog/openclaw-memory-how-to-make-your-agent-actually-remember) , doesn’t replace it.


**Fetch** Retrieve and convert web pages to markdown Low (no API key) Gives your agent the ability to read any URL. Useful for research tasks.


These five cover the most common use cases: file management, code, databases, memory, and web access. Start here.


For servers that need API keys, add them as environment variables in the server config:


```text
{
"mcpServers"  : {
"github"  : {
"command"  :   "npx"  ,
"args"  : [  "-y"  ,   "@modelcontextprotocol/server-github"  ],
"env"  : {
"GITHUB_PERSONAL_ACCESS_TOKEN"  :   "ghp_your_token_here"
}
}
}
}
```


A few servers I’ve tested that aren’t in the official set but work well:


- **Brave Search** (community): Gives your agent web search. Needs a Brave API key. Reliable, returns clean results.
- **Slack** (official): Lets your agent read and post to Slack channels. Needs a Slack bot token. Setup takes 10 minutes in the Slack API dashboard.
- **Obsidian** (community): Reads from your Obsidian vault. Useful if your notes are in Obsidian and you want your agent to reference them.


One thing to keep in mind: each MCP server you add increases the token overhead in your agent’s context window because OpenClaw has to load the tool definitions. With 5 servers, the overhead is manageable. With 12+, you might notice your agent’s context filling up faster.


## MCP Servers vs OpenClaw Skills: When to Use Which


MCP servers and OpenClaw skills both give your agent access to external tools, but they work differently and serve different purposes.


MCP Servers OpenClaw Skills


**Standard** Open protocol, works with Claude, ChatGPT, Cursor, VS Code OpenClaw-native


**Portability** Same server works across any MCP client Only works in OpenClaw


**Integration depth** Tool calls only Can use workspace files, session context, bootstrap files


**Setup** JSON config + server install Skill file in ~/.openclaw/skills/ or from[Clawhub](https://klausai.com/blog/best-openclaw-skills-for-business)


**Ecosystem** Hundreds of community servers 20,000+ skills on Clawhub


The practical version:


**Use an MCP server when** a well-maintained server already exists for the tool you need. Postgres, GitHub, Slack, Filesystem all have solid MCP servers. You get the benefit of a standard interface, and if you switch from OpenClaw to another agent platform later, your server configs come with you.


**Use a skill when** you need OpenClaw-specific behavior. Skills can read bootstrap files (AGENTS.md, TOOLS.md, MEMORY.md), access workspace files, and use session context. They’re also the right choice for niche tools where no MCP server exists.


On Klaus, we pre-configure both. Our VMs come with the most useful MCP servers ready to go, and we ship[recommended skills](https://klausai.com/blog/best-openclaw-skills-for-business) for everything else. In practice, most of our customers use a mix: MCP servers for standard tools (databases, file access, search) and skills for OpenClaw-specific workflows (daily reports, lead enrichment, email management).


## Advanced Setup: Routing, Environment Variables, and Custom Servers


### Routing Servers to Specific Agents


If you run multiple agents on the same OpenClaw instance, you might want different agents to have access to different tools. The` mcpServers` block in your config applies globally, but you can override it per agent by placing a separate config in the agent’s directory.


This is useful for security: your research agent gets read-only database access, while your dev agent gets GitHub write access. Neither sees the other’s tools.


### Managing API Keys and Secrets


If other people have access to your machine, pasting API keys directly into` openclaw.json` means anyone who can read the file has your keys. Better approach:


1. Store keys in environment variables (e.g., in` .env` or your shell profile)
2. Reference them in the server config’s` env` block
3. On Klaus VMs, we store secrets in a protected directory that isn’t accessible through the OpenClaw chat interface


For remote MCP servers (SSE/HTTP transport), authentication usually goes in the` headers` field:


```text
{
"mcpServers"  : {
"remote-tool"  : {
"url"  :   "https://mcp.example.com"  ,
"headers"  : {
"Authorization"  :   "Bearer your_api_key"
},
"connectionTimeoutMs"  :   5000
}
}
}
```


### Building a Custom MCP Server


When no existing server covers your tool, you can build one. The[MCP specification](https://modelcontextprotocol.io/docs/getting-started/intro) provides SDKs in TypeScript, Python, and[9 other languages](https://en.wikipedia.org/wiki/Model_Context_Protocol) .


The basic structure: define tools (functions your agent can call), resources (data your agent can read), and prompts (templates your agent can use). A minimal TypeScript server that exposes one tool takes about 50 lines of code.


I won’t walk through the full build process here because the[MCP docs](https://modelcontextprotocol.io/docs/getting-started/intro) cover it well. But the short version: if you can write a function that calls an API, you can wrap it in an MCP server. The SDK handles all the protocol plumbing.


## What Doesn’t Work (Yet)


MCP is still young, and some parts are rough.


**Token overhead is real.** Every MCP server registers its tools in your agent’s context window. The tool definitions for 5 servers might add 2,000-3,000 tokens. That’s fine for most conversations, but it adds up if you’re running 10+ servers or working with long documents.


**Silent failures.** Some community servers crash on startup without logging anything useful. The server process exits, OpenClaw doesn’t see the tools, and there’s no error message telling you what went wrong. My debugging approach: run the server’s command directly in your terminal first. If it works standalone, it will work in OpenClaw.


**Unmaintained servers.** The ecosystem is growing fast, which also means some community servers haven’t been updated in months. Check the last commit date before adding a server from GitHub. If the last update was 6+ months ago and there are open issues, consider finding an alternative or forking it.


**Security is your responsibility.** MCP servers can access your local filesystem, databases, and APIs. A malicious or poorly written server could read files it shouldn’t, or make API calls you didn’t authorize.[Security research in April 2025](https://en.wikipedia.org/wiki/Model_Context_Protocol) identified prompt injection and tool-poisoning attack vectors. Stick to official and well-reviewed community servers, and always restrict access paths (like the directory argument on the Filesystem server).


## Frequently Asked Questions


### How many MCP servers can I run at once?


No hard limit. Practically, 5-8 servers is a good range for most workflows. Beyond that, the token overhead from tool definitions starts to eat into your conversation context. If you need more, figure out which tools your agent actually uses regularly and remove the rest.


### Do MCP servers work on Klaus or only self-hosted OpenClaw?


MCP servers work on both. Klaus VMs come with the` openclaw mcp` commands available, and you can add, remove, or configure servers the same way you would on a self-hosted instance. We also pre-configure common servers so you don’t have to set them up manually.


### Can I use MCP servers alongside regular OpenClaw skills?


Yes. Your agent sees MCP tools and skills in the same tool list. No conflict. In practice, most people use both: MCP servers for standard tool connections (databases, file access, search) and skills for OpenClaw-specific behavior (report generation, email workflows, automations).


### Is MCP secure?


The protocol itself is well-designed, and governance moved to the[Linux Foundation](https://en.wikipedia.org/wiki/Model_Context_Protocol) in December 2025. But security depends on the specific servers you run. Only install servers from trusted sources. Restrict file access paths. Use read-only database connections where possible. And keep servers updated, because the ecosystem is still maturing.


## Key Takeaways


- MCP is the universal standard for connecting AI agents to external tools. OpenClaw supports it natively through the` openclaw mcp` CLI commands ([docs](https://docs.openclaw.ai/cli/mcp) ).
- Start with 5 servers: Filesystem, GitHub, Postgres, Memory, and Fetch. These cover the most common workflows.
- Use MCP when a well-maintained server exists for the tool you need. Use OpenClaw skills when you need OpenClaw-specific features or the tool is too niche for an MCP server.
- Watch the token overhead. Each server adds tool definitions to your context window. Five servers is fine; twelve starts to matter.
- The ecosystem is growing fast.[70% of large SaaS brands](https://thenewstack.io/why-the-model-context-protocol-won/) now offer MCP servers. What’s rough today will smooth out.
- On Klaus, MCP works out of the box. We pre-configure common servers so you can focus on using the tools, not setting them up.


---


Want MCP pre-configured on a cloud VM?[Klaus](https://klausai.com/) ships with the most useful servers ready to go, alongside[pre-configured skills](https://klausai.com/blog/best-openclaw-skills-for-business) and integrations. Sign up and start connecting your agent to any tool.


If you want to build your own tools instead, check out our guide on writing[custom OpenClaw skills](https://klausai.com/blog/how-to-write-custom-openclaw-skills) (coming soon).


## Sources


### Primary Sources


- Anthropic. “Introducing the Model Context Protocol.” November 2024.[https://www.anthropic.com/news/model-context-protocol](https://www.anthropic.com/news/model-context-protocol)
- Model Context Protocol. “What is the Model Context Protocol (MCP)?”[https://modelcontextprotocol.io/docs/getting-started/intro](https://modelcontextprotocol.io/docs/getting-started/intro)
- OpenClaw. “MCP CLI Documentation.”[https://docs.openclaw.ai/cli/mcp](https://docs.openclaw.ai/cli/mcp)
- Model Context Protocol. “MCP Servers Repository.” GitHub.[https://github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)


### Secondary Sources


- Wikipedia. “Model Context Protocol.”[https://en.wikipedia.org/wiki/Model_Context_Protocol](https://en.wikipedia.org/wiki/Model_Context_Protocol)
- The New Stack. “Why the Model Context Protocol Won.”[https://thenewstack.io/why-the-model-context-protocol-won/](https://thenewstack.io/why-the-model-context-protocol-won/)
