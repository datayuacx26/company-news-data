---
schema_version: "1.0.0"
document_id: "880f643b3a999265e57d7bb12f465b17184149a726843a48a935c4044b7f0958"
company_key: "yc-lumen-payments"
company: "Lumen Payments"
source_id: "yc-lumen-payments-news-import-87c7fb1c30f2"
canonical_url: "https://getlumen.dev/blog/how-to-start-accepting-payments-online"
published_at: "2025-09-24T00:00:00+00:00"
first_seen_at: "2026-07-22T02:53:16.718446+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:fb28dfdac0dd6315bedef1ca219948143d7b43a9972cd4d124842ebfb6b6eeca"
---

# How to start accepting payments online

If you are building a product online, at some point you probably need to accept payments. This can feel confusing if you’ve never done it before. The good news is that you don’t need to build everything from scratch. There are existing services that help you get started. Which option makes sense for you depends a lot on **the country you live in** and **your budget** .


---


## If Stripe is available where you live


[Stripe](https://stripe.com/) is usually the easiest choice. It lets you accept credit cards and a bunch of local payment methods. The documentation is good, and most developers find it easy to set up. The downside is that **Stripe** is **not available everywhere** . If your country is not on their supported list, you’ll need to look for a different solution.


---


## If Stripe is not available


If you can’t use Stripe, there are other choices:


- [Paddle](https://paddle.com/) : Common for software companies outside the US or EU. Paddle acts as a Merchant of Record, which means they handle taxes (like VAT) for you. The fees are a bit higher, but you don’t need to worry about tax across countries.
- [DodoPayments](https://dodopayments.com/) : Similar idea, for developers in countries where Stripe does not work. It also acts as a Merchant of Record and lets you get started quickly without setting up a foreign company.


---


## Opening a US Entity (LLC or C‑Corp)


Some developers decide to register a US company (like a Delaware C‑Corp or an LLC) just to gain access to Stripe or other US‑only payment providers. This does work, but you should know what comes with it.


### Costs to expect


- **Formation fees** : $200–400 to set up an LLC, $500–800 for a C‑Corp.
- **Registered agent** : around $100–300 per year.
- **Franchise tax and annual reports** : roughly $400–800 per year for Delaware.
- **Accounting and tax filing** : at least $1,000–2,000 per year.
- **Bank account setup** : can be tricky without visiting the US.


So you’re looking at around **$2,000–3,000 per year** just to maintain the company.


---


## What is a Merchant of Record?


A **Merchant of Record (MoR)** is the company that officially sells your product to the customer. They collect the payment, handle taxes, deal with chargebacks, and then send the rest to you.


Popular MoRs include:


- **[Paddle](https://paddle.com/)**
- **[DodoPayments](https://dodopayments.com/)**
- **[Stripe Managed Payments](https://docs.stripe.com/payments/managed-payments)** (currently in Beta)


Using a MoR makes compliance and taxes much easier, but you pay higher fees and give up some direct control.


---


## Comparison table


Here’s a quick side‑by‑side view to help you choose:


Option Availability Who handles taxes Complexity Yearly costs Control


**[Stripe](https://stripe.com/)** (direct) Countries supported by Stripe You Easy if supported None (normal fees) Full


**[Paddle](https://paddle.com/)** (MoR) Global (many countries) Paddle Simple % of each payment Limited


**[DodoPayments](https://dodopayments.com/)** (MoR) Global (where supported) DodoPayments Simple % of each payment Limited


**[Stripe Managed Payments](https://docs.stripe.com/payments/managed-payments)** (Beta, MoR) Expanding (still Beta) Stripe Simple % of each payment Limited


**US Entity + Stripe** Worldwide (via US) You Complex $2,000–3,000 per year Full


---


## A note if you’re building with AI


If you’re building an AI app, you often need extra features like **usage‑based billing, adding credits for AI tokens, and free trials** . Normally, this takes weeks of work to implement on top of payment providers.


With **[Lumen Payments](https://getlumen.dev/)** , you can plug into Stripe, Paddle, DodoPayments, or Stripe Managed Payments and get all of this running in **under 1 hour** . That means you can skip weeks of billing logic and stay focused on your product.


---


## Putting it all together


- If Stripe is in your country → use Stripe.
- If not → try a Merchant of Record like Paddle or DodoPayments.
- If you want full control worldwide → set up a US entity, but remember the $2,000–3,000 yearly cost.
- Keep an eye on Stripe Managed Payments (Beta).
- And if you’re working on an AI app, check out[Lumen Payments](https://getlumen.dev/) to save time on billing and credits.


#### Ramon Garate


Founder of Lumen
