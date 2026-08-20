---
schema_version: "1.0.0"
document_id: "1b2908b97e07af90c365a5672b77d38593e0f644825bc9aa9d1592297f3536d6"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/agent-signup-ai-agents-can-now-provision-cosmic-projects-for-anyone"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:b6bd199a6424e56df703234dd918e49d3f34fefe99e0205a076c5e08cd4f1b9b"
---

# Agent Signup: AI Agents Can Now Provision Cosmic Projects for Anyone

AI agents can now create a Cosmic project on behalf of a human in a single API call. The agent calls one endpoint, the human gets an email, and a fresh project + bucket is live in seconds, no Cosmic account required up front. Once the human pastes the verification code back to the agent (or clicks the claim link), the bucket graduates to full free-tier limits and the agent keeps building.


This is the onboarding path purpose-built for the new generation of coding assistants and app builders: Cursor, Claude Code, Codex, Bolt, Lovable, v0, and any custom agent you ship.


## TL;DR


Follow the steps in the[Agent Skills area](https://www.cosmicjs.com/docs/agent-skills) in the docs.
Follow the steps to add the[Cosmic MCP Server](https://www.cosmicjs.com/docs/mcp-server#agent-scope) .


## What's New


- **One-call project creation.** provisions a new project and bucket tied to a human's email. The response includes raw bucket keys ready to drop into the Cosmic SDK, an for managing the signup lifecycle, and a the agent can hand to the human as an alternative to the OTP.
- **Three auth states with sensible guardrails.** Projects start in mode (50 objects, 5 MB media, no AI credits), graduate to once the human confirms via OTP, and move to if the same email signs up at . Auto-absorb is built in: the agent's work isn't lost when the human takes ownership later.
- **Email-based claim, no shared secrets.** A 6-digit code is emailed straight to the human. The agent never sees it. The human pastes it back, or clicks a claim link in the Cosmic dashboard. Either path lifts restrictions on the agent's bucket instantly.
- **Built into the CLI.** , , and handle the full flow from the terminal. Keys are persisted to so verify and status work without copy-pasting anything.
- **Built into the hosted MCP.** A new scope at exposes the same flow as three MCP tools (, , ), so any MCP-compatible AI assistant can onboard a user without leaving the conversation.
- **Conservative defaults, fair recovery.** Unclaimed projects auto-delete after 14 days if not claimed. AI generation is disabled while unclaimed to keep credits where they belong. Everything lifts the moment the human verifies.


## Why This Matters


Until now, "use Cosmic with AI" meant: a human signs up, copies three keys out of the dashboard, pastes them into an file, then asks their AI assistant to build something. Plenty of users never made it past step one. Plenty of AI assistants stalled out trying to coach the user through it.


Agent Signup flips the model. The AI assistant is the one with the API in hand, so let the AI assistant do the signup. The human stays in the conversation: they answer "what's your email?", confirm the 6-digit code, and the agent keeps shipping. No tab-switching, no manual key copying, no "I'll wait while you create an account."


It's the same pattern that AgentMail uses for inboxes, that Twilio uses for phone numbers, and that Stripe uses for Connect. The user doesn't sign up for the underlying service first; the agent provisions on their behalf, and ownership transfers when the human wants it to.


For AI platforms building on Cosmic, this means three things: shorter time-to-first-value, fewer dropped users in the funnel, and a clear path from "I'm trying this thing" to "this is my Cosmic account."


## How It Works


### Sign up


```text


```


Returns:


```text


```


The bucket keys are raw and ready to use with immediately. The is what the agent uses to call and .


### Verify


The human reads the OTP from their email and hands it to the agent:


```text


```


Restricted-mode limits lift, AI generation turns on, the bucket is on standard free-tier limits.


### From the CLI


```text


```


### From the hosted MCP


Point your assistant at and the same three tools are available, no install required:


```text


```


Sign-up needs no auth. Verify and status take (the agent key from sign-up).


## Examples


- **Recipe blog from scratch.** "Build me a recipe blog with Cosmic." The agent asks for an email, calls , gets bucket keys back, scaffolds the Next.js project, and creates the first three Object Types and a sample post. The user pastes the OTP, AI generation flips on, the agent generates hero images and writes copy. End-to-end in under five minutes.
- **App builder bootstrap.** A v0-style builder doesn't ask the user to "go sign up at cosmicjs.com first." It calls the moment the user picks a CMS, embeds the keys in the generated repo, and ships a working preview before the user has thought about accounts.
- **Cursor "use Cosmic in this project" flow.** The user runs from the integrated terminal, pastes the OTP from their inbox, and Cursor's chat now has a live Cosmic bucket with credentials in . The same skill ( in ) covers both onboarding and ongoing usage.
- **Internal AI tools.** A company's internal coding assistant provisions a sandbox Cosmic project per employee on first use, using their work email. Each project is isolated, claimable later, and auto-deletes if abandoned. Zero IT involvement.


## Designed With Agent Safety in Mind


A few things worth calling out:


- **Email validation, not phone or payment.** Sign-up is gated on a real email the human controls. No card on file, no PII collection up front.
- **The agent never sees the OTP.** It only flows from Cosmic to the human, then human to agent. That keeps the human firmly in the loop on whether the project actually belongs to them.
- **Idempotent by + .** Re-running sign-up refreshes the OTP and returns the existing project instead of creating duplicates. Safe to retry.
- **Existing accounts always win.** If the email already belongs to a Cosmic user, sign-up returns a clear response with a login URL. No silent account hijacking, no duplicate accounts.
- **Hard TTL on unclaimed projects.** 14 days, then the project is fully deleted. Keeps the namespace clean and removes any "what if I left this lying around?" anxiety.
- **Rate-limited per IP and per .** Production carries a strict per-IP cap on plus a daily ceiling per agent platform, with looser limits in development so AI partners can test freely.


## Try It


Agent Signup is live today across the REST API, the CLI, and the hosted MCP.


- Read the full guide:[Agent Skills documentation](https://www.cosmicjs.com/docs/agent-skills#agent-signup) .
- REST reference:[Agents API](https://www.cosmicjs.com/docs/api/agents) .
- MCP scope:[Agent scope](https://www.cosmicjs.com/docs/mcp-server#agent-scope) on the hosted MCP.
- CLI install: (or ).


Building an AI platform that creates content infrastructure on the fly? We'd like to talk.[Get in touch](https://www.cosmicjs.com/contact) about the Cosmic AI Partner program: deeper integration, co-marketing, and dedicated support for partners shipping Cosmic-backed agent experiences.


Ready to get started with Cosmic now?[Create your free account](https://app.cosmicjs.com/signup) , connect your AI assistant, and ship content faster than you thought possible.
