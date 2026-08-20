---
schema_version: "1.0.0"
document_id: "3145e6039e997807b8aabeae0a5ab9830ed9637c42f9a8a32397bdc2ad06fb9b"
company_key: "yc-middesk"
company: "Middesk"
source_id: "yc-middesk-news-import-ab19352e1f86"
canonical_url: "https://www.middesk.com/blog/how-to-design-an-smb-underwriting-stack"
published_at: "2026-06-18T13:10:43.358+00:00"
first_seen_at: "2026-07-24T11:25:42.522750+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:193f81effd95eca34b9bd0df3afc8af98a05ffb4ee28eb48fe93c55561b81c93"
---

# How to design an SMB underwriting stack

01


## Overview


Middesk is the underwriting intelligence layer between application and credit decision for underwriting small business borrowers. The platform consolidates business identity, ownership, liens, bankruptcies, and person-level risk checks into one real-time file. Evaluating the right tools comes down to four dimensions: data source coverage for entity verification, lien and bankruptcy data pulled from primary filing systems, person-level risk checks on owners and guarantors, and how underwriters see all of that. Middesk Underwriting Suite offers a unified report where they can see everything in one place.


**In brief**


- The best SMB underwriting tools cover the business, its liabilities, and the people behind it.


- Most lenders run separate vendors to reach one decision which creates data gaps, delays, and manual review.


- Middesk's Underwriting Suite consolidates all four layers into one decision-ready file.


The SMB lenders growing fastest right now aren't proportionally increasing their underwriting headcount. They're building infrastructure that produces a complete credit file before an underwriter touches a case. For every lender that's figured this out, there are many more still stuck with highly manual systems: pulling business identity from one system, lien data from a second, bankruptcy records from a third, person-level checks in another. Even if each point solution for data is returning good results, the manual effort prevents scale and growth.


There’s more at play than just the efficiency cost: the potential for risk. When data assembly is manual, decisions can be made from incomplete data or missed due to human error. For example, some files go to a credit decision with a person-level lien search while others don’t. A sole proprietor's personal UCC liens that would have changed the decision never gets reviewed. A tax lien filed two weeks before origination isn't in the credit file because the lien vendor runs monthly batch refreshes, not real-time orders. All of these realistic scenarios expose your organization to risk when you’ve onboarded and lended to a business.


Stacks that look comparable in a vendor evaluation can diverge significantly in production, especially when the business types and edge cases in a real SMB portfolio start hitting the system. Four data categories are required to underwrite an SMB borrower correctly. Here's what separates the vendors in each.


02


## What’s the foundation for every commercial underwriting decision?


Business identity must be verified before credit decisions begin. Surprisingly, this isn’t the standard for all underwriting solutions. At Middesk, we believe that accurate customer information means higher quality results and more accurate decisions. If the actual business identity verification is not done correctly, all other underwriting decisions are jeopardized.


One meaningful way this fails is in correctly resolving different entity types. SMB lenders are often working with recently formed businesses, a large percentage of which are LLCs or sole-proprietors. If the entity records the lender checks against are stale, miscategorized, or incomplete, they may automatically turn away good potential business customers. Some KYB providers claim to verify businesses Secretary of State records across all 50 states, but often fail to verify most of your SMB borrowers because they return incomplete data due to their slow refresh cadence, or don’t recognize edge-cases like sole-proprietors.


Business identity verification also anchors the SMB underwriting check on what a business is and its operating history. Sometimes even the history of a business’s state registrations can tell you if this is a business you can trust. The[Middesk Business Timeline](https://www.middesk.com/product-releases/business-timeline-detect-fraud-through-business-history) feature highlights the velocity of an entity’s changes, signaling if they are a regular company, or one that might have just been bought off a shelf and reactivated.


When evaluating coverage for core business identity verification, dig beyond which states a provider includes, and into how they cover them:


- What entity types are verified against primary government sources?


- Are sole proprietors returned with meaningful identity data, or routed to "unable to verify"?


- Are foreign-qualified entities tracked alongside domestic registrations, and can you see when a business recently changed its primary state of formation?


- What percentage of your borrower population returns an "unable to verify" result? What entity types are concentrated in that group?


- Do you return the history of a business entity’s changes in a timeline feature?


03


## How to get a real picture of lien, bankruptcy, and judgment data for SMB credit risk


Once the business is verified, the next step in the underwriting process is to check if the business has any liabilities. Data like UCC liens, federal and state tax liens, bankruptcy filings, civil judgments, and litigation history are public records that speak directly to creditworthiness, but most of them live outside what consumer credit bureaus aggregate.


Lenders in this space traditionally used business credit bureaus to evaluate their buyers. Vendors in this category range from business credit bureaus (Dun & Bradstreet, Experian Business Credit, Equifax Small Business) to specialist lien data providers (Cobalt Intelligence, CT Corporation). Bureau products offer scoring models and familiar workflows for credit teams doing manual research, but the underlying data is almost always stale. A lien filed last week may not appear in the report your underwriter looks at today, representing a huge risk for your business. In contrast, specialist lien providers source more directly from state and federal filing systems but typically carry narrower state coverage and they won’t be tied to an authoritative business identity record. Both options leave your team manually putting the pieces together. The[Middesk Underwriting Suite](https://www.middesk.com/products/assess) pulls fresh lien, bankruptcy, and judgment data from primary sources across all 50 states. Everything is integrated in a single business profile along with entity verification and ownership data. Lenders work from one current credit picture rather than separate records that need to be reconciled before a decision can be made. Middesk also understands that filing liens is an important part of your SMB underwriting stack, and the platform has a[2-click lien filing](https://docs.middesk.com/assess-risk/file-lien) solution approved loans.


Ask providers directly:


- What is the update latency for lien data?


- Is it a real-time pull from the filing system at the time of the order, or a batch refresh on a weekly or monthly cycle?


04


## What person-level risk exists in business underwriting?


In SMB lending, the person and the business are often inseparable, but the data on each exists in different tech stacks. A business entity may carry clean credit signals while the people behind it carry personal UCC liens, prior bankruptcies, or outstanding judgments that the entity search or standard underwriting tools never surfaces.


Middesk's People Lien Search runs UCC lien checks on submitted owners, guarantors, and sole proprietors in the same API call as the business verification and credit-risk orders. The results arrive in the same file, so underwriters see the complete picture without switching systems or reconciling across vendor reports. Ask any vendor in this category where person-level lien checks happen in their workflow. If the answer is "that's a separate integration" or "we handle that through a manual search portal," the gap is in your credit file.


Going deeper, our proprietary entity graph connects businesses through shared people, addresses, and business relationships that shows customers how a business fits into a broader network.


05


## Is a consolidated underwriting workflow faster?


It’s common for SMB underwriting architecture to be an accumulation of various tools. The result is four or five vendors, four or five data models, and at least 30 minutes of manual assembly per application to evaluate a file.


That manual aggregation and review time is where operational cost and risk concentrate.


A consolidated underwriting stack is better than a patchwork of point solutions because underwriters spend less time chasing information and more time applying judgment. This helps lenders move faster on strong borrowers with confidence and without sacrificing the context needed to catch risk. The result is an underwriting operation that scales growth and protects the portfolio at the same time.
