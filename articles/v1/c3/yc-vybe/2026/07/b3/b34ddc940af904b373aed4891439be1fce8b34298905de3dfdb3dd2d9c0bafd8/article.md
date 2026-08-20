---
schema_version: "1.0.0"
document_id: "b34ddc940af904b373aed4891439be1fce8b34298905de3dfdb3dd2d9c0bafd8"
company_key: "yc-vybe"
company: "Vybe"
source_id: "yc-vybe-news-import-452c5ac9be13"
canonical_url: "https://www.vybe.build/blog/learn-what-not-to-tokenize"
published_at: "2026-07-06T12:42:25.753+00:00"
first_seen_at: "2026-07-24T07:18:06.647302+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:3a4c976bbb9fe06125d0db40180873c5fe8c379de3b7763386decb0d245bc692"
---

# Not everything should cost a token: the case for deterministic AI

A team I talked to recently wired up an agent to do something simple: pull a metrics API every morning, reshape the JSON, and drop the result into a table. Clean idea. It worked on day one.


Then the invoice came in. Every morning that job loaded a few thousand tokens of raw JSON into a context window and asked a language model to reformat data it had no reason to think about. It occasionally got the reformatting wrong, because a probabilistic model was handling work that a five-line script does the same way every time. And the context window filled up with stale data, so the reasoning the agent was actually there to do got weaker.


That trap catches almost everyone right now. Teams reach for the language model as a universal runtime: if you can describe a task in a sentence, you prompt it. But a model is a reasoning engine you rent by the token. Point it at a cron job or make it hold your data and you pay a premium for work that never needed a brain in the first place.


The skill worth learning is knowing what to keep away from the model.


I am not the only one saying it. Ivan Burazin, who runs Daytona, made the same point this week:


