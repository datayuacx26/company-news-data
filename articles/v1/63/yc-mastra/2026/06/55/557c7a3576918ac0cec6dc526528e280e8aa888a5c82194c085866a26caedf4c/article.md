---
schema_version: "1.0.0"
document_id: "557c7a3576918ac0cec6dc526528e280e8aa888a5c82194c085866a26caedf4c"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/ai-agent-evaluation"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-24T10:57:46.286490+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:78b63a7492f7de7da0bd54e790eeada52b48f2e4102f2456ce1df29cf54b4c90"
---

# How to build AI agent evaluation that ships reliable agents

Your AI agent works in a demo. It answers the happy-path question, calls the right tool, and returns a clean response. Then you change the prompt, swap a model, or add a third tool, and something breaks in a way you cannot reproduce.


That gap between “it works on my machine” and “it works in production at scale” is where AI agent evaluation becomes essential. According to[LangChain's 2026 State of AI Agents report](https://www.langchain.com/state-of-agent-engineering) , only 52.4% of teams run offline evaluations on test sets, and just 37.3% run online evals, which means most teams are flying blind until something fails in front of a user.


An agent with 75% per-trial reliability has only a 42% chance of passing all three trials under pass^3. This guide covers what you need to build a production-grade AI agent evaluation framework: the metrics, the graders, the CI/CD integration, and the iteration loop that keeps evals useful long after your first benchmark run.


## Why AI agent evaluation is different from LLM evaluation


You already know how to evaluate an LLM: send a prompt, check the response, compute a score. Agent evaluation shares some of that DNA, but the similarities end quickly. Agents built on top of large language models operate across multiple turns, call tools, modify external state, and make autonomous decisions that compound over time. The machine learning fundamentals that underpin single-turn LLM evaluation, ground truth labels, precision/recall tradeoffs, benchmark datasets, still apply. For generative AI systems that act rather than just respond, however, those fundamentals are not sufficient on their own.


### Non-determinism and path variability


When you send the same prompt to an LLM twice, you might get slightly different wording, but the structure is predictable. When you run the same task through an agent twice, it might choose different tools, call them in a different order, and still arrive at a correct answer, or take the same path and fail for an unrelated reason. This non-determinism is not a bug; it is a property of agentic AI systems that your evaluation methodology has to account for explicitly.


[τ-bench](https://arxiv.org/abs/2406.12045) , a benchmark that simulates dynamic conversations between a language-model user and a tool-using customer service agent under domain-specific policies, was built to surface exactly this problem. It addresses non-determinism with two metrics: **pass@k** , which measures whether the agent succeeds at least once in k attempts, and **pass^k** , which measures whether it succeeds every time. An agent with 75% per-trial reliability has only a 42% chance of passing all three trials under pass^3. For customer-facing agents where consistency matters, pass^k is the metric that tells you the truth.


### Multi-step reasoning and tool use


Each decision in a multi-step task depends on the one before it, and mistakes propagate. A single LLM call is stateless; an agent run is a chain of decisions where errors compound. If a support agent misidentifies a customer’s intent in turn two, every tool call after that is wasted compute at best and a wrong action at worst.


This means evaluating agents requires inspecting the full trajectory, not just the final answer. You need to know which tools were called, in what order, with what parameters, and whether the intermediate results were used correctly. Outcome-only evaluation can miss agents that arrive at the right answer through unsafe or unreliable paths.


### Balancing autonomy with reliability


The more autonomy you give your agent, the harder it becomes to verify correct behavior across all possible paths. A coding agent might find a creative solution that passes all tests but violates a security policy. This isn’t hypothetical:[Claude Opus 4.5 once technically “failed” a τ²-bench eval](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) by discovering a policy loophole that was actually a better solution for the user than the reference answer. When your eval becomes the goal, it stops being a good eval, a dynamic worth building around explicitly.


Your agent evaluation framework needs to capture both outcome correctness and process compliance. This typically means combining outcome-based graders (did the task succeed?) with trajectory-based checks (did the agent follow acceptable paths to get there?).


## Common agent failure modes


Before you can evaluate agents effectively, you need to know how they fail. The most common failure modes include:


-


**Tool selection errors:** the agent picks the wrong tool for the task or calls tools unnecessarily, expanding context and increasing latency.


-


**Parameter extraction mistakes:** the agent selects the right tool but passes incorrect or incomplete arguments.


-


**Reasoning drift:** over long trajectories, the agent loses track of the original goal and pursues a subtask that no longer matters.


-


**Error non-recovery:** the agent encounters a tool failure or unexpected response and either halts or enters a retry loop without adapting its strategy.


-


**Policy violations:** the agent completes the task but takes actions that violate safety constraints, business rules, or compliance requirements.


Each of these failure modes requires a different evaluation strategy, which is why AI agent evaluation demands a multi-layered approach rather than a single metric.


## The structure of an agent evaluation


Your eval system is built from a small set of composable building blocks. Once you understand them, you can assemble them to match your specific agent architecture and use case.


### Defining tasks and success criteria


Every task in your eval set needs a clear instruction, an environment setup, and verifiable success criteria. Good tasks share a critical property: two domain experts reviewing the same agent run should independently reach the same pass/fail verdict. If they cannot, your task specification is ambiguous and your results will be noisy.


Each task should include a clear instruction, an environment setup, specific and verifiable success criteria, and a reference solution that proves the task is solvable and validates that your graders work correctly. Start with 20 to 50 tasks drawn from real failures and manual checks you already perform during development.


### Types of graders for agents


Graders are the logic that scores agent performance. You will typically combine three types, each with different strengths.


Grader type Methods Best for Limitations


Code-based String matching, unit tests, static analysis, tool call verification Objective, reproducible checks with clear pass/fail criteria Brittle to valid variations; limited nuance


Model-based (LLM-as-judge) Rubric scoring, natural language assertions, pairwise comparison Subjective quality dimensions, open-ended outputs Non-deterministic; requires calibration against human judgment


Human Expert review, crowdsourced judgment, inter-annotator agreement Gold-standard quality; calibrating model-based graders Expensive, slow, hard to scale


Rather than one monolithic grader, decompose evaluation into specialized graders per dimension. A shopping agent, for example, might need five separate graders, intent classification, tool selection, parameter accuracy, policy compliance, and response tone, each with its own threshold.


### End-to-end vs. component-level evaluation


Your eval strategy needs both. End-to-end evaluation runs the full agent on a complete task and grades the final outcome. Component-level evaluation isolates individual pieces, the reasoning layer, tool selection, or retrieval pipeline, and tests them independently. End-to-end evals tell you whether the agent works. Component-level evals tell you why it does not.


### Offline vs. online evaluation


You need both, and neither alone is sufficient. Offline evaluation runs before deployment against a fixed dataset. It catches regressions and validates prompt or model changes before they reach production. Online evaluation runs against real traffic and captures distribution drift, edge cases your offline dataset missed, and performance under real load.


## Key evaluation metrics


### Task performance and goal completion


Your most fundamental metric is goal completion: did the agent accomplish what you asked? Measure both binary completion (pass/fail) and partial credit. Tracking task completion rate at the granular level, by task type, by tool combination, by user segment, lets you pinpoint where your agent underperforms before those gaps surface as user complaints.


### Trajectory and path evaluation


Trajectory metrics evaluate the path the agent took, not just the destination. You check whether the sequence of tool calls was logical, whether the agent avoided unnecessary actions, and whether intermediate decisions were grounded in the available context. Trajectory evaluation is especially important for safety-critical applications: an agent that arrives at the correct answer by accessing data it should not have read is a compliance risk even if the outcome looks correct.


### Tool calling and function execution


Your tool-use metrics should cover three dimensions:


-


**Selection accuracy:** did the agent choose the right tool?


-


**Parameter accuracy:** did it pass the correct arguments based on available context? Function calling correctness, including edge cases where parameters must be inferred from implicit context, is one of the most common sources of silent failure in production agents.


-


**Sequence accuracy:** when multiple tools were needed, did the agent call them in a valid order?


Track tool call error rates separately from task completion. A high task completion rate can mask a high tool error rate if the agent is silently recovering through retries. When function calling accuracy matters, deep learning model graders can catch subtle argument errors that string-matching checks miss.


### Efficiency metrics


Efficiency tells you what it cost to get the right answer: token usage, number of turns, latency, and dollar cost per task including inference and LLM judge calls. Agents that solve tasks correctly but consume 10x more tokens than necessary are not production-viable.


### Safety, ethics, and compliance


Safety metrics cover whether your agent’s outputs and actions meet your organization’s policies: hallucination rate, policy compliance, and guardrail adherence. For agentic AI systems with broad tool access, code execution, database writes, external API calls, the cost of a compliance miss is qualitatively different from a chatbot giving a mildly unhelpful answer. Generative AI models are backed by artificial neural networks that carry significant computing power, and every inference call is an action with real-world cost. Evaluating safety means accounting for what that compute can do, not just what the output says.


## Core evaluation methodologies


Each methodology covers different ground. The table below shows when to reach for each one and what it cannot do on its own.


Methodology When to use it Key setups or variants Limitation to watch for


LLM-as-a-judge Open-ended tasks where deterministic grading is not possible Direct assessment (single output, defined scale); pairwise comparison (pick the better output); reference-guided (score against a golden reference) Non-deterministic; calibrate against human ratings and give the judge an “Unknown” escape hatch to avoid hallucinated scores


Human evaluation Calibrating LLM judges; subjective dimensions automated methods cannot capture; high-confidence edge case audits Expert review with annotation guidelines; inter-annotator agreement measurement Expensive and slow; not scalable as a primary method


Benchmark testing and golden datasets Measuring absolute capability and detecting regressions across releases Source tasks from real failures and production edge cases, not synthetic generation alone Eval saturation: a suite at 100% tracks regressions but gives no signal for improvement


A/B testing and experimentation Validating high-stakes changes against real users: model swaps, major prompt revisions Split live traffic across agent variants; compare production metrics including retention and satisfaction Requires sufficient traffic to reach statistical significance; too slow for day-to-day iteration


## How to evaluate AI agents by type


### Coding agents


Software is straightforward to verify: does the code compile, do the tests pass, did it fix the failing test without breaking existing ones? Start with deterministic graders (unit tests, static analysis, type checking) for outcome verification. Layer on model-based graders for code quality and adherence to project conventions. SWE-bench Verified and Terminal-Bench are widely used benchmarks for this agent type.


### Conversational agents


Combine state checks (was the ticket resolved?), transcript constraints (did it finish in fewer than 10 turns?), and LLM rubrics (was the tone appropriate?). Use LLM-simulated users to generate diverse interaction patterns at scale, but periodically validate against real conversations to avoid distribution drift.


### Research agents


Combine groundedness checks (are claims supported by retrieved sources?), coverage checks, and source quality checks. Research agents often work over natural language processing pipelines and document corpora where NLP-specific failure modes, like semantic drift or entity confusion, are as important to track as factual accuracy. Calibrate your LLM rubrics against domain expert judgment more frequently here, because the subjectivity of “good research” makes automated scoring drift faster.


### Tool-use and shopping assistant agents


When your agent has access to hundreds of APIs, evaluate tool orchestration specifically. Assess not just whether your agent calls the right tool but whether it avoids calling unnecessary tools, redundant calls expand context, increase latency, and escalate cost.


### Multi-agent systems


Track orchestrator-specific metrics: planning accuracy, handoff accuracy, and collaboration success rate. Human-in-the-loop review is especially important here because emergent coordination failures are hard to capture with automated metrics alone.


## Component-level evaluation


### Router evaluation


If your agent uses a router to dispatch requests to specialized sub-agents or tool groups, evaluate the router’s accuracy independently. Misrouted requests cascade into failures that look like tool or reasoning problems but originate in the router. Build a balanced eval set with cases where the router should route and cases where it should not, one-sided datasets produce routers that over-trigger or under-trigger. The router is often the single highest-leverage component to evaluate early, since a router error contaminates every downstream metric.


### Tool calling and parameter extraction


Evaluate at three levels: did the agent decide to call a tool when appropriate (invocation accuracy), did it select the correct tool (selection accuracy), and did it construct valid parameters (structural accuracy)? Test edge cases explicitly: tools with overlapping descriptions, required parameters that must be inferred from context, and scenarios where the agent should answer directly rather than call a tool.


### Evaluating the reasoning layer


Evaluate your reasoning layer by checking whether the agent’s plan is logically consistent, whether each step is grounded in available context, and whether the plan leads to the correct sequence of actions. Use model-based graders with structured rubrics that score task decomposition quality, logical coherence, and grounding accuracy individually. The reasoning layer in modern agents is typically a deep learning model, and machine learning evaluation techniques like consistency checks across prompts help surface instability that single-run testing misses.


### Retrieval quality (RAG)


Evaluate retrieval precision and recall independently from the generator. A perfect model cannot compensate for a retrieval pipeline that returns irrelevant documents. Measure whether retrieved chunks actually contain the information needed to answer the question, and whether the agent correctly uses retrieved context versus hallucinating beyond it.


## A roadmap for building AI agent evals from scratch


Getting your first eval suite in place comes down to six steps. Each one builds on the last.


-


**Collect tasks for the initial eval dataset.** Start with the manual checks you already run during development. What do you verify before each release? What do users report as bugs? Convert those into structured tasks with clear success criteria. Twenty to fifty tasks is a strong starting point.


-


**Design the eval harness and graders.** Your eval harness runs tasks, records transcripts, applies graders, and aggregates results. Keep it simple initially. Isolate each trial by starting from a clean environment to avoid correlated failures.


-


**Choose metrics aligned to your goals.** Select metrics that map to what you actually care about. If you are building a customer-facing agent where consistency matters, use pass^k over pass@k. If cost is a concern, track token usage and latency alongside correctness.


-


**Implement evaluation workflows.** Wire your eval harness into your development workflow. At minimum, run your full eval suite before merging prompt or model changes. Store transcripts so you can debug failures by reading what the agent actually did.


-


**Analyze results and failures.** Scores without analysis are just numbers. Read the transcripts of failed tasks. Ask whether the failure is a genuine agent mistake or a grader bug.


-


**Maintain and iterate.** Treat your eval suite as a living artifact. Graduate high-pass-rate capability evals into your regression suite. Add harder tasks to keep pushing the frontier.


## Evaluating AI agents in CI/CD and production


### Integrating evals into CI/CD pipelines


Run your regression eval suite on every pull request that changes agent behavior: prompt updates, model changes, tool modifications, configuration adjustments. Set pass rate thresholds as merge gates so regressions cannot ship without explicit override. Tier your evals by cost: fast deterministic checks belong in CI and should complete within minutes; expensive multi-turn scenarios and LLM-judge evaluations on large datasets belong in a nightly or pre-release pipeline.


### Production monitoring and alerting


Once your agent is in production, track key metrics on live traffic: task completion rate, tool error rate, latency, and token usage. Production monitoring means more than dashboards. It means setting alert thresholds for deviations from baseline and implementing anomaly detection for gradual performance drift that threshold-based alerts miss. CI/CD gates catch regressions before they ship; production monitoring catches the problems that only appear under real user behavior.


### Capability vs. regression evals


Maintain two distinct suites. Capability evals start at a low pass rate and target tasks the agent struggles with; they give you a hill to climb. Regression testing suites should have a near-100% pass rate and protect against backsliding. As you improve your agent on capability evals, tasks with high pass rates graduate to the regression suite.


### Managing evaluation costs


Track eval cost as a metric alongside agent cost. The tiering approach above, deterministic checks in CI, LLM-judge evals nightly, full benchmark runs pre-release, is the most reliable way to manage this without sacrificing coverage.


## Building AI agent evaluation with Mastra


If you are building agents in TypeScript,[Mastra](https://mastra.ai/) gives you agents, workflows, memory, evals, and observability in one framework, with a built-in model router (AI SDK providers are supported too, if you want them). Mastra ships built-in evals, traces, and guardrails as part of @mastra/core, so you define agents, workflows, and evaluation logic in one typed codebase. The workflow engine supports .then(), .branch(), and .parallel() with durable execution state, which means your evals can test suspend/resume scenarios and human-in-the-loop approval flows, not just happy paths.


Mastra's evaluation primitives let you write code-based graders, LLM-as-judge scorers, and composite metrics that combine both. You can define eval tasks as typed objects with input schemas, expected outputs, and grader configurations. Because the eval runner is part of the same framework that defines your agents, you get tight integration between your agent definitions and your test harness. Traces capture every LLM call, tool invocation, and intermediate result, which makes debugging failed evals straightforward. You can run evals locally during development, wire them into CI/CD pipelines for regression detection, and monitor production runs with the same tracing infrastructure.


The framework is open-source under Apache 2.0 and deploys to Vercel, Netlify, Cloudflare Workers, and standalone servers. It supports model routing across hundreds of models across dozens of providers through a single interface, letting you benchmark agent behavior across different LLMs without changing your eval code. For teams that need end-to-end control over agent definition, orchestration, and evaluation without stitching together separate libraries, Mastra provides all three in a single package.


[Get started with Mastra to set up your first eval suite.](https://mastra.ai/)


## Evaluation tools, frameworks, and platforms


### Open-source evaluation libraries


Several open-source options fit different team needs:


-


**Harbor:** designed for running agents in containerized environments with standardized task and grader formats. Benchmarks like Terminal-Bench ship through the Harbor registry.


-


**Langfuse:** self-hosted platform for LLM tracing, evaluation, and dataset management, suitable for teams with data residency requirements.


-


**Phoenix (by Arize):** open-source platform for tracing, debugging, and running offline and online evaluations.


-


**LangGraph:** part of LangChain’s ecosystem, provides graph-based agent orchestration with built-in tracing that pairs with LangSmith for evaluation.


-


**CrewAI:** open-source framework for multi-agent systems with role-based agent definitions. Its eval story relies on external tooling, but it is worth tracking alongside the agent frameworks you are already evaluating.


-


**Mastra:** open-source TypeScript framework that bundles agent definition, workflow orchestration, and evaluation in a single package.


### Cloud platform solutions


Braintrust, LangSmith, and Amazon Bedrock AgentCore Evaluations combine offline evaluation with production observability, experiment tracking, and pre-built scorers. They reduce setup time but introduce vendor dependencies.


### Build vs. buy


Your decision depends on how custom your evaluation needs are, how much infrastructure overhead you can absorb, and whether you need self-hosted control over your data. For most teams, the right move is to pick a framework that fits your workflow quickly, then invest your energy in the eval tasks and graders themselves, that is where your competitive advantage lives.


## Observability, tracing, and debugging agent runs


### Tracing multi-step agent execution


A trace gives you the complete record of your agent’s run: every LLM call, tool invocation, intermediate result, reasoning step, and state change. Good traces capture inputs, outputs, token usage, latency, and error states at each step. Invest in tracing infrastructure early, without traces, debugging a failed agent run means guessing which of a dozen steps went wrong. Observability at the agent level is fundamentally different from observability at the API level: a single user action can produce a tree of model calls, tool invocations, and sub-agent spawns, and you need to navigate all of them to find the root cause.


### Debugging multi-step failures


When debugging, start from the failure and work backward through the trace. Identify the first step where the agent’s behavior diverged from what you expected. Component-level evals help because they let you isolate whether the failure is in reasoning, tool selection, parameter extraction, or retrieval.


### Edge and adversarial cases


Build eval tasks that specifically target edge cases and adversarial inputs. Test what happens when tools return errors, when the user changes their request mid-conversation, when inputs include ambiguous instructions, and when prompts are designed to make the agent violate its guardrails. Your users will find edge cases you did not anticipate, adversarial evals help you find them first.


## Best practices for production agent evaluation


### Evaluation-driven development


Write evals before you write the feature. Define what success looks like for a new capability, build tasks that test it, and iterate until the agent passes. When a new model releases, running your eval suite tells you within hours which capabilities improved, which regressed, and where you need prompt adjustments. This is a problem-solving discipline more than a technical one, the engineering discipline of defining done before you start.


### High-quality datasets


Your evals are only as good as your tasks. Avoid class-imbalanced eval sets: if you only test whether the agent searches when it should, you will end up with an agent that searches for everything. Build tasks for both directions.


### Balancing automation and human review


Automate everything you can, but schedule periodic human review to keep your automated evals calibrated. Sample transcripts weekly. Dig into disagreements between your LLM judge and human reviewers. Real-world failures that humans catch during review should be converted into automated test cases.


### Enterprise governance and compliance


For regulated industries, your agent evaluation framework must produce auditable records. Store every trace, every grader result, and every human review decision. Version your eval tasks and graders alongside your agent code. The compliance stakes vary by domain. Robotic process automation, autonomous vehicles, and facial recognition systems each operate under distinct regulatory frameworks, and the evaluation rigor required scales with the consequences of failure. Artificial general intelligence remains a long-horizon target, but most teams today are building narrow agents. Even narrow agents in high-stakes domains, healthcare, finance, legal, face AGI-adjacent scrutiny: regulators are not waiting for full artificial general intelligence before writing rules about autonomous decision-making. A well-documented evaluation framework accelerates security reviews, customer due diligence, and regulatory audits.


## Common challenges and how to address them


### Judge disagreements and false positives


When judge agreement drops below your threshold, do not average the scores. Decompose your rubric into more specific dimensions, provide clearer examples for each score level, and re-calibrate against a fresh sample of human judgments. False positives are more dangerous than false negatives because they create false confidence.


### Handling non-determinism at scale


A single eval run is a sample, not a census. Run multiple trials per task and report statistical aggregates. Use pass@k and pass^k to quantify both capability and reliability. Your eval reporting should surface variance, not hide it behind an average.


### Maintaining eval relevance over time


Your eval suite will decay if you do not actively maintain it. Schedule quarterly reviews: add tasks sourced from recent production failures, and retire tasks that have been at 100% for multiple consecutive months. Generative AI capabilities are evolving faster than most teams’ eval suites, models that were meaningfully different six months ago may now behave similarly on your existing tasks, requiring harder benchmarks to reveal real differences.


## Evaluation maturity model


Your evaluation practice will evolve through recognizable stages:


-


**Stage 1, ad hoc testing:** you test manually before each release. Works for early prototyping; breaks down with multiple contributors.


-


**Stage 2, structured offline evals:** you have a curated task set with defined success criteria and automated graders. You can detect regressions and measure the impact of prompt or model changes.


-


**Stage 3, automated CI/CD and online monitoring:** evals run automatically in CI on every change. You have production monitoring with alerting and anomaly detection. You maintain separate capability and regression suites.


-


**Stage 4, continuous improvement and governance:** your eval suite is a living artifact with clear ownership. You practice evaluation-driven development. You produce auditable evaluation records that satisfy compliance requirements.


Building reliable agents requires treating evaluation as a core engineering practice, not an afterthought. Start with a small set of real tasks, define unambiguous success criteria, combine grader types to cover both objective and subjective dimensions, and integrate evals into your CI/CD pipeline. Then iterate: read transcripts, add tasks from production failures, retire saturated evals, and keep your eval suite as current as the agent it tests.
