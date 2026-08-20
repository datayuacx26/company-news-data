---
schema_version: "1.0.0"
document_id: "113de5bc8c57a9ef15aaab01b50547715f3c71da8c48ec87a8c72b9f358ac540"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-62ed1fb859d3"
canonical_url: "https://slite.com/blog/dont-drop-claude-code-add-what-it-cant-see"
published_at: null
first_seen_at: "2026-07-24T13:26:47.289442+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:a289666eda3fcd3f69119b697cf3a814898938aa25a188fdea8483ac70ca696a"
---

# Don't drop Claude Code. Add what it can't see

We ran a test a while back.


We gave 2 different Claude setups the same simple question, *"What has Jason done in the last 12 months?"*


One Claude instance was connected to Slite Agent.


The other was Claude wired to the same tools through MCPs.


Our brain knew Jason was an employee and pulled what he'd shipped that year. Claude with MCPs didn't know who Jason was, so it fired off in every direction, found the name scattered across our CRM, latched onto 2 deals where the point of contact happened to be named Jason, and returned a polished summary that was completely wrong.


The poor agent didn't know better. *We think this might be happening to your agents too* . They might be underperforming right now because **MCPs fed them false context and can't tell it's false.**


For personal work, it's fine, *you move on* , but if you're AI-native, it tells you that no agent can be trusted to run on its own. It doesn't know which info is current, so every task needs hand-holding. A company brain, like the one we attached, already knows, because everything's plugged in and clean.


Maybe you've already reached this conclusion and said:


> I've connected a few MCPs to Claude, it kind of works, and a dedicated tool for context feels like overkill on top of our current plans. I'll take the occasional hit without inflating spends.


Washing dishes by hand is fine too. But a dishwasher is instrumentally better, and once you've lived with it you don't go back.


## Why MCPs don't cut it when it comes to context


You can already see timeouts and tokens burn but can probably work with it.


But underneath them is a myriad of reasons that make MCPs unfit for context.


They're missing fundamental things that make retrieval trustworthy, and no amount of configuration adds them back.


Here's a list of 5 reasons we think makes MCPs bad for context retrieval:


1. Lack of freshness signals
2. No entity extraction
3. Bloating of the context window through raw responses
4. Improvised tool use
5. Tool count degrades accuracy


Let's go into details of each one.


### No authority or freshness signals


An MCP has no idea what's authoritative or current.


It'll surface a number from a stale Slack message with the same confidence as the number from your source of truth, because to an MCP they're both just text it found.


AI workflows need retrieval that scores for authority and freshness beyond naive semantic query matching.


### No entity extraction or resolution


In our "What did Jason do?" test, MCP saw a string, "Jason," and matched it everywhere that string showed up. It never knew Jason was a person on our team, separate from the 3 other Jasons sitting in your CRM as deal contacts.


Without entity understanding, your agent is pattern-matching without reasoning about your company.


AI workflows need retrieval that does real entity extraction, resolving "Jason" to a known person *within* the company before it even decides which tools to call.


### Raw responses bloat the context window


Every raw MCP tool response gets dumped straight into the model's context.


One API call can return a wall of JSON that's mostly noise, and your agent pays for all of it, in tokens and in the limit you're slowly burning through.


AI workflows need a retrieval layer that filters and ranks results before they ever reach the context window.


### Non-deterministic, improvised tool use


When you ask through MCPs, the agent improvises its retrieval on the fly. It decides, in the moment, which tools to call and how.


Ask the same question tomorrow and it may take a completely different path and reach a different answer. You can't build a reliable team workflow on something that won't behave the same way twice.


AI workflows need retrieval with orchestration defined ahead of time, not tool use the model reinvents on every call.


### Tool count degrades accuracy


The instinct, when retrieval is shaky, is to add more connectors.


More tools should mean more coverage and better answers. It works the other way.


Accuracy degrades as you hand an agent more tools to juggle. A May 2025 paper from arXiv tested what happens to tool selection as the MCP pool grows from 1 server to over 11,000.


Past around 100 tools, retrieval precision collapsed. When every tool description gets dumped into context at once, selection accuracy fell to 13.62%.


The same queries, with only relevant tools surfaced, hit 43%. Same model. Same questions. The only variable was how many tool definitions the agent had to wade through.


