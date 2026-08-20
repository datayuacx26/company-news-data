---
schema_version: "1.0.0"
document_id: "2bc97869e883142c596eebe3d11802cae5da339f5595bef2bd7fe95b80b26930"
company_key: "webull-corporation-class-a-ordinary-shares"
company: "Webull Corporation"
source_id: "webull-corporation-class-a-ordinary-shares-news-import-1b16e7f1bcf1"
canonical_url: "https://www.webull.com/blog/218-How-to-Calculate-Compound-Interest-Formulas-and-Tools"
published_at: "2026-08-11T08:33:18+00:00"
first_seen_at: "2026-08-18T18:14:04.639513+00:00"
fetched_at: "2026-08-18T18:14:06.057355+00:00"
content_hash: "sha256:b4f96771b2d7b901a88957d453b9797eaeeaaa944d9638d39180f8c176fb3599"
---

# How to Calculate Compound Interest: Formulas and Tools

Compound interest is one of the most important concepts in personal finance, because it determines how quickly a balance grows when earnings are reinvested rather than paid out. Understanding how to calculate compound interest — and the compound interest formula behind it — can help you set realistic expectations for a savings account, a CD, or a brokerage account. This guide walks through the math step by step and reviews tools, including features available through Webull, that can help you track how a balance changes over time.


### Key Takeaways


-


Compound interest is calculated on the principal plus any interest already earned, not on the principal alone.


-


The standard compound interest formula is A = P(1 + r/n)^(nt), where P is principal, r is the annual rate, n is the number of compounding periods per year, and t is time in years.


-


More frequent compounding — daily versus annually, for example — generally produces more total interest over the same period.


-


The Rule of 72 (72 ÷ interest rate) offers a quick way to estimate how many years it takes an investment to double.


-


Brokerage platforms such as Webull provide account-level metrics like Cumulative P&L, which factor in dividends received and other account activity, that can help investors observe how a balance changes over time.


## Part 1: What Is Compound Interest and How Does It Work?


**Definition:** Compound interest is interest calculated on the initial principal of a deposit, investment, or loan *and* on the interest that has already accumulated in prior periods. This is often summarized as "interest on interest." It differs from simple interest, which is calculated only on the original principal for the entire term.


Because each period's interest becomes part of the base for the next period's calculation, a balance earning compound interest grows at an increasing rate rather than a flat, linear one. This is the mechanism behind long-term growth in savings accounts, certificates of deposit (CDs), and reinvested investment returns.


### The Compound Interest Formula


The most commonly used compound interest formula is:


**A = P(1 + r/n)^(nt)**


Where:


-


**A** = the accrued amount (principal plus interest)


-


**P** = the principal, or starting amount


-


**r** = the annual interest rate, expressed as a decimal


-


**n** = the number of compounding periods per year


-


**i** = interest amount, where CI = A − P


To isolate just the interest earned rather than the total balance, the formula is written as:


**CI = P(1 + r/n)^(nt) − P**


### A Simple Worked Example


Consider a $10,000 deposit earning 5% annual interest, compounded annually, over three years:


Year


Opening Balance


Interest at 5%


Closing Balance


1


$10,000.00


$500.00


$10,500.00


2


$10,500.00


$525.00


$11,025.00


3


$11,025.00


$551.25


$11,576.25


Over three years, total compound interest earned is $1,576.25 — noticeably more than the $1,500 that simple interest on the same principal and rate would generate, because each year's interest is calculated on a growing balance rather than the fixed original $10,000.


### Compound Interest vs. Simple Interest


Feature


Simple Interest


Compound Interest


Calculated on


Principal only


Principal + accumulated interest


Growth pattern


Linear


Accelerating over time


Formula


A = P(1 + rt)


A = P(1 + r/n)^(nt)


Typical use


Some auto loans, short-term notes


Savings accounts, CDs, reinvested investments


**Pros of compound interest (for savers and investors):**


-


Balances can grow faster over long holding periods than with simple interest.


-


More frequent compounding periods (daily or monthly) can modestly increase total returns compared with annual compounding.


**Cons of compound interest (for borrowers):**


-


On debt such as credit cards, compound interest can cause balances to grow faster than the borrower expects.


-


Small differences in compounding frequency or rate can meaningfully change long-term totals, which makes it easy to underestimate the cost of carrying a balance.


## Part 2: How to Calculate Compound Interest Step by Step


Calculating compound interest by hand or with a spreadsheet involves the same basic inputs each time. Below is a straightforward, repeatable process.


### Step-by-Step Calculation


1.


**Identify the principal (P).** This is the amount you are starting with — for example, $10,000.


2.


**Convert the annual rate to a decimal (r).** Divide the percentage rate by 100. A 5% rate becomes 0.05.


3.


