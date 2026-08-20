---
schema_version: "1.0.0"
document_id: "adc9a6f73ccf351605db2ddac07dea262089dfc2147a3196ee63a19e1423accf"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/engineering/build-or-buy-a-core-ledger"
published_at: "2026-03-11T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:a672fe727232bed7140e8f74dd13f0c47989f7aa923fef968443c1c12d14efb9"
---

# Build or Buy a Core Ledger? The Tradeoffs Aren't Simple.

Building your own software can feel like a point of pride. It gives teams a sense of control, and in many cases, it is the right call.


But a core ledger is different. It isn’t your core product, but it will become foundational infrastructure that has to be correct, resilient, and explainable under pressure. It has to be maintained.


There are certainly interesting engineering challenges in building a core ledger. But there are also accounting constraints that are easy to underestimate, and they are often[shaped by principles that most product teams do not live in every day](https://www.formance.com/blog/financial-operations/ensuring-ledger-integrity) .


More importantly, there is a business cost: building a ledger is never a one-off project. It is a long-term commitment that can pull focus, roadmap, and ownership away from the product your customers actually pay for.


## The Hidden Costs of Developing a Ledger In-House


At first, a ledger can seem more tractable than it is. It is essentially a database, and for engineers who have built databases before, the shape of the problem looks familiar. But a reliable ledger is not just a well-engineered data store. It also requires a working understanding of accounting principles—how entries are structured, how corrections must be handled, and how the system needs to behave under audit.


When these aspects of your ledger project are ill-conceived or the complexity is underestimated, you’re going to run into some problems. Because of this, building a properly functioning ledger generally requires more than a couple of software engineers. Accounting and compliance experts will be needed for their expertise, as well.


Furthermore, if a team goes on to create their own ledger without a full understanding of the requirements or without thoroughly investigating what other ledger projects have incorporated (and why), they can end up with an inferior product. This leads to the need for constant code changes and quick fixes to try and work in the necessary features, and often to a lot of technical debt.


None of this means that in-house ledger builds can't be done—only that they tend to require more resources, time, and sustained attention than initial estimates suggest. If those constraints are real, there are well-tested alternatives.


## The Cost Nobody Budgets For: Focus


A core ledger is usually a prerequisite for the business you want to run, not the business itself. But building a ledger is a long-term commitment that pulls engineering time away from your core product. Once that time is pulled away, it can often be hard to get it back, and your ledger just becomes that much more technical debt.


Early on, teams often love the challenge. A ledger is a deep technical problem, and it can feel like real progress during a fast-moving go-to-market phase. But later, the incentives shift. The original builders move on. Product priorities change. And the ledger quietly becomes an internal platform that still needs to evolve, be documented, and be maintained, even when nobody is excited about it anymore.


This is how “we built it ourselves” turns into a recurring tax. Documentation is likely thin, and exists as institutional knowledge rather than being written down. New engineers inherit a system they did not design. You end up spending multiples of the original effort on maintenance and changes, for a return that is hard to justify.


The pattern is common. Startups often build internal tools early, like a homegrown project management system, and it feels great until the business evolves and the tool cannot keep up. The difference is that a ledger has real internal and external stakeholders. Auditors, finance teams, regulators, and partners expect the numbers to be right. Customers rarely care how you built it. They just expect correct accounting, even if it runs on spreadsheets behind the scenes.


## Common DIY Ledger Issues


Most DIY ledger problems fall into two categories: structural gaps baked in during design, and operational failures that surface later in production. They're related, and the latter usually follows from the former.


**Structural gaps** are decisions made early that are painful or impossible to retrofit:


- **No audit trail from the start.** Immutability, traceability, and a full audit log are not features you can add cleanly after the fact. If they weren't designed in, you'll be scrambling to reconstruct them under pressure, usually right before an audit.
- **No handling for edge cases.** Partial failures, retries, and reversals have specific accounting semantics. Treating them as engineering edge cases rather than first-class requirements leads to correctness problems that are expensive to fix later.
- **No clear path to scale.** Supporting multiple currencies, asset types, or legal entities tends to require structural decisions made at the start. Bolting them on after the fact usually means rearchitecting core parts of the system.


**Operational failures** are what you discover once the system is running:


- **Duplicate postings and missing entries.** These are often symptoms of missing idempotency guarantees or incomplete transaction modeling. Finding them requires forensic work; fixing them requires both a correction process and changes to prevent recurrence.
- **Corrections that erode transparency.** How you handle mistakes matters as much as avoiding them. Corrections that mutate existing records, rather than creating new entries, undermine the audit trail and make the ledger harder to trust over time.


We covered additional failure modes in more depth[in a previous post](https://www.formance.com/blog/engineering/how-not-to-build-a-ledger-2) .


## When Buying a Ledger is the Most Effective Solution


While capable engineers certainly can build a reliable ledger, given the right resources, it may not be worth the effort. This will depend largely on the company’s goals, staff availability, and their particular financial infrastructure needs.


Buying a pre-built ledger that has already gone through necessary troubleshooting and testing might be a better solution in several situations, such as:


**1. When you don’t have the resources needed for a reliable ledger.**


Engineering a dependable and auditable ledger requires the help of accounting and security experts, along with database architects and multiple backend engineers. Building the ledger can take several months to over a year, so labor costs alone are going to be steep. As client, compliance, and security needs inevitably change, additional time and resources must go into ledger changes. Ledger systems can continue to eat up resources, like time and money long after launch. If your resources are limited, buying a ledger might be the most effective long-term solution.


**2. Your ledger needs are urgent.**


As stated earlier, building a ledger is no simple task, and it can take a long time. If you need to meet compliance requirements quickly (MiCA, for example), buying a ledger might be your only realistic option.


**3. When you don’t have room for mistakes** .


While some software engineering projects have room for mistakes, ledgers are probably not in that group. The risks of a bad ledger are steep: Legal penalties, damaged reputation, difficulty obtaining capital or future investments, and operational disruptions. If there is any doubt about whether your team can handle a ledger project, or you’ve already had less-than-perfect audits, you may want to go with a ledger that was engineered by experts, has undergone testing, and stood the test of time.


## Formance Already Did the Troubleshooting


[Formance Ledger](https://www.formance.com/modules/ledger) is a programmable financial ledger built for scalable asset tracking. We built it by working through the same problems described in this post—and we continue to iterate on it as compliance requirements, scale demands, and edge cases evolve.
