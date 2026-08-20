---
schema_version: "1.0.0"
document_id: "caa9b5f34811360349c57eae3796b2768cca8acc11a8b0543b3bb0dafaf75d0c"
company_key: "upstart-holdings-inc-common-stock"
company: "Upstart Holdings Inc."
source_id: "upstart-holdings-inc-common-stock-rss-a598e0871937"
canonical_url: "https://medium.com/upstart-tech/how-we-built-our-most-advanced-underwriting-model-yet-a6b46ca3225f"
published_at: "2024-08-14T19:01:09+00:00"
first_seen_at: "2026-07-20T23:17:16.152971+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:740b5031b44f7053e2097b9c7bd417eea349169201c8c561fe0bf9b11f9a7794"
---

# How We Built Our Most Advanced Underwriting Model Yet

# **How We Built Our Most Advanced Underwriting Model Yet**


[Brett Manning](https://medium.com/@brett.manning?source=post_page---byline--a6b46ca3225f---------------------------------------)


5 min read


·


Aug 14, 2024


--


*By Brett Manning & Omar Venado*


Press enter or click to view image in full size


Last week we released Upstart’s personal loan underwriting model 18, or M18 as we call it internally.[As our CEO Dave Girouard mentioned during our most recent earnings call](https://www.upstart.com/news/upstart-q2-2024-earnings-materials-ceo-transcript/) , this model represents one of the largest and most impactful improvements to our core credit pricing model in our history. In fact, with this launch, 18% of all accuracy gains in this model since our inception have been delivered by our machine-learning team in the last 12 months.


Model 18 includes our latest implementation of APR as a feature. This means APR is an input to the model, as a measure of the total cost of the loan. The model delivers significant gains as a result of modeling improvements for two key determinants of repayment behavior:


1. Loan Affordability — all else equal a higher interest rate means higher repayments which are harder to afford
2. Adverse Selection — accepting a very high interest rate might be a signal of particular financial stress


To understand how Model 18 achieves this breakthrough, let’s first explain how an underwriting model works. The model takes what we know about the borrower, and predicts their probability to default or repay the loan. For traditional lending this might be a mapping from FICO score and debt-to-income ratio to a single default probability.


But at Upstart we look deeper. Our advanced ML model is at the core of our difference. First, we take into account a much broader set of borrower characteristics than a traditional credit model. That’s well more than 1,600 features about borrowers, including their traditional credit file but also new data like their employment and how well they manage their finances. Second, we use cutting-edge AI to predict the probability of repayment on a month-by-month basis, which gives us a granular view of every borrower.


The probability of default and prepayment is combined with lender risk tolerance to calculate the interest rate the borrower pays. The interest rate plus any fees are used to create an APR and an associated repayment schedule. The APR is itself a key driver of the default probability, which raises an interesting question: How can the APR be both an input and an output of the model?


The mathematician reading this will recognize this as a “fixed point problem.” In a simple linear model this fixed point is quite easy to solve for (similarly it’s easy to add in a crude adjustment to an ML model). However, to truly solve for a large scale multilayered ML model you have to think creatively.


The brute force solution would be to run an iterative scheme for each calculation, but given the sophistication of our models that would leave borrowers waiting hours for offers. To solve it, we came up with a clever approach that uses discretization to capture much of the effect, while reducing the number of calculations at runtime. This enables us to run our risk models many times in parallel to arrive at the appropriate answer. In fact, M18 generates approximately 1 million predictions for each applicant in order to converge to the correct APR, which is six times the number of predictions of our prior model.


Beyond simply accounting for APR in our model, we went a step further and modeled for adverse selection effects. That is, when a borrower accepts an offer that is significantly higher than similar borrowers, it can be seen as higher risk. To address this, we used cutting-edge ML-in-ML to identify offers likely to be subject to adverse selection and price that risk appropriately, avoiding any spurious correlations.


Even with clever use of math and statistics, our calculations increased tenfold which raised our compute costs by 3x and latency by 70%. We had a challenge to deliver these model benefits at scale without negatively affecting the customer experience and keeping our costs in check. To speed up our predictions by 85%, we made extensive low-level changes to our XGBoost library to optimize tree traversal while maintaining full decimal parity — creating a brand new innovative inference algorithm as a result. This allowed us to reduce those higher compute costs by 37%, as well as model inference latency by 26%.


Given the increase in complexity and scale of Model 18 compared to its predecessor, we needed to thoroughly validate the model’s end-to-end performance and how well it was integrated throughout our systems. To accomplish this, we added the ability to “shadow price” loans, which entails asynchronously replaying all pricing transactions in production using Model 18 and comparing the outcomes of both models in near-real time. This piece of infrastructure allowed us to test the new model at scale with zero customer impact, ensure the correctness of our loan offers across thousands of transactions, and validate that production outcomes aligned with those obtained during research.


For the borrower, this model means (1) higher likelihood of approval as we can lend to a large population and (2) getting a more accurate rate. For some borrowers that will mean a lower rate. For others it will help us better ensure that our loans are affordable and do not contribute to financial stress. For our lenders, this new model means greater confidence that they will realize their expected returns thanks to lower losses, even as borrowers continue to feel the effect of high inflation in an uncertain economy.


This kind of innovation is what Upstart is all about: Bringing together the best in ML/AI research and engineering to offer loans that drive the best possible outcomes for consumers and lenders. But we’re just getting started. We have a pipeline of research and new data sources ready to roll out over the next six months with the potential for even bigger accuracy wins along with more new ideas on the horizon. By continuing to drive improvements in our model, we can approve more borrowers and offer even more reliable returns to our lenders.
