---
schema_version: "1.0.0"
document_id: "4cd2ec0b4d356f5f210d4fc9a830c04e07141d4fd0cd71643bb98d661deb0ddf"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/jan-2022"
published_at: "2022-02-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:da5f77a64257243a3cae31d9a7cb52cb379e852233017482f19f88b443ea433e"
---

# Product Update: January

## Welcoming the New Year


January has already come and gone! With 2022 officially upon us, we're happy to announce another month jam-packed with updates.


In December, SpiceDB had a huge new feature release:[wildcards](https://authzed.com/blog/unveiling-wildcard-permissions-in-spicedb/) . Like any sufficiently large new feature, this functionality was not without some initial hiccups: wildcards didn't have their full semantics present in` authzed.api.v1.LookupResources` responses. Being the developers of a security product, the team decided that there was no time like the present to test our vulnerability mitigation workflow, so that any future issues would have a tried-and-tested process. We documented[our experience](https://authzed.com/blog/using-github-to-manage-your-first-cve/) on our blog and provided valuable product feedback to the team at GitHub.


The rest of the month maintained a focus on improving the transparency of API performance by landing major functionality in both Authzed.com and Authzed Enterprise. SpiceDB API responses now returns additional metadata that is surfaced in debug logs for[zed](https://github.com/authzed/zed) and metrics in the[AuthZed Serverless](https://app.authzed.com/) . This metadata is useful for understanding the complexity and cache usage while responding to the request.


You can also keep your eyes out for our upcoming talk at[Carnegie Mellon University](https://db.cs.cmu.edu/events/vaccination-2022-spicedb-flexible-permissions-database-for-the-internet-era-jake-moshenko/) as well as the an upcoming[Kubelist podcast](https://kubelist.com/podcast/) . If you need something to hold you over in the mean time, we[published another video](https://www.youtube.com/watch?v=dlARPyDVPZQ) of us modeling familiar apps, this time[Google Groups](https://groups.google.com/) . Google Groups is pretty interesting as it has lots of user configurable permissions and is created by the same company that built Zanzibar, the inspiration for SpiceDB.


ICYMI: in addition to posting[weekly blog posts](https://authzed.com/blog) , we're now officially posting monthly wrap-ups of events and new functionality we've added to Authzed. We're also giving everyone a little taste of our Slack's random channel and all of it's off-topic goodness.


## January Updates


### Open Source


- [SpiceDB v1.4.0](https://github.com/authzed/spicedb/releases/tag/v1.4.0) exercised our security workflow by fixing[CVE-2022-21646](https://github.com/authzed/spicedb/security/advisories/GHSA-7p8f-8hjm-wm92) and introduces[revision-aware schemas](https://github.com/authzed/spicedb/pull/332)
- [Zed v0.3.1](https://github.com/authzed/zed/releases/tag/v0.3.1) shipped with support for logging request complexity and fixed Alpine Linux builds


### Authzed.com


- [The Playground](https://play.authzed.com/) got various fixes for Safari users
- Dispatch, Caching, and Billing metrics are now available in[AuthZed Serverless](https://app.authzed.com/)
- Authzed.com users can now delete development permission systems
- Authzed.com users have access to an[alpha HTTP Gateway](https://github.com/authzed/authzed-go/blob/main/proto/apidocs.swagger.json)
- Emails from the dashboard got way prettier thanks to[Maizzle](https://maizzle.com/)


### Authzed Enterprise


- OIDC is now available for authentication in addition to the existing Auth0 support
- Installations now have an system-org with tenancy and load testing permission systems
- Load testing can now be performed on installations configured without TLS


### Blogs / Talks


- Jimmy covered how powerful[modeling users](https://authzed.com/blog/why-model-users/) in SpiceDB can be
- Jake & Evan[were interviewed](https://www.youtube.com/watch?v=oFRSLA781nc) on The Cockroach Hour
- Jimmy & Jake recorded a video[writing a schema](https://www.youtube.com/watch?v=dlARPyDVPZQ) for Google Groups
- Jake[blogged the highlights](https://authzed.com/blog/build-you-a-google-groups/) from the video
- Jake's talk on the SpiceDB internals for CMU[got scheduled](https://db.cs.cmu.edu/events/vaccination-2022-spicedb-flexible-permissions-database-for-the-internet-era-jake-moshenko/)
- Sam documented his upgrade from[M1 to M1 Max](https://authzed.com/blog/m1-to-the-max/)


## Entropy from #random


- Despite reading the story about[quitting caffeine](https://keygen.sh/blog/i-quit/) , our coffee connoisseurs are still dependent
- Speaking of coffee, our laziest are now ordering[flash-frozen brews](https://cometeer.com/)
- Among all of the misinformation, there's a ray of hope: a[peer-reviewed paper](https://twitter.com/sanhotree/status/1480969407648366592) proving cannabinoids prevents COVID19
- We're all ready to throw out our TVs now that[QD-OLED](https://www.theverge.com/2022/1/4/22865220/sony-a95k-qd-oled-qdoled-4k-tv-announced-features-explainer) has begun shipping
- We found a thread of[no-code utilities](https://twitter.com/heyeaslo/status/1481808298768683009) for startups, but haven't tried a single one
- Microsoft[bought](https://news.microsoft.com/2022/01/18/microsoft-to-acquire-activision-blizzard-to-bring-the-joy-and-community-of-gaming-to-everyone-across-every-device/) Activision-Blizzard... we hope they got a discount.
- US Residents can and should get their[free COVID19 test kits](https://special.usps.com/testkits)
- The folks over at GitHub must be LOTR fans, because they've got an API event called[gollum](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#gollum)
- We're hyped for the[Netflix Cuphead animation](https://www.theverge.com/2022/1/18/22884643/netflix-the-cuphead-show-trailer-release-date) , but that's still no DLC expansion!
- Roblox had an[interesting outage](https://blog.roblox.com/2022/01/roblox-return-to-service-10-28-10-31-2021/) . They should probably talk to our friends at[Polar Signals](https://www.polarsignals.com/)
- The New York Fed publishes their economic models (written in Julia!), but they have some[concerning comments](https://github.com/FRBNY-DSGE/DSGE.jl/blob/bcd8ed296653e42d5e56f06c5eda22d55ba47147/src/models/representative/m805/observables.jl#L31)
- [Biscuits](https://www.biscuitsec.org/blog/biscuit-2-0/) got a 2.0 release, which is a nice iteration on the previous-generation of authorization
- The parents on our team are[starting their kids](https://computerengineeringforbabies.com/) off young
- The US Gov released a Zero Trust memo, only[one person in the world](https://www.bastionzero.com/blog/i-read-the-federal-governments-zero-trust-memo-so-you-dont-have-to) read it, but he explained it for us
- Pour one out for[Dgraph Labs](https://discuss.dgraph.io/t/quitting-dgraph-labs/16702/6) -- we'll still contribute and offer to maintain any shared code between the two of us
- Wordle got a[big exit](https://www.nytimes.com/2022/01/31/business/media/new-york-times-wordle.html) ! It's great to hear a success story from a totally bootstrapped startup
- It's official, we all got removed from the Quay GitHub org:


> 😭[pic.twitter.com/bnCIRS0Xw4](https://t.co/bnCIRS0Xw4)
>
>
> — Jimmy Zelinskie (@jimmyzelinskie)
>
>
> [January 6, 2022](https://twitter.com/jimmyzelinskie/status/1479213190466752512?ref_src=twsrc%5Etfw)


On this page


- Welcoming the New Year
- January Updates
- Open Source
- Authzed.com
- Authzed Enterprise
- Blogs / Talks
- Entropy from #random


## Related


[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)[Product Backup And Restore Your SpiceDB With zed Learn how to backup and restore your SpiceDB instance using zed, the SpiceDB companion CLI tool. Victor Roldan Betancort · Feb 6, 2024 · 7 min](https://authzed.com/blog/how-to-backup-and-restore-spicedb)


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)
