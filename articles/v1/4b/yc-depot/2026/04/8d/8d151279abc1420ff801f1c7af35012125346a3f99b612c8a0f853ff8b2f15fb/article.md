---
schema_version: "1.0.0"
document_id: "8d151279abc1420ff801f1c7af35012125346a3f99b612c8a0f853ff8b2f15fb"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/distributed-rust-fuzzing"
published_at: "2026-04-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:45:30.754431+00:00"
content_hash: "sha256:818f6bb2818afe3bbd5a8ed2c45262302311331a2185e9e22ad95c9a33f8036c"
---

# How I run distributed Rust fuzzing in GitHub Actions

With agents I’ve found myself creating a lot of tests for extra safety and peace of mind. I’ve started adding more and more sophisticated testing strategies like fuzzing.


This technique helped me find several bugs in the custom block device driver I’ve been building for Depot CI. So I’m currently a big fan.


I landed on a setup that uses` cargo fuzz` , GitHub Actions cache, and a matrix job. Here's an[example repo](https://github.com/depot/distributed-fuzzing) that demonstrates the setup that follows.


## The most annoying part of fuzzing is pretty much gone


Fuzzing is a type of test that I’d rarely set up because it felt like the upfront time was just too much. I had to write the harness, wire up CI, and keep the corpus healthy.


Running fuzzing a few times locally felt “good enough” and I could never quite find myself willing to climb the mountain of CI setup. This was despite distributed fuzzing being really well-suited to CI.


Well, nowadays I’d say that agents are pretty good at building the initial harness and I feel like my simple CI workflow for a healthy corpus means that much of the setup tax is close to gone.


All in all it’s not really so much how to *do* fuzzing but more about how to run it efficiently.


## What is fuzzing?


At a high level, fuzzing is automated testing with weird inputs.


You ask the fuzzer to exercise some code, prime the fuzzer with a few seed inputs to the code, and let it mutate them forever. You can test all sorts of stuff this way. I typically focus on API boundaries. The inputs that the fuzzer sends are its “corpus.”


So, most of the inputs are garbage. Good, that’s the whole point. The fuzzer keeps the cases that reach new code paths. It keeps pushing on whatever looks interesting or seems to take more time.


The mutating inputs and the ability to follow branching code makes fuzzing great for parsers, decoders, protocol handlers, and, for me, anything else that sees untrusted input, really.


Fuzzing naturally gets much better when it keeps its corpus and just keeps on running in CI. It gets even better when several runners explore in parallel. It is also good at finding the bugs folks have trouble inventing by hand.


Fuzzing can find problems like bad lengths, duplicate fields, giant counts, broken UTF-8, and so on. Most of these are not typically written in run-of-the-mill unit tests. The fuzzer will continually try out new inputs forever.


Another very nice property is that fuzzing keeps the interesting failures. Once it finds a crash or a weird input that reaches new code, that input becomes part of the corpus.


Over time, the corpus turns into a useful set of tests you did not have to dream up yourself.


## My setup requirements


