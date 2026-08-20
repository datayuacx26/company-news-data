---
schema_version: "1.0.0"
document_id: "17ae054dadff35b1060fce710e648fbcbabf9c996279aa38a5b143ac544b4124"
company_key: "yc-afternoon-co"
company: "Afternoon.co"
source_id: "yc-afternoon-co-news-import-d204747fe0e0"
canonical_url: "https://www.afternoon.co/blog/RAG-state-taxes"
published_at: null
first_seen_at: "2026-07-21T20:47:37.461730+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:b4e4cde8cf51b73e24de73ca2432725290a8ee950655b2d8a8927e7e91ff26fc"
---

# How Retrieval-as-a-Service (RAG) Companies Handle Multi-State Tax

If you run a Retrieval-as-a-Service platform, you are not just selling “software.” You store data, generate embeddings, run retrieval, and call LLMs, often inside a single request. From a tax angle, that looks like several different services packed into one API call.


States do not have a clean category for this yet. They borrow from old rules for SaaS, data processing, and information services. Each state picks a different bucket, which is where the fun starts.


If you want to keep building instead of decoding 50 different tax regimes,[Afternoon’s bookkeeping](https://www.afternoon.co/product/bookkeeping) and[tax services](https://www.afternoon.co/product/taxes) can help you map each API to a tax category, track exposure by state, and stay audit ready.


This article is general information, not tax or legal advice. Always check your exact facts with a tax advisor.


---


## Quick facts


For most RAG and retrieval-heavy AI platforms:


Layer / topic How states often see it What to watch


Vector DB storage Often treated like hosting or storage Some states tax stand alone storage when that is the main service


Embedding generation Data processing or information service Texas data processing rules, New York information services rules


Retrieval and ranking Automated search or information service Digital automated services in Washington, information in New York


LLM augmentation Data processing plus information service Use of OpenAI or Anthropic does not remove your exposure


Overall RAG API SaaS, data processing, or information service depending on state Washington, Texas, New York, Massachusetts, Chicago as early focus


Nexus drivers High transaction counts and fast growing B2B deals Economic nexus thresholds and where customers use your API


Pricing models Per query, token based, or subscription plus usage Tokens can look like prepaid credits, bundles create mixed deals


---


## Why RAG is hard to classify


Old school software is simple. You sell access to an app. States either tax remote software or they do not.


RAG is different. A single request might cover:


- Retrieval from a private or shared index
- Data processing to rank or filter results
- API access to your platform
- LLM generation on top of that context


States do not have a “RAG” rule. They lean on three older categories:


- Data processing services
- Information services
- Remote access to software and digital automated services


Your product probably fits all three. That is why the tax answer jumps around once you cross state lines.


---


Talk to a sales tax expert


Get guidance tailored to your business


---


## Breaking down the RAG stack for tax


You do not need to walk an auditor through your whole architecture. You do need a plain description of each layer.


### Vector database storage


What it does:


- Stores embeddings or documents
- Exposes read and write operations on that index


Tax view in many states:


- Looks like hosting or storage sold to businesses
- Often not taxed by itself, unless storage is clearly the main thing you sell


If you bill storage as a separate line item, some states will treat it as its own service. That can be good or bad depending on the state.


### Embedding generation


What it does:


- Takes text or other inputs
- Creates embeddings using your model or a third party model


This is classic data processing:


- You take customer data
- Run it through automated systems
- Return a transformed version


States that tax data processing or electronic information services will look hard at this layer. That includes Texas and a few others.


### Retrieval and ranking


What it does:


- Accepts a query
- Searches your index or other sources
- Returns ranked hits or chunks


From a tax lens, this looks like search and research:


- In Washington, online research and search tools are digital automated services
- In New York, furnishing electronic reports and search results falls under information services


If your marketing says “search your private data like Google,” expect to be treated like a search and information service in many states that tax digital services.


### LLM augmentation


What it does:


- Sends the query and context to an LLM
- Returns an answer, plan, or tool call


States do not care that the answer came from a neural net. They still see:


- Data in
- Automated processing
- Information out


So the LLM step tends to fall under the same data processing or information service rules as your retrieval.


---


## How key states treat RAG style services


No one has a clean “RAG tax rule” yet. But you can borrow from how they treat similar services.


### Washington State


Washington has rules for digital products and digital automated services. Those rules cover many online tools that use software to deliver search, reports, or automated results.


For a RAG API, Washington will usually see:


- A digital automated service, because software runs on your side
- Results transferred electronically to your customers


Once you cross Washington’s economic nexus thresholds, you are expected to:


- Register
- Charge sales tax at destination rates
- Pay the separate B&O tax on gross receipts


Where you host, for example AWS us-west-2, matters less. Washington cares where the customer uses the service.


### Texas


Texas is known for broad data processing rules. The state defines data processing as computerized entry, retrieval, search, compilation, manipulation, or storage of data.


For RAG, Texas will usually treat:


- Embedding, vector search, and retrieval as taxable data processing
- Subscription or UI layers as data processing plus remote software access


Texas allows a 20 percent exclusion, so 80 percent of the charge is taxable. That still adds up.


If you sell to another SaaS company and they resell RAG features to their own customers, resale arguments can work in some setups. You need careful contracts and a tax pro to stand that up.


### New York


New York taxes many information services. That includes services that:


- Take in data
- Analyze or process it
- Return reports or electronic information to the customer


RAG looks like that. You accept queries, mix in customer data, and send back an answer.


New York has already applied information service rules to analytics tools, marketing data, and other “black box” outputs. Expect them to put RAG in the same bucket unless an exclusion clearly applies.


### California


California is still one of the friendlier large states for SaaS style products.


High level:


- Remote access to software is usually not subject to sales tax when nothing tangible is delivered
- Digital only services and pure SaaS often fall outside the taxable base for sales and use tax


A RAG API or hosted console delivered only over the internet usually follows that pattern.


You still need to watch for:


- Economic nexus for any taxable sales of tangible goods
- Local business and gross receipts rules
- Possible future bills that target digital services


Right now, most pure RAG access sold to California customers shows up as non taxable for sales tax, even when you still have to register for other reasons.


### Massachusetts


Massachusetts treats many cloud and SaaS products as taxable prewritten software. Its computer industry rules say that remote access to standard software is taxable in most cases.


A multi tenant RAG platform that Massachusetts customers log into will usually be treated as:


- Remote access to prewritten software
- Taxable once you have nexus with the state


If you do heavy custom work or managed services on top, you can sometimes carve that piece out. The core platform fee is still likely taxable.


### Illinois and Chicago


At the Illinois state level, many SaaS products are outside normal sales tax.


Chicago is different. The city has a lease transaction tax that applies to nonpossessory computer use and many cloud services.


If you have customers in Chicago who use your RAG platform there, you may owe this local tax even when state sales tax does not apply. Chicago has its own nexus and sourcing rules, so treat it as a separate risk item.


---


## Nexus triggers for RAG companies


RAG companies tend to hit nexus faster than classic SaaS.


### Transaction counts


Some states look at both:


- Revenue into the state
- Number of separate transactions


If a state counts “sales” by invoice or by billed usage events, a single enterprise RAG customer might push you over a 200 transaction test long before you hit a dollar threshold.


### Customer location vs usage location


Digital services are sourced in different ways:


- Some states look at where the billed customer is located
- Others look at where the service is used, based on user addresses or IP


For RAG, this can get messy:


- A Delaware C corp with a remote team might use your platform across ten states
- Certain states will argue that use in their state creates both sourcing and nexus


You do not need perfect tracking on day one. You should know which states tie tax to user location rather than account address.


### Infrastructure and data location


Owning hardware in a state can create physical nexus. Using public cloud regions usually does not by itself, but rules keep evolving.


Most early RAG companies find that:


- Customer footprint drives nexus
- Cloud region choice is more about latency and data rules than sales tax


Still, keep a simple map of where your data is stored and which pieces you control directly.


---


## Pricing model implications


How you charge makes a difference in how tax applies and how you explain it.


### Per query pricing


Per query is the cleanest for tax:


- One request
- One charge
- One primary service type


States that tax data processing or information services will apply that rule to each charge based on how you source the service.


### Token based pricing


Tokens and credits are trickier.


They can look like:


- Prepaid credits for future taxable services
- Stored value that can only be used on one type of service
- A blended price that covers several service types


Some states tax at:


- The time you sell tokens, if the only use is to buy a taxable service
- The time tokens are redeemed, when the underlying service is delivered


You need records that show:


- Where tokens were sold
- Where they were used
- What kind of service that use bought


### Monthly subscriptions


Flat plans are easier for customers but can blur service types.


A plan that includes:


- A fixed number of queries
- Storage
- Access to a workspace or console


looks more like SaaS. States that tax SaaS will apply that rule to the full charge unless you clearly separate pieces on your invoices or in your internal allocation.


### Embedded or white label RAG


If you sell RAG to other platforms:


- They may try to treat your service as a resale input in some states
- In others, you will still be treated as selling a service to them, not to their users


Your contracts, invoice descriptions, and how they bill their own customers all feed into that resale story.


---


## Simple compliance strategy for RAG teams


You cannot remove every gray area yet. You can make your position clear and repeatable.


### Step 1: Map your APIs to service types


Start with a one page table:


- Endpoints that mainly fetch and rank data → information service or digital automated service
- Endpoints that mostly transform data → data processing
- Consoles and workspaces → SaaS or remote access software


This gives your tax advisor something concrete to work from.


### Step 2: Track usage and revenue by state


At least monthly, pull:


- Revenue by billing state
- Large customer usage by region if you have that data
- Product line splits, such as “search API” vs “storage” vs “workspace”


Use that to:


- Monitor economic nexus
- Decide where to register first
- Build a simple state exposure view for your board and investors


### Step 3: Register in priority states


Most RAG teams start by registering in:


- Washington
- New York
- Texas


Then they add:


- Massachusetts as Boston and East Coast usage grows
- Chicago specific registration if they see meaningful Chicago traffic


The right list for you depends on your customer base, but those states and cities are common early hits for digital tax risk.


### Step 4: Align pricing and invoicing with tax


You do not need to change your entire pricing page. You may want to:


- Break out platform, usage, and support on invoices
- Document an internal allocation if you keep a single SKU externally
- Decide how you will treat tokens for tax before you scale that model


You will be glad you did when a state asks why nothing was taxed for three years.


---


## Documentation and audit prep


RAG is new. Auditors will not know the term. Your job is to give them a simple story they can follow.


Keep:


- Short service descriptions in plain language
- A mapping of each main endpoint to a tax category in each focus state
- A clean architecture diagram that shows where data lives and where compute runs
- Standard contracts and sample invoices for RAG customers
- A short memo that explains how you source and tax RAG charges today


If an audit appears, this packet should be ready before the first call.


---


## How Afternoon.co can help


Afternoon works with AI infra and RAG teams that:


- Sell retrieval and vector search as APIs or embedded features
- Have customers spread across several states
- Want tax questions solved before they hit eight figures of volume


We help you:


- Connect billing exports, product usage, and accounting in one clean system
- See revenue and tax exposure by state and by product layer
- Coordinate with tax pros so your contracts, invoices, and returns all tell the same story


If you want sales tax to be a known part of your model, not a surprise letter in year three,[talk to Afternoon](https://www.afternoon.co/product/taxes) .


---


### Sources


- [Washington Department of Revenue – Digital Products and Digital Automated Services](https://dor.wa.gov/forms-publications/publications-subject/tax-topics/digital-products-including-digital-goods)
- [WAC 458-20-15503 – Digital Products and Digital Automated Services](https://app.leg.wa.gov/wac/default.aspx?cite=458-20-15503)
- [Texas Admin. Code §3.330 – Data Processing Services](https://www.law.cornell.edu/regulations/texas/34-Tex-Admin-Code-SS-3-330)
- [Grant Thornton – Texas updates data processing services tax rule](https://www.grantthornton.com/insights/alerts/tax/2025/salt/p-t/tx-updates-data-processing-services-tax-rule-04-11)
- [20 NYCRR 527.3 – Sale of information services](https://www.law.cornell.edu/regulations/new-york/20-NYCRR-527.3)
- [The CPA Journal – The complexity of sales taxation of information services](https://www.cpajournal.com/2022/03/15/the-complexity-of-sales-taxation-of-information-services/)
- [CDTFA Publication 109 – Internet Sales](https://www.cdtfa.ca.gov/formspubs/pub109/)
- [Massachusetts 830 CMR 64H.1.3 – Computer Industry Services and Products](https://www.mass.gov/regulations/830-CMR-64h13-computer-industry-services-and-products)
- [City of Chicago – Personal Property Lease Transaction Tax](https://www.chicago.gov/city/en/depts/fin/supp_info/revenue/tax_list/personal_propertyleasetransactiontax.html)
- [BDO – United States local taxation of SaaS: compliance strategies and potential exposures](https://www.bdo.global/en-gb/insights/tax/indirect-tax/united-states-local-taxation-of-saas-compliance-strategies-and-potential-exposures)
