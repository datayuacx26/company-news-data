---
schema_version: "1.0.0"
document_id: "bbd17d554fa148e3cb9b8f323d79fed653468f26a71a8cba9d015f737e6f409e"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/signalbase-real-time-gtm-signals-for-agents"
published_at: null
first_seen_at: "2026-08-01T00:11:23.509993+00:00"
fetched_at: "2026-08-01T00:11:25.201569+00:00"
content_hash: "sha256:ec6ea0a41291555151a63e588e707758c4c9b6d997141770098cbd4f79975ad3"
---

# Signalbase: Real-Time GTM Signals for AI Agents

Most GTM systems work from old data. A rep reaches out after the funding round is already cold. A recruiting workflow uses last quarter's title. A research agent scores an account before the company starts hiring.


The problem is not effort. It is signal latency.


Today we're excited to announce our partnership with[Signalbase](https://www.trysignalbase.com/) , a real-time signal layer for GTM teams, now available on Orthogonal. Signalbase sources, verifies, and normalizes raw market events into structured company intelligence your agents can use right away.


## What is Signalbase?


Signalbase turns live market events into API-ready account intelligence. It tracks funding rounds, acquisitions, hiring signals, job changes, investors, companies, and people. Each signal is verified at the source, enriched with context, and connected back to the company profile behind the event.


That matters because a signal is only useful if an agent can act on it with enough context. A funding round by itself is a trigger. A funding round with company size, investors, source links, open roles, and relevant people becomes a workflow.


Signalbase is built API-first. You can query the data directly, poll paginated results, or run CSV enrichment jobs against account lists you already have.


## Using Signalbase with Orthogonal


Signalbase is available through the same Orthogonal SDK your agents already use. One Orthogonal key gives you access to Signalbase alongside the rest of your API stack.


### Find Recently Funded Companies


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


"signalbase"


,


path


:


"/signals/funding"


,


method


:


"GET"


,


query


:


{


date_preset


:


"last_30d"


,


countries


:


"US"


,


round


:


"seed,series a"


,


sort_by


:


"occurred_at"


,


sort_order


:


"desc"


,


limit


:


25


,


}


,


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


`


Use this when an agent needs a fresh target account list. The result includes funding details, company context, investors, source data, and pagination metadata.


### Detect Hiring Momentum


` const


hiring


=


await


orthogonal


.


run


(


{


api


:


"signalbase"


,


path


:


"/signals/hiring"


,


method


:


"GET"


,


query


:


{


countries


:


"US,GB"


,


departments


:


"sales"


,


seniorities


:


"manager,director,head"


,


date_preset


:


"last_14d"


,


limit


:


50


,


}


,


}


)


;


console


.


log


(


hiring


.


data


)


;


`


Hiring signals help agents spot team expansion before it shows up in a stale database. A GTM agent can route accounts with new sales roles to outbound, while a recruiting agent can watch for companies building teams around a new function.


### Track Job Changes


` const


jobChanges


=


await


orthogonal


.


run


(


{


api


:


"signalbase"


,


path


:


"/signals/job-changes"


,


method


:


"GET"


,


query


:


{


new_role


:


"VP Sales"


,


countries


:


"US"


,


date_preset


:


"last_30d"


,


limit


:


20


,


}


,


}


)


;


console


.


log


(


jobChanges


.


data


)


;


`


Job changes are one of the clearest moments to reach out. Signalbase returns role movement data with the matched company context, so agents can identify new decision makers and explain why the timing matters.


### Search Companies


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


"signalbase"


,


path


:


"/companies"


,


method


:


"GET"


,


query


:


{


search


:


"AI infrastructure"


,


countries


:


"US"


,


employee_count_min


:


25


,


employee_count_max


:


500


,


sort_by


:


"employee_count"


,


sort_order


:


"desc"


,


limit


:


25


,


}


,


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


The Companies API lets agents browse company profiles independently from the signal feed. Use it to build account lists, enrich a CRM, or check whether a company matches your ICP before taking action.


### Enrich a CSV List


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


"signalbase"


,


path


:


"/csv-enrichment"


,


method


:


"POST"


,


body


:


{


wait


:


true


,


companies


:


\[


{


name


:


"Acme"


,


domain


:


"acme.com"


}


,


{


name


:


"Northwind"


,


linkedin_url


:


"linkedin.com/company/northwind"


}


,


\]


,


}


,


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


)


;


`


CSV enrichment is useful when you already have a target account list. Signalbase adds funding, acquisition, hiring, job-change, and investor intelligence so an agent can decide which accounts deserve attention now.


### Discover People


` const


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


"signalbase"


,


path


:


"/people"


,


method


:


"GET"


,


query


:


{


title


:


"VP of Sales"


,


country


:


"US"


,


signal_type


:


"funding_round,job_change"


,


signal_date_range


:


"6m"


,


limit


:


25


,


}


,


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


The People API lets agents find operators and buyers connected to recent signal activity. Use it when the workflow needs both the account event and the person most likely to care.


## Using x402 Protocol


Signalbase on Orthogonal supports[x402](https://www.x402.org/) for native HTTP payments. Agents can pay for individual API calls with USDC, no separate API key required.


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


params


=


new


URLSearchParams


(


{


date_preset


:


"last_30d"


,


countries


:


"US"


,


round


:


"series a"


,


limit


:


"10"


,


}


)


;


const


response


=


await


fetchWithPayment


(


\`


https://x402.orthogonal.com/signalbase/signals/funding?


${


params


}


\`


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


## Using MPP


Signalbase also works through[MPP](https://mpp.dev/) , the open standard for machine-to-machine payments co-authored by Tempo and Stripe. Agents can pay for Signalbase calls with stablecoins, cards, or Bitcoin on the same endpoint.


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


params


=


new


URLSearchParams


(


{


countries


:


"US"


,


date_preset


:


"last_14d"


,


limit


:


"10"


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


\`


https://mpp.orthogonal.com/signalbase/signals/hiring?


${


params


}


\`


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


The` mppx` client handles the 402 challenge, signs a credential with your wallet, and retries with proof of payment.


## What Agents Can Do with Signalbase


Signalbase gives agents a live view of market movement. A sales agent can find companies that just raised a seed or Series A round and generate outreach while the event is still fresh. A RevOps agent can refresh CRM records when an account starts hiring or when a buyer changes roles. A recruiting agent can watch for companies expanding specific teams and route targets by department, location, or seniority.


It is also useful for investor and market research. Agents can track funding trends, map active investors, watch acquisition activity, and keep an internal company graph current without building a sourcing and verification system from scratch.


## Why We Partnered with Signalbase


Agents do better work when they know what changed. Static company data answers who a company is. Signalbase answers what happened, when it happened, and why it matters.


That is a natural fit for Orthogonal. We want agents to call the right API at the right moment, pay for only what they use, and act from current information. Signalbase brings the event layer GTM agents need.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Signalbase and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/signalbase) |[Signalbase Website](https://www.trysignalbase.com/)
