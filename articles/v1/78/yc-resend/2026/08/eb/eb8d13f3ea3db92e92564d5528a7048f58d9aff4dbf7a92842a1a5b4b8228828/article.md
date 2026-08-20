---
schema_version: "1.0.0"
document_id: "eb8d13f3ea3db92e92564d5528a7048f58d9aff4dbf7a92842a1a5b4b8228828"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/agent-plugins-standard"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T18:12:53.051942+00:00"
fetched_at: "2026-08-13T18:12:53.931430+00:00"
content_hash: "sha256:b68d83e21ba49bbb131514ba799a6cbc03b32120b6a31e37f393f708325619ca"
---

# Agent Plugin Support

Today,[Resend Skills](https://github.com/resend/resend-skills) announces support for the[Agent Plugins Standard](https://agent-plugins.org/) , an open, vendor-neutral standard for packaging skills and MCP servers into a portable plugin.


## What is the Agent Plugins Standard?


Two tools have become critical in agentic development:


- **MCP:** agent tools for accomplishing tasks
- **Skills:** domain-specific, opinionated expertise


Many AI coding tools package these together in a plugin format. For instance, we launched our[Claude Connector](https://resend.com/changelog/claude-connector) and[Codex Plugin](https://resend.com/changelog/codex-plugin) . These plugins package together the MCP and agent skills in one simple install, and yet each one expects a **different vendor-specific manifest, folder structure, and setup process.**


[Agent Plugins](https://agent-plugins.org/) is a vendor-neutral plugin format that provides a simple, portable way to **package MCP and agent skills for use with any AI coding tool** , without a vendor-specific manifest required.


## How to use it


Nothing changes for existing installs like[Claude](https://resend.com/changelog/claude-connector) ,[Codex](https://resend.com/changelog/codex-plugin) , and[Cursor](https://cursor.com/marketplace/resend) .


Any other client that supports the Agent Plugins format can install both Resend Skills and the MCP server using the` plugin.json` config. Follow your AI client's instructions to install the plugin.


For example, in VS Code:


1. Open the Command Palette (` Ctrl+Shift+P` or` Cmd+Shift+P` )
2. Type` Chat: Install Plugin from Source` .
3. Paste` https://github.com/resend/resend-skills` in the input field.


After install, the plugin will be available in your AI client with full support for all the Resend skills. When you first use the MCP server, you will be prompted to authenticate.


At launch, Agent Plugins are supported in[ChatGPT](https://chatgpt.com/) ,[Codex](https://openai.com/codex/) ,[Cursor](https://cursor.com/) ,[GitHub Copilot](https://github.com/features/copilot) ,[Kiro](https://kiro.dev/) ,[VS Code](https://code.visualstudio.com/) ,[Hermes](https://github.com/nousresearch/hermes-agent) , and[OpenClaw](https://github.com/openclaw/openclaw) .


## How does it work


Every compatible client checks for` plugin.json` in the root of the plugin directory. The standard also allows a` skills/` directory and/or` mcp.json` configuration file.


```text
my-plugin/   ├── plugin.json   ├── skills/   │   └── summarize/   │       ├── SKILL.md   │       ├── scripts/   │       └── references/   └── mcp.json
```


- ` plugin.json` : defines the name, version, and details of the plugin
- ` skills/` : contains the plugin's skills, each following the Agent Skills spec
- ` mcp.json` : defines the plugin's MCP configuration


The standard was developed in a collaborative effort by Amazon, Cursor, GitHub, Microsoft, OpenAI, and Vercel and is already supported by many AI clients.


## Conclusion


We'll keep the vendor manifests and the Agent Plugins files in sync as the standard evolves. If you have any questions, please reach out to us and we'll be happy to help.
