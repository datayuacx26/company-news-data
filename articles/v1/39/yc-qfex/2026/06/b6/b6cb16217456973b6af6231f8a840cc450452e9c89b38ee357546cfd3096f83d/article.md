---
schema_version: "1.0.0"
document_id: "b6cb16217456973b6af6231f8a840cc450452e9c89b38ee357546cfd3096f83d"
company_key: "yc-qfex"
company: "QFEX"
source_id: "yc-qfex-rss-5ea6110e160b"
canonical_url: "https://research.qfex.com/p/regularization-and-bagging-first"
published_at: "2026-06-03T17:46:55+00:00"
first_seen_at: "2026-07-25T20:12:49.931294+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:220dd496ffcf3c3bea3d707ed1b256381f55d7c2e388e2bd9259104d8ce07345"
---

# Regularization and Bagging: First Techniques in HFT Modelling

# Regularization and Bagging: First Techniques in HFT Modelling


### Stabilizing linear alphas in low signal-to-noise microstructure data


[Annanay Kapila](https://substack.com/@annanaykapila)


Jun 03, 2026


### Building intuition for regularization and bagging


High-frequency trading operates in one of the noisiest data regimes in finance. Order-book alphas built from microsecond-level imbalances, weighted mid-price changes, and queue dynamics carry an extremely low signal-to-noise ratio. A single linear model fitted on historical data tends to overfit noise rather than capture persistent patterns. The result is unstable coefficients that look great in-sample and fall apart out-of-sample.


Tier 1 HFT desks deal with this using two practical and complementary techniques:


**regularization** and


**bagging** (bootstrap aggregating). Both target the dominant source of error in live trading, variance while preserving the underlying signal. This article explains exactly how each method works, why it succeeds where plain OLS falls short, and how the two approaches complement each other.


Thanks for reading QFEX Research! Subscribe for free to receive new posts and support my work.


### The Core Problem: Low Signal-to-Noise and Model Instability


Say I have three noisy alphas extracted from the order book: x_1, x_2, x_3 The standard thing to do is to fit a linear model of the form:


\\(pred_{future price} = b_1 x_1 + b_2 x_2 + b_3 x_3\\)


Coefficients are usually found via


**Ordinary Least Squares** (OLS), by minimizing the sum of squared errors:


\\(L = ∑(b_1 x_1 + b_2 x_2 + b_3 x_3 - actual_{future price})^2\\)


This result is called the Gauss-Markov Theorem. In the idealized Gauss-Markov world (zero-mean errors, constant variance, uncorrelated errors), OLS gives the best linear unbiased estimator. In live HFT, those assumptions break quickly. We get non-stationarity, fat tails, and highly correlated signals. Small changes in the training window can swing the b_1 dramatically, producing large out-of-sample variance.


### Regularization: Trading A Bit Of Bias For Stability


The first practical fix is regularization. Instead of minimizing only the sum of squared errors, we add a “penalty” for large values of b_1


\\(L + a(b_1^2 + b_2^2 + b_3^2)\\)


\\(L + a(|b_1| + |b_2| + |b_3|)\\)


Where “


*a* ” controls how much we penalize large


*b_1*


We couldn’t have used this model under the original Gauss-Markov framing, because it isn’t unbiased. But that’s the trade we want to make. A small increase in bias buys a large reduction in variance, which is exactly what low signal-to-noise regimes demand. This is called regularization, and it matters even more once we scale beyond toy linear models


### Bagging: Bootstrap Aggregation for Further Variance Reduction


Regularization handles variance by constraining the model. Bagging does it through a different angle, through ensemble averaging.


A particular problem in HFT is the low signal-to-noise ratio. Models are prone to fit to noise rather than patterns. What if we want to focus on a stable, consistent prediction in live trading, rather than just maximizing in-sample fit?


One idea would be to partition the historical data and fit a separate model on each partition, then combine. The problem is that each model will likely be much worse than the original, since it doesn’t have enough points to produce a good fit. We need more data.


The ‘trick’ here is to realize that we can effectively create more by randomly sampling (with replacement) from the historical data we have. This approximates what we were doing when we collected the data in the first place: randomly sampling the ‘real world. This has caveats but you basically end with a load of fitted models.


\\(pred_{future price} = b_1x_1 + b_2x_2 + b_3x_3 \\)


\\(pred_{future price'} = b'_1x_1 + b'_2x_2 + b'_3x_3 \\)


….


And the final prediction is the average. The key thing to understand is that each model has the same average error (bias), but by running the fit many times and averaging, we improve the stability of the process. In other words, we reduce the variance of the errors, just like regularization.


### Two Routes To The Same Goal


Regularization and bagging both reduce variance, through different mechanisms:


-


Regularization constrains the model directly in the optimisation objective.


-


Bagging averages over many perturbed versions of the data.


They’re complementary. A regularized linear model is an excellent base learner for bagging, and the resulting ensemble is stable and robust to the correlated, non-stationary nature of order-book signals.


More to come.
