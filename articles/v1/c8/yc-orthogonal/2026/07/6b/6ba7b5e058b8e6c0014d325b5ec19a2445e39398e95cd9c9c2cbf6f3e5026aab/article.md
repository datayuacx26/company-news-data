---
schema_version: "1.0.0"
document_id: "6ba7b5e058b8e6c0014d325b5ec19a2445e39398e95cd9c9c2cbf6f3e5026aab"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/happenstance-ai-network-search-for-agents"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:b314e007cc9d4169c4cb762c77ec23ae7754329c072283b966d442995ff03fdf"
---

# Happenstance: Search Your Network with AI for Sales, Hiring & Fundraising

Cold outreach has a 2% response rate. Warm intros convert at 40%. The difference is your network, but most people barely use it. You know someone who knows the perfect candidate, investor, or buyer. You just don't know that you know.


Today we're excited to announce our partnership with[Happenstance](https://happenstance.ai/) , a network search engine that lets you search your connections with AI, now available on Orthogonal. Happenstance connects to your email, calendar, LinkedIn, and other platforms to make your entire professional network searchable with natural language.


## What is Happenstance?


Happenstance indexes your connections across Gmail, Calendar, Contacts, LinkedIn, Instagram, Twitter, and Outlook into one searchable graph. Ask it anything: "Who do I know at Stripe?", "Find me engineers in my network who've worked at fintech startups", or "Who can intro me to Series A investors in SF?" It searches not just the people you know directly, but the people they know.


Loved by 400,000+ users, Happenstance turns your existing relationships into your unfair advantage.


## Key Features


### Person Research


Get deep background research on anyone: their recent posts, hiring history, career trajectory, areas of expertise, and how you're connected. Feed in a name, company, title, or social handle and Happenstance pulls together a comprehensive profile.


## Using Happenstance with Orthogonal


Happenstance's research API is available on Orthogonal. Give your agent a name, company, title, or social handle and get back a comprehensive professional profile.


### Research a Person


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


// Start a research request


const


research


=


await


orthogonal


.


run


(


{


api


:


"happenstance"


,


path


:


"/v1/research"


,


method


:


"POST"


,


body


:


{


description


:


"Garry Tan Y Combinator"


}


}


)


;


console


.


log


(


research


)


;


// Returns: { id: "research_abc123", url: "https://happenstance.ai/research/..." }


`


### Get Research Results


` // Poll for results (research runs asynchronously)


let


profile


;


while


(


true


)


{


profile


=


await


orthogonal


.


run


(


{


api


:


"happenstance"


,


path


:


\`


/v1/research/


${


research


.


id


}


\`


,


method


:


"GET"


}


)


;


if


(


profile


.


status


===


"COMPLETED"


)


break


;


await


new


Promise


(


r


=>


setTimeout


(


r


,


2000


)


)


;


}


console


.


log


(


profile


)


;


// Returns: background summary, recent activity, career history,


// expertise areas, mutual connections, and more


`


### Using x402 Protocol


Happenstance on Orthogonal supports[x402](https://www.x402.org/) for autonomous agent payments using USDC stablecoins.


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


// Research a person with automatic USDC payment


const


response


=


await


fetchWithPayment


(


"https://x402.orth.sh/happenstance/v1/research"


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


description


:


"Garry Tan Y Combinator"


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


## Available Endpoints


Endpoint Method Description


/v1/research POST Start a person research request


/v1/research/{research_id} GET Get research results


## Use Cases


### Sales Prep


Research decision-makers before outreach. Get their career history, recent activity, areas of expertise, and how you're connected. Turn a cold email into a personalized message that shows you've done your homework.


### Recruiting


Research candidates before interviews. Pull together their career trajectory, expertise areas, and public activity so your team walks into every conversation with full context.


### Investor Research


Research investors before pitching. Understand their background, portfolio companies, and recent activity. When you know who you're talking to, your pitch becomes a conversation.


### Meeting Prep


Research anyone before a meeting. Get a comprehensive profile with career history, recent public activity, expertise areas, and how you're connected. Walk into every conversation with context.


## Why We Partnered with Happenstance


Before any meeting, call, or outreach, the question is always the same: who am I talking to? Happenstance answers that by pulling together everything about a person across your connected platforms into one comprehensive research profile. Career history, recent activity, expertise, mutual connections. Context that would take hours to gather manually, delivered in seconds through an API call.


For AI agents, this is transformative. An agent preparing for a sales call can research the decision-maker, pulling together career history, recent activity, and mutual connections, all through a single API call. Instead of going in blind, your agent walks you into every conversation with full context on who you're talking to.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Happenstance and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/happenstance) |[Happenstance Website](https://happenstance.ai/)
