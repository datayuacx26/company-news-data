---
schema_version: "1.0.0"
document_id: "459b22156478461e1cde1d0afb21ca48e3f62867f05624efb4267900d4f9799f"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/microsoft-ip-co-sell-how-it-works/"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T10:32:29.919238+00:00"
fetched_at: "2026-08-19T10:32:31.370146+00:00"
content_hash: "sha256:8b8664def622e9aaec673a094e33689ccc4c9beecb68cd193a630f95a0d3c68d"
---

# Microsoft IP Co-Sell: How It Actually Works

*Microsoft IP co-sell is Microsoft’s joint go-to-market motion for qualified partner software, where a Microsoft seller earns a stake in an independent software vendor’s (ISV’s) deal in exchange for helping win it. Starting in FY27, the incentive flow runs through Microsoft Marketplace rather than partner-reported revenue — which changes what a seller has to operationalize.*


---


Most explanations of Microsoft IP co-sell stop at “build a relationship with your Microsoft account team.” That advice is true and it is nowhere near enough. IP co-sell is a qualification process, a registration gate, and an incentive flow, and each one has rules that decide whether a closed deal counts for anything.


Those rules are also mid-shift. In July 2026 Microsoft told partners that in FY27, Microsoft Marketplace becomes the primary path for co-sell at scale, and that Partner Reported Azure Consumed Revenue (PRACR) no longer operates as a broad co-sell mechanism. That is not a cosmetic change to a portal — it moves where co-sell credit is earned.


