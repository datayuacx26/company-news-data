---
schema_version: "1.0.0"
document_id: "05597829e09890654b4df615ac1b628792515ee52dd3e58094ed906a599db181"
company_key: "yc-truewind"
company: "Truewind"
source_id: "yc-truewind-rss-014af42b96d1"
canonical_url: "https://www.truewind.ai/blog/braintree-settlement-sage-intacct-workflow"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T21:49:05.662974+00:00"
fetched_at: "2026-08-11T21:49:07.984047+00:00"
content_hash: "sha256:f62ac9436355d6ddbeabc0a204158b324c12ccb4215bbd2524f8df8b791cf045"
---

# Raw Braintree Files to Sage Journal Entries (August 2026)

Getting Braintree data into Sage Intacct sounds like a data problem, but it's really a translation problem. The settlement export gives you transaction-level detail; Sage needs structured journal entries with account codes, dimensions, and balanced debit and credit lines. Closing that gap manually works once. It gets harder fast as volume grows or you're managing multiple Braintree accounts across different entities.


**TLDR:**


- Braintree exports raw transaction data with no GL account mapping, dimension codes, or Sage-ready structure built in.
- Gross settlement posts fees as a separate expense line; net settlement requires you to reconstruct gross revenue before your bank feed and GL will tie.
- Use settlement date, not authorization date, on every Sage Intacct journal entry line to avoid open items in reconciliation.
- Refunds, chargebacks, and voids each need a distinct entry structure; Braintree groups them by status code, not by accounting treatment.
- Truewind's WorkPaper Agent reads Braintree CSV exports, maps each transaction to your live Sage Intacct account and dimension codes, and posts entries through the API after human review.


## What Braintree Exports Actually Contain


Braintree's transaction exports follow a predictable structure, but that structure requires interpretation before anything lands cleanly in Sage Intacct. The[Braintree Settlement Batch Summary](https://developer.paypal.com/braintree/articles/control-panel/reporting/settlement-batch-summary) displays totals by batch, not the line-level detail Sage needs for journal entries.


A standard Braintree CSV or JSON export includes transaction ID, settlement date, gross amount, processing fees, refund amounts, dispute adjustments, and payout status. What it does not include is GL account mapping, Sage dimension codes, or any indication of how a given transaction should be split between revenue, fees payable, and net settlement.


### What the Raw Data Looks Like in Practice


