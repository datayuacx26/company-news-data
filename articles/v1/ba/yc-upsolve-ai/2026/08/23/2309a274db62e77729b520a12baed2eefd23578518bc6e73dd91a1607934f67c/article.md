---
schema_version: "1.0.0"
document_id: "2309a274db62e77729b520a12baed2eefd23578518bc6e73dd91a1607934f67c"
company_key: "yc-upsolve-ai"
company: "Upsolve AI"
source_id: "yc-upsolve-ai-news-import-0ed3b4ab08c4"
canonical_url: "https://upsolve.ai/blog/internal-data-agents-uber-openai-anthropic"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T22:34:02.329552+00:00"
fetched_at: "2026-08-19T22:34:03.585438+00:00"
content_hash: "sha256:6fac18258c94213c22d2c1be115ff6419f749bafbb39a94c2b796c27699e871f"
---

# How Uber, OpenAI & Anthropic Built Internal Data Agents

Three of the most technically capable engineering organizations in the world each built an internal data agent, published exactly how it works, and arrived at the same conclusion: the model was never the bottleneck. Uber shipped QueryGPT in 2024, OpenAI documented its in-house data agent in January 2026, and Anthropic published its self-service analytics stack in June 2026. Read them side by side and a shared blueprint emerges, along with a few genuine disagreements that anyone building an analytics agent should understand before they start.


This article compares all three systems, extracts the practices they agree on, and is honest about where their published evidence points in opposite directions.


## Key Takeaways


-


**All three teams reached the same diagnosis independently:** accuracy in analytics is a context problem, not a code generation problem. Anthropic's numbers make it unusually concrete: the same Claude model scored under 21% on their internal evals without curated context and consistently above 95% with it.


-


**Narrowing the search space is the single most common design pattern.** Uber built workspaces, OpenAI curated six context layers, and Anthropic built a routing skill. None of them let an agent search the full warehouse.


-


**The teams disagree on how context should be produced.** OpenAI derived meaning automatically by crawling its codebase. Anthropic tried auto-generating metric definitions and found it actively harmful compared to a smaller human-curated set.


-


**Context decays faster than most teams expect.** Anthropic watched its offline accuracy fall from roughly 95% to around 65% in a single month without maintenance, which reframes context as ongoing infrastructure rather than a setup task.


## The Three Systems at a Glance


Before the detail, here is the shape of each system. The differences in framing matter as much as the architectures, because each team defined the problem differently and got a different product as a result.


**Uber QueryGPT** (Sept 2024)


**OpenAI Data Agent** (Jan 2026)


**Anthropic + Claude** (Jun 2026)


**Problem framed as**


Query authoring is slow


Finding the right table is slow


Answers look right but cannot be verified


**Core mechanism**


Intent, table, and column-prune agents over curated workspaces


Six context layers, retrieved on demand


Data foundations, sources of truth, skills, validation


**What it returns**


SQL for a human to run


A validated answer


A validated answer with a provenance footer


**Headline result**


About 10 minutes per query down to about 3


Days down to minutes


95% of business analytics queries automated


**Scale**


1.2M interactive queries per month


3,500+ users, 70,000 datasets, 600+ PB


Company-wide, ~95% aggregate accuracy


**Biggest unsolved problem**


Hallucinated tables and columns


Resource intensity of the approach


Silent failures that look plausible


## Uber QueryGPT: Solving for Speed First


Uber's system, described in its[QueryGPT engineering writeup](https://www.uber.com/us/en/blog/query-gpt/) , started as a hackathon project in May 2023 and went through more than twenty iterations before production. The motivation was throughput. Uber's data platform handles roughly 1.2 million interactive queries a month, with the Operations organization alone contributing about 36% of them, and each query took around ten minutes to author. QueryGPT brought that to roughly three minutes.


### The naive version broke at scale


The first build was straightforward retrieval augmented generation. Uber vectorized the user's question, ran a nearest-neighbor similarity search across SQL samples and schemas, and pulled back three relevant tables and seven sample queries to include in the prompt. It worked on a starting set of seven tier-one tables and twenty queries.


Accuracy then declined as they onboarded more tables. The team identified why, and the reasons will be familiar to anyone who has tried this. Similarity search between a plain-English question and a` CREATE TABLE` statement does not return relevant results, because the two do not share vocabulary. Some tier-one tables carried more than 200 columns and consumed 40,000 to 60,000 tokens each, which broke the context limits of the models available at the time.


### Decomposition into specialized agents


