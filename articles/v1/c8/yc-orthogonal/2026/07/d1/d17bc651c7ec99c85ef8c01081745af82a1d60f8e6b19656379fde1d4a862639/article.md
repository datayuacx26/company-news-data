---
schema_version: "1.0.0"
document_id: "d17bc651c7ec99c85ef8c01081745af82a1d60f8e6b19656379fde1d4a862639"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/riveter-web-data-for-your-products"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:a2a3c94c786e644ca488bdc65ba069d9fdae1904afb6a01722b288cd01f893bd"
---

# Riveter: Power Your Product with Data from the Web

Building web scrapers is painful. You need to handle proxies, browser infrastructure, rate limits, anti-bot detection, and constantly fix things when websites change. It's a never-ending maintenance burden.


Today we're excited to highlight[Riveter](https://riveterhq.com/) , a Y Combinator-backed platform that replaces your entire scraper infrastructure with a single API.


## What is Riveter?


Riveter is an all-in-one web search agent that handles web search, scraping, browser infrastructure, and proxies for you. Just tell it what data you want - in plain English - and get structured, source-cited results back.


No more building and maintaining scrapers. No more proxy management. No more broken pipelines.


## Key Features


### Web Search Agent


Riveter's agents combine multiple AI tools to find and extract data: SERP search, web scraping, image reading, PDF scraping, headless browsers, and automatic proxy rotation.


### Custom Data Outputs


Tell Riveter exactly what data you want using natural language prompts. Define your input data, write a prompt describing what you need, set your response format, and get consistent results.


### Real-Time API Integration


Generate custom API endpoints with a single click. Every response includes sources, audit trails, and reasoning - so you know exactly where your data came from.


### Workflow Automation


Create self-evolving workflows where Riveter automatically pulls in new data and feeds it into the next step of your sequence.


## Using Riveter with Orthogonal


### Scrape a Webpage


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


// Scrape text content from any public webpage


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


"riveter"


,


path


:


"/v1/scrape"


,


body


:


{


url


:


"https://stripe.com/about"


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


### Run a Custom Data Extraction


` // Define your input and output structure in one request


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


"riveter"


,


path


:


"/v1/run"


,


body


:


{


input


:


{


company


:


\[


"Stripe"


\]


// Input values must be arrays


}


,


output


:


{


founding_year


:


{


prompt


:


"What year was this company founded?"


,


contexts


:


\[


"company"


\]


// Reference input keys for context


}


,


headquarters


:


{


prompt


:


"Where is the company headquarters located?"


,


contexts


:


\[


"company"


\]


}


}


}


}


)


;


// Check the run status


const


status


=


await


orthogonal


.


run


(


{


api


:


"riveter"


,


path


:


"/v1/run_status"


,


query


:


{


run_key


:


result


.


data


.


run_key


}


}


)


;


// Get the processed data when complete


const


data


=


await


orthogonal


.


run


(


{


api


:


"riveter"


,


path


:


"/v1/run_data"


,


query


:


{


run_key


:


result


.


data


.


run_key


}


}


)


;


console


.


log


(


data


.


data


.


formatted_data


)


;


// { company: \[{ value: "Stripe" }\],


// founding_year: \[{ value: "2010" }\],


// headquarters: \[{ value: "San Francisco, California, U.S." }\] }


`


### Using x402 Protocol


Riveter on Orthogonal also supports[x402](https://www.x402.org/) - an open protocol that enables native payments in HTTP. With x402, your agents can pay for API calls directly using USDC stablecoins, with no API keys required.


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


url


=


"https://x402.orth.sh/riveter/v1/scrape"


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


url


:


"https://stripe.com/about"


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


x402 enables a new paradigm where AI agents can autonomously pay for services using stablecoins - no subscriptions, no API key management, just seamless agent-to-agent payments.


## Use Cases


Riveter is trusted by teams across industries:


- **E-Commerce** : Research competitors' products, pricing, and inventory
- **Sales & GTM** : Enrich leads and prepare for customer calls
- **Risk & Fraud** : Collect KYB/KYC data on businesses and people
- **Finance** : Find revenue data on private companies for due diligence
- **Industry Labeling** : Classify companies with NAICS codes, SIC codes, or custom labels


## Available Endpoints


Riveter offers these endpoints through Orthogonal:


Endpoint Method Description


/v1/scrape POST Scrape a webpage and return text content


/v1/run POST Define input data and output structure in one request


/v1/run_status GET Check the current status of a project run


/v1/run_data GET Retrieve processed data from a completed run


/v1/stop_run POST Stop a currently running project


## Why Choose Riveter


1. **No infrastructure** : Skip building scrapers, proxies, and SERP
2. **Source-cited** : Every data point comes with its source
3. **Custom outputs** : Define exactly the data format you need
4. **Real-time API** : Generate endpoints with one click
5. **YC-backed** : Trusted by leading startups


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Riveter and dozens of other APIs. No API keys to manage, no accounts to create - just instant access to powerful tools for your agents.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/riveter) |[Riveter Website](https://riveterhq.com/)
