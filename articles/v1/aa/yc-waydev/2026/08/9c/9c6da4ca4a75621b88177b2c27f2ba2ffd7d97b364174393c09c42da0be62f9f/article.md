---
schema_version: "1.0.0"
document_id: "9c6da4ca4a75621b88177b2c27f2ba2ffd7d97b364174393c09c42da0be62f9f"
company_key: "yc-waydev"
company: "Waydev"
source_id: "yc-waydev-rss-a82ef0eb6171"
canonical_url: "https://waydev.co/cortex-and-the-measurement-question-your-portal-should-not-be-your-scoreboard/"
published_at: "2026-08-01T19:19:36+00:00"
first_seen_at: "2026-08-06T22:29:32.289791+00:00"
fetched_at: "2026-08-06T22:29:33.321240+00:00"
content_hash: "sha256:ce329019d305abb8531b72aaad790818e053cd72e58a5f6f1e3507737ad9db2d"
---

# Cortex and the measurement question: your portal should not be your scoreboard

Measurement strategy briefing


Waydev · Migration series · No. 09


Cortex built a genuinely good internal developer portal: service catalog, scorecards, golden paths, self-service. Platform teams like it, and for what it was built to do, they are right to. But Cortex now positions itself as mission control for the AI software factory, complete with an Eng Intelligence module, an AI Impact product, and its own measurement framework. This is where a good tool starts answering questions it was not shaped for. This briefing separates the two jobs, governance and intelligence, and makes the case for keeping your portal and moving your measurement.


Key findings


I.


Cortex answers a governance question: are our services owned, compliant, and production-ready. Engineering intelligence answers a performance question: is the organization delivering, improving, and getting a return on AI. Same data sources, different jobs.


II.


Measurement inside a portal inherits the portal’s grain: the service. Delivery performance, team dynamics, and AI impact live at the commit and PR level, below the catalog’s resolution.


III.


Cortex’s measurement rests on a vendor-authored framework. Your system of record should rest on portable, industry-standard definitions, DORA and SPACE, that survive any vendor change.


IV.


This is not rip-and-replace. Keep Cortex for cataloging and governance if your platform team values it. Move measurement to a dedicated layer, and lose nothing: Waydev rebuilds your full metric history directly from Git.


Section 01


## Governance and intelligence are different jobs


Cortex’s own homepage draws the picture: catalogs, scorecards, golden paths, production readiness, and human-in-the-loop governance for the AI era. All of that is the compliance and safety layer of an engineering organization, and it matters. But notice what question every one of those features answers: does the work meet our bar? The question your board and your CFO ask is different: what did the work produce, is it improving, and what is the AI spend returning?


The governance job (Cortex)


### Is the factory up to code?


Service ownership, production readiness, security standards, migration tracking, golden paths for humans and agents. Unit of analysis: the service. Consumer: platform teams and SREs. Cadence: continuous conformance.


**Right tool for:** keeping a sprawling service estate owned, compliant, and safe as AI multiplies output.


The intelligence job (Waydev)


### What did the factory produce?


Delivery performance, cycle time, DORA, team dynamics, business alignment, and AI adoption, impact, and ROI down to the spend. Unit of analysis: the work, at commit and PR level, USPTO-patented. Consumer: engineering leadership, the CFO, the board.


**Right tool for:** knowing whether the organization is getting better, and proving it.


Mission control tells you every system is green. It does not tell you whether the mission was worth flying. A scorecard can read 100% while delivery slows by a third, and neither number will explain the other.


Section 02


## The grain problem: portals see services, performance lives in the work


Everything in Cortex hangs off the catalog and its context graph: entities, owners, relationships. Measurement built on that foundation inherits its resolution. Services are the right grain for readiness. They are the wrong grain for performance, which lives in how commits become PRs, how PRs move through review, how AI-assisted work behaves on its way to production, and how teams, not services, improve over time.


