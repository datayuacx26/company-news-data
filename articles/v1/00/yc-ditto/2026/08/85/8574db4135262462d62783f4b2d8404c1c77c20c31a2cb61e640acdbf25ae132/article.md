---
schema_version: "1.0.0"
document_id: "8574db4135262462d62783f4b2d8404c1c77c20c31a2cb61e640acdbf25ae132"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/set-up-your-ai-toolkit-in-minutes-with-dittos-agent-package"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T19:35:57.148902+00:00"
fetched_at: "2026-08-18T19:35:58.314474+00:00"
content_hash: "sha256:8a26a4aeafa668110ab661c6ae84355f46b7565846d131029d41caad054d34a7"
---

# Ditto Blog | Set up your AI toolkit in minutes with Ditto's Agent Package

We've made it easier to get started with Ditto's AI tools, assembled in one plug-and-play Agent Package.


The Ditto Agent Package is a single plugin — for Claude Code, Claude Desktop & Web, and Cursor — that bundles Ditto's entire agent integration into one install: the MCP, always-on instructions that keep every session consistent, and a couple of built-in commands for reviewing and auditing copy.


Installing the MCP by itself gets you a tool an agent *can* use, but you often have to explicitly tell your agent to use it when interacting with product copy. With Ditto’s Agent package, your agent is instructed to use it automatically, every session, without anyone having to ask.


Install is two commands: add the marketplace, then install the plugin. More on that in a minute.


## **Configuration shouldn’t stand in your way**


The Ditto MCP is powerful, but you shouldn't have to know how to configure it just to get going.


Even with the MCP installed, an agent won't reliably check Ditto unless someone remembers to prompt it to.


Our docs included example prompts to help steer the agent towards using Ditto's MCP, but they had to be manually copy-pasted, and drifted out of date whenever we’d make updates.


This is the same failure mode as any homegrown fix — a line in your[CLAUDE.md](http://claude.md/) file, a skill someone wrote last quarter, a Notion link pasted into a prompt out of habit. None of it is wrong, exactly, it's just not reliable. There’s too much variability between sessions, teammates, and prompts.


Ditto’s Agent Package gives you that consistency and reliability. Same integration, same way, for every teammate and every agent, every time. You always have the best, most up-to-date prompts available, and you offload the process of finding better ways for the agent to accomplish Ditto-related tasks. So you can take advantage of Ditto’s AI tools, with the confidence that everything is configured correctly and being used consistently.


## **What changes once it's installed**


MCP is on by default, as part of the install.


Every agent session starts with Ditto instructions already injected. The agent checks your style guide and your existing text in every product copy-related task.


Two commands also come built in to the Agent Package, and neither requires anyone to write their own prompt:


- ` /ditto-review` — checks the user-facing strings in your current diff against your style guide and existing Ditto text, and hands back a fix list
- ` /ditto-audit \[path\]` — same check, run against an entire directory instead of a diff


If your team uses[Ditto Specs](https://www.dittowords.com/blog/introducing-ditto-specs-integrate-your-style-guide-directly-into-your-design-system-in-code) to inject copy guidance alongside your design system, right in code, you get three more commands in this Agent Package:` /ditto-spec-audit` ,` /ditto-spec-component` , and` /ditto-spec-gaps` , for auditing and maintaining text specs at the component level.


Together, these features remove the dependence on “perfect use” of Ditto’s AI features, and makes consistency the default behavior of a tool your team's already using.


## **Setting it up**


Available now for Claude Code (CLI and desktop) and Cursor.


Install the plugin, connect it to your workspace, and you're done. Platform-specific steps live in the[developer docs](https://developer.dittowords.com/agent-setup-package/overview) .


Haven't set up Ditto MCP at all yet?[Check out what you can do with it](https://www.dittowords.com/blog/the-ditto-mcp-expanded-do-more-alongside-ditto-in-your-agent-workflows) — the Agent Package builds on top of it.


Want to see it running before you install anything?[Book time with us](https://calendly.com/ditto-team/ditto-chat) .
