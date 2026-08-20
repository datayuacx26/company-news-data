---
schema_version: "1.0.0"
document_id: "b26eec3503554e2849de2cb4d02fbb1aad41fb84ff8ebc7e391339f782cf2cb0"
company_key: "yc-zatanna"
company: "Zatanna"
source_id: "yc-zatanna-news-import-3a347cd3961c"
canonical_url: "https://www.zatanna.ai/articles/betting-against-computer-use"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-24T07:55:16.296366+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:44ed552728024f93d6e963235aea822d6bd9c2392838fa370dc020f4d5d4bbe5"
---

# Why We're Betting Against Computer Use

In 2026, the frontier labs have thrown hundreds of millions of dollars into computer use.


The promise is compelling: unlock automation in places where traditional RPA used to live. Computer use promises to automate everything from electronic health record (EHR) systems to enterprise software, and many AI companies have raised hundreds of millions of dollars on the back of this promise alone.


On its face, computer use sounds like a fantastic idea.


You programmatically call an LLM with access to a server, and the LLM uses a decision loop to click buttons, reads state from the screen, performs actions, and extracts the data you need. This automation is much more intelligent and eloquent than the RPA of the past.


But in practice, the solutions on the market we have yet to see real production pipelines hit scale. This is because CUAs break when layouts shift, miss popups, fail to reason about state changes in dynamic environments, and perhaps most importantly, are painfully slow.


Claude's computer use is so bad that I once started a task, forgot it was even running, and twenty minutes later watched it click Spotlight on my laptop to "find my browser."


Fundamentally, CUAs are the wrong primitive for most use cases.


## This isn't the first time this has happened


I spent most of college consulting for companies migrating from browser-based scraping and automation systems to request-based approaches.


Every single company said the same thing.


They initially chose browser automation because browsers:


- bundled JavaScript rendering
- handled anti-bot challenges without requiring reverse engineering
- felt more reliable than raw request-based scraping


But by the time they called me, they were getting destroyed by the cost and latency problems you deal with when running browsers in production.


Computer use is the exact same tradeoff.


Labs are pouring hundreds of millions into "high-quality labeled data," and the agents still cannot reliably handle basic tasks in dynamic environments.


The same thing happened with browser automation years ago. The demos simply do not match reality.


## So What's the Alternative?


For web applications, the answer is straightforward:


Reverse engineer the network requests through the browser inspect tab or a MITM proxy.


Desktop applications are different. Most of them connect directly to databases, meaning there are no HTTP requests to intercept.


When we first started Zatanna, we had several companies reach out with exactly this problem: on-prem desktop software with direct database access.


My background was primarily in reverse engineering web anti-bots and web applications, so initially we were skeptical that these systems could be fully reverse engineered.


Two things changed our minds:


- The underlying frameworks are trivial to decompile. Most of these applications run on PowerBuilder or Java, both of which decompile cleanly.
- Direct database access is actually better than API access. You can read data and aggregate trends that do not even appear in the UI.


## Why "Just Write to the Database" Is Terrifying


When we started working with companies dealing with on-prem SORs, we kept hearing the same warnings about direct-to-database automation breaking production databases, and causing thousands of dollars in damages.


That made us nervous.


At the same time, we believed LLMs were uniquely suited to make sense of the massive amount of context required to safely operate on these systems. So we started experimenting on development instances. We've written up our findings here.


### Problem #1: Finding the Database Credentials


For some systems, this was trivial.


The credentials were sitting inside .ini files under Program Files.


For others, we had to:


- decompile the application
- inspect the source
- dump memory from the running process


Giving an agent memory access is obviously precarious, so we built hooks that only allow read-only memory access.


### Problem #2: Enterprise Databases Suck


Enterprise databases are often arcane (login functions, procs, weird access layers, encrypted storage etc)


Nearly every operation routes through stored procedures wrapping layers of joins, triggers, and business logic.


Fortunately, Claude has significantly more contextual knowledge about these systems than we did. It handles login flows and database reasoning surprisingly well.


At this stage, we manually approved every SQL statement to ensure nothing modified the database unexpectedly. We also had Claude give relevant technical documentation on formats we didn't understand ourselves. In the end, they are all SQL databases, just with weird quirks from a pre-cloud era.


### Problem #3: I Wrote the SQL Myself and Nothing Happened


This one was mostly our fault.


For our first write operation on a development instance, I manually wrote the SQL to assign a truck to a load.


I executed the statement and…


Nothing happened.


The UI did not update. Refreshing did nothing. Restarting the application did nothing. I genuinely could not for the life of me figure out why the writes were not landing.


Fortunately, one nice property of direct-to-database systems is that reverting changes is easy.


The solution was to decompile the frontend and inspect the source directly.


Every write action in these desktop applications follows a specific execution pattern:


- SQL statements
- stored procedure calls
- TCP streams
- message queue events (we've seen Apache Artemis)
- sometimes all three simultaneously


Writing the SQL alone is just not sufficient. You have to reproduce the full behavioral pattern the application itself would have executed.


Now, for every operation, we transfer the frontend code and let Claude/Codex analyze it directly. The agent extracts:


- SQL behavior
- side-channel communication
- stored procedure patterns
- TCP or messaging requirements


Writes happen inside transaction blocks, and all test writes are rolled back automatically.


## Where We Landed


We ultimately built an agent that automates this entire reverse engineering process.


By default, it can:


- make read-only SQL queries
- inspect Windows systems
- analyze frontend code paths
- reconstruct application write behavior


Write access requires explicit approval and executes inside rollback-able transactions.


The result is effectively a programmatic API for any on-prem SOR:


- no screenshot loop
- no layout-detection agent
- no fragile browser automation
- no production-database horror stories


## Conclusion


Computer use will continue attracting investment because the demos are seductive. And that's not entirely a bad thing. There is a completely valid need for computer use when it comes to discovery and low-scale automations. We're actually looking at using some of the most popular CUA companies right now to sandbox some of our environments.


But the real enterprise problem is much less about if AI can click a button on the screen and way more about getting reliable programmatic access to UI-only software.


For most enterprise systems, there is a much better answer hiding in plain sight – the database/network layer. We think that this answer presents itself as a new way for agents to interact with software. This time not as a mimic of humans, but as a machine.
