---
schema_version: "1.0.0"
document_id: "50f69d0640bfec98f13d9f4fd347a14ffc86f8846e2e3f19811ffb4ddc53d287"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T08:10:57.347630+00:00"
fetched_at: "2026-08-11T08:11:00.150542+00:00"
content_hash: "sha256:0d61f28b67c0b92ba1bb8a617b60d8d2e656c29090685903382562797c53bba0"
---

# Why Your Marketplaces Report Different Numbers

*Marketplace revenue reconciliation is the work of tying a marketplace transaction to the money that eventually arrives, across systems that measure it at different moments and call it different things. The reason the numbers never match is not error — each marketplace is reporting a different, correct thing.*


---


Every RevOps lead running more than one marketplace has had this meeting. Three consoles are open. Three revenue figures are on screen. None of them agree with each other, and none agrees with the CRM.


The instinct is to hunt for a bug. There usually isn’t one. The figures differ because the marketplaces measure at different points in a process that takes months, withhold their fees at different moments, and pay out on completely different triggers — in one case *before* the customer has paid.


Understanding those differences is what turns reconciliation from a monthly argument into a mapping exercise. Here are the five that matter.


---


## **Why don’t marketplace revenue numbers match?**


**Because a marketplace dollar exists in several states, and each console reports a different one.** Not a different amount — a different state of the same amount.


At minimum, one transaction passes through: usage or purchase recorded, billing amount calculated, invoice issued to the buyer, invoice paid by the buyer, fees withheld, payout prepared, payout sent, funds received. Eight moments. A console reporting the second and a console reporting the seventh will never agree, and both are right.


The gap is also *long* . Microsoft’s published timeline shows customers paying invoices anywhere from month 3 to month 12 after the usage occurred. A figure taken at any single point in that window is a snapshot of a process, not a total.


---


## **Difference 1: the payout trigger is not the same**


This is the largest structural difference between marketplaces, and the one that surprises finance teams most.


**Microsoft pays out on different triggers depending on how the customer buys.** For customers on an Enterprise Agreement, Microsoft is explicit: “Payouts often occur before Microsoft collects payment from a customer.” For customers on a Microsoft Customer Agreement or through a Cloud Solution Provider, the opposite applies — “Transactions become eligible for payment once Microsoft collects payment from the customer.”


So two identical deals, same product, same price, same month, can pay out a month apart with no error anywhere. The variable is the customer’s agreement type, which sits in the customer’s contract with Microsoft and not in your records.


That distinction has a tail. Because Microsoft sometimes pays before collecting, it also has a documented recovery process when the customer never pays. Collections run “approximately four months”; if the money is written off, Microsoft “might subtract the unpaid amounts from future payouts,” and future payouts are withheld until the balance clears. A negative line in a later period may therefore belong to a transaction two quarters back.


---


## **Difference 2: the states have different names everywhere**


Each marketplace exposes the process through its own vocabulary, and the vocabularies do not line up.


**Microsoft** shows a transaction as *Uncollected* in the Revenue dashboard when invoiced but unpaid, then *Unprocessed* , *Upcoming* , and *Sent* in the Earnings report as the payout is prepared and released. Usage, invoicing, and earnings appear on three different dashboards.


**AWS** separates finance operations into a billed revenue dashboard — “information about billed revenue for accounting and other financial reporting purposes” — and a collections and disbursement dashboard, which reports “funds that AWS collected and disbursed to your bank accounts since the previous disbursement.” As of January 2026 AWS added payment collection status to billed revenue reporting so sellers can “distinguish between invoiced, collected, and disbursed amounts” directly.


**Google Cloud** publishes Charges and Usage Reports (“details of your software’s usage”), Disbursements Reports (“details about the amount paid to you”) and Detailed Disbursements Reports (“a detailed breakdown of the amount paid to you, for accounting and financial reconciliation purposes”).


The mapping exercise is therefore not “find the revenue number in each console.” It is “decide which state you are reporting, then find *that* state in each console.” Those are very different projects, and only the second one converges.


---


## **Difference 3: fees come out at different moments**


Every marketplace withholds its fee before paying you, but the figure you see depends on which report you opened.


Microsoft describes an agency model in which “publishers set prices, Microsoft bills customers, and Microsoft pays revenue to publishers while withholding an agency fee,” and its payout tables show the store service fee determined at the *transaction reported* step — after the customer transacts, before the payout is prepared.


