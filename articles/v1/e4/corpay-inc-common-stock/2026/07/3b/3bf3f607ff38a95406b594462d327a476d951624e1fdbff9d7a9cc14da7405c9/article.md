---
schema_version: "1.0.0"
document_id: "3bf3f607ff38a95406b594462d327a476d951624e1fdbff9d7a9cc14da7405c9"
company_key: "corpay-inc-common-stock"
company: "Corpay Inc."
source_id: "corpay-inc-common-stock-news-import-8649661f155f"
canonical_url: "https://www.corpay.com/resources/blog/virtual-card-bin"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-24T09:57:54.717504+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:c92fa928bfa1cfa7b08fd15daf9cd62c13759879da4a3a60d15c08e073206a72"
---

# Virtual Card BIN: How Virtual Card Numbers Work

##


1. What is a BIN, and what does it tell you?


1. Is a BIN the same as an IIN?
2. Where does the BIN sit inside a card number?


2. How is a virtual card number structured and generated?


1. Does a virtual card have its own BIN?
2. How is a virtual card number issued for a payment?


3. What's the difference between single-use and lodged BIN ranges?


1. When should AP teams use a single-use number?
2. When does a lodged or multi-use card make more sense?


4. What controls can you put on a virtual card number?


1. How do these controls reduce fraud?
2. Can you lock a card to a single merchant or category?


5. How do virtual card BINs affect reconciliation?


1. How does a virtual card number match back to an invoice?
2. Why does this matter for ERP users?


6. Run virtual cards as a managed program with Corpay


A **virtual card BIN** is the Bank Identification Number, the leading digits of a virtual card number that identify the issuing bank, the card network, and the card type. Now standardized as the IIN (Issuer Identification Number), it tells the payment system where a charge routes and who stands behind it.


That's the short answer. The longer one is more useful to an AP team, because the BIN isn't a separate code stapled onto the card. It's the first stretch of the card number itself, and a virtual card number is a real, network-routable number generated on demand from the issuer's assigned BIN range. Understanding how that number is built, what rides on top of it, and how it clears is what lets you answer your own team's questions about whether a card is legitimate, why a BIN changed, and how a one-time number reconciles back to an invoice.


***Key Takeaways***


-


A BIN (Bank Identification Number) is the leading digits of any payment card number. It identifies the issuing institution, the card network, and the product type. The ISO standard term is now IIN (Issuer Identification Number); the two are used interchangeably.


-


Mastercard and Visa expanded the BIN from six to eight digits effective April 2022, so explainers that still say "the first six digits" are describing the old standard.


-


A virtual card number is a genuine primary account number (PAN), with 16 digits, a CVV, and an expiration date, generated on demand from the issuer's BIN range, usually through an API call from an AP system or a click from a controller.


-


Single-use and lodged (multi-use) programs can draw from different BIN ranges, and the controls layered on the number, including amount caps, merchant locks, and expiry windows, are what make a stolen virtual card number close to worthless.


-


The same number-level data that fraud-proofs the payment also carries it cleanly back to the invoice, which is where the BIN stops being trivia and starts mattering for ERP reconciliation.


## What is a BIN, and what does it tell you?


A BIN (Bank Identification Number) is the set of leading digits on a payment card number that identify the institution that issued the card, the network it runs on, and the type of product it is. It's read by the payment system at the moment of authorization to route the transaction to the right issuer for approval.


Those digits do a surprising amount of work. From the BIN alone, a merchant's payment processor can tell that a card is a Mastercard rather than a Visa, a commercial card rather than a consumer one, a credit product rather than a debit one, and which bank will be asked to approve or decline the charge. None of the account-specific detail is exposed. The BIN describes the issuer and the product, not the cardholder.


Two facts trip people up. First, the BIN expanded. Mastercard and Visa moved from a six-digit to an eight-digit BIN effective April 2022, adopting the ISO eight-digit standard, according to Mastercard's 2021 guidance on 8-Digit BIN Expansion and PCI Standards. A lot of older reference material still describes "the first six digits," which was correct under the previous scheme and is now incomplete. Second, the term itself has drifted, which is worth its own subsection.


### Is a BIN the same as an IIN?


