---
schema_version: "1.0.0"
document_id: "043804905c59ca3e3efa722c0fcf909047caf90397d9de5290fe48d4cea28222"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/dynamics-365-marketplace-integration/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T08:10:57.347630+00:00"
fetched_at: "2026-08-11T08:11:00.150542+00:00"
content_hash: "sha256:62014116f04d5b32c6fa1f81c17a784d30c726184ba7730a22f92f9811001a2a"
---

# Connect Dynamics 365 to Your Marketplace Pipeline

*A Dynamics 365 marketplace integration connects your CRM to the systems that hold marketplace offers, entitlements, and revenue — so a private offer, its acceptance, and the money that follows appear against the opportunity your sellers already work in, rather than in a portal they do not open.*


---


There is a category of ISV that keeps hitting the same wall: Microsoft-first companies whose product runs on Azure, whose enterprise buyers purchase through Microsoft Marketplace, and whose CRM is Dynamics 365.


Everything in the cloud go-to-market tooling market assumes Salesforce, or occasionally HubSpot. So the Microsoft-first company — the one whose marketplace motion is arguably the most native — ends up with the least connected pipeline. Marketplace state sits in Partner Center. CRM state sits in Dataverse. A human copies between them.


This post is about closing that gap: what the connection actually involves, what moves in which direction, and which objects to map first.


---


## **What does a Dynamics 365 marketplace integration do?**


**It puts marketplace facts on the CRM record.** Specifically: which of your accounts have a marketplace agreement, which offers are outstanding, when an entitlement starts and ends, and what has been billed against it.


That matters because of who is asking. A seller working a renewal needs to know whether the customer bought through Microsoft Marketplace, because that changes the renewal mechanics entirely. A partner manager needs to know whether a co-sell referral exists. A finance lead needs to know whether an opportunity marked closed-won has an entitlement behind it. None of those people should be opening Partner Center.


The integration is not a replacement for the marketplace console. It is the join that lets your CRM answer marketplace questions.


---


## **What Suger’s Dynamics 365 integration supports**


Suger documents Dynamics 365 as a first-class CRM integration alongside Salesforce and HubSpot: “Sync sales, customer, and operational data between Dynamics 365 and Suger.”


The capability matrix is worth reading carefully before you design anything, because it is narrower than the Salesforce connector:


Capability Org level User level


Read data Supported Supported


Write data Supported Supported


Scheduled sync Not supported Not supported


Webhooks Not supported Not supported


Suger states this plainly: “Dynamics 365 integration does not support sync or webhooks at this time.”


**That single line should shape your design.** Read and write both work, so you can pull CRM records into Suger and push marketplace state back. What you do not get is a managed background sync or a push notification when something changes in Dataverse. Movement is initiated — through workflows, jobs, or API calls — not ambient.


Practically, that means:


- **Choose a cadence deliberately.** Nightly is usually right for entitlements and revenue; faster for offers in flight.
- **Do not build anything that assumes near-real-time CRM change detection.** If a Dynamics field change must trigger a marketplace action within seconds, that trigger has to come from the Dynamics side.
- **Design for idempotency.** Pull-based movement means the same record will be processed more than once. Every write needs a stable external key.


---


## **Setting it up: two auth paths**


Suger documents two ways to connect, and the choice is not cosmetic — it determines whose permissions the integration inherits.


**Org level, via Azure AD credentials.** You need the Dynamics 365 instance URL, the tenant ID from Azure AD, an application user in Dynamics 365 linked to your Azure AD app registration, and security roles assigned to that application user through the Power Platform Admin Center — with the System Administrator role required.


**User level, via OAuth 2.0.** You need the instance URL, Microsoft credentials for the individual, and that person’s consent to grant Suger data access.


Use org level for anything that must keep running when a person changes role. Use user level only where the action genuinely should be scoped to one individual’s permissions.


One constraint to plan around, because it has no workaround: **“For security purposes, editing an existing Microsoft Dynamics 365 integration is not allowed.”** Get the instance URL, tenant, and application user right the first time. Changing them means creating a new integration, which means anything keyed to the old one has to be repointed.


Suger’s published integration timeline puts CRM connectors in the same order of magnitude as each other — the Salesforce connector at “approx. 1 hour,” the HubSpot connector at “approx. 10 minutes.” Budget an afternoon for Dynamics, most of it spent in the Power Platform Admin Center rather than in Suger.


---


## **Which objects to map first**


Do not try to mirror everything. Map the objects that answer a question somebody is currently asking a human.


Suger’s own data model gives you the vocabulary. The datasets available to move include` marketplace_offer` (“data on marketplace offers, including public and private offers”),` marketplace_entitlement` (“customer entitlements for marketplace products or services”),` billing_revenue_record` (“detailed records of revenue generated through various billing channels”),` identity_buyer` (“buyer profile data, such as company name, industry, contacts and locations”),` marketplace_product` , and` cosell_referral` (“data on cosell referrals”).


