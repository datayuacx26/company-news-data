---
schema_version: "1.0.0"
document_id: "8c9482842b7955aa81828cf7d1320275c78659b3e24389366b7a764dccd725db"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/brand-dev-brand-data-api"
published_at: null
first_seen_at: "2026-07-22T07:50:11.536244+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:356cd722474b6ead3b126e54ffeab54dc7942134534528a9f526f02e1e16ec2d"
---

# Brand.dev: Instant Brand Data for Any Domain

Building a product that touches multiple companies? You need their brand assets. Logos, colors, fonts, screenshots. Traditionally this means scraping websites, guessing colors, or begging customers for brand guidelines.


Today we're highlighting[Brand.dev](https://brand.dev/) , a powerful brand data API now available on Orthogonal.


## What is Brand.dev?


Brand.dev extracts complete brand information from any domain. Pass in a URL, get back logos, colors, industry classification, company description, and more. No scraping, no guesswork.


## Key Features


### Brand Retrieval


Get logos, backdrops, colors, industry, and descriptions from any domain. Multiple lookup methods: domain, company name, stock ticker, ISIN, or email.


### Design System Extraction


Extract complete styleguides including typography, color palettes, spacing, shadows, and UI components directly from a brand's website.


### AI Data Extraction


Use AI to extract custom data points from any website. Define what you need, Brand.dev crawls and extracts it.


### Transaction Identification


Identify brands from transaction data (bank statements, receipts). Great for fintech apps that need to display merchant logos.


## Using Brand.dev with Orthogonal


### Retrieve Brand Data


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


// Get brand data for any domain


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


"brand-dev"


,


path


:


"/v1/brand/retrieve"


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


output


)


;


// Returns logos, colors, industry, description, socials, and more


`


### Get Simplified Brand Data


` // Get just the essentials: logo, colors, backdrops


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


"brand-dev"


,


path


:


"/v1/brand/retrieve-simplified"


,


query


:


{


domain


:


"notion.so"


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


// Returns: { domain, title, colors, logos, backdrops }


`


### Extract Website Styleguide


` // Extract complete design system


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


"brand-dev"


,


path


:


"/v1/brand/styleguide"


,


query


:


{


domain


:


"linear.app"


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


// Returns typography, colors, spacing, shadows, UI components


`


### Identify Brand from Transaction


` // Identify merchant from bank transaction data


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


"brand-dev"


,


path


:


"/v1/brand/transaction_identifier"


,


query


:


{


transaction_info


:


"AMZN MKTP US*2X1234567"


,


country_gl


:


"US"


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


// Returns: Amazon brand data with logo, colors, etc.


`


### AI-Powered Data Extraction


` // Extract custom data points from any website


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


"brand-dev"


,


path


:


"/v1/brand/ai/query"


,


body


:


{


domain


:


"anthropic.com"


,


data_to_extract


:


\[


{


datapoint_name


:


"pricing_tiers"


,


datapoint_type


:


"list"


,


datapoint_description


:


"Names of pricing plans"


,


datapoint_example


:


"Free, Pro, Enterprise"


}


,


{


datapoint_name


:


"founded_year"


,


datapoint_type


:


"number"


,


datapoint_description


:


"Year the company was founded"


,


datapoint_example


:


"2021"


}


\]


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


Brand.dev on Orthogonal supports[x402](https://www.x402.org/) for autonomous agent payments using USDC stablecoins.


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


// Get brand data with automatic USDC payment


const


url


=


"https://x402.orth.sh/brand-dev/v1/brand/retrieve?domain=figma.com"


;


const


response


=


await


fetchWithPayment


(


url


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


Endpoint Method Description Price


/v1/brand/retrieve GET Get full brand data by domain $0.03


/v1/brand/retrieve-simplified GET Get essential brand data $0.03


/v1/brand/retrieve-by-name GET Look up brand by company name $0.03


/v1/brand/retrieve-by-ticker GET Look up brand by stock ticker $0.03


/v1/brand/retrieve-by-email GET Look up brand by email domain $0.03


/v1/brand/retrieve-by-isin GET Look up brand by ISIN code $0.03


/v1/brand/transaction_identifier GET Identify brand from transaction $0.03


/v1/brand/styleguide GET Extract design system $0.03


/v1/brand/fonts GET Extract font information $0.03


/v1/brand/screenshot GET Capture website screenshot $0.03


/v1/brand/naics GET Get NAICS industry code $0.03


/v1/brand/ai/query POST AI-powered data extraction $0.03


/v1/brand/ai/products POST Extract product information $0.03


## Use Cases


### Fintech & Banking


Display merchant logos and colors in transaction feeds. Transform "AMZN MKTP US*123" into a beautiful Amazon card with logo and brand colors.


### CRM & Sales Tools


Auto-populate company profiles with logos and brand info when adding new leads. No manual data entry.


### White-Label Products


Automatically theme your product to match each customer's brand. Extract their colors, fonts, and apply them to your UI.


### Competitive Intelligence


Extract pricing, products, and company data from competitor websites using AI queries.


### Design Tools


Import a company's complete design system to create on-brand assets.


## Why Agents Need Brand Data


Modern agents interact with hundreds of companies. Displaying brand information makes experiences more:


1. **Trustworthy** : Users recognize familiar logos and colors
2. **Professional** : No broken images or placeholder text
3. **Personalized** : Match the context to the company being discussed
4. **Automated** : No human intervention to collect assets


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Brand.dev and dozens of other APIs. No API keys to manage, no accounts to create.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/brand-dev) |[Brand.dev Website](https://brand.dev/)
