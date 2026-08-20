---
schema_version: "1.0.0"
document_id: "3320d1bbdbca1078765bf55bf3972c0793b519e9fbe5699326649cfc841aac8d"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/actionable-ci"
published_at: "2026-02-23T12:00:00+00:00"
first_seen_at: "2026-07-20T03:32:06.876214+00:00"
fetched_at: "2026-07-28T22:19:22.533448+00:00"
content_hash: "sha256:0f149365fa2795a13ea60d749db118c57306824af7094b1fafdb89ec7f4ede92"
---

# Actionable CI

At Block, we have thousands of engineers pushing code across large, interconnected repositories. Our CI pipelines are thorough, which means when something breaks, the failure output can be overwhelming. We realized the bottleneck was not running CI, but actually understanding CI.


Our DX team asked what if CI failures came with an explanation, a root cause, and in the best case, a fix? Then they implemented Actionable CI.


## The Architecture: Three Layers of Intelligence


We did not just throw a language model at build logs and call it a day. We built a pipeline with three distinct layers, each designed to handle a different part of the problem.


### Layer 1: Static Analysis


When a CI build fails, the first step is entirely deterministic. We scan artifacts from failed jobs using static analysis rules that match common, well-known failure patterns.


These are the known knowns. Dependency conflicts, import violations, common configuration mistakes. They are fast, reliable, and do not require AI. This layer is the foundation.


This matters because language models are powerful, but they are slower and more expensive than a simple pattern match. If a failure can be identified with a regex, we do that. Static analysis handles the easy cases and produces structured signal for the next layer.


### Layer 2: LLM Analysis


After static analysis, we bundle the logs from failing jobs and send them to a language model with clear instructions to:


1. Identify the distinct issues present in the logs
2. Explain each issue in plain language, with awareness of the code changes on the branch
3. Group similar issues across multiple failing jobs


This is where the experience changes. A single broken import can cause 15 test suites to fail. Without grouping, a developer sees 15 problems. With grouping, they see one.


The analysis is fetched and cached as soon as the build fails. By the time a developer clicks into the CI results, the analysis is already there.


### Layer 3: Agentic Autofix


For certain categories of failures, we go beyond explanation and offer an autofix path.


These include compile errors, dependency violations, lint failures, and some unit test regressions.


Autofix is initiated explicitly from the CI results by the developer.


The flow looks like this:


1.


Eligibility check
When a build fails, we check whether every detected issue is a supported type and the total count is under a threshold. If any issue is not a good candidate, we skip autofix entirely.


2.


Agent invocation
The analysis from the first two layers is sent to a headless instance of Goose, with instructions to generate a fix.


3.


Draft pull request
Goose opens a draft PR with the proposed changes.


4.


Validation
CI runs on the draft PR. If it passes, we move forward. If it fails, Goose analyzes the new failure and retries, up to a configured limit.


5.


Promotion
If the PR passes and the original branch has not changed, the draft is promoted to ready for review and the developer is notified that their build has been fixed.


6.


Graceful exit
If the developer already pushed their own fix, the autofix PR is quietly closed.


## The Developer Experience


We built this directly into our CI results page, so it feels like part of the workflow, not a separate tool.


Eligible failures surface an Autofix option directly in the CI results. When a developer chooses to run it, a draft pull request is generated in the background and validated by CI. If the developer already pushed their own fix, the autofix PR is quietly closed.


For failures that are not eligible for autofix, the Results page gives developers the tools to move fast. Each issue has an` Explain` button that generates a plain-language breakdown grounded in the actual code changes and logs. A fix prompt can be copied to clipboard or launched directly into the developer's preferred AI tool. And the raw logs are always one click away.


We're quickly learning that "Agent Experience" is now a key part of Developer Experience. We have built an MCP server and agent skills that expose CI analysis and fix capabilities programmatically, so AI coding agents can iterate on CI failures without a developer needing to manually check the results. An agent can pull the failure analysis, generate a fix, and validate it against CI, all in a loop. This turns Actionable CI from a developer facing tool into infrastructure that both humans and agents can use.
