---
schema_version: "1.0.0"
document_id: "f2a6b638ed57d06a7e93d2da97b3532fbe332b599ad27fe2487e77e442e92471"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/self-employment-tax"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T17:53:17.757440+00:00"
fetched_at: "2026-08-19T17:53:18.803914+00:00"
content_hash: "sha256:02621eff5a9c4b5cebb0212a42fa84c855176ade69f5a8c92af35019e6ca91a9"
---

# Self-Employment Tax: How 15.3% Rate Works (Schedule SE Filing Guide)

**Self-employment tax is a 15.3% federal tax that self-employed people pay on their net business earnings to fund Social Security and Medicare.** It replaces FICA withholding that would normally come out of a W-2 paycheck and because you're both employer AND employee, you pay both halves. This is why net Schedule C profit gets taxed heavier at margin than W-2 wages of same amount.


This guide walks through 15.3% breakdown, $400 threshold that triggers filing Schedule SE, 92.35% adjustment that most calculators skip, half-deduction that partly offsets sting, and additional Medicare tax on high earners.


## What self-employment tax is


Per[IRS self-employment tax page](https://www.irs.gov/businesses/small-businesses-self-employed/self-employment-tax-social-security-and-medicare-taxes) , self-employment tax is equivalent of FICA (Social Security + Medicare) for people who don't receive a W-2. When you work for an employer, FICA is split: employee pays 7.65% withheld from paycheck, and employer pays a matching 7.65%. When you're self-employed, there is no employer you pay both halves yourself:


- **12.4% for Social Security** , up to annual Social Security wage base
- **2.9% for Medicare** , no wage cap
- **= 15.3% total** on first dollars, dropping to 2.9% + 0.9% additional Medicare above SS wage base


## The 15.3% breakdown at a glance


Component


Rate


Wage cap


Social Security (SE)


12.4%


$168,600 (2024) / $176,100 (2025)


Medicare (SE)


2.9%


No cap


Additional Medicare


0.9%


Kicks in above $200K single / $250K MFJ / $125K MFS


‍


Above SS wage base, 12.4% Social Security portion stops but 2.9% Medicare continues. And once your net earnings pass additional Medicare thresholds, another 0.9% stacks on top of 2.9%.


## Who owes SE tax


You owe self-employment tax if:


- **Your net earnings from self-employment are $400 or more** for year, OR
- **You had church employee income of $108.28 or more**


The $400 floor is trigger. Below $400, you don't owe SE tax and don't have to file Schedule SE (though you may still need to report income on your Form 1040).


The people who typically owe SE tax:


- Sole proprietors filing Schedule C (see our[Schedule C filing guide](https://www.finlens.app/blogs/schedule-c) for how net profit is calculated)
- Single-member LLC owners (disregarded entity by default → report on Schedule C → subject to SE tax on net)
- Gig workers (rideshare, delivery, marketplace platforms all Schedule C income)
- Freelancers and independent contractors
- Partners in a general partnership (their share of ordinary business income from Schedule K-1)
- Farmers filing Schedule F


The people who do NOT owe SE tax:


- W-2 employees (they already pay FICA through paycheck withholding)
- Statutory employees (paid via W-2 with Box 13 checked; they report on Schedule C but no SE tax)
- Rental real estate landlords (rental income is passive Schedule E, no SE tax)
- S-corp shareholder-employees (their W-2 wages pay FICA; their K-1 profit distribution is not SE-taxable)
- Limited partners (LP share of partnership income is generally passive no SE tax)
- Investors receiving dividends, capital gains, or interest income


The S-corp exception is why S-corp elections are popular for profitable businesses: only "reasonable compensation" W-2 portion pays FICA, and remaining profit distribution avoids 15.3% entirely. The IRS is aggressive about ensuring W-2 wage is actually reasonable, though set it too low and expect an audit.


## The 92.35% adjustment nobody talks about


Here's calculation step most guides skim past. You don't pay SE tax on 100% of your net earnings you pay on **92.35% of net earnings** .


Why? Because 7.65% "employer half" that a W-2 employer would pay is a business expense, not personal income. Congress wanted self-employed people to be treated equivalently, so SE tax calculation reduces base by 7.65% before applying 15.3% rate. The formula:


SE tax = Net Schedule C profit × 0.9235 × 15.3%


= Net Schedule C profit × 0.14130


So effective SE tax rate on your Schedule C profit is closer to **14.13%** , not 15.3%. On $50,000 net profit, SE tax is $50,000 × 0.9235 × 0.153 = **$7,065** .


Above SS wage base, math changes only 2.9% Medicare piece applies, so effective rate on "above cap" dollars is 2.9% × 0.9235 = **2.68%** , plus 0.9% additional Medicare if applicable.


## Half of SE tax is deductible


Here's second offset that helps: **half of SE tax you pay is deductible above line** on Schedule 1, Line 15. This isn't a "business deduction" that reduces Schedule C net profit; it's an adjustment to gross income that reduces your Form 1040 taxable income.


Using $50,000 example:


- SE tax owed: $7,065
- Half of SE tax deducted on Schedule 1: $3,533
- Reduces your Form 1040 AGI (and therefore federal income tax) by $3,533


If your marginal federal income tax bracket is 22%, that deduction saves you an additional $777 in federal income tax. Net cost of SE tax after half-deduction offset: $6,288 on $50,000 net profit, or 12.6% effective.


## How SE tax flows through return


Here's sequence:


1. **Schedule C** calculate net profit (Line 31)
2. **Schedule SE** apply 92.35% adjustment, then compute 15.3% (with SS cap at 12.4%)
3. **Schedule 2, Line 4** SE tax gets added to your total tax
4. **Schedule 1, Line 15** half of SE tax deducted (above line)
5. **Form 1040** combined with income tax to get total tax liability


You pay both federal income tax AND self-employment tax on same net profit. For a sole proprietor in 22% bracket, that's roughly 22% + 12.6% (SE after half-deduction) = **~34.6% marginal rate** on additional business income before any state tax. This is why self-employed people underestimate their tax burden so consistently in year one.


## Estimated tax payments SE tax has no withholding


W-2 employees have their FICA and income tax withheld automatically. Self-employed people don't. If you'll owe $1,000+ at year-end (which is easy once you cross ~$5,000 in net profit), you're required to make quarterly estimated tax payments to cover both:


- Federal income tax
- Self-employment tax


The IRS quarterly deadlines are April 15, June 15, September 15, and January 15 of following year. Missing them triggers Form 2210 underpayment penalties. Our[quarterly estimated tax deadlines guide](https://www.finlens.app/blogs/when-are-quarterly-estimated-taxes-due) covers safe-harbor rules for avoiding penalty.


## Reducing your SE tax legally


**S-corp election:** For profitable single-member LLCs, electing S-corp taxation (via Form 2553) means you split your compensation into "reasonable W-2 wages" (subject to FICA) and "distribution" (no SE tax). This is classic self-employment tax planning move for businesses netting $80K+ per year.


**Retirement contributions:** SEP-IRA, Solo 401(k), and SIMPLE IRA contributions reduce federal income tax but NOT SE tax SE tax is calculated on Schedule C net profit BEFORE retirement plan deduction. The[Traditional vs Roth IRA guide](https://www.finlens.app/blogs/traditional-ira-vs-roth-ira) covers retirement plan menu for self-employed filers.


**Health insurance premiums:** Same rule self-employed health insurance is a Schedule 1 adjustment (income tax reduction), not a Schedule C expense, so it doesn't reduce SE tax.


**Legitimate business expenses on Schedule C:** Every dollar you shift from personal to legitimate Schedule C expense reduces net profit → reduces SE tax by ~14.13% AND reduces income tax by your marginal rate. See our guide on[how to write off business expenses](https://www.finlens.app/blogs/how-to-write-off-business-expenses) for what qualifies.


## Conclusion


**Self-employment tax is tax most sole proprietors underestimate.** It's separate from income tax, it applies before most deductions kick in, and it stacks on top of your federal bracket. Understanding 92.35% adjustment and half-deduction is difference between an accurate quarterly estimate and a surprise April bill.


## Frequently asked questions


### **Do I still owe SE tax if my Schedule C shows a loss?**


No. SE tax is only calculated on positive net earnings. Losses reduce your other income on Form 1040 but don't create negative SE tax.


### **How is SE tax calculated for someone above SS wage base?**


The 12.4% Social Security portion stops at wage base ($168,600 in 2024). Above that, only 2.9% Medicare piece continues, plus 0.9% additional Medicare if your net earnings pass $200K/$250K/$125K.


### **Does SE tax reduce my future Social Security benefits?**


No opposite. Paying SE tax builds Social Security credits same way FICA withholding does for W-2 employees. Your future benefit is based on lifetime earnings.


### **If I have both W-2 wages and self-employment income, do I pay double SS?**


No. The SS wage base is a combined cap. If your W-2 wages already put you above $168,600, you owe zero SS on SE side (only 2.9% Medicare piece plus any additional Medicare). Schedule SE has a specific line for reconciling this.


### **Are LLC members subject to SE tax?**


Single-member LLC (disregarded): yes, owner reports on Schedule C and pays SE tax. Multi-member LLC taxed as partnership: general partners yes, limited partners generally no. LLC taxed as S-corp: only W-2 wages pay FICA; K-1 distribution does not.


### **Do I file Schedule SE if my only income is a K-1 from a passive investment?**


No. Passive K-1 income (from a limited partnership or a rental) is not subject to SE tax. Only general partnership K-1s with active business income trigger Schedule SE.


### **What's difference between SE tax and FICA?**


Same underlying taxes (SS + Medicare). FICA is 7.65% × 2 = 15.3% split employer/employee for W-2 wages. SE tax is 15.3% × 0.9235 = 14.13% effective, paid entirely by self-employed person, with half deductible above line.


### **When did $400 threshold last change?**


The $400 threshold has been in place since 1990 and has never been indexed to inflation, so it captures increasingly small side-hustle income each year.
