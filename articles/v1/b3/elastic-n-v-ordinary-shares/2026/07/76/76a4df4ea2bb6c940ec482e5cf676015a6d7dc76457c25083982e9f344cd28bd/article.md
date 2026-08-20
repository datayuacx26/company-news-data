---
schema_version: "1.0.0"
document_id: "76a4df4ea2bb6c940ec482e5cf676015a6d7dc76457c25083982e9f344cd28bd"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T16:09:01.006563+00:00"
fetched_at: "2026-07-28T20:33:07.280239+00:00"
content_hash: "sha256:71f56ff319426530500223939980ac6b332fecb1ce83d3e3082d06dc0562d64d"
---

# Inside Elastic InfoSec's agentic SOC: When to inline your agent's skills for a 5× cost reduction

24 July 2026 •


[Aaron Jewitt](https://www.elastic.co/security-labs/author/aaron-jewitt)


# Inside Elastic InfoSec's agentic SOC: When to inline your agent's skills for a 5× cost reduction


We tested two agentic SOC architectures in parallel across 36,822 real Agent Builder conversations. One won by 5.7x: a specialized workflow triaging alerts for $0.69 each, against $3.42 for a single agent juggling 14 Skills. The data and the decision framework are both below.


10 min read


[Generative AI](https://www.elastic.co/security-labs/category/generative-ai) ,


[Detection Engineering](https://www.elastic.co/security-labs/category/detection-engineering)


This is Part 2 of the **Inside Elastic InfoSec's Agentic SOC** series.[Part 1: How we triage every alert before an analyst opens it](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows) .[Part 3: how we cut AI agent LLM calls by 60%](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale) .


Investigating a Windows endpoint alert in Elastic InfoSec's production agentic security operations center (SOC) costs $0.69. That's what we pay running an orchestration workflow of specialized[Elastic AI agents](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) on the[Elastic Inference Service](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) (EIS). Route the same alert to a single agent working through 14[skills](https://www.elastic.co/security-labs/skills-elastic-security-9-4) , and the bill jumps to $3.42, 5.7x more. At 100 investigations a day, that's an $8,000 monthly gap, and we didn't get it from a lab. It came out of 36,822 real[Elastic Agent Builder](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) conversations running in our own production environment.


The gap comes down to how you build the SOC in the first place. Give one broad agent a library of skills, and it loads whatever it needs on the fly. Build a fleet of specialized agents instead, and each one runs a fixed methodology through an orchestration layer. Agent Builder handles either setup fine. At our volume, though, running the unoptimized configuration for batch triage is exactly what turns into that $8,000 a month. We'll walk through why the gap opens up, when each architecture earns its keep, and how you can run this same comparison on your own alerts.


##


Multiple specialized agents versus a single agent with skills


The **single agent with skills** is one broad agent paired with a library of[Agent Builder skills](https://www.elastic.co/security-labs/skills-elastic-security-9-4) . The agent has a thin system prompt that describes its general purpose and lists 14 skills it can invoke: macOS forensics, Windows forensics, AWS CloudTrail, Okta investigation, and others. When a new alert or analyst question arrives, the agent decides which skills are relevant, loads them on demand, and reasons over the result. No routing layer, no separate agents. One agent, one context window, one conversation.


The single-agent approach is also significantly simpler to build. For teams that aren’t yet ready to invest in a full multi-agent workflow, it’s a practical starting point: Deploy a single agent with skills, scope it to critical severity alerts only, and get agentic investigation coverage running quickly. As your team builds familiarity with Agent Builder and capacity to maintain specialized agents, you can graduate your highest-volume investigation types into the specialized workflow, while the skills agent remains the front door for everything else.


Skills aren’t inefficient. They’re loaded on demand, which is exactly what you want when a human analyst is exploring an alert and may need to pivot in unexpected directions. An analyst who starts with macOS forensics, discovers a lateral movement indicator, and needs to pull in the Okta investigation skill next benefits from that on-demand loading. It’s the right behavior for a conversation-driven workflow.


The **specialized agent workflow** is built around a deterministic orchestration layer and a fleet of specialized agents. An[Elastic workflow](https://www.elastic.co/docs/explore-analyze/workflows) fires when an alert is generated. It enriches the alert with data from 15 or more sources using[Elasticsearch Query Language (ES|QL)](https://www.elastic.co/guide/en/elasticsearch/reference/current/esql.html) queries, runs infrastructure checks that close low-risk alerts with no AI cost, and routes the surviving alert to an initial triage agent that makes a first-pass verdict.


If the initial triage agent is uncertain, the workflow opens a[Kibana case](https://www.elastic.co/guide/en/kibana/current/cases.html) and dispatches a set of specialized agents, each scoped to one domain. The macOS forensics agent knows exactly which tools to use, in what order, with what stop criteria. That methodology is written directly into its system prompt. It doesn’t browse a library of methodologies at runtime; it runs one methodology, deterministically, every time. A Final Review agent reads the findings from all the specialized agents and writes the analyst-facing verdict.


For the full pipeline walkthrough, see our companion post[Part 1: How we triage every alert before an analyst opens it](https://www.elastic.co/security-labs/alert-triage-agentic-soc-elastic-workflows) .


Both architectures use the same underlying platform:[Agent Builder](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) for agent construction and deployment,[Elastic Workflows](https://www.google.com/url?q=https://www.elastic.co/docs/explore-analyze/workflows&sa=D&source=docs&ust=1782935035485041&usg=AOvVaw11wn4xGkAgk8qgVHeH8wyu) for orchestration in the specialized workflow, and[Elastic Inference Service](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) (EIS) for large language model (LLM) inference. The difference is where investigation methodology lives: written inline across many specialized agents, or loaded on demand into one general agent.


##


Customer Zero: How Elastic InfoSec runs Agent Builder in its own production SOC


At Elastic, our InfoSec team operates as Customer Zero. We run the newest versions of[Elastic Security](https://www.elastic.co/guide/en/security/current/) and Agent Builder in our production environment, often before they reach general availability, across a globally distributed fleet of laptops, servers, and cloud workloads. We’re the first and most demanding user of every feature we ship.


The numbers in this post aren’t a benchmark we built for the blog. They come from 36,822 real conversations across our production and QA Agent Builder deployments, totaling about 8 billion tokens. Roughly 99.3% of all agent executions ran on Claude Sonnet 4.5 via EIS. The architectural question we answer here is one we had to answer ourselves, as our monthly EIS bill started to climb quickly.


##


How do you measure per-agent token cost in Agent Builder?


Agent Builder exposes a[consumption endpoint](https://www.elastic.co/docs/api/doc/kibana/operation/operation-post-agent-builder-agents-agent-id-consumption) that returns token usage by agent over any time range:


```text
curl -X POST \
-H "Authorization: ApiKey ${KIBANA_API_KEY}" \
-H 'kbn-xsrf: true' \
-H 'Content-Type: application/json' \
"${KIBANA_URL}/s/${KIBANA_SPACE}/api/agent_builder/agents/${AGENT_ID}/consumption" \
-d '{"from":"2026-04-01T00:00:00Z","to":"2026-05-01T00:00:00Z"}'
```


Replace` ${KIBANA_URL}` ,` ${KIBANA_API_KEY}` ,` ${KIBANA_SPACE}` , and` ${AGENT_ID}` with your Kibana URL, API key, space name, and target agent ID.


The response includes:


- ` conversations` : Total conversation count in the range.
- ` tokens.input` : Total input tokens consumed.
- ` tokens.output` : Total output tokens consumed.
- Per-model breakdown, so you can verify which model is actually in use.
- The time range echoed back for confirmation.


The API returns totals and statistical summaries (including median) for the period. It doesn’t return per-conversation traces. That makes it straightforward to track fleet-level costs over time, but harder to measure what a single investigation actually costs. To close that gap, we ran matched-live experiments: the same alert, submitted to both architectures in sequence, with the output of each run recorded independently.


Per-investigation cost for the specialized workflow is a composed estimate, not a single call measurement. Each specialized agent runs in its own context. We sum the median token counts of the specialized agents involved in a route, plus the Final Review agent, to get the per-route median. These route estimates are consistent with our matched-live Windows and[Higher-Order](https://www.elastic.co/security-labs/higher-order-detection-rules) threshold runs.


##


Token cost by investigation route


Specialized agents columns use median per-agent token counts from the consumption API, summed across the agents in each route (hundreds to thousands of conversations per agent). Single agent with on-demand skills columns show the average tokens used across our matched runs.


Investigation route Specialized agents Single agent Token ratio


Endpoint Windows ~113k ~649k **5.7×**


Higher-Order threshold alert ~243k ~722k **~3.0×**


The Windows and Higher-Order threshold pairs are matched-live measurements: the same alert submitted to both architectures. The specialized workflow route totals are composed estimates (sum of per-agent medians) consistent with those matched runs.


At[EIS](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) rates for Claude Sonnet 4.5, those token counts translate directly to dollars. Current rates are on the[EIS pricing page](https://cloud.elastic.co/cloud-pricing-table) . Cost includes EIS inference charges plus Elastic Agent Builder execution metering ($0.025 per execution on Serverless; each 50,000 input tokens count as one additional execution beyond the base interaction).


Investigation route Specialized agents cost Single agent cost Savings per investigation


Endpoint Windows $0.69 $3.42 **$2.73**


Higher-Order threshold alert $1.48 $3.82 **$2.34**


At scale, that per-investigation difference compounds quickly. Monthly figures below extrapolate from the Windows-route cost ($0.69 specialized versus $3.42 single agent); replace with your own per-route cost to estimate your spend.


Daily volume Specialized agents / month Single agent / month Monthly savings


100 / day $2,070 $10,260 **$8,190**


500 / day $10,350 $51,300 **$40,950**


1,000 / day $20,700 $102,600 **$81,900**


The single-agent token counts vary substantially across runs: from 218k to 786k in our sample of five unified-agent investigations. That variance is itself a signal. As the matched test in the next section shows, even the same alert investigated by the same agent can take different paths, depending on how many skills get invoked and how many reasoning-only LLM calls the agent makes before committing to a tool call.


##


Inline methodology versus a skill-delegated agent: A matched test


To isolate the effect of skills specifically, we ran a tighter experiment. We picked four macOS alerts that only required a single skill to investigate and sent each one to both architectures: the macOS forensics agent from the specialized workflow (methodology inline), and the same single agent with 14 skills. The numbers below are averages across the four matched runs per architecture.


Metric Specialized agent (inline methodology) Test agent (skill-delegated)


LLM calls 4 12


Total tokens 43,333 346,767


EIS cost (Claude Sonnet 4.5) $0.23 $1.65


Wall-clock time 41 seconds 148 seconds


Process ancestry traced 100% (4 of 4) 50% (2 of 4)


Reasoning-only LLM calls ~25% ~57–60%


The inline agent uses 8× fewer tokens and 3.6× less wall-clock time, and it ran identically across all four runs. The skill-delegated agent followed a different investigation path on each run. Two of the four runs traced process ancestry correctly; the other two took a cheaper path that skipped that step and produced a shallower result. Same setup, different paths, driven by nondeterministic tool selection.


The reasoning-only LLM call rate explains a large portion of the cost difference. About 57–60% of the skill-delegated agent's LLM calls were pure deliberation, with no new tools called and no new evidence gathered. Those LLM calls still pay the full, growing conversation-history cost. The inline agent spent only 25% of its LLM calls in reasoning, because the prompt told it what to do next.


> **Disclaimer:** The macOS matched test used four matched alerts, one run per architecture per alert. Sample size is small. The Windows and Higher-Order threshold observations used five unified-agent runs versus hundreds to thousands of specialized agent runs. These results are internally consistent, but a larger controlled experiment would tighten the confidence intervals. Treat the ratios as directionally correct, not laboratory precision.


##


Why does on-demand skill loading cost more at scale?


We found during testing that the biggest predictor of total token usage in an agentic investigation is the number of LLM calls the agent makes, not the size of its system prompt. Every LLM call pays the full, growing conversation-history cost, so each extra deliberation step multiplies the bytes already in play. In our investigations, average input tokens per LLM call held steady at roughly 36,000 tokens, regardless of system prompt size, with the growing conversation history doing most of the work. This produces a counterintuitive result: A longer, more detailed system prompt often reduces total token cost, because spelling out the methodology eliminates the LLM calls the agent would otherwise spend deciding what to do next. That insight is the main reason a workflow of specialized agents costs less at scale than a single agent with skills. The optimization workflow we use to identify and reduce these reasoning-only LLM calls in production agents is covered in[Part 3](https://www.elastic.co/security-labs/ai-agent-optimization-production-scale) .


Skills in Agent Builder are loaded on demand, not pre-injected into the agent's context. When an agent invokes a skill, it reads the skill file at runtime. That read costs one LLM call and adds the skill's content (typically 600–1,500 tokens) to the conversation context, where it stays for every subsequent LLM call.


If an investigation requires three skills, the agent pays three LLM calls just to load the skills before any forensic work begins. Those skill bytes sit in the growing context window for every remaining LLM call, including all the reasoning-only LLM calls that follow. The result is compound cost: load overhead up front, plus heavier context on every LLM call that comes after.


The problem is when that same flexibility runs hundreds of times a day on the same class of alert. Automated endpoint triage on macOS endpoints always follows the same path:


1. Check the alert.
2. Trace the process ancestry.
3. Run two ES|QL queries.
4. Write the verdict.


There’s no exploration. The flexibility is overhead you pay for without using, every single time.


Writing the methodology inline eliminates the load step. More importantly, it eliminates the deliberation. The model doesn’t need to reason about which tool to pick when the prompt tells it: Use` execute_esql` on` kibana.alert.uuid` , and then call` endpoint.process_entity_id` once or twice, stop. That constraint is why the inline agent runs four LLM calls and the skill-delegated agent runs 12.


##


How to pick your agentic SOC architecture: A decision framework


Which architecture to pick should follow from what you’re trying to do with each part of your SOC.


Need Specialized agent workflow Single agent with skills


Automated triage at hundreds of alerts per day Right tool. 3–5.7× cheaper per investigation; consistent depth Higher cost at scale; variance in depth across runs


Forensic-depth reproducibility on identical input 100% process ancestry traced (4 of 4 runs) 50% process ancestry traced (2 of 4 runs); different paths across runs


Token cost per investigation ~113k–243k, depending on route ~532k–786k across our five observed unified-agent runs


Interactive analyst chat over one alert Mismatched for this use case; routing and specialization add friction where flexibility helps Right tool. Analyst can steer; skills load on demand as the conversation evolves *(experiential, not yet measured at scale)*


Time to add a new domain Build a new narrow agent; update the workflow to add logic and error handling for the new agent Author one new skill; existing agent picks it up immediately


Methodology change ergonomics Edit each agent's system prompt Edit one skill file; every agent that invokes it picks up the change


Observability of why a decision happened Linear and predictable: enrichment, specialized agent finding, Final Review verdict Variable: Skill choices at runtime determine the path


**Run both** when your SOC does both things. These architectures aren’t mutually exclusive, and they can coexist in the same Agent Builder deployment. Automated batch triage runs on the specialized workflow. Analyst-led interactive investigation runs on the single-agent approach. Different jobs, different shapes.


**Use the specialized workflow** when you automate the same investigation type repeatedly and need cost control, reproducibility, and auditability. Alert triage running hundreds of times a day on the same rule class is the canonical case. Maintaining one agent per domain adds overhead compared to a single agent, but the cost savings at scale offset that quickly. At 500 endpoint investigations per day, a 5× cost difference isn’t a rounding error.


**Use the single agent with skills** when the investigation is analyst-led and the direction may shift mid-conversation. On-demand skill loading is a feature in that context, not a cost. The analyst can start with macOS forensics, discover an anomalous Okta login, and pivot to identity investigation without switching interfaces or writing a new query.


**Measure before you decide.** The per-agent consumption API makes this tractable even before you commit to a design. Deploy both approaches in a QA environment, run each against the same set of representative alerts, and sum the median token costs by route. Your numbers will differ from ours, depending on your alert mix, your methodology depth, and which models your connectors use. But the measurement approach is the same.


##


What Elastic InfoSec runs in production today


The specialized agent workflow runs in production for automated alert triage at Elastic InfoSec. Every alert from our detection rules passes through the workflow, gets enriched by ES|QL queries, and routes to the appropriate specialized agents before an analyst opens it. For the full pipeline walkthrough, see[Part 1: How we triage every alert before an analyst opens it](http://link_tbd/) .


The single agent with skills is available in our environment for analyst-led investigation. It handles conversational pivots and follow-on questions in ways the specialized workflow does not, and it gives the analyst flexibility to investigate a single alert, work across multiple domains in one session, hunt threats using indicators, or generate an executive summary for a case.


##


Specialized agents versus skills: The bottom line for your agentic SOC


For agentic automations that you run hundreds of times a day, building specialized agents with inlined skills cuts token cost 3–5.7x, increases efficiency, and improves the consistency of your analysis. For human-led, exploratory, cross-domain work the skills-based agent is the right shape, and it’s the easiest way to get started.


The broader principle is worth keeping as you design your SOC: Match architecture to use case, and measure before you assume. An architecture that works well for one part of your SOC may be the wrong shape for another. The consumption API gives you the data to make that call on your own alerts, with your own agent configurations, rather than relying on numbers from a different environment.


If you’re standing up an agentic SOC on Elastic, start with the[Agent Builder documentation](https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder) and the[Workflows documentation](https://www.elastic.co/docs/explore-analyze/workflows) . Run the consumption API against your own deployments, and tell us what you find.


#### Jump to section


- [Multiple specialized agents versus a single agent with skills](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture#multiple-specialized-agents-versus-a-single-agent-with-skills)
- [Customer Zero: How Elastic InfoSec runs Agent Builder in its own production SOC](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture#customer-zero-how-elastic-infosec-runs-agent-builder-in-its-own-production-soc)
- [How do you measure per-agent token cost in Agent Builder?](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture#how-do-you-measure-per-agent-token-cost-in-agent-builder)
- [Token cost by investigation route](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture#token-cost-by-investigation-route)
- [Inline methodology versus a skill-delegated agent: A matched test](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture#inline-methodology-versus-a-skill-delegated-agent-a-matched-test)
- [Why does on-demand skill loading cost more at scale?](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture#why-does-on-demand-skill-loading-cost-more-at-scale)
- [How to pick your agentic SOC architecture: A decision framework](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture#how-to-pick-your-agentic-soc-architecture-a-decision-framework)
- [What Elastic InfoSec runs in production today](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture#what-elastic-infosec-runs-in-production-today)
- [Specialized agents versus skills: The bottom line for your agentic SOC](https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture#specialized-agents-versus-skills-the-bottom-line-for-your-agentic-soc)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Inside%20Elastic%20InfoSec%27s%20agentic%20SOC:%20When%20to%20inline%20your%20agent%27s%20skills%20for%20a%205%C3%97%20cost%20reduction&url=https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture&title=Inside%20Elastic%20InfoSec%27s%20agentic%20SOC:%20When%20to%20inline%20your%20agent%27s%20skills%20for%20a%205%C3%97%20cost%20reduction)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/agentic-soc-token-budget-architecture&title=Inside%20Elastic%20InfoSec%27s%20agentic%20SOC:%20When%20to%20inline%20your%20agent%27s%20skills%20for%20a%205%C3%97%20cost%20reduction)
