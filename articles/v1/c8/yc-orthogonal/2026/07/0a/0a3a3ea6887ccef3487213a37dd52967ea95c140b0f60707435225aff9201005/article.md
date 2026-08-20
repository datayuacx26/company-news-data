---
schema_version: "1.0.0"
document_id: "0a3a3ea6887ccef3487213a37dd52967ea95c140b0f60707435225aff9201005"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/context-dev-structured-data-from-any-domain"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:21d612ae80c3cef14a3d933a0e44fcf29f4381b24af245d730ba7b428c42fb0a"
---

# Context.dev: Turn Any Domain into Structured, AI-Ready Data

Agents building onboarding flows, CRM enrichment, or competitor analysis need structured data about companies. Logos, colors, descriptions, social handles, industry codes, product catalogs. The usual approach is scraping a website, parsing HTML, guessing at image URLs, and hoping nothing breaks when the site redesigns. It's fragile, slow, and every company's site is different.


Today we're excited to announce our partnership with[Context.dev](https://www.context.dev/) , a platform that turns any domain or URL into structured, AI-ready data with a single API call. Send a domain, get back logos in every format, brand colors, company metadata, social handles, industry classification, and more. Send a URL, get clean markdown, a full design system, product data, or a screenshot. Every response is typed JSON. Context.dev powers brand data for companies like Mintlify, daily.dev, and Propane.


## What is Context.dev?


Context.dev provides five categories of APIs that extract structured information from domains and URLs:


- **Brand Intelligence** : Retrieve logos (light, dark, icon variants in SVG + PNG), brand colors, company profiles (description, slogan, email, phone), social handles for 25+ platforms, company addresses, and stock tickers. Look up brands by domain, company name, email address, stock ticker, or ISIN.
- **Web Scraping** : Scrape any URL to clean markdown or raw HTML, crawl entire sitemaps, capture screenshots at custom viewports, search the web with optional markdown extraction, and extract images with metadata.
- **AI Extraction** : Extract product data (name, price, features, images, SKU) from any product page, and extract custom data points from any website by describing what you need.
- **Design Systems** : Pull full design systems (colors, typography, spacing, shadows, component styles) and scrape font families with usage stats from any brand's website.
- **Classification** : Classify any brand into NAICS codes with confidence scores. Enrich transaction descriptors (like "AMZN MKTP US") into full merchant profiles with logos and industry labels.


Bot detection bypass and proxy escalation are handled automatically. No browser automation required on your end.


## Using Context.dev with Orthogonal


### Retrieve Brand Data


Get a complete brand profile from any domain: logos, colors, description, social handles, address, and industry classification.


` import


Orthogonal


from


"@orth/sdk"


;


const


orthogonal


=


new


Orthogonal


(


{


apiKey


:


process


.


env


.


ORTHOGONAL_API_KEY


,


}


)


;


const


result


=


await


orthogonal


.


run


(


{


api


:


"context-dev"


,


path


:


"/brand/retrieve"


,


query


:


{


domain


:


"stripe.com"


}


}


)


;


console


.


log


(


result


)


;


// Returns: logos (SVG + PNG, light/dark/icon variants),


// brand colors, title, description, slogan, social handles,


// address, stock ticker, NAICS industry classification, and more


`


You can also look up brands by company name, email address, or stock ticker:


` const


result


=


await


orthogonal


.


run


(


{


api


:


"context-dev"


,


path


:


"/brand/retrieve-by-name"


,


query


:


{


name


:


"Stripe"


}


}


)


;


`


### Scrape to Markdown


Turn any URL into clean, LLM-ready markdown with navigation and ads stripped out.


` const


result


=


await


orthogonal


.


run


(


{


api


:


"context-dev"


,


path


:


"/web/scrape/markdown"


,


query


:


{


url


:


"https://stripe.com/pricing"


}


}


)


;


console


.


log


(


result


.


markdown


)


;


// Returns: clean markdown content ready for RAG pipelines,


// summarization, or agent consumption


`


### Extract Products


Given a product page URL, extract structured product data: name, description, price, billing frequency, features, images, category, and SKU.


` const


result


=


await


orthogonal


.


run


(


{


api


:


"context-dev"


,


path


:


"/brand/ai/product"


,


method


:


"POST"


,


body


:


{


url


:


"https://allbirds.com/products/mens-wool-runners"


}


}


)


;


console


.


log


(


result


)


;


// Returns: product name, description, price, currency,


// billing frequency, pricing model, features, images,


// category, target audience, tags, and SKU


`


### Extract Structured Data with a Schema


Define the shape of data you want as a JSON Schema, and Context.dev crawls the site and fills it in.


