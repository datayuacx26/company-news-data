---
schema_version: "1.0.0"
document_id: "c6d76cad84603ac7119283f8c75f3ac3f203cfa507972d7ebfbb7fb5f2f8c5f9"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/leadsforge-lead-search-for-ai-agents"
published_at: null
first_seen_at: "2026-08-08T11:23:43.303389+00:00"
fetched_at: "2026-08-08T11:23:44.648008+00:00"
content_hash: "sha256:d3831cd2e0d34fc54cf00cf0af0a7bc45813e51bf8d4a7654d7c16a70a107458"
---

# Leadsforge: Lead Search for AI Agents

Prospecting agents need more than a search box. They need a way to describe an ICP, estimate the market, pull matching leads, enrich missing contact details, and hand clean records to whatever happens next.


Today we're excited to announce our partnership with[Leadsforge](https://www.leadsforge.ai/) , a conversational lead search and enrichment platform, now available on Orthogonal. Leadsforge brings lead search, lookalike company discovery, follower search, and contact enrichment behind APIs that agents can call directly.


## What is Leadsforge?


Leadsforge is built for teams that want to find and enrich B2B leads without living inside filter-heavy prospecting tools. Users can describe the companies and people they want, then let Leadsforge search across lead data and enrichment sources to return matched contacts and companies.


The API surface is practical for agents. They can count leads before spending time on a campaign, search for people by title, seniority, department, company profile, industry, location, employee range, revenue range, funding signals, and technology filters, preview lookalike companies from seed domains, enrich emails and phone numbers, and poll async enrichment jobs.


That matters because a prospecting agent should not stop at "find companies like this." It should estimate the audience, pull candidates, enrich the records it needs, and pass qualified leads to a CRM or outreach system.


## Using Leadsforge with Orthogonal


Leadsforge is available through the same Orthogonal SDK your agents already use. One Orthogonal key gives you access to Leadsforge alongside the rest of your GTM stack.


### Count an Audience


Count matching leads before creating a list. This is useful when an agent needs to tune ICP filters before pulling records.


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


audience


=


await


orthogonal


.


run


(


{


api


:


"leadsforge"


,


path


:


"/search/count"


,


method


:


"POST"


,


body


:


{


leadJobTitles


:


{


include


:


\[


"Head of Growth"


,


"VP Marketing"


\]


,


exactMatch


:


false


}


,


leadSeniorities


:


{


include


:


\[


"head"


,


"vp"


\]


}


,


companyIndustries


:


{


include


:


\[


"Software Development"


\]


}


,


companyEmployeeNumberRange


:


{


min


:


51


,


max


:


500


}


,


companyRequired


:


true


,


maxContactsPerCompany


:


2


}


}


)


;


console


.


log


(


audience


.


totalCount


)


;


`


### Search for Leads


Search returns lead records and a pagination cursor, so an agent can fetch a small first page, inspect quality, then continue only when the audience looks right.


` const


leads


=


await


orthogonal


.


run


(


{


api


:


"leadsforge"


,


path


:


"/search"


,


method


:


"POST"


,


body


:


{


limit


:


10


,


maxContactsPerCompany


:


1


,


companyRequired


:


true


,


leadJobTitles


:


{


include


:


\[


"Head of Growth"


\]


,


exactMatch


:


false


}


,


companyIndustries


:


{


include


:


\[


"Software Development"


\]


}


,


companyEmployeeNumberRange


:


{


min


:


51


,


max


:


500


}


}


}


)


;


console


.


log


(


leads


.


leads


)


;


console


.


log


(


leads


.


cursor


)


;


`


### Preview Lookalike Companies


Start from domains that look like good customers and ask Leadsforge for similar companies before building the people list.


` const


lookalikes


=


await


orthogonal


.


run


(


{


api


:


"leadsforge"


,


path


:


"/lookalikes/preview"


,


method


:


"POST"


,


body


:


{


domains


:


\[


"stripe.com"


\]


}


}


)


;


console


.


log


(


lookalikes


.


companies


)


;


`


### Enrich an Email


Once an agent has a lead candidate, it can enrich the email synchronously with a name and company domain.


` const


enriched


=


await


orthogonal


.


run


(


{


api


:


"leadsforge"


,


path


:


"/enrichment/email"


,


method


:


"POST"


,


body


:


{


firstName


:


"Alicia"


,


lastName


:


"North"


,


companyDomain


:


"northwindlabs.com"


}


}


)


;


console


.


log


(


enriched


.


email


)


;


`


### Poll Enrichment Jobs


Bulk email, phone, and LinkedIn enrichment endpoints return a job ID. Agents can poll the job and fetch results once it completes.


` const


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


"leadsforge"


,


path


:


"/enrichment/emails"


,


method


:


"POST"


,


body


:


{


people


:


\[


{


firstName


:


"Alicia"


,


lastName


:


"North"


,


companyDomain


:


"northwindlabs.com"


}


\]


}


}


)


;


const


status


=


await


orthogonal


.


run


(


{


api


:


"leadsforge"


,


path


:


\`


/enrichment/jobs/


${


job


.


jobID


}


\`


,


method


:


"GET"


}


)


;


console


.


log


(


job


.


jobID


)


;


console


.


log


(


status


.


status


)


;


`


## Using x402 Protocol


Leadsforge on Orthogonal also supports[x402](https://www.x402.org/) for native HTTP payments. Agents can pay for individual API calls with USDC, no separate API key required.


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


"https://x402.orthogonal.com/leadsforge/search/count"


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


companyRequired


:


true


,


leadJobTitles


:


{


include


:


\[


"Head of Growth"


\]


,


exactMatch


:


false


}


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


.


totalCount


)


;


`


## Using MPP


Leadsforge also works through[MPP](https://mpp.dev/) , the open standard for machine-to-machine payments co-authored by Tempo and Stripe. Agents can pay for Leadsforge calls with stablecoins, cards, or Bitcoin on the same endpoint.


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


"https://mpp.orthogonal.com/leadsforge/lookalikes/preview"


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


domains


:


\[


"stripe.com"


\]


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


.


companies


)


;


`


The` mppx` client handles the 402 challenge, signs a credential with your wallet, and retries with proof of payment.


## What Agents Can Do with Leadsforge


A prospecting agent can start with a target account, preview similar companies, count the matching audience, pull a page of leads, enrich missing emails or phone numbers, and write the results into a CRM. A RevOps agent can use count endpoints to size territories or campaign segments before anyone pays the cost of a full export.


Leadsforge is also useful for systems that already have outbound infrastructure. Instead of asking a human to build a CSV, an agent can generate the lead list, enrich contact fields, and pass qualified records downstream to the right campaign or sales workflow.


## Why We Partnered with Leadsforge


Agents need a way to turn market intent into usable lead records. Leadsforge gives them the discovery and enrichment layer: audience counts, people search, lookalike companies, company follower search, and contact enrichment. That fits Orthogonal well. Builders can connect data, payments, and action through one API layer, then pay per request instead of stitching together another one-off integration.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Leadsforge and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/leadsforge) |[Leadsforge Website](https://www.leadsforge.ai/)
