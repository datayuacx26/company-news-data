---
schema_version: "1.0.0"
document_id: "0ad10fe03cb4649df1271f7e9c507e4ea899e50300b0660a7c08136944860e30"
company_key: "yc-hitpay"
company: "HitPay"
source_id: "yc-hitpay-news-import-8b4a406bcaf9"
canonical_url: "https://hitpayapp.com/blog/recurring-billing-ph"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-14T15:34:52.932290+00:00"
fetched_at: "2026-08-14T15:34:54.779+00:00"
content_hash: "sha256:a1b7fbc7b149ff1917a5def9f91cdade5c10e084c76c38b96d17dbc0a5d0bbdd"
---

# Recurring Billing for Philippine Businesses: A Practical Guide (2026)

**Quick Answer:** Philippine businesses can run automated recurring billing through HitPay using cards (Visa, Mastercard) and ShopeePay — with no monthly platform fees and next business day PHP payouts. GCash and Maya are not available for automated recurring charges but work well for initial member onboarding. HitPay is a BSP-registered Operator of Payment System (OPSCOR-2023-0006) and MAS-licensed (PS20200643), with no-code plan setup and automated retry logic.


The Philippines has one of the highest GCash and Maya penetration rates in Southeast Asia — yet neither wallet supports automated recurring billing mandates. For a gym in BGC or an online course platform in Makati, this creates a practical challenge: your customers prefer GCash, but you cannot use it to charge them automatically each month.


Understanding this distinction before setting up your subscription billing system saves significant operational pain later. This guide explains how Philippine subscription and membership businesses should structure their payment setup, which methods power automated recurring, and how to use GCash and Maya strategically without asking them to do something they cannot.


## Which payment methods support recurring billing in the Philippines?


The following methods support automated recurring billing through HitPay in the Philippines:


-


