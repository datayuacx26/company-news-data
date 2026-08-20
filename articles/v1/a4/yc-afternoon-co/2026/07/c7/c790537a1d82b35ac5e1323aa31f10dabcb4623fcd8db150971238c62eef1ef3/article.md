---
schema_version: "1.0.0"
document_id: "c790537a1d82b35ac5e1323aa31f10dabcb4623fcd8db150971238c62eef1ef3"
company_key: "yc-afternoon-co"
company: "Afternoon.co"
source_id: "yc-afternoon-co-news-import-d204747fe0e0"
canonical_url: "https://www.afternoon.co/blog/aws-sales-tax-guide"
published_at: null
first_seen_at: "2026-07-21T20:47:37.461730+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:d61a2a98a384ca65cc85d964143480ba2421e369e6caee4a038bad9018d33cab"
---

# AWS Marketplace Sales Tax Guide for SaaS Sellers (2026)

If you sell SaaS, APIs, or software through the AWS Marketplace, understanding how sales tax works is critical.


This guide covers everything U.S. SaaS founders and finance teams need to know about AWS Marketplace sales tax in 2026:


- When AWS collects and remits U.S. sales tax on your behalf
- How Product Tax Codes (PTCs) determine if your product is taxable
- When you might still need to register or file your own returns
- What records to keep for compliance and audits


