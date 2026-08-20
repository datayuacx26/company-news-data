---
schema_version: "1.0.0"
document_id: "20d344bccc7d1db864530e3dca55801b44bfd535df8dcf81f7227c75f8477cbb"
company_key: "yc-increase"
company: "Increase"
source_id: "yc-increase-news-import-16282b6988d7"
canonical_url: "https://increase.com/articles/check-updates"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-21T23:41:43.316828+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:caae1b6789d456d157c506cffd7794b3c6838f670ccf17052f95409e86450222"
---

# Why we’re still talking about paper checks in 2026 — Increase

We’re not going to sell you on the benefits of sending checks. If you’re sending checks in 2026, you’re doing it because you have to.


Many healthcare, real estate, and insurance businesses still[rely on checks](https://www.atlantafed.org/research-and-data/publications/take-on-payments/2026/01/12/why-do-businesses-still-use-paper-checks) to send and receive funds. While the majority of our transactions are digital, Increase processed billions of dollars in old-fashioned paper checks in the past six months.


So we invested heavily in features that support platforms scaling check send: support for attachments, overnight shipping, delivery notifications, design customization, and fraud controls.


If you don’t send checks at scale, congratulations! You can read the unmitigated pedantry that follows just for fun.


## Checks love company


If you’re going to the trouble of putting a check into an envelope, you may want other things in there too.


Attachments to checks can be useful for a range of use cases. Bill Pay platforms include remittance information for invoices. Healthcare platforms send explanations of benefits. Some companies send marketing materials along with each check to acquire counterparties as customers.


## Speed matters, even when you’re sending paper checks


When we initially built our[check send API](https://increase.com/documentation/api/check-transfers) , we assumed our users would always prefer the cheapest mailing option and wouldn’t be particularly sensitive to shipment times.


This turned out to be wrong! The CheckTransfer API now supports[Fedex Overnight](https://increase.com/documentation/api/check-transfers#create-a-check-transfer.physical_check.shipping_method.fedex_overnight) shipping.


Users often send checks when collecting bank details isn’t possible or desirable. This includes refunds, rebates, settlements, class-action payouts and more. And sometimes those payments are urgent.


## It’s nice to know where your paper check is


Our users want to give their customers delivery update notifications. So we found ourselves integrating into the United States Postal Service and the FedEx APIs to get these for them. For USPS, that’s the Informed Visibility® Mail Tracking & Reporting (IV-MTR).


You may have had the experience of receiving an in-transit notification a few days after the package is delivered. Delivery tracking update events use[operations codes](https://postalpro.usps.com/informedvisibility/OperationCodesList) , and there are a little less than 1000 different codes. (Fedex, mercifully, uses only a few dozen).


The result is a straightforward and readable timeline, so you can track every step of your paper check’s journey to its destination. We now surface these tracking updates in the Dashboard.


## Customization requires going direct-to-printer


To deliver much of the above, we invested in integrating into an industrial printer. Their preferred delivery format from us was a single, very large PDF, with all of a day’s checks and attachments in a single file. (As you might expect, this created a side quest around file size optimization).


This unlocked an ability to customize appearance on a per-check basis. The American National Standards Institute’s standard for checks specifies the font ([E-13B](https://en.wikipedia.org/wiki/Magnetic_ink_character_recognition) ), the placement of the magnetic ink character recognition line, and the paper used. But that leaves a fairly wide space for customization, which we’re excited to support.


## Fraud controls are critical


No matter how beautifully customized your paper checks may be, they quickly become a hassle if lost or stolen. This unfortunately does happen, and when it does, you’ll need controls to block checks from being fraudulently deposited.


We now offer[fine-grained positive pay controls](https://increase.com/documentation/positive-pay) on every Check Transfer, allowing users to review attempted deposits before funds get withdrawn from an Increase account, and decline the deposit attempt for any reason.


## Designing for programmable paper checks


At Increase, we default to building financial primitives with[no abstractions](https://increase.com/articles/no-abstractions) for flexibility, transparency and reliability.


For this reason, we resisted investing in a printer integration. Our customers are large, sophisticated financial technology companies: maybe they’d prefer to bring their own printer integration for checks?


They didn’t.


So, where it made sense, we picked up as much of the toil as we could for printing and mail tracking. And by directly integrating into an industrial printer for printing, USPS/FedEx for tracking updates, and Check21 for money movement, we’re able to offer API-powered paper checks that are programmable and flexible.


If you’re sending checks at scale, let’s chat! Reach us athello@increase.com .
