---
schema_version: "1.0.0"
document_id: "09fff3f3653cd985abda806a3cf5be14713afb6a23f4f90dd62a30e2dcbef916"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/march-2022"
published_at: "2022-04-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:0e8ffc673d6d56c7982ea061bc7136b11ba04e86b701ad188c01416104208463"
---

# Product Update: March

## Spring has sprung!


The season is changing. There's the new growth where there was nothing before. Taking inspiration from mother nature, this month the team at Authzed built out a variety of new functionality.


Major highlights include:


- [Cloud Spanner](https://docs.authzed.com/spicedb/selecting-a-datastore#cloud-spanner-beta) datastore supporting performant, planet-scale SpiceDB clusters
- Thumper, our load monitoring tool, was rewritten to be fully scriptable for emulating different workloads
- The[HTTP API](https://app.swaggerhub.com/apis-docs/authzed/authzed/1.0) is now available for all permission systems on Authzed.com


ICYMI: in addition to posting[weekly blog posts](https://authzed.com/blog) , we're now officially posting monthly wrap-ups of events and new functionality we've added to Authzed. We're also giving everyone a little taste of our Slack's random channel and all of it's off-topic goodness.


## March Updates


### Open Source


- [SpiceDB v1.5.0](https://github.com/authzed/spicedb/releases/tag/v1.5.0) was released adding support for[Cloud Spanner](https://docs.authzed.com/spicedb/selecting-a-datastore#cloud-spanner-beta)
- [Zed v0.4.1](https://github.com/authzed/zed/releases/tag/v0.4.1) was released adding transparent prefix rewriting to` zed import`


### Authzed.com / Authzed Enterprise


- The[HTTP API](https://app.swaggerhub.com/apis-docs/authzed/authzed/1.0) is now being run on Authzed.com
- Multi-tenancy is now enforced with more granular permissions
- The dashboard now reloads correctly on email validation
- SpiceDB Enterprise migrations are now unified into one revision value
- Empty schemas are now valid input to the[v1.WriteSchema](https://buf.build/authzed/api/docs/main:authzed.api.v1#WriteSchema) API
- Thumper, the load monitoring tool, was rewritten to be fully scriptable for emulating different workloads
- The playground got many bug fixes focusing on YAML exporting and improved error reporting


### Blogs / Talks


- We highlighted[how to use our HTTP API](https://authzed.com/blog/authzed-http-api/) powered by[gRPC Gateway](https://github.com/grpc-ecosystem/grpc-gateway)
- Jimmy was featured to talk about the SpiceDB community on CommonRoom's[Perfect 10](https://twitter.com/CommonRoomHQ/status/1503854890153504770)
- Our HeavyBit interview is now available[on YouTube](https://youtu.be/gNUdmvuzQ5o)
- Continuing to detail how SpiceDB permission checks work, Joey covered[intersections and exclusions](https://authzed.com/blog/check-it-out-2/)


## Entropy from #random


- Simon Peyton Jones[left Microsoft Research](https://discourse.haskell.org/t/an-epic-future-for-spj/3573)
- We read about how[SPAs were a mistake](https://gomakethings.com/spas-were-a-mistake/) , but we'll keep relaxing in our's
- NYC is getting its first taste of[Raising Cane's](https://greaterlongisland.com/popular-raising-canes-chicken-is-opening-its-first-new-york-city-location/)
- We started playing with the glorified bookmarks known as[go links](https://www.trot.to/)
- Konami discovered the power of emulation and announced[TMNT remasters](https://www.youtube.com/watch?v=nlaZF5bxc50)
- None of us can ever remember cron syntax[even with help](https://crontab.guru/)
- The US Senate finally unanimously agreed on something:[DST policy](https://twitter.com/senatecloakroom/status/1503797632745025542) .[XKCD](https://xkcd.com/2594/) always has a relevant comic
- We reminisced after reading the[SoFuckingAgile's Indispensablibility](https://www.sofuckingagile.com/blog/on-being-indispensable) post
- [Spoken launched](https://www.spoken.io/) to the dismay of many popular furniture retailers
- Beer can now be brewed with[0 carbs](https://www.budlight.com/our-beers/next/) . We still have no idea how
- Okta[got breached](https://www.wsj.com/articles/okta-investigates-reports-of-a-digital-breach-11647931174) . Okta & Auth0 users should follow[their updates](https://www.okta.com/blog/2022/03/oktas-investigation-of-the-january-2022-compromise/) on the matter.
- Our friends at[Dagger](https://dagger.io/) had their public launch!
- Google Docs FINALLY[supports markdown](https://workspaceupdates.googleblog.com/2022/03/compose-with-markdown-in-google-docs-on.html)
- We were horrified by the results non-SpiceDB users are getting from their permissions check:


On this page


- Spring has sprung!
- March Updates
- Open Source
- Authzed.com / Authzed Enterprise
- Blogs / Talks
- Entropy from #random


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)
