---
schema_version: "1.0.0"
document_id: "a779889d465092d6306b39ec5554bb2c3dfe2db8cf7ed196ac6caa0c851a2fda"
company_key: "yc-abacum"
company: "Abacum"
source_id: "yc-abacum-news-import-d37bd80a09f0"
canonical_url: "https://www.abacum.ai/blog/how-to-approve-ai-budget-requests"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-08-10T16:30:53.596641+00:00"
fetched_at: "2026-08-10T16:30:55.519497+00:00"
content_hash: "sha256:4081d8b205d8fb7119a49b28f0111b7a2db6fd7e2ea166d825f1d297d8b6b4e5"
---

# How to Approve AI Budget Requests: Automation vs Experimentation

**The short version:**[Abacum](https://www.abacum.ai/ai-native-fpa-platform) works with finance teams approving AI budget requests across the business this planning season. Split every request into automation, which replaces a process that already has a cost and gets a unit metric, or experimentation, which buys information and gets a question, a spend cap and a kill date.


The FY27 asks are in and the AI lines are bigger and vaguer than last year. Sales wants an agent nobody has scoped. Engineering has a token number that changes every time you ask for it. Somewhere in the stack there is a slide with a vendor’s ROI calculator output pasted onto it.


SpendHound surveyed 172 finance and procurement leads. 46% went over budget on AI last year. 57% are not confident they are paying a fair price. Almost twice as many are raising the line again this year.


Deloitte had 91% of organisations planning to spend more in the next twelve months. KPMG asked 2,145 leaders in May and 7% could point to an established return.


*Investment is up, evidence has not followed. Two separate surveys of different populations and dates, shown at the same scale for comparison only. Sources: Deloitte, State of Generative AI (Oct 2025); KPMG Global AI Pulse Q2 (Jun 2026).*


I do not read that gap as the tools failing. We are running two different kinds of spending through one approval process.


## Automation spend and experimentation spend are not the same request


Some of what you are being asked for is **automation** . It replaces a process that already exists, that somebody already does, that already costs something. Invoice matching. Contract compliance checks. First-line tickets. There is a before-state, which means there is a number.


The rest is **experimentation** . Somebody wants to find out whether a model can do something the company has never done. No before-state. Asking for ROI there is asking for a forecast of a discovery. You will get one and it will be fiction.


Both are worth funding. The test has to be named when the money is approved, not reconstructed in April when you are working out what happened.


## What to hold each one to


**Automation gets a unit.** Cost per invoice, days to close, touchless rate, tickets resolved without a human. Written down with today’s number and a date, before the money moves. If the unit has not moved by the second quarter of running, the tool is wrong for the job. Stop paying for it.


Most of these do not fail on the model. Only 18% of companies have AI workflows running across every department and 84% have not redesigned a single job around what the tool can do. If nobody changed how the work happens, the unit was never going to move.


**Experiments get a question, a cap and a kill date.** What are we trying to find out, what is the most we will spend finding out, when do we stop. Then measure the department on how many experiments reached production and how many were killed on time. If nothing got killed in twelve months, nobody was experimenting.


Getting the baseline right is the same discipline as[driver-based budgeting and forecasting](https://www.abacum.ai/use-cases/budgeting-forecasting) . If you cannot state today’s number, you are not ready to approve the spend.


## None of this survives a single AI cost centre


KPMG found leaders with strong cost visibility were five times more likely to have established a return, 15% against 3%. In the same survey, 42% could not say where their AI money was going.


This is what an FP&A platform is actually for at budget time. Abacum holds the baseline and the plan in the same place, so a tag applied in September still means something in June. Tag it by team and by use case at the point of approval. Doing it afterwards never works, because by then it is one line from one vendor and nobody remembers who asked for what.


## How to model AI cost in the plan


On modelling the cost itself, the most useful framing I have read comes from CJ Gustafson at[Mostly Metrics](https://www.mostlymetrics.com/p/the-thing-about-more-tokens-9128) : budget tokens the way you budget benefits, as a per-head cost that varies by function rather than a project line. His poll of 250 public company CFOs landed near 20% of cash compensation. a16z’s Ivan Makarov puts per-employee token budgets at 20 to 50% of R&D salaries.


What matters for your plan is that it stacks on top of headcount instead of replacing it. If your FY27 model assumes AI spend offsets people, find out whether anyone has actually committed to that offset. I wrote about why I do not expect it in[AI will not shrink your finance team](https://www.abacum.ai/blog/ai-won-t-shrink-your-finance-team) . Uber’s CTO went through the whole 2026 token budget by April.


This is also where[scenario planning](https://www.abacum.ai/use-cases/scenario-planning) earns its keep. Model the plan with the offset and without it, and see which one you would still sign.


## What tells you it worked, by mid-2027


For automation, one question: did the unit move against the baseline you wrote down. For experiments, two: what share reached production, and what share were killed on schedule.


Then put the next dollar where the graduation rate is highest. It is the only signal I have found that is hard to game in either direction.


One thing to avoid on the way. You will be tempted to bring the MIT number into the room, the one where 95% of AI pilots fail. I would not. It measured bespoke builds against a six-month payback window, it came out of a preliminary working paper by a group with an interest in the answer, and the same report had general purpose tools converting from pilot to implementation above 80%. Somebody in that meeting has read it properly.


## The case nobody has solved


A lot of what is working shows up as “we can do things we could not do before.” That is real and I do not have a clean way to put it on a P&L. Usage will not stand in for it either. All consumption tells you is that somebody opened the thing.


So budget it as capability, cap it, give it a graduation path, and say exactly that to the board. Naming the unproven spend yourself is what keeps you credible on the rest of it.


If you change one thing this cycle, make every AI ask say which of the two it is before you approve it.
