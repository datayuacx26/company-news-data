---
schema_version: "1.0.0"
document_id: "98d2ccd2e076e969c5246461474f2ae0d01ae8ea4a1775f4bf6e88f4b187c4b1"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/why-rootly-acquired-thinkhive"
published_at: "2026-07-28T14:00:00+00:00"
first_seen_at: "2026-07-28T18:54:17.108489+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:e23641fad964192619bf0e16f18223f75090c1ae2419ee237c340876d2c801ef"
---

# Why Rootly acquired ThinkHive

A service going down trips every alarm you own. An AI agent that quietly starts giving wrong answers trips nothing. Latency is fine, the dashboard is green, and the agent has told customers the wrong refund policy. This type of situation is a reliability problem wearing a new disguise.


That is why we acquired ThinkHive, and why the team that built it is joining Rootly.


## Agents fail silently


A few years ago, reliability meant keeping your infrastructure up. That is the problem Rootly was built to solve, and we have spent every day getting better at it. Recently, the problem changed. Our customers are not just running systems anymore. They are running AI agents across support agents answering customers, internal agents touching production, coding agents writing code that ships, and more.


And those agents fail in ways the old playbook never had to handle, they hallucinate, they drift three weeks after launch because the world changed and the model did not. The failure does not announce itself, which is exactly what makes it dangerous. We kept hearing the same thing from engineering teams putting AI into production: building an agent is easy now, and building one you can trust is brutally hard. Nothing in the old toolkit tells you which one you shipped.


## What ThinkHive does


ThinkHive traces every step an agent takes, then evaluates whether the agent actually did its job, not just whether it returned a response. It does not rely on a single number. It correlates multiple signals, metrics, traces, and evals, to catch the two failures that matter most in production, hallucination and drift. It clusters those failures into patterns instead of a wall of individual complaints, proposes fixes, and validates them with shadow testing before they reach a user.


Then it does the part I care about most, it ties agent behavior to outcomes. A quality score floating on its own tells you nothing. ThinkHive connects the dip to what it cost you.


## We ship agents now, not just incident orchestration


Rootly is not only the place you run an incident. We ship AI agents into that incident, agents that help find root cause, draft the fix, and stay ahead of the next failure.


An agent that helps run your incident has to be right, and proving it is right is a reliability discipline of its own; detect when it is wrong, correlate signals to find why, fix it, and make sure the fix holds. That is the discipline ThinkHive spent years building, that we are now integrating into our products.


## What this means for Rootly's AI capablities


Every other AI SRE vendor ships you an agent and asks you to trust it. None of them can show you, with evidence, that the agent is grounded, that it has not drifted, that last week's prompt change did not quietly regress it. An AI SRE you cannot measure is a black box wearing a confident UI.


ThinkHive is how we refuse to be that. We ship AI ourselves, into our customers' incident response, which is one of the most sensitive moments an engineering team has. So we instrument our own agents the way we instrument any production system, in groundedness scoring, hallucination and drift detection across metrics, traces, and evals, regression gates, and shadow testing before anything touches a real incident. We are giving our agents SLOs and holding them to it. We will not ask customers to trust our AI to a standard we cannot evaluate ourselves.


That makes Rootly's AI SRE categorically different from everything else in the industry. It is the difference between an agent you hope is right and one we can prove is right.


## The team behind ThinkHive


Left: JJ Tang, CEO and Co-founder of Rootly Right: Nour Alkhatib CEO and Co-founder of ThinkHive


The third, and most important reason we did this is the team. ThinkHive was founded by Nour Alkhatib and Abdulwahab Omira on a conviction I share:


Using AI to judge AI is like asking the same student to mark their own exam.


Most teams think they have solved AI quality because they wired up an automated eval pipeline. Real confidence comes from tracing what actually happened and catching the failure a clean score hides.


Nour did not arrive at that from theory. While leading AI products at Instacart, where agents served millions of customers, she saw firsthand how painful and high the bar is to build agents that work. Failures were silent, and every week she and her team did forensic work just to understand why the agents were not driving better business outcomes. That recurring effort is what led her to leave Instacart and start ThinkHive with Abdulwahab, to solve the problem for other teams.


That kind of rigor, and that kind of honesty, is rare. The AI SRE space is full of companies promising to replace your most experienced engineer with a fully autonomous agent. We have never believed that, and we have said so. ThinkHive team delivers the truth about what reliable AI takes; work, traceability, and humans who stay in the loop. They are joining Rootly to lead our work on agent reliability, and I could not be happier to have them.


## What this means for you


The same rigor ThinkHive brings to understanding agent behavior, correlating active telemetry, traces, and evals into an explanation you can trust, is what makes our agents better at the hard parts of an incident, pinpointing root cause and proposing a fix you can act on with confidence. It pushes us earlier in the lifecycle too. Our agents will score the risk of a code change against a service's incident history and live telemetry before that change ever pages anyone; it determines probable incidents based on incident history, similarity, and live telemetry. With ThinkHive's evaluation engine underneath them, our agents reason from evidence. That is the AI SRE we have been building, and ThinkHive is how we get there faster.


If you have ever wanted an AI SRE you could trust to find the cause and propose the fix, instead of one you have to double-check at two in the morning, this is the work that gets us there.


Welcome to Rootly, Nour and the ThinkHive team.


Check out the[press release](https://www.businesswire.com/news/home/20260728909768/en/Rootly-Acquires-ThinkHive-to-Bring-Reliability-Engineering-to-its-AI-Agents) for further details.
