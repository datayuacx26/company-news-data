---
schema_version: "1.0.0"
document_id: "3ead3dafcb9698114a50085df9057d516b428ca0a81e9ac54b720806e52d6175"
company_key: "yc-syntheticfi"
company: "SyntheticFi"
source_id: "yc-syntheticfi-news-import-d32c9f12b13e"
canonical_url: "https://www.syntheticfi.com/blog/tax-deductibility-of-box-spreads"
published_at: "2025-04-24T00:00:00+00:00"
first_seen_at: "2026-07-22T23:25:32.275748+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:cd7275c8ef813ea01ec1ecedcf60d604015da22540e0d137795ae45e8b2dec09"
---

# Tax Deductibility of Box Spreads

## Interest Expenses as a Capital Loss


SyntheticFi's lending program is based on an options strategy that uses box spreads on S&P 500 (SPX) Index Options. This means that interest expenses on SyntheticFi loans can be written off as a **capital loss** (60% long-term and 40% short-term). So if a client has an open box spread position at year-end, they can claim capital losses based on the occurred interest on a mark-to-market basis.


### How It Works: Section 1256 Contracts


S&P 500 (SPX) Index Options, the building blocks behind SyntheticFi loans, are considered[Section 1256 contracts](https://www.law.cornell.edu/uscode/text/26/1256) by the IRS. SPX options fall under the category of non-equity options because they are based on a broad-based index (the S&P 500) and are cash-settled, meaning they do not involve the delivery of individual securities upon expiration. This classification aligns them with the types of instruments specified in Section 1256 of the[Internal Revenue Code](https://www.law.cornell.edu/uscode/text/26/1256) .


The Section 1256 contracts are marked-to-market at the end of every year, which means contract owners can claim a loss on these contracts even if the contract is still outstanding. So when clients borrow money through our program, they will receive the principal when opening the trade, and pay the principal plus interest at settlement time. However, each year the loan is unpaid, they will incur a loss equal to the interest expense. So these losses can be written off each as capital losses—a mix of 60% long-term and 40% short-term.


> If Tony borrowed $500K against his portfolio through SyntheticFi to purchase a car, while incurring $25K in interest by the end of the year, he can deduct $25K × 60% = $15K from his long-term capital gains and $25K × 40% = $10K from his income (short-term capital gains is taxed at income rates).


### Fixed Interest Loans - Balloon Interest Payment, but Annual Deductions


Multi-year[fixed-rate](https://www.syntheticfi.com/blog/fixed-rates) box spread loans do not incur interest until maturity (i.e. the underlying Section 1256 contracts expire). If the loan is outstanding through the end of the tax year, these clients will still claim a capital loss: for tax purposes, custodians will use the fair market value of the loan to calculate the losses on those contracts at the end of each year, which will equal the accrued interest for that year.


> If Tony elected to borrow the $500K at an annual fixed rate of 5% for a 3-year term, then after the end of the first year, the mark-to-market value of his loan will likely be $525K ($500K + 5%). $15K (60% of $25K) will be treated as a long-term capital loss and $10K (40% of $25K) will be treated as a short-term capital loss (i.e. income deduction).


> At the end of the loan period, Tony would make a lump sum repayment of $575K. But for the final tax year, Tony would only claim $25K in capital losses (60% long-term and 40% short-term).


## Tax Reporting


Custodians such as Schwab and Fidelity will report the gains and losses on Section 1256 contracts as part of the consolidated 1099 package. Read more from[Schwab](https://www.schwab.com/learn/story/form-1099-b-cost-basis-and-options-trading) .


Reporting for gains and losses on Section 1256 contracts are supported by major tax reporting software, such as TurboTax, and can be easily handled by a professional tax preparer.


## Conclusion


Box spreads allow for the deduction of interest expenses as capital losses. Under Section 1256, these contracts are marked-to-market at the end of each year, enabling borrowers to claim annual losses even if the loan remains unpaid. The losses are divided into 60% long-term and 40% short-term capital losses. This tax treatment is supported by major custodians and tax reporting software, simplifying the process for investors and tax professionals. (Learn about how the differences in taxation allow clients to take advantage of[tax-aware borrowing strategies through SyntheticFi](https://www.syntheticfi.com/blog/the-tax-benefits-of-syntheticfi-securities-backed-loans) .)
