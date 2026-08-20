---
schema_version: "1.0.0"
document_id: "520fb9480c7d1cf0cd3b2857cbaaacbcfdf9db8a27bc05fff9a56e2107b3b121"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/ai-agent-optimization-production-scale"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T15:43:13.959850+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:a33a3052fefe690c888b8623c347cf47e500169470f4f77f57c1a8e080157e1a"
---

# Inside Elastic InfoSec's agentic SOC: How we cut AI agent LLM calls by 60%

27 July 2026 •


[Aaron Jewitt](https://www.elastic.co/security-labs/author/aaron-jewitt)


# Inside Elastic InfoSec's agentic SOC: How we cut AI agent LLM calls by 60%


We run fourteen AI agents that triage Elastic InfoSec alerts. They were taking 19 LLM calls to do work that needed 8. Here's the five-step optimization loop we run across the fleet, plus the prompt template you can use with any AI assistant.


11 min read


[Generative AI](https://www.elastic.co/security-labs/category/generative-ai) ,


[Internals](https://www.elastic.co/security-labs/category/internals)


*This is Part 3 of the **Inside Elastic InfoSec's Agentic SOC** series.[Part 1: How we triage every alert before an analyst opens it](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows) ·[Part 2: Choosing the right agent architecture for a 5× cost reduction](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture)*


We run 14 AI agents in the Elastic InfoSec security operations pipeline. They were producing correct verdicts and taking up to 19 large language model (LLM) calls to do work that needed 8, at thousands of input tokens per call. At hundreds of runs per day, that compounds fast. We built a five-step optimization loop to measure, diagnose, and fix exactly this. On the same workload, LLM call counts dropped to 7–9. Every step applies to any[Elastic Agent Builder](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) agent, and the instruction-revision step works with any AI assistant your team already uses.


We needed a repeatable process for identifying what was wrong with each agent rather than guessing at prompt edits and hoping the numbers moved in the right direction. The loop works on any Agent Builder agent: fully automated triage pipelines, analyst-led chat assistants, single-purpose enrichment agents, or anything in between.


At Elastic, our InfoSec team operates as Customer Zero. We run the newest versions of[Elastic Security](https://www.elastic.co/guide/en/security/current/) and Agent Builder in our production environment, often before they reach general availability, across a globally distributed fleet of laptops, servers, and cloud workloads. We’re the first and most demanding user of every feature we ship. The agents we’re optimizing run against real alerts from our production detection rules, so the numbers matter.


This post focuses on the optimization process itself, which applies to any agent regardless of purpose. The companion posts ([Part 1](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows) ,[Part 2](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture) ) describe the pipeline we apply this methodology to.


##


Why agent optimization is different from prompt engineering


*Prompt engineering* focuses on getting a model to produce the right answer. *Agent optimization* focuses on getting an agent to produce the right answer consistently, thousands of times, at a predictable cost. These are different problems that require different approaches.


The dominant cost driver in an agentic workflow isn't the length of the system prompt. It's the number of LLM calls the agent makes to complete its work. Cost scales with` llm_calls × input_tokens_per_call` , and input tokens per call grow as the conversation accumulates more context. In our own measurements, average input tokens per LLM call ranged from roughly 10,000 for narrowly scoped specialized agents to roughly 36,000 for skills-based agents with broader toolsets. The conversation history carrying forward each LLM call was the dominant weight, regardless of system prompt size. An agent that makes six extra LLM calls to finish work it could have completed earlier isn't slightly more expensive; trimming the prompt by a third wouldn't recover the same cost.


> **A note on terminology.** The Agent Builder consumption API exposes two related metrics:` round_count` (the number of user-agent turns) and` llm_calls` (the total LLM API invocations across those turns). For automated workflow agents that handle one alert per invocation,` round_count` is always 1, so the cost lever is` llm_calls` . For analyst-led interactive agents where a conversation can run multiple messages, both matter. This post uses "LLM calls" as the primary unit, except when quoting verbatim from a system prompt that uses "rounds."


Three things are worth optimizing separately:


- **LLM call count:** The single biggest factor to reduce. An agent that makes 14 LLM calls when 8 are sufficient costs roughly 75% more per run, before considering context-growth overhead on the later calls.
- **Tool-call discipline:** Redundant queries, schema-exploration calls, and requerying data that was already retrieved earlier in the conversation are all avoidable once you see them in a trace.
- **Behavioral consistency:** The same input should produce the same investigation path on every run. High variance in token count across similar inputs is a signal that the agent is deciding what to do at runtime rather than following a prescribed methodology.


##


The five-step agent optimization loop


The process has five steps. Each is described in its own section below.


1. **Measure the baseline:** Capture LLM call count and token usage for the agent in its current state before changing anything.
2. **Capture representative test conversations:** Run the agent against a curated set of inputs under your own credentials so the full conversation bodies are available for analysis.
3. **Analyze conversation traces for inefficiency patterns:** Compare the agent's actual behavior in those conversations against a checklist of known cost drivers.
4. **Revise and verify:** Use the analysis to produce revised instructions, apply them in a QA environment, and measure again to confirm improvement.
5. **Monitor for drift:** Run the measurement step on a schedule in production so emerging inefficiencies surface before they grow expensive.


##


Step 1: Measure the baseline


Before touching the prompt, record how the agent is performing today. The[Agent Builder consumption endpoint](https://www.elastic.co/docs/api/doc/kibana/operation/operation-post-agent-builder-agents-agent-id-consumption) returns per-conversation token usage,` llm_calls` , and` round_count` for a given agent, across all users in the space:


```text
curl -X POST \
-H "Authorization: ApiKey ${KIBANA_API_KEY}" \
-H 'kbn-xsrf: true' \
-H 'Content-Type: application/json' \
"${KIBANA_URL}/s/${KIBANA_SPACE}/api/agent_builder/agents/${AGENT_ID}/consumption" \
-d '{
"size": 100,
"sort_field": "updated_at",
"sort_order": "desc"
}'
```


Replace` ${KIBANA_URL}` ,` ${KIBANA_API_KEY}` ,` ${KIBANA_SPACE}` , and` ${AGENT_ID}` with your Kibana URL, API key, space name, and target agent ID.


The API uses cursor-based pagination. If you have more than 100 conversations in the window, pass the` search_after` value from each response into the next request body until the results array is empty.


Four things to track:


- ` llm_calls` : Total LLM API invocations across the conversation. **For automated agents, this is the primary cost lever.** Each LLM call pays the full and growing conversation-history cost.
- ` round_count` : The number of user-agent turns. For automated workflow agents that handle one alert per invocation, this is always 1. For analyst-led interactive agents, it grows with the conversation; watch it for those agents.
- ` token_usage.total_tokens` : The total cost for that conversation. Record the median across conversations, not the mean. A single runaway conversation on an unusual alert can skew the mean significantly.
- ` Run-to-run variance` : If similar inputs produce a 2–3× spread in token counts, the agent isn't following a consistent investigation path. Variance is as meaningful a signal as the median.


Capture these numbers for a representative time window (the last 14 days is usually sufficient for high-volume agents) before you make any changes. Without a baseline, you cannot tell whether a subsequent prompt edit helped or hurt.


> **Note:** The consumption API aggregates across all users in the space, so it reflects real-world usage across analysts and automated workflows alike. The individual conversation bodies are per-user access-controlled (see Step 2), but the token statistics are fleet-wide.


##


Step 2: Capture representative test conversations


Each conversation in Agent Builder is readable only by the user who created it. A request for another user's conversation returns 404, regardless of role. This means conversations that analysts generate in the Kibana UI under their own credentials aren't available to optimization scripts running under a separate API key.


The solution is to generate test conversations under your own credentials before running the trace analysis. Use the[Converse API](https://www.elastic.co/docs/api/doc/kibana/operation/operation-post-agent-builder-converse) to submit test inputs to the agent:


```text
curl -X POST \
-H "Authorization: ApiKey ${KIBANA_API_KEY}" \
-H 'kbn-xsrf: true' \
-H 'Content-Type: application/json' \
"${KIBANA_URL}/s/${KIBANA_SPACE}/api/agent_builder/converse" \
-d '{
"agent_id": "<your_agent_id>",
"input": "<representative test input>"
}'
```


The response includes a` conversation_id` . Poll` GET /api/agent_builder/conversations/{conversation_id}` until the` status` field returns` completed` , and then retrieve the full conversation for analysis.


A few guidelines for selecting test inputs:


- Cover the distinct input types your agent handles. For a triage agent, include an endpoint alert, a cloud alert, and a software as a service (SaaS) alert. For a domain-specific agent, cover the alert variants it handles most often.
- Include at least one input where you suspect the current prompt performs poorly (where it runs long, misses steps, or produces variable output). That's the signal the analysis step needs.
- Avoid inputs that your environment closes or suppresses before the agent runs. They produce short, uninformative conversations that don't reflect the agent's actual investigation behavior.


Aim for two to four conversations per agent before the analysis step. You don't need a large sample. You're looking for patterns in behavior, not statistical significance.


Symptom in trace Likely cause Prompt-level fix


` llm_calls` consistently > 12 No concrete stopping criterion Replace text budget with a specific checklist: "After completing \[named steps\], emit your verdict regardless of remaining hypotheses"


Same tool called 2–3× in one investigation Agent requerying to "confirm" results "Never requery a tool whose result is already present in the conversation"


Most LLM calls produce no tool action Excessive reasoning-only LLM calls Specify what the agent should do next at each stage rather than leaving it to deliberate


Returns full document, uses 1–2 fields Missing field projection "You MUST use` KEEP field1, field2` in all ES


Calls` get_mapping` or runs` LIMIT 1` first Agent exploring schema before working List the relevant fields in the prompt directly so discovery is unnecessary


Queries use a long default time range Default range is too wide for the task Specify the window in the instruction: "Restrict login history to the last 24 hours unless instructed otherwise"


Query fails with case-mismatch error Case-sensitive field comparison "Use` LOWER(field) == \\"value\\"` for all name comparisons"


` verification_exception` on query Field used doesn't exist in that index Add an explicit field-to-index mapping in the prompt: "Do not use` field_x` in queries against` index_y` "


Ancestry traces on high-event processes No skip list for noisy processes List processes to log and skip (shells, integrated development environment \[IDEs\], system daemons, build tools) rather than trace further


Conversation ends without a final answer Agent hit an implicit LLM call limit Add near the top of the prompt: "Emit a verdict at the end of your investigation even if some hypotheses remain"


High` input_tokens` on the first LLM call Pre-enrichment context injected into the first message "Do not requery any entity or field already present in the context you received. Cite it; do not restate it."


###


A concrete example: Textual budgets versus stopping checklists


One of the most common and fixable issues is an ineffective budget instruction. Before we applied this loop to one of our forensics agents, the system prompt included the following instruction:


```text
Target 5–10 rounds for this investigation. Hard cap: 12 rounds.
```


In practice, the agent consistently made 14–19 LLM calls on the same class of alert. It found another thing to check at the end of every LLM call, each one individually justifiable. The textual budget was not actionable. There was no mechanism to stop.


We replaced it with a concrete stopping checklist:


```text
Complete your investigation in this order:
1. Retrieve process context for the alerting process
2. Trace one hop of process ancestry
3. Run the entity behavioral correlation check
After completing these three steps, emit your verdict. Do not continue investigating remaining hypotheses.
```


The specific steps will differ for every agent. These are ours. The pattern is the same: a named, ordered list that ends with an explicit "emit verdict" instruction.


The same class of alert that previously took 14-19 LLM calls came in at 7-9 after this change. The prompt didn't get shorter; the stopping criterion got specific enough that the agent could follow it.


###


A second pattern: Redundant queries against enrichment context


A related issue appears in agents that receive a pre-enrichment block at the start of a conversation (a workflow step that injects Elasticsearch Query Language \[ES|QL\] query results before the agent runs). The agent frequently requeries the same entities in subsequent LLM calls because the prompt doesn't say not to.


Adding one explicit rule eliminates this: *"Do not requery any entity or field that is already present in the enrichment context you received at the start of this conversation. Reference that data in your reasoning; do not call a tool to retrieve it again."*


###


Auditing the instructions for ambiguity and contradictions


Beyond trace analysis, it's also worth reviewing the prompt itself for structural issues that don't always show up in conversation traces:


- Logical contradictions between instructions.
- Ambiguous wording that leaves the agent guessing at runtime.
- Excessive nested conditions that increase cognitive load.
- Missing coverage for error cases ( *"What happens if this step fails?"* ).


These issues often explain why a prompt behaves inconsistently even when the conversation trace looks normal. Reading the prompt with those specific questions in mind ( *"Where is this ambiguous?"* , *"Does any instruction contradict another?"* ) surfaces a different class of problems than trace analysis does.


##


Step 4: Revise and verify


Two options exist for producing revised instructions: write the changes directly or use an AI assistant to analyze and rewrite them.


1.


**Write the changes directly.** If the trace analysis points to a small number of clear issues (a missing stopping criterion, a redundant query instruction, a field name typo), edit the system prompt directly. Small, targeted changes are easy to review and understand. They're also easier to isolate as the cause if something regresses.


2.


**Use an AI assistant to analyze and rewrite.** For agents with more complex issues, or where the trace suggests several overlapping problems, pairing with an AI coding assistant can produce a more thorough revision. What matters most is giving the assistant the right inputs: the current instructions, a compact conversation summary, and the optimization guidelines.


The compact summary doesn't need to be the full raw conversation JSON. Extract the number of LLM calls, the tool names and call counts, the approximate payload size of each result, any error messages, and short excerpts from the agent's reasoning steps where those are visible. This keeps the assistant's context small and focused on behavioral patterns rather than raw data.


Here's the analyzer prompt template we use at Elastic InfoSec, adapted for any AI coding assistant or chat interface:


```text
You are analyzing an AI agent's system prompt and recent conversation behavior to identify
inefficiencies and produce a revised version of the instructions.


You will receive:
1. The agent's current instructions
2. A compact conversation summary (LLM calls, tool calls, payload sizes, errors, reasoning excerpts)


Your task: Identify inefficiencies based on the patterns below, then output a full revised
version of the agent's instructions. Output only the revised instructions: no issue list,
no commentary, no wrapper text. The output should be ready to replace the current instructions.


---


Optimization patterns to apply:


1. Excessive data retrieval: If the agent retrieves full documents but uses only 1–2 fields,
add explicit field projections (e.g. KEEP clauses in ES|QL) so only the needed fields return.


2. Redundant lookups: If data retrieved in an early step is re-queried later, add an explicit
rule: "Do not re-query [entity]: use the [entity] value from the earlier step."


3. Post-filtering waste: If many records are fetched then filtered in text, move the filter
into the query.


4. Schema exploration: If the agent calls get_mapping or runs a LIMIT 1 query to discover
available fields, list those fields directly in the instructions so discovery is unnecessary.


5. Textual call budgets: If the instructions say "complete in N rounds" or "target N–M rounds"
(or any vague numeric budget), replace with a named, ordered list of steps to take before
emitting a final answer.


6. Missing final answer: If conversations end without a verdict or conclusion, add near the top
of the instructions: "After completing your investigation, emit a final answer even if some
hypotheses remain uninvestigated."


7. Time window defaults: If queries use a long default range when only recent data is needed,
specify the window: "Restrict [query type] to the last [N] hours unless instructed otherwise."


8. Case-sensitive comparisons: If queries fail on case mismatches, add: "Use case-insensitive
comparisons for all name and string fields."


9. High-volume entity noise: If the agent traces ancestry or queries raw events for known
high-volume processes (common shells, IDE tools, build agents, system services), add a named
skip list with instruction to log and proceed rather than query.


10. Missing output constraints: If the final output includes raw identifiers (UUIDs, entity IDs,
internal index names), add a rule specifying human-readable labels in the final response.


---


Current instructions:
[PASTE CURRENT AGENT INSTRUCTIONS HERE]


Conversation summary:
[PASTE COMPACT SUMMARY HERE]
```


###


How to verify an AI agent optimization actually worked


Apply the revised prompt in a QA environment (not production) and rerun the same test inputs from Step 2. Use the consumption endpoint with a` --since` filter on the post-change window, and compare median` llm_calls` and` total_tokens` against your Step 1 baseline.


Two things to check before promoting to production:


1. The target metrics improved. LLM call count dropped, token count dropped, or token variance narrowed, whichever pattern you were targeting.
2. The output quality held. The agent still reaches a coherent conclusion, covers the expected investigation steps, and doesn’t skip something it was previously doing correctly.


If the output quality regressed while the metrics improved, the revised instructions are too constraining. The stopping checklist is likely cutting off necessary steps. Loosen it and retest before deploying.


> **Note:** See the[Agent Builder documentation](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) for version-specific feature availability, including the consumption API.


##


Step 5: Monitor for drift


Deploying an optimized prompt isn't the end of the process. The inputs an agent receives in the real world evolve. New detection rules fire, data sources change format, and an agent that was well tuned for last quarter's alert mix can drift as the environment around it changes. The same measurement step that established the baseline becomes the monitoring step; run it on a schedule.


Metrics to watch after a prompt change:


Metric Threshold to investigate What it usually means


` avg_total` tokens per conversation >20% above your baseline New tool calls or growing context injections from upstream workflow changes


` avg_llm_calls` per conversation Rising steadily over 1–2 weeks The agent is entering reasoning loops or spending more LLM calls deliberating


` max_total` spike on a single conversation Single outlier well above the median One alert type is triggering a runaway path; pull that conversation and apply Step 3 to it


` llm_calls` variance 2–3× spread reappears after being tight New input variation the current stopping criterion doesn’t handle


A useful pattern for identifying the source of a spike: Run the consumption endpoint with a short` --since` window covering only the elevated period, and then retrieve and summarize the top-N most expensive conversations from that window. One alert type usually dominates. Once you identify the pattern, you have everything you need for another pass through the loop.


A drift signal isn’t a failure. It means new inputs have surfaced a case that the current instructions don’t handle well. Return to Step 2, generate test conversations for the new case type, and run the loop again. Each pass through the loop narrows the gap between what the prompt expects and what the real world sends.


##


The four-part discipline behind agent optimization at scale


The five-step loop is the process we run across all 14 Agent Builder agents in the Elastic InfoSec fleet. It isn't a one-time optimization exercise. It’s closer to a maintenance discipline. Each time a detection rule changes significantly or a new class of alert enters high volume, the agents that handle it are candidates for another pass.


The process isn’t specific to any use case, architecture, or AI model. The core discipline has four parts:


1. Measure before you change anything.
2. Analyze the actual conversation behavior rather than reading the instructions in isolation.
3. Verify improvements before deploying them.
4. Watch the metrics after.


Those habits apply to any agent that runs at production volume.


If you’re running Agent Builder agents in your own environment, start with the consumption endpoint and look at your` llm_calls` counts and run-to-run variance. Those are the most common findings on a first pass, and both are addressable with targeted prompt changes. The measurement step itself usually takes minutes; the analysis and revision follow from what you find.


The[Agent Builder documentation](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) and[Elastic Workflows documentation](https://www.elastic.co/docs/explore-analyze/workflows) are the right starting points if you’re building agents in your environment. If you’re not already running Elastic Security, you can[start a free trial](https://www.elastic.co/cloud/elasticsearch-service/signup) to explore both. The[Elastic Security community forum](https://discuss.elastic.co/c/security) is a good place to share what you find and ask questions.


###


Citations


**Documentation:**


- [Elastic Agent Builder documentation](https://www.elastic.co/guide/en/security/current/agent-builder.html)
- [Elastic Workflows documentation](https://www.elastic.co/guide/en/security/current/workflows.html)
- [Agent Builder consumption API reference](https://www.elastic.co/docs/api/doc/kibana/operation/operation-post-agent-builder-agents-agent-id-consumption)


**Companion posts:**


- [Part 1: How we triage every alert before an analyst opens it](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows)
- [Part 2: Choosing the right agent architecture for a 5× cost reduction](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture)


#### Jump to section


- [Why agent optimization is different from prompt engineering](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale#why-agent-optimization-is-different-from-prompt-engineering)
- [The five-step agent optimization loop](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale#the-five-step-agent-optimization-loop)
- [Step 1: Measure the baseline](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale#step-1-measure-the-baseline)
- [Step 2: Capture representative test conversations](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale#step-2-capture-representative-test-conversations)
- [A concrete example: Textual budgets versus stopping checklists](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale#a-concrete-example-textual-budgets-versus-stopping-checklists)
- [A second pattern: Redundant queries against enrichment context](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale#a-second-pattern-redundant-queries-against-enrichment-context)
- [Auditing the instructions for ambiguity and contradictions](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale#auditing-the-instructions-for-ambiguity-and-contradictions)
- [Step 4: Revise and verify](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale#step-4-revise-and-verify)
- [How to verify an AI agent optimization actually worked](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale#how-to-verify-an-ai-agent-optimization-actually-worked)
- [Step 5: Monitor for drift](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale#step-5-monitor-for-drift)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Inside%20Elastic%20InfoSec%27s%20agentic%20SOC:%20How%20we%20cut%20AI%20agent%20LLM%20calls%20by%2060%&url=https://www.elastic.co/security-labs/ai-agent-optimization-production-scale)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/ai-agent-optimization-production-scale)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/ai-agent-optimization-production-scale&title=Inside%20Elastic%20InfoSec%27s%20agentic%20SOC:%20How%20we%20cut%20AI%20agent%20LLM%20calls%20by%2060%)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/ai-agent-optimization-production-scale&title=Inside%20Elastic%20InfoSec%27s%20agentic%20SOC:%20How%20we%20cut%20AI%20agent%20LLM%20calls%20by%2060%)
