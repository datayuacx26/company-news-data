---
schema_version: "1.0.0"
document_id: "d999cf627c90d918a7585e9ccc7133bc6b36f4de5a1da00f2a63839c60338e80"
company_key: "yc-afternoon-co"
company: "Afternoon.co"
source_id: "yc-afternoon-co-news-import-d204747fe0e0"
canonical_url: "https://www.afternoon.co/blog/api-metered-billing-accounting-guide"
published_at: null
first_seen_at: "2026-07-21T20:47:37.461730+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:6a89ec8d2130c584750d0792b5e2bdd4d807c9d3b5e9175ad944e22c46f44da1"
---

# API Metered Billing: How to Track Usage and Recognize Revenue

API metered billing charges customers based on how much they actually use your API. Instead of flat monthly fees, you track every API call and bill accordingly. This can create a fair pricing model for customers but requires the right systems to track usage accurately and account for revenue properly.


## Last updated: November 2026


## What Is API Metered Billing?


API metered billing means customers pay based on their actual API consumption. It is similar to a utility bill. You pay for the units you use, not a flat rate.


Common ways to meter API usage:


- Per API call or request
- Per gigabyte of data processed
- Per minute of compute time
- Per successful transaction
- Mixed metrics (for example, calls multiplied by data size)


The system captures every billable event, adds up usage for each customer, applies your pricing, and generates invoices automatically.


Why companies use metered billing:


- Customers pay only for what they use
- Low-volume customers are not scared away by high minimums
- You earn more as customers grow without constantly renegotiating contracts


---


## How API Metered Billing Works


### Step 1: Capture Every API Request


When someone hits your API, your system logs the event. This usually includes:


- Who made the request (customer ID)
- When it happened (timestamp)
- Which endpoint they called
- Whether it succeeded or failed
- How much data was transferred


This data flows into a log or streaming system (for example, Kafka, Kinesis, Pub/Sub) and then into a database for aggregation.


### Step 2: Calculate What To Charge


Your rating engine takes the raw usage data and applies pricing rules:


- Look up the customer's plan
- Count their total API calls or other units for the period
- Apply volume discounts or tiered rates
- Factor in any prepaid credits
- Generate the final charge amount


### Step 3: Create The Invoice


At the end of the billing period (usually monthly), the system:


- Sums up all usage
- Applies minimum charges if you have them
- Generates an invoice showing what was used and how it was priced
- Sends it to the customer and updates accounts receivable


### Step 4: Recognize Revenue


For accounting purposes, you recognize revenue as customers use your API, not when you send the invoice or get paid. More on this below.


---


## Common API Pricing Models


### Pay As You Go


Customers pay a fixed rate per API call with no minimums.


Example: $0.001 per request


Best for: New products, developer friendly APIs, attracting small customers.


How it works: Count every API call, multiply by your rate, bill monthly.


### Tiered Pricing


The price per call drops as volume increases.


Example:
0–1M calls: $0.0010 each
1M–5M calls: $0.0008 each
Over 5M calls: $0.0006 each


Best for: Rewarding high volume customers and encouraging growth.


How it works: Track cumulative usage and apply the correct rate as customers cross thresholds.


### Monthly Minimum With Overages


Customers commit to a minimum monthly spend. Extra usage beyond what is included costs more.


Example: $5,000 per month includes 10M calls, overages at $0.0005 each.


Best for: Predictable revenue and enterprise customers.


How it works: Charge the minimum every month. If they use more than what is included, add overage fees.


### Prepaid Credits


Customers buy credits upfront. Each API call deducts from their balance.


Example: Buy $1,000 in credits, each call costs $0.001.


Best for: Cash flow management and reducing payment failures.


How it works: The customer's credit balance decreases with each call. When the balance is low, they either buy more or the service stops.


### Different Prices Per Endpoint


Complex endpoints cost more than simple ones.


Example:
GET /users: $0.0001
POST /analyze: $0.01
GET /search: $0.005


Best for: APIs where some endpoints use far more compute or third party costs.


