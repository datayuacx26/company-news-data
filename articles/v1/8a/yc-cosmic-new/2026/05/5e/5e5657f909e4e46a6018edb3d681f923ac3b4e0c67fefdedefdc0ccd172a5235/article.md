---
schema_version: "1.0.0"
document_id: "5e5657f909e4e46a6018edb3d681f923ac3b4e0c67fefdedefdc0ccd172a5235"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/how-to-make-your-product-agent-callable"
published_at: "2026-05-27T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:c01ccc42b5d33d503f1411bacf20c53ea5c7aceee242dd0bace6866fea7e59e9"
---

# How to Make Your Product Agent-Callable: What We Learned Building Agent Signup

Nobody handed us a playbook for this. We built one by accident.


When we started thinking about how AI assistants would interact with Cosmic, we weren't thinking about distribution. We were thinking about developer experience: how do you get a coding assistant up and running with a live CMS bucket without making the human stop everything and copy-paste keys from a dashboard?


The answer we landed on turned out to be something bigger than a UX improvement. It's a new acquisition channel. And almost nobody is building for it yet.


## The Shift: AI Assistants Are the New Discovery Layer


For the past decade, distribution meant SEO, app stores, and word of mouth. A potential user would Google their problem, land on your site, read your docs, and decide whether to sign up. The human was always the one initiating.


That model is breaking.


When a developer opens Cursor or Claude Code and says "build me a blog with a headless CMS," the AI assistant makes recommendations. It picks tools. It writes the integration code. And then, increasingly, it tries to provision the infrastructure on the user's behalf.


If your product isn't callable by an AI agent, you're not in that consideration set. It doesn't matter how good your SEO is. The agent never sees your pricing page.


This is the shift: the discovery layer is moving from search engines to AI assistants, and the API surface your product exposes to those assistants is becoming a distribution asset.


## What "Agent-Callable" Actually Means


There's no single standard. But from what we've built and observed, an agent-callable product has four layers:


**1. A clean REST API with a well-documented agent-specific endpoint**


AI coding assistants know how to call REST APIs. They will read your docs, find the right endpoint, and call it. If you don't have an obvious starting point for an agent to provision or interact with your product without human intervention, you have a gap. The key design question: what's the one call an agent needs to make to get started with your product on behalf of a user?


**2. An MCP server**


The Model Context Protocol (MCP) is becoming the standard way for AI assistants like Claude to access external tools. An MCP server exposes your product's capabilities as callable tools inside the assistant's context window. If you have a meaningful API, you can wrap it in an MCP server. We ship ours at and also offer a self-hosted version via . Users can point any MCP-compatible assistant at it and get full access to content management tools without leaving the conversation.


**3. llms.txt and llms-full.txt**


This is the low-effort, high-leverage move most teams haven't made yet. is a plain-text file (similar in spirit to ) that gives AI assistants a structured, markdown-friendly index of your documentation. When an LLM is trying to understand your product, it will fetch this file. We put ours at[cosmicjs.com/docs/llms.txt](https://cosmicjs.com/docs/llms.txt) . The first line of that file is a direct instruction to agents: start here, and here's the one-curl signup flow. That's intentional. You're writing documentation for a non-human reader, and you should treat it that way.


**4. robots.txt allowlist for AI crawlers**


If you're not explicitly allowing GPTBot, ClaudeBot, PerplexityBot, and similar crawlers in your , you may be blocking the tools that would otherwise recommend you. Check your config. Most teams have rules that are broader than they intended.


None of these are exotic. They're all table stakes if you want to be part of the agentic web.


## What We Built: Agent Signup


The concrete problem we were solving: a coding assistant trying to use Cosmic on a user's behalf had to stop the flow, tell the user to go create an account, copy three API keys out of the dashboard, and paste them back into the conversation. A lot of users never made it through that. The assistant would stall, the user would lose momentum, and we'd lose the signup.


So we built[Agent Signup](https://cosmicjs.com/changelog/agent-signup-ai-agents-can-now-provision-cosmic-projects-for-anyone) : a single API endpoint that lets an AI agent provision a Cosmic project on behalf of any user, before that user even has an account.


The flow looks like this:


1. The agent calls with the user's email.
2. Cosmic creates a project and bucket, returns raw API keys and an the agent can use to manage the lifecycle.
3. The user gets an email with a 6-digit OTP. They paste it back to the agent (or click a claim link).
4. The bucket graduates to full free-tier access. The agent keeps building.


The human never leaves the conversation. The agent never sees the OTP. The project auto-deletes after 14 days if unclaimed, keeping the system clean.


We also exposed the same flow as three MCP tools (, , ) on the hosted MCP server, so any MCP-compatible assistant can trigger it without any installation. And we made the Agent Skills docs available in markdown format so coding assistants reading our docs can find and follow the flow without human interpretation.


The design was influenced by patterns we respect: AgentMail does something similar for email inboxes, Twilio for phone numbers, Stripe Connect for merchant accounts. The agent provisions on the user's behalf; ownership transfers later. It's a pattern that will become common.


## The Consideration Set Problem


Building the infrastructure is necessary but not sufficient. You also have to get into the consideration set.


When a user tells an AI assistant "I need a headless CMS for my Next.js app," the assistant makes a recommendation. That recommendation is based on what the model was trained on: docs, blog posts, GitHub repos, forum discussions, Stack Overflow answers. It's not real-time search. It's accumulated pattern matching.


That means the content you've published over the last few years matters more than you think. If your documentation is clear, if developers write about you, if your GitHub examples are clean and findable, you're more likely to get recommended. Traditional content marketing is, accidentally, agent optimization.


But there are things you can do specifically for the agentic context:


- **Write docs for agents, not just humans.** Use to give agents a fast path to the right information. Write your agent-specific docs in plain markdown with direct action steps.
- **Make your first API call obvious.** The simpler the "getting started" story, the more likely an agent is to successfully complete it and keep going with your product.
- **Get into the training data.** Published tutorials, GitHub repos, real-world usage patterns. This is a long game, but it's the same game as SEO.
- **Ship an MCP server.** MCP is still early, but adoption is accelerating. Being listed in MCP directories and having a working server means you show up in that conversation.


## What We Still Don't Know


I want to be honest here, because this is genuinely new territory.


We don't know what percentage of our new signups are coming from agent-initiated flows versus traditional search. Our attribution model wasn't built for this. When a Cursor user has a Cosmic project provisioned on their behalf mid-conversation, does that count as organic? Referral? Something else?


We don't know which AI assistants are most likely to recommend Cosmic, or how much the quality of our actually influences recommendations versus raw training data volume.


We don't know how quickly MCP will become a universal standard versus one of several competing protocols. We're betting on it, but it's a bet.


And we definitely don't know how other products in our category are thinking about this. Most aren't talking about it publicly. That's either because they haven't figured it out yet, or because they have and they're not sharing. Either way, there's a real first-mover window right now for teams willing to invest in the infrastructure.


If you're building something in this space, I'd genuinely like to compare notes. This is a problem worth talking through openly.


---


**If you want to see what agent-callable infrastructure looks like in practice** , the Cosmic[Agent Skills docs](https://www.cosmicjs.com/docs/agent-skills) walk through the full signup flow. The[MCP server](https://www.cosmicjs.com/docs/mcp-server) is live and usable today. And if you're building an AI platform that needs content infrastructure,[let's talk](https://calendly.com/tonyspiro/cosmic-intro) .
