---
schema_version: "1.0.0"
document_id: "e2e635ab5ae0547a92ec3deddaae6090e381ca018bd7bfae5f6d61250bfd0117"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/how-naic-embedded-ai-powered-observability-directly-into-the-ide/"
published_at: "2026-06-12T17:54:29+00:00"
first_seen_at: "2026-07-20T03:31:43.037247+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:793ea53f099366c883625e0470664a5020d003645bdd19fa4686205d64dbe234"
---

# From reactive to proactive: How NAIC embedded AI‑powered observability directly into the IDE

Every developer knows the feeling: You’re in your IDE when something breaks. Error rates spike, alerts fire, and suddenly you’re out of the flow. Michael Kobush, Performance Engineer III at the National Association of Insurance Commissioners (NAIC®), wanted to eliminate the gap between development and runtime. Instead of switching tools or waiting on SRE support, NAIC set out to bring production insight directly into the developer workflow.


Let’s take a look at how NAIC embedded real-time observability directly into their development workflow and reduced investigation time to a few minutes.


## The problem: Context switching kills developer productivity


Developers lose time the moment they leave their IDE, jumping between views of logs, metrics, and traces simply to understand what has changed.


For NAIC, this friction was slowing down their teams. Developers didn’t have access to production context, creating a dependency on SRE teams whenever investigations were needed. An analysis that should have taken minutes routinely took 45 minutes to an hour. Root-cause identification required manual correlation across multiple systems, a process that was neither scalable nor sustainable.


> At Dynatrace Perform 2026, Kobush demonstrated how his team uses Kiro and Dynatrace at NAIC:[Real-time observability in your IDE: How NAIC uses Kiro powers to drive developer productivity](https://www.dynatrace.com/perform/on-demand/perform-2026/?_location=Breakouts&topics=ALL&industries=ALL&jobTitles=ALL&level=ALL&track=ALL&session=real-time-observability-in-your-ide-with-kiro-powers-and-dynatrace)


## The solution: Kiro powers and intelligent observability


Kiro is AWS’s agentic AI-powered IDE that takes a spec-driven approach to software development by turning natural language prompts into structured requirements, architecture designs, and implementation tasks to carry code from prototype to production.


NAIC installed the[Dynatrace power for Kiro](https://kiro.dev/powers/#dynatrace) , one of Kiro’s installable powers that
dynamically connect domain-specific tools and context to the agent. Once connected, Kiro gives developers and AI agents access to Dynatrace data and insights, helping them pinpoint root causes and receive remediation recommendations directly in their workflow.


No switching between tools. No waiting on another team.


## The aha moment: Root-cause analysis in minutes, not hours


The first prompt NAIC ran after connecting Kiro to Dynatrace set the tone for everything that followed. Kobush typed a single line into Kiro: *“Tell me about problem P-18576.”*


Within 30 seconds, Kiro returned a full problem summary with details and recommendations, pulling everything from Dynatrace automatically. Then, he pushed further: *“Give me a really deep dive root-cause analysis of what happened.”*


In under two minutes, Kiro returned a full root-cause analysis correlating telemetry, infrastructure signals, historical incidents, and the current problem from Dynatrace into a structured response that included:


- An executive summary
- Detailed problem context
- Infrastructure analysis
- Technical root-cause analysis
- Remediation strategies
- Conclusions and next steps


A preliminary assessment that would have previously taken 45 minutes to an hour was now done in minutes. More importantly, it wasn’t just faster; it gave the team a clear, connected view of how services, infrastructure, and dependencies contributed to the issue.


## Beyond root cause: Automation across the entire workflow


What makes this more than just a faster diagnostic tool is how NAIC extended Kiro’s capabilities to automate the full incident response workflow.


Using Kiro’s steering files feature, NAIC configured Kiro to automatically generate a structured Markdown file whenever a root-cause analysis was completed. That file includes:


- Relevant DQL queries used during the investigation
- Direct links to the Dynatrace dashboards and data sources that surfaced the issue
- A clear summary of findings


With a Targetprocess MCP also connected, Kiro can take that analysis and populate a ticket directly, automatically loading all relevant context and sending it to the development team. For NAIC, this means the handoff from investigation to remediation is essentially hands-off. This level of automation doesn’t just save time; it creates consistent, repeatable workflows with built-in guardrails. Every incident gets the same structured, data-rich documentation, regardless of who’s investigating it or when.


This isn’t just about faster incident response. It changes how teams build and release software—giving developers immediate feedback on how their changes behave in real environments.


## Proactive alerting: Catching problems before they crash


Root-cause analysis after the fact is valuable. With observability embedded directly into the workflow, teams can detect issues earlier in development and respond faster in production, closing the gap between building and operating software.


After noticing that a specific process had crashed, Kobush asked Kiro to set up an alerting profile that would trigger both before the crash, based on stress signals visible in the logs, and *at the point* of the crash. Kiro analyzed historical log data, identified pre-crash indicators, and built the alert profile automatically.


The result: NAIC’s team now receives early warning signals before a process fails, giving engineers time to intervene rather than react.


This shift from reactive to proactive operations is central to what the Dynatrace and AWS partnership enables. When observability data is embedded in the developer workflow rather than siloed in a separate platform, the entire engineering organization is better equipped to prevent incidents, not just resolve them.


## Debugging a sneaky production bug


Perhaps the most telling story from NAIC’s experience with Kiro occurred during a routine error-rate investigation.


An application error rate had increased unexpectedly. Kobush asked Kiro to investigate. Two minutes later, Kiro identified the culprit: A developer had left debug code in the development environment, and it had made its way into production. Every time a user triggered that code path, it threw errors.


When Kobush sent the Markdown report to the developer, the response was immediate: *“How did you find that? I’ve been looking for that.”*


Kiro leveraged correlated logs, traces, systems context, and historical behavior from Dynatrace to pinpoint exactly where the issue originated.


## Start embedding observability into your development workflow


NAIC’s experience highlights a broader shift: When developers, AI assistants, and systems all operate from the same runtime context, debugging becomes faster, releases become safer, and teams spend less time chasing issues and more time building.


The broader message from Kobush is simple: *“I’m not a developer. I have a degree in biology and a minor in chemistry… But this, to me, is a game changer in the observability space. I can do things in seconds that would take me hours.”*


The productivity gap between observability data and developer action is a solvable problem.


For DevOps engineers, SREs, and platform teams looking to accelerate incident resolution, reduce context switching, and move from reactive troubleshooting to proactive operations, the Dynatrace and Kiro integration offers a practical, immediately actionable path forward.


For developers, this means fewer interruptions, faster answers, and the ability to stay in flow, even when issues arise.


For more information on how Dynatrace and AWS work together, and to access integration best practices, read our guide,[Master AI Observability](https://www.dynatrace.com/info/whitepapers/master-ai-observability/) , or come and see us at an[AWS Summit near you](https://www.dynatrace.com/about/events/dynatrace-at-aws-summits/) .
