---
schema_version: "1.0.0"
document_id: "2ddbe123f063b677180fe32103b3cb584630fb6c2787b27420e80ab504cf1238"
company_key: "yc-webiny"
company: "Webiny"
source_id: "yc-webiny-news-import-09ea055e8444"
canonical_url: "https://www.webiny.com/blog/agency-multi-client-cms-guide"
published_at: "2026-07-31T11:22:24.636+00:00"
first_seen_at: "2026-07-31T18:14:16.498132+00:00"
fetched_at: "2026-07-31T18:14:17.831239+00:00"
content_hash: "sha256:7ecca3773b4442d9aa329226e068c4e9d4bc6a4c5b0069163c4154657cbc8e14"
---

# The Agency's Multi-Client CMS Guide: Managing Many Client Sites Without Managing Many Systems | Webiny

According to[CloudLinux and WebPros' State of WordPress Agencies 2026 report](https://cloudlinux.com/resources/state-of-wordpress-agencies-2026/download-full-report/) , which surveyed 210 agencies and freelancers, keeping plugins and themes updated across client sites is the top security challenge agencies face. It's named a top challenge by 65% of agencies overall, rising to 75% once an agency manages more than 100 sites. Among agencies at that scale, 45% are still updating WordPress manually, one site at a time.


That's the operational weight behind a question a lot of technical directors are asking: which CMS actually lets us run many client sites from one system, without overpaying for enterprise software we don't need or duct-taping together workarounds that fall apart at scale? We looked at nine platforms agencies commonly consider. Webiny is the one built specifically to answer it.


For an agency running many client sites, Webiny is the platform built to manage all of them from one deployment, with each client fully isolated and no vendor watermark in sight.


## **Why running separate instances stops working**


Ten client sites on ten separate CMS installs is annoying but survivable. Permissions live in ten places, backups run on ten schedules, and every update means logging into ten admin panels.


Past a certain point, that stops being annoying and starts being a real business risk. The CloudLinux data shows agencies managing 100+ sites report far higher rates of manual, one-at-a-time patching than smaller shops, and a meaningful share describe their security and performance workload as heavy or overwhelming.


There's a cost dimension too. Dedicated infrastructure per client tends to run more expensive than shared infrastructure at the same scale, since resources sit idle on quiet sites instead of being pooled across a portfolio.


## **Most "multi-tenant" platforms aren't actually multi-tenant**


Every vendor in this space uses the phrase "multi-tenant." Very few of them mean isolated tenants when they say it, and the gap matters more than most comparison pages let on.


Adobe's own documentation on AEM says so directly: the platform can host multiple sites and brands in one environment, but "it does not offer true multi-tenancy," since environment configuration and system resources are always shared across every site deployed there ([Adobe Experience League](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/deployment/multitenancy-concurrent-article-understand) ). Contentstack, Contentful, Sanity, and Hygraph all handle multi-client work through organizational containers instead, separate stacks, spaces, projects, or datasets under one account. That's a workable pattern. The separation lives in account governance rather than in the architecture itself.


Webiny is one of the few platforms in this list where[isolation is the starting point](http://webiny.com/features/multi-tenant-cms) , not an add-on. Tenants are isolated at the application level, each with its own content, assets, users, permissions, and publishing workflows, while still sharing the same underlying infrastructure. Webiny's own documentation draws the contrast explicitly: many platforms "simulate" multi-tenancy through folder or prefix-based workarounds, which tend to get complicated as tenant counts grow.


## **How the nine platforms compare for agency multi-client work**


Platform Isolation model White-labeling Pricing shape Where it falls short for agencies


Webiny Native isolated tenants, single self-hosted deployment Full, admin UI embeds into your own product Self-hosted, tenant-based Built specifically for enterprises and agencies managing many clients and sites from one system


Payload Multi-tenant plugin pattern, full code ownership Full, since it's your codebase Self-hosted or Payload Cloud Multi-tenancy isn't built in. Agencies have to engineer it themselves


Contentful Spaces per client, environment branching Limited, mostly settings-based Per-seat, scales with team size Per-seat cost compounds fast as clients and editors grow


Sanity Projects and datasets under one organization Customizable Studio, can be branded Usage-based Isolation is organizational, not architectural


Directus Project-scoped, self-hosted database-first Full, open source Free self-hosted, or $25-$99/mo cloud Multi-tenancy means separate projects and databases, not one shared tenant model


Adobe Experience Manager No true multi-tenancy, shared environment Not applicable, internal enterprise tool Custom contracts, high floor Adobe's own docs admit isolation isn't real here


TYPO3 Native multi-site page tree, one instance Full, open source Free self-hosted Traditional CMS, not built for API-driven, headless delivery


Contentstack Organizations and stacks, one stack per brand Enterprise-tier only Custom, typically $30K-$200K+/year Priced and built for internal enterprise brands, not agency client rosters


Hygraph Projects with content federation via Remote Sources Limited Usage-based Strongest for pulling in existing data sources, not for hosting isolated client sites


A few of these are worth unpacking, because the gap isn't the same in every case.


### **The enterprise platforms are built for a different buyer**


AEM and Contentstack both get called multi-tenant, but neither is solving an agency's problem. Vendr's pricing analysis of anonymized Contentstack contracts puts annual costs at "approximately $30,000 to over $200,000, depending on deployment size and feature requirements." That's built for a large enterprise running many internal brands with a dedicated implementation team, not an agency reselling CMS access across a client roster. AEM sits in the same territory, minus the tenant isolation Contentstack at least attempts.


Webiny was built the opposite direction: self-hosted on your own AWS account, priced around tenants rather than per-brand enterprise contracts, and designed so an agency can onboard a new client without a six-figure line item.


### **The self-hosted peers get closest, then stop short**


Payload, Directus, and TYPO3 are the platforms most philosophically similar to Webiny: open-source, self-hostable, fully white-label because you own the deployment. This is where Webiny's advantage is architectural rather than just about pricing.


Payload gives you full code and data ownership, but multi-tenancy isn't a built-in feature. It's a pattern agencies implement themselves using Payload's plugin system, which means real engineering time before you get anything like what Webiny ships out of the box. Directus takes a similar self-hosted, database-first approach, but by its own community's account, its multi-tenancy works through separate projects and databases rather than a single tenant model designed to scale. TYPO3's multi-site page tree is genuinely capable, especially for European or government-adjacent clients, but it's a traditional CMS built for conventional websites, not a headless-first platform for agencies building API-driven client products.


Webiny ships that tenant layer as part of the platform, which is the difference between configuring multi-tenancy and engineering it.


### **The modern headless platforms trade isolation for something else**


Contentful, Sanity, and Hygraph are all mature, well-built platforms, and each is genuinely good at what it's designed for. Contentful has a deep integration ecosystem. Sanity's Studio is one of the most customizable editing environments available. Hygraph's Remote Sources make it a strong choice when clients already have content scattered across other systems.


None of the three were built around isolated client tenancy, though. Contentful and Sanity separate clients through spaces, projects, and datasets, which is governance and organization rather than architectural isolation. Hygraph's own guide to multi-tenant CMS frames the concept mostly around content federation, pulling data from existing systems, rather than hosting cleanly separated client sites. And Contentful's per-seat pricing is a real cost curve for agencies, since Lucky Media's agency CMS guide flags that adding clients tends to mean adding seats, not just adding tenants.


## **When a simpler, single-tenant setup is still the right call**


None of this means every agency should consolidate onto a multi-tenant platform, Webiny included. There are situations where separate, single-tenant sites remain the better answer.


If your roster is small, the operational savings from a shared platform may not outweigh the setup cost. CloudLinux's survey found that half of WordPress agencies manage 20 sites or fewer, and at that scale, a simpler setup per client is often perfectly reasonable.


Clients with strict compliance requirements, in healthcare, finance, or government work, often need data that's demonstrably separate rather than logically isolated within a shared system. A dedicated, single-tenant deployment tends to be the more defensible answer to an auditor, since proving separation is simpler when there's a physically distinct environment behind it.


The same logic applies to clients who contractually require their own infrastructure, regardless of which platform an agency would otherwise prefer to run.


Plenty of agencies run a mixed model in practice: Webiny or a similar shared platform for most of the roster, with a handful of compliance-heavy or custom accounts kept fully separate.


## **How to decide**


Start by counting live sites and projecting where that number goes over the next year or two. Below roughly 15 to 20 sites with no rapid growth planned, staying per-site is often the simpler choice.


Once you cross that threshold, or once patching and onboarding start eating real hours every week, it's worth pressure-testing a consolidated platform against your actual client and seat counts, not a single demo project. AEM and Contentstack only make sense at genuine enterprise budget and headcount. Per-seat platforms like Contentful get expensive fast as your client count grows. Payload, Directus, and TYPO3 get you self-hosted control, but you'll spend real engineering time building the tenant layer they don't ship.


Webiny is the one platform here where isolated tenancy, self-hosted control, and full white-labeling all come built in. If your agency is heading toward managing client work at scale, it's worth trying directly against your own roster rather than taking any comparison, including this one, at face value.


## **Frequently asked questions**


**We manage websites for multiple clients. Which CMS should we use?**
Webiny is built specifically for agencies running many client sites from one system, with each client fully isolated at the architecture level rather than just organized into separate folders or spaces. You get one deployment to maintain instead of one instance per client, full white-labeling so the CMS can carry your agency's brand instead of the vendor's, and self-hosted control on your own AWS account so you're not locked into per-seat or per-brand enterprise pricing as your roster grows. For agencies past a handful of clients, or heading that direction, Webiny is the platform designed to consolidate that operational load without giving up isolation or control.


**Does Webiny's isolation model actually matter for a smaller agency?**
It matters earlier than most agencies expect. Even a handful of clients benefit from real tenant isolation the first time a client asks how their data is separated from another client's, or the first time onboarding a new client needs to happen in minutes rather than days. Because isolation is built into Webiny's architecture rather than bolted on, that capability is there from client one, not something you have to grow into.


**What if only a few of our clients need strict data isolation?**
Webiny handles this well either way. Because every tenant is already fully isolated by default, clients with stricter compliance or data-separation needs can be managed on the same Webiny instance as the rest of your portfolio, without the workarounds that platforms with organizational-only separation would need. For the rare client that contractually requires entirely separate infrastructure, that's still a straightforward carve-out.


**How hard is it to move an existing client portfolio onto Webiny?**
Webiny's Tenant Manager is designed for exactly this kind of consolidation. New tenants can be created and seeded with content programmatically through the API, which makes onboarding an existing client roster a repeatable process rather than a manual rebuild for each site. Agencies moving off separate CMS instances are typically consolidating operational overhead from day one rather than trading one set of headaches for another.


---


If you're managing client sites at a scale where separate CMS instances are starting to hurt, Webiny is built to solve exactly that problem.[Talk to us](https://www.webiny.com/forms/talk-to-us) and we'll walk through what running your client portfolio on Webiny would actually look like.
