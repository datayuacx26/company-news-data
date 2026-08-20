---
schema_version: "1.0.0"
document_id: "81402851079c4a300ea9dd472dfbdf34d54cc2789442af129bb5ffa54c552b80"
company_key: "yc-leena-ai"
company: "Leena AI"
source_id: "yc-leena-ai-rss-fe37033ad6de"
canonical_url: "https://blog.leena.ai/how-do-we-benchmark-an-agentic-ai-system-heres-our-approach-fully-open/"
published_at: "2026-07-20T17:13:44+00:00"
first_seen_at: "2026-07-25T11:51:01.527856+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:ccc9bb78071a4310ea2ee6c7a3bb1ff6f86e084a87601424cc7a0c7ad6deddd8"
---

# How Do We Benchmark an Agentic AI System? Here’s Our Approach, Fully Open.

### **THE INDUSTRY PROBLEM**


Every enterprise software category eventually converges on a shared way to measure quality. Databases have benchmarks for throughput and consistency. Cloud providers publish uptime SLAs against a common definition of availability. Security tools are measured against known frameworks and shared threat libraries.


[Agentic AI](https://blog.leena.ai/glossary/agentic-ai/) has no equivalent yet.


Every vendor building AI agents for the enterprise, ourselves included, makes claims about accuracy, reliability, and performance. Almost none of those claims are independently reproducible. There is no shared definition of what “accurate” means for a system that reads a document, decides which tool to call, executes a multi-step workflow across your systems, and then explains what it did. There is no common way to compare one vendor’s agent against another’s, because there is no common test.


This creates a real problem for the people responsible for buying and operating these systems. A CIO evaluating two agentic AI vendors is choosing between two black boxes, each insisting theirs is better, with no independent way to check.


We think that gap is worth closing. Not just for us. For the category.


So we built a benchmark for testing agentic AI systems, and we open-sourced it.


### **WHY THIS IS HARDER THAN IT SOUNDS**


[Benchmarking](https://blog.leena.ai/glossary/benchmarking/) a traditional software feature is straightforward: give it an input, check the output against an expected value. Benchmarking an AI agent is not, for a specific reason. An agent’s job is to make a sequence of judgment calls, not to return one deterministic answer.


Consider a workflow like reconciling a purchase order against an invoice and a receipt. A capable agent needs to parse three documents, identify discrepancies, decide whether the discrepancy is within tolerance, and either approve the match or escalate it with the right context. There is no single “correct” output string to check against. What matters is whether the agent did the right things, in a defensible order, reached the right conclusion, and left a clear trail showing why.


That is true across most of the workflows[enterprise AI](https://blog.leena.ai/glossary/enterprise-ai/) agents are built to handle: onboarding a new hire and provisioning the right systems, answering a benefits question correctly and in the employee’s own language, running a[background check](https://blog.leena.ai/glossary/background-check/) and flagging the right exceptions, resolving a learning-management query without inventing a policy that does not exist.


None of this shows up in a product demo. A demo is a curated path through a system’s best-case behavior. It tells you nothing about what happens on the other 95% of real requests, the ones with a missing field, an ambiguous phrasing, or a workflow that needs to pause and resume across multiple turns.


To evaluate this honestly, you need two things most vendors do not have: a large, varied set of realistic scenarios, and a way to grade judgment, not just output strings.


### **HOW WE APPROACH IT**


We split evaluation into two layers, because they fail in different ways and need to be measured differently.


### **AGENTIC OPERATING PROCEDURES (AOPs)**


These are the multi-step, tool-using business workflows an agent has to complete end to end. Purchase order reconciliation. New hire onboarding and system provisioning. Employee offboarding. Invoice query handling. Background check processing. Learning management queries. Each one strings together multiple tool calls, decision points, and, often, a handoff between automated steps and human approval.


### **TOOLS**


These are the individual capabilities an agent draws on inside those workflows: parsing a document, looking up an employee record, running a calculation, executing code, managing a case. A tool can work perfectly in isolation and still fail inside a workflow, if the agent invokes it at the wrong time or misreads its output. That is exactly why testing tools alone is not enough, and why we test both layers separately and together.


### **REPRODUCIBILITY BY DESIGN**


Every scenario runs against deterministic, mocked tool outputs. This matters more than it sounds like it should. If your test environment calls live systems, results get noisy for reasons that have nothing to do with the model itself: a flaky API, a slow network hop, a record that changed between runs. Mocking the tool outputs means every model sees the exact same inputs, so when one model passes a scenario and another fails it, you know that’s a real difference between the models, not a difference in what each one happened to encounter. That is what makes results comparable across models, and it is what lets anyone, including you, clone the repository, re-run a scenario, and trace a failure back to the exact input that caused it.


### **HOW WE GRADE**


Each scenario is graded by an independent LLM judge against explicit criteria, split into two categories.


**Execution-behaviour criteria** gate the pass/fail decision. Did the agent call the right tools, in a defensible order, and reach the right outcome? For the purchase order example: did it parse all three documents, correctly identify the discrepancy, and request approval before proceeding? These are written per-workflow, because “correct” looks different for onboarding than it does for a benefits question.


**General-quality criteria** are graded alongside every scenario, regardless of workflow:


- Language fidelity — did the agent respond in the language the user wrote in, and stay consistent throughout the conversation?


- Efficiency — did it reach the goal without redundant or looping steps?


- Faithfulness — is every claim in the response grounded in the actual tool outputs and context provided, with nothing fabricated?


A scenario passes only when its execution-behaviour criteria are satisfied. The quality criteria are reported alongside, because a technically correct answer delivered in the wrong language, or padded with hallucinated detail, is not actually a passing experience for the end user.


***We also report[latency](https://blog.leena.ai/glossary/latency/) at the 95th percentile, not the average, because averages hide exactly the failures that generate support tickets*** . And we estimate cost per scenario using public list pricing, because performance without a cost lens is an incomplete picture for anyone actually running these systems in production.


### **HOW TO READ THESE RESULTS**


A quick key for the tables below:


#### **Table 1: How to Read the Numbers**


**Term** **Plain meaning**


Accuracy / Pass rate


Of all scenarios, how many the agent got right end-to-end (the headline number).


Execution-behaviour pass


Did the agent


*do* the right things — call the right tools, in a defensible order, and reach the right outcome? This drives the pass/fail decision.


General-LLM pass


Did the agent’s


*writing* hold up — answer correct, on-topic, in the user’s language, no[hallucination](https://blog.leena.ai/glossary/hallucination/) ? These are softer quality checks reported alongside.


TTFT


Time to first token — how quickly the agent starts responding.


#### **Table 2: Grading Criteria**


**Criterion** **What it checks**


Execution Rubric


The run made the right tool calls (e.g. parsed all three documents; requested approval before provisioning). AOP-specific; gates pass/fail.


Response Rubric


The final user-facing answer is correct and complete (e.g. states the exact figure, presents the exception table). AOP-specific; gates pass/fail.


Language Fidelity


The answer is in the same language the user wrote in, kept consistent.


Efficiency


No redundant or looping steps on the way to the goal.


Faithfulness


Every claim is grounded in the tool outputs or provided context — no fabricated facts.


### **RESULTS AT A GLANCE**


As of this writing, we’ve run this benchmark across five frontier models: GPT-5.5, GPT-5.6 Sol, GPT-5.6 Terra, Claude Sonnet 4.6, and Claude Opus 4.7, each at low[reasoning](https://blog.leena.ai/glossary/reasoning/) effort, across 6 AOPs and 7 tools. All runs use deterministic, mocked tool outputs. Current numbers, updated as we test new models, live at github.com/shubham-leena-ai/aic-benchmarks.


### **OVERALL LEADERBOARD (macro-averaged across all AOPs and tools)**


Read gaps with care: each cell is a single run over a small scenario set (7-13 per AOP, 5-11 per tool). Differences of a few points are within run-to-run noise, these are directional signals, not a definitive ranking.


### **AOP RESULTS (pass rate per workflow)**


###


### **TOOL RESULTS (pass rate per capability)**


###


### **LATENCY BY PROVIDER (time-to-first-token, averaged across the 6 AOPs, single run per provider)**


Every GPT model we tested was substantially slower on Azure than on OpenAI directly, 35% to 54% depending on the model.


Here the picture is mixed. Sonnet 4.6 was slower on Bedrock than on Anthropic directly. Opus 4.7 was faster on Bedrock. That inconsistency is itself a useful data point: provider effects are real and often large, but not a universal law you can assume applies to every model. It has to be tested per model, not taken on faith.


### **THE FINDING THAT SURPRISED US MOST**


We expected model choice to be the dominant variable in performance. It was not the whole story. The provider layer, OpenAI versus Azure OpenAI, Anthropic versus AWS Bedrock, mattered just as much, and sometimes more, as the charts above show.


***This matters because provider choice is usually treated as a procurement or compliance decision, driven by existing cloud commitments or data residency requirements, not a performance decision. Our data says it should be treated as both*** *.* The same model, the same weights, the same prompt, can feel meaningfully slower to your employees purely because of the network path the request takes.


This is the kind of finding we expect to remain true as new model generations ship, because it is about infrastructure, not about which model currently tops a leaderboard. That is also why we are highlighting it here in detail, and being more conservative about model rankings themselves.


### **A SNAPSHOT, NOT A VERDICT**


The leaderboard above is real and current, but it will not stay accurate for long. Model rankings shift every few months. The specific numbers above will look dated within a quarter, and that’s fine, because it isn’t the point of this article.


What does not go out of date is the methodology: the workflow types, the grading criteria, the reproducibility approach. That is what we want this article to be useful for a year from now, regardless of which models are current at the time.


The current results, for whichever models we have most recently tested, are kept up to date in the repository itself:


github.com/shubham-leena-ai/aic-benchmarks


. If you’re reading this a few months from now, check there before citing the numbers above.


What we’ll say confidently, and expect to remain true: no single model wins across every workflow and every tool. Different models show different strengths depending on the type of task, and the gap between top performers is often smaller than the gap created by provider routing choice for the same model. That pattern has held steady even as the specific rankings have shifted underneath it.


### **FROM BENCHMARK TO DECISION**


This is not a purely academic exercise for us. We built this because we needed it.


Earlier this year, customer feedback told us AI Colleague was responding slower than it should. The benchmark gave us a rigorous way to test whether a different model or provider configuration would resolve it, without guessing. The data showed a configuration change that improved both accuracy and latency, and we shipped it. That decision is now out of date, because a newer model has since become available and we have already re-tested against it. That is exactly the point. The benchmark is the durable asset. Any single decision it produces has a shelf life.


### **WHAT THIS MEANS IF YOU ARE EVALUATING AI VENDORS**


If you are buying or operating an enterprise AI platform, the useful question is not “is your AI accurate.” Every vendor will say yes. Ask instead:


- Can you show me the actual scenarios you test against, not just a summary claim?


- How do you grade a multi-step workflow, not just a single response?


- Do your published results run against reproducible conditions, or against a demo environment I cannot inspect?


- How do you measure latency, at what percentile, and have you tested whether your serving infrastructure, not just your model, affects it?


- What is your process when the underlying model changes, and how do you verify nothing regressed?


If a vendor cannot answer these with something concrete, that itself is the answer.


### **THE INVITATION**


We are publishing our benchmark methodology, our scenario definitions, and our current results at


github.com/shubham-leena-ai/aic-benchmarks


. It is open. You can read exactly how each workflow is defined and graded. You can run it yourself if you want to verify our results independently. You can fork it and adapt the scenarios to test your own agentic AI vendor, including us, against workflows that actually matter to your organization.


We plan to keep updating it as new models ship, and to write about what changes and what does not. This is the first post in that series, not the last word on it.


The absence of an industry standard for testing agentic AI is not a permanent condition. It is just something nobody has built yet. We built a version of one. We would rather it get better because the industry pressure-tests it than sit unchallenged because we kept it to ourselves.


Shubham Agarwal is VP of Engineering at Leena.ai. The benchmark referenced in this article is open source at github.com/shubham-leena-ai/aic-benchmarks.
