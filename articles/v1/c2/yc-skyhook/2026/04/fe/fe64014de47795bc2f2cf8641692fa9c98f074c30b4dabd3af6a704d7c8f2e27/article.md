---
schema_version: "1.0.0"
document_id: "fe64014de47795bc2f2cf8641692fa9c98f074c30b4dabd3af6a704d7c8f2e27"
company_key: "yc-skyhook"
company: "Skyhook"
source_id: "yc-skyhook-news-import-0a5e972b70e5"
canonical_url: "https://www.skyhook.io/blog/when-not-to-build-an-internal-developer-platform"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:39:47.906327+00:00"
fetched_at: "2026-07-28T22:16:19.189952+00:00"
content_hash: "sha256:cd3901d2d28648cf32e710ce7a85d5201cff56dde21d31b966dab35e6856f6cc"
---

# Backstage Fatigue: When NOT to Build an Internal Developer Platform

TL;DR: Building an internal developer platform from scratch used to be the only option, and it only paid back for the largest orgs that could afford it. Self-hosted Backstage runs $450K-$800K in year one, takes 6-12 months to be useful, and stalls at around 10% adoption outside Spotify. The Gartner "80% by 2026" stat is about *large* orgs, but even there, teams that operate as federations of smaller squads land in the same trap when they don't actually have bespoke needs. This post is the honest version: when building still makes sense, when it doesn't, and the pre-built path that actually delivers what teams want. The short answer for most teams:[buy, don't build](https://www.skyhook.io/blog/why-pre-built-internal-developer-platforms) .


## The Stat Everyone Quotes Wrong


The[Gartner forecast](https://www.gartner.com/en/infrastructure-and-it-operations-leaders/topics/platform-engineering) that started this whole conversation says, verbatim: "By 2026, 80% of **large** software engineering organizations will establish platform engineering teams as internal providers of reusable services, components and tools, up from 45% in 2022."


That word "large" does most of the work, but the size axis is also misleading on its own. Plenty of 500-engineer orgs run as a federation of four mostly-independent squads with their own stacks, and the build-vs-buy math inside each squad looks like a 100-engineer team's, not a 500-engineer team's. Conversely, in a 30-person engineering org with one product squad and one cluster, the math is often inverted - building looks attractive on a slide and breaks under operating it. And in a 5-person team, building an IDP is the same conversation as building your own database: technically possible, almost certainly the wrong call. The real question is not size; it is whether your needs are bespoke enough to justify the build cost when a pre-built option exists.


But the stat keeps showing up in board decks at companies it was never about, and the conclusion gets reverse-engineered from there: we are an organization, 80% of organizations have one, therefore we should have one. The[DORA 2024 report](https://cloud.google.com/blog/products/devops-sre/announcing-the-2024-dora-report) , which actually surveyed practitioners rather than predicting CIO behavior, found something less flattering: orgs that increased their platform engineering focus saw throughput drop by 8% and change stability drop by 14%. The same orgs reported productivity gains of 8% individual / 10% team, so the picture is "developers feel more productive, but the team ships slower and less reliably." DORA's hypothesis was that IDPs introduce handoffs that previously did not exist.


That is not a verdict on platform engineering. It is a verdict on doing it before you need it.


## What Backstage Actually Costs


Backstage is the default suspect when this conversation comes up because it is the only widely-adopted open source IDP. The Spotify-adjacent numbers are sobering.


Cost line Realistic range


Time to first useful state 6-12 months


Dedicated engineers to maintain 3+


Year-one cost (loaded salaries) $450K - $800K


External adoption rate ~10% (vs Spotify's internal 99%)


Those numbers come from vendors selling alternatives (Roadie, Cortex, Port), so treat them as directional rather than gospel. But the time-to-value range is consistent across enough independent sources that I would trust the lower bound. And the adoption number is the one that should haunt anyone considering this:[Helen Greul](https://thenewstack.io/spotifys-backstage-roadmap-aims-to-speed-up-adoption/) , head of engineering for Backstage at Spotify, has said publicly that average external Backstage adoption stalls at around 10% of engineers because most adopters never get past the proof-of-concept phase. Spotify's own response was to launch Spotify Portal for Backstage, a hosted edition. When the canonical reference customer concludes that self-hosting is broken for everyone else, that should land somewhere.


This is not a Backstage-is-bad argument. Backstage works exactly as advertised when you have the engineering surface area to amortize the cost. Below somewhere around 150 engineers, you do not.


## Platforms Before Portals


Octopus has a piece called "Platforms Before Portals" that frames this better than I can, and GitLab made the same case in "Beyond the portal hype." The argument: a portal is navigation. If there is nothing to navigate to, the portal is theater. You are pointing developers at a beautiful index of services they could deploy to, except that the deploy experience itself is still "open a ticket with DevOps."


The right order is:


1. **Paved path for deployment.** A self-serve way to ship a service from commit to production without filing a ticket.
2. **Paved path for environment.** A self-serve way to spin up a preview environment, get a database, get secrets.
3. **Paved path for observability.** Logs, metrics, and traces that are wired up by default, not as a follow-up ticket.
4. **Then a portal.** Once the underlying services are real, the portal becomes useful as a navigation layer.


Most teams I talk to skip 1-3 and start at 4 because Backstage has a marketing site and the others do not. They end up with a beautiful portal pointing at the same broken deploy experience. Six months in, the portal is stale because nobody updates the catalog, and the team that built it has moved on.


Charity Majors put this more bluntly in her[QCon NYC 2023 talk on platform engineering pitfalls](https://www.infoq.com/presentations/platform-engineering-teams/) : "If your platform team spends a lot of time writing software, something's probably wrong." That was Pitfall #2 of eight. The point is not that platform teams should not write code. The point is that if they are writing it from scratch instead of integrating things that already exist, you are paying twice for the same outcome.


## When an IDP Genuinely Makes Sense


There is a real threshold. From what I have seen across customer conversations and what shows up in the public data, dedicated internal platforms start to pay back somewhere around **~150 engineers** , with some accelerants:


- **More than 3 product groups with conflicting infrastructure preferences.** When Group A wants Postgres on RDS and Group B wants Postgres in-cluster and Group C wants CockroachDB, a platform layer that says "here is Postgres, you do not get to choose" is genuinely valuable.
- **Multi-cloud or multi-region by requirement, not preference.** If you have to deploy to AWS and GCP because of a customer contract, abstracting that away has obvious leverage.
- **Compliance scope that affects every service.** SOC 2, HIPAA, PCI - any of these benefit from "the platform enforces it" rather than "every team has to remember."
- **A platform team that exists to maintain it.** Not three SREs you renamed. A real team with a product manager and a roadmap.


If you tick at least two of these and you are above 150 engineers, build the platform. Backstage may even be the right base. If you tick zero of these and you are anywhere under that line - whether you are five engineers or eighty - do something else.


## The Wrong-Reason Signals


The conversations that end badly tend to start with one of these:


- **"We are getting enterprise-ready."** Enterprise readiness is SOC 2, audit logs, and an SSO story. It is not a developer portal.
- **"Every modern company has one."** Not at your size, they do not.
- **"Our VP of Eng read a Gartner report."** See above.
- **"We need to attract platform engineers."** Hiring a team to build a platform so you can hire more people for the team is recursive in a way that should set off alarms.
- **"It is on our 2026 OKRs."** OKRs are not requirements. Push back.


The honest reason to build an IDP is that developers cannot deploy without a ticket and you have measured the cost of that and decided a platform is the cheapest path to fix it. If that is not the reason, do not start.


## What Most Teams Actually Need


Most of the value people expect from a Backstage build is achievable from a pre-built platform plus a couple of small tools. The list below applies whether you are 5 engineers, 100, or 500 split across federated squads - the difference is which row hurts first:


What developers actually want How to deliver it without building one


Deploy without filing a ticket Opinionated deploy platform with a UI and CLI


Preview environment per PR Argo Rollouts or similar, automated from CI


Find services and their owners A spreadsheet, then a` services.yaml` in a known repo, then maybe a portal once it hurts


Self-serve Postgres / Redis / S3 Crossplane or a Terraform module library with a` plan` step in CI


Documentation that is not stale Mintlify, GitBook, or just a` docs/` folder with a search bar


Service catalog The README of your monorepo, until that hurts


The pattern is the same one Kelsey Hightower has been making for a decade: do not build the abstraction until the thing you are abstracting is real and painful enough to justify it. Pick opinionated tools that already do most of what you want. Tolerate the spreadsheet. Resist the urge to build. If you do decide you need one coherent platform across all of this,[buy a pre-built one rather than start a Backstage project](https://www.skyhook.io/blog/why-pre-built-internal-developer-platforms) .


## The Inverse Argument


Here is the contrarian frame: for most teams under ~150 engineers, the deployment platform IS the developer experience. There is no separate portal to build because the thing developers interact with every day is the deploy experience itself. If that experience is good, you have an IDP. It just does not have a homepage.


This is the argument behind opinionated PaaS-style tools like Skyhook, Render, Northflank, Porter, Fly, and Railway. None of us market against Backstage directly, but the implicit pitch is the same: the platform is the product. You do not need to build it. Most of these vendors say it through their product surface rather than head-on. What is missing from that conversation is a numerical threshold for *when the math flips* and a checklist for what to actually build when it does. That is what the rest of this post is for.


For Skyhook specifically, this is why we ship things like preview environments per PR, golden paths, deploy strategies, and a service catalog as defaults rather than as plugins you have to assemble. A three-engineer team gets the IDP outcome on day one without ever owning a platform. A growing 80-engineer team gets the same baseline but with the room to extend it as their needs sharpen. And when an organization eventually crosses into "we genuinely need a custom platform" territory, you still have not spent engineer-quarters building the wrong thing - you have a working production baseline to either extend or replace.


## The Question to Actually Ask


Stop asking "should we build an internal developer platform?" Ask:


> What is the specific developer pain that an IDP would solve, and is building one cheaper than buying or simplifying the thing causing the pain?


If the answer is "deploys are slow," fix deploys. If the answer is "nobody knows who owns what," start with a markdown file. If the answer is "we want to look modern," that is not an answer.


The companies winning at developer experience right now are not the ones with the prettiest Backstage instance. They are the ones who shipped fewer abstractions and let developers actually deploy software.


## Further Reading


- [Golden Paths and the 80/20 Rule](https://www.skyhook.io/blog/golden-paths-and-the-80-20-rule) - The opinionated-defaults argument from the inside: how to pick the 20% of paths that cover 80% of work without building a portal.
- [Self-Serve Platforms and Service Catalogs](https://www.skyhook.io/blog/self-serve-platforms-and-service-catalogs) - The narrower problem an IDP is usually trying to solve, and what minimum viable looks like.
- [The Rise of Platform Engineering](https://www.skyhook.io/blog/the-rise-of-platform-engineering-redefining-devops-for-the-modern-era) - Companion piece on when a platform team starts to make sense. This post is the contrarian counterweight to that one.
- [Octopus Deploy: Platforms Before Portals](https://octopus.com/blog/platforms-before-portals) and[GitLab: Beyond the Portal Hype](https://about.gitlab.com/the-source/platform/beyond-the-portal-hype-why-you-need-a-platform-first/) - Primary sources for the platforms-first argument.
