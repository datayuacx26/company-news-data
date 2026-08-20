---
schema_version: "1.0.0"
document_id: "cab17d1112e2fd39fb6b8f80474e079294b456b44d7220392a075eef4c56f0bb"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/tako-ai-data-analyst-for-your-application"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:ff3e7d21b18e67b4f1b2a343b12665c114059c769d31ec8605f51e44621de707"
---

# Tako: AI Data Analyst Embedded into Your Application

Most AI tools return walls of text. When your users ask about data, they don't want paragraphs. They want charts, tables, and visual answers they can trust.


Today we're excited to announce our partnership with[Tako](https://tako.com/) , an AI data analyst that embeds directly into your application. Ask a question in natural language, get back interactive visual analytics built from structured, cited data sources.


## What is Tako?


Tako is a search engine for visualizing and sharing the world's knowledge. Instead of generating answers from unsourced web data, Tako pulls real-time answers directly from licensed, structured sources and returns them as visual knowledge cards.


Think of it as having an AI data analyst that turns your questions into beautiful, trustworthy insights, complete with charts, tables, maps, and methodology notes.


Here's what happens when you ask Tako *"Nvidia vs AMD revenue since 2015"* :


One natural-language question. An interactive chart with cited sources from S&P Global, ready to embed. Try hovering over the data points above.


This chart was generated from an actual Tako API call through Orthogonal.


### Key Capabilities


- **Natural language to visuals** : Ask questions in plain English, get back interactive charts and reports
- **Trusted, cited data** : Every result comes from high-quality, structured sources across finance, macroeconomics, geopolitics, sports, weather, and more
- **Knowledge cards** : Compact, interactive representations of data designed to be embedded and shared
- **Private data support** : Upload your own datasets with Tako Connect and get AI-powered visualizations from your data
- **Embeddable** : Cards can be themed to match your product and embedded anywhere


### Visualize Your Own Data


Tako also makes it easy to visualize your data with its Visualize and ThinViz endpoints.[Visualize](https://docs.tako.com/api-reference/visualize) takes data plus a natural language query and resolves it to a visualization using Tako's reasoning infra. For example, your agent could pass a database about UFO sightings and ask Visualize to "Create a treemap visualizing report count by location (stripping country prefixes) to compare relative sizes, and title it 'Top Regions for UFO Sightings'" and receive this:


If you want your agent to handle reasoning, but want an attractive, responsive visualization to inject into without having to waste money and latency on codegen, you can use[ThinViz](https://docs.tako.com/documentation/integrating-tako/guides/chart-creation) to take advantage of Tako's card library and component system.


## Using Tako with Orthogonal


### Knowledge Search


Query Tako's knowledge base with natural language and get back structured visual results:


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


// Search for visual knowledge cards


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


"tako"


,


path


:


"/v1/knowledge_search"


,


body


:


{


inputs


:


{


text


:


"Nvidia vs AMD headcount since 2013"


}


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


// Returns structured knowledge cards with interactive charts


`


### Using the REST API


` curl


-X


POST


'https://api.orth.sh/v1/run'


\\


-H


"Authorization: Bearer


$ORTHOGONAL_API_KEY


"


\\


-H


'Content-Type: application/json'


\\


--data


'{


"api": "tako",


"path": "/v1/knowledge_search",


"body": {


"inputs": {


"text": "US GDP growth rate over the last 10 years"


}


}


}'


`


### Using x402 Protocol


Tako on Orthogonal also supports[x402](https://www.x402.org/) , an open protocol that enables native payments in HTTP. Your agents can pay for API calls directly using USDC stablecoins, no API keys required.


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


const


url


=


"https://x402.orth.sh/tako/v1/knowledge_search"


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


inputs


:


{


text


:


"Tesla stock price vs revenue since 2020"


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


## What Makes Tako Different


Traditional search engines and AI chatbots return paragraphs of unstructured text. Tako focuses on **AI data visualization** , building visually compelling, structured knowledge cards that cite their sources and are embeddable anywhere.


Each knowledge card includes:


- **Clear sources** so you know where the data came from
- **Timestamps** to show how recent the data is
- **Methodology notes** that explain how the data was calculated


For AI developers, this means your results become more accurate, real-time, citeable, and engaging.


## Topics & Data Sources


Tako covers a wide range of topics powered by sources like S&P Global, SimilarWeb, CoinMarketCap, YouGov, Polymarket, Sportradar, and more. Here's a sample of what you can ask:


### Global Public Companies


- **Stock Prices** : "Nvidia stock price"
- **Earnings & Estimates** : "Coinbase earnings results"
- **Financial Ratios** : "Spotify liquidity ratios"
- **Industry Normalized Financials** : "Citigroup income statement"
- **Valuation Metrics** : "Airbus market cap"
- **Competitive Analysis** : "Hilton competitors stock performance"
- **Transactions** (M&A, Real Estate, Buybacks, Spinoffs, VC): "Alibaba M&A history", "Databricks M&A"
- **Non-GAAP Operating Metrics** : "Airbus operating metrics"
- **Salaries (H-1B)** : "Netflix salaries"
- **Funding** : "Cohere funding"


### Prices & Rates


- **Forex** : "134 euros in dollars"
- **Commodities** : "Oil price"
- **Metals** : "Silver price"
- **Crypto** : "Bitcoin price"
- **Real Estate** : "San Francisco condo prices"


Ask Tako *"Bitcoin price"* and you get a live chart:


### Economics & Demographics


- **Key Economic Indicators** : "US unemployment rate"
- **Energy Indicators** : "US oil exports to China"
- **Country Economic Indicators** : "China inflation"
- **Country Demographic Indicators** : "Colombia population"
- **Country Health Indicators** : "Mexico fertility rate"
- **Country Military Indicators** : "Ukraine military spending"
- **US Health Stats** : "Cocaine use in the USA"


### Prediction Markets & Public Opinion


- **Prediction Markets** : "How many Fed rate cuts this year"
- **Public Opinion Polling** : "Trump rating"


Ask *"Trump approval rating"* and Tako pulls polling data from YouGov:


### Weather


- **Current + Forecasts** : "Tokyo weather"
- **Averages** : "Does it rain in Shanghai in July?"


### Sports


- **Games** : Pre-game odds, in-game scores, post-game results
- **Schedules** : League and team schedules
- **Stats** : Player and team statistics


### App & Web Analytics


- **Web Traffic** : "Claude.ai traffic in Germany"


## Use Cases


- **Research and analysis** : Ask complex questions about finance, economics, or market trends and get visual answers instantly
- **Embedded analytics** : Add Tako to your product so your users can query data in natural language
- **Content creation** : Generate charts and visualizations for reports, articles, and presentations
- **Agent workflows** : Give your AI agents the ability to retrieve and present structured data visually
- **Due diligence** : Pull financial data, company metrics, and market comparisons on demand


## Why We Partnered with Tako


Tako solves a problem that text-based AI can't: turning questions into trustworthy visual answers. Their knowledge cards are interactive, cited, and built from structured data rather than scraped web content. For agents that need to present data to users, that's a meaningful upgrade over plain text responses.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Tako and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/tako) |[Tako Website](https://tako.com/)
