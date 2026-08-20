---
schema_version: "1.0.0"
document_id: "d92680d3865a71fe1ebb3e91275b6341dcfde35b5f5dc95807f1f0b110137ea6"
company_key: "yc-hitpay"
company: "HitPay"
source_id: "yc-hitpay-news-import-8b4a406bcaf9"
canonical_url: "https://hitpayapp.com/blog/recurring-billing-my"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-14T15:34:52.932290+00:00"
fetched_at: "2026-08-14T15:34:54.779+00:00"
content_hash: "sha256:e1148f10ffa3917b71c02d55fa82ebaa28b752664a400cc8c4e9d92cb49348ec"
---

# Recurring Billing for Malaysian Businesses: A Practical Guide (2026)

**Quick Answer:** Malaysian businesses can run automated recurring billing through HitPay using cards (Visa, Mastercard, Apple Pay, Google Pay), Touch ‘n Go, GrabPay, and ShopeePay — with no monthly platform fees and T+2 calendar day MYR payouts. FPX and DuitNow QR cannot be used for automated recurring charges. HitPay is MAS-licensed (PS20200643) and supports no-code plan setup with automated retry logic and pre-renewal reminders.


Recurring billing in Malaysia is more nuanced than it first appears. FPX and DuitNow QR are the dominant payment methods across the country — but neither supports automated mandate-based recurring charges. Setting up a subscription billing system without understanding this leads to broken billing cycles after the first payment.


This guide explains how Malaysian subscription and membership businesses should structure their payment setup, which methods work for automated recurring, and how to use FPX and DuitNow QR strategically as onboarding tools without trying to make them do something they cannot.


## Which payment methods support recurring billing in Malaysia?


The following methods are supported for automated recurring billing through HitPay in Malaysia:


-


**Cards — Visa, Mastercard (including Apple Pay and Google Pay):** The most reliable method for automated recurring charges in Malaysia. Card-on-file allows HitPay to charge the saved card automatically each cycle. Domestic card rate: 1.2% + RM1; international cards: 3% + RM1.


-


**Touch ‘n Go eWallet:** Recurring mandates are supported. Customers authorise once through the TnG app; subsequent charges are automated. Rate: 1.9%.


-


**GrabPay (+ PayLater by Grab):** GrabPay recurring mandates are supported. Rate: 2%.


-


**ShopeePay (+ ShopeePay Later):** ShopeePay recurring is supported. Rate: 2.2%.


**FPX is not available for automated recurring billing.** FPX is a session-based bank transfer method — each transaction requires the customer to log into their bank and approve the payment individually. There is no mechanism for the merchant to automatically pull funds from an FPX-authorised account on a recurring schedule. FPX is excellent for one-off payments and initial authorisations.


**DuitNow QR is not available for automated recurring billing.** DuitNow QR requires the customer to scan and confirm each payment manually. Like FPX, it has no mandate capability. It is best used for initial membership onboarding or one-off collection.


## What is the right onboarding strategy for Malaysian subscription businesses?


Because FPX and DuitNow QR cannot power automated recurring charges — but are among the most familiar payment methods to Malaysian consumers — the most practical strategy is a two-step approach:


1.


**Onboard using a familiar local method.** Offer FPX or DuitNow QR for the first payment. This lowers friction at sign-up, especially for customers who are cautious about saving card details with a new merchant.


2.


**Collect a card or wallet mandate for ongoing charges.** During or immediately after the first payment, prompt the customer to set up a recurring mandate via card, Touch ‘n Go, GrabPay, or ShopeePay. Make it clear this is for their convenience — no manual payment required each month.


Communicating this clearly at sign-up prevents confusion. A gym in Bangsar or a tuition centre in Petaling Jaya can accept the first month’s payment via DuitNow QR, then collect a Touch ‘n Go or card mandate for all subsequent months — giving the customer a familiar first experience and the merchant a reliable recurring billing setup.


## Which Malaysian businesses use recurring billing?


Recurring billing suits any Malaysian business collecting predictable amounts on a fixed schedule. Common use cases include:


-


**Fitness studios and gyms:** Monthly memberships and class packages. A Bangsar yoga studio or a PJ gym billing monthly member fees benefits directly from automated charging — removing the need for front-desk collection or WhatsApp payment reminders each month.


-


**Tuition centres and enrichment:** Term-based or monthly billing. Parents across the Klang Valley are familiar with recurring payment arrangements for their children’s enrichment; a clean mandate-based setup reduces arrears.


-


**Co-working spaces:** Monthly hot-desk or private office memberships. Touch ‘n Go mandates are particularly well-received among younger co-working members who use TnG for most everyday transactions.


-


**Subscription boxes:** Monthly curated product boxes, beauty subscriptions, and specialty food deliveries. Cards are the standard method here, particularly for customers with international cards.


-


**Professional services:** Lawyers, accountants, and consultants billing monthly retainers. Card-on-file is standard; GrabPay is increasingly accepted among Malaysian SME clients.


-


**SaaS and digital products:** Monthly or annual software subscriptions. Cards dominate; international card rates apply for subscribers outside Malaysia.


## How does settlement and payout work for Malaysian recurring billing?


