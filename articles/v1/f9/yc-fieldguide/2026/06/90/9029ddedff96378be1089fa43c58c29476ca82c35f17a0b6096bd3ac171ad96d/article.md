---
schema_version: "1.0.0"
document_id: "9029ddedff96378be1089fa43c58c29476ca82c35f17a0b6096bd3ac171ad96d"
company_key: "yc-fieldguide"
company: "Fieldguide"
source_id: "yc-fieldguide-rss-652d61b18f53"
canonical_url: "https://www.fieldguide.io/blog/field-orchestrator"
published_at: "2026-06-08T05:29:12+00:00"
first_seen_at: "2026-08-18T01:47:26.942382+00:00"
fetched_at: "2026-08-18T01:47:28.276336+00:00"
content_hash: "sha256:22549a3b790ea149c338679c13144583ee5c62cca9bfad12811352251890c585"
---

# Introducing Field Orchestrator: The Intelligence Layer for Financial Audit

*Field Orchestrator is now live in Fieldguide's Financial Audit product. The conversational interface to the Agent Workforce, built to execute substantive testing end-to-end.*


Every auditor knows the shape of substantive testing. Select a population, determine the sample, gather evidence, link the right documents to the right rows, extract the values that matter, compare them against what was recorded, surface the exceptions. The logic is not complicated. The execution is. And for most teams, the execution is still almost entirely manual.


That is not because better tools have not existed. It is because the tools that existed required a human to drive every step individually: trigger this action, check that output, move to the next procedure. The auditor was the orchestrator. They were also the one doing the work. That is a hard combination to scale, and it is why the time between evidence collection and a completed workpaper has stayed stubbornly long.


Field Orchestrator, now live in Fieldguide's Financial Audit product, changes what that execution looks like. The auditor directs the work. Field Orchestrator coordinates the testing agents that execute it.


## What Field Orchestrator coordinates


Field Auditor is one of the Field Agents inside Fieldguide's Financial Audit platform. It contains a roster of specialized testing agents, each built to perform discrete pieces of substantive audit work: requesting evidence, extracting data, linking documents, and evaluating results. Field Orchestrator sits above them as the coordinating layer, translating what an auditor asks into directed action across those agents.


## What Field Orchestrator actually does


Field Orchestrator is the conversational interface to Field Auditor in Financial Audit. When an auditor is working through a substantive test, they direct Field Orchestrator through the steps: planning the test, what to extract, which documents to link, and how to evaluate the results. Field Orchestrator takes those instructions, coordinates the specialized testing agents underneath it, and executes the work.


This is the chain of command in practice. Practitioners direct through Field Orchestrator. Field Orchestrator coordinates Field Auditor. Field Auditor's testing agents execute the work.


The distinction matters because it is what separates a conversational interface from an agentic one. Field Orchestrator does not just answer questions about a sheet. It takes a multi-step plan, breaks it into tasks, dispatches them to the right testing agents, and handles the sequencing. When it needs clarification from the auditor before proceeding, it asks. When a step is complete, it moves to the next one. The auditor reviews the results and applies judgment. The execution runs.


## The substantive testing workflow, inside one workspace


Substantive testing in financial audit has a clear shape: identify the population, analyze it, build the sample, collect evidence, test, document. What has historically made this hard in a platform context is that each step requires different data, different actions, and often different tools. Teams piece it together.


Field Orchestrator is built around the full workflow, inside a single testing workspace. When an auditor opens an audit program in Fieldguide, the testing workspace opens with it. Field Orchestrator loads the context of the engagement, prompts the auditor for the population file, and then works through the steps from there.


Population analysis comes first. The auditor instructs Field Orchestrator on what to analyze, Field Orchestrator executes the analysis and surfaces the results in an updated document. From there, the auditor confirms the sample parameters: account balance, materiality, risk level, sampling approach. Field Orchestrator calculates the sample size, creates the sample sheet, and links it to the workspace.


Once evidence files are collected, the substantive testing plan executes from there. For each document linking step, Field Orchestrator prompts the auditor for the relevant documents. After linking, the extraction and evaluation work runs. The auditor reviews the output at each major step before proceeding.


## The first test case: Search for unrecorded liabilities


The first substantive test Field Orchestrator is built to execute end-to-end is Search for Unrecorded Liabilities. It is a useful starting point because it captures the complexity of real financial audit testing in a single procedure.


The assertion being tested is completeness: did the company record all liabilities it owed as of the cutoff date? The procedure involves inspecting payments made after the cutoff date that relate to obligations incurred before it, then determining whether those obligations were properly recorded in the books. The evidence is bank statements, vendor invoices, and payment records from the period following cutoff.


What makes this test genuinely complex is not the logic. It is the volume and variety of the evidence, the need to match documents across multiple categories, the cutoff judgment calls that cannot be reduced to a simple value comparison, and the requirement that exceptions be flagged with enough context for a reviewer to understand why. That is the kind of testing that previously required a senior auditor to drive every step.


Field Orchestrator executes the procedure. The auditor reviews the exceptions and signs off on the conclusion.


## Human-in-the-loop by design


One of the meaningful architectural decisions in Field Orchestrator is where it pauses. The agent does not run silently to completion and hand the auditor a finished workpaper. It stops at each major step for human review. The auditor can confirm the plan before execution begins, review outputs after each significant phase, and give follow-up instructions before the next step runs.


This is not a limitation of the current version. It is the model. The professional responsibility for the conclusions in a workpaper belongs to the auditor, not the platform. Field Orchestrator is built so that the human is always in a position to exercise that judgment, at the right moments, without having to supervise every individual action in between.


## Why this matters for Financial Audit specifically


The capacity problem in financial audit is structural. The volume of substantive testing required on a complex engagement has not decreased. The number of experienced auditors available to perform it has not grown fast enough to keep pace. The result is that senior staff spend time on procedure execution that should be spent on the judgment calls that actually require their experience.


Field Orchestrator is built to shift that. The procedure execution runs. The senior auditor is reviewing results, evaluating exceptions, and advising on conclusions. That is the work that earns the fee. That is the shift the new operating model makes possible.


Field Orchestrator is live in Fieldguide's Financial Audit product. The firms that put it to work this year will enter 2027 with a materially different picture of what their teams can accomplish in a busy season.


See Field Orchestrator in action on a real substantive test.


[Book a demo](https://www.fieldguide.com/demo) and walk through the workflow with your engagement structure in mind.


Dhruv Dhingra


VP, Product at Fieldguide


## Related posts


[View All Posts chevron_right](https://www.fieldguide.com/blog)


- [Announcing Tax Export: A Seamless Bridge Between Audit and Tax September 30, 2025 News Blog](https://www.fieldguide.com/blog/announcing-tax-export-a-seamless-bridge-between-audit-and-tax)
- [Enhancements to Financial Audit with AI Automation April 16, 2025 News Blog AI](https://www.fieldguide.com/blog/new-enhancements-to-financial-audit-more-ai-automation-and-insights)
- [Reflections on Being Named to Accounting Today's Top 100 December 11, 2024 News Blog AI](https://www.fieldguide.com/blog/reflections-on-being-named-to-accounting-todays-top-100)
- [Fieldguide and Mercia Partner to Bring Trusted UK Audit Methodology to Industry-Leading Agentic AI June 4, 2026 News](https://www.fieldguide.com/blog/fieldguide-mercia-uk-partnership)