> Someone on our team wanted to copy text from a screenshot. He uploaded the image to Claude and asked it to retype the text. I watched him burn tokens to do what Preview on Mac does natively for free in two seconds. \[...\] Even agents are constantly using inference for tasks that should be deterministic. Once the cost pressure catches up, the stack will change.
>
>
> —[Ivan Burazin](https://x.com/ivanburazin/status/2074175297126076603) , founder of Daytona


His example is trivial on purpose. You never notice it on any single call. Run it a few thousand times a day and it turns into a real line on the invoice.


## Why "everything is a prompt" feels right and costs so much


The appeal is obvious. Prompting is fast. You skip the schema design, the endpoint, and the deploy, and just ask. For a genuine one-off, that is the right call.


The pain shows up when the one-off becomes a recurring job. Route a deterministic task through a model and it inherits three properties it should never have had: it turns non-deterministic, it slows down, and it starts metering. You pay, per run, for work a scheduled function would do for free and get right every time.


Two symptoms follow, and they feed each other.


First, cost that scales with the wrong thing. Token spend should track the value of the judgment you ask for. Instead it tracks the volume of mechanical work you push through the model. Pull ten APIs a day instead of one and your bill grows tenfold, even though none of that pulling took any intelligence.


Second, context bloat. To process data, teams read it into the context window, which is finite and is where reasoning happens. Fill it with raw records and you crowd out the reasoning the model is actually good at. Quality drops, and because most providers meter[input tokens](https://platform.openai.com/docs/guides/prompt-engineering) , a long context costs more at the same time. You pay more and get worse answers. That part stings.


## Sort the work before you decide where it runs


Here is the mental model I keep coming back to. Every task falls into one of two buckets.


Belongs with the **agent** (probabilistic) Belongs in the **app** (deterministic)


Judgment and interpretation Scheduled and recurring tasks


Drafting, summarizing, classifying API calls and data transforms


Deciding whether something matters Storing, retrieving, and querying data


Handling ambiguity and edge cases Anything that must be exact and repeatable every run


The line is simple: tasks that need judgment go to the agent, and tasks that have to be exact and repeatable go to an app.[Determinism](https://en.wikipedia.org/wiki/Deterministic_algorithm) is a feature you want. A scheduled job that runs identically every morning should be boring and predictable.


Here is the anchor for the whole idea. You build the app with tokens once, then stop paying tokens every time it runs. The intelligence goes into creating the machine. The machine then runs for free.


## The one everyone gets wrong: memory notes as a database


There is a specific version of this mistake worth calling out, because I watch smart people make it constantly. They use their agent's memory or notes as a database.


They drop structured data into notes: records, metrics, pipeline stages, customer lists. It feels natural, because the note is right there and the agent already reads it. But notes make a poor database. There is no schema to enforce consistency. There is no query, so the agent reads the entire note into context just to pull one field. And the whole thing degrades as it grows.


Memory notes still earn their keep for context that has no clean schema and never will: preferences, tone, patterns the agent has learned, the reasoning behind past decisions. That kind of context is unstructured on purpose, and it exists to inform judgment. So the rule cuts both ways. Structured, queryable, high-volume data belongs in a real database. Unstructured, interpretive context belongs in notes. Match the tool to the shape of the data and both problems disappear.


## What this looks like on Vybe


This is where it stops being theory and turns into a build decision. On[Vybe](https://www.vybe.build/?utm_source=blog&utm_medium=article&utm_campaign=learn-what-not-to-tokenize) , agents do more than chat. They build and operate real applications, which gives the agent somewhere to put the deterministic work.


Take a churn-analysis workflow. Here is how you would build it so the model only spends tokens where they earn their keep.


**Step 1: Build the app.** Ask Vybe to spin up an app with a database of your customers. The agent designs the schema and builds the app once. Building it costs tokens; running it does not.


**Step 2: Automate the mechanical part.** Add an in-app cron that updates each customer's status on a schedule. This is pure deterministic plumbing. It runs identically every time, touches no model, and costs nothing per run.


**Step 3: Reserve the model for judgment.** Ask Vybe to build a second cron that runs once a week. It queries the customer database for at-risk accounts, cross-references your production database to find why those customers are slipping, and emails you a summary.


Look at where the tokens actually go. Steps 1 and 2 are one-time or zero-per-run. In step 3 the querying is deterministic, and the only thing the model reasons about is the "why are they churning" question, once a week. Everything else runs as cheap, reliable code. Build that same workflow entirely inside the model (status updates, queries, and all) and you would pay tokens on every row, every run, forever.


## Why this is hard to copy


Most tools cannot work this way, for structural reasons rather than a lack of polish.


Pure chat-agent assistants, the single-model copilots and Slack bots, have no app layer to offload to. Every scheduled task, data pull, and lookup runs as tokens, because tokens are all those tools have. Pure no-code and app builders have the opposite gap: they run deterministic apps all day but have no reasoning layer deciding what matters.


Vybe puts the reasoning agent and the deterministic app in the same place, built together by the same system. And because Vybe is model-agnostic, you avoid getting locked into one vendor's token rates for whatever mechanical work does still run through a model.


## A checklist for what to keep off the model


Before you prompt, run the task through these questions.


- Does this need judgment, or is it mechanical? Judgment goes to the agent; mechanical goes to an app.
- Does it repeat on a schedule? Move it into an app-level cron and stop paying per run.
- Is the agent making an API call directly? Replace it with an app endpoint that makes the call.
- Are you keeping structured data in notes? Move it to a database and query it when you need it.
- Is data sitting in your context to be reasoned over, or just stored? If it is storage, get it out of the window.


Run through that list a few times and it becomes instinct. You start to feel which tasks deserve a token and which ones are just burning money.


## The point was never "more AI"


A good agent system applies intelligence exactly where it earns its cost and lets cheap, deterministic code handle everything else. Build the machine once, then let it run. Your model should spend its time on judgment and leave the plumbing to the app.


Vybe is built for exactly that: agents that reason and apps that execute, together, so you stop paying a premium to make one do the other's job.[See what the AI plus apps model looks like in practice](https://www.vybe.build/?utm_source=blog&utm_medium=article&utm_campaign=learn-what-not-to-tokenize) , or[browse the agent gallery](https://www.vybe.build/gallery) to see it running.