*(RAG-MCP: Mitigating Prompt Bloat in LLM Tool Selection via Retrieval-Augmented Generation, arXiv 2505.03275, May 2025)*


Every connector you add slows the whole thing down and widens the set of choices the agent has to improvise through, giving it more places to get confused.


In the Jason test, having more tools is exactly what sent it into a CRM when it had no business being there.


AI workflows need a retrieval layer that already holds the schema of where your context lives, so the agent isn't picking through a dozen tool definitions on every question.


### Where MCPs actually shine


MCPs are great for appending data to your actual tools, or one specific retrieval scenario. That said, **MCPs are great for retrieval only when you already know where the data lives and exactly what you want from it.**


If you want to append your research to a specific Jira ticket, an MCP is the right tool and it'll do it cleanly. They fall apart the moment the agent is the one that has to figure out where the answer is.


Retrieval-heavy AI workflows need context that understands the full map of where everything lives. Direct MCP calls are for when you already are the map.


## "But MCPs are good enough"


We hear this on almost every call. After wiring a few MCPs into Claude, watching it answer a question or two, it's easy to decide that a dedicated tool for context sounds like an overkill.


The catch is that "good enough for me, right now" and "good enough for my team, every day" are different bars.


### "It's already free"


If you say: "I already pay for my Claude subscription and get MCPs for free, so why pay another vendor on top of plans I'm already covering. I'll eat the occasional wrong answer rather than inflate spend."


Fair. *Until* you look at what the occasional wrong answer actually costs.


