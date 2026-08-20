---
schema_version: "1.0.0"
document_id: "fe35cc876cbea49626632b35003573d6a0ae64a60b472755cef5ce8fc3101fdf"
company_key: "yc-weave-3"
company: "Weave"
source_id: "yc-weave-3-news-import-b8160165addf"
canonical_url: "https://weaveos.com/blog/telnyx-article"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-22T19:33:03.286750+00:00"
fetched_at: "2026-08-19T18:23:14.055629+00:00"
content_hash: "sha256:ff6c7947a4ef6bdd178e9eb3f11a3a44dcf1ed4e19d28cc0832d6198f528096a"
---

# 150 Engineers. 0 Engineering Managers. 1,400 Bots. Why the best engineers at Telnyx get compute not headcount.

Most of the companies people point to as "AI-native engineering orgs" are early-stage startups with no enterprise customers and no production stakes. Telnyx has both.


They operate a private global IP network spanning 30+ countries, run five-nines reliability for some of the largest enterprises in their category, ship thousands of deployments across dozens of products, and maintain a polyglot codebase that includes Elixir, Python, Go and more. Every layer is real-time, regulated, and unforgiving when something breaks.


Despite that, they’ve built one of the most advanced AI organizations we’ve spoken to all year.


Work enters the org through an AI dispatcher. From there, every engineer is supervising bots 80% of the time, rather than writing code. Inside Telnyx, the measure of seniority is how much ROI you can generate when given unconstrained AI resources.


Here are 7 things they did to transform their team:


### **1. 1,400 bots are running inside the company right now**


Telnyx has 1,400 autonomous bots running inside the business right now. They are long-running, have their own jobs, and act on the codebase, the infrastructure, and customer requests.


There are help bots. Incident response bots. Troubleshooting bots. Repo maintainer bots that understand a specific codebase well enough to do a proper code review on it.


“Very little code is written by hand directly anymore. Most engineers are supervising, reviewing, and directing bots,” David Casem, Telnyx's co-founder and CEO, told us.


### **2. A Slack bot named ADA writes their tickets**


If you don't have managers, who tells 150 engineers what to do? The answer at Telnyx is a system they call ADA, the AI Dispatcher Agent (AIDA). It lives in Slack. Anyone in the company (sales, support, customer success) can describe a customer issue or feature request, and ADA picks it up. It pulls context from across the org, talks to a support assistive bot, and writes a properly scoped ticket into the backlog.


PMs still own product vision and backlog prioritization. But no PM at Telnyx writes tickets manually anymore. Roughly half of the tickets in flight are PM-feature-directed, and the other half come in through ADA from across the business.


That removes the layer that, at most companies, eats up a huge fraction of an engineering manager's time: translating fuzzy business signals into specific work.


### **3. AI Factory teams became the highest form of leverage for product teams**


Telnyx increasingly treats engineering as two systems:


1.


Product teams that ship customer-facing capabilities.


2.


AI factory teams that build the bots, deployment systems, observability, and automation that multiply the output of product teams.


Roughly half the engineering org now sits on the AI factory side. Every improvement they make compounds across the entire engineering organization.


### **4. Repo-specific maintainer bots remove the code review bottleneck**


Telnyx is a polyglot shop (Java, Python, Node, Elixir, Go), and with bots opening PRs across teams, code review became the new constraint.


They implemented Codex to handle the general-purpose review layer, catching common bugs and shipping with clean context. This covers most of what a human reviewer would catch on a quick pass.


But Codex doesn't know your codebase. So Telnyx built maintainer bots, one per repo, that actually understand the code they sit on top of. Conventions, prior decisions, the history of why something works the way it does. The maintainer bot reviews against the repo's own logic, not just generic best practices. And when one maintainer bot learns a pattern that works, the improvement gets pushed to every other repo's bot automatically.


### **5. A growing percentage of incidents are opened, triaged, remediated, and closed without human intervention**


For a business running mission-critical infrastructure across communications and compute at five-nines reliability, on-call is one of the hardest jobs to fill and one of the biggest sources of attrition.


Telnyx's incident response bots now open and close incidents end to end. Alert fires. Bot opens the incident. Bot fixes it. Bot closes it.


"On-call is a detractor in a five-nines business," Casem said. "We can solve on-call with bots, that shows you what is possible."


### **6.They're building toward fully autonomous deploys.**


The phase Telnyx is in right now is what they call AFK, Away From Keyboard.


Phase 1 was pair programming. Phase 2 is the current state, every engineer supervising bots in real time. Phase 3, AFK, is bots that pull work off the backlog while engineers sleep and ship it all the way through to production without a human in the loop.


They are not there yet, but the infrastructure investment, the trailing deploys, the maintainer bots, the observability team, ADA, are all designed to make AFK safe when it lands.


### **7. Their best engineers get compute, not headcount.**


In the traditional model, the best engineers get promoted into management, and the size of their team is the measure of their seniority. Headcount equals status.


At Telnyx, the best engineers do not get headcount. They get **compute** . The measure of seniority is how much ROI you can generate when given unconstrained AI resources. Your leverage is no longer how many humans you direct. It is how clearly you can articulate intent to a fleet of bots that will execute against it.


"If you can really perfect your craft of communicating ideas in a way that bots can understand," Casem said, "you are operating with limitless leverage."


### **They're hiring 40 more engineers right now.**


This is not a story about AI replacing engineers. Telnyx has 40 open engineering roles right now. They want to hire more.


"We are using a tremendous amount of AI, and it is not killing jobs. It is doing the opposite. We need more engineers than ever," the Casem said.


The job is changing, obviously. The "engineer" at Telnyx is doing less keyboard work and more bot supervision, more infrastructure thinking, more clear-intent communication. But the headcount line is going up, not down.
