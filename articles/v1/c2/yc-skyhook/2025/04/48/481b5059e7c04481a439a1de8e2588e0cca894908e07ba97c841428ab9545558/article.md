---
schema_version: "1.0.0"
document_id: "481b5059e7c04481a439a1de8e2588e0cca894908e07ba97c841428ab9545558"
company_key: "yc-skyhook"
company: "Skyhook"
source_id: "yc-skyhook-news-import-0a5e972b70e5"
canonical_url: "https://www.skyhook.io/blog/why-pre-built-internal-developer-platforms"
published_at: "2025-04-01T00:00:00+00:00"
first_seen_at: "2026-08-11T12:26:27.483932+00:00"
fetched_at: "2026-08-11T12:26:28.752523+00:00"
content_hash: "sha256:281dada8754d3d9868b8a15c29c2092077671f0e79e67fb9264b37e6d1b0e8c4"
---

# Buy, Don't Build: The Case for Pre-Built Internal Developer Platforms

TL;DR: Once you have decided you actually need an internal developer platform, the build-vs-buy question almost always resolves to buy. A self-built Backstage runs $450K-$800K in year one and stalls at ~10% adoption outside Spotify. A pre-built platform replaces 80% of that work for a fraction of the cost. The remaining tradeoffs are real but small. This post is what to evaluate and which flavor to pick.


## First, Make Sure You Need One


Most teams that say "we need an IDP" don't actually mean it. They mean "deploys hurt, nobody knows who owns what, and onboarding takes too long." Those are real problems, but the cheapest fix is rarely a developer portal.