[Hasan from Wuffes](https://slite.com/customers/wuffes) tried to build this himself.


$200 of tokens, a weekend, parallel MCP calls wired together into something that looked like a company brain. It worked on simple questions.


- Then it hit entity resolution, the same name appearing as a customer, a teammate, and a deal contact, and returned the wrong one confidently.
- Stale data came back with the same weight as current data.
- When a connector updated its API, a chunk of the retrieval quietly broke and nobody knew until an answer was wrong enough to notice.


If you workshopped a solution like this, would you deploy it to your team? Would you hand them this DIY and trust they'll be happy with its quality?


### Free is the cost of your reputation


Good enough holds right up until you point an agent at something that runs without you watching.


The moment you want a workflow that runs while you sleep, or a number your CEO reads in a board update, the bar moves. A confidently wrong suddenly becomes double work when someone catches it, or worse, nobody catches it at all.


Picture


- A help-center article that auto-updates from a meeting note where someone was just thinking out loud, and the agent treats that musing as settled truth.
- A support rep who pulls an answer from your internal tool, sends it to a customer, and it's wrong. The customer gets frustrated and churns.


There's a name attached to that mistake now, and it's probably yours.


"I'll just maintain it" - Do you have the time?


Harness improvement is literally a full-time job for our dev team. They had to develop 6 guards against hallucination prevention *alone* . And while it gets us to 95%+ accuracy, it's still a work in progress until we get as close to 100%.


### The cost you're not counting


Every time you double-check the agent because you don't fully trust its information, that's time nobody put on an invoice. The MCP feels free only if you think your time is.


The real risk comes up the first time you're confident enough to take yourself out of the loop, let the workflow run, and something wrong reaches a customer.


MCPs leave that gap wide open because they have no review gate, no verification step, nothing between the agent's guess, and the person who trusts it.


At 10,000 extra tokens per request, a conservative number for a multi-server MCP setup, a team making 1,000 daily queries is burning 10 million extra input tokens a day on tool definitions alone. At current model rates, that's around $30 a day in pure overhead, before a single answer comes back. Multiply that by the reruns when an answer is wrong, and the "free" option has a real invoice attached to it.


Timeframe Queries Wasted tokens (tool defs only) Overhead cost Real cost incl. reruns*


Per day 1,000 10M $30 ~$39


Per month ~20K 200M $600 ~$786


Per year ~250K 2.5B $7,500 ~$9,825


Pure tool-definition overhead at ~$3 per million input tokens, before a single answer token comes back. "Reruns" applies the 31% wrong/poor rate from our own benchmark, roughly a third of answers get asked again. Assumes ~20 working days/month, ~250/year.


A company brain closes *exactly* that.


## The antidote? A company brain.


A company brain is a tool with one job. To know everything about your company the way a good coworker does. It runs on its own, around the clock, and acts as a layer of memory and reasoning that any agent or workflow can draw on, across the context scattered across every tool you use.


### It's not parallel MCP calls with a UI


"I can reconstruct a company brain with $200 of tokens and MCPs over a weekend because it's just parallel MCP calls by an AI doing RAG"


For instance, under the hood, Slite Agent has an agentic retrieval pipeline:


- A query planner breaks your question into parts and decides how to answer each one.
- It pulls from every relevant source in parallel, across semantic search, keyword search, SQL, and live API calls.
- It returns only the top hits plus a map of where more lives
- Reranks what it found
- Checks whether the results are consistent enough to answer
- Loops back if they aren't.
- Every answer comes back cited, with the source, owner, and timestamp.


This is a bird's eye view and everything that happens for each query that passes through before an answer is constructed.


Moreover, it respects source-level permission, so the newest intern cannot find what was said in the leadership meeting last week. It has an intelligent model routing that promises speeds that would be much faster than your state-of-the-art models doing random MCP calls while being more accurate, and it comes with GDPR compliance and data security.


### Why you don't build this yourself


You could try to assemble this out of MCPs and a frontier model. People do. Then they spend their quarters maintaining it.


Entity resolution, verification, de-duping, ranking, freshness tracking, you'd be building and owning all of it. A tool changes its API or its MCP config and half your workflows quietly break. Your retrieval and reranking run on a frontier model, so the bill climbs every time someone asks a question.


And the whole thing only works if every connector stays configured correctly, which it won't.


The difference in practice:


- Agorapulse saw a 90% reduction in internal questions after connecting their knowledge base through Slite Agent.
- Instaleap went from sporadic usage to 478 queries a week, hitting a record 2,606 in a single week by March 2026.


### Unlocks novel workflows


Of course, the brain is right more often, but it also unlocks net new work that can *never* be done with MCPs.


If you ask why a customer churned, Slite Agent stitches:


- The hesitation from their very first sales call
- The notes from every customer success touchpoint
- Every Intercom ticket they filed
- The product-usage data underneath


Into one cohesive read on what actually happened.


Likewise, you can also give it a client name and it will pull


- A company overview
- Subscription details
- Renewal history
- Key contacts
- Product usage and patterns
- Full communication timeline


And a lot more. In fact,[here](https://www.linkedin.com/posts/christophepasquier_i-know-exactly-how-to-sell-super-to-a-sales-ugcPost-7424719423179186176-KEpY/) 's a video where I’m giving a walkthrough of how I use this workflow myself every week.


If you try to replicate the same with raw MCPs, Claude gets lazy, finds a few things and stops short of the answer you needed.


## What you're actually missing


We ran 41 questions against our own company data.


We gave the same questions to Slite Agent and to Claude wired to MCPs covering the same sources.


We rated the answers blind, 0 to 4. Here's what we found.


Claude + MCPs Company brain


Accuracy 2.61 / 4 avg, 24% perfect answers 3.59 / 4 avg, 83% perfect answers


Wrong or poor answers 31% of the time 9% of the time


Speed to a complete answer 101.9s avg, up to 282s on multi-source 39.5s avg, ~2.6x faster


Context per question raw payloads, ~90% noise in our test ~3.5x leaner, almost all signal


Autonomy needs a human in the loop trustworthy enough to run unattended


*41 questions across simple lookups, people queries, multi-source synthesis, and data retrieval. Rated 0 to 4 (0 = wrong or hallucinated, 4 = factually correct, well-formatted, and contextually complete). Blind-rated by a Slite team member against our own production data. Methodology available on request. Read[our benchmark](https://super.work/blog/how-do-mcps-compare-against-a-dedicated-company-search-agent) for more details.*


### Accuracy


Across 41 real questions, our company brain scored 3.59 out of 4 and gave a complete, perfect answer 83% of the time.


Claude with MCPs scored 2.61 and got there 24% of the time. It also returned a wrong or poor answer in 31% of cases, against 9% for the brain.


When retrieval is improvised, a third of your answers are quietly off, and you have no way to know which third.


### Speed


On a single policy lookup, Slack's search came back in 11 seconds and our brain took 49. Faster, except the 11-second answer never actually answered. It returned 15 keyword matches and none of them were the policy.


The 49 seconds bought a finished, cited answer. On the questions where the MCPs *did* return an answer, the brain averaged 2.6x faster across the benchmark, and on multi-source questions the gap ran as wide as 35 seconds against 282.


MCPs call each tool one after another, so latency stacks up with every source, and one slow or failing tool stalls the whole chain. A brain hits everything in parallel in a single pass. Adding sources barely moves the clock.


### Tokens and usage limits


This is the cost you feel even when the answer is right.


Every MCP tool you connect loads its definitions into context before you ask anything.


One team measured 3 MCP servers eating[143,000 of 200,000 tokens](https://www.stackone.com/blog/mcp-token-optimization/) , 72% of the window gone to tool definitions alone.


GitHub's official MCP server runs about 17,600 tokens a request. Then the responses pile on top, raw JSON dumped in untrimmed.


[Anthropic published](https://www.anthropic.com/engineering/code-execution-with-mcp) a whole engineering piece on cutting MCP token cost.


> Every intermediate result must pass through the model. In this example, the full call transcript flows through twice. For a 2-hour sales meeting, that could mean processing an additional 50,000 tokens. Even larger documents may exceed context window limits, breaking the workflow.
> With large documents or complex data structures, models may be more likely to make mistakes when copying data between tool calls.


Every token spent on bloat is a token off your context window and a slice of your usage limit, which is why a few calls can eat 10 to 20% of your five-hour cap on what felt like nothing.


### Autonomy


Accuracy you can trust without rereading, speed that holds as you add sources, and a context budget that doesn't evaporate, each one on its own is a convenience.


Together, they're the difference between an agent you have to babysit and one you can let run.


That's **autonomy** , and it's the only reason any of this matters. You don't want a faster way to look things up. You want workflows that run while you sleep and answers you'd put in front of your CEO.


## How do you actually make the decision?


If you're a team of 5 doing the occasional lookup, MCPs are genuinely fine. This framework is for when you're not sure anymore.


So before you decide, run yourself through these. The more of them that land, the more this stops being optional.


### **How many of your workflows depend on context today?**


For one or two personal lookups, stay on MCPs. Once you've got a handful of recurring workflows that pull company context, or a single agent meant to run unattended, you've already outgrown them.


### **How many sources does a typical answer touch?**


If your real questions only ever hit one tool, an MCP call is fine. The moment a good answer needs 3 or more sources stitched together, you're paying the fan-out tax in latency and accuracy every single time.


### **How much does it cost you when an answer is wrong?**


If the output stays between you and your screen, wrong is cheap. If it reaches a customer or gets treated as your source of truth, wrong gets expensive fast. Rank your workflows by who sees the mistake.


### **Is anyone reviewing before it goes out?**


If you're heading toward workflows that run without you in the loop, you need a verification step built in. MCPs don't have one. A company brain reviews and cites by default.


### **How often does your stack change?**


Every time a tool changes its API or you add a connector, hand-wired MCP setups quietly break. If your tooling moves even a few times a year, you'll spend more time repairing plumbing than using it.


### **What is your AI spend actually buying?**


You're likely already paying a frontier model to do clumsy retrieval, burning tokens and usage limits on every fan-out. Add the cost of reruns when answers come back wrong, and the cost of cleaning up after a public mistake. Netted out, a company brain is usually cheaper than the way you're doing it now.


## Or, just don't choose


MCPs aren't going anywhere, and you shouldn't want them to. When your agent needs to write a follow-up into the CRM or move a ticket, an MCP is exactly right. What they were never built to be is the place your agent reasons from.


Let the company brain hold the full map and decide what's true. MCPs reach into a single tool and change one thing. Wire them together and you stop choosing between trust and action.


Once context is solved, the work itself changes. The agent stops being a search box you babysit and starts being closer to a teammate. It can run your churn debrief before the call, keep your help center honest against what's true today, and hand your CEO a pipeline read you'd actually stand behind while you sleep.


If you'd like,[book a demo](https://slite.com/book-demo) and we'd love to show you what that looks like on your own data, with the questions your team actually asks on a Tuesday.
