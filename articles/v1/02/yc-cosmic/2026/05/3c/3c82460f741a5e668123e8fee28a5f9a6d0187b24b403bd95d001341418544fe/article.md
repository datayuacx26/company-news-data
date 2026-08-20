---
schema_version: "1.0.0"
document_id: "3c82460f741a5e668123e8fee28a5f9a6d0187b24b403bd95d001341418544fe"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/mcp-vs-agent-skills"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T22:14:43.061733+00:00"
content_hash: "sha256:cde9dc68ad34c6d475bf8ffcbe973dc54e48a39e4fdff38a7fe878d631ed0612"
---

# MCP vs Agent Skills: What's the Difference and Which Do You Need?

If you have been following the AI developer tooling space lately, you have probably run into both "MCP" and "Agent Skills." These two concepts get mentioned in similar contexts, and the overlap in terminology makes it easy to conflate them.


They are not the same thing. They solve related but distinct problems, and understanding the difference will help you make better decisions about how to connect your AI tools to your content infrastructure.


This post covers both in the context of Cosmic, where we support both[MCP Server](https://www.cosmicjs.com/docs/mcp-server) and[Agent Skills](https://www.cosmicjs.com/docs/agent-skills) (for Cursor, Claude Code, GitHub Copilot, and other AI coding tools). But the concepts apply broadly.


---


## What Is MCP?


MCP stands for Model Context Protocol. It is an open standard, originally published by Anthropic, that defines how AI assistants can discover and call external tools.


The basic idea: instead of hard-coding integrations between AI models and specific services, MCP creates a standard interface. An AI assistant that speaks MCP can connect to any MCP server. An MCP server that exposes tools can be used by any MCP-compatible client.


Think of it like USB-C for AI tools. Before USB-C, every device had its own connector. USB-C standardized the interface so any device can plug into any port. MCP does the same for AI tools and the services they need to interact with.


### How MCP Works


An MCP server exposes a list of tools. Each tool has a name, a description, and a schema defining its inputs and outputs. When you connect an AI assistant (like Claude Desktop, Cursor, or GitHub Copilot) to an MCP server, the assistant can see those tools and call them when relevant.


For example, Cosmic's MCP server exposes tools like , , , , and . When you connect Claude Desktop to your Cosmic bucket via MCP, Claude can:


- List your content types
- Search for objects
- Create new content
- Upload media
- Update existing objects


All from within your AI assistant's interface, without you switching tabs or writing API calls manually.


### Cosmic's Hosted MCP Server


Cosmic runs a hosted MCP endpoint at:


```text


```


Connecting is a three-line config addition. For Claude Desktop:


```text


```


For Cursor:


```text


```


Once connected, your AI coding assistant can read and write your Cosmic content directly. You can ask it to "create a new blog post draft about our latest product update" and it will call the Cosmic API, create the object, and confirm what it did, all without you leaving your editor.


### The 18 Tools Cosmic's MCP Exposes


Cosmic's MCP server gives AI assistants 18 tools across four categories:


- **Object Types:** List, get, create, update, delete content models
- **Objects:** Search, list, get, create, update, delete, bulk-fetch content
- **Media:** List, upload, delete media assets
- **AI Generation:** Generate text, images, video, and audio using Cosmic's AI


These map directly to the Cosmic REST API, so anything your app can do via the API, your AI assistant can now do through conversation.


---


## What Are Agent Skills?


[Agent Skills](https://www.cosmicjs.com/docs/agent-skills) are a different concept. While MCP is about connecting an AI assistant to external tools, Agent Skills are about extending what an AI coding environment can do within the context of your specific project.


The term "skills" shows up in different forms depending on the tool:


- **Cursor** calls them "rules" or "context" files (, )
- **Claude Code** uses files at the project or directory level
- **GitHub Copilot** has custom instructions via


The common thread: these are structured instructions, context, and knowledge that you give your AI coding assistant to make it more effective in your specific codebase.


### What Agent Skills Actually Do


Think of Agent Skills as onboarding documentation written specifically for your AI assistant. When Claude Code opens your project, it reads your file and learns:


- How your project is structured
- What conventions you follow
- Which packages and imports to use
- What to avoid
- How to run tests, build, and deploy


Without these instructions, AI coding assistants make educated guesses based on what they have seen in training data. With them, they behave like a developer who has already been onboarded to your project.


### A Real Example: Cosmic Skills for Claude Code


Here is what a for a Cosmic-powered project might look like:


```text


```


Now when Claude Code helps you build a feature, it already knows your SDK, your content model, your conventions, and your constraints. It does not have to guess.


### Agent Skills Are Not Just Documentation


Agent Skills can also include:


- **Workflow instructions:** "When creating a new content type, always add a field and a field."
- **Testing requirements:** "Every new function needs a test. Run before submitting any changes."
- **Code style rules:** "Use TypeScript strict mode. No implicit any. All async functions should handle errors explicitly."
- **Tool preferences:** "Use Zod for schema validation. Use Tailwind for styling. Do not add new dependencies without asking."


This is different from MCP. MCP gives your AI assistant access to external tools. Agent Skills give your AI assistant knowledge about how to use your project correctly.


---


## MCP vs Agent Skills: The Key Differences


MCP Agent Skills


What it is A protocol for connecting AI to external tools Project-specific context and instructions for AI coding assistants


How it works AI calls tools exposed by an MCP server AI reads instruction files in your project


Scope Cross-tool standard (any MCP client can use any MCP server) Project-specific (instructions live in your repo)


What it enables Reading and writing external services from AI Smarter, more accurate code generation in your specific codebase


Where it lives On a server (hosted or local) In your project files (, , etc.)


Primary users Developers using AI assistants Developers using AI coding tools in a specific project


Requires setup Config file pointing to MCP server URL Markdown or text files in the project directory


MCP and Agent Skills are complementary, not competing. You often want both.


---


## Using Both Together: A Practical Workflow


Here is how MCP and Agent Skills work together in a real Cosmic-powered project:


**MCP handles access:**
You connect Cursor to your Cosmic bucket via MCP. Now Cursor can see your content types, search your objects, and create content directly.


**Agent Skills handle context:**
Your file tells Cursor to always use , to filter published content, to use specific field names, and to follow your TypeScript conventions.


**Together:**
When you ask Cursor to "add a new page that lists all published case studies," it:


1. Knows to use (from your skills)
2. Calls the MCP tool to inspect the object type and its metafields (via MCP)
3. Generates accurate TypeScript that fetches the right fields with the right filters
4. Follows your project conventions without being reminded


Without MCP, the AI is guessing about your content schema. Without Agent Skills, it might use the wrong SDK or ignore your conventions. With both, it behaves like a developer who has read your docs and has direct database access.


---


## Which One Do You Need?


**You need MCP if:**


- You want your AI assistant to read, create, or modify content in your Cosmic bucket from within your editor or chat interface
- You want to manage content through conversation without switching to the Cosmic dashboard
- You are building complex content operations and want to describe them in natural language rather than code


**You need Agent Skills if:**


- You want your AI coding assistant to generate accurate, convention-compliant code for your specific project
- You have established patterns (imports, field names, error handling) that you want the AI to follow automatically
- You want to reduce the time spent correcting AI-generated code that breaks your conventions


**You likely want both if:**


- You are actively developing a Cosmic-powered project and using AI tools to write code
- Your team uses multiple AI tools (Cursor, Claude Code, Copilot) and wants consistent behavior across all of them


---


## Setting Up Cosmic's MCP Server


The Cosmic MCP server is hosted and ready. No installation required. Hosted at:


```text


```


1. Get your bucket slug and API keys from your Cosmic dashboard under **Settings > API Access**
2. Add the config to your AI assistant (see the snippets above for Claude Desktop and Cursor)
3. Restart your AI assistant
4. Test it: ask your assistant to list your object types


For the self-hosted option, install the npm package:


```text


```


Set your environment variables and point your AI assistant to the local server. The hosted and self-hosted versions expose the same 18 tools.


Full setup guide:[cosmicjs.com/docs/mcp-server](https://www.cosmicjs.com/docs/mcp-server)


---


## Setting Up Agent Skills for Your Project


For Claude Code, create a in your project root. For Cursor, add rules to . For GitHub Copilot, use .


Start with:


- The correct SDK and import patterns
- Your content type names and key metafields
- Any conventions specific to your stack (TypeScript config, styling approach, testing setup)
- Explicit "do not" rules for common mistakes


You do not need to write a novel. A focused, accurate one-page instruction file is more useful than a sprawling document that the AI has to parse through.


Learn more in the[Cosmic Agent Skills documentation](https://www.cosmicjs.com/docs/agent-skills) .


---


## The Bigger Picture


MCP and Agent Skills represent two different answers to the same underlying problem: AI coding assistants are general-purpose, but your project is specific.


MCP gives your AI assistant live access to your data. Agent Skills give it institutional knowledge about your project. Together, they close the gap between what AI tools can do in general and what they can do for you specifically.


For developers building on Cosmic, both tools are available today. The MCP server is hosted and ready to connect. Agent Skills live in your project files and take twenty minutes to set up.


The result is an AI coding environment that knows your content model, follows your conventions, and can read and write your CMS directly. That is a meaningfully different experience from a general-purpose code assistant.


---


## Get Started


Cosmic works with both MCP Server and Agent Skills for Cursor, Claude Code, and GitHub Copilot.


- [Connect the Cosmic MCP Server](https://www.cosmicjs.com/docs/mcp-server) to your AI assistant in minutes
- [Explore Cosmic Agent Skills](https://www.cosmicjs.com/docs/agent-skills) to give your AI coding tools project-specific context
- [Start a free Cosmic account](https://app.cosmicjs.com/signup) to get your bucket slug and API keys
- [See the full AI documentation](https://www.cosmicjs.com/ai) for agents, workflows, and integrations
- Want a live walkthrough?[Book 30 minutes with Tony](https://calendly.com/tonyspiro/general-meeting)


No credit card required to get started. Free plan includes 1 Bucket, 1,000 Objects, and full API access.


---


## Frequently Asked Questions


**Is MCP only for Anthropic's tools?**
No. MCP is an open protocol. It was initially published by Anthropic, but it is supported by Cursor, GitHub Copilot, and many other AI tools. Cosmic's MCP server works with any MCP-compatible client.


**Do Agent Skills work across all AI coding tools?**
Each tool has its own format. works with Claude Code. works with Cursor. works with GitHub Copilot. The concepts are the same; the file names differ. You can maintain separate files for each tool, or write instructions general enough to use in all of them. See the[Cosmic Agent Skills docs](https://www.cosmicjs.com/docs/agent-skills) for examples.


**Does Cosmic support both hosted and self-hosted MCP?**
Yes. The hosted endpoint at requires no installation. The self-hosted option via works offline or for fully local setups. Both expose the same 18 tools.


**Is the Cosmic MCP server free touse?**
Yes. The Cosmic MCP server is included with every Cosmic account, including the free plan. You only pay for your Cosmic usage (Objects, API requests, media storage) based on your plan. There is no additional charge for MCP access.


**Can I use MCP and Agent Skills without being a developer?**
MCP is most useful inside developer tools like Cursor, Claude Code, and Claude Desktop, so some technical comfort helps. Agent Skills are written in plain markdown, so content editors and product managers can contribute to them even if they do not write the code themselves. For non-technical users, the[Cosmic dashboard](https://app.cosmicjs.com/) and built-in AI features may be a better starting point.


**What happens if my MCP config has the wrong API keys?**
The MCP server will return an authentication error, and your AI assistant will surface it in the conversation. Double-check your read key and write key under **Settings > API Access** in the Cosmic dashboard. Make sure the header uses the format with a colon between the two keys.


**Can I limit what the MCP server can do?**
Yes. If you only provide a read key (omit the write key), the MCP server will only expose read operations. This is useful when you want your AI assistant to query content without the ability to modify it. For write access, include both keys.


---


Ready to connect your AI tools to your content?[Start with the Cosmic MCP Server](https://www.cosmicjs.com/docs/mcp-server) or[explore Agent Skills](https://www.cosmicjs.com/docs/agent-skills) to get the most out of your AI coding workflow.
