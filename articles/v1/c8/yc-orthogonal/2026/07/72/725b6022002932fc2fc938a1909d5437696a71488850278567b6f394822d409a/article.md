---
schema_version: "1.0.0"
document_id: "725b6022002932fc2fc938a1909d5437696a71488850278567b6f394822d409a"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/openfunnel-live-market-events-for-gtm-teams"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:3a819b66da67e0bf50eea7e5c8d7638c77bbe093b662f449cbffaaf242073d23"
---

# OpenFunnel: Live Market Events for GTM Teams

Static prospect lists tell you who companies and people are. They don't tell you what just happened to them.


Right now, across 50 million companies and 1 billion people, market events are unfolding that create buying windows: a new hire brought in to solve a specific pain-point, a platform migration underway, a team being built out to expand into a new category. The GTM teams that win catch these moments as they emerge, not in a quarterly list refresh.


Today we're excited to announce our partnership with[OpenFunnel](https://openfunnel.ai/) , a Live Market Events Engine for GTM Teams. OpenFunnel gives GTM teams a live stream of domain-specific market events that helps GTM teams unlock 2x pipeline that was previously invisible.


## What is OpenFunnel?


OpenFunnel is a Live Market Event Capture Engine that monitors your entire market for pain-points in your ICP and feeds them directly to Slack or your CRM. Instead of a static list, you get a live feed of events, custom-defined for your ICP, that show up as your market moves daily.


**What makes OpenFunnel different:** OpenFunnel daily captures live, domain-specific market events that auto-surface companies with relevant pain-points, vs static databases that lack context and timing.


## Market Event Types


OpenFunnel's Event Intelligence engine tracks six categories of market events. Each one is custom promptable and represents something that just changed at a company, and change is where the pain-points live.


**Deep ICP Activity Search** (also called Deep Company Search): Discovery using the TAQ framework (Trait, Activity, Qualifier) to find companies matching complex, multi-dimensional criteria.


**Hiring Events** : A job opening is a company announcing a pain-point in public. OpenFunnel doesn't just track that a company is hiring; it extracts the team name, role content, and what the hire reveals about where the company is investing.


**Job Changes** : Key people moving between companies. When a champion joins a new company, they often bring the problems, and the vendors, they know. When they leave, both the old and new account are in motion.


**Social Events** : Posts from company employees on LinkedIn and other platforms, with context about why the content matters for your market, not just that it happened.


**LinkedIn Engagement** : ICP personas interacting with industry leaders, competitors, and relevant content. Useful for identifying ICPs who are active and consuming content, but not necessarily a buying signal.


**Technographics** : Technology stack changes that indicate a company is migrating, expanding, or shifting priorities in ways relevant to your product.


## Using OpenFunnel with Orthogonal


The typical workflow moves through two stages: **spot** net-new accounts with active pain-points, then **enrich** the ones worth pursuing. Here's how each step looks in practice.


### 1. Spot Net-New ICP Accounts with Active Pain-Points


Start with the Deep ICP Activity Search Agent. Deploy a search using three inputs that describe exactly the kind of company you're after: what they are (Trait), what they're currently doing that signals a pain-point (Activity), and what they already have as a qualifying condition (Qualifier).


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


// Find B2B SaaS companies scaling outbound with an existing SDR team


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


"openfunnel"


,


path


:


"/api/v1/signal/deploy/deep-company-search-agent"


,


body


:


