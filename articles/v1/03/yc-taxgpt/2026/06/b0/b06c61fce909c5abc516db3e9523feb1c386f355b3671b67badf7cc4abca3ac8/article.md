---
schema_version: "1.0.0"
document_id: "b06c61fce909c5abc516db3e9523feb1c386f355b3671b67badf7cc4abca3ac8"
company_key: "yc-taxgpt"
company: "TaxGPT"
source_id: "yc-taxgpt-rss-7837c4d6eefc"
canonical_url: "https://www.taxgpt.com/blog/form-1098-checklist"
published_at: "2026-06-17T13:31:15+00:00"
first_seen_at: "2026-07-26T01:35:33.382394+00:00"
fetched_at: "2026-07-28T20:48:41.890548+00:00"
content_hash: "sha256:aed9aeb4e20e1a9cc2d45b0f8e0750ac5f1f3aefb2c15a6e6092eeb60ebd5f05"
---

# Form 1098 Checklist for Tax Professionals

### Form 1098 looks simple, but it is full of traps


Form 1098 looks simple, but it touches multiple moving pieces: who must file, what counts as interest, how to handle points, mortgage insurance premiums, multiple borrowers, foreign parties, and more. A small mistake can turn into notices, penalties, and amended returns for you and your borrowers.


This guide turns the technical rules into a practical, repeatable workflow you can use in your practice.


It is designed for:


- Mortgage lenders and servicers
- Banks and credit unions
- Developers or businesses that finance property sales
- Cooperative housing corporations
- Governmental units


