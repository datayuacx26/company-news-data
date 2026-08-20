---
schema_version: "1.0.0"
document_id: "36543bd28be9ddc5257a3e2a204a5539fd698e27a3120dbc462ebe6bb01d7377"
company_key: "yc-datafold"
company: "Datafold"
source_id: "yc-datafold-news-import-e233b68116ce"
canonical_url: "https://www.datafold.com/blog/80-percent-done-migration-trap/"
published_at: "2025-04-08T00:00:00+00:00"
first_seen_at: "2026-07-24T03:54:32.907441+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:b6639e0bc1478f7c3f9fa2a91b9dfce33032e9035fd7fa3efb6994b38ff4a54d"
---

# Data migrations: 80% done, 80% left

You know the moment in a data migration when someone says, “We’re basically done, just a few models left to validate.” But then two weeks go by, or maybe two months, and you’re decoding institutional memory,[undocumented edge cases,](https://www.datafold.com/blog/fixing-migrations-with-ai) and ghost logic from a system no one fully understands.


The last 20% of a data migration isn’t spread evenly across the project. You’re 80% done, but somehow still have 80% left. Timelines stretch and stakeholders start losing confidence. The migration goes from “almost done” to “somehow still not done.”


We’ve seen this play out[across dozens of migration stories.](https://www.datafold.com/data-migration-guide/what-data-practitioners-wish-they-knew/) There’s two reasons why this happens: the **gnarly model problem,** and the **validation problem. **


## Where timelines break: The bottleneck model(s)


The first phase of a migration is intensive but largely defined. You’re translating SQL, porting stored procedures, migrating schemas, and[reworking transformation logic](https://www.datafold.com/blog/three-practices-to-migrate-to-dbt-faster/) to fit a new platform.


Sure, it’s time-consuming and a little boring but the logic is mostly out in the open. You might be rewriting hundreds of models or stored procedures, but you can scope it, assign it, and Google it. You already know that most of the trouble will come from known quirks like null handling, casing mismatches, and collation differences, but these issues are solvable with the right experience and a strong bag of tricks.[There’s a clear path](https://www.datafold.com/blog/data-warehouse-migration-strategy) that you (and your SQL translator) can reason through.


Then you hit that model, the one that’s a deeply layered operational report with 15 intermediate steps and a half-dozen undocumented filters. Suddenly, you’re stuck.


The model that looked like a week’s work is now a month of archaeology. It fans out into several new models; each one touches different source systems or teams. And the behavior of the legacy system isn’t just complex but it’s wrong in ways people have come to depend on.


At this point, the migration isn’t about SQL anymore but about[reverse-engineering institutional memory.](https://www.datafold.com/blog/migrating-data-is-easy-migrating-code-is-hard)


## Where confidence breaks: The validation problem


Let’s say you finally rewrite the gnarly model. That’s a win, right?


Not quite. Now you have to prove that the new system behaves the same as the old one. And that’s where migrations hit the second wall:[validation](https://www.datafold.com/blog/what-is-data-validation/) .


The legacy system had quirks: implicit sorting, undocumented filters, timezone assumptions baked into Excel models. You fix one thing, and another goes out of alignment. You rerun, retest, re-compare, write spot checks, stare at sample rows. But it never seems to satisfy the stakeholders, who ask: “Are we sure this matches production?”


This is the second reason migrations stall: you can’t ship what you can’t prove. And proving it, without the right tooling, turns into a slow, manual loop. Worse, it’s a loop with no clear definition of done. Even when the numbers match, confidence may not. If stakeholders don’t trust the outputs, the legacy system never gets turned off.


## Why this last stretch is so expensive


These two problems amplify each other. The gnarly models take forever to translate and rebuild – and also become the hardest to validate.


This is when “almost done” turns into another quarter on the roadmap. The team isn’t in execution mode, but stuck trying to show that things “work the same way” even when there’s no single source of truth that proves it. You’re burning hours trying to reconstruct logic that was never fully written down in the first place.


Throwing more hours (and people) at the problem rarely helps. You can add more engineers, write more ad hoc tests, and re-run models for the tenth time. But without a structured way to validate data parity, you’re just hoping to stumble into confidence.


What helps is a system that:


- Confirms that new outputs match legacy behavior
- Traces mismatches to their root causes
- Clearly communicates what changed, what didn’t, and why it’s safe to move forward That’s not something you get from more effort. That’s something you get from **applying leverage.**


## Leverage turns “almost done” into done


What I mean by **leverage** is collapsing the cost of iteration. The loop (translate, test, validate, fix, repeat) has to be tight, structured, and fast. That’s how you shrink the hardest part of a migration down to something manageable.


Leverage means designing your tools and workflow to work **with** the problem, not around it. It means building context into your comparisons, aligning inputs before[you diff outputs,](https://www.datafold.com/blog/what-the-heck-is-data-diffing/#:~:text=In%20the%20simplest%20of%20terms,tables%20in%20your%20data%20warehouse.) and generating proof that scales across hundreds, or thousands, of models.


### Getting help from an expert, or something like one


[The Datafold Migration Agent (DMA)](https://www.datafold.com/blog/what-is-the-datafold-migration-agent/) was built to support end-to-end migrations, and it’s the final 20%, where ambiguity, edgecases, and missing documentation stall progress where DMA becomes critical.


It starts by aligning source and target inputs so you’re diffing clean data. Then, it translates transformation logic, whether that is legacy SQL, nested GUI-based pipelines, or tool-specific abstractions, into modern testable code. In many cases, that means unpacking layers of logic before you even SQL.


[DMA can handle it.](https://www.datafold.com/blog/data-migrations-reimagined-introducing-the-ai-powered-datafold-migration-agent) Once translated, DMA runs row- and column-level diffs between systems to catch mismatches that would have been missed without custom tests for it (and you won’t have complete testing coverage since[there’ll always be unknown unknowns](https://www.datafold.com/blog/catching-unintended-changes-to-immutable-data) ).


If outputs don’t match, DMA refines the translation, reruns the diff, and loops until you’ve reached validated parity. When it’s done, you have a complete, validated record of what changed, what didn’t, and why it’s safe to move forward.


## See the feedback loop in action


That’s what real leverage looks like: the ability to prove parity with speed, confidence, and context. You get a tight feedback loop that builds trust[without burning engineering hours on every pass.](https://www.datafold.com/blog/how-data-migrations-waste-engineering-talent)


Want to see how this kind of loop works in real migrations?[Book a demo with our team.](https://www.datafold.com/demo/) We’ll walk you through how DMA handles code translation, validation, and diffing—so “almost done” finally means done.
