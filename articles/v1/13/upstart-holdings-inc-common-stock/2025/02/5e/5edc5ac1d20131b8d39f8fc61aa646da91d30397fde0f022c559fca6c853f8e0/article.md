---
schema_version: "1.0.0"
document_id: "5edc5ac1d20131b8d39f8fc61aa646da91d30397fde0f022c559fca6c853f8e0"
company_key: "upstart-holdings-inc-common-stock"
company: "Upstart Holdings Inc."
source_id: "upstart-holdings-inc-common-stock-rss-a598e0871937"
canonical_url: "https://medium.com/upstart-tech/upstarts-model-19-leverages-richer-data-for-even-greater-accuracy-6d7873efaaac"
published_at: "2025-02-21T18:51:01+00:00"
first_seen_at: "2026-07-20T23:17:16.152971+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:331d239c118954cb3142d889195bd2748d52964d9e9d93887cda7c1f5d003082"
---

# Upstart’s Model 19 Leverages Richer Data for Even Greater Accuracy

AI


Machine Learning


Loans


Credit


Data


# **Upstart’s Model 19 Leverages Richer Data for Even Greater Accuracy**


[Brett Manning](https://medium.com/@brett.manning?source=post_page---byline--6d7873efaaac---------------------------------------)


3 min read


·


Feb 21, 2025


--


Press enter or click to view image in full size


In December, Upstart launched its latest personal loan underwriting model: Model Version 19.0.0.


Model 19 added two standout features that allow the model to make even more accurate predictions when assessing credit risk: the Payment Transition Model (PTM) and Trade Account Embeddings. PTM allows us to consider the different stages of a loan payment (current, delinquent, paid off, and charged off) rather than only whether or not a loan is current right now. Embeddings make it possible for the model to learn subtle patterns in historic credit behavior.


Together, these features increase model complexity by 88%, representing both a significant advance in data analysis and a massive feat of engineering. Let’s have a deeper look at them.


**Payment Transition Model**


While our previous[loan-month model](https://www.upstart.com/ptcc#:~:text=Understanding%20'loan%20month'&text=With%20our%20patented%20loan%2Dmonth,default%20in%20month%2030!).) architecture predicted whether or not a borrower would make each specific monthly payment, it left some information on the table, such as the exact payment pattern of a borrower.


To capture this payment behavior, the PTM framework models the probabilities for a loan to transition among different payment statuses. The ability to tell the difference between on-time and late payments delivers a greater level of sophistication when predicting risk. For example, Model 19 can now estimate the probability that a borrower who is one month past due will either recover to “current” status or move further past due for each month of payment.


Adding PTM to the model results in a richer dataset, with up to four additional observations per loan in delinquency scenarios. This improves predictive accuracy while aligning loan behavior more directly with the charge-off timelines of Upstart’s lending partners.


**Trade Account Embeddings**


Embeddings are a method for transforming categorical, or text-like, data into numerical representations. In the case of Model 19, embeddings transform trade account attributes — such as the portfolio type or current balance amount you’d see listed on a typical credit report — into compact, high-dimensional numerical features (which is to say, they pack a lot of information into more easily digestible numbers for the model).


Imagine each borrower’s credit history as a series of “words” built from trade account attributes. For example, a single account could yield a word like “XYJN50011,” which would represent the connection among the different types of credit held, account balances, and payment details that feeds into a risk assessment of that borrower. The embedding model converts these words into a compressed numerical vector space. These vectors allow the model to capture complex relationships among credit accounts and identify subtle patterns in borrower behavior that enhance the model’s ability to make more accurate predictions. Moreover, embeddings make it possible to scale the amount and complexity of information the model can process and do so efficiently.


One challenge with embeddings is their perceived complexity. Regulators, consumers, and lending partners need clear explanations for loan decisions and compliance with fair lending laws. Upstart solved this by leveraging its existing approach to[Adverse Action Notices](https://www.upstart.com/lenders/regulatory-compliance/adverse-action-notices/) and by applying comprehensive fair lending controls, including rigorous[quantitative proxy analysis](https://www.upstart.com/lenders/regulatory-compliance/detecting-eliminating-proxies/) and disparate impact testing.


These safeguards ensure that embeddings improve model performance without sacrificing explainability or fairness — a critical balance for expanding access to credit.


**Enabling Advanced Models**


The advances outlined above, in addition to the “[APR as a feature](https://medium.com/upstart-tech/how-we-built-our-most-advanced-underwriting-model-yet-a6b46ca3225f) ” that we added to[Model 18](https://medium.com/upstart-tech/how-we-built-our-most-advanced-underwriting-model-yet-a6b46ca3225f) , have resulted in a 55x increase in model complexity. Between Model 17 and now, we’ve seen a 16x increase in calculations downstream of the model as a result of an increase in model predictions and more lending programs (i.e., the credit parameters and target returns laid out by our lending partners on the Upstart platform). To keep pace with complexity and deliver these accuracy improvements at scale, we’ve optimized multiple areas of our stack for better performance, including service-to-service communication, high parallelization, and balanced distribution of workloads across our compute infrastructure.


We’re excited about the results. As our CEO, Dave Girouard,[said](https://www.upstart.com/news/q4-and-full-year-2024-earnings-call-transcript/) on our most recent earnings call: “We’re often surprised by the increase in accuracy these types of innovations deliver. But concepts like[“APR as a Feature”](https://medium.com/upstart-tech/how-we-built-our-most-advanced-underwriting-model-yet-a6b46ca3225f) and “PTM” aren’t one-time boosts — they’re new model forms entirely. You can think of them as innovation vectors that offer our team many ways to refine and build on their advantages for a long time to come.”