{


name


:


"Outbound scaling search"


,


trait


:


"B2B SaaS companies"


,


activity


:


"Trying to scale outbound without hiring more SDRs"


,


qualifier


:


"Has at least 3 SDRs"


,


repeat


:


false


,


enable_safe_crm_addition


:


true


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


// Returns: matched accounts added to your audience and CRM


`


### 2. Enrich: Go Deep on the Best Accounts


Once you've identified a high-signal account worth pursuing, pull its full profile or run deep enrichment to qualify it end-to-end.


` // Get full account details by domain


const


account


=


await


orthogonal


.


run


(


{


api


:


"openfunnel"


,


path


:


"/api/v2/account/batch"


,


body


:


{


account_domains


:


\[


"stripe.com"


,


"notion.so"


\]


,


icp_people_page


:


1


,


icp_people_page_size


:


100


}


}


)


;


// Returns: firmographics, hiring signals, social signals,


// LinkedIn engagement, job changes, technographics,


// and ICP-qualified people


`


For accounts you want to qualify from scratch, deep enrichment runs OpenFunnel's full pipeline, finding intent signals and relevant decision-makers for a given domain. This runs in the background and typically takes 15 to 30 minutes:


` // Kick off deep enrichment


const


enrich


=


await


orthogonal


.


run


(


{


api


:


"openfunnel"


,


path


:


"/api/v1/enrich/deep-enrich"


,


body


:


{


domain


:


"databricks.com"


,


target_icp_roles


:


\[


"VP Engineering"


,


"CTO"


,


"Head of AI"


\]


,


timeframe


:


90


,


max_jobs_to_check


:


200


}


}


)


;


// Returns: { status: "accepted", account_id: 12345 }


// Poll for results once complete


const


results


=


await


orthogonal


.


run


(


{


api


:


"openfunnel"


,


path


:


"/api/v2/account/batch"


,


body


:


{


account_ids


:


\[


enrich


.


data


.


account_id


\]


}


}


)


;


`


## Available Endpoints


Endpoint Method Description


/api/v1/signal/deploy/deep-company-search-agent POST Deploy Deep Company Search Signal


/api/v1/signal/deploy/deep-hiring-agent POST Deploy Deep Hiring Signal


/api/v1/signal/deploy/social-listening-agent POST Deploy Social Listening Signal


/api/v1/signal/deploy/technography-search-agent POST Deploy Technography Search Signal


/api/v1/signal/deploy/icp-job-change-agent POST Deploy ICP Job Change Signal


/api/v1/signal/deploy/competitor-engagement-agent POST Deploy Competitor Engagement Signal


/api/v1/signal/deploy/competitor-activity-agent POST Deploy Competitor Activity Signal


/api/v1/account/search-lookalikes GET Find lookalike companies from a natural-language description


/api/v1/enrich/account POST Enrich a company account with firmographic data


/api/v2/account/batch POST Get full account details by ID or domain


/api/v1/enrich/deep-enrich POST End-to-end company qualification and people enrichment


## Use Cases


**Event-Triggered Outreach** : Deploy event monitors for your domain, then use the feed to trigger outreach the moment a company shows intent. Instead of blasting a static list, you reach out with context about what just changed and why you're reaching out now.


**Competitive Intelligence** : Track events around competitor engagement to see which ICP personas are interacting with your competitors' teams. When a prospect comments on your competitor's CEO's post about a feature you also offer, that's a warm lead with a clear angle.


**Account Prioritization** : Surface accounts where multiple events are overlapping: a relevant hire, a public post about the problem, engagement with competitors. Companies in the middle of change are the warmest leads.


**Account Research** : Pull a deep, evidence-backed brief on any target account. OpenFunnel surfaces the recent events at the company (hires, posts, job changes, technography shifts) alongside firmographics and ICP-qualified people, so reps walk into outreach with context, not guesses.


## Using x402 Protocol


OpenFunnel on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


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


"https://x402.orth.sh/openfunnel/api/v1/account/search-lookalikes?query=AI+code+review+tools&limit=10"


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


## Why We Partnered with OpenFunnel


The B2B data market is flooded with static lists and commoditized signals. Everyone sees the same funding rounds, the same job changes, the same G2 reviews. The problem isn't access to data: it's that most teams aren't watching market movements in real time. Events are difficult to index, capture, and draw inference from, so pipeline gets left on the table.


OpenFunnel takes a fundamentally different approach: a time-aware context graph that streams custom-defined market events as they happen, not a list you pull once and work until it goes stale. When your AI agents can catch the moment a pain-point emerges and act on it through a single API call, the gap between signal and action collapses. That's the future of GTM, and OpenFunnel is building it.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try OpenFunnel and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/openfunnel) |[OpenFunnel Website](https://openfunnel.ai/)
