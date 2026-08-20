---
schema_version: "1.0.0"
document_id: "9f6d4a363c6774dbce76c230ef0ed08dc43a087de29c8b331dd200b5d439f4f6"
company_key: "yc-truewind"
company: "Truewind"
source_id: "yc-truewind-rss-014af42b96d1"
canonical_url: "https://www.truewind.ai/blog/accrual-journal-entries-month-end"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.745562+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:9da3c7fce73fd2e988e781d8e5fee2fd868b795cc1b5552f7f1c2c4813e04805"
---

# Accruals in Accounting: How They Post and Where Close Risk Hides (July 2026)

The accruals concept in accounting is straightforward in theory: record revenue and expense in the period they belong to, not the period cash moves. In practice, accrual accounting entries pile up at month-end, reversals miss their schedule, and estimates posted on day one of close are sometimes wrong by the time the actual invoice lands on day ten. If you're working through how accruals work in accounting, what the difference between cash basis and accrual basis looks like in real journal entries, or why year-end treatment of accruals creates audit exposure, the mechanics are worth understanding at the transaction level. That's what this post covers.


**TLDR:**


- Accruals record revenue or expense when earned or incurred, not when cash moves, enforcing the matching principle across periods.
- Four accrual types drive most close activity: accrued expenses, accrued revenue, prepaids, and deferred revenue, each posting differently to the GL.
- GAAP requires accrual basis for audited financial statements; the IRS generally requires it above $29 million in average annual gross receipts (IRC §448(c)).
- Month-end accrual risk comes from volume and coordination gaps, not conceptual difficulty; missed reversals and stale estimates compound across the close window.
- Truewind drafts accrual journal entries from historical GL patterns via API, routes them for human review, and posts directly to QuickBooks Online or Sage Intacct.


## What an Accrual Is


An accrual is the recognition of revenue or expense in the period it is earned or incurred, regardless of when cash moves.


The matching principle is what makes this necessary. Revenue and the costs that generate it belong in the same reporting period. Pay a vendor in January for work completed in December, and the expense belongs in December's books. A customer owes you for March services but pays in April, and March is when the revenue posts. Accruals are the mechanism that enforces this alignment between economic activity and the period it reflects.


### How This Differs From Cash Basis Recording