The production architecture fixed this by breaking one large task into several small ones. An **intent agent** classifies the incoming question into one or more business domains. Those domains map to **workspaces** , which are curated collections of tables and SQL samples for areas like Mobility, Ads, and Core Services, alongside eleven other system workspaces and user-created custom ones. A **table agent** then proposes the specific tables and surfaces them to the user to confirm or edit. Finally a **column prune agent** strips irrelevant columns from the schemas before generation, cutting both cost and latency.


> **The pattern worth stealing:** Uber concluded that language models perform well when given a small, specialized unit of work rather than one broad task. Every subsequent system in this comparison reaches the same conclusion by a different route.


### What Uber could not solve


Uber built a real evaluation practice around golden question-to-SQL pairs, scoring intent accuracy, table overlap, whether the query ran, whether it returned rows, and how closely it resembled the reference query. That last set of signals is instructive: they were measuring whether the generated SQL was *mechanically* correct, not whether the answer was *meaningful* .


Their stated limitations are the most useful part of the writeup. Hallucinated tables and columns remained unsolved at publication. And they found that user questions were not context-rich enough on their own, which forced them to build a prompt expander to enrich questions before sending them to the model. That is a context gap being patched at the prompt layer, because there was nowhere else to put it.


At the time of writing, QueryGPT had about 300 daily active users, with 78% reporting that it reduced the time they would have spent writing queries from scratch.


## OpenAI: Context as an Engineered System


OpenAI's[in-house data agent writeup](https://openai.com/index/inside-our-in-house-data-agent/) describes a system operating at a different order of magnitude: more than 3,500 internal users, over 600 petabytes, and roughly 70,000 datasets. At that size, simply locating the correct table becomes one of the most time-consuming parts of any analysis.


The critical shift is in the framing. Where Uber treated the problem as query authoring, OpenAI treated it as context retrieval, and built six distinct layers to solve it: table usage and lineage, human annotations from domain experts, code-level enrichment derived by crawling the codebase, institutional knowledge pulled from Slack and internal documents, memory of past corrections, and live runtime validation against the warehouse.


Their most transferable finding is that a table's real definition is not in its schema. It lives in the pipeline code that produces it, including the assumptions baked in and the freshness guarantees. They also found that highly prescriptive prompting made results worse, and that consolidating an oversized toolset improved reliability.


