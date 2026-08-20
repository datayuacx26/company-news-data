---
schema_version: "1.0.0"
document_id: "a2b1512d1a10257c14cc3c9fdf1478d8531bd5c4917331bebbb55d73fbdd2d14"
company_key: "yc-halluminate"
company: "Halluminate"
source_id: "yc-halluminate-news-import-9819d7d93bdb"
canonical_url: "https://www.halluminate.ai/blog/websailor-benchmark"
published_at: null
first_seen_at: "2026-08-11T04:50:25.359940+00:00"
fetched_at: "2026-08-11T04:50:26.871958+00:00"
content_hash: "sha256:957452f8d932e6e732be28488f053387415bcc62a40c5269547e917d3dddc32a"
---

# WebSailor Benchmark Dataset Size

“How many questions are in the WebSailor benchmark?” is a reasonable question with an awkward answer: **WebSailor is not a frozen UI exam with one public task count the way WebArena is.**


WebSailor (Alibaba NLP / Tongyi lab line of work) is mainly a **method for training deep information-seeking web agents** , plus a synthetic data pipeline called **SailorFog-QA** . The papers evaluate those agents on *other* benchmarks—BrowseComp, GAIA, Xbench-DeepSearch, and similar suites—each with its own official size.


If you need a single size figure people cite for the WebSailor *training* corpus, use this:


**SailorFog-QA-V2 (WebSailor-V2): over 30,000 high-quality instruction-tuning pairs.**


That is training data cardinality, not “number of WebSailor leaderboard tasks.”


## Why the count searches keep failing


WebArena can answer “how many tasks?” with **812** . Web Bench can answer with **~2,454** open tasks. WebSailor gets searched the same way because blog posts and Discord threads call it a benchmark. Structurally it is closer to:


1. A difficulty taxonomy for information-seeking questions (levels of uncertainty / path complexity)
2. A graph-based synthesis pipeline (SailorFog-QA) that obfuscates entities and relations so agents cannot solve the question with one lookup
3. An RFT / RL training recipe for tool-using search agents
4. Evaluations on external hard suites (especially BrowseComp-en/zh)


The public[WebSailor code](https://github.com/Alibaba-NLP/DeepResearch/tree/main/WebAgent/WebSailor) even keeps only example eval files in-tree and asks you to download official benchmarks separately—to reduce test leakage. That is the opposite of “here is our fixed 812-task exam.”


## Numbers that are easy to mix up


Name What it is Size people cite


SailorFog-QA Synthetic training QA for WebSailor Generated at scale; not a single WebArena-style public N


SailorFog-QA-V2 SFT/RL instruction data for WebSailor-V2 **30,000+** pairs


WebArena Self-hosted web UI benchmark **812** tasks


Web Bench (open set) Live-web UI benchmark **~2,454** tasks / 452 sites


SimpleQA (full set) Factual QA (used in WebSailor sanity checks) **4,326** pairs; papers often sample a subset


When someone asks for “WebSailor benchmark problem count,” ask which row they mean. Nine times out of ten they want either V2’s **30k+** training pairs or the size of BrowseComp / GAIA, not a nonexistent WebSailor-only exam.


## Different job from Web Bench


WebSailor-style work optimizes for **uncertain, multi-hop web questions** —search, browse, cross-check evidence, answer under obfuscation.


[Web Bench](https://www.halluminate.ai/blog/benchmark) optimizes for **browser UI agents on live sites** —click, type, log in, create/update/delete state across hundreds of domains.


Both involve “the web.” They measure different skills. A strong SailorFog-trained search agent can still fail a Nordstrom wishlist task; a strong Web Bench agent can still fail BrowseComp.


## Sources


- WebSailor:[arXiv:2507.02592](https://arxiv.org/abs/2507.02592)
- WebSailor-V2:[arXiv:2509.13305](https://arxiv.org/abs/2509.13305)
- Code:[Alibaba-NLP/DeepResearch — WebSailor](https://github.com/Alibaba-NLP/DeepResearch/tree/main/WebAgent/WebSailor)


For UI-agent evals and how they relate, see[browser agent benchmarks in 2026](https://www.halluminate.ai/blog/browser-agent-benchmarks) .
