---
schema_version: "1.0.0"
document_id: "0e56910b7778ae4f2c4ee16e51c33344daa3874b7148a37deed073eff3f3fb02"
company_key: "upstart-holdings-inc-common-stock"
company: "Upstart Holdings Inc."
source_id: "upstart-holdings-inc-common-stock-rss-a598e0871937"
canonical_url: "https://medium.com/upstart-tech/how-numerical-optimization-unlocked-value-for-upstart-14ab3e7abff8"
published_at: "2023-08-10T21:43:51+00:00"
first_seen_at: "2026-07-20T23:17:16.152971+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:a1f50ba8f1c50d769c362bc49dff214def911c9f1c261b4ee369bc2bb509dcef"
---

# How Numerical Optimization Unlocked Value for Upstart

# **How Numerical Optimization Unlocked Value for Upstart**


[John Vandivier](https://medium.com/@vandivi3r?source=post_page---byline--14ab3e7abff8---------------------------------------)


5 min read


·


Aug 10, 2023


--


*By John Vandivier and Xingwen Zhang*


Upstart is the leading AI lending marketplace, working at the intersection of finance and machine learning (ML). The application of ML to finance involves many interesting problems related to[numerical optimization](https://en.wikipedia.org/wiki/Mathematical_optimization) , which is the mathematical process of selecting particular numbers that best satisfy criteria that are important to the outcome you’re trying to produce. In this article, we’ll focus on a specific kind of problem in this area — convergence errors — and show how we found a solution that allowed Upstart to recover substantial value for the company, our customers, and our lending partners.


First, let’s show how both numerical optimization and convergence errors apply to Upstart. For numerical optimization, the outcome we’re trying to produce could be loan terms that account for a borrower’s default risk on that loan, while a criteria we’d optimize for would be the right level for the interest rate, one that’s neither too low nor too high. A convergence error in this case would mean we were unable to mathematically determine a single ideal interest rate for a given loan. That’s a problem for Upstart because it impacts our accuracy in determining risk and our revenue, two key business metrics.


This played out in a feature we launched called Dynamic Pricing. As a lending marketplace, Upstart connects prospective borrowers with loan offers from our lending partners. One aspect of Dynamic Pricing allows our lenders to state their desired return on a loan using a table of applicant FICO scores and risk tiers, where the applicant risk is assessed and graded by Upstart.


Dynamic Pricing is a valuable feature for our partners because each partner has a unique appetite for risk, a unique risk strategy, and differing requirements on return for different levels of risk. But this feature caused errors to occur in our prior mathematical approach to loan offer creation, which involved using a[root-finding algorithm](https://en.wikipedia.org/w/index.php?title=Root-finding_algorithms&oldid=1145256963) to calculate the accompanying interest rate on a loan. This calculation integrates a borrower’s risk assessment with the desired return expressed by our lending partner.


The Dynamic Pricing rollout included an ensemble of testing and observability measures. One concrete observability tactic in particular allowed us to rapidly catch and resolve the convergence issues: a tool called[Celery](https://docs.celeryq.dev/en/stable/getting-started/introduction.html) . Because real-time numerical optimization is resource intensive, Upstart engineers strategically encapsulate this work into asynchronous jobs using Celery. By ensuring that Celery is integrated into our production logging system, we were able to quickly observe and react to any kind of error that is internal to this complex logic.


Dynamic Pricings were observed to produce convergence errors in a low single-digit percentage of cases. Upstart engineers rolled out a quick fix that cut this value in half. The quick fix was a simple mathematical change: We reduced the convergence precision requirement, which enabled more root-finding routines to claim that they had converged, thereby decreasing the errors. But the problem is that lower precision means Upstart becomes less effective in our main service — accurately assessing risk. Therefore, this fix wasn’t a fix at all by our standards.


Upstart engineers quickly coordinated with our experts in machine learning to form a longer-term solution. A simple solution that proved effective was a linear interpolation using a smoothing factor. This solution is diagrammed below:


Press enter or click to view image in full size


Mathematically, the smoothing factor is a distance on the interest rate axis, over which a new and flatter line is calculated to replace the prior vertical line. In the graph above, B is the interest rate location where the vertical discontinuity, and its associated convergence issue, were originally encountered. At the point of convergence failure, our root-finding algorithm is modified to look around and interpolate linearly within a distance of the smoothing factor in either direction from the discontinuity.


In this context, as shown in the prior diagram, the output variable is the[net present value (NPV)](https://www.investopedia.com/terms/n/npv.asp) of the loan. The NPV represents the sum of the loan’s future cash flow, discounted to account for the time value of money, which reflects the concept that money available today is worth more than the same amount in the future.


Upstart calculates an interest rate that satisfies a target[internal rate of return (IRR)](https://www.investopedia.com/terms/i/irr.asp) — the annual rate at which an investment grows. This target IRR is the return our lending partners expect on the capital invested in the loan. It’s the rate that yields an NPV of zero with respect to Upstart because every dollar is accounted for: The net cash flows from the borrower exactly match the net payments to the lending partner.


To find a loan that matches the target IRR, Upstart calculates an expected internal rate of return based on the default risk and prepayment probability of the loan. The higher this risk, the lower the expected IRR at a particular interest rate. We iterate through the set of possible interest rates until we find an interest rate that yields a net present value of approximately zero, indicating that the target IRR is approximately equal to the expected IRR.


Alongside the smoothing factor, engineers added log messages to enable Upstart analyst correlation between the smoothing factor and convergence errors. This enabled Upstart to tune the smoothing factor as a hyperparameter, finding the smoothing factor number that minimized the frequency of convergence errors. Larger values eliminate more errors at the cost of precision. Smaller values increase both precision and the risk of a convergence failure. The smoothing factor was implemented as a[LaunchDarkly multivariate flag](https://docs.launchdarkly.com/home/flags/variations) . This implementation enabled Upstart to update the value many times without many code changes or deployments. This implementation is also strategically defensive. If needed, the feature can be disabled without a code release by assigning a value of zero.


An initial smoothing factor was rolled out in Q4 of 2022. Within weeks, an ideal value was found that reduced the daily error frequency to zero while retaining maximum precision in the convergence calculation. An error frequency of zero means more applicants are awarded loans, lending partners are able to profit by funding more loans, and Upstart is able to improve our bottom line through increased revenue, reduced cost, and higher accuracy.
