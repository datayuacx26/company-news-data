---
schema_version: "1.0.0"
document_id: "13240900f6d953cd834b6044960c854c8c2202ba0b8400f9e1cde9407d49140c"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/connect-claude-code-to-cms-with-mcp"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T18:03:14.531466+00:00"
fetched_at: "2026-07-31T18:03:17.660255+00:00"
content_hash: "sha256:f6623804334eb6be4659a380f4dc520ae78e85bdf2eba351c2b2d168ba8746af"
---

# How to Connect Claude Code to Your CMS with MCP

Claude Code is fluent in your repository and blind to your content. It can refactor the component that renders your blog, but ask it to publish the post that component displays and it has nowhere to look. The Model Context Protocol (MCP) closes that gap. It gives Claude Code a set of tools it can call against your CMS directly, so reading and writing content happens in the same conversation as the code.


This guide connects Claude Code to a Cosmic bucket. With the hosted endpoint it takes about five minutes and requires no install.


## What MCP actually gives Claude Code


MCP is an open protocol for exposing tools to AI assistants. The Cosmic MCP server implements it and exposes **18 tools** across four areas:


- **Objects** (5 tools): list, get, create, update, and delete content
- **Media** (4 tools): list, get, upload, and delete files
- **Object Types** (5 tools): list, get, create, update, and delete content models
- **AI Generation** (4 tools): generate text, images, video, and audio into your bucket


Once connected, "publish the MCP draft and generate a hero image for it" resolves to real tool calls against your bucket. No browser tab, no copy-paste, no manual export.


There are two ways to connect:


- **Hosted MCP (recommended):** point your client at and authenticate with your bucket keys. Nothing to install.
- **Self-hosted (stdio):** run the npm package locally via . Useful for offline work, or when you want the MCP process running inside your own dev environment.


## Before you start


You need three things:


1. **A Cosmic bucket.** The Free plan is $0/month and includes 1 Bucket, 2 team members, and 1,000 Objects, which is more than enough to follow along.[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=connect-claude-code-to-cms-with-mcp&utm_content=prereq-signup) .
2. **Your bucket slug, read key, and write key.** Step 1 below covers where to find them.
3. **Claude Code installed.**


The hosted path needs no local runtime at all. Node is only required if you choose the self-hosted stdio option, since that runs through .


## Step 1: Get your bucket credentials