It also inherits the catalog’s oldest weakness: metadata upkeep. Every IDP customer knows the failure mode, because it is the reason IDPs exist: ownership goes stale, definitions drift, and someone maintains the graph. Building your governance on the catalog is the point of the product. Building your measurement on it means your metrics are downstream of your metadata hygiene. Waydev’s measurement is downstream of the work itself, which does not go stale, because it is the source.


### The test that exposes the grain


Ask both platforms: “Which teams improved cycle time this quarter, why, and what did Cursor-assisted PRs return against their cost?” A service-grained system answers with service health. A work-grained system answers the question. Run it in the parallel run below and judge for yourself.


Section 03


## Whose framework is your system of record built on?


Cortex ships its measurement wrapped in DRIVE, a framework authored by its own CTO, supported by a book, a maturity assessment, and a conference. Credit where due: it is a serious content effort. But look at it as an architecture decision. Adopting a vendor-authored framework as your organization’s definition of engineering effectiveness means your metrics, your targets, and your board narrative are expressed in one vendor’s proprietary language. That is lock-in of the most durable kind: not data lock-in, vocabulary lock-in.


Measurement infrastructure should rest on definitions that are portable and industry-standard: DORA, SPACE, and the DX Core frameworks, benchmarkable across the industry and legible to any executive, auditor, or future vendor. Waydev implements those, grounded in commit-level telemetry, and adds the AI ROI layer the standards bodies have not caught up to yet, defined transparently so your CFO can audit the math.


There is also the breadth question. Catalog, scorecards, workflows, IDP, MCP, plugins, Eng Intelligence, AI Impact, a framework, a conference: Cortex is building a suite, and in a suite, measurement is one module competing for roadmap attention against nine others. Waydev builds one thing. The consolidation logic you have watched play out across this category, Flow, Velocity, DX, applies to modules too: when priorities shift, the non-core module is where investment quietly stops.


The core of it


Keep the portal for what portals do: catalog, govern, pave the golden paths. Put measurement where measurement belongs: on the work itself, in industry-standard language, with a vendor whose only job is the answer. Two tools, two jobs, no compromise on either.


Section 04


## What moves to Waydev, and what stays in Cortex


Capability Where it belongs Why


**Service catalog, ownership, context graph** Stays in Cortex Governance is the portal’s job, and Cortex does it well


**Scorecards, production readiness, golden paths** Stays in Cortex Conformance and paved roads belong with the platform team


**DORA and delivery metrics** ✓ Moves to Waydev Commit-level grain, industry-standard definitions, benchmarks


**AI adoption, impact, and ROI** ✓ Moves to Waydev Measured in the delivery path, per tool, per team, down to the spend, as the core product, not a module


**Team performance and executive reporting** ✓ Moves to Waydev Teams are the performance unit, not services. Waydev AI answers leadership in plain language, in seconds


**Targets and improvement tracking** ✓ Moves to Waydev Goals and AutoGoals: targets from your own baselines, tracked automatically, Signals to Slack daily


**Historical baselines** ✓ Moves to Waydev Rebuilt directly from your Git provider. The measurement move costs nothing historical


### One honest caveat: Waydev is not a portal, and will not pretend to be


If you came here hoping to consolidate cataloging, scaffolding, and measurement into one tool, we will not make that pitch. Waydev does not do service catalogs or golden paths, deliberately, for the same reason we argue Cortex should not be your scoreboard: bundled jobs get compromised tools. If catalog consolidation is the priority, evaluate that separately. If measurement quality is the priority, that is our conversation.


Section 05


## The play: unbundle before the Eng Intelligence line item grows


Whether you already pay for Cortex Eng Intelligence or are being pitched the expansion at renewal, the sequence is the same: evaluate measurement as its own decision, on its own merits, before it gets bundled into a platform-wide renewal where nobody scrutinizes the module.


When Move Outcome


T minus 90 days **Split the invoice and the jobs.** Separate what you pay for governance (catalog, scorecards, workflows) from what you pay, or are quoted, for measurement. Ask which leadership questions the measurement module answered last quarter without manual assembly. Each job priced and judged on its own


