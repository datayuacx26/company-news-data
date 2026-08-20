---
schema_version: "1.0.0"
document_id: "ad0154ad8ddd7082c7dcd9820eadec6c1d73ae39683e8e4f1b5dd505bc1a1c7e"
company_key: "yc-hatchet-run"
company: "Hatchet"
source_id: "yc-hatchet-run-rss-3f16f06bd764"
canonical_url: "https://hatchet.run/blog/murphys-law"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-09T22:57:06.374584+00:00"
fetched_at: "2026-08-09T22:57:07.291413+00:00"
content_hash: "sha256:9d0987286a4375911b4d5d1df146580e3b757ea25672d46cf6ed1bb82140b297"
---

# Coding agents supercharge Murphy's Law

Alexander Belanger


Co-Founder


Hatchet


Like many others, we were substantially impacted by GitHub’s outage yesterday. Before I realized our actions were down, I was working on debugging some failures in CI, and my local Claude session was requesting to run something like this:


Loading syntax highlighting...


I rejected this: it felt like an unnecessary cudgel, and I didn’t want to place more load than necessary on GitHub’s servers. And this was *before* I realized they were down. I’m doing my part!


But it clarified some behavior we’d been seeing on our own service over the past few months—specifically, that AI agents supercharge Murphy’s Law (for those unfamiliar, paraphrased as *Anything that can go wrong will go wrong* ).


I won’t pretend to know that agents are the root cause of GitHub’s reliability issues; it’s likely a complex cultural and architectural problem.


But I do think that coding agents place unpredictable patterns of load on service providers, and this can supercharge a service’s instability—like GitHub.


Here are three examples that illustrate what I mean.


#### Hammering undocumented endpoints


We intentionally expose only a narrow subset of our REST API via our SDKs. The specifics here aren’t too important; suffice it to say there are a subset of endpoints which we only call through our frontend. These are usually complex, slow filters, and we don’t expose them via SDK because slow, long-running queries usually aren’t a good foundation to build automations on top of, which is the primary use-case for this endpoint.


But agents don’t know this. They’ll happily scan our API surface—our OpenAPI spec, like the rest of Hatchet, is public—and find the exact endpoint they need. They won’t really question whether the lack of documentation is a signal that this shouldn’t be called; they’ll test it out, and if it works, happily commit it to their automation.


At some point, this will surface as rather unusual activity and load on our API surface; higher tail latencies, more pressure on the monitoring database, etc.


There’s naturally a deeper question here: should it even be possible to call this endpoint outside of the frontend environment? But even if this wasn’t in our public OpenAPI spec, we have users who regularly search and index our open-source codebase to discover behaviors and features. We’d have to restrict this endpoint to only cookie-based sessions, which now is a breaking change.


#### Hammering deprecated endpoints


A related set of issues that we’ve seen is unusual load on endpoints which we publicly deprecated when we incremented major versions. After keeping those endpoints around for a year, we finally removed them entirely from the API surface a few months ago, after a deprecation period.


But often, LLMs are operating on stale information, which means that we occasionally see traffic spikes to these old endpoints. Worse yet, we’ve seen some coding agents run versions of our SDK which are *years* behind, where these endpoints are part of the SDK surface.


This leads to spikes in 4xx errors, which we monitor and track, and creates additional noise for our incident response team.


#### Implementing features against hidden docs pages


One last example of this, and then I’ll stop: we ship early and often, and we sometimes will ship opt-in features to certain customers. We’ll sometimes ship these behind a “hidden” docs page, which isn’t linked in our docs and isn’t indexed in our docs search. But they’re technically still discoverable.


There have been occasions when users who are self-hosting will discover these hidden pages, track down the relevant settings in our open source, turn them on, and then send issues related to the feature, creating noise for the folks working on the product features.


#### Wrapping it up


Now imagine these issues at an organization like GitHub, who have orders of magnitude more surface area and have been around nearly ten times as long.


The behavior of the average software user is going to change significantly in the coming years. It’s not worth fighting this trend, but rather, designing your API surface area and tooling around this change in behavior.
