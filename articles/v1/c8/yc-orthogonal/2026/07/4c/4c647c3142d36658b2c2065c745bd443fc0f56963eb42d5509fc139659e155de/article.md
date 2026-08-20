---
schema_version: "1.0.0"
document_id: "4c647c3142d36658b2c2065c745bd443fc0f56963eb42d5509fc139659e155de"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/bytemine-b2b-data-for-agents"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:e59c825237986817859a98bb03455de3447ec098ffea913dcf9c056cdd26dab4"
---

# Bytemine: 135M+ Contacts, Web Crawling, and Intent Signals in One API

Sales teams spend too much time stitching together data from different vendors. One tool for contacts, another for company info, a third for email verification, a fourth for web scraping. Each has its own API key, its own rate limits, its own billing model. For AI agents trying to run outbound workflows end to end, this fragmentation is a dealbreaker.


Today we're excited to announce our partnership with[Bytemine](https://www.bytemine.ai/) , a B2B data platform that brings contacts, companies, web crawling, and intent signals together under one API. Bytemine covers 135M+ B2B contacts with 80M+ verified mobile numbers and 10M+ company profiles, all updated daily. Their APIs respond in under 200ms.


## What is Bytemine?


Bytemine is a data platform built for sales, marketing, and GTM teams. Instead of paying for separate tools for each step of the prospecting workflow, Bytemine gives you everything through a single REST API.


Their core products:


- **Contact Search API** : Search 135M+ B2B contacts by job title, seniority, department, industry, location, and 50+ other attributes. Returns verified emails, mobile numbers, and direct dials.
- **Contact Enrichment API** : Pass an email, phone number, LinkedIn URL, or name+company and get back 50+ verified fields in under 200ms. Works for single contacts or bulk batches.
- **Company Search API** : Search 10M+ company profiles by name, industry, NAICS code, keywords, location, employee count, and revenue range.
- **Company Lookalikes API** : Submit seed domains and get back similar companies ranked by firmographic and technographic similarity.
- **Web Crawler APIs** : Scrape, batch-process, and crawl websites. Extract structured data with AI, generate business intelligence reports, and track page changes with diffs. 12 endpoints covering everything from single-page scraping to full site crawls.
- **Signals API** : Real-time intent signals for funding rounds, acquisitions, and LinkedIn posts. Trigger outreach when a prospect raises capital, gets acquired, or publishes a buying-intent post.
- **Proposal Generator API** : Generate personalized sales proposals with branding and ROI projections from prospect data.
- **Meeting Prep API** : Auto-generate briefing documents before sales calls using company and contact context.


## Using Bytemine with Orthogonal


### Contact Search


Search 135M+ contacts with filters. Find decision-makers by title, seniority, department, company, and location.


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


// Search for VP-level contacts at fintech companies


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


"bytemine"


,


path


:


"/contacts/search"


,


body


:


{


title


:


"VP of Sales"


,


seniority


:


"VP"


,


industry


:


"Financial Services"


,


company_employee_range


:


"51-200"


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


result


.


contacts


)


;


// Returns: name, job_title, seniority, department, work_email,


// mobile_number, company_name, company_domain, and 40+ more fields


`


### Contact Enrichment


Start with an email or LinkedIn URL and get back a full profile: name, title, company, phone numbers, education, work history, and more.


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


"bytemine"


,


path


:


"/contacts/enrich"


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


// Returns: full_name, job_title, seniority, department, mobile_number,


// work_email, linkedin_profile, education, work_experience, salary,


// company_name, company_domain, company_industry, and more


`


You can also enrich by LinkedIn URL, phone number, or name + company:


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


"bytemine"


,


path


:


"/contacts/enrich"


,


body


:


{


linkedin_url


:


"https://www.linkedin.com/in/janedoe"


}


}


)


;


`


### Company Search


Search 10M+ companies by name, industry, NAICS code, location, employee count, and more.


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


"bytemine"


,


path


:


"/b2b-search"


,


body


:


{


industry


:


"Computer Software"


,


employee_range


:


"11-50"


,


country


:


"US"


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


result


.


companies


)


;


// Returns: company_name, domain, industry, employee_count,


// revenue_range, founded_year, location, naics_codes, and more


`


### Web Crawling


Scrape up to 100 URLs in parallel. Returns markdown content, metadata, and tech stack detection per page.


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


"bytemine"


,


path


:


"/crawl/batch"


,


body


:


{


urls


:


\[


"https://acme.com"


,


"https://acme.com/pricing"


,


"https://acme.com/about"


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


.


data


)


;


// Returns: markdown content, page metadata, links,


// and tech stack detection for each URL


`


### Proposal Generation


Generate personalized sales proposals from prospect data. Useful for agents that need to create outreach materials automatically.


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


"bytemine"


,


path


:


"/v1/proposals/generate"


,


body


:


{


source_domain


:


"yourcompany.com"


,


target_domain


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


// Returns: formatted proposal with company context,


// ROI projections, and personalized talking points


`


### Using x402 Protocol


Bytemine on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


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


"https://x402.orth.sh/bytemine/contacts/search"


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


title


:


"CTO"


,


industry


:


"Computer Software"


,


limit


:


5


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


## Data Coverage


Bytemine's database covers:


- **135M+ B2B contacts** with verified work emails, personal emails, and direct dials
- **80M+ verified mobile numbers** for direct outreach
- **10M+ company profiles** with firmographics, NAICS codes, revenue, and employee data
- **50+ attributes per contact** including education, work history, skills, and interests
- **Daily updates** to keep records fresh


Contact records include job title, seniority, department, LinkedIn profile, email addresses, phone numbers, education history, work experience, salary data, and company context (domain, industry, employee range, revenue range, headquarters, NAICS/SIC codes).


## Use Cases


### Outbound Prospecting


Search for contacts matching your ICP, enrich them with verified emails and phone numbers, and feed the results directly into your outreach tool. One API call to go from "VP of Sales at fintech companies with 50-200 employees" to a list of real people with contact info.


### CRM Enrichment


When a new lead enters your CRM, enrich it instantly. Pass an email address, get back 50+ fields including title, seniority, company size, and direct phone number. Route and score leads without manual research.


### AI Agent Workflows


Bytemine's API suite covers every step of an agent-driven GTM pipeline: identify target companies, find decision-makers, verify contact info, scrape competitor websites, monitor buying signals, and generate personalized proposals. All through one integration.


### Competitive Intelligence


Use the web crawler to monitor competitor websites for pricing changes, product launches, and hiring patterns. Combine with the signals API to track funding rounds and acquisitions in your market.


### Account-Based Marketing


Start with a list of target accounts, use company lookalikes to expand your list, then find every relevant decision-maker at each account. The signals API tells you which accounts are showing buying intent right now.


## Why We Partnered with Bytemine


Most B2B data tools solve one piece of the puzzle. Bytemine solves several. Contacts, companies, web crawling, intent signals, and AI-powered tools like proposal generation and meeting prep, all through a single API with sub-200ms response times. For AI agents running GTM workflows, having all of this behind one integration means fewer moving parts and faster execution. The fact that they cover 135M+ contacts with 80M+ verified mobile numbers gives agents the depth they need to find and reach real people.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Bytemine and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/bytemine) |[Bytemine Website](https://www.bytemine.ai/)
