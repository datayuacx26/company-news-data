---
schema_version: "1.0.0"
document_id: "0c0315cd3fddce38b66f7c8473f752fe6f0fbe7413108be85090fa632b5e2ff5"
company_key: "yc-campfire-2"
company: "Campfire"
source_id: "yc-campfire-2-rss-3be8123e2374"
canonical_url: "https://campfire.ai/blog/march-product-updates"
published_at: "2026-03-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:46.218928+00:00"
fetched_at: "2026-07-28T21:56:50.434513+00:00"
content_hash: "sha256:56aba7efed5290fc138a5ce9b2ebadf263fc9c22b70603367d4e01edaba677f8"
---

# March Product Updates

[Blog](https://campfire.ai/blog) Article


[Back to Home](https://campfire.ai/blog)


2026-03-31


# March Product Updates


Campfire


Team


March 31, 2026


[Finance](https://campfire.ai/blog?category=Finance)[Accounting](https://campfire.ai/blog?category=Accounting)[Invoicing](https://campfire.ai/blog?category=Invoicing)[Taxes](https://campfire.ai/blog?category=Taxes)[General](https://campfire.ai/blog?category=General)


In March, we shipped Ember 2.0, the biggest upgrade to our AI layer since launch, alongside a range of improvements across reporting, integrations, fixed assets, and billing. Here is everything that is new.


# **Ember 2.0: upgraded agents + custom and templated agents**


Ember is Campfire's AI agent layer built for finance teams. In 2.0, the biggest change is how Ember processes data. It now writes and executes code on your behalf, which means faster answers, fewer errors, and up to 20% higher success rates on complex tasks.


Alongside the 2.0 upgrade, we are launching Custom Agents. Any recurring workflow your team runs, Ember can now handle automatically on a schedule. You describe what you need, and Ember takes it from there. Two examples of custom agents teams are using today:


- **Weekly CFO Cash Variance Report.** Every Monday at 7am, Ember pulls your actuals, compares them to forecast, flags the variances, and emails a CFO-ready summary before anyone opens their laptop.
- **AR Monthly Monitor.** On the 15th of every month, Ember pulls your AR aging, flags invoices more than 90 days overdue, highlights high-priority clients, and sends a structured report with recommended next steps.


# **Custom reports redesign**


Custom Reports has been redesigned from the ground up to make it easier to build and configure reports without needing help. The new interface works like a pivot table in Excel: drag rows, columns, and values exactly like a spreadsheet, with no multi-screen wizard and no rebuilding from scratch if you make a mistake.


Column totals now appear automatically at the bottom of every report. This was one of the most requested features from customers.


# **CRM integration improvements: HubSpot and Salesforce**


### **HubSpot 2-way sync**


Changes made in Campfire now flow back to HubSpot, and vice versa. Contract and subscription updates sync in both directions. Invoices push from Campfire to HubSpot automatically, so your sales team has full visibility into billing status without ever leaving their CRM.


Setup is also more self-serve now. Field mapping is dropdown-based so you are not typing values from memory, and most contract mappings are auto-created when you connect.


### **Salesforce 2-way sync**


The Salesforce integration now goes deeper in both directions. Contracts and Contract Subscriptions sync in real time between Campfire and Salesforce, so changes in either system are reflected in the other automatically. Invoices, invoice lines, and invoice payments flow from Campfire to Salesforce as well, keeping your CRM current without any manual export or data entry. Custom fields and tag groups can now be mapped entirely on your own.


# **Fixed assets**


### **Fixed asset reconciliation report**


Reconciling your fixed asset register against the balance sheet previously meant exporting both and tying them out manually in a spreadsheet. The new Fixed Asset Reconciliation Report shows GL control account balances and fixed asset register balances side by side as of any reporting date, with differences broken out for cost, accumulated depreciation, and net book value. You can drill into the GL side to see underlying transactions, drill into the register to see the depreciation schedule behind any balance, and export to Excel.


### **Fixed asset enhancements**


Four improvements to how fixed assets are managed:


1. **Pause and resume depreciation.** If an asset goes out of service temporarily, pause depreciation and resume it when the asset comes back. The schedule adjusts automatically.
2. **Non-depreciable assets.** Land, construction in progress, and similar assets can now be tracked in the fixed asset register without generating depreciation entries.
3. **No depreciation start date required.** Record an asset before you know when depreciation begins. Set the date later and the schedule kicks in from there.
4. **Asset-level end date override.** Override the class-level depreciation end date on individual assets for early disposals or extended useful life.


### **Fixed asset reclassification**


Moving a fixed asset between asset classes, for example from Construction-in-Progress to Leasehold Improvements, previously meant disposing and re-creating it, which broke the audit trail and accumulated depreciation history. A new Reclassify / Transfer action on the asset detail page handles catch-up depreciation, generates the correct reclassification journal entries, and supports partial transfers, all while preserving the full history.


# **Lease accounting**


Campfire's lease accounting module supports both operating and finance lease classifications under ASC 842. You can now assign Department and Tags to individual leases, making it straightforward to allocate lease costs by business unit and filter lease data in reports.


# **Claude connector**


Campfire is now live on the Claude App Store. Connect your workspace with a single click and start querying your financial data directly inside Claude. Find us under Connectors in your Claude settings.


We have also expanded our MCP tools to twelve. You can now connect your LLM of choice to your chart of accounts, income statement, balance sheet, cash flow, contracts, aging reports, and more, all through natural language.


# **Sales commission amortization**


Sales commission accounting is now handled in Campfire. Commission expenses can be linked to the Amortize transaction type, spreading the cost over the life of a contract instead of recognizing it all upfront. Create the commission, link it to the contract, and the amortized journal entries populate automatically from there. Define your amortization schedule, generate the associated journal entries, and link everything back to the underlying contract. No more spreadsheets.


# **Evergreen contracts**


Contracts that do not end should not have an end date. Month-to-month SaaS subscriptions, open-ended service agreements, and rolling retainers can now be set up as evergreen contracts. Invoices generate on schedule. Revenue data updates automatically. No more arbitrary end dates or manual renewals.


Billing frequency can be monthly, quarterly, biannually, or annually. Subscriptions can be added, modified, or removed at any point over the contract's lifetime.


### **Deel payroll integration**


Deel is now available as a payroll connection in Campfire. Find it on the Connections page with a New badge. The connections page also got general improvements: alphabetical sorting, search fixes, a Cmd+F shortcut, and logos added for Deel, Okta, and Airwallex.


### **Draft invoices**


Invoices can now be saved as drafts with no GL impact. Edit, delete, or route them through an approval workflow before anything hits the ledger. Once approved and posted, the invoice syncs to your ERP as normal.


### **Credit and debit memo tax handling**


Credit and debit memos now support tax rate selection and line-level tax amounts. Sales Tax Payable is generated as a separate journal entry line, and tax is excluded from contract value calculations. This eliminates phantom Unbilled AR balances and ensures accurate reporting across both memo types.


### **Activity and downloads page**


When you kick off a report export or bulk download, a new Activity page in Settings shows you the status, start time, and completion time of every async download in one place. Once a file is ready, grab it directly from the page.


That's March. More to come next month. Want to stay up to date?[Sign up](https://share-na2.hsforms.com/2X5Ot47rYTWyZo3nf8uIAXgnqq9m) for our release emails or[follow us on LinkedIn](https://www.linkedin.com/company/meetcampfire/?viewAsMember=true) .


Recent Articles


Loading posts...