**Determine the compounding frequency (n).** Common examples include annually (n = 1), semiannually (n = 2), quarterly (n = 4), monthly (n = 12), or daily (n = 365).


4.


**Identify the time period in years (t).**


5.


**Apply the formula:** A = P(1 + r/n)^(nt), then subtract P to isolate the interest earned: CI = A − P.


### Example With Monthly Compounding


A principal of $10,000 at a 3.875% annual rate, compounded monthly, over 7.5 years:


-


r = 0.03875, n = 12, t = 7.5


-


A = 10,000 × (1 + 0.03875/12)^(12 × 7.5)


-


A = 10,000 × (1.0032291667)^90


-


**A ≈ $13,366.37**


-


Interest earned: **$3,366.37**


### How Compounding Frequency Changes the Outcome


Using a $10,000 principal at 10% annual interest over 10 years, the compounding frequency alone changes the total interest earned:


Compounding Frequency


Periods per Year


Total Interest (10 Years)


Annually


1


$15,937.42


Semiannually


2


$16,532.98


Quarterly


4


$16,850.64


Monthly


12


$17,059.68


This table illustrates a broader pattern: the higher the number of compounding periods, the greater the compound interest, all else being equal. This is why savings and money market accounts often compound daily, while many bonds compound semiannually.


### Estimating Growth Quickly: The Rule of 72


For a fast mental estimate of how long it takes an investment to double, the **Rule of 72** divides 72 by the annual interest rate:


-


At a 4% annual return, money doubles in approximately 72 ÷ 4 = **18 years** .


-


At a 6% annual return, money doubles in approximately 72 ÷ 6 = **12 years** .


-


At an 8% annual return, money doubles in approximately 72 ÷ 8 = **9 years** .


The Rule of 72 works most reliably for annual rates roughly between 6% and 10%, and reasonably well for rates up to about 20%.


### Calculating Compound Interest in a Spreadsheet


For those who prefer a spreadsheet over manual math, a fixed formula can calculate compound interest directly:


` =(B1*(1+B2)^B3)-B1`


Where B1 is the principal, B2 is the interest rate (as a decimal), and B3 is the number of periods. For a $1,000 principal at 5% over 5 years, this formula returns approximately **$276.28** in compound interest, matching a year-by-year multiplication approach.


### Continuous Compounding


Some formulas extend compounding to a theoretical limit — continuous compounding, where n approaches infinity:


**A = Pe^(rt)**


For example, $5,000 compounded continuously at a 7% annual rate for three years results in approximately **$6,168.39** , using this exponential form of the formula.


## Part 3: Tools and Platforms for Tracking Compound Interest Growth


Once you understand the compound interest formula, the next practical question is how to track compounding as it happens in a real account. Several free calculators and brokerage account features can help with this, and the level of detail available varies by platform.


### [Webull: Account-Level Performance and P&L Tracking](https://www.webull.com/)


Among brokerage platforms, **Webull** stands out for the level of detail it provides on the Performance page of a brokerage account, which can help investors observe how compounding-related activity — such as reinvested dividends — factors into their overall account balance over time.


**Cumulative P&L on Webull.** Webull's Cumulative Profit and Loss (P&L) metric represents an investor's total profit and loss over a selected period. It accounts for several components together:


-


**Realized Trading P&L** — profits or losses from completed trades


-


**Open Positions P&L** — unrealized profits or losses from currently held positions


-


**Dividends Received** — income earned from dividend payouts, which is one of the mechanisms by which compounding occurs in an equity portfolio when dividends are reinvested


-


**Margin Interest** — costs associated with borrowing on margin


-


**Fees** — any applicable trading or account fees


Webull notes that these P&L values are estimates calculated from trading and banking activity within the account, provided for informational purposes only. They may not represent an investor's actual account profit and loss, do not affect tax filings, and do not serve as investment advice.


**Day's P&L.** This metric shows how an account performed over a single trading day by comparing the start-of-day value to the end-of-day value, adjusted for any trades, dividends, interest, or other transactions that occurred during the day.


**Open P&L.** Also called unrealized P&L, this reflects the potential profit or loss on a current position if it were closed, calculated as (Last Price − Average Cost Basis) × Number of Shares Owned. Webull notes this is an estimate only; there is no guarantee that a displayed profit or loss will be realized when an order is actually executed, since orders are generally filled at prevailing bid or ask prices, which can differ from the last quoted price in fast-moving or less liquid markets.


**Money-weighted vs. time-weighted returns.** For investors who make regular deposits or withdrawals, Webull offers two P&L% methodologies:


-


The **time-weighted formula** minimizes the impact of cash flows like deposits and withdrawals by calculating a daily profit rate and compounding it across the period: \[(1 + rate day 1) × (1 + rate day 2) × … × (1 + rate day N) − 1\] × 100.


-


