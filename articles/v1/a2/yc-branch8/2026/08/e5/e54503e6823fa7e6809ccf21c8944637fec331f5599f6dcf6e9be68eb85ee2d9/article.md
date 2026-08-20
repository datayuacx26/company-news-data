---
schema_version: "1.0.0"
document_id: "e54503e6823fa7e6809ccf21c8944637fec331f5599f6dcf6e9be68eb85ee2d9"
company_key: "yc-branch8"
company: "Branch8"
source_id: "yc-branch8-news-import-c52687a2f2d5"
canonical_url: "https://branch8.com/posts/shopify-plus-b2b-features-expansion-guide-2026"
published_at: "2026-08-10T03:00:42+00:00"
first_seen_at: "2026-08-10T09:26:44.875574+00:00"
fetched_at: "2026-08-10T09:26:45.366953+00:00"
content_hash: "sha256:12fada103741d3d4765c73b356e83eb90c20e572de2efb370faa677cb5540d9b"
---

# Shopify Plus B2B Features Expansion Guide 2026: A Practical Playbook

**Quick Answer:** To expand your Shopify Plus store into B2B in 2026, activate company accounts with location-based permissions, build tiered pricing catalogs per currency, configure net payment terms, integrate your ERP with rate-limited middleware, and pilot with your top 10 wholesale accounts before full launch.


---


Imagine this: your DTC Shopify Plus store already handles $3M in monthly revenue across Hong Kong and Singapore. Your wholesale inquiries arrive via email, get processed through spreadsheets, and ship with manually calculated trade discounts. Now imagine that same wholesale channel generating $1.2M per month through self-service ordering, automated net-30 payment terms, and real-time ERP sync — all running on the same Shopify Plus instance your DTC channel uses.


