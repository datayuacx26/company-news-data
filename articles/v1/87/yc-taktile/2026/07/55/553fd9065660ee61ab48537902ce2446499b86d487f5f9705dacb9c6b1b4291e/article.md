---
schema_version: "1.0.0"
document_id: "553fd9065660ee61ab48537902ce2446499b86d487f5f9705dacb9c6b1b4291e"
company_key: "yc-taktile"
company: "Taktile"
source_id: "yc-taktile-news-import-01317932feb4"
canonical_url: "https://engineering.taktile.com/blog/llm-wiki-agent-memory/"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-24T03:38:54.472601+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:576e6e92f108cd74d8218c34b3f21e04cce476425bfff846dc41e09ac86314ce"
---

# We gave our agent a memory: building an LLM Wiki over sources that never sit still

Jul 13, 2026 ·


9 min read


# We gave our agent a memory: building an LLM Wiki over sources that never sit still


---


*Our agent kept re-deriving the same answers from scratch, burning minutes and dollars on every question. So we gave it a persistent memory. Here’s how we built it, what we tested, and what changed.*


# The problem


Our agent answers questions for the team. Ask it something and it goes and finds out: it searches our internal knowledge, our code, Notion, Slack, GDrive, and the web, synthesizes an answer including citations for each claim, and replies back.


Our first approach was standard retrieval-augmented generation (RAG): fetch the relevant raw documents at query time and stuff them into context. The same facts got retrieved and re-derived from the same documents over and over, and answer quality was only ever as good as whatever chunks got pulled into context that particular time.


An alternative is the idea Andrej Karpathy[sketched out as an “LLM Wiki”](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) : instead of retrieving raw documents on every query, you compile what you learn once into a persistent, AI-maintained knowledge layer and reuse it. You pay to understand something once, then answer from accumulated knowledge that improves over time instead of re-deriving it from scratch. We’re still early in building one, but here’s what we’ve learned so far.


## The part the research hasn’t caught up on yet: our sources move


Here’s the wrinkle. Most of the LLM Wiki work out there assumes a *local, personal* wiki: your raw sources are files you own, and you know exactly when one is added or edited. Under those conditions maintenance is easy - when a file changes, you just prompt the LLM to recompile the affected pages.


But we wanted a *shared* wiki compiled from shared sources like Notion, GDrive, or the Web, that are constantly changing underneath us, edited by people who have no idea a wiki depends on them. We get no signal when a source shifts, or when fresh information shows up in a corner we’ve never indexed. Keeping a compiled knowledge layer trustworthy when its underlying sources drift silently turned out to be the hard part, and existing research hasn’t explored it much yet in the context of a LLM wiki.


We haven’t fully solved this yet. Today we lean on two things. A weekly lint job catches contradictions and stale claims after the fact. And more importantly, at query time we re-validate every wiki fact against its linked source before we use it. It means we’re still paying to fetch the source on every answer, a cost the wiki could otherwise reduce.


# What our wiki actually is


The wiki is a directory of markdown files living on a dedicated branch of one of our repos: markdown files per entry, each with frontmatter metadata, organized into a nested folder structure. Therefore the wiki is version-controlled, diffable, and reviewable like any other code. There’s no separate piece of infrastructure to babysit.


But it isn’t one flat pile of files, and to see why, start with what happens when a question comes in: we classify it into a category first. Each category has a defined set of sources it’s allowed to draw on.


That classification is what the structure is built around. The first idea was to break one large wiki into smaller sub-wikis, one per category, which makes it easier for the LLM to navigate. Once a question is classified, we route it to the matching sub-wiki, and the agent reads from (and later writes back to) that sub-wiki specifically. This keeps retrieval scoped: a question about one category never has to wade through entries (or sources) that belong to another, which cuts both noise and cost.


Navigation across the structure runs on` index.md` files. At every level of nesting, the LLM maintains an` index.md` that summarizes what lives beneath it. To find something, the agent walks the tree top-down: reading the index at each layer to decide which folder or file to open next rather than scanning everything. The indexes are how a growing wiki stays navigable instead of collapsing into an undifferentiated heap of markdown.


Karpathy’s design starts with a dedicated **ingest** step - you drop sources in and the LLM compiles them into pages up front. Our flow is different: we don’t ingest ahead of time, because our sources won’t let us. They’re shared and constantly changing, so anything we compiled up front might be stale before it gets queried. Instead the wiki fills itself as a byproduct of answering questions. That leaves us with two moves:


1. **Query + Ingest** - a person asks something. The agent pulls the relevant wiki entries, confirms they’re still accurate against their sources, and also checks the other shared sources for anything new worth adding, then writes a cited answer, additionally flagging any conflicts it finds along the way. When an answer gets positive feedback, that’s the signal to keep it: we don’t store the response itself, but distil the factual claims embedded in it and write those back as entries, so the knowledge is in a reusable shape for the next query.
2. **Lint** - a periodic health-check keeps the wiki from rotting as it grows. Anything that doesn’t need human judgement is auto-resolved; the rest is raised in a report.


