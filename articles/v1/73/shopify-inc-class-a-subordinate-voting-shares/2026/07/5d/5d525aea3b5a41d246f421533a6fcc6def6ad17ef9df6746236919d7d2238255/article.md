---
schema_version: "1.0.0"
document_id: "5d525aea3b5a41d246f421533a6fcc6def6ad17ef9df6746236919d7d2238255"
company_key: "shopify-inc-class-a-subordinate-voting-shares"
company: "Shopify Inc."
source_id: "shopify-inc-class-a-subordinate-voting-shares-news-import-b162817d780b"
canonical_url: "https://www.shopify.com/blog/store-migration-shopify-app"
published_at: "2026-07-21T18:58:33+00:00"
first_seen_at: "2026-07-22T13:22:21.421268+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:0d99251d75fa4af5f4db23ca5d309e8ec68eb613686745394867b2ba57cb44eb"
---

# How to Use the Store Migration Shopify App & Import Data into Shopify (2026)

A store migration is the process of moving your business’s digital infrastructure—including data, storefronts, SEO strategies, and operations—from one platform to another.


US ecommerce sales reached $1.2 trillion in 2025,[according to the US Census Bureau](https://www.census.gov/retail/ecommerce.html) . The platforms store owners use to capture that revenue impact their ability to grow, adapt, and operate during high-traffic periods. Concerns about losing product data, disrupting search engine rankings, or unexpected downtime are legitimate, especially if you’re processing a large volume of transactions every day both online and offline.


The free[Store Migration app for Shopify](https://apps.shopify.com/store-migration) offers a straightforward way to import product and customer data without technical expertise. This guide shares how to import store data into Shopify with the Store Migration app, including a pre-migration checklist and pre-launch quality check to make sure the migration process runs smoothly.


## What Shopify’s Store Migration app does


Store Migration is a free, Shopify-built app to help you quickly import product and customer data from other platforms into a new Shopify store. You can use it to move data from Square, WooCommerce, Etsy, Wix, Amazon, eBay, Clover, and Lightspeed R/X (Vend) into Shopify.


The Store Migration app is best for fast imports of specific objects; it’s not a full[replatforming](https://www.shopify.com/enterprise/blog/ecommerce-replatforming-guide) by itself. There are two ways to import data:


1. Upload a CSV file exported from your previous platform.
2. Connect your account directly for automatic data transfer via a guided setup (for supported platforms only).


What the Store Migration app imports What it doesn’t import


- Product titles, descriptions, and variants
- Pricing (standard and compare-at prices)
- Inventory levels
- Product images
- SKUs and barcodes
- Customer names, email addresses, and phone numbers
- Billing and shipping addresses


- Orders and order history
- Collections
- Navigation menus
- [SEO data](https://www.shopify.com/enterprise/blog/replatforming-seo-strategies)
- Blog posts and pages
- Reviews
- Integrations and apps


Here are a few questions to help you determine whether the Store Migration app is right for your store:


- Do you only need to move products and customers? The app imports product and customer data. Orders, reviews, and navigation menus require separate tools or manual setup after import.
- Is your store on a supported platform? If your platform isn’t on the list above, see the[Shopify Help Center](https://help.shopify.com/en/manual/migrating-to-shopify) for other options.
- Do you use a single price across all store locations? The app sets prices to the highest price for a product and doesn’t import inventory by location. You’ll need to configure those manually in Shopify after import.


Why BKLYN Larder switched from Square to Shopify POS


Watch a panel discussion to learn why Mandy Wynn, owner and proprietor of cheese shop BKLYN Larder, decided to switch from Square to Shopify POS, and see the success she’s enjoyed since.


[Watch now](https://www.shopify.com/webinar/bklyn-larder-switch-from-square-to-shopify?utm_source=blog&utm_medium=content&utm_campaign=retail-bau_q425wuzkvh_Q425WUZKVH#register)


## All migration paths Shopify supports


There are several paths retailers can take when migrating to Shopify, each of which is a process rather than a one-click event. The Store Migration app is one lane best suited for smaller brands looking only to import product and customer data.


If you need complex catalog rules, business-to-business (B2B) pricing, or multistore, consider another migration path:


Migration path Store Migration app Partner-led migration (or[white glove migration](https://www.shopify.com/enterprise/blog/white-glove-migrations) ) Manual/CSV + theme rebuild


Speed Minutes to hours for a basic import. Guided setup for supported platforms. Requires scoping, data mapping, quality analysis (QA), and handoff which elongate the process. Every object—products, customers, theme—requires manual work or custom scripts.


Cost Free. Highest (depends on complexity). Free if you do it yourself, but time-intensive. Developer hours add up quickly.


Risk Limited to supported data types; unsupported fields may require manual correction. Most[implementation partners](https://www.shopify.com/enterprise/blog/implementation-partner) include QA, testing, and rollback planning. Depends on the accuracy of your own process and review. Technical skills required.


SEO control URL redirects, meta fields, and canonical tags are not handled by the app. It requires manual setup in Shopify after import. A partner can handle full redirect mapping, metadata migration, and Search Console handover. Offers full control, but requires search engine optimization (SEO) knowledge to execute correctly.


Catalog complexity Best suited to straightforward catalogs. Multi-variant products with complex pricing or SKU structures need cleanup post-import. Handles high complexity well—including custom attributes, bundled products, and non-standard data. Handles any complexity, but effort scales directly with catalog size.


Orders/history Orders importing require a separate third-party app. A partner can migrate complete order history including line items, refunds, and fulfillment status. Possible via third-party CSV importers.


[Integrations](https://www.shopify.com/enterprise/blog/data-integration-challenges) Third-party apps, enterprise resource planning systems (ERPs), and payment gateways must be reconfigured manually in Shopify. Covered as part of scoped work; a partner will audit your current stack and recommend Shopify equivalents. Requires separate research and setup for each integration.


## Pre-migration checklist


The most common cause of a difficult store migration is the data going into it. Duplicate product records, inconsistent product variant naming, and unresolved operational decisions create problems that no migration app can fix automatically. Working through this checklist before you import reduces the likelihood of errors that are harder to resolve after the fact.


### 1. Clean up your data


Most migration pain is caused by messy source data. Before migrating anything, agree on which system holds the authoritative version of each data type:


- SKU numbers
- Product titles
- Variant names
- Customer emails
- Image filenames
- Shipping weights
- Pricing rules


Remove or archive any discontinued products or inactive[customer accounts](https://www.shopify.com/enterprise/blog/unified-customer-profiles) , and do a full backup of your current platform’s data before you close it down.


**Tip:** Implement a change freeze in the run up to your store migration. This means no new products, no price edits, no active discount codes, and no promotional campaigns that modify product data. Communicate the freeze clearly to anyone on your team with admin access. Use this window to complete your final data export and backup.


### 2. Prioritize essential functionality


To avoid the migration becoming an open-ended project, think in phases rather than attempting to move everything at once.


Essential functionality (the minimum your store needs to trade) should go live first. This includes your active product catalog, current customer accounts, and core shipping and payment configuration. Everything else—including historical order data, archived products, and advanced integrations—can follow.


A phased approach also reduces risk. A smaller, cleaner initial import is easier to audit and correct than a single large migration where errors are harder to trace. It also shortens the time your team spends in a transitional state, where two systems run simultaneously, each with associated costs.


Heritage retailer[Le Chameau](https://www.lechameau.com/) took this approach and completed its transition to Shopify in just five weeks (down from the 19 weeks it originally forecast), which included two weeks of user acceptance testing. This rapid migration[saved them between £160,000 and £170,000](https://www.shopify.com/case-studies/lechameau) in costs and significantly improved operational efficiency.


“Our total cost of ownership has gone down by 65%,” says former digital marketing officer Aaron Smedley. “We’ve been able to be a lot more agile and respond quicker to either needs of the business or customer needs.”


### 3. Confirm operational dependencies


These dependencies aren’t imported by the Store Migration app, so you’ll need to configure them manually after Shopify[data migration](https://www.shopify.com/enterprise/blog/ecommerce-data-migration) :


- Document your current tax settings (rates, exemptions, and any regional rules applicable to your locations).
- List all active[shipping profiles](https://help.shopify.com/en/manual/fulfillment/setup/shipping-profiles/setting-up-shipping-profiles) , including rates, carrier integrations, and any location-specific rules.
- Confirm all fulfillment locations (warehouses, retail stores, or third-party logistics providers) and how inventory is split between them.
- Verify your email domain configuration (transactional emails, customer notifications) and note any[domain name system (DNS) records](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-manual) you’ll need to update.
- List all third-party apps or integrations currently in use, and confirm Shopify equivalents are in place before go-live.


### 4. Set an analytics baseline


An analytics baseline gives a reliable way to determine whether changes in traffic, revenue, or conversion rate after launch are a result of the migration itself, or whether outside factors like seasonal variation influenced performance.


Before migrating, export the following[key performance indicators (KPIs)](https://www.shopify.com/blog/what-is-kpi) from your current platform over a 90-day window. This smooths out short-term fluctuations and gives you a more representative picture of normal performance:


- Top landing pages
- Top revenue pages
- Current conversion rate, segmented by device
- Average page load time
- Organic search sessions
- Revenue by channel
- Average keyword ranking positions


Once your new Shopify store has been live for 30 days, revisit each of these figures. A 30-day comparison will surface any immediate issues—a drop in indexed pages, for example, often signals a redirect problem. A 90-day comparison gives a more complete picture of whether SEO and revenue performance has recovered to the pre-migration baseline.


Retail talk: The real cost of bad data


It’s time to get real about retail data problems. Join industry experts for an honest conversation about what bad data really costs retailers. No sugar-coating—just real stories and real solutions.


[Watch now](https://www.shopify.com/webinar/cost-of-bad-retail-data?utm_source=blog&utm_medium=content&utm_campaign=retail-bau_q425wuzkvh_Q425WUZKVH)


## How to use the Store Migration app


Once you’ve prepared your data for import, here’s how to move it to Shopify using the Store Migration app:


1. **Install the**[Store Migration app](https://apps.shopify.com/store-migration) **and pick source.** Options include ecommerce platforms (WooCommerce, Wix, Lightspeed), marketplaces (Etsy, Amazon, and eBay), and point-of-sale[(POS) systems](https://www.shopify.com/blog/point-of-sale-system) (Lightspeed, Square, Clover).
2. **Export data from source.** Each platform has its own guidance. Clover, for instance, separates data exports into three categories: products, customers, and orders. Find the relevant list inside the platform and export it as a CSV file.
3. **Prep CSV.** Normalize headers, variants, images, and weights.
4. **Import files.** Check the **Publish to online store** button. Shopify will match your CSV to its own template once you upload the file into the app. This process takes a few minutes.
5. **Run validation pass.** Press **View products** and check the data has been imported correctly.
6. **Fix errors.** Common importing errors include duplicate SKUs, missing images, and shipping weights. Re-run import or make bulk edit corrections to fix.


Import product and customer information into Shopify with the free Store Migration app.


### Quality check before launch


Before going live with the new site and closing down your old platform, run a final quality check that covers:


- **Products.** Check variant count, prices, inventory, and images. Start with your highest-value and highest-traffic products, then work through the remainder of your catalog. Pay particular attention to products with multiple variants—these are a common source of import errors.
- [Customer data](https://www.shopify.com/enterprise/blog/customer-data) **.** Check for duplicate customer profiles and marketing consent (where applicable). This information is responsible for every[personalized touchpoint](https://www.shopify.com/enterprise/blog/personalized-customer-experience) on Shopify, so it needs to be accurate.
- **Storefront.** Walk through the following as a customer would, on both desktop and mobile. Check collections, navigation, and search results appear as expected.


## What success can look like


Learn more about how migration outcomes from real Shopify migrations:


### FAUCHON: 50% faster checkout speeds post-migration


Parisian luxury food brand[FAUCHON](https://www.fauchon.com/en) migrated from Magento to Shopify to unify its retail and ecommerce operations across 70 boutiques, franchise partners, and its online store.


On Magento, the team was burdened by high maintenance costs and slow updates—particularly problematic given that more than 50% of FAUCHON’s ecommerce revenue falls in the holiday season. In-store, outdated POS systems slowed checkout, complicated inventory management, and left staff unable to order out-of-stock items for customers on the spot.


After adopting[Shopify POS](https://www.shopify.com/pos) in its Parisian flagship stores, FAUCHON’s sales associates could process transactions faster and check inventory in real time. They could also place direct orders for unavailable products, thanks to unified data inside the POS system. The results were measurable: a[20% increase in add-to-cart rate](https://www.shopify.com/case-studies/fauchon) , a 4% peak ecommerce conversion rate, and checkout speeds 50% faster than before.


“Before Shopify, we would spend all of October worrying about technical issues for the holiday season,” says marketing director Marianne Tanguy-Rossé. “Now, we can actually focus on what matters—our products, our customers, and our sales.”


### Bauer: 60% revenue increase within six months


Sports retailer[Bauer](https://www.bauer.com/?srsltid=AfmBOoqIFKm86qywuD7vrv5NQR__7ADqJZCH_XQOvQX5cRS4Nu6f7c11) is one of the most recognized brands in hockey. When reevaluating its tech stack, it determined that its previous ecommerce platform (Salesforce Commerce Cloud) wasn’t serving it as well as it could. The retailer faced high development costs and long timelines if it wanted to make a change to the site.


Bauer enlisted the help of Shopify Partner agency[CQL](https://www.shopify.com/partners/directory/partner/cql1) to migrate to Shopify and reduce total cost of ownership (TCO). CQL helped Bauer redesign its site with categories, develop a customer loyalty program, and integrate third-party apps like[SAP](https://apps.shopify.com/sap-business-one-integration) . Plus, with its new page designer feature, non-designers could make changes to the site without technical input.


In the six months post-migration,[Bauer recorded](https://www.shopify.com/case-studies/bauer) :


- 60% increase in revenue
- 30% increase in orders
- 20% cost savings


“The move to Shopify has been so beneficial to our business,” says Chris Cocca, Bauer’s chief digital officer. “We get so much out of the box, plus a huge pipeline of new features dropping regularly. And the Shopify ecosystem of apps and developers helps us move fast. This has let us shift focus from basic ecommerce feature development and maintenance to high-value areas for our consumers, like content creation and loyalty.”


### Bombas: $108,000 in platform cost savings


Sock and apparel brand[Bombas](https://bombas.com/) ’s appearance on *Shark Tank* catapulted the brand to success. But when the episode premiered (and re-ran), the worst happened: Bombas’s site fell apart. Product images broke and customers couldn’t check out, leading to losses of more than $15,000 in just a few minutes of outage.


Bombas began paying thousands of dollars each month in server maintenance fees to keep the lights on. It needed a better solution, so it turned to Shopify.


Bombas timed its migration with the busiest time of the year:[Black Friday Cyber Monday](https://www.shopify.com/blog/bfcm-strategies) (BFCM). They pull in as much as 60% of annual revenue during the final quarter of the year, so if the new site could handle the busiest shopping day, re-runs of its *Shark Tank* appearance wouldn’t be a problem.


Bombas’s new Shopify-powered site didn’t crash during the BFCM weekend—nor any re-airs. They[saved $108,000 in platform costs](https://www.shopify.com/case-studies/bombas) in their first year alone, recording $17.2 million in sales in the first full year post-migration (300% year-over-year growth).


“From an infrastructure standpoint, Shopify Plus has our back,” says Bombas CTO Adam Weinstock. “We feel incredibly comfortable that, as we continue to scale, our website’s going to be up and running. As an IT function, that allows us to really focus on the things that we do best.”


The future of retail: Why unified commerce is no longer optional


New research shows businesses using unified commerce platforms like Shopify POS see 22% better total cost of ownership and 20% faster implementation. Learn what this means for your retail strategy.


[Get the report](https://www.shopify.com/pos/resources/pos-market-report?utm_source=blog&utm_medium=content&utm_campaign=retail-bau_Q425WUZKVH)


## Store migration Shopify app FAQ


### Is Shopify’s Store Migration app free?


Shopify’s Store Migration app is free. You can use it to bulk import data from another platform—such as Etsy, Clover POS, or Amazon—into Shopify.


### What platforms does the Shopify Store Migration app support?


Shopify’s Store Migration app supports the following platforms:


- Square
- WooCommerce
- Etsy
- Wix
- Amazon
- eBay
- Clover
- Lightspeed R/X (Vend)


### What does the Shopify Store migration app import?


The Store Migration app imports product and customer data from your existing platform into Shopify. You can do this by importing CSV files or connecting your source account.


### What common issues happen?


Common issues that happen when importing data using the Store Migration app include:


- Incorrect variants and SKUs
- Missing images or product descriptions
- Incorrect shipping weights
- Duplicate customer profiles
- Missing customer tags


### How long does a Shopify migration take?


Most businesses complete their Shopify migration in[less than three months](https://www.shopify.com/migrate) . This is much faster for some brands—like Gresham Blake, which[replatformed in just two weeks](https://www.shopify.com/case-studies/gresham-blake) and recorded a 68% increase in online sales.
