---
schema_version: "1.0.0"
document_id: "7bb5c8c6fab9fffedf34028c8f08a952bd9591058af8f7ef0671712fb62f2380"
company_key: "grindr-inc-common-stock"
company: "Grindr Inc."
source_id: "grindr-inc-common-stock-news-import-400bd2d70735"
canonical_url: "https://www.grindr.com/blog/using-an-agent-as-a-judge-to-fix-ai-written-unit-tests"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-21T22:09:59.703208+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:602e5e383c4472b5955f742742f3feba723dd607414eae6877ab1eba2560cf69"
---

# Using an Agent‑as‑a‑Judge to Fix AI‑Written Unit Tests

## The Problem


Does Claude constantly write bad unit tests for you?


In our repo, Claude kept producing tests with the same recurring issues:


- Using` Task.sleep` instead of` XCTestExpectation` , leading to flaky tests
- Writing far too many tests (often the same test expressed 10 different ways)
- Producing tautological tests that always pass but don't actually test anything
- Ignoring good dependency‑injection patterns


Despite trying all kinds of prompts, I couldn't get a Claude agent to reliably write solid unit tests. Reviewing its output started to feel very familiar — and very repetitive.


Every review cycle looked something like this:


- "Don't use` Task.sleep` — always prefer` XCTestExpectation` ."
- "You don't need this many tests."
- "This test isn't actually asserting anything meaningful."


Claude and I would do this dance for two or three iterations before the tests finally came out looking nice and clean 🧼


## The Aha Moment


Then it hit me: *I could automate myself out of this loop.*


What I really needed wasn't a better prompt — it was a **second agent** .


I created a **unit‑test‑reviewer agent** whose only job is to review generated tests and look for:


- Flaky async patterns
- Redundant or duplicate tests
- Tautological assertions
- Poor dependency‑injection practices


This agent doesn't rewrite tests. It simply reviews them and returns a **PASS** or **FAIL** , along with concrete reasons for any failure.


## Wiring It Together with a Skill


Once I had the two agents, I just needed to connect them. I did this using a skill called` create-unit-tests` .


Here's how the flow works:


1. The *unit‑test‑writing agent* generates tests for the current changes
2. The *unit‑test‑reviewing agent* reviews those tests
3. If the review returns **PASS** → we're done ✅
4. If the review returns **FAIL** → the failure reasons are fed back into step 1


This loop continues until the reviewing agent returns a PASS.


Crucially, the feedback is always specific and consistent — the same things I used to comment on manually, every single time.


## The Result


Hooking up two agents with a skill like this has been a huge win for the **quality** and **consistency** of the unit tests we write. It also saves me a lot of time as I no longer need to review the "first pass" of unit tests.


Next up: applying this paradigm to other parts of our workflow where human review patterns are predictable and repeatable.


‍