**Cards — Visa and Mastercard:** The primary vehicle for automated recurring billing in the Philippines. Card-on-file allows HitPay to charge the saved card automatically each billing cycle without requiring customer action. Card rates vary by card type — see[hitpayapp.com/pricing](https://hitpayapp.com/pricing) for current PHP rates.


-


**ShopeePay:** ShopeePay recurring mandates are supported in the Philippines at a 2.5% rate per transaction. Customers authorise once through the Shopee app; subsequent charges are automated.


**GCash is not available for automated recurring billing.** GCash is the Philippines’ most widely used e-wallet, but it operates as a pay-per-transaction method — each GCash payment requires the customer to approve it individually through the app. There is no recurring mandate mechanism that allows a business to pull funds automatically each billing cycle. GCash is excellent for one-off payments, initial fees, and ad-hoc collections — but it cannot power an automated subscription.


**Maya is not available for automated recurring billing** for the same reason. Maya processes individual customer-initiated transactions; it does not support merchant-initiated recurring mandates.


**QR Ph is not available for automated recurring billing.** Like GCash and Maya, QR Ph requires a customer to scan and confirm each payment. It is a one-off payment method.


## What is the right onboarding strategy for Philippine subscription businesses?


Because GCash and Maya are the dominant payment methods for Filipino consumers — but cannot power automated recurring — the most practical approach is a two-step strategy:


1.


**Accept GCash or Maya for the first payment.** This lowers the sign-up barrier significantly. Filipino consumers are comfortable with GCash and Maya; asking them to enter card details immediately creates friction, especially for new merchants they don’t yet trust.


2.


**Collect a card or ShopeePay mandate for ongoing automated charges.** After the first payment, prompt the member to set up their recurring method. Frame it as convenience — no need to pay manually each month. For members who use Shopee, ShopeePay mandate set-up takes under a minute.


This two-step approach is particularly effective in the Philippines because card penetration, while growing, is still lower than in Singapore or Malaysia. For subscribers outside Metro Manila — in Cebu, Davao, or Iloilo — ShopeePay may reach customers who do not hold a credit card.


## Which Philippine businesses use recurring billing?


-


**Fitness studios and gyms:** Monthly memberships and class packages. A BGC boutique gym or a Makati fitness studio billing monthly member fees can automate collection entirely once card mandates are in place — removing the monthly payment follow-up from front-desk operations.


-


**Online courses and e-learning:** Monthly access to course libraries or cohort-based programmes. Card-on-file is the standard method; ShopeePay reaches Shopee-active learners who may not have cards.


-


**Content creators and community memberships:** Creators in the Philippines running paid membership communities (Patreon-style models) need a local recurring billing option that works without relying on GCash each month. Cards via HitPay fill this gap.


-


**Professional services:** Consultants, accountants, and virtual assistants billing monthly retainers. Cards are the most reliable method for B2B recurring arrangements in the Philippines.


-


**Subscription boxes:** Monthly curated product deliveries — beauty, food, lifestyle. Cards cover the banked customer segment; ShopeePay reaches Shopee shoppers.


-


**SaaS and digital tools:** Monthly or annual software subscriptions. Cards dominate for local and international subscribers alike.


## How does settlement and payout work for Philippine recurring billing?


HitPay settles domestic Philippines (PHP) transactions on the **next business day** . This applies to all recurring billing payment methods — cards and ShopeePay alike. There is no settlement delay for recurring transactions versus one-off charges.


Cross-border transactions settle at T+2. If you have international subscribers paying via cross-border methods, factor the two-day settlement into your cash flow planning.


HitPay operates in the Philippines under registration with the[Bangko Sentral ng Pilipinas (BSP)](https://www.bsp.gov.ph/) as a Registered Operator of Payment System (OPSCOR-2023-0006), and is also registered with the Anti-Money Laundering Council (AMLC). HitPay’s Singapore entity holds a Major Payment Institution licence from the Monetary Authority of Singapore (MAS, PS20200643).


## How do you set up recurring billing with HitPay in the Philippines?


1.


Sign up at hitpayapp.com — free, no setup fee. Individual and registered business accounts are both accepted.


2.


Complete identity and business verification — approval typically takes 1–3 business days.


3.


In the dashboard, navigate to **Recurring Billing → Plans** .


4.


Click **Add New Plan** and enter the plan name, amount (PHP), billing interval (weekly, monthly, quarterly, annually), and charge cap if applicable.


5.


Save the plan and choose whether to make it public (self-signup via shared link) or enrol customers manually.


6.


Share the plan link via Facebook Messenger, Viber, email, or embed it in your website.


Once a member completes enrolment using their card or ShopeePay, HitPay manages all subsequent charges automatically — including pre-renewal reminders 7 days before each charge and failed payment retries for up to 7 consecutive days. No developer involvement is needed for standard setup.


## How should Philippine subscription businesses handle failed payments?


Failed recurring payments are the primary driver of involuntary churn. Philippine-specific considerations:


-


**Card limits and declines:** Philippine prepaid and debit cards may have daily transaction limits that cause recurring charge failures even when the account holds sufficient funds. If a specific card type shows a pattern of failures, prompt the member to use a credit card or a different payment method instead.


-


**Card expiry:** Send proactive card-update prompts 60 days before recorded expiry dates. A Messenger or Viber message with a link to update payment details recovers most at-risk members before the failure occurs.


-


**ShopeePay balance:** ShopeePay charges can fail if the wallet balance is insufficient. Pre-charge notifications — sent a few days before the billing date — give members time to top up.


-


**Retry logic:** HitPay retries failed charges for up to 7 consecutive days. Pair this with a dunning message sequence (day 1, day 4, day 7 via email or Messenger) to prompt manual payment updates before access is suspended.


## Frequently Asked Questions


Can I use GCash for recurring billing in the Philippines?


No. GCash requires the customer to approve each payment individually — it has no recurring mandate mechanism. Use GCash for the initial onboarding payment to reduce sign-up friction, then collect a card or ShopeePay mandate for automated ongoing billing.


Can I use Maya for recurring billing?


No. Like GCash, Maya processes individual customer-initiated payments and does not support merchant-initiated recurring mandates. Maya is best used for one-off collections, initial fees, or onboarding payments.


Does HitPay support ShopeePay recurring billing in the Philippines?


Yes. ShopeePay recurring mandates are supported in the Philippines through HitPay at 2.5% per transaction. Customers authorise recurring charges through the Shopee app at enrolment; all subsequent charges are automated with no further customer action required each cycle.


How fast does HitPay pay out PHP for Philippine recurring billing?


HitPay settles domestic PHP transactions on the next business day. A recurring charge processed on Monday arrives in the business bank account on Tuesday. Cross-border transactions settle at T+2.


What is the best recurring billing setup for a gym or studio in the Philippines?


Accept the first month's membership fee via GCash or Maya — this removes the sign-up barrier for members who do not want to enter card details immediately. During onboarding, collect a card or ShopeePay mandate for automated monthly charges from the second month onward. HitPay's recurring plan links make both steps possible without any developer work.


Is HitPay licensed to operate recurring billing in the Philippines?


Yes. HitPay Payment Solutions Inc is registered with the Bangko Sentral ng Pilipinas (BSP) as a Registered Operator of Payment System (OPSCOR-2023-0006) and with the Anti-Money Laundering Council (AMLC). HitPay's Singapore entity holds a Major Payment Institution licence from MAS (PS20200643).


Is there a monthly fee to use HitPay for recurring billing in the Philippines?


No. HitPay charges no monthly platform fee and no plan creation fee. Costs are per-transaction: card rates and ShopeePay at 2.5%. Full current rates for the Philippines are at hitpayapp.com/pricing.


HitPay vs Xendit for recurring billing in the Philippines — which is better?


HitPay suits Philippine SMBs that want no monthly fees, ShopeePay recurring support, next business day PHP payouts, and no-code plan setup. Xendit is a strong option for Philippine businesses that need broader local payment method coverage for one-off transactions or a more developer-oriented integration. For straightforward subscription and membership billing without a technical team, HitPay's no-code dashboard is the lower-effort path.


Recurring billing is one of the most effective ways to stabilise revenue, reduce administrative overhead, and deliver a seamless experience to your subscribers. For[HitPay partners](https://hitpayapp.com/what-is-hitpay) across the Philippines, the tools to get started are already built in — no complex setup required.


[Set up recurring billing on HitPay Philippines today — no monthly fee, no complex setup →](https://hitpayapp.com/recurring-billing)
