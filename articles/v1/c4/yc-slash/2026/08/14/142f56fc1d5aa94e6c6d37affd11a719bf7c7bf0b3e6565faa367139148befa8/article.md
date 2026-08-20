---
schema_version: "1.0.0"
document_id: "142f56fc1d5aa94e6c6d37affd11a719bf7c7bf0b3e6565faa367139148befa8"
company_key: "yc-slash"
company: "Slash"
source_id: "yc-slash-news-import-890b67683897"
canonical_url: "https://www.slash.com/blog/how-to-cut-ai-costs-without-cutting-ai-usage"
published_at: "2026-08-07T16:14:00+00:00"
first_seen_at: "2026-08-07T20:30:31.371826+00:00"
fetched_at: "2026-08-07T20:30:33.486763+00:00"
content_hash: "sha256:eb7040ca588e34464f616364c7635aa42996da9e968481db4e232c501e74327e"
---

# How to Cut AI Costs Without Cutting AI Usage

## Reducing AI Costs Doesn't Mean Slowing Your Team Down


AI isn't cheap. Deloitte reported that AI is now the[fastest-growing line item in corporate technology budgets](https://www.deloitte.com/us/en/services/consulting/articles/how-to-navigate-economics-of-ai.html) , consuming a quarter to one-half of total IT spend at some firms. Companies like[Uber](https://techcrunch.com/2026/06/02/uber-caps-employee-ai-spending-after-blowing-through-budget-in-four-months/) and[Accenture](https://www.404media.co/the-tokenpocalypse-is-here-companies-are-scrambling-to-stop-spending-so-much-on-ai/) went from pushing the gas to pumping the brakes, with technical leads at each explaining that the costs of heavy use ran well beyond expectations.


