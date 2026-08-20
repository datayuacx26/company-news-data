---
schema_version: "1.0.0"
document_id: "a4a7da249c6c893e75fce827c39625b22926679ac1524a7eae3b6cf47c4cce93"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/findymail-verified-email-finder-for-agents"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:0ef7985f563a5eea9777b366ea16413a1898e8e99ace0288b090a8a5f029d2a5"
---

# Findymail: Verified Email Finder with Sub-5% Bounce Guarantee

Finding the right email address shouldn't be a multi-step process. Most prospecting tools make you find an email with one service, then verify it with another, burning two sets of credits and hoping the data is still fresh by the time you hit send. For AI agents running outbound workflows, this fragmentation means more API calls, more failure modes, and worse deliverability.


Today we're excited to announce our partnership with[Findymail](https://www.findymail.com/) , an email finder and verification platform that combines both steps into a single API call. One credit finds and verifies an email, with a guaranteed bounce rate under 5%. Findymail also offers phone enrichment, AI-powered lead search with Intellimatch, and bulk processing with up to 300 concurrent requests.


## What is Findymail?


Findymail is a B2B email discovery and verification tool built for outbound sales teams and developer workflows. Instead of paying for a finder and a verifier separately, Findymail runs both in a single step using proprietary algorithms that detect catch-all addresses and validate deliverability before returning a result.


Core capabilities:


- **Email Finder** : Find verified B2B emails from a name and domain. Discovery and verification happen in the same request, one credit per lookup.
- **Email Verifier** : Standalone verification for external lists. Catches invalid, disposable, and catch-all addresses with a sub-5% bounce guarantee.
- **Phone Finder** : Direct phone numbers for non-EU contacts. Works alongside email lookups for full contact enrichment.
- **Intellimatch AI Search** : Describe your ideal customer in natural language and get back matching contacts. No manual filter configuration needed.
- **Domain Search** : Find all emails associated with a domain. Useful for mapping out contacts at target accounts.
- **Employee Search** : Find people at a specific company with role and department filtering.
- **Bulk Processing** : CSV upload and batch enrichment for email, phone, and company data at scale.
- **REST API** : 300 concurrent requests, Bearer token auth, webhook support. Endpoints for search, verify, reverse lookup, and company enrichment.


## Using Findymail with Orthogonal


### Find a Verified Email


Pass a name and domain. Get back a verified email in one step.


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


"findymail"


,


path


:


"/api/search/name"


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


"Jane Doe"


,


domain


:


"acme.com"


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


contact


)


;


// Returns: verified email, domain, and contact name


`


### Verify an Email


Check if an existing email is valid before sending. Catches disposable addresses, catch-alls, and invalid mailboxes.


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


"findymail"


,


path


:


"/api/verify"


,


method


:


"POST"


,


body


:


{


email


:


"jane@acme.com"


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


// Returns: email, verified status, provider info, catch-all detection


`


### Search by Domain


Find all known email addresses associated with a domain.


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


"findymail"


,


path


:


"/api/search/domain"


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


"acme.com"


,


roles


:


\[


"CEO"


,


"CTO"


\]


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


// Returns: matching contacts with verified emails at the domain


`


### Find Employees at a Company


Find people at a company by job title. Returns names and LinkedIn profiles. Use the email finder on matched contacts to get verified emails.


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


"findymail"


,


path


:


"/api/search/employees"


,


method


:


"POST"


,


body


:


{


website


:


"acme.com"


,


job_titles


:


\[


"VP of Sales"


,


"VP of Marketing"


\]


,


count


:


3


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


// Returns: matching employees with name, LinkedIn URL, and company info


`


### Intellimatch AI Lead Search


Describe your ICP in plain language. Findymail's AI finds matching contacts.


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


"findymail"


,


path


:


"/api/intellimatch/search"


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


"VP of Sales at SaaS companies with 50-200 employees in the US"


}


}


)


;


// Poll for results using the returned hash


const


data


=


await


orthogonal


.


run


(


{


api


:


"findymail"


,


path


:


"/api/intellimatch/data"


,


method


:


"GET"


,


query


:


{


hash


:


result


.


hash


}


}


)


;


console


.


log


(


data


)


;


// Returns: matching contacts with verified emails and company data


`


### Using x402 Protocol


Findymail on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


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


"https://x402.orthogonal.com/findymail/api/search/name"


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


"Jane Doe"


,


domain


:


"acme.com"


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


Findymail on Orthogonal is also available via[MPP](https://mpp.dev/) , the open standard for machine-to-machine payments co-authored by Tempo and Stripe. MPP supports stablecoins, cards, and Bitcoin on the same endpoint.


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


"https://mpp.orthogonal.com/findymail/api/search/name"


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


"Jane Doe"


,


domain


:


"acme.com"


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


The` mppx` client handles 402 challenges automatically: it intercepts the payment request, signs a credential with your wallet, and retries with proof of payment.


## Key Differentiators


Findymail stands out from other email tools in a few ways:


- **Find + verify in one credit** : Most tools charge separately for discovery and verification. Findymail combines both, so you never pay to find an email that bounces.
- **Sub-5% bounce guarantee** : Not a target, a guarantee. If your bounce rate exceeds 5%, they credit you back.
- **Catch-all detection** : Proprietary algorithms identify catch-all domains and still validate deliverability, something most verifiers skip.
- **EU-hosted** : Data residency in the EU for teams with compliance requirements.
- **Credit rollover** : Unused credits carry forward up to 2x your monthly allocation. No use-it-or-lose-it pressure.


## Use Cases


### Outbound Prospecting


Find verified emails for target accounts in a single API call. Feed results directly into Smartlead, Instantly, or Lemlist through native integrations. One credit per contact, no separate verification step.


### CRM Enrichment


When a new lead enters your CRM with just a name and company, enrich it with a verified email and phone number. Findymail's API returns results fast enough for real-time enrichment in HubSpot, Salesforce, or Pipedrive workflows.


### AI Agent Workflows


Agents running multi-step prospecting can use Findymail to go from a target company to verified contact info in one call. Combine with Intellimatch to find ICP-matching contacts without manually configuring search filters. The 300-concurrent-request limit means agents can process large lists without throttling.


### List Cleaning


Import existing contact lists and verify them in bulk before a campaign. Catch-all detection and bounce prediction prevent wasted sends and protect your sender reputation.


## Why We Partnered with Findymail


The combined find-and-verify model is exactly what AI agents need. Agents shouldn't have to orchestrate separate finder and verifier APIs, handle the edge cases when they disagree, and manage two sets of credits. Findymail collapses that into one step with a deliverability guarantee. The REST API handles 300 concurrent requests with straightforward Bearer auth, which makes it easy to integrate and hard to break. For teams running outbound at scale through agents, fewer moving parts and guaranteed data quality is the right tradeoff.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Findymail and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/findymail) |[Findymail Website](https://www.findymail.com/)
