---
schema_version: "1.0.0"
document_id: "89089b1196569bec6e9b4f17f8fa046f93807d41b7afe0e2e491d76b7b8ee14b"
company_key: "yc-salt-security"
company: "Salt Security"
source_id: "yc-salt-security-news-import-678c364a0dd5"
canonical_url: "https://salt.security/blog/we-trained-cybersecurity-startups-to-win-povs-not-solve-problems"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-22T12:39:58.305485+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:2b0c69f98c92f20ff2e02dbfc39bd0411d1b51b36ed3664ee1b6bd15637fd903"
---

# We Trained Cybersecurity Startups to Win POVs, Not Solve Problems

## Frameworks are useful. Checkboxes are not enough.


Frameworks like OWASP, NIST, EU AI Act, MITRE, and CIS give teams a common language. The mistake is treating every requirement like a yes or no question.


Take BOLA, Broken Object Level Authorization, the number one risk in the OWASP API Top 10. In simple terms, BOLA means a user can access someone else’s data by changing an identifier.


Imagine I open my banking app and the app calls:


` GET /accounts/12345`


The API returns my name, email, balance, recent transactions, and the last four digits of my bank account. If an attacker changes the ID and gets another customer’s account details, that is BOLA.


In many evaluations, the test is obvious: a scanner sends 1,000 API calls in a few seconds, rapidly changing IDs. Most tools can detect that. It is noisy, fast, and easy to recognize.


Real attackers are more patient. They may enumerate slowly over hours or days, use valid tokens, change one value every few minutes, and move across accounts, endpoints, sessions, regions, or business units. Each request may look normal on its own.


[AI agents](https://salt.security/agentic-ai) make this worse. If agents are connected to these APIs, attackers can use them to explore and exploit weak authorization paths faster. The API vulnerability was already serious. Agentic access makes it scalable.


At Salt, we invested heavily in a[big-data intent engine](https://salt.security/platform) to detect this slow and low behavior: not just whether someone changed an ID, but whether their behavior shows intent to enumerate objects they should not access.


The teams that tested for this understood the difference. But many evaluations had no clear way to score it. If two vendors both showed “BOLA detected” on the noisy test, both got the same checkmark.


**The depth disappeared into the spreadsheet.**


## The showroom is not the road


A POV is the showroom. Production is the road.


In the showroom, everything is controlled. The data set is limited. The vendor is watching closely. The dashboard is clean. The report looks good.


Then the solution goes live, and real teams inherit it.


AppSec triages findings. The SOC decides what matters. Engineering needs tickets they can fix. Risk needs evidence. Business units need ownership. Existing tools need integration.


Often, the evaluators are not the daily users. That is why operationalization matters.


The real test is not week-one clarity. It is whether the product still creates value six months later, across business units, distributed teams, noisy data, exceptions, ownership gaps, and real attackers.


The same applies to the vendor: do they help build the program, train teams, and drive adoption, or only react after the deal is signed?


## We learned this the hard way


At Salt, we learned this the hard way.


We started with technology. We focused on the hardest problems first: deep behavioral analysis, accurate detection, complex attack patterns, and runtime understanding that surface-level approaches often miss.


Some customers immediately understood why that depth mattered. But many evaluations were not designed to measure it. They were designed to compare columns: does it deploy quickly, support this framework, have this report, and show something impressive in the first meeting?


That forced us to learn an important lesson: depth matters, but depth also has to be packaged in a way the market can adopt.


So we built the journey: no-deployment value, agentless visibility and governance, then full runtime protection. Make it easier to start, without giving up the depth required to actually secure an enterprise.


That took more than eight years. Real security depth is hard to build. If evaluations do not measure it, fewer companies will invest in it.


## Evaluate the road, not only the showroom


The startup ecosystem responds to incentives.


If buyers reward first-day dashboards, broad framework mappings, and frictionless deployment above everything else, vendors will optimize for that.


So ask what the tool cannot see without deeper integration or runtime visibility. Ask how deeply it covers each requirement and how it behaves when the environment is messy. Ask what happens six months later, when real teams, workflows, attackers, and consequences show up.


Fast value matters. But it has to become deeper value.


POVs matter. RFPs matter. Frameworks matter.


But they are not the goal.


The goal is to make the enterprise safer.


If we want startups to build for that, we have to evaluate for that.


AI Agents drive innovation, but they also introduce unseen risk. Salt's Agentic Security Platform™ gives you full visibility and control, so you can reduce risk, meet compliance, and stay resilient.[Book a demo](https://salt.security/demo-request) to learn more.
