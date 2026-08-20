---
schema_version: "1.0.0"
document_id: "5a545a85a5e8f2b05854fa31e33af14432bfb281a0ccf8050d883c01a5fcc4bd"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/headless-ssr-rendering"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:e0c04f620cfcf119c5d7e4b0a82dcbb0163f0bfc8cac912cce41bea9fc13fad0"
---

# Server-side rendering on a headless storefront: making product data visible to Google and AI

Enriching product data is only half the job — it also has to land in the HTML that Google, Bing, and AI agents actually read. Headless storefronts split rendering work between server and client in ways that can quietly leave your richest product data out of the document a crawler sees. This guide covers how that split works, where data typically falls through, and how to confirm it's actually in the response.


## How a headless storefront renders a product page


A headless storefront is a frontend — Next.js, Nuxt, Remix, or Shopify's Hydrogen — that calls a commerce API or PIM for product data and turns it into HTML. There are three ways that HTML gets built, and they behave very differently for crawlers:


- **Server-side rendering (SSR):** the frontend server calls the product API on each request and returns fully formed HTML, with title, price, description, and attributes already in the markup. This is the default in Next.js with[Server Components](https://nextjs.org/docs/app/getting-started/server-and-client-components) , and it's how Hydrogen works via React Router loaders that fetch data server-side before rendering.
- **Static generation / ISR:** the same idea, but the HTML is built ahead of time (at build, or on a revalidation interval) instead of per request. Good for catalog pages that don't change every second.
- **Client-side rendering (CSR):** the server returns a near-empty HTML shell — essentially an empty root container element — and a JavaScript bundle fetches product data in the browser after load, then paints it into the DOM. The initial HTTP response contains none of the product content.


Most modern headless builds are a mix: a server-rendered shell carrying the product's core facts, with small "islands" of client-side interactivity — an add-to-cart button, a size selector, a reviews widget — hydrated on top. The question that matters for SEO isn't "SSR or CSR" as a whole-app choice; it's which fields land in the server response versus which are fetched client-side after mount.


## Why client-only rendering hides product data from crawlers and AI agents


Google's own developer documentation on JavaScript SEO describes indexing as happening in two waves: Googlebot crawls and queues a URL, then queues it separately for a rendering pass where a headless Chromium instance executes the JavaScript before Google indexes the rendered result. That rendering pass isn't instant — it runs on Google's own schedule, and pages that depend on it are exposed to delay and occasional script errors that silently drop content. The docs note that "not all bots can run JavaScript," and still recommend server-side or pre-rendering because it's faster for users and crawlers alike. See[Understand JavaScript SEO basics](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) .


Two consequences matter for a product page. First, many non-Google crawlers — including bots behind AI answer engines and shopping agents — don't render JavaScript at all, or budget very little for it; they fetch the raw HTML and reason over that alone. If title, price, availability, and specs only exist after a client-side` fetch()` call, those systems see an empty shell. Second, even engines that do render JavaScript still treat server-rendered HTML as the first and most reliable signal: it's what gets picked up fastest, what feed validators and structured-data testers read by default, and what survives if a client-side fetch fails or times out.


## Where product data typically leaks out of server-rendered HTML


The most common failure pattern isn't "we used React" — it's specific fields quietly moved to the client for convenience:


- Price and availability fetched client-side to reflect real-time inventory, while everything else is server-rendered.
- Attributes, specs, or long-form descriptions loaded lazily on scroll or on a tab click, so they never appear in the initial document.
- [JSON-LD](https://www.anglera.com/glossary/json-ld) injected into the DOM after hydration via a client-side script instead of rendered into the initial HTML.
- Variant data (color, size, configuration) held entirely in client-side state and never reflected in the markup for the default variant.


None of these are wrong choices for the interactive experience — they're wrong only when the same data has no server-rendered fallback.


## Getting product data into the server-rendered HTML


The fix: fetch and render the core product facts on the server, and keep only true interactivity — state, event handlers, cart mutations — as client components layered on top. In a Next.js App Router storefront, that's a Server Component for the page and a small Client Component for the interactive bits:


```text
// app/products/[handle]/page.tsx — Server Component (default, no "use client")
import { getProduct } from '@/lib/commerce'
import AddToCartButton from './add-to-cart-button'


export default async function ProductPage({
params,
}: {
params: Promise<{ handle: string }>
}) {
const { handle } = await params
const product = await getProduct(handle)


const jsonLd = {
'@context': 'https://schema.org',
'@type': 'Product',
name: product.title,
description: product.description,
sku: product.sku,
gtin13: product.gtin,
brand: { '@type': 'Brand', name: product.brand },
offers: {
'@type': 'Offer',
priceCurrency: product.currency,
price: product.price,
availability: `https://schema.org/${product.availability}`,
},
}


