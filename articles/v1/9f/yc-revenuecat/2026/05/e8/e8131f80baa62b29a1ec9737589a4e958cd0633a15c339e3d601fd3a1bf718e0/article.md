---
schema_version: "1.0.0"
document_id: "e8131f80baa62b29a1ec9737589a4e958cd0633a15c339e3d601fd3a1bf718e0"
company_key: "yc-revenuecat"
company: "RevenueCat"
source_id: "yc-revenuecat-rss-41fa37a4cb36"
canonical_url: "https://www.revenuecat.com/blog/company/flexible-discounts-web-billing"
published_at: "2026-05-18T16:44:47+00:00"
first_seen_at: "2026-07-20T23:24:09.080214+00:00"
fetched_at: "2026-07-28T22:13:05.421454+00:00"
content_hash: "sha256:769e30c341ffe3e4d7deb5d6714b74e7714a4237a08d3996cf6622bda9a10b0d"
---

# Flexible Discounts for Web Billing

If you want to run a seasonal sale, partner with an influencer, or win back churned subscribers, you need promo codes. But if you rely solely on the App Store or Google Play, you are fundamentally limited in what you can offer.


Both platforms restrict promo codes to free trials and introductory pricing. Neither supports percentage-off or fixed-amount discounts on your regular subscription price. If you want to offer 30% off to a win-back segment, the web is the only place you can do it.


Flexible Discounts for Web Billing is now available to all RevenueCat Web customers. You can create and manage percentage-based discounts and promo codes directly from your dashboard and apply them via URL, through the SDK, or via a code input field through the checkout UI.


## **Control your pricing on the web**


With Flexible Discounts, you can create percentage-based discounts and manage specific discount codes (like “SUMMER30”) directly from your RevenueCat dashboard. When you create a discount, you control exactly how long it lasts.


You have three duration options:


- One-time: The discount applies only to the user’s first invoice.
- Forever: The discount applies to all future invoices indefinitely.
- Time-window: The discount applies to all invoices generated within a specific calendar period (e.g., all invoices within the next 3 months).


> The time-window duration is calendar-based, not cycle-based. If a user buys a weekly subscription with a 1-month discount, they get the discount on the initial purchase and the renewals within that month. If they buy an annual subscription with a 1-month discount, they only get the discount on the first payment.


## **How it works**


Discounts can be applied in three ways:


- **URL parameter:** Append the code to your Web Purchase Link (e.g.,` ?discount_code=SUMMER30` ). This is ideal for email campaigns, influencer links, and targeted landing pages where the discount is pre-applied.
- **SDK:** Apply discounts programmatically via the RevenueCat SDK for in-app or server-side flows.
- **Checkout UI:** End users can enter a promo code directly in the web checkout field.


You can also control exactly where the coupon field appears. Visibility is configurable per Web Purchase Link or Funnel checkout, so you only show it when it makes sense for that flow.


Discount codes can be scoped to specific products or applied globally across all products. Eligibility criteria can also be configured to control which users can redeem a given discount.


> Discounts do not stack with introductory offers or trials. If a discount is applied, any configured intro offer is ignored.


Funnels support and discount code analytics are on the way. Head to your RevenueCat dashboard to set up your first discount code, or[read the docs](https://www.revenuecat.com/docs/web/web-billing/discounts) to get started.