This post covers what IP co-sell is, how a deal qualifies and registers, how the incentive flows, and what the FY27 direction changes for the team running it. The mechanical state-machine detail — statuses, fields, sales stages across all three clouds — sits in the companion guide,[how co-sell works on AWS, Azure, and Google Cloud](https://www.suger.io/resources/blog/how-co-sell-works/) .


---


## **What is Microsoft IP co-sell?**


Microsoft IP co-sell is a deal type in Microsoft Partner Center where a software development company co-sells its own intellectual property — its product — with Microsoft, using at least one Azure IP co-sell eligible solution. Microsoft’s own definition is exactly that: “An IP co-sell opportunity is one where a software development company chooses to co-sell with Microsoft using at least one Azure IP co-sell eligible solution.”


The word that carries the weight is *IP* . It separates this motion from *Services co-sell* , which is for partners with a Solutions Partner designation selling implementation and consulting work rather than a product. IP co-sell is for the ISV whose software is the thing being sold, and it is the motion that connects to Azure consumption and Marketplace transactions.


The vehicle is a co-sell opportunity record you create in Partner Center. A Microsoft seller can see it, accept it, and work it — and if the deal meets the registration criteria, it converts into program credit. Notification, attribution, and coordination all run through that one record.


---


## **How do you qualify for Microsoft IP co-sell?**


You qualify for Microsoft IP co-sell by having at least one co-sell-ready solution before you create anything. Microsoft is explicit: “To create a new co-sell deal, you need at least one co-sell-ready solution.” That single prerequisite is the gate every IP co-sell deal passes through, and it is upstream of any individual opportunity.


Getting a solution to co-sell ready is a solution-level exercise, not a deal-level one. It means the product is packaged, listed, and marked as an Azure IP co-sell eligible solution in Partner Center — the status a Microsoft seller checks before they will engage. Without it, a well-run deal simply has nowhere to attach.


Two more qualification facts decide eligibility inside the deal itself, and both are set at the *first* form fields rather than at close:


- **The customer account has to be Microsoft-managed.** When you search for the customer, Partner Center returns results on three tabs — *Microsoft Managed* , *Microsoft Unmanaged* , and *Other* . Only a Microsoft-managed account is eligible for IP co-sell deal registration. Pick from the *Other* tab and, in Microsoft’s words, “you won’t know whether deal registration is possible until later, and you won’t be able to change the account without involving support.”
- **Solution Area and Solution Play are now required.** Microsoft made both mandatory for IP co-sell deals created through the Partner Center portal and bulk upload. They exist so a Microsoft seller can identify the deal in standard terminology and route it.


There is also an IP-co-sell-only step called *capture intent* , where you tell Microsoft whether the customer intends to purchase on Microsoft Marketplace — *Yes* , *No* , or *Have not decided* . That field is a small thing that has become a large one, for reasons the FY27 section makes clear.


---


## **How does the Microsoft co-sell incentive flow work?**


The incentive flow has two halves: **registration** , which establishes that the deal counts, and **recognition** , which is how the credit and benefits accrue. Getting paid for co-sell means clearing the first and then transacting through the second.


Registration is the hard gate. A deal is eligible for Azure IP co-sell deal registration only if **all** of the following hold:


Criterion Requirement


Status The deal is marked *won*


Solution An Azure IP co-sell eligible solution is in the deal


Deal type *Partner-led* or *co-sell*


Microsoft participation If *co-sell* , Microsoft accepted the invitation or marked the deal won


Deal value Greater than or equal to **USD 25,000** (converted at Reuters monthly rates)


Customer account Managed by Microsoft


Miss any one and the registration is rejected. One timing rule catches fast-moving teams in particular: Microsoft asks for “a gap of 72 hours between creating the deal and marking the deal *won* ,” because closing earlier “might result in deal registrations being rejected.” And the terminal states — *won* , *lost* , *declined* , *expired* — cannot be edited, so a deal marked won to tidy the pipeline is done, whether or not it was ready.


Recognition is the other half. Beyond registration credit, transacting partners unlock Marketplace Rewards benefits that scale “by tier of Marketplace billed sales (MBS)” — sales and marketing support, and Azure sponsorship that provides free Azure usage to offset a customer’s deployment costs on a Marketplace deal. The more you transact through Marketplace, the more you earn. That link between transacted revenue and benefit tier is the mechanism FY27 leans into.


---


## **What changes for IP co-sell in FY27?**


In FY27 — from July 1, 2026 to June 30, 2027 — Microsoft Marketplace becomes the primary path for co-sell at scale, and PRACR is retired as a broad co-sell mechanism. Microsoft states both directly: “Marketplace becomes the primary path for co-sell at scale,” and “In FY27, PRACR no longer operates as a broad co-sell mechanism, and partner engagement instead aligns to Marketplace-first execution.”


Here is what that actually moves. Historically, a partner could report Azure consumption a deal drove — Partner Reported Azure Consumed Revenue — and have that recognized as co-sell impact. FY27 shifts recognition to verified Marketplace transactions instead. In Microsoft’s framing, “co-sell credit for Marketplace transactions continues to be recognized through Marketplace Billed Sales (MBS).” The credit now follows the transaction, not the report.


Microsoft describes this as announced direction with a stated rationale — “a more consistent, predictable, and auditable experience” — rather than a switch that flips overnight. Its own guidance to partners is operational: “Focus on Marketplace readiness and execution,” and where gaps prevent transacting through Marketplace, “work with your Partner Development Manager (PDM) on the path forward.” Treat the specifics as evolving, and check the[July 2026 Partner Center announcement](https://learn.microsoft.com/en-us/partner-center/announcements/2026-july) for the current wording before you plan around a date.


One more thing is coming. Microsoft announced Frontier Accelerate for Marketplace, a unified offering arriving in September 2026 that combines ISV Success, Marketplace Rewards, Azure IP co-sell, and certified software designations into one experience, with existing partners transitioning automatically at renewal. It is the packaging around the same Marketplace-first direction.


---


## **How does a seller operationalize Microsoft IP co-sell?**


You operationalize IP co-sell by making the Marketplace transaction, not the co-sell form, the center of the motion — and by keeping the co-sell record, the registration gate, and the transaction in one reconciled view. The FY27 shift rewards the team that can transact, and penalizes the team that can only report.


Four operating rules follow from everything above:


- **Get the solution co-sell ready and transactable first.** The co-sell-ready gate is upstream of every deal, and FY27 recognition runs on Marketplace billed sales. A product that co-sells but can’t transact through Marketplace is exposed under the new model. Listing and transacting on Azure Marketplace is covered on the[Azure Marketplace seller solution page](https://www.suger.io/solutions/microsoft-marketplace/) .
- **Qualify the account and the taxonomy at creation, not at close.** Microsoft-managed status, Solution Area, and Solution Play decide eligibility at the first fields. An unmanaged account can’t be fixed later without support.
- **Respect the clocks.** The 72-hour gap before *won* , the 14-day Microsoft response window, and terminal-state immutability are all silent failure modes. Batch-submitting at quarter end is how registrations get rejected.
- **Reconcile the co-sell record against the Marketplace transaction.** When credit follows MBS, the co-sell opportunity and the private offer that fulfills it have to resolve to the same deal — or the registration and the revenue never line up. Marketplace-native tooling closes that loop;[partner relationship management for ISVs](https://www.suger.io/prm/) covers the broader partner side of it.


Suger automates this motion across AWS, Microsoft, and Google Cloud — creating and syncing co-sell records with Partner Center, executing the Marketplace private offers that transact them, and keeping the two reconciled so a registered deal and its billed sale stay linked. That reconciliation is exactly the capability FY27 makes load-bearing.


---


## **Frequently asked questions**


**What is Microsoft IP co-sell?** Microsoft IP co-sell is a Partner Center deal type where a software company co-sells its own product with Microsoft using an Azure IP co-sell eligible solution. A Microsoft seller can engage the deal, and qualifying deals register for program credit.


**How do you qualify for Azure IP co-sell?** You need at least one co-sell-ready solution before creating any deal. Individual deals then require a Microsoft-managed customer account and a mandatory Solution Area and Solution Play, all set at the first form fields rather than at close.


**What is the minimum deal size for Microsoft IP co-sell deal registration?** Deal registration requires a deal value of at least USD 25,000, converted at Reuters monthly rates. It is one of six criteria, alongside a won status, an eligible solution, a partner-led or co-sell deal type, Microsoft’s participation, and a Microsoft-managed account.


**Is PRACR being retired?** Microsoft has announced that in FY27, Partner Reported Azure Consumed Revenue no longer operates as a broad co-sell mechanism. Co-sell recognition shifts to verified Marketplace transactions, credited through Marketplace Billed Sales. Treat the specifics as announced direction and confirm current wording in Microsoft’s July 2026 announcement.


**How does the FY27 Marketplace-first change affect co-sell incentives?** Co-sell credit at scale now follows Marketplace transactions rather than partner-reported consumption. Microsoft’s guidance is to focus on Marketplace readiness and execution, and to work with a Partner Development Manager where gaps prevent transacting through Marketplace.


**What is Frontier Accelerate for Marketplace?** It is a unified Microsoft offering announced for September 2026 that combines ISV Success, Marketplace Rewards, Azure IP co-sell, and certified software designations into one experience. Existing partners transition automatically at renewal.


---


## **Takeaways**


- IP co-sell is the motion for an ISV’s own product, gated on an Azure IP co-sell eligible, co-sell-ready solution before any deal exists.
- Registration is all-or-nothing across six criteria — won status, eligible solution, partner-led or co-sell type, Microsoft’s participation, USD 25,000, and a Microsoft-managed account.
- Respect the 72-hour-before-won gap and the 14-day response window; terminal states can’t be undone.
- FY27 moves co-sell recognition from partner-reported consumption (PRACR) to verified Marketplace transactions, credited through Marketplace Billed Sales. Treat the specifics as announced direction and confirm against Microsoft’s July 2026 announcement.
- Operationalize it by making the solution transactable, qualifying accounts at creation, and reconciling every co-sell record against the Marketplace transaction that fulfills it.


---


The FY27 direction rewards sellers who can transact co-sell through Marketplace, not just report it. See how Suger runs listing, private offers, and co-sell on the[Azure Marketplace seller solution](https://www.suger.io/solutions/microsoft-marketplace/) , and how it fits the wider partner motion on the[Suger PRM page](https://www.suger.io/prm/) .
