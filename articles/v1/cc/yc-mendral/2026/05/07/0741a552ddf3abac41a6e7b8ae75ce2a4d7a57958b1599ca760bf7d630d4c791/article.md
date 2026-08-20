---
schema_version: "1.0.0"
document_id: "0741a552ddf3abac41a6e7b8ae75ce2a4d7a57958b1599ca760bf7d630d4c791"
company_key: "yc-mendral"
company: "Mendral"
source_id: "yc-mendral-news-import-538be81b6878"
canonical_url: "https://mendral.com/blog/how-we-know-if-our-agent-is-right"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-08-10T00:32:33.388808+00:00"
fetched_at: "2026-08-10T00:32:34.981599+00:00"
content_hash: "sha256:1ba911007508ee70f2b3cea47299028414b894931fda37666310243722e3a54d"
---

# How We Know If Our Agent Is Right

How do we know our agent is right? We get the question all the time, internally and externally, and we don't have a clean answer.


In the last 60 days, our CI failure diagnosis agent ran 36,564 investigations on top of 5.7M CI jobs and 14.4 billion log lines. Median time to diagnosis was 134 seconds. Average cost per investigation was about $0.29. Completion rate was 96.6%. And we still can't tell you what its accuracy is, with one number, with a straight face.


(For context, Mendral is a team of AI DevOps agents that helps engineering teams ship faster. The agents analyze CI failures, fix flaky tests, spot performance regressions, and catch supply chain attacks before they hit production. They open fix PRs when confidence is high enough. This post is about how we evaluate one slice of that work: CI failure diagnosis.)


We don't have a public benchmark. We don't have a labeled dataset of "this CI failure had this root cause and this fix." We can't replay an investigation deterministically because the world keeps moving (the bug gets fixed, the dependency moves, the runner restarts). And every customer's repo is different.


What we have is production traffic, a handful of noisy signals, and two months of arguments inside the team about which signal to trust. The rest of this post is what we figured out.


## Why CI agent eval is harder than it looks


If you're building a coding assistant, you have benchmarks. SWE-bench gives you 2,294 GitHub issues with labeled fixes. HumanEval gives you function signatures with hidden test suites. The eval problem is mostly about being honest with the data.


DevOps agents don't have that for CI. Three reasons.


First, no public benchmark exists. A CI failure isn't a self-contained problem. It's a state of a repo, a workflow file, a set of test runs, a history of recent commits, and the operational state of the runner that produced it. There's no SWE-bench equivalent because the inputs aren't even file-shaped.


Second, multiple fixes can be correct. A flaky test can be fixed by adding a retry, by adding a sleep, by removing a race condition in the code under test, by isolating shared state between tests, or by deleting the test entirely. We've seen all five in production. They're correct in different senses, and a "good" agent should pick the right one for the team's situation, not the most aggressive one.


Third, state changes after the fact. By the time you want to grade a diagnosis, the broken branch has been fixed, the runner has been restarted, the flake has stopped firing, or the dependency has moved. The investigation can't be replayed cleanly because the inputs no longer exist.


Every eval signal we have is a proxy. Some are cleaner than others. None of them are ground truth.


## What we measure, and why each signal is noisy


Our actual signal stack, with 60-day counts:


Signal 60-day count What it tells us Noise


Auto-Remediation PR merge rate 628 PRs decided Did the team accept the fix? Captures fix-style preference, not just correctness


Dismiss-as-invalid 81 incidents Team said "this isn't a real problem" Cleanest, but rare


User-initiated reassessment 94 sessions Team said "you got this wrong, here's context" Cleanest, but rare


System-initiated reassessment 8,523 sessions Agent re-ran on new context Not a grade, but sometimes flips the diagnosis


Slack thumbs (positive/negative) 70 sessions Explicit feedback 0.14% coverage on completed sessions


The highest-volume signal (auto-remediation merge rate) is also the noisiest. The cleanest signals (dismiss-as-invalid, user reassessment) cover less than 2% of investigations.


Auto-Remediation merge rate is the closest thing we have to "did the team accept the agent's fix?" Of the 628 PRs decided in the last 60 days, 68.3% were merged. That sounds like a quality signal. The next section is about why it isn't.


Dismiss-as-invalid is when an engineer explicitly tags an insight with "this isn't a real problem." There were 81 of those in the last 60 days against 9,226 total incidents. That's a 0.88% floor on the demonstrably-wrong rate. It's a floor and not the truth, because most resolutions go untouched and many "wont_fix" or "duplicate" dismissals are also probably wrong but uninteresting.


User-initiated reassessments are the strongest "you got this wrong, here's the missing context" signal we have. There were 94 of those in 60 days against 35,305 completed sessions. High-quality but rare.


System-initiated reassessments are different. They're triggered automatically when the context around an insight changes (new occurrence, related incident, time threshold). They're not grades, they're re-investigations. But sometimes they flip the original diagnosis, which is the closest thing we have to the agent self-correcting. There were 8,523 of those in 60 days. One of them shows up in the case studies below.


