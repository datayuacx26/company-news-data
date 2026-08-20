---
schema_version: "1.0.0"
document_id: "faee2f8f99fd58a9d0df86da9e3a07e1d13c1342c3cf39a836e8d03450e1bba5"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-f703c271ba19"
canonical_url: "https://www.greptile.com/blog/nvidia-nemotron-ultra-in-code-review"
published_at: "2026-06-04T12:00:00+00:00"
first_seen_at: "2026-07-21T22:07:23.426026+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:94f17cf0b2851afb2d5ec84a5cf60b509605de74e53ad7ae7d9f23fb50658d4d"
---

# Frontier Code Review Accuracy at Lower Cost with NVIDIA Nemotron 3 Ultra

## What Greptile does for developers


Greptile is an AI agent that reviews and tests pull requests with full context of the codebase. Every time a developer opens a pull request (a proposed change to a company's software), Greptile reads the change, understands the surrounding codebase, and leaves the kind of detailed, context-aware feedback a senior engineer would: what might break, what to improve, and why. When the developer has a follow-up question about Greptile's feedback, they simply reply in the thread and Greptile responds with a thorough answer.


For developers, that means bugs caught before they ship, less time waiting on a human reviewer, and feedback that actually understands their codebase instead of generic suggestions. Today 22,000 engineering teams rely on Greptile to do this thousands of times a day.


## Why we run multiple models


Behind that simple experience is a decision most people never see: which AI model should handle each step? It is tempting to pick one large model and route everything through it. We have found that this is the wrong approach. Reviewing a code change, deciding whether a threaded reply needs an answer, and continuously mapping an entire codebase are genuinely different jobs, with different demands on speed, cost, and the amount of information a model has to hold at once.


So instead of one model, we run several. A lightweight internal router sends each task to the model that handles it best. This is what we mean by a multi-model architecture, and it is the single most important design choice behind Greptile's quality and economics. It also means we are always evaluating new models for each job. Recently, we put NVIDIA Nemotron Ultra, currently in pre-release, through its paces on two of the most demanding jobs in that system.


## Where we put Nemotron 3 Ultra to the test


We evaluated Nemotron 3 Ultra on two parts of the product where its strengths (a very large context window and fast, cost-efficient inference) map directly onto the problem. In both, we benchmarked it head-to-head against the proprietary frontier models we run today. The results were excellent.


### 1. Chat with PR


When Greptile leaves a comment on a pull request, developers reply in the thread. Sometimes it is a real question ("Why is this a problem?"), sometimes a quick "thanks," and sometimes a message aimed at a teammate, not Greptile at all. The first decision is figuring out which of those it is, so that Greptile jumps in when it is genuinely being asked something and stays quiet otherwise.


We tested Nemotron 3 Ultra (Nemotron-3-Ultra-550B-A55B) on exactly this job: classifying each reply into one of three actions: **reply, acknowledge, or skip** . It reads the whole thread to judge intent, then routes accordingly. This is a step that runs on every reply across 22,000 teams, so whichever model handles it has to be fast and inexpensive without misreading what the developer meant.


\[ FIG. 01 / CHAT WITH PR: SHOULD GREPTILE REPLY AT ALL? \]


A developer replies in a thread


on a Greptile review comment


Beta tested: Nemotron Ultra


Classifies the reply


Reads the whole thread and decides the intent


in ~3 seconds


Reply


A real question or pushback. Greptile answers in the thread.


Acknowledge


A simple "thanks." Greptile notes it and doesn't clutter the thread.


Skip


The reply was aimed at a teammate, not Greptile. It stays quiet.


Step evaluated with NVIDIA Nemotron Ultra (pre-release)


Figure 1.


Every threaded reply is classified before Greptile acts, so it answers real questions and stays out of the way otherwise.


In beta testing, the classifier read intent correctly across our test scenarios, returning a decision in roughly two to three and a half seconds each time.


Developer's reply Action Why (Nemotron's reasoning) Time


"hmmm greptile, can you tell me more…"


Reply


Directly addressing Greptile and asking for clarification 2.3 s


"ben, what do you think about this?" Skip


Directed at a human named Ben, not Greptile 3.4 s


"i see, thanks for letting me know…" Acknowledge


Pure acknowledgment, no questions or pushback 3.0 s