` const


result


=


await


orthogonal


.


run


(


{


api


:


"context-dev"


,


path


:


"/web/extract"


,


method


:


"POST"


,


body


:


{


url


:


"https://linear.app"


,


schema


:


{


type


:


"object"


,


properties


:


{


has_free_trial


:


{


type


:


"boolean"


}


,


pricing_tiers


:


{


type


:


"array"


,


items


:


{


type


:


"object"


,


properties


:


{


name


:


{


type


:


"string"


}


,


price


:


{


type


:


"number"


}


}


}


}


,


founded_year


:


{


type


:


"number"


}


}


}


,


instructions


:


"Find pricing and company info"


}


}


)


;


console


.


log


(


result


.


data


)


;


// Returns typed JSON matching your schema:


// { has_free_trial: true, pricing_tiers: \[...\], founded_year: 2019 }


`


### Industry Classification


Classify any brand into NAICS codes from just a domain or company name.


` const


result


=


await


orthogonal


.


run


(


{


api


:


"context-dev"


,


path


:


"/web/naics"


,


query


:


{


input


:


"shopify.com"


}


}


)


;


console


.


log


(


result


)


;


// Returns: NAICS code, industry name, and confidence score


`


### Using x402 Protocol


Context.dev on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


` import


{


wrapFetchWithPayment


}


from


"x402-fetch"


;


import


{


privateKeyToAccount


}


from


"viem/accounts"


;


const


privateKey


=


process


.


env


.


PRIVATE_KEY


;


if


(


!


privateKey


)


{


throw


new


Error


(


"PRIVATE_KEY environment variable is required"


)


;


}


const


account


=


privateKeyToAccount


(


privateKey


.


startsWith


(


"0x"


)


?


privateKey


:


\`


0x


${


privateKey


}


\`


,


)


;


const


fetchWithPayment


=


wrapFetchWithPayment


(


fetch


,


account


)


;


const


response


=


await


fetchWithPayment


(


"https://x402.orth.sh/context-dev/brand/retrieve?domain=stripe.com"


,


{


method


:


"GET"


}


)


;


const


data


=


await


response


.


json


(


)


;


console


.


log


(


data


)


;


`


### Using MPP (Machine Payments Protocol)


Context.dev is also available via[MPP](https://mpp.dev/) , the open standard for machine-to-machine payments co-authored by Tempo and Stripe. MPP supports stablecoins, cards, and Bitcoin on the same endpoint.


` import


{


privateKeyToAccount


}


from


"viem/accounts"


;


import


{


Mppx


,


tempo


}


from


"mppx/client"


;


Mppx


.


create


(


{


methods


:


\[


tempo


(


{


account


:


privateKeyToAccount


(


"0x..."


)


}


)


\]


,


}


)


;


const


response


=


await


fetch


(


"https://mpp.orthogonal.com/context-dev/brand/retrieve?domain=stripe.com"


)


;


const


data


=


await


response


.


json


(


)


;


console


.


log


(


data


)


;


`


The` mppx` client handles 402 challenges automatically: it intercepts the payment request, signs a credential with your wallet, and retries with proof of payment.


## Capabilities


Context.dev's APIs cover a wide surface:


- **Brand lookups by domain, name, email, stock ticker, or ISIN** with logos in SVG + PNG (light, dark, and icon variants)
- **25+ social platform handles** per brand (X, LinkedIn, GitHub, YouTube, and more)
- **NAICS industry classification** with confidence scores
- **Transaction enrichment** that resolves raw card descriptors to real merchants
- **Full design system extraction** : colors, typography, spacing, shadows, and component CSS
- **Product extraction** with structured fields for name, price, features, images, and SKU
- **Schema-driven extraction** that lets you define exactly what data you want from any website
- **Web scraping** to markdown, HTML, or screenshot with automatic bot detection bypass
- **Site crawling** across entire sitemaps with markdown extraction for every page


## Use Cases


### CRM and Onboarding Enrichment


When a user signs up with a work email, resolve their domain to get company logos, brand colors, industry, and social links. Pre-fill onboarding forms and personalize their experience from day one.


### Agent-Driven Research


Agents building competitive analyses can scrape competitor sites to markdown, extract pricing data, pull product catalogs, and classify companies by industry. All structured, all typed.


### Transaction Categorization


Fintech platforms can turn raw card descriptors like "AMZN MKTP US" into full merchant profiles with logos, colors, and industry labels. Build spend analytics and transaction tagging without maintaining a merchant database.


### White-Label Theming


Extract a customer's full design system (colors, fonts, spacing, component styles) from their website and auto-generate white-labeled UIs that match their brand.


### Logo Walls and Partner Pages


Use Logo Link to embed always-fresh brand logos from a CDN without API calls. Build dynamic logo walls, partner pages, and integration directories that stay current automatically.


## Why We Partnered with Context.dev


Most approaches to extracting company data from the web involve brittle scrapers, regex, and constant maintenance. Context.dev replaces all of that with typed API responses that handle bot detection, proxy escalation, and data normalization for you. Whether your agent needs a company logo, a pricing page in markdown, or a full product catalog, it's one API call that returns structured JSON. That reliability and breadth is exactly what agents need to operate autonomously.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Context.dev and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/context-dev) |[Context.dev Website](https://www.context.dev/)