How it works: Tag each API call with the endpoint type and apply endpoint specific pricing.


---


## Revenue Recognition: The Accounting Side


### When Do You Record Revenue?


Under ASC 606 (the revenue recognition standard), you recognize revenue when you deliver the service. For metered APIs, that is when the customer makes the API call or consumes the metered unit.


Not when you send the invoice.
Not when the customer pays.
When you actually provide the service.


At month end, you should:


1. Pull all usage data through the last day of the month
2. Calculate total revenue earned from that usage
3. Record it in your books for that period
4. Then generate and send invoices


For usage that has occurred but not yet billed, you will often record unbilled revenue.


### The Journal Entry


When you recognize revenue from API usage that has not been billed yet:


- Debit: Unbilled Revenue (or Contract Asset)
- Credit: API Revenue


When you issue the invoice and move from unbilled to billed:


- Debit: Accounts Receivable
- Credit: Unbilled Revenue


If customers prepaid credits:


-


At the time of prepayment:


- Debit: Cash
- Credit: Deferred Revenue


-


As they use credits:


- Debit: Deferred Revenue
- Credit: API Revenue


### Why This Matters


Investors, lenders, and auditors look at when and how you recognize revenue. Doing it wrong can:


- Misstate your financial results
- Trigger audit issues or restatements
- Create problems during fundraising or an acquisition


Tools like Afternoon can help automate this by connecting usage tracking directly to your billing and accounting systems.


---


## What You Need To Build


### Usage Tracking System


Captures every API request with details needed for billing. This usually means:


- Adding instrumentation or middleware to your API gateway
- Logging to a streaming platform or durable log store
- Storing events in a database that supports time based queries
- Handling duplicates from retries and idempotent requests


### Billing Engine


Takes usage data and creates invoices. It should:


- Aggregate calls or other units by customer and period
- Apply your pricing model and discounts
- Handle tiers, minimums, and overages
- Generate customer invoices and usage line items
- Track payment status


You can use tools like Stripe Billing, Zuora, Chargebee, or custom solutions.


### Accounting Integration


Connects billing to your financial system so that:


- Revenue is recorded in the right period
- Deferred revenue for prepaid credits is tracked and released correctly
- Accounts receivable matches what you have billed
- Financial reports reflect actual usage and revenue


This often means an integration with QuickBooks, NetSuite, Xero, or other accounting software.


---


## Tax Treatment


This is a simplified overview. Actual treatment depends on your jurisdiction and specific facts.


### For API Providers


Income tax: Under an accrual method, you generally recognize income for tax purposes as you perform services and earn the revenue. For usage based APIs, that typically aligns with when API calls happen and revenue is recognized for books, but there can be differences.


Sales tax (or similar transaction taxes): Many states tax SaaS and API access as taxable services or access to software. You will need to:


- Determine where you have nexus (tax obligations)
- Check whether your product is taxable in each state
- Collect sales tax where required
- File returns and remit collected tax


Prepaid credits: Record as a liability (deferred revenue) until consumed. When customers use credits, recognize revenue and, if applicable, any related sales tax.


Reference (general background on methods):


