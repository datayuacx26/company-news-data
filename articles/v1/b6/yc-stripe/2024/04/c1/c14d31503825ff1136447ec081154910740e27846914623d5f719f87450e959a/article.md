---
schema_version: "1.0.0"
document_id: "c14d31503825ff1136447ec081154910740e27846914623d5f719f87450e959a"
company_key: "yc-stripe"
company: "Stripe"
source_id: "yc-stripe-news-import-ac4ed02840f0"
canonical_url: "https://stripe.com/blog/biggest-updates-sessions-2024"
published_at: "2024-04-24T00:00:00+00:00"
first_seen_at: "2026-07-21T04:05:27.623858+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:c918bdac13d8d88747bb16ed8209ba961b3a649976d9271f5b2cf2a5c0d1e14b"
---

# Our biggest product updates from Sessions 2024

This morning at our annual conference,[Stripe Sessions](https://stripesessions.com/) , we shared our biggest product updates of the year with thousands of business leaders and builders who gathered at the Moscone Center in San Francisco. Our keynote covered Stripe’s progress in our three core product areas and shared how we’re evolving Stripe to interoperate with other payments providers.


This post recaps the top announcements and product releases. For a comprehensive list of everything that’s new since Sessions last year, see our[changelog](https://stripe.com/blog/changelog) .


## Increase conversion and reduce costs with Global Payments


We shared that users who switched to our[Payment Element](https://docs.stripe.com/payments/payment-element) saw an average revenue uplift of 11.9%; that enterprises such as Hertz, Alaska Airlines, and Shopify are now delivering best-in-class in-person payments experiences with[Terminal](https://stripe.com/terminal) ; and that the world’s most sophisticated payments teams are using our AI-enabled Advanced Payments Engine to boost authorization rates, decrease costs, and combat fraud. Here’s some of the latest:


### **Optimized Checkout Suite**


- We doubled the number of payment methods we provide access to, from[50 to over 100](https://stripe.com/payments/payment-methods) —now including[Amazon Pay](https://stripe.com/payment-method/amazon-pay) ,[Revolut Pay](https://stripe.com/payment-method/revolut-pay) ,[Zip](https://stripe.com/payment-method/zip) ,[TWINT](https://stripe.com/payment-method/twint) , and[Swish](https://stripe.com/payment-method/swish) .
- With the new[payment method rules engine](https://docs.stripe.com/payments/payment-method-rules) , you can customize which payment methods are displayed at checkout based on size of the transaction, currency, or buyer location, right from the Dashboard.
- You can now configure and run payment method[A/B tests](https://docs.stripe.com/payments/a-b-testing) in the Dashboard, with no code. These tests can help you understand how new payment methods will impact your business across conversion, average order value, and share of wallet.
- We expanded support for[Adaptive Pricing](https://docs.stripe.com/payments/checkout/automatic-currency-conversion) in the UK and Europe, enabling you to automatically present prices in your customer’s local currency.
- Your customers can now more easily pay with their US bank accounts on[Link](https://stripe.com/payments/link) .


### **Terminal**


- [Stripe Reader S700](https://stripe.com/terminal/s700) is now generally available in 23 countries.
- Terminal now supports core enterprise features such as[offline mode](https://docs.stripe.com/terminal/features/operate-offline/collect-payments#:~:text=Examine%20offline%20payments-,Enable%20offline%20mode,3%20devices%20at%20your%20Location%20.) and[fleet management](https://docs.stripe.com/terminal/fleet/locations) , and we announced that surcharging is coming soon.
- We launched new[integrations with POS and PMS providers](https://support.stripe.com/questions/recommended-resources-and-partners-for-stripe-terminal#enterprise) such as Oracle OPERA, Adobe Commerce, Teamwork Commerce, Erply, Mercaux, and Tulip.


### **Advanced Payments Engine**


- Our upgraded[authentication engine](https://stripe.com/payments/authentication) now includes machine learning–based routing optimizations to determine the best authentication process for each transaction.
- Stripe now supports your[authentication needs](https://docs.stripe.com/payments/3d-secure) across different types of payment flows, whether you want to use Stripe for both authentication and payment processing, or as part of a multiprocessor setup via[3DS import](https://docs.stripe.com/payments/payment-intents/three-d-secure-import) and[Standalone 3D Secure](https://docs.stripe.com/payments/payment-intents/standalone-three-d-secure) .
- Stripe has maintained API success rates above 99.999% over the last year, meaning just 26 seconds of downtime per month—including[over Black Friday and Cyber Monday](https://stripe.com/newsroom/news/bfcm2023) .
- We launched[Radar Assistant](https://docs.stripe.com/radar/rules#radar-assistant) , which translates your natural language prompts into fraud prevention rules.
- With[fraud insights](https://docs.stripe.com/radar/analytics/fraud-insights) , you can now review fraud trends specific to your business and tailor your fraud prevention strategy.


### **Organizations**


- Scaled businesses with multiple entities and product lines can now manage their business across multiple Stripe accounts with[Organizations](https://docs.stripe.com/payments/account/orgs) .
- With Organizations, you can use Stripe Sigma to[query your Stripe data across all your Sigma-enabled accounts](https://docs.stripe.com/stripe-data/sigma-organizations) .


## **Build a thriving platform business with Embedded Payments and Finance**


[Stripe Connect](https://stripe.com/connect) enables you to embed payments and other financial services into your own products and platforms. We shared new ways for you to customize Connect to your needs, accelerate your payments attach rate and monetization, keep your platform compliant and protected, and more easily offer financial services beyond payments. Updates in this area include:


### **Connect**


- We shipped our biggest-ever update to Connect, making it more modular so that you can now choose[which capabilities you want to own and which to offload to Stripe](https://docs.stripe.com/connect/design-an-integration) . And you can change your choices as your business evolves.
- We launched 17 new[embedded components](https://docs.stripe.com/connect/supported-embedded-components) , including 10 for payments, with UIs and workflows that allow you to quickly build feature-complete payments products and localize your experience across 118 countries.
- With our new[pricing tools](https://docs.stripe.com/connect/platform-pricing-tools) , you can now set and modify your pricing based on payment method, country, and other factors, without any code.
- You can now understand how pricing changes affect your margins, with our new[margin reports](https://docs.stripe.com/connect/margin-reports) .
- New[merchant risk tooling](https://docs.stripe.com/connect/risk-management/risk-tooling) alerts you to potentially fraudulent businesses on your platform, offers you account-level analytics to assess fraud risk, and enables you to block accounts that look suspicious based on signals derived from Stripe’s vast network.
- You can now see the[support cases](https://docs.stripe.com/connect/dashboard/managing-individual-accounts) your connected accounts have with Stripe—and message Stripe directly to help resolve them.


### **Embedded finance**


- We released new[embedded components](https://docs.stripe.com/connect/supported-embedded-components) for Treasury, Issuing, and Capital.
- [Capital for platforms](https://stripe.com/capital/platforms) now supports a streamlined onboarding flow, so you can complete onboarding directly from the Dashboard and provide loans to your users in a matter of days.
- We announced new[Issuing fraud tools](https://docs.stripe.com/issuing/controls/advanced-fraud-tools) , based on Stripe’s proprietary data, that surface transaction risk signals while enabling valid spend on Issuing cards.
- We released our[sample app](https://docs.stripe.com/baas/start-integration/sample-app) , which enables you to view the connected account experience of Issuing- and Treasury-onboarded customers and create cards, test authorizations, and make outbound payments from a financial account. Without writing any code, you can copy the app to use with your platform and follow the API logs generated by your actions.


## **Grow revenue faster with Revenue and Finance Automation**


Revenue models are becoming more complicated because of the internet, and most off-the-shelf solutions are not built to keep pace. We’re building a revenue platform that enables your growth ambitions, and today we shared new ways to orchestrate and grow your revenue with Stripe.


### **Billing**


- We launched support for[usage-based billing](https://docs.stripe.com/billing/subscriptions/usage-based) with our new Meters API, so you can now ingest, aggregate, and view usage events on Stripe in real time.
- We launched[automations](https://docs.stripe.com/billing/revenue-recovery/automations) so that you can chain together custom workflows in Stripe—triggers, conditions, and actions—to help with business processes, starting with revenue recovery.
- With the[Entitlements API](https://docs.stripe.com/billing/entitlements) , you can now control access to various features within your product based on customers’ billing plans.
- The new[migration toolkit](https://docs.stripe.com/billing/subscriptions/migrate-subscriptions) lets you quickly and accurately migrate tens of millions of subscriptions to Billing, without requiring your customers to reenter their payment details.
- [Advanced discounting logic](https://docs.stripe.com/billing/subscriptions/coupons) lets you offer custom deal structures to your customers.
- The new[Invoice Preview API](https://docs.stripe.com/invoicing/preview) makes it easy to accurately preview invoices at the line-item level.


### **Tax**


- We now support tax collection in[57 countries](https://docs.stripe.com/tax/supported-countries) .
- We launched[tax overrides](https://docs.stripe.com/tax/tax-overrides) , which let you customize[Stripe Tax](https://stripe.com/tax) to fit your business needs by creating rules that apply to a product tax code in any supported location.
- You can now[import transaction data](https://docs.stripe.com/tax/reports#tax-data-imports) from platforms such as Amazon, Shopify, and eBay directly into Tax, so you can access a consolidated view of all your sales and tax obligations across platforms.


### **Reporting and data**


- You can now import your transactions from[Google Play](https://docs.stripe.com/revenue-recognition/data-import/google-play)1 and the[Apple App Store](https://docs.stripe.com/revenue-recognition/data-import/apple-app-store) ®2 to consolidate revenue recognition on Stripe.


## **Use Stripe within an open ecosystem of third-party providers**


You’ve been asking us to make Stripe more modular so that our services fit better with your existing technology stack. Over the last few years, we’ve quietly made many Stripe services work with other payment service providers (PSPs). Today we shared how we are extending Stripe’s modularity. Some of the highlights include:


- We launched a new[Vault and Forward API](https://docs.stripe.com/payments/forwarding) , so you can use even more of Stripe with other payment processors. As of today, you can use the[Payment Element](https://docs.stripe.com/payments/payment-element) with other PSPs, and later this year,[Billing](https://docs.stripe.com/billing/subscriptions/third-party-payment-processing) and[Radar](https://stripe.com/radar) will also work seamlessly with third-party processors.
- Thanks to our partnership with Korean PSP NICEPAY, you can give your customers a fully localized[checkout experience in South Korea](https://docs.stripe.com/payments/kr-market) , including 21 local card brands and 3 local wallets.
- We more than doubled the number of apps in the[Stripe App Marketplace](https://marketplace.stripe.com/) , from 70 to 150, including new apps for[Adobe Commerce](https://marketplace.stripe.com/apps/adobe-commerce) ,[Salesforce CPQ](https://docs.stripe.com/connectors/salesforce-cpq/overview) ,[NetSuite](https://docs.stripe.com/connectors/netsuite/overview) , and more.
- We’ve added four new issuer partners, including American Express, to the[Enhanced Issuer Network](https://stripe.com/newsroom/news/enhanced-issuer-network) , which now has coverage for nearly 40% of Stripe’s US payment volume.


## **Building for our users**


We’d love your feedback on everything we announced at Sessions this year. Share your thoughts, questions, and requests[here](https://stripe.co1.qualtrics.com/jfe/form/SV_b7ne4W5n1kbdYJ8) .


You can also read our[changelog](https://stripe.com/blog/changelog) to learn about the full set of more than 200 updates we’ve made to Stripe in the last year.