A sensible first pass:


Object Direction Lands on Answers


` identity_buyer` Marketplace → CRM Account Is this account a marketplace buyer at all?


` marketplace_offer` Marketplace → CRM Opportunity Is an offer outstanding, and for how much?


` marketplace_entitlement` Marketplace → CRM Account / Opportunity When does this contract start and end?


` billing_revenue_record` Marketplace → CRM Account What has actually been billed?


` cosell_referral` Both Opportunity Does a Microsoft co-sell record exist for this deal?


Opportunity ID CRM → Marketplace Offer, referral Which CRM deal is this offer for?


The last row is the one teams skip and later regret. Microsoft Partner Center carries an optional **CRM ID** field on a co-sell deal — “ID of the opportunity in your CRM system (used as a tag for your tracking purposes).” Populating it with the Dynamics opportunity ID is the cheapest reconciliation you will ever build, and it costs nothing at creation time. Leave it blank and every later attempt to tie Partner Center records back to Dynamics is fuzzy matching on customer name.


### Two things not to sync


**Account ownership.** Decide which system is authoritative for the account record and leave the other one read-only on that field. Bidirectional ownership sync produces update loops and a lot of confused audit history.


**Anything you would not want a seller to act on.** Offer state before it is real, provisional pricing, internal notes. The CRM is the most widely read system you own.


---


## **The Microsoft-first advantages worth using**


If you are on Dynamics *and* selling through Microsoft Marketplace, two things line up in your favour.


**Identity is already common.** Partner Center, Azure AD, and Dynamics all sit inside the same tenant. The org-level auth path is an app registration you probably already know how to create, and the admin who owns Power Platform is the admin who owns Partner Center access.


**Committed spend is visible earlier.** Partner Center shows whether a selected customer account has a Microsoft Azure Consumption Commitment. Getting that flag onto the Dynamics account record turns a deal-desk question into a field a seller can see while qualifying —[what a MACC is](https://www.suger.io/resources/blog/what-is-a-macc-azure-committed-spend-explained/) covers why that changes the deal shape.


The rest of the Microsoft motion — listing types, Partner Center, private offers versus private plans — is covered in[how to sell on Azure Marketplace](https://www.suger.io/resources/blog/how-to-sell-on-azure-marketplace-isv-guide/) and[Azure private offers vs private plans](https://www.suger.io/resources/blog/azure-private-offers-vs-private-plans/) .


---


## **Frequently asked questions**


**Does Suger integrate with Microsoft Dynamics 365?** Yes. Suger documents Dynamics 365 as a CRM integration alongside Salesforce and HubSpot, supporting reads and writes at both organization level and user level.


**Does the Dynamics 365 integration support real-time sync?** No. Suger states that the Dynamics 365 integration does not support scheduled sync or webhooks at this time. Reads and writes both work, but movement is initiated rather than ambient, so pick a cadence.


**What do I need to connect Dynamics 365?** For org-level access: the instance URL, your Azure AD tenant ID, an application user in Dynamics 365 linked to an app registration, and security roles assigned through the Power Platform Admin Center. For user-level access: the instance URL and Microsoft credentials with consent.


**Can I edit a Dynamics 365 integration after creating it?** No. Suger does not allow editing an existing Microsoft Dynamics 365 integration, for security reasons. Changing the instance, tenant, or application user means creating a new integration.


**Which records should I sync first?** Buyers, offers, and entitlements onto the account and opportunity, plus your CRM opportunity ID pushed back onto marketplace and co-sell records. That last one is what makes later reconciliation possible.


**Does Microsoft Partner Center hold a CRM reference?** Yes. A co-sell deal in Partner Center has an optional CRM ID field for the opportunity ID in your own system. It is optional, frequently left blank, and the cheapest reconciliation key available.


---


## **Takeaways**


- Dynamics-first ISVs are the least served by marketplace tooling and often the most native to the Microsoft motion. The gap is connection, not capability.
- Suger’s Dynamics 365 integration reads and writes at org and user level; it does not offer scheduled sync or webhooks, so movement is initiated and cadence is a design decision.
- Org-level auth needs an app registration, an application user, and security roles from the Power Platform Admin Center. User-level auth is OAuth against an individual.
- The integration cannot be edited after creation. Get the instance, tenant, and application user right the first time.
- Map buyers, offers, entitlements, and revenue into the CRM; push your opportunity ID back out.
- Populate Partner Center’s optional CRM ID field. It is the difference between reconciliation and name matching.


---


Your CRM should not be the last system to learn that a customer bought. See how[Suger’s integrations](https://www.suger.io/platform/integrations/) connect marketplace offers, entitlements, and billing to the CRM your sellers already work in — including Dynamics 365, not only Salesforce.
