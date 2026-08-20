---
schema_version: "1.0.0"
document_id: "c3b81cda6b31ff9ded7080a21c2c845cf055617ae5681b0e872b8c2c3d2927fd"
company_key: "yc-telmai"
company: "Telmai"
source_id: "yc-telmai-rss-c2782ae860ac"
canonical_url: "https://www.telm.ai/blog/from-ai-adoption-to-ai-native-how-we-rebuilt-engineering-at-telmai/"
published_at: "2026-04-06T15:44:43+00:00"
first_seen_at: "2026-07-20T23:20:35.564140+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:6474f734ec55ef5ed462452552a295c760e35165497c03902165492382b20a68"
---

# From AI Adoption to AI-Native: How We Rebuilt Engineering at Telmai

There’s a difference between encouraging your team to use AI and rebuilding your engineering process around it. Most leaders do the first. We did both at[Telmai](https://www.telm.ai/products/data-quality/) .


Here’s what that looks like.


## **Encouraging Is Easy. It’s Also Not Enough.**


We started where most teams start. “Try the tools. Experiment. See what sticks.” A few engineers adopted quickly. Most kept working the same way.


The gap wasn’t motivation. It was structure and process.


When AI tools are optional, engineers use them for the obvious low-risk tasks: generating sample data, writing a script, drafting a commit message. Useful. Not the shift you’re looking for.


Our co-founder and


CTO,[Max](https://www.linkedin.com/in/maximlukichev/) , and I spent a day feeding our product docs, UI screenshots, and notes into an AI and asked it to rebuild a simplified version of our application. It didn’t build everything. But the UX thinking it produced, and the small details it surfaced that our own team had been missing, were jaw-dropping. It also pointed us toward simplifying features we already had. A day of work. More product clarity than we’d gotten in months of sprint planning.


That’s when we decided optional wasn’t working. We had to rebuild around it.


## **How Adoption Actually Happened**


We didn’t flip a switch. We ran a deliberate phased path.


Phase 1 was low-risk and isolated: sample data generation, CLI helpers, small standalone changes. Low stakes, fast feedback. This is where skeptics start seeing the point.


Phase 2 was harder. We rearchitected parts of the codebase carrying too much accumulated debt for AI to navigate effectively, and started building medium-sized features agent-first. We accepted that some code needed to be rebuilt. Not because it was broken. Because it was structured in a way that made it opaque to AI.


Phase 3 is where we are now: major features, built agent-first from the start.


We’re there now. Full end-to-end features, design through testing, built agent-first. That’s not the goal anymore. That’s the baseline.


## **30% Of The Team Pushed Back. That Was The Right Response**


The objections were specific: it hallucinates too much, it doesn’t understand our existing codebase, and I don’t trust what it outputs.


These aren’t wrong. They’re accurate observations about where AI breaks down. We didn’t argue with them. However, we gave those engineers something more useful than a counter-argument: protected time to work through those limitations on real features, not toy examples.


We also accepted that removing technical debt wasn’t a side project. A messy codebase is hard for humans to navigate. It’s worse for AI. Some of what we rearchitected wasn’t about code quality. It was about making the codebase legible enough for AI to reason about it.


The skeptics converted when they saw the output. Not when we made the case for AI.


## **The Numbers**


Our team went from 0% AI-assisted code to 80-90%. Velocity gains range from 5-6x on legacy code to over 10x when the stack is built AI-native from the start. The difference is the codebase, not the AI. We’re pushing toward 100% AI-assisted. Not there yet, but that’s the target.


A core deployment architecture change we had been deferring for two quarters got done in a day. That was the moment the team stopped debating whether this was real.


## **What Actually Changed**


It didn’t change how many engineers we need. It changed what we need from them.


The engineers moving fastest aren’t the ones with the deepest technical expertise alone. They’re the ones who think clearly about what to build, care about what it actually solves for the customer, and can direct AI to get there.


That shows up in how we work now. A feature isn’t just a ticket anymore. It’s a well-written prompt, clear acceptance criteria, and enough context for the AI to reason about intent. The engineers who can write that well ship faster than the ones who can’t.


The question we ask now isn’t “how much can this engineer produce?” It’s “how much value can this engineer drive?” Those aren’t the same question. For a long time, we treated them like they were.


## **The New Bar**


The best engineer on my team today isn’t the one writing the most code. It’s the one who can own a full feature: understands the problem, defines the outcome, and directs AI throughout the process. That scope used to require a team. Now it doesn’t. The bar has moved.


The shift is there for any team willing to make it. Encouragement is the easy path. Rebuilding is the harder one. We chose the harder one. The difference is measurable.


Passionate about data quality? Get expert insights and guides delivered straight to your inbox –click here to subscribe to our newsletter now.
