---
schema_version: "1.0.0"
document_id: "298a154d526c3e80865ba5d31cda278961640d6aad55363252222a4fd02c5e16"
company_key: "yc-activeloop"
company: "Activeloop"
source_id: "yc-activeloop-news-import-11accf1de1c3"
canonical_url: "https://deeplake.ai/blog/scrapegraph-x-activeloop-hivemind"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-24T14:16:27.843975+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:161247bb2a48b0b8d3a19d44fc0d970b448208f42da3f9b7413d245607fe9ca0"
---

# Hivemind Skills, Enriched: Turn Session Lessons into Full Playbooks with ScrapeGraphAI

**TLDR** :[Hivemind](https://github.com/activeloopai/hivemind) already captures what your agents learn. A drop-in Claude skill we built with[ScrapeGraphAI](https://scrapegraphai.com/) takes those focused skills and layers on live web research.


- **What Hivemind does:** At[Activeloop](https://activeloop.ai/) , we built Hivemind to watch your coding agent sessions and distill them into reusable skill files, each one a real lesson your team learned, trimmed to what matters.
- **What we added:** A skill called` enrich-hivemind-skill` . It reads a Hivemind skill, runs one ScrapeGraphAI research pass, and expands the file with pitfalls, code samples, and citations.
- **How to run it:** Drop the skill in` ~/.claude/skills/` , then tell your agent "enrich the X skill."
- **What you get:** The original 44-line Hivemind skill stays the high-signal core; ScrapeGraphAI grows it into a 263-line playbook with sourced references and implementation depth.


The pipeline: **Hivemind codifies your sessions into skills. ScrapeGraphAI enriches them with what the open web already documents.**


---


At Activeloop, we built[Hivemind](https://github.com/activeloopai/hivemind) as a continual-learning layer for coding agents. It watches your sessions, spots recurring patterns, and turns them into reusable` SKILL.md` files so the next agent on your team starts smarter. The` capture -> codify -> propagate` loop handles the hard part: converting a real session trace into a portable lesson your whole team can reuse.


Hivemind skills are intentionally tight. They carry tacit knowledge, what your team actually hit in production, not a full textbook. That is the right shape for fast recall, but agents often need more: official docs, community gotchas, wrong-vs-right patterns. So we partnered with[ScrapeGraphAI](https://scrapegraphai.com/) and built an enrichment skill that keeps every word Hivemind wrote and layers on explicit knowledge from the open web.


Below: what Hivemind gives you out of the box, how the enrichment skill works, a real before/after example, and the full skill to copy.


## What Is Hivemind


Hivemind turns what your agents learn into reusable skills and propagates them across your team, continual learning without manual wiring. The` capture -> codify -> propagate` loop:


1. **Capture** what happens across your sessions
2. **Codify** the lessons into` SKILL.md` files
3. **Propagate** them so every agent on your team starts ahead


What that gives you:


- **AI session summaries:** A background worker writes a wiki-style summary of every session (key decisions, code changes, next steps) and indexes it for recall.
- **Org-level access control:** Invite team members with` ADMIN` ,` WRITE` , or` READ` permissions, and switch between orgs and workspaces on the fly.
- **Virtual filesystem:** Memory lives under` ~/.deeplake/memory/` . Use` cat` ,` ls` ,` grep` , the same commands you already know. No new APIs to learn.
- **Real-time sync:** All writes go to Deeplake Cloud and propagate to every agent in the org in real time.
- **Zero infrastructure:** No databases to run, no servers to maintain. Deeplake Cloud handles storage, indexing, and search as a managed service.
- **Privacy controls:** Opt out of capture anytime with` HIVEMIND_CAPTURE=false` . Credentials stay local with strict file permissions (` 0600` /` 0700` ).


## Installing Hivemind


One command, every agent on your machine. The unified installer detects every supported assistant, wires up the hooks, and opens your browser once to sign in. Restart your assistants and they all share the same brain.


Recommended (all assistants):


bash


```text
curl   -fsSL   https://deeplake.ai/hivemind.sh   |   sh
```


That auto-detects Claude Code, OpenClaw, Codex, Cursor, Hermes, and pi.


Prefer to wire up a single assistant? After` npm install -g` , target one instead of all of them:


bash


```text
hivemind   claude   install     # Claude Code
hivemind   claw   install       # OpenClaw
hivemind   codex   install      # Codex
hivemind   cursor   install     # Cursor
hivemind   hermes   install     # Hermes
hivemind   pi   install         # pi
```


## Enriching Hivemind Skills with ScrapeGraphAI


[ScrapeGraphAI](https://scrapegraphai.com/) is the web-research engine in this integration. You hand it a natural-language prompt and a URL, and it returns clean, structured JSON, with no CSS selectors and no brittle markup. One call covers search, scrape, extract against a schema, crawl, and page monitoring. It ships as an API, SDK, MCP tools, and a CLI skill. Our enrichment skill uses a single` search` call: research a topic across the open web, get structured data back, merge it into a SKILL.md.


Hivemind supplies the session-proven core. ScrapeGraphAI adds the surrounding depth from docs and community sources.


### A real example


Here is a skill Hivemind codified after a Next.js debugging session:


markdown


```text
---
name: nextjs-server-client-boundary-verification
description: "Catch all server/client boundary violations in Next.js before declaring fixes done by systematically checking data flow through component trees."
source_sessions:
-   emanuele.fenocchi_activeloop_hivemind_177626fa-90c1-4d83-b130-8aad087c3b5c
version: 1
created_by_agent: claude_code
created_at: 2026-05-26T12:00:00.000Z
updated_at: 2026-05-26T12:00:00.000Z
---


## When to Use
After fixing any Next.js server→client prop passing issue, before marking work complete.


## Workflow
1.   Identify the error component (e.g., PermitsTable, CorridorsTable)
2.   Trace the component tree, verify directives and props
3.   For each prop: function? data? serializable?
4.   Run dev server and check for residual errors before closing the task
```


Forty-four lines, each one earned. This is Hivemind at its best: distilling a real debugging session into the problem your team hit and the workflow to catch it again. The enrichment skill preserves every word and builds on top with implementation detail from the live web:


- **An Error → Cause → Fix lookup table** for the 9 most common boundary errors (` Only plain objects can be passed...` ,` Event handlers cannot be passed...` ,` Hydration mismatch` , etc.)
- **Wrong-vs-right code pairs** for the actual fixes (Mongoose` .lean()` , server-action` "use server"` pragma, arrow-wrap on` onClick` )
- **Serialization rules** spelling out what crosses the boundary and what does not
- **A tooling row** with` next lint` ,` next-safe-action` , React DevTools
- **References** to official[Next.js](https://nextjs.org/docs) ,[React](https://react.dev/reference/react) , and[Vercel](https://vercel.com/docs) docs


Below is a real slice of the enriched output. The full file is 263 lines; what follows is roughly the first third.


The frontmatter now carries enrichment provenance:


markdown


```text
---
name: nextjs-server-client-boundary-verification
source_sessions:
-   emanuele.fenocchi_activeloop_hivemind_177626fa-90c1-4d83-b130-8aad087c3b5c
version: 2
created_by_agent: claude_code
created_at: 2026-05-26T12:00:00.000Z
updated_at: 2026-05-26T15:58:00.000Z
enriched_by: ScrapeGraphAI
enriched_at: 2026-05-26T15:58:00.000Z
enrichment_sources:
-   https://nextjs.org/docs/app/getting-started/server-and-client-components
-   https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations
-   https://react.dev/reference/react/use-server#serializable-parameters-and-return-values
-   https://stackoverflow.com/questions/77091418/warning-only-plain-objects-can-be-passed-to-client-components-from-server-compo
-   https://github.com/vercel/next.js/discussions/46795
-   https://upsun.com/blog/avoid-common-mistakes-with-next-js-app-router/
-   https://www.propelauth.com/post/5-common-pitfalls-with-server-components-in-next13-with-examples
---
```


The original body stays untouched. New sections are appended below, each suffixed` (enriched)` so they stay distinct from what Hivemind wrote.


The first new section is the error lookup: grep it the moment a stack trace appears:


markdown


```text
## Error → Cause → Fix lookup table (enriched)


| Error Message | Root Cause | Fix |
|---|---|---|
| Only plain objects can be passed to Client Components from Server Components | Server passing a class instance, Mongoose doc, or object with a custom prototype | JSON.parse(JSON.stringify(obj)) OR manually extract primitives |
| Event handlers cannot be passed to Client Component props | Server passing a function (onClick, formatter) to a client component | Add "use client" to the receiving component, or define the handler inside the client component |
| Objects with toJSON methods are not supported | Object has custom toJSON (common with ORMs) | Strip the method via destructuring or JSON.parse(JSON.stringify(obj)) |
| Hydration mismatch: Server rendered markup does not match client rendered markup | Prop changed between server/client render (Date, random ID, Date.now()) | Move the dynamic value generation into the client component via useState(() => ...) or useEffect |
| Passing a server function reference to an onClick handler is not allowed | onClick={serverAction} forwards the event object to the server action | Wrap in arrow: onClick={() => serverAction()} |
| Mongoose document cannot be serialized when passed to a client component | Mongoose docs contain ObjectId, Date, internal methods | Use .lean() on the query, then _id: doc._id.toString() |
| Missing "use server" directive in a server action | File exporting server actions without the pragma | Add "use server"; at the top of the file or function |
| Fetch response returned directly to a client component | Returned raw Response object from fetch | return await response.json() instead of return response |
| API route returns 404 / unexpected behavior | API file named page.ts instead of route.ts | Rename to route.ts |
```


Then serialization rules so the agent stops guessing:


markdown


```text
## Serialization rules: what CAN cross the boundary (enriched)


Allowed (serializable):
-   Primitives: string, number, bigint, boolean, undefined, null, globally-registered Symbols
-   Plain objects (object literals) with only serializable properties
-   Arrays, Map, Set, TypedArray, ArrayBuffer containing serializable values
-   Date instances (Next.js handles these specifically)
-   Promise (resolved values must be serializable)
-   Server Actions (functions marked with "use server")
-   React elements (JSX), Client or Server Component elements


NOT allowed (will throw):
-   Functions (except Server Actions)
-   Class instances (Mongoose documents, Prisma models, custom classes)
-   Objects with custom prototypes
-   Objects with custom toJSON methods
-   RegExp, raw Request/Response, Node.js streams
-   Symbols NOT registered globally
-   Circular references


Hidden traps:
-   undefined values are silently dropped during serialization, don't rely on them surviving
-   ORM result objects look plain but have hidden prototype methods, always .lean() (Mongoose) or destructure (Prisma)
```


Then wrong-vs-right code pairs, eight of the nine errors above (the` page.ts` →` route.ts` rename needs no snippet). Two examples:


tsx


```text
// Pattern 1: Server action as onClick handler (forward-event bug)


// ❌ Wrong: forwards the event object to the server, breaks
<  button   onClick  =  {revalidateEvents}>Refresh</  button  >


// ✅ Right: wrap in arrow, drop the event arg
<  button   onClick  =  {()   =>   revalidateEvents  ()}>Refresh</  button  >
```


tsx


```text
// Pattern 2: Mongoose document leak


// ❌ Wrong: full Mongoose doc has ObjectId + methods
const   user   =   await   UserModel.  findById  (id);
return   <  UserCard   user  =  {user} />;


// ✅ Right: .lean() returns plain obj, stringify _id
const   user   =   await   UserModel.  findById  (id).  lean  ();
user._id   =   user._id.  toString  ();
return   <  UserCard   user  =  {user} />;
```


A tooling row so the agent knows which lint/runtime tools catch this class of bug:


markdown


```text
## Tooling (enriched)


| Tool | Purpose |
|------|---------|
| next lint (built-in) | Runs ESLint with Next.js config, catches missing directives |
| @next/eslint-plugin-next | Server/client boundary checks, illegal imports |
| Next.js TypeScript plugin (tsconfig   `"name": "next"`  ) | TS helpers surfacing server/client contract violations in your editor |
| React DevTools | Inspect component hierarchy and actual props received post-boundary |
| next-safe-action | Library enforcing serializable inputs to server actions, with runtime type checks |
```


After these sections come **five more** : the full set of eight wrong-vs-right patterns, a` "use client"` decision list, a debugging workflow, a pre-completion checklist, and seven authoritative references. Hivemind's original 44 lines anchor all 263, and every new line traces back to a source in the frontmatter.


The key idea: Hivemind gives you **tacit** knowledge (what your team learned the hard way). ScrapeGraphAI adds **explicit** knowledge (what docs and the community already document). The enrichment skill merges both in one file.


## How To Use It


No infrastructure beyond what you already have for Hivemind. The enricher is itself a skill file.


**1. Install Hivemind** (if you have not already):


bash


```text
curl   -fsSL   https://deeplake.ai/hivemind.sh   |   sh
```


**2. Set up scrapegraph-mcp.** Grab a key from the[ScrapeGraphAI dashboard](https://scrapegraphai.com/dashboard) (the free tier covers plenty of runs), then add the server to your Claude Code MCP config:


json


```text
{
"mcpServers"  : {
"scrapegraph-mcp"  : {
"command"  :   "npx"  ,
"args"  : [
"mcp-remote@latest"  ,
"https://mcp.scrapegraphai.com/mcp"  ,
"--header"  ,
"X-API-Key:YOUR_API_KEY"
]
}
}
}
```


Replace` YOUR_API_KEY` with your key (keep the colon, no space after it). Full setup guide:[github.com/ScrapeGraphAI/scrapegraph-mcp](https://github.com/ScrapeGraphAI/scrapegraph-mcp) . Your agent now has scrapegraph-mcp's` search` ,` scrape` , and` extract` tools.


**3. Drop the skill in place.** Save the skill below as` ~/.claude/skills/enrich-hivemind-skill/SKILL.md` :


markdown


```text
---
name: enrich-hivemind-skill
description: Enrich concise Hivemind-codified skills with real-world web research using ScrapeGraphAI MCP tools. Use when the user asks to "enrich", "deepen", "research-boost", or "improve" a SKILL.md, OR when you encounter a SKILL.md under ~/.claude/skills/ that has Hivemind provenance frontmatter (source_sessions, created_by_agent) but lacks an   `enriched_by: ScrapeGraphAI`   field and has room to grow (under ~50 lines, no Code Examples section yet, no References section yet). The skill uses scrapegraph-mcp's search tool with a topic-adapted output_schema to research the skill's topic on the open web in a single call, then rewrites the SKILL.md with added Known Gotchas, Code Examples, Best Practices, and References while preserving the original pattern and frontmatter.
version: 2
---


# Enrich Hivemind Skill with ScrapeGraphAI


You are the enrichment loop between a memory tool (which codifies concise skills
from agent traces) and the open web. Read a concise SKILL.md, research its topic
with ScrapeGraphAI, and expand it with richer, sourced content.


## When to fire
-   A SKILL.md has provenance frontmatter (source_sessions, created_by_agent)
-   It does NOT already have   `enriched_by: ScrapeGraphAI`
-   The topic is researchable on the open web (a library, tool, or pattern, not a
generic principle like "verify before claiming")


## Pipeline
1.   Read the target SKILL.md. Hold the original body in memory verbatim.
2.   Write a SPECIFIC research query. Not "Next.js bugs" but "Next.js 15 App
Router server client component boundary errors serialization".
3.   Pick an output_schema shaped to the topic. Always include a floor of
gotchas, code_patterns, best_practices, tools_and_libraries, key_references.
Add topic-specific fields:
-   error-debugging skills: error_messages, wrong_vs_right_patterns
-   detection skills: detection_heuristics, diagnostic_workflow
-   API skills: auth_requirements, rate_limits, endpoint_patterns
4.   Call scrapegraph-mcp's search tool with user_prompt + prompt + output_schema.
One call does search + scrape + extract together.
5.   If the response is too large for context, it is auto-saved to disk. Pull
sections out with jq instead of reading the whole file.
6.   Dedupe: tools by name, references by url, gotchas by meaning, code by
language+purpose. Keep the most specific version. Aim to cut 30-50%.
7.   Compose the new file:
-   Preserve the original body and frontmatter verbatim.
-   Bump version, add enriched_by: ScrapeGraphAI, enriched_at, enrichment_sources.
-   Append new sections, each suffixed "(enriched)" so they stay distinct.
-   Order by what the reader reaches for first (errors -> lookup table first,
detection -> heuristics first, API -> endpoints first).
-   Tables for 3+ column data, bullets for flat lists, fenced+language-tagged
code blocks for every snippet.
8.   Quality gate. Refuse to write if the content is generic ("be careful"), the
code looks hallucinated, all references share one domain, or the extract is
nearly empty.
9.   Atomic write: write SKILL.md.new, then mv it over SKILL.md.
10.   Report one line: enriched <name>: +N gotchas, +M code, +K refs, vX->vY.


## Non-goals
-   Do NOT enrich hand-written skills (no provenance frontmatter).
-   Do NOT call ScrapeGraphAI for topics that need no web research.
-   Do NOT modify the original body or the source_sessions / created_by_agent fields.
```


The skill is intentionally model-driven. It tells the agent what to do and trusts it to handle each topic, rather than hard-coding a rigid script. That is why one skill works across scraping, API usage, and framework-debugging topics without changes.


**4. Ask for it.** In any Claude Code session, say:


```text
enrich the nextjs-server-client-boundary-verification skill


```


The agent reads the skill, runs one ScrapeGraphAI search with a topic-shaped schema, dedupes the results, and rewrites the file. It bumps the version, marks every new section` (enriched)` , and lists the sources it used.


That is the whole workflow. Point it at any Hivemind skill, and its lesson grows into a full playbook.


## Try It


If you are already running Hivemind, this layers on top without changing anything else. Install Hivemind, connect ScrapeGraphAI, drop the skill in` ~/.claude/skills/` , and run it against one of your codified skills. See if the deeper version helps your next session. If it does, run it across the rest.


Both projects are open source:


- **Hivemind:**[github.com/activeloopai/hivemind](https://github.com/activeloopai/hivemind)
- **ScrapeGraphAI:**[github.com/ScrapeGraphAI/Scrapegraph-ai](https://github.com/ScrapeGraphAI/Scrapegraph-ai) (and the[just-scrape](https://github.com/ScrapeGraphAI/just-scrape) skill)


## Related Articles


- [Software Factory Ran Autonomously for 15h, 2x Speed Up on TPC-H, ASAN-verified, Cost $160](https://deeplake.ai/blog/coding-agent-15h) - How an autonomous agent optimized a large C++ codebase over 15 hours for $160
- [Spin up Postgres in a second: How We Built Serverless PG for Agents.](https://deeplake.ai/blog/serverless-pg) - A serverless, PostgreSQL-compatible database built for agent workloads
- [Your Agents Are Drowning in Quicksand. Give Their Data a Sandbox.](https://deeplake.ai/blog/agent-sandbox) - A sandboxed, serverless Postgres instance that spins up with every agent