Under[cash basis accounting](https://www.truewind.ai/blog/cash-vs-accrual-how-to-help-clients-choose-the-right-accounting-method) , the transaction date is the recording date. No cash, no entry. Accrual basis accounting breaks that link entirely. The entry follows the economic event, not the bank statement. That distinction matters most at period-end, when accrued expenses and accrued income sit on the balance sheet as liabilities and receivables, waiting for cash settlement in a future period.


## The Accrual Journal Entry: How It Posts and Reverses


Accrual journal entries follow a two-step pattern: record the obligation when it's earned or incurred, then reverse or settle it when cash moves.


A standard accrual posts as a debit to an expense or revenue account and a credit to an accrued liability or accrued asset account on the balance sheet. When payment is made or received in the following period, the entry flips: the balance sheet account clears and cash posts on the other side.


### How a Common Accrual Looks in Practice


Take a December payroll that pays out in January. At December 31: debit Salaries Expense $50,000, credit Accrued Salaries Payable $50,000. When payroll runs in January: debit Accrued Salaries Payable $50,000, credit Cash $50,000. The liability clears and cash posts on the other side.


### The Reversal Question


Many teams post an automatic reversal on the first day of the new period. The reversal credits the expense account and debits the liability, netting to zero before the actual payment posts. This prevents double-counting but requires the subsequent cash entry to hit the same expense account cleanly.


Whether to reverse depends on the nature of the accrual. Recurring estimates like[payroll and interest accruals](https://www.truewind.ai/blog/reconciling-payroll-registers-sage-intacct) are well-suited to automatic reversals. Non-recurring items, like a one-time legal fee accrual, often need manual clearance and review before the balance sheet account closes out.


## Types of Accruals in Accounting


Four categories drive most of the accrual activity in a close cycle, and each posts differently in the journal and on the balance sheet.


### Accrued Expenses


These are costs incurred but not yet paid or invoiced. Wages earned by employees through the end of the month, interest that has accumulated on a loan, and utility charges for services already consumed all fall here. The liability sits on the balance sheet until cash goes out.


### Accrued Revenue


Revenue earned but not yet billed or collected. A consulting firm that completes work in June but invoices in July records the earned amount as accrued revenue in June, keeping income aligned with the period it was generated.


### Prepaid Expenses


Cash paid before the expense period arrives. An annual insurance premium paid in January gets recognized ratably across twelve months, not all at once in the month of payment. See how the[prepaid expense schedule](https://www.truewind.ai/blog/prepaid-expenses-schedule-and-journal-entries) works and where it breaks.


### Deferred Revenue


Cash received before the related service or product is delivered. A SaaS subscription billed annually sits as a[deferred revenue](https://www.truewind.ai/blog/sage-intacct-deferred-revenue-automation) liability until each month of service is actually provided, at which point it gets recognized into revenue.


## Accrual Basis vs. Cash Basis Accounting


Accrual accounting records revenue when it is earned and expenses when they are incurred, regardless of when cash changes hands. Cash basis accounting records both only when cash is received or paid. That single difference in timing produces two entirely different pictures of a company's financial position.


### When the Gap Between the Two Methods Matters Most


Most small businesses start on cash basis because it is simple. As they grow, the mismatch between cash flow and economic activity becomes harder to ignore.


Consider a SaaS company that invoices a client $60,000 in December for a full year of service. Under cash basis, if that invoice goes unpaid until January, December looks like a flat month. Under accrual basis, $5,000 of revenue posts in December and each subsequent month, matching the period in which the service is delivered.


The core tradeoffs break down this way:


Accrual Basis Cash Basis


Revenue recognition When earned When cash is received


Expense recognition When incurred When cash is paid


Financial picture Matches economic activity Reflects cash position


GAAP compliant Yes No (for most entities)


Complexity Higher Lower


[GAAP requires accrual basis](https://www.investopedia.com/ask/answers/011315/why-does-gaap-require-accrual-basis-rather-cash-accounting.asp) for any entity that issues audited financial statements, and the[IRS generally requires it](https://www.irs.gov/pub/irs-pdf/p538.pdf) for businesses exceeding $30 million in average annual gross receipts.


## How Accruals Show Up in Nonprofits


Nonprofits run on grant cycles, donor pledges, and restricted funding streams, and each of those creates accrual timing mismatches that general-purpose accounting guidance doesn't fully cover.


A few patterns show up consistently across nonprofit close cycles:


- Pledges receivable are recognized as revenue when the donor makes a commitment, not when the cash arrives. If a donor pledges $50,000 in June but pays in September, June's financials carry the accrued revenue and a matching receivable on the balance sheet.
- [Grant reimbursements](https://www.truewind.ai/blog/sage-intacct-nonprofit-grant-accounting) often require expenses to be incurred before revenue can be recognized. That means payroll and program costs post in one period while the reimbursable grant income accrues separately, waiting on the drawdown request.
- Deferred revenue from multi-year grants or membership dues requires careful period allocation. Cash received upfront gets recognized only as the performance obligation or time restriction is satisfied.


What makes nonprofit accruals particularly risky at month-end is the restriction layer. Under FASB ASC 958, revenue must be classified as net assets with donor restrictions or net assets without donor restrictions. An accrual posted to the wrong net asset class does more than misstate income; it misrepresents the organization's legal obligation to donors and can create compliance exposure, a gap that[Sage Intacct fund accounting](https://www.truewind.ai/blog/sage-intacct-fund-accounting-missing-features) tools may not catch during audit. That's a harder error to catch than a simple timing difference, and it's one that manual close processes tend to surface late.


## How Accruals Show Up in Family Offices


Family offices carry accrual complexity that most general accounting guides never touch. The asset mix alone creates it: private equity capital calls get accrued before cash leaves the account, carried interest accrues against fund performance before any distribution, and management fees owed to external advisors sit as liabilities until invoiced. None of these follow a simple invoice-and-pay cycle.


A few patterns show up consistently across family office books:


- Capital call accruals require matching the capital call notice to the correct investment entity and vintage, often across multiple fund relationships running on different reporting cycles, a workflow closely tied to[investment rollforwards for family offices](https://www.truewind.ai/blog/rollforward-gl-automation) .
- Advisory fee accruals depend on AUM calculations that may not be finalized until after period-end, leaving an estimated liability on the books that gets trued up the following month.
- Income accruals on fixed income or private credit positions require the team to calculate days-accrued interest manually when custodian statements arrive late or in non-standard formats, a recurring pressure point that drives the case for a[family office external reconciliation layer](https://www.truewind.ai/blog/family-office-external-reconciliation-sage-intacct) .


Month-end in a family office context means holding all of this open while waiting on third-party data. That waiting period is where accrual risk concentrates. An estimate posted in good faith on day one of close can be materially wrong by the time the actual statement arrives on day ten.


## Why Accruals Create Close Risk


Accruals introduce timing gaps between when work happens and when it gets recorded, and those gaps compound at month-end in ways that create real close risk.


The core problem is volume and dependency. A single close cycle can require dozens of accrual entries across payroll, interest, vendor invoices, and revenue, each one dependent on information that may not arrive until after the books are nominally due. When that information is late, incomplete, or estimated differently than last month, the entries are wrong before they are even posted.


Several failure points surface consistently:


- Estimates made without current data get posted and reversed the following period, creating a recurring correction cycle that inflates both periods and obscures the real run rate.
- Reversals fail to post on schedule because no one owns the[close checklist](https://www.truewind.ai/blog/building-a-close-checklist-that-actually-works) item, leaving a prior-period accrual sitting alongside the current one and doubling the liability balance.
- Cutoff errors place an expense or revenue item in the wrong period, triggering a restatement or an auditor question that costs more time to explain than the entry itself took to post.
- Accruals built from last month's numbers get carried forward without adjustment, silently misrepresenting the current period whenever the underlying activity has changed.


The risk is not that accruals are conceptually difficult. It is that they require coordinated information, consistent ownership, and timing discipline across a compressed close window, and most teams manage that coordination manually.


## Accruals FAQ


Accruals in accounting follow a consistent set of mechanics, but the application questions come up repeatedly. Here are the ones that surface most often.


### What is the difference between an accrual and a deferral?


An accrual records revenue or expense before cash moves. A deferral records cash that has already moved but where the revenue or expense recognition hasn't happened yet. Prepaid insurance is a classic deferral: cash went out, but the expense posts over the coverage period.


### Do accruals affect cash flow?


Accruals affect the income statement and balance sheet, not cash. The cash flow statement adds back non-cash items and working capital changes, which is exactly why net income and operating cash flow often diverge.


### When do accruals reverse?


Most accruals reverse in the first period after the close, once the actual invoice or payroll run posts. Reversing entries prevent double-counting and keep the GL clean without requiring manual cleanup.


### Are accruals required under GAAP?


Yes. GAAP requires accrual basis accounting for any entity issuing financial statements under its standards. Cash basis may work for tax purposes or small internal reporting, but it does not comply with GAAP or IFRS.


## How Truewind Handles Accruals at Month-End


Truewind sits directly on top of QuickBooks Online and Sage Intacct at the API level, which means accrual entries move from preparation to posting without a manual handoff.


When month-end arrives, Truewind drafts accrual journal entries based on historical GL patterns and transaction data it has already read. Your team reviews each proposed entry in a single interface, approves or adjusts, and posts directly to the GL. No spreadsheet staging. No copy-paste into the ledger.


The workflow covers the entries that cause the most month-end friction:


- Accrued expenses where vendor invoices haven't landed yet, built from recurring payment patterns Truewind has already learned.
- Accrued revenue where services have been delivered but billing hasn't gone out, flagged against open customer activity in the GL.
- Prepaid amortization schedules that Truewind tracks period by period and[rolls forward automatically](https://www.truewind.ai/blog/rollforwards-in-accounting) , so the monthly expense hits without a manual reminder.


Human reviewers stay in the loop at every step. Truewind prepares; your team approves. Final posting authority stays with the accountant, which keeps the control environment intact and the audit trail clean.


For teams running multiple entities, this matters more than it might appear. Each entity carries its own accrual population, and manually tracking which entries are posted, which are pending, and which need reversal next period compounds quickly. Truewind surfaces close status by entity so nothing gets dropped between periods.


## Final Thoughts on Accrual Accounting and How It Works


Accruals reflect economic reality, not cash timing, and that distinction is what makes financial statements actually useful. The mechanics follow a clear pattern: record when earned or incurred, settle when cash moves. Where your close process gets harder is managing that pattern across dozens of entries, multiple entities, and data that doesn't always arrive on schedule.[See a Truewind demo](https://www.truewind.ai/see-a-demo) to watch the accrual review and posting workflow in action.


## FAQ


### What is accrual accounting in simple terms?


Accrual accounting records revenue when it is earned and expenses when they are incurred, regardless of when cash moves. A December payroll that pays out in January still posts to December's books because that is when the work happened.


### Accrual basis vs. cash basis accounting: which method should my company use?


For any entity issuing GAAP-compliant or audited financial statements, accrual basis accounting is required. Cash basis is simpler and works for small businesses or internal reporting, but it does not comply with GAAP or IFRS, and the IRS generally requires accrual basis once average annual gross receipts exceed $30 million. The practical difference shows up most at period-end, when the two methods can produce materially different pictures of revenue, expenses, and financial position.


### How do accruals work in accounting at month-end, and why is that the riskiest point in the close?


Each accrual entry depends on information that may not arrive until after the books are nominally due: vendor invoices, payroll confirmations, custodian statements. When that data is late or estimated, entries post with incorrect figures, reversals miss their schedule, and cutoff errors place activity in the wrong period. The risk is not conceptual difficulty; it is coordinating volume, timing, and ownership across a compressed close window where most teams are still working manually.


### What are the main types of accruals in accounting?


There are four: accrued expenses (costs incurred but not yet paid, such as wages or interest), accrued revenue (income earned but not yet billed), prepaid expenses (cash paid before the expense period, recognized ratably), and deferred revenue (cash received before the related service is delivered, held as a liability until earned). Each posts differently in the journal and appears differently on the balance sheet.


### How do accrual journal entries reverse, and when should reversals be automatic?


Most accruals reverse on the first day of the new period: the entry credits the expense account and debits the liability, netting to zero before the actual payment posts. Recurring estimates like payroll accruals and interest accruals are well-suited to automatic reversals. Non-recurring items, such as a one-time legal fee accrual, typically need manual clearance and review before the balance sheet account closes out, because the underlying amount may differ from the original estimate.
