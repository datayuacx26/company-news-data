---
schema_version: "1.0.0"
document_id: "82dc1079fa324ee5bc25581562047fef33b76486fc140ff708012104f7704415"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/fullenrich-b2b-contact-data-for-agents"
published_at: null
first_seen_at: "2026-08-13T13:38:12.505397+00:00"
fetched_at: "2026-08-13T13:38:14.352554+00:00"
content_hash: "sha256:e8a3516f39812c19146820591befbb38e67bc74edc39bcdf82585ccbb9e6450a"
---

# FullEnrich: B2B Contact Data for AI Agents

Outbound agents are only as useful as the contact data they can reach. A list of names and companies is not enough. The agent still needs verified work emails, mobile numbers, current titles, company context, and a way to recover when one data source comes up empty.


Today we're excited to announce our partnership with[FullEnrich](https://fullenrich.com/) , a B2B email and phone waterfall enrichment platform now available on Orthogonal. FullEnrich gives agents access to contact enrichment, reverse email lookup, people search, people lookup, company search, and company lookup through one API.


## What is FullEnrich?


FullEnrich helps GTM teams find emails and phone numbers for the people they want to reach. Instead of depending on one static database, FullEnrich checks more than 20 data providers and verifies the result before returning it.


Their core API capabilities:


- **Contact Enrichment API** : Enrich up to 100 B2B contacts per batch using names, companies, domains, or professional network URLs.
- **Reverse Email Lookup API** : Identify the person and company behind one or more email addresses.
- **People Search API** : Search professionals by company, title, seniority, function, skills, location, and other filters.
- **People Lookup API** : Look up one person by professional network URL, profile ID, or name plus company.
- **Company Search API** : Search companies by name, domain, industry, headcount, location, company type, specialty, keyword, or founding year.
- **Company Lookup API** : Look up one company by domain, professional network URL, or professional network ID.


## Using FullEnrich with Orthogonal


FullEnrich on Orthogonal gives agents one interface for lead enrichment, account research, CRM cleanup, and reverse lookup workflows.


### Enrich a Batch of Contacts


Send up to 100 contacts with names and company identifiers. FullEnrich starts an asynchronous enrichment job and returns an` enrichment_id` .


` import


Orthogonal


from


"@orth/sdk"


;


const


apiKey


=


process


.


env


.


ORTHOGONAL_API_KEY


;


if


(


!


apiKey


)


{


throw


new


Error


(


"ORTHOGONAL_API_KEY environment variable is required"


)


;


}


const


orthogonal


=


new


Orthogonal


(


{


apiKey


,


}


)


;


const


job


=


await


orthogonal


.


run


(


{


api


:


"fullenrich"


,


path


:


"/contact/enrich/bulk"


,


method


:


"POST"


,


body


:


{


name


:


"founder-leads-august"


,


data


:


\[


{


first_name


:


"Benjamin"


,


last_name


:


"Douablin"


,


company_name


:


"FullEnrich"


,


enrich_fields


:


\[


"contact.work_emails"


,


"contact.personal_emails"


,


"contact.phones"


\]


,


custom


:


{


crm_id


:


"lead_123"


}


}


\]


}


}


)


;


console


.


log


(


job


.


data


.


enrichment_id


)


;


`


### Retrieve an Enrichment Result


Use the returned` enrichment_id` to check the job result. Webhooks are better for production, but polling is useful for tests and backfills.


` import


Orthogonal


from


"@orth/sdk"


;


const


apiKey


=


process


.


env


.


ORTHOGONAL_API_KEY


;


if


(


!


apiKey


)


{


throw


new


Error


(


"ORTHOGONAL_API_KEY environment variable is required"


)


;


}


const


orthogonal


=


new


Orthogonal


(


{


apiKey


}


)


;


const


enrichmentId


=


"YOUR_ENRICHMENT_ID"


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


"fullenrich"


,


path


:


\`


/contact/enrich/bulk/


${


enrichmentId


}


\`


,


method


:


"GET"


,


query


:


{


enrichment_id


:


enrichmentId


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


// Returns enriched contacts with verified emails, phone numbers, and company data.


`


