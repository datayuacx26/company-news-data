---
schema_version: "1.0.0"
document_id: "4432b8ab4657cc72e14a948a4aa05440717277e22e414718db457862b8dc2b8e"
company_key: "yc-flower"
company: "Flower"
source_id: "yc-flower-news-import-15689314081b"
canonical_url: "https://flower.ai/blog/2026-07-06-flowerbench"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-21T20:31:29.302072+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:169815990f5cb528de2cbd4defa951af06538fe9315c3839777d86537c48b753"
---

# FlowerBench: Benchmarking AI Agents on Real Enterprise Work

AI agents have crossed an important threshold: they can answer questions, call tools, write code, and move through polished benchmark tasks with impressive speed.


Enterprise workflows are different. They depend on private context, internal tools, domain rules, and strict deliverables. To be useful, an agent has to complete the workflow, not just produce a reasonable answer.


This creates a benchmark problem: most AI agent benchmarks are either realistic but too small, or scalable but too artificial. To address it, we introduce **FlowerBench** , a benchmark designed to measure the gap between benchmark performance and real enterprise work.


## Why Is Enterprise Agent Evaluation Hard?


The reason is privacy. The most realistic tasks are usually locked inside organizations, behind walled gardens of proprietary data and internal systems that are too sensitive to centralize into a public benchmark.


The tasks that matter most often depend on:


- Proprietary data
- Internal tools
- Domain rules
- Human handoffs
- Strict deliverable requirements


That is exactly what makes them valuable, and exactly why they are hard to share.


## Introducing FlowerBench


Instead of asking enterprises to upload sensitive work to a benchmark, FlowerBench runs evaluation where the actual work already lives.


Here is the basic idea:


1. Enterprises contribute local, proprietary tasks.
2. Evaluations run inside the enterprise environment.
3. Private files, datasets, and internal context stay in place.
4. Only sanitized results are shared outside the organization.
5. Results contribute to a broader enterprise evaluation network.


This is the same principle behind Flower: move the computation to the data, not the data to the computation.


## FlowerBench for Real Enterprise Work


FlowerBench is a benchmark suite of real enterprise tasks for evaluating AI agents on high-value workflows across domains such as finance, healthcare, insurance, operations, and legal.


Each task is designed to capture real enterprise work:


- Domain expertise
- Multiple workflow steps
- Heavy use of internal tools
- Significant human time
- Strict deliverable requirements


For example, an insurance task may require an agent to extract policy terms, cleanse a statement of values, run a rating model, generate quote tables, and finalize a quote report that passes strict verifier checks.


This makes the benchmark useful beyond a single score: teams can see whether an agent completed the work, how many intermediate artifacts it produced correctly, how long it took, how many tokens it used, and what the run cost.


## The Flower Enterprise Evaluation Network


FlowerBench would not be possible without the Flower Enterprise Evaluation Network. The Network provides a privacy-preserving, opt-in layer for connecting private task environments from various organizations into a shared evaluation network. Organizations keep proprietary work inside their own environments, while users get measurable, comparable results on real workflows under real constraints.


## FlowerBench Pilot Findings


We compare agents on the[FlowerBench tasks](https://flower.ai/benchmarks/flowerbench) in terms of score, time, tokens, and cost. Our evaluation shows where each agent succeeds, where it breaks, and what it costs to get there.


The early results show a clear quality-cost distinction: higher-scoring agents are not always the fastest or cheapest.


- Codex with GPT-5.5 often gets the main logic right, but can still fail by occasionally missing literal verifier details.
- Claude Code with Claude Opus 4.7 and 4.8 can produce polished end-to-end work when aligned, but may stop early or miss literal verifier requirements.
- Gemini CLI, Co-Pilot, and Terminus2/Nemotron expose different trade-offs, some are fast or inexpensive, but they often lose points on later-stage validation or completeness.
- OpenCode, Qwen Coder, Pi, Mini-SWE-Agent, and Hermes with Qwen3.6 Plus show that lower-cost Qwen-based approaches can be competitive, but reliability depends on the agent harness.
- Kimi CLI and Hermes with Kimi K2.6 are cost-efficient and sometimes strong on individual domains, but can be slower and less consistent end-to-end.


## Join the Flower Enterprise Evaluation Network


FlowerBench is built for organizations that want agent evaluation to more closely resemble the work they actually do.


Our aim is to help enterprises assess how agents perform on their own proprietary workflows. At the same time, each contribution helps the broader community build a standardized benchmark for enterprise agent evaluation.


Our call for enterprises is open: you can contribute local, proprietary tasks, run evaluations in your own environments, keep private data and internal context in place, and help shape the next generation of enterprise-ready AI agents.


If you are interested in joining the Flower Enterprise Evaluation Network, please reach out using the form below.