But some companies are getting better at cutting AI costs. A good example is AT&T, which[scaled AI across more than 100,000 employees](https://www.deloitte.com/us/en/services/consulting/articles/cfo-guide-ai-token-economics.html) and reached 8 billion tokens a day. Rather than cutting usage, the company restructured how its agents worked, putting supervising agents over worker agents. Reported cost savings landed around 90% while token volume tripled to 27 billion a day. More tokens, smaller bill.


With some clever strategies and better financial tools, your business can keep scaling its AI use without the bill scaling with it. Here's how.


## Corporate cards built for control


Cashback, automation, and insights, simplified.


[Get Started](https://www.slash.com/sales-inquiry)


## How AI Token Pricing Works


First, you need to know what a token is. A token is a small piece of text, roughly four characters or about three-quarters of a word. Models read and write in tokens, and providers bill you for how many pass through.


Not all tokens are priced equally. There are two types, input and output. Input tokens are everything you send the model, meaning your instructions, your documents, and your question. Output tokens are everything the model sends back. Across the current Claude lineup, output tokens run about five times the price of input tokens, and other major providers price output at a similar markup.


Here is roughly what the major providers charge for their flagship and lower-cost models per million tokens, as of mid-2026:


**Model** **Est. cost per million input tokens** **Est. cost per million output tokens**


GPT-5.6 Sol


$5


$30


Claude Opus 5


$5


$25


Gemini 3.1


$4


$18


Claude Sonnet 5


$3


$15


Gemini 3 Flash


$1.50


$9


GPT-5.6 Luna


$1


$6


Claude Haiku 4.5


$1


$5


Take a workload of 10 billion tokens a month, split 70% input and 30% output. On Claude Opus 5 at $5 and $25, that is about $110,000. Move the same volume to Sonnet 5 at $3 and $15 and it drops to roughly $66,000. Move it to Haiku 4.5 at $1 and $5 and it lands near $22,000, a reduction of about 80%.


Those per-token rates only cover part of your bill. Most companies end up paying for AI in two different ways:


- **Seat subscriptions:** ChatGPT Business, Claude Team, Gemini in Google Workspace, and similar plans charge a flat monthly fee per user. Your bill is the seat count times the rate, and it does not move based on how much anyone types. When someone switches from a cheaper model to a more expensive one inside one of those apps, your invoice does not change at all. What they hit instead are usage limits, and higher tiers buy more headroom rather than more tokens.
- **API usage:** When your engineers call a model programmatically, or when a coding agent or internal tool runs against a provider's API, you pay per token consumed. This is the spend that scales with volume, moves without warning, and rewards the optimizations below. It is also the spend that can quietly triple in a month.


Depending on your enterprise plan and API reliance, your token consumption could be hard to curb. And lower-cost models tend to produce less-polished work, so the time your team spends on rewrites and prompt adjustments may end up costing more overall.


Reducing your AI bill isn’t about simply using AI less. That can be hard to do when your team already relies on it, anyway. Really, it’s about paying less for the same amount of work.


## 4 Methods to Reduce AI Token Spend


AI providers have their own reasons to help you spend less, and they have built discounts directly into their token pricing mode for teams that know how to use them. Companies are working the other end, figuring out which internal controls keep token spend predictable. Here are four techniques that can help your business lower its token budget:


### Prompt caching


Even though it may seem like it, models keep no memory between each prompt in a conversation. Every time you make a request, the model reinterprets your entire prompt: your standing instructions, any reference documents, the conversation so far, and the new question. That's why as a conversation gets longer, token consumption can accelerate.


Prompt caching is a built-in mechanism to cut that cost. Providers bill a reduced rate, up to 90% off input on some models, for reinterpreting prompts they have already processed recently. You still send the whole prompt every time; they simply charge less for the familiar part.


The discount only applies if the opening of your prompt is character-for-character identical to what you sent before, which both[Anthropic](https://docs.claude.com/en/docs/build-with-claude/prompt-caching) and[OpenAI](https://developers.openai.com/api/docs/guides/prompt-caching) document as a strict requirement. That match can break in several ways:


- **Time:** The cache holds for roughly five minutes to an hour depending on the provider. Someone reads a reply, gets pulled into a meeting, and answers thirty minutes later. That next request pays full rate on the entire history even though not one word changed. This is the most common way the discount disappears.
- **Compaction:** As a conversation approaches the model's context limit, most tools summarize or drop earlier messages to make room. That rewrites the opening, so the cache breaks.
- **Editing an earlier turn:** Regenerating a response or revising a question partway up the thread invalidates the cache from that point forward.
- **Switching models:** Caches are stored per model, so moving a conversation from one model to another starts over at the full rate.
- **Variable content near the top:** A tool that stamps the current date above your instructions, or an app that inserts freshly retrieved documents ahead of the conversation, builds a slightly different opening every time.


There are three takeaways here. First, avoid editing earlier segments of a conversation when you can. Second, work in concentrated bursts, since messages spread across a day miss the discount that a focused hour captures. Third, check that caching is on. OpenAI caches automatically above a minimum prompt length, while Anthropic and Google each offer automatic modes alongside manual controls; your provider's usage report will show how many of your input tokens billed at the cached rate.


### API batching


Not everything needs a response in two seconds. Anthropic, OpenAI, and Google all offer batch endpoints that accept large volumes of requests and process them asynchronously. Instead of handling each request individually and immediately, you submit a job and collect the results later, often overnight.


This can come in handy if you're generating summaries for large datasets, analyzing a wide variety of content, or producing dozens of documents or classifications. The turnaround window is typically up to 24 hours, though jobs frequently finish sooner. All three providers currently discount batch requests by about 50% on both input and output.


### Create an AI usage policy


It's a simple step, but drafting written rules around how your employees should be using AI in your company's policies can go a long way. Instead of trying to handle everyone's usage case by case, laying out the rules around API calls, model usage for different tasks, or weekly spend limits can help create a more accountable environment. It's the same idea as any company expense policy, just for a new type of cost.


For anyone on a seat subscription, the rules should focus on model discipline and output quality rather than cost per message. Lay out which tasks should be processed through AI and which shouldn't, then sub-divide those tasks by model tier. A lightweight model can whip up a social media post, while a frontier model can do a sweeping analysis of internal data.


Some businesses have turned to model routers, tools that automatically send simple requests to cheap models and hard ones to frontier models instead of leaving the choice to each person.


Engineers or anyone else using AI more heavily may need more specific rules. First, if you aren't using a router, you can name a default model for routine work and set parameters for using something stronger. You can also explicitly list how much token consumption or spend different roles are allowed each week, so a power user doesn't end up spending thousands on a recursive agent loop before anyone notices. Assign an owner to each API key, set a monthly review, and require a rough spend estimate before new integrations ship.


### Track and cap AI spending


It's one thing to lay out all your AI usage policies on paper. Enforcing them is harder without financial tools built for the job. Rather than reprimanding an employee for a weekend agent project that burned millions of tokens, you can stop out-of-control AI spending before it happens using configurable company cards and accounts.


Here's how businesses are using Slash to do this.


The first is card controls. Create a Slash card for an individual or a team, restrict it to only allow purchases from AI providers, and set a weekly limit matching whatever your policy allows.¹ Issuing a separate card per team also keeps each group's AI spend distinct, so you can tell who is spending what.


The second is a dedicated AI budget account. Open a virtual account, fund it with the team's monthly AI allowance, then attach each AI-specific card to it. Individual card limits govern how fast any one person can spend, while the account balance sets the ceiling for the team as a whole. Watching that balance draw down over the month gives you a running read on burn rate, and keeping AI spend in its own account means it won't cut into your operating cash for payroll or vendor payments.


Both approaches work because of the way AI providers bill users. Most now put new API accounts on prepaid credits, so your team buys a balance and draws it down as they spend tokens.[Anthropic requires credits to be purchased before use](https://support.anthropic.com/en/articles/8977456-how-do-i-pay-for-my-api-usage) , and[Google defaults new Gemini API users to prepay](https://ai.google.dev/gemini-api/docs/billing) . The charge therefore hits the card the moment someone buys or reloads credits rather than weeks later on an invoice, which is what lets a card limit stop a purchase instead of just recording it.


## Cut Back on Costs, Not Tokens


The companies getting the most out of AI are not necessarily the ones spending the most on it.


Using the discounts your providers already offer can cut the cost of the same output dramatically. Caching and API batching may sound like highly technical strategies, but they are accessible to most teams and only ask for a bit of internal policy and technical understanding.


Putting a hard limit on AI spend takes more than provider settings, though. With Slash, you can segment your AI budget in its own account, issue cards carrying the limits and restrictions your AI usage policy calls for, and get the visibility you need to make calculated adjustments to your spending. If your business needs better tools to manage AI spend, click here to get started with Slash today.


## Apply in less than 10 minutes today


Join the 10,000+ businesses already using Slash.


## Read more from us


[International Payments](https://www.slash.com/blog/category/international-payments)


Nov 4, 2025


### [How to Pay International Contractors in 2026](https://www.slash.com/blog/pay-international-contractors)


≈ 14 minutes read


[Corporate Cards](https://www.slash.com/blog/category/credit-cards)


Jun 11, 2026


### [What Is a Penalty APR? How Late Payments Raise Your Interest Rate](https://www.slash.com/blog/penalty-apr)


≈ 15 minutes read


[Crypto & Digital Assets](https://www.slash.com/blog/category/crypto-digital-assets)


Oct 25, 2025


### [The Best Crypto Wallets for Storing, Sending, and Receiving](https://www.slash.com/blog/best-crypto-wallets)


≈ 8 minutes read


[Business Banking](https://www.slash.com/blog/category/business-banking)


Aug 3, 2026


### [What Is Business Spend Management? Key Components Explained](https://www.slash.com/blog/spend-management)


≈ 12 minutes read


[Corporate Cards](https://www.slash.com/blog/category/credit-cards)


May 7, 2026


### [Card Issuing API Guide: How to Digitally Control & Manage Cards](https://www.slash.com/blog/card-issuing-api)


≈ 16 minutes read


[Accounting & Bookkeeping](https://www.slash.com/blog/category/accounting-bookkeeping)


Jul 16, 2026


### [What is a Depreciation Schedule? Guide + Examples](https://www.slash.com/blog/what-is-a-depreciation-schedule)


≈ 12 minutes read


Jun 29, 2026


### [Open a US Bank Account in the Philippines: Complete Guide](https://www.slash.com/blog/open-us-bank-account-philippines)


≈ 11 minutes read


[Platform Comparisons](https://www.slash.com/blog/category/platform-comparisons)


Mar 2, 2026


### [Bluevine vs. Novo vs. Slash: Financial Platform Comparison for 2026](https://www.slash.com/blog/bluevine-vs-novo)


≈ 12 minutes read
