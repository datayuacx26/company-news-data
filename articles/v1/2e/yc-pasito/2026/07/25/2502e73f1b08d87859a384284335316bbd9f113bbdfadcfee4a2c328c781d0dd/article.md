---
schema_version: "1.0.0"
document_id: "2502e73f1b08d87859a384284335316bbd9f113bbdfadcfee4a2c328c781d0dd"
company_key: "yc-pasito"
company: "Pasito"
source_id: "yc-pasito-news-import-e387a0e151ed"
canonical_url: "https://pasito.ai/blog/why-we-built-otis"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-22T08:19:57.648799+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:4dc9efb17a7dfe9701a4e6cdbe04a60c060ec4793bdc45da7c0abc290b029b2b"
---

# Why we built Otis: An AI benefits company’s case for building its own agentic software engineer

Every two weeks, one of our engineers rotates onto on-call. For that sprint, they own whatever comes through the door: client requests, urgent bugs, sales and demo asks. When the team was small, that worked. The person on call knew most of the codebase, knew most of the clients, and could triage a strange ticket by memory.


Then the engineering team doubled, and kept going toward triple. Our client count grew with it, and client count translates directly into the size of the on-call queue. At the same time, the newest engineers had the least context, so we faced an awkward choice: delay their on-call rotations while they ramped, or put them on call with a backup team standing behind them. Either way, the engineers with the most context kept absorbing the interrupt work, which is exactly the work that keeps you from building anything new.


That queue is where Otis came from.


## **Why an AI benefits company builds its own tools**


As VP of Engineering at Pasito, I'm thinking about the best way to build our product every day. The Pasito platform reads benefit plan documents. Our product reads benefit plan documents, extracts key information, maps new data to structured schema, then does the work that follows: builds the guides, answers the employee questions, and keeps the data consistent across every instance. The whole company is a bet that AI software agents can carry real operational work.


It would be strange if the engineering team itself didn’t work in that same, agent-empowered way. But I want to be honest about the stakes, because this isn’t a flashy feature announcement. Otis sits in the path of shipping real HIPAA-compliant code, and that code touches core benefits data. That constraint shaped every decision that follows.


## **The constraints that forced the build**


Three pressures stacked up at once: the team was growing, client onboarding was accelerating, and new products were in flight. The on-call rotation had to absorb all the interrupts without slowing the roadmap.


And a lot of on-call work repeats: the same bug categories, the same client configuration requests, the same demo setup asks. When the same ticket keeps coming back, that’s a signal that software should be doing more of the work. But the bar stays high, because our code has to meet compliance rules and get benefits details exactly right. That’s the squeeze every engineer knows: move fast and be correct, and don’t trade one for the other.


## **Why the coding assistants you can buy aren’t enough**


The question was never whether a tool can write code. Plenty can. The question was whether the tool knows our world: how our codebase is written, our domain rules, our Notion docs, our designs, our database, our industry, the history sitting in past pull requests and tickets.


The assistants we tried helped one line or one file at a time. The work we wanted handled was a whole ticket, start to finish, drawing on all of those sources at once.


And for us, the deciding issue was compliance. Handing a third-party tool access to our codebase is one decision. Handing it access to our internal docs and our database is a different one entirely. Every external tool we evaluated meant a new risk surface plus real vetting work: how does it process our data, where does it store it, does it meet the bar we hold ourselves to? With benefits data in the loop, “looks about right” isn’t good enough. We needed predictable steps and points where a person signs off, not a tool we can’t see inside.


## **Buy vs. build, honestly**


Treated as an engineering decision, the tradeoff looks like this. Buying means handing an outside vendor control over three things: what the tool knows, how it works, and how its output gets reviewed. Building means paying for that control with our own time.


For our situation, being able to control what the tool knows and the safety checks around it was the deciding factor. That control means we decide exactly what Otis can and can’t access, and it means we can evolve the tool for our specific use cases over time instead of waiting on someone else’s roadmap.


## **Why now?**


The honest answer to “why didn’t you do this earlier?” is that the models weren’t good enough earlier. A year ago, this would have been a side project that never shipped. Now it opens real pull requests against a production codebase. The shift is concrete: agentic coding tools crossed the line from autocomplete to finishing scoped work, and once that line is crossed, the economics of internal tooling change.


## **What Otis had to do**


Before writing any code, we set the requirements. Rules first, architecture later:


- Knows our world. Otis connects to our tools over MCP: the codebase and git history, Notion, Linear, the designs, the database, and our analytics.
- Follows set steps. It works in a predictable order instead of wandering off on its own.
- Fits the tools we already use. It picks up tickets in Linear and opens draft pull requests in GitHub, with notes on what it changed and how it was checked.
- A person always reviews and merges. That’s built in from the start, not bolted on later.
- Access restrictions are baked in. Compliance and data access rules are part of the design, so PII stays out of places it doesn’t belong.
- Model agnostic. Otis runs on Claude models today, and we can swap in other models.


We’ll cover how it actually works in an upcoming post, so stay tuned.


## **Why a person stays in the driver’s seat**


Otis is great at the part of on-call that eats the most time: collecting context. It pulls the code history, the ticket, the bug report video, and the relevant database state into one place before anyone starts debugging. What it still needs is an engineer to verify the solution and supply the context that isn’t written down anywhere.


That division of labor is a strength, and it’s most visible with new hires. An engineer in their first months doesn’t have years of product history in their head. With Otis assembling the context, they can take on-call tickets that used to require a veteran, and the process of reviewing Otis’s work teaches them the codebase faster than any onboarding doc we’ve written.


## **What changed**


The numbers are still getting sign-off, so here’s the change in words, with one placeholder: 78% of on-call tickets now close with Otis doing the legwork.


New hires take on-call rotations without a backup team, and they come out of those rotations knowing the platform better. The tool also escaped the engineering org, which we didn’t plan for: our UX designers use Otis to close UI bugs, and our operations team uses it to close client request tickets. Engineers spend more of their time on genuinely new problems, which is the point.


## **Where we’re taking Otis**


Today we’re in the middle of the maturity curve: an engineer fills in the missing context, assigns Otis, and reviews what comes back. The destination is an agent-first flow where Otis picks up the ticket with the right context already attached and completes the task on its own. Look at what doesn’t change: QA and an engineer’s review stay in the loop at every stage, all the way through.


**Traditional Software Development** Manual implementation cycle


Pasito member creates Linear ticket


Triage + PO cycle


Dev assigned


Dev works on it


Other dev reviews PR


QA review


Done


**Otis-assisted** Dev provides context, Otis builds


Pasito member creates Linear ticket


Triage + PO cycle


Dev assigned


Provide missing info + assign Otis


Otis completes task


QA review


Same dev reviews PR


Done


**After maturity** Agent-first with right context


Pasito member creates Linear ticket


Triage + PO cycle


Otis assigned with right context


Otis completes task


QA review


Dev reviews PR


Done


## **What I’d tell another engineering team**


Pay for your engineers’ AI subscriptions. Claude Code, Codex, Cursor, all of it, for everyone. Otis wasn’t on any roadmap. It came out of engineers having room to experiment, noticing a need, and being equipped to chase it. The cost and the bar for building internal tools that fit your exact needs have never been lower.


But start small, with one specific problem you want to solve. Ours was the on-call queue. Yours will be something else, and you’ll know it when you see the same ticket for the third time.
