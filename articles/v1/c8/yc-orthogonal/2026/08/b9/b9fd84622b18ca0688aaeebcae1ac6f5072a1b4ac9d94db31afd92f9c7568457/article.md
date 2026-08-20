---
schema_version: "1.0.0"
document_id: "b9fd84622b18ca0688aaeebcae1ac6f5072a1b4ac9d94db31afd92f9c7568457"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/abstractapi-automation-apis-for-agents"
published_at: null
first_seen_at: "2026-08-07T07:17:12.335258+00:00"
fetched_at: "2026-08-07T07:17:14.056845+00:00"
content_hash: "sha256:3c4c3a25467fe0926ad41bcf3828edc95f1c633c7b1ee1b8aa826fcf08a27343"
---

# AbstractAPI: Utility Data APIs for Agents

Agents that interact with the real world need clean data. What company is behind this domain? What is the current exchange rate? What time is it for this user? Is this VAT number valid? These are simple questions, but answering them reliably requires specialized infrastructure that most teams don't want to build.


Today we're excited to announce our partnership with[AbstractAPI](https://www.abstractapi.com/) , a platform that provides a full suite of data quality and enrichment APIs trusted by 10,000+ developers and teams at companies like Google, Salesforce, LinkedIn, and Wells Fargo. AbstractAPI handles the undifferentiated data work so your agents can focus on decisions.


## What is AbstractAPI?


AbstractAPI offers a set of focused, production-grade APIs that each solve one problem well. Their APIs are RESTful, return JSON, and are backed by SOC 2 Type II compliance, GDPR readiness, and a 99.99% uptime SLA.


Their core products:


- **Email Reputation API** : Assess email reputation, detect risky senders, check deliverability signals, and surface breach context.
- **Phone Intelligence API** : Retrieve carrier, location, messaging, registration, risk, and breach data for phone numbers.
- **IP Intelligence API** : Detect VPNs, proxies, TOR usage, abuse potential, hosting services, relays, and mobile IPs.
- **Company Enrichment API** : Submit a domain or email and get company data such as name, industry, headcount, location, and social profiles.
- **Web Scraping API** : Extract data from websites for agents that need page content without running their own scraping stack.
- **Website Screenshot API** : Capture screenshots of URLs or HTML for monitoring, thumbnails, and visual verification.
- **Exchange Rates API** : Look up live and historical exchange rates for 80+ currencies and convert amounts.
- **Public Holidays API** : Get holidays by country and date.
- **Timezone API** : Get current time for a location or convert time between locations.
- **IBAN Validation API** : Validate International Bank Account Numbers and get associated bank details.
- **VAT Validation API** : Validate VAT numbers and retrieve VAT rate data.
- **Image Processing API** : Compress, convert, and optimize images via URL or upload.


## Using AbstractAPI with Orthogonal


AbstractAPI is available through the same Orthogonal SDK your agents already use. One Orthogonal key gives you access to AbstractAPI alongside the rest of your API stack.


### Company Enrichment


Turn a domain into a full company profile. Useful for lead qualification, CRM enrichment, and account research.


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


"abstractapi/company-enrichment"


,


path


:


"/v2"


,


method


:


"GET"


,


query


:


{


api_key


:


"orthogonal"


,


domain


:


"stripe.com"


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


company


.


company_name


)


;


console


.


log


(


company


.


industry


)


;


console


.


log


(


company


.


linkedin_url


)


;


`


### Exchange Rates


Get current currency exchange rates for pricing, billing, and financial reporting workflows.


` const


rates


=


await


orthogonal


.


run


(


{


api


:


"abstractapi/exchange-rates"


,


path


:


"/v1/live"


,


method


:


"GET"


,


query


:


{


api_key


:


"orthogonal"


,


base


:


"USD"


,


target


:


"EUR"


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


rates


.


base


)


;


console


.


log


(


rates


.


exchange_rates


.


EUR


)


;


`


### Time Zone Lookup


Resolve a location into local time and timezone metadata for scheduling and international workflows.


` const


tokyo


=


await


orthogonal


.


run


(


{


api


:


"abstractapi/timezone"


,


path


:


"/v1/current_time"


,


method


:


"GET"


,


query


:


{


api_key


:


"orthogonal"


,


location


:


"35.6762,139.6503"


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


tokyo


.


datetime


)


;


console


.


log


(


tokyo


.


timezone_location


)


;


console


.


log


(


tokyo


.


timezone_abbreviation


)


;


`


### VAT Validation


Validate VAT numbers and return associated company details for compliance workflows.


` const


vat


=


await


orthogonal


.


run


(


{


api


:


"abstractapi/vat-validation"


,


path


:


"/v1/validate"


,


method


:


"GET"


,


query


:


{


api_key


:


"orthogonal"


,


vat_number


:


"IE6388047V"


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


vat


.


valid


)


;


console


.


log


(


vat


.


company


.


name


)


;


console


.


log


(


vat


.


country


.


name


)


;


`


### Using x402 Protocol


AbstractAPI on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins with no separate AbstractAPI account setup.


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


params


=


new


URLSearchParams


(


{


api_key


:


"orthogonal"


,


domain


:


"stripe.com"


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


https://x402.orthogonal.com/abstractapi/company-enrichment/v2?


${


params


}


\`


,


{


method


:


"GET"


,


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


### Using MPP


AbstractAPI also works through[MPP](https://mpp.dev/) , the open standard for machine-to-machine payments co-authored by Tempo and Stripe. Agents can pay for AbstractAPI calls with stablecoins, cards, or Bitcoin on the same endpoint.


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


api_key


:


"orthogonal"


,


domain


:


"stripe.com"


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


https://mpp.orthogonal.com/abstractapi/company-enrichment/v2?


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


## Enterprise-Grade Infrastructure


AbstractAPI is built for teams that can't afford downtime or bad data:


- **99.99% uptime SLA** backed by a global architecture with redundant service across regions
- **SOC 2 Type II** certified with GDPR compliance baked in
- **Millisecond response times** across all endpoints
- **Trusted by enterprise** including Google, Salesforce, LinkedIn, PepsiCo, Wells Fargo, United Airlines, and Paramount


## Use Cases


### Lead Verification Pipeline


Before your agent adds a company to a campaign, enrich the domain to understand its name, size, industry, and social profile. Use that context to route the account, personalize outreach, or decide whether it belongs in the campaign at all.


### Account Enrichment


A new company domain enters your CRM. Enrich it to get company context, then route it to the right sales team based on size, industry, and geography.


### Compliance and KYC


Validate IBAN numbers for banking workflows, verify VAT numbers for cross-border transactions, and resolve geographic context for account records. AbstractAPI covers the data layer so your compliance agents can focus on policy logic.


### Utility Data for Agents


Convert currencies for international reporting, resolve local times for scheduled operations, and enrich company records for market monitoring. These utility APIs fill the gaps that most data providers ignore.


## Why We Partnered with AbstractAPI


Most enrichment platforms focus on one vertical: either contacts, or companies, or fraud. AbstractAPI covers a different surface area. Their APIs handle the utility data questions that come up across every workflow: what company is behind this domain, what time is it in Tokyo, what is the current exchange rate, is this VAT number valid. These are the building blocks that agents need to operate reliably. The fact that they serve enterprise customers at 99.99% uptime with SOC 2 compliance means your agents can depend on them in production.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try AbstractAPI and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/abstractapi) |[AbstractAPI Website](https://www.abstractapi.com/)
