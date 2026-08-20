---
schema_version: "1.0.0"
document_id: "cee39bf288ba9c53840c754149b9929a1615eb3f3462f4d25929616a3017e74f"
company_key: "yc-bytebot"
company: "Bytebot"
source_id: "yc-bytebot-news-import-2c444721769b"
canonical_url: "https://bytebot.io/blog/nextjs-headless-cms"
published_at: "2025-12-19T19:51:55.012+00:00"
first_seen_at: "2026-07-27T08:16:37.499212+00:00"
fetched_at: "2026-07-28T22:24:52.050374+00:00"
content_hash: "sha256:c8a4599e7ac182d396f89db91f4f216ebd06bebdee35fe5dd3d52533d217f8f7"
---

# Next.js CMS: How to Choose a Headless CMS for Next.js (Sanity, Payload, and More)

A Next.js CMS is easy to choose in a demo. It’s harder to live with.


After launch, the cracks show up: preview links aren’t safe, approvals don’t exist, publishing requires a rebuild, and revalidation is flaky.


Here’s how to pick a Next.js headless CMS that won’t turn publishing into a fire drill.


## What is a Next.js headless CMS?


A Next.js headless CMS manages content and delivers it to your Next.js site via an API.


- The CMS handles drafts, approvals, roles, and publishing.
- Next.js handles routing, rendering, SEO, performance, and layout.


**Headless** means the CMS doesn’t control your front-end. Your Next.js app does.


That split is great, until preview links, publishing, and cache revalidation get weird, and someone has to own the breakage.


## The lifecycle your Next.js CMS has to survive


Content is a loop: draft, review, preview, publish, update, repeat.


Everyone pulls a Next.js CMS in different directions:


1. Marketing wants speed.
2. Leadership wants control.
3. Developers want stability.


Most Next.js CMS picks happen in a demo that skips the painful parts. After launch, those parts become your whole week: secure preview links, approvals, fast rollbacks, and changes that hit production without forcing a rebuild.


So don’t choose a Next.js headless CMS because it can “fetch content.” Choose it because the loop stays boring: draft, preview, publish, update. If that works, your Next.js CMS will still feel like the right call six months after launch.


## The problems that make teams hate their Next.js CMS


A Next.js CMS usually feels fine at first. The pain shows up once real people are publishing under deadlines.


### 1. Preview is a mess


Drafts need to look like the real site, and preview links need to be safe to share. A lot of setups end up hacky, flaky, or accidentally public.


### 2. Publish doesn’t actually go live


Someone hits publish and the site is still stale. Or it only updates after a rebuild. Or it takes 10 minutes and nobody trusts it.


### 3. The content model gets stuck


What starts as “a page with a title and body” turns into sections, variants, CTAs, SEO fields, and weird one-offs.


If updating the content model in your Next.js CMS is painful, you stop touching it. Then the site slowly rots because everyone works around the CMS instead of improving it.


### 4. Access control is tough


Real teams need rules in their Next.js CMS.


Marketing should edit posts without touching navigation. Someone should approve changes. Contractors should get temporary access, not full keys.


If permissions are too loose, you ship mistakes. If they’re too strict, every edit turns into a dev ticket.


## The 4 types of Next.js headless CMS


A lot of Next.js CMS tools look the same. They aren’t.


They all “manage content,” but the tradeoffs hit where teams feel it: preview, publishing, permissions, and content model changes.


Most Next.js headless CMS options fall into four buckets.


### 1. Hosted platforms (SaaS-first)


Log in, start publishing. Hosted admin UI + APIs + solid editorial features out of the box.


**Good fit when:** you want low maintenance, marketing moves fast, and you don’t want to run CMS infrastructure.
**Where it bites:** pricing and platform limits, plus you’re stuck with their approach to Next.js CMS preview, roles, and workflows.


### 2. Self-hosted / open-source headless CMS


You run it. More control, more flexibility, more responsibility.


**Good fit when:** ownership matters, you can handle ops, and you want to avoid lock-in.
**Where it bites:** upgrades, security, and the “who owns the outage” problem.


### 3. Database-first CMS


Point at a database, get an admin UI, expose APIs. Great when “content” is really structured data.


**Good fit when:** you already have structured data, or you need a fast back office for a Next.js app.
**Where it bites:** less editor-friendly, and content modeling starts to feel like schema design.


### 4. App-native CMS (CMS inside your app)


The CMS lives in your product. Same codebase, auth, and deployments.


**Good fit when:** permissions are weird, content is tied to users/accounts, and you need total control.
**Where it bites:** you own everything, including building an editing experience people don’t hate.


