---
schema_version: "1.0.0"
document_id: "c41c96fbc747462c44fca850c3b57ced14052d95a8b3aee431e6ec63a8490735"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/ai-agent-write-access-cms-controls"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T18:33:20.401646+00:00"
fetched_at: "2026-08-13T18:33:23.174733+00:00"
content_hash: "sha256:570e1bd554908ecb8c9b72af82dd5d8f4a3eaa46c45b4b78363946c1dbee2129"
---

# AI Agent Write Access: The 4 Real Controls You Get With Your CMS

Every team evaluating AI tooling for their content stack arrives at the same question, usually about ten minutes into the demo: what stops the thing from deleting everything?


It is a fair question and most vendors answer it with adjectives. Enterprise-grade. Secure by design. Trusted. None of that is checkable. You cannot hand an adjective to a security reviewer.


So here is a direct answer for Cosmic, with four controls you can verify yourself in about a minute each, and an honest list of the two controls we do not ship yet.


## First, what you are actually connecting


Cosmic's MCP server is a tool surface for your own AI client. You connect it to something like Claude Desktop or Cursor, and that client gains a set of tools for working with your content. It works with any MCP client, and setup is documented for Claude Desktop and Cursor.


Two things follow from that, and both matter for the safety conversation.


The server responds to tool calls. It does not run on a schedule, it does not wake up on its own, and it does not decide to do anything without a request from the client in front of you. Every action in this post starts with something you or your client initiated.


