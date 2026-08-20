---
schema_version: "1.0.0"
document_id: "1dfbb363600216dae98433a4abeeebcf6a4fc711cc8b8707ca4e917724a430b5"
company_key: "yc-general-legal"
company: "General Legal"
source_id: "yc-general-legal-news-import-cc42147fa719"
canonical_url: "https://general.legal/blog/illinois-joins-the-bnpl-map"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-21T21:25:58.163138+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:064e224a50125b53b0e4fcd3e7f1127c7e26e03a28878ad352c51101e35aa658"
---

# Illinois Joins the BNPL Map: A Full Compliance Roadmap

On June 25, 2026, Illinois Gov. JB Pritzker signed the Buy-Now-Pay-Later Loan Consumer Protection Act (SB3561 (#)) (the "Act"), making Illinois the second state, after New York's comprehensive 2025 law, to put BNPL lending under a dedicated regulatory regime. California previously required licensing, but Illinois now joins New York with a full set of substantive protections. The law takes effect immediately, but providers have until January 1, 2028, to come into compliance. If your product offers "pay-in-4" or similar installment plans to Illinois consumers, the runway to operationalize these requirements starts now. Registration is a prerequisite to lending, so treat 2028 as the deadline to be fully licensed and operationally ready, not the date to begin planning.


## The Basics


The Act reaches closed-end loans of four or fewer installments or a term of 120 days or less, covering both interest-free "pay-in-4" products and BNPL loans that carry interest or finance charges and servicers of such loans.


**Key requirements:**


- State licensing. Providers must obtain a license from the Illinois Department of Financial and Professional Regulation (IDFPR), administered by its Division of Financial Institutions, before lending, even for zero-interest products. A $5,000 license fee, a $50,000 surety bond, and annual renewal apply.
- 36% APR cap. Illinois's 36% all-in rate cap under the Predatory Loan Prevention Act now applies to BNPL loans.
- Fee authority. IDFPR may limit late fees and other charges, and the Act bars tips and expedited-payment "junk" fees.
- Enforcement teeth. A violation is an unlawful practice under the Illinois Consumer Fraud and Deceptive Business Practices Act, enforceable by the Attorney General, and loans made by an unlicensed, non-exempt lender are null and void.


## Why a Bank Partner Won't Automatically Save You


While the Act exempts banks, savings banks, savings and loan associations, credit unions, and insurance companies, it makes clear that having a bank partner does not, by itself, exempt you.


The Act includes a broad anti-evasion and "true lender" framework: a person is a lender subject to the Act "regardless of any claim that the person is acting as an agent, service provider, or in another capacity" for an exempt entity if any one of three tests is met.


- Predominant economic interest test. You are the lender if you hold, acquire, or maintain, directly or indirectly, the predominant economic interest in the loan. In a typical forward-flow arrangement where the fintech buys the receivables and bears the economic upside and risk, this prong is hard to avoid.
- The "operational control" test. This captures anyone who markets, brokers, arranges, or facilitates the loan and holds the right, requirement, or first right of refusal to purchase the loan or receivables—which describes most bank-partnership BNPL structures almost verbatim.
- Totality-of-the-circumstances test. Unlike the first two prongs, this one requires a finding that the deal is structured to evade the Act, but it lists non-exclusive factors that count against you—and the first applies to most deals: the person "indemnifies, insures, or protects an exempt person or entity for any costs or risks related to the loan." Nearly every bank-partnership agreement contains exactly this indemnification.


> Practical takeaway: If you rely on a bank partner, pressure-test your structure against all three Section 5(e) tests now. The indemnification clause that protects your bank partner may be the very provision that makes you the lender.


## A Shift Toward Traditional Lending


The Act pulls BNPL toward the obligations that already apply to mainstream consumer credit. The substantive requirements fall into fourcriteria, or trade secrets, but it plainly signals an ability-to-repay standard. Notably, lenders may not base creditworthiness on the credit standing of members of a borrower's social network or on any group score.


Disclosures. At the time of a specific offer, lenders must clearly and conspicuously disclose the cost (interest and fees), the repayment schedule, how to dispute charges, whether the loan is reported to credit bureaus, how to file a complaint with IDFPR, and the underwriting factors considered, consistent with Regulation Z and the federal Truth in Lending Act. The lender's license must also be posted on its app, website, or other consumer interface.


Disputes and refunds. Lenders must extend to BNPL borrowers the same dispute and unauthorized-charge rights that apply to credit cards under the Truth in Lending Act, regardless of whether that law would otherwise apply, and must maintain fair, transparent, and not-unduly-burdensome processes for disputes, refunds, and credits, with a readily available method for raising them.


Payment practices. Lenders may not require autopay (and may not charge to cancel it if a consumer elects it), may not require payment by credit card, and may not re-present an ACH debit more than twice or after learning the account has insufficient funds. Consumers may prepay at any time with no charge beyond interest accrued, and lenders may not collect tips or expedited-payment fees.


## Practical Steps for BNPL Providers


The steps below are a starting point, not a complete compliance plan:


- Confirm whether your installment products fall within the four-payment / 120-day definition; map each SKU of credit you offer in Illinois.
- File for IDFPR licensing well ahead of Jan. 1, 2028, including for zero-interest plans, and budget for the fee and surety bond.
- If you use a bank-partner model, run your program through all three Section 5(e) tests (economic interest, true lender, totality of circumstances), and review your indemnification provisions specifically.
- Re-paper consumer disclosures to surface every cost and fee, and verify your effective APR (with all fees) stays at or below 36%.
- Stand up or upgrade ability-to-repay underwriting, document the methodology, and confirm you are not using social-network or group-score data.
- Build credit-card-grade dispute and refund workflows, including merchant chargeback coordination.
- Remove mandatory-autopay conditions, the credit-card-payment requirement, and add NSF / two-attempt re-debit limits to your payment logic.


## Practical Steps for Retailers & E-Commerce Offering Pay-in-4


There is a meaningful merchant carve-out. A merchant or platform that simply makes BNPL available to consumers through a licensed lender or exempt entity is not covered; so long as it does not originate, underwrite, service, or hold an ownership interest in the loans. The exemption breaks the moment you take on any of those functions. (A related carve-out protects genuinely passive investors who don't control origination or servicing.) The steps below are a starting point, not a complete compliance plan:


- Confirm you fit the merchant carve-out: you offer BNPL through a licensed/exempt lender and do not originate, underwrite, service, or hold an ownership interest in the loans.
- Determine who the lender of record is on your checkout flow; in-house or balance-sheet programs likely trigger the Act.
- If you partner with a BNPL provider, confirm in writing that they will hold the Illinois license and own compliance, and review indemnification terms in light of Section 5(e).
- Review checkout disclosures, autopay defaults, and refund handling for Illinois shoppers.
- Flag the 2028 date to product, legal, and payments teams now—builds of this kind take quarters, not weeks.


## A Growing Multi-State Patchwork


With California, New York, and now Illinois moving, and consumer advocates circulating model language for other states, BNPL is shifting from a lightly regulated checkout convenience to a state-licensed lending product. Illinois consciously built on New York's framework, and advocates are encouraging additional states to adapt the same template, so the near-term trajectory points toward more states, not fewer. Expect divergent definitions, fee rules, disclosure formats, and language-access requirements across jurisdictions, especially with federal oversight in flux. Building to the strictest common denominator now, rather than the minimum any single state requires, is usually cheaper than retrofitting your stack state by state as each new law lands.


## How We Can Help


We offer a fixed-scope BNPL compliance gap assessment: mapping your products and existing contracts against the Illinois Act (and the NY/CA regimes), identifying licensing triggers and exposure, and delivering a prioritized 2028 remediation roadmap. Reach out to start a scoping call.


---
*Disclaimer: This alert is for general informational purposes only and does not constitute legal advice, nor does it create an attorney-client relationship. Laws change and application depends on specific facts; consult qualified counsel before acting.*


*Sources: Buy-Now-Pay-Later Loan Consumer Protection Act (SB3561, Enrolled); Payments Dive | NCLC*
