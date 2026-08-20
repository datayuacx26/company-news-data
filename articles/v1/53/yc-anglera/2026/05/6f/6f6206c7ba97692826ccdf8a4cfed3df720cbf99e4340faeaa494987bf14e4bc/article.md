---
schema_version: "1.0.0"
document_id: "6f6206c7ba97692826ccdf8a4cfed3df720cbf99e4340faeaa494987bf14e4bc"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/optimizely-ssr-rendering"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:13:02.720629+00:00"
content_hash: "sha256:25530462e7d045c94d05d7007bfc68374680390279dafd89d185b4e3622aeeb0"
---

# Server-side rendering on Optimizely Configured Commerce: making product data visible to Google and AI

Optimizely Configured Commerce's modern storefront, Spire, is a React application that can render product pages either on the server or in the browser. For a distributor, the difference is not academic: if a[product detail page](https://www.anglera.com/glossary/pdp-product-detail-page) 's specs, attributes, and pricing only materialize after client-side JavaScript runs, search engines and AI answer engines may never see them. This guide covers how Spire's rendering pipeline works, where it commonly falls short for SEO, and exactly how to check what a crawler actually receives.


## How Spire renders a product page


Spire replaced the older Classic (ASP.NET MVC/Handlebars) storefront as the current-generation front end for Configured Commerce 4.x/5.x. It's built on React with Redux for state, and Optimizely's own documentation is direct about the point of the work: Spire supports server-side rendering "to ensure search engine crawlers see the same content as users," so that a crawler that never executes JavaScript still gets a fully populated page rather than an empty shell.


Mechanically, an incoming request runs through a Node.js SSR render loop that renders the React tree, lets Redux-connected components fire their data-loading logic, waits for the resulting promises to resolve, and re-renders — repeating until no new promises are created (Optimizely caps this at 10 iterations, logging or throwing if a component keeps triggering new fetches). The output is a fully populated HTML document sent to the browser, which then hydrates it into an interactive React app client-side.


Two things make this different from a simple "SSR on/off" switch:


1. **SSR is not always on for every visitor.** By default, Configured Commerce sends the SSR-rendered HTML to requests it recognizes as web crawlers, based on the User-Agent header. Since release 5.2.2411, an "Enable Server-Side Rendering for All User Agents" option lets admins extend full SSR to every visitor rather than just known bots — useful if you also care about Core Web Vitals/LCP for real shoppers, not just crawlability.
2. **Not all React logic runs during SSR.**` useEffect` and` useLayoutEffect` do not execute on the server — only during client-side hydration. If a component's data-fetching or content logic lives in one of those hooks, that content simply will not exist in the HTML a crawler receives, even though it renders correctly in a live browser. Optimizely's own guidance is that logic required for SEO must live in` UNSAFE_componentWillMount` on a class component instead (or otherwise be handled outside the hook lifecycle).


## Where product data actually goes missing


Configured Commerce gives admins three sub-settings that determine how much of a PDP the SSR pass waits for before returning HTML:


- **Include the Catalog in the SSR Response** (waits for catalog/product-attribute APIs — default **Yes** )
- **Include Real Time Pricing in SSR Response** (default **No** )
- **Include Real Time Inventory in SSR Response** (default **No** )


This is the most common source of "why isn't my product data in Google" tickets on this platform. If catalog data is included but pricing and inventory are not (the out-of-the-box default), that's usually fine — descriptions, attributes, specs, and identifiers render server-side, while price/stock defer to client hydration, which is a reasonable tradeoff since real-time price and inventory calls are typically per-user and slower. But if a custom widget was built to pull specs, use-case content, or attribute data through a real-time API call gated behind pricing or inventory logic (or behind a` useEffect` ), that content silently drops out of the server HTML, even though a merchandiser looking at the live site in a browser sees it rendering just fine. AI crawlers make this worse than Googlebot does: Google's indexer will at least attempt a secondary JavaScript-rendering pass, on a rendering budget that isn't guaranteed and can lag by days. Most AI agents and LLM-based crawlers (ChatGPT's browsing, Perplexity, Claude's web fetch, and most third-party "AI visibility" scrapers) fetch the raw HTML response and do not execute JavaScript at all — so anything that depends on client-side rendering is invisible to them by default, full stop.


