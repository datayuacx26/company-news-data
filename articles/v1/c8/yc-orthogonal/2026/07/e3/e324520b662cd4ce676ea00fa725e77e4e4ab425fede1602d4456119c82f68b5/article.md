---
schema_version: "1.0.0"
document_id: "e324520b662cd4ce676ea00fa725e77e4e4ab425fede1602d4456119c82f68b5"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/companyenrich-b2b-data-for-agents"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:588b7abafd98abe04ffc6fe8a7ff1feb5e2b7d2dda9a682fc73d013e4cc3ee07"
---

# CompanyEnrich: B2B Company and People Data for AI Agents

Most B2B data providers give you a database that was fresh six months ago. Parked domains, dead websites, and duplicate entries waste your credits and pollute your pipeline. For AI agents making real-time decisions about which companies to target, stale data is broken data.


Today we're excited to announce our partnership with[CompanyEnrich](https://companyenrich.com/) , a B2B data platform that provides real-time company and people intelligence. CompanyEnrich covers 30M+ verified companies and 170M+ people profiles, distilled from over 900M raw records sourced from the public web, maps, social media, and government records. They don't just hand you a stale dump: their dataset is continuously refreshed so you always work with the most up-to-date information.


## What is CompanyEnrich?


CompanyEnrich is a B2B data API suite built for platforms, AI agents, and sales teams. Instead of stitching together enrichment from one vendor, search from another, and lookalikes from a third, CompanyEnrich delivers everything through a single REST interface.


Their core products:


- **Company Enrichment API** : Enrich any company by domain, name, or social profile. Returns 60+ data points including industry, revenue, tech stack, employee count, funding history, NAICS codes, and social profiles.
- **Company Search API** : Query 30M+ companies by 15+ criteria including industry, headcount, location, founding year, revenue, and technology stack.
- **Similar Companies API** : Submit seed domains and get AI-ranked lookalike companies for ICP expansion and TAM discovery.
- **People Search API** : Search 170M+ verified people profiles by name, title, company, seniority, department, and location.
- **Company Headcount API** : Track historical employee counts and department-level workforce trends to spot growth signals.


## Using CompanyEnrich with Orthogonal


### Company Enrichment


Start with a domain and get back a full firmographic profile: industry, revenue, employee count, technologies, funding history, social profiles, and more.


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


"company-enrich"


,


path


:


"/companies/enrich"


,


query


:


{


domain


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


// Returns: name, industry, revenue, employees, technologies,


// funding history, NAICS codes, social profiles, and 50+ more fields


`


You can also enrich by company name or social profile URL using the POST endpoint:


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


"company-enrich"


,


path


:


"/companies/enrich"


,


body


:


{


name


:


"Stripe"


}


}


)


;


`


### Company Search


Find companies matching specific criteria. Filter by industry, headcount, location, revenue, founding year, tech stack, and more. Use semantic search to describe what you need in plain language.


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


"company-enrich"


,


path


:


"/companies/search"


,


body


:


{


filters


:


{


employees


:


\[


"51-200"


\]


,


technologies


:


\[


"React"


\]


}


,


semanticSearch


:


"fintech companies in the US"


,


page


:


1


,


pageSize


:


25


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


// Returns paginated results with full enrichment included


`


### Similar Companies (Lookalikes)


Submit one or more seed domains and get AI-ranked lookalike companies. This is how sales teams expand TAM without guessing.


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


"company-enrich"


,


path


:


"/companies/similar"


,


body


:


{


domains


:


\[


"stripe.com"


,


"square.com"


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


// Returns ranked similar companies with relevance scores


`


### People Search


Search 170M+ people profiles by name, title, company, seniority, department, and location.


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


"company-enrich"


,


path


:


"/people/search"


,


body


:


{


filters


:


{


domains


:


\[


"stripe.com"


\]


,


titles


:


\[


"CTO"


\]


,


seniorities


:


\[


"director"


\]


}


,


page


:


1


,


pageSize


:


25


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


// Returns matching people with full profiles


`


### Company Workforce / Headcount


Track historical employee counts and department-level workforce trends. Hiring signals are one of the strongest indicators of company growth.


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


"company-enrich"


,


path


:


"/companies/workforce"


,


body


:


{


domain


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


// Returns observed employee count, department breakdowns


// (engineering, sales, marketing, etc.), and monthly history


`


### Using x402 Protocol


CompanyEnrich on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


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


"https://x402.orth.sh/company-enrich/companies/enrich?domain=stripe.com"


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


## Data Quality


CompanyEnrich takes data quality seriously. They start with over 900M raw records sourced from the public web, maps, social media, and government records, then verify and clean them down to 30M companies and 170M profiles. This means you're not paying credits for parked domains, dead websites, or duplicate entries.


Their data includes:


- **Firmographics** : Industry, employee count, revenue range, founding year, company type
- **Technographics** : Technology stack detection across hundreds of technologies
- **Financials** : Funding rounds, total funding raised, funding stage, stock info
- **Social profiles** : Professional networks, Twitter, Facebook, Instagram, YouTube, GitHub, Crunchbase, AngelList, G2
- **Location** : Country, state, city, address, postal code
- **SEO & web data** : Page rank, SEO descriptions, keywords
- **NAICS codes** : Standard industry classification for compliance and targeting


## Use Cases


### Sales Intelligence


Build targeted prospect lists by searching companies that match your ICP. Filter by industry, headcount, revenue, and tech stack, then use People Search to find decision-makers. Combine with lookalikes to expand beyond your known market.


### CRM Enrichment


Auto-fill missing company fields when new leads arrive. Use Company Enrichment to instantly resolve a domain to a full company profile. Route leads based on company size or industry before they touch your CRM.


### AI Agent Workflows


CompanyEnrich's data is built for agentic workflows. Their REST APIs integrate cleanly with any automation tool. Semantic search, lookalikes, and enrichment all work as building blocks for autonomous agent pipelines.


### Competitive Intelligence


Monitor competitors using headcount trends and hiring signals. Track which departments are growing, spot key hires and departures, and use lookalike search to map the competitive landscape.


### TAM Analysis


Define your ICP with filters like industry, headcount, revenue, tech stack, and region, then enrich every match. Use lookalikes to discover companies you didn't know existed. CompanyEnrich's 30M+ company database gives you the coverage to size your market accurately.


## Why We Partnered with CompanyEnrich


CompanyEnrich solves the core problems that plague B2B data: stale records, fragmented tooling, and wasted credits on junk data. Their approach of distilling 900M+ raw records down to 30M verified companies means developers get clean, reliable data from day one. The fact that they offer enrichment, search, lookalikes, and people data all through a single API suite makes integration straightforward. For AI agents that need fresh, accurate company intelligence to make autonomous decisions, CompanyEnrich delivers the data infrastructure to build with confidence.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try CompanyEnrich and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/company-enrich) |[CompanyEnrich Website](https://companyenrich.com/)
