---
schema_version: "1.0.0"
document_id: "e2602cd668acc7ddcd2880efc9b608024f4521fa1c9e2fc96ee16ec5d18ba21b"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2026-08-12-agents-randomness-and-receipts-notes-from-trivagos-qa-meetup/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T13:59:12.698129+00:00"
fetched_at: "2026-08-13T13:59:14.366478+00:00"
content_hash: "sha256:77c43c1b6cd507130e095ab6b601229c3669aed09e9d9b3d2ae546517d543bf5"
---

# Agents, Randomness, and Receipts: Notes from trivago's QA Meetup

In July, we opened the doors of our Düsseldorf campus for this year’s QA Meetup, bringing together around 90 people from testing, engineering, and beyond. Hosting this meetup has become a bit of a tradition for our QA team over the past few years, and it’s been a great success every time. This year, we had a great lineup of three speakers: Andrei Khabarov from Qase.io, Alexei Vinogradov from Curio IT, and our own Renjith Rajasekharan from the Backend QA team.


Two of the talks looked at how AI is changing software testing. One through adoption metrics, the other through an AI agent built to test API integrations. The third took a different route: randomizing input values in automated tests to catch the defects that deterministic testing tends to miss.


## Talk 1: AI-Driven Dev & QA, the metrics that prove it


[Andrei Khabarov](https://www.linkedin.com/in/akhabarow/) , AI/Frontend Engineer at[Qase.io](http://qase.io/) , presented on measuring the impact of AI adoption on software delivery. He compared Qase.io’s adoption process to levels of driving automation, describing five stages: autocomplete, chat-based assistance, agent-assisted development, an AI-first organization with a shared harness, and full autonomy. According to Khabarov, Qase.io reached the fourth stage within approximately eighteen months.


Tool access alone, he noted, did not increase usage. The main driver of broader adoption was a recurring internal knowledge-sharing session, where engineers presented tools and workflows they had built themselves.


Turning to measurement, he explained that counting AI-authored pull requests is an insufficient metric, since splitting a single change into multiple pull requests would inflate the count without reflecting real progress. To measure adoption more accurately, Qase.io built **git-ai** , an open-source tool that records structured attribution data for each commit using git notes, without storing source code or prompt content. This data feeds an internal dashboard, Devportal, which reports an AI-authored code percentage per team, ranging from 61% to 92% among the teams observed.


He then presented **DORA** metrics comparing a pre-AI baseline from January 2025 to current values. Deployment frequency increased approximately threefold, reaching the Elite category. Cycle time decreased to a median of 0.2 hours, also Elite. Change failure rate remained at 0% (Elite), and MTTR (mean time to recovery), fell to a median of 4.5 hours, roughly half the pre-AI baseline. Lead time was the exception, remaining in the Low category, a shift he attributed to the bottleneck moving from code implementation to planning and review.


Based on these metrics, Qase.io ‘s leadership approved a monthly AI tooling budget of approximately $5,000. The organization’s engineering practices are documented in a shared configuration file,` CLAUDE.md` , supplemented by six defined agent roles and eleven reusable skills, including one that maps a pull request’s changes to the QA test domains it affects.


He concluded the presentation with the following statement:


> *“Stop guessing. Start measuring.”*


## Talk 2: Randomize Your Automated Tests


[Alexei Vinogradov](https://www.linkedin.com/in/vinogradov/) , CEO of Curio IT, presented on the use of randomized input values in automated software tests, a technique independent of AI-based test generation. He described the **pesticide effect** , in which a fixed set of tests using hardcoded input values stops detecting new defects once the underlying code has been adapted to pass them. His proposed approach is to randomize input values for list-type parameters, code branching conditions, and actor attributes such as country or user role.


The talk then addressed three common objections to this approach. On reproducibility, he recommended logging all input values, seeds, and responses so that any test failure can be reproduced exactly. On flakiness, he argued that it should be treated as an expected outcome rather than a defect, since it surfaces issues that deterministic tests do not detect, and specifically advised against re-running a failed test without first investigating its cause. On coverage, he acknowledged that randomization improves coverage but does not guarantee it is complete.


To implement this in practice, he recommended a **test data broker** : a builder-pattern data generator in which a seed value determines the randomization, allowing a specific failure to be reproduced by reusing its seed. He also suggested using a configurable switch between randomized and fixed-seed modes, and repeating each test 20 to 50 times during active development rather than as part of routine CI/CD execution.


He was equally clear on when randomization is not appropriate: infrequent test suites, well-established configuration testing, or cases where coverage gaps are already well understood. On the role of AI in testing, his position was that it can generate test code or investigate failures, but that engineers remain responsible for the final decision.


## Talk 3: Agent Assisted API Testing


[Renjith Rajasekharan](https://www.linkedin.com/in/renjith-r-rajasekharan/) , QA Engineer at trivago, presented an AI agent developed to test API integrations for trivago’s advertiser connectors. He explained that with approximately 200 connectors in operation and frequent changes to response mapping, manually maintained rule-based tests cannot provide coverage for all of them.


The system he described includes a test engine that executes randomized inputs against backend services and records full response traces, and a command-line interface that allows engineers and the agent to execute identical test actions. A test planning component, which references JIRA tickets and GitHub pull requests, is governed by three reference files:` Scenarios.md` , documenting connector-agnostic test scenarios;` test_cases.md` , translating scenarios into CLI commands; and` AGENTS.md` , defining operating constraints for the agent, including a requirement to flag uncertainty rather than generate unverified test logic.


According to Rajasekharan, each test run is logged as structured JSON and validated in two stages: a mechanical check for structural correctness, covering non-zero prices, populated booking links, and correct error codes; and an LLM-based check that compares observed changes against the stated intent of the pull request. Every test is assigned one of four outcomes: PASS, WARN, FAIL, or SKIP.


He also described an auditing process applied to the agent itself, which re-runs test proposals against a fixed set of prior pull requests to check for consistency, producing a quality score between 0 and 100. Planned next steps, he noted, include cataloging additional test scenarios, adding alerting for the agent and audit pipeline, and extending validation to the advertiser-facing web interface.


## Summary


The three talks covered different approaches to software testing: measuring the impact of AI adoption using DORA metrics and commit-level attribution data; using randomized input values to identify defects that deterministic tests do not detect; and using an AI agent, with a defined auditing process, to test API integrations. What tied them together was that each speaker added a way to verify the testing process itself, while still keeping human review as part of the final decision.
