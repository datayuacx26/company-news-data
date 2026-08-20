---
schema_version: "1.0.0"
document_id: "d81a3880a0387d3954442913f7592670d569287d383c5459a5240d780b959332"
company_key: "yc-kaelio"
company: "Kaelio"
source_id: "yc-kaelio-news-import-d43a8938b457"
canonical_url: "https://www.kaelio.com/blog/who-governs-the-agents-context-layer-outside-your-warehouse"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-24T01:10:57.591835+00:00"
fetched_at: "2026-07-28T21:46:31.447870+00:00"
content_hash: "sha256:587ba251fb52d8a207eab7d5e3389d7f6a48015476bbd863ed4cd423b97604c5"
---

# Who Governs the Agents? Why AI Analytics Needs a Context Layer Outside Your Warehouse

*Dashboards came with guardrails built in. Agents don't. Here is why the layer that governs them works best sitting alongside your whole stack, not just inside the warehouse.*


A lot of data teams are in the same spot right now. The business has realized it can point Claude, ChatGPT, Gemini, or a BI copilot at the warehouse and ask questions in plain language, and the early results are good enough that people want to roll it out widely. Self-serve analytics without the usual BI backlog is a genuinely appealing idea. Somewhere in that conversation, a governance lead usually raises a quieter question that is easy to skip past: who decides what these agents are allowed to know, and whether the answers they give are the approved ones?


For a lot of teams this is not urgent yet. It is the kind of thing you can see coming. At some point a non-technical user asks an agent for "active customers this quarter" or "revenue from at-risk accounts" and gets a confident number that does not match what the analytics team would have produced. The agent did not malfunction. It answered with whatever definition it could find, because nobody had decided which definition it was supposed to use.


This post is about why that problem does not live where most teams first look for it, and why the layer that fixes it works best when it sits outside the warehouse rather than inside it.


## The problems are not new. The consumer is.


Think about the issues you already manage. The same metric defined 3 ways across 3 tools. Documentation written by dozens of people over many years, some of it thorough and some of it a single line. Lineage that goes dark as soon as you step upstream of the warehouse. Sensitive fields classified carefully in one place and exposed casually in another. None of this is new, and you have been managing it for years, partly because the people consuming that context apply judgment. An experienced analyst can tell when a definition looks out of date and quietly works around it.


An agent does not do that. It has no instinct for which number is the trustworthy one, no colleague to check with mid-question, and no sense that a definition looks suspiciously old. It uses the context it can find and produces an answer. And when the context is missing, an agent often does not stop to ask. It invents a plausible definition and answers anyway, which is the more dangerous case, because a made-up definition that looks reasonable is harder to catch than a blank. An out-of-date definition is just as bad: the agent has no reason to doubt it, so it runs with it.


