---
schema_version: "1.0.0"
document_id: "ed8d769c724e1d01958f1c5a4e8a2af9765eda2de258bc78e781164542302b7c"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/sap-commerce-ssr-rendering"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:acb7e8948703c280d0fd61ad7e09df5f9d441a545383c2dfa96074f34456c744"
---

# Server-side rendering on SAP Commerce Cloud: making product data visible to Google and AI

SAP Commerce Cloud gives distributors and manufacturers two very different storefront architectures, and they render product data in opposite ways. If your enriched attributes, specs, and identifiers never make it into the HTML a crawler actually receives, none of that enrichment work pays off in search or in AI answer engines. Here's how rendering works on each SAP Commerce Cloud storefront, why it matters, and how to check it yourself.


## Two storefront architectures, two rendering models


SAP Commerce Cloud storefronts fall into two families:


- **Accelerator (JSP-based)** — the legacy storefront, built on Java Server Pages. Every request is rendered fully on the server; the HTML sent to the browser (and to any crawler) already contains the rendered product content. SAP deprecated Accelerator's UI extensions and add-ons in the 2205 release, with removal targeted for a 2027 release, and active development now focused on the Composable Storefront — but a large share of existing deployments still run Accelerator today, and for those sites the crawler-visibility question is largely moot: what you see in "view source" is what a bot sees too.
- **Composable Storefront (Spartacus)** — the current, actively developed storefront, built as an Angular single-page application that talks to commerce back ends through the OCC (Omni Commerce Connect) REST API. Spartacus renders in the browser by default. Without server-side rendering enabled, the initial HTML response is a near-empty shell — an app root element plus JavaScript bundles — and the product name, price, specs, and identifiers only appear after Angular boots and fetches data client-side.


This second point is the one that catches teams out. A PDP can look complete in a normal browser, because the browser executes the JavaScript and fills in the DOM, while the exact same URL returns almost no product content to anything that doesn't execute JavaScript, or that gives up before the client-side render finishes.


## Why client-only rendering hides data from crawlers and AI agents


Googlebot does execute JavaScript, but it does so in a second, deferred rendering pass, on a delay and a resource budget separate from the initial crawl. Many other consumers of your pages — AI assistants and agents that fetch pages to answer product questions, comparison tools, some retailer and marketplace feed crawlers, and plenty of SEO and monitoring tools — either don't execute JavaScript at all or apply much stricter timeouts than a full browser. If your Spartacus storefront ships CSR-only, all of them are working from that same near-empty shell.


Two additional consequences of Spartacus SSR are easy to miss:


- **Rendering strategy is per-request, not global.** Spartacus's SSR layer can fall back to client-side rendering under load, on a timeout, or for URLs excluded from the rendering rules. If that fallback is happening more often than intended, product pages intermittently serve the empty-shell version.
- **[JSON-LD](https://www.anglera.com/glossary/json-ld) structured data depends on SSR.** Spartacus generates product` Product` ,` Offer` , and review schema through a` ProductSchemaBuilder` and injects it as a JSON-LD script tag via a` JsonLdDirective` :


```text
<script type="application/ld+json">
{ "@type": "Product", "name": "..." }
</script>


```


In production, that structured-data generation is scoped to the server-side rendering pass — it is not emitted on a pure client-side render. If SSR isn't running (or falls back to CSR for a given request), the JSON-LD block that AI agents and Google's Rich Results parser rely on simply isn't in the response.


## Making sure product data is in the server-rendered HTML


For an Accelerator storefront, there is no separate SSR toggle to manage — confirm the JSP templates that render the PDP (name, description, price, attributes) are populated from the same product/CMS data your PIM or Anglera feeds, and check that no client-side widget is silently replacing static content after load.


For a Spartacus (Composable Storefront) deployment, SSR is an explicit build and runtime layer on top of the Angular application, using Angular Universal:


1. **Add SSR support.** SAP's recommended path is the Spartacus schematics command, which scaffolds the Angular Universal server bundle, an Express server entry point, and the required` app.server.module.ts` wiring:


```text
ng add @spartacus/schematics --ssr


```


1. **Disable the PWA service worker where SSR must serve first-paint HTML** , since a caching service worker will serve the cached` index.html` and JS bundles before the server can render, skipping SSR entirely. Set this in the Spartacus config:


```text
pwa: {
enabled: false
}


```


1.


**Tune the SSR runtime for your traffic** , in the SSR-specific configuration (` ssr` config object):` concurrency` controls how many renders run in parallel before new requests fall back to CSR (default 20), and` timeout` controls how long a single render is allowed to run before falling back (default 3000ms). A` renderingStrategyResolver` can force` ALWAYS_SSR` for bot user agents and product/category URLs specifically, with its own` forcedSsrTimeout` , so crawlers never get shed to the CSR fallback even under load.


2.


**Confirm the build and start the SSR server** and hit it directly rather than the CSR dev server:


```text
yarn run build:ssr && yarn run serve:ssr


```


1. **On SAP Commerce Cloud (Public Cloud/CCv2), verify the SSR service is actually in front of the app** — some Spartacus/CCv2 version combinations have had documented routing issues that bypassed the SSR container and served the CSR bundle directly, silently undoing all of the above.


## How to validate


Don't rely on what you see in a normal browser tab — that's always the fully-hydrated, client-rendered view. Compare the raw server response to the rendered DOM instead:


- **View-source vs. Inspect/Elements.** Open the PDP, then use "View Page Source" (the raw HTML SAP Commerce Cloud actually sent) alongside DevTools' Elements panel (the DOM after Angular hydration). On a working SSR setup, both should already contain the product name, price, and description — Elements just adds interactivity on top. If view-source shows an empty app-root shell (something like` app-root` with no children) and Elements shows a full PDP, SSR isn't reaching that request.
- **curl the URL directly** , which never executes JavaScript, so it shows exactly what a non-browser crawler receives:


```text
curl -s https://www.example.com/product/12345/widget-abc | grep -i "widget-abc"
curl -s https://www.example.com/product/12345/widget-abc | grep -i 'application/ld+json'


```


If the product name/SKU and a` ld+json` script block both come back, SSR and structured-data generation are working for that URL. An empty result from the first` grep` with content still visible in a browser is the signature of CSR-only rendering.


- **Disable JavaScript in DevTools** (Command Menu → "Disable JavaScript") and reload the PDP. What remains is close to what a non-JS-executing crawler sees.
- **Google's Rich Results Test** (or Search Console's URL Inspection tool) shows Google's own rendered HTML and flags any structured data it detected — useful for confirming JSON-LD survives Googlebot's render pass, not just your own curl check.
- **Spot-check a sample across categories** , not just one PDP — SSR fallback behavior under load, and any rendering-strategy exclusions, mean one clean page doesn't guarantee the pattern holds site-wide.


Verified as of July 2026 against current Spartacus/Composable Storefront documentation; SSR configuration options and defaults can shift between Spartacus versions, so confirm against the release your storefront is pinned to before changing production` ssr` config.


None of this matters if the underlying product record is thin to begin with — SSR just faithfully serves whatever data your PIM or commerce platform hands it. That's the half of the problem Anglera is built for: it enriches product data continuously — attributes, specs, use-cases, identifiers — directly in the PIM or commerce platform you already run, without a rip-and-replace migration. Once that data is rich, the server-rendered HTML your SAP Commerce Cloud storefront produces has something worth rendering, for buyers and AI agents alike.
