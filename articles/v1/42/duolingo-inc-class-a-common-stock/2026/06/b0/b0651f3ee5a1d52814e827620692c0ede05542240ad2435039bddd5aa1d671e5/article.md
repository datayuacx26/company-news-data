---
schema_version: "1.0.0"
document_id: "b0651f3ee5a1d52814e827620692c0ede05542240ad2435039bddd5aa1d671e5"
company_key: "duolingo-inc-class-a-common-stock"
company: "Duolingo Inc."
source_id: "duolingo-inc-class-a-common-stock-news-import-1f1e82a2742f"
canonical_url: "https://blog.duolingo.com/ai-ios-unit-test-generation-pipeline/"
published_at: "2026-06-24T12:00:24+00:00"
first_seen_at: "2026-07-25T02:01:49.609700+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:48dc65d18e958644c1a3e56572478104da0c02aacd0dfc9130a40ada2a8b637d"
---

# How we built an automated unit test generation pipeline for iOS

Duolingo ships fast. iOS releases go out weekly, our codebase grows by tens of thousands of lines a month, and a meaningful portion of new code is now LLM-generated. As code generation gets cheaper, verification becomes the bottleneck: The limiting factor is no longer how fast engineers can write code, but how fast we can confirm that code behaves correctly. Unit tests are the cheapest, fastest layer of that verification, and to keep our pace sustainable we need a lot more of them.


So we built a set of workflows that use LLMs to generate unit tests, manage the resulting PRs through their full lifecycle, and steadily expand iOS test coverage without placing a heavy burden on engineers. In ~17 weeks, the pipeline merged 250 PRs and added ~85,000 lines of test code, almost entirely autonomously.


## How we approached it


We split the project into four pieces:


Component Description


Local Validation Tune prompts, build a file selection script, and merge ~30 test PRs by running Claude Code locally


Label Trigger Devs can add a` trigger:write-tests` label to their PR so that tests are generated automatically


Scheduled Backfill A Temporal job that generates test PRs for untested files every few hours


PR Lifecycle Manager Auto-assign reviewers, auto-heal CI failures, and auto-close stale PRs


## System Architecture


The overall system is composed of multiple Temporal workflows that coordinate through a shared S3 state store and GitHub’s API. We’ll go deeper into each of these below.


## Local validation


Before building any automation, we spent a few weeks running Claude Code locally and analyzing every CI result. The goal was to answer two questions: Which files can LLMs test well, and what are the common failure modes?


### What we learned


We tracked every generated PR’s CI outcome across two early batches:


Batch Count Merged Top failure modes


Batch 1 17 8 (47%) Mock type mismatch, Swift 6 Sendable, SwiftTesting/XCTest mixing


Batch 2 40 19 (48%) Access control, missing try/throws, publisher timing, Sendable


The failures fell into a small number of categories, some of which we could systematically address. Some were prompt issues (the LLM mixing SwiftTesting and XCTest APIs). Others revealed real architectural problems in our codebase:


- **SwiftTesting vs XCTest confusion:** We rewrote our LLM rules to be explicit about which framework to use and when.
- **UserClient coupling:** Multiple repository classes took a concrete instance of the` UserClient` object instead of` UserClientProtocol` , making it impossible to inject mocks. We migrated the entire codebase to use the protocol and added a linter rule to enforce it going forward.
- **Swift 6 Sendable violations:** We added guidance to the prompt about` @Sendable` annotations and` @preconcurrency` imports.


These fixes improved CI pass rates for everyone, not just the pipeline, and the file selection heuristics we built during this phase became the scoring system used by the backfill workflow.


## The label trigger


For generating tests for new PRs (as opposed to backfilling existing code), developers can now add the` trigger:write-tests` label to any iOS PR. Here’s what happens under the hood:


A GitHub webhook fires when the label is applied, which triggers the` iOSTestGenerationForPRWorkflow` in Temporal via our Nexus gateway. The workflow fetches the PR’s changed files, then calls` CodingAgentWorkflow` to generate tests for all testable files in a single agent session. The result is a draft PR branched off the developer’s feature branch, with a comment linking back to the original PR.


The developer can then review and merge the test PR into their feature branch before their own PR lands.


## The backfill workflow


The backfill Temporal workflow is the heart of the pipeline. It runs every few hours, and its job is to identify which files need tests the most and kick off parallel test generation jobs.


### Step 1: Fetch and score files


The workflow starts by pulling the latest Xcode coverage from S3 (uploaded by CI on every merge to main). It parses coverage data for every Swift file in the repo and runs each through a scoring system that weighs four signals:


Signal Weight Description


File type 2.5x Files that follow clean MVVM patterns (e.g., ViewModels & Repositories) are upweighted. Ambiguous file types are downweighted.


Coverage gap 0.5x More uncovered lines = higher score


Size -0.3x Smaller files preferred


Module priority 0.5x Feature and Legacy libraries score highest. Core libraries score lower.


### Step 2: Filter already-tested files


The scored list then goes through three filters before anything gets generated:


1. **Open PR check:** If an existing branch already has an open PR for this file, skip it.
2. **Existing test check:** If a` *Tests.swift` file already exists in the repo, skip it.
3. **Cooldown check:** If the PR lifecycle workflow previously gave up on this file, skip it for 30 days.


After filtering, the workflow takes the top N files (currently 5 per run).


### Step 3: spawn child workflows