### Reverse Lookup an Email


Start with one or more email addresses and identify the person and company behind them.


` import


Orthogonal


from


"@orth/sdk"


;


const


apiKey


=


process


.


env


.


ORTHOGONAL_API_KEY


;


if


(


!


apiKey


)


{


throw


new


Error


(


"ORTHOGONAL_API_KEY environment variable is required"


)


;


}


const


orthogonal


=


new


Orthogonal


(


{


apiKey


}


)


;


const


reverseLookup


=


await


orthogonal


.


run


(


{


api


:


"fullenrich"


,


path


:


"/contact/reverse/email/bulk"


,


method


:


"POST"


,


body


:


{


name


:


"inbound-email-research"


,


data


:


\[


{


email


:


"founder@example.com"


}


\]


}


}


)


;


console


.


log


(


reverseLookup


.


data


.


enrichment_id


)


;


`


### Retrieve a Reverse Lookup Result


Use the returned` enrichment_id` from a reverse email lookup to poll for the completed person and company profile.


` import


Orthogonal


from


"@orth/sdk"


;


const


apiKey


=


process


.


env


.


ORTHOGONAL_API_KEY


;


if


(


!


apiKey


)


{


throw


new


Error


(


"ORTHOGONAL_API_KEY environment variable is required"


)


;


}


const


orthogonal


=


new


Orthogonal


(


{


apiKey


}


)


;


const


reverseEnrichmentId


=


"YOUR_REVERSE_ENRICHMENT_ID"


;


const


reverseResult


=


await


orthogonal


.


run


(


{


api


:


"fullenrich"


,


path


:


\`


/contact/reverse/email/bulk/


${


reverseEnrichmentId


}


\`


,


method


:


"GET"


,


query


:


{


enrichment_id


:


reverseEnrichmentId


}


}


)


;


console


.


log


(


reverseResult


.


data


)


;


`


### Search for People


Build a list of prospects by title, company, seniority, function, geography, or skills.


` import


Orthogonal


from


"@orth/sdk"


;


const


apiKey


=


process


.


env


.


ORTHOGONAL_API_KEY


;


if


(


!


apiKey


)


{


throw


new


Error


(


"ORTHOGONAL_API_KEY environment variable is required"


)


;


}


const


orthogonal


=


new


Orthogonal


(


{


apiKey


}


)


;


const


people


=


await


orthogonal


.


run


(


{


api


:


"fullenrich"


,


path


:


"/people/search"


,


method


:


"POST"


,


body


:


{


current_position_titles


:


\[


{


value


:


"Head of Growth"


}


\]


,


current_company_headcounts


:


\[


{


value


:


"51-200"


}


\]


,


person_locations


:


\[


{


value


:


"United States"


}


\]


,


limit


:


10


}


}


)


;


console


.


log


(


people


.


data


)


;


`


### Look Up a Person


Look up a single person by profile URL, profile ID, or a name plus company identifier before enriching contact details.


` import


Orthogonal


from


"@orth/sdk"


;


const


apiKey


=


process


.


env


.


ORTHOGONAL_API_KEY


;


if


(


!


apiKey


)


{


throw


new


Error


(


"ORTHOGONAL_API_KEY environment variable is required"


)


;


}


const


orthogonal


=


new


Orthogonal


(


{


apiKey


}


)


;


const


person


=


await


orthogonal


.


run


(


{


api


:


"fullenrich"


,


path


:


"/people/lookup"


,


method


:


"POST"


,


body


:


{


person_name


:


"Benjamin Douablin"


,


company_domain


:


"fullenrich.com"


}


}


)


;


console


.


log


(


person


.


data


)


;


`


### Look Up a Company


Enrich one account before routing it, scoring it, or finding the right people inside it.


