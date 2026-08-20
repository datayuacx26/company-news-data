---
schema_version: "1.0.0"
document_id: "62c90b0198decc8de15a62d5b797ccb3a63591308b33ffffbb7c63325316400f"
company_key: "yc-truewind"
company: "Truewind"
source_id: "yc-truewind-rss-014af42b96d1"
canonical_url: "https://www.truewind.ai/blog/account-reconciliation-how-it-works"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.745562+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:f219025dfd29ec8ef42d903895d429bf18dcab4eda87f3f79df51cfa39a752ac"
---

# Account Reconciliation: How It Works and Why It Delays Close July 2026

Account reconciliation in accounting has a clean definition and a messy reality. The definition: compare your GL balance to an external source, match transactions, clear the differences, document what remains open. The reality: the exception queue grows faster than your team can work through it, source documents arrive late, and there's no centralized view of what's cleared and what's still pending. If you're managing bank reconciliation in accounting, balance sheet account reconciliation, intercompany reconciliation, or all of the above across multiple entities, the friction usually comes from the same structural gaps. This breaks down what the process involves, which types of reconciliation in accounting your team is likely running, and where the delays actually come from.


**TLDR:**


- Account reconciliation compares your GL balance to an external source to verify the balance is correct, with every gap either explained or corrected.
- Six reconciliation types cover distinct account layers: bank, balance sheet, intercompany, vendor/customer, credit card, and payroll.
- Close delays stem from procedural gaps, not math: missing source documents, misaligned preparer/reviewer schedules, and no centralized status view.
- Open items unresolved past 30 days rarely clear on their own and tend to accumulate into write-offs or audit findings.
- Truewind sits on top of QuickBooks Online and Sage Intacct, matching transactions against source documents and routing open items into an exception queue for reviewer sign-off; final posting decisions stay with your team.


## What Account Reconciliation Is


Account reconciliation is the process of comparing a GL account balance to an independent source, such as a bank statement, subledger, custodian report, or third-party confirmation, to verify that the balance is correct and complete.


The comparison is the work. You pull your internal ledger balance, line it up against the corresponding external record, and work through every difference until both sides agree or each gap has a documented explanation.


One question sits at the center of every reconciliation: is this balance right, with evidence to back it up?


## GL Balance vs. Source Balance: The Two-Column Framework


Every account reconciliation compares two numbers: what your GL shows and what an external source confirms. When those two figures match, the account is clean. When they don't, you have an open item to resolve before close.


The framework is straightforward. The GL balance is the internal record, pulled directly from your chart of accounts. The source balance is the external reference: a bank statement, a sub-ledger, a vendor confirmation, or a custody report, depending on the account type.


Work through the difference by asking three questions:


- Are there timing items that legitimately belong in one period but not the other, such as deposits in transit or outstanding checks?
- Are there entries in the GL that have no corresponding external support, or vice versa?
- Are there errors (duplicate postings, miscoded transactions, or currency conversion issues) that need a correcting journal entry?


Each unresolved gap is an open exception item. The goal of reconciliation is to clear that queue down to zero, with every difference either explained by a timing item or corrected through an adjusting entry.


## Types of Account Reconciliation


Most accounting teams work with several distinct reconciliation types, each targeting a different layer of the financial record. Understanding which type applies to a given account or workflow helps teams allocate review time where it matters most.


Reconciliation Type GL Account(s) External Source Primary Focus


Bank Cash Bank statement Timing differences: outstanding checks, deposits in transit, bank fees


Balance Sheet AR, fixed assets, prepaids, and other BS accounts Sub-ledger or supporting schedule Tie ending GL balance to AR aging, depreciation schedule, or amortization rollforward


Intercompany Due-to / due-from accounts Counterpart entity's GL Confirm related-party entries net to zero at the consolidated level


Vendor & Customer Accounts payable / accounts receivable Vendor statements, customer confirmations Resolve disputed invoices, unapplied payments, and timing differences


Credit Card Credit card liability Card statement Match each cardholder transaction to a posted charge with receipt or expense report


Payroll Payroll expense, tax & benefits liabilities Payroll register Tie register to GL journal entry and liability accounts; catch rounding errors and missed accruals


### Bank Reconciliation


Bank reconciliation matches the cash balance in the general ledger against the closing balance on the bank statement. It catches timing differences from outstanding checks, deposits in transit, and bank fees that hit the statement before they hit the books.


### Balance Sheet Account Reconciliation