For each selected file, the backfill workflow starts a child` iOSTestGenerationWorkflow` . Each child calls a` CodingAgentWorkflow` , which kicks off a remote Claude Code session. The agent clones the iOS repo, reads our testing rule files (a 12-step testing process plus patterns for mock and fake generation), analyzes the source file, and generates the test file plus any required mocks and fakes. Each PR is generated with the` generated-test` label for easy filtering. Since the Temporal workers are Linux machines, the agent has to deal with the constraint of not being able to build or compile the code it writes.


The child workflows run in parallel. When each completes, the backfill workflow records the resulting PR in the S3 state store so the lifecycle workflow can pick it up.


## The PR lifecycle workflow


Generating PRs is only half the problem. If those PRs sit in a queue, accumulate merge conflicts, or never get reviewed, the pipeline produces noise instead of value. The PR lifecycle workflow runs on a schedule and acts as an automated project manager for every open` generated-test` PR.


### The decision tree


For each open PR with the` generated-test` label, the workflow evaluates a decision tree:


1. **Merge conflict?** Close the PR immediately and label it. No cooldown is set, because regenerating from a clean main branch will succeed.
2. **Stale (>14 days)?** Close the PR and set a 30-day cooldown on the source file so the backfill doesn’t re-generate it right away.
3. **CI pending?** Skip it and check again next cycle.
4. **CI green + no reviewer assigned?** Assign a reviewer and enable auto-merge.
5. **CI green + reviewer already assigned?** Skip it. The reviewer will handle it.
6. **CI red + max retries (5) exceeded?** Close the PR, label it as a failure, and set a 30-day cooldown.
7. **CI red?** Add the` fix-ci` label, which triggers another Temporal workflow to fetch CI logs and push up a commit to fix the build.
8. **pre-commit failure?** Add the` apply-pre-commit` label, which triggers a GitHub Action that runs` pre-commit run` , commits the fixes, and pushes.
9. **Rejected by the reviewer?** Set a 30-day cooldown.


### Cross-workflow coordination via S3


The backfill and lifecycle workflows need to share state. When the lifecycle workflow gives up on a PR after 5 retries, the backfill workflow needs to know not to re-select that file. We considered DynamoDB but settled on a lightweight S3-based state store. Each source file and each PR gets its own JSON record, tracking PR history, retry counts, action history, and cooldown timestamps. The backfill workflow checks these records before selecting files, and the lifecycle workflow updates them after every action.


```text
{
"file_path": "Libraries/Feature/Sessions/Sources/SessionVM.swift",
"prs": [
{
"pr_number": 64210,
"pr_url": "https://github.com/duolingo/duolingo-ios/pull/64210",
"created_at": "2026-03-15T08:00:00+00:00",
"gave_up_at": "2026-03-22T14:30:00+00:00"
},
{
"pr_number": 65102,
"pr_url": "https://github.com/duolingo/duolingo-ios/pull/65102",
"created_at": "2026-04-21T08:00:00+00:00",
"gave_up_at": null
}
]
}
```


## Results


Over 17 weeks, we generated and merged 250 test PRs, adding ~85K lines of test code and 4,460 test functions across 233 unique classes. Test coverage of our core MVVM components more than tripled. Overall MVVM coverage climbed 240%, from 9% to 30%. Repositories grew the most at 352%, with ViewModels up 203% and DataSources up 192%. The pipeline went from zero to fully automated in under two months and now runs autonomously, generating ~20 test PRs per day. Each one of those 250 PRs was reviewed and approved by a human engineer before merging. The pipeline assigns reviewers using` git blame` history, so the reviewer has appropriate context on the source file being tested.


### Growth over time


February was spent on local validation and prompt tuning, with only a handful of PRs merged. In March, the Temporal workflows went live and volume picked up sharply. By April, the pipeline was running fully autonomously.


### CI quality and self-healing


76% of merged PRs passed CI on their first attempt. For the rest, the lifecycle workflow’s auto-healing kicked in: pre-commit fixes, CI retry loops, and (where those failed) closing the PR and setting a cooldown.


## Challenges and lessons learned


### SwiftLint failures


Since our Temporal workers run on Linux hosts, the agent can’t compile or lint the code it writes before opening a PR. As a result, our most recent batch of PRs saw a 13.6% failure rate, with the vast majority caused by SwiftLint violations. (Two rules alone accounted for 62% of those failures). We’re upgrading our workflows to run on Mac instances soon, which will let the agent run SwiftLint and validate changes locally before pushing the PR up.


### Agents going off-script


We had an incident where the CI auto-fix agent modified production source files in a test PR instead of limiting changes to test files. We’re actively adding guardrails to enforce stricter rules about what files the agent can and can’t touch.


### Reviewer bandwidth


Generating tests is now the easy part. The real bottleneck is review: The pipeline can produce PRs faster than the existing review process can absorb them, and the quality of those PRs determines how quickly they move through the reviewer queue. Our focus going forward is to keep improving the quality of generated PRs through tuning prompts and adding a second reviewer agent that verifies each generated test before it reaches a human reviewer.


The pattern of “prompt → generate PR → get CI feedback → iterate” is a generalizable loop. We’ve started with unit testing, but the same loop applies to a lot of other engineering workflows: refactors, migrations, dead code cleanup. We plan to bring it to all of them. If that excites you, we’re hiring.


[SEE OUR OPEN ROLES HERE](https://careers.duolingo.com/?department=Engineering&utm_source=blog.duolingo.com&utm_medium=blog&utm_campaign=testgen_blog_062426#careers)


*Thanks to Jesse Squires, Guadalupe Aliseda-Canton, Hunter Zhang, and Zhihao Wang for all their help and the infrastructure primitives these workflows are built on.*
