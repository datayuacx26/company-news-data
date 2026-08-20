---
schema_version: "1.0.0"
document_id: "c8936f1ecc96904caeaf26b3b318c3f9d69c0c4682b4a2c40fc8880e779af4b1"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/we-fixed-23-bugs-in-2-weeks-by-getting-ai-to-break-things-while-we-slept"
published_at: "2026-01-12T04:45:53.622+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T22:23:44.568277+00:00"
content_hash: "sha256:e66eda831753c2d3d8cacbc93e41117e8f971bad36197f9dd3432646e46fb8ea"
---

# We Fixed 23 Bugs in 2 Weeks By Getting AI to Break Things 24/7

## Always-On Debugging


We've been building[Tusk Drift](https://github.com/use-tusk/drift-node-sdk) , an open-source testing tool that records and replays live traffic to find regressions. Think of this as a Postman Collection that's automatically created and maintained based on user interactions with your service.


To record traces that include request/response bodies, we create instrumentation (similar to OpenTelemetry's) for libraries like Postgres, MySQL, and Redis. Each time we add instrumentation for a new library, we write tests to verify that it works. Being a testing company ourselves, we try to be exhaustive in our testing but––being human––we aren't able to think of every possible execution path.


I had an idea: what if we could use Claude Code to proactively hunt for bugs in our instrumentation?


So I decided to let Claude Code run in the background. It would poke around in the SDK repo, try different call patterns, and exercise methods we forgot to test. Naturally, it ended up generating a bunch of hypotheses about potential bugs.


Some examples of threads it would dive into:


- "What if they use callbacks instead of promises?"
- “What if they use connection pooling?”
- "What if they pass` null` here?"


Great, but now we have a different problem. Out of the dozens of hypotheses generated, **how do we know which of them are actually bugs?**


It’s common knowledge now that LLMs are really good at generating a confident-sounding list of issues. A few will be real, while a good amount will be false positives. And then there are some that are technically possible but may only happen in some sort of black swan event.


This is why a lot of surface-level "AI finds bugs" demos don't translate to actual workflows in production. The AI exploration part is easy. The verification part is where it falls apart.


The first step of building this true AI developer then was to narrow the possibilities of its own thinking. Otherwise, we were trading one problem for another by adding a workflow step that was almost as tedious as the original.
‍


## Building a Verification System


What worked for us was building e2e test infrastructure that could deterministically verify whether something was actually a bug.


We run our tests in three modes:


` DISABLED → RECORD → REPLAY`


Each mode answers a specific question:


- **Disabled** : Does the test work without our instrumentation? (Baseline)
- **Record** : Does it still work with our instrumentation? Do we capture traffic?
- **Replay** : Can we replay the captured traffic deterministically?


If a test passes in “Disabled” but fails in “Record”, our instrumentation broke something. If it passes in “Record” but fails in “Replay”, we didn't capture traffic correctly. If it fails in “Disabled”, the test itself is broken and should be disregarded.


The outcomes are binary, which means there's no room for subjective interpretation.


Now Claude Code can generate as many hypotheses as it wants, and the verification system filters out the noise automatically.
‍


## How It Works


I gave Claude Code a structured workflow:


**Phase 1: Understand the codebase**


Read the instrumentation code, the package we're instrumenting, and existing tests


**Phase 2: Generate hypotheses**


Get the agent to poke around and ask questions like:


- What functions are patched?
- What functions should be patched but aren't?
- What parameter combinations haven't been tested?
- What alternative call patterns exist?


**Phase 3: Test each hypothesis**


For each potential bug:


1. Write an e2e test endpoint
2. Run it in “Disabled” mode (no instrumentation)
3. If it fails, discard the test because it's broken
4. If it passes, run it in “Record” mode (with instrumentation)
5. If it fails or doesn't capture traces, the bug is confirmed
6. If it passes, run it in “Replay” mode
7. If it fails or shows warnings, the bug is confirmed
8. If it passes all three, discard the test because there's no bug


**Phase 4: Document immediately**


After each test, update a` BUG_TRACKING.md` file with the results. This creates an audit trail on an ongoing basis and prevents Claude Code from forgetting what it's tested.


At the same time, we provide instruction to Claude Code to only keep test endpoints that found actual bugs in its changes. Remove all others to prevent bloat.


**Phase 5: Fix confirmed bugs**


For any confirmed bug in` BUG_TRACKING.md` , start a separate agent conversation with the context from the markdown file and failing e2e test that surfaced the bug.


Have Claude Code generate a fix, re-run the e2e test, look at the test execution output and iterate if the test still isn’t passing. Do this for each confirmed bug until the e2e test is passing.
‍


## Example: Postgres Instrumentation Bug


Let me show you what this looks like in practice.


Claude Code was analyzing our Postgres instrumentation. It read through the Postgres library source code and noticed that queries can accept a` rowMode` parameter. When set to` 'array'` , Postgres returns results as arrays instead of objects:` ‍` ‍


```text
// Normal mode returns objects
// result.rows = [{ id: 1, name: 'John', email: 'john@example.com' }]


// rowMode: 'array' returns arrays
// result.rows = [[1, 'John', 'john@example.com']]
```


Claude Code generated a hypothesis: "What if users specify` rowMode: 'array'` in their queries?" It wrote a test endpoint for our SDK:` ‍`


```text
// Test endpoint for query with rowMode: 'array'
if   (url ===   "/test/query-rowmode-array"   && method ===   "GET"  ) {
const   result =   await   client.query({
text  :   "SELECT id, name, email FROM test_users WHERE id = $1"  ,
values  : [  1  ],
rowMode  :   "array"  ,
});
res.writeHead(  200  , {   "Content-Type"  :   "application/json"   });
res.end(
JSON  .stringify({
success  :   true  ,
data  : result.rows,
rowCount  : result.rowCount,
queryType  :   "query-rowmode-array"  ,
}),
);
return  ;
}
```


**Disabled mode:** ✅ Endpoint works fine, returns arrays as expected **‍**


**Record mode:** ✅ Endpoint runs without errors, traces captured


At this point, you might assume there's no bug. The code runs successfully with instrumentation and we're capturing traffic. However, when in replay mode, Claude Code found a data format mismatch.


Instead of getting:


```text
[[  1  ,   'John'  ,   'john@example.com'  ]]
```


The application received:


```text
[{   '0'  :   1  ,   '1'  :   'John'  ,   '2'  :   'john@example.com'   }]
```


i.e., objects with numeric keys instead of arrays. The bug is confirmed.


The agent then helped push a fix ([PR #79](https://github.com/Use-Tusk/drift-node-sdk/pull/79) ) that made three changes:


1. Track the` rowMode` parameter when parsing queries
2. Pass` rowMode` through to our type conversion logic
3. Handle both formats when converting PostgreSQL types back to JavaScript


The type conversion logic now checks` rowMode` and processes rows accordingly:` ‍`


```text
// If rowMode is 'array', handle arrays differently
if   (rowMode ===   'array'  ) {
// For array mode, rows are arrays of values indexed by column position
const   convertedRows = result.rows.map(  (  row:   any  ) =>   {
if   (!  Array  .isArray(row))   return   row;   // Safety check


return   row.map(  (  value:   any  , index:   number  ) =>   {
const   field = result.fields[index];
if   (!field)   return   value;


return     this  .convertPostgresValue(value, field.dataTypeID);
});
});


return   {
...result,
rows  : convertedRows,
};
}
```


This is the type of subtle bug that's easy to miss. Our existing tests all used the default object mode. They passed. But any user specifying` rowMode: 'array'` would get incorrect data during replay.


Without the verification system, Claude Code would have generated the hypothesis "what if they use` rowMode: 'array'` ?" and I might think, "Well, we're capturing the query results, so that should work." The hypothesis would seem unnecessary. With the verification system, there's no debate that the data format was wrong. The bug is real.
‍


## Building Your Own


Having an AI developer work 24/7 trying to break your code always felt like a pipe dream. So it’s really cool to see that some semblance of this is possible today.


We've tested this workflow across multiple libraries now. Claude Code, when used this way, reminds me of a senior engineer who thinks systematically about edge cases, triages with precision, and tests rigorously before claiming victory.


This pattern generalizes beyond our SDK’s instrumentation. The core idea is that AI generates hypotheses while systems verify truth.


Here's what you need:


**1. A way to express potential bugs as tests**


Can you write code that exercises the potential bug? For us, those are e2e test endpoints. For you, it might be unit tests, integration tests, or something else.


**2. Multiple modes or branches that reveal bugs**


For us, the modes are disabled/record/replay. For you, the modes might be:


- With/without optimization flags (compiler bugs)
- Direct API calls vs SDK calls (client bugs)
- Mocked vs real database (ORM bugs)
- Different runtime versions (compatibility bugs)


The key is having at least two modes where bugs manifest differently.


**3. Binary outcomes**


Each verification step needs a clear, objective pass/fail. "The test crashed" is binary. "The output seems weird" –– not so much.


**4. Incremental documentation**


Don't wait until the end to document a summary of the agent’s findings. After each test, get the agent to write down what happened. This keeps the agent on track and creates an audit trail for it (and you) to review.
‍


## What We Found


Across 12 libraries, we found and resolved 23 bugs using this Claude Code workflow. Bugs included things like:


- Methods that should be instrumented but weren't
- Callback vs promise variations not handled correctly
- Nested client scenarios breaking context propagation
- Parameter combinations that broke serialization


Claude Code tested 100+ hypotheses total. That means it generated a ton of false positives. Still, without the verification system, I'd be manually investigating all 100 odd hypotheses. With it, I know the 23 are real and the remainder aren't worth my time.
‍


## Final Note


In the last 8 months, a lot of attention has shifted from human-in-the-loop towards background agents like[OpenAI Codex](https://openai.com/codex/) and[Google Antigravity](https://antigravity.google/) performing long-running tasks.


What has been undervalued, however, is the design of smart feedback loops and exit conditions that enable your coding agents to be creative while reaching objective truths autonomously.


As with most coding agents, Claude Code is great at generating hypotheses. It thinks of edge cases and explores exhaustively in a way that humans can’t. But distinguishing real bugs from hallucinated ones is where it still struggles.


Your verification system doesn't need to be fancy. Ours relies on running e2e tests in three different modes. The most important thing is for it to be deterministic and binary.


Build the system that says "yes" or "no." Then let AI work within it.
‍


---


**About Tusk:** Tusk is an AI-powered unit/integration testing platform that allows engineering teams to ship changes confidently, even in the age of AI.


Our core product, Tusk Drift, auto-records live traffic and replays traces as deterministic tests to find regressions. For that to work, our SDK’s instrumentation needs to handle every edge case. This debugging workflow is how we ensure quality at scale.


Try it yourself at[usetusk.ai](https://usetusk.ai/?utm_source=tuskblog) or check out our[open-source SDK](https://github.com/use-tusk/drift-node-sdk) . Stars ⭐ and contributions welcome.
