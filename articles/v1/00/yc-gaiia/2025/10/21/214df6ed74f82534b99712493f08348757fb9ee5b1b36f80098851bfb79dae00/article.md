---
schema_version: "1.0.0"
document_id: "214df6ed74f82534b99712493f08348757fb9ee5b1b36f80098851bfb79dae00"
company_key: "yc-gaiia"
company: "Gaiia"
source_id: "yc-gaiia-news-import-0ba41f3cf900"
canonical_url: "https://gaiia.com/newsroom/cdr-and-cabs-billing-for-voice-providers"
published_at: "2025-10-06T00:00:00+00:00"
first_seen_at: "2026-07-23T10:16:42.852450+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:479d71f0d693d7f0f9283dd3ccac1411d18e1f0ba46f42b4b527fb5584a7f5fb"
---

# CDR & CABS Billing for Voice Providers

CDR and CABs billing support is now live in gaiia. This marks a major step forward for ISPs offering voice services, especially those that have struggled with black-box billing systems, limited visibility, or inflexible plan structures.


With this update, gaiia supports both **CDR billing** (for charging subscribers based on voice usage) and **CABs billing** (for inter-carrier compensation when others use your phone infrastructure).


The intention of this article is to outline the new features within gaiia and highlight how we’re bringing clarity, configurability, and transparency to one of the most complex areas of ISP billing.


‍


‍


## CDR Billing


**What is CDR Billing:** CDR stands for **Call Detail Records** . These records contain all the details about a phone call: who made it, who received it, how long it lasted, when it happened, and where the call originated and terminated.


With gaiia, ISPs can now:


- Ingest CDR files directly through SFTP in the format provided by the phone switch
- Automatically match call records to customer accounts
- Apply accurate, usage-based pricing using fully configurable rate tables
- Support tiered and graduated pricing (for example, 2 cents per minute for the first 5 minutes, 4 cents thereafter)
- Handle recurring monthly fees and usage charges on the same plan
- Offer free minutes, usage caps, and maximum charges per plan
- Set up rate schedules to adjust pricing by time of day or day of week
- Show full call logs on the account, giving your team insight into every billed minute
- Automatically include call record summaries on invoices for compliance and transparency
- Generate default regulatory reports, including FCC call reporting and usage summaries


‍


**Why gaiia’s CDR feature stands out:** In many legacy systems, call records are hidden or processed behind the scenes with little visibility for operators. In gaiia, everything is visible, customizable, and auditable. You can see exactly how each call was rated and billed, and make updates to your plans as needed, without relying on support tickets or external vendors.


‍


## CABs Billing


**What is CABs Billing:** CABs stands for Carrier Access Billing. It refers to the charges your ISP collects when other carriers use your voice network infrastructure, including your switches, lines, and interconnects. This is common when you operate rural or last-mile infrastructure that enables calls to reach customers outside another carrier’s network.


gaiia now supports:


- Processing **carrier-to-carrier call records**
- Rating those calls based on your access charge structure
- Assigning and tracking charges for **inter-carrier settlements**
- Generating reports that support regulatory filings and reconciliation


‍


CABs billing is highly specialized and often difficult to manage using traditional platforms. Many systems rely on custom setups, with critical information not exposed in their UI, leaving operators in the dark. With gaiia, we bring the process into a more modern and standardized workflow, giving your team more control and flexibility.


‍


## Why CDR & CABs billing matter


Voice billing is complex. Every phone plan comes with unique rules, usage scenarios, and regulatory requirements. Most legacy billing systems were built decades ago, and many modern platforms still treat usage billing as an afterthought. That leads to hidden logic, poor visibility, and costly delays any time you need to make a change.


gaiia is different. We’ve built a modern, usage-based billing engine from the ground up. It gives you complete control over your pricing logic, makes call records fully transparent, and supports both subscriber and carrier billing.


You can see exactly how rates are applied, when charges are triggered, and how every invoice is built. Your team has the tools to create, update, and manage even the most complex plans without relying on support tickets or external vendors.