\[ FIG. 02 / REPLY CLASSIFICATION LATENCY BY SCENARIO (BETA) \]


Time to classify (seconds)


0.0


0.5


1.0


1.5


2.0


2.5


3.0


3.5


4.0


2.3s


Reply


3.4s


Skip


3.0s


Acknowledge


"…can you tell me more?"


"ben, what do you think?"


"i see, thanks for letting me know"


Figure 2.


Time to classify a reply, measured across the scenarios above.


Cost is where the gap is widest. The classifier runs without a prompt cache, so the realistic comparison is against an uncached frontier model, and there Nemotron 3 Ultra came in roughly **4.5 times cheaper per classification, about a 78% saving** . Even measured against a cached frontier model it was meaningfully cheaper. At Greptile's volume, that difference compounds fast.


\[ FIG. 03 / COST PER 1,000 REPLY CLASSIFICATIONS (BETA) \]


~78% lower than uncached


Cost per 1,000 classifications (USD)


0.0


0.5


1.0


1.5


2.0


2.5


3.0


3.5


4.0


$0.78–$1.07


~$1.67


$3.51–$3.90


Nemotron Ultra


Prior frontier model (cached)


Prior frontier model (uncached)


Figure 3.


Cost per 1,000 reply classifications, comparing Nemotron 3 Ultra against the prior frontier model with and without prompt caching.


### 2. Codebase indexing


Great code review depends on context. A reviewer who knows how the whole system fits together gives far better feedback than one looking at a single file in isolation. To give Greptile that context, we run an agent that continuously crawls each customer's codebase and maintains a living internal wiki, a structured map of what the code does and how its pieces relate.


\[ FIG. 04 / CODEBASE INDEXING: KEEPING GREPTILE'S MAP FRESH \]


Triggers


Code merged to main


Continuous scheduled crawl


Indexing agent


crawls every file in the repository, file after file


Beta tested: Nemotron Ultra


Reads and summarizes code into structured knowledge.


Efficient at scale.


Living internal wiki


A structured map of what the code does and how its pieces relate


Fresher context feeds better reviews & answers


Step evaluated with NVIDIA Nemotron Ultra (pre-release)


Figure 4.


How the codebase map stays current. The indexing agent crawls the repository, the model turns code into structured knowledge, and the result loops back into better reviews and answers.


This is heavy, repetitive work that runs constantly across thousands of repositories, so cost and throughput matter enormously. In testing, Nemotron 3 Ultra's efficiency changed the math: it indexed faster and at a fraction of the cost of our current model. That kind of efficiency would let us keep each customer's map fresher and refresh it more often, and fresher context flows straight back into better reviews and better answers.


\[ FIG. 05 / CODEBASE INDEXING: THROUGHPUT & COST (PRIOR MODEL = 100) \]


Indexing throughput


Files indexed per minute (indexed)


0


25


50


75


100


125


150


175


200


100


173


Prior model


Nemotron Ultra


Cost per index


Cost per full index (indexed)


0


20


40


60


80


100


120


100


38


Prior model


Nemotron Ultra


Figure 5.


Indexing throughput and cost per full index in beta testing, relative to our current production model (set to 100).


## What it means for developers


Evaluating the right model for each job is not an academic exercise. It shows up directly in the experience developers have with Greptile, across three things they feel every day.


**Accuracy.** In testing, the reply classifier read developer intent correctly across our scenarios, so Greptile answers genuine questions and stays out of threads meant for a teammate. Combined with fresher codebase indexing, its review feedback reflects how the code actually works today, not a stale snapshot.


**Latency.** Classification decisions came back in about three seconds, so a follow-up in a pull request feels like a conversation rather than a wait. Faster indexing means the codebase map keeps up with the code.


**Cost.** Roughly 4.5x cheaper reply classification and substantially cheaper indexing keep Greptile fast and affordable at the scale of 22,000 teams. Efficiency at the model layer is what lets us offer senior-level review broadly rather than as a premium add-on.


This is the payoff of a multi-model architecture: we match each job to the model that does it best, and adopt better models as they arrive without re-platforming. In our evaluation, Nemotron 3 Ultra cleared the bar we set for every model we consider, faster, cheaper, and accurate compared to the models it was benchmarked against. We are excited about what it could mean for Greptile as Nemotron 3 Ultra moves toward general availability.
