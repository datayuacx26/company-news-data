---
schema_version: "1.0.0"
document_id: "652145df55bd3a95852bcc769bfabb62d73c9d385603c2f75184480086ebf01a"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/marketplace-data-in-salesforce/"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-15T09:12:25.375763+00:00"
fetched_at: "2026-08-15T09:12:26.807099+00:00"
content_hash: "sha256:b96ea4b6f1e6b659d457f61a670693a1e7e6bb3104dba2d6cdf320d8c2239c63"
---

# Marketplace Data in Salesforce: What to Sync

*A marketplace-to-CRM sync works when each system owns a different set of facts. It fails when both systems own the same fact, because then there are two answers and no way to tell which is current.*


---


The request always arrives the same way: “can we see marketplace data in Salesforce?” And the answer is always yes, which is the problem — because the useful question is *which* marketplace data, and the default answer of “all of it” produces a CRM that contradicts the marketplace console.


Once a rep has seen two different numbers for the same customer, they stop trusting both.


---


## **Ownership is the design, everything else is plumbing**


The one decision that determines whether this works is which system is authoritative for which object.


The boundary that holds in practice:


Object Owned by Why


Account, Opportunity, Contact **Salesforce** Sales process lives here. Reps edit these daily; a sync that overwrites them will be turned off within a week


Offer **Marketplace** The offer is a contractual instrument the platform issued. Your CRM cannot amend it by editing a field


Entitlement **Marketplace** What the customer is actually allowed to use is a platform fact


Agreement state **Marketplace** Active, expired, cancelled — the platform decides, you observe


Co-sell referral **Shared** Created in the CRM, submitted to the cloud, and its status comes back


The Suger Salesforce integration is built on exactly this split:[Salesforce owns opportunity and account records, and Suger owns offers, entitlements, referrals and marketplace state](https://doc.suger.io/integrations/salesforce/) , surfaced in Salesforce as Suger-owned custom objects —` Suger__Offer__c` ,` Suger__Entitlement__c` and` Suger__Referral__c` .


The custom-object detail is not incidental. It is what stops marketplace state being written into standard fields that reps also edit, which is the specific mechanism by which these integrations create contradictions.


---


## **What not to sync**


Three categories look valuable and cost more than they return.


**Raw usage records.** A metered product can produce millions of usage rows a month. Salesforce is not a warehouse, storage limits are real, and no rep has ever needed a row-level usage record. Sync the aggregate that answers a sales question — consumption against commitment, trending up or down. Send the rows to a warehouse instead, which is a different pipeline with[different modelling requirements](https://www.suger.io/resources/blog/exporting-marketplace-data-to-your-warehouse/) .


**Every historical state transition.** An agreement that was amended four times does not need four records visible to sales. It needs current terms and a link to the history.


**Fields nobody has asked for.** The temptation with a configurable mapping is to map everything because it is cheap at setup time. It is not cheap later: every mapped field is a field that can disagree, and a support question about why it disagrees.


The discipline is to start with the fields a rep or a deal desk would act on, and add the rest when somebody names a decision that needs them.


---


## **Latency, and what to promise**


Sync cadence determines what you can honestly tell a rep.


The Suger integration documents[enrichment running every two hours and backfill every four](https://doc.suger.io/integrations/salesforce/) . That is appropriate for what the data is used for — deciding who to call, seeing where an offer stands — and inappropriate for anything a customer is waiting on.


The rule that avoids trouble: **never make a customer-facing promise on synced CRM data.** If a rep tells a customer “you’re provisioned, I can see it in Salesforce,” and the sync is two hours behind, the rep is wrong roughly two hours a day. Provisioning status should be answered from the system that grants it, which is one of the reasons entitlement changes are delivered as[events rather than reports](https://www.suger.io/resources/blog/marketplace-events-and-webhooks/) .


---


## **The mapping decisions that cause trouble later**


Two mapping choices are worth making deliberately, because both are painful to change once data has accumulated.


**How you match a marketplace customer to a Salesforce account.** The marketplace knows a customer identifier; Salesforce knows an account. These do not correspond one-to-one — a single company can buy from several cloud accounts, and a cloud account can belong to a reseller rather than the end customer. Matching on domain or company name works until it silently does not, and then revenue attaches to the wrong account.


Store the marketplace identifier on the account as a first-class field and treat the mapping as data, not as a guess made at sync time. When it is wrong, you want to fix one row rather than re-run a heuristic.


**What happens to a record when the marketplace disagrees.** If an offer’s value changes on the platform, does the CRM overwrite, flag, or ignore? Silent overwrite loses the rep’s context; silent ignore lets the CRM drift. Flagging is more work to build and the only option that surfaces the disagreement to somebody who can resolve it.


The general problem — two systems, one fact, no adjudicator — is the same one described in[Why Marketplace Numbers Never Match](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) .


---


## **Frequently asked questions**


**Which system should own marketplace offers and entitlements?** The marketplace. Offers are contractual instruments the platform issued and entitlements are what the customer is actually allowed to use. Salesforce should own accounts, opportunities and contacts.


**Why use custom objects rather than standard Salesforce fields?** Because marketplace state written into fields reps also edit is how the CRM starts contradicting the console. Suger surfaces offers, entitlements and referrals as its own custom objects.


**Should raw usage records sync into Salesforce?** No. A metered product can produce millions of rows a month and no rep acts on a row. Sync an aggregate that answers a sales question and send the detail to a warehouse.


**How fresh is synced marketplace data in a CRM?** It depends on the cadence. The Suger integration runs enrichment every two hours and backfill every four, which suits sales decisions but not anything a customer is waiting on.


**How should a marketplace customer be matched to a CRM account?** Store the marketplace customer identifier on the account as a real field. Matching on domain or company name fails silently when one company buys from several cloud accounts, or when a reseller is involved.


---


## **Takeaways**


- Decide ownership per object first. Everything else is plumbing.
- Keep marketplace state in its own objects, not in standard fields reps edit.
- Do not sync raw usage, full state history, or fields nobody has asked for.
- Sync cadence is a promise. Never answer a customer-facing question from data that is hours old.
- Store the marketplace customer identifier as real data rather than re-deriving the match on every sync.


Suger keeps offers, entitlements, agreements and co-sell referrals in sync between every marketplace it supports and Salesforce, with the ownership boundary above built in.[See how Suger handles integrations](https://www.suger.io/platform/integrations/) , read the[Salesforce integration docs](https://doc.suger.io/integrations/salesforce/) , or[talk to our team](https://www.suger.io/contact-us/) .
