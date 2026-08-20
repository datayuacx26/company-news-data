---
schema_version: "1.0.0"
document_id: "4546fbaca0e7ca8dcedfc8a8d25c0f64e8c5b6f25c832bdf6a7e0c3f7ad59d11"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/introducing-rubric-scorers-for-mastra-agents"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T17:26:21.590786+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:8d72d3dbeac9b393b3cda82255b3f70896bee7d1b41d0fe52ae719de3e1e44d4"
---

# Introducing Rubric Scorers for Mastra Agents

You can now evaluate your agent's output with a[rubric scorer](https://mastra.ai/reference/evals/rubric) . Define an array of criteria and an LLM-as-judge grades responses against each one.


A rubric scorer returns a pass/fail` score` (1 or 0) and a` reason` summarizing each criterion's outcome. Failed checks are fed back to the agent so it can re-run — set` strategy: 'all'` if every criteria should pass, or` 'any'` if one is enough.


Your browser does not support the video tag.


Before rubric scorers, checking an agent's output against a checklist meant using a post-run eval or hand-rolling a retry loop that fired on failure. With a rubric scorer, the judge validates against the checklist as part of the agentic loop.


Attach it declaratively to your agent's config, or imperatively at runtime when you call` .stream()` or` .generate()` .


## Get started


Install the latest Mastra evals package:


Terminal


```text
npm   install   @mastra/evals
```


note


Requires` @mastra/evals@1.3.0` or later, added in[PR #17724](https://github.com/mastra-ai/mastra/pull/17724) .


Define a rubric with` createRubricScorer` — pass a judge model and an array of criteria:


src/mastra/scorers/match-report-rubric-scorer.ts


```text
import   {   createRubricScorer   }   from   "  @mastra/evals/scorers/prebuilt  "  ;


export   const   matchReportRubricScorer   =   createRubricScorer  ({
model:   "  anthropic/claude-haiku-4-5  "  ,
criteria: [
{ description:   "  A `## Result` section states the score in the format `Team A X Vs Y Team B`.  "   },
{ description:   "  A `## Line-ups` section lists exactly 11 named players for each team.  "   },
{ description:   "  A `## Match Report` section contains 2 short prose paragraphs.  "   },
{ description:   "  The report ends with a `## Sources` section with at least 2 external `https://` links.  "   }
]
});
```


Attach the scorer to your agent's` isTaskComplete` config to grade every run:


src/mastra/agents/match-reporter-agent.ts


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   matchReportRubricScorer   }   from   "  ../scorers/match-report-rubric-scorer  "  ;


export   const   matchReporterAgent   =   new   Agent  ({
id:   "  match-reporter  "  ,
name:   "  Match Reporter  "  ,
instructions:   /* ... */  ,
model:   "  anthropic/claude-sonnet-5  "  ,
defaultOptions: {
maxSteps:   6  ,
isTaskComplete: {
scorers: [matchReportRubricScorer],
strategy:   "  all  "
}
}
});
```


Or attach it per call on` .stream()` or` .generate()` :


```text
const   stream   =   await   agent.  stream  (  "  Write a report for England Vs France World Cup 2026  "  ,   {
maxSteps:   6  ,
isTaskComplete: {
scorers: [matchReportRubricScorer],
strategy:   "  all  "
}
});


for   await   (  const   chunk   of   stream.fullStream)   {
if   (chunk.type   ===   "  is-task-complete  "  )   {
console.  log  (  "  results:  "  ,   chunk.payload.results);
}
}
```


A failed check would output something similar to the below:


```text
[
{
"  score  "  :   0  ,
"  passed  "  :   false  ,
"  reason  "  :   "  ❌ Rubric not yet satisfied...  "  ,
"  scorerId  "  :   "  rubric-scorer  "  ,
"  scorerName  "  :   "  Rubric (LLM)  "  ,
"  duration  "  :   51993
}
]
```


A passed check would output something similar to the below:


```text
[
{
"  score  "  :   1  ,
"  passed  "  :   true  ,
"  reason  "  :   "  ✅ Rubric satisfied: every required criterion is met...  "  ,
"  scorerId  "  :   "  rubric-scorer  "  ,
"  scorerName  "  :   "  Rubric (LLM)  "  ,
"  duration  "  :   36499
}
]
```


For more information and full configuration options, see:


- [Rubric scorer reference](https://mastra.ai/reference/evals/rubric)
- [Supervisor agents — Rubric scorer](https://mastra.ai/docs/agents/supervisor-agents#rubric-scorer)
- [isTaskComplete on stream()](https://mastra.ai/reference/streaming/agents/stream)