## Finding and setting the SSR toggles


In the Admin Console, SSR configuration lives under **Administration → System → Settings** , in the settings group covering server-side rendering (search "Server Side Rendering" from the Settings landing page if the category tiles have been reorganized in your version — Configured Commerce groups Settings into a dashboard-style landing page with category tiles, and only shows settings relevant to your installed edition and role, so exact placement and visibility can shift between versions; the search box on that landing page is the most reliable way to find a setting by name). The toggles to check:


```text
Enable Server Side Rendering            → Yes
Include the Catalog in the SSR Response → Yes
Enable Server-Side Rendering for All User Agents → evaluate per Core Web Vitals goals (5.2.2411+)


```


Separately, structured data and crawler visibility for individual pages are configured on the **Websites → your website → SEO** tab (or the equivalent global defaults under Administration → Settings → Site Configurations) — this is where structured data and per-page crawler inclusion are turned on, and where canonical-link behavior for product pages is set. Optimizely's documentation notes that updating an SEO setting in one of these two locations reflects it in the other, since the website-level tab is a scoped view onto the same underlying configuration.


A minimal example of what should land in the server HTML for a PDP:


```text
<script type="application/ld+json">
{
"@context": "https://schema.org",
"@type": "Product",
"sku": "PMP-4400-SS",
"name": "4400 Series Stainless Centrifugal Pump",
"description": "1.5 HP, 316 stainless housing, NPT 2-inch inlet...",
"brand": { "@type": "Brand", "name": "Acme Fluid Systems" },
"offers": {
"@type": "Offer",
"priceCurrency": "USD",
"price": "1249.00",
"availability": "https://schema.org/InStock"
}
}
</script>


```


## How to validate


Don't trust what you see in a live browser tab — that's post-hydration DOM, not what a crawler fetched. Check the raw response instead:


- **View-source vs. rendered DOM** : In Chrome,` Ctrl+U` /` Cmd+Option+U` (view-source:) shows the literal HTML the server sent, before hydration. Compare it against the regular Inspector/Elements panel (the live DOM after React hydrates). If specs, attributes, or price appear in Elements but not in view-source, that content is client-only.
- **curl as a plain client** , no JS execution:


```text
curl -s -A "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" \
https://www.example.com/products/pmp-4400-ss | grep -i "application/ld+json" -A 20


```


Run it again with a generic browser User-Agent and diff the two responses — if Configured Commerce is set to serve SSR only to recognized crawlers, the bodies may differ until "Enable Server-Side Rendering for All User Agents" is turned on.
- **Google's Rich Results Test / URL Inspection in Search Console** : paste the PDP URL and check the rendered HTML tab against the "page fetch" (raw) HTML tab — a mismatch on your Product schema or key attributes confirms a client-side-only dependency.
- **Fetch with JavaScript disabled** (Chrome DevTools → Command Menu → "Disable JavaScript," then reload) to simulate how a non-executing crawler sees the page.


Verified as of July 2026 against Optimizely's Configured Commerce developer documentation and Support Help Center articles; SSR-related setting names, defaults, and menu locations are version-dependent (notably the 5.2.2411 "all user agents" option and ongoing reorganization of the Settings landing page), so confirm exact placement against your instance's release notes before making changes.


None of this matters if the underlying product data isn't there to render in the first place. Anglera plugs into your PIM or Configured Commerce catalog directly and continuously enriches products — attributes, specs, use-cases, identifiers — without displacing it as your system of record, so once your SSR pipeline is confirmed to pass catalog data through, there's substantive, structured content for it to put on the page.
