---
schema_version: "1.0.0"
document_id: "0a2ff074f4614771919cb5e3e0efd48c97b27774e9dafad29644a8e83ec4fec8"
company_key: "yc-thunder-compute"
company: "Thunder Compute"
source_id: "yc-thunder-compute-news-import-c1dcc13dd65c"
canonical_url: "https://www.thundercompute.com/blog/we-cut-support-tickets-by-rewriting-error-messages-for-llms"
published_at: "2025-08-20T00:00:00+00:00"
first_seen_at: "2026-07-22T16:40:14.192629+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:7916d8b8f9a8e2a12b2d2ea93db4e76c73060053c50deff55f561a781ecf5d3b"
---

# Context-Rich Error Messages are Great for LLMs

The first thing many people do when they hit a bug is ask ChatGPT. For common issues, it's great: a quick answer without having to dig through StackOverflow.


Where this breaks down is niche problems with little public context. As a startup, we don't have years of StackOverflow threads and GitHub issues, so the model often guesses while sounding confident. That sends users down rabbit holes.


We've compensated with hands-on support. Our team lives in Discord and usually replies within minutes. We run a GPU cloud platform and have seen most Linux/PyTorch/AI tooling weirdness, so can point people to the right steps fast.


That said, it's expensive. Time spent diagnosing why an image-generation model is OOM is time not spent on our product. Worse, well-meaning ChatGPT commands can push a VM into bizarre states and convince users that our platform is the issue.


## A Memorable Case


ChatGPT convinced a user their filesystem was corrupted. The real issue was a bad package name in` requirements.txt` . The user kept pasting our replies back into ChatGPT, which doubled down on the corruption theory.


After an hour, we tried something different: we replied with a carefully worded response, designed to be a prompt that would lead the model to the correct conclusion.


They pasted it, ran the right commands, and immediately confirmed the instance was fine.


## The Revelation


That led us to a broader idea: error messages can be written with enough context for an LLM to recommend the correct fix.


For example, instead of showing:


> “You are currently using 99% of your instance's CPU memory.”


We now show:


> “You are currently using 99% of your instance's CPU memory. Expand its memory by adding more vCPUs with` tnr modify --vcpus` .”


This small change eliminated support requests for OOM errors. Users also get a better experience. They avoid debugging entirely because the error message guides them to the solution.


In a way, well-designed error messages have turned ChatGPT into our front line of support, letting us get back to building.
