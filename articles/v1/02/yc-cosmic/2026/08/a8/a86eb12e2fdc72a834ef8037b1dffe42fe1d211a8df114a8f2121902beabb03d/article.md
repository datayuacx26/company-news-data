---
schema_version: "1.0.0"
document_id: "a86eb12e2fdc72a834ef8037b1dffe42fe1d211a8df114a8f2121902beabb03d"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/contentful-vs-storyblok"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-07T19:31:51.633599+00:00"
fetched_at: "2026-08-07T19:31:54.584119+00:00"
content_hash: "sha256:49ac1fb3854c27af2b80f102ec3cf5c44814a1b3d663658fbfd5f54c70bb25b2"
---

# Contentful vs Storyblok: Which Headless CMS Should You Choose in 2026?

Contentful and Storyblok get shortlisted together constantly, usually by a team that has outgrown a monolithic CMS and wants something API-first that marketing can still operate without filing a ticket. Both are mature, both are used in production at scale, and both will technically work for most projects. The differences that matter show up in how you model content, who can edit it comfortably, and what the bill looks like in year two.


Contentful started in Berlin in 2013 and grew up inside enterprise content operations. Storyblok started in Austria in 2017 and built its reputation on a visual editor that non-developers actually enjoy using. That origin gap still explains most of what follows.


