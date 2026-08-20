---
schema_version: "1.0.0"
document_id: "402d49e2c04c60a4fb49723b51de5f75ae9a6f1ec7dda064ce5bbd2b41bcd510"
company_key: "yc-miden"
company: "Miden"
source_id: "yc-miden-atom-c1e119f02e59"
canonical_url: "https://blog.miden.co/midens-new-tap-to-pay-brings-contactless-payments-to-businesses-across-africa--and-its-just-one-part-of-a-bigger-play"
published_at: "2026-03-05T10:56:00+00:00"
first_seen_at: "2026-07-25T14:13:42.218758+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:bbff9e7d2adad7edbd0524a53c13696c3d63531cfb8eccbe80ef1b29ed971183"
---

# Miden's new Tap-to-Pay brings contactless payments to businesses across Africa — and it's just one part of a bigger play

Building a financial product in Africa means stitching together providers. One for card issuance, another for disbursements, another for cross-border payments, maybe a fourth for accruals. Each integration is a new contract, a new point of failure, and a new reconciliation headache. It's the plumbing problem that sits quietly beneath most fintech products on the continent.


[Miden](https://miden.co/) is one of the infrastructure companies working on that problem. The company powers card issuing, accruals, disbursements, and global payments — and its pitch to businesses is that they shouldn't have to manage multiple providers to get all of that. This week, it added a new piece to that stack: Tap-to-Pay.


Miden Tap-to-Pay is enabled for Apple Pay, Google Pay, and Samsung Pay. They are compatible for digital wallets and not just used on web and mobile checkouts.


## What changed under the hood


The contactless functionality is built on tokenization. When they issue a card today, after giving us the details of the cardholder they want to issue the card to, we then return a response saying their action was successful, then securely give them the details of the card they just issued.


When a business issues a card through Miden's API, they submit the cardholder's details and receive a success response along with the card's information. But what comes back is not the real card PAN. To retrieve the actual card number, the one that would be supplied to the end user, the business has to perform a separate decryption step on the encrypted PAN that Miden returns.


> "One integration for cards, disbursements, accruals, global payments. The fewer providers in the stack, the fewer operational surprises you’re dealing with at 2am."
>
>
> **Ini Udoh, Co-founder and CTO, Miden.**


In practice what moves through Miden’s system after issuance is an encrypted version of the PAN, not the PAN in plain text. That encrypted value can only be revealed by an authorised party with the appropriate decryption controls. Tokenization, however, is a separate process. When a card is provisioned into Apple Pay, Google Pay, or Samsung Pay, the card networks generate a device-specific token that replaces the PAN during contactless transactions. That token can authorise payments but does not expose the underlying card number, keeping the PAN out of the payment flow.


## Miden is the financial operating system for modern businesses and enterprises.


From card issuing to global payments, pay-ins & pay-outs, plus an AI-driven core banking engine, our composable, API-first infrastructure gives you the tools to launch and scale with confidence.


Built in Africa. Designed for the world.


[Learn more](https://www.miden.co/)


The security implication is straightforward: if a merchant's systems are breached, there is nothing useful to extract. And because the card's identity isn't tied to a specific device, a user who loses their phone doesn't need to cancel or replace their card. They log in on a new device, and the card reconstitutes itself. For businesses building on Miden's infrastructure, that reliability shows up directly in transaction completion rates and the volume of support tickets that never get raised.


> **Read also -**[Payment Tokenizations: What it is and How it Works](https://blog.miden.co/payment-tokenizations)


##
The broader stack


Tap-to-Pay is the newest addition to what Miden describes as a unified payments infrastructure. Rather than positioning it as a standalone feature, the company frames it as part of a single stack that businesses can build on without accumulating dependencies.


- **CARD ISSUING:** Virtual cards — now with NFC contactless and digital wallet support — issued via API and ready to embed in any product.
- **ACCRUALS:** Earned balance management for rewards programs, wallets, and deferred payment models — handled natively within the platform.
- **DISBURSEMENTS:** Bulk and individual payouts, without routing through a separate provider or managing an additional reconciliation layer.
- **GLOBAL TREASURY:** Global treasury rails for businesses that need to send or receive money across markets —same platform, same API.


``


The argument for consolidation is straightforward. Every additional provider in a fintech stack is a contract to maintain, an SLA to monitor, and a point of friction when something goes wrong and something always goes wrong. Miden's case is that handling more of this natively removes compounding failure points, and lets product and engineering teams focus on what they're actually building rather than reconciling data across systems.


## Getting started


Tap-to-Pay is live now. Existing Miden customers can upgrade their card programs from within the dashboard. New businesses can reach out through Miden's[sales team](https://www.miden.co/talk-to-an-expert) . The Card Issuing API is designed to slot into existing product infrastructure without a significant lift. Miden's support team handles onboarding.


Found in:


[Miden News](https://blog.miden.co/miden-news)[Product Updates](https://blog.miden.co/product)


Share


[Twitter](https://twitter.com/intent/tweet?text=Miden%27s%20new%20Tap-to-Pay%20brings%20contactless%20payments%20to%20businesses%20across%20Africa%20%E2%80%94%20and%20it%27s%20just%20one%20part%20of%20a%20bigger%20play%20https://blog.miden.co/midens-new-tap-to-pay-brings-contactless-payments-to-businesses-across-africa--and-its-just-one-part-of-a-bigger-play)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://blog.miden.co/midens-new-tap-to-pay-brings-contactless-payments-to-businesses-across-africa--and-its-just-one-part-of-a-bigger-play)[Linkedin](https://www.linkedin.com/sharing/share-offsite/?url=https://blog.miden.co/midens-new-tap-to-pay-brings-contactless-payments-to-businesses-across-africa--and-its-just-one-part-of-a-bigger-play)