Yes — BIN and IIN refer to the same digits, and the two terms are used interchangeably. IIN (Issuer Identification Number) is the term the international standard uses; BIN (Bank Identification Number) is the older, more colloquial name that stuck in day-to-day payments conversation.


The governing standard is ISO/IEC 7812, which defines the numbering system for card issuers. Issuer Identification Numbers are administered under that standard's registration program, with ANSI acting as the registration authority in the United States, per ANSI's 2024 description of its ANSI-Managed Registration Programs. So when a vendor's payment operations team says "BIN" and your card provider's documentation says "IIN," they're pointing at the same leading digits. The name difference is historical, not technical.


### Where does the BIN sit inside a card number?


The BIN sits at the front of the card number, and the rest of the digits break down into two more parts. A payment card number, formally the primary account number, or PAN, is defined by ISO/IEC 7812-1:2017 as three components:


-


The IIN / BIN, the leading digits (now up to eight), identifying the issuer, network, and product type.


-


The individual account identifier, the middle digits that distinguish one account from another within that issuer's range.


-


A single check digit, the final digit, calculated with the Luhn algorithm, which catches most keying errors before a bad number ever reaches the network.


The very first digit does its own job as the Major Industry Identifier, which flags the broad category of issuer (for the major card networks used in B2B payments, that leading digit lands in the range these networks were assigned). If you've ever noticed that most commercial cards you handle start with the same digit, that's the MII doing its job. The way these three parts fit together is identical whether the card is a plastic one in a wallet or a virtual number that exists for an afternoon. That's worth holding onto, because it's the reason a virtual card behaves like any other card at the point of sale. The mechanics carry straight over from how a[physical card compares to a virtual one](https://www.corpay.com/resources/blog/virtual-card-vs-physical-card) .


## How is a virtual card number structured and generated?


A virtual card number is a real, fully routable payment card number, carrying the same 16 digits, CVV, and expiration date as a plastic card, that gets generated on demand from an issuer's BIN range rather than embossed onto a physical card. It isn't a token or a placeholder. It's a live PAN that clears through the card networks exactly like any other card, which is what makes[a virtual card](https://www.corpay.com/resources/blog/virtual-card) usable at any merchant that accepts the network.


The difference is where it comes from and how long it lasts. Instead of a bank printing a card and mailing it, an AP system asks the issuer's platform for a number, and the platform mints one from the issuer's assigned BIN range with whatever controls the request specifies. In a B2B setting that request usually comes one of two ways. It's either an API call fired automatically by the AP or ERP system when an invoice hits approved status, or a controller generating a card by hand in the issuer's portal for a specific payment. Either way, the number that comes back is structured like any card — BIN, account identifier, Luhn check digit — and it's ready to charge within seconds.


### Does a virtual card have its own BIN?


A virtual card doesn't get a unique BIN of its own. It draws from the issuer's assigned BIN range, the same pool that backs the issuer's other cards on that network. The BIN identifies the issuer and product, so every card the issuer generates on a given program shares the BIN that describes that program.


Where it gets more textured is that a single issuer can run multiple BIN ranges and map different program types to different ranges. A single-use program and a lodged (stored) program might sit on separate BIN ranges, which is one legitimate reason the leading digits on two cards from the same provider can differ. If someone on your team notices that a single-use card and a recurring-vendor card carry different BINs, that's usually program design, not an error. It's the issuer segmenting card types at the number level, which can also make downstream reporting cleaner.


### How is a virtual card number issued for a payment?


A virtual card number moves through a short, well-defined life cycle from request to close. In a typical AP workflow it looks like this:


1.


**Request.** The AP or ERP system calls the issuer's API (or a controller clicks "generate") once an invoice is approved for payment.


2.


**Generate.** The issuer mints a PAN from its BIN range, stamped with the controls the request carried — an amount cap, a merchant or category lock, an expiry window.


3.


**Deliver.** The number reaches the vendor or the AP system, through a supplier portal, a remittance email, or a direct API handoff.


4.


**Clear.** The vendor charges the card, the transaction routes to the issuer for authorization, and it settles like any card payment.


5.


**Close.** A single-use number dies after it clears; a lodged number stays open within its controls until it expires or is deactivated.


The whole cycle takes seconds on the issuing side and looks like an ordinary card charge on the vendor side, with no special rail and no separate enrollment step for the vendor to run the payment. How that flow connects to the rest of the payables process is covered in more depth in[how virtual card payments work for B2B](https://www.corpay.com/resources/blog/how-virtual-card-payments-work-b2b) . Before you commit to a program, generating a couple of test cards and watching where the BIN and controls actually land in your reporting is worth the ten minutes. Demo environments smooth over details that production doesn't.


## What's the difference between single-use and lodged BIN ranges?


Single-use and lodged (multi-use) cards differ in how long the number lives and how many charges it's built to handle, and issuers often map the two program types to different BIN ranges. A single-use number is generated for one transaction and closes the moment it clears. A lodged number is stored against a vendor or a category and reused across many charges, held in check by spend limits and expiration dates instead of single-transaction termination.


**Dimension**


**Single-use BIN range**


**Lodged / multi-use BIN range**


Number lifespan


One transaction, then dead


Weeks, months, or open-ended within controls


Charges allowed


Exactly one


Many, up to the caps set


Natural fit


Approved one-off invoices


Recurring vendors, subscriptions, project spend


Reconciliation


One card, one charge, one ledger entry


Many charges per card, reconciled at the vendor level


Post-charge fraud exposure


Near zero; number is closed


Open until expiry or deactivation


*The two program types often sit on separate BIN ranges from the same issuer, which is why leading digits can differ card to card.*


Both draw from the same issuer, both run on the same networks, and both can carry merchant locks and spend caps. What separates them is duration and reuse, and matching the right one to the payment in front of you is most of the skill. A deeper walk through the single-use case lives in the[single-use virtual cards](https://www.corpay.com/resources/blog/single-use-virtual-cards) breakdown.


### Best practices for a virtual card program


Learn the internal strategies that make a virtual card program succeed — from program design to driving the vendor acceptance that determines how much of your AP spend earns rebates.


[Download the guide](https://www.corpay.com/resources/whitepapers/best-practices-for-implementing-a-virtual-card-program)


### When should AP teams use a single-use number?


Reach for a single-use number when one vendor, one amount, and one payment window are all known before the card is issued. That describes most traditional AP work, meaning an approved invoice with a fixed amount going to an identified supplier on a predictable timeline.


The payoff is precision. A controller paying 60 invoices in a month doesn't want 60 live card numbers sitting in vendor inboxes; she wants 60 clean payments that each tie back to one approved invoice and then disappear. The single-use number does that. It also compresses one of the more expensive AP mistakes, paying the wrong amount or the wrong party, because the number only works at the merchant it was built for, for the amount it was built for. A misroute simply declines.


### When does a lodged or multi-use card make more sense?


A lodged card makes more sense when re-issuing a number for every charge would create more friction than the extra control is worth. Recurring relationships are the clear case: a software vendor billed monthly, a shipping account charged on every pickup, a travel program where a single stored number covers a stream of bookings.


Here you accept a longer-lived number in exchange for not touching the payment setup every cycle. The controls do the containment work instead of the card's expiry. A number locked to one merchant, capped at a monthly limit, and set to expire at the end of a contract term behaves a lot like a dedicated account for that vendor, without handing over a static card that could be skimmed and reused elsewhere. Lodged cards also sit close to the older concept of a shared department card, so it's worth understanding[how a ghost card differs from a virtual card](https://www.corpay.com/resources/blog/what-is-a-ghost-card) before you decide which model fits a given spend category.


## What controls can you put on a virtual card number?


The controls that ride on a virtual card number are what turn a live PAN into a payment that can only do one specific thing. They're set at generation and enforced by the issuer at authorization, which means they hold even if the number is intercepted after it's issued. The main levers:


-


Amount cap. The number authorizes up to a set value, often the exact invoice total, and declines anything above it.


-


Merchant or category lock. The number works only at a named merchant, or only within a permitted merchant category, and declines everywhere else.


-


Expiry window. The number is valid for a defined stretch of hours, days, or a contract term, then stops authorizing.


-


Single-use vs. multi-use. Whether the number dies after the first successful charge or persists for many, within the caps above.


Stack two or three of these and the number becomes narrowly purpose-built. An $8,432.17 card, locked to one vendor's merchant ID, good for 72 hours, is useful for precisely one payment and close to inert for anything else. This is the same control surface that governs plastic commercial cards, laid out in the guide to[card controls and spend policies](https://www.corpay.com/resources/blog/card-controls-spend-policies) . The difference is that on a virtual number you can set them fresh for every single card.


### How do these controls reduce fraud?


The controls reduce fraud by shrinking the window and the surface a stolen number can be used in. A single-use number that's already cleared authorizes nothing further, so a breach of the vendor's systems after the fact captures a dead number. Lock the number to one merchant and it can't be run at another. Set a tight expiry and a leaked number is worthless within days.


The data tracks with the mechanism. Just 5% of organizations reported fraud involving virtual cards in 2024, far below the exposure on checks and ACH, according to the Association for Financial Professionals' 2024 AFP Payments Fraud and Control Survey. That's not because virtual card numbers are magic; it's because a number carrying an amount cap, a merchant lock, and a short fuse gives an attacker very little to work with. Compare that to a static card number stored in a vendor's billing system for years, where a single leak stays exploitable until someone notices.


### Can you lock a card to a single merchant or category?


Yes — a merchant lock ties a virtual card number to one specific vendor, and a category lock restricts it to a permitted class of merchant. Both are enforced at authorization, so a charge from anywhere outside the allowed merchant or category declines automatically.


The category version leans on Merchant Category Codes, the four-digit codes card networks assign to classify what a business sells. Lock a card to the codes that match legitimate spend — say, office supplies or freight — and a charge at an unrelated merchant type won't go through, even if the number is otherwise valid. The mechanics of those codes, and how they're assigned, are worth understanding when you set category rules; the[merchant category codes](https://www.corpay.com/resources/blog/merchant-category-codes) explainer covers how they work. Merchant-level locks are tighter still, pinning the number to a single vendor's merchant ID.


## How do virtual card BINs affect reconciliation?


Virtual card BINs and the controls attached to each number make reconciliation cleaner because the payment carries its own identifying data all the way back to the invoice. When a card is generated for one payment, the number itself becomes a key that ties the charge to the invoice, the vendor, and the approval that authorized it. The matching that AP teams usually do by hand is largely pre-built into the card.


Reconciliation is where the topic stops being card trivia. What most finance teams struggle with isn't understanding a BIN, it's the cleanup when a payment doesn't map cleanly to a record. As one AP practitioner put it, "Our ERP didn't flag it because it only checks for exact matches. We only caught it weeks later during reconciliation." A number issued per payment, carrying its own amount and merchant data, is built to avoid exactly that lag. The broader reconciliation workflow — matching, exceptions, close — is laid out in the[payment reconciliation](https://www.corpay.com/resources/blog/payment-reconciliation) overview.


### How does a virtual card number match back to an invoice?


A single-use virtual card number matches back to an invoice because it was issued for that invoice and nothing else. One card, one charge, one ledger entry. The card's amount was set to the invoice total, the merchant lock was set to that vendor, and the remittance data travels with the transaction, so when the charge settles the system already knows what it was for.


That one-to-one mapping is the quiet advantage. There's no exercise to figure out which of a vendor's several invoices a lump charge covered, because the charge was never a lump — it was a purpose-built card that only ever paid the one invoice. Lodged cards reconcile a step differently, since many charges roll up under one number, matched at the vendor level using the transaction detail and remittance data each charge carries. Either way, the number-level data does work that would otherwise fall to a person comparing a bank statement against an invoice register.


### Why does this matter for ERP users?


It matters for ERP users because the ERP records the liability but wasn't built to run the last mile of payment delivery and settlement matching. Your NetSuite, Microsoft Dynamics 365 Business Central, Sage Intacct, or Acumatica instance knows what you owe. Getting a controlled payment out the door and getting the cleared transaction to reconcile itself against that record is the gap.


Checks are still the drag here. Roughly 75% of organizations still use checks, and 34% make more than a quarter of their payments by check, according to the AFP's 2024 Payments Fraud and Control Survey — and every one of those checks is a manual reconciliation waiting to happen, plus the payment type most often hit by fraud. A virtual card number that arrives pre-matched to its invoice is the opposite kind of work: it closes the loop the ERP leaves open, rather than adding one more artifact to chase at month-end.


## Run virtual cards as a managed program with Corpay


Everything above is the mechanics. Where it becomes an operating reality is a program that generates the numbers, gets vendors to accept them, and feeds the cleared transactions back for reconciliation without your team stitching it together by hand.


That's what Corpay's[virtual card program](https://www.corpay.com/commercial-cards/virtual-cards) is built to do. We issue single-use and lodged numbers from our BIN ranges with the controls set per payment, and our managed service handles the part that usually stalls a card program, which is supplier enrollment and the follow-up to get vendors accepting card payments in the first place. The controls, the number-level data, and the settlement matching land together, so the same card that fraud-proofs a payment is the card that reconciles it. As Mastercard's #1 commercial B2B issuer, connected to a network of 4M+ accepting vendors, we can get more of your spend onto cards than a program you'd stand up alone. Virtual cards sit inside our broader[commercial card program](https://www.corpay.com/commercial-cards) , so the same controls and reporting extend across physical and virtual spend.


Two decisions tend to come up as teams move from understanding the mechanics to running them. The first is enrollment, the practical reality of[getting vendors to accept virtual card payments](https://www.corpay.com/resources/blog/vendor-enrollment-virtual-card-success) , which is where most of the program value is won or lost. The second is provider fit, and the criteria that separate one card program from another are laid out in the guide to[how to choose a virtual card provider](https://www.corpay.com/resources/blog/how-to-choose-virtual-card-provider) . Both feed directly into whether the reconciliation payoff in this article shows up in your close, or stays theoretical.


## Frequently Asked Questions


### What is a BIN number?


A BIN (Bank Identification Number) is the leading digits of a payment card number, up to eight digits since the April 2022 expansion, that identify the issuing institution, the card network, and the card type. The payment system reads it at authorization to route the charge to the right issuer.


### Is a BIN the same as a bank identification number?


Yes, BIN stands for Bank Identification Number, so the two are the same thing. In current usage the ISO standard term is IIN (Issuer Identification Number), and BIN and IIN are used interchangeably for the same leading digits of the card number.


### Does a virtual card have a real card number?


Yes. A virtual card number is a genuine primary account number with its own CVV and expiration date, generated from the issuer's BIN range. It routes and clears through the card networks like any other card. The one difference is that it's created on demand and often carries controls that limit it to a single purpose.


### Can a virtual card BIN be reused?


The BIN itself is shared across every card an issuer generates on that program, so in that sense it's reused constantly — it identifies the issuer, not the individual card. A specific single-use card number, though, isn't reusable. Once it clears, it's closed, and any further charge declines.


### How many digits is a BIN now?


A BIN is up to eight digits for Mastercard and Visa, following the expansion from the older six-digit standard that took effect in April 2022. Some reference material still cites six digits, which reflects the previous scheme rather than the current ISO eight-digit standard.


### Are virtual card numbers safe?


Virtual card numbers are among the lower-risk payment methods for B2B, largely because of the controls that ride on each number. AFP's fraud data puts them well below checks and ACH for reported fraud, and the reason is mechanical: a stolen number that's capped, merchant-locked, and short-dated gives an attacker almost nothing to use.


#### David Luther


###### Product Marketing Program Manager


David Luther, MBA is a product marketing program manager with years of experience in commercial banking, finance, and technology sectors, with research and writing appearing in financial publications.


Virtual Card


Commercial Cards


##


1. What is a BIN, and what does it tell you?


1. Is a BIN the same as an IIN?
2. Where does the BIN sit inside a card number?


2. How is a virtual card number structured and generated?


1. Does a virtual card have its own BIN?
2. How is a virtual card number issued for a payment?


3. What's the difference between single-use and lodged BIN ranges?


1. When should AP teams use a single-use number?
2. When does a lodged or multi-use card make more sense?


4. What controls can you put on a virtual card number?


1. How do these controls reduce fraud?
2. Can you lock a card to a single merchant or category?


5. How do virtual card BINs affect reconciliation?


1. How does a virtual card number match back to an invoice?
2. Why does this matter for ERP users?


6. Run virtual cards as a managed program with Corpay


#### Switch to Corpay


Discover how making the move to Corpay streamlines payments and strengthens your business.


[Talk to an Expert](https://www.corpay.com/resources/blog/virtual-card-bin#contactUsSection)
