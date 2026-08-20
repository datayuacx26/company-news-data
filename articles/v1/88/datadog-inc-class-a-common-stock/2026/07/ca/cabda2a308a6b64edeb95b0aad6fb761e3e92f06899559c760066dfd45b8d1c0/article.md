---
schema_version: "1.0.0"
document_id: "cabda2a308a6b64edeb95b0aad6fb761e3e92f06899559c760066dfd45b8d1c0"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/creating-mcp-tools-for-cloud-siem/"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:b3452cd26674d51e680212e2bcf2adde402ee8d480a19cc6ca59327037ba54a3"
---

# How we brought agentic workflows to Cloud SIEM with the Datadog MCP Server

Chelsea Xu


Software Engineer


Eddie Cai


Technical Content Writer


Romain Kirszbaum


Software Engineer


Mohamed Hachem Ouertani


Software Engineer


Security engineers using[Cloud SIEM](https://www.datadoghq.com/product/cloud-siem/) spend their day-to-day investigating signals, tuning detection rules, managing suppressions, running historical jobs across interconnected workflows, and more. Agents are becoming a practical way to navigate that complexity, and we built a set of security tools for the[Datadog MCP Server](https://docs.datadoghq.com/mcp_server/) to support them.


Cloud SIEM is only one part of a broader cloud security ecosystem, so the Security MCP toolset has to grow across many teams and products. That growth comes with a constraint. The tools share a single context window, so every new tool consumes more of that window. Add enough tools and the agent can lose track of which one fits the task in front of it.


We built these tools under that constraint, and we’ll cover what we learned in this post. We’ll show how we scoped tools around what users already do and managed the shared context window with progressive disclosure. We’ll also cover how we tested non-deterministic agent behavior with a custom eval framework and governed a growing multi-team toolset.


## Scope MCP tools around what users already do


Deciding what to build first is challenging for a product as broad as Cloud SIEM. Assumptions about what an agent should do can send you in the wrong direction and waste time and resources on developing tools that may prove unhelpful.


Before building any tools, we analyzed API call patterns and[Real User Monitoring (RUM)](https://www.datadoghq.com/product/real-user-monitoring/) data to find where users already spent time and where they hit friction. We also reviewed[Bits Chat](https://docs.datadoghq.com/bits_ai/bits_chat/) logs to see what people asked for directly.


A few patterns stood out. Detection rule authoring was among the most common. Of the sample pool we analyzed, 44% of messages involved authoring and editing detection rules, and users often asked the agent to assess a rule and even name it:


> *Can we read the security signal I have open? Tell me: What exactly fired and why (rule name, detection method, what was new/anomalous) with Who/what triggered it (user, account, IP, user agent, geo)?*


Signal investigation was a distinct pattern. Users regularly pasted raw signal JSON into the chat to understand what a signal meant and what to do about it. That told us an agent needs direct access to a signal’s full contents, not just a summary, so we built a tool that returns a signal’s full payload.


Another pattern was bulk triage. Users wanted the agent to take triage actions for them, such as assigning a signal, closing it with a verdict like benign or malicious, and leaving a note for whoever looks next:


> *Assign the signal to me, and close it as a benign true positive.*


They also wanted to do this at scale:


> *Mark all those signals as archived against these IPs.*


The Cloud SIEM UI lets you manage up to 50 signals at a time, and we found users working through signals in consecutive batches of 50 to get past that ceiling. About 25% of customers triaged signals in bulk, and 14% pushed right up against the 50-signal cap.


One session showed how far past the cap the demand could go. Over a 9-minute window, a single customer fired seven bulk triage calls back to back. Each of these seven came back at exactly 52,366 bytes, roughly 50 signals each.


Time (UTC) Response Gap


17:34:48 52,366 B 53s


17:36:06 52,366 B 78s


17:37:06 52,366 B 60s


17:38:25 52,366 B 79s


17:39:07 52,366 B 42s


17:41:22 52,366 B 135s


17:42:41 52,366 B 79s


A response that size meant that the user was maxing out the 50-signal cap repeatedly, triaging more than 350 signals in one sitting. Users wanted to act on more than 50 signals at once, and an agent was a natural way to achieve that.


## Keep the shared context window lean


Scoping the work still left the harder problem of managing shared context. The Security MCP toolset started with tools for querying security findings. As it grew to cover Cloud SIEM,[Code Security](https://docs.datadoghq.com/security/code_security/) ,[App and API Protection](https://www.datadoghq.com/product/app-and-api-protection/) , and more, the shared context window grew with it. A larger context window raises the risk of confusing the agent. We didn’t want an agent investigating App and API Protection signals when a user asked about Cloud SIEM.


We built more than a dozen security tools in all, and two of them best illustrate the context problem. The first is a schema tool that teaches the agent how to author a detection rule, giving it the fields and grammar a valid rule needs. The second is a triage tool for acting on signals in bulk.


The principle behind both is the same. Give the agent only what a task needs, and keep everything else out of the context window. The schema tool applies it through progressive disclosure, and the triage tool resolves signal IDs from a query instead of carrying them in context.


### Return only the schema a task needs


Detection rule creation made the problem concrete. The schema tool carries rule schemas for several products, including Cloud SIEM,[Workload Protection](https://www.datadoghq.com/product/workload-protection/) , and App and API Protection, each with its own fields, options, and permission models. Serving everything at once fills the context window with detail most tasks never use.


We designed a schema tool that uses progressive disclosure. Instead of serving every product’s schema upfront, the tool returns what a task actually needs.


The tool breaks the schema into named sections, such as top-level fields, query syntax, options, and examples. It also accepts filters for rule type and detection method, and it can drop any sections that aren’t needed. Leave the filters out and the tool returns everything. Pass them and it returns only the matching slice.


Take a request to build a brute-force detection rule:


> *I want to create a Cloud SIEM detection rule for brute-force attempts. Trigger it when the same source IP makes an unusual number of failed login attempts to our login or auth API endpoints within 5 minutes. The traffic comes from our nginx API gateway logs in production.*


The agent doesn’t pull the full catalog. It leaves the section list unset, so the tool returns every section, but the rule type and detection methods it specifies filter the contents down to what the task needs.


```text
1   {    2       "telemetry"  : {    3         "intent"  :   "Fetch detection rule schema/grammar before authoring a log_detection brute-force rule for nginx auth endpoints"    4        },    5       "rule_types"  : [    6         "log_detection"    7        ],    8       "detection_methods"  : [    9         "threshold"  ,    10         "anomaly_detection"    11        ]    12   }
```


The savings depend on how narrowly the agent can scope its request. Each security product owns its own rule types, so filtering to a single rule type cut token usage by about 15% in our testing. Filtering further by detection method, as in the request above, brought the reduction to 41–47%.


### Triage hundreds of signals from a single query


Bulk triage needed its own tool. Our first version accepted a list of signal IDs and called the batch triage endpoint in a loop until every signal was updated. The approach worked, but two problems surfaced as we tested it.


Signal IDs are long strings, and every ID counted toward the tool’s input tokens. Passing hundreds of them meant loading a large block of text into context on every call. Including each ID was also slow for the agent, which added latency on top of the token cost.


We redesigned the tool to accept a search query instead of a list of IDs. The tool runs the query, fetches the matching signal IDs itself, and triages them in batches until it finishes. The IDs never enter the agent’s context, so input token cost stays flat no matter how many signals the query matches. The tool now handles up to 500 signals in a single call.


Before the redesign, updating 50 signals took at least 2 minutes. Afterwards, updating hundreds of signals took a little over a minute.


## Test non-deterministic agent behavior with a custom eval framework


Careful scoping improved tool selection, but it didn’t prove the tools worked. Agent behavior is non-deterministic, and the technology is young enough that no checklist can sign off a tool as production-ready. Traditional tests assume the same input produces the same output, and an agent doesn’t offer that guarantee. We needed a way to verify tools worked reliably and didn’t break existing ones.


We built a custom eval runner and integrated it with the AI Platform team’s continuous integration (CI) eval runner. When anyone modifies the toolset, the runner automatically runs evals across all security tools. Any regression in the baseline eval score greater than 5% is flagged. The runner also flags any tool that ships without eval coverage, which became one of our first governance mechanisms. Contributors can’t quietly add tools that go untested.


### Make the evals meaningful and cost effective to run


An eval is only as useful as the behavior it measures. We check whether the agent called the tools it should, avoided the ones it shouldn’t, used a sensible number of tools, passed correct arguments, and followed a reasonable trajectory to the answer. A weighted score rolls these checks up, and we give tool selection accuracy the most weight, because the agent picking the right tool from a large toolset is our top priority.


We also run each scenario several times and average the results, which reduces the noise that non-deterministic behavior introduces. Each eval generates an experiment in Agent Observability that shows where a failure occurred, so a drop in the score comes with the detail needed to fix it. We targeted a baseline success rate of about 80%, which leaves room for the occasional hallucination without masking real regressions.


Running scenarios repeatedly gets expensive, so the runner mocks tool responses. Instead of calling a real tool and pulling back a long response, the eval uses stubbed data. Mocking cuts output token cost, and the savings compound every time we rerun a scenario to average out noise.


## Govern a growing multi-team toolset


Automated evals caught regressions in a single tool, but quality across the whole toolset became a coordination problem as more teams contributed. Without shared standards, a new tool could degrade the ones already serving customers.


We set up a lightweight governance committee designed around a self-service model. The reason is development velocity. Unlike deterministic features, MCP tools improve mainly through rapid iteration and heavy dogfooding against real use cases, so a heavy approval process would slow the exact feedback loop that makes them better. Every tool stays self-service through the stages before general availability (GA), so contributors ship and iterate without waiting on review. Contributors are responsible for meeting the established standards, such as a minimum eval score, before a tool reaches customers.


To make those standards clear and easy to understand, the committee maintains shared infrastructure and develops best practices that any team can reuse. The committee’s main concern is whether a new tool degrades the toolset already in production.


## Key takeaways for building MCP tools in complex domains


Building MCP tools for Cloud SIEM showed us that real user behavior, not assumptions, should decide what you build first. Real agent behavior should decide what you refine next. Progressive disclosure keeps a shared context window manageable, custom evals keep non-deterministic tools reliable, and light governance keeps a multi-team toolset coherent without slowing anyone down.


Another lesson is that no checklist marks an MCP tool as done. Building production-quality tools in a complex domain is iterative, and the most reliable signal is watching how agents and users behave together in practice. To learn more about what workflows our MCP Server supports, see our[Cloud SIEM documentation](https://docs.datadoghq.com/security/cloud_siem/) .


If you’re not already a Datadog customer, you cansign up for a free 14-day trial to get started with Cloud SIEM.


-
-
-
