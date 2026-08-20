---
schema_version: "1.0.0"
document_id: "781726e94830ac5ec89b4187b6135d74686a8b9df564410384a4dc4f64cc3f37"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/april-may-2022"
published_at: "2022-06-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:c187c62357d10faa15dd9b1ba33e8814a7eb77c7d5aeedb7a32bfaeae30575b7"
---

# Product Update: April & May

## A product update double feature


This time around we've got a bargain: we're wrapping up 2 months of progress in the same update. In addition to the updates below, there have been lots of invisible changes to improve the performance and stability of Authzed.com. We've also been heads down privately bringing together a bunch of exciting new developments that we can't wait to share. Keep a look out for future announcements!


Major highlights include:


- [Brad joined the team](https://twitter.com/jimmyzelinskie/status/1524106025082793984) as a full-time SRE
- [SpiceDB released](https://github.com/authzed/spicedb/releases) huge performance improvements and support for MySQL
- The[v0 SpiceDB API](https://docs.authzed.com/reference/api#versioning--deprecation-policy) was deprecated and is being removed.
- [Zed released](https://github.com/authzed/zed/releases) warnings for old SpiceDB versions and the ability to store` --insecure` in contexts


ICYMI: in addition to posting[weekly blog posts](https://authzed.com/blog) , we're now officially posting monthly wrap-ups of events and new functionality we've added to Authzed. We're also giving everyone a little taste of our Slack's random channel and all of its off-topic goodness.


## Updates


### Open Source


- The proposal for[caveats](https://github.com/authzed/spicedb/issues/386) got a lot of good discussion
- A proposal was created for an API to[discover permissions](https://github.com/authzed/spicedb/issues/613) for a resource
- A proposal was created for[intersection arrows](https://github.com/authzed/spicedb/issues/597) which would simplify ABAC designs


### Authzed


- The[playground](https://play.authzed.com/) now has a grid UI for inputting relationships
- SpiceDB & Authzed.com underwent a penetration test
- Chat support is now fully on[Discord](https://authzed.com/discord)
- The blog now has an[RSS feed](https://authzed.com/feed/rss)


### Blogs / Talks


- Joey described the[SpiceDB release process](https://authzed.com/blog/automatic-release-notification/) , how to get notified, and how devs can reuse our code
- Jimmy hopped on the[Cloud Unfiltered podcast](https://soundcloud.com/cloudunfiltered/ep130-authorization-as-a-service-using-googles-zanzibar-system-with-jimmy-zelinskie-of-authzed) at KubeCon Valencia


## Entropy from #random


- [Apple joined the fray](https://www.apple.com/newsroom/2022/03/apple-business-essentials-now-available-for-small-businesses/) alongside Google and Microsoft for enterprise software suites
- We learned all about the[hard drive](https://youtu.be/JcJSW7Rprio) we never knew we never wanted
- Atlassian had an outage, but maintained honesty about their[support SLAs](https://old.reddit.com/r/sysadmin/comments/u14qqq/atlassian_just_gave_us_an_estimate_on_our_support/)
- Have you seen how[heatsink fins are made](https://twitter.com/rombik_su/status/1514294871850250241) ? We hadn't either
- DuckDuckGo brought their[mobile browser](https://spreadprivacy.com/introducing-duckduckgo-for-mac/) to the Mac
- GitHub Actions pricing had a[momentary increase](https://twitter.com/mgreensmith/status/1514338815678156800)
- Why buy a home when you can have[a castle](https://twitter.com/zillowgonewild/status/1515016489010708487) ? Wait... why aren't we working from a castle?
- Gophers writing performant code: I bet you didn't know about[this trick](https://rakyll.org/profiler-labels/) !
- Remember when Elon tried to buy Twitter? Yeah, we're done talking about it, too
- [Google's smart watch](https://www.androidcentral.com/wearables/google-pixel-watch-live-images-exclusive) got leaked
- We found someone[patching Kronos](https://www.rubrik.com/blog/architecture/22/4/choosing-the-right-metadata-store-part-3) into CockroachDB! We keep our eyes on the clock!
- Congrats to our friends at Tailscale on raising their[Series B](https://tailscale.com/blog/series-b/)
- Sam surprised us with his enthusiasm for[trackballs](https://ploopy.co/mini-trackball/)
- We started using[whimsical](https://whimsical.com/) for mock-ups
- CloudFlare launched... a[database service](https://blog.cloudflare.com/introducing-d1/) . When can we call them a cloud provider?
- Google finally launched a competitor to AWS Aurora:[AlloyDB](https://cloud.google.com/alloydb)
- Vercel continues to find ways to[become even faster](https://twitter.com/rauchg/status/1524819061674086405)
- We finally caught wind of this "[data mesh](https://www.amundsen.io/) " trend
- There are almost as many articles on Go's generics as there used to be complaints about them. We liked this explanation of[calculating type sets](https://blog.merovius.de/posts/2022-05-16-calculating-type-sets/) , though.
- Pinterest wrote about[memcache performance](https://thenewstack.io/how-pinterest-tuned-memcached-for-big-performance-gains/) and it's wild to see folks measuring the 99.9 percentile
- Who ever said there's no free lunch? New York got it...[oh wait](https://nypost.com/2022/05/18/grubhub-free-lunch-turns-into-an-nyc-food-fiasco/) .
- Our friends at Tilt got[acquired by Docker](https://www.docker.com/blog/welcome-tilt-fixing-the-pains-of-microservice-development-for-kubernetes/) . Congrats!
- Wait... even VMWare is[being acquired](https://www.reuters.com/markets/us/chipmaker-broadcom-buy-vmware-61-bln-deal-2022-05-26/) !
- As of April 1st, the SpiceDB repository more stargazers than our previous project, Quay:


> We waited too long before open sourcing and building a community for Quay, but we're not making that mistake again! As of today SpiceDB passed Quay in GitHub stars![pic.twitter.com/eADDR7wHg7](https://t.co/eADDR7wHg7)
>
>
> — Jimmy Zelinskie (@jimmyzelinskie)
>
>
> [April 1, 2022](https://twitter.com/jimmyzelinskie/status/1509929057894510602?ref_src=twsrc%5Etfw)


On this page


- A product update double feature
- Updates
- Open Source
- Authzed
- Blogs / Talks
- Entropy from #random


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)