The consequence is that a gross figure from one console and a net figure from another differ by a percentage that itself varies by product type, offer type, contract value, and channel. If you are modelling that percentage as a constant, the model will drift.[What a marketplace dollar actually costs you](https://www.suger.io/resources/blog/what-a-marketplace-dollar-costs-you/) works through the lines.


---


## **Difference 4: usage is dated when it happened, not when it was counted**


Usage-priced products add a reporting-period problem on top of everything else.


Microsoft is precise about it: “Usage date in reporting shows as beginning of the month in which the usage occurred (for example, October 1 for usage that occurred anytime in October).” The billing amount for that usage is calculated the following month, invoiced after that, and paid after that.


So a single row can carry three defensible dates — when the usage happened, when it was billed, and when it was paid — and the three systems in the room are each keyed on a different one. Any reconciliation that joins on “month” without saying *which* month will produce a plausible, wrong answer.


And a separate exposure sits underneath: on usage pricing, marketplaces bill from records your product transmits. Records you never sent are not a reporting discrepancy — they are revenue that does not exist. That is a metering integrity problem, not a reconciliation one, and it is covered in[cloud marketplace metrics](https://www.suger.io/resources/blog/cloud-marketplace-metrics/) .


---


## **Difference 5: nobody is reporting on your entities**


The last difference is the one no console can fix.


Marketplace reports are keyed on marketplace accounts — an AWS account ID, a Microsoft tenant, a Google Cloud billing account. Your finance system is keyed on legal entities and your CRM on account records. One customer can hold several marketplace accounts across several marketplaces, and no marketplace knows about the others.


That join is yours, permanently. It is also the single highest-leverage thing to build, because every cross-marketplace question — total revenue per customer, renewal exposure, concentration risk — depends on it and none of them can be answered without it.


---


## **How to build a reconciliation that converges**


Five rules, in the order they pay off.


**1. Pick one grain and stick to it.** The agreement or entitlement is the right one: it survives amendments, it exists on every marketplace, and it is what a renewal happens to. Reconciling at invoice-line grain is possible and much harder.


**2. Store states, not a number.** For each agreement, hold invoiced, collected, disbursed and their dates — separately. Collapsing them to one “revenue” field destroys the information that explains the difference, and you cannot get it back.


**3. Stamp both dates on usage.** Occurrence and billing. Then declare which one your reporting is keyed on, in writing, once.


**4. Keep the marketplace’s own identifier.** Agreement ID, offer ID, disbursement ID. When a figure is queried nine months later, the identifier is what lets you find the transaction; a customer name will not.


**5. Model recoupment as a first-class event.** Negative adjustments arriving in a later period, referring to an earlier one, are normal — not an anomaly to be investigated each time.


Then set expectations properly: the goal is not three consoles showing the same number. It is one number whose relationship to each console is explained.[Bridging the GAAP on marketplace revenue](https://www.suger.io/resources/blog/finance-101-bridging-the-gaap-on-marketplace-revenue/) covers the accounting treatment on top of that, and the[marketplace billing guide](https://www.suger.io/resources/guides/marketplace-billing/) covers the operational layer.


---


## **Frequently asked questions**


**Why do AWS, Microsoft, and Google Cloud report different revenue numbers?** Because each reports a different state of the same transaction — usage recorded, invoiced, collected, or disbursed — and each withholds fees at a different point. The figures differ structurally, not because one is wrong.


**Does Microsoft pay before the customer does?** Sometimes. For Enterprise Agreement customers, Microsoft states that payouts often occur before it collects from the customer. For Microsoft Customer Agreement and CSP customers, transactions become eligible for payment only once Microsoft has collected.


**What happens if a marketplace customer never pays?** Microsoft runs a collections process of roughly four months, then writes the amount off. If it had already paid you, it may subtract the amount from future payouts and withhold them until the balance clears.


**What is the difference between billed, collected, and disbursed?** Billed is what the marketplace invoiced the buyer. Collected is what the buyer actually paid. Disbursed is what reached your bank after fees. They are three different amounts at three different times.


**Which grain should marketplace reconciliation use?** The agreement or entitlement. It exists on every marketplace, survives amendments, and is the object a renewal acts on. Invoice-line grain is possible but much harder to keep aligned.


**Why don’t marketplace reports match my CRM?** Marketplace reports are keyed on marketplace accounts; your CRM is keyed on account records. One customer may hold several marketplace accounts across several marketplaces, and no marketplace knows about the others. That join is yours to build.


---


## **Takeaways**


- The consoles disagree because each reports a different state of the same dollar, not a different amount.
- Microsoft’s payout trigger depends on the customer’s agreement type: Enterprise Agreement transactions often pay out before collection, MCA and CSP only after.
- Money already paid can be recovered. Model recoupment as a normal event, not an anomaly.
- Usage is dated when it occurred; billing and payment follow in later months. Declare which date your reporting keys on.
- Fees come out at different moments and vary by product, offer, value, and channel. A constant percentage will drift.
- Marketplace reports know marketplace accounts, not your customers. That join is permanently yours — and it is the one worth building first.


---


Reconciliation is a data-model problem long before it is an accounting one. See how[billing and metering in Suger](https://www.suger.io/platform/billing-metering/) holds offers, entitlements, invoices, and disbursements across every marketplace at one grain — so the states are separate and the join already exists.
