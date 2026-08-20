---
schema_version: "1.0.0"
document_id: "3c42fbe1056661000d36f58751b3dac4f360086656e8bc08b21a14d530e6fecf"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/how-to-sell-on-azure-marketplace-isv-guide/"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T01:55:10.648301+00:00"
fetched_at: "2026-08-06T01:55:12.500095+00:00"
content_hash: "sha256:1e98a15ec64fe48fb8c585fdb0c2e96a974cb551004c7992823fef12c565401b"
---

# How to Sell on Azure Marketplace: An ISV Guide

*You sell on Azure Marketplace by enrolling in Microsoft Partner Center, publishing a transactable offer — SaaS, VM, container or managed application — and then reaching enterprise buyers through private offers and Microsoft co-sell. The listing is the entry ticket; the private offer and the buyer’s committed spend are where the revenue closes.*


---


For most independent software vendors (ISVs), the reason to sell on Azure Marketplace has nothing to do with Microsoft’s storefront and everything to do with the buyer’s budget. Enterprise customers who run on Azure carry committed cloud spend, and marketplace purchases draw down against that commitment — turning a procurement conversation from “find new budget” into “spend budget I’ve already committed.”


That single mechanic reshapes the deal. Marketplace purchases route through a billing relationship the customer’s finance and procurement teams already trust, which compresses the paperwork that stalls a typical enterprise contract. For the seller, it means faster legal cycles, a co-sell path into Microsoft’s field, and access to buyers who would rather transact where their spend already lives.


Here’s how the pieces fit together, in the order an ISV meets them.


---


## **What is Azure Marketplace, and how does it differ from AppSource?**


Azure Marketplace is Microsoft’s commercial storefront for cloud infrastructure and platform software — the place enterprise buyers purchase and provision the technical building blocks they run on Azure, from virtual machine images to SaaS applications, billed through their Microsoft account.


Microsoft runs two storefronts from one back end. Azure Marketplace targets IT and developer buyers purchasing infrastructure and developer tooling; Microsoft AppSource targets business buyers purchasing line-of-business applications and add-ins for products like Microsoft 365 and Dynamics. You publish both from the same Partner Center account, and a single offer can appear in whichever storefront fits its audience. For most infrastructure-adjacent ISVs, Azure Marketplace is the primary surface, because that’s where cloud-committed spend gets deployed.


---


## **Who can sell on Azure Marketplace? Eligibility and prerequisites**


Any software company can sell on Azure Marketplace once it holds a Microsoft Partner Center account, has completed the account verification Microsoft requires, and has a product that meets the technical and policy requirements for at least one offer type. There is no revenue threshold to start.


Before you can publish a transactable offer, you need a few things in place:


- **A Partner Center account** enrolled in the commercial marketplace program, which is the console where every offer, plan and payout lives.
- **A verified organization identity** , including a Partner ID (still commonly called the MPN ID, from the program now known as the Microsoft AI Cloud Partner Program) that ties your offers to your company.
- **Tax and banking (payout) profiles** completed and validated, so Microsoft can disburse your earnings and handle withholding correctly.
- **A product that satisfies the offer-type requirements** — a working SaaS endpoint, a published VM image, a container image, or a packaged managed application, depending on what you list.


Get the tax and payout profiles moving early. They involve validation steps outside your control, and an unpublished offer waiting on a banking profile is the most avoidable delay in the whole process.


---


## **What are the Azure Marketplace listing and offer types?**


Azure Marketplace supports several offer types, and your choice is dictated by how your product is delivered and how you want to charge. The first decision is whether the offer is *list-only* or *transactable* : a list-only offer sends the buyer to your own site to purchase (contact-me or bring-your-own-license), while a transactable offer is billed by Microsoft and — critically — draws down committed spend.


Offer type Best for Transactable?


**SaaS** Cloud-hosted software billed per seat, per unit or by usage Yes — flat-rate, per-user or metered billing


**Azure Virtual Machine** Software delivered as a VM image the buyer deploys Yes — hourly or BYOL pricing


**Container** Software packaged as container images for Azure Kubernetes Service Yes, for supported billing models


**Azure Application (managed app / solution template)** Deployed infrastructure you or the customer manages in their subscription Yes, for managed applications


**List / contact-me (BYOL)** Lead generation where the transaction closes off-marketplace No — no Microsoft billing or drawdown


