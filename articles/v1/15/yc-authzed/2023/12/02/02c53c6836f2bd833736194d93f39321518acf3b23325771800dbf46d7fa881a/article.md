---
schema_version: "1.0.0"
document_id: "02c53c6836f2bd833736194d93f39321518acf3b23325771800dbf46d7fa881a"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/november-2023-product-updates"
published_at: "2023-12-01T22:05:02.110+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:628b5f34569b39c64d38a2c6f739abd71d1392788679128ab5f69c4c4de9aacb"
---

# November 2023 Product Updates

As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates!


A few highlights:


- **Two** SpiceDB releases this month:[v1.27](https://github.com/authzed/spicedb/releases/tag/v1.27.0) and[v1.28](https://github.com/authzed/spicedb/releases/tag/v1.28.0)
- **Two** new videos:[Interpreting SpiceDB OpenTelemetry traces](https://authzed.com/blog/spicedb-tracing-how-to-interpret-our-opentelemetry-traces) and[SpiceDB Academy | Language Primer](https://www.youtube.com/watch?v=AoK0LrkGFDY)
- Updates to the zed CLI and SpiceDB operator


## Announcements


🥜 Evan Cordell discussed the highly relatable problem of getting Policy in your ReBAC and getting ReBAC in your Policy in his latest blog:[Policy-Based Access Control (PBAC) vs Google Zanzibar: When You Should Use One or the Other](https://authzed.com/blog/policy-based-access-control)


🔎 Victor shared SpiceDB’s handy-dandy tracing UX improvements in a short video:[SpiceDB Tracing: How to interpret our OpenTelemetry traces](https://authzed.com/blog/spicedb-tracing-how-to-interpret-our-opentelemetry-traces)


🎓 Jake taught the fundamentals of modeling permissions and schema building in the first-ever episode of SpiceDB Academy![SpiceDB Academy | Language Primer](https://www.youtube.com/watch?v=AoK0LrkGFDY)


## SpiceDB


### Releases


-


**SpiceDB v1.27 |[Release Notes](https://github.com/authzed/spicedb/releases/tag/v1.27.0)**


- Enhanced traces: identify high-demand parts of your schema
- Over 50% latency and CPU usage reduction possible with new dispatch deduplication
- Observability improvements: Go runtime and Spanner performance metrics now available
- Improved Spanner performance with new watch-driven schema caching compatibility
- MySQL datastore migration adds a new index addressing inefficiencies in the
- Watch API Deprecated support for MySQL v5 (EOL)
- Special thanks to community contributor[@bradengroom](https://github.com/bradengroom) for fixing[Issue 1217](https://github.com/authzed/spicedb/issues/1217) - Run postgres datastore tests with pgbouncer


-


**SpiceDB v1.28 |[Release Notes](https://github.com/authzed/spicedb/releases/tag/v1.28.0)**


- Datastore deduplication improvements
- New "SpiceDB Repair" command will help folks running` pg_dump / pg_restore` on Postgres-based SpiceDBs
- Better error reporting on Watch API for CockroachDB-based SpiceDBs


-


**SpiceDB-operator v1.12 |[Release Notes](https://github.com/authzed/spicedb-operator/releases/tag/v1.12.0)**


- You can now[define the filepath](https://github.com/authzed/spicedb-operator/issues/272) for the CA certificate in the tls config, eliminating the need for additional processing when creating a secret that resides in a different namespace.
- Adds[upgrade edges](https://github.com/authzed/spicedb-operator/pull/274) for SpiceDB 1.26 to the stable channel, which has been out for a while without any known regressions. There is a single-phase migration for postgres.


-


**zed CLI v0.15.1 |[Release Notes](https://github.com/authzed/zed/releases/tag/v0.15.1)**


- Restore and backup commands can take a long time, depending on the number of relationships. When trying to abort some instances of those commands, zed wouldn't react to Control+C because context cancelation wasn't being checked.[With this change](https://github.com/authzed/zed/pull/308) the operation will abort immediately⚡


## Entropy


-


In Discord, Yordis shared an easy and delicious method for making Cuban coffee in an[imusa](https://www.imusausa.com/product/imusa-electric-moka-maker-3-cup-6-cup-480-watts-black/) pot (you can also use a[Moka](https://en.wikipedia.org/wiki/Moka_pot) or similar). Add two tablespoons of cane sugar in the bottom of the pot, add your coffee/espresso as normal and cook it like you normally would. It came out perfect!


-


A lively discussion in the Discord on mechanical keyboard suggestions:


- The[HHKB Studio](https://hhkeyboard.us/hhkb-studio#introduction) features a gesture pad aka “the Thinkpad nubbin”
- The small but mighty[NuPhy Air75](https://nuphy.com/products/air75) came highly recommended
- The open source Launch keyboard from System76 got rave reviews after demoing at a conference.


-


The AuthZed team shared more breakfast cereal-related trivia:


- Apparently Oreo-Os have a new recipe since they launched
- Timing of cereal consumption varied greatly from breakfast to late at night and even in the afternoon.
- Golden raisins as a cereal topping were generally celebrated


On this page


- Announcements
- SpiceDB
- Releases
- Entropy


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)


[Engineering Introducing: Fine-Grained Access Management The systems we build at AuthZed are the direct result of feedback from our community and customers. Because security is the core of our flagship product, SpiceDB, we take feedback on this topic very seriously. We’ve heard you, and today we’re proud to introduce a better way to secure AuthZed customers’ client applications accessing the SpiceDB API: **Fine-Grained Access Management** (FGAM). Apr 27, 2023 · 4 min](https://authzed.com/blog/introducing-fine-grained-access-management)[Engineering Introducing: Fine-Grained Access Management The systems we build at AuthZed are the direct result of feedback from our community and customers. Because security is the core of our flagship product, SpiceDB, we take feedback on this topic very seriously. We’ve heard you, and today we’re proud to introduce a better way to secure AuthZed customers’ client applications accessing the SpiceDB API: **Fine-Grained Access Management** (FGAM). Victor Roldan Betancort · Apr 27, 2023 · 4 min](https://authzed.com/blog/introducing-fine-grained-access-management)