Ask[TaxGPT Research](https://www.taxgpt.com/tax-research-software) to pull the latest Form 1098 authority, examples, and box-level instructions, then operationalize those rules in your internal workflows.


## Form 1098 checklist


### 1. Confirm you are required to file Form 1098


You must file Form 1098 if both of the following apply:


- You are engaged in a trade or business (including governmental units), and
- In the course of that trade or business, you received at least $600 of reportable mortgage interest during the calendar year from an individual (including a sole proprietor) on any one mortgage.


Key points:


- The $600 threshold applies per mortgage, not per borrower and not across multiple mortgages for the same borrower.
- You can be required to file even if you are not a traditional lender, for example, a real estate developer that finances home sales in the ordinary course of business.
- Collection agents and servicers generally file as the first recipient to receive the interest, unless a narrow exception applies, such as where you lack necessary information and the owner receives the interest in its own trade or business.
- Governmental units must file without regard to the trade or business test.
- Cooperative housing corporations must file for the tenant-stockholder’s proportionate share of interest under section 216.


You can ask[TaxGPT Research](https://www.taxgpt.com/tax-research-software) to confirm whether your specific fact pattern (developer financing, related-party loans, co-op structures) falls within the filing requirement and to review IRS examples and regulations.


### 2. Verify that the obligation is a mortgage


You report on Form 1098 only for interest received on a mortgage. A mortgage means:


- Any obligation secured by real property, including manufactured homes as defined in section 25(e)(10).


Additional points:


- Real property is land and generally anything built on it, growing on it, or attached to the land.
- Lines of credit and even credit card obligations can be mortgages if they are secured by real property.
- The payer of record is the individual carried on your books as the principal borrower. If your system does not identify a principal borrower, you must designate one.
- If a subsequent purchaser assumes the loan without releasing the original borrower, the subsequent purchaser becomes the new payor of record for reporting purposes.


### 3. Determine what counts as reportable interest and the correct reporting year


Form 1098 Box 1 generally captures mortgage interest received from the payer of record. This is a common area for errors.


Included as interest when applicable:


- Regular mortgage interest, other than points
- Certain prepayment penalties that are treated as interest
- Certain late charges that are interest rather than service fees


You have to evaluate on a mortgage-by-mortgage basis whether a charge is interest or a fee.


Excluded from Box 1:


- Amounts paid by sellers, for example:


- Rate buy downs
- Lump sum amounts placed in escrow to subsidize interest payments


- These are not interest received from the payer of record and should not be included in Box 1.
- Governmental housing assistance payments. Only the portion paid by the borrower above the assistance is interest for Box 1.


You may optionally disclose seller-paid interest or points in Box 10 “Other” to help the borrower understand their records, but these amounts are not Box 1 interest.


Timing, calendar year reporting:


- Generally, report interest on a calendar year basis for the year in which it is received.
- However, report prepaid interest (other than points) only in the year in which it properly accrues.:


- Example: Interest received on December 20, of the current year, that accrues by December 31, of the current year, but is not due until January 31, of the following year, is reportable on the current year Form 1098.


- Interest received during the current year that will accrue in full by January 15 of the following year may be considered received in the current year and is reportable on Form 1098 for the current year. If any part of an interest payment accrues after January 15, then only the amount that properly accrues by December 31 of the current year is reportable on Form 1098 for the current year.


### 4. Points – Box 6


Points are a high-risk area because taxpayers rely heavily on Form 1098 to claim deductions.


Report in Box 6 only points that meet all of the regulatory conditions, including that they:


- Are clearly designated as points on the closing or settlement statement
- Are computed as a percentage of the principal amount of the mortgage
- Conform to established practice in the locality
- Are paid directly by the payor of record, including certain seller-paid points treated as paid by the borrower
- Relate to the purchase of the payor’s principal residence and are secured by that residence


These reportable points must be reported in the year of the closing, regardless of the accounting method used.


Do not report in Box 6 points that are:


- Paid for home improvements
- Related to refinances, except where certain requirements rfor construction loans are met
- Related to second homes, investment properties, or home equity loans or lines
- Actually paid as service fees, such as appraisal fees, inspection fees, title fees, attorney fees, and property taxes


If points are received by a mortgage broker, the lender of record generally has the reporting obligation.


### 5. Complete Form 1098, box by box


Once you have confirmed a filing obligation and identified the mortgage and reportable amounts, complete the form as follows for mortgages that meet the threshold.


Box 1, Mortgage interest received from payor(s) or borrower(s)


- Include only interest paid by the payor of record.
- Exclude seller-paid interest and governmental assistance amounts.


Box 2, Outstanding mortgage principal


- Report the principal balance at the beginning of the year or as of the mortgage acquisition date, based on the instructions.


Box 3, Mortgage origination date


- Use the date the mortgage was originated, not the purchase date on the secondary market.


Box 4, Refund of overpaid interest


- Report reimbursements or credit of prior-year(s) interest.


Box 5, Mortgage insurance premiums


- Report mortgage insurance premiums only if reporting applies for that year under section 163(h)(3)(E).


Box 6, Points on purchase of principal residence


- Include only those points that satisfy the criteria described in section 4 of the checklist.


Box 7 / 8, Address or description of property securing the mortgage


- If the address of the property securing the mortgage is the same as the payer’s/borrower’s mailing address either check box 7 or provide a clear address or legal description in box 8.
- If the address of the property securing the mortgage is not the same as the payer’s/borrower’s mailing address, enter the street address (including the apartment number) of the property securing the mortgage in box 8.
- If multiple properties secure the loan, follow the instruction guidance.


Box 9, Number of mortgaged properties


- If there is more than one property securing the mortgage, indicate the number of properties in box 9.


Box 10, Other


- Use for optional disclosure of seller-paid amounts or other information you choose to provide.


Box 11, Mortgage acquisition date


- If you acquired the mortgage in the calendar year, enter the date of acquisition.


You can convert this box-by-box logic into internal checklists, so staff capture all required data consistently before year-end processing.


### 6. Identify the correct filer and payor information


Form 1098 must clearly identify both the interest recipient and the payer of record.


Interest recipient or qualified person:


- Name, address, and TIN or EIN are required
- A qualified person, for example a servicer, can be designated by written agreement to file and furnish statements
- Keep designation agreements for at least 4 years


Payer or borrower (payer of record):


- Name, address, and TIN
- The payor of record is the principal borrower on your books, and if that is not indicated you must designate one


### 7. Solicit and manage TINs properly


To avoid possible penalties, you need to show reasonable efforts to obtain TINs.


- If you do not have a TIN on file, request it using Form W-9 (for U.S. persons) or a W-8 series form (for foreign persons).


Truncation rules:


- You may truncate the payee’s TIN on statements furnished to recipients.
- You may not truncate any TIN or EIN on forms filed with the IRS.
- You may not truncate your own TIN on recipient statements.


### 8. Special situations to evaluate


Form 1098 becomes more complex when parties or property fall outside of a straightforward United States individual borrower scenario.


Nonresident alien payors:


- Reporting is generally not required if the payor is a nonresident alien and the security for the mortgage is real property outside of the United States.
- For United States property or payments, follow the applicable documentation rules, such as requesting Forms W-8 and the regulations under section 1441.


Foreign recipient, lender, or servicer:


- A foreign interest recipient may have a Form 1098 obligation if interest is received in the United States If the interest is received outside the United States, a Form 1098 must be filed if the entity is a controlled foreign corporation or otherwise has effectively connected income under the applicable lookback tests.


Multiple borrowers:


- Only one Form 1098 is required per mortgage.
- File in the name of the payor of record, if that person is an individual.


Collection agent versus owner:


- The initial recipient, such as a servicer, generally files Form 1098.
- An exception may apply where the servicer lacks needed information and the owner receives interest in its own trade or business.


Multi-jurisdiction note:
If you deal with borrowers or property across multiple states, or with cross-border elements such as United States property owned by foreign individuals,[TaxGPT Matrix](https://www.taxgpt.com/taxgpt-matrix) can help compare how federal Form 1098 reporting interacts with different state-level reporting or disclosure requirements.


### 9. Documentation, retention, and penalties


Keep documentation that supports your filings and any penalty defenses.


- Written designation agreements with qualified persons, such as servicers, for at least 4 years after the year the loan is made
- Evidence of TIN solicitations and any responses
- Internal policies showing how you classify charges as interest versus fees


Penalties may apply for:


- Failure to file correct information returns under section 6721
- Failure to furnish correct payee statements under section 6722


A reasonable cause penalty waiver may be available when you can show that the failure was due to reasonable cause and not willful neglect. Thorough documentation of TIN solicitations, internal policies, and corrective actions is your best defense.