# How do you search a pile of markdown?


The whole value of a wiki depends on the agent finding the *right* entry when it asks a question. Get that wrong and you either miss knowledge you have or pull in junk that pollutes the answer.


We looked at four retrieval strategies.


1. **grep - word count.** Search directly for the query’s words inside each entry, count matches, return the highest counts. It’s the baseline.
2. **SQLite FTS5 + BM25 - keyword search done properly.** Same idea, but scored with the BM25 formula on top of SQLite’s built-in FTS5 full-text index. A Porter stemmer collapses word forms to a common root, and BM25 adds the rarity weighting and length normalization that plain grep lacks.
3. **Vector search - semantic matching.** Embed every entry and the question into vectors that capture meaning, then find the entry whose vector points closest to the question’s.
4. **LLM judgment - let the model choose.** This is the approach started with in production. Three passes: (1) read` index.md` and pre-select entries from their summaries, (2) read the frontmatter of the pre-selected set to filter out the irrelevant ones, (3) deep-read only what survives for a final judgment. Intelligent, but high latency and cost, and heavily dependent on the quality of the index and frontmatter, which the LLM writes itself, so errors compound.


On top of these, we also tried a hybrid: combining two of the methods (LLM judgment and vector search) with **Reciprocal Rank Fusion (RRF)** . RRF is a simple way to merge two ranked lists into one.


#### What the numbers said


We evaluated the four methods on two axes: how *good* the results are and how *fast* they are.


We ran this against a set of 24 queries over our production wiki, 88 entries at the time.


**For quality** we used a gold-labeled set of queries and measured, at cutoff K (tested at 3, 5, and 10):


- **Recall** - of all the correct (“gold”) entries, what fraction did we find (` gold-found ÷ total-gold` ).
- **Hit-rate** - fraction of queries where at least one gold entry showed up in the top K.
- **Gold missed / non-gold retrieved** - average correct entries skipped, and average junk pulled in.


A few things jump out. **grep is not viable** . It finds barely a fifth of the relevant entries at K=3 and never really recovers. **LLM judgment is the accuracy leader** , with the best recall and hit-rate at low K *and* the cleanest results (the lowest non-gold/junk count by a wide margin). Fusing it with vector search via RRF squeezes out slightly higher recall at K=10 (0.90), but the junk count balloons to 8 - you find a bit more by dragging in a lot more noise. **BM25 is the pragmatic middle** : it beats plain vector search at low K and lands within striking distance of the LLM methods, for a rounding error of the cost.


Which brings us to that cost. **On latency** , the spread across methods was enormous:


Method Latency


LLM judgment 69.1 s


BM25 0.47 ms


Vector 12.28 ms


grep 6.00 ms


The headline: LLM judgment wins on accuracy but is roughly 150,000× slower than BM25. That’s the trade-off. Its 69 seconds per query buy the cleanest results, while BM25 gets you most of the way in half a millisecond. Which side of that trade-off wins depends on what you’re optimizing for.


Further, these numbers will likely shift as the wiki grows in size. We then expect performance to shift toward a hybrid approach, because each search method is strong exactly where the others are weak: BM25 nails exact tokens, vector catches meaning, and LLM judgment brings intelligent reasoning. Fusing two or more of them is what we expect to hold up best at scale.


# What the payoff *could* be


Here’s the tension. Because we currently re-validate every fact against its live source, we’re not yet capturing the full benefit of the wiki in production. We still pay to fetch the source upon answering. But we can measure the ceiling, under our current setup. We ran the same queries in two ways: with the agent validating each claim against its documented source the way it does today, and with it answering from the wiki *without* the re-validation round-trip.


Across a few selected runs, on average, a run went from **9 m 40 s and $2.50** with live source lookups to **1 m 19 s and $0.98** answering straight from the wiki - roughly **8 minutes 21 seconds and $1.52 saved per run, 86% faster and 61% cheaper.** And even that wiki-only number still leans on LLM judgment for retrieval, which, as we saw above, carries real latency of its own. Swap in a cheaper retriever and the floor should drop further.


That is the number we’re chasing. So a question that arises: *how do we skip re-validation when it’s safe to?*


# Where we’re headed


As we saw, re-validation is a big contributor here, hence one improvement is to only re-validate when we actually need to. The plan: store a cheap fingerprint of each linked source alongside the wiki entry, such as a` last_updated_at` timestamp or a content hash. At query time we compare the current fingerprint against the stored one. If it hasn’t changed, we *know* nothing about the source moved, so we can trust the compiled wiki fact and answer immediately. If it has changed, we fall back to re-validating (and recompiling) just that entry.


That’s the exciting part. Every question the agent answers makes the wiki a little smarter, and every improvement we ship compounds on the last. We’ll close the gap on re-validation, re-evaluate and sharpen retrieval as the wiki grows, and push to prove that a shared, self-maintaining knowledge layer can hold up at scale. We think it can - and we’re just getting started.
