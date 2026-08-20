---
schema_version: "1.0.0"
document_id: "753d60149a7ce13efcb150dfeb99d068a10149c266107408f02b20e5ad111aab"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/how-wix-saved-650-developer-days-in-one-quarter-by-automating-code-migrations"
published_at: "2026-07-22T14:10:18+00:00"
first_seen_at: "2026-07-22T14:39:28.795505+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:4f218928d36c16a0d30b381bddddbc231159da1e42490cff63e977536d8b1197"
---

# Code Migrations Don't Have to Hurt. This is how switched from cross team manual work to an automatic infrastructure

**No one wakes up excited to do a code migration.**


Deprecated libraries, method version upgrades, infra changes - they happen constantly, they affect dozens of teams, and they always arrive at the worst possible moment. A small technical change somehow becomes months of coordination. Slack threads. Priority negotiation. Manual tracking. Reminders. Many google sheets.


For years, I managed cross-team projects at Wix, and migrations were always the hardest part. Not because they're technically complex, but mainly because they depend on everyone else. My job was essentially to request priority shifts, negotiate timelines, unblock people, track everything manually, and repeat. Indefinitely.


At some point I asked myself:


**what if migrations could just... happen?**


That challenge led us to build


**Code Migration Platform** - and it changed how we think about platform-wide change.


##


**The Scale of the Problem**


To understand why this mattered, here's what code migrations look like at Wix's scale:


-


**50 migrations** initiated annually by various infrastructure teams.


-


**Hundreds of repositories** affected in each migration.


-


**6,000 repositories** touched in total every year.


-


**25% of developer time** across the organization is spent on migrations.


That last number stood us the most. We'd rather spend that engineering capacity building features, improving reliability, and exploring new ideas. Instead, a quarter of it is consumed by migrations. Although these migrations are necessary to improve the platform, they represent a significant opportunity cost.


This brought us to our core insight:


**This is more an organizational challenge than a technical one.** The bottleneck isn't engineering ability - it's engineering attention.


##


**The Idea Behind Code Migration Platform**


The spark came during a particularly painful migration. I was trying to coordinate a change across 20 teams - teams who didn't yet know they were involved. The usual process was painful: identify affected repos, explain the change to each team, wait for their bandwidth, follow up, and repeat.


We stepped back and asked a different question:


What if the migration owner could identify all affected areas in the code, explain the change once, create the PRs everywhere automatically, and track everything in one place? This is the product. Explain once, apply everywhere.


##


**How It Works: Three-Layer Architecture**


Code Migration Platform is designed as a self-contained ecosystem built on three distinct layers:


###


**1. The Migration Dashboard**


This is where the flow starts. The migration owner identifies all targets - a specific repository and path within that repo where the change needs to apply. For simple cases, a basic search is enough. For cases requiring code dependency awareness, we built a


**smarter target finder** that runs periodically and automatically surfaces new targets as they appear in our codebase.


###


**2. The Execution Layer**


Targets and prompts are grouped into batches. Each batch runs on Buildkite, and a code agent, with the migration owner choosing their preferred model - opens each repository, applies the change, and creates a pull request. No human intervention required per repo.


###


**3. The Database and Tracking**


This layer closes the loop. Every migration, PR, and batch is stored. PR statuses are pulled continuously from GitHub Actions. The full history of every target is visible - if a PR's check status changes at any point, it's reflected immediately on the dashboard. The migration owner has a single place to see everything, always.


##


**The Prompt Bank: Where Migrations Actually Live**


One of the most important architectural decisions we made was treating prompts as first-class, version-controlled artifacts.


The


**Prompt Bank** centralizes not just the prompt itself, but the full migration logic and any supporting scrip


ts. The workflow is deliberately iterative:


But the most interesting part is what happens after PRs are merged.


We track whether service owners make manual changes on top of what the agent generates. When they do, we feed those changes back into the migration prompt. Over time, the prompt improves through deliberate iteration by service owners and by learning from the adjustments teams make in production.


**We're not just storing prompts. We're storing how migrations actually succeed.** This also means the target finder compounds in value. For any open migration, as new targets are identified, the same refined prompt automatically creates the fix.


