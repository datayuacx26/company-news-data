---
schema_version: "1.0.0"
document_id: "bf5a86c4af7cdfeffa05efd481919022f8fa27861a68be1e2d8381fc7bf05350"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/seltz-web-knowledge-for-ai-agents"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T21:46:31.447870+00:00"
content_hash: "sha256:c2a84e1b16391bd897e677b524f299bd3167f7cf7095f20c09ea5f1e5ea9d5db"
---

# Seltz: Web Knowledge for AI Agents

Building AI agents that need access to precise, factual, and up-to-date web information? Large language models often struggle with exact figures, niche facts, and continuously changing data, relying instead on patterns learned from past exposure. Your agents need real-time, machine-ready access to web knowledge.


Today we're excited to highlight[Seltz](https://seltz.ai/) , a Web Knowledge API designed from the ground up to serve AI as the primary consumer of the Web, now available on Orthogonal.


## What is Seltz?


Seltz is a Web Knowledge API that provides real-time, context-engineered data from the live web, optimized for use in AI systems and applications. It delivers fast, up-to-date web data, providing context-engineered web content with sources for real-time AI reasoning.


Seltz is designed for teams building large language model applications, RAG pipelines, and autonomous agents that require reliable and efficient access to live web knowledge.


## Key Features


### Context That Matters


Context is selected, filtered, and ranked to deliver only the most relevant web signals, no irrelevant clutter. Web content is processed and shaped to maximize usefulness for LLMs, AI agents, and RAG pipelines.


### Fast at Scale


Built for speed and scalability, with predictable latency and performance under real-world load. Designed for high-throughput environments, with predictable performance and support for enterprise deployment needs.


### Simple API


One endpoint. Send a query, get back structured documents with URLs and full content. No complex query syntax, no parsing raw HTML. Integrate fast, source-backed web context into your system in minutes.


### Enterprise-Grade by Design


End-to-end pipeline ownership ensures customer data remains within internal systems, without reliance on external providers. Security, availability, and confidentiality controls are designed in alignment with SOC 2 trust service principles and requirements.


## Using Seltz with Orthogonal


### Get Real-Time Web Context


Unlike cached indexes or stale training data, Seltz returns live web content. Ask about something that happened today and get structured, sourced results back.


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


// Get real-time information, not stale training data


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


"seltz"


,


path


:


"/v1/search"


,


body


:


{


query


:


"latest AI funding rounds this week"


,


max_results


:


10


}


}


)


;


// Every result comes back as a structured document with source URL and full content


// No parsing HTML. No short snippets. Just clean, machine-ready text.


for


(


const


doc


of


result


.


documents


)


{


console


.


log


(


doc


.


url


)


;


// Source URL for citation and verification


console


.


log


(


doc


.


content


)


;


// Full document content, ready for LLM context


}


`


### Structured Documents, Not Links


This is the key difference. Traditional search returns ranked links and short snippets. Seltz returns full document content with sources, structured for LLM consumption.


` // Your agent gets back structured documents it can reason over directly


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


"seltz"


,


path


:


"/v1/search"


,


body


:


{


query


:


"Claude Opus 4.7 capabilities and benchmarks"


,


max_results


:


5


}


}


)


;


// Each document has the full content, not a 160-character snippet


// Feed these directly into your LLM context window


for


(


const


doc


of


result


.


documents


)


{


console


.


log


(


\`


Source:


${


doc


.


url


}


\`


)


;


console


.


log


(


\`


Content length:


${


doc


.


content


.


length


}


chars


\`


)


;


console


.


log


(


doc


.


content


)


;


}


// Use in a RAG pipeline: pass documents directly as context


const


context


=


result


.


documents


.


map


(


doc


=>


\`


\[Source:


${


doc


.


url


}


\]\\n


${


doc


.


content


}


\`


)


.


join


(


"\\n\\n"


)


;


`


### Using x402 Protocol


Seltz on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


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


// Make a paid request (payment is automatic)


const


url


=


"https://x402.orth.sh/seltz/v1/search"


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


query


:


"best ai search engines"


,


max_results


:


10


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


x402 enables a new paradigm where AI agents can autonomously pay for services using stablecoins. No subscriptions, no API key management, just seamless agent-to-agent payments.


## Use Cases


### AI Assistants and Agents


Systems that reason over live data need current, factual information. Seltz provides the web context that agents need to answer questions accurately with sources.


### RAG Pipelines


Add real-time web knowledge to your retrieval-augmented generation pipeline. Get structured documents optimized for LLM context windows instead of raw HTML or short snippets.


### Analytical Tools


Applications that depend on accurate external facts, like current statistics, specifications, pricing, regulations, or technical documentation, can pull fresh data with a single API call.


### Knowledge Systems


For systems where correctness and freshness matter more than general language understanding, Seltz delivers up-to-date, source-backed web content.


## Why Agents Need Seltz


Seltz was designed from the ground up to serve AI as the primary consumer of the Web, providing reliable, efficient, and machine-ready access to web knowledge:


1. **Structured documents, not links** : Full content with source URLs, ready for LLM context windows
2. **Real-time web data** : Access current information, not stale training data
3. **Built for reasoning** : Web content is processed and shaped to maximize usefulness for AI applications
4. **Predictable performance** : Built for speed and scalability, with consistent latency under real-world load
5. **Privacy by design** : Full pipeline ownership ensures customer data remains within internal systems, without reliance on external providers


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Seltz and dozens of other APIs. No API keys to manage, no accounts to create. Just instant access to powerful tools for your agents.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/seltz) |[Seltz Website](https://seltz.ai/)
