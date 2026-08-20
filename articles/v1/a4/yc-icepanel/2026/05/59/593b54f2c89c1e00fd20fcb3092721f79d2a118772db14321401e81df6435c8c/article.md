---
schema_version: "1.0.0"
document_id: "593b54f2c89c1e00fd20fcb3092721f79d2a118772db14321401e81df6435c8c"
company_key: "yc-icepanel"
company: "IcePanel"
source_id: "yc-icepanel-news-import-9cf2a09ec197"
canonical_url: "https://icepanel.io/blog/2026-05-05-adopting-adrs-in-enterprise"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-21T23:29:04.459524+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:1cb0ad7c2f19435df92e44361e13a0e9a75d6889c968518c31959a8ef3b3c6ad"
---

# Adopting ADRs in Enterprise

For anyone who has spent the past few years repeatedly giving LLMs the context behind something before it can give you a useful answer, then I’m sure you’ve come to understand just how easy it is for information to become siloed inside even a small organisation and maybe known only by you.


In enterprise organisations, this unintentional information gatekeeping not only means team members have to spend time running around searching for the right person to understand why an architectural decision was made three years ago. A lot of the time, the reasoning behind those decisions has eroded away or been forgotten completely.


This isn’t good, and it’s certainly not useful if you intend to dust off your auth flow and consider something new, but need to check in with those who built the original version. Those previous decisions may be propping up other architectural decisions. It’s like a stack of fragile playing cards, ready to tumble at the slightest gust of wind.


There’s a whole world of ways in which teams may choose to share why important decisions were made. Maybe in long-form text in a word editor, maybe in a markdown file tied to a repo, a Slack thread, an email or perhaps a mixture of all of them (which is unfortunately most common). You get the idea, these decisions and most importantly all of that context is scattered, and often dependent on the right person still being around.


That’s one reason I’m so sold on ADRs (Architectural Decision Records). Designed to serve as lightweight records of the context behind architecture decisions, ADRs are a log of important decisions made by a team, and the context that surrounds them.


## What goes inside an ADR?


Michael Nygard, the man who popularised the idea of ADRs,[suggests](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) they should consist of five things: a title (a logical name for the decision), a status (was this idea adopted, rejected or superseded), the context, the decision itself, and the consequences.


That’s a great default and many teams also benefit from expanding on it slightly. One of the most valuable additions is an alternatives considered section, a record of the options you evaluated but didn’t choose, and why. This is where the real value of an ADR lives.


Without this, future engineers have no way of knowing whether an alternative was already debated and ruled out, or simply never considered. The difference matters enormously when someone shows up three years later, suggesting you switch from REST to GraphQL and the team has no memory of the two-week debate that already happened… 3 times!


You might also consider including assumptions and constraints that were true at the time of the decision. Architectural decisions are often only correct under a specific set of circumstances. Capturing those circumstances makes it far easier to know when a decision should be revisited.


Example ADR inside IcePanel


## Keep them concise


ADRs shouldn’t become short novels. The goal is a document that’s easily digestible, something an engineer or architect can read in five minutes and walk away with a clear understanding of what was decided and why.


It’s far too easy to create documentation that’s “over-engineered” (please pardon the pun).


The reality is that fewer people will read longer documents, and if the important details are buried three pages in, people skim and miss things. The goal should be to keep the main signal as clean as possible. You can always link out to supporting documentation if a topic needs expansion, but resist the urge to make the ADR itself overly explained.


## Who writes them, and when


ADRs shouldn’t be the exclusive domain of architects or tech leads. Any engineer driving a significant decision should be authoring one. If people assume it’s someone else’s job, they simply never get written. You may choose to have them only approvable by a tech lead or architect, but they can be approached collaboratively.


The right moment to write an ADR is before or during the decision, not after. After-the-fact ADRs have a tendency to become rationalisations once the outcome is already known. If they are written in the moment, they capture the genuine uncertainty, the tradeoffs actually considered, and the constraints that were real at the time. That’s what makes them useful later.


## ADRs have a lifecycle


One of the most important and non-obvious things about ADRs is what happens when a decision gets revisited. You don’t delete the old record. You mark it as superseded e.g. “superseded by ADR-042” and write a new one explaining what changed and why. This creates a useful audit trail you can follow through time. This kind of internal org memory is rare and has outsized value.


## How granular should ADRs be?


As a rule of thumb, any decision that required significant debate, or carries a large number of upstream or downstream dependencies, is worth logging. Choosing PostgreSQL over MongoDB for a new core service is an ADR. Deciding on a caching strategy that affects multiple teams is an ADR. Renaming a variable, or choosing a library for a one-off script, is not.


If you find your team debating whether something warrants an ADR, it probably does.


## Rolling this out


If you’re interested in adopting ADRs across your organisation, attempting to do this across all teams at once is possible, but not advisable. Start with one team. Ideally, the tech lead is already bought into the idea and understands the value of having important decisions be searchable and clearly logged. Let that team build the habit, refine the template to suit your organisation, and then introduce the concept to other teams, with evidence of how this has been useful for the original team. Start small, experiment, customize, then scale.


## Why is now a good time to implement this sort of structure?


LLMs rely on context to give you useful answers and output. The greater the context, the greater the answer. With the IcePanel MCP, your AI tooling can now read directly from IcePanel. Super handy if you’re about to set an agent on a task to rebuild a part of your service and need to surface constraints. It’s becoming clear that teams who invest in internal memory now are going to get significantly more out of AI tooling than those who don’t. ADRs are a small habit with compounding returns, and it couldn’t be a better time to start building that habit!


Get started and[write your first ADR](https://app.icepanel.io/) ✍️