##


**The PR Merge Problem**


Creating PRs is the easy part. Getting them merged is where migrations usually become harder. A PR lands in someone's queue. They don't know the context. They're in the middle of something else. It sits there.


Three things dramatically improved our merge rates:


-


**Clear ownership per change:** A PR that five teams need to review is a PR that gets ignored. Every target needs a single, identified owner. We store that mapping explicitly.


-


**Context inside the PR itself:** We include the migration motivation, the business impact, and exactly what the change does - right in the PR description. We're switching the reviewer's context mid-day, so we make that switch as painless as possible.


-


**Automerge when safe:** For changes where the risk assessment allows it, we automerge. Done carefully, with proper safety criteria, but where it applies, it removes the organizational bottleneck entirely.


Starting from a low baseline, we reached


**60% of migrations fully merged within two weeks** of PR creation - and that number continues to improve quarter over quarter.


##


**The Results**


Over the past several months of running Code Migration Platform across different use cases, domains, and stacks:


-


**650 developer-days saved** in a single quarter (calculated by multiplying services affected per migration by estimated manual time per service, then comparing against actual time spent).


-


**88% of PRs merged without manual changes.** The code agent got it right, and the service owner approved it as-is. This is a direct reflection of how much investment migration owners put into their prompts upfront - that work pays dividends across hundreds of repos.


##


**Five Key Takeaways**


Here are five takeaways from our experience:


1.


**Optimize the pain itself, not around it.** We could have built better tracking tools, better notifications, or nicer dashboards for the existing process. Instead, we stepped back and asked whether the process itself was the problem. It was. Fix the root workflow, not the symptoms.


2.


**AI isn't always the answer.** We use scripts for a significant portion of migrations. Deterministic changes don't need an LLM, and using one adds complexity without adding value. Know when to use each tool.


3.


**Familiarity with the pain matters.** The solution wasn't obvious until we'd spent years experiencing the problem up close. That intimacy tells you where the real friction is, not where it appears to be.


4.


**Move fast with the people closest to the problem.** We built Code Migration Platform with minimal internal meetings and maximum collaboration with actual migration owners and stakeholders. Real feedback loops beat planning in isolation.


5.


**If you can solve it for yourself, scale it for everyone.** The prompt bank, the target finder, the tracking - all of it started as a solution to our own pain. Once it worked for us, extending it to the rest of the organization was the natural next step.


##


**So, what's Next?**


Code Migration Platform is in production and continuing to evolve:


-


**Conversational migration (via MCP):** You can now run Code Migration Platform locally and handle the entire flow in a single conversation with the agent. From target discovery through prompt creation through PR generation, just explain what you need.


-


**Automatic detection for new cases:** The target finder runs periodically across all open migrations, finds newly matching repos, and automatically creates the fix. Migrations keep progressing without anyone triggering them.


-


**Background execution:** Migrations run continuously without waiting for manual triggers.


-


**Automatic build failure fixes:** Our current focus is mapping the common reasons PRs come back red, and addressing them systematically. The goal is green PRs by default.


The hardest part of building it wasn't the code. It was understanding the pain was real enough to justify a real solution - and believing a solution could exist before we'd seen one.


If you're managing a cross-team process that feels structurally broken, that feeling is data. Step back, look at the pain from a wider angle, and stop optimizing around the friction.


You can also watch Dana's full


**Wix Engineering Conference 2026** - Astra Migrations: Making Infrastructure Changes Smooth and Seamless:


This post was written by


**Dana Avital**


**More of Wix Engineering's updates and insights:**


-


**Follow us on:**[Twitter](https://twitter.com/WixEng) **|**[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/) **** **|** ****[Instagram](https://www.instagram.com/wix_eng/) **** **|** ****[Facebook](https://www.facebook.com/WixEngineering/)


-


**Join our**[Telegram channel](https://t.me/wixeng) ****


-


**Visit us on**[GitHub](https://github.com/wix) ****


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/) ****


-


**Subscribe to our**[YouTube channel](https://www.youtube.com/WixTechTalks) ****


-


[Follow our Medium publication](https://medium.com/wix-engineering) ****