Slack thumbs are what most teams reach for. We instrumented up/down feedback in Slack early. Coverage is 0.14% of completed sessions (70 events out of 35,305). Engineers don't click feedback buttons. They mark insights resolved, dismiss them, or merge the PR. We've stopped treating thumbs as a primary signal and started treating the absence of them as a UX problem.


## The first surprise: PR merge rate isn't an accuracy signal


We pulled the worst-performing customer in the dataset (1 merged out of 14 high-confidence Auto-Remediation PRs in 60 days) expecting to find a calibration failure. The agent must be over-confidently wrong on this customer's repo.


We read three of the rejected PRs in detail and found the opposite. All three diagnoses were correct.


Take this one. The agent saw 19 frontend test files failing with` ReferenceError: window is not defined` and similar DOM-globals errors. It diagnosed (95% confidence) that the Vitest config was missing` test.environment: "jsdom"` and proposed adding it to the existing` vite.config.ts` :


```text
-import { defineConfig } from "vite";
+import { defineConfig } from "vitest/config";


return {
plugins,
resolve: { ... },
server: { port: 5173 },
+    test: {
+      environment: "jsdom",
+    },
};
```


The team rejected the PR, then shipped the same fix in a new` vitest.config.ts` file instead of editing the existing config. Same diagnosis, different file.


The other two rejected PRs followed the same pattern. One was rejected because the team chose to delete the broken test file rather than fix the import path the agent identified. One was rejected because the team chose not to invest in the structural fix the agent proposed (a workflow to auto-sync` bun.lock` on bot-authored PRs), even though the agent's diagnosis was correct and the same failure keeps recurring.


So "PR rejected" doesn't reliably mean "agent was wrong." It often means "I'd prefer to handle this differently" or "we'll skip the structural fix for now." The signal contains style preference, team capacity, and codebase opinion, all mixed in with diagnosis quality.


We still measure merge rate. It's just no longer the headline metric.


## The second surprise: confidence isn't calibrated probability


Confidence scoring isn't just "ask the model for a number." We have three different schemes tuned to different surfaces (insights, CI analysis, code review), with the most engineering invested in the diagnosis path.


Before the CI analysis agent is allowed to assign a confidence score, the diagnosis has to clear a five-criteria evidence gate. The agent has to explain *why* the failure happened (not just what), point to specific logs or traces, name a file and line, propose a concrete fix, and link the fix to the changes that introduced the bug. From there, the score builds additively. Meeting all five criteria establishes a baseline. Multiple corroborating traces raise it. A direct correlation between a code change and the failure raises it more. A validated fix raises it again. The agent isn't allowed to commit a diagnosis below a hard floor.


That score gates auto-remediation downstream. By default, Mendral only opens a fix PR autonomously when confidence is at or above 90.


Even with the evidence gate, the additive scoring, and the hard floor, the resulting distribution looks like this:


Confidence Insights Resolved Dismissed Still open


60 353 154 (44%) 88 (25%) 111 (31%)


70 712 372 (52%) 150 (21%) 190 (27%)


80 1,554 798 (51%) 382 (25%) 374 (24%)


**90** **6,230** **2,566 (41%)** **2,594 (42%)** **1,070 (17%)**


100 36 30 (83%) 2 (6%) 4 (11%)


67.5% of insights crowd the confidence-90 bucket. The model commits in one place. It's not using the lower buckets except when something is genuinely unclear, in which case the resolution rates are actually similar to the 90 bucket.


At confidence 90, the dismiss rate (42%) is higher than the resolve rate (41%). At confidence 70, the dismiss rate is 21%. The model is more likely to be wrong (in the dismiss-as-invalid sense) when it claims to be more confident.


The structure we put around the score shapes when the model is *allowed* to commit. It doesn't fix the model's tendency to crowd the top bucket once it does. That's the part of calibration that prompt rules and scoring tables don't solve. The 90-bucket pile-up is a model behavior, not a prompt bug.


We show the exact confidence percentage on every insight in the dashboard, attached to the hypothesis the agent settled on at the end of root cause analysis. Engineers see the raw number. We're working on a calibration layer that grades the agent's confidence against historical resolution outcomes, so that a 90 displayed to an engineer reflects the empirical resolve rate of past 90-confidence diagnoses rather than the model's self-report. That's a future post.


## Two failures we caught


Two recent ones, both caught by system-initiated reassessment.


### Case 1: confused symptom with cause


A customer's CI was failing on jobs running self-hosted runners. The logs showed a long parade of` ERR_MODULE_NOT_FOUND` warnings around dynamic imports in a` codeowners.ts` script. The agent diagnosed (90% confidence) that the dynamic import path was broken and proposed a code fix to use absolute paths.


Eight days later, a system-initiated reassessment ran. It found four things:


1. The` ERR_MODULE_NOT_FOUND` warnings appeared exactly 52 times per job, in both passing jobs and failing jobs. The errors were caught by a try/catch and logged with` console.log` . They had no effect on job exit status.
2. Every failed job in the window carried the annotation: "The self-hosted runner lost communication with the server. Verify the machine is running and has a healthy network connection." The failing jobs ran 15 to 20 minutes before disconnecting.
3. Failed jobs varied between runs. The` codeowners.ts` code hadn't changed in months.
4. The actual cause of failures was runner-side resource exhaustion (likely OOM under heavy test load), not the import warnings.


