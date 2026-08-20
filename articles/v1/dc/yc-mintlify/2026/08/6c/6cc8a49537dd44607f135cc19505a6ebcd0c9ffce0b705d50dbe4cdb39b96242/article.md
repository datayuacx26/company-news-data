---
schema_version: "1.0.0"
document_id: "6cc8a49537dd44607f135cc19505a6ebcd0c9ffce0b705d50dbe4cdb39b96242"
company_key: "yc-mintlify"
company: "Mintlify"
source_id: "yc-mintlify-news-import-4dae4ee3e362"
canonical_url: "https://www.mintlify.com/blog/mint-signup"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T01:27:44.119720+00:00"
fetched_at: "2026-08-13T01:27:46.853511+00:00"
content_hash: "sha256:845ca59243e798ff2d37ee02e9dbe1300ebf3493486e192cf4bb44b4937b8086"
---

# Your agent can sign up for Mintlify fully on its own

Software increasingly needs to be agent-first. We saw 250+ million agent queries across Mintlify-powered docs in July 2026, representing about[66% of total measured traffic](https://www.mintlify.com/blog/state-of-docs-traffic) .


As developers use agents more and more, you'll want to ensure that each step of your product signup process is accessible through agent-friendly means.


So, introducing[mint signup](https://www.mintlify.com/docs/cli/commands#mint-signup) .


Our` mint signup` command allows your agent to sign up for Mintlify fully on its own through the CLI. It's designed for coding agents (like Cursor, Devin, etc.), so they can create new accounts, set up a docs project, and deploy without ever opening a browser.


## How mint signup works


Use` mint signup` to register a new Mintlify account from the terminal. Your agent can specify first name, last name, company, and email address as flags, or enter details interactively. When prompted to verify email, the agent can confirm the link directly if it has email access, or you can click the link to verify yourself. Once verified,` mint signup` logs in and stores auth locally for later commands.


This is the same pattern as[mint login](https://www.mintlify.com/docs/cli/commands#mint-login) , so new accounts get the same terminal experience.


```text
npm   i   -g   mint


mint   signup   \
--firstName   Jane   \
--lastName   Doe   \
--company   Acme   \
--email   jane@acme.com


```


## Why coding agents need a CLI


Tools like Claude Code, Cursor, and Devin already live in the terminal. They execute scripts, interpret output, and respond to errors dynamically.


The ability to access a product from the CLI opens up automation that a dashboard can't. From signup through preview, validation, custom domain setup, and beyond, your agent now has a complete terminal-first experience.


## What we already built for agents


To serve agents, we've built features that bridge the gap between human-readable documentation and programmable access:


- [llms.txt](https://www.mintlify.com/docs/ai/llmstxt) : Allows models to read product documentation from the command line.
- [MCP servers](https://www.mintlify.com/docs/ai/model-context-protocol) : Enable agents to interact with documentation through a standardized protocol.
- [Mintlify Index](https://www.mintlify.com/docs/search-index/index) : Provides retrieval over Mintlify-hosted docs.


Users familiar with Contextual Menu already know it can pull docs into Claude, ChatGPT, Cursor, Devin, etc. Now with` mint signup` , your agent can set you up with Mintlify from the terminal too.


## Set up docs from the terminal


With the help of our command-line tools, your agent can now set up your documentation fully on its own.


1. Create your Mintlify account using the command:` mint signup` . This initializes a new account and configures your profile.
2. Scaffold a new documentation project locally:` mint new` . This sets up a local project template you can start writing in.
3. Start development mode with:[mint dev](https://www.mintlify.com/docs/cli/preview) . This runs a live preview locally at[http://localhost:3000](http://localhost:3000/) , including full-text search and the AI companion.
4. Run validation:[mint validate](https://www.mintlify.com/docs/cli/commands#mint-validate) . This checks your configuration and content for issues.
5. Configure a domain in production:[mint add-domain](https://www.mintlify.com/docs/cli/commands#mint-add-domain)` <your-subdomain>.mintlify.app` .