This is why “best Next.js CMS” is a trap. The better question is: which type matches how your team actually ships content in Next.js.


## The major Next.js CMS options and where they fit


Once you know which type of CMS you want, the list of “real” options gets a lot smaller. Here are the names you’ll keep running into, grouped by the bucket they usually land in.nd in.


Bucket Best when Pros Tradeoffs Picks


Hosted (SaaS-first) You want the best editor experience and don’t want to run CMS infra Fast setup, strong workflows, marketing-friendly Pricing climbs, vendor constraints Storyblok, Sanity


Self-hosted / open-source You want ownership and can operate it Full control, flexible hosting, no “SaaS lock-in” You own upgrades, security, backups Strapi


Database-first You already have the data and just need an admin UI + API Quick admin UI on real tables, great for internal data Content modeling becomes “your DB,” less editor-friendly Directus


App-native (inside your app) Content is tied to product logic, auth, roles, workflows Tight integration, custom rules, same codebase You own the editing experience Payload CMS


Pick the right bucket and you won't have to worry about choosing the wrong CMS.


## How to choose a Next.js CMS for your content lifecycle


Scenario What you want Why Recommendation


Marketing site with frequent changes Hosted Next.js headless CMS Best editor experience, lowest ops burden Storyblok


Small team that wants control and can run it Self-hosted / open-source headless CMS You own it, but you also own updates, security, backups Strapi


You already have a database and just need a UI + API Database-first CMS Fast admin UI + API on top of existing tables Directus


App-native Next.js CMS Database-first CMS One auth/roles system, workflows live with the app Payload CMS


Four common setups. Four CMS picks. Don’t overthink it.


## Next.js CMS integration basics


This is the part nobody wants to think about during setup, but it decides whether your Next.js headless CMS feels smooth or annoying.


### Preview and drafts


Previews are easiest when you treat them as a first-class feature, not a hacky “hidden URL.”


- Use Draft Mode in the App Router so Next.js can render draft content on request (instead of serving cached or static pages).
- Most CMS setups do this by having the CMS hit a Route Handler that enables draft mode, then redirects to the page being previewed.
- [Draft Mode](https://nextjs.org/docs/app/api-reference/functions/draft-mode) is designed so the bypass cookie can’t be guessed and changes per build, but you still need to lock down the endpoint.


### Publishing and revalidation


“Publish” needs to mean “the site updates,” not “wait for a rebuild and hope.”


- Have your CMS send a webhook to Next.js when content is published/unpublished.
- In that webhook handler, call:


- [revalidatePath()](https://nextjs.org/docs/app/api-reference/functions/revalidatePath) when you know the exact page URL that changed.
- [revalidateTag()](https://nextjs.org/docs/app/api-reference/functions/revalidateTag) when multiple pages depend on the same data, and you want to refresh them together.


- Next.js supports[on-demand revalidation](https://nextjs.org/docs/14/app/building-your-application/data-fetching/fetching-caching-and-revalidating) with both path-based and tag-based approaches.


## Caching expectations


Caching is great until you’re debugging “why didn’t it update.”


- Next.js has multiple[caching layers](https://nextjs.org/docs/app/guides/caching) , and you should decide up front what’s cached and how it gets invalidated. Next.js
- Server[fetch()](https://nextjs.org/docs/app/api-reference/functions/fetch) in Next.js has framework-level caching and revalidation behavior, so be deliberate with tags and revalidate settings.


## What “good” looks like in a Next.js CMS setup


If you want a Next.js CMS that stays boring after launch, most setups have four things.


### 1. A real preview flow


- The CMS generates a preview link.
- That link hits a Next.js route, enables Draft Mode, then redirects to the page.
- Draft content is only visible to people who should see it.


### 2. Publishing fires a webhook.


- Publishing fires a webhook.
- The webhook tells Next.js what changed.
- You revalidate a path or tag, not a full rebuild.


### 3. A content model that can evolve


- Start simple.
- Add sections/blocks only when needed.
- Deprecate fields instead of breaking them overnight.


### 4. Permissions match real risk


- Editors can safely update most content.
- High-risk areas (homepage, nav, legal) require approvals or tighter roles.
- Temporary access is normal, not a hack.


## Wrapping it up


Most teams don’t hate their Next.js CMS because they picked the “wrong” product. They hate it because it doesn’t match how they create and ship content. Pick the right category, get preview and revalidation working, and your Next.js headless CMS turns into what it should be: a boring, reliable Next.js CMS.
