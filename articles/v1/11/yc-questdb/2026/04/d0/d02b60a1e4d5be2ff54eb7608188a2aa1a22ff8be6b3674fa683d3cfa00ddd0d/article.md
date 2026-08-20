---
schema_version: "1.0.0"
document_id: "d02b60a1e4d5be2ff54eb7608188a2aa1a22ff8be6b3674fa683d3cfa00ddd0d"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/code-review-tripled-histogram-speedup/"
published_at: "2026-04-21T00:00:00+00:00"
first_seen_at: "2026-07-23T21:54:08.144992+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:e8665f2c717edf5dfbcf9016c02f9b98bbf8fa73b08595eaef38ae360a692d46"
---

# Code review turned a 3x speedup into 8.9x (off-heap HdrHistogram in QuestDB)

QuestDB is the open-source time-series database for demanding workloads—from trading floors to mission control. It delivers ultra-low latency, high ingestion throughput, and a multi-tier storage engine. Native support for Parquet and SQL keeps your data portable, AI-ready—no vendor lock-in.


---


Community contributions to QuestDB are some of my favourite things to read as a developer advocate. They tend to be opinionated, they often poke at corners of the engine that the core team has been meaning to revisit for a while, and the back-and-forth on the PR is usually where the most interesting engineering happens.