This is also separate from Cosmic Agents, which is a different product for teams that want autonomous content workflows. If you are looking for that,[start on the AI page](https://www.cosmicjs.com/ai?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=agent-write-safety&utm_content=intro-ai-page) . The rest of this post is about the tool surface.


One related topic this post leaves alone: knowing after the fact which content a model produced. Provenance is a separate question from permissions, and the two are worth reading together. We covered the provenance side in[how Claude marks AI-generated content, and what it means for your CMS](https://www.cosmicjs.com/blog/claude-ai-content-watermarking-provenance-cms?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=agent-write-safety&utm_content=intro-provenance) .


The models and tools you just read about keep changing every few months. Cosmic gives you a model-agnostic, API-first content backend that stays stable underneath them: structured objects, a TypeScript SDK, scoped API keys, and built-in analytics.[See how it works with AI agents](https://www.cosmicjs.com/ai?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=inline-ai-page) or[start free, no credit card required.](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=inline-top-cta)


## Control 1: Read and write are separate keys


This is the spine of the whole thing.


Cosmic issues two different API keys per bucket. A read key and a write key. Each one is a separate string, and you copy them from separate places in your dashboard.


Give your AI client a read-only key and it gets the read tools only. Listing objects, fetching an object, listing media, listing object types. That is the entire surface. Ask it to create a post and the write tool returns a clear blocked error. Ask it to delete an object type and you get the same thing. The refusal happens at the API boundary, so it does not depend on the model behaving well, on your system prompt being well written, or on the client honoring any instruction you gave it.


That distinction matters more than it sounds. Prompt-level guardrails fail in the ways language models fail: a clever input, a confused context window, a tool description the model misreads. A credential that was never granted write access fails in none of those ways, because there is nothing to talk it out of.


The same separation exists in the SDK, which is a useful way to think about it:


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;


// Read-only client. No writeKey, so there is no write path at all.
const   cosmic   =     createBucketClient  (  {
bucketSlug  :     'your-bucket-slug'  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
}  )  ;


const     {   objects   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  'title,slug,metadata'  )  ;
```


Adding write capability is an explicit act. You pass a second credential:


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;


const   cosmic   =     createBucketClient  (  {
bucketSlug  :     'your-bucket-slug'  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY  !  ,
}  )  ;


await   cosmic  .  objects  .  insertOne  (  {
type  :     'blog-posts'  ,
title  :     'Draft from an AI client'  ,
status  :     'draft'  ,
metadata  :     {   markdown_content  :     '...'     }  ,
}  )  ;
```


**Verify it in under a minute:** connect your client with the read key only, then ask it to create a test object. You should get a blocked error rather than a new object. If you want to be thorough, check the bucket afterward and confirm nothing was created.


## Control 2: The tool surface is bounded and countable


Cosmic's MCP server exposes exactly 18 bucket-scoped tools, in four groups:


- **Objects:** list, get, create, update, delete
- **Media:** list, get, upload, delete
- **Object types:** list, get, create, update, delete
- **AI generation:** text, image, video, audio


That is the complete list. There is no shell tool, no arbitrary query execution, no escape hatch that lets a model do something outside those 18 operations. When you review the risk of connecting this to a client, you are reviewing a finite list you can read in thirty seconds and hand to a colleague.


Compare that to the general anxiety around agent tooling, where the honest answer to "what can it do?" is often "we are not sure, it depends what the model tries." A bounded surface turns that into a review you can actually complete.


**Verify it in under a minute:** ask your client to list the Cosmic tools it has available. You should see 18 with a read-write key, and only the read tools with a read-only key.


## Control 3: The connection is scoped to one bucket


The hosted endpoint is per-bucket. It looks like this:


```text
https  :  /  /  mcp  .  cosmicjs  .  com  /  v1  /  buckets  /  {  bucket  -  slug  }
```


The credentials are per-bucket too. A client connected to your staging bucket has no path to your production bucket. It cannot list your other buckets, discover them, or reach them by guessing a slug, because the key it holds is not valid there.


This makes the standard safe pattern easy to set up. Point the AI client at a sandbox or staging bucket with a read-write key so it can create, update, and iterate freely. Point it at production with a read-only key so it can research, summarize, and audit without touching anything. Two configurations, one credential difference, and the blast radius of the more permissive one is a bucket you do not mind breaking.


**Verify it in under a minute:** connect with a staging key, then ask the client to list objects from your production bucket slug. It should fail rather than return content.


## Control 4: You choose where the process runs


There are two connection modes.


Hosted, at the URL above, with nothing to install. This is the fastest path and the one most teams use.


Self-hosted, by running` npx @cosmicjs/mcp` yourself. The process runs on your machine or your infrastructure, and your keys live in your environment.


Self-hosting matters for teams whose security review asks where credentials are held and which network hops are involved. Having the option means that question has an answer other than "trust us."


## What we do not have


I cut two things from the outline of this post because they described controls Cosmic does not actually ship. Naming them is more useful than quietly leaving them out.


**There is no per-tool permission layer.** The granularity today is read-only or read-write. If you want a client that can create objects but never delete them, or one that can edit content but never touch object types, that boundary does not exist at the key level. You can approximate it with prompt instructions. Treat that approximation as a preference the model is free to override.


**Draft status is a workflow convention.** A client holding a write key can set status to` published` . Nothing in the protocol requires content to land as a draft first or routes it through an approval step. Instructing your client to always write drafts is worth doing, and it holds only as long as the client follows the instruction. If you need a hard gate between an AI client and your live site, put it in your deployment pipeline where you can actually enforce it.


Both of these are real gaps. Four controls that hold up under a security review beat six where two fall apart the moment someone tests them.


## The practical setup


For most teams, this is the whole configuration decision:


Use case Bucket Key


Research, audits, content Q&A Production Read-only


Drafting and iteration Staging or sandbox Read-write


Migration or bulk cleanup A cloned bucket Read-write


Start with read-only against production. It is genuinely useful on day one: asking a client to find every post missing a meta description, or to summarize what you published last quarter, requires no write access at all. Add write access deliberately, in a bucket where a mistake costs you nothing.


**Keep reading:**


- [Cosmic for AI teams: the content layer your agents read and write](https://www.cosmicjs.com/ai?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=keep-reading&utm_content=keep-reading-ai)
- [Claude Opus 5 is available in Cosmic](https://www.cosmicjs.com/blog/claude-opus-5-available-in-cosmic)
- [MCP Server Complete Guide for Developers (2026)](https://www.cosmicjs.com/blog/mcp-server-complete-guide)
- [Claude Code vs GitHub Copilot vs Cursor: Which AI Coding Tool Wins in 2026?](https://www.cosmicjs.com/blog/claude-code-vs-github-copilot-vs-cursor-which-ai-coding-agent-should-you-use-2026)
- [Your AI Stack Shouldn't Break Every Time a New Model Drops](https://www.cosmicjs.com/blog/model-agnostic-cms-ai-provider-lock-in)


The full setup instructions, including the exact client configuration for Claude Desktop and Cursor, are in the[MCP server docs](https://www.cosmicjs.com/docs/mcp-server?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=agent-write-safety&utm_content=body-mcp-docs) .


### Build AI-powered content workflows with Cosmic


Your content layer for AI agents. Structured, versioned, queryable, and analytics-ready out of the box.


[See how Cosmic works with AI agents](https://www.cosmicjs.com/ai?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-ai-page)[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-signup-cta)[Book a demo](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-demo)
