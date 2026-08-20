---
schema_version: "1.0.0"
document_id: "c208a7ab776aa9ba9351e9568d18914e724be4af2d1eb24738bcb17f734496b7"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/icypeas-email-enrichment-for-agents"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:fe81d5735396e5676a17facf67c9b629df349f53657b92c657692547bc31e824"
---

# Icypeas: Email Enrichment That Doesn't Break the Bank

Most email enrichment tools fall into one of two traps. Either they're bloated all-in-one platforms that charge enterprise prices for features you'll never use, or they're cheap but unreliable, filling your pipeline with bounced emails and wasted outreach. For AI agents running outbound workflows, neither option works. Agents need accurate data, fast APIs, and economics that don't blow through budgets on verification alone.


Today we're excited to announce our partnership with[Icypeas](https://www.icypeas.com/) , a specialist email enrichment platform trusted by Clay, Lemlist, Apollo, Instantly, LaGrowthMachine, and PhantomBuster. Icypeas focuses on doing a few things exceptionally well: finding professional emails, verifying them, scanning domains, and searching people and company databases. Their email discovery delivers the lowest bounce rate on the market with one of the highest find rates, and they do it at 3 to 10x cheaper than Wiza, Findymail, and Dropcontact.


## What is Icypeas?


Icypeas is a B2B email enrichment platform built for sales, marketing, and GTM teams who need reliable contact data without paying for a bloated suite. Founded by Pierre Landoin, who brings over 10 years of lead generation expertise, Icypeas takes the specialist approach: a focused set of features that outperform generalist alternatives.


Their core products:


- **Email Finder** : Find professional email addresses from a person's first name, last name, and company domain. Returns emails with confidence scores (ultra_sure, very_sure, probable, risky) and MX provider detection.
- **Email Verification** : Verify whether an email address exists and is deliverable. Catches invalid, disposable, and catch-all addresses before they bounce.
- **Domain Scan** : Discover all role-based email addresses for a domain (support@, sales@, info@, hr@). Useful for finding the right team to contact at any organization.
- **People Search** : Search a database of professionals by job title, seniority, company size, location, and more. Returns contact details at 0.02 credits per result.
- **Company Search** : Search companies by industry, employee count, address, keywords, and domain. Build target account lists programmatically.
- **LinkedIn Profile Scraping** : Scrape full LinkedIn profiles including work history, education, headline, and contact info.
- **LinkedIn Company Scraping** : Get employee count, industry, revenue, growth metrics, founded year, and address for any LinkedIn company page.
- **Reverse Email Lookup** : Find the LinkedIn profile URL behind any professional email address.
- **Bulk Search** : Run up to 5,000 email searches, verifications, or domain scans in a single API call with webhook notifications on completion.


## Using Icypeas with Orthogonal


### Email Finder


Find a professional email from a name and company domain. Results include confidence scores so your agent knows when to send and when to verify first.


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


"icypeas"


,


path


:


"/api/email-search"


,


method


:


"POST"


,


body


:


{


firstname


:


"Pierre"


,


lastname


:


"Landoin"


,


domainOrCompany


:


"icypeas.com"


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


// Returns: email, certainty (ultra_sure/very_sure/probable/risky),


// mxRecords, mxProvider, and more


`


### Email Verification


Verify an email before sending. Catches bounces before they hurt your domain reputation.


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


"icypeas"


,


path


:


"/api/email-verification"


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


"pierre.landoin@icypeas.com"


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


// Returns: verification status, deliverability score,


// catch-all detection, and MX records


`


### Domain Scan


Find all role-based email addresses for any domain. Useful when you know the company but not the person.


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


"icypeas"


,


path


:


"/api/domain-search"


,


method


:


"POST"


,


body


:


{


domainOrCompany


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


)


;


// Returns: contact@, info@, hr@, careers@, sales@, support@, etc.


`


### People Search


Search a database of professionals by job title, seniority, company size, and location. Build targeted prospect lists programmatically.


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


"icypeas"


,


path


:


"/api/find-people"


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


currentJobTitle


:


{


include


:


\[


"VP Sales"


,


"Head of Sales"


\]


}


,


currentCompanySize


:


\[


">= 50"


,


"<= 500"


\]


,


address


:


{


include


:


\[


"United States"


\]


}


}


,


size


:


50


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


// Returns: name, job title, company, email, phone,


// LinkedIn URL, and more for each matching person


`


### Using x402 Protocol


Icypeas on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


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


"https://x402.orthogonal.com/icypeas/api/email-search"


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


firstname


:


"Jane"


,


lastname


:


"Doe"


,


domainOrCompany


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


Icypeas on Orthogonal also supports[MPP](https://mpp.dev/) , the open standard for machine-to-machine payments co-authored by Tempo and Stripe. Your agents can pay for API calls with stablecoins, cards, or Bitcoin, with no API key required.


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


// Set up automatic 402 handling with your Tempo wallet


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


// Global fetch now handles 402 challenges automatically


const


response


=


await


fetch


(


"https://mpp.orthogonal.com/icypeas/api/email-search"


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


firstname


:


"Jane"


,


lastname


:


"Doe"


,


domainOrCompany


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


## Capabilities


Icypeas' API covers the full email enrichment workflow:


- **Email discovery** with confidence scoring (ultra_sure through risky) and MX provider detection
- **Email verification** with catch-all detection, the lowest bounce rate on the market
- **Domain scanning** for role-based addresses (support@, sales@, info@)
- **People database** searchable by 10+ filters including title, seniority, company size, and location
- **Company database** searchable by industry, employee count, revenue, and keywords
- **LinkedIn scraping** for both profiles and company pages
- **Bulk operations** for up to 5,000 items per request with webhook notifications
- **Async architecture** with polling and webhook-based result delivery
- **GDPR compliant** , using only open-source data with an opt-out system


## Use Cases


### Outbound Email Enrichment


Your agent identifies a prospect by name and company, calls the email finder to get a verified address with a confidence score, then decides whether to send immediately (ultra_sure) or verify first (probable/risky). One workflow, zero bounces.


### Enrichment Waterfall


Use Icypeas as a step in a multi-provider enrichment waterfall. Try the email finder first. If it returns risky or not_found, fall back to domain scan for a role-based address. Verify everything before sending. The cost per lead stays low because Icypeas only charges when it finds something.


### Lead Database Building


Combine people search and company search to build targeted prospect lists. Filter by title, seniority, company size, and geography. Enrich each result with verified emails. Export to your CRM or outreach tool.


### Pre-call Research


Before a sales call, use People Search to pull the prospect's profile, then enrich with email verification and domain scan. Feed this context to an agent that generates personalized talking points.


### Domain Intelligence


Scan a prospect company's domain to map out their team structure through role-based emails. Identify which departments are active and find the right point of contact without knowing individual names.


## Why We Partnered with Icypeas


Email enrichment is a commodity until it isn't. The difference between a tool that finds emails and one that finds *deliverable* emails is the difference between successful outreach and a trashed sender reputation. Icypeas has built a reputation for accuracy that companies like Lemlist, Clay, and Apollo trust in their own enrichment waterfalls. The fact that they do it at a fraction of the cost of alternatives makes them a natural fit for agent-driven workflows where every credit counts. We're excited to partner with Pierre Landoin and the Icypeas team to make this available through Orthogonal.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Icypeas and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/icypeas) |[Icypeas Website](https://www.icypeas.com/)
