---
schema_version: "1.0.0"
document_id: "4892259bed6b5e6576833ed56c8a26e30684821e9e45d653218d8616efd05b26"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/feb-2022"
published_at: "2022-03-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:99b71e6a6886de2d29fa1d7478d995f967c0541139e1df34440fbb461e7ee345"
---

# Product Update: February

## Being transparent about our views on transparency


At Authzed, January was not only the beginning of the new year, but also a new set of initiatives. Transparency can mean different things to different people, but here it's built into our DNA. From developing our products open source, publishing our post-mortem reviews of technical and operational processes, providing users direct access to our service's metrics, to blogging our weekly thoughts, we always take an eye towards ensuring that our software, services, and business motivations are not a black box.


We began preparing to ship our usage-based billing for Authzed.com in January, and we're proud to announce that it shipped this February. Because this functionality is directly related to how customers are charged, we made it a hard requirement to ship that users have all the information possible to understand how their bills are calculated. All SpiceDB API responses return metadata to help users understand the performance of their requests; we've chosen this as the building block to charge our Authzed.com users. In the dashboard, billing comes with a brand new set graphs that help users understand in real time their API usage via their computation costs, which are the same metrics used to calculate their bill. These graphs are not only for Authzed.com users, but are also made available in Authzed Enterprise so that on-prem businesses can better understand their projects' authorization usage. By making less complex responses for Authzed.com cheaper, we've aligned our incentives with app developers that want to improve the performance of their applications.


February's focus on transparency involved not only our open source SpiceDB, but also a healthy split between Authzed.com and Authzed Enterprise.


Major highlights include:


- Developing a new distribution pipeline for Authzed Enterprise
- Applying performance and operational improvements to harden SpiceDB and Authzed.com
- Making usage-based billing for Authzed.com generally available
- Implementing a Cloud Spanner datastore for SpiceDB
- Announcing the March 14th[deprecation](https://docs.authzed.com/reference/api#versioning--deprecation-policy) of the V0 APIs


- Creating a new GitHub Action for validating schemas


We're looking forward to a Spring full of pollinating new ideas and helping grow healthy new SpiceDB deployments.


ICYMI: in addition to posting[weekly blog posts](https://authzed.com/blog) , we're now officially posting monthly wrap-ups of events and new functionality we've added to Authzed. We're also giving everyone a little taste of our Slack's random channel and all of it's off-topic goodness.


## February Updates


### Open Source


- SpiceDB got a wide variety of architectural improvements:


- A brand new[Cloud Spanner](https://cloud.google.com/spanner/) datastore
- Middleware now supports a pluggable authentication function
- Consistency is now handled in a middleware with reduced datastore roundtrips
- SpiceDB can now serve gRPC over an in-memory byte buffer
- Validation error messages now include context from current applied schema
- Caching was made configurable with a CLI flag


- [Zed v0.4.0](https://github.com/authzed/zed/releases/tag/v0.4.0) shipped with support to validate playground files, perfect for your CI/CD pipelines
- The folks at[Bitski](https://www.bitski.com/) have authored a[community Rust client](https://github.com/BitskiCo/authzed-rs)
- [action-spicedb-validate](https://github.com/authzed/action-spicedb-validate) is now available to GitHub users for validating schemas in their CI/CD workflows


### Authzed.com


- Usage-based billing is now GA without requesting access
- New graphs for API usage to visualize real-time performance and costs
- Improved[API latency](https://status.authzed.com/) ~25%
- Demonstrated our transparency processes with a[post-mortem](https://authzed.com/blog/post-mortem-feb-2022/)


### Authzed Enterprise


- Authzed Enterprise v0.1.0 was released
- Authzed Enterprise got a new distribution platform:[GitHub](https://github.com/authzed-enterprise)
- Authzed Enterprise container images are now[signed by sigstore](https://github.com/sigstore/friends/tree/main/Authzed) !


### Blogs / Talks


- Jimmy published a[post-mortem](https://authzed.com/blog/post-mortem-feb-2022/) deep dive for our latency spikes
- Joey detailed different strategies for[getting your data into SpiceDB](https://authzed.com/blog/writing-relationships-to-spicedb/)
- Evan enumerated the types of[online schema migrations](https://authzed.com/blog/online-schema-migrations/)
- Folks asked and Sam delivered our[GitHub merge workflows](https://authzed.com/blog/our-github-merge-workflow/)
- The recording for Jake's Carnegie Mellon talk is[now available](https://www.youtube.com/watch?v=lXizkPvSbHU)
- The[Kubelist interviewed](https://www.youtube.com/watch?v=gNUdmvuzQ5o) Jake & Jimmy


## Entropy from #random


- For those avoiding Oracle, MariaDB[announced a public listing](https://mariadb.com/newsroom/press-releases/mariadb-corporation-ab-to-become-a-publicly-traded-company-via-combination-with-angel-pond-holdings-corporation/)
- While we're talking liquidity events,[Linode got acquired](https://www.akamai.com/newsroom/press-release/akamai-to-acquire-linode) by Akamai
- We're keeping our eyes on[cuelang cropping up](https://github.com/grafana/grafana/pull/32527) in Grafana 👀
- Speaking of Grafana, they[launched incident management](https://grafana.com/blog/2022/02/02/announcing-grafana-incident-smart-incident-management-for-your-teams/) , slowly making them a one-stop shop
- We fully exhausted the Wordle variants:[dordle](https://zaratustra.itch.io/dordle) ,[mathler](https://www.mathler.com/) ,[absurdle](https://qntm.org/files/absurdle/absurdle.html)
- We spent far too much time designing our hypothetical[home in the woods](https://johnnyrodgers.is/building-a-modern-home)
- While GitLab's had it for ages, GitHub[finally supports](https://github.blog/2022-02-14-include-diagrams-markdown-files-mermaid/) mermaid.js!
- For those of us that[modded our iPods](https://ellie.wtf/ipod/) back in the day, we had some nostalgia
- A few of us took some well needed PTO with some awesome landscapes:


On this page


- Being transparent about our views on transparency
- February Updates
- Open Source
- Authzed.com
- Authzed Enterprise
- Blogs / Talks
- Entropy from #random


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)