The layer most teams underestimate is memory. By retaining corrections,[the six context layers behind OpenAI's data agent](https://upsolve.ai/blog/openai-data-agent) ensure that a fix learned once carries forward instead of resurfacing every time a similar question is asked.


## Anthropic: Solving for Verification


Anthropic's[self-service data analytics writeup](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude) , published in June 2026 by its data science and data engineering team, is the most operationally detailed of the three. It reports that 95% of business analytics queries at Anthropic are automated via Claude, at roughly 95% accuracy in aggregate.


### The number that settles the argument


Buried in the skills section is the most quotable finding in this entire comparison. Without curated skills, Claude's accuracy on Anthropic's internal evals did not exceed 21%. With skills, it consistently exceeded 95%, and reached around 99% in certain domains.


Same model. Same warehouse. Same questions. The only variable was the context around it.


### Why analytics is not like coding


Anthropic opens with a distinction that explains why so many teams are surprised by their results. Coding is an open-ended solution space that rewards model creativity, and documentation and tests act as natural guardrails against hallucination. Analytics is the opposite. There is usually one correct answer from one correct source, and no deterministic way to prove correctness after the fact.


This is why the Anthropic team warns that pointing an agent at a warehouse and letting it run can create "a false sense of precision." The query executes, a number appears, and nothing in the system tells the reader whether to trust it.


### Three failure modes and a four-layer stack


Anthropic attributes the overwhelming majority of wrong answers to three causes:


-


**Concept-to-entity ambiguity:** with hundreds of plausible fields, the agent cannot identify which ones actually answer the question. Their example is deliberately mundane: to count active users, what counts as active, do you include fraudulent accounts, and what lookback window applies?


-


**Data staleness:** definitions, schemas, and sources change constantly, so encoded knowledge rots and starts producing subtly wrong answers.


-


**Retrieval failure:** the correct information exists and is properly documented, but the search space is so large the agent never finds it.


Their stack attacks each one. **Data foundations** shrink ambiguity by curating a small set of canonical, governed datasets and aggressively deprecating near-duplicates. **Sources of truth** give the agent reference surfaces to navigate, in descending order of trust: the semantic layer first, then lineage and the transformation graph, then the query corpus, then business context. **Skills** solve retrieval by routing the agent to a few dozen curated reference files instead of a million-field warehouse. **Validation** catches whatever leaks through.


### The maintenance problem, quantified


The most sobering detail is what happened when they stopped maintaining it. Offline accuracy drifted from roughly 95% at launch to around 65% over a single month, because skill documentation describes a data model that changes daily.


Their fix was organizational rather than technical. Skill files live in the same repository as the transformation models, so the pull request that changes a model is the same pull request that updates the documentation describing it. A code-review hook flags any reporting-model change that does not touch a skill file. Roughly 90% of their data-model pull requests now include a skill change in the same diff.


> **Bottom line:** two of the three teams treat context maintenance as a first-class engineering commitment with enforcement in CI. The third had not reached that stage yet, and its unsolved hallucination problem is consistent with that.


## What All Three Teams Agree On


Strip away the differences in scale and vocabulary and six practices appear in all three systems. Treat this as the common good practice for anyone building in this space.


### 1. Narrow the search space before generating anything


This is the most universal finding. Uber built workspaces to cut the retrieval radius by business domain. OpenAI curated six context layers rather than exposing the raw warehouse. Anthropic built a routing skill whose entire job is to narrow a million-field warehouse to a few dozen curated files before a query is ever written. None of these teams trusted retrieval over an undifferentiated surface, because all of them tried it first and watched it fail.


### 2. Decompose the task into specialized steps


Uber's intent, table, and column-prune agents. OpenAI's finding that a smaller consolidated toolset outperformed a sprawling one. Anthropic's split between a knowledge skill that finds sources and a runbook skill that executes the analysis. Every team converged on narrow, well-scoped units of work.


### 3. The schema is not enough


All three had to import meaning from somewhere outside the data warehouse. Uber injected custom instructions about internal terminology and date handling. OpenAI crawled its codebase and pulled institutional knowledge from Slack and internal docs. Anthropic built a company knowledge graph of indexed documents, roadmaps, decision logs, and organizational structure, and describes business context as the layer most teams skip and the one they underrated longest.


### 4. Build the evaluation set before you scale


Uber curated golden question-to-SQL pairs with manually verified intent, schemas, and reference queries. OpenAI validates against known-good results. Anthropic runs offline evals, pins ground truth to snapshot dates so it cannot drift, stores results like telemetry rather than test logs, and gates each domain launch on clearing an accuracy threshold.[Building an evaluation suite of this kind](https://upsolve.ai/blog/agent-evaluation-framework) is the piece of work that cannot be deferred, because without it there is no way to know whether a context change helped or hurt.


### 5. Keep a human in the loop somewhere


The placement varies but the principle holds. Uber asks the user to confirm table selection before generating. OpenAI relies on curated human annotations from domain experts. Anthropic requires human ownership of metric definitions and explicit sign-off on anything leadership-bound.


### 6. Context decays, so maintenance is the job


Anthropic quantified it. OpenAI's memory layer exists precisely so a correction learned once carries forward. Uber's evaluation set was explicitly designed to evolve as the product and its failure modes changed. None of these teams treated context as something you configure once.


## Where They Diverge


The agreements are useful. The disagreements are more interesting, because they mark the questions the field has not settled yet.


### Should context be generated or curated?


This is the sharpest split. OpenAI derived meaning automatically, crawling its codebase so the agent could understand how a table is built, how fresh it is, and what it excludes. Anthropic tried something adjacent and reported it as a failure: bootstrapping the semantic layer by having a model auto-generate metric definitions from raw tables and query logs produced plausible-looking definitions that encoded the very ambiguities they were trying to eliminate. It scored net-negative against a smaller human-curated layer. Their recommendation is to generate the documentation with a model but have a human own the definition.


These are not quite the same experiment, and the distinction matters. Reading deterministic pipeline code to learn how a table is constructed is a different act from inferring what a business metric *should* mean from historical query patterns. The first extracts something that exists. The second invents something that does not. The practical takeaway is that automation is safe where ground truth already lives in code, and unsafe where the ground truth lives in a human decision nobody has written down.


### Is query history a signal or noise?


OpenAI includes historical query patterns in its table usage layer as one input among six. Anthropic ran a direct ablation on the same idea and published the null result. They gave the agent grep access to thousands of dashboard, transformation, and analyst notebook SQL files, verified in transcripts that it actually read them, and measured a change of less than one percentage point in either direction.


They then checked the obvious explanation. The answer was present in the corpus for roughly 80% of the questions the agent got wrong, and whether an answer was present did not predict whether the agent got it right. The information was there, the agent saw it, and it still did not use it. Their conclusion was that the bottleneck was not access to prior work but structure: mapping a question to the right entity in the first place.


For anyone evaluating a platform that promises to learn from your query logs, that is the most important paragraph published on this topic in the last year.


### How much freedom should the agent have?


OpenAI found that rigid step-by-step prompting made results worse and that handing the agent a clear goal produced better outcomes. Anthropic went the other direction on the paths that matter most: their agents are structurally required by skill instruction to try the semantic layer first, with raw SQL available only as a documented fallback, and every query passes through a mandatory adversarial review sub-agent before an answer is returned. Uber sits at the conservative end, pausing to have a human confirm table selection.


The reconciliation is probably that autonomy and constraint apply at different levels. Let the agent reason freely about *how* to approach a question, and constrain *which sources* it is allowed to treat as authoritative. That combination appears in Anthropic's system, and it is the version most likely to survive contact with a business user.


### Where does the agent stop?


Uber returns SQL for a human to run. OpenAI and Anthropic return the answer itself. This is the clearest marker of maturity across the three, and it changes what the system must guarantee. Handing back a query puts verification on a person who can read SQL. Handing back a number removes that safety net entirely, which is exactly why Anthropic attaches a provenance footer to every response showing which source tier the answer came from, how fresh the underlying data is, and who owns the model.


## Grading All Three Against the Three-Layer Context Architecture


At Upsolve we assess analytics agents against a three-layer context architecture: **Structure** (what data exists and how it connects), **Meaning** (what the data means at this specific company), and **Trust** (which answers have been validated). It is a useful lens here because none of these teams used that vocabulary, yet all three built toward the same three functions.


Layer


Uber QueryGPT


OpenAI


Anthropic


**Structure**


Workspaces, table agent, column pruning. Solid but curated by hand per domain.


Table usage, lineage, and live runtime validation. Comprehensive.


Canonical datasets with aggressive deprecation of near-duplicates, plus lineage and table ranking. Strongest.


**Meaning**


Custom instructions for internal terminology and dates. Thin, and the source of most residual errors.


Human annotations, code-level enrichment, institutional knowledge from Slack and docs. Strong and largely automated.


Semantic layer as the mandatory first path, human-owned definitions, plus a business context knowledge graph. Strongest.


**Trust**


Golden SQL evaluation set. Offline only, with no runtime trust surface.


Memory of corrections plus golden-query evaluation. Solid.


Offline evals, ablations, adversarial review, provenance footers, passive monitoring, and automated correction harvesting. Substantially ahead.


The pattern is clean. Every team built Structure well, because that is the layer the warehouse already supports. Meaning is where effort concentrated as the systems matured. Trust is where almost nobody starts, and it is the layer that separates a system people use from a system people believe.


Uber's unsolved hallucination problem, OpenAI's investment in institutional knowledge, and Anthropic's 21% baseline are three views of the same fact:[the context layer determines agent accuracy](https://upsolve.ai/blog/context-engineering-for-analytics) , and the model contributes far less than teams expect.


## What This Means If You Are Building One


The uncomfortable part of this comparison is the resource asymmetry. All three organizations have dedicated data platform teams, and two of them build frontier models and the tooling around them. The blueprint is portable. The engineering capacity behind it is not.


The industry evidence is consistent with that gap. MIT's State of AI in Business research found that[roughly 95% of enterprise generative AI pilots deliver no measurable business impact](https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/) , and that externally built tools succeeded roughly twice as often as internal builds. Independent testing points the same direction: when analyst Claire Gouze[benchmarked more than a dozen analytics agents](https://thenewaiorder.substack.com/p/i-tested-14-analytics-agents-so-you) , the differentiator was consistently how well each tool handled business context rather than raw query generation. The venture analysis reached the same place, with[a16z arguing that data agents are close to useless without the right context](https://a16z.com/your-data-agents-need-context/) .


Anthropic's own advice for teams starting from zero is refreshingly modest: a handful of canonical datasets, a few dozen offline evals, and one thin routing skill will capture most of the upside. Everything else in their post came later.


If you are weighing this against buying, the honest framing is that you are not deciding whether to build an agent. You are deciding whether to build and permanently staff a context infrastructure practice, with CI enforcement, an eval suite, and a maintenance loop. That is the real scope, and underestimating it is[the most common reason agent projects stall after a promising demo](https://upsolve.ai/blog/why-ai-data-agents-fail) .


## Where Upsolve's Approach Fits


We built Agent Studio around the same conclusion these three teams reached independently, which is why their published architectures map onto it so closely.


The three-layer context architecture is the product rather than a feature bolted onto a notebook or a dashboard tool. Institutional knowledge gets encoded deliberately, the way Anthropic curates its reference docs, with humans owning definitions rather than a model inventing them. Golden query testing functions as unit tests for agent accuracy, the same practice Uber and Anthropic both landed on. Context gap detection surfaces where the agent is missing knowledge, and corrections feed back through human approval, which is the encode, deploy, and tune loop all three systems eventually built by hand.


One practical difference is worth naming. Anthropic routes every question through a mature semantic layer first, and that is the right design if you have one. Many teams do not, and building one before you can start is the most common reason these projects never begin. Agent Studio does not require an existing semantic model to get started, though it integrates with one where it exists.


If you are evaluating options, the criteria in this article travel well. Ask any vendor how they handle all three layers, what happens to accuracy after a month without maintenance, and whether a human owns the metric definitions. Those three questions separate[platforms built around the context problem](https://upsolve.ai/blog/ai-agent-builder-platforms-analytics) from tools that treat context as a feature, and they apply just as sharply to[the semantic layer you may already maintain](https://upsolve.ai/blog/semantic-layer) .


## Three Teams, Twenty Months, One Conclusion


Every team in this comparison started by assuming the model was the hard part, and every one of them finished by concluding it was the context. That is not a coincidence across three organizations, twenty months, and three completely different problem framings. It is the shape of the problem.


## Frequently Asked Questions


## How do companies build internal data agents?


The published architectures follow a consistent pattern: narrow the searchable surface to curated domains, decompose the work into specialized steps rather than one large prompt, encode business meaning that does not exist in the schema, and validate answers against a golden question set. Uber, OpenAI, and Anthropic all arrived at this structure independently between 2024 and 2026.


## What is the difference between Uber's QueryGPT and Anthropic's approach?


QueryGPT generates SQL for a human to run and optimizes primarily for authoring speed, reducing query writing from roughly ten minutes to about three. Anthropic's system returns the answer itself and optimizes for verification, adding a semantic layer requirement, adversarial review, and a provenance footer on every response. The gap reflects two years of the field learning that speed without trust is not useful to business users.


## Does a better model fix analytics agent accuracy?


The published evidence says no. Anthropic reported that the same Claude model scored under 21% on their internal evals without curated context and consistently above 95% with it. The variable that moved accuracy was the surrounding context, not the model.


## Can you use OpenAI's data agent or Anthropic's system at your company?


Neither is a purchasable product. Both were built around a specific warehouse, permission model, and set of business definitions, and both were published as architectural references rather than products. The principles transfer directly, but the implementations do not.


## How long does context stay accurate without maintenance?


Anthropic measured offline accuracy falling from roughly 95% to around 65% over a single month, because documentation describes a data model that changes daily. Their response was to colocate context files with transformation models so both change in the same pull request, with a review hook enforcing it.


## Should you let AI generate your metric definitions?


Anthropic tested this and recommends against it. Auto-generated definitions built from raw tables and query logs encoded the ambiguities they were meant to remove and performed worse than a smaller human-curated layer. Their guidance is to draft documentation with a model but keep human ownership of the definitions themselves.


## Sources


\[1\] **Uber Engineering** : QueryGPT: Natural Language to SQL Using Generative AI, September 2024.[https://www.uber.com/us/en/blog/query-gpt/](https://www.uber.com/us/en/blog/query-gpt/)


\[2\] **OpenAI** : Inside Our In-House Data Agent, January 2026.[https://openai.com/index/inside-our-in-house-data-agent/](https://openai.com/index/inside-our-in-house-data-agent/)


\[3\] **Anthropic** : How Anthropic Enables Self-Service Data Analytics with Claude, June 2026.[https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude)


\[4\] **a16z** : Your Data Agents Need Context, March 2026.[https://a16z.com/your-data-agents-need-context/](https://a16z.com/your-data-agents-need-context/)


\[5\] **Claire Gouze, The New AI Order** : I Tested 14 Analytics Agents So You Do Not Have To, January 2026.[https://thenewaiorder.substack.com/p/i-tested-14-analytics-agents-so-you](https://thenewaiorder.substack.com/p/i-tested-14-analytics-agents-so-you)


\[6\] **Fortune, reporting on MIT State of AI in Business 2025** : Enterprise generative AI pilot outcomes and internal versus vendor build success rates.[https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/](https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/)
