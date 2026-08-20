---
schema_version: "1.0.0"
document_id: "db17abba24a30d89ef6d05ec0839a2a74d0f5f7779d4f1909e2b8348b0872697"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-f703c271ba19"
canonical_url: "https://www.greptile.com/blog/automating-code-validation"
published_at: "2026-07-10T12:00:00+00:00"
first_seen_at: "2026-07-21T22:07:23.426026+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:870e540f028a914a71e96219d9803faac005a25749d16b3a17379abf35aa900d"
---

# Automating code validation

People know Greptile as the AI code review company. To us, code review is a small part of a much bigger and more interesting goal, which is to automate code validation.


I want to tell you a bit about what that means, why it matters, and how we have chosen to approach the problem.


## Daytime and nighttime coding


Those of us that have software jobs and also build side projects experience two different forms of coding.


At night, coding feels like flying. Every prompt is a splatter of paint joyfully thrown at a canvas. Worlds emerge in hours and get deployed in seconds.


The daytime feels different. Carefree creativity is replaced by the cruft of code review, tedious CI pipelines, flaky E2E tests, slow deployments. Instead of flying, it feels like wading through a swamp.


Why does daytime coding feel so different from nighttime coding?


It's because production-grade code has a far higher bar for correctness than side project code.


As coding agents become very good, we're able to ship features on our side projects faster than ever before.


The impact on a team's ability to ship new features or improvements at work, however, has been relatively modest.


Most of the remaining cruft is in code validation.


## What does it mean to validate code?


At Greptile, we believe that to validate a code change, one must answer three questions:


1. Does this change violate the user contract?
2. Does this change increase future propensity for a violation of the user contract? This encompasses things like scalability and maintainability.
3. Does this change fulfill the intent expressed by the author?


We often describe our goal like this:


A large, regulated bank should be able to merge a change to flow-of-funds with only Greptile's approval. No manual code review, no tests, no QA.


This is a difficult and exciting goal. Code can be wrong in thousands of ways. It can have logical issues, security issues, performance issues. It can also be wrong in subjective ways. It may choose the wrong abstractions or make unsavory tradeoffs.


## Our plan to automate code validation


The way code has been validated for the last decade or more is roughly this:


- peer code review
- unit and integration testing
- end-to-end testing and/or QA
- static analysis, linting, and security scanning


As AI agents have become more intelligent, a flurry of startups have set out to automate each of these steps. There are AI test generators, AI QA agents, AI security scanners, and so on.


We believe that these approaches are needlessly coupled to an outdated set of primitives.


Our approach is to treat validation as one holistic problem. Bugs can't clearly be divided into, say, security and logic. Many security bugs are really logic bugs with security-related consequences. Many performance issues are really just logic bugs with latency-related consequences.


To validate a code change, Greptile has to do the following really well:


**1. Deeply understand the product, the business, the codebase, and the intent of the change**


We do this today by giving teams many ways to express context. Teams can give Greptile access to Jira/Notion/Linear. They can link Greptile to the docs in the repo.


As for the codebase, we've spent a lot of time building better ways to index a codebase. Today, Greptile generates and maintains a large internal knowledge base about every part of the codebase. What it does, how it works, how it's architected, how it's connected to other parts of the codebase, bugs that have been introduced in the past here, and potential sources of risk. A monorepo for a medium-sized company could yield hundreds of pages of content for the knowledge base. This makes Greptile much more context-aware.


**2. Understand the "blast radius" of the change**


Many changes are harmless on their own but introduce a new bug two function calls away in an unchanged part of the codebase. With the knowledge base, Greptile is very good at detecting these second order effects. It traces the data flow, traces function calls, etc. so it can not only find bugs in the diff, but bugs introduced by this change in far away parts of the codebase.


**3. Read changed files and change-related files and evaluate the architectural decisions and tradeoffs**


Frontier models are quite good at this, but they are also very expensive. We want to be efficient in how we allocate frontier inference tokens so we can provide a really great experience at a lower cost. We do this in two ways:


The first is the knowledge base, which eliminates the tens of thousands of tokens that the agent would otherwise consume re-learning how the codebase works.


The second is that for simple tasks like running` grep` and following traces which are very token-heavy but don't need frontier intelligence, we use fast open source models.


By doing this, we are able to take the excess token budget and use it to apply more frontier inference to the tasks that need it most, particularly the evaluation of complex technical decisions.


**4. Run the code and simulate users to test the application through browser/mobile agents, simulation of production traffic on backend APIs, etc.**


At its core, Greptile has to tell you whether or not a change will break something for users. Theoretically, very smart models with enough inference might be able to determine this by simply reading the code, but we find that a much more predictable and efficient way to do this is to install the dependencies, run the dev server, use browser agents to click around the app, simulate production traffic on the APIs, and write and run ephemeral test scripts.


**5. Ensure the code is secure, through a combination of agents and comprehensive security scans**


There are two extreme approaches around security scanning for code changes today. The first is the old way, static rule-based scanners that detect potential security issues. Low recall, low precision, but also low cost and high consistency. The other is brute-force inference with powerful frontier models. High recall, high precision, but also high cost and low consistency. Greptile's approach is hybrid. Let agents use deterministic scanners to reduce the entropy of the search space. Use AI to eliminate false positives and detect chained exploits.


**6. Continuously learn from other patches, other engineers' comments, etc.**


Every time a new PR is opened that appears to be patching a bug that had been merged, Greptile records that as a potential risk area for that part of the codebase, and checks future PRs with that context so no mistake is repeated.


## Why automate?


When coding agents first started taking off, people started wondering what the job of the programmer would be. The most common answer was that humans would spend their time reviewing AI-generated code. My colleagues and I found that to be dystopian. We didn't want to spend our work days reading and reviewing slop. Validation felt like the type of tractable task that should be automated, so humans can focus on creativity.


The road ahead will be challenging, but if we are able to truly automate code validation, the impact will be profound. The world's software will be better and more reliable. Coding at work will feel like coding for fun. The only bottleneck on technological progress will be the supply of brilliant ideas.


Interested in working on automating code validation with us?


We'd love to hear from you.hiring@greptile.com
