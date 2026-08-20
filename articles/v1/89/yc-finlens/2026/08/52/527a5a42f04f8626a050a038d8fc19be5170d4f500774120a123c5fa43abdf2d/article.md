---
schema_version: "1.0.0"
document_id: "527a5a42f04f8626a050a038d8fc19be5170d4f500774120a123c5fa43abdf2d"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/how-to-record-business-loan"
published_at: "2026-08-15T00:00:00+00:00"
first_seen_at: "2026-08-16T08:31:25.828538+00:00"
fetched_at: "2026-08-16T08:31:27.308199+00:00"
content_hash: "sha256:166401887e189e9101e27353d0e91be0281a46d4efba0a4dbf6735722c10a548"
---

# How to Record a Business Loan: Journal Entries, Interest Expense, and Amortization

When a business receives loan proceeds, correct entry is a **debit to Cash and a credit to Loans Payable** business now has money in bank and a corresponding obligation on balance sheet. Monthly payments then split between **principal reduction** (which reduces Loans Payable further) and **interest expense** (which hits P&L). Getting principal-vs-interest split right every month is what keeps loan balance on books matching lender's amortization schedule.


This guide covers journal entries for loan receipt, monthly payments, and payoff, plus how loan flows through balance sheet, P&L, and cash flow statement.


## The initial loan receipt entry


When loan proceeds hit business bank account, entry is straightforward:


Debit


Credit


Cash


$100,000


Loans Payable


$100,000


Cash goes up. Loans Payable a liability goes up by same amount. The books balance.


If loan carries **origination fees** , discount points, or other lender-charged costs deducted from proceeds, treat them separately. For example, a $100,000 loan with $2,000 in origination fees actually delivers $98,000 in cash:


Debit


Credit


Amount


Cash


$98,000


Loan Origination Costs


asset, amortized


$2,000


Loans Payable


$100,000


