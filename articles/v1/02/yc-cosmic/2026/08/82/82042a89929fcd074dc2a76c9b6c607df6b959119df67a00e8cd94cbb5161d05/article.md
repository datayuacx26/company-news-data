---
schema_version: "1.0.0"
document_id: "82042a89929fcd074dc2a76c9b6c607df6b959119df67a00e8cd94cbb5161d05"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/connect-claude-to-your-cms-mcp-server"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T22:07:49.775991+00:00"
fetched_at: "2026-08-10T22:07:52.299335+00:00"
content_hash: "sha256:4d91a1ca28f578bcad135d1c157e8d96b43e015171d7517810263b594e849cc3"
---

# Connect Claude to Your CMS: A 5-Minute Guide to the Cosmic MCP Server

If you already run Claude every day, the next obvious question is whether it can touch your content. Not summarize it. Not draft copy in a chat window you then paste somewhere. Actually read your content model, query it, and write back to it.


Cosmic answers that with an MCP server. This guide gets you connected in about five minutes, then covers the part most people skip: what Claude is allowed to do once it is connected.


---


## What MCP actually gives you


Model Context Protocol is a standard way for an AI client to discover and call tools. Instead of you describing your CMS to Claude in prose, Claude asks the server what tools exist and calls them directly.


The Cosmic MCP server exposes 18 bucket-scoped tools. They cover objects, object types, media, and bucket metadata. Bucket-scoped is the important word: the server operates against one Bucket, using the keys you give it, and it cannot reach across your other Buckets.


---


## The five-minute setup


### 1. Get your Bucket keys


In your Cosmic dashboard, open your Bucket, then **Settings > API Access** . You will see two keys: a read key and a write key. They are separate on purpose, and that separation is the single most useful control in this whole setup. More on that below.


### 2. Add the server to your MCP client config


For Claude Desktop, add the Cosmic server to your MCP configuration with your Bucket slug and key as environment values. Point your client at the Cosmic MCP server, restart the client, and the tools appear in the tool list.


### 3. Verify the connection


Ask Claude something read-only first:


> "List the object types in this Bucket and tell me how many objects are in each."


If you get back your real content model, you are connected. If you get nothing, the key or the Bucket slug is wrong, and the error message will say which.


### 4. Try a real task


Once reads work, the useful prompts look like this:


> "Find every published blog post missing an SEO description and list the slugs."


> "Create a draft post from this outline, set the author to Tony Spiro, and leave status as draft."


> "Add alt text to every image in the media library that does not have any."


That last one is the kind of chore that never gets done manually and takes an agent about a minute.


---


## The control that matters: read keys and write keys


Here is the part worth understanding before you hand an agent your production Bucket.


Cosmic issues separate read and write keys. Give an MCP client a read-only key and it gets the read tools only. Every write tool returns a clear blocked error. Nothing is guessed, nothing is inferred from a system prompt, and no amount of clever prompting talks its way past it.


You can verify this yourself in under a minute:


1. Configure the MCP server with only the read key
2. Ask Claude to create a new object
3. Read the blocked error it returns


That is a checkable guardrail, not a policy document. It is also the honest starting posture for anyone connecting an agent to real content: read-only first, then widen scope deliberately once you trust the workflow.


When you are ready for writes, the safe pattern is a write-enabled key pointed at a staging Bucket, with the production Bucket still read-only. Cosmic plans include multiple Buckets from Builder up, so a dedicated staging Bucket for agent work is a reasonable setup rather than an exotic one.


---


## Writing content through the API instead


If you would rather script the workflow than chat it, the TypeScript SDK covers the same ground:


```text
npm     install   @cosmicjs/sdk
```


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  !  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
writeKey  :   process  .  env  .  COSMIC_WRITE_KEY  !  ,
}  )  ;


// Read: find drafts with no SEO description
const     {   objects  :   posts   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'  ,   status  :     'draft'     }  )
.  props  (  'id,title,slug,metadata.seo_description'  )  ;


const   missing   =   posts  .  filter  (  (  p  )     =>     !  p  .  metadata  ?.  seo_description  )  ;


// Write: patch one of them
await   cosmic  .  objects  .  updateOne  (  missing  [  0  ]  .  id  ,     {
metadata  :     {
seo_description  :     'A short, specific summary under 155 characters.'  ,
}  ,
}  )  ;
```


Note that` writeKey` is a separate argument from` readKey` . Omit it and every write call fails, which is the same guardrail the MCP server relies on.


---


## What this is good for, and what it is not


**Good fits today:**


- Content audits: missing metadata, broken internal links, stale dates, orphaned objects
- Bulk mechanical edits: alt text, SEO descriptions, tag normalization, category cleanup
- Draft generation against your real content model, so the shape is correct on the first pass
- Answering questions about your own content library without opening the dashboard


**Not a good fit yet:**


- Unsupervised publishing to production. Keep a human on the publish step.
- Anything where a wrong write is expensive and hard to reverse.


The practical rule: let the agent do the reading and the drafting, and keep the irreversible actions behind a person.


---


## Frequently asked questions


**Do I need a paid plan to use the MCP server?**
You can start on the Free plan, which includes 1 Bucket, 2 team members, and 1,000 Objects. If you want a separate staging Bucket for agent writes, that starts at the Builder plan ($49/month, 2 Buckets, 3 team members, 5,000 Objects).


**Does Cosmic offer a GraphQL endpoint for this?**
No. Cosmic offers a REST API and the JavaScript/TypeScript SDK. The MCP server sits on top of the same REST API.


**Can I limit which object types an agent can touch?**
The key-level control is read versus write. For tighter scoping, point the agent at a Bucket that only contains the content you want it to work on.


**Does this work with clients other than Claude?**
MCP is a client-agnostic standard, so any MCP-compatible client can connect to the same server with the same 18 tools.


---


## Try it on a real Bucket


The fastest way to understand what an agent can do with a proper content model behind it is to connect one and ask it a question about your own content.


- [Create a free Cosmic account](https://app.cosmicjs.com/signup?utm_source=blog&utm_medium=organic&utm_campaign=mcp-server&utm_content=connect-claude-to-your-cms-mcp-server) . No credit card required.
- Read the[MCP server docs](https://www.cosmicjs.com/docs/mcp-server) for the exact client configuration
- Want a walkthrough of the agent workflows other teams are running?[Book 30 minutes with Tony](https://calendly.com/tonyspiro/cosmic-intro?utm_source=blog&utm_medium=organic&utm_campaign=mcp-server&utm_content=connect-claude-to-your-cms-mcp-server)


Cosmic is a YC W19 company building the content layer for teams that want their AI tools to work against real, structured content instead of copy pasted into a chat box.


## Learn how to build this in Cosmic


The[Learn Cosmic](https://www.cosmicjs.com/learn) hub has step-by-step lessons on building agentic workflows, connecting AI tools to your content layer, and shipping sites with Next.js, Astro, and more.


- [Build an AI Agent Team That Ships Content](https://www.cosmicjs.com/learn/build-ai-agent-team-that-ships-content)
- [Connect Cosmic to Cursor and Claude Code with MCP](https://www.cosmicjs.com/learn/connect-cosmic-to-cursor-claude-code-with-mcp)
- [Publish Content from Slack with an AI Agent](https://www.cosmicjs.com/learn/publish-content-from-slack-with-an-ai-agent)


[Browse all lessons →](https://www.cosmicjs.com/learn)
