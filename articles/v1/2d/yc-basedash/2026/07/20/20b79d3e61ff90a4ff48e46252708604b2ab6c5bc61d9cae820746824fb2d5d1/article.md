---
schema_version: "1.0.0"
document_id: "20b79d3e61ff90a4ff48e46252708604b2ab6c5bc61d9cae820746824fb2d5d1"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/analytics-maturity-model-the-5-stages-and-how-to-move-up-one/"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-22T18:54:24.148984+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:c16f42707d448fefa9bda5353aa10c8d0d5d3e43097fa8f3fdb1d97026a3e6f8"
---

# The analytics maturity model: 5 stages and how to move up one

An analytics maturity model describes how a team’s use of data evolves, from ad hoc spreadsheet pulls to governed self-serve analytics and, eventually, analytics embedded in products and decisions. The point of the model is not to reach the final stage. It is to figure out which stage you are actually in, and make the one change that moves you up without skipping the work the next stage depends on.


Most maturity models are built for large enterprises and describe five or six abstract stages that take years to traverse. This one is written for startups and lean teams. It focuses on concrete symptoms you can recognize, the specific capability each stage adds, and the honest reality that most companies should aim for the middle of the model, not the top.


## What is analytics maturity?


Analytics maturity is how reliably and independently a team turns questions into trustworthy answers. A mature analytics function is not defined by how many dashboards it has or whether it uses machine learning. It is defined by four things:


- **Access:** how many people can get data without filing a request.
- **Trust:** whether two people who pull the same number get the same result.
- **Speed:** how long it takes to go from a question to a defensible answer.
- **Leverage:** whether data changes decisions, or just decorates them.


A team can have expensive tooling and low maturity, or a single well-run BI tool and high maturity. Maturity is about the workflow around the data, not the size of the stack.


## The 5 stages of analytics maturity


Here is the model. Each stage adds one capability the previous stage lacked. The stages are cumulative: you cannot run trustworthy self-serve analytics (Stage 3) if nobody agrees on what “active customer” means (a Stage 4 problem you skipped).


Stage Name Who answers data questions Source of truth Typical tooling


1 Reactive One founder or operator, ad hoc Spreadsheets and exports Google Sheets, CSV exports


2 Reporting A designated data person A set of static dashboards A BI tool, SQL, scheduled reports


3 Self-serve Anyone on the team, for their own questions Live connection to the database or warehouse Self-serve BI, shared queries


4 Governed Anyone, using shared definitions Defined metrics and a semantic layer Semantic layer, permissions, quality checks


5 Embedded The product and automated workflows Metrics wired into apps and alerts Embedded analytics, AI assistance, forecasting


### Stage 1: Reactive


At Stage 1, data lives in spreadsheets and one-off exports. A founder or an early operator pulls numbers when a decision or an investor update forces the question. There is no shared definition of anything, and the same metric can differ between two decks because two people exported it on two different days.


This stage is fine, and even correct, very early. You do not need a BI stack to run a five-person company. The problem is not the spreadsheets. The problem is that nobody can answer a data question except the one person who knows where the exports live, and that person becomes a bottleneck the moment the company starts to grow.


**Symptom you have outgrown it:** the same person is manually rebuilding the same spreadsheet every week, and other people are waiting on them to make decisions.


### Stage 2: Reporting


At Stage 2, the team adopts a BI tool and someone, often a technical founder or the first data hire, builds a set of dashboards. Numbers now come from one place and update on a schedule instead of being copied by hand. This is a real jump in reliability.


The limitation is that all knowledge routes through one person. Every new question (“can you break that down by plan?”) becomes a ticket in a queue. Dashboards answer the questions the builder anticipated, but real decisions generate follow-up questions the builder did not. The data person spends their week servicing requests instead of doing analysis.


**Symptom you have outgrown it:** there is a growing backlog of “quick data questions,” and non-technical teammates have stopped asking because the turnaround is too slow.


### Stage 3: Self-serve


At Stage 3, non-technical teammates can explore data and ask their own follow-up questions without waiting on the data person. This usually means a live connection to the production database or warehouse and a tool where someone can filter, pivot, or ask a question in plain language rather than requesting a new chart.


