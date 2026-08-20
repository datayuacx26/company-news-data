---
schema_version: "1.0.0"
document_id: "d83a48d230c16efe5b2f0167974a5c84fe75d55c89bfdcd1ea77c6910398d58b"
company_key: "yc-vapi"
company: "Vapi"
source_id: "yc-vapi-news-import-8dd0a49247bf"
canonical_url: "https://vapi.ai/blog/claude-skills"
published_at: "2026-02-25T00:00:00+00:00"
first_seen_at: "2026-07-24T05:51:02.715878+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:5d538e34a97be26d5e7c8399e1387b973ad54b6e173fbab7702a8730422619f5"
---

# Your AI Coding Assistant Just Learned to Build Voice Agents - Vapi AI Blog

If you build Vapi agents using the API, you probably work in Cursor, Claude Code, or something similar. And you have probably noticed that your coding agent has no idea how Vapi works.


You type something like "create a Vapi assistant that handles inbound support calls." Your agent either hallucinates the API shape, guesses at nonexistent parameters, or asks you to find the docs yourself. You end up copy-pasting documentation into your editor and manually explaining the API to a tool that is supposed to help you move faster.


The agent can write your backend logic, scaffold your infrastructure, and wire up your database. But when it comes to Vapi, it is flying blind.


Today, we are shipping Vapi Skills to fix that.


## What we shipped


Two things, designed to work together.


The first is a Vapi Skills repo that follows the Agent Skills standard (agentskills.io). With one install command, your coding agent gets structured, procedural knowledge for every core Vapi workflow: creating assistants, adding tools, making calls, configuring webhooks, setting up phone numbers, and building squads. The agent is not summarizing documentation. It is loading step-by-step instructions with the exact API schemas, so it generates working code on the first try.


The second is a Model Context Protocol (MCP) connector that gives your agent live access to the full Vapi documentation. Skills cover the core workflows. The MCP connector fills every gap: advanced configuration, SDK details, troubleshooting, and edge cases. Between the two, your agent has everything it needs to build and debug Vapi integrations without you leaving your editor.


## What this looks like in practice


You add Vapi Skills (instructions below). You type...


"Create a Vapi assistant that handles inbound customer support calls and escalates to a human if the caller is frustrated."


The agent loads the create-assistant skill, knows the exact schema, and generates working code. No hallucinated parameters. No context-switching to a browser tab. No copying from docs into your editor.


The skills available out of the box cover the workflows developers use most:` setup-api-key` ,` create-assistant` ,` create-tool` ,` create-call` ,` create-squad` ,` create-phone-number` ,` setup-webhook` , and` create-workflow` . For anything beyond these, the MCP connector queries the docs in real time.


## Why this matters


The shift here is subtle but significant.


Reading docs and building from reference is fine when you are learning. But when you are iterating on production agents, switching between your editor and documentation tabs slows you down. Every context switch costs time. And when your coding agent tries to help but gets the API wrong, it costs more time because you have to find the mistake, correct it, and verify the fix.


Skills give your agent the same structured knowledge that an experienced Vapi developer carries around. Not prose to summarize, but procedural instructions it can execute. The difference between "here is a paragraph about assistants" and "here is exactly how to create one, with every required field and valid option."


This is especially useful for developers who prototype fast and iterate often. If you are building multiple agents, testing different configurations, or wiring up new integrations weekly, the time saved compounds.


## Works across agents


Vapi Skills follow the Agent Skills standard, which means the same skill pack runs on Claude Code, Cursor, VS Code Copilot, Gemini CLI, OpenClaw, and any other Skills-compatible tool. One install covers your whole workflow regardless of which agent you prefer.


The MCP connector works the same way. Configure it once, and your agent has live access to the Vapi knowledge base alongside the pre-packaged skills.


## Get started


Install for any supported agent:


```text
npx skills add VapiAI/skills


```


Or for Claude Code:


```text
/plugin marketplace add VapiAI/skills
/plugin install vapi-voice-ai@vapi-skills


```


The only setup beyond the install is your Vapi API key:


```text
export VAPI_API_KEY="your-key"


```


The MCP connector auto-activates in supported agents with no additional configuration.


Skills are maintained by the Vapi team and versioned alongside the API, so your agent always has current information. No third-party wrappers. No stale documentation.


Let your coding agent handle the Vapi boilerplate. Focus on the product logic that matters.


[GitHub repo](https://github.com/VapiAI/skills) |[Vapi docs](https://docs.vapi.ai/) |[Get your API key](https://dashboard.vapi.ai/org/api-keys)


---


*Questions? Find us on[Discord](https://discord.gg/vapi) or atsupport@vapi.ai .*
