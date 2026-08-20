---
schema_version: "1.0.0"
document_id: "950aec1d84e9ce2bc69ae3a43cad5853cb196db66a45304734f9f27cb6ff0ead"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/jan-2023"
published_at: "2023-02-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:3402927713ece9ada3de79d4bc750c998333dd625dbbef6533adf385bbcdd947"
---

# Product Update: January

## A fresh new year of updates


We decided to kick off the year fresh with a ton of new updates on almost all of our open source projects. Open source isn't the only place where we've been building, though! Our managed services also saw some pretty big updates. I'm super proud of the community and our team at Authzed for what they've managed to deliver.


Major highlights include:


- Google Cloud is now generally available for SpiceDB Dedicated
- Releases on almost all of our open source projects
- Caveats are available in zed and the Playground
- SpiceDB Operator now supports automated update channels


ICYMI: in addition to posting[weekly blog posts](https://authzed.com/blog) , we're now officially posting monthly wrap-ups of events and new functionality we've added to Authzed. We're also giving everyone a little taste of our Slack's random channel and all of its off-topic goodness.


## Updates


### Open Source


- SpiceDB[v1.16.0](https://github.com/authzed/spicedb/releases/tag/v1.16.0) and[v1.16.1](https://github.com/authzed/spicedb/releases/tag/v1.16.1) were released


- Developer API now supports caveats
- Dispatch concurrency limits are now configurable on each request type
- v1.CheckPermission is now optimized for cases where many subjects have the same relation


- SpiceDB Operator[v1.1.0](https://github.com/authzed/spicedb-operator/releases/tag/v1.1.0) was released


- Introduced update channels for fully automating SpiceDB upgrades
- Added support for custom annotations
- Added support for custom service accounts


- Controller Idioms[v0.6.0](https://github.com/authzed/controller-idioms/releases/tag/v0.6.0) and[v0.7.0](https://github.com/authzed/controller-idioms/releases/tag/v0.7.0) were released


- Added a hash.Set for hashable objects
- Bumped Kubernetes to v1.26
- Simplified unpausing to removing a label


- Zed[v0.8.0](https://github.com/authzed/zed/releases/tag/v0.8.0) was released


- Contexts can now store custom CAs
- Full support for caveats, including in debug traces


### Authzed Products


- Playground


- Caveats are now available in the Playground
- Traces were also expanded to include caveats
- Zed is now[integrated](https://twitter.com/authzed/status/1621161709749649409) into the Playground
- New drop-down example loader


- Dedicated


- Now available on Google Cloud!
- New clusters can now specify a size (small, medium, large)
- OIDC, Okta, GSuite SSO support


- Serverless


- Rate limits now grpc error code` RESOURCE_EXHAUSTED` instead of` UNAVAILABLE`


### Community


- [Discord](https://authzed.com/discord) is now 975 users strong!
- Jake walked-through modeling a[Google Cloud IAM Service](https://authzed.com/blog/google-cloud-iam-modeling)
- Jake[gave a shoutout](https://twitter.com/CockroachDB/status/1618719178906640405) to our friends at CockroachDB
- Lots of[example schemas](https://github.com/authzed/examples/tree/main/schemas) were added


## Entropy from #random


- CircleCI users[rotate your secrets](https://twitter.com/CircleCI/status/1610828227349463041) !
- Years and years of Postgres use and we still find \[features we never knew about\]
- Apple fans rejoice! The Butterfly Keyboard class action[settlement](https://www.keyboardsettlement.com/dates) is coming
- We aren't writing much Python these days, but we wish we had[Ruff](https://github.com/charliermarsh/ruff) when we did
- Victor got a[new toy](https://www.analogue.co/pocket) and we're all jealous
- We choked back our tears reading the[YAML document from Hell](https://ruudvanasseldonk.com/2023/01/11/the-yaml-document-from-hell)
- Go[1.20](https://go.dev/doc/go1.20) shipped with PGO
- A Discord member used[ChatGPT](https://discord.com/channels/844600078504951838/1064121957303455795/1064491406225322014) to write a SpiceDB schema
- Our M1 Max are all garbage now that[M2 is out](https://www.apple.com/newsroom/2023/01/apple-unveils-macbook-pro-featuring-m2-pro-and-m2-max/)
- macOS user with an external monitor?[Lunar](https://lunar.fyi/) is a game-changer
- The[Beechers](https://beecherscellar.com/) in NYC closed! We'll just have to visit Seattle now.
- ` << EOF` We finally learned the name to a[very common shell feature](https://en.wikipedia.org/wiki/Here_document)` EOF`


On this page


- A fresh new year of updates
- Updates
- Open Source
- Authzed Products
- Community
- Entropy from #random


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)