1. Log in to the[Cosmic dashboard](https://app.cosmicjs.com/)
2. Navigate to your bucket
3. Go to **Settings** → **API Access**
4. Copy your **bucket slug** , **read key** , and **write key**


A recommendation before you paste anything: start with the read key only. Cosmic issues separate read and write keys per bucket, so you can give Claude Code full visibility into your content while making it structurally incapable of changing it. Add the write key once you trust the setup. The read-only vs full access section below covers exactly what changes.


## Step 2: Connect with hosted MCP (recommended)


Claude Code picks up project-scoped MCP servers from a file at the root of your repository. Create it with:


```text


```


Replace , , and with the values from Step 1. The endpoint supports the streamable-HTTP MCP transport.


### How the authorization header works


Cosmic packs both keys into a single bearer token, separated by a colon. The write key is the part after the colon:


```text


```


Omit the colon and the write key for read-only access. If your client cannot send a colon-packed token, you can pass the write key out-of-band using the header instead.


One housekeeping note: now contains live credentials, so add it to before your next commit.


The same block works for Claude Desktop and Cursor. The[MCP server docs](https://www.cosmicjs.com/docs/mcp-server) list the exact config file paths for each client, including on macOS and for Cursor.


## Step 2, alternative: self-hosted over stdio


If you would rather run the server yourself, the package ships a stdio binary. Point Claude Code at :


```text


```


The stdio binary reads credentials from environment variables:


- (required): your Cosmic bucket slug
- (required): bucket read key for read operations
- (optional): bucket write key for write operations


Leave out entirely for a read-only server. You can also install it globally with instead of resolving it through each time.


## Step 3: Verify the connection


Restart Claude Code and run:


```text


```


You should see listed with its tools. Then confirm it can actually reach your bucket by asking for something only your bucket knows:


```text


```


Claude Code should call and return your real content models. If it returns your object types, the connection is live and correctly authenticated.


## The 18 tools, and when each one fires


### Objects


- : list or search objects, filtered by type, status, and locale, with pagination
- : fetch a single object by ID or slug, with optional metafield, depth, and locale params
- : create a new object with title, slug, status, and metafields (write key required)
- : update an existing object's title, slug, status, or metafield values (write key required)
- : permanently delete an object by ID (write key required)


### Media


- : list media files, optionally scoped to a folder
- : fetch metadata and the imgix URL for a single file
- : upload from a URL or base64 payload into the media library (write key required)
- : delete a media file (write key required)


### Object Types


- : list every object type in the bucket
- : fetch the full schema for one object type, including metafields, options, and helper text
- : create a new object type with a metafield schema (write key required)
- : update a type's schema or metafield definitions (write key required)
- : delete an object type and all its objects (write key required)


### AI Generation


- : generate text with optional context pulled from existing objects in your bucket
- : generate an image and store it in the media library (write key required)
- : generate video with Google Veo and store it in the media library (write key required)
- : generate narration via OpenAI TTS, 13 voices available, stored in the media library (write key required)


The two tools worth calling out for agent work are and . An agent that reads your schema before writing produces valid metafields on the first attempt instead of guessing key names and failing.


## Read-only vs full access


This is the part to get right before you point an agent at a production bucket.


With a read-only token, every write tool is blocked with a clear error message and read tools work as normal. Specifically, the blocked set is every , , and tool, plus all four AI generation tools, since each of those writes generated assets into your media library.


So a read-only setup still lets Claude Code explore your content models, read every object, and reason about your content while it writes application code. It just cannot mutate anything. That is a good default for a first session against real data.


## What this looks like in practice


With the server connected, these are all single prompts:


```text


```


```text


```


```text


```


```text


```


```text


```


```text


```


The schema management case is the one developers tend to underestimate. Modeling content is usually a dashboard task. Through MCP it becomes something you can do from the same prompt where you are scaffolding the components that will consume it.


## Learn how to build this in Cosmic


The[Learn Cosmic](https://www.cosmicjs.com/learn) hub has step-by-step lessons on building agentic workflows, connecting AI tools to your content layer, and shipping sites with Next.js, Astro, and more.


- [Build an AI Agent Team That Ships Content](https://www.cosmicjs.com/learn/build-ai-agent-team-that-ships-content)
- [Connect Cosmic to Cursor and Claude Code with MCP](https://www.cosmicjs.com/learn/connect-cosmic-to-cursor-claude-code-with-mcp)
- [Publish Content from Slack with an AI Agent](https://www.cosmicjs.com/learn/publish-content-from-slack-with-an-ai-agent)


[Browse all lessons →](https://www.cosmicjs.com/learn)


## Agent scope: when the human has no Cosmic account yet


The hosted endpoint exposes a second, smaller scope at for the agent signup flow. It lets an AI agent provision a brand new Cosmic project and bucket on behalf of someone who does not have an account, without leaving the MCP transport. It exposes three tools:


- (no auth): creates an unclaimed project and bucket tied to a . Returns the , , , and a . Cosmic emails the human a 6-digit OTP.
- (requires ): submits the OTP, lifts restricted-mode limits, and enables AI generation.
- (requires ): checks claim status, remaining limits, and recovers the bucket keys.


New buckets start in restricted mode: no AI credits, a maximum of 50 objects, and a 5 MB media cap. Unclaimed projects are hard-deleted after 14 days.


The bucket-scoped tools listed earlier are not available on the agent endpoint, and the agent tools are not available on the bucket endpoint. A single conversation often uses both: the agent signs the human up, captures the returned bucket keys, then switches to the bucket scope to start creating content.


## MCP server vs Agent Skills


Cosmic offers two things that sound similar and do different jobs:


- **MCP server** is for direct content management. It answers "list my blog posts." The AI calls tools that operate on your bucket.
- **Agent Skills** is for code generation guidance. It answers "build a blog with Cosmic." The AI writes application code using the SDK.


Use both. Agent Skills helps Claude Code write code like this:


```text


```


The MCP server then lets the same session manage the content that code renders. One tool writes the app, the other operates the data behind it.


## Guardrails for production buckets


Four habits worth adopting:


1. **Read key first.** Connect with a read-only token for your first few sessions. Add the write key when you have seen what the agent actually does.
2. **Experiment in a separate bucket.** Bucket allowances scale with your plan: Free includes 1, Builder ($49/month) includes 2, Team ($299/month) includes 3, and Business ($499/month) includes 5. Point destructive experiments at a bucket you do not mind losing.
3. **Treat the delete tools as manual-approval only.** , , and especially are permanent, and deleting an object type takes all of its objects with it. Never let an agent call these speculatively.
4. **Mind your seats.** Plans include a set number of team members (Free 2, Builder 3, Team 5, Business 10) and additional users are $29/user/month, so decide who needs dashboard access rather than adding everyone by default.


## Troubleshooting


**The server does not appear in .** Confirm is valid JSON at the repository root and restart Claude Code. Some Claude Code versions want the transport named explicitly, so if a hosted config still will not connect, try adding alongside .


**cannot find the package.** The package name is , scoped, including the . Verify Node is installed and on your PATH.


**Write tools return an error but reads work.** Your bearer token is missing the write key. Check that the header is with a colon and no spaces, or send the write key via .


**404 from the hosted endpoint.** The bucket slug in the URL is wrong. Copy it again from **Settings** → **API Access** , since the slug is not always identical to your project's display name.


**Tools connect but return nothing.** Confirm you are pointed at the bucket you think you are. Ask Claude Code to run and compare the result against the dashboard.


## Next steps


Start with the hosted endpoint and a read-only token. Ask Claude Code to list your object types, then ask it to summarize the content in your bucket. Once that works, add the write key and let it draft something. The full tool reference and per-client config paths live in the[MCP server documentation](https://www.cosmicjs.com/docs/mcp-server) .


**Keep reading:**


- [MCP Server Complete Guide for Developers (2026)](https://www.cosmicjs.com/blog/mcp-server-complete-guide)
- [Claude Code vs GitHub Copilot vs Cursor: Which AI Coding Tool Wins in 2026?](https://www.cosmicjs.com/blog/claude-code-vs-github-copilot-vs-cursor-which-ai-coding-agent-should-you-use-2026)
- [Your AI Stack Shouldn't Break Every Time a New Model Drops](https://www.cosmicjs.com/blog/model-agnostic-cms-ai-provider-lock-in)


### Build AI-powered content workflows with Cosmic


Your content layer for AI agents. Structured, versioned, queryable, and analytics-ready out of the box.


[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-signup-cta)[Book a demo](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-demo)[Log in](https://app.cosmicjs.com/login?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-login)
