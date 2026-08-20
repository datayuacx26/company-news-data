---
schema_version: "1.0.0"
document_id: "673ee8c6362579d2a393db6c937de636d77aeeb693a02fe81408a01bff2493bc"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/you-com-web-search-apis-for-ai"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:2bc7ad4ded35bd3f8b9525bb98ccd4a4d67faa8395cade0e95dfa83ad3453826"
---

# You.com: Web Search APIs for AI Agents

AI agents that can't access the web are working with stale knowledge. They hallucinate facts, miss recent events, and can't verify claims. Most web search APIs either return noisy results that need heavy post-processing, or they're too slow for real-time agent workflows. For agents that need to search, read, and reason over live web data, the options have been limited.


Today we're excited to announce our partnership with[You.com](https://you.com/home) , a web search API platform built for AI agents and LLMs. You.com powers search for DuckDuckGo, Amazon, Alibaba, OpenAI, Salesforce, and more. Their APIs deliver structured, LLM-ready results at 300ms p99 latency, serving 10M+ daily queries at 99.99% uptime.


## What is You.com?


You.com provides three core APIs that cover the full spectrum of web intelligence for AI:


- **Search API** : Real-time web and news search that returns structured JSON. Supports livecrawl for full page content inline with results. Over 10M+ news sources indexed.
- **Contents API** : Extract clean HTML or Markdown from any URL. No browser automation, no headless Chrome. Just pass a URL and get readable content back.
- **Research API** : Multi-step reasoning that searches the web, reads pages, and synthesizes cited answers. #1 on the DeepSearchQA benchmark. Control depth with` research_effort` (lite, standard, deep, exhaustive).


You.com is SOC2 certified with zero data retention, DPA-ready, and supports custom QPS limits for enterprise deployments. Their Search API delivers results 2X faster than the competition at 300ms p99 latency.


## Using You.com with Orthogonal


### Web Search


Search the web and get structured, LLM-ready results. Supports news filtering, livecrawl for inline page content, and result count control.


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


// Search for recent AI agent frameworks


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


"you"


,


path


:


"/v1/search"


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


"best AI agent frameworks 2026"


,


count


:


10


,


livecrawl


:


"all"


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


results


)


;


// Returns: web and news results with title, url, description,


// full page content (when livecrawl enabled), snippets, and metadata


`


### Content Extraction


Extract clean Markdown or HTML from any URL. No browser automation needed. Works on JavaScript-rendered pages.


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


"you"


,


path


:


"/v1/contents"


,


method


:


"POST"


,


body


:


{


urls


:


\[


"https://example.com/pricing"


,


"https://example.com/docs/api-reference"


\]


,


formats


:


\[


"markdown"


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


)


;


// Returns: array of objects with url, title, markdown content,


// and metadata for each URL


`


### Deep Research


Run multi-step research that searches, reads, and synthesizes answers with citations. Control how deep it goes with` research_effort` .


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


"you"


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


input


:


"What are the pricing models and key differentiators of the top 5 vector databases in 2026?"


,


research_effort


:


"deep"


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


output


.


content


)


;


// Returns: synthesized Markdown answer with inline citations


// and sources array with URLs, titles, and relevant excerpts


`


### Using x402 Protocol


You.com on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


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


"https://x402.orthogonal.com/you/v1/search"


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


query


:


"latest AI news"


,


count


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


### Using MPP (Machine Payments Protocol)


You.com on Orthogonal also supports[MPP](https://mpp.dev/) , the open standard for machine-to-machine payments co-authored by Tempo and Stripe. Your agents can pay for API calls with stablecoins, cards, or Bitcoin, with no API key required.


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


"https://mpp.orthogonal.com/you/v1/search"


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


query


:


"best AI agent frameworks 2026"


,


count


:


10


,


livecrawl


:


"all"


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


## Performance and Coverage


You.com's infrastructure handles the scale that production AI agents demand:


- **300ms p99 latency** : 2X faster than competing search APIs
- **10M+ daily queries** served at 99.99% uptime
- **10M+ news sources** indexed for comprehensive coverage
- **#1 on DeepSearchQA** : Top-ranked research quality benchmark (Research API)
- **SOC2 certified** with zero data retention and DPA-ready compliance


Trusted by DuckDuckGo, Amazon, Alibaba, OpenAI, Salesforce, Databricks, Harvey, Windsurf, Cognizant, and ThoughtSpot.


## Use Cases


### Agent Web Access


Give your AI agents real-time web search. Instead of relying on training data cutoffs, agents can search for current information, verify facts, and stay up to date. The Search API returns structured JSON that's ready to feed directly into LLM context windows.


### RAG with Live Data


Augment your retrieval pipeline with live web results. Use the Search API to find relevant pages, then the Contents API to extract clean text for your vector store or context window. No scraping infrastructure needed.


### Research Automation


Build agents that research topics end to end. The Research API handles the full loop: searching, reading multiple sources, synthesizing findings, and citing everything. Set` research_effort` to control the depth and cost tradeoff.


### Content Monitoring


Track competitors, industry trends, or brand mentions by running periodic searches. The Contents API can extract clean content from any page for comparison or analysis.


## Why We Partnered with You.com


When AI agents need web access, speed and quality both matter. You.com delivers both. Their APIs are purpose-built for LLMs: structured output, fast response times, and deep coverage. The Research API is particularly compelling because it handles multi-step reasoning that would otherwise require agents to orchestrate multiple search-read-synthesize loops themselves. The fact that companies like DuckDuckGo and Amazon trust You.com for their own search infrastructure says a lot about the reliability. For agents on Orthogonal, this means real-time web intelligence through a single integration.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try You.com and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/you) |[You.com Website](https://you.com/home)
