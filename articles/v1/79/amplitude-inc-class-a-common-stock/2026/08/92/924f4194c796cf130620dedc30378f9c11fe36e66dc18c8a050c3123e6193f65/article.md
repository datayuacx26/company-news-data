---
schema_version: "1.0.0"
document_id: "924f4194c796cf130620dedc30378f9c11fe36e66dc18c8a050c3123e6193f65"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/cashbook-ai-failure-rate"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T17:50:07.505280+00:00"
fetched_at: "2026-08-19T17:50:08.251700+00:00"
content_hash: "sha256:bcf692e285e08cea7f364be03631917ff2e03a9371507ae14d57277150b43b6e"
---

# The Hidden Cost of a Bad AI Answer

At[CashBook](https://cashbook.in/) (Y Combinator W21), our mission is to empower every Indian business with real-time financial control. We’ve built a business finance app used by small and medium businesses across India. Every business running on CashBook builds up a detailed record of its cash-ins, cash-outs, and spending by category.


## **Launching our AI agent**


Almost none of that history got used. Turning it into an answer, like whether this month's spend is trending up or down, meant exporting to a spreadsheet and working it out by hand, and most people running a shop, a fleet, or a job site don't have time for that. The data already had the answer. Nobody was asking for it.


In May 2026, we built an AI agent to do the asking for them. It sits on top of a business's own data and answers in seconds what used to take that spreadsheet: how last month compared, which spending category is running hottest, where the cash is actually going. We also pointed it at the FAQs that show up early in onboarding, the kind of questions people ask when they're trying to get more out of a feature they haven't used yet.


We released it by feature flag to a few thousand users, chosen specifically for having enough transaction history banked to make the test mean something. A business with a dozen entries wasn't going to tell us whether the idea worked at all.


Even in that curated group, an assistant asking to be trusted with a business's financial record has to clear a high bar. Our first build didn't always clear it. Answers were slow to arrive, and sometimes wrong. Worse, we had no shared definition of what a correct answer even looked like for something as open-ended as "where did my money go this month."


## **Reading transcripts doesn't scale**


Our first instinct was the one most teams reach for: read the sessions. A product manager went through transcripts by hand and sorted users into rough types. That works until sessions turn into dense, multi-turn conversations, and then it becomes archaeology.


Our existing analytics didn't help. It was built for funnels and feature adoption, so it had no concept of a "session" or a "task." It could show us the agent got opened. It couldn't show us whether it did its job. Between hand-read transcripts and telemetry that couldn't tell a good answer from a bad one, we didn't have a real way to grade the thing.


## **A grade for every session, not a pass or fail**


We wired the agent to Amplitude's Agent Analytics and let an automated judge grade every session against a real taxonomy: response quality, user intent, data-quality issues, friction, technical failures, cost, and latency, each with the model's own reasoning attached.


That let an engineer open a single conversation and see exactly which function call broke it. That's how a hunch became a named, fixable defect: a 16% error rate in one internal tool, sum_across_books. We rolled the taxonomy into one number we could watch daily: Failure Rate.


We also skipped the dashboard once and just asked. We pointed Amplitude's assistant at a week of transcripts and told it to sort real users by what they were actually asking for and hand us a shortlist worth calling. A research pass that would have taken weeks was done by lunch.


## **The number under the number**


Usage did what most betas do: daily sessions climbed hard for two weeks, peaked, then fell back to a trickle within a month. We saw the decline in our own logs before any dashboard forced the point.


##### A bad first answer with the agent didn't stay contained to the agent. Four weeks later, those users were retaining 15 points lower across all of CashBook.


On the surface, quality looked fine. Eighty-six percent of sessions passed the automated response-quality check. But finishing a task and mattering to the user aren't the same thing, something Amplitude's own published research had already told us. The number that actually mattered, Failure Rate, ran at one session in four or five through late June and topped two in five on the worst days. That's what froze a wider rollout.


The number that connected all of it to something we actually care about was retention.


Early signals point to the agent helping users find insights and reach value faster. More business value, delivered in low friction, interactive format.


In week one the two lines are close enough to call flat. If anything, the failed-session cohort holds on slightly better, which tracks: nobody's had time to act on one bad exchange yet. By week four they're 15 points apart, 79.2% against 63.9%. Our week-four sample is thin, and the difference is smaller than the roughly threefold effect Amplitude has reported across its own customer base, but the mechanism is the same one Amplitude had already flagged: a bad first exchange follows a user out of the agent and into how much they use the rest of CashBook.


## **What's next**


Some of what's on our roadmap:


- Memory, so the agent doesn't need to be reminded of a business twice
- A personality built for a fintech product, not a generic assistant
- Sharper visualizations, so key insights reach the business admin unprompted
- An agent that acts instead of just answering, creating entries and reports on its own


We'll judge and release each of those against the same measure of quality and adoption. Whatever ships next gets checked against Failure Rate before it gets checked against how many people opened it.


See how Agent Analytics turns a hunch into a fixable defect →[Explore Amplitude Agent Analytics](https://amplitude.com/agent-analytics)
