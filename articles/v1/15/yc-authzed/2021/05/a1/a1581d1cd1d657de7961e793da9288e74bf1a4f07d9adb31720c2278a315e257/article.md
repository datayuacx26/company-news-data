---
schema_version: "1.0.0"
document_id: "a1581d1cd1d657de7961e793da9288e74bf1a4f07d9adb31720c2278a315e257"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/six-month-profile-page"
published_at: "2021-05-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:bf44f764ec7ff85b615978b19f72c43730e6a1b696304a44a55aee35915b7b28"
---

# The Challenge of ACL Filtering in Relational Databases

## Setting the scene


It's a crisp Monday morning, err -- 11AM -- close enough. You take a seat at your desk and sip your coffee to avoid the chill you get from loading JIRA to find today's task. Like some kind of haiku straight from hell, the ticket reads:


> As a user that's a part of many organizations, I'd like to see all the resources that I can access on my profile page.


Sure thing! Nothing a bit of SQL can't handle:` SELECT FROM resources WHERE resource.created_by = $1` . Oh wait, that'll only get the resources that a user has created. It makes sense to list resources directly created by the user, but the ticket clearly says *that I can access* . You'll have to consider organization admins and any teams that have been explicitly or implicitly granted access to each resource. The metadata directly in the` resources` table isn't going to cut it.


You take a deep breath. This means you've got to touch the permission system. Whenever that happens, your change sits in limbo for a month while it gets "security audited". There's no way around it, though. How else will you know if a user can access something?


Turns out the permissions system is storing its data in SQL like everything else. Ok, You'll just do a` JOIN` on the` permissions` table. Hmmm... So, apparently, it's not just one table... Oh God, there're like 5 tables. That's probably fine; afterall, the latency budget for the whole page is a pretty lenient 300ms. Ok, let's see what the[query planner](https://en.wikipedia.org/wiki/Query_plan) thinks about all the` JOIN` s with a table as large as` resources` . Oh, *multiple seconds* . Alright, that's not going to work.


You spend the rest of the day talking to the code-owners of the permission system. They've got a plan to[denormalize](https://en.wikipedia.org/wiki/Denormalization) their schema so that you only have to` JOIN` on one table. But it's a major rewrite. And it's security critical. There's no way you're going to get this change in anytime soon. "I'll see you in six months" says the security team. You look at the clock to find that it's already 7PM. You take a seat at your desk and sip your microbrew as you drag the ticket to the "[Ice Box](https://www.reddit.com/r/agile/comments/41204f/what_is_an_icebox/) ", wishing it were a real ice box holding more beer.


## How could this happen to me?


Because software development is a perpetual battle for the balance between delivering value in a timely manner, the robustness of the solution, and the long term impact of maintaining the functionality, it's natural that people start with a straightforward solution until they're forced to reinvest. Permission systems are not typically fun to work on; they're difficult to get right and, because they are a critical part of security, often iterate at a much slower, methodical pace. Structuring your permission system as tables in a relational database is a pretty reasonable trade-off. It doesn't introduce a new dependency and reuses all of the abstractions your application has for storing data in the database. But, clearly, it's not without its flaws or complexity. The more *graph-like* your permissions requirements are, like having inherited roles or recursive relationships, the less performant a relational database will be. On the mature systems that I've personally worked on using relational databases, there was always a large portion of database CPU usage spent computing permissions. There was no trick to optimizing these queries further because it wasn't worth the tradeoff of building a vastly more complex system in our application code that managed a denormalized permissions schema.


## So, what's the solution?


Before we talk about solutions, we should clearly define the problem. The general name for the problem being discussed is *[ACL filtering](https://docs.authzed.com/authz/acl-filtering)* . *ACL* , or "Access Control List" and often pronounced "ackle", in its colloquial usage, is synonymous with the word "permission". *ACL filtering* is simply filtering a list of objects by whether or not a particular user has access to the items in the list. Because fetching data can be costly, there are two ways to go about filtering: *prefiltering* and *postfiltering* .


*Prefiltering* is determining the set of objects that you can access before you fetch them, while *postfiltering* is throwing away objects that you've fetched based on whether or not you can access them. There are scenarios that make sense to use either type of filtering or even both in conjunction with each other. Performing` JOIN` s to filter results from a relational database is kind of like the quantum mechanics of filtering, because it can be either type. You are left entirely at the whims of the the database's query planner to determine when filtering occurs and whether or not it'll be performant.


## Where do we go from here?


It's tough to say because the requirements for permissions systems vary from application to application. It also depends on what trade-offs you've made when deciding to use a relational database. Is it most important that all data live in the same place? Then, you'll likely want to stick with it and continue to rewrite security-critical code into a more complex design in order to get a performant layout in your database. Is it more important that you be able to represent complex relationships and be able make changes to your system more swiftly? Then it might be time to try a service like Authzed.


In a future, we'll be covering exactly how Authzed facilitates efficient ACL filtering. Stay tuned!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Setting the scene
- How could this happen to me?
- So, what's the solution?
- Where do we go from here?
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