return (
<main>
<h1>{product.title}</h1>
<p>{product.description}</p>
<ul>
{product.attributes.map((a) => (
<li key={a.name}>
{a.name}: {a.value}
</li>
))}
</ul>
<p>{product.price}</p>
<AddToCartButton productId={product.id} />
<script
type="application/ld+json"
dangerouslySetInnerHTML={{
__html: JSON.stringify(jsonLd).replace(/</g, '\\u003c'),
}}
/>
</main>
)
}


```


```text
// app/products/[handle]/add-to-cart-button.tsx — Client Component (interactivity only)
'use client'


import { useState } from 'react'


export default function AddToCartButton({ productId }: { productId: string }) {
const [loading, setLoading] = useState(false)
return <button disabled={loading} onClick={() => setLoading(true)}>Add to cart</button>
}


```


The title, description, attributes, price, and JSON-LD are all produced on the server and present in the initial response; only the button's click behavior is client-side. Next.js's own JSON-LD guidance confirms this pattern: render the structured-data script tag directly in the component, escaping angle brackets in the stringified payload, rather than injecting it after hydration (see the[Next.js JSON-LD guide](https://nextjs.org/docs/app/guides/json-ld) ). The same principle holds on Hydrogen (fetch in a loader) or Nuxt (server components) — the framework changes, the rule doesn't: if a non-rendering crawler should see it, it has to be in the response body.


If price or availability genuinely needs to be real-time and can't be baked into SSR, render a last-known-good value server-side (from cache or your PIM feed) as the default, then let client-side JavaScript update it. That gives crawlers a truthful fallback instead of nothing.


## How to validate


Check three things, in this order:


**1. View-source vs. rendered DOM.** In Chrome,` Ctrl+U` on Windows/Linux or` Cmd+Option+U` on Mac opens view-source, showing the raw server response before any JavaScript runs. Compare it against the Elements panel in DevTools (the rendered DOM, post-hydration). If the price, description, or attributes are visible in the Elements panel but absent from view-source, they're client-rendered only.


**2. curl the URL directly.** This mimics what a non-rendering crawler sees:


```text
curl -sL https://www.example.com/products/your-handle | grep -i "your-product-title"
curl -sL https://www.example.com/products/your-handle | grep -i "application/ld+json"


```


If the title or JSON-LD block doesn't show up in that output, it isn't in the server-rendered HTML.


**3. Google's URL Inspection Tool (Search Console) and the Rich Results Test.** URL Inspection shows Google's own rendered HTML and screenshot after its rendering pass runs, revealing what Googlebot eventually indexes even if it lags the raw response. The[Rich Results Test](https://search.google.com/test/rich-results) validates that your Product JSON-LD parses correctly and flags missing fields; Google's[Product structured-data reference](https://developers.google.com/search/docs/appearance/structured-data/product) lists the fields required for merchant listings and product snippets.


**Verified as of July 2026:** this reflects current Next.js App Router Server/Client Component behavior, Shopify Hydrogen's server-loader model, and Google's published JavaScript SEO and Product structured-data documentation. Framework APIs and rendering behavior change between releases, so recheck the linked docs before treating specific field names as permanent.


None of this matters if the underlying product data is thin — a perfectly server-rendered page with three sparse attributes still gives crawlers and AI agents little to work with. Anglera plugs into your existing PIM or commerce platform and continuously enriches attributes, specs, use-cases, and identifiers, so the server-rendered markup above has genuinely rich, structured data to carry rather than placeholder text.
