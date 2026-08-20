---
schema_version: "1.0.0"
document_id: "c0de98d66ffa63e934116ea5e974fe26beb9276bb26337c7043cca8f36668b18"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/edges-linkedin-automation-api"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:03754eb2d55eeb7562192c8e2aa147c0d48c6240c7f18a1ea5dcda324cf2de31"
---

# Edges: LinkedIn API Automation for AI Agents

Building AI agents that need LinkedIn data or automation? Scraping is fragile, official APIs are limited, and most providers only cover basic profile extraction.


Today we're highlighting[Edges](https://edges.run/) , a comprehensive LinkedIn API automation platform with 60+ actions, now available on Orthogonal.


## What is Edges?


Edges is the API for LinkedIn automation and data enrichment. One API call lets you extract profiles, companies, posts, and jobs. Search across LinkedIn and Sales Navigator. Automate messages, invites, and follows. With managed identity mode, you can start immediately without connecting any LinkedIn accounts.


Their platform includes built-in Smart Limits to protect LinkedIn accounts from restrictions automatically, and supports real-time, async, and scheduled execution modes.


## Key Features


### Profile & Company Extraction


Extract detailed information from LinkedIn profiles, company pages, and Sales Navigator. Get names, headlines, work history, employee insights, company affiliates, and more.


### LinkedIn & Sales Navigator Search


Search for people, companies, content, jobs, schools, and events across LinkedIn and Sales Navigator. Use the same powerful filters available in the LinkedIn UI.


### Outreach Automation


Send connection requests, messages, post comments, follow profiles, and like posts programmatically. Schedule actions or run them in real-time.


### Data Enrichment


Enrich profiles with contact information, extract education and experience history, and discover company employee insights with headcount breakdowns by function and seniority.


### Flexible Identity Modes


Start with managed identities (no setup required), connect your users' accounts directly, or rent dedicated accounts for high-volume operations.


### Async & Scheduled Execution


Run actions in real-time for immediate results, queue them asynchronously with callbacks for batch processing, or schedule them for specific times.


## Using Edges with Orthogonal


### Search for People on LinkedIn


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


// Search LinkedIn for software engineers


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


"edges"


,


path


:


"/actions/linkedin-search-people/run/live"


,


body


:


{


input


:


{


linkedin_people_search_url


:


"https://www.linkedin.com/search/results/people/?keywords=software%20engineer"


}


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


// Returns: profiles with name, title, company, headline, location


`


### Extract a LinkedIn Profile


` // Extract full profile details


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


"edges"


,


path


:


"/actions/linkedin-extract-people/run/live"


,


body


:


{


input


:


{


linkedin_profile_url


:


"https://www.linkedin.com/in/satyanadella"


}


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


// Returns: full name, headline, summary, experience, education, skills, and more


`


### Extract a Company Page


` // Get detailed company information


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


"edges"


,


path


:


"/actions/linkedin-extract-company/run/live"


,


body


:


{


input


:


{


linkedin_company_url


:


"https://www.linkedin.com/company/stripe"


}


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


// Returns: company name, description, industry, employee count, locations, and more


`


### Search Sales Navigator


` // Search Sales Navigator for leads


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


"edges"


,


path


:


"/actions/salesnavigator-search-people/run/live"


,


body


:


{


input


:


{


sales_navigator_profile_search_url


:


"https://www.linkedin.com/sales/search/people?query=(keywords%3ACTO)"


}


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


// Returns: matching profiles from Sales Navigator with detailed data


`


### Find a Company's LinkedIn URL


` // Use AI to find a company's LinkedIn page from its domain


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


"edges"


,


path


:


"/actions/linkedin-find-company-url/run/live"


,


body


:


{


input


:


{


domain


:


"stripe.com"


}


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


// Returns: { linkedin_company_url: "https://www.linkedin.com/company/stripe" }


`


### Extract Company Employee Insights


` // Get employee analytics for a company


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


"edges"


,


path


:


"/actions/linkedin-extract-company-employees-insights/run/live"


,


body


:


{


input


:


{


linkedin_company_url


:


"https://www.linkedin.com/company/openai"


}


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


// Returns: headcount by function, location, seniority, and growth trends


`


### Using x402 Protocol


Edges on Orthogonal supports[x402](https://www.x402.org/) for autonomous agent payments using USDC stablecoins.


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


// Search LinkedIn for people with automatic USDC payment


const


url


=


"https://x402.orth.sh/edges/actions/linkedin-search-people/run/live"


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


input


:


{


linkedin_people_search_url


:


"https://www.linkedin.com/search/results/people/?keywords=software%20engineer"


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


)


;


`


## Available Endpoints (Selection)


Edges has 42 of its 60+ actions available on Orthogonal. Here are the most commonly used:


Endpoint Method Description


/actions/linkedin-search-people/run/live POST Search for people on LinkedIn


/actions/linkedin-extract-people/run/live POST Extract a full LinkedIn profile


/actions/linkedin-extract-company/run/live POST Extract company page details


/actions/salesnavigator-search-people/run/live POST Search Sales Navigator for leads


/actions/linkedin-find-company-url/run/live POST AI-powered company LinkedIn URL finder


/actions/linkedin-find-profile-url/run/live POST AI-powered profile LinkedIn URL finder


/actions/linkedin-extract-company-employees-insights/run/live POST Employee analytics and headcount


/actions/linkedin-search-content/run/live POST Search posts and articles


/actions/linkedin-search-jobs/run/live POST Search for job postings


## Use Cases


### Sales Prospecting


Search LinkedIn and Sales Navigator for leads matching your ICP. Extract profiles, get contact info, and automate connection requests at scale.


### Lead Enrichment


Start with a name and use AI to find their LinkedIn URL, then extract full profile data. Enrich your CRM with current titles, companies, and work history.


### Account-Based Marketing


Map entire company org charts by searching employees. Get headcount insights to identify growth signals and target the right stakeholders.


### Recruiting


Search for candidates with specific skills and experience. Extract detailed profiles including education, certifications, and career progression.


### Market Research


Monitor LinkedIn content for industry trends. Extract job postings to understand hiring patterns. Analyze company employee insights for competitive intelligence.


### AI Agent Automation


Give your agents full LinkedIn capabilities. Managed identity mode means zero setup. Agents can search, extract, enrich, and even automate outreach autonomously.


## Why Agents Need LinkedIn Automation


LinkedIn is the largest professional network with 1B+ members. For agents doing sales, recruiting, or research:


1. **60+ actions** : From simple profile extraction to complex Sales Navigator searches, agents get full LinkedIn coverage
2. **Zero setup start** : Managed identity mode means agents can start working immediately without LinkedIn accounts
3. **Built-in safety** : Smart Limits protect accounts from restrictions automatically
4. **Real-time and async** : Run actions instantly or queue thousands with callback delivery


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Edges and dozens of other APIs. No API keys to manage, no accounts to create.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/edges) |[Edges Website](https://edges.run/)