` import


Orthogonal


from


"@orth/sdk"


;


const


apiKey


=


process


.


env


.


ORTHOGONAL_API_KEY


;


if


(


!


apiKey


)


{


throw


new


Error


(


"ORTHOGONAL_API_KEY environment variable is required"


)


;


}


const


orthogonal


=


new


Orthogonal


(


{


apiKey


}


)


;


const


company


=


await


orthogonal


.


run


(


{


api


:


"fullenrich"


,


path


:


"/company/lookup"


,


method


:


"POST"


,


body


:


{


domain


:


"fullenrich.com"


}


}


)


;


console


.


log


(


company


.


data


)


;


`


### Search for Companies


Find accounts that match a market, industry, headcount, geography, or keyword.


` import


Orthogonal


from


"@orth/sdk"


;


const


apiKey


=


process


.


env


.


ORTHOGONAL_API_KEY


;


if


(


!


apiKey


)


{


throw


new


Error


(


"ORTHOGONAL_API_KEY environment variable is required"


)


;


}


const


orthogonal


=


new


Orthogonal


(


{


apiKey


}


)


;


const


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


"fullenrich"


,


path


:


"/company/search"


,


method


:


"POST"


,


body


:


{


industries


:


\[


{


value


:


"Software Development"


}


\]


,


headcounts


:


\[


{


value


:


"51-200"


}


\]


,


headquarters_locations


:


\[


{


value


:


"United States"


}


\]


,


limit


:


10


}


}


)


;


console


.


log


(


companies


.


data


)


;


`


### Using x402 Protocol


FullEnrich on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that lets agents pay for API calls directly with USDC stablecoins. No API key handoff. No separate account setup.


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


"https://x402.orthogonal.com/fullenrich/people/search"


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


current_position_titles


:


\[


{


value


:


"VP Sales"


}


\]


,


current_company_domains


:


\[


{


value


:


"stripe.com"


}


\]


,


limit


:


10


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


FullEnrich on Orthogonal also supports[MPP](https://mpp.dev/) , the open standard for machine-to-machine payments co-authored by Tempo and Stripe. Your agents can pay for API calls with stablecoins, cards, or Bitcoin, with no API key required.


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


"https://mpp.orthogonal.com/fullenrich/company/lookup"


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


domain


:


"fullenrich.com"


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


## Data Quality and Coverage


FullEnrich is built around waterfall enrichment. It checks more than 20 premium data sources, then verifies the contact data before returning it. Their homepage reports an 80%+ find rate, triple email verification, direct and mobile phone numbers, and GDPR and CCPA compliance.


For agents, the important part is reliability. A single-source lookup often fails silently. A waterfall gives the agent more chances to find a reachable email or phone number without adding vendor-specific retry logic to your product.


## Use Cases


### Outbound Prospecting


Search for accounts, find the right people, then enrich those contacts with verified work emails and mobile numbers before sending them to your outreach system.


### Inbound Lead Research


Start with an email address from a form fill, support ticket, or product signup. Reverse lookup the sender, identify the company, and decide whether the lead should go to sales, support, or an automated follow-up.


### CRM Enrichment


Fill missing contact fields in HubSpot, Salesforce, or your own CRM. Use webhooks to update records as soon as enrichment jobs finish.


### Recruiting


Search for candidates by role, seniority, company, skills, and location. Look up the best matches and enrich them before sourcing.


### Account-Based Workflows


Search companies by industry, headcount, location, and keywords. Use company lookup to keep account records fresh before routing, scoring, or researching them.


## Why We Partnered with FullEnrich


Agents need contact data they can act on. FullEnrich combines search, lookup, reverse lookup, and waterfall enrichment in a way that maps cleanly onto GTM workflows. That makes it a strong fit for Orthogonal: one key, one bill, and one way for agents to call the data they need.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try FullEnrich and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/fullenrich) |[FullEnrich Website](https://fullenrich.com/)