So the gaps do not disappear when you add agents. They become more visible, because a person was absorbing them before and now a literal-minded system is surfacing them. We wrote more on that side in[how to govern AI agent access to business metrics](https://www.kaelio.com/blog/how-to-govern-ai-agent-access-to-business-metrics) and[why semantic layers alone won't stop AI hallucinations](https://www.kaelio.com/blog/why-semantic-layers-alone-wont-stop-ai-hallucinations) .


## The problem was never the semantic layer


A semantic layer does exactly what an agent needs. When an agent or BI tool maps a question like "monthly revenue" to a modeled metric, Cube, dbt MetricFlow, or LookML turns it into canonical, dialect-correct SQL with the joins handled for you. Canonical metrics and safe queries are the whole point, and they are worth having. The trouble is not the idea. It is two things the hand-built versions leave on the table.


The first is upkeep. A modeled metric stays correct only while someone maintains it, and a hand-built layer is slow to keep current: models, joins, and dimensions are written by hand and do not learn from the warehouse, BI tools, or query history around them. So definitions drift. A person notices a number that looks off and works around it. An agent cannot tell a definition written this quarter from one that went stale a year ago, so it runs both with the same confidence.


The second is everything that is not a formula. A semantic layer holds how a metric is computed. It does not hold the why: that a metric excludes a region for a policy reason, that a table was deprecated last quarter, what a field in the CRM actually means, which of two sources finance treats as the real one. That context is most of what makes an answer trustworthy, and it lives in docs, Confluence, Notion, BI tools, tickets, and people's heads, not in the formula.


This is exactly why you run a data catalog. A catalog sits horizontally across the stack, including the source systems upstream of the warehouse, and a mature one does real governance work: classification, lineage, stewardship, sometimes masking and access workflows. The semantic layer never describes that much surface. But even an active catalog is not, by itself, an executable context runtime for an agent. It documents and governs metadata; it does not hand the agent a runnable, approved definition or apply a masking rule to the SQL the agent just wrote. An agent needs the catalog's breadth made executable: an approved definition it can run, and policy enforced on its query, not just described.


A semantic layer provides the metric formula. A governed context layer keeps every category current and governs all of it.


So "outside the warehouse" is not a claim about where data is computed. It is about scope and ownership. The layer that governs agents needs the breadth your catalog already has, made executable, and it should not be confined to one vendor's warehouse-and-metrics tooling. To be clear, sitting outside does not mean bypassing your warehouse: the context layer is a control and context plane, and it still enforces policy through the warehouse, BI layer, identity provider, or query gateway you already trust. We compared those shapes in[data catalog vs. context layer](https://www.kaelio.com/blog/data-catalog-vs-context-layer-what-ai-data-agents-actually-need) .


## Definitions rarely have a clear owner


There is a second issue, and it is the one governance teams tend to react to most. Plenty of teams have real engineering rigor around the modeling layer: pull-request review, dbt CI, LookML validation, ownership metadata, controlled deploys. That rigor is worth having. But it checks whether the code is sound, not whether the business agreed with the number. A definition can pass every technical gate and still have no business owner who signed off that this is how the company counts revenue. So "who approved this metric?" usually has a clean engineering answer and a blank business one, and the trust signal an agent would need, that finance or the metric owner actually stands behind it, is the one that is missing.


That gap was survivable when the consumers were people who apply judgment. It is harder to justify when the consumer is an agent that treats whatever definition it finds as correct and uses it. The problem gets sharper when definitions conflict: the same "weekly revenue" defined one way in a modeling file and another way in a BI tool, both plausible, both discoverable. A person picks the one they trust. An agent may use whichever one retrieval or prompting happens to surface first.


Governing this means treating definitions more like code: proposed, reviewed, approved, versioned, with conflicts surfaced and settled by someone before anything reaches an agent. We go deeper on that workflow in[why revenue metrics break in AI self-serve analytics](https://www.kaelio.com/blog/why-revenue-metrics-break-in-ai-self-serve-analytics) .


## The real choice: extend your core platform, or build alongside it


Put those 2 issues together, a semantic layer that quietly rots and one that nobody formally signs off on, and you arrive at the decision most data leaders are actually weighing. It comes down to 2 paths:


1. **Extend your core platform.** Invest more deeply in your warehouse and modeling vendor's tooling, govern agents through it, and accept that you are governing one vendor's slice and tying that governance to it.
2. **Build the capability alongside your stack.** Stand up a layer that keeps the semantic layer current, signs off on its definitions, carries the business context around them, stays vendor-neutral, and remains portable if you change a part of the stack later.


Both are reasonable, and the right call depends on your situation. For governance specifically, though, building alongside has a structural advantage. Governance tied to one platform can only ever govern that platform's part of the picture. The reason many teams bought a data catalog that sits horizontally across the stack was to compare definitions, check quality, and trace lineage across every system, including the source systems the warehouse's models never describe. The layer that governs agents wants to live in the same place, for the same reasons. There is a practical upside too. If your platform contract changes in a couple of years, reviewed context that lives in portable files comes with you instead of staying behind in a tool you are leaving.


This is also the part that tends to land with a VP or CDO. Most organizations have far more data sources than they have warehouses and modeling tools. Governing agents inside one vendor's layer governs one slice and ties you to that vendor. Governing them in a portable layer alongside the stack covers more of the picture and keeps your options open if the stack changes later.


## Who actually owns this?


That leads back to the title, and to the part with no settled answer yet. When a definition needs an owner, a stale doc needs retiring, or a conflict needs resolving, whose job is that? Data governance? Analytics engineering? Data engineering? It touches all 3, and that overlap is why the work keeps slipping between teams.


The cleanest approach we have seen is to stop treating "context" as a feature buried inside someone's existing tool and start treating it as a shared responsibility with its own home:


- **Data engineering** keeps the sources connected and the pipelines running, including the upstream systems.
- **Analytics engineering** writes and refines the definitions and the joins.
- **Business data owners and metric owners** approve the definitions in their domain and settle what a metric actually means. Governance runs the process, but the owners make the call.
- **Data governance** owns the operating model: the approval workflow, classification and access policy, conflict resolution, and the signal that an answer is sanctioned.


None of that holds together if each function edits a different tool. It works when all 3 collaborate on the same reviewable files, the way engineering teams collaborate on code. That is the organizational point, and it is why the form of the context layer matters as much as its contents. For where this sits relative to the tools you already own, see[context layer vs. semantic layer](https://www.kaelio.com/blog/context-layer-vs-semantic-layer-why-you-need-both-for-ai) .


## What a governed context layer looks like


So what is the thing that sits alongside the stack and gives all 3 teams something to maintain together? A governed context layer takes your schema, joins, approved definitions, BI logic, query history, and the business knowledge sitting in tools like Confluence and Notion, and turns it into reviewable files that agents search and run instead of guessing. We pulled apart the mechanics in[building a context layer for the agentic era](https://www.kaelio.com/blog/building-a-context-layer-for-the-agentic-era) . For a governance lead, a handful of properties are what make it governed rather than just convenient:


Your semantic layer (Cube, dbt MetricFlow, LookML) Governed context layer


What it covers The metrics you have modeled The same metrics, plus the business context and conflicting definitions around them


Stays current Hand-maintained, drifts stale Auto-reconciled from your warehouse, BI, and query history


Approval and trust signal Technical merge review, but no business-owner sign-off Reviewed and signed off by the metric owner, versioned, conflicts resolved


Access and PII for agents Rarely enforced on the agent's query Classification, masking, and role policy applied to the generated SQL


Trust over time Manual spot-checks Tested against known-correct answers


Survives a tooling change Tied to the vendor Portable, vendor-neutral files


2 of those rows are worth a moment.


**Access rules need to be enforced, not requested.** Telling an agent "do not show PII" inside a prompt or a skill is a request to a system that does not guarantee compliance. Real enforcement applies your data classification, masking, row-level, and role-based policies to the SQL the agent generates, before it runs, with nothing left to the model's discretion. The cleanest version of this maps your existing access model onto agents rather than inventing a separate one, which is the pattern in[how to govern AI agent access to business metrics](https://www.kaelio.com/blog/how-to-govern-ai-agent-access-to-business-metrics) .


**Trust needs to be measured.** Writing down known-correct answers for your most important questions ("average order value last quarter should be roughly X") and re-checking them after each update is how you catch the moment a new definition quietly changes an answer you had already approved. Done properly this looks less like a checklist and more like a test suite: golden queries over fixed time windows, snapshot fixtures so the data underneath does not move, tolerances for acceptable drift, expected outputs per permission level, and freshness checks. Pasting example SQL into a skill gets you started; a versioned suite like that is what scales it and puts it on governance footing. More on that in[how to prove your AI analytics answers are trustworthy](https://www.kaelio.com/blog/how-to-prove-your-ai-analytics-answers-are-trustworthy) .


This is the gap[ktx](https://github.com/Kaelio/ktx) is built to close. **ktx** is an open-source[context layer](https://docs.kaelio.com/ktx/docs/concepts/the-context-layer) for data agents. It reads from your stack (warehouses, BI tools, modeling code, query history, Notion, Confluence), reconciles what it finds, flags contradictions for a person to resolve, and exposes the approved context to any agent over[MCP](https://modelcontextprotocol.io/introduction) . Everything is stored as git files you can[review, diff, and merge](https://docs.kaelio.com/ktx/docs/guides/reviewing-context) . There is also a hosted version for teams that adds managed sync, the review and conflict-resolution workflow, role-based access, per-agent access tokens, activity monitoring, and the evaluation checks, which is the multi-user setup a governance function tends to need.


## How to start without replacing your stack


You do not replace your warehouse or your modeling tool, and you do not try to clean up years of scattered documentation in week one. A reasonable first quarter looks like this:


1. **Pick one high-stakes, contested metric** , the one 2 teams define differently, and govern its definition end to end: gather the candidate definitions, surface the conflict, approve one, version it.
2. **Pull in the knowledge around that metric that is not a formula** : the caveats, the canonical-source decision, the deprecated tables to avoid, from your docs and Confluence, so the agent has the why and not just the how.
3. **Map your existing access model** onto the agents querying it, enforced rather than prompted, so masking, row-level, and role boundaries actually hold.
4. **Stand up a small set of golden queries** with fixed time windows and expected answers (per permission level where it matters), and re-run them on every update.
5. **Show the portable result to your VP** : reviewed files that cover more than modeled metrics and do not belong to any single vendor.


That is a contained pilot a governance team can run alongside the stack, the same way you already run a horizontal catalog. If a fuller migration is ahead of you,[moving from a semantic layer to a governed context layer](https://www.kaelio.com/blog/how-to-migrate-from-semantic-layer-to-governed-context-layer) walks through the path.


The agents are already in the building, answering questions in plain language, whether or not the governance is ready for them. The reassuring part is that this is not a brand-new discipline you have to invent. It is the governance you already practice, applied to a new and very literal consumer, and moved to a layer that can reach everything that consumer will touch. Govern the context once, outside the warehouse, and every agent works from the same approved version. If you want to see what that looks like on your data,[book a walkthrough](https://www.kaelio.com/book-demo) .
