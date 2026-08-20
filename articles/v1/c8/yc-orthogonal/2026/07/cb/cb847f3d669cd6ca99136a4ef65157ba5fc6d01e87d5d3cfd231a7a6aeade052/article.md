---
schema_version: "1.0.0"
document_id: "cb847f3d669cd6ca99136a4ef65157ba5fc6d01e87d5d3cfd231a7a6aeade052"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/captaindata-people-company-data-for-agents"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:caf2b1c25214387270d23b142b400a402ed7b1217f7a4b16189ede05b36db3ae"
---

# Captain Data: People & Company Data API for AI Agents

Building AI agents or B2B SaaS products that need reliable people and company data? Stale records and partial profiles slow everything down.


Today we're highlighting[Captain Data](https://www.captaindata.com/) , a people and company data API built for AI agents and B2B applications, now available on Orthogonal.


## What is Captain Data?


Captain Data delivers fresh, high-quality data about people and companies through a production-ready API. With 500M+ profiles, 100M+ API calls per month across their platform, and 99% uptime, they power some of the leading SaaS and AI product companies.


Their API is built around LinkedIn Sales Navigator data, giving you access to real-time people and company information with a single integration.


## Key Features


### People Search


Search for people using Sales Navigator compatible queries. Filter by keywords, job title, location, company, and more. Results include full name, headline, job title, company, location, and LinkedIn profile data.


### People Enrichment


Start with a LinkedIn URL and get back a comprehensive profile: full name, headline, summary, job history, education, skills, languages, volunteer experience, and connection count. Full enrichment mode adds detailed experiences, skills, and education history.


### Company Search


Discover companies using Sales Navigator search queries. Filter by industry, headcount, location, and other criteria to find your ideal targets.


### Company Enrichment


Enrich any company from a LinkedIn URL. Returns comprehensive data: name, industry, headcount, founding date, specialties, funding details, website, locations, and affiliate companies.


## Using Captain Data with Orthogonal


### Search for People


` // Using @orth/sdk


// Install: npm install @orth/sdk


import


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


// Search for people using Sales Navigator queries


const


output


=


await


orthogonal


.


run


(


{


api


:


"captaindata"


,


path


:


"/people/search"


,


query


:


{


query


:


'(keywords:"Software Engineer")'


}


}


)


;


console


.


log


(


output


)


;


// Returns matching profiles with name, title, company, location


`


### Enrich a Person


` // Enrich a LinkedIn profile


const


output


=


await


orthogonal


.


run


(


{


api


:


"captaindata"


,


path


:


"/people/enrich"


,


query


:


{


li_profile_url


:


"https://www.linkedin.com/in/satyanadella"


}


}


)


;


console


.


log


(


output


)


;


// Returns: full name, headline, job title, company, education, location, and more


`


### Full Profile Enrichment


` // Get detailed profile with experiences, skills, and education


const


output


=


await


orthogonal


.


run


(


{


api


:


"captaindata"


,


path


:


"/people/enrich"


,


query


:


{


li_profile_url


:


"https://www.linkedin.com/in/satyanadella"


,


full_enrich


:


"true"


}


}


)


;


console


.


log


(


output


)


;


// Returns: everything above plus work history, skills, education details,


// volunteer experiences, and connection count


`


### Enrich a Company


` // Enrich a company from its LinkedIn URL


const


output


=


await


orthogonal


.


run


(


{


api


:


"captaindata"


,


path


:


"/companies/enrich"


,


query


:


{


li_company_url


:


"https://www.linkedin.com/company/stripe"


}


}


)


;


console


.


log


(


output


)


;


// Returns: name, industry, headcount, website, funding data, locations, and more


`


### Search for Companies


` // Search companies using Sales Navigator queries


const


output


=


await


orthogonal


.


run


(


{


api


:


"captaindata"


,


path


:


"/companies/search"


,


query


:


{


query


:


'(keywords:"artificial intelligence")'


}


}


)


;


console


.


log


(


output


)


;


// Returns matching company profiles


`


### Find a Company


` // Find a company by name or domain


const


output


=


await


orthogonal


.


run


(


{


api


:


"captaindata"


,


path


:


"/companies/find"


,


query


:


{


company_name


:


"Stripe"


}


}


)


;


console


.


log


(


output


)


;


// Returns matching company with LinkedIn URL and details


`


### Using x402 Protocol


Captain Data on Orthogonal supports[x402](https://www.x402.org/) for autonomous agent payments using USDC stablecoins.


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


// Search for people with automatic USDC payment


const


url


=


'https://x402.orth.sh/captaindata/people/search?query=(keywords:"Software Engineer")'


;


const


response


=


await


fetchWithPayment


(


url


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


## Available Endpoints


Endpoint Method Description


/people/search GET Search people with Sales Navigator queries


/people/enrich GET Enrich a person by LinkedIn URL


/companies/search GET Search companies with Sales Navigator queries


/companies/enrich GET Enrich a company by LinkedIn URL


/companies/find GET Find a company by name or domain


## Use Cases


### Sales Prospecting


Search for decision-makers at target companies using Sales Navigator queries. Enrich profiles to get full context before outreach.


### Lead Enrichment


Start with a name or LinkedIn URL and get back a complete profile. Fill gaps in your CRM with fresh data.


### Account Research


Enrich companies to understand headcount, industry, funding status, and specialties. Map employees to identify the right contacts.


### Recruiting


Search for candidates matching specific skills and experience. Get detailed work history and education data for screening.


### Market Intelligence


Search across companies and people to identify trends, track hiring patterns, and monitor competitive landscapes.


### AI Agent Workflows


Give your agents access to real-time people and company data. Sales Navigator powered search means agents can find and qualify leads programmatically.


## Why Agents Need Fresh People Data


AI agents doing outreach, research, or qualification need current, accurate data to work effectively:


1. **Freshness matters** : 500M+ profiles kept current means agents work with up-to-date information
2. **Sales Navigator search** : Agents can use the same powerful search that sales teams rely on
3. **One integration** : People search, enrichment, company data, and employee lists through a single API
4. **Production-ready** : 99% uptime and 100M+ monthly API calls means reliability at scale


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Captain Data and dozens of other APIs. No API keys to manage, no accounts to create.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/captaindata) |[Captain Data Website](https://www.captaindata.com/)
