---
schema_version: "1.0.0"
document_id: "735733da89a8bd9775b4814450ec6a4be970ede915b072b7f71b489481978137"
company_key: "yc-campfire-2"
company: "Campfire"
source_id: "yc-campfire-2-rss-3be8123e2374"
canonical_url: "https://campfire.ai/blog/january-2026-product-updates-automation-intelligence-and-global-scale"
published_at: "2026-02-10T18:58:30+00:00"
first_seen_at: "2026-07-20T23:23:46.218928+00:00"
fetched_at: "2026-07-28T21:57:41.767482+00:00"
content_hash: "sha256:fc21b6e67db6c8ea7fcd6cd175140744df049d4a26f3677bb738d7636e8448df"
---

# January 2026 Product Updates: Automation, Intelligence, and Global Scale

[Blog](https://campfire.ai/blog) Article


[Back to Home](https://campfire.ai/blog)


2026-02-10T18:58:30Z


# January 2026 Product Updates: Automation, Intelligence, and Global Scale


Campfire


Team


February 10, 2026


[Finance](https://campfire.ai/blog?category=Finance)[Accounting](https://campfire.ai/blog?category=Accounting)[General](https://campfire.ai/blog?category=General)


# **January 2026 Product Updates: Automation, Intelligence, and Global Scale**


Welcome to our January product update. This month, we've shipped significant improvements across AR automation, AI-powered close workflows, lease accounting, and multi-entity operations. Whether you're managing international subsidiaries, closing books faster, or scaling billing operations, these updates are designed to reduce manual work and improve accuracy across your financial systems.


# **Accounts Receivable & Billing**


### **Bulk Invoice Management via CSV**


Manual invoice updates are now a thing of the past. Upload a CSV to mark multiple invoices as paid in bulk or update invoice metadata like GL codes and line item descriptions across dozens or hundreds of records simultaneously. This is particularly valuable during month-end close when you're reconciling payment batches from payment processors or bank feeds, or when you need to reclassify revenue across multiple customer invoices after a contract amendment.


The CSV interface validates your data before processing, flagging any mismatched invoice numbers or invalid GL codes so you can correct issues before they hit your ledger. One file, batch changes, zero repetitive data entry.


### **Invoice Butler Integration**


We've integrated with Invoice Butler to fully automate your AR follow-up workflows. Generate invoices in Campfire as usual, then let Invoice Butler handle customer portal submissions, automated payment reminders, and cash collection communications without manual intervention from your finance team.


This integration is live now in your integration ecosystem and connects through our standard OAuth flow. Configure your collection cadence, customize email templates, and set escalation rules directly in Invoice Butler while maintaining Campfire as your source of truth for invoice data and payment status. For teams spending hours each week chasing down customer payments, this eliminates a significant operational burden and accelerates DSO.


### **Configurable Invoice Fields Per Entity**


International operations require local compliance, not one-size-fits-all invoicing. You can now tailor invoice fields on a per-entity basis to match local regulatory requirements and reduce customer confusion.


Hide fields that don't apply to specific jurisdictions—like tax registration numbers that only matter in certain countries—or rename standard fields to match local legal terminology. For example, rename "Tax ID" to "VAT Number" for your European entities or "GST Number" for APAC operations. Each legal entity can now generate invoices that look and read correctly for its local market without requiring separate systems or manual template management.


### **Bulk Void Invoices**


Contract terminations no longer require voiding invoices one by one. When you terminate a customer contract, select all associated unpaid invoices and void them in a single action. This is especially useful for SaaS businesses handling mid-contract cancellations or enterprises managing master service agreement terminations that affect multiple outstanding invoices.


The bulk void operation maintains a complete audit trail, recording which user initiated the void, when it occurred, and which contract termination triggered the action. All associated revenue recognition schedules are automatically adjusted to reflect the voided invoices.


### **Stripe Revenue Recognition with AR Impact**


We've added a toggle to include AR impact in your Stripe revenue recognition flows. Turn this on to see both cash movements and outstanding customer balances in your books, giving you a closer-to-accrual view of Stripe activity without changing your integration setup.


Previously, our Stripe integration primarily tracked cash basis movements—what hit your bank account. With AR impact enabled, you'll now capture the full receivable when Stripe invoices are generated, then relieve that receivable when payment is collected. This is critical for businesses that issue Stripe invoices with payment terms (rather than immediate card charges) or for teams preparing for accrual-basis audits while using Stripe as their primary billing system.


The feature activates with a single toggle and retrospectively applies to recent Stripe data based on your sync settings. Revenue recognition schedules automatically incorporate both cash and accrual movements.


### **Usage-Based Billing Tables**


Prepaid credit management no longer requires manual reconciliation spreadsheets. Our new Usage-Based Billing Tables track customer credit purchases, consumption events, and remaining balances in a single interface, enabling accurate invoicing and revenue recognition for consumption-based business models.


Load initial credit balances, record consumption as it occurs (either manually or via API), and let Campfire calculate remaining balances and trigger billing events when thresholds are met. This is purpose-built for companies selling cloud infrastructure, API calls, compute credits, or any other usage-based product where customers prepay and draw down over time.


Revenue recognition automatically defers prepaid amounts and recognizes revenue as consumption occurs, maintaining GAAP compliance without custom scripts or manual journal entries each month.


# **Financial Close & Reporting**


### **Ember Agents in the Close Checklist**


You can now save Ember AI prompts directly to your monthly close checklist, standardizing AI-powered audit checks and accrual entry drafting as repeatable tasks in every close cycle.


Create an Ember prompt that reviews prepaid expenses for proper amortization, audits intercompany eliminations, or drafts standard accrual entries based on invoice aging. Save that prompt to your close checklist, and each month your team can execute it with one click rather than remembering the exact phrasing or logic.


This transforms Ember from an ad-hoc assistant into a structured part of your close process. Teams using this feature report faster month-end cycles and more consistent error detection across close periods, especially valuable when training new team members or managing rotating close responsibilities.


### **Custom Balance Sheet Builder**


Build balance sheets with flexible groupings, filters, and period comparisons that match your reporting requirements, not generic templates. Configure custom sections, filter by department or custom tags, compare multiple periods side-by-side, and save templates for repeatable reporting to boards, lenders, or business units.


The custom balance sheet engine supports nested groupings (assets → current assets → cash accounts), dynamic calculations (working capital, quick ratio), and custom sub-totals. Apply department filters to see balance sheets for individual business units, or use tag filters to isolate project-specific financials.


Save your configurations as named templates, then generate updated reports each month without rebuilding the structure. Export to Excel with formatting intact, or share live links with stakeholders who need recurring access.


### **Consolidated Entity Reporting**


Income statements and balance sheets now support mid-level entity consolidation in the entity's native currency. This is a significant improvement for holding company structures with regional operating companies that need to see consolidated performance in local currency before translating to group reporting currency.


For example, consolidate your three European subsidiaries into a "Europe Region" view in EUR, then consolidate all regions into group-level USD reporting. This multi-tier approach provides clearer visibility into regional performance without currency translation noise obscuring operational trends.


Intercompany eliminations apply at each consolidation level, and you can drill down from consolidated figures into source transactions across any entity in the consolidation tree.


### **Build Custom Reports in Ember**


Ember can now create detailed custom nested reports with full drill-down capability into source transactions. Describe the report structure you need—whether it's revenue by product line and sales rep, expenses by department and vendor category, or any other multi-dimensional analysis—and Ember will build it.


More importantly, you can click through from summary figures to the underlying transactions that comprise them, verifying the data and understanding exactly what's included in each calculation. This bridges the gap between high-level analysis and transaction-level audit trails without requiring pivot tables or data exports.


### **Close Checklist Frequency Options**


Close checklists now support quarterly and annual task frequencies, not just monthly. Add quarterly board package preparation, annual tax provision reviews, or semi-annual lease accounting assessments directly to your checklist with the appropriate recurrence pattern.


Tasks automatically appear in the relevant close periods and maintain their own completion history separate from monthly tasks. This eliminates the need for separate tracking systems for non-monthly close activities.


# **Accounts Payable & Spend Management**


### **AP Aging by GL Account**


Filter your Bill Aging report by AP account to reconcile different payable streams separately. This matters when you're tracking standard vendor bills through one AP account and corporate card reimbursements through another, or when different entities use different AP accounts that need separate aging analysis.


Run aging reports filtered to your Ramp AP account to see only card-related liabilities, or filter to your standard vendor AP account for traditional invoice-based payables. Each stream maintains its own aging buckets and payment analytics, improving visibility into what's actually driving your payable balances.


### **Ramp Treasury Integration**


Connect Ramp Treasury to automate treasury account reconciliation. Import cash movements from Ramp Treasury directly into Campfire, eliminating manual transaction entry for treasury sweeps, interest income, or liquidity transfers.


The integration syncs daily and categorizes transactions with appropriate GL codes based on your mapping rules. Reconcile your treasury account against Ramp's records directly in Campfire's bank reconciliation interface.


### **Automatic Expensify GL Mapping Backfill**


Our Expensify integration now automatically applies missing GL account mappings during each sync. When employees submit expenses to categories that haven't been mapped to GL accounts yet, Campfire applies your default mapping rules retroactively, reducing misclassified expenses and minimizing manual AP cleanup work.


Previously, unmapped expenses would flow through with blank GL codes, requiring manual review and reclassification. The automatic backfill respects your mapping hierarchy—department-specific mappings override company-wide defaults—and maintains an audit trail of when mappings were applied.


### **Improved Vendor Visibility**


Schedule a daily Vendor List Report to track vendors added as recently as the previous day. This automated report helps ensure new vendors are captured and reviewed promptly, critical for teams managing vendor onboarding approval workflows or monitoring for duplicate vendor records.


Configure the report to send to your AP manager or vendor master data owner each morning with a list of vendors added in the past 24 hours, including who created them and which entity they're associated with.


# **Workflows & Permissions**


### **Custom Roles Enhancements**


Approval permissions are now separated from read, draft, and write permissions, giving you precise control over review workflows. Additionally, entity-level permissions let you delegate close tasks securely across entities without granting access to the entire organization's financial data.


Set up roles where users can draft journal entries but can't approve them, or where they can review and approve bills up to a certain threshold without edit access. Combine this with entity-level restrictions to create regional controllers who manage their entity's close process but can't access other regions' data.


This granular permission model supports mature financial operations with segregation of duties requirements and distributed close responsibilities across multiple legal entities.


### **Approval Workflows**


Configure multi-level approvals for bills and transactions based on dollar thresholds. Define approval chains where bills under $5,000 require one approval, bills from $5,000-$25,000 require two approvals, and bills over $25,000 require CFO sign-off.


Approval workflows route automatically based on transaction amount and can incorporate entity-specific routing rules. Track approval status in real-time, send automated reminders to pending approvers, and maintain complete audit trails of who approved what and when.


# **Data & Integration Infrastructure**


### **On-Demand S3 Data Uploads**


Manually trigger data uploads to S3 instead of waiting for the daily sync schedule. Push updated transactions, bills, or invoices to your data warehouse immediately when you need them for reporting or analysis.


This is particularly valuable during month-end close when you're updating data throughout the day and need downstream systems to reflect changes in real-time. Click "Sync Now" from your S3 integration settings to initiate an immediate export of all updated records since the last sync.


### **S3 Monthly Export Option**


Large transaction volumes can now be split into month-scoped files rather than single monolithic exports. This improves auditability, simplifies data warehouse loading, and makes downstream processing more manageable for high-volume operations.


Configure your S3 integration to export January transactions as transactions_2026_01.csv, February as transactions_2026_02.csv, and so on. This partitioning aligns with typical financial reporting periods and makes it easier to reprocess specific months without reloading your entire transaction history.


### **Float Integration**


Connect your cash flow forecasting to Campfire and sync Float data directly without manual exports. This integration maintains consistency between your actual financial data in Campfire and your forward-looking cash projections in Float.


Map Float categories to Campfire GL accounts, sync historical actuals to inform forecasts, and eliminate the duplicate data entry that typically plagues cash flow planning processes.


### **Stripe Tax in Balance Exports**


Stripe transaction downloads now include tax fields, improving compliance and reconciliation accuracy across revenue and liabilities. When you export Stripe data, you'll see separate columns for gross revenue, tax collected, and net proceeds, making it straightforward to reconcile both revenue and sales tax liability accounts.


This is critical for businesses operating in multiple tax jurisdictions where Stripe handles tax calculation and collection. Previously, parsing tax amounts required custom scripts or manual calculation. The enhanced export provides clean, structured data that maps directly to your GL accounts.


### **Salesforce/Eudia Tag Groups Sync**


Preserve granular revenue tags in CRM-to-ERP mapping, aligning pipeline analytics with ledger classification. When opportunities close in Salesforce and sync to Campfire as invoices, maintain product line, sales region, and custom dimension tags so your revenue reporting in Campfire matches your pipeline reporting in Salesforce.


This eliminates the common problem where sales analytics and finance analytics tell different stories because tagging conventions diverge between systems. Tag groups sync bidirectionally, and you can map Salesforce custom fields to Campfire tag dimensions with flexible transformation rules.


### **Avalara Sandbox Connections**


Enable safe, end-to-end validation of tax configurations and flows without risking production data. Connect Campfire to Avalara's sandbox environment to test tax calculation rules, validate exemption certificate flows, and verify compliance settings before deploying to production.


This is essential for businesses expanding into new tax jurisdictions or implementing complex tax scenarios like multi-state nexus or international VAT. Test thoroughly in sandbox, then promote validated configurations to production with confidence.


### **Plaid Enabled for EU**


European users can now access banking data syncs for nearly 2,000 European institutions covered by Plaid Europe. Connect bank accounts from major European banks, sync transactions automatically, and reconcile cash movements without manual statement imports.


This brings the same banking automation that North American users have been leveraging to European operations, critical for teams managing multi-region entities with local banking relationships.


# **Asset Management**


### **Foreign Currency Fixed Asset Updates**


Create fixed assets in foreign currency with accurate exchange rates, and toggle the fixed asset waterfall report between native currency and consolidation currency views. This is essential for international operations where assets are purchased in local currency but need to be reported in group currency.


Record asset acquisitions at the transaction-date exchange rate, calculate depreciation in native currency, then translate to reporting currency for consolidated financial statements. The waterfall report shows you both perspectives—local currency for operational management and reporting currency for consolidated financials.


### **Amortization Updates**


Create amortizations in foreign currency with accurate exchange rate handling, and choose monthly amortization instead of daily for prepaid expenses. Monthly amortization simplifies GL entries for teams that don't need daily precision and reduces transaction volume in your ledger.


Foreign currency prepaid expense amortization now properly handles exchange rate fluctuations over the amortization period, recording both expense recognition and foreign exchange gains or losses as needed.


# **Accounting Intelligence & Ember**


### **Accounting Intelligence Updates**


We've expanded our Accounting Intelligence labeling capabilities and improved classification accuracy. Accounting Intelligence now automatically labels chart of accounts, vendors, and departments for beta customers, learning from your existing data patterns to suggest appropriate classifications for new transactions.


This machine learning model gets smarter as you use Campfire, eventually reaching the point where the majority of transactions are correctly classified without manual intervention. Beta customers are seeing 60-70% auto-classification rates within three months of activation.


To join the Accounting Intelligence beta, contact your Campfire rep. Beta access is currently limited as we continue refining the model based on customer feedback.


### **Improved Tax Application**


New functionality to apply tax rates across multiple transactions in the bulk transaction update module, and apply taxes across multiple transactions on the transactions page. Select dozens of transactions and apply the appropriate sales tax or VAT rate in one operation rather than editing transactions individually.


This is particularly useful when importing transactions from systems that don't calculate tax automatically, or when correcting transactions after a tax rate change. The bulk tax application respects transaction dates and applies the tax rate that was effective on each transaction date.


# **Additional Updates**


**Ramp Bill Sync Improvement** : Bills rejected in Ramp's platform now automatically delete in Campfire, keeping your AP clean without manual reconciliation. When approvers reject expense reports or card transactions in Ramp, those bills disappear from your Campfire bill queue automatically.


**Product Bundle Management** : Adjust product bundles directly from the contract page instead of navigating to Settings. This streamlines contract setup workflows when you need to modify bundle definitions while configuring customer agreements.


**Smoother Invoice Bulk Upload** : The Bulk Invoices API provides clearer error feedback when invoices fail validation, so your development team can quickly diagnose and fix integration issues. Error messages now include specific field-level validation failures rather than generic error codes.


**Expanded Audit Logs** : Audit logs now capture user session events and API activity performed by API users and tokens, giving you a complete trail of who (or what) did what, whether actions happened in the Campfire UI or via API integration. Track API token usage, monitor automated process activity, and maintain comprehensive audit trails for compliance and security reviews.


As always, we'd love to hear what you're building with Campfire and what features would accelerate your financial operations. Reach out to your account team or book a demo to learn more.


Recent Articles


Loading posts...
