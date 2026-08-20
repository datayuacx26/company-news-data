---
schema_version: "1.0.0"
document_id: "f5bb3d8de7e60db8143b58f951f8f2eda368eb4a667f7c32bb39b713a998baac"
company_key: "yc-archil"
company: "Archil"
source_id: "yc-archil-news-import-fbbb60f1501e"
canonical_url: "https://archil.com/post/context-that-agents-understand"
published_at: null
first_seen_at: "2026-08-09T19:14:48.524951+00:00"
fetched_at: "2026-08-09T19:14:50.194785+00:00"
content_hash: "sha256:fff942327758f04d0d578c33e28463268c6d41d32d0b4aa348d5bd20f3b64e32"
---

# Context that agents understand

Like software before them, AI agents are now eating the world.


Organizations everywhere are rapidly shifting how they work with and consume data from internal portals to agents which can act on behalf of users, but these agents are only as good as the context that they can access.


Over the past 6 months, teams at the forefront of building agents (like[LangChain](https://www.langchain.com/blog/how-agents-can-use-filesystems-for-context-engineering) ,[Ramp](https://builders.ramp.com/post/meet-ramp-research) , and[Vercel](https://vercel.com/blog/how-to-build-agents-with-filesystems-and-bash) ) have found that agents perform better when they have fewer tools to use when interacting with their context. To solve this problem, these teams have started giving their agents context in the form of a file system.


File systems are ideal because they allow you to represent arbitrarily large amounts of context, while preserving progressive discovery for the LLM, with a small, universal set of tools to interact with them -- bash, python, and javascript.


Up until now, though, every team building agents has needed to build a bespoke file system solution and integration into their agent harnesses.


This has led to an explosion of brittle and poorly performing ways to build file systems: storing everything in a single SQLite file, putting links to S3 objects in a Postgres database, or synthesizing files on-demand.


Today, we're making it simple to use high-performance, enterprise file storage, like Archil, as the backing file system for any agent being built -- with just a few lines of code.


## Archil agent tools


We're calling this new suite of integrations, the "Archil agent tools", and it works with any agent framework that you already use -- including the[AI SDK](https://docs.archil.com/integrations/ai-sdk) ,[eve](https://docs.archil.com/integrations/eve) , **Mastra** , and **LangChain** .


Archil agent tools allow you to define your context the way that the best agent-builders do, as a *composition* of different kinds of data that we call a "Workspace", and get the tools to read, write, and run code on that context without needing a separate sandbox platform.


With one new function` createDiskTools` , you can expose an entire persistent, shared, compute-enabled filesystem to your agents for:


- **Gathering context** — Read, grep, and glob their way through your data to incorporate real-world knowledge during task execution
- **Sharing context** — Your context becomes **live** ; agents can simultaneously persist information during long-running sessions and collaborate amongst themselves
- **Running compute** — For complex operations, a single bash tool exposes an entire sandbox to your agent, pre-mounted with all of your files


## Examples


TYPESCRIPT


```text
import   { generateText }   from   "ai"  ;
import   *   as   archil   from   "disk"  ;
import   { createDiskTools }   from   "@archildata/ai-sdk"  ;


const   disk   =   await   archil  .getDisk  (  process  .  env  .  ARCHIL_DISK_ID  !  );
// or mount multiple disks as a workspace...
const   disk   =   archil  .workspace  ({
source  :   { disk  :   await   archil  .getDisk  (  process  .  env  .  SOURCE_DISK_ID  !  )  ,   readOnly  :   true   }  ,
reports  :   await   archil  .getDisk  (  process  .  env  .  REPORTS_DISK_ID  !  )  ,
});


const   {   text   }   =   await   generateText  ({
model  :   "anthropic/claude-sonnet-5"  ,
tools  :   createDiskTools  (disk)  ,
prompt  :   "Analyze source/sales/**/*.csv, find regional revenue trends, and write a concise report to reports/q2-revenue.md."  ,
});
```


You can integrate different disks, with different sources, together as a workspace and have the agent use them seamlessly as a single filesystem—one interface for all your data.


## Speaking the language of agents


Unlike regular sandboxes, disk tools don't operate in a vacuum—they operate on your existing, shared, **live** context. They transform the agentic compute layer into a highly persistent and highly connective mechanism. Archil acts as a translation layer, translating the systems that contain your context into an integrated filesystem that your agents don't need to learn how to use.


Bash and filesystems are the language of choice for agents. Agents don't need more opaque infrastructure that forces them to reorganize their thoughts. They need infrastructure that adapts to the way they think and work. Give them context and an action space that matter.