Each row in a Braintree settlement report represents a single transaction event, not a[journal entry](https://www.truewind.ai/blog/journal-entries-accounting-guide) . A single payout from Braintree may bundle hundreds of individual transactions across multiple days, with fees netted out at the payout level instead of at the transaction level. That netting creates a gap: the gross revenue line and the net cash deposit rarely match, and closing it requires reconstructing the fee detail from the transaction-level export.


The fields you typically work with include:


- Transaction ID and type (sale, refund, dispute), which determines whether the entry is a debit or credit to your AR or revenue account
- Settlement amount and currency, which drives the gross revenue recognition line
- Braintree fee amount, which needs to post to a separate merchant fees expense account, not net against revenue
- Payout date and payout ID, which is how you tie individual transactions back to the bank deposit that actually hit your Sage cash account
- Refund and chargeback amounts, which require their own reversal entries, not a simple offset


Without explicit[Sage Intacct dimension mapping](https://www.truewind.ai/blog/sage-intacct-dimension-mapping-errors) to department, location, or project, none of this data posts anywhere on its own. The export tells you what happened; it does not tell Sage where to put it.


## The Accounting Gap Between Braintree Data and Sage Intacct


Braintree exports raw transaction data. Sage Intacct expects structured journal entries with dimensions, entity assignments, and account codes. Nothing in Braintree's default export format bridges that gap automatically.


What comes out of Braintree is a flat CSV: transaction IDs, gross amounts, fees, and settlement dates. What Sage needs is a debit to cash, a credit to revenue, a separate line for processing fees, and the correct class, department, and location dimensions attached to each line. That mapping has to happen somewhere, and without automation, it happens manually.


For most teams, the workflow looks like this:


- Pull the Braintree settlement report and cross-reference it against the payout batch to confirm gross-to-net math
- Build a journal entry template in Excel, map each transaction type to the right GL account, and populate dimensions by hand
- Review for coding errors, check that fee lines net correctly, and then key the entry into Sage


That process works once. It breaks down when transaction volume climbs, when you have multiple Braintree accounts feeding different entities, or when a settlement batch spans multiple revenue categories that need to land in separate Sage dimensions. At that point, the manual build is not a one-hour task.


### Why Standard Braintree Exports Fall Short for Sage


Braintree's reporting tools are built for payment operations, not accounting. The settlement summary gives you totals; the transaction-level report gives you line-by-line detail. Neither gives you a file Sage can consume directly.


The core structural problems:


- Braintree reports gross transaction amounts and fees in separate columns, but Sage journal entries require them on separate debit and credit lines within the same entry
- Braintree has no concept of Sage dimensions, so class, department, project, and location codes have to be applied after export based on rules your team maintains separately
- Multi-currency transactions require additional conversion steps before the entry can post correctly in Sage's base currency
- Refunds and chargebacks appear as line items in the settlement report but need to be treated as reversals in Sage, not as new debits


Each of these is a solvable problem in isolation. Together, they make the Braintree-to-Sage workflow one of the more time-consuming recurring tasks in a finance team's close cycle.


## Gross vs. Net Settlement and Why It Changes Your Journal Entry Structure


Braintree pays out in one of two ways, and which one you have determines the shape of every journal entry you build.


With gross settlement, Braintree deposits the full transaction amount and deducts processing fees separately, usually on a monthly invoice or in a distinct line item. With net settlement, Braintree deducts fees before the deposit hits your bank, so what lands in your account is already reduced.


### Why This Matters for Sage Intacct


The fee treatment changes both the debit and credit sides of the entry:


- Gross settlement entries record the full gross receipt as revenue, then recognize the processing fee as a separate expense line, typically to a cost of goods sold or merchant fee account.
- Net settlement entries record only the net deposit amount, which means the gross revenue figure never explicitly appears in the entry unless you gross it up manually or through an automation layer.


Most Sage Intacct teams running gross settlement find reconciliation more straightforward: the gross deposit ties directly to transaction-level reporting from Braintree, and the fee hits a discrete expense account on a predictable schedule. Net settlement requires an extra step to reconstruct gross revenue, which matters for revenue recognition accuracy and for any reporting that separates gross volume from fee burden.


The practical question for your journal entry template is whether you want your Sage entries to reflect gross or net amounts, and whether your bank feed will show the gross deposit or only the net payout. Mismatching those two creates open items in[bank-to-book reconciliation](https://www.truewind.ai/blog/bank-to-book-reconciliation-sage-intacct-guide) that your team will spend time chasing down at close.


## Mapping Braintree Fields to Sage Intacct Journal Entry Fields


Every Braintree transaction export carries a predictable set of fields, and knowing exactly which ones map to which Sage Intacct journal entry components is what separates a clean import from an afternoon of manual cleanup.


The core mapping looks like this:


Braintree FieldSage Intacct JE FieldNotesTransaction IDMemo / Reference NumberUse as the unique identifier to prevent duplicate entriesSettlement DateJournal Entry DateNot the authorization date; settlement is when cash movesSettlement AmountDebit / Credit AmountGross amount before fees; fees post as a separate lineTransaction Type (sale, refund, dispute)Account CodeDrives which GL account receives the entryPayment Method (card, PayPal, Venmo)Department or Class DimensionUseful for revenue segmentation by channelCurrencyCurrency fieldRequired if you run multi-currency books in SageCustomer IDCustomer dimensionOptional, but valuable for[AR reconciliation](https://www.truewind.ai/blog/ar-reconciliation-sage-intacct-deposits-payout-statements) subledger tie-outs


A few mapping decisions require judgment calls that the raw export won't make for you.


### Settlement Date vs. Authorization Date


Braintree timestamps every transaction twice: once at authorization and once at settlement. Sage Intacct journal entries should use the settlement date consistently. Authorization dates create timing mismatches between your bank feed and your GL, which surfaces as open items in reconciliation that are genuinely hard to clear without knowing why they exist.


### Gross vs. Net Posting


Braintree can report either the gross transaction amount or the net amount after payment processing fees are deducted. For Sage, gross posting is generally cleaner: debit your payment processing receivable or cash clearing account for the full transaction amount, then record the fee as a separate expense line. Net posting obscures fee visibility and makes it harder to tie your bank deposits to your GL without a separate fee schedule.


### Dimension Mapping for Multi-Entity Books


If your Sage Intacct setup uses dimensions such as location, department, or project, Braintree's payment method and merchant account fields are the most reliable source for that mapping logic. A card transaction through your primary merchant account maps differently than a PayPal or Venmo payout, and getting that dimension assignment right at the mapping stage keeps your reports clean without requiring manual correction after posting.


## Handling Refunds, Chargebacks, and Voids in the Journal Entry


Refunds, chargebacks, and voids each hit the books differently, and mapping them incorrectly into Sage Intacct creates reconciliation headaches that compound across close cycles.


- A refund is a real cash outflow. It needs a debit to the original revenue account and a credit to cash or the settlement liability, following the same entry structure used when[matching Stripe payouts in Sage Intacct](https://www.truewind.ai/blog/reconcile-stripe-payouts-sage-intacct-without-spreadsheets) , depending on whether Braintree has already settled the funds.
- A chargeback reverses a previously settled transaction. Braintree reports it as a negative net settlement amount, so your journal entry must reverse the original revenue recognition and record any associated chargeback fees separately as an expense line.
- A void cancels a transaction before settlement. No cash moved, so the entry is a full reversal of the original authorization with no fee impact.


Getting these three scenarios into a single journal entry template is where most manual exports break down, contributing to[reconciliation bottlenecks](https://www.truewind.ai/blog/how-to-reduce-reconciliation-bottlenecks-without-more-headcount) that compound at close. Braintree's transaction export groups them by status code, not by accounting treatment, so your team has to sort and reclassify before building any entry.


### How Truewind Handles the Split


Truewind reads the transaction status codes directly from Braintree and routes each transaction type into the correct entry structure automatically. Refunds post as debits to revenue with the matching settlement date. Chargebacks generate a two-line entry that separates the revenue reversal from the fee. Voids are excluded from the settlement reconciliation entirely.


Your team reviews flagged exceptions before anything posts to Sage Intacct. The classification happens without manual sorting, and the audit trail connects each entry back to the originating Braintree transaction ID.


## Sage Intacct's CSV Import Requirements for Journal Entries


Sage Intacct accepts journal entries through a structured CSV import, and the format requirements are strict. A file that fails validation gets rejected entirely, which means any error sends you back to square one. Sage publishes the full field requirements in its[GL journal entry import documentation](https://www.intacct.com/ia/docs/en_US/help_action/More/Uploading_Data/GL/import-GL-journal-entries.htm) .


The required fields for each row are:


- Journal symbol identifying which journal the entry posts to (such as GJ for general journal)
- A unique batch or entry reference number to group debit and credit lines together
- Transaction date in MM/DD/YYYY format
- Account number matching an active GL account in your Intacct chart of accounts
- Debit or credit amount, with each entry requiring a balanced pair before Intacct will accept the file
- Memo or description field, which Intacct requires to be populated for audit trail purposes


Dimension fields such as department, location, class, and project are optional columns, but if your Intacct configuration requires them for a given account, omitting them triggers a validation error on import. Getting dimensions right matters because Intacct uses them for reporting segmentation, and a journal entry posted without the correct dimension values is harder to correct after the fact than to build correctly from the start.


## Dimension Assignment for Braintree Transactions in Sage Intacct


Braintree transactions carry more data than a settlement amount and a date. Each charge typically includes a customer ID, a product or service type, a currency, and sometimes a regional identifier. In Sage Intacct, that data maps directly to dimensions: department, location, class, project, and customer. Getting that mapping right on every transaction is where manual journal entry prep breaks down.


### Why Dimension Assignment Fails at Volume


When a finance team processes Braintree exports by hand, dimension assignment gets inconsistent fast. One accountant maps a SaaS subscription charge to the "Software" class; another logs the same transaction type under "Product Revenue." By close, the GL has dimension gaps that take hours to trace and correct.


The core problem is that Braintree's raw export files do not speak Sage's dimension schema. The fields exist in the source data, but the translation layer, the logic that says "this customer ID maps to this department and this location," has to be built and applied consistently every time.


### What Consistent Dimension Assignment Actually Requires


Getting dimensions right across a full Braintree transaction set requires three things working in sequence:


- A field mapping that connects Braintree metadata (customer ID, product type, region) to the corresponding Sage dimension values, applied at the transaction level before any journal entry is drafted.
- A rule set that handles edge cases, such as transactions with missing regional identifiers or multi-currency charges where the location dimension needs to follow the currency, not the billing location.
- A review layer where an accountant confirms dimension assignments on flagged transactions before the journal entry posts to Sage.


Without all three, dimension data in the GL accumulates errors that only surface during flux analysis or audit prep, well after the close has posted.


## Common Import Errors and How to Fix Them


When Braintree files land in Sage Intacct, three import errors account for the majority of failed journal entries.


### Duplicate Transaction IDs


Braintree exports can include the same settlement transaction in multiple date-range pulls. Sage will reject a journal entry if the reference ID already exists in the GL, but it won't always surface a clear error message. Before importing, run a deduplication check on the transaction ID column. Any ID appearing more than once in your import file needs to be removed.


### Currency Mismatches


If your Braintree account processes in multiple currencies and your Sage entity is set to a single functional currency, the import will fail on any line where the transaction currency differs from the entity default. Convert foreign-currency amounts to your functional currency before building the journal entry, and document the exchange rate used so the audit trail is clean.


### Missing or Unmapped Dimensions


Sage Intacct requires dimension values on every journal entry line where dimensions are active. Braintree exports carry no dimension data, so every transaction arrives without a department, location, or project code. A journal entry missing required dimensions will either error on import or post to a default that misrepresents the actual cost center. Map dimensions during the transformation step, before the file reaches Sage, not after a failed post. This is an area where[AI reconciliation vs. Sage Intacct matching](https://www.truewind.ai/blog/ai-reconciliation-vs-sage-intacct-matching) makes a measurable difference.


## Setting Up Recurring Journal Entries for Ongoing Braintree Activity


Sage Intacct's recurring journal entry feature cuts repetitive setup work for teams processing Braintree data every month.


To configure one, go to General Ledger, open Journal Entries, and select a recurring schedule. Define the journal symbol, start date, and monthly interval. From there, the setup splits based on how predictable your Braintree activity is:


- Fixed-amount lines work well for costs that do not change period to period, such as a flat monthly gateway fee.
- Variable lines, where the amount changes based on each period's Braintree settlement total, require a manual amount update before the entry posts, a gap that[Sage Intacct reconciliation automation](https://www.truewind.ai/blog/sage-intacct-reconciliation-automation) can close.


To pause or modify a recurring entry mid-cycle without losing the template, edit the schedule directly in Sage. Prior-period postings stay untouched; only future cycles pick up the updated parameters. Deleting the template removes the configuration entirely and forces a rebuild from scratch, so editing in place is the safer path when adjusting amounts or stopping a recurrence temporarily.


## How Truewind Automates the Braintree-to-Sage Intacct Workflow


Truewind's[workpaper automation for Sage Intacct](https://www.truewind.ai/workpaper-automation-sage-intacct) ingests raw Braintree CSV exports and produces complete workpapers, summary tabs, pivot tables, and GL-ready staging journal entries mapped to the client's live Sage Intacct codes. From there, a human reviewer approves the entries before anything posts to the GL, keeping your team in control of the final record.


The process runs in three stages:


- Truewind reads the Braintree export and maps each transaction line to the correct Sage Intacct account, department, and dimension codes based on your existing chart of accounts.
- It groups transactions into a summary workpaper with supporting pivot tables, so the reviewer can spot discrepancies before approval, not after posting.
- Once approved, entries post directly to Sage Intacct through the API, with no manual re-entry required.


## Final Thoughts on Getting Braintree Data Into Sage Intacct Cleanly


The core challenge is not pulling data out of Braintree. It's the translation work between what Braintree exports and what Sage needs. Dimension mapping, gross vs. net fee treatment, refund reversals, and currency conversion all have to be resolved in the right order, before the file reaches Sage, or your team spends close chasing errors instead of reviewing a clean ledger.[See a Truewind demo](https://www.truewind.ai/see-a-demo) to watch that process run without the manual prep work.


## FAQs


### How do I map Braintree settlement fields to Sage Intacct journal entry fields without manual rework each month?


Start with four core mappings: transaction ID to the reference number field (to block duplicate entries), settlement date to the journal entry date (not authorization date), settlement amount to the gross debit line, and Braintree fee amount to a separate merchant fees expense line. Dimension values (department, location, class) have no equivalent in Braintree's export, so you need a translation layer that applies your Sage codes before the file reaches the import step. Without that layer built and locked, dimension assignment drifts across close cycles and accumulates GL errors that surface during flux review.


### What's the fastest way to handle Braintree-to-Sage Intacct journal entries when transaction volume is high and settlement batches span multiple revenue categories?


At low volume, a manually built Excel template works. Once settlement batches start bundling hundreds of transactions across multiple revenue categories and Sage dimensions, the manual build breaks down. Truewind's WorkPaper Agent ingests the raw Braintree CSV, applies your existing GL account codes and dimension mapping rules, and groups transactions into a reviewer-ready workpaper with summary tabs and pivot tables before anything posts to Sage Intacct through the API. A human reviewer approves before posting, so your team retains sign-off on the final record without rebuilding the entry from scratch each cycle.


### Should I use gross or net settlement posting for Braintree transactions in Sage Intacct?


Gross settlement posting is generally cleaner for Sage Intacct reconciliation. Recording the full gross receipt as revenue and the processing fee as a separate expense line keeps your bank deposit traceable to transaction-level Braintree reporting and gives you a discrete fee visibility line for reporting. Net settlement requires reconstructing gross revenue separately, which adds a step every period and can create open items in reconciliation when the bank feed shows only the net payout amount.


### How does Truewind handle refunds, chargebacks, and voids differently when building Braintree journal entries for Sage Intacct?


Each transaction type requires a different entry structure. Refunds post as debits to the original revenue account with a matching credit to cash or settlement liability. Chargebacks generate a two-line entry that separates the revenue reversal from any associated chargeback fee. Voids are excluded from settlement reconciliation entirely because no cash moved. Truewind reads the transaction status codes from the Braintree export and routes each type into the correct entry structure automatically, with flagged exceptions routed to a reviewer queue before anything posts to Sage Intacct.


### What causes Sage Intacct CSV import failures on Braintree journal entries and how do you fix them?


Three issues account for most failed imports. First, duplicate transaction IDs from overlapping date-range pulls in Braintree: run a deduplication check on the transaction ID column before building the import file. Second, currency mismatches on multi-currency transactions where the Braintree export currency differs from your Sage entity's functional currency: convert amounts and document the exchange rate before the import step. Third, missing dimension values on journal entry lines where Sage requires department, location, or project codes: these must be applied during the transformation step, before the file reaches Sage, because correcting dimension errors after a failed post is slower than mapping them correctly the first time.
