---
schema_version: "1.0.0"
document_id: "034406e2df6adb34d3e72ceab3a97d76e648835e4fcde29a20a1df5d28c6e4a2"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/june-2022"
published_at: "2022-07-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:692aa4de0fb5570a51263e6d8514a49d1644bc40f2455f90330b6f11a5afd1a0"
---

# Product Update: June

## This June, we dedicated ourselves


We've kept ourselves fairly quiet during the past month, but that's because today I have the honor of sharing some great news. I'm excited to announce that in June we've shipped the first iteration of[Authzed Dedicated](https://authzed.com/pricing) . We're all extremely proud of what the team has crafted: the world's first dedicated offering of SpiceDB.


Authzed Dedicated comes with the functionality that you'd expect from a dedicated database service, but with our unique twist. All SpiceDB clusters are managed by our expert SRE team who carry the pager. Each environment is secured by a private network that is peered only to your internal network. Beyond configuring the cloud region, we've taken an eye towards reducing latency everywhere we can. Behind the scenes, we've built a whole new deployment stack. This stack lead to the development of some new projects that we plan to open source, so[keep a lookout](http://eepurl.com/hEeb6z) for future product updates!


Major highlights this month include:


- [Authzed Dedicated](https://authzed.com/pricing) is now available!
- [Victor joined the team](https://twitter.com/vroldanbet/status/1533936652267167746) as a full-time software engineer
- [GitHub sponsored the SpiceDB maintainers](https://twitter.com/jimmyzelinskie/status/1541837849607159808) for Maintainer Month
- SpiceDB[v1.8.0](https://github.com/authzed/spicedb/releases/tag/v1.8.0) &[v1.9.0](https://github.com/authzed/spicedb/releases/tag/v1.9.0) shipped with improved datastore performance, deep health checking, and more!
- The community is growing! We broke 500 users in[Discord](https://authzed.com/discord)


ICYMI: in addition to posting[weekly blog posts](https://authzed.com/blog) , we're now officially posting monthly wrap-ups of events and new functionality we've added to Authzed. We're also giving everyone a little taste of our Slack's random channel and all of its off-topic goodness.


## Updates


### Open Source


- [SpiceDB released](https://github.com/authzed/spicedb/releases) improving datastore performance and feature-parity for MySQL
- The deprecated[v0 SpiceDB API](https://docs.authzed.com/reference/api#versioning--deprecation-policy) was removed from open source in v1.9.0
- The[documentation landing page](https://docs.authzed.com/) got a facelift


### Authzed


- [Authzed Dedicated](https://authzed.com/pricing) is now available!
- Dedicated now supports single-region AWS deployments
- Dedicated networking on AWS is now peered via[PrivateLink](https://aws.amazon.com/privatelink) and designed for low-latency
- Our community broke 500 users in[Discord](https://authzed.com/discord) and 800 followers on[Twitter](https://twitter.com/authzed)


### Blogs / Talks


- The J-team went upstate to[Bear Mountain for LFGNYC](https://twitter.com/psomrah/status/1537561734088646658)
- Jimmy[reshared](https://twitter.com/jimmyzelinskie/status/1540019907492036609) his podcast with Cisco's Cloud Unfiltered podcast


## Entropy from #random


- Our friends over at[buf released Connect](https://twitter.com/bufbuild/status/1532035666237849602) their gRPC alternative
- Our favorite Go library, errgroup, got the ability to[limit the number of active goroutines](https://pkg.go.dev/golang.org/x/sync/errgroup#Group.TryGo)
- [WorkOS acquired Modulz](https://twitter.com/colmtuite/status/1532024535037403137)
- Ever wonder what GameBoy camera looks like[with a Canon lens](http://ekeler.com/game-boy-camera-canon-ef-mount) ?
- [WWDC 2022 aired](https://www.youtube.com/watch?v=3tmHJHUsiCI) . Thankfully our M1s are not completely superseded... yet!
- Did we mention that[Rosetta for x86 Linux](https://www.phoronix.com/scan.php?page=news_item&px=macOS-13-Rosetta-Linux-Binaries) is going to be a thing?
- The[Cuphead DLC](https://cupheadgame.com/dlc.html) finally shipped!
- GitHub's[Atom editor was sunset](https://github.blog/2022-06-08-sunsetting-atom/) -- time to migrate to VSCode
- Apparently there just aren't enough people in the US to[sustain Amazon warehouses](https://www.vox.com/recode/23170900/leaked-amazon-memo-warehouses-hiring-shortage)
- Amazon doesn't just need living people: they're also teaming up with[the deceased for Alexa](https://twitter.com/ani_digital/status/1539853467392430080)
- The folks over at Azure[released Eraser](https://github.com/Azure/eraser) to clean up non-running images from Kubernetes clusters
- [Python 3.11](https://deepsource.io/blog/python-3-11-whats-new/) has some really nice new quality of life features and performance improvements
- What does your monitor config tell us about you?


On this page


- This June, we dedicated ourselves
- Updates
- Open Source
- Authzed
- Blogs / Talks
- Entropy from #random


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)
