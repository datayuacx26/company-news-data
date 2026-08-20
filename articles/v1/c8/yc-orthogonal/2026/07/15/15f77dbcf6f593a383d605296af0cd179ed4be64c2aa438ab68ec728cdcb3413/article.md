---
schema_version: "1.0.0"
document_id: "15f77dbcf6f593a383d605296af0cd179ed4be64c2aa438ab68ec728cdcb3413"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/andi-search-ai-powered-search"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:842c80266e6b0a89c87b1e9b95748e95f2c77389f08f66abac289fcda0f3f09c"
---

# Andi Search: AI Search You Can Trust

When AI agents search the web, they need accurate, reliable results - not ads, spam, or SEO-gamed content. They need a search engine built for truth, not clicks.


Today we're excited to highlight[Andi Search](https://andisearch.com/) , a next-generation AI search engine now available on Orthogonal.


## What is Andi Search?


Andi is an AI-powered search engine that works like humans do - combining language models with reasoning and common sense. Unlike traditional search engines that rely on snippets and ranking signals, Andi reads and understands full page content to deliver accurate, synthesized answers.


Loved by over 1 million users worldwide, Andi has been benchmarked to beat Google on AI accuracy. And it does all this while being completely private - no logging, no tracking, no ads.


## Key Features


### Accurate AI Answers


Get direct, reliable answers to any question. Andi uses an ensemble of language models to synthesize information from credible sources, not just surface-level snippets.


### Private by Design


Andi doesn't log or track your searches. Period. Your queries stay private, making it perfect for sensitive research and enterprise applications.


### Source-Cited Results


Every answer comes with sources so you can verify information. Explore results in depth or trust the synthesized answer - your choice.


### Summarize & Explain


Andi can summarize long articles, explain complex topics, and generate text content - all grounded in real web data.


## Using Andi Search with Orthogonal


### AI-Powered Search


` // Using @orth/sdk - The simplest way to call APIs


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


// Search with AI-powered answers


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


"andi"


,


path


:


"/v1/search"


,


query


:


{


"q"


:


"What are the best practices for RAG applications?"


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


`


### Research Any Topic


` // Get intelligent search results with instant answers


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


"andi"


,


path


:


"/v1/search"


,


query


:


{


"q"


:


"latest developments in autonomous AI agents 2026"


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


`


### Using x402 Protocol


Andi Search on Orthogonal also supports[x402](https://www.x402.org/) - an open protocol that enables native payments in HTTP. With x402, your agents can pay for API calls directly using USDC stablecoins, with no API keys required.


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


,


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


// Make a paid request - payment is automatic


const


query


=


"best practices for RAG applications"


;


const


url


=


\`


https://x402.orth.sh/andi/v1/search?q=


${


encodeURIComponent


(


query


)


}


\`


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


x402 enables a new paradigm where AI agents can autonomously pay for services using stablecoins - no subscriptions, no API key management, just seamless agent-to-agent payments.


## Available Endpoints


Endpoint Method Description


/v1/search GET Fast, high-quality search with intelligent ranking, instant answers, and result enrichment


## Why Agents Love Andi


1. **Accuracy** : Benchmarked to beat Google on AI answer accuracy
2. **Privacy** : No tracking, no ads, no data collection
3. **Trust** : Every answer has verifiable sources
4. **Intelligence** : Understands context and nuance like a human
5. **Clean results** : No SEO spam or clickbait


## How Andi is Different


Traditional search engines (and most AI search tools) use Bing, Google, or Brave results combined with LLMs to generate answers from snippets. Andi is different:


- **Own search index** ranked by meaning and credibility
- **Full page understanding** instead of snippet-based synthesis
- **Ensemble approach** using multiple language models
- **Human-like reasoning** with common sense


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Andi Search and dozens of other APIs. No API keys to manage, no accounts to create - just instant access to powerful tools for your agents.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/andi) |[Andi Website](https://andisearch.com/)