HitPay settles domestic Malaysia (MYR) transactions within **T+2 calendar days** — that is, two calendar days after the transaction date, not two business days. If a recurring charge processes on a Friday, the funds arrive in your bank account on Sunday.


Cross-border transactions settle at T+2 as well. There is no T+3 tier — cross-border settlement through HitPay operates at the same T+2 timeframe as domestic cross-border flows.


Understanding this timing matters for cash flow planning. A business billing 200 members on the first of each month should expect MYR funds to arrive by the 3rd, not the 1st — and should plan supplier payments and operational costs accordingly.


## How do you set up recurring billing with HitPay in Malaysia?


1.


Sign up at hitpayapp.com — free, no credit card required. Sole traders and registered Malaysian businesses (SSM-registered) are both eligible.


2.


Complete identity and business verification — approval takes 1–3 business days.


3.


In the dashboard, navigate to **Recurring Billing → Plans** .


4.


Click **Add New Plan** and set the plan name, amount (MYR), billing interval (weekly, monthly, quarterly, annually), and charge cap if applicable.


5.


Save the plan and choose whether to make it public (self-signup via shared link) or enrol customers manually.


6.


Share the plan link via WhatsApp, email, or embed it in your website’s pricing or membership page.


Once a customer completes enrolment using their card, Touch ‘n Go, GrabPay, or ShopeePay, HitPay manages all subsequent charges automatically — including pre-renewal reminders 7 days before each charge and failed payment retries for up to 7 consecutive days.


## How should Malaysian subscription businesses handle failed payments?


Failed recurring payments are the primary cause of involuntary churn. In Malaysia, the most common failure scenarios are:


-


**Card expiry:** Cards are reissued periodically. Send proactive card-update prompts 60 days before recorded expiry dates. A prompt WhatsApp message with a link to update payment details recovers most at-risk members.


-


**Touch ‘n Go balance:** Unlike cards, TnG eWallet charges can fail if the wallet balance is insufficient at charge time. Consider sending pre-charge balance reminders for TnG recurring members.


-


**Mandate cancellation:** Members can cancel GrabPay or ShopeePay mandates at any time from within the app. Monitor failed mandate notifications promptly and follow up within 24 hours.


HitPay retries failed charges for up to 7 consecutive days. Pair this with a dunning email or WhatsApp sequence (day 1, day 4, day 7) to prompt manual payment updates before access is suspended. Access suspension policy is set by the merchant — HitPay does not impose one.


## Frequently Asked Questions


Can I use FPX for recurring billing in Malaysia?


No. FPX requires the customer to log into their bank and approve each payment individually — it has no mandate mechanism. FPX is excellent for one-off or initial payments but cannot be used to automatically charge customers on a recurring schedule. Use cards, Touch 'n Go, GrabPay, or ShopeePay for automated recurring billing.


Can I use DuitNow QR for recurring billing?


No. DuitNow QR requires the customer to scan and confirm each payment manually. Like FPX, it cannot pull funds automatically on a recurring schedule. Use it for initial member onboarding or one-off payments, then collect a card or wallet mandate for ongoing automated charges.


Does HitPay support Touch 'n Go recurring billing in Malaysia?


Yes. Touch 'n Go eWallet recurring mandates are supported in Malaysia through HitPay at a 1.9% rate per transaction. Customers authorise once through the TnG app; subsequent charges process automatically each billing cycle.


How fast does HitPay pay out MYR for Malaysian recurring billing?


HitPay settles domestic MYR transactions within T+2 calendar days. A charge processed on Monday arrives in the business bank account by Wednesday. Cross-border transactions also settle at T+2.


What is the best recurring billing setup for a gym in Malaysia?


Accept the first month's payment via DuitNow QR or FPX — these are familiar to Malaysian members and lower the sign-up barrier. During onboarding, collect a Touch 'n Go, GrabPay, or card mandate for automated monthly charges going forward. HitPay's recurring plan links make both steps straightforward without any developer work.


Is there a monthly fee to use HitPay for recurring billing in Malaysia?


No. HitPay charges no monthly platform fee and no plan creation fee. Costs are per-transaction: 1.2% + RM1 for domestic cards, 1.9% for Touch 'n Go, 2% for GrabPay, 2.2% for ShopeePay. International card transactions are 3% + RM1. Full current rates are at hitpayapp.com/pricing.


HitPay vs Stripe for recurring billing in Malaysia — which is better?


HitPay is better suited to Malaysian SMBs that need Touch 'n Go, GrabPay, and ShopeePay recurring mandates alongside cards, no monthly fees, and no-code plan setup. Stripe supports FPX for one-off payments in Malaysia and offers strong developer tooling for complex subscription logic, but its local wallet recurring coverage is more limited. For a Bangsar fitness studio or KL tutoring centre, HitPay's payment method breadth and straightforward setup make it the more practical choice.


Recurring billing is one of the most effective ways to stabilise revenue, reduce administrative overhead, and deliver a seamless experience to your subscribers. For[HitPay partners](https://hitpayapp.com/what-is-hitpay) in Malaysia, the tools to get started are already built in — no complex setup required.


[Set up recurring billing on HitPay Malaysia today — no monthly fee, no complex setup →](https://hitpayapp.com/recurring-billing)
