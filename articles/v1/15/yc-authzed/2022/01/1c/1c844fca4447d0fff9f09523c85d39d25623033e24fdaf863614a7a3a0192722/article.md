---
schema_version: "1.0.0"
document_id: "1c844fca4447d0fff9f09523c85d39d25623033e24fdaf863614a7a3a0192722"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/december-2021"
published_at: "2022-01-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:26:34.525443+00:00"
content_hash: "sha256:94ec57992a3ea61846067afdf441e2332b3e84fa4d101986a1188b75eaf20c33"
---

# Product Update: December

## Tis the season


With the holidays wrapping up, we're excited to welcome the new year. While some people have resolutions each year, others have predictions. This year, Jake's already made his prediction and it's something that really resonates with rest of the team:


> "There will be more high profile security incidents (similar to the recent[npm registry vulnerability](https://github.blog/2021-11-15-githubs-commitment-to-npm-ecosystem-security/#security-issues-related-to-the-npm-registry) at GitHub) within the open-source and developer tooling ecosystem directly caused by inconsistently implemented, ad-hoc authorization rules. In preparation for this, security teams will invest more heavily in centralized authorization and permission server databases to help prevent such attacks and others that can arise from permissions inconsistency."


Because the community paid down tech debt last month, this month we had some fun introducing new functionality.[SpiceDB 1.3.0](https://github.com/authzed/spicedb/releases/tag/v1.3.0) was released with two substantial changes:[internally versioned namespaces](https://github.com/authzed/spicedb/pull/332) and[object wildcards](https://authzed.com/blog/unveiling-wildcard-permissions-in-spicedb/) . In addition to open source goodness, we've also been spending our time simplifying our deployment strategy and building out functionality for our enterprise users.


ICYMI: in addition to posting[weekly blog posts](https://authzed.com/blog) , we're now officially posting monthly wrap-ups of events and new functionality we've added to Authzed. We're also giving everyone a little taste of our Slack's random channel and all of it's off-topic goodness.


## December Updates


- [SpiceDB 1.3.0](https://github.com/authzed/spicedb/releases/tag/v1.3.0) was released and deployed to Authzed.com! Lots of new functionality!
- Joey wrote about using the new wildcard functionality to[implement public resources](https://authzed.com/blog/unveiling-wildcard-permissions-in-spicedb/) .
- How granular should your permissions be?[Jake has it covered](https://authzed.com/blog/how-small-is-too-small/) in his blog post.
- Now that the schema language is 5 months old, Sam detailed some[common patterns that've emerged](https://authzed.com/blog/schema-language-patterns/) .
- Got a huge schema? No problem --[the Playground](https://play.authzed.com/) got some performance boosts with large schemas in mind.
- You want privacy? You got it![The Playground](https://play.authzed.com/) also no longer uploads a copy when downloading schemas.
- [AuthZed Serverless](https://app.authzed.com/) now has a latency graph in the metrics tab for LookupResources.
- SpiceDB Enterprise now supports[multi-tenancy](https://docs.authzed.com/reference/glossary#notable-differences) , just like Authzed.com
- Ingress reliability improved for enterprise deployments by migrating to[Contour](https://projectcontour.io/)


## Entropy from #random


- It's a little late now, Jake wrote a few lines of Python to[automate Secret Santa](https://gist.github.com/jakedt/0cecf9279b7b99231e0b2cbe6b26e2fc)
- Audiophiles. They finally did it, they[found something](https://twitter.com/jonny290/status/1466455503572279296) more snake-oil than[cable risers](https://audiophilestyle.com/forums/topic/453-cable-risers-snake-oil-or-they-do-improve-the-sq/) .
- The kind folks at[Work-Bench](https://www.work-bench.com/) and[Amplify](https://amplifypartners.com/) left the team some[holiday goodies](https://authzed.com/static/blogs/images/december-2021/holidaygoodies.jpg) .
- Can't wait for web standards? Me neither! Add this project to your site to allow[deeplinks anywhere](https://github.com/WesleyAC/deeplinks) !
- I've been using Slack for 8 years and had no idea you could could[cmd+click DMs](https://twitter.com/rothgar/status/1468721811567251457) .
- Join the waitlist... for GitHub search that[actually works](https://github.blog/2021-12-08-improving-github-code-search/) .
- Foodies brace yourself for a[hilarious review](https://www.today.com/food/brutal-review-michelin-starred-restaurant-bros-goes-viral-t242696) of a Michelin Star resturant.
- Our friends over at Buf[awarded Kenton](https://twitter.com/KentonVarda/status/1470529536408395776) for his valiant efforts battling incorrect people on the Internet.
- The FOMO for working at GitLab grows as they[just acquired](https://opstrace.com/blog/gitlab) our friends at OpsTrace!
- The longer you stare at these stickers the more your mind feels broken:


On this page


- Tis the season
- December Updates
- Entropy from #random


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)