The reassessment rewrote the insight title to start with "misattributed to codeowners.ts (actual cause: runner communication loss)" and dropped the severity. The original action plan was abandoned.


What the agent got wrong was reasoning over log noise. Visible errors in CI logs are tempting causes. Quiet infrastructure failures aren't visible in any single log line. The agent latched onto what it could see and missed what it couldn't.


The lesson: agents need to grade evidence by its co-occurrence with the failure outcome, not by its presence in the failed job's log. If a warning appears with the same frequency in passing and failing jobs, it's probably not the cause. We've added a base-rate check to the diagnosis prompt and the same case re-investigated under the new prompt now correctly identifies the runner disconnect.


### Case 2: investigated a branch as if it were main


A different customer had a Ruby/Rails repo with a service class being renamed in a draft PR. The agent investigated the failing tests on the draft branch, confidently (97%) diagnosed an "incomplete rename" (some call sites still referenced the old class name), and proposed a fix to complete the rename across the codebase.


The diagnosis sounded clean. Tests failing with` NameError` . Old class references in a job file and its spec. Action plan that swapped the references.


The system-initiated reassessment ran 8 days later, after a team member had dismissed the insight as invalid. It found three things:


1. The rename was on a draft branch, never merged to main. The old class still existed on main. The "new" class didn't exist anywhere on main.
2. Main was fine. The job spec on main correctly used the old class name. No` NameError` failures appeared in CI logs on main over the previous 14 days.
3. The actual failures on the branch were` ActiveRecord::RecordNotFound` , not` NameError` . The agent had named the wrong error class entirely. Different stack trace shape.


If the proposed action had auto-merged, it would have advanced an in-progress refactor without the original author's knowledge.


What the agent got wrong was scope. It investigated the branch's state and reasoned about it as if the changes had landed on main. The fix is to check, before recommending a change, whether the state being repaired exists on main or only on a feature branch. Lower confidence wouldn't have helped: the agent was right about the branch, just wrong to treat the branch as the canonical state.


We've added a "branch vs main" check to the investigation prompt. We're also evaluating whether to gate auto-remediation on changes-landed-on-main as a hard prerequisite, rather than relying on the model to reason about it.


## What's still hard


A few things we haven't solved.


P95 latency is climbing as we scale. Median time to diagnosis is stable around 134 seconds. P95 went from 486 seconds in January to 1,255 seconds in April. The cause is structural. As we onboard larger teams with more active repos, the agent has more historical context to correlate against on every investigation. More data per investigation, longer tail.


We work the other side of this constantly. Prompt caching cut runtime where it applies. Vectorizing the insight database made historical search faster. Each optimization frees up time that gets used by the next jump in scale. Holding P95 to a reasonable range is a permanent engineering load, not a fix we ship once and walk away from.


Feedback coverage is 0.14%, which means we don't get an explicit accept/reject on most investigations. We're working on extracting more signal from passive interactions: how long an insight stays open, whether it gets reassigned, whether the action gets edited before merge. Those are noisier than thumbs but cover 100% of sessions instead of 0.14%.


We have a replay loop, not a regression suite. When sessions go badly (flagged by user feedback or PR rejection signals), we pull the whole session: prompts, tool calls, returned tool messages, the agent's thinking trace. We replay it locally against a model to find where the reasoning broke down. That feeds back into prompt edits, tool parameter tweaks, and adjustments to what tools return to the agent. We ship improvements from this loop regularly.


What we don't have yet is the proactive version: a curated golden set of historical investigations that every prompt change runs against before it ships, with a model-graded comparison of new outputs against the original. Without it, we catch regressions on cases we've already pulled into the replay set. We don't catch them on cases we haven't.


Multiple correct answers are hard to grade. A flake fixed by adding a retry and a flake fixed by removing a race condition are both` resolved` in our system. We know they're not equivalent. We don't grade them differently yet. The right answer probably involves the agent classifying its own remediation as a workaround vs. a root-cause fix and feeding that back into the eval loop, but we haven't built it.


## What we trust, and what we don't


The honest answer to "how do we know the agent is right?" is that we don't, with a single number. We have noisy signals, two months of arguments, and a growing collection of cases where the system caught itself.


What we trust most: dismiss-as-invalid, plus user-initiated reassessment, plus system reassessments that flip the original diagnosis. Together those are fewer than 9,000 events out of 36,000 investigations, but they're the cleanest grades we have.


What we trust least: PR merge rate as an accuracy signal in isolation. The case earlier in this post showed it's measuring fix-style preference and team capacity at least as much as diagnosis quality.


What we're building next: a proactive regression set that runs on every prompt change, a calibration layer for confidence scores, and a way to extract eval signal from passive interactions that we currently throw away. If you've shipped an eval framework for an agent in production and have opinions on what worked, I'd like to hear it. We're hiring engineers who've thought about this problem.
