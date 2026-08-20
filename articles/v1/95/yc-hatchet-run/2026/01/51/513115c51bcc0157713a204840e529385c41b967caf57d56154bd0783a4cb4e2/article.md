---
schema_version: "1.0.0"
document_id: "513115c51bcc0157713a204840e529385c41b967caf57d56154bd0783a4cb4e2"
company_key: "yc-hatchet-run"
company: "Hatchet"
source_id: "yc-hatchet-run-rss-3f16f06bd764"
canonical_url: "https://hatchet.run/blog/two-years-open-source"
published_at: "2026-01-13T00:00:00+00:00"
first_seen_at: "2026-08-09T22:57:06.374584+00:00"
fetched_at: "2026-08-09T22:57:07.291413+00:00"
content_hash: "sha256:5ca91625faa6b0916545b6900587a81f3c146814989b50dd5524a2bf360f6363"
---

# Reflecting on two years of building an open-source startup

Alexander Belanger


Co-Founder


Hatchet


A perennial question of many[Launch HNs](https://news.ycombinator.com/launches) for open-source companies is some variation of the following:


*How do we know you won’t change your license?*


As the founder of an open-source developer tools startup, it’s difficult to answer this question—and rightfully so! There are typically no legal mechanisms or restrictions in place to guarantee that you don’t change your license at some point in the future, and companies will usually make a drastic decision like this when it threatens their survival as a business. You’re basically asking founders to choose, in this hypothetical scenario, whether or not they’d want to survive as a business or are willing to go down with the ship.


We’ve always been quite opinionated on licensing here at Hatchet. Our philosophy can be best summarized as “MIT or bust.” And as we’re coming up on 2 years of working on Hatchet, I wanted to write down a collection of thoughts about our journey so far and open-source goals for the next year, along with documenting some of the challenges that we’re facing while building a fully MIT-licensed developer tools startup.


#### How we got here


Hatchet started as part of the YC Winter 2024 batch with just an idea. For both my co-founder Gabe and me, it was our second time doing YC. We had gone through the S20 batch with different companies—[Porter](https://www.porter.run/) and[Clearmix](https://www.ycombinator.com/companies/clearmix) —and it didn’t kill us, so we decided to try again.


During the W24 batch, we were lucky enough to avoid[pivot hell](https://www.ycombinator.com/library/NA-from-pivot-hell-to-1-4-billion-unicorn) . We came in with a clear vision (I wanted to improve[Temporal](https://github.com/temporalio/temporal) , Gabe[wanted to improve Celery](https://hatchet.run/blog/problems-with-celery) ), and fortunately that vision resonated with enough users that we got off the ground during the batch with our first Hacker News launch.


Us founders looking tired during W24, Piccino coffees in hand.


#### Libraries vs platforms


We[initially launched](https://news.ycombinator.com/item?id=39643136) in early 2024 as an open-source, distributed task queue built on top of Postgres. One of the things we grappled with initially (and continue to grapple with) is that we wanted to build a platform, not a library. Many task queue libraries do a great job of invoking tasks across a message queue like Redis or RabbitMQ—our favorites here are Celery, BullMQ, and River. But having seen these libraries inevitably turn into home-grown platforms (like entire teams building admin dashboards around Celery), we knew that we wanted to build something which bundled observability, alerting, manual replays, and logging into a nice UI and API.


While good platforms create a better production experience, libraries are a much better development experience because they’re so easy to adopt and use. Frankly, Hatchet isn’t as lightweight as we want. Which brings us to resolution #1:


🔔


2026 Goal #1: make Hatchet more lightweight.


We already have a single container called` hatchet-lite` which bundles our API, engine and frontend, but we want to go further here. We’re[currently working on a CLI](https://github.com/hatchet-dev/hatchet/pull/2701) to make spinning up a local Hatchet server even easier. And we haven’t fully decided whether or not we can offer Hatchet as a single “library-mode” binary, but we would really damn well like to.


Generally, founders working on open source companies should think more about which adoption curve is appealing to them. Libraries and frameworks have a much faster adoption curve but are much harder to monetize, while platforms have a slower adoption curve but tend to be easier to monetize (a third category, databases, feels like playing on hard mode—slow adoption curve and difficult to monetize).


#### The license


One of the things that’s distinct about Hatchet (at least compared to developer tool startups) is that we’re[fully MIT licensed](https://github.com/hatchet-dev/hatchet/blob/main/LICENSE) , instead of a more restrictive license or a dual license with an enterprise edition. There are a few reasons for this:


1. **It’s what we would use** —if I encounter an exciting open-source project, the first thing I check is the license. I usually don’t fit the profile of someone who could pay big bucks for an enterprise edition, so I’ll get extremely frustrated by the things that I *can’t* do with the exciting new tool. Almost all of our product decisions are guided by what we would use as engineers, and being MIT licensed is a core value for us.
2. **Focus** —my previous startup offered a self-hosted enterprise edition, a community edition, and a cloud version, and it became impossible to do all three well at an early stage. From a product perspective, community feedback and cloud feedback tend to improve the core product massively, while the enterprise edition can improve the auxiliary features (RBAC, log-in options, audit logs, etc). At our current stage, the most important thing is making the core product as good as possible.
3. **Enshittification** —we also don’t want the product to gradually get worse. Enshittification is an overused but apt term for what would inevitably happen if we started discussing which features belonged in the enterprise edition versus the community edition. In an ideal world, companies with money will pay for the enterprise edition and smaller companies and hobbyists can get by just fine with the community edition, but I have yet to see an open-source offering where this works out in practice.
4. **Economics** —one of the only convincing paths I see for an open-source developer tooling company is to offer a cloud infra product which is more cost-effective at scale than self-hosting the product yourself. Being MIT licensed and easily self-hostable forces us to invest in our cloud platform’s cost effectiveness and performance profile.


I’m proud that we generate significant revenue from our cloud version while continuing to develop primarily on the open-source platform and maintaining the MIT license. Which brings me to what will hopefully be a continuing resolution of ours:


🔔


2026 Goal #2: maintain the 100% MIT license.


#### Plugins


One of the challenges we ran into this year was that our cloud offering runs more tasks per day than any of the self-hosted instances out in the wild (at least that we’re aware of). Which means we’ve started to do more custom work on our cloud platform which we can’t always merge promptly into our open source, because we’re either in the process of gradually rolling it out, or it’s extremely coupled to our custom infra layer. And even though all of this custom work is built using a plugin pattern on top of the core Hatchet OSS offering, we know that there are open source users who would benefit from parts of this plugin model. So resolution #3 is:


🔔


2026 Goal #3: build out and document guidelines for extending Hatchet’s core offering.


Some ideas here are: better support for additional auth plugins, support for the OLAP providers we currently use on our cloud platform, and support for reducing Hatchet’s Postgres disk footprint by offloading cold payloads to an object store.


#### Roadmap


If being MIT licensed and relatively easy to self-host are our big wins, there are two major items that we struggle with.


First, we’re awful at communicating our roadmap. We operate internally on two-week timelines and some longer-term goals, but historically it’s been very difficult to forecast exactly what the next quarter or two will look like, so we aren’t super transparent about this with the community.


The good news is, we’re getting much better at this! And as a result we will be providing a product and open-source roadmap relatively soon.


🔔


2026 Goal #4: launch a publicly available roadmap.


#### Releases


For those that have been following the product for a while, we had a major product launch in April of this year when we announced Hatchet v1: a full rewrite of the core Hatchet engine along with new SDKs for each language we support (Typescript, Python, and Go). From our perspective, this was a major success: not only did it make our product easier to use with more powerful features, but we also maintained backwards compatibility with v0 for over 6 months, and nearly full compatibility migrating from v0 to v1, which for a team of our size was a massive effort.


Unfortunately, after the October 1st deprecation deadline, we were waiting to remove v0 pathways from the engine before the next release of Hatchet, which was quite a bit more cleanup than we were expecting. Due to these hiccups, we didn’t release a new` latest` version this fall. The good news is,[it’s released now](https://github.com/hatchet-dev/hatchet/releases/tag/v0.74.9) !


🔔


2026 Goal #5: maintain a two-week release cadence.


#### Contributors


While we absolutely welcome external contributions to our project, we haven’t invested enough in our local development or contributor guides to make it particularly easy for developers to contribute. And in the early days, Hatchet was very far from being able to accept significant external contributions, as we were iterating so quickly on the core product that the architecture was incredibly confusing.


Explaining our early architecture to contributors, from[this gem](https://www.youtube.com/watch?v=y8OnoxKotPQ)


These days, the codebase is much more stable, but we’re facing a new challenge: a lot of the small PRs /` good-first-issue` s are becoming faster for core contributors to handle because of AI-assisted coding, and simultaneously there’s reduced trust in larger PRs because of AI slop and an increase in supply chain attacks. So while small PRs were usually an easy entrypoint into a new codebase or product, and a good way to build trust with core contributors, it feels like there’s less low-hanging fruit in the product these days.


That said, there’s obviously plenty of room to build a healthy contributor ecosystem. While this isn’t an immediate priority in the next few months, I’d hope that by the end of the year we can invest more time in helping new contributors get up to speed on the project. So the last resolution:


🔔


2026 Goal #6: invest in tooling and documentation for contributors.


So, if you’ve read this far and you’re interested in either getting more involved in our open source or working with us as a contributor, please reach out to us in[Discord](https://discord.com/invite/ZMeUafwH89) !


#### Wrapping up 2025


I also wanted to share some of my personal highlights from the past year:


- [Matt](https://github.com/mrkaye97) and[Nafees](https://github.com/mnafees) joined our team as founding engineers! They continue to set an incredibly high standard for code, communication and kindness, and I’m really excited to continue working with them for the next year.
- We shipped some things:


- We[launched Hatchet v1](https://news.ycombinator.com/item?id=43572733) earlier this year, which featured:


- Feature parity with durable execution engines
- Significant performance improvements
- New SDKs for Python, Typescript, and Go
- [Conditional triggering](https://docs.hatchet.run/home/conditional-workflows)


- We shipped[a Terraform provider](https://registry.terraform.io/providers/hatchet-dev/hatchet/latest) for managing Hatchet tenants and organizations
- We recently released an overhaul of the Hatchet frontend with improvements across almost every view
- We built[Hatchet webhooks](https://docs.hatchet.run/home/webhooks) , an easy way to add Stripe, Github, Slack or generic webhooks to trigger workflows and tasks
- We’re now publishing weekly on[The Hatchet Times](https://hatchet.run/blog)
- [Priority tasks](https://docs.hatchet.run/home/priority)
- … and over 1k commits of bug fixes, performance improvements, and smaller features!


- We grew revenue on our cloud platform by 9×
- We saw two major open-source projects built with Hatchet:


- An[unofficial Rust SDK](https://github.com/eswolinsky3241/hatchet-rust-sdk) built by[eswolinsky](https://github.com/eswolinsky3241)
- [Scythe](https://github.com/szvsw/scythe) , a project for running heavily parallel experiments built by[Sam Wolk](https://github.com/szvsw)


- We had our first team offsite in Stockholm, Sweden, which coincided with the fantastic PyCon Sweden, where Gabe and Matt gave a workshop on Hatchet:


Matt and Gabe, pictured here regretting not providing a standard Python dev env for our workshop guests


*Here’s to 2026!*
