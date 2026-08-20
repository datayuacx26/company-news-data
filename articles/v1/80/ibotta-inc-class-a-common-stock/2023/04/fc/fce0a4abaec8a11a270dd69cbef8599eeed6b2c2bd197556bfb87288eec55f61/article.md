---
schema_version: "1.0.0"
document_id: "fce0a4abaec8a11a270dd69cbef8599eeed6b2c2bd197556bfb87288eec55f61"
company_key: "ibotta-inc-class-a-common-stock"
company: "Ibotta Inc."
source_id: "ibotta-inc-class-a-common-stock-rss-dcf741155171"
canonical_url: "https://medium.com/building-ibotta/measuring-incrementality-when-control-groups-are-not-possible-e7ca5cc48b09"
published_at: "2023-04-07T21:33:05+00:00"
first_seen_at: "2026-07-25T01:07:04.216753+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:61cd0afef0e60249c33e7ac1ea7944934c0d7aa58947f85eb0f4998cf2e0a410"
---

# Measuring Incrementality When Control Groups Are Not Possible

# **Measuring Incrementality When Control Groups Are Not Possible**


[Emily Kaegi](https://medium.com/@emily.kaegi_45960?source=post_page---byline--e7ca5cc48b09---------------------------------------)


5 min read


·


Apr 7, 2023


--


The[Ibotta Performance Network (IPN)](https://ipn.ibotta.com/) is the first digital network that enables cash back offers to be securely delivered in a coordinated fashion to over 120 million US consumers across a wide ecosystem of digital properties including the Ibotta app, top retailers, recipe sites, and more. Much of our offer content comes from CPG (consumer packaged goods) brands on grocery or general merchandise products that you can buy at the grocery store or places like Walmart. One of the main benefits for advertising clients when working with Ibotta is that we provide verified incremental sales at scale. We define incremental sales as the additional units purchased because a user was incentivized by an Ibotta offer.


For example, as I’m preparing to go grocery shopping, I open the[Ibotta app](https://home.ibotta.com/) and plan for my trip. I navigate through the gallery of offers and notice an offer for a candy bar I did not originally plan to buy. When I buy it at the grocery store, that purchase counts as an incremental sale because the offer drove me to buy a product that I otherwise would not have. Importantly, Ibotta only classifies a purchase as incremental if we determine that it was the offer stimulus that drove the sale and not other variables such as seasonality or market variation.


The most precise approach to measure incrementality is with an A/B test where a random set of users receive the offer (treatment) and the other users do not receive the offer (holdout/control). We then can compare the purchasing patterns between these two groups when the offer was live and measure the “incremental lift” driven by the offer with high statistical confidence. This is the best methodology because it isolates the impact of the offer on driving consumer behavior, eliminating other factors.


Press enter or click to view image in full size


A/B methodology for calculating incremental sales


Historically, the A/B methodology has been how we measured and reported incrementality to our clients, and we still use this method most of the time. In some cases, however, brands may wish to see incremental performance of a campaign for which ample data to do a true test vs. control measurement may not be available. In that case, we still want to provide accurate insight into the impact of a campaign beyond typical marketing metrics such as ROAS (return on ad spend), impression counts, click through rates or lift metrics.


Our data science team set out to fill in this gap by using our first party purchase data from users who have linked their loyalty accounts on Ibotta. Using this data, we can drill into historical purchasing patterns of any product that we have on offer.


For offers that we cannot run a holdout, we will already have the “treatment” data as we know all our users will be given the offer. We will need a way to generate what a control group would have done because we will no longer be able to see purchasing trends for users without the offer — this is what we refer to as “modeled control”. To create this “modeled control” we explored two methodologies.


Press enter or click to view image in full size


“Modeled Control” methodology for calculating incremental sales in the absence of a control group


### **Approach 1: Time Series Modeling**


We fit pure time series models on the historical purchasing patterns for the products in the offer and projected out the trends during the offer period to create a “modeled control”. Time series models are able to control for seasonal trends (weekly variations) as well as longer period trends (steady increase in sales). We programmed the model to take into account holidays, as on certain holidays our data spikes (Labor Day) vs. others when we see very few sales (Christmas). In addition, we added external regressors to these models to account for periods in our training data where products had been part of another offer.


Press enter or click to view image in full size


Example of using external regressors in model training (created with Greykite)


The time series models do a great job at predicting purchases of core products that have consistent patterns but fall short on more seasonal products or those where demand is not consistent. Pure time series models also require much more historical data in order to be performant when predicting future sales. We experimented with 2 different packages for time series modeling:[LinkedIn’s Greykite](https://engineering.linkedin.com/blog/2021/greykite--a-flexible--intuitive--and-fast-forecasting-library) and Meta’s[Prophet](https://facebook.github.io/prophet/docs/quick_start.html) . The team found Prophet much easier to use out of the box, but after fitting models with both packages, Greykite out performed Prophet.


### **Approach 2: Synthetic Control Modeling**


Synthetic Control modeling is a methodology made popular by economists for estimating a treatment effect without a true control group. Formally it is defined as an observational causal inference approach where a counterfactual is created using a weighted combination of untreated units. In simpler terms, it is a model built using other products’ sales that correlate strongly to the products that we want to predict. This methodology allows you to control for more macro economic trends. See[this article](https://www.washingtonpost.com/news/wonk/wp/2015/10/30/how-to-measure-things-in-a-world-of-competing-claims/) for a “non-technical” overview of synthetic control modeling.


This approach picked up on more complicated purchase trends that our simple time series models missed, but feature selection was more time-consuming and these models can be more prone to overfitting. We experimented with[Google’s Causal Impact package](https://google.github.io/CausalImpact/CausalImpact.html) but found that a simpler[ARIMAX model](https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model#ARMAX) was more performant, interpretable and faster for our use case. Causal Impact took much longer to fit models than ARIMAX which slowed down our model testing. Additionally, Google’s Causal Impact was more of a “black box” so it was harder to diagnose why a model might have produced inaccurate results for a specific campaign while we could easily troubleshoot our ARIMAX model. While we decided to go with our own model, we appreciated that Causal Impact outputs confidence intervals for its predictions so we did not need to calculate those ourselves.


### **Evaluating our Solutions**


Once we had a variety of models built, we needed a way to figure out which solution was most accurate at creating a “modeled control”. Luckily for us, Ibotta has been running A/B incrementality tests on thousands of offers over the last 2 years providing us with a rich historical dataset to measure against. We selected a set of historical offers and compared our “modeled incrementality” results to the A/B results to calculate the accuracy of our models. Ultimately we combined the time series and synthetic control models into one ensembled model, as each had its own advantages.


Now with our new approach we are able to measure incrementality for our clients with “modeled incrementality” in those rare instances where we cannot run a true holdout group for their offer. This ensures our advertising clients receive the statistically robust results they expect regardless of publisher data limitations and continue to[trust the IPN to deliver incremental sales at scale](https://ipn.ibotta.com/blog/know-your-source-ibotta-measurement-methodology-explained) , securing more content for our publishers and savers.


Huge shoutout and thanks to the rest of the team that came up with the solution and built these models — Michael Byman, Neil Fonseca and Siu Yin Lee.


*Interested in working at Ibotta? Check out*[https://ibotta.com/careers](https://ibotta.com/careers)