*Related reading:*[How to Build a Headless Commerce Business Case That Gets Board Approval](https://branch8.com/posts/how-to-build-a-headless-commerce-business-case)


That's exactly what we built for a Hong Kong-based consumer goods brand in Q1 2026. The Shopify Plus B2B features expansion guide 2026 you're reading is the distilled playbook from that project and three others like it across APAC. This isn't a feature overview. It's a step-by-step operational guide for e-commerce directors who need to expand a running DTC store into a legitimate B2B channel without breaking what already works.


*Related reading:*[Omnichannel Retail Data Architecture APAC Guide 2026: A Step-by-Step Blueprint](https://branch8.com/posts/omnichannel-retail-data-architecture-apac-guide-2026)


*Related reading:*[AI Workflow Automation ROI Measurement Framework for 2026](https://branch8.com/posts/ai-workflow-automation-roi-measurement-framework-2026)


*Related reading:*[WTO E-Commerce Agreement Impact for APAC Sellers: A Step-by-Step Adaptation Guide](https://branch8.com/posts/wto-e-commerce-agreement-impact-apac-sellers-guide)


According to Shopify's 2025 Commerce Trends report, 65% of B2B buyers now prefer self-service digital ordering over sales rep interactions. In Asia-Pacific specifically, Forrester Research projects B2B e-commerce transaction volume will exceed $9 trillion by 2027. The window to capture wholesale digitally isn't closing — but your competitors are already stepping through it.


## Prerequisites Before You Start


### Confirm Your Shopify Plus Plan and Feature Access


Shopify Plus starts at $2,300/month (Shopify's official pricing page, 2026). Before building anything, verify you have access to the B2B-specific features that remain Plus-exclusive: unlimited catalogs, direct catalog-to-company assignment, partial payment collection, and custom checkout extensibility via Checkout UI Extensions. As IWD Agency's 2026 analysis correctly notes, some B2B features have trickled down to lower Shopify plans, but the full expansion toolkit still requires Plus.


*Related reading:*[Managed Squad vs In-House Team Total Cost 2026: A Full Breakdown](https://branch8.com/posts/managed-squad-vs-in-house-team-total-cost-2026)


Run this quick check in your admin:


```text
1  Settings → B2B → Verify "Company accounts" toggle is available    2  Settings → Payments → Confirm "Payment terms" section is visible    3  Settings → Markets → Check expansion store allocation (up to 9 included)
```


### Map Your B2B Requirements Against Native Capabilities


Before writing a single line of code, document your actual B2B requirements across four dimensions:


- **Pricing complexity** : Do you need company-specific pricing, volume-based tiers, or both?
- **Payment terms** : Net-15, net-30, net-60? Do you need deposit collection upfront?
- **Order workflows** : Will buyers self-serve, or do sales reps create draft orders?
- **Integration needs** : Which ERP, WMS, or accounting system must sync?


This requirements map determines whether native Shopify Plus B2B features cover your use case or whether you'll need middleware and custom apps. In our experience, roughly 70% of mid-market APAC brands can operate within native features plus Shopify Flow. The remaining 30% need custom integration work — usually because of legacy ERP constraints.


### Assemble Your Technical Stack


You'll need these tools ready before proceeding:


- **Shopify Plus admin access** with checkout.liquid deprecated in favour of Checkout Extensibility (mandatory since August 2025 per Shopify's developer changelog)
- **Shopify CLI v3.x** for building custom apps and extensions
- **A staging/development store** — never build B2B features on your live DTC store first
- **API credentials for your ERP** (SAP Business One, Oracle NetSuite, Microsoft Dynamics, or MYOB for AU/NZ operations)
- **Shopify Flow** enabled (included with Plus)


## Step 1: Activate Company Accounts and Define Your Customer Hierarchy


### Setting Up the Company Account Structure


Shopify's B2B model centres on "Companies" — distinct from individual customer accounts. Each company can have multiple locations, multiple contacts with different permissions, and assigned catalogs. This is the foundational architecture for everything that follows.


In your Shopify Plus admin:


```text
1  Customers → Companies → Add company    2    → Company name    3    → Add location(s) — each with shipping address and tax ID    4    → Add contacts — assign roles: "Order only", "Location admin", or "Full access"
```


For APAC operations, pay special attention to location-level tax IDs. Hong Kong businesses use Business Registration numbers, Singapore uses UEN, Taiwan uses Unified Business Numbers (統一編號), and Australia uses ABN. Each location can carry its own tax exemption status, which matters significantly for cross-border wholesale.


### Designing Permission Levels That Match Real Procurement Teams


One mistake we see repeatedly: brands configure flat permission structures where every buyer contact can do everything. Real B2B procurement doesn't work that way. A purchasing officer at a Taipei retail chain shouldn't have the same access as their CFO.


Shopify Plus now supports three contact permission tiers. Map them to your customers' actual org charts:


- **Order only** : Junior buyers who place repeat orders against pre-approved catalogs
- **Location admin** : Regional procurement managers who can manage their location's orders and view invoices
- **Full access** : Finance controllers or business owners who manage payment methods and view all company activity


According to Gartner's 2025 B2B Buying Survey, the average B2B purchase involves 6-10 decision-makers. Your permission structure should accommodate this complexity, not flatten it.


### Migrating Existing Wholesale Customers from Spreadsheets


If you're like most of our APAC clients, your current B2B "system" is a combination of Excel files, WhatsApp messages, and email threads. Migrating this data into Shopify's company structure requires a structured CSV import.


Prepare your migration file with these columns:


```text
1  company_name,location_name,address_line1,city,country_code,tax_id,contact_email,contact_role,catalog_name    2  "Golden Dragon Trading","HK Warehouse","Unit 5 Kwai Chung","Hong Kong","HK","BR-12345678"," [email protected]  ","Full access","Wholesale Tier A"
```


Use Shopify's Company API (Admin API version 2026-01) for bulk imports exceeding 50 companies. The REST endpoint handles up to 250 companies per batch, and the GraphQL mutation` companyCreate` is more flexible for complex location hierarchies.


We migrated 340 wholesale accounts for a Hong Kong food and beverage brand in 11 days using a custom Node.js script that pulled from their legacy MYOB Advanced system, transformed the data, and pushed it through the Company API. The key learning: validate tax IDs before import. We lost two days troubleshooting import failures that traced back to malformed Taiwan UBN numbers.


Ready to Transform Your Ecommerce Operations?


Branch8 specializes in ecommerce platform implementation and AI-powered automation solutions. Contact us today to discuss your ecommerce automation strategy.


[Get Started](https://branch8.com/contact)


## Step 2: Build Your Pricing Architecture with Catalogs


### Creating Tiered Catalog Structures


Catalogs are how Shopify Plus handles B2B pricing. Each catalog contains product pricing overrides and can be assigned to specific companies or company locations. Plus gives you unlimited catalogs — a critical advantage over lower-tier plans, which cap at three (as Skailama's 2026 guide confirms).


Design your catalog hierarchy before building:


- **Tier A (Strategic Partners)** : 40-55% off retail, assigned to your top 20 accounts
- **Tier B (Regular Wholesale)** : 25-40% off retail, your standard B2B pricing
- **Tier C (Marketplace/Reseller)** : 15-25% off retail, for smaller retail partners
- **Regional Catalogs** : Market-specific pricing for different APAC currencies


```text
1  Products → Catalogs → Create catalog    2    → Name: "APAC Wholesale Tier A - HKD"    3    → Price adjustment: Fixed prices (recommended) or percentage off    4    → Assign to: [Select companies]    5    → Quantity rules: Minimum order 24 units, increment by 12
```


### Fixed Prices vs. Percentage Discounts: Choose Deliberately


Shopify offers two pricing models within catalogs. Percentage-based adjustments are faster to set up but create problems when your DTC prices fluctuate. A flash sale on your consumer site suddenly drops your wholesale price below cost.


Fixed prices per product per catalog give you full control. The trade-off is maintenance overhead — every price change requires updating each catalog individually. For brands with fewer than 500 SKUs, fixed pricing is manageable. Above that, build a Shopify Flow automation that recalculates catalog prices based on a master price list.


At Branch8, we recommend fixed pricing for APAC B2B operations because of currency volatility. When the Japanese yen swung 12% against USD in Q4 2025 (per Reuters FX data), our clients with percentage-based catalogs saw their B2B margins compress overnight. Fixed pricing with quarterly reviews gives you control.


### Setting Quantity Rules and Volume Breaks


B2B buyers expect minimum order quantities (MOQs) and case-pack increments. Configure these at the catalog level:


```text
1  Catalog → Product → Quantity rules    2    → Minimum: 24    3    → Maximum: 10,000    4    → Increment: 12    5
6  Catalog → Product → Volume pricing    7    → 24-99 units: $8.50    8    → 100-499 units: $7.25    9    → 500+ units: $6.00
```


Volume pricing tiers display directly on the product page for logged-in B2B customers. This self-service pricing transparency is what Shopify's own 2026 B2B operations report calls one of the top features reducing "quote request friction" — they found it cuts average order cycle time by 35%.


## Step 3: Configure Payment Terms and Draft Order Workflows


### Enabling Net Payment Terms


Cash on delivery doesn't scale for wholesale. Your B2B buyers expect net terms, and Shopify Plus now supports this natively without third-party apps.


Navigate to:


```text
1  Settings → Payments → Payment terms    2    → Enable: Net 15, Net 30, Net 60, Due on receipt, Due on fulfillment    3    → Assign default terms per company
```


For APAC specifically, consider that payment term expectations vary significantly by market. Hong Kong businesses typically expect net-30. Australian retailers often push for net-60. Taiwanese distributors frequently negotiate net-45 with early payment discounts (2/10 net-45). Configure your default terms by market, then override at the company level.


Shopify Plus also now supports ACH payments and, as of early 2026, partial payment collection — meaning you can require a 30% deposit at order time with the remaining 70% due on net-30 terms. This is particularly useful for large orders shipping from APAC to overseas buyers where trade finance risk is a real concern.


### Building Draft Order Workflows for Sales-Assisted Selling


Not every B2B order should be self-service. For high-value accounts or custom product configurations, your sales team needs to create draft orders.


Draft orders in Shopify Plus support:


- B2B catalog pricing applied automatically when the company is selected
- Custom line item discounts stacked on top of catalog pricing
- Payment terms assignment at the order level
- Tax-exempt flagging per location


Set up a Shopify Flow automation to route draft orders through an approval chain:


```text
1  Trigger: Draft order created    2  Condition: Order total > $10,000    3  Action: Send Slack notification to #sales-approvals channel    4  Action: Add tag "pending-approval" to draft order    5  ---    6  Trigger: Draft order tag contains "approved"    7  Action: Send invoice to customer automatically
```


This gives your sales managers oversight on large orders while allowing junior reps to process standard reorders independently.


### Managing B2B Invoicing Across Multiple Tax Jurisdictions


APAC B2B invoicing is complicated by differing tax systems. Hong Kong has no VAT/GST. Singapore charges 9% GST (increased from 8% in January 2024 per IRAS). Australia charges 10% GST. Taiwan has 5% business tax.


Shopify Plus handles tax-exempt B2B transactions per company location. But — and this is a significant limitation — it does not generate tax-compliant invoices for every APAC jurisdiction. Taiwan's e-invoice (電子發票) system requires integration with a government-certified turnkey provider. Singapore's InvoiceNow standard requires Peppol-format e-invoicing.


We solve this by connecting Shopify Plus to Xero or a localised invoicing system via middleware. For our Taiwan clients, we built a custom Shopify app that generates MOF-compliant e-invoices through the EZPay turnkey service and pushes them back to the order record.


Ready to Transform Your Ecommerce Operations?


[Get Started](https://branch8.com/contact)


## Step 4: Integrate Your ERP and Fulfillment Systems


### Choosing Your Integration Architecture


ERP integration is where most B2B expansion projects stall. You have three architecture options:


- **Direct API integration** : Shopify Plus to ERP via REST/GraphQL. Works well for modern cloud ERPs like NetSuite or Xero. Shopify Plus offers 500% higher API rate limits than standard plans — up to 100 requests per second on the REST Admin API (Shopify Developer Docs, 2026).
- **Middleware platform** : Tools like Celigo, MuleSoft, or Boomi sit between Shopify and your ERP. Best for complex transformation logic or when connecting multiple systems.
- **Custom integration layer** : A bespoke Node.js or Python service. We use this when clients run legacy on-premise ERPs (common with SAP B1 installations across APAC manufacturing).


### Syncing Inventory, Orders, and Customer Data


The minimum viable integration for B2B covers three data flows:


- **Inventory levels** (ERP → Shopify): Sync every 15 minutes minimum. B2B buyers ordering 500 units need accurate stock counts, not stale data. Use Shopify's` inventorySetQuantities` GraphQL mutation for bulk updates.
- **Orders** (Shopify → ERP): Push confirmed B2B orders to your ERP as sales orders. Include company name, location, payment terms, and catalog pricing. Map Shopify's order metafields to your ERP's custom fields.
- **Customer/Company data** (bi-directional): Credit limits set in your ERP should reflect in Shopify's company records. New companies created in Shopify should auto-create customer records in your ERP.


Example webhook listener for order creation:


```text
1  // Express.js webhook handler for Shopify B2B order     2  app  .  post  (  '/webhooks/orders/create'  ,     async     (  req  ,   res  )     =>     {     3      const   order   =   req  .  body  ;     4        5      if     (  order  .  purchasing_entity  ?.  company_id  )     {     6        // This is a B2B order     7        const   erpPayload   =     {     8          customerRef  :   order  .  purchasing_entity  .  company_id  ,     9          locationRef  :   order  .  purchasing_entity  .  company_location_id  ,     10          paymentTerms  :   order  .  payment_terms  ?.  payment_terms_name  ,     11          lineItems  :   order  .  line_items  .  map  (  item     =>     (  {     12            sku  :   item  .  sku  ,     13            quantity  :   item  .  quantity  ,     14            unitPrice  :   item  .  price  ,     15            // Catalog price is already applied     16          }  )  )  ,     17          currency  :   order  .  currency  ,     18          taxExempt  :   order  .  tax_exempt     19        }  ;     20          21        await     pushToERP  (  erpPayload  )  ;     22      }     23        24    res  .  status  (  200  )  .  send  (  'OK'  )  ;     25   }  )  ;
```


### Multi-Currency Considerations for APAC Operations


If you're selling B2B across Asia-Pacific, you're dealing with HKD, SGD, TWD, AUD, MYR, PHP, VND, and potentially JPY and KRW. Shopify Plus supports multi-currency through Shopify Markets, but B2B catalogs and Markets interact in ways that trip up many implementers.


Key rule: each catalog's prices are tied to the market's currency. If you have an APAC Tier A catalog priced in HKD and a buyer from your Singapore market places an order, Shopify will apply the currency conversion — but using Shopify's exchange rate, not your negotiated B2B rate.


The fix is to create separate catalogs per currency for your major B2B markets. Yes, this creates more catalogs to manage. But it gives you precise control over B2B pricing in each currency, independent of FX fluctuations. For a client running B2B across four APAC markets, we maintain 12 catalogs (3 tiers × 4 currencies) managed through a master Google Sheet that syncs to Shopify via a scheduled Cloud Function.


## Step 5: Launch, Test, and Scale Your B2B Channel


### Running a Controlled Pilot with Your Top 10 Accounts


Don't launch to all wholesale customers simultaneously. Select 10 accounts representing different tiers, markets, and ordering patterns. Give them a 2-week pilot window with dedicated support.


Track these metrics during pilot:


- **Order completion rate** : What percentage of started B2B orders are completed? Below 60% signals UX friction.
- **Average order value vs. previous manual orders** : Should be comparable or higher (self-service often increases AOV by 15-25% according to McKinsey's 2025 B2B Pulse Survey).
- **Support tickets per order** : Aim for fewer than 0.3 tickets per order after week one.
- **Payment terms utilisation** : Are buyers selecting the terms you've configured?


### Automating Reorders with Shopify Flow


B2B revenue compounds through repeat purchasing. Set up automations that reduce friction for reorders:


```text
1  Flow: Reorder Reminder    2  Trigger: Order fulfilled + 30 days elapsed    3  Condition: Order has B2B company tag    4  Action: Send email with previous order summary and "Reorder" deep link    5  ---    6  Flow: Low Stock Alert for Key Accounts    7  Trigger: Inventory quantity < threshold    8  Condition: Product is in "Tier A" catalog    9  Action: Email assigned sales rep with restock timeline
```


These automations are what turn a B2B channel from a project into a revenue engine. One of our Singapore-based clients saw reorder automation increase their B2B repeat purchase rate from 45% to 72% within three months.


### Leveraging Expansion Stores for Dedicated B2B Storefronts


Shopify Plus includes up to nine expansion stores at no additional cost (per Shopify Plus pricing, 2026). Consider deploying a dedicated expansion store exclusively for B2B if your wholesale catalog, branding, or checkout experience differs significantly from DTC.


The advantage: complete separation of themes, navigation, and content between DTC and B2B without complex conditional logic. The trade-off: inventory and product data must sync between stores, adding integration complexity.


For most APAC brands under 2,000 SKUs, we recommend running B2B and DTC on a single store using Shopify's native B2B context switching. Above 2,000 SKUs with significantly different B2B merchandising, a dedicated expansion store pays for itself in reduced development complexity.


Ready to Transform Your Ecommerce Operations?


[Get Started](https://branch8.com/contact)


## Common Mistakes and Troubleshooting


### Mistake 1: Ignoring B2B-Specific Theme Customisation


Shopify's default themes don't optimise for B2B buying patterns. B2B buyers need quick-order lists, bulk add-to-cart by SKU, and saved order templates. If you launch with a stock Dawn theme and no B2B customisation, expect buyer complaints within the first week.


Install or build a B2B-focused theme section that includes:


- CSV upload for bulk ordering
- Quick-order table with SKU search
- "Reorder previous" functionality
- Visible quantity pricing tiers on product pages


### Mistake 2: Misconfiguring Tax Exemptions Across Markets


We've debugged this issue on four separate projects. The symptom: B2B orders in tax-exempt markets still calculate tax. The cause is usually that the company's tax exemption is set at the company level but the specific location doesn't have the correct tax ID format.


Always verify tax exemption at the location level, not just the company level. Run a test order for every market before going live.


### Mistake 3: Not Rate-Limiting Your ERP Sync


When you first enable B2B and your wholesale team starts processing orders, you can easily overwhelm a legacy ERP's API. SAP Business One's Service Layer, for example, handles approximately 50-100 concurrent requests before performance degrades.


Implement a job queue (Bull, RabbitMQ, or AWS SQS) between Shopify webhooks and your ERP. This buffers spikes during high-order periods and retries failures automatically.


```text
1  // Bull queue for ERP order sync     2   const   orderQueue   =     new     Bull  (  'erp-order-sync'  ,     {     3      redis  :     {     host  :   process  .  env  .  REDIS_HOST     }  ,     4      limiter  :     {     5        max  :     10  ,            // Max 10 jobs     6        duration  :     1000       // Per second     7      }     8   }  )  ;     9
10  orderQueue  .  process  (  async     (  job  )     =>     {     11      await     pushToERP  (  job  .  data  )  ;     12   }  )  ;
```


### Mistake 4: Launching Without Clear Credit Limit Enforcement


Shopify Plus doesn't natively enforce credit limits on B2B accounts using net payment terms. A buyer on net-30 terms with $50,000 outstanding can place another $50,000 order without restriction. You need a custom checkout extension or a Shopify Function to block orders that would exceed a company's credit limit stored in your ERP.


This is non-negotiable for APAC wholesale operations where credit risk management is fundamental to trade finance.


### Mistake 5: Treating B2B Analytics Identically to DTC


Your DTC dashboard metrics (conversion rate, bounce rate, sessions) don't translate directly to B2B. A B2B buyer who visits once per month and places a $25,000 order is far more valuable than 10,000 DTC sessions.


Set up separate analytics views for B2B using Shopify's customer segment filters. Track B2B-specific KPIs: average order value per company, reorder frequency, catalog utilisation rate, and days-sales-outstanding on net terms.


## Further Reading


- [Shopify B2B Documentation](https://shopify.dev/docs/apps/build/b2b) — Official developer reference for Company APIs, catalog management, and B2B checkout extensions
- [Shopify Plus Pricing and Features (2026)](https://www.shopify.com/plus) — Current plan details including expansion store allocation
- [Forrester: Asia-Pacific B2B E-Commerce Forecast](https://www.forrester.com/) — Market sizing and growth projections for APAC digital wholesale
- [McKinsey B2B Pulse Survey 2025](https://www.mckinsey.com/capabilities/growth-marketing-and-sales/our-insights/b2b-pulse) — Self-service ordering adoption benchmarks and buyer preference data
- [Shopify Flow Documentation](https://shopify.dev/docs/apps/build/flow) — Automation trigger and action reference for B2B workflows
- [IRAS GST Rate Change 2024](https://www.iras.gov.sg/) — Singapore's current GST framework for B2B tax compliance
- [Gartner B2B Buying Report](https://www.gartner.com/en/sales/insights/b2b-buying-journey) — Research on B2B purchase decision complexity and stakeholder mapping


The Shopify Plus B2B features expansion guide 2026 you've just worked through covers the core build. But the real trajectory here is convergence — B2B and DTC operations merging onto unified platforms where the same product data, inventory, and customer intelligence serve both channels. Shopify is moving aggressively in this direction with every quarterly release. The brands that build their B2B channel now, on solid architectural foundations, will compound that advantage as platform capabilities expand.


For APAC specifically, we expect the next 18 months to bring native trade finance features, deeper regional payment method support (PayNow for SG, FPS for HK in B2B contexts), and improved localised invoicing. The foundation you build today using the steps above will accommodate those features as they ship.


**If you're planning a Shopify Plus B2B expansion across Asia-Pacific and need a team that's built this before,**[reach out to Branch8](https://branch8.com/contact) **. We scope B2B projects in two weeks and typically launch pilot channels within 8-10 weeks.**