The **money-weighted formula** factors in the timing and size of cash flows to produce a more personalized return calculation, adjusting total beginning assets based on when money moved in or out of the account.


This distinction matters for anyone trying to understand compound growth in an actively funded account, since simple percentage calculations can be misleading when deposits or withdrawals occur mid-period.


**Regulatory standing.** Securities trading through Webull is offered to self-directed customers by Webull Financial LLC, a member of SIPC and FINRA. As with any brokerage account, investing involves risk, including the possible loss of principal, and account performance metrics like those described above are informational rather than a guarantee of future results.


### Other Calculators for Estimating Compound Interest


Beyond brokerage account tools, several publicly available calculators can help estimate compound interest for savings, CDs, or hypothetical scenarios:


-


**Investor.gov Compound Interest Calculator (SEC):** A free tool that accepts an initial investment, monthly contributions, time horizon, and estimated interest rate, and can show a range of outcomes based on a rate variance.


-


**CalculatorSoup.com:** Supports multiple currencies and allows for monthly deposits or withdrawals, along with inflation-adjusted results.


-


**Calculator.net:** Provides both simple and compound interest calculations, including sample amortization-style schedules showing deposits, interest, and ending balances year by year.


-


**Moneychimp.com:** Offers a straightforward future-value calculator based on current principal, annual additions, years to grow, and compounding frequency.


-


**Council for Economic Education Calculator:** Designed with students in mind, this tool visualizes growth over time graphically.


These tools use the same underlying compound interest formula described in Part 2, differing mainly in interface, supported currencies, and whether they allow for recurring contributions or withdrawals.


### Fees, Risk, and Platform Safety Considerations


Before relying on any calculator or account feature to plan around compound interest, keep the following in mind:


-


**These figures are estimates.** Whether from a brokerage's Performance page or a third-party calculator, compound interest projections are informational and are not a guarantee of future results.


-


**Fees and margin interest reduce net returns.** Trading costs, account fees, and margin interest — all tracked as separate components in a metric like Webull's Cumulative P&L — can offset gross interest or investment gains.


-


**Regulatory oversight matters.** Choosing a brokerage that is a member of SIPC and regulated by FINRA is one factor investors commonly consider when evaluating where to hold an account used for long-term, compounding growth.


-


**Past performance does not guarantee future results.** This applies to any historical compound interest example, including the illustrations in this article. This is not financial advice.


## FAQs


**What is the formula for compound interest?**


The standard compound interest formula is A = P(1 + r/n)^(nt), where A is the final amount, P is the principal, r is the annual interest rate as a decimal, n is the number of compounding periods per year, and t is time in years. Subtracting P from A isolates the interest earned (CI = A − P).


**How is compound interest different from simple interest?**


Simple interest is calculated only on the original principal for the life of the loan or deposit, using the formula A = P(1 + rt). Compound interest is calculated on the principal plus any interest already earned, so the base amount used for each new calculation grows over time.


**How often is interest typically compounded?**


Compounding frequency varies by product. Savings and money market accounts often compound daily, CDs may compound daily or monthly, many bonds compound semiannually, and many loans compound monthly. Credit cards commonly compound daily as well.


**What is the Rule of 72, and how is it used to estimate compound interest?**


The Rule of 72 estimates how many years it takes an investment to double by dividing 72 by the annual interest rate. For example, at an 8% annual rate, 72 ÷ 8 = 9 years to double. It works best for rates roughly between 6% and 10%.


**Can I calculate compound interest in Excel or another spreadsheet?**


Yes. A common spreadsheet formula is` =(B1*(1+B2)^B3)-B1` , where B1 is the principal, B2 is the interest rate as a decimal, and B3 is the number of compounding periods. This returns the interest earned directly.


**How can I track compound growth within a brokerage account?**


Some brokerages provide account-level performance tools. For example, Webull's Performance page shows Cumulative P&L, which factors in realized and unrealized gains, dividends received, margin interest, and fees, along with money-weighted and time-weighted return calculations that account for deposits and withdrawals over time.


## The Bottom Line


Compound interest formulas explain why balances grow faster when earnings are reinvested rather than withdrawn. Comparing compounding frequency, using the Rule of 72 for quick estimates, and reviewing account-level tools — such as Webull's Cumulative P&L and dividend tracking — can help investors set more realistic expectations for how a balance may change over time.


---


## Disclamer


Securities trading is offered to self-directed customers by Webull Financial LLC, member SIPC, FINRA. All investments involve risk, including the possible loss of principal. You should consider your investment objectives carefully before investing. This is not a recommendation, investment advice, or a solicitation for the purchase or sale of a security. Additional info:[webull.com/disclosures](https://webull.com/disclosures)


## Disclosure


Webull Financial LLC, Member SIPC, FINRA. Investing involves risk. More info at[webull.com/policy](https://webull.com/policy)