We covered the threshold question in[Backstage Fatigue: When NOT to Build an Internal Developer Platform](https://www.skyhook.io/blog/when-not-to-build-an-internal-developer-platform) . Short version: under ~150 engineers with one or two product groups, what you usually need is an opinionated deploy platform, not a portal. If you read that and concluded you do need a coherent platform, the next question is build vs buy. This post is about that decision.


## What "Build" Actually Costs


Backstage is the open source default, so most "build" plans turn into "stand up self-hosted Backstage." The numbers below come from vendors selling alternatives - treat them as directional, not gospel.


Cost line Realistic range


Time to first useful state 6-12 months


Dedicated platform engineers 3+


Year-one fully-loaded cost $450K - $800K


Median external adoption rate ~10% (vs Spotify's internal 99%)


The adoption number is the one that should haunt anyone considering self-hosting.[Helen Greul](https://thenewstack.io/spotifys-backstage-roadmap-aims-to-speed-up-adoption/) , head of engineering for Backstage at Spotify, has said publicly that average external adoption stalls around 10% because most adopters don't make it past proof-of-concept. Spotify's own response was to launch Spotify Portal for Backstage - a hosted edition. When the canonical reference customer concludes the self-hosted experience is broken for everyone else, that should land somewhere.


The non-cash cost is arguably worse. Every quarter a platform engineer spends on Backstage plugins is a quarter they didn't spend on something that moves your product. Three years in, you have a portal that 10% of engineers use and 100% of leadership has to keep funding because too much was spent to walk away.


## What "Buy" Actually Delivers


Pre-built platforms come in two flavors. The right one depends on what hurts.


**Hosted Backstage / portal-first.** Roadie, Cortex, Port, Spotify Portal. You get the Backstage UX without owning the chassis. Plugins, software catalog, scorecards, scorecard automation - on day one. You pay per developer.


**Opinionated PaaS / platform-first.** Skyhook, Render, Northflank, Porter, Fly, Railway. You get the *outcome* an IDP is supposed to produce - one-click deploys, preview environments per PR, golden paths, secrets, observability - without a portal layer. The deploy experience itself is the developer experience.


The naming around this category is intentionally fuzzy. Port markets itself as "Internal Developer Portal & Platform." Cortex calls itself both a "portal" and an "Engineering Operations Platform." Roadie is the most honestly named of the three - "hosted Backstage" - but the whole category often gets discussed as if portal-first and platform-first tools are interchangeable. They are not. None of the portal-first tools ship your code. They describe code that is already shipping: service catalogs, ownership, scorecards, dashboards. That is real value when your services already deploy reliably. It is not a substitute for the deploy experience itself. Octopus called this["Platforms Before Portals"](https://octopus.com/blog/platforms-before-portals) for a reason: if the underlying deploy experience is the problem, a portal layered on top of it will not fix it.


If you are 80 engineers with seven services and your real problem is "deploys are still tickets," portal-first won't fix that and platform-first will. If you are 150 engineers across three product groups, the deploy experience is already self-service, and your real problem is "nobody can find anything and ownership is a mess," portal-first is the right shape - but only because the platform underneath is already there.


In either flavor, what you skip is everything that doesn't differentiate you: SSO/SCIM, RBAC, audit logging, the Kubernetes operators, the upgrade treadmill, the on-call rotation for the platform itself. That is the entire 80% that DIY Backstage spends two years rebuilding.


## The Honest Tradeoffs


Buying isn't free in non-cash terms. Three real tradeoffs:


**Less customization at the seams.** A pre-built platform has opinions. If your deploy flow has a gnarly compliance step that doesn't fit those opinions, you'll either bend your flow or use a vendor escape hatch. Most teams find this less painful than they feared, but walk through your weirdest workflow with the vendor before signing.


**Vendor dependency.** You are now coupled to a roadmap that isn't yours. Mitigate by picking platforms that commit changes to *your* GitOps repo, so you can leave with your manifests intact, and that are open about their APIs.


**Pricing scales with engineers, not value.** Per-developer pricing means a 200-engineer org pays roughly 10x what a 20-engineer org pays for the same platform, even if the larger org gets less marginal value because it has more in-house ops capacity. At a certain scale - Stripe, Shopify, Spotify - self-hosting Backstage actually does pencil out. That scale is much higher than most readers of this post.


What you do *not* trade away by buying: extensibility (good vendors expose escape hatches), reliability (you skip the "your platform team is on call for the platform" trap), or technical depth (the pre-built ones are mostly built by people who watched DIY fail at their last company).


## What to Evaluate


If you decide to buy, look for these in order:


1. **Does it solve your specific painful workflow today?** Walk through your three worst deploy scenarios with the vendor live. If they hand-wave on any of them, walk away.
2. **Is the configuration in your Git repo?** If the platform is "stateful in the vendor's database," your migration cost is locked in. Manifests-in-your-repo is the structural escape hatch.
3. **Does it cover both deploys and the boring stuff?** Preview environments per PR, secrets, observability, RBAC, SSO. If any are "on the roadmap," that's a six-month gap you'll fill yourself.
4. **What's the upgrade path when your needs sharpen?** A platform that handles you at 30 engineers but breaks at 150 is one you'll outgrow at exactly the wrong moment.
5. **Real customers in your shape.** Not logos at the top of the website - actual customers with the same engineer count and stack as you. Ask for two on a call.


## Where Skyhook Fits


Skyhook is in the platform-first flavor. The thesis is that for teams under ~150 engineers, the deployment platform *is* the IDP - so we ship preview environments per PR, golden paths per language, deploy strategies, secrets, and a service catalog as defaults rather than as plugins you assemble. Configuration commits to your Git repos, so the escape hatch is structural. We are not the right fit if your real problem is "we need a beautiful Backstage portal for 250 engineers across five product groups." For most teams under that threshold, we are the version of "buy" that actually replaces the IDP project.


If you are earlier in the decision and not sure you even need an IDP yet, start with[Backstage Fatigue](https://www.skyhook.io/blog/when-not-to-build-an-internal-developer-platform) . If you have decided you do, the build-vs-buy answer is almost always buy.


## Further Reading


- [Backstage Fatigue: When NOT to Build an Internal Developer Platform](https://www.skyhook.io/blog/when-not-to-build-an-internal-developer-platform) - The companion piece. Read this first if you are not sure you need an IDP at all.
- [Golden Paths and the 80/20 Rule](https://www.skyhook.io/blog/golden-paths-and-the-80-20-rule) - Why a small set of opinionated paths covers most of what an IDP is supposed to do.
- [Self-Serve Platforms and Service Catalogs](https://www.skyhook.io/blog/self-serve-platforms-and-service-catalogs) - The narrower problem most "we need an IDP" conversations are actually about.
- [Helen Greul on Backstage adoption (The New Stack)](https://thenewstack.io/spotifys-backstage-roadmap-aims-to-speed-up-adoption/) - Primary source for the 10% adoption figure.
