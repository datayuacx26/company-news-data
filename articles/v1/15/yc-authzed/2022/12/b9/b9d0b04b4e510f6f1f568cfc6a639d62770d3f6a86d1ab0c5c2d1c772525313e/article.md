---
schema_version: "1.0.0"
document_id: "b9d0b04b4e510f6f1f568cfc6a639d62770d3f6a86d1ab0c5c2d1c772525313e"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/year-to-remember"
published_at: "2022-12-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:d96ba7d669f485cc9052dcc127c7f98e966093af210c4d15e85459b55935b659"
---

# A Year to Remember

It was just another day at the office in 2019 when we first read the paper that would set the course of our future careers. My cofounders and I were no strangers to reading the occasional research paper--I had originally joined their team at[CoreOS](https://wikipedia.org/wiki/CoreOS) because I had read the[Raft paper](https://en.wikipedia.org/wiki/Raft_(algorithm)) and discovered[etcd](https://etcd.io/) . But there was something about this paper in particular: it laid out the solution to major problems we were having at the time.


Our container registry product,[Quay](https://quay.io/) , had scaled quite far, but we'd reached the limits for our existing strategy for authorizing API requests:


- The majority of our database CPU usage was performing` JOIN` s necessary for checking permissions
- Our authorization library was complex, brittle, and had to be audited before a change could ship
- We had feature requests that could only be accomplished by writing a new authorization system
- We had no strategy for sharing authorization code and data with new services


A year later, we quit[Red Hat](https://www.redhat.com/en/about/press-releases/red-hat-acquire-coreos-expanding-its-kubernetes-and-containers-leadership) , who had acquired CoreOS, to return to start-up life. It was obvious to us what we'd work on next.


It's been 3 years since the fateful day we read that paper and two years since we left to start a company around it. As we wrap up 2022, I think it's important to pause and reflect on all that's been accomplished. I honestly still can't believe how far we've come and I'm both incredibly proud and humbled by everyone involved. I'd like to personally thank not only our team, but also everyone in our community.


In 2022, SpiceDB had a ton of major developments:


- Triple the number of releases
- Support for[Cloud Spanner](https://cloud.google.com/spanner) as a datastore
- Support for MySQL (e.g.[MariaDB](https://mariadb.org/) ,[Vitess](https://vitess.io/) ) as a datastore was contributed by the[GitHub Authorization team](https://github.com/authzed/spicedb/pull/525)
- Dramatic performance improvements for the Postgres datastore
- A new compilation target: WebAssembly (which now powers the[Playground](https://play.authzed.com/) )
- An additional API,[LookupSubjects](https://authzed.com/blog/lookup-subjects) , to find all of the subjects with a permission
- Even deeper integration with Kubernetes via the[SpiceDB Operator](https://github.com/authzed/spicedb-operator)
- [Caveats](https://authzed.com/blog/caveats) : dynamic policies that scale


As well as the community:


- The number of[Discord](https://authzed.com/discord) users quadrupled
- Our GitHub organization now has a bunch of[followers](https://github.com/orgs/authzed/followers) and SpiceDB doubled its[stargazers](https://github.com/authzed/spicedb/stargazers)
- Thousands of questions were asked and answered (mostly on[Discord](https://authzed.com/discord) , but also on[GitHub Issues](https://github.com/authzed/spicedb/issues?q=is%3Aissue+label%3Akind%2Fquestion) and even[StackOverflow](https://stackoverflow.com/search?q=spicedb) )
- Hundreds of user stories were documented and represented for various feature proposals
- We remained true to our roots by joining the[Cloud Native Computing Foundation](https://authzed.com/blog/cncf-membership) extending our community
- SpiceDB maintainers successfully exercised our security policy with our[first CVE](https://osv.dev/vulnerability/GHSA-7p8f-8hjm-wm92) and[documented the experience](https://authzed.com/blog/using-github-to-manage-your-first-cve) for others to learn, too
- An[Awesome List](https://github.com/authzed/awesome-spicedb) was created to chronicalize all the awesome new community projects
- An[examples repository](https://github.com/authzed/examples) was created so folks could contribute example setups for various environments


And finally our company:


- We launched[zanzibar.tech](https://zanzibar.tech/) , our annotated love letter to Google's Zanzibar paper
- We're honored to have Brad, Victor, and Damian join the team
- We had an annual off-site event in upstate New York
- We built and launched[SpiceDB Dedicated](https://authzed.com/pricing) on AWS and GCP
- SpiceDB Serverless SREs managed their first critical incident whose[post-mortem](https://authzed.com/blog/post-mortem-feb-2022) contributed to better understanding for both maintainers of SpiceDB and the Go runtime


This is quite the list of accomplishments, but what would be an annual wrap-up without posting some predictions? I decided to collect one prediction from each of us on the team:


- Jimmy: Databases that support edge/mutli-region (like SpiceDB) will become table-stakes for building new applications.
- Jake: More products will say that they’re “Zanzibar inspired” while continuing to miss the mark.
- Joey: The year of the WASM application
- Sam: The sudden disappearance of “Lorem Ipsum” placeholders
- Brad:[Platform Engineering](https://platformengineering.org/) will continue to gain cultural importance in DevOps / SRE circles.
- Victor: we get the first AI-generated short movie


And with that, that's a wrap! From our team, happy holidays and have a great new year!


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)
