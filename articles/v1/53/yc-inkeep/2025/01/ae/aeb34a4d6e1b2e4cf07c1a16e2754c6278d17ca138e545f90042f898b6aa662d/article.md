---
schema_version: "1.0.0"
document_id: "aeb34a4d6e1b2e4cf07c1a16e2754c6278d17ca138e545f90042f898b6aa662d"
company_key: "yc-inkeep"
company: "Inkeep"
source_id: "yc-inkeep-rss-006a915c529f"
canonical_url: "https://inkeep.com/blog/anthropic-s-guide-to-ai-agent-evals-what-support-teams-need"
published_at: "2025-01-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:13.852300+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:b039126060daa60c9c1e01cb635147848f1b427bd134cacdc41431c249d3722b"
---

# Anthropic's Guide to AI Agent Evals: What Support Teams Need to Know

**What's New**


Anthropic published a comprehensive guide to evaluating AI agents, sharing field-tested methods from their work with customers building coding agents, research agents, and conversational AI.


The core insight: teams without evals get stuck in reactive loops—catching issues only in production, where fixing one failure creates others.


**Why It Matters**


If you're building AI-powered support or developer tools, evaluation infrastructure determines how fast you can ship improvements.


Without evals, debugging is reactive: wait for complaints, reproduce manually, fix the bug, hope nothing else breaks. You can't distinguish real regressions from noise or test changes against hundreds of scenarios before shipping.


With evals, new model releases become opportunity instead of risk. Anthropic notes that teams with evals can upgrade models in days while competitors without them face weeks of manual testing.


**The Big Picture**


AI agents are fundamentally harder to evaluate than traditional chatbots. They operate over many turns, call tools, modify state, and adapt based on intermediate results.


Anthropic's Opus 4.5 demonstrated this when it "failed" a flight-booking eval by discovering a loophole in the policy—technically wrong per the test, but actually a better solution for the user.


Frontier models have progressed from 40% to over 80% on SWE-bench Verified in just one year. Evals that worked six months ago may already be saturated.


**How to Build Agent Evals: Anthropic's Playbook**


*Start smaller than you think*


You don't need hundreds of tasks. Anthropic recommends 20-50 simple tasks drawn from real failures. Early changes have large effect sizes, so small sample sizes suffice.


Convert user-reported failures into test cases. If you're in production, your bug tracker and support queue are your best source material.


*Design tasks that two experts would grade identically*


Ambiguity in task specs becomes noise in metrics. Everything the grader checks should be clear from the task description. A 0% pass rate across many trials usually signals a broken task, not an incapable agent.


Create reference solutions for each task—known-working outputs that prove the task is solvable and graders are correctly configured.


*Choose graders strategically*


Three types: code-based, model-based, and human.


**Code-based graders** are fast, cheap, and reproducible. Use them for unit tests, state verification, and tool call validation. They're brittle to valid variations but objective.


**Model-based graders** handle nuance and open-ended tasks. Anthropic recommends clear rubrics, isolated judges for each dimension, and regular calibration against human experts.


**Human graders** set the gold standard but don't scale. Reserve them for calibrating model-based graders and subjective outputs.


*Grade outcomes, not paths*


Anthropic found that checking specific tool call sequences is too rigid. Agents regularly find valid approaches designers didn't anticipate. Grade what the agent produced, not the path it took.


Build in partial credit. A support agent that identifies the problem correctly but fails to process a refund is meaningfully better than one that fails immediately.


*Read the transcripts*


You won't know if graders work without reading transcripts from many trials. When a task fails, the transcript tells you whether the agent made a genuine mistake or your graders rejected a valid solution.


**What's Next**


This framework applies directly to AI support systems. If you're building customer-facing AI assistants, start with these three actions:


1.


**Audit your current feedback loop.** Are you catching issues only after users complain? That's the reactive loop Anthropic describes.


2.


**Extract 20 tasks from your support queue.** Real failures make better evals than synthetic scenarios.


3.


**Define success criteria before building.** Two engineers reading the same spec often interpret edge cases differently. Evals resolve this ambiguity.


As Anthropic notes: "Evals get harder to build the longer you wait. Early on, product requirements naturally translate into test cases. Wait too long and you're reverse-engineering success criteria from a live system."


The teams that invest in eval infrastructure now will ship faster when the next model drops.