[cargo fuzz](https://rust-fuzz.github.io/book/cargo-fuzz.html) is really great for rust projects. It wraps libFuzzer to find inputs that hit new code paths. When it finds those inputs, it then saves them into a corpus. The longer the corpus grows, the deeper the fuzzer can reach. However, if you start cold without a corpus every run, you throw away that progress. I think of a corpus as an artifact about your system that you want to keep around.


Thus, I want the corpus to survive. The[cargo fuzz book](https://rust-fuzz.github.io/book/cargo-fuzz/ci.html) shows the simplest way to run and save the corpus in CI by using Actions cache.


However, I also want runs to fan out across multiple machines and to merge all those results back together.


## Keeping the corpus around


GitHub Actions cache and artifacts work great for this. Caching is write once per key but can be searched by a cache key prefix. This way I save the corpus with a unique key per run and restore it with a prefix match:


```text
-   name  :   Restore corpus from cache
uses  :   actions/cache/restore@v4
with  :
path  :   fuzz/corpus
key  :   fuzz-corpus-
restore-keys  :   |
fuzz-corpus-
```


On restore, the` fuzz-corpus-` prefix grabs the newest cache entry. After the run, I save with a fresh key:


```text
-   id  :   ts
run  :   echo "ts=$(date +%s)" >> "$GITHUB_OUTPUT"


-   name  :   Save merged corpus to cache
uses  :   actions/cache/save@v4
with  :
path  :   fuzz/corpus
key  :   fuzz-corpus-${{ steps.ts.outputs.ts }}
```


Old fuzz corpus can age out or be removed when the cache is over size. So you get a rolling corpus history without too much extra storage.


## More runners, more coverage


A single machine can already use` -fork` . So why spread across runners at all?


Fuzzing is mostly a time game. More runners means more total executions. That usually means more chances to find new coverage.


There is also a diversity benefit. One runner can get stuck wandering in an uninteresting corner of the input space. Several runners tend to wander in different directions.


That matters more than it sounds. Coverage guided fuzzers can settle into local minima. Several machines seem to make it more likely to find a useful input. Unsurprisingly, success rates are better with more diversity.


Distributing also helps when the target is memory hungry. Fuzzing can eat memory fast. One machine runs out of room sooner than you think. I’ve lost progress before through the OOM killer. So, with multiple machines fuzzing crashes stay isolated. If one machine dies, the others keep going.


So, it feels like it “fits” CI better. Ten minutes on six machines is usually easier to justify than one CI job running for an hour. You get more total work without making one job painfully long.


If you want to search more, scale up and add more machines. You do not need a special fuzzing service for any of this to start paying off.


## Fanning out with a matrix


The fan out part is pretty simple. Start N runners with a matrix job. Each one restores the same corpus and fuzzes for a fixed time:


```text
jobs  :
fuzz  :
runs-on  :   depot-ubuntu-latest
strategy  :
fail-fast  :   false
matrix  :
shard  : [  0  ,   1  ,   2  ,   3  ]
steps  :
-   uses  :   actions/checkout@v4


-   name  :   Restore corpus from cache
uses  :   actions/cache/restore@v4
with  :
path  :   fuzz/corpus
key  :   fuzz-corpus-
restore-keys  :   |
fuzz-corpus-


-   name  :   Run fuzzer
run  :   |
mkdir -p fuzz/corpus new_findings/${{ matrix.shard }}
cargo fuzz run my_target new_findings/${{ matrix.shard }} fuzz/corpus -- \
-max_total_time=600 \
-fork=$(nproc)


-   name  :   Upload new findings
if  :   always()
uses  :   actions/upload-artifact@v4
with  :
name  :   findings-${{ matrix.shard }}
path  :   new_findings
if-no-files-found  :   ignore
```


Each shard starts from the same place, but libFuzzer still sends them down different paths. That is the nice part.


` -fork=$(nproc)` also uses every core inside each runner. So you get parallelism across shards and inside each shard.


## Merging the results back together


This job ties the room together. libFuzzer has a built-in` -merge=1` mode. It keeps only the inputs that add coverage.


After the shards finish, a second job downloads the findings and folds them back into the corpus:


```text
merge  :
needs  :   fuzz
if  :   ${{ always() && !cancelled() }}
runs-on  :   depot-ubuntu-latest
steps  :
-   uses  :   actions/checkout@v4


-   name  :   Restore existing corpus
uses  :   actions/cache/restore@v4
with  :
path  :   fuzz/corpus
key  :   fuzz-corpus-
restore-keys  :   |
fuzz-corpus-


-   name  :   Download all findings
uses  :   actions/download-artifact@v4
with  :
pattern  :   findings-*
path  :   all_findings/
merge-multiple  :   true


-   name  :   Merge into corpus
run  :   |
mkdir -p fuzz/corpus
cargo fuzz run my_target fuzz/corpus all_findings/* -- -merge=1


-   id  :   ts
run  :   echo "ts=$(date +%s)" >> "$GITHUB_OUTPUT"


-   name  :   Save merged corpus to cache
uses  :   actions/cache/save@v4
with  :
path  :   fuzz/corpus
key  :   fuzz-corpus-${{ steps.ts.outputs.ts }}
```


The` if: ${{ always() && !cancelled() }}` guard makes the merge job run even if one shard fails, so successful shards still contribute findings. This is what takes the distributed fuzzing results and merges them together into one output result. It’ll keep the useful cases and drop the rest.


## Saving crashes


I think when we get a crash it should be saved as an artifact.


```text
-   name  :   Upload crashes
if  :   failure()
uses  :   actions/upload-artifact@v4
with  :
name  :   crashes-${{ matrix.shard }}
path  :   fuzz/artifacts/my_target/
if-no-files-found  :   ignore
```


The` if: failure()` guard makes this run only when the fuzzer actually crashes. I usually keep crash artifacts longer because I want time to inspect them.


## Letting it run


I run this on a cron so the corpus keeps growing:


```text
on  :
schedule  :
-   cron  :   '0 */6 * * *'
workflow_dispatch  :
```


Every six hours, four runners fuzz for 10 minutes, then the merge job rolls their work forward.


That is enough time to make steady progress without turning the workflow into a budget fire. You can tune the shard count, run length, and schedule to fit your repo.


## More than one target


If you have several fuzz targets, keep each corpus in its own directory.` fuzz/corpus/\[target_name\]` already gives you that separation.


You can add targets to the matrix, or loop over targets in the merge job. This’ll keep the different corpus separate.


## Conclusion


What I like about this setup is how simple it is. It’s just` cargo fuzz` and a workflow file. There is no shared filesystem and no fuzzing service. It uses cache immutability for versioning and libFuzzer merging for deduplication.


And once it's running, it's running. The corpus grows on its own, hour by hour, without me thinking about it. Over time it turns into a set of tests I didn't have to write. Doing distributed fuzzing means you get that compounding effect faster, all inside a CI setup that is super easy to get going.


## FAQ


How does the corpus get shared across distributed fuzz runners?


It doesn't get shared in real time. Each shard starts from the same cached corpus and fuzzes independently for the duration of the run. After all shards finish, a separate merge job downloads everyone's findings and uses libFuzzer's` -merge=1` mode to fold them back into the corpus, keeping only the inputs that add new code coverage.


Why use a timestamped cache key instead of just overwriting the same key each run?


GitHub Actions cache is write-once per key. If you reuse the same key, the save step skips with a warning because the entry already exists. By appending a timestamp, each run creates a new entry. The restore step uses a prefix match (` fuzz-corpus-` ) to grab the most recent one, so you always start from the freshest corpus without manually managing versions.


Can fuzzing find bugs that unit tests typically miss?


Yes, that's mostly the point. Fuzzers are particularly good at finding edge cases in parsers, decoders, and protocol handlers: bad lengths, duplicate fields, malformed UTF-8, integer overflows triggered by giant counts. These are the kinds of inputs that are tedious to invent by hand but easy for a fuzzer to stumble into once the corpus has grown.


## Related posts


- [How to leverage GitHub Actions matrix strategy](https://depot.dev/blog/github-actions-matrix-strategy)
- [What we need from CI for agentic engineering](https://depot.dev/blog/ci-for-agentic-engineering)
- [We reverse-engineered the GitHub Actions cache so you don't have to](https://depot.dev/blog/github-actions-cache)


Chris Goller


Principal Software Engineer at Depot