Full disclosure: we build[Cosmic](https://www.cosmicjs.com/) , a competing headless CMS. Everything before the Cosmic section is written to be useful even if you never sign up with us. Where numbers appear, they were checked against each vendor's public pricing page or official developer documentation on August 6 and 7, 2026, and we date every claim.


## Quick verdict


If this is you Pick


Large content org, many locales, strict roles and approval workflows, procurement already knows the vendor Contentful


Marketing site or campaign-heavy content where editors need to see the page while they edit it Storyblok


Small team that wants transparent pricing and a fast REST API without an enterprise contract Look at a lighter alternative, including us


## Content modeling: entities versus page trees


**Contentful** models discrete content entities. You define content types with typed fields, link entries to each other with references, and organize everything into spaces. Environments and environment aliases let you branch a schema, test a migration, and swap it into production without downtime. There is a migration CLI for scripted schema changes, which matters once more than one developer touches the model.


One hard ceiling to know before you design anything: Contentful documents a limit of 50 fields per content type, and 5,000,000 records per space. The field cap is the one teams hit first, usually on a sprawling "page" type that has been absorbing one-off fields for two years.


**Storyblok** models pages as trees. A story holds nestable components (Storyblok calls them blocks), and a page is a composition of those blocks serialized as JSON. Editors add, reorder, and nest blocks to build layouts.


The practical consequence: if your content feeds many surfaces (web, mobile app, in-store screen, partner API), an entity-first model like Contentful's travels better because the content is not shaped like a web page. If your primary surface is a marketing site with dozens of layout variations, Storyblok's block composition gets you there faster and with fewer developer requests.


A useful test before you commit. Take your five most complex existing pages and model them in a trial account of each. You will learn more in two hours of modeling than in two weeks of reading feature matrices.


## Editing experience


This is Storyblok's strongest argument. Its visual editor renders your real front end in a side-by-side preview, and clicking an element on the page opens the fields that control it. Editors get spatial context instead of a stack of form inputs. The cost is setup: you wire the Storyblok bridge into your front end and expose a preview route, and every new component needs to be registered so it appears in the editor.


Contentful's editing model is form-first. Recent years have added page-building and live preview tooling, though the richer experience layers have historically been sold as add-ons or bundled into higher tiers. For an editor who works in structured chunks (a product spec, a press release, a legal disclosure reused in nine places), forms are fine and often faster. For an editor who thinks in pages and campaigns, forms feel like filling out a tax return.


Be honest about who your editors are. Teams routinely buy the enterprise-grade option and then discover marketing still asks a developer to move a section.


## Developer experience and APIs


**Contentful** exposes a Content Delivery API, a Preview API, and a Content Management API, along with a GraphQL Content API and SDKs across most major languages. The App Framework lets you extend the editor UI with custom apps and field editors. Webhooks cover the usual automation paths. Environments are the standout developer feature: safe schema iteration is a real operational advantage on a big team.


Rate limits are documented and they differ by plan. Contentful's docs list Content Delivery API calls at 55 per second per space on Free plans and 78 per second on paid plans, with the Management API at 7 and 10 per second respectively. Those are per space, which matters if you were planning to consolidate several brands into one.


**Storyblok** exposes a REST Content Delivery API with draft and published versions, a Management API, framework starters, field-type plugins, and webhooks. One detail worth knowing before you scope anything: Storyblok's GraphQL Content Delivery API is listed under its Premium and Elite plans, so if a GraphQL layer is central to your architecture, you are planning an enterprise contract rather than a self-serve one.


Both ship a CDN-backed delivery API, both support scheduled publishing, and both have healthy framework integrations for Next.js, Nuxt, Astro, and SvelteKit. Neither will bottleneck a normal site on raw delivery speed.


## Pricing structure


Pricing is where these two feel most different, and it is the section most comparison posts get wrong by quoting stale numbers. Here is exactly what we could and could not verify.


**Storyblok publishes its self-serve tiers.** Re-verified against Storyblok's pricing page on August 7, 2026:


Storyblok plan Monthly price Seats included Seat ceiling


Starter $0 1 2


Growth $99/mo 5 10


Growth Plus $349/mo 15 20


Premium and Elite Quoted by sales Custom Custom


Additional seats are $15 per month each. Annual billing discounts the effective monthly rate, bringing Growth to $90.75 and Growth Plus to $319.91. The details that catch teams off guard:


- Self-serve plans include one space. A second brand, a second environment, or a second product line generally means a second subscription.
- Traffic, API requests, locales, assets, and stories are metered, with published overage rates once you pass the included allowance.
- The uptime SLA on Growth and Growth Plus is 97%. It rises to 99.9% on Premium and 99.99% on Elite. If you need a contractual availability number, that is a sales conversation.
- Enterprise-flavored features (SSO, SCIM, custom roles and approval workflows, releases, A/B testing, GraphQL) sit on Premium and Elite.


**Contentful does not publish a monthly figure we can verify.** Its pricing page renders plan details client-side and its upper tiers are negotiated through sales, so we are not going to print a dollar amount we cannot stand behind. Confirm current pricing with Contentful directly.


What *is* published, and what actually determines which tier you land on, is the limits table in Contentful's own developer documentation. As of August 6, 2026 it draws the line in three places: Free, Lite, and paid plans above Lite.


Documented limit Free Lite Paid above Lite


Spaces per organization 1 2 750


Webhooks per space 20 20 100


API keys per organization 100 100 200


Max asset size 50 MB 1,000 MB 1,000 MB


App installations per environment 10 50 50


CDA calls per second, per space 55 78 78


Source:[Contentful technical limits](https://www.contentful.com/developers/docs/technical-limits/) .


The consequential row is spaces. Free is capped at one space per organization and Lite at two, so a second brand, a genuinely isolated second project, or a sandbox you do not want touching production pushes you into the tier where pricing goes through sales. Environment aliases follow the same pattern: you get one (master) unless your plan includes custom aliases, which unlocks three.


Storyblok has the same shape of constraint from the other direction, since its self-serve plans include a single space. Budget for it in both cases.


Whichever way you go, price the same five dimensions and ignore the headline number: seats, API calls or traffic, locales, spaces or environments, and total records. Those five drive nearly every surprise renewal.


## Migration and lock-in


Both platforms can be exported. Contentful's Content Management API and CLI produce clean entry and asset dumps, and the reference model maps well onto other systems. Storyblok's story JSON is portable, though the nested block tree maps tightly to your component library, so the export is only as reusable as your components are.


Most of the switching cost sits outside the content itself. Budget for the integrations, the preview wiring, the webhook consumers, and the front-end code that assumes a particular response shape. If a migration is even remotely plausible for you within three years, keep your data access behind a thin internal module rather than calling the vendor SDK from fifty components. That one decision turns a quarter-long project into a two-week project.


We have written up both paths in detail:[migrating from Storyblok](https://www.cosmicjs.com/blog/migrate-from-storyblok-to-cosmic) and[migrating from Contentful](https://www.cosmicjs.com/blog/contentful-to-cosmic-migration) .


## Where Cosmic fits


If you are comparing these two because you want an API-first CMS without an enterprise procurement cycle, we are worth ten minutes.


Cosmic is an AI-powered headless CMS. We are a[Y Combinator W19](https://www.ycombinator.com/companies/cosmic) company, and our pricing is published in full:


Plan Price Buckets Team members Objects


Free $0/mo 1 2 1,000


Builder $49/mo 2 3 5,000


Team $299/mo 3 5 20,000


Business $499/mo 5 10 50,000


Enterprise Custom Custom Custom Custom


Additional team members are $29 per user per month. We charge for seats like the rest of the category does, and we would rather you see that number here than find it at renewal. Current details live on the[pricing page](https://www.cosmicjs.com/pricing) .


On the developer side, Cosmic offers a REST API and an official TypeScript SDK. We do not offer a GraphQL API, so if GraphQL is a hard requirement, take that into account. Reading content looks like this:


```text
import     {   createBucketClient   }     from     '@cosmicjs/sdk'  ;


const   cosmic   =     createBucketClient  (  {
bucketSlug  :   process  .  env  .  COSMIC_BUCKET_SLUG  !  ,
readKey  :   process  .  env  .  COSMIC_READ_KEY  !  ,
}  )  ;


const     {   objects  :   posts   }     =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  [  'title'  ,     'slug'  ,     'metadata'  ]  )
.  depth  (  1  )  ;
```


That is the whole setup. No schema compilation step, no local emulator, no separate query language to learn.


The outcome we optimize for is the one Maximilian Wuhr, Co-Founder at FINN, described:


> "Cosmic is: us never having to ask a developer to change anything on the backend of our website."


## FAQ


**Which one is better for a marketing website?**
Storyblok, in most cases. The visual editor removes the developer from routine layout changes, which is usually the actual bottleneck on a marketing team.


**Which one is better for a large multi-brand content operation?**
Contentful, generally. Environments, granular roles, and a reference-heavy model hold up better when many teams share a schema. Note the spaces limit though: multi-brand setups usually need more than the one or two spaces included on Free and Lite, so budget for the enterprise contract that comes with it.


**How much does Contentful cost?**
Contentful publishes a free tier and quotes its paid plans through sales, and its pricing page renders figures client-side rather than in the page source. We could not verify a current monthly price on August 6, 2026, so we are not quoting one. Ask Contentful directly. What is publicly documented is the plan limits table linked in the pricing section above.


**Does either one offer a GraphQL API?**
Contentful offers a GraphQL Content API. Storyblok lists GraphQL delivery on its Premium and Elite plans. Cosmic offers REST and a TypeScript SDK only.


**How do seats work?**
Storyblok includes 1 seat on Starter, 5 on Growth, and 15 on Growth Plus, and charges $15 per month for each additional seat up to a per-plan ceiling. Contentful allocates users by plan tier and documents a ceiling of 5,000 users per organization. Cosmic includes 2 to 10 team members depending on plan, with additional users at $29 per month. Count your real editor list before comparing headline prices.


**Can I switch later?**
Yes, and both export cleanly. Keep your content access behind an internal module and the switch stays cheap.


## Related comparisons


- [Sanity vs Contentful](https://www.cosmicjs.com/blog/sanity-vs-contentful)
- [Strapi vs Contentful](https://www.cosmicjs.com/blog/strapi-vs-contentful)
- [Best Contentful Alternatives in 2026](https://www.cosmicjs.com/blog/contentful-alternatives)


## Try Cosmic


Model your three hardest pages in a free bucket and see how it feels. No credit card, no sales call.


[Start building for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=contentful-vs-storyblok-signup) or[book 20 minutes with our CEO](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=contentful-vs-storyblok-demo) if you want a second opinion on your migration plan.
