---
schema_version: "1.0.0"
document_id: "0b38bc265544f356f57c27550a66acb23b3bb2578488876a2c0faf3e2ec95772"
company_key: "yc-campfire-2"
company: "Campfire"
source_id: "yc-campfire-2-rss-3be8123e2374"
canonical_url: "https://campfire.ai/blog/february-product-update-26"
published_at: "2026-03-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:46.218928+00:00"
fetched_at: "2026-07-28T21:57:38.048658+00:00"
content_hash: "sha256:e767b76a88d537c6c4efd2e08806b2b9e6d29efc13424fe077a1cf5821ccffe0"
---

# Campfire's February 2026 product updates: AI-powered accounting, FX management, CRM Integrations, and more

[Blog](https://campfire.ai/blog) Article


[Back to Home](https://campfire.ai/blog)


2026-03-01


# Campfire's February 2026 product updates: AI-powered accounting, FX management, CRM Integrations, and more


Campfire


Team


March 01, 2026


[General](https://campfire.ai/blog?category=General)


February was one of Campfire's most productive months yet. From a major AI upgrade to new fixed asset reporting, CRM automation improvements, and multi-currency support, here's a deep dive into everything we shipped.


# **Ember 1.4: your AI accounting agent just got a lot smarter**


Campfire's AI agent Ember received its most significant upgrade to date in February, with four major capability improvements that make it a more reliable and powerful part of your close process.


### **Charts in Chat**


Ask Ember to visualize your AP aging as a bar chart or plot revenue trends as a line graph, and it builds the chart directly in your conversation. You can update titles and axis labels without leaving the chat window — no more exporting data to a separate BI tool for basic visualizations.


### **Verified calculations with code execution**


Previously, Ember reasoned through math in plain text — which occasionally led to rounding errors or incorrect aggregations. Now, Ember writes and executes Python in a secure sandbox for any calculation, aggregation, or data extraction task. The result it returns is the verified output of that code, not an estimate. Every number ties out.


### **Expense accrual automation**


Ember can now automate month-end accrual analysis and integrate it directly into your close checklist. Build the prompt once, save it to your checklist, and run it with one click every month.


### **Upgraded to claude sonnet**


Ember's underlying model is now Anthropic's Claude Sonnet — one of the most capable models available for multi-step reasoning tasks. Expect meaningfully better responses on flux analysis, transaction categorization, variance explanations, and anything that requires working through several steps at once.


# **Accounting Intelligence: now available to all customers**


Accounting Intelligence (formerly LAM) is now live for all Campfire customers. This feature handles the notoriously tricky problem of bill and payment matching — particularly for partial payments and invoices with similar amounts. Instead of surfacing multiple ambiguous matches, Accounting Intelligence surfaces the right one, reducing manual review time and accelerating your close.


# **Fixed asset rollforward report**


Period-end reporting for fixed assets used to mean pulling beginning balances, depreciation schedules, and disposal entries from three different places and hoping they reconcile. Not anymore.


The new Fixed Asset Rollforward report shows the complete movement of every fixed asset over any date range in a single view:


- Beginning balances
- Additions and disposals
- Depreciation charges
- Adjustments
- Ending net book value


Results are grouped by asset class with subtotals. Click any number to drill into the journal entries behind it. Filter by entity, asset class, date range, or tag. Export on demand or schedule the report to land in your inbox automatically before close.


# **Lease accounting: more scenarios, better classifications**


Campfire's lease accounting module now handles a broader range of real-world lease structures, including variable payments, lease modifications, and early terminations — scenarios that previously required manual workarounds.


Lease liability now also splits automatically into current and non-current classifications, which is required for ASC 842 balance sheet presentation. This removes a common manual adjustment at period-end for companies with operating or finance leases.


# **CRM integration improvements: HubSpot and Salesforce**


### **HubSpot: multi-condition sync triggers**


HubSpot sync triggers now support multiple conditions, giving you precise control over which deals flow into Campfire as contracts. Instead of syncing everything and cleaning it up afterward, you configure the rules upfront — for example, Stage = Closed Won AND Pipeline = Enterprise — so only the right deals come through.


### **Salesforce: self-serve field mapping and automatic payment sync**


Two major quality-of-life improvements for Salesforce users:


**Self-serve custom field mapping.** You can now map Salesforce fields and tag groups to Campfire yourself — no engineering ticket, no waiting on support. Configure how opportunities, accounts, and custom fields sync into your books directly from the settings UI.


**Automatic invoice payment sync.** Invoice payment status now syncs back to Salesforce automatically. Payment records stay accurate in both systems without anyone doing it manually.


# **FX rates management: use your negotiated rates**


For finance teams operating across multiple currencies, this is a big one.


Your treasury team negotiates contract FX rates for a reason — to control currency exposure and maintain consistent internal pricing. Campfire now actually uses those rates.


Upload your own FX rates via CSV — whether contract rates, internal transfer pricing rates, or period-end spot rates — and Campfire applies them automatically to currency conversions across the platform. A new FX Rates page gives you full visibility into every rate in the system, including any custom overrides.


The contract uploader also now accepts exchange rate columns, so multi-currency contracts load in one step with no manual cleanup after the fact.


# **More updates shipped in February**


### **Line-item tags on invoices and bills**


If a single bill covers multiple departments or cost centers, you no longer have to choose between applying one tag to the whole document (and losing detail) or splitting it into multiple bills. Tag each line individually, so the right cost center gets the right charge and your reports reflect it accurately.


### **Bulk transaction updates**


Re-categorizing or re-tagging transactions one at a time during close is the kind of work that shouldn't exist. Select as many transactions as you need from the list view and update the category, tag, department, or other fields in a single action.


### **Global search improvements**


Finding a specific transaction, invoice, contract, or vendor used to depend on knowing exactly where to look in Campfire. Global search now returns fast, accurate results across all major record types, so you can get to any record from one place.


### **Payroll sync redesign**


The redesigned payroll sync flow lets you review payroll data before it posts to your books. A guided setup validates entity settings, accounts, and department maps upfront. Pay runs land in distinct states for review and approval before posting. Payroll now outputs Accounting Bills instead of Journal Entries, enabling AP subledger tracking, and period locks block syncs into closed periods.


### **Bank of America CashPro integration**


Connect your Bank of America CashPro account directly to Campfire via API integration for automated bank feed ingestion.


### **Multi-tier usage billing**


Set up usage pricing across multiple tiers — bill in-allowance usage at $0 and calculate overages automatically as consumption moves through percentage-based or fixed-rate tiers.


### **Sales Tax Reporting**


View all sales tax collected and paid in one place. See total amounts for any date range, understand your net tax position, and drill into individual transactions to verify what tax was applied.


### **Product Filtering on AR Aging Reports**


Filter AR aging by product to see which product lines have outstanding receivables. Spot collection patterns and manage cash flow at the product level.


*Want to learn more about any of these features? Visit our Help Center or reach out to your account team.*


Recent Articles


Loading posts...
