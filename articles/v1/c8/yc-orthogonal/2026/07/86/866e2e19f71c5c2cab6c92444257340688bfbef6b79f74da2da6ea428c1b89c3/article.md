---
schema_version: "1.0.0"
document_id: "866e2e19f71c5c2cab6c92444257340688bfbef6b79f74da2da6ea428c1b89c3"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/rocketreach-contact-company-data-for-agents"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:f553061e23dfb49778983fd64ace2748842628d444e1e02f5b911b3729c4dcae"
---

# RocketReach: Contact and Company Data for AI Agents

Finding the right person is still one of the slowest parts of sales, recruiting, and market research. You need a real profile, the company behind it, and enough contact data to act. Most teams solve this with manual search, browser tabs, CSV exports, and a few brittle scripts.


Today we're excited to announce our partnership with[RocketReach](https://rocketreach.co/) , a lead intelligence platform for finding professional emails, phone numbers, social profiles, and company data. RocketReach is now available on Orthogonal, so your agents can search people and companies through the same API layer they already use for the rest of their workflow.


## What is RocketReach?


RocketReach helps sales, recruiting, and growth teams find professional contact and company data. Their API lets you search and retrieve contact information for 700M+ professionals and 35M companies.


Their core API capabilities:


- **People Search API** : Search people by name, title, company, location, keyword, and other criteria.
- **Company Search API** : Search companies by name, domain, industry, location, employee count, revenue, and more.
- **People and Company Lookup API** : Look up a person and their company data in one request using identifiers like email, LinkedIn URL, name plus employer, phone, or RocketReach profile ID.
- **Lookup Status and Webhooks** : Retrieve completed lookup results through status checks or webhook callbacks when enrichment takes longer.


## Using RocketReach with Orthogonal


### Search for People


Find prospects by role, company, location, or keyword. Search returns matching profiles, then you can use lookup to enrich the exact person.


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


profiles


=


await


orthogonal


.


run


(


{


api


:


"rocketreach"


,


path


:


"/person/search"


,


method


:


"POST"


,


body


:


{


query


:


{


current_title


:


\[


"VP of Sales"


,


"Head of Growth"


\]


,


current_employer


:


\[


"Stripe"


\]


,


location


:


\[


"San Francisco"


\]


}


,


page_size


:


10


,


order_by


:


"popularity"


}


}


)


;


console


.


log


(


profiles


)


;


// Returns matching professional profiles for follow-up lookup.


`


### Search for Companies


Build account lists by industry, size, geography, domain, or keywords.


` const


companies


=


await


orthogonal


.


run


(


{


api


:


"rocketreach"


,


path


:


"/searchCompany"


,


method


:


"POST"


,


body


:


{


query


:


{


industry


:


\[


"Computer Software"


\]


,


employees


:


\[


"51-200"


\]


,


geo


:


\[


"United States"


\]


}


,


page_size


:


10


,


order_by


:


"relevance"


}


}


)


;


console


.


log


(


companies


)


;


// Returns matching companies with firmographic details.


`


### Look Up a Person and Company


Start with a LinkedIn URL, email, phone, RocketReach profile ID, or name plus employer. Get the person and company data in one response.


` const


profile


=


await


orthogonal


.


run


(


{


api


:


"rocketreach"


,


path


:


"/profile-company/lookup"


,


method


:


"GET"


,


query


:


{


linkedin_url


:


"https://www.linkedin.com/in/jamesgullbrand"


,


lookup_type


:


"standard"


}


}


)


;


console


.


log


(


profile


)


;


// Returns profile data and associated company data.


`


You can also look up by name and employer:


` const


profile


=


await


orthogonal


.


run


(


{


api


:


"rocketreach"


,


path


:


"/profile-company/lookup"


,


method


:


"GET"


,


query


:


{


name


:


"Jamie Gullbrand"


,


current_employer


:


"RocketReach"


,


lookup_type


:


"standard"


}


}


)


;


`


### Using x402 Protocol


RocketReach on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that lets agents pay for API calls directly with USDC stablecoins. No API key handoff. No separate account setup.


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


account


=


privateKeyToAccount


(


process


.


env


.


PRIVATE_KEY


as


\`


0x


${


string


}


\`


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


"https://x402.orthogonal.com/rocketreach/person/search"


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


query


:


{


current_title


:


\[


"VP of Sales"


\]


,


current_employer


:


\[


"Stripe"


\]


}


,


page_size


:


5


,


order_by


:


"popularity"


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


`


### Using MPP (Machine Payments Protocol)


RocketReach on Orthogonal also supports[MPP](https://mpp.dev/) , the open standard for machine-to-machine payments co-authored by Tempo and Stripe. Agents can pay for RocketReach calls with stablecoins, cards, or Bitcoin on the same endpoint.


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


"https://mpp.orthogonal.com/rocketreach/person/search"


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


query


:


{


current_title


:


\[


"VP of Sales"


\]


,


current_employer


:


\[


"Stripe"


\]


}


,


page_size


:


5


,


order_by


:


"popularity"


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


`


The` mppx` client handles the 402 challenge, signs a credential with your wallet, and retries with proof of payment.


## Data Coverage


RocketReach covers 700M+ professional profiles and 35M companies. People search can filter by names, titles, employers, locations, keywords, and other profile fields. Company search can filter by domains, industries, geographies, employee counts, descriptions, and related company attributes.


For contact enrichment, RocketReach supports identifiers like email, phone, LinkedIn URL, RocketReach profile ID, and name plus employer. That makes it useful both for net-new prospecting and for filling gaps in existing CRM records.


## Use Cases


### Outbound Prospecting


Search for buyers by role, company, and location. Enrich the best matches with contact and company data, then send them into your outreach workflow.


### Recruiting


Find candidates by current title, employer, location, and keywords. Use profile lookup to get more context before sourcing or outreach.


### CRM Enrichment


Start with an email, LinkedIn URL, or name plus employer. Fill in missing profile and company fields before routing, scoring, or assigning leads.


### Account Research


Search companies by industry, size, and geography. Build target account lists, then find the right people at each company.


### Agentic GTM Workflows


Give an agent one path from account discovery to person search to contact enrichment. It can build lists, choose the right people, and prepare outreach without switching tools.


## Why We Partnered with RocketReach


Agents need contact data they can act on. RocketReach brings a large people and company dataset together with search and lookup APIs that map cleanly onto GTM workflows. That fits Orthogonal well: one integration, one bill, and one way for agents to call the data they need.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try RocketReach and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/rocketreach) |[RocketReach Website](https://rocketreach.co/)
