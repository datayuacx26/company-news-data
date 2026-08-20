---
schema_version: "1.0.0"
document_id: "61a93147ddab97e34b22a3a6a1eab2920a774fdf6b13905e9fa499444142043c"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/how-i-built-ecommerce-store-three-prompts-cosmic-ai"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:13c423a7e6aeb2324a267bc774d25a3faeb5f92a134bd771bd9a8688bc590fbd"
---

# How I Built a Full E-Commerce Store in Three Prompts Using Cosmic AI

The[Cosmic Store](https://store.cosmicjs.com/) is a real, live, production-grade e-commerce storefront selling Cosmic company swag: hoodies, mugs, t-shirts, stickers, and a bundle box. It has a full product catalog, collection pages, dynamic routing, server-side rendering, and imgix-powered image optimization.


I built it with three prompts.


No scaffolding, no boilerplate, no back-and-forth on the content model. I described what I wanted, the Cosmic Agent figured out the data structure, generated the codebase, wired up Stripe checkout, and produced a deployable Next.js application. The whole thing is live, the source is public, and anyone can clone it to their own Cosmic account in one click.


Here is exactly what I typed and what came out the other side.


## The Three Prompts


These are verbatim. Nothing was edited after the fact.


**Prompt 1: Content Model**


> Create a store for the Cosmic company swag. Products include: 1) Hoodie 2) Coffee Mug 3) T Shirt 4) Stickers and a 5) Box of all products. Sizes: S, M, L, XL. Style like[https://shop.x.com](https://shop.x.com/) . Factor these preferences into the content structure.


**Prompt 2: Code Generation**


> Build a Next.js application for a website called 'Cosmic Store'. The content is managed in Cosmic CMS with the following object types: collections, products. Create a beautiful, modern, responsive design with a homepage and pages for each content type. Style like[https://shop.x.com](https://shop.x.com/) .


**Prompt 3: Stripe Checkout**


> Create add to cart and checkout functionality with Stripe. I've already added the env var STRIPE_PRIVATE_KEY.


That is the full input. From those three prompts, the Cosmic Agent produced a working content model, populated it with demo data, generated a full Next.js application, wired everything together using the Cosmic SDK, and added a complete Stripe checkout flow.


## What the Agent Built


### The Content Model


From Prompt 1, the agent created two object types:


**** — for grouping products by category. Each collection has:


-
-
-


**** — the full product schema. Each product has:


-
-
-
- (S, M, L, XL)
-
- (multiple images)
- (relationship to a collection object)
- (boolean flag for homepage display)


The agent correctly inferred that size variants belong on the product, that products should belong to collections, and that you need both a featured image and a gallery. I did not specify any of that. It read the product list and the style reference and reasoned about what the data model should look like.


### The Application


From Prompt 2, the agent generated a complete Next.js 15 application using the App Router. What came out:


- *Homepage* with featured products pulled dynamically from Cosmic
- *Collections index page* listing all product collections
- *Dynamic collection pages* at
- *Dynamic product pages* at with full gallery support
- *Server components* throughout for SEO-friendly, cache-optimized data fetching
- *imgix CDN* wired automatically for responsive, optimized product images
- *Tailwind CSS dark-mode design* with a clean, modern layout
- *Full TypeScript* with typed Cosmic SDK responses


### The Checkout


From Prompt 3, the agent added a complete cart and checkout flow using Stripe. I had already set the environment variable. The agent picked up the key, wired Stripe into the existing product pages, and added:


- Add to cart functionality with size selection
- Cart state managed client-side
- Stripe Checkout session creation via a Next.js API route
- Redirect to Stripe-hosted payment page on checkout
- Success and cancel return URL handling


No additional setup required on my end. The agent read the env var, inferred the Stripe integration pattern, and shipped working checkout code.


## The Cosmic SDK in Action


Here is the actual data fetching code from the repo. No custom API routes, no middleware. Just the Cosmic SDK querying content directly in server components:


```text


```


The call resolves the relationship automatically, so each product comes back with its full collection object attached. No secondary queries, no manual joins.


The client is initialized once in using the official SDK:


```text


```


Want to build AI-powered content workflows? Cosmic gives your agents a structured, versioned content store with a REST API, TypeScript SDK, and built-in analytics. See what your agents produce and whether it worked.[Start for free, no credit card required.](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=in-article-signup-cta)


## Clone It Yourself


The entire project, content model and codebase together, is available as a one-click clone from the Cosmic community.


**[Clone the Cosmic Store](https://app.cosmicjs.com/projects/new?clone_bucket=6a42be0a903866d6a2d3207f&clone_repository=6a42bee2903866d6a2d320ac)**


Cloning copies the full Cosmic bucket (with the collections and products schema and demo content) and the GitHub repository into your own account. You get the working application, the content model, and the demo data. From there you can swap in your own products, update the Stripe keys, and deploy.


The source is also public on GitHub if you want to read the code first:[github.com/cosmic-community/cosmic-store-rbb9](https://github.com/cosmic-community/cosmic-store-rbb9) .


And the live store is at[store.cosmicjs.com](https://store.cosmicjs.com/) .


## What This Actually Changes


The limiting factor in shipping a new web product used to be implementation time: setting up the content model, scaffolding the app, wiring the API, styling the components, integrating payments. That work now compresses from days to minutes.


The Cosmic Agent reads plain-English intent, reasons about the right data structure, and produces production-grade output. Three prompts to a deployable, Stripe-enabled store is the real result, and the clone link means anyone can take that same output and make it their own in one click.


If you want to build something,[sign up for free](https://app.cosmicjs.com/signup) and open the Cosmic Agent. The constraint is knowing what you want to build.


### Build AI-powered content workflows with Cosmic


Your content layer for AI agents. Structured, versioned, queryable, and analytics-ready out of the box.


[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-signup-cta)[Book a demo](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-demo)[Log in](https://app.cosmicjs.com/login?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-login)


---


*Resources:*


- [Live store](https://store.cosmicjs.com/)
- [Clone the project](https://app.cosmicjs.com/projects/new?clone_bucket=6a42be0a903866d6a2d3207f&clone_repository=6a42bee2903866d6a2d320ac)
- [View the source on GitHub](https://github.com/cosmic-community/cosmic-store-rbb9)
- [Community project page](https://www.cosmicjs.com/community/projects/cosmic-store-rbb9)
- [Sign up for Cosmic](https://app.cosmicjs.com/signup)
