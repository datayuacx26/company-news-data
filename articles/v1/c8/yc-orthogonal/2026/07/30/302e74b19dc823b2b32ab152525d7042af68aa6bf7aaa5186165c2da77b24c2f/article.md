---
schema_version: "1.0.0"
document_id: "302e74b19dc823b2b32ab152525d7042af68aa6bf7aaa5186165c2da77b24c2f"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/fantastic-jobs-ai-enriched-job-data"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:8a461201f26b8b37866d23f66a5bfd7858f1906662d1bdd8b01b7dc25007d2c5"
---

# Fantastic.jobs: 14M+ AI-Enriched Job Postings, Refreshed Hourly

If you're building anything that touches job data, you know the pain. Career sites are scattered across dozens of ATS platforms, each with its own format. Job boards update on their own schedule. By the time you scrape, normalize, and deduplicate everything, half the listings are already stale.


Today we're excited to announce our partnership with[Fantastic.jobs](https://fantastic.jobs/) , a job posting API that aggregates 14M+ listings per month from 200,000+ career sites and major job boards, with hourly refresh and AI enrichment baked in. Every posting comes normalized with structured fields, matched company data, and 20+ AI-generated attributes, so your agents can work with clean data without additional processing.


## What is Fantastic.jobs?


Fantastic.jobs is a multi-source job aggregation API built by[Remco Strijdonk](https://www.linkedin.com/in/remcostrijdonk) in Amsterdam. Instead of scraping individual career pages yourself, Fantastic.jobs polls 200,000+ ATS-hosted career sites and job boards every hour, normalizes the data, and serves it through a clean REST API.


Their core capabilities:


- **ATS Jobs API** : 3M+ jobs per month from 200,000+ career sites across 54 ATS platforms including Workday, Greenhouse, Lever, BambooHR, SAP SuccessFactors, and Oracle Recruiting
- **Job Board API** : 11M+ jobs per month from LinkedIn, Wellfound, and Y Combinator
- **Hourly Refresh** : 99% of indexed career sites polled every hour, with 95% of new jobs discovered within 3 hours of posting
- **AI Enrichment** : 20+ structured fields generated per job, including normalized titles, employment types, remote/hybrid classification, salary parsing, and seniority levels
- **Organization Enrichment** : Matched company profiles with LinkedIn data, industry taxonomies, Crunchbase funding, Glassdoor ratings, headcount timeseries, and news articles
- **Expired Jobs API** : Dedicated endpoint to track which listings have been taken down, keeping your database in sync
- **Modified Jobs API** : Catch re-posted and updated listings so your dataset mirrors the latest version of every job
- **6-Month Backfill API** : Seed a new application instantly with historical ATS and job board data going back six months


## Using Fantastic.jobs with Orthogonal


### Search ATS Career Site Jobs


Query fresh job listings from 200,000+ career sites with 30+ filters. Search by title, location, ATS source, AI-enriched fields, and more.


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


// Search for recent software engineering jobs in the US


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


"fantastic-jobs"


,


path


:


"/v1/active-ats"


,


query


:


{


time_frame


:


"24h"


,


title


:


"Software Engineer"


,


location


:


"United States"


,


limit


:


"25"


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


// Returns: title, organization, locations, salary, employment_type,


// ai_experience_level, ai_work_arrangement, url, and 20+ AI-enriched fields


`


### Get Modified Jobs


Keep your database in sync by tracking which ATS jobs have changed. Returns full job data plus modification details. Typically 100k-150k jobs per day.


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


"fantastic-jobs"


,


path


:


"/v1/modified-ats"


,


query


:


{


date_modified_gte


:


"2026-06-14T00:00:00Z"


,


limit


:


"100"


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


// Returns: jobs with date_modified and modified_fields


// Use this to update changed listings in your database


`


### Backfill Historical Data


Seed a new job board or application with six months of historical data by setting` time_frame` to` 6m` .


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


"fantastic-jobs"


,


path


:


"/v1/active-ats"


,


query


:


{


time_frame


:


"6m"


,


source


:


"greenhouse"


,


location


:


"United States"


,


limit


:


"1000"


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


// Returns: historical job postings from the last 6 months


// Perfect for populating a new job board or training dataset


`


### Using x402 Protocol


Fantastic.jobs on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


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


"https://x402.orth.sh/fantastic-jobs/v1/active-jb?time_frame=24h&title=Data%20Scientist&location=United%20States&limit=10"


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


Fantastic.jobs covers an enormous breadth of the global job market:


- **14M+ job postings per month** across ATS career sites and job boards
- **200,000+ career sites** indexed across 54 ATS platforms
- **Global coverage** with strong representation of white collar roles across all major markets
- **Hourly polling** with 95% of new jobs discovered within 3 hours
- **20+ AI-enriched fields** per posting including normalized titles, employment types, salary parsing, and remote classification
- **Company matching** with LinkedIn profiles, industry codes, funding data, and Glassdoor ratings


Used by 5,000+ subscribers across RapidAPI and Apify, plus direct enterprise customers.


## Use Cases


### Job Boards & Aggregators


Feed your job board with millions of fresh listings. The hourly refresh and expired jobs endpoint mean your users see current opportunities, not dead links. The backfill API lets you launch with six months of data on day one.


### Resume & Career Products


AI career assistants can match resumes against real-time job data. With 30+ filters and AI-enriched fields like seniority and remote status, agents can surface precisely relevant opportunities for each user.


### Sales Lead Generation


Job postings are buying signals. Companies hiring for specific roles reveal budget, priorities, and growth direction. Use Fantastic.jobs to identify companies actively hiring in your target market, then enrich with the organization data to qualify leads.


### AI Agent Workflows


Agents building hiring pipelines, market research tools, or competitive intelligence can pull structured job data without managing scraping infrastructure. The normalized format and company matching eliminate the data cleaning step entirely.


### LLM Training & Analytics


The structured, AI-enriched dataset is ready for downstream ML pipelines. Salary trends, hiring velocity, geographic distribution, skills demand: all queryable through the API without building your own enrichment layer.


## Why We Partnered with Fantastic.jobs


Job data is one of the most requested categories on Orthogonal. Fantastic.jobs stood out because they solve the hardest part of the problem: keeping the data fresh. Most job APIs refresh daily or weekly. Fantastic.jobs polls hourly and gives you dedicated endpoints for expired and modified listings, so your database actually stays in sync. The AI enrichment means you get structured, normalized data out of the box. For agents that need to work with job postings at scale, this is the cleanest path from "I need job data" to "I have job data."


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Fantastic.jobs and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/fantastic-jobs) |[Fantastic.jobs Website](https://fantastic.jobs/)
