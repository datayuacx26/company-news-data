---
schema_version: "1.0.0"
document_id: "7e6a02a8336fe728add7e217a8441ec05fa18d125cb87ef32bdc57d1f6ac50bc"
company_key: "ibotta-inc-class-a-common-stock"
company: "Ibotta Inc."
source_id: "ibotta-inc-class-a-common-stock-rss-dcf741155171"
canonical_url: "https://medium.com/building-ibotta/taming-a-heavy-tailed-distribution-how-we-built-ibottas-offer-velocity-model-3ab160715e9e"
published_at: "2026-06-30T19:05:12+00:00"
first_seen_at: "2026-07-25T01:07:04.216753+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:d5b207ee8e99865f41f107d5a9c7b6c56c05e2d0834db53f1877623160d694a4"
---

# Taming a Heavy-Tailed Distribution: How We Built ibotta’s Offer Velocity Model

AI


Machine Learning


Sales Forecasting


Redemption


Ibotta


# Taming a Heavy-Tailed Distribution: How We Built ibotta’s Offer Velocity Model


## *By*[Paul Kellar](https://www.linkedin.com/in/kellarpaul11/) *, Senior Data Scientist @ ibotta*


[Paul Kellar](https://medium.com/@paul.kellar_66592?source=post_page---byline--3ab160715e9e---------------------------------------)


8 min read


·


Jun 30, 2026


--


When a brand spends marketing dollars on digital promotions, the questions they care most about are simple: **will consumers engage, and will that engagement actually drive sales of their products** ? But what gets complicated is predicting the total volume of units that will move, and (most importantly) how quickly.


At ibotta, a leader in performance marketing for the consumer packaged goods (CPG) industry, we call these “redemptions” (the verified units purchased that qualify for a cash back offer). When investing in cash back offers, brands want to know how their campaigns are expected to perform before they buy, **which makes our goal of accurate redemption velocity predictions the most critical** .


Furthermore, it is important to accurately predict redemptions across our full network, from our direct-to-consumer (D2C) app to our publisher partner channels like Walmart and DoorDash. The better we can answer those questions — and deliver on those predictions in the real world — the more compelling of a case it is for brands to deepen their ibotta partnership.


## **The Foundation: Predicting Redemption Velocity**


Our velocity model predicts redemptions before a campaign goes live. The challenge for us is the breadth of brands that run campaigns on the ibotta Performance Network ([IPN](https://ipn.ibotta.com/) ), and the underlying differences in their products’ sales and redemption popularity.


Campaigns on the IPN span a wide range of sizes/structures, **from small, targeted promotions to large multi-publisher pushes** . A model that only predicts well on a narrow slice of that range isn’t very useful in practice.


One tempting approach is filtering training data to more uniform subsets of offers, but that approach rules itself out almost immediately for tree-based models like LightGBM. Unlike neural networks or generalized linear models, **they can’t extrapolate beyond the training distribution** , so reliable predictions across the full set of real campaigns require exposure to the full range of offer sizes. Predicting accurately across that range also forces a deliberate choice in how success is measured.


Choosing an evaluation metric for this problem similarly posed some interesting challenges. Percent-based error metrics like median absolute percent error (medAPE) treat all errors as equivalent on a relative basis, but the raw magnitude of a miss (and the downstream impact on how long a given budget will actually last) scales with the size of the offer being predicted. As the model matured, that gap became the natural next frontier to address.


## **Aligning Error Measurement with Prediction Impact**


We began with medAPE as our evaluation metric. This was a natural starting point that gave us a clean, interpretable signal for establishing and validating the model’s initial architecture. As our understanding of the prediction landscape deepened, we recognized an opportunity to align our error measurement even more closely with actual prediction consequence: the same percentage miss carries fundamentally different downstream implications depending on offer magnitude. To capture this, we introduced two additional metrics:


- ***Weighted medAPE*** : Each offer’s contribution is scaled by its total redemption volume relative to the entirety of the data’s distribution.
- ***Top quintile weighted medAPE*** : The same metric restricted to the top 20% of offers by volume (our sharpest signal for performance where stakes are highest).


Balancing these two weighted medAPE metrics alongside unweighted medAPE and R-squared gave us greater visibility into accuracy improvements across the full offer distribution, and helped us distinguish genuine gains at the top of the range from changes that improved averages while masking tail performance. Alongside other methodological improvements described in later sections, we achieved a significant improvement in weighted medAPE, with meaningful gains across overall medAPE, top quintile of offers, and R-squared.


## **Teaching the Model What to Prioritize**


Advancing the evaluation metric opened a corresponding opportunity on the training side. We constructed a **train_weights** vector encoding two signals per training observation.


**Log-transformed volume-based weights:** Weights are proportional to each observation’s redemptions using log-transformation. We specifically chose a log-transform over a linear one: a linear weighting would let the handful of extreme-volume campaigns dominate gradient updates such that the model would lose its well-calibrated behavior across average offers. And since tree-based models can’t extrapolate, we need them trained across the full distribution. Log-scaling preserves the priority ordering (large offers still get more signal) while keeping the weight spread bounded.


**Temporal decay:** Redemption velocity typically peaks in the early weeks of a campaign — not because later weeks are unimportant but because early-week redemption volume tends to be disproportionately large relative to the campaign total. Thus, we apply temporal decay to training weights to reflect this. Early-campaign observations carry more signal, sharpening the model’s focus where the highest redemption volume (and therefore the highest prediction stakes) lives. Prediction quality doesn’t degrade in later weeks, but nailing early-week velocity is a very high-leverage window for overall accuracy.


**Optuna hyperparameter optimization** : Works to minimize weighted medAPE on holdout validation data, with volume prioritization happening in the gradient signal during training. This separation keeps components interpretable and independently verifiable. Alongside the train_weights improvements, employing a weighted medAPE objective (further encoding volume directly into the gradient computation) and an asymmetric penalty that discourages under-prediction on high-volume offers both drove genuine top-quintile accuracy improvements in experiments.


## **New Features That Further Moved the Needle**


**Product purchase history** remains by far the most predictive feature family, particularly recent purchase activity for products on the offer. We found two key additions that specifically moved the needle.


Combining third-party, market-level signals with our first-party purchase data improved accuracy most visibly for offers where our own purchase history was sparse. When building the initial version of this model, we focused deliberately on not overfitting to purchase-based features, so offer setup parameters like cash back amount and purchase quantity required could naturally carry predictive value. As the model matured, it became clear that layering in a clearer signal of a product’s inherent **purchase popularity** (a popular product will naturally be sold and redeemed at higher volumes than an innovation product) gave us an important complementary dimension that offer setup features alone don’t capture.


Because the IPN is a network where we distribute brands’ offers to different publishers, we also needed a consistent measure of varied publishers’ **audience sizes over time** . Features around publisher-level offer clips (clipping/selecting a cash back offer for a given product) addressed this visibility gap outside of our D2C app, where we own all event and user-level data tracking. Capturing publisher-level selection activity (essentially mid-funnel interest) across multiple time horizons provides a strong macro-level audience size signal, with multi-window look-backs letting the model distinguish sustained publisher activity from transient spikes.


## **XGBoost → LightGBM, and the Case for Tweedie**


For most of this model’s history, the architecture was XGBoost-based, first with *reg:absoluteerror* and then with *reg:quantileerror* as the optimization metrics of choice. Performance was strong, and that foundation gave us deep insight into the offer distribution’s characteristics. That accumulated understanding pointed us toward an architectural evolution: **LightGBM with a Tweedie objective** , which gave us a stronger modeling structure aligned to the specific structure of our dataset.


LightGBM is a tree-based model that utilizes leaf-wise (best-first) splitting for learning rather than the level-wise (depth-first) approach of models like XGBoost. This allows for better training on the interactions between highly correlated features. Specifically, LightGBM’s leaf-wise growth finds the highest-gain split anywhere in the tree at each step rather than XGBoost models, which expand level by level, as illustrated in the below visual.


Press enter or click to view image in full size


**XGBoost** : Completes each depth level uniformly (splits low cashback branch to fill d2, wasting 2 of 3 splits). **LightGBM** : all splits chase the highest gain at every step (drilling to d3 to isolate the exact feature interactions of high cashback amount × high retailer count × 7d prior purchases that define high-velocity offers)


For redemption velocity specifically, this matters because offer population is extremely heterogeneous — a viral CPG offer with broad retailer coverage behaves nothing like a niche single-retailer offer with tight targeting. Leaf-wise growth lets the model carve out these high-variance, high-signal subgroups with surgical precision: a leaf representing “high cash back amount + high retailer coverage + past purchaser targeting” can get many additional splits to nail its prediction, while a stable low-variance leaf stops splitting early. Level-wise growth would waste splits uniformly across the tree even in regions that are already well-characterized.


The practical result is better accuracy on the long tail of unusual or high-volume offers that dominate medAPE loss, **without over-smoothing the predictions for common offer archetypes** . Combined with gradient-based one-side sampling (GOSS), which focuses computation on the hardest-to-predict samples, the architectural shift on its own produced a meaningful additional reduction in weighted medAPE.


From there, we recognized that the target variable **fits a Tweedie distribution very well** . Redemptions are strictly non-negative, heavily concentrated among small-to-medium offers, and punctuated by a handful of offers with higher (right-skewed) volumes. This is precisely the type of distribution Tweedie was designed for, as it naturally handles the dense mass of modest offers without being overwhelmed by the outsized influence of outliers at the right tail.


Intuitively, the Tweedie variance power acts as a dial between two regimes: pure count-like behavior on one end, and continuous multiplicative scaling on the other. Our offer data lives somewhere in between. Redemption volume doesn’t scale linearly with the factors that drive it. A highly popular product on a generous offer doesn’t just add more redemptions than a niche one; it compounds them, which is behavior that count-based distributions systematically underestimate.


In turn, rather than forcing the model to assume upfront how variance scales with the mean (which quantile-based regressions implicitly did), **Tweedie lets the data answer that question directly during training** . That flexibility matters: the model found a middle ground between two extremes that neither a pure count-based nor a pure continuous distribution would have captured on its own, and the accuracy gains on our largest offers reflected it.


Moreover, switching to a Tweedie-based regression objective allowed us to enable monotonic constraints that are unavailable with quantile-based modeling in LightGBM, as the underlying LightGBM package does not support monotone constraints with a quantile-based tuning structure. We constrain the model’s award amount to have a monotonically positive effect on predicted velocity; logically, more cash back will produce equal or higher predicted redemptions, all else equal.


Press enter or click to view image in full size


Since our offer training set more closely matches a Tweedie distribution compared to a normal distribution, model accuracy increased significantly when changing this in the underlying objective function.


## **What’s Next**


Accuracy on our offer distribution has iteratively and continuously improved over time, and we continue to push forward — richer purchase-based features, continued architectural experimentation, and more systematic at-scale model tuning as the feature space and IPN publisher coverage continues to grow. Continuous improvement isn’t just a byproduct here; it’s the design principle. Each version of the model opens up new visibility into what the next version should tackle, and that compounding progress is what makes the system increasingly valuable over time. The trajectory has never been “good model, done.” It’s been good to great to better still, with every iteration building the foundation for the next.


The broader principle we’d apply to any high-stakes regression problem with a skewed distribution target: ***be deliberate about where to prioritize accuracy, encode this into how the model defines, weights, and judges success, and invest in data quality alongside architectural experimentation*** . The most impactful changes in this model’s history weren’t complex algorithms; they were surfacing the most relevant purchase popularity features, weighting training toward predicting larger offers more accurately without sacrificing the rest of the distribution, and optimizing a training objective genuinely aligned with what the business cares about.


Thanks to the full team that built this system: Emily Kaegi, Kaileigh Stopa, Josh Roberti, Bobby Crimi, Taylor Names, Maiia Hackler, & Margarita Kretowicz.


### Interested in working at ibotta? Check out roles at:[https://ibotta.com/careers](https://ibotta.com/careers)
