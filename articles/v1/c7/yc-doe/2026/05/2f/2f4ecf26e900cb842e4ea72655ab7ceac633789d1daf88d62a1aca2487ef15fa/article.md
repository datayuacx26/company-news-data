---
schema_version: "1.0.0"
document_id: "2f4ecf26e900cb842e4ea72655ab7ceac633789d1daf88d62a1aca2487ef15fa"
company_key: "yc-doe"
company: "Doe"
source_id: "yc-doe-news-import-a3f9325ff0ec"
canonical_url: "https://doe.so/blog/units-of-output-per-person"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-21T16:44:00.145230+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:dafbc4cabc0bc34afee000361c9a8b26f0dd243e0e5097ee65171d87ae7b8991"
---

# Units of Output Per Person

The headlines scream "AI means fewer people."[Block cut more than 4,000 roles](https://apnews.com/article/block-dorsey-layoffs-ai-jobs-18e00a0b278977b0a87893f55e3db7bb) from a workforce of more than 10,000 and tied the move to AI. Cisco announced nearly 4,000 cuts while reporting record revenue and shifting more investment toward AI.[Cloudflare cut 1,100 roles](https://techcrunch.com/2026/05/08/cloudflare-says-ai-made-1100-jobs-obsolete-even-as-revenue-hit-a-record-high/) while saying internal AI use had changed how the company operates. AP[captured the broader pattern](https://apnews.com/article/ai-layoffs-cisco-meta-block-65f9944fa25306bf5c975dd94805731e) : companies are increasingly pointing to AI when they announce cuts.


Thankfully, that framing does not consider the complexity and competitiveness of real world markets. The ugly truth is it's a dog eat dog world, and each company is going to do whatever it takes to beat their competition. When a cost curve falls, the savings do not just sit there. They get pushed into more customers, faster shipping, better service, lower prices, or higher margins. Once you accept that, the more useful business question is how much revenue-producing work you can move through the company at acceptable quality, cost, and risk.


## Leverage Per Person


The first thing to get right is the unit. Every company has them, even if it does not call them that. In sales, a unit might be an account brief that changes the next call. In finance, it might be a variance narrative tied to the system of record. In legal, it might be a redline package a lawyer can actually review. In success, it might be a renewal packet that lets one CSM cover more accounts.


AI output is a unit of work. A draft is a unit. A summary is a unit. A slide is a unit. The mistake is treating every unit as equal.


A raw draft in a chat window might be worth a small fraction of a business unit if someone still has to check it, rewrite it, source it, and move it into the system of record. A sourced draft in the right format is worth more. A finished artifact that helps win a deal, save a renewal, reduce support load, or close the books faster is worth a lot more. A slide is not valuable because AI made it. It is valuable if it is the right slide in the right meeting and it changes the outcome.


So the company should not ask, "How many things did AI generate?" It should ask, "How much useful work did each person move, and what did that work do to revenue, margin, risk, or cost to serve?"


Models, infrastructure, and people all matter. Better models increase the number and quality of units. Better infrastructure increases the conversion rate from raw output to useful business work. Better strategy chooses the units that matter in the first place. Most companies will rent the same models. The edge comes from the system around them.


But every unit has a cost too: the salary tied up in producing it, the model and tool spend, and any review or rework that consumes extra capacity. Two employees with the same AI tools will not produce the same number of useful units, and two employees with the same salary will not have the same cost per unit. AI does not erase performance differences. In a lot of cases, it amplifies them.


No wonder AI productivity studies look noisy. METR's[2025 developer study](https://metr.org/Early_2025_AI_Experienced_OS_Devs_Study-paper.pdf) found that AI could slow experienced people down on real tasks, while its[2026 follow-up](https://metr.org/blog/2026-02-24-uplift-update/) and[task-substitution analysis](https://metr.org/blog/2026-05-08-task-substitution-and-uplift/) showed a more complicated picture. AI changes the work people attempt, the quality bar they apply, and the cleanup they leave for someone else. HBR's[discussion of the productivity cost of AI](https://hbr.org/webinar/2026/02/the-productivity-cost-of-ai) points at the same issue.


Output is not leverage. Leverage is incremental useful output, at a lower cost, pointed at work that matters.


## How It Compounds


This compounds because every new person inherits the system. Models get smarter, faster, and cheaper, so more raw output clears the quality bar. The work layer gets better, so less value is lost to context gathering, review, retries, formatting, and writeback.


Let's do the math where AI implementation is furthest along: coding. The feedback loop is tighter there. Code compiles, tests pass or fail, CI catches regressions, and review can inspect the diff. That makes it a good place to see the pattern before it spreads to sales, finance, legal, and ops.


Here is a simplified implementation ladder.


**Baseline:** one engineer produces 20 useful coding units per week. A unit could be a bug fix, testable feature slice, refactor, migration, or reviewed pull request chunk.


The edge case is overlap. If AI writes a snippet the engineer would have written anyway, that is still work, but it is not an extra unit for the business. If five drafts collapse into one pull request, you do not count five business units. You count the reviewed artifact that lands. So the numbers below use candidate units after obvious duplication has been removed, then apply acceptance and cleanup.


**Basic ChatGPT-style use:** the engineer asks ChatGPT or Claude for help in a browser. This helps, but not magically. A good engineer already has docs, search, Stack Overflow, code review, and taste. After discounting overlap, browser chat adds one loose AI stream: 18 candidate units, 65% become useful, and manual cleanup costs 2 units. The engineer gets to roughly 30 useful units per week.


**AI IDE:** the connected work layer is the IDE. The assistant can see the repo, edit files, use symbols, run tests, and keep the human closer to the actual diff. After discounting overlap, three AI streams produce 20 candidate units each, 75% become useful, and cleanup costs 5 units. The engineer gets to 60 useful units per week.


**Full agent system:** orchestration means agents can take issues, make changes, run CI, repair failures, prepare diffs, and go through review. The human is still accountable, but the system can push more work to the review boundary. After discounting overlap, six bundles produce 22 candidate units each, 82% become useful, and coordination costs 8 units. The engineer gets to roughly 120 useful units per week.


At the same ten-engineer headcount, that is the difference between 200, 300, 600, and 1,200 useful coding units per week.


Same ten engineers, different implementation layer: 200, 300, 600, and 1,200 useful coding units per week under the assumptions above.


Cost is where the math gets easy to fake. If the same engineer reviews the output, fixes mistakes, and retries the prompt, do not count salary, review time, and retry time as three separate costs. Salary already covers that person's time. Review drag and retry drag show up by reducing net useful units. Add review cost only when another person, team, or vendor is actually being paid.


Token cost only makes sense next to human labor. A fully loaded $200,000 employee producing 20 useful units a week costs roughly $192 per unit in labor. At 60 units, that falls to about $64. At 120, about $32. Then add model and tool spend.


A cheap model route is not cheap if it creates expensive cleanup. A more expensive model can be cheap if it saves the review pass or wins the account. The best NBA contracts are all-stars on rookie deals: elite production at a discount. AI routing should work the same way. Use the frontier model when the unit needs it. Use cheaper models, cached context, retrieval, and learned workflows when they produce the same useful unit at a lower cost.


If the model-and-tool portion falls from $10 to $5, the same $1,000 budget buys 200 useful units instead of 100. Near $3, it buys more than 300. The business gets leverage twice: each person supervises more useful work, and the non-labor cost of each useful unit falls.


Agent-system case: 20 baseline useful units plus 132 non-overlapping AI candidate units. After acceptance and review drag, the net is roughly 120 useful units. Manual baseline is left out on purpose because there is no model/tool denominator. This isolates the AI-supported system cost curve.


## The Economic Choice


Layoff math only works if output is fixed, but ambitious people and competitive companies do not behave that way. Give them a better system and they try to move more pipeline, ship more product, save more renewals, cover more accounts, and improve margin.


Cheaper models only matter if the savings reach the work. If AI stays in chat windows, the discount leaks into copy paste, repeated context, manual checking, and writeback to Salesforce, spreadsheets, contracts, and tickets.


The advantage is the infrastructure: choosing the right point on the intelligence-cost curve, feeding the right company context, learning from review, and putting finished work back where it belongs.


Then people can manage more agents, agents return more useful work, and every model improvement lowers the cost of the next unit.


That is what we're building at Doe.