For most ISVs the answer is a transactable SaaS offer, because it is the fastest path to Microsoft-billed revenue and MACC drawdown without shipping an image. Whatever you pick, keep a single source of truth for the plans and pricing behind the listing —[managing marketplace listings](https://www.suger.io/platform/product-listing/) across offer types and clouds is where drift creeps in.


---


## **What does the listing process in Partner Center look like?**


Publishing a transactable offer in Partner Center is a staged workflow: you create the offer, define its plans and pricing, provide the marketing and legal content, connect the technical fulfillment, and submit for Microsoft’s certification review before the offer goes live.


In practice you’ll move through offer setup (name, type, selling options), one or more plans (each with its own pricing model and audience), listing content (description, logos, screenshots, support and privacy links), and a technical configuration that differs by type — a landing-page and webhook URL for SaaS, an image reference for VM and container. Microsoft then reviews the offer against its certification policies before it can be purchased. Treat the first submission as a draft: preview it, test the purchase flow end to end, and expect at least one round of certification feedback before publish.


---


## **How do private offers and private plans work?**


A private offer is a custom deal — negotiated price, custom terms, specific quantities — extended to a single named customer rather than the public listing. It is how enterprise deals actually close on Azure Marketplace, and it still draws down the buyer’s committed spend the same way a public purchase does.


Private offers let you agree on a price the public plan never shows, set a custom start date and duration, and structure multi-year or ramped commitments. A private plan does something adjacent: it makes a specific plan visible only to nominated customers. Together they let you run enterprise pricing through the marketplace without exposing it publicly. This is the mechanic most first-time sellers underuse — the public listing gets you found, but[private offers](https://www.suger.io/platform/private-offers/) are where negotiated enterprise revenue lands.


---


## **How does a MACC make the marketplace the preferred buying path?**


A Microsoft Azure Consumption Commitment (MACC) is an enterprise customer’s contractual pledge to spend a certain amount on Azure over time — and eligible marketplace purchases count toward it. That is the single biggest reason an Azure buyer will prefer to purchase your software through the marketplace rather than direct.


When your transactable offer draws down a customer’s MACC, buying you helps them meet a commitment they’ve already made and are motivated to fulfill. The budget objection disappears, procurement moves faster, and your deal competes for spend that is already earmarked for Azure. For any ISV selling to Azure-committed enterprises, understanding[how a MACC drives Azure committed spend](https://www.suger.io/resources/blog/what-is-a-macc-azure-committed-spend-explained/) is the difference between a listing that collects dust and one that closes seven-figure deals — so make MACC eligibility part of every enterprise conversation.


---


## **How do payouts, fees and disbursement work?**


Microsoft bills the customer for your transactable offer, deducts a marketplace fee, and disburses the remainder to the bank account on your payout profile on a recurring schedule. You never invoice the buyer or chase collections — Microsoft is the merchant of record for the transaction.


A few qualitative realities to plan around: the marketplace fee is a percentage Microsoft has reduced over time and can vary by program, so confirm the current rate rather than assuming one; payouts follow a defined cycle after the customer is billed, so there’s a lag between close and cash; and reconciling marketplace payouts against your own billing systems is real operational work, especially once you’re live on more than one cloud. Clean payout and tax profiles up front are what keep that reconciliation from becoming a monthly fire drill.


---


## **How do you co-sell with Microsoft?**


You co-sell with Microsoft by registering your qualified opportunities in Partner Center’s co-sell tooling, which shares the deal with Microsoft’s field sellers and, for IP co-sell-ready solutions, aligns their incentives with closing it. Marketplace transactions and co-sell registrations reinforce each other: a deal that transacts on the marketplace and is registered for co-sell is visible to the Microsoft account team working the same customer.


Co-sell is where marketplace selling stops being a self-serve listing and becomes a field motion. It rewards ISVs that keep opportunity data flowing into Partner Center and treat Microsoft sellers as partners on the deal. Suger connects to[three hyperscalers](https://www.suger.io/solutions/microsoft-marketplace/) for exactly this — so co-sell records, private offers and marketplace transactions stay in one system instead of drifting across portals.


---


## **Frequently asked questions**


**How do you start selling on Azure Marketplace?** Enroll in Microsoft Partner Center’s commercial marketplace program, verify your organization, complete tax and payout profiles, then create and publish a transactable offer. Once Microsoft’s certification review passes, buyers can purchase your listing and draw it against their committed spend.


**What is the difference between Azure Marketplace and Microsoft AppSource?** Both run on Partner Center. Azure Marketplace serves IT and developer buyers purchasing infrastructure and cloud software; AppSource serves business buyers purchasing line-of-business apps for products like Microsoft 365 and Dynamics. A single offer can appear in whichever storefront fits its audience.


**Do I need to be a Microsoft partner to sell?** Yes. You need a Microsoft Partner Center account enrolled in the commercial marketplace program, a verified organization with a Partner ID, and completed tax and banking profiles before you can publish a transactable offer. There is no minimum revenue to begin.


**What is a private offer on Azure Marketplace?** A private offer is a custom deal — negotiated price, terms and quantities — extended to one named customer instead of the public listing. It’s how most enterprise deals close on the marketplace, and it still draws down the buyer’s committed Azure spend.


**Does buying through Azure Marketplace count toward a customer’s Azure commitment?** Yes, for eligible transactable offers. Purchases draw down the customer’s Microsoft Azure Consumption Commitment (MACC), which is why Azure-committed buyers often prefer marketplace transactions — they spend budget they’ve already committed rather than finding new budget.


**How does Microsoft pay ISVs for marketplace sales?** Microsoft bills the customer as merchant of record, deducts a marketplace fee, and disburses the balance to your payout bank account on a recurring cycle. You don’t invoice buyers or manage collections — but you do reconcile payouts against your own billing.


---


## **Takeaways**


- The listing gets you discoverable; private offers and MACC drawdown are where enterprise revenue actually closes. Plan for both from day one.
- Complete tax and payout profiles before you build the offer — validation happens outside your control and is the most common self-inflicted delay.
- Choose your offer type from how you deliver and bill. For most ISVs, a transactable SaaS offer is the fastest path to Microsoft-billed revenue.
- Make MACC eligibility part of every enterprise conversation. Drawing down committed spend removes the budget objection before it’s raised.
- Treat co-sell as a field motion, not a checkbox. Keeping opportunity data in Partner Center is what puts Microsoft sellers on your deals.


---


Suger runs Azure Marketplace listings, private offers, metering and Microsoft co-sell from one platform, and reconciles every payout back to your billing. See how it works on[Suger for Microsoft Marketplace](https://www.suger.io/solutions/microsoft-marketplace/) — and if AWS is next,[the equivalent guide for AWS Marketplace](https://www.suger.io/resources/blog/aws-marketplace-for-sellers-complete-guide/) covers the same ground for sellers.
