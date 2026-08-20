---
schema_version: "1.0.0"
document_id: "13840bcf4198fabef9b2640d0c87df600155b0c954d8febe3319cf941cadf80c"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/fundable-vc-deals-investor-data-for-agents"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:edfdaba12aac22d6e3b93cbf96e0187a5156eb60d73df377a8fdb5912df4da9e"
---

# Fundable: Real-Time Startup and Investor Data for AI Agents

If you're building tools for investors, salespeople, or analysts, you need startup and investor data that's real-time and affordable. Most data providers update quarterly, charge enterprise pricing, and force you to wade through everything just to find the signals that matter.


Today we're excited to announce our partnership with[Fundable](https://tryfundable.ai/) , a startup and investor data platform that's real-time and affordable. Fundable provides 50+ datapoints on 90k startups, with data on 60k investors including partners and angels involved on deals, all updated multiple times per day.


## What is Fundable?


Fundable is a complete sourcing stack for startup and investor data. Their platform combines comprehensive data with the tools to actually do something with it.


Their core capabilities:


- **Extensive Startup Dataset** : Access 50+ datapoints on 90k startups, updated multiple times per day
- **Best-in-Class Investor Coverage** : Explore data on 60k investors including partners and angels involved on deals
- **AI-Powered Discovery** : Find hidden gems with AI-powered semantic search using natural language descriptions and advanced filters
- **Bulk Data Access** : Access the dataset via API, Data Feed (BigQuery), or MCP


## Using Fundable with Orthogonal


### Search Funding Rounds


Find recent Series A deals in AI:


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


"fundable"


,


path


:


"/deals"


,


body


:


{


deal


:


{


financing_types


:


\[


{


type


:


"SERIES_A"


}


\]


,


date_start


:


"2026-01-01"


}


,


company


:


{


industries


:


\[


"artificial-intelligence"


\]


}


,


page_size


:


25


,


sort_by


:


"most_recent_deal"


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


// Returns: deals with company info, round details, investor IDs


`


### Look Up a Company


Get detailed company information by domain:


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


"fundable"


,


path


:


"/company"


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


// Returns: company profile with latest funding round details


`


### Get Full Funding History


See every round a company has raised:


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


"fundable"


,


path


:


"/company/deals"


,


query


:


{


domain


:


"coinbase.com"


,


page_size


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


.


data


)


;


// Returns: all funding rounds with amounts, dates, and types


`


### Search and Filter Investors


Find investors active in your space:


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


"fundable"


,


path


:


"/investors"


,


body


:


{


company_investments


:


{


industries


:


\[


"artificial-intelligence"


\]


,


financing_types


:


\[


{


type


:


"SEED"


}


,


{


type


:


"SERIES_A"


}


\]


,


deal_start_date


:


"2025-01-01"


}


,


page_size


:


25


,


sort_by


:


"most_recent_deal"


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


// Returns: investors with filtered_deal_count and filtered_lead_count


`


### AI-Powered Company Discovery


Use natural language to find companies:


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


"fundable"


,


path


:


"/companies"


,


body


:


{


company


:


{


search_query


:


"AI-powered developer tools for code review"


,


min_relevance


:


0.5


}


,


page_size


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


data


)


;


// Returns: companies with similarity scores sorted by relevance


`


## Use Cases


- **Sales Prospecting** : Identify companies matching your ICP that recently raised and reach out to key decision makers when they're ready to purchase
- **Investor Research** : Explore 60k investors with portfolio filtering to find VCs active in your industry and stage
- **Competitive Intelligence** : Track funding rounds in your space and discover emerging competitors with AI-powered search
- **Deal Sourcing** : Filter deals by size, stage, industry, and geography for investment analysis
- **Market Mapping** : Use semantic search to discover companies in any niche with natural language descriptions


## Getting Started


Access Fundable through Orthogonal with any of our integration methods:


` # Via CLI


orth api run fundable /deals


--body


'{"deal": {"financing_types": \[{"type": "SERIES_A"}\]}, "page_size": 5}'


# Via curl


curl


-X


POST https://api.orth.sh/v1/run


\\


-H


"Authorization: Bearer YOUR_ORTHOGONAL_KEY"


\\


-H


"Content-Type: application/json"


\\


-d


'{"api": "fundable", "path": "/company", "query": {"domain": "stripe.com"}}'


`


No Fundable API key needed. Pay per request through Orthogonal.


---


*Fundable is available now on Orthogonal.[Get started](https://orthogonal.com/) or[read the docs](https://docs.tryfundable.ai/) .*
