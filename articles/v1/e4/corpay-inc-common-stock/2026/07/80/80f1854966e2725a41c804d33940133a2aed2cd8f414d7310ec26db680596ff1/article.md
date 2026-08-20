---
schema_version: "1.0.0"
document_id: "80f1854966e2725a41c804d33940133a2aed2cd8f414d7310ec26db680596ff1"
company_key: "corpay-inc-common-stock"
company: "Corpay Inc."
source_id: "corpay-inc-common-stock-news-import-8649661f155f"
canonical_url: "https://www.corpay.com/resources/blog/level-2-level-3-card-data"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-24T09:57:54.717504+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:4783db853342ce7c14d7341e15328d131972480198c138af59262697c004440a"
---

# Level 2 and Level 3 Card Data: How Line-Item Detail Lowers Interchange

##


1. What are Level 1, Level 2, and Level 3 card data?


1. What counts as Level 1 data?
2. What extra fields does Level 2 require?
3. What line-item detail does Level 3 add?


2. How does sending more data lower interchange?


1. How much can Level 2 and Level 3 actually save?
2. Who pays the interchange that this lowers?


3. Which transactions and fields actually qualify?


1. What does Visa's CEDP (Product 3) change about Level 2 and Level 3?
2. Do Amex and Discover support Level 3 data?


4. Where does enhanced data matter most for large-ticket and government spend?


1. Why do large-ticket and government transactions need Level 3 data?
2. How does lower interchange improve supplier acceptance and rebate economics?


5. Capture enhanced-data savings with Corpay commercial cards


**Level 2 and Level 3 card data** are the extra transaction details, from sales tax and a purchase order number at Level 2 up to full line-item breakdowns at Level 3, that qualify a commercial card payment for a cheaper interchange tier. Send more data, and the same transaction settles at a lower rate. One point on terminology: Level 3 here means card-transaction line-item data, not stock-market Level 3 quotes, which are an unrelated order-book term. The reason this matters to a finance team runs backward from where you'd expect. Interchange is paid by the supplier's side, so lowering it is what makes a supplier willing to take your card in the first place.


***Key Takeaways***


-


Level 1 is a basic card sale; Level 2 adds sales tax and a customer or PO code; Level 3 adds full line-item detail like item description, quantity, unit cost, and commodity code.


-


Supplying that data drops a qualifying commercial or B2B transaction into a lower interchange tier, and the savings can run well over a full percentage point on large-ticket spend.


-


The savings land on the supplier's side of the transaction, which is why enhanced-data capability is really an acceptance lever that makes the card cheaper for a vendor to take.


-


Visa replaced its legacy Level 2, Level 3, and Large Ticket programs with the Commercial Enhanced Data Program in October 2025, and is sunsetting standalone Level 2 interchange in April 2026; Mastercard still supports Level 2.


-


Not every transaction qualifies. Amex and Discover handle enhanced data differently, and travel and entertainment merchant categories are generally excluded.


## What are Level 1, Level 2, and Level 3 card data?


