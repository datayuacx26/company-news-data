---
schema_version: "1.0.0"
document_id: "b1282387b116ae36c717ae54a1c8294b124ac66fc67f0a65d92ecdc7367ac8b8"
company_key: "yc-dyspatch"
company: "Dyspatch"
source_id: "yc-dyspatch-rss-ea7cf496860a"
canonical_url: "https://www.dyspatch.io/blog/agent-operators-in-dyspatch-connect-claude-to-your-workspace-in-one-click/"
published_at: "2026-08-05T15:27:22+00:00"
first_seen_at: "2026-08-05T15:34:10.750275+00:00"
fetched_at: "2026-08-05T15:34:12.400292+00:00"
content_hash: "sha256:d98874496c9da1d6f012c46d2a7cc6585acb74aba8f8bd00769da56ef356549f"
---

# Agent Operators in Dyspatch: Connect Claude to Your Workspace in One Click

The last time we wrote about MCP, you still had to install it. Node, npx, an API key pasted into a config file, repeated on every machine. It worked, but it was setup only a developer could love.


Agent Operators in Dyspatch hands that busywork to your AI client. It's a hosted MCP connection you add to Claude in one click, straight from the connector directory. No API key. No developer ticket. No config file. You approve access once, and Claude can start working in your Dyspatch workspace on your behalf.


That last part is a major shift for Dyspatch teams. Our first MCP release was great if you were comfortable editing a JSON config. This one is for everyone else, especially folks who live outside of the command line (aka don’t touch code).


### **What "Agent Operators" actually means**


MCP (Model Context Protocol) is the open standard that lets AI clients like Claude connect to outside tools and data. Agent Operators is Dyspatch's hosted MCP server. Once it's connected, Claude can work on your templates in Dyspatch, with your permissions, in the same conversation where you're already planning the campaign.


It uses OAuth, the same kind of sign-in you already use for Dyspatch, and it's scoped to your account. Every action Claude takes runs through your existing permissions. It can't do anything you couldn't already do yourself.


Setup is one screen, and you don't need engineering


Add Agent Operators from Claude's connector directory, sign in to Dyspatch, and approve access on a single screen. That's it. Nothing to install, nothing to paste, no one to wait on. It works the same way in Claude on the web and in Claude Code.


### **What you can do with it**


This first release focuses on the template library, the part of Dyspatch most marketing teams touch every day. There's a lot you can do here. These are the workflows customers have been experimenting with first:


- **Audit your library by asking.** "List every published template, tell me what each one does, and flag anything that hasn't been updated in six months." What used to mean exports and a spreadsheet becomes a question with an answer. Ideal for pre-launch reviews and account clean-ups.


- **Unblock your team.** Ask which templates are waiting on your approval and sign off on recent changes, without bypassing your guardrails, QA, or testing. The approval queue stops being the thing that holds up a launch.


- **Build from what already works.** Duplicate a proven template, save it as a new draft, and adjust it. Spinning up a seasonal variant or a regional version takes a sentence, not an afternoon.


- **Keep your content blocks current.** Find a reusable block, create a new one, or update an existing one so your modular system stays in sync across campaigns.


- **Publish when it's ready.** When a template is good to go, Claude can publish it. Because that changes live content, it confirms with you before it happens, not after.


### **Built to be safe by default**


Marketing ops runs on trust, so Agent Operators is read-first and locked down by design. It only exposes an allowlist of template, block, and folder tools. Admin, billing, user management, and destructive account-level actions simply aren't reachable through it.


A few things worth knowing:


- Org admins can turn Agent Operators on or off for the whole organization.


- Every action is logged, so there's a clear record of what ran and when.


- Access tokens are short-lived and stored only as hashes.


### **Who it's for**


This is built first for the people who run email operations: the marketers and mops folks who own audits, approvals, and launch prep. Instead of clicking through tabs, you get that work done by asking.


Developers get value too. They can drive Dyspatch from the same AI client they already code in, with nothing to set up per machine. But you no longer have to be one to get the benefit.


### **The bigger shift**


AI is most useful when the line between thinking about the work and doing it disappears. Our first MCP release proved the workflow. Agent Operators removes the setup, so the people who actually run your email program can connect in a click and get the busywork off their plate.


Head to the


[docs](https://docs.dyspatch.io/integrations/mcp-server/) to connect, or


[get a demo](https://www.dyspatch.io/demo/) to see it on your own templates.


### **The Email Builder that Global Brands Love**


Learn more


```text


```
