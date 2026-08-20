---
schema_version: "1.0.0"
document_id: "6075b136e53d1afa30b6bc1e540be656bbfa54d92911ca245f0ff17fcbfef0e7"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/november-2021"
published_at: "2021-12-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:a38774396ccac4591b1b312d874fa730b54b2b17668a49937106dd6e5eb1bf38"
---

# Product Update: November

## Giving Thanks


As we make our way into holiday season, the team over at Authzed is showing no signs of slowing down. This month we give thanks -- and updates on the product.


November was full of feedback from our quickly growing community. We've been working alongside outside contributors to fix a variety of bugs and mature existing functionality in SpiceDB. November also marks the beginning of a major experiment --[importing schema and relationships](https://authzed.com/blog/zed-import/) from external systems. This functionality can be primarily found in[zed](https://github.com/authzed/zed) , the SpiceDB & Authzed command line tool.


Speaking of Authzed, we're still keeping our eyes out for an SRE to architect and realize our planet-wide deployment of our SpiceDB serverless platform. If these updates catch your interest, check out our[careers page](https://www.workatastartup.com/companies/authzed) .


ICYMI: in addition to posting[weekly blog posts](https://authzed.com/blog) , we're now officially posting monthly wrap-ups of events and new functionality we've added to Authzed. We're also giving everyone a little taste of our Slack's random channel and all of it's off-topic goodness.


## November Updates


- We wrote about the[architecture of SpiceDB](https://authzed.com/blog/spicedb-architecture/) and how we[load-balance gRPC traffic](https://authzed.com/blog/consistent-hash-load-balancing-grpc/)
- We archived[Servok](https://github.com/authzed/servok) and built discovering Kubernetes pods for re-dispatching[right into SpiceDB](https://github.com/authzed/spicedb/pull/284)
- The[playground](https://play.authzed.com/) now supports deploying to the Authzed dashboard in a single click
- Various community-reported bugs in SpiceDB were fixed including various goroutine leaks, intersection being[displayed as union](https://github.com/authzed/spicedb/pull/281) in Expand responses,[duplicate relationships](https://github.com/authzed/spicedb/pull/275) in memdb, and[lack of caching](https://github.com/authzed/spicedb/pull/268) for Lookup requests
- Traceability of SpiceDB was improved by reworking tracing into a[gRPC middleware](https://github.com/authzed/spicedb/pull/272)
- SpiceDB can listen on[unix sockets](https://github.com/authzed/spicedb/pull/267) (including abstract UDS)
- [Jon Whitaker](https://github.com/jonwhitty) contributed an implementation of the[v1 Watch API](https://github.com/authzed/spicedb/pull/263) to SpiceDB
- Zed can now[import schemas and relationships](https://github.com/authzed/zed/pull/61) from URLs to the Playground and various text hosts like Gists and Pastebin
- Zed can experimentally[import from PostgreSQL databases](https://authzed.com/blog/zed-import/)


## Entropy from #random


- New Yorkers[got their boosters](https://newyork.cbslocal.com/2021/11/15/nyc-covid-vaccine-boosters-for-all-adults/) and subsequently their sick days
- Our friends at Graphite wrote about[Stacked changes](https://news.ycombinator.com/item?id=29255195)
- Is[fusion power](https://blog.samaltman.com/helion) a reality? I'll stop paying my ConEd bill and find out
- Someone spent too much time[playing with legos](https://www.youtube.com/watch?v=pwglOlD7e0M) and we spent too much time watching them
- Evan blew our mind with an all-American recipe of adding[even more corn](https://www.bonappetit.com/recipe/cornbread-stuffing-with-sausage-and-corn-nuts) to your cornbread
- While none of us made a[Flamin' Hot](https://www.reynoldsbrands.com/recipes/hot-cheese-puffs-turkey-recipe) Turkey, that didn't mean we didn't have our fair share of fun this holiday:


On this page


- Giving Thanks
- November Updates
- Entropy from #random


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)