T minus 60 days **Connect Waydev to production repos.** Read-only against GitHub, GitLab, Bitbucket, or Azure DevOps. Coexists with Cortex completely, since the two do different jobs. History processes automatically. Side-by-side baselines within days


T minus 30 days **Run the grain test.** Leadership asks both platforms the cycle-time-and-AI-ROI question from Section 02. EMs run weekly reviews in both. Platform team keeps using Cortex for governance throughout, untouched. Evidence at the grain where performance lives


Renewal **Renew the portal, move the measurement.** Keep Cortex scoped to governance at a price that reflects the narrower scope. Sign off measurement with the memo below. Two right-sized tools instead of one stretched one


Internal memo template for sign-off


Ahead of our Cortex renewal, we evaluated governance and measurement as separate decisions, and ran Waydev in parallel on our production repositories for 30 days. I recommend we keep Cortex scoped to its portal and governance role, and adopt Waydev as our engineering measurement layer.


Cortex serves our platform team well for cataloging, scorecards, and golden paths. For measurement, we need commit-level delivery analytics, industry-standard DORA and SPACE definitions, AI adoption and ROI quantified down to the spend, and executive reporting that answers questions in plain language. Waydev provides these as its core product rather than a platform module, and rebuilt our historical baselines directly from Git, so the move costs no trend data. The two tools coexist without conflict.


Waydev is used by Fortune 500 engineering organizations including American Express and PwC, holds a USPTO patent in Git analytics, and is recognized by Gartner in the Developer Productivity Insight Platforms category. As a bootstrapped and profitable single-product vendor, its roadmap has exactly one priority: the measurement layer we are buying.


## Questions leadership will ask


### Isn’t one platform for everything simpler than two tools?


Simpler to procure, yes. The question is what the bundle costs in quality: a measurement module inside a portal inherits the portal’s data grain, roadmap priorities, and framework vocabulary. Governance and intelligence have different consumers, different cadences, and different units of analysis. Two right-sized tools that each do their job beat one platform stretched across both, and they coexist with zero integration burden since both read from the same sources independently.


### Cortex already shows us DORA metrics. Why isn’t that enough?


Ask what sits under the number. Service-grained DORA tells you which services deploy often. It does not explain why a team’s cycle time moved, how review load is distributed, or what AI-assisted PRs returned against their cost, because those answers live at the commit and PR level, in the work itself. Run the grain test from this briefing on your own data and compare the answers, not the metric names.


### We already pay Cortex. Isn’t adding Waydev a new cost?


Price the alternative honestly: the Eng Intelligence and AI Impact modules are their own line items, growing at each renewal, plus the staff time spent assembling executive answers the portal cannot produce natively. In most evaluations, a right-scoped Cortex renewal plus Waydev lands at or below the bundled expansion path, with a better answer at the end of it.


### Can the two run side by side during evaluation?


Yes, permanently, not just during evaluation. Waydev connects read-only to your Git provider and does not touch Cortex’s catalog or workflows. Your platform team notices nothing. The 30-day parallel run is the strongest evaluation you can do, and it carries zero disruption.


### What about security and privacy?


Waydev is SOC 2 certified and never stores source code. SSO and RBAC are standard, with role-based access and anonymization controls approved by works councils at enterprise scale. Full documentation is available for CISO review before any data flows.


### Could Waydev expand into a suite and dilute focus, the way you describe Cortex doing?


Fair question, and the structure answers it. Waydev is bootstrapped and profitable with one product and nine years of doing exactly this. Suite expansion is what venture math demands of platform companies; nobody’s math demands it of us. Ask every vendor what forces act on their roadmap, and verify the answers.


## Run the grain test


Thirty days, your production repositories, zero disruption to your portal. Ask both platforms which teams improved this quarter, why, and what your AI spend returned. One answers in service health. One answers the question.


[Start your parallel run](https://waydev.co/demo/)