Every balance sheet account needs a reconciliation that ties the ending GL balance back to a supporting schedule or sub-ledger. Accounts receivable ties to the AR aging. Fixed assets tie to the depreciation schedule. Prepaid expenses tie to the[amortization rollforward](https://www.truewind.ai/blog/rollforwards-in-accounting) .


### Intercompany Reconciliation


When one entity in a corporate group transacts with another, both sides record the activity. Intercompany reconciliation confirms those entries net to zero at the consolidated level. A mismatch here flows directly into the consolidated financial statements.


### Vendor and Customer Reconciliation


Accounts payable and accounts receivable teams periodically compare their internal ledgers against vendor statements and customer confirmations. Discrepancies often trace to invoices in dispute, unapplied payments, or timing differences between when a party records a transaction.


### Credit Card Reconciliation


Each cardholder transaction needs to match a posted charge on the card statement, with a receipt or expense report attached. This type tends to generate high transaction volume and frequent coding disputes.


### Payroll Reconciliation


[Payroll reconciliation](https://www.truewind.ai/blog/reconciling-payroll-registers-sage-intacct) ties the payroll register to the journal entry posted to the GL and to the liability accounts that will settle when tax payments and benefits deductions clear. It catches rounding errors, missed accruals, and benefit deduction mismatches before they age.


## How Account Reconciliation Works Step by Step


Account reconciliation follows a sequential verification process. Each stage builds on the prior one, and skipping steps compounds errors forward into the close.


**Stage 1: Gather source documents.** Pull the GL balance, the external statement (bank, vendor, credit card, or sub-ledger), and any supporting documentation such as invoices, receipts, or prior-period workpapers.


**Stage 2: Match transactions.** Compare each line item in the GL against the external source. Flag any item that appears in one record but not the other.


**Stage 3: Investigate exceptions.** For each open item, determine the root cause: timing difference, data entry error, duplicate posting, or a missing transaction entirely.


**Stage 4: Post adjusting entries.** Record any journal entries needed to correct the GL balance before the period closes.


**Stage 5: Confirm the ending balance.** Verify that the adjusted GL balance ties to the external statement. Document the reconciliation with a sign-off.


The process sounds straightforward. In practice, the friction lives in Stage 3. Investigating exceptions requires pulling source documents, tracing transactions across systems, and often waiting on responses from banks or vendors. That wait time is what pushes reconciliation from a one-day task into a multi-day bottleneck during close.


## How Account Reconciliation Works in Nonprofits


Nonprofits carry reconciliation requirements that differ from standard corporate close work in ways that matter in practice.


The fund accounting structure creates layered complexity. Each restricted grant or donation sits in its own fund, and net assets with donor restrictions must stay segregated from net assets without donor restrictions per[FASB ASC 958 reporting rules](https://www.journalofaccountancy.com/news/2018/dec/fasb-not-for-profit-financial-reporting-standard-201819721/) . That means reconciliation confirms that balances are accurate and that restricted funds haven't been spent outside their designated purpose.


A few areas where nonprofit reconciliation diverges from the standard playbook:


- [Grant drawdown reconciliation](https://www.truewind.ai/blog/sage-intacct-nonprofit-grant-accounting) ties expenditures recorded in the GL against reimbursement requests submitted to funders. If those two figures drift, the organization either under-draws cash it's owed or risks disallowable costs on audit.
- [Donation processor reconciliation](https://www.truewind.ai/blog/sage-intacct-donation-bank-deposit-reconciliation) matches deposits from processors like Stripe, PayPal, or Givebutter against donor records in the CRM, netting out processing fees before the entry hits the GL.
- Restriction release reconciliation confirms that funds moved from restricted to unrestricted net assets had a qualifying expense event, and that the journal entry documenting the release is supported by the underlying program documentation.


The audit exposure for nonprofits is direct. Auditors reviewing Form 990 filings expect verified fund balances and clear evidentiary trails for every restriction release. Gaps here don't just delay close; they create findings.


## How Account Reconciliation Works in Family Offices


Family offices run reconciliation across a fundamentally different set of variables than a typical corporate accounting team. The account set extends well beyond bank and credit card feeds. It includes custodian statements from institutions like Schwab, Fidelity, or Pershing,[brokerage account reconciliation](https://www.truewind.ai/blog/reconcile-brokerage-accounts-sage-intacct) across private equity and venture holdings, distributions from fund investments, intercompany loans between family entities, and often multiple currencies.


Each of those source types arrives in a different format, on a different schedule, and with a different level of detail. A custodian statement might arrive monthly as a PDF. A capital call notice might come as an email attachment with a wired amount that needs to be tied back to a specific fund commitment. A distribution might hit the bank account before the formal notice arrives, leaving the accounting team to match the deposit against an expected amount that hasn't been formally confirmed yet.


The reconciliation workflow in a family office typically runs through several distinct verification stages.


1. Bank and custodian statement matching: Each account is tied to its corresponding statement. Cash balances are confirmed. Any deposits or withdrawals not yet reflected in the GL are identified and either cleared or flagged as open items.
2. Investment activity verification: Capital calls, distributions, and return-of-capital transactions are matched against fund-level notices and LP agreement terms. This requires the accountant to hold the transaction detail against the legal document, alongside the bank record.
3. Intercompany and entity-level reconciliation: Loans, expense allocations, and transfers between family entities need to balance at both ends. A payment recorded in one entity's GL needs a corresponding entry in the receiving entity's books.
4. Valuation and unrealized gain/loss review: For marked-to-market holdings, the period-end valuation needs to be tied against the prior period plus any realized activity. This is where the rollforward logic applies.


Each stage generates its own set of open items, and those open items don't always resolve quickly. That lag is a core reason why[family office external reconciliation](https://www.truewind.ai/blog/family-office-external-reconciliation-sage-intacct) requires a dedicated layer. A custodian might take several days to post a transaction. A fund administrator might not send a formal capital call notice until after the wire has already cleared. The family office accounting team is often working with partial information and building toward a complete picture as data arrives.


## Why Reconciliations Delay Close


Reconciliations drag out the close for reasons that compound on each other. A single missing bank statement pushes the cash reconciliation back a day. That delay holds up the intercompany eliminations. That holds up the consolidated trial balance. By the time the first domino falls, close is already late.[APQC benchmarking data](https://www.apqc.org/resource-library/resource/cycle-time-perform-monthly-close) puts the median monthly close at roughly 6 to 7 business days, a figure that understates the reality for teams running reconciliations across multiple entities or high-volume accounts.


The most common sources of delay are procedural and structural, not mathematical.


- Preparer and reviewer schedules rarely align, so completed reconciliations sit in a queue waiting for sign-off, stalled before they can move forward.
- Supporting documentation arrives late from third parties, banks, or internal teams, leaving preparers unable to start.
- No standardized template means each preparer formats work differently, and reviewers spend time normalizing presentation before they can assess accuracy, an area where[AI reconciliation vs rule-based matching](https://www.truewind.ai/blog/ai-reconciliation-vs-sage-intacct-matching) makes a measurable difference.
- Manual data entry from bank statements or sub-ledgers introduces transcription errors that require rework.
- There is no centralized view of reconciliation status, so the controller has to chase each preparer individually to understand where close stands.


The structural problem is that most reconciliation workflows were designed for a single-entity, single-currency environment. As organizations add entities, accounts, or transaction volume, the workload scales linearly but the process does not. A team that closes one entity in five days does not close ten entities in five days, and[multi-account reconciliation at scale](https://www.truewind.ai/blog/multi-account-reconciliation-sage-intacct) is a different problem entirely.


## Account Reconciliation Best Practices


Reconciliation errors compound quickly when processes lack structure. The practices below reflect what high-performing finance teams consistently get right.


### Build a Reconciliation Schedule and Own It


Every account should have a defined cadence tied to close requirements. High-volume accounts like cash and accounts payable warrant monthly reconciliation at minimum. Lower-activity accounts may support a quarterly cycle. The key is that the schedule is documented, assigned to a specific owner, and treated as a hard deadline with no exceptions for best-effort completion.


### Match at the Source, Not the Summary


Matching at the transaction level catches errors that summary-level comparisons miss entirely. Teams using[AI to match payments to invoices](https://www.truewind.ai/blog/using-ai-to-reconcile-payments-to-invoices-in-80-less-time) report far faster exception clearance. A balance that ties at the top can still contain offsetting errors underneath. Building the habit of tracing each line to its source document keeps exceptions surfaced early, when they are still easy to resolve.


### Separate Preparation from Review


The accountant who prepares a reconciliation should not be the same person who signs off on it. This segregation of duties is a standard internal control, and skipping it creates audit exposure. Even in small teams, a second set of eyes on final reconciliations before close sign-off reduces the risk of errors passing undetected.


### Document Open Items with Specificity


When an open item cannot be cleared immediately, the notation should include what the item is, why it remains open, who owns it, and when resolution is expected. Vague notes like "in process" or "to be confirmed" leave the next reviewer with no way to assess risk or track progress.


### Standardize the Format Across Accounts


Using a consistent reconciliation format across all accounts makes review faster and exception tracking more reliable. When every preparer uses the same structure, a reviewer can move through a reconciliation package without reorienting to a new layout on each tab. Standard templates also make it easier to onboard new staff and maintain quality during turnover.


### Flag Stale Items Before They Age Out of Resolution


Exception items that sit unresolved past 30 days rarely resolve themselves. Building a review step targeted at aging open items into the monthly close process keeps the exception queue from accumulating entries that eventually become write-offs or audit findings.


## Account Reconciliation FAQ


### What is the difference between account reconciliation and bank reconciliation?


Bank reconciliation is one specific type of account reconciliation. It compares a company's cash ledger balance against the bank statement to identify timing differences and errors. Account reconciliation is the broader process applied across any balance sheet account, including prepaids, accruals, fixed assets, and intercompany balances.


### How often should accounts be verified?


Most balance sheet accounts are verified monthly, timed to the close cycle. High-volume cash and credit card accounts sometimes warrant weekly review. Lower-activity accounts may be verified quarterly, depending on materiality and audit requirements.


### What causes account reconciliation to take so long?


The most common culprits are missing source documents, manual data entry across disconnected systems, and exception queues that grow faster than teams can clear them. When reconciliation depends on one person pulling statements, building workpapers, and chasing open items, the process compounds in time at every step.


### Is there a standard account reconciliation template?


There is no universal standard, but most workpapers share a common structure: beginning balance, activity during the period, adjusting entries, ending balance, and a tie-out to the GL. Free templates in Excel or Word can serve smaller teams; larger close operations typically need something that ties directly into the GL, not a standalone file.


## How Truewind Handles Account Reconciliation


Truewind sits on top of QuickBooks Online and Sage Intacct as an AI execution layer, automating the reconciliation work that typically consumes the first several days of close.


When a period ends, Truewind pulls transaction data directly through the GL API, matches entries against source documents, and routes any open items into an exception queue for reviewer sign-off. Your team works through flagged exceptions at the start of close, not a from-scratch rebuild each month.


### What Truewind Automates in the Reconciliation Workflow


- Transaction matching against bank feeds, credit card statements, and sub-ledger records, with dimension-aware posting that preserves class, department, and location tagging already configured in the GL.
- Exception routing so open items surface at the start of review, not buried at the end of a manual spreadsheet pass.
- Reconciliation status tracking across accounts, giving controllers a single view of what is cleared, what is pending, and what needs escalation before the consolidated close can start.
- Journal entry preparation with audit-ready support tied directly to the matched source records.


Final posting decisions stay with your team. Truewind handles the matching, flagging, and queue management so senior accountants spend their close time on judgment calls, not transaction verification.


## Final Thoughts on Account Reconciliation and the Close Process


Most reconciliation problems are structural, not mathematical. A defined schedule, transaction-level matching, and tight documentation on open items remove most of the friction before it compounds into a delayed close. The judgment calls still belong to your team.[See how Truewind handles matching and exceptions](https://www.truewind.ai/see-a-demo) so that's all that's left.


## FAQ


### What is the difference between account reconciliation and bank reconciliation in accounting?


Bank reconciliation is one specific type of account reconciliation: it compares your cash ledger balance against the bank statement to catch timing differences like outstanding checks and deposits in transit. Account reconciliation is the broader process applied across every balance sheet account, from prepaids and accruals to fixed assets and intercompany balances. The terminology matters because close checklists often treat them separately, with bank reconciliation as the first gate and full balance sheet account reconciliation as the broader deliverable.


### What are the main types of reconciliation in accounting, and how do they differ?


The most common types are bank reconciliation, balance sheet account reconciliation, intercompany reconciliation, vendor and customer reconciliation, credit card reconciliation, and payroll reconciliation. Each targets a different layer of the financial record: bank reconciliation clears timing differences against the bank statement; intercompany reconciliation confirms that related-party entries net to zero at the consolidated level; payroll reconciliation ties the payroll register to posted GL liabilities. Which type dominates your close depends on your entity structure, transaction volume, and whether you carry restricted funds, investment accounts, or intercompany debt.


### What causes account reconciliation to take so long during close?


The most common bottlenecks are missing source documents from banks or third parties, manual data entry across disconnected systems, and exception queues that grow faster than preparers can clear them. Preparer and reviewer schedules rarely align, so completed reconciliations sit waiting for sign-off. There is also typically no centralized status view, which forces controllers to chase each owner individually to understand where close stands. The structural problem compounds when organizations add entities or accounts: a team that closes one entity in five days does not close ten entities in five days.


### How does Truewind automate account reconciliation for QuickBooks Online and Sage Intacct users?


Truewind pulls transaction data directly through the GL API, matches entries against source documents, and routes any open items into an exception queue for reviewer sign-off. Your team works through flagged exceptions at the start of close instead of rebuilding the reconciliation from scratch. The matching layer is dimension-aware, preserving class, department, and location tags already configured in the GL. A consolidated reconciliation status view shows which accounts are cleared, which are pending, and which need escalation before the consolidated close can begin. Final posting decisions stay with your team.


### What should a standard account reconciliation template include?


Most reconciliation workpapers share a common structure: beginning balance, activity during the period, adjusting entries, ending balance, and a tie-out to the GL. Free templates in Excel or Word cover the basics for smaller teams, but they work as standalone files with no direct connection to your general ledger, meaning status tracking, reviewer sign-off, and exception documentation all require separate manual steps. Close operations managing multiple entities or high account volumes typically need a workpaper structure that ties to the GL and maintains an audit trail at the transaction level, down to individual line items.
