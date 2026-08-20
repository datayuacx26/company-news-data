---
schema_version: "1.0.0"
document_id: "5aabd25249583a693e0aa10ae70aa5c544b275e11d5586993c69e2bd3326cfdc"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-step-retry-depot-ci"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:98c7e97cb1ef7789fc08b6165158031604bf65575e4055becc3281d1207ef935"
---

# Now available: Step retry in Depot CI

## The problem of flakiness


Sometimes steps are flaky.


And sometimes there is no getting around the flakiness. Let's take an integration test. There is just stuff outside of my control: network calls, natural race conditions, a browser taking longer than normal. You know, entropy.


Unit tests are supposed to be boring and deterministic. Integration tests can have an annoying sprinkling of chaos inside them.


Most annoying is when a thing fails after a bunch of other work already happened. Maybe the job installed dependencies, built the app, started services, did all the expensive stuff, and then one known-flaky step decided to fall over.


Before, I had to sit there and watch the damn thing and retry it manually.


## Introducing step retry


Now, I can tell Depot CI ahead of time: this step might need a "smack on the top of the TV."


```text
-   name  :   Run integration tests
retry  :   2
run  :   pnpm test:integration
```


` retry: 2` means, wait for it, retry this step up to two times.


For my math nerds out there, that means Depot will do its initial run of the step and then try up to two more times for that step to succeed, for a total of 3 max attempts.


The important bit: Depot does not rerun the steps before it. It retries this step.


Pretty sweet, right?


We can retry workflows. We can retry jobs. Why not steps? It is the same concept a layer deeper, and potentially saves even more time because now I only have to rerun a subset of a subset.


Credit where it is due:` nick-fields/retry` is the go-to action for this. It is simple and it does its job well.


The thing is, it is not ours.


## Why native support matters


Bringing this under the Depot CI roof does a couple of things. First, you do not have to install yet another thing that could break, update, or behave differently from the rest of your CI system. Second, it gives us room to extend it later.


Wouldn't it be cool if Depot could retry for you, if you opt into it, when we know something was a transient issue on our side? Some network glitch on one of our machines, or something else that we can identify better than your YAML can.


We're not there yet. But having step retry be native gives us a place to build from.


## Moving forward


I do not want to be prescriptive about when you should use this. You know which parts of your CI are flaky. You know which failure is "oh, that again" versus "we actually broke something."


This is an affordance to take the mental burden off retrying a thing you already know might fail for reasons outside your control.


A bunch of the pain of being a developer is death by a thousand cuts. Sometimes I get numb to those cuts and just accept them as the status quo.


At Depot, I selfishly ask myself: how can I make my life easier today? Does this have to suck?


Sometimes the answer is yes. But sometimes it's "maybe not."


This is one of those times. Step retry is an attempt to turn those 1000 cuts to 999.


## Related posts


- [Now available: Depot CI](https://depot.dev/blog/now-available-depot-ci)
- [Speed up CI by running independent steps in parallel](https://depot.dev/blog/independent-steps-in-parallel)
- [Migrating my workflows from GitHub Actions to Depot CI](https://depot.dev/blog/migrate-to-depot-ci)


Andrew "Watts" Watkins


Engineering Manager at Depot
