---
schema_version: "1.0.0"
document_id: "db12218f3aee235ec059b6248ce8f598a720a443462e43afd3913be7ea08fdf5"
company_key: "yc-stripe"
company: "Stripe"
source_id: "yc-stripe-news-import-ac4ed02840f0"
canonical_url: "https://stripe.com/blog/helping-businesses-optimize-network-costs-with-visa-digital-commerce-authentication-program"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-21T04:05:27.623858+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:27a252c0afffa88033f3b696f8a51d80b1f09f04e376988cc70cb4fd7ba4f467"
---

# Helping businesses optimize network costs with the Visa Digital Commerce Authentication Program (DCAP)

Visa recently launched the[Digital Commerce Authentication Program (DCAP)](https://support.stripe.com/questions/understand-the-visa-digital-commerce-authentication-program-%28dcap%29-on-stripe) , a new global framework designed to reduce fraud and increase authorization rates for card-not-present transactions. The program rewards businesses in the US for sharing richer transaction data with issuers during authentication, such as device ID, billing address, IP address, and customer email. Qualifying transactions receive a net interchange reduction of five basis points.


New network programs create opportunity, but they also introduce complexity. Businesses need to understand which transactions qualify, ensure their integration passes the required data, and determine whether participating will improve their end-to-end transaction economics or have unintended consequences, such as hurting authorization rates.


To participate in DCAP, businesses need to share required cardholder data with issuers via frictionless authentication in their checkout. This might introduce latency and uncertainty around how issuers interpret these newer signals.


We moved quickly to help Stripe businesses take advantage of DCAP and capture interchange savings while protecting authorization rates. Here’s what we did.


### Optimizing DCAP savings without sacrificing conversion


Before rolling out DCAP, we worked with Visa to run readiness testing and identify the right implementation approach. This collaborative testing underscored the need for transaction-level intelligence.


With[Stripe Authorization Boost](https://stripe.com/authorization-boost) , we intelligently select which transactions should go through[Data Only 3DS](https://docs.stripe.com/payments/3d-secure/strong-customer-authentication-exemptions#data-only) , which sends additional risk data from the card network to the issuer for authorization. Rather than applying static rules, Authorization Boost evaluates cost savings, conversion impact, and fraud risk at the individual transaction level to determine when to apply Data Only 3DS. This allows businesses to capture DCAP savings while limiting the impact to the customer experience and optimizing authorization rates.


Since April 18, we’ve helped Stripe businesses capture $18.4 million in annualized network cost savings from DCAP. By helping businesses collect and pass the required data, we saw an 8x increase in the number of DCAP-eligible transactions. We’re continuing to work with Visa to optimize eligibility, so more transactions can benefit from DCAP.


## Automatically benefit from DCAP optimizations


If you use Authorization Boost and are collecting the required data points, you’re already automatically benefiting from DCAP optimizations. For businesses using[standalone 3DS](https://docs.stripe.com/payments/3d-secure/standalone-3d-secure) , you can participate by setting **flow_preference\[type\]** to` data_share` on authentication requests and ensuring required fields are populated.


Learn more about how[Authorization Boost](https://docs.stripe.com/payments/analytics/optimization) can help optimize your payments performance.
