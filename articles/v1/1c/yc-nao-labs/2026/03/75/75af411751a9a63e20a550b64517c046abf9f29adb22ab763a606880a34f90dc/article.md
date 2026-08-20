---
schema_version: "1.0.0"
document_id: "75af411751a9a63e20a550b64517c046abf9f29adb22ab763a606880a34f90dc"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/how-to-do-context-engineering-for-data-teams/"
published_at: "2026-03-19T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T21:56:54.694470+00:00"
content_hash: "sha256:3e5734f2e9fb39dbca641f8a93e0474937b543e40571ddcfbd4b83f32d152f0e"
---

# How to do context engineering for analytics agents

Over the past few months, I ran three studies on context engineering for[analytics agents](https://getnao.io/) . Same data model. Same 40 unit tests. Different context setups each time.


The goal was simple: figure out what actually makes an analytics agent reliable, instead of repeating claims about semantic layers, RAG, or ontologies without evidence.


Here is what I found, and how you can apply it to your own data team.


If you want the broader framing on why data teams should own this, start with[Data Teams Should Become Context Teams](https://thenewaiorder.substack.com/p/data-teams-should-become-context) .


## What actually moves agent reliability


In my[first study](https://thenewaiorder.substack.com/p/first-context-engineering-study-are) , I tested 30 different context setups against the same 40 text-to-SQL tests. The best setup reached 45% reliability. Nothing exotic. Just three pieces of context:


- data schema
- a sample of rows per table
- a` rules.md` file with business logic and edge cases


Things that sound fancy but did not help:


- a full dbt repo pumped into context (noisy, dropped performance)
- a MetricFlow YAML as a shortcut (no improvement vs` rules.md` )
- a metrics-store-style semantic layer (killed coverage)


The` rules.md` file was the single biggest lever. Well-structured rules alone nearly matched full schema + profiling combinations. More detail on which context piece matters in[Which context really improves analytics agent performance](https://getnao.io/blog/context-impact-analytics-agent) .


Then in my[second study](https://thenewaiorder.substack.com/p/how-i-improved-my-analytics-agent) , I pushed that 45% baseline to **86%** by doing failure analysis on the 28 tests that were failing. The breakdown:


- 18 data model errors
- 6 date selection errors
- 4 test errors


Fixing them in that order looked like this:


1. Fix test errors (49→51%)
2. Add explicit date selection rules with DO/DON'T examples (51→60%)
3. Clean the data model: add missing computed fields, document metric sources of truth, clarify ambiguous terms like "our users" (65→86%)


The takeaway: **context engineering is mostly data engineering** . Most agent failures are data model failures in disguise. Step-by-step in[How I improved my analytics agent reliability from 45% to 86%](https://getnao.io/blog/improve-analytics-agent-reliability-steps) .


## Should you add a semantic layer?


Short answer: not in your first pass. Maybe later, and only at scale.


In study #1, adding a dbt MetricFlow semantic layer reduced hallucinations but also reduced answers to almost zero. It was 4x more costly in tool calls and 3x slower, and the semantic YAML added no performance vs a plain` rules.md` .


In my[third study](https://thenewaiorder.substack.com/p/how-to-make-semantic-layer-work-for-analytics-agents) , I tried again, this time forcing the agent to use only the semantic layer. I got it to 82% by:


1. using the dbt semantic layer skill to generate it
2. using the dbt natural language querying skill so the agent can pick between the semantic layer and raw SQL
3. making the semantic layer exhaustive (every metric and dimension I needed in my tests)
4. reviewing entities and keys manually to avoid bad joins
5. enriching descriptions with business context
6. adding` rules.md` rules on date filtering, null handling, ambiguous terms
7. adding a nao context layer so the agent doesn't rely solely on dbt MCP for discovery


More detail in[Does a semantic layer improve analytics agent performance?](https://getnao.io/blog/semantic-layer-impact-analytics-agent) .


My honest verdict after running it:


- the semantic layer **reduces** hallucinations, it doesn't remove them. The agent still has to pick the right metric among many.
- at 12 tables with clean OBT models, I hit 86% without a semantic layer and 82% with one. It's a tie.
- semantic-layer agents are more expensive and slower because of MCP tool calls.
- there is probably a complexity threshold (hundreds of tables, heavy joins) where a semantic layer starts paying off.


If you're a solo data analyst or a small team, skip it. Invest in` rules.md` and a clean data model first. If you run 150+ tables across multiple domains with real governance pain, it's worth the effort.


Reference patterns are in the[nao rules context docs](https://docs.getnao.io/nao-agent/context-builder/rules-context) and[context builder databases docs](https://docs.getnao.io/nao-agent/context-builder/databases) .


## How to set up a testing and monitoring framework


You cannot improve what you cannot measure. This is the part most teams skip, and it's the one that compounds.


Here is the setup I use for every context change:


**1. Define your success metrics.** I track 5:


- coverage: % of questions the agent attempts to answer
- reliability: % of correct answers
- cost: tokens spent per answer
- speed: time to first useful response
- data scanned: volume queried against the warehouse


**2. Build a unit test set.** 20 to 50 real questions from your business users, each paired with the expected SQL. Store them as YAML. Cover KPI lookups, cohorts, error analysis, distributions. Mix single-table and multi-join queries. See the[nao evaluation framework docs](https://docs.getnao.io/nao-agent/context-engineering/evaluation) .


**3. Run evaluation after every context change.** Treat` rules.md` edits and data model changes like model releases. Re-run the tests. Diff the results. Keep what improves reliability, revert what degrades it. The full setup is in[How to benchmark your analytics agent with a context stack](https://getnao.io/blog/analytics-agent-benchmark-context-stack) .


**4. Categorize failures, don't fix randomly.** Every time a test fails, tag the root cause: data model, date scope, business ambiguity, test error, out-of-scope. This is how I found that 18 of 28 failures in my first study were data model issues, which told me exactly where to invest.


**5. Monitor in production.** Log user questions, agent answers, and flag runs for human review. Feed confirmed failures back into your test set. This is the loop that turns a demo into a system.


The playbook is in the[nao context engineering playbook docs](https://docs.getnao.io/nao-agent/context-engineering/playbook) , and a broader view in[How to build a context stack for agentic analytics](https://getnao.io/blog/how-to-build-context-stack-for-agentic-analytics) .


## Where to start


If you're a data team starting today:


1. Write a minimal` rules.md` with your business definitions, ambiguous-term rules, and date logic.
2. Add schema and a small data sample to your agent's context.
3. Set up 20 unit tests based on real user questions.
4. Run an evaluation baseline.
5. Do failure analysis. Fix data model issues first, then rules.
6. Skip the semantic layer unless you have a large, messy model.


You can get a nao project running in 5 minutes with the[nao agent quickstart](https://docs.getnao.io/nao-agent/quickstart) .


## Final takeaway


Context engineering is not a tooling question. It's a discipline: clean data models, explicit rules, versioned context, and a test loop you run every time something changes.


If dashboards needed a governed data layer, analytics agents need a governed context layer. Teams that treat context like infrastructure will ship reliable agents. Teams that hope a semantic layer will fix it won't.