- [IRS Publication 538 - Accounting Periods and Methods](https://www.irs.gov/publications/p538)


### For API Customers


Customers typically deduct API costs as ordinary and necessary business expenses when incurred.


Prepaid credits: If buying large blocks of credits that extend beyond 12 months, you may need to treat them as a prepaid asset and deduct as consumed, subject to rules like the 12 month rule.


Reference (general background on business expenses):


- [IRS Publication 535 - Business Expenses](https://www.irs.gov/publications/p535)


Always confirm specific treatment with a tax advisor.


---


## Month End Close Process


To close your books accurately with metered billing:


### Days 1–3 After Month End


- Pull complete usage data through the last moment of the month
- Aggregate by customer and apply pricing
- Calculate total revenue including minimums and overages
- Validate against raw usage logs and sample customers


### Days 4–5


- Generate customer invoices with usage details
- Record revenue in your accounting system
- Update deferred revenue for prepaid credit consumption
- Reconcile accounts receivable to billing records


### Days 6–7


- Review anomalies or unusual patterns
- Update forecasts based on actual trends
- Document any adjustments for audit trails


---


## Common Problems And How To Avoid Them


### Double Counting Usage


Problem: API retries create duplicate events and lead to overbilling.


Solution: Use idempotency keys or request IDs so you can deduplicate events before billing.


### Wrong Billing Period


Problem: Usage near midnight gets recorded in the wrong month.


Solution: Standardize on a single timezone (often UTC) and lock your cutoff procedures. Test edge cases around period close.


### Missing Usage Data


Problem: Some API calls do not get logged due to system issues.


Solution: Implement monitoring for gaps in usage data and build replay from durable logs or queues.


### Revenue Timing Errors


Problem: Recognizing revenue when invoiced instead of when used.


Solution: Automate revenue recognition based on usage date and reconcile against invoices regularly.


### Lost Prepaid Credits


Problem: Customer credits expire but you do not adjust your books.


Solution: Track expiration dates and write off expired credits on a schedule consistent with your policy and contracts.


---


## Handling Special Cases


### Failed API Calls


Should you charge for requests that failed?


- Server errors (5xx): Usually do not charge, since the service did not work
- Client errors (4xx): You may charge, since the customer sent a bad request and still used resources
- Rate limits: Many providers do not charge for blocked calls but count them toward quotas


Write a clear policy and apply it consistently.


### Free Tiers


Many APIs offer free usage such as the first 1,000 calls per month at no charge.


How to handle:


- Track free tier usage but do not generate revenue for it
- Only recognize revenue once the customer exceeds the free tier limits


### Service Credits For Downtime


If your API goes down and you issue credits:


- Reduce revenue in the period you issue the credits or treat as a reduction of future revenue, depending on your policy and contract terms
- Record credits as a liability if they can be used later
- Recognize back as revenue when the customer uses the credits


### International Customers


For customers outside your country:


- Decide whether to bill in their currency or your own
- Apply correct VAT or GST rules where required
- Handle currency conversion at the transaction date
- Recognize foreign exchange gains or losses where relevant


---


## Best Practices


- Start simple: Launch with basic per call pricing before layering on many tiers and bundles
- Instrument from day one: Adding usage tracking later is much harder than building it in from the start
- Show customers their usage: Provide dashboards so customers can see consumption and avoid surprise bills
- Set up alerts: Let customers configure budget warnings and usage alerts
- Test pricing carefully: Model revenue, margin, and customer costs before changing rates
- Automate reconciliation: Build checks that compare usage data to billing and to accounting records
- Document your approach: Clear documentation helps with audits and team transitions


---


## Key Metrics To Track


Revenue metrics:


- Total usage based revenue
- Revenue per API call or unit
- Revenue by customer tier or plan


Usage metrics:


- Total API calls per month
- Average calls per customer
- Growth rate in consumption


Customer metrics:


- Free tier conversion rate
- Net dollar retention
- Customer concentration


Operational metrics:


- Billing error rate
- Time to close each month
- Payment collection rate


---


## Summary


Topic Details


What it is Pay per use API pricing based on actual consumption


How it works Track usage → Apply pricing → Generate invoices → Recognize revenue


Revenue recognition As API calls occur, not when invoiced (ASC 606)


Key system needs Usage tracking, billing engine, accounting integration


Common models Pay as you go, tiered, minimums with overages, prepaid credits


Main challenge Accurately tracking high volume usage in real time


Tax treatment Recognize income as services are delivered (accrual basis)


---


## Learn More


For deeper understanding of related topics:


- Usage based pricing revenue recognition and ASC 606 overview from major firms and FASB summaries
- Token based pricing for AI services and LLM APIs


You can also review:


- Stripe Billing documentation on metered billing and usage records
- Chargebee and Zuora guides on usage based billing design


---


## Official References


- [FASB ASC 606 - Revenue from Contracts with Customers](https://www.fasb.org/standards)