A[recent pull request](https://github.com/questdb/questdb/pull/6502) is a great example. It takes on a hard problem: rewriting QuestDB's HdrHistogram integration as an off-heap, flyweight class so that[approx_percentile()](https://questdb.com/docs/query/functions/aggregation/#approx_percentile) can scale across worker threads without paying GC or allocator costs on the data path. The first benchmark showed a 3x parallel speedup but a single-threaded regression; after one review pass, the regression was gone and the parallel number hit 8.9x.


[HdrHistogram](https://hdrhistogram.org/) , created by[Gil Tene](https://github.com/giltene) , is a well-known histogram library that gives bounded relative error across the full value range. It works well, but the reference implementation and QuestDB's integration both live on the JVM heap. In a database that goes to some lengths to keep hot data structures off-heap, that sticks out. And HdrHistogram is not just a counts array: it carries configuration (` lowestDiscernibleValue` ,` numberOfSignificantValueDigits` ), derived bucket geometry, and a fair amount of floating point math used to map values to bucket indices. Porting all of that to off-heap, flyweight form, while preserving exact parity with the original on-heap implementation across an extensive test suite, takes serious effort. Big kudos to[Mircea Cadariu](https://github.com/mcadariu) for taking it on.


## The design


The new class is called[GroupByHistogram](https://github.com/questdb/questdb/pull/6502/files) . It follows a pattern that recurs throughout QuestDB's group-by execution: a small Java object that carries a pointer to a buffer in native memory and is "repointed" to a different buffer for every row it processes.


```text
public GroupByHistogram of(long ptr) {       this.ptr = ptr;       // load state from the off-heap header...       return this;   }
```


No per-row Java allocation, no GC pressure on the data path, and many threads can work on independent buffers without contention. That is how most of QuestDB's group-by aggregates already work;` approx_percentile()` was the odd one out.


The trade-off is that every` of(ptr)` call has to load the histogram's state from off-heap memory, and that load happens on the hottest possible path: once per group-by key switch, potentially millions of times per query.


## First benchmark


The PR includes a parallel benchmark that runs` approx_percentile()` over a partitioned table, varying the number of partitions and the number of worker threads.` master` is the existing on-heap implementation;` branch` is the new off-heap one.


Partitions Workers master (ms) branch (ms) Speedup


8 1 272.83 332.00 0.82x


8 4 261.97 151.48 1.73x


8 8 278.01 93.12 2.99x


16 8 277.70 91.61 3.03x


32 8 279.90 89.28 3.13x


64 8 282.44 91.61 3.08x


` master` is flat at around 275 ms regardless of worker count. That is because the on-heap implementation is single-threaded: QuestDB's parallel group-by data structures assume off-heap memory, so the old` approx_percentile()` could not use multiple workers at all. Moving the histogram off-heap is what unlocks parallelism in the first place.


The new off-heap implementation scales up to roughly 3x at 8 workers, which is where parallel aggregation matters most. But there is one rough edge: at a single worker, the new implementation is slower than master (0.82x). The off-heap flyweight is paying a per-row tax that the on-heap version does not pay. With enough parallelism the rewrite still wins, but a single-threaded regression is something we would prefer not to ship alongside the parallel gains. Can we keep the parallel speedup *and* remove the regression?


## What the review found


Most of the review findings were small correctness and hygiene items, plus one change that moved the benchmark.


### The hot path:[of()](https://github.com/questdb/questdb/pull/6502#discussion_r3009698578) was doing more work than it had to


In QuestDB's group-by execution model,` of(long ptr)` is called every time the engine moves to a different row. The original implementation did the straightforward thing: read everything the histogram needs from the off-heap header, then call a helper to recompute the derived bucket geometry.


That helper,` recalculateDerivedFields()` , performs a couple of` Math.log` calls, a` Math.pow` , and a` Math.ceil` to compute eight derived fields, all strictly determined by the histogram's configuration parameters (` lowestDiscernibleValue` ,` numberOfSignificantValueDigits` ).


The observation from the review:


> ` lowestDiscernibleValue` and` numberOfSignificantValueDigits` are constructor parameters. They are identical for every histogram instance created by a given` approx_percentile()` call and never change after construction.


Those derived fields are the same for every group within a single query. They can be computed once, in the flyweight's constructor, and reused. The per-row path only needs the values that genuinely vary per buffer. This is a textbook loop-invariant hoist: moving work that produces the same result on every iteration out of the loop.


(Why are configuration fields stored on the off-heap header at all if they never vary? So that any process reading the buffer in isolation can reconstruct the histogram without knowing which query created it.)


Hoisting the configuration-dependent work out of` of()` shrinks the hot path from "5 Unsafe reads + a handful of` Math.*` calls" to "3 Unsafe reads, no math":


```text
// Before: load configuration + state, then recompute derived fields.   public GroupByHistogram of(long ptr) {       this.ptr = ptr;       if (ptr != 0) {           this.lowestDiscernibleValue = Unsafe.getUnsafe().getLong(...);           this.numberOfSignificantValueDigits = Unsafe.getUnsafe().getInt(...);           this.countsArrayLength = Unsafe.getUnsafe().getInt(...);           this.bucketCount = Unsafe.getUnsafe().getInt(...);           this.highestTrackableValue = Unsafe.getUnsafe().getLong(...);           this.allocatedSize = headerSize + (countsArrayLength * 8L);           recalculateDerivedFields(); // 2x Math.log, 1x Math.pow, 1x Math.ceil       }       return this;   }
// After: configuration moved to the constructor; only state is loaded.   public GroupByHistogram of(long ptr) {       this.ptr = ptr;       if (ptr != 0) {           this.countsArrayLength = Unsafe.getUnsafe().getInt(...);           this.bucketCount = Unsafe.getUnsafe().getInt(...);           this.highestTrackableValue = Unsafe.getUnsafe().getLong(...);           this.allocatedSize = headerSize + (countsArrayLength * 8L);       }       return this;   }
```


A small change. The histogram still computes the same thing, it just no longer recomputes invariants on every key switch.


### Two smaller findings


**Header / Java field divergence in error handling.** When the buffer needs to grow due to a value above the current tracking range,` handleRecordException` triggers a` resize()` followed by writing` highestTrackableValue` into the Java field. But the resize path had already written the previous value into the off-heap header. The Java field and the header could disagree. A later` of(ptr)` from a different flyweight pointing at the same buffer would read the stale value, which could trigger redundant resizes downstream. Fix: keep the field and the header in sync.


**Redundant allocate-then-realloc in` merge()` .** When merging into an empty destination from a source whose maximum exceeds the destination's tracking range, the code first allocated at the small initial size, then immediately resized to the larger size needed for the source.` resize()` already handles the "no previous allocation" case, so the right fix was to take that path directly.


Plus a few smaller test and hygiene improvements: use` assertMemoryLeak()` in the tests so off-heap leaks fail loudly, use` TestUtils.generateRandom(LOG)` so fuzz tests can be replayed from the seed in the failure log, add tests for the` autoResize=false` path and the negative-value rejection path, and tidy a couple of stale references in comments.


## Second benchmark


With the changes applied, the same benchmark looks like this:


Partitions Workers master (ms) branch (ms) Speedup


8 1 282.36 101.67 2.78x


8 4 267.42 51.23 5.22x


8 8 282.67 33.91 8.34x


16 8 284.20 32.61 8.72x


32 8 282.83 31.78 8.90x


64 8 286.16 35.06 8.16x


The single-threaded regression is gone, replaced by a 2.78x improvement. The 8-worker case moves from a roughly 3x ceiling to roughly 8.5x. Everything in between improves in line with that.


The PR comment that came with these numbers:


> in another installment of "why is QuestDB so fast", after applying your comments, the measurements look much better!


## Takeaways


**Benchmarks often need to be ugly first.** Hiding a regression in a benchmark does not benefit anyone. The honest first table is what made the single-threaded problem visible and gave the review something concrete to aim at.


**Off-heap trades one cost for another.** Removing GC pressure and unlocking parallelism is a win, but it leaves you with a different optimisation problem.


**Reviews bring a second point of view.** The person who wrote the histogram knows its internals. The person who reviews it knows how the engine calls it. Neither perspective alone spots the redundant work on the hot path, but together they do.


If you want to dig into the code, the discussion is on[questdb/questdb#6502](https://github.com/questdb/questdb/pull/6502) . The benchmark itself lives in Mircea's[fork branch](https://github.com/mcadariu/questdb/pull/1/files) as` ApproxPercentileParallelBenchmarkTest` .
