---
schema_version: "1.0.0"
document_id: "e8813e0a8c4c56453969b3b6f42fa9505625d5ca5e92f7ee3b526143d96d3270"
company_key: "upstart-holdings-inc-common-stock"
company: "Upstart Holdings Inc."
source_id: "upstart-holdings-inc-common-stock-rss-a598e0871937"
canonical_url: "https://medium.com/upstart-tech/how-we-estimate-the-impact-of-the-macroeconomy-on-loans-b02ec701d7c0"
published_at: "2023-03-23T15:01:34+00:00"
first_seen_at: "2026-07-20T23:17:16.152971+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:f3dd9473136f566d4d8e1b0af5d5479d55dc5b4df903979286c4c7f7787cb805"
---

# How we estimate the impact of the macroeconomy on loans

# ***How we estimate the impact of the macroeconomy on loans***


[Grant Schneider](https://medium.com/@gschneidz?source=post_page---byline--b02ec701d7c0---------------------------------------)


7 min read


·


Mar 23, 2023


--


*Grant Schneider, Vice President of Machine Learning*


Over my eight and a half years at Upstart, I’ve had the pleasure of working on a lot of interesting and challenging problems that were essential to our business. (See[here](https://www.upstart.com/updates/meet-grant-schneider-head-of-upstart-columbus) for just how many!) Recently, the Machine Learning team has been working on yet another: quantifying one of the most dynamic macroeconomic environments seen in decades.


The past year reinforced the big impact that changing macroeconomic conditions like inflation and unemployment can have on Americans’ finances, and in turn, the credit performance of loans.


So, we wondered: Is there a way to measure the impact of the changing macro on Upstart-powered loans so that our bank and credit union partners could better manage their lending?


Turns out there is. We call it the[Upstart Macro Index (UMI)](http://www.upstart.com/umi) . We launched it yesterday, and I’m going to explain how it was built.


Press enter or click to view image in full size


First, we needed a way to quantify the macro impact on our borrowers across different months in a way that was both rigorous and actionable. As a classically trained statistician, my first instinct would be to run an experiment holding everything constant except the month. But that doesn’t work because no such constants exist. We can’t give the same person the same loan in multiple months in order to A/B test the effect of a given pair of months. Even if the same person came back for a second loan, they would be in different financial circumstances and, at the very least, have an additional loan.


In theory, we could instead randomly sample a group of loan applicants, slow down the loan process for them to get a set of loans that spanned multiple months, and track their performance in relation to the macroeconomy during that period. However, we know our[borrowers really love our industry-leading speed](https://www.trustpilot.com/review/www.upstart.com) , and intentionally slowing things down is[near heresy around these parts](https://review.firstround.com/speed-as-a-habit) .


So, we can’t run an experiment. Could we try a simple comparison of the same cohort’s loan performance in two different months? This won’t work, either, because the risk profile of a loan will change while it’s in payment. This “month on book” factor — the number of months since the loan was originated — is an incredibly important feature for predicting risk, and we’ve spent a significant amount of time modeling and thinking about the relationship between them at both the individual loan and portfolio level.


But what about comparing the performance of different cohorts in the same month on book? For example, August 2022 loan originations in November 2022 and October 2022 loan originations in January 2023 would both be scheduled for their third payments. The problem with this approach is that upgrades to our underwriting model, changes in our marketing efforts, macroeconomic shifts, seasonality, and any number of other factors can make monthly cohorts very different, especially when there is a long period of time between them.


You may also ask if we could look at a subset of borrowers within these cohorts based on their credit score or income. But that’s too simplistic. Our entire business is premised on the idea that a person is more than just a three-digit credit score and that artificial intelligence can be used to better determine if someone is creditworthy.


If none of these methods work, then how did we do it?


At a high level, we evaluate our underwriting model’s predictions in comparison to observed defaults to assess any uncaptured effects in a given month. Fortunately, our innovative (and[patented](https://patentimages.storage.googleapis.com/c0/f9/14/e1b80540e2545b/US11568312.pdf) !) “loan-month model” already provides us predictions on a per-loan, per-month basis. This is in contrast to more traditional models, which predict losses at the individual loan level or even just the portfolio level.


We model default hazard ratios (for those of you familiar with survival analysis) as the dependent variable here. These ratios are the predicted probabilities of default at a given time in the life of a loan conditional on having not already defaulted or fully paid off the loan, and there are multiple ways to adjust them to try to quantify the macro effect. The simplest would be to apply a post-hoc adjustment to match the actual losses in a given month we’re tracking — called an “observation month” — to those predicted by the model. But in circumstances where monthly macro effects are correlated with other inputs to the model, such as credit or employment variables, this sequential approach of first fitting the model and then estimating the effects could cause the macro effects to be under or overweighted and therefore incorrect.


Instead, we add an indicator variable for each observation month to the model inputs in our model-training pipeline and retrain it. The indicator variable assigns a specific value to the observation month in such a way that the model won’t make that mistake in correlation. As a result, we can jointly estimate the effect of a given month alongside the more than a thousand other variables we track, allowing the model to detect nuanced relationships between them.


```text
model_pl = Pipeline([      ('features', feature_eng_step),      (          'add_obs_month',          ColumnTransformer([              (                  'observation_month',                  convert_to_obs_month_cat_feature,                  ['month_on_book', 'first_payment_month']              ),              ('other_features', 'passthrough', other_features)          ])      ),      ('estimator', estimator)  ])  model_pl.fit(X, y)
```


We then take a sample of 2 million historical loan payment events (meaning whether a payment was made or not and what month on book) and regenerate predictions for the entire set of possible observation months since January 2017. Why this particular point in time? Because it was one of the earliest time points where we had enough loan volume for the predictions to be stable. As an example, say that in our sample of 2 million payment events we isolate payments two and five for a specific loan originating in May 2022, corresponding to observation months of July 2022 and October 2022. We would then recompute the predicted hazard ratios for each of these two payment events for each of the possible observation months to date (January 2017 to February 2023), yielding 74 months × 2 million sampled payments for a total of 148 million predictions.


```text
for first_pmt_month_i in payment_months_of_interest:      X[:, first_pmt_month_idx] = first_pmt_month_i      preds = model_pl.predict(X)
```


Next we take the average of the 2 million predicted hazard ratios for each of the calendar months, resulting in a vector of length 74, or the average of that 2 million for each of the 74 months. Finally, we normalize these average predicted hazards by a long-term average hazard ratio. This makes the number more interpretable, so that a UMI value of 1 means that defaults should be in line with long-term average expectations, a value of 2 means they should be doubled, a value of 0.5 means they should be halved, and so on.


Now that we have our quantification of the relative macro effects, we (and you, as of today!) can update our perspectives monthly on how the creditworthiness of American consumers is evolving. Furthermore, we can apply an adjustment to the hazard ratios for loans we’re originating today, which is informed by UMI. This forward adjustment is a powerful tool that allows our lending partners to combine their unique outlook on macro with our state-of-the-art risk models to obtain a bespoke model at the cutting edge of credit risk.


A final question you might ask is whether we’re able to use this observation-month variable in prediction given we won’t have any training data for future months. This is an exciting area of active research, and we have multiple promising ideas to pursue here — so, stay tuned!


*Upstart intends to release UMI monthly, including revisions to prior months when applicable. Subscribe for first access to those updates here:*[upstart.com/umi](http://upstart.com/umi)


**LEGAL DISCLAIMER**


The statements and information on this site are current as of March 21, 2023, unless another date with respect to any information is indicated, and are provided for informational purposes only. Past UMI performance can provide no assurance and is not indicative of future UMI results. UMI is based on historical data and Upstart’s analysis of the losses within Upstart-powered loan portfolios and is specific to Upstart’s borrower base. UMI is not intended to measure the macroeconomic risks in terms of losses of loan portfolios or asset classes that are not Upstart-powered loans, including loans held by other segments of the U.S. population. It is not designed to measure the current state of the overall economy or to measure or predict future macroeconomic conditions, trends or risks. It is also not designed to measure or predict the future performance of Upstart-powered loans or of Upstart’s other products, overall financial results of operations or stock price. We expect that our research and development efforts to improve UMI could result in changes or revisions to current or past UMI values.


All forward-looking statements or information on this site are subject to risks and uncertainties that may cause actual results to differ materially from those that Upstart expected. Any forward-looking statements or information on this site are only as of the date hereof. Upstart undertakes no obligation to update or revise any forward-looking statements or information on this site as a result of new information, future events or otherwise. More information about these risks and uncertainties is provided in Upstart’s public filings with the Securities and Exchange Commission, copies of which may be obtained by visiting Upstart’s investor relations website at[www.upstart.com](https://www.upstart.com/) or the SEC’s website at[www.sec.gov](https://www.sec.gov/) .
