---
schema_version: "1.0.0"
document_id: "a0578601795fa00c7041e28d93ac751c7d8062819d1a851e84e5f88ded732fd0"
company_key: "yc-whatnot"
company: "Whatnot"
source_id: "yc-whatnot-rss-30861744a6f8"
canonical_url: "https://medium.com/whatnot-engineering/eliminating-graphql-schema-bloat-with-ai-so-you-dont-have-to-5f6ae84d0ee1"
published_at: "2025-12-03T18:29:57+00:00"
first_seen_at: "2026-07-24T07:07:55.027426+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:801c6a7675e69af13dbe83699ec1c3271d12a1e71885ff781be6da15300a9988"
---

# Eliminating GraphQL Schema Bloat with AI (So You Don’t Have To)

GraphQL


AI Agent


LLM


Agentic Workflow


Developer Tools


# Eliminating GraphQL Schema Bloat with AI (So You Don’t Have To)


[Whatnot Engineering](https://medium.com/@whatnotengineering?source=post_page---byline--5f6ae84d0ee1---------------------------------------)


5 min read


·


Nov 18, 2025


--


[Nikhil Patel](https://www.linkedin.com/in/patel-nikhil/) | Seller Growth Engineering


Every engineer has faced this: you build something new, ship it, and move on. Somewhere in your codebase, a few deprecated patterns or retired endpoints keep living on. Over time, that cruft piles up. You tell yourself you’ll clean it up someday, but “someday” never comes.


At Whatnot, we realized our GraphQL schema had quietly become a graveyard of unused fields left behind from old features. Not only has this made our schema more complex and hard to navigate, but it’s also introduced risks. We even found ourselves accidentally reusing old, unoptimized endpoints in production client code. We’d always wanted to clean it up, but making breaking changes to remove fields from a GraphQL schema is tricky, time-consuming, and risks breaking client experiences if done incorrectly.


As we started adopting LLMs more deeply into our day-to-day, we saw an opportunity: what if AI could automate and de-risk this cleanup process for us?


## Why It’s Hard to Know What’s Safe to Delete


At first glance, identifying unused GraphQL fields sounds simple: just scan the codebase, find what’s unreferenced, and delete it. When we looked for prior art surrounding this problem, this was the approach the majority of solutions took. But in a complex system like Whatnot’s, “unused” is deceptively hard to define.


We have multiple clients: iOS, Android, web, and internal admin tools, all of which query our GQL API. The tricky part is that older versions of our mobile apps may still call queries that have long been removed from the main branch, and admin tooling dashboards often execute queries entirely outside our primary repositories. That means static analysis can’t tell the whole story.


To get a truly reliable picture, we turned to real traffic data. Every GraphQL request that hits our backend is logged, including the full query shape. We decided to define a field as “unused” if it hadn’t been requested in the last 30 days, a reasonable window to prevent overdeletion. But even then, analyzing GraphQL usage isn’t as simple as tallying API endpoint calls like you would with REST.


## Navigating GraphQL Complexities


Unlike REST, where each endpoint stands alone, GraphQL requests can hit multiple fields across multiple types. In GraphQL, the two fundamental entities are fields and types. Types can either be primitives (ints, strings, booleans) or have multiple subfields. We could request a user — get the user’s first livestream, and the usernames of all of the users who bought from that livestream — all in one query. Fields are often shared across types: a` User` field might appear in dozens of different queries, and removing it from one place could unintentionally break another.


Press enter or click to view image in full size


This query crosses several types and reuses shared fields, illustrating why GraphQL cleanup isn’t straightforward


To measure true field usage, we had to do what GraphQL itself does: walk through this graph of types and fields. We built a pipeline that parsed 30 days of queries, deduplicated traffic by unique query hashes, and traversed the schema’s Abstract Syntax Tree (AST) to record every field “visit.”


When the results came in, we were stunned. The data showed over **2,600 unused fields** , including nearly **200 root queries and mutations** .


Press enter or click to view image in full size


GraphQL field usages over the last 30 days


## Automating it all with AI


Once we had a reliable way to identify unused fields, the next question was: *how do we remove them safely?*


Deleting an unused GraphQL field is pretty trivial, albeit time-consuming, once you have the domain knowledge. Just identify an unused field, remove the field from the schema, remove the resolver definition and any now-dead code, and clean up tests. The process could take as little as 15 minutes and as long as a couple of hours for the more complex fields. However, educating every engineer and asking them to manually delete their unused fields wasn’t realistic.


So, instead of asking engineers to spend hours manually validating and removing fields, we built an AI subagent to do it for us. The agent was designed to follow the same steps an engineer would, only faster and completely running in the background.


We then wired this process into a GitHub Action that runs on a regular schedule. Each day, the action automatically triggers the cleanup agent, picks an unused field, and removes it one at a time. This turns schema maintenance into an ongoing background process, not a one-time cleanup project.


Press enter or click to view image in full size


Every PR clearly explains what the agent did, links to validation data, and provides a safety checklist for reviewers. Since each field in our schema already has a defined code owner, the agent automatically assigns the PR to the right team.


The result? A process that once took an engineer one to two hours per field now takes minutes to review. Most PRs require no edits at all — code owners can simply skim the diff, verify the context, and approve it while getting their morning coffee.


Press enter or click to view image in full size


So far, our subagent has safely removed **24 of ~200 unused root fields** . Each run costs roughly $1–3 in LLM credits. After working out some kinks in the initial runs, only three needed manual edits, and even then, it was simply merge conflicts due to the timing of the change.


## What’s Next


As our cleanup system continues to run, we’ve started to uncover a few lessons from the early runs. The subagent consistently puts up clean, high-quality PRs — but every automated system is only as good as the environment around it.


In a few cases, we found the agent proposing deletions for fields that were technically still referenced in client code, particularly on the web. This highlighted an important gap: while our mobile clients (iOS and Android) already have linters to catch unused GraphQL fragments, the web repo doesn’t yet have the same level of tooling. Automations and linters to enforce code cleanliness and remove dead code are essential to ensure agents like these can reach their full potential.


There are countless menial chores that we do as part of our day-to-day as software engineers, like cleaning up stale feature gates or deprecating old flags. We’re exploring new ways to automate these with AI, freeing engineers from the repetitive, trivial parts of development. As we continue finding new use cases, I’m excited to continue working towards a future where these systems handle the maintenance work in the background, allowing engineers to focus on what truly matters: designing novel features and building great products instead of dealing with cruft and tech debt.


*If tackling large-scale engineering challenges like this excites you, we’re hiring. Join us in building the future of social commerce at*[whatnot.com/careers](https://www.whatnot.com/careers) *.*
