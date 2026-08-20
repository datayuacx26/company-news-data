---
schema_version: "1.0.0"
document_id: "ab8698a2b9fb591c2e01e886d7ec8d193cfd27dfe3b7dcbe1ebcfb624d4eaa44"
company_key: "yc-increase"
company: "Increase"
source_id: "yc-increase-news-import-16282b6988d7"
canonical_url: "https://increase.com/articles/how-depository-placement-networks-provide-extended-fdic-coverage"
published_at: "2024-01-25T00:00:00+00:00"
first_seen_at: "2026-07-24T11:51:21.619731+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:a10b75059451791b80a242860c51d265631cd025a3a9efaba329fac48e506e4e"
---

# How depository placement networks provide extended FDIC coverage — Increase

Increase and our bank partners now offer access to Federal Deposit Insurance Corporation (“FDIC”) coverage on deposits of more than $250,000. This works through our connection to a deposit placement network offered by[IntraFi](https://www.intrafi.com/) . You can place deposits using IntraFi’s ICS service via Increase’s Dashboard or API. Enrollment does not impact access to your funds.


If you’re in a hurry, that’s all you need to know. Deposit placement networks are a fascinating topic, however. The rest of this article dives into why this feature exists and how it works.


## Silicon Valley Bank


On Friday, March 10, 2023, California regulators closed Silicon Valley Bank and appointed the FDIC as receiver, which issued a press release:


All insured depositors will have full access to their insured deposits no later than Monday morning, March 13, 2023. The FDIC will pay uninsured depositors an advance dividend within the next week. Uninsured depositors will receive a receivership certificate for the remaining amount of their uninsured funds. As the FDIC sells the assets of Silicon Valley Bank, future dividend payments may be made to uninsured depositors.


Unusual to this closure was the percentage of uninsured deposits: 93.9%. Depositors rushed to wire their funds out. At 6:24pm on Friday evening, a message came in extending the Fedwire window by 30 minutes:


The deadline was extended by 30 minutes eight times that evening, then by 15 minutes three more times, until it finally closed at 11:45pm.


The confusion continued over the weekend. The Silicon Valley Bank web portal started disallowing transfers. Alarmism circulated online. A news story on Sunday, March 12, reported,[“Just 30% of uninsured SVB deposits could be available Monday as the FDIC gears up for a long and drawn-out sale process: sources.”](https://fortune.com/2023/03/12/how-many-silicon-valley-bank-svb-deposits-fdic-30-50-percent-long-sale-process/)


## Bank failures


While Silicon Valley Bank’s failure happened quickly, it certainly wasn’t novel.


The Federal Deposit Insurance Corporation is well-named. As you might expect, it was founded in the 1930s when bank failures and bank runs were common. It aims to solve the *runs* , not the failures. It does so by guaranteeing your first $250,000 of deposits at a single bank. It’s funded by an annual levy on insured banks.


\[Note: Interestingly, the insurance is per ownership category, per account holder, per institution. Checking accounts, savings accounts and CDs are treated as one category; retirement accounts are another; trust accounts another; employee benefit accounts yet another.[Ownership categories are well documented.](https://www.fdic.gov/resources/deposit-insurance/brochures/insured-deposits/) \]


The amount insured was originally $2500 and has been raised gradually since then, most recently from $100,000 in 2008.


Today, bank failures are rare. Of the[4,645 FDIC-insured banks in the United States](https://www.fdic.gov/analysis/quarterly-banking-profile/statistics-at-a-glance/2023jun/industry.pdf) , there were zero failures in 2021 and 2022. But as you’re likely aware, there were 5 failures in 2023, including Silicon Valley Bank, First Republic Bank and Signature Bank.


Silicon Valley Bank’s failure was unique in that the vast majority of deposits there were uninsured (93.9%). Since then, there’s been a resurgence of attention on FDIC insurance limits, and how to protect funds above the limit.


## Deposit networks, explained


If you’re storing funds above the limit, you’re likely interested in ways to insure them. In theory, you could get yourself multiples of $250,000 in deposit coverage by opening accounts at 2, 3 or even 400 banks. This would be a hassle. A startup CFO might spend all day on the phone with different banks putting together instructions to wire $3M.


“The swapped deposits ensure customers have access to additional FDIC insurance without the inconvenience of maintaining accounts at multiple banks.”


Suppose she has an idea: her fellow CFOs also have large deposits parked at the same banks. What if, *operationally* , they could each do business at their primary bank, and simply swap each other’s deposits? You have each bank place their deposits at several other banks in a placement network, with FDIC insurance up to $250,000 available at each. A bank that placed a large deposit through the network could also receive deposits in the same amount.


This is roughly how reciprocal deposits work. The swapped deposits ensure customers have access to additional FDIC insurance without the inconvenience of maintaining accounts at multiple banks. From the depositor’s perspective, everything is seamless: you can still access all the funds at your bank even if in the background some of those funds are “swapped” to another institution. You’re just better insured.


\[Note: Until 2018, reciprocal deposits were considered brokered deposits, which are subject to different rules. Congress intervened, and now[most reciprocal deposits are considered non-brokered.](https://www.dallasfed.org/research/economics/2023/1128) \]


Increase’s implementation of this is transparent. You can see in the Dashboard which banks are holding deposits that have been placed for you.


The deposit network will only ever place funds for a depositor up to the current FDIC insurance limit at any other single institution. Of course, it’s possible that you already have funds stored at one or more other banks. To ensure that your funds held there are still insured, it’s straightforward to exclude financial institutions where you’re already storing funds.


## Are uninsured deposits really uninsured?


When regulators announced Silicon Valley Bank’s closure on Friday evening, it made clear that insured and uninsured deposits would be treated separately. But that didn’t end up being the case: over the weekend, federal regulators decided to invoke the[systemic risk exception](https://www.fdic.gov/news/press-releases/2023/pr23017.html) , which protected uninsured deposits.


Because of this, you may assume your deposits will be protected even above insurance limits. The regulators make no assurances on that point. Past performance is no guarantee of future results.


The FDIC itself has reviewed insurance limits. It[issued a report](https://www.fdic.gov/analysis/options-deposit-insurance-reforms/index.html) that considered various options, including changing limits or abolishing them. Instead, the FDIC seems to be leaning towards targeting insurance at specific account types — specifically business payment accounts. However, any changes to deposit insurance would require congressional action.


So where does that leave us?


The FDIC insures funds up to $250,000, per ownership category, per account holder, per institution. In the event of a bank failure, uninsured deposits could be safe. Another institution might step in to acquire all deposits, or federal regulators could invoke the systemic risk exception again. But there is no guarantee either of these things will happen.


That’s why we’re offering access to reciprocal deposits. It’s a way to obtain more peace of mind with access to additional insurance for more of your balances without impacting your operational access to funds. This is all now available in the Increase[Dashboard](https://dashboard.increase.com/) and[API](https://increase.com/documentation/api) .


---


*Deposit placement through IntraFi Cash Service (“ICS”) is subject to the terms, conditions, and disclosures in applicable agreements.*