Level 1, Level 2, and Level 3 describe how much information a card transaction carries with it, from a bare-bones sale up to a full itemized invoice. Each step up adds required fields, and each step up can move a qualifying business card into a cheaper interchange category. The framework applies to the[main commercial card types](https://www.corpay.com/resources/blog/corporate-cards-guide-types-and-benefits) , meaning corporate, purchasing, and business cards, not to a consumer swiping at a coffee shop, where Level 1 is the end of the road.


The cleanest way to see the difference is field by field.


**Data level**


**What it captures**


**Typical use**


**Level 1**


Merchant name, transaction date, and total amount


Every card sale, consumer or commercial


**Level 2**


Level 1 plus sales tax amount and a customer code or PO number


B2B and B2G card payments with basic AP data


**Level 3**


Level 2 plus line-item detail — item description, quantity, unit of measure, unit cost, commodity code, freight, and duty


Large-ticket and government purchasing card spend


*Source: Mastercard Gateway Integration Guidelines, "Level 2 and 3 Data" (na-gateway.mastercard.com), 2025.*


The jump from one level to the next isn't cosmetic. Each added field gives the card network and the issuing bank more evidence that the transaction is a legitimate business purchase rather than a fraud-prone consumer charge, and lower risk is what earns the lower rate.


### What counts as Level 1 data?


Level 1 data is the minimum any card transaction carries, meaning the merchant name, the transaction date, and the total dollar amount. That's it. When a purchasing manager buys office supplies and the receipt shows only a vendor and a total, that transaction ran at Level 1.


For consumer cards, Level 1 is the whole story, and there's no cheaper tier to reach for. For a commercial card, Level 1 is the most expensive way to process, because the transaction carries no context and qualifies for no interchange improvement. A business card that only ever transmits Level 1 data is leaving the entire enhanced-data discount on the table, which for large recurring supplier payments adds up fast.


### What extra fields does Level 2 require?


Level 2 adds two things on top of Level 1, a valid sales tax amount and a customer code or purchase order number. Both have to be present and correctly formatted for the transaction to qualify. The sales tax field is the one that trips merchants up most often, because the networks require a real tax amount between roughly 0.1% and 22% of the transaction. A tax value of zero disqualifies the transaction from Level 2 treatment, according to Mastercard's Gateway Integration Guidelines.


The customer code, sometimes called a purchase order number, is what lets the buyer's accounting team tie the charge back to a specific requisition. That reconciliation value is a real benefit on its own, separate from the interchange savings, and it's part of why purchasing-card programs push suppliers to submit Level 2 in the first place. Getting the fields right is more of an integration exercise than a strategy question, since the supplier's payment gateway either passes the tax and PO fields or it doesn't.


### What line-item detail does Level 3 add?


Level 3 adds a full itemized breakdown of the purchase, the same detail you'd see on a printed invoice. Where Level 2 tells the network how much tax and which PO, Level 3 tells it exactly what was bought, line by line. The required fields typically include:


-


Item description and product or commodity code


-


Quantity and unit of measure


-


Unit cost and line-item total


-


Freight and duty amounts


-


Any line-item discount


This is why Level 3 is associated with large-ticket and public-sector purchasing. A single equipment order or a government supply contract might have dozens of line items, and transmitting all of them is what earns the lowest interchange tier the transaction can reach. It also raises the integration bar, since the supplier's system has to capture and pass structured line-item data, which not every processor or ERP connection does cleanly. Where card payments fit among the broader[set of B2B card types](https://www.corpay.com/resources/blog/card-payment-101-a-guide-to-card-types) is worth understanding before you push a supplier toward Level 3.


## How does sending more data lower interchange?


Sending more data lowers interchange because the card networks price risk, and richer data means lower risk. A fully itemized Level 3 transaction gives the issuer enough detail to treat the charge as a verified commercial purchase, which the networks reward with a cheaper interchange category than a bare Level 1 swipe of the same card. What's happening is a pricing tier, not a discount code. A transaction qualifies for the better rate automatically when the data is present and valid.


Interchange is the largest slice of card acceptance cost. US merchants paid roughly $187.2 billion in card processing fees in 2024, according to The Nilson Report, and the interchange portion is what enhanced data acts on. For a business running large supplier payments through cards, the tier a transaction lands in is the difference between a card program that suppliers tolerate and one they refuse.


Consider the arithmetic on a real invoice. A $50,000 supplier payment processed at a non-qualified rate versus one that qualifies for Level 3 can differ by more than a full percentage point of interchange. At 100 basis points, that's $500 on a single transaction. That money comes off the supplier's cost of accepting the card, which is exactly the cost that makes a supplier hesitate to take cards at all.


### How much can Level 2 and Level 3 actually save?


Level 2 and Level 3 together can cut interchange by roughly 50 to 150 basis points versus a non-qualified commercial card transaction, according to NMI's 2026 guide to Level 2 and 3 interchange rates. The savings stack in two steps. Level 2 qualification typically saves around 30 to 60 basis points over non-qualified for the same card, and Level 3 adds roughly another 50 to 100 basis points over Level 2, scaling with transaction size.


The recoverable amount is meaningful at B2B volume. Optimus.tech's 2025 analysis puts the enhanced-data discount at up to 1.0% to 1.5% of qualifying B2B card spend before processor markups. On a purchasing program running tens of millions in annual card spend, a full percentage point is real money, and it's the mechanism behind[how card spend can turn into rebate revenue](https://www.corpay.com/resources/blog/virtual-card-rebates) rather than a pure cost.


I'd add one caution I've watched play out with finance teams evaluating these numbers. The headline "1.5% savings" figures come mostly from processors and consultants who sell Level 3 enablement, so treat them as a ceiling, not an average. What you actually capture depends on the card type and the ticket size more than anything else, and the small-ticket transactions that fill most AP files barely move. The savings concentrate on large-ticket spend, which is where you should look first.


### Commercial cards success story


See how commercial cards transformed expense management and reporting for a finance team — turning a manual burden into measurable savings and a more strategic AP function.


[Read the success story](https://www.corpay.com/resources/whitepapers/commercial-cards-success-story)


### Who pays the interchange that this lowers?


The supplier's side pays interchange, not the buyer's company, which is the detail that makes this whole topic counterintuitive for finance leaders. A card transaction runs on a four-party model that connects the buyer, the buyer's issuing bank, the card network, and the supplier's acquiring bank. When you pay a supplier by card, their acquiring bank collects the payment and absorbs the interchange cost, and your company never sees a line item for it.


That's why enhanced data is an acceptance play more than a cost-cutting one for the payer. Lowering interchange doesn't cut a fee on your side. It cuts the fee on the supplier's side, which is what makes them willing to take the card. The full economics of who pays and how the fee funds rebates is a longer story than this section needs, and the four-party flow is the same one that makes[B2B virtual card payments](https://www.corpay.com/resources/blog/how-virtual-card-payments-work-b2b) work at all.


## Which transactions and fields actually qualify?


A transaction qualifies for Level 2 or Level 3 interchange when it's made on an eligible commercial card, at an eligible merchant category, with every required field present and correctly formatted. Miss any one of those and the transaction falls back to a higher rate. All three conditions have to hold, not just the data completeness that gets most of the attention.


Here's the practical checklist finance teams and their suppliers work through:


1.


The card is a commercial product such as a corporate, purchasing, or business card, not a consumer card.


2.


The merchant category code is eligible, not one of the excluded categories.


3.


Level 2 fields are complete, meaning a valid, non-zero sales tax amount and a customer or PO code.


4.


For Level 3, every line-item field is present, including description, quantity, unit of measure, unit cost, commodity code, and applicable freight or duty.


5.


The supplier's payment gateway or processor is configured to actually transmit the enhanced data.


That last point is where good programs break down in practice. The card can be right and the data can exist in the ERP, but if the supplier's processor isn't passing the fields, the transaction still qualifies at nothing. Which merchant categories qualify also isn't universal. Travel and entertainment categories such as restaurants, hotels, car rental, and airlines are generally disqualified from Level 2 and Level 3 improvement, according to Payrix's 2025 guidance on interchange data requirements. A card program built on the assumption that every transaction can reach Level 3 will overestimate its savings.


### What does Visa's CEDP (Product 3) change about Level 2 and Level 3?


Visa's Commercial Enhanced Data Program, or CEDP, replaced the legacy interchange programs (Level 2, Level 3, and Large Ticket) with a single framework built around a rate category called Product 3. The change took effect in October 2025 and reshapes how enhanced data earns savings on Visa commercial cards. Under CEDP, a transaction has to come from a verified merchant transmitting complete, accurate enhanced data to reach the best Product 3 rates, so data completeness moved from optional optimization to a qualification gate.


The rate math shifted with it. Visa's large-ticket Corporate and Purchasing interchange dropped roughly 15 basis points under Product 3, moving from about 1.45% plus $35 to 1.30% plus $35, according to Wind River Payments' 2025 breakdown of the Visa changes and Priority Commerce's coverage of the CEDP rules. Optimized Payments' 2025 analysis of the new Product 3 rate tables puts the approximate interchange savings for verified Corporate and Purchasing cards at around 90 basis points versus transactions processed with no enhanced data.


The larger structural change is that Visa is eliminating standalone Level 2 interchange for business cards in April 2026, while Mastercard continues to support Level 2, according to Checkout.com's guidance on the US commercial interchange changes. The two networks now diverge. On Visa, the path to savings runs through CEDP and Product 3, while on Mastercard the Level 2 and Level 3 structure remains in place. Any program quoting "Level 2 savings" as a flat, network-agnostic number is working from a model that's already out of date on the Visa side.


### Do Amex and Discover support Level 3 data?


Amex and Discover do accept enhanced data, but they handle it on their own terms rather than mirroring the Visa and Mastercard Level 2 and Level 3 structure. American Express runs its own large-ticket and enhanced-data programs, and whether a given Amex transaction captures Level 3 depends on the specific program and the merchant's processor setup, so the savings and requirements don't map one-to-one with the Visa or Mastercard framework.


The practical takeaway for a finance team is that "we pass Level 3 data" is a claim worth checking network by network. A supplier's processor might transmit full line-item data on Visa and Mastercard while treating Amex differently, and the interchange behavior follows each network's rules. This is one of the places where the details matter more than the headline, and where it pays to ask a processor exactly which networks their Level 3 enablement covers rather than assuming it's universal.


## Where does enhanced data matter most for large-ticket and government spend?


Enhanced data matters most where the savings compound. Three situations account for most of the payoff:


-


Large-ticket transactions, where a single percentage point of interchange is a large dollar figure.


-


Government and public-sector purchasing, where cards are already itemized by procurement rules.


-


High-volume supplier AP moved onto cards, where small per-transaction savings add up across thousands of payments.


On a small charge, the basis-point difference is a rounding error. On a $75,000 equipment invoice or a recurring six-figure supplier relationship, the same percentage swing is the difference between a card program that pencils out and one that doesn't. The bigger and more itemized the purchase, the more Level 3 has to work with and the more it returns.


Large-ticket and government spend also tend to come with the line-item detail already in hand, which lowers the friction of qualifying. A public agency's purchase order is itemized by mandate; a big equipment order arrives with a detailed bill of materials. The data Level 3 requires is exactly the data these transactions already generate, so the qualification hurdle is lower precisely where the payoff is highest.


### Why do large-ticket and government transactions need Level 3 data?


Large-ticket and government transactions need Level 3 data because their interchange is priced specifically around it, and without the line-item detail they fall to a materially higher rate. Government and B2B purchasing cards were designed for itemized procurement, and the networks built their large-ticket and purchasing interchange categories on the assumption that the data would be there. When it's missing, the transaction can miss the purchasing-card tier entirely, not just lose a small optimization.


Public-sector purchasing is the clearest case. A government buyer using a purchasing card for a large supply contract is exactly the transaction Level 3 was built for, and the interchange categories reward the full itemization with the lowest available rate. For the supplier accepting that card, the difference between passing Level 3 and not can determine whether accepting the card is even economically viable on a large order.


### How does lower interchange improve supplier acceptance and rebate economics?


Cheaper interchange improves supplier acceptance because it directly cuts what the supplier pays to take the card, and it improves rebate economics because it makes more spend card-eligible. When accepting a card costs a supplier roughly 1.3% instead of 2.5%, the objection that "cards are too expensive to accept" gets a lot weaker, and supplier acceptance is the single biggest constraint on any card program's reach. A program only earns rebates on the spend suppliers actually let you route to cards.


The two effects reinforce each other. Cheaper acceptance brings more suppliers onto the card, which grows the base of card-eligible spend, which grows the rebate the program generates. That reach depends on the same groundwork covered in[why vendor enrollment makes or breaks a card program](https://www.corpay.com/resources/blog/vendor-enrollment-virtual-card-success) , because the enhanced-data savings are only as valuable as the acceptance they enable. Card isn't always the right rail. Some suppliers are better paid by ACH or check, and the choice between a[single-use and a multi-use virtual card](https://www.corpay.com/resources/blog/single-use-virtual-cards) shapes both control and acceptance for the ones where card does fit. But for those suppliers, lower interchange is what turns a reluctant "maybe" into a yes.


## Capture enhanced-data savings with Corpay commercial cards


The whole point of Level 2 and Level 3 data is to make card payments cheaper to accept so more suppliers say yes, and that only happens if your card program is actually engineered to pass the enhanced data. A purpose-built commercial card program earns its keep exactly here.[Corpay's commercial card programs](https://www.corpay.com/commercial-cards) are designed to transmit the Level 2 and Level 3 data (and Visa's CEDP fields) that qualify transactions for lower interchange tiers, so the savings that make suppliers willing to accept the card actually materialize instead of leaking away at Level 1.


Scale is part of what makes that work. As Mastercard's #1 commercial B2B issuer connected to a network of more than 4 million accepting vendors, Corpay has the reach to get suppliers onto cards and the data plumbing to keep their acceptance cost low. For AP teams paying suppliers by[virtual card](https://www.corpay.com/commercial-cards/virtual-cards) , the same enhanced-data capability applies, so single-use and multi-use card payments carry the line-item detail that keeps interchange in the lowest tier the transaction can reach. That is what protects both supplier acceptance and the rebate the program returns. The result finance leaders care about isn't a data spec. It's more suppliers accepting cards, at a lower cost to them, funding a better rebate to you.


## Frequently Asked Questions


### What is Level 3 credit card processing?


Level 3 credit card processing is the transmission of full line-item transaction detail on a commercial card payment. Each line carries its own description, quantity, unit cost, and commodity code, the same breakdown you'd see on an itemized invoice. Supplying that data qualifies a B2B or government transaction for the lowest interchange tier available to the card, which lowers what the accepting supplier pays.


### What is the difference between Level 2 and Level 3 processing?


Level 2 adds sales tax and a customer or PO code to a basic card sale, while Level 3 adds a complete itemized breakdown of the purchase on top of that. Level 3 requires far more data, including a description, quantity, unit cost, and commodity code for every line, and generally earns a deeper interchange discount, which is why it's associated with large-ticket and purchasing-card spend.


### Does Amex get Level 3 data?


American Express accepts enhanced data, but through its own large-ticket and enhanced-data programs rather than the exact Level 2 and Level 3 structure Visa and Mastercard use. Whether a specific Amex transaction captures Level 3 depends on the program and the merchant's processor configuration, so it's worth confirming network by network instead of assuming coverage is universal.


### What are the Visa Level 3 data requirements?


Visa's Level 3 requirements, now folded into the Commercial Enhanced Data Program and its Product 3 rates, call for complete line-item detail from a verified merchant. That means an item description and commodity code, quantity and unit of measure, unit cost, plus any freight and duty, all on top of the Level 2 tax and PO fields. Missing or invalid fields drop the transaction to a higher rate.


### Is Level 2 going away?


On Visa, yes. Visa is eliminating standalone Level 2 interchange for business cards in April 2026, replacing it with the Commercial Enhanced Data Program. Mastercard continues to support Level 2. So whether Level 2 still applies depends on the network the card runs on, and any flat "Level 2 saves X" claim needs a network attached to it.


#### David Luther


###### Product Marketing Program Manager


David Luther, MBA is a product marketing program manager with years of experience in commercial banking, finance, and technology sectors, with research and writing appearing in financial publications.


Commercial Cards


##


1. What are Level 1, Level 2, and Level 3 card data?


1. What counts as Level 1 data?
2. What extra fields does Level 2 require?
3. What line-item detail does Level 3 add?


2. How does sending more data lower interchange?


1. How much can Level 2 and Level 3 actually save?
2. Who pays the interchange that this lowers?


3. Which transactions and fields actually qualify?


1. What does Visa's CEDP (Product 3) change about Level 2 and Level 3?
2. Do Amex and Discover support Level 3 data?


4. Where does enhanced data matter most for large-ticket and government spend?


1. Why do large-ticket and government transactions need Level 3 data?
2. How does lower interchange improve supplier acceptance and rebate economics?


5. Capture enhanced-data savings with Corpay commercial cards


#### Switch to Corpay


Discover how making the move to Corpay streamlines payments and strengthens your business.


[Talk to an Expert](https://www.corpay.com/resources/blog/level-2-level-3-card-data#contactUsSection)
