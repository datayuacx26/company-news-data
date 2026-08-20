---
schema_version: "1.0.0"
document_id: "65c222be87468f86df92417154925f6f78696fefcd971ee5ecb5deb6d12d989a"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-3219c37ba697"
canonical_url: "https://browser-use.com/posts/our-browser-agent-evaluation-system"
published_at: "2026-02-23T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:29.258885+00:00"
fetched_at: "2026-07-28T22:03:45.188546+00:00"
content_hash: "sha256:9475079615f6f6eb8b5f45163548845e31af4ee01fd77a6f695658e1db9f2c7a"
---

# How we built scalable evaluation infrastructure for AI web agents

Evaluating an agent that browses the real web isn't like evaluating an LLM on text benchmarks. The web is messy, non-deterministic, and constantly changing.


A popup that takes 100ms longer to load can throw off an entire agent trajectory. A random A/B test can break a selector. Because of this, agent traces are inherently chaotic.


We sourced our evaluation tasks from **millions of LLM-labeled user spans** rather than synthetic sites, because synthetic environments completely fail to capture the bizarre reality, complexity, and ugliness of how the actual web is built.


But dealing with real websites means dealing with variance. Single-run deterministic tests are useless here.


It's concerning that many AI agent benchmarks do not include error bars or variance estimations; coming from backgrounds in experimental particle physics, that lack of statistical rigor is alarming.


You need **real statistical rigor** . That means running the exact same task **multiple times** , messing with a ton of agentic settings, ensuring **perfect reproducibility** of those settings, and aggregating the results with **statistical bootstrapping** .


To do that at scale, you need massive parallelization. Here is a deep dive into how we built a highly parallel, fully observable, and completely agent-driven evaluation loop.


## Building the Evaluation Engine


### 100 Tasks in Under 5 Minutes


We cycled through several third-party evaluation tools before throwing them out to build an engine in-house.


Using[Blacksmith](https://blacksmith.sh/) runners on GitHub Actions, we achieve insane scaling, running 100 complex web tasks in parallel in under 5 minutes end-to-end.


Crucially, we built the LLM judge **directly into the agent code itself** , running after the agent returns` done` . This means that the judge can also double as a **real-time validation layer** for the agent during regular use.


### Observability at Scale


Running fast isn't enough; you need absolute observability. We stream every single token, prompt, timing metric, and cost directly into **ClickHouse** via **Laminar** , which efficiently handles the massive amount of LLM messages.


We even record each browser session and save the frames to Laminar. (For the real-time UI state and making dashboards, we rely on **Convex** to keep our engineers in sync with active runs and results.)


This architecture creates a **massive, queryable dataset** of agent behavior that our team—and our agents—can tap into.


## The LLM Judge & The Failure of Clustering


### Why Deterministic Checks Fail


When tasks involve navigating real websites (e.g., "Find the cheapest flight from JFK to LHR and create a Doc with the options"), there is no simple` assert(success == true)` .


It takes complex reasoning and judgment, but human judges are not scalable. Instead, we need an agentic judge.


### The LLM Judge


We iterated through many judge frameworks, aligning them against **200 meticulously hand-labeled traces** .` gemini-2.5-flash` powers our final judge, achieving an **87% alignment** with human labels.


Crucially, the judge is unbiased when wrong, so the slight misalignment is mitigated by running tasks and judgments many times.


We found that simple prompts and absolute True/False verdicts work best. Complex rubrics lead to indecisive judging.


To enforce strict, actionable outputs, we force the LLM judge to respond with EXACTLY this JSON structure. If a task is simply broken or unreachable, the judge can flag it as impossible based on our clear instructions:


```text
{
"reasoning"  :   "Breakdown of user task into key points.
Detailed analysis covering: what went well,
what didn't work, trajectory quality assessment,
tool usage evaluation, output quality review,
and overall user satisfaction prediction."  ,
"verdict"  :   "<boolean>"  ,
"failure_reason"  :   "Max 5 sentences explanation of why
the task was not completed successfully in case
of failure. If verdict is true, use empty string."  ,
"impossible_task"  :   "<boolean>"  ,
"reached_captcha"  :   "<boolean>"
}
```


### The Clustering Trap


Initially, we tried embedding and clustering the failed traces to find common issues. It was a complete failure—the clusters didn't represent actionable product improvements.


**The Pivot** : We shifted to having Claude Code extract the raw` failure_reason` string from the judge.


Claude reads hundreds of these raw reasons, suggests **concrete categories** , drops the small ones, and iteratively subcategorizes the big ones until we have **highly specific, actionable error buckets** . We sample and classify these failure modes every day to find low-hanging fruit.


## The Agentic Self-Improvement Loop


### Slack as the Control Center


Comparing A/B runs manually in custom dashboards was overwhelming. Making an MCP server helped but had friction with the old architecture.


So we rebuilt the whole evaluation process to be **agent-first** , integrated directly into Slack.


### Claude Code Orchestrator


1. A developer pings Claude in Slack: *"Run an eval with five runs on the brand new gemini-3.1-pro-preview model. Compare it to existing results from gemini-3-pro and report if it is statistically significantly better. On what tasks does it succeed where the old model failed?"*
2. Claude triggers the Blacksmith runners via our custom **MCP server** and by running Python scripts. The traces stream into Laminar and ClickHouse.
3. When done, Claude is pinged. We provide it with the Laminar schema and an example snippet so it knows how to query the raw trace data directly:


```text
import   httpx
import   os


def   query  (sql:   str  ) -> list[  dict  ]:
resp   =   httpx.post(
"https://api.lmnr.ai/v1/sql/query"  ,
timeout  =  30  ,
headers  =  {
"Authorization"  :   f  "Bearer   {  os.environ[  'LMNR_PROJECT_API_KEY'  ]  }  "
},
json  =  {  "query"  : sql},
)
resp.raise_for_status()
return   resp.json()[  "data"  ]


# Claude writes custom SQL like this
# to fetch the results for its analysis:
datapoints   =   query(  f  """
SELECT
trace_id, index, scores,
group_id, executor_output
FROM evaluation_datapoints
WHERE evaluation_id = '  {  EVAL_ID  }  '
"""  )
```


1.


Claude executes its generated SQL queries against ClickHouse and performs statistical A/B analysis, then posts a summary back in Slack suggesting *why* the delta occurred.


2.


The developer can then ping Claude to make changes to the codebase based on that theory. If a subsequent eval run proves the fix worked, we merge it.


## Conclusion & Open Source


This pipeline isn't just for manual testing—it runs automatically on every single PR in our open-source repo to guarantee we don't regress performance.


Our commitment to open source remains a top priority. We have open-sourced one benchmark for LLM providers and researchers at[github.com/browser-use/benchmark](https://github.com/browser-use/benchmark) . The judge prompt and settings are in our open-source agent at[github.com/browser-use/browser-use](https://github.com/browser-use/browser-use) .


We are moving toward a fully closed loop: an automated infinite self-improvement cycle where the agent evaluates itself, finds its own flaws, writes its own patches, and proves its success statistically.


To see how these models actually compare, check out our[definitive model benchmark](https://browser-use.com/posts/what-model-to-use) . For the full technical report on our earlier WebVoyager results, see our[SOTA technical report](https://browser-use.com/posts/sota-technical-report) .