This is the stage where analytics starts to compound. A support lead can check refund rates, a marketer can see which channel actually converted, and neither of them files a ticket. For most lean teams, functional self-serve is the stage where data starts to pay for itself. Our guide to[self-serve analytics](https://www.basedash.com/blog/self-serve-analytics-a-practical-guide-to-bi-adoption-across-your-organization) covers how to roll this out without it descending into chaos.


The risk at Stage 3 is trust. When everyone can build their own view, everyone builds their own definition. Two teams report different revenue because one includes trials and the other does not. Self-serve without shared definitions creates confident, conflicting numbers, which is arguably worse than no self-serve at all.


**Symptom you have outgrown it:** people can get their own answers, but meetings now stall on “whose number is right?”


### Stage 4: Governed


At Stage 4, the team standardizes what its metrics mean. Core definitions (“active customer,” “MRR,” “qualified lead”) are written down once and reused everywhere, usually through a[semantic layer](https://www.basedash.com/blog/what-is-a-semantic-layer-the-complete-guide-for-modern-bi) or a shared metric store. Permissions and row-level security control who sees what, and basic quality checks catch broken data before it reaches a dashboard.


Governance sounds like bureaucracy, but at this stage it is the opposite: it is what makes self-serve safe to scale. When the definition of revenue lives in one place, a new hire’s dashboard and the CFO’s board deck agree by construction. This is where a[single source of truth](https://www.basedash.com/blog/what-is-a-single-source-of-truth-and-how-to-build-one-for-analytics) stops being a slogan and becomes an actual system.


Most successful lean teams should treat Stage 4 as the destination, not a waypoint. Governed self-serve analytics covers the vast majority of what a company needs to run on data.


**Symptom you are ready for Stage 5:** definitions are stable, trust is high, and the constraint is no longer “can we get the number?” but “can we act on it fast enough?”


### Stage 5: Embedded


At Stage 5, analytics leaves the BI tool and enters the places where work happens. Metrics are embedded in the product for customers, wired into operational workflows, surfaced through alerts, and increasingly assisted by AI that can answer a question or flag an anomaly without a human building the query. Forecasting and predictive models start to inform decisions rather than only describing the past.


This stage is powerful and, for most companies, partly optional. Embedded customer-facing analytics matters if analytics is part of your product. AI-assisted exploration and alerting are genuinely useful at any size. But full predictive analytics is often oversold; a startup rarely needs a churn model before it has a reliable churn definition, which is a Stage 4 problem.


**The trap:** teams try to buy Stage 5 (AI, forecasting, embedded dashboards) while still living at Stage 2. Bolting AI onto undefined, untrusted data produces fast, confident, wrong answers.


## How to diagnose your stage


Score your team on each dimension below. Your true stage is roughly the lowest one you can honestly claim across all four rows, because maturity is limited by your weakest link, not your best dashboard.


Dimension Stage 1-2 Stage 3 Stage 4-5


Access Only a data person can pull numbers Most teammates self-serve Data reaches people inside their tools and workflows


Trust Numbers depend on who pulled them Live data, but definitions vary One agreed definition per metric, enforced


Speed Days, via a request queue Minutes, self-serve Real time, pushed via alerts and embeds


Leverage Data confirms decisions after the fact Data informs recurring decisions Data triggers actions and forecasts


A quick heuristic: if answering “how many active customers do we have?” requires messaging a specific person, you are at Stage 1 or 2. If anyone can answer it but two people might disagree, you are at Stage 3. If anyone can answer it and everyone gets the same number, you are at Stage 4 or beyond.


## How to move up exactly one stage


The most common mistake teams make with maturity models is trying to jump two stages at once, usually by buying tooling. You cannot skip the organizational work each stage depends on. Here is the single highest-leverage move for each transition.


- **Stage 1 to 2:** connect a BI tool to your real data source and rebuild your most-repeated spreadsheet as one live dashboard. The goal is to stop copying numbers by hand, not to build everything at once.
- **Stage 2 to 3:** give non-technical teammates a safe, read-only way to ask their own questions, so the data person stops being a request queue. A tool that supports plain-language questions or easy filtering does most of this work.
- **Stage 3 to 4:** write down your ten most-used metric definitions and enforce them in one place. This is the least glamorous and highest-value move in the entire model. Our[data governance framework](https://www.basedash.com/blog/data-governance-framework-a-practical-guide-for-lean-teams) is a lightweight way to start.
- **Stage 4 to 5:** embed the metrics people check most often into the tools where they already work, and add alerting so the important changes come to people instead of waiting to be discovered.


Notice that only two of these four moves are primarily about buying software. The others are about definitions and access, which no vendor can install for you.


## Common mistakes with maturity models


- **Treating the top stage as the goal.** Stage 5 is not “better” for everyone. A 20-person company running trustworthy governed self-serve (Stage 4) is more mature, in the way that matters, than one with an unused machine learning model bolted onto messy data.
- **Skipping governance because it feels slow.** Stage 4 is the step teams most want to skip and most regret skipping. Undefined metrics do not cause problems until self-serve makes everyone a publisher of conflicting numbers.
- **Buying tooling to fake maturity.** A new BI purchase raises your stage only if the surrounding workflow changes. Tools enable maturity; they do not confer it.
- **Applying the model per company instead of per domain.** Finance might be at Stage 4 while product analytics is at Stage 2. It is normal and fine for different domains to sit at different stages.


## Where tooling fits


Tooling should follow your stage, not lead it. Early on, the modern stack for a lean team is deliberately small: a database or warehouse and one flexible BI tool. Our guide to[the modern BI stack for lean teams](https://www.basedash.com/blog/the-modern-bi-stack-for-lean-teams-what-to-build-and-what-to-skip) covers what to buy and what to skip.


The useful property in a BI tool is that it can grow with you across stages rather than forcing a migration at each one. A tool that connects directly to your production database, lets non-technical teammates ask questions in plain language, and supports shared metric definitions and permissions can carry a team from Stage 2 through Stage 4 without a replatform.[Basedash](https://www.basedash.com/) is built for that path: it connects to your database, supports AI-assisted and self-serve exploration, and enforces permissions and shared definitions as you grow. The broader point holds regardless of tool: choose something that fits your next stage, not a stage you are years away from.


## FAQ


### How many stages are in an analytics maturity model?


Most models use four to six stages. The exact number matters less than what each stage adds. This model uses five: reactive (spreadsheets), reporting (dashboards), self-serve (independent exploration), governed (shared definitions and permissions), and embedded (analytics in products and workflows). Each stage adds one capability the previous one lacked, and the stages are cumulative rather than interchangeable.


### What stage should a startup aim for?


Most lean teams should target Stage 4, governed self-serve analytics. That means teammates can answer their own questions and everyone agrees on what the core metrics mean. Stage 5 (embedded and predictive analytics) is worth pursuing selectively, for example if customer-facing analytics is part of your product, but it is not a requirement for running a company on trustworthy data.


### What is the difference between a data maturity model and an analytics maturity model?


A data maturity model usually covers the full data lifecycle, including collection, storage, quality, and governance across an organization. An analytics maturity model focuses more narrowly on how people turn that data into answers and decisions. In practice they overlap heavily, and for a lean team the analytics view is the more actionable one because it maps directly to daily workflows.


### Can we skip a stage in the model?


Not really. Each stage depends on work done in the previous one. You can adopt Stage 5 tooling early, but if you skip the governance work of Stage 4, AI and dashboards will produce fast, confident, conflicting answers. The reliable path is to move up one stage at a time by fixing the specific constraint that defines your current stage.


### How do we know which stage we are in?


Ask how your team answers a basic question like “how many active customers do we have?” If only one person can pull it, you are at Stage 1 or 2. If anyone can but the answers disagree, you are at Stage 3. If anyone can and everyone gets the same number, you are at Stage 4 or higher. Your true stage is limited by your weakest dimension: access, trust, speed, or leverage.
