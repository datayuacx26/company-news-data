---
schema_version: "1.0.0"
document_id: "355ca4f0b8cdc42da5fcc7d92eaeded09f88ba5efde85aa83285171ac69b3870"
company_key: "yelp-inc-common-stock"
company: "Yelp Inc."
source_id: "yelp-inc-common-stock-rss-c1f3492370c5"
canonical_url: "https://engineeringblog.yelp.com/2026/08/migrating-a-large-flow-monorepo-to-typescript.html"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T18:37:47.077113+00:00"
fetched_at: "2026-08-04T18:43:13.169222+00:00"
content_hash: "sha256:b55b7f048c895e26f0f373fbe9bd0eea356bab7fc6e3e45a3c2ff742bd638ba4"
---

# Migrating a Large Flow Monorepo to TypeScript

# Migrating a Large Flow Monorepo to TypeScript


- Shawn Walton, Software Engineer


- Aug 4, 2026


In early 2017, Webcore selected Flow as Yelp’s next-generation typechecker over TypeScript. At the time there was no clear frontrunner. Flow had better support for React, better performance, and a respectable repository of types for NPM packages in` flow-typed` . However, a few years later the landscape had shifted, and it became clear that TypeScript was emerging as the stronger choice. TypeScript’s DefinitelyTyped repo had thousands more packages than` flow-typed` , and many more packages shipped with built-in TypeScript headers. TypeScript also proved to be more stable over time, and limited uptake in the wider JS community made finding tools and documentation for Flow comparatively difficult. With the Flow team’s announcement that it was[refocusing on Facebook’s internal needs](https://medium.com/flow-type/clarity-on-flows-direction-and-open-source-engagement-e721a4eb4d8b) rather than the OSS community’s in mid-2021, it was clear that TypeScript was a better fit. We began scoping our migration in early 2022.


## Goals


Our North Star was maintaining and growing trust in our typechecking. That meant avoiding “temporarily” disabling typechecking and bulk-inserting “typecheck ignore” comments, as well as maintaining type integrity throughout the entire process. Broken trust is hard to rebuild, and we’ve found that code that loses coverage from its type checker, tests, or linter almost never regains it.


This immediately ruled out most approaches. A single massive rewrite would have been ideal, but even the best codemods in the world aren’t good enough to convert all million+ lines of code with few errors over the holiday break. A code freeze in our main web repo while we did it manually was also out of the question.


But if we couldn’t migrate all at once, that meant we’d have to do it gradually. And if we had to do it gradually, that meant we’d have to deal with having both Flow and TypeScript code in the same repo, at the same time, in production, without losing type safety. This made us nervous, but after lots of research and prototyping we decided we could make it work.


## Implementation


Our plan was to convert our whole monorepo to TypeScript one package at a time. We would use an open-source package called[flowts](https://github.com/zxbodya/flowts) to do the bulk of the Flow to TypeScript conversion work, augmented with a few codemods of our own to handle some common edge cases we found. Unfortunately there were many errors that could not be automatically fixed with codemods, and manual effort was almost always required to complete the conversion. Most of these stemmed from minor differences in how Flow and TypeScript handled types and were typically unique to each package or team. Errors that cropped up frequently, but not in a consistent enough manner to be codemodded, were documented with recommended solutions. Completing the conversion would still require a lot of manual effort, but the tooling still saved tens of thousands of mind-numbing engineer hours.


Once a package was successfully converted to TypeScript, we’d use another excellent open-source tool called[flowgen](https://github.com/joarwilk/flowgen) to convert the` .d.ts` type declaration that the TS typechecker generated back into Flow-compatible type headers. This also took a bit of finessing to get working reliably, but we eventually got our toolchain to a place where converting a package to TypeScript was fast, easy, and seamlessly typechecked with all the Flow code that imported it. Unfortunately, there is no equivalent tool to generate` .d.ts` headers from Flow code, so we would have to start at the deepest Yelp-internal dependencies first and repeat up the dependency tree. This ended up being a thorn in our side for the entire migration, but the alternative was compromising type integrity between packages which we had already resolved to avoid.


This migration approach was enabled by our[then-recent migration of all our JavaScript code into a Lerna monorepo](https://engineeringblog.yelp.com/2023/03/gondola-an-internal-paas-architecture-for-frontend-app-deployment.html) . Having all our JavaScript code in a single monorepo allowed us to update our tools to support TypeScript across the entire ecosystem with a single PR, rather than the hundreds that it would have taken with each package in its own repo. It also allowed us to convert one package at a time and only worry about handling Flow/TS interop at the package level rather than at per-file granularity. Each package is usually under a few thousand lines of code total, so most migrations could be completed within a day or two; our hope was this would allow teams to convert a package or two during lulls in on-point duties or other gaps in their work schedules.


The setup ended up looking something like this:


###


A diagram showing how we used Babel, tsc, Webpack, and Flowgen to get Flow and TypeScript types working during the migration period.


Getting those two conversion tools to a working state in addition to doing all the rote Webpack and Babel work to make everything actually work end-to-end took a few months. We could have launched right there in mid-2022, but we knew we only had one chance at a first impression. If the tools were unreliable, slow, or difficult to use, our engineers would have a negative impression of our new typechecker and toolchain right out of the gate. Even if we fixed everything later, that sentiment would stick around for a long time and make it difficult to persuade busy feature teams to take time out of their schedules to do a migration they weren’t even interested in. We really wanted the migration to sell itself.


## Preparing for prime time


We started an extensive dogfooding period. We at Webcore, along with our colleagues at Design Systems, converted around 90 foundational packages to TypeScript. This took several months, but we found several problems in our toolchain that we were able to fix before releasing to Yelp engineers at large. Plus, having a big chunk of our styleguide packages already in TypeScript meant that once we launched to feature teams, they could get started on converting their packages right away without having to wait for their dependencies to be ported over.


While helping teams get a jump start on migrating their code was helpful for our strategy, the real goal was polishing the tools and documentation to a mirror finish. An hour spent writing documentation or automating some small facet of the process[would save hundreds of hours of developer time](https://xkcd.com/1205/) during the next phase of the migration. By the time we had converted an internal test page (hosted on[Gondola](https://engineeringblog.yelp.com/2023/03/gondola-an-internal-paas-architecture-for-frontend-app-deployment.html) ) along with all the Yelp-maintained packages in its dependency tree to TypeScript, the process was as easy as it possibly could have been. Every step that could be automated was, and every tool and error message we ran into was extensively documented. We also wrote an extensive metatest suite to ensure that every conversion was done correctly and wouldn’t cause issues down the line.


After over a year of ideation, toolchain development, dogfooding, and documentation, we launched TypeScript to feature teams in May 2023. Alongside the internal announcement were several presentations and Q&A sessions for each region we had engineers in, plus a live demo of a package conversion. We also leveraged[EE Metrics](https://engineeringblog.yelp.com/2024/03/keeping-track-of-engineering-wide-goals-and-migrations.html) , our internal code quality monitoring service, to help teams understand exactly which of their packages could be converted and to help Webcore keep tabs on the overall progress. When a package was unblocked, it automatically opened a ticket on the owning team’s Jira project, and automatically closed it when the migration was complete.


## The migration begins


Right out of the gate, the TypeScript development experience was at least as polished as the Flow experience was after five years of iteration and improvement. We had done everything we could to build confidence in both our new type system and the temporary bridge we had built from the old one. Throughout the entire migration, we only broke typechecking once for a few days early on in the project. The number of type errors that had appeared in such a short time after fixing it was alarming, and only confirmed that our decision to preserve type safety at all points in the migration was the right one.


We had no idea how quickly the migration would proceed once we opened the floodgates. That and the incredibly complicated dependency graph of the monorepo precluded us from being able to plan out a prescriptive schedule.


The initial uptake was fairly quick – we went from around a hundred converted packages on launch day to over 270 six months later. But with a total of 570 packages and counting in the monorepo, we still had a long way to go. The feedback we received from the early adopting teams was strongly positive, both for the conversion process and the resulting TypeScript development experience. Unfortunately, engineers have features to build and deadlines to meet and so weren’t always able to prioritize the migration.


###


The migration proceeded slowly, with some spikes here and there for Yelp's Hackathons and teams spending a sprint on converting many of their packages at once. A few of the packages stayed “unmigrated” because they weren’t typed at all; these are mostly configurations for tools.


While the monorepo gradually migrated to TypeScript, our tooling didn’t stand still. We adopted[Stripe’s](https://stripe.com/blog/migrating-to-typescript) excellent[flow-to-typescript-codemod](https://github.com/stripe-archive/flow-to-typescript-codemod) as soon as it was released on GitHub and found it to be a significant improvement over` flowts` , reducing the amount of manual work required after running the automated conversion script. The` .d.ts` to Flow header pipeline likewise saw continuous iteration to handle edge cases that didn’t crop up in our initial conversion phase.


Not all of our ideas ended up panning out! We tried to run the automatic conversion toolchain on each package as soon as it was unblocked via a cronjob, but since it was unable to reliably convert anything but the simplest packages on its own, we scrapped it. We also hacked together a script that would use an LLM to fix errors arising from the automated conversion, but we found the LLMs of early 2023 just weren’t up to the task. More mature LLMs released towards the end of our migration performed better than our prior experiments in 2023, and teams were free to use them as they saw fit, but they were still unable to reliably convert entire packages without human intervention so we didn’t build them into our core toolchain.


As the initial enthusiasm for TypeScript waned and the teams that were eager to convert their packages had already done so, the number of TypeScript packages leveled off. At this point the dependency graph was still too complicated to untangle into a coherent plan, so we instead built tools to extract actionable data from it where we could. One of these traversed the graph starting at each unblocked package and incremented a score indicating the “importance” of converting each package. Generally, packages whose conversion would make the most progress towards unblocking other packages were assigned a higher score than packages that few other packages depended on. Keeping tabs on these scores allowed Webcore to approach individual teams and ask them to prioritize high-scoring packages and keep the overall conversion moving.


###


A small section of our dependency tree showing dependency relationships, the names of the owning teams, and the length of the longest Flow dependency chains for each package.


Another useful tool was one that generated a Graphviz visualization of the remaining Flow dependency graph. As the tree gradually dwindled to a manageable size, we were able to understand it and use it to finally come up with a plan to finish this migration.


## Finishing up


As we hit a critical mass of TypeScript packages in late 2024, we saw an opportunity to speed the last 200 packages or so along by coordinating teams’ conversions of packages that were bottlenecking the process. We used the graph visualization to release quarterly lists of packages that needed to be converted through our planned Flow end-of-life at the end of Q3 2025. The conversion rate chart shown earlier reflects a corresponding uptick around the end of Q1 2025. Although we had to push the EoL deadline back a quarter to the last day of 2025, it was still a crucial step to finishing the migration in a timely manner.


Near the end of 2025, it was clear that Webcore would need to manually convert a few dozen packages to finish off the conversion because the owning teams did not have bandwidth to do the conversions themselves. We auto-suppressed the errors remaining after running the conversion tool on these packages. We wanted to avoid this entirely, but we decided that finishing the migration within a reasonable timeframe was worth the suppressions.


We migrated the final Flow package in early February 2026, bringing the era of outdated typecheckers at Yelp to an end. All told, we migrated 1.4 million source lines of code (SLoC) to TypeScript over the course of three years and seven months, including the migrations performed before general availability. The final code only had around 6,000` @ts-expect-error` directives, around 2,000 of which were mass-added for the last handful of packages that were converted with auto-suppression. The overall type coverage is 96.44%, significantly higher than the 83.15% we had with Flow.


## Results


82% of surveyed engineers reported some amount of increased productivity in TypeScript over Flow, with 45% of them reporting that the increase was “significant”. Ditching Flow also allowed us to adopt OSS tools that didn’t support Flow such as` swc` , and take advantage of TS-only tools such as` @typescript-eslint` . We also found that LLMs are better at handling TypeScript code than Flow due to a much larger training set available online.


## Conclusion


The TypeScript migration is the largest JavaScript migration Yelp has ever done, and we learned several lessons along the way. Engineers identified the huge amount of outreach and documentation as a strong point of the migration, and frequent emails along the way helped keep the community up to date on the latest status. Once the migration was at the point that we could set incremental deadlines for teams to meet, things moved a lot faster than relying on teams to do things in their own time. And finally, we learned a lot about exactly when and where to spend our time to accelerate the migration. Doing the entire migration ourselves wasn’t tenable, but stepping in to migrate key packages and kick-start migrations when things stalled allowed us to work as force multipliers in getting things moving.


## Acknowledgements


Between proposal reviews, building infra, migrating packages, and fixing bugs, every single member of Webcore over a four-year period had a hand in making this happen. Special thanks go to Jack Guy for the initial TypeScript prototypes back in 2019 and first draft of the proposal doc that proved this migration was even possible.


And of course, we’d never have gotten anywhere without all the engineers who put in the hours to actually migrate this huge codebase. Thank you!


[Tweet](https://twitter.com/share)


### Become an Engineer at Yelp


Want to help us build out exciting new web features?


[View Job](https://www.yelp.careers/us/en/c/engineering-jobs?lever-source=engineering_blog)


[Back to blog](https://engineeringblog.yelp.com/)
