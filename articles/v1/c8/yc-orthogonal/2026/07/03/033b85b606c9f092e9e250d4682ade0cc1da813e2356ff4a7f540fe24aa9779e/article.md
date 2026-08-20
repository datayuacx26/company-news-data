---
schema_version: "1.0.0"
document_id: "033b85b606c9f092e9e250d4682ade0cc1da813e2356ff4a7f540fe24aa9779e"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/crustdata-real-time-b2b-data-for-agents"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:2c0d6af9f2e600431e731d39f1c38d01849f7d70f6c43a50a21449fc5f88e729"
---

# Crustdata: Real-Time B2B Data for AI Agents

Most B2B data providers give you stale records refreshed quarterly at best. By the time your agent acts on that data, the person has changed jobs, the company has raised a round, and the headcount numbers are wrong. For AI agents making real-time decisions, stale data is broken data.


Today we're excited to announce our partnership with[Crustdata](https://crustdata.com/) , a real-time B2B data platform backed by Y Combinator. Crustdata provides people and company search for AI, pulling live data from 15+ diverse datasets to power agents in sales, recruiting, and investment.


## What is Crustdata?


Crustdata is a real-time B2B data provider covering 1 billion people and 60 million companies. They aggregate data from multiple sources across the web, providing 350+ company datapoints and 90+ people datapoints. Instead of relying on cached databases, they search the web at the moment of your query to provide the freshest and most accurate data.


Their core capabilities:


- **Search** : Build live lists of companies or people based on any criteria with 150+ filters
- **Enrich** : Get comprehensive profiles for any company or person from a single identifier


They have two methods of enrichment. Their cached database is refreshed every 14 to 28 days while their real-time enrichment gets data from the web at the moment of query.


## Using Crustdata with Orthogonal


### Company Enrichment


Start with a domain, company name, or Crustdata ID and get back comprehensive firmographic data: headcount, revenue, funding history, employer reviews, web traffic, news, job openings, founders, and decision makers.


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


// Enrich a company by domain


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


"crustdata"


,


path


:


"/screener/company"


,


query


:


{


company_domain


:


"stripe.com"


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


// Returns: headcount, revenue, funding, employer reviews,


// web traffic, news, job openings, founders, decision makers


`


You can enrich multiple companies in a single call by comma-separating domains:


` // Enrich multiple companies at once


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


"crustdata"


,


path


:


"/screener/company"


,


query


:


{


company_domain


:


"stripe.com,hubspot.com,notion.so"


,


fields


:


"company_name,company_website_domain,headcount.headcount,linkedin_company_url,founded_year,total_funding_raised_usd"


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


`


### Person Enrichment


Enrich individuals by LinkedIn URL or business email. Returns employment history, education, skills, contact info, and GitHub profiles.


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


"crustdata"


,


path


:


"/screener/person/enrich"


,


query


:


{


linkedin_profile_url


:


"https://www.linkedin.com/in/satyanadella"


,


fields


:


"name,title,headline,email,current_employers,past_employers,education_background,skills"


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


// Returns: name, title, headline, email, employment history,


// education, skills, LinkedIn URL, GitHub profiles


`


### Company Search


Find companies matching specific criteria using advanced filters. Filter by headcount, region, industry, revenue, funding events, growth rates, and more.


` // Find US companies with 50+ employees that raised funding recently


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


"crustdata"


,


path


:


"/screener/company/search"


,


body


:


{


filters


:


\[


{


filter_type


:


"COMPANY_HEADCOUNT"


,


type


:


"in"


,


value


:


\[


"51-200"


,


"201-500"


\]


}


,


{


filter_type


:


"REGION"


,


type


:


"in"


,


value


:


\[


"United States"


\]


}


,


{


filter_type


:


"ACCOUNT_ACTIVITIES"


,


type


:


"in"


,


value


:


\[


"Funding events in past 12 months"


\]


}


\]


,


page


:


1


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


// Returns 25 companies per page with full details


`


### People Search


Search for professionals with filters for current company, title, seniority, industry, region, skills, years of experience, and more.


` // Find CTOs at fintech companies


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


"crustdata"


,


path


:


"/screener/person/search"


,


body


:


{


filters


:


\[


{


filter_type


:


"CURRENT_TITLE"


,


type


:


"in"


,


value


:


\[


"CTO"


,


"Chief Technology Officer"


\]


}


,


{


filter_type


:


"INDUSTRY"


,


type


:


"in"


,


value


:


\[


"Financial Services"


\]


}


,


{


filter_type


:


"REGION"


,


type


:


"in"


,


value


:


\[


"United States"


\]


}


\]


,


page


:


1


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


// Returns 25 profiles per page with full employment history,


// education, skills, and contact info


`


### Company Identification


Resolve a company name, domain, or LinkedIn URL to Crustdata's database. Useful as a first step before enrichment or when you need to match fuzzy inputs to specific companies.


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


"crustdata"


,


path


:


"/screener/identify/"


,


body


:


{


query_company_website


:


"hubspot.com"


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


// Returns matching companies with relevance scores and Crustdata IDs


`


### Job Listings


Access job listings data filtered by company, location, date, and more. Hiring signals are one of the strongest indicators of company growth and priorities.


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


"crustdata"


,


path


:


"/data_lab/job_listings/Table/"


,


body


:


{


dataset


:


{


name


:


"job_listings"


,


id


:


"joblisting"


}


,


filters


:


{


op


:


"and"


,


conditions


:


\[


{


column


:


"company_id"


,


type


:


"in"


,


value


:


\[


7576


\]


}


\]


}


,


offset


:


0


,


count


:


25


,


sorts


:


\[


\]


,


tickers


:


\[


\]


,


groups


:


\[


\]


,


aggregations


:


\[


\]


,


functions


:


\[


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


// Returns job listings with title, location, remote status, and more


`


### Using x402 Protocol


Crustdata on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


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


account


=


privateKeyToAccount


(


process


.


env


.


PRIVATE_KEY


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


"https://x402.orth.sh/crustdata/screener/company?company_domain=stripe.com"


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


## Data Sources


Crustdata aggregates data from 15+ diverse datasets including:


- **Professional profiles** : People and company data with employment history, education, skills, and contact info
- **Headcount metrics** : Employee count, growth rates, department breakdowns, historical headcount
- **Job boards** : Real-time job listings across major platforms with detailed job descriptions
- **Funding data** : Investment rounds, amounts, and investor details
- **Social media posts** : Posts from people and companies with engagement data
- **News** : Press coverage, announcements, leadership changes
- **Web analytics** : Traffic trends, technology stack, SEO metrics
- **Employer reviews** : Company ratings and reviews from employees
- **Product reviews** : Ratings from major product review platforms and product launch reviews


## Use Cases


### Sales Intelligence


Build targeted prospect lists using Company Search. Filter by headcount, region, industry, funding stage, and growth signals. Enrich contacts with People Search to find the right decision-makers with full profiles and contact info.


### Recruiting


Find candidates matching specific criteria: title, skills, years of experience, current company, and seniority level. Get full employment histories and education backgrounds to qualify candidates before reaching out.


### Investment Research


Find and track specific companies that match your thesis and enrich details such as headcount trends and job listings to find high growth companies. Use Web Search to pull in real-time context like product announcements, regulatory filings, or market developments that enrich your thesis.


### Competitive Intelligence


Monitor competitors' job listings to reverse-engineer their product roadmap and hiring priorities. Track headcount changes, key hires and departures, product reviews, and web traffic trends. Use Web Search to catch announcements, partnerships, or product updates the moment they go live to know what your competitors are up to and what the market's saying about them.


### TAM Analysis


Combine Company Search and enrichment to size and map your total addressable market programmatically. Define your ICP using filters like industry, headcount range, funding stage, region, and tech stack and then enrich every matching company for revenue signals, growth trajectory, exact headcount and more.


## Why We Partnered with Crustdata


Crustdata's multi-source and real-time approach to B2B data solves the freshness and coverage problems that plague most data providers. They pull data from 15+ sources on the web at the moment of your query, while their cached database refreshes every 14 to 28 days. When you're building AI agents that autonomously reach out to prospects, source candidates, or find investment opportunities, you can't afford to have them working off data that's weeks or months old. Crustdata gives developers the data infrastructure to build AI agents with confidence.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Crustdata and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/crustdata) |[Crustdata Website](https://crustdata.com/)
