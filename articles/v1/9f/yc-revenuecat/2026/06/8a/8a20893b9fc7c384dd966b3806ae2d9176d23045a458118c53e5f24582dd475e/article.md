---
schema_version: "1.0.0"
document_id: "8a20893b9fc7c384dd966b3806ae2d9176d23045a458118c53e5f24582dd475e"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/engineering/launch-party-june-26-the-new-features-you-should-know"
published_at: "2026-06-12T00:46:12+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:c1116a6043cc5e73357c19eb6656c0e2ba71f33d9df52f8e94624bf61b648288"
---

# Launch Party June ’26: The New Features You Should Know

On June 5, 2026 we hosted special edition of our[bi-weekly office hours](https://app.livestorm.co/revenuecat/live-revenuecat-demo?type=detailed) featuring live demos and Q&A around our recent features launches. It was hosted by the Product Managers who built and shipped the features, giving you the chance to discuss your questions with the team live.


If you couldn’t join live, here’s a summary of what we covered and clips of the demos.


## Paywalls


### AI Editor


The AI Editor is a conversational AI agent built directly into the RevenueCat Paywall Editor that lets you create, edit, and refine paywalls using natural language prompts.


This feature is ideal for rapidly generating a production-ready paywall from scratch, adjusting visual design like colors and typography, or updating existing templates without manual tweaking. You chat with the AI (for example, “Make a paywall for a book tracking app targeting the BookTok community”) and it automatically adjusts the copy, layout, and imagery. It can also accept a` design.md` file to match your brand guidelines or a screenshot for inspiration.


To set it up, go to Paywalls in the RevenueCat dashboard, click **Create paywall** , and select **Generate with AI** , or open an existing paywall and use the **AI Editor** tab in the left sidebar.[Check out the docs here.](https://www.revenuecat.com/blog/company/paywalls-ai-editor/?_gl=1*awxc43*_up*MQ..*_ga*NjI4MTgwNDM0LjE3ODEyMjUyMTM.*_ga_0MLNVKXFGB*czE3ODEyMjUyMTMkbzEkZzAkdDE3ODEyMjUyMTMkajYwJGwwJGg1MTcxNzAwNjU.)


### Paywall Rules


[Paywall Rules](https://www.revenuecat.com/docs/tools/paywalls/creating-paywalls/rules) allow you to customize the visibility of paywall components based on both preset and Custom Variable based rules, enabling you to customize a single paywall to support multiple scenarios.


One example is using Paywall Rules to show a trial timeline only when a trial is available, or to display a different package (like family sharing vs. individual) based on a custom variable.


Rules are evaluated at runtime. You define conditions such as` offer.intro` ,` package.identifier` , or custom variables, and set components to be visible or hidden when those conditions are met.


To set this up, open the **Paywall logic** tab in the Paywall Editor, create a new rule based on your desired condition, and then set the visibility of your paywall components for that rule. Check out[this blog post](https://www.revenuecat.com/blog/engineering/announcing-paywall-rules-show-or-hide-paywall-components/) for more info.


## Funnels


### Funnels Builder


Funnels allow you to build customizable, hosted web onboarding experiences that can be designed remotely from the RevenueCat dashboard and shipped by anyone on your team.


Use cases for funnels include web-to-app acquisition funnels for ad campaigns or influencers, surveying users before checkout, and capturing web conversions with lower friction. You build multi-step web flows using the Funnels editor, which is similar to the Paywalls editor. You can add screens, branching logic, survey questions, and a checkout step. It currently supports Stripe and Paddle for payments.


To set it up, configure a payment provider in RevenueCat, create a Funnel in the dashboard, design your steps and branching logic, and deploy the generated URL.[Check out the docs here.](https://www.revenuecat.com/docs/tools/funnels/creating-funnels)


### A/B Testing


Testing within Funnels enables you to run experiments directly within your web Funnels to optimize conversion.


This feature is used for testing different price points, paywall designs, or onboarding survey flows to see which yields the highest conversion rate on the web. In the Funnel Editor, you can add an “Experiment” branch that splits traffic between different paths (e.g., a high-price paywall vs. a low-price paywall) and track the results in the funnel analytics.


To set it up, add an Experiment node in your funnel flow and connect it to your variant screens or paywalls.[Check out the docs here.](https://www.revenuecat.com/docs/tools/funnels/experimenting-with-funnels)


## Web


### One-Tap Purchases via Express Checkout


One-Tap Purchases via Express Checkout adds Apple Pay and Google Pay buttons directly to your web paywalls and funnels for frictionless purchasing.


This maximizes web conversion rates by allowing users to skip the traditional credit card form and check out with a single tap. The Express Checkout component surfaces Apple Pay or Google Pay (if supported by the user’s device and browser) and processes the payment through Stripe.


To set it up, add the “Express checkout buttons” component to your paywall in the Paywall Editor or Funnel Editor.[See the docs here.](https://www.revenuecat.com/docs/tools/paywalls/creating-paywalls/components?_gl=1*1q9msjx*_up*MQ..*_ga*NjI4MTgwNDM0LjE3ODEyMjUyMTM.*_ga_0MLNVKXFGB*czE3ODEyMjUyMTMkbzEkZzAkdDE3ODEyMjUyMTMkajYwJGwwJGg1MTcxNzAwNjU.#express-checkout)


### Stripe Billing & Stripe Managed Payments


RevenueCat supports both Stripe Billing and Stripe Managed Payments, a Merchant of Record solution where Stripe handles tax compliance, collection, and remittance.


A merchant of record solution is ideal for selling subscriptions on the web while offloading the complexity of global sales tax, VAT, and GST to Stripe. RevenueCat creates a Stripe Checkout session, and for eligible products, Stripe acts as the Merchant of Record, processes the payment, handles taxes, while RevenueCat tracks the transaction and unlocks entitlements.


To set it up, connect your Stripe account via the RevenueCat Stripe App, create a Stripe Web Config in RevenueCat, and toggle on “Use Managed Payments when available”.[See the docs.](https://www.revenuecat.com/docs/web/integrations/stripe?_gl=1*o7m74e*_up*MQ..*_ga*NjI4MTgwNDM0LjE3ODEyMjUyMTM.*_ga_0MLNVKXFGB*czE3ODEyMjUyMTMkbzEkZzAkdDE3ODEyMjUyMTMkajYwJGwwJGg1MTcxNzAwNjU.)


### Discounts and Discount Codes


This is a flexible system to apply percentage-based discounts to products in RevenueCat Web Billing using shareable, customer-facing discount codes.


Use cases include running promotional campaigns like Black Friday, attributing sales to influencers via unique codes, or offering win-back discounts. You create a discount (e.g., 50% off for 3 months) and generate codes (e.g., BLACKFRIDAY50). Customers enter the code at checkout, or it can be auto-applied via a URL parameter on a Web Purchase Link.


To set it up, navigate to **Product catalog > Web discounts** in the dashboard, create a discount, define its rules and codes, and enable discount codes on your Web Purchase Links.[Check out the docs here.](https://www.revenuecat.com/docs/web/web-billing/discounts?_gl=1*awxc43*_up*MQ..*_ga*NjI4MTgwNDM0LjE3ODEyMjUyMTM.*_ga_0MLNVKXFGB*czE3ODEyMjUyMTMkbzEkZzAkdDE3ODEyMjUyMTMkajYwJGwwJGg1MTcxNzAwNjU.)


## Experiments


### pLTV Winner, Credible Intervals, Astra Analysis, Winner Rollout


This suite of features enhances Experiments with advanced statistical tools: predicted LTV (pLTV) winner predictions, credible intervals for metrics, AI-driven analysis via Rico (Astra), and one-click winner rollouts.


These tools help you make confident decisions on A/B tests by understanding not just initial conversion, but long-term value, and quickly deploying the winning variant. RevenueCat calculates the Chance to Win and 95% credible intervals for conversion metrics. Once sufficient data is gathered, it predicts the pLTV winner. You can ask Rico to analyze the nuances of the test, and use the “Rollout” option to set the winner as the default offering or create a targeting rule.


These features automatically populate in the Results tab of your running Experiments.


## AI Tools


### Rico


Rico is RevenueCat’s AI-powered app growth advisor built into the dashboard and available in Slack.


It is designed for diagnosing revenue drops, analyzing experiment results, benchmarking against industry data, or answering SDK integration questions using plain language. Rico has access to your app’s data, charts, experiments, RevenueCat documentation, and industry benchmarks. You ask it a question, and it reasons through the data to provide an answer.


To set it up, use the chat interface in the dashboard overview. To use it in Slack, go to Account Settings, select the Rico tab, and connect it to your workspace.


[Read more on Rico in this blog post.](https://www.revenuecat.com/blog/company/rico-app-growth-advisor/?_gl=1*o7m74e*_up*MQ..*_ga*NjI4MTgwNDM0LjE3ODEyMjUyMTM.*_ga_0MLNVKXFGB*czE3ODEyMjUyMTMkbzEkZzAkdDE3ODEyMjUyMTMkajYwJGwwJGg1MTcxNzAwNjU.)


### AI Toolkit


RevenueCat offers a comprehensive AI toolkit to help AI agents set up RevenueCat, integrate it into apps, and access your revenue data — all directly from your AI coding assistant.


[Explore the docs.](http://revenuecat.com/docs/tools/overview)


## Charts & Dashboard


### Benchmarks


Benchmarks allow you to compare your app’s key metrics like conversion, churn, and realized LTV against other apps in the same store and category.


This helps identify growth opportunities, set meaningful goals, and see how your app stacks up against peers in metrics like trial conversion or refund rates. RevenueCat calculates benchmark percentiles using anonymized data from apps in the same category over the last 12 months. Your app’s performance is shown as a percentile (e.g., 90th percentile).


Benchmarks are automatically available in the dashboard under the Benchmarks tab for eligible apps.


[See the docs.](https://www.revenuecat.com/docs/dashboard-and-metrics/benchmarks?_gl=1*nnzjuy*_up*MQ..*_ga*NjI4MTgwNDM0LjE3ODEyMjUyMTM.*_ga_0MLNVKXFGB*czE3ODEyMjUyMTMkbzEkZzAkdDE3ODEyMjUyMTMkajYwJGwwJGg1MTcxNzAwNjU.)


### In-App Ad Revenue in Charts


In-App Ad Revenue in Charts tracks ad impressions, clicks, and revenue alongside your subscription data for a unified view of your app’s hybrid monetization.


This is essential for calculating accurate Realized LTV that includes both ads and subscriptions, and understanding how ad revenue varies by segment. By hooking into your ad mediation platform’s (like AdMob or AppLovin) impression-level revenue data (ILRD) callbacks, RevenueCat tracks ad events and displays them in dedicated Ad Charts and the main Revenue mix chart.


To set it up, opt-in to the beta in the Ads page of the dashboard, and integrate the AdMob Adapter SDK or manually call the` AdTracker` methods from your ad SDK callbacks.[See the docs.](https://www.revenuecat.com/docs/dashboard-and-metrics/charts/ads)


## Store Updates


### Apple 12-Month Commitments


This feature supports Apple’s new subscription option that bills customers monthly but requires a 12-month commitment.


It is highly effective for lowering the upfront price barrier for annual subscriptions in price-sensitive regions while maintaining long-term retention. Apple models this as a billing plan under an annual product. RevenueCat exposes the monthly commitment plan as a separate product ID with a` :monthly` suffix (e.g.,` com.myapp.product:monthly` ).


To set it up, configure the billing plan in App Store Connect. In RevenueCat, create a second product and check the “12 months commitment” option. Ensure you are using StoreKit 2 and iOS 26.4+.


[More on the blog.](https://www.revenuecat.com/blog/engineering/monthly-subscription-12-month-commitment/)


## Join our next office hours


Wish you were there? Don’t worry, you can find our team live every other[Friday for Office Hours.](https://app.livestorm.co/revenuecat/live-revenuecat-demo?type=detailed)


We gather members of the RevenueCat team, our community, and anyone curious about RC or consumer-software monetisation as a whole for a fast-moving, fully interactive Q&A and platform demo session.


We plan to take over one of these session with a launch party at least once a quarter, so stay tuned for the next one