Origination costs get amortized over life of loan as additional interest expense under GAAP (per ASC 470 debt classification rules see[IRS Publication 535 guidance on business expenses](https://www.irs.gov/forms-pubs/guide-to-business-expense-resources) for tax treatment).


## Recording monthly payment


The monthly payment is where accounting gets more careful. Each payment covers both **principal** (reducing loan balance) and **interest** (an expense).


Example: $100,000 loan at 8% annual interest, 5-year term. The monthly payment is approximately $2,028. In first month:


- Interest for month: $100,000 × 8% ÷ 12 = **$667**
- Principal reduction: $2,028 − $667 = **$1,361**


The journal entry for first payment:


Debit


Credit


Amount


Loans Payable


$1,361


Interest Expense


$667


Cash


$2,028


In month 2, loan balance is now $98,639. Interest recalculates on new balance:


- Interest for month: $98,639 × 8% ÷ 12 = **$658**
- Principal reduction: $2,028 − $658 = **$1,370**


Each month, interest portion shrinks and principal portion grows. The mechanics of amortization mean early payments are mostly interest; later payments are mostly principal.


The lender provides an **amortization schedule** at loan origination showing this split for every month of loan. Every monthly journal entry should match amortization schedule if they drift apart, something is being recorded incorrectly.


## Long-term vs. current portion of debt


On balance sheet, GAAP requires splitting loan balance into two lines:


- **Current portion of long-term debt** principal amount due within next 12 months
- **Long-term debt (net of current portion)** principal due beyond 12 months


At end of year 1, $100,000 loan example has a remaining balance of approximately $83,655. Of that, roughly $17,000 will be paid in year 2 (current portion), and $66,655 sits as long-term debt.


Reclassifying at each year-end is a manual entry most bookkeeping systems don't automate:


Debit


Credit


Loans Payable — Long-Term


$17,000


Current Portion of Long-Term Debt


$17,000


This is often missed at year-end close in businesses without accountant oversight, especially difference between[bookkeeping and accounting](https://www.finlens.app/blogs/bookkeeping-vs-accounting) tasks shows up here a bookkeeper records payments but an accountant catches year-end reclassification.


## Interest expense on P&L vs. principal on balance sheet


The distinction that trips up new bookkeepers:


- **Interest expense** hits **income statement** it reduces net income
- **Principal payments** hit **balance sheet** they reduce Loans Payable, not net income


If you record entire monthly payment as interest expense, you're overstating expenses by principal portion. Net income is understated. Taxes are understated. When auditor or CPA reviews at year-end, they'll flag entire loan balance as still outstanding when it should be materially reduced.


If you record entire monthly payment as principal, you're understating expenses. Net income is overstated. Interest expense deduction is missed on tax return.


The clean way to avoid this is to attach amortization schedule inside books (most modern accounting software supports this see[amortization schedule automation](https://www.finlens.app/uses/amortization-schedule-automation) for how split runs automatically) and reconcile loan balance monthly against what lender's statement shows.


## The three financial statements loan touches


**Balance sheet:**


- Cash increases by net proceeds when loan is drawn
- Loans Payable appears as a liability (split into current + long-term)
- Each month's principal payment reduces Loans Payable


**Income statement:**


- Interest expense appears in operating or non-operating expenses each period
- No impact from principal payments


**Cash flow statement:**


- Loan proceeds appear as a positive cash flow in **financing activities**
- Principal payments appear as a negative cash flow in **financing activities**
- Interest payments (if separated) can appear in operating activities


This split is why[reading cash flow separately from profit](https://www.finlens.app/blogs/cash-flow-vs-profit) matters. A business with significant loan payments shows healthy operating cash flow, healthy net income, but a big cash outflow in financing activities loan payoff.


## Paying off loan


When final payment is made, remaining Loans Payable balance should be exactly zero. The final month's entry looks same as every prior month's some interest, some principal but principal portion equals remaining balance.


At payoff:


Debit


Credit


Amount


Loans Payable


$Final principal balance


Interest Expense


$Final interest


Cash


$Final payment


Loans Payable is now zero. The lender releases any UCC filings or collateral liens. The loan is closed.


If business pays off loan early, prepayment penalties (where applicable) are recorded as additional interest expense in year of prepayment.


## Common loan bookkeeping mistakes


**1. Recording entire monthly payment as interest.** Understates net income, overstates expense, misses balance sheet effect entirely.


**2. Not tracking amortization schedule.** Without a schedule, split between principal and interest becomes guesswork. Modern accounting software or a spreadsheet reconciled monthly against lender's statement is minimum standard.


**3. Missing year-end current/long-term reclassification.** Both amounts are correct in total, but balance sheet misclassification causes issues in ratio analysis, loan covenants, and audit review.


**4. Not amortizing loan origination fees.** These reduce cash at loan origination but are properly recognized as additional interest expense over loan life not entirely in year 1.


**5. Confusing loan proceeds with revenue.** Loan proceeds go to Loans Payable (a liability), NOT to revenue. Revenue is earned; loan proceeds are borrowed. This mistake happens most often in QuickBooks or Xero when bank deposit isn't categorized correctly.


**6. Recording loans from owners incorrectly.** A loan from business owner is a liability (Loans from Shareholder or similar), not equity or revenue. When repaid, it comes off liability side just like a third-party loan.


For firms managing many client books, clean[monthly reconciliation](https://www.finlens.app/blogs/reconciliation-in-accounting) is what catches these errors early. When loan balance in books matches lender's statement every month, amortization is correct.


## Conclusion


**Debit Cash, credit Loans Payable at loan receipt. Every monthly payment splits into principal (reducing liability) and interest (hitting P&L). Follow amortization schedule.** Get these two flows right and loan tracks correctly through balance sheet, income statement, and cash flow statement. Get them wrong most commonly by treating entire payment as interest and every downstream number is off.


## FAQ


### How do I record a business loan in QuickBooks or Xero?


When loan is received: create loan as a long-term liability account, then record bank deposit as a transfer from Cash to that liability account. Monthly payments split between liability principal reduction and Interest Expense. Match to lender's amortization schedule.


### Is a business loan considered income?


No. Loan proceeds are a liability, not income. You borrowed money you don't own it and you have to pay it back. It's not taxable when received and not deductible when repaid. Only interest portion of payments is deductible.


### How do I record loan payments in accounting?


Each payment is split between principal (debit Loans Payable, credit Cash) and interest (debit Interest Expense, credit Cash). The split follows amortization schedule provided by lender. Early payments are mostly interest; later payments are mostly principal.


### Is loan interest tax deductible for a business?


Yes, business loan interest is generally deductible on tax return, subject to some limitations (§163(j) business interest limitation for larger taxpayers). Report as an expense on Schedule C, Form 1120, Form 1120-S, or Form 1065 depending on entity type.


### What's difference between short-term and long-term debt?


Short-term (current portion) is due within 12 months of balance sheet date. Long-term is due beyond 12 months. The distinction matters for balance sheet classification and for ratios like current ratio and working capital.


### Do I record full loan amount or just amount I've drawn?


Only what you've actually drawn. For a line of credit, only drawn balance is a liability. For a term loan received in full at closing, full amount is recorded when received.


### How do I record loan origination fees?


Debit a Loan Origination Costs asset account for fee amount. The fees are then amortized over loan life as additional interest expense usually straight-line for simplicity, or effective interest method for more precise GAAP compliance.


### What if I pay off loan early?


The final payment's principal portion equals whatever balance remains. Any prepayment penalty is recorded as additional interest expense in year of prepayment. Once paid, Loans Payable equals zero and account can be closed.