If you’re scaling your SaaS on AWS and want your sales tax registrations, filings, and bookkeeping handled automatically,[Afternoon.co](https://www.afternoon.co/) helps startups stay compliant in every state without adding operational overhead.


---


### Quick Answer: Does AWS Collect Sales Tax for SaaS?


U.S. Sales Tax:


- AWS acts as a marketplace facilitator in nearly all U.S. states
- AWS automatically collects and remits the correct sales tax based on customer location
- You receive your payout after AWS withholds and remits the tax
- Some states may still require sellers to register or file $0 sales tax returns once nexus thresholds are met


International VAT/GST:


- AWS may collect VAT or GST on non-U.S. transactions, depending on the buyer’s location and product type
- Sellers remain responsible for reviewing local tax laws outside AWS coverage


Last updated Jan 2026, based on official AWS Marketplace documentation.


---


Talk to a sales tax expert


Get guidance tailored to your business


---


## How AWS Marketplace Sales Tax Works


### AWS as a Marketplace Facilitator


Under U.S. state marketplace facilitator laws, AWS Marketplace, not the individual seller, is generally responsible for collecting and remitting sales tax.
This means AWS calculates the correct tax at checkout, charges it to the customer, and sends it to the relevant tax authority.


From AWS:


> “When required by law, AWS calculates, collects, and remits U.S. sales tax for sales made through AWS Marketplace.”
> ([AWS U.S. Sales Tax Help](https://aws.amazon.com/tax-help/united-states/) )


AWS uses automated tax software to determine which transactions are taxable and applies rates by state and locality.


#### What AWS Handles


- Collects the correct sales tax based on buyer location
- Remits the tax directly to each state’s Department of Revenue
- Reports the transaction and tax amount internally


#### What You Receive as a Seller


- A disbursement that already excludes any collected sales tax
- Reports showing gross sales, AWS fees, and customer tax amounts


Reference:[AWS Knowledge Center – How Tax Is Calculated](https://repost.aws/questions/QUjuYvPnueQoiBNmZIM_KfVw/how-is-tax-calculated-in-aws)


---


## Your Sales Tax Responsibilities as an AWS Seller


Even though AWS handles collection and remittance, sellers still have compliance tasks, especially around registration, documentation, and monitoring nexus.


### 1. Register and Provide Tax Documentation


AWS requires valid U.S. tax information to pay out revenue correctly.


- Submit your W-9 (for U.S. entities) or W-8BEN-E (for non-U.S. entities)
- Verify that your legal name and EIN match IRS records exactly
- Reference:[AWS Marketplace Seller Guide](https://docs.aws.amazon.com/marketplace/latest/userguide/user-guide-for-sellers.html)


### 2. Monitor Economic Nexus Thresholds


Your total AWS Marketplace sales count toward state economic nexus thresholds (commonly $100,000 in annual sales or 200 transactions).


If you cross those limits:


- Some states require you to register and file returns, even if AWS collected all sales tax
- Other states fully exempt marketplace-only sellers


Action item: Review each state’s[marketplace facilitator rules](https://www.streamlinedsalestax.org/for-businesses/marketplace-facilitator) to confirm your filing obligations.


### 3. Choose the Correct Product Tax Code (PTC)


AWS determines taxability using Product Tax Codes. The right PTC ensures AWS collects the correct amount of tax for your SaaS or software listing.


From the official[AWS Product Tax Code list (PDF)](https://s3.amazonaws.com/aws-mp-seller-tax-terms/Product_Tax_Codes_for_US_Sales_Tax.pdf) :


PTC Code Description Typical Treatment


A_SaaS Software as a Service Usually taxable in most states


A_SoftDL Downloadable software Taxable in almost all states


A_API API-based software service Varies by state


A_Digital Digital goods or media Varies


A_Consult Professional or consulting services Usually non-taxable


Choosing the wrong code can cause over- or under-collection of sales tax. Review your PTCs regularly as your product evolves.


---


## When AWS Does Not Collect Sales Tax


There are a few situations where AWS does not collect and remit tax, leaving responsibility with the seller:


### 1. Direct (Non-Marketplace) Sales


If you also sell SaaS directly through your own website or API billing system, AWS has no role in collecting or remitting that tax.


### 2. International Transactions


AWS may not collect VAT or GST in every country. Check local rules before billing non-U.S. customers through AWS.


### 3. Off-Platform Services


Consulting, integration work, or support sold separately from the Marketplace listing are your responsibility to report and remit.


---


## How AWS Calculates Sales Tax


AWS applies tax based on:


- The customer’s billing or usage address
- The Product Tax Code assigned to your product
- State and local tax rates pulled from certified tax software


Official reference:[AWS Knowledge Center – How Tax Is Calculated](https://repost.aws/questions/QUjuYvPnueQoiBNmZIM_KfVw/how-is-tax-calculated-in-aws)


AWS automatically updates rates as states change their tax laws, so sellers do not need to manage rate tables or remittances manually.


---


## Recordkeeping and Compliance Documentation


Good recordkeeping protects your business in case of audit or reconciliation.


Recommended records to maintain:


- Monthly disbursement and payout reports from AWS
- Product Tax Code selections and documentation
- Detailed transaction reports showing tax collected
- Copies of AWS tax invoices for business buyers


Reference:[AWS Marketplace Seller Tax Help](https://aws.amazon.com/tax-help/marketplace-sellers/)


---


## AWS Marketplace Sales Tax Compliance Checklist


### Initial Setup


- Complete W-9 or W-8BEN-E tax forms
- Assign accurate Product Tax Codes for all listings
- Confirm business addresses match IRS or state records
- Review annual state nexus thresholds


### Ongoing Compliance


- Monitor[AWS U.S. Tax Help Center](https://aws.amazon.com/tax-help/united-states/) for coverage updates
- Reassess Product Tax Codes when product functionality changes
- Download and store monthly tax and disbursement reports
- Reevaluate registration requirements each year


### Recordkeeping


- Retain AWS invoices, reports, and correspondence for at least seven years
- Keep internal documentation on how tax classifications were chosen


---


## Frequently Asked Questions About AWS Marketplace Sales Tax


### Does AWS collect sales tax for Marketplace sales?


Yes. AWS acts as the marketplace facilitator and handles collection and remittance in states that require it.


### Do I still need to register in every state?


Not always. Many states exempt marketplace-only sellers, but some still require registration once you pass nexus thresholds.


### How do I know if my SaaS is taxable?


Check your Product Tax Code against AWS’s official list and verify how your product is categorized by each state’s tax authority.


### Does AWS collect international VAT or GST?


AWS collects VAT/GST in certain regions but not everywhere. Sellers remain responsible for local compliance in uncovered countries.


### Where can I find official AWS sales tax documentation?


See AWS’s published resources below for the most current guidance.


---


## Official AWS Documentation


- [AWS U.S. Tax Help Center](https://aws.amazon.com/tax-help/united-states/)
- [AWS Knowledge Center: U.S. Sales Tax Overview](https://repost.aws/knowledge-center/aws-us-sales-tax)
- [AWS Marketplace Seller Tax Help](https://aws.amazon.com/tax-help/marketplace-sellers/)
- [AWS Marketplace Seller Guide](https://docs.aws.amazon.com/marketplace/latest/userguide/user-guide-for-sellers.html)
- [Product Tax Codes for U.S. Sales Tax (PDF)](https://s3.amazonaws.com/aws-mp-seller-tax-terms/Product_Tax_Codes_for_US_Sales_Tax.pdf)
- [AWS Knowledge Center: How Tax Is Calculated](https://repost.aws/questions/QUjuYvPnueQoiBNmZIM_KfVw/how-is-tax-calculated-in-aws)
