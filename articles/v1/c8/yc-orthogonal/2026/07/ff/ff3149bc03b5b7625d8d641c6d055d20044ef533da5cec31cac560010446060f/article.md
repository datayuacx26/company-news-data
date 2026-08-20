---
schema_version: "1.0.0"
document_id: "ff3149bc03b5b7625d8d641c6d055d20044ef533da5cec31cac560010446060f"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/voygr-location-data-validation-and-enrichment"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:f06b8aed9803465a1c3841bed4434a8cd44bf61f357dfb6e344a86366b1c06d0"
---

# Voygr: Business Validation API for Apps, Agents & Analytics

AI agents and applications that work with real-world locations need more than pins on a map. They need to know if a business actually exists and whether it's still open.


Today we're excited to highlight[Voygr](https://voygr.tech/) , a location and POI data validation platform now available on Orthogonal.


## What is Voygr?


Voygr provides a high-fidelity location data layer for analytics, agents, and apps. Their Business Validation API confirms whether a business exists at a given address and whether it's currently open or closed, with up to 99.62% validation precision. Backed by Y Combinator and trusted by partners like Overture Maps, Voygr is built by former Google Maps and Apple Search leadership.


## How It Works


Voygr uses context-aware matching across multiple attributes and multi-source verification. Rather than relying on a single data source, it cross-references web, social, and authoritative data to catch closures, relocations, and rebrands that single-source systems miss.


## Using Voygr with Orthogonal


### Validate a Business Location


Send a business name and address, get back a validated existence and operating status.


` // Using @orth/sdk


// Install: npm install @orth/sdk


import


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


// Validate a business location


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


"voygr-business-validation"


,


path


:


"/v1/business-status"


,


body


:


{


name


:


"Blue Bottle Coffee"


,


address


:


"66 Mint St, San Francisco, CA 94103"


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


existence_status


)


;


// "exists", "not_exists", or "uncertain"


console


.


log


(


result


.


open_closed_status


)


;


// "open", "closed", or "uncertain"


console


.


log


(


result


.


validation_timestamp


)


;


// ISO 8601 UTC timestamp


`


### Using x402 Protocol


Voygr on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


` // Install: npm install x402-fetch viem


import


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


// Validate a business location with x402 payment


const


url


=


"https://x402.orth.sh/voygr-business-validation/v1/business-status"


;


const


response


=


await


fetchWithPayment


(


url


,


{


method


:


"POST"


,


headers


:


{


"Content-Type"


:


"application/json"


}


,


body


:


JSON


.


stringify


(


{


name


:


"Starbucks"


,


address


:


"1390 Market St, San Francisco, CA 94102"


}


)


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


// { success: true, existence_status: "exists", open_closed_status: "open", request_id: "abc123", validation_timestamp: "2026-03-13T12:00:00Z" }


`


## Use Cases


### Real Estate and Property Intelligence


Validate business presence at commercial addresses for property valuation, site selection, and investment analysis. Know which storefronts are occupied, which are vacant, and which have recently turned over.


### Retail and Sales Optimization


Identify where businesses are open, closed, or have relocated. Voygr's validation data makes it easier to build analytical models that organize sales efforts, visualize opportunities, and prioritize accounts.


### Insurance and Risk Scoring


Verify business existence and operating status for underwriting and claims. Reduce risk exposure from stale data by catching businesses that have closed or relocated.


### AI Agents and Autonomous Systems


Agents that interact with the physical world need to know what's actually there. Voygr provides the ground truth that enables agents to reason about places, not just coordinates.


## Why Agents Need Voygr


1. **99.62% validation precision** : Context-aware matching delivers high-confidence results
2. **Multi-source verification** : Cross-references web, social, and authoritative data to catch what single-source systems miss
3. **Simple API** : One endpoint, two fields in, validated status out
4. **Built by mapping veterans** : Founded by former Google Maps and Apple Search engineering leadership


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Voygr and dozens of other APIs. No API keys to manage, no accounts to create. Just instant access to powerful tools for your agents.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/voygr) |[Voygr Website](https://voygr.tech/)
