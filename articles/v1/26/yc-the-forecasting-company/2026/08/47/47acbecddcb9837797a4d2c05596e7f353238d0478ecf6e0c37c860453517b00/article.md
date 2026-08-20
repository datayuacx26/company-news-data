---
schema_version: "1.0.0"
document_id: "47acbecddcb9837797a4d2c05596e7f353238d0478ecf6e0c37c860453517b00"
company_key: "yc-the-forecasting-company"
company: "The Forecasting Company"
source_id: "yc-the-forecasting-company-news-import-6163f6ce3bb7"
canonical_url: "https://www.theforecastingcompany.com/en/blog/from-arima-to-foundation-models"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-12T09:07:35.157798+00:00"
fetched_at: "2026-08-12T09:07:38.003363+00:00"
content_hash: "sha256:cb9d1e0b7ebc5e95b1102ea32a73757e028217c47d74c3fc72447ab54dd84f11"
---

# From ARIMA to Foundation Models

# From ARIMA to Foundation Models


By Geoff · August 10, 2026


Share


Part 3 of 5


·


The Future of Forecasting


In[Part 1](https://www.theforecastingcompany.com/en/blog/the-automated-forecaster) we sketched the **automated forecaster** : a system that does what an expert human forecaster does, at the scale of millions of series. In[Part 2](https://www.theforecastingcompany.com/en/blog/time-series-are-not-tables) we explained why the data underneath has to be treated as a **trajectory, not a table** . This post focuses on the model at the core of the system, and on the long-awaited shift it brings: zero-shot generalization across businesses, probabilistic forecasts by default, knowledge that transfers between series, and forecasting as infrastructure rather than a bespoke project.


In this post, we explore both halves: the work we are building on, and what we are building towards. Our first public model,[t0-alpha](https://huggingface.co/theforecastingcompany/t0-alpha) — 102M parameters, open weights under Apache-2.0 — is out now, with a dedicated post to follow.


We'll walk through it the way most practitioners lived through it over the last decades: era by era, what changed, what didn't, and what's still painful today. If you have ever fit an ARIMA, written a` groupby().rolling()` , or wondered whether the latest time-series foundation models are hype or substance, this one is for you.


## The five eras of forecasting


The history of forecasting is the history of trying to see around corners.


Same series, five eras


pre-1950


history


ground truth


equations forecast


Representative


Richardson, 1922


If you know the mechanism, you can simulate it. Works for planets, fails for sneaker demand.


Capabilities


-


cold-start with analogues
-


cross-series transfer
-


probabilistic by default
-


covariates
-


no feature engineering


Five eras forecasting the same series. The history is identical; only the model walking in changes. Each era strips away another piece of boilerplate. Schematic illustration on synthetic data, not model output.


### Era 1 — Equations of the world (pre-1950s)


The dream of forecasting is much older than 1922. People predicted seasons and planet positions for thousands of years before anyone wrote down equations for any of it — Babylonian astronomers logged Venus on clay tablets, Greek astronomers predicted eclipses with the[Saros cycle](https://en.wikipedia.org/wiki/Saros_(astronomy)) , medieval almanacs told farmers when to plant. The pursuit of *accurate* astronomical forecasts is what produced classical mechanics:[Tycho](https://en.wikipedia.org/wiki/Tycho_Brahe) 's observations forced[Kepler](https://en.wikipedia.org/wiki/Johannes_Kepler) into ellipses, and Kepler's ellipses forced[Newton](https://en.wikipedia.org/wiki/Philosophi%C3%A6_Naturalis_Principia_Mathematica) into universal gravitation. The *Principia* was, among other things, a forecasting tool.


Then in 1922 the English mathematician[Lewis Fry Richardson](https://en.wikipedia.org/wiki/Lewis_Fry_Richardson) tried to do the same thing for weather, by hand: six weeks of arithmetic to predict six hours. He got it spectacularly wrong — his forecast called for hurricane-force pressure jumps that never happened — but the *approach* , discretize the physics and integrate forward, survived. Every modern weather model is a descendant.


The lesson was that **if you know the mechanism, you can simulate it** . The problem: for most things we want to forecast in the enterprise — demand, prices, traffic, energy load, ad spend, hospital admissions — there are no equations. Demand for sneakers does not obey Navier–Stokes.


### Era 2 — Classical statistics (1950s–2000s)


When physics runs out, statistics step in.[Box and Jenkins](https://en.wikipedia.org/wiki/Box%E2%80%93Jenkins_method) published *Time Series Analysis* in 1970 and gave the world ARIMA. State-space models, exponential smoothing, Holt-Winters, ETS, VAR are the workhorses of every BI tool, every Excel macro, every` forecast()` function in your favorite stats package.


Two things made them durable:


- **They are explainable.** Every parameter maps to something you can defend in a meeting. An MA(1) coefficient of, say, 0.5 says *half of yesterday's surprise carries into today's value* — the kind of statement a planner or CFO likes to sanity-check. A seasonality term means what it sounds like: how much higher December is than the trend.
- **They are cheap.** Fit one model per series on a laptop, and you're done.


The same trait is their main issue. Classical models are **local** — every time series is an island. There is no mechanism for the model to learn that the 50,000 SKUs in a category all spike during Black Friday, or that a brand-new product behaves like other products in its family. **Knowledge does not transfer.** And cold-starts were handled by hand: a planner picked an analogue SKU, copied its seasonal profile, and argued about the launch multiplier in a meeting. That works, in the sense that one person does it one item at a time. It does not survive forty thousand launches a year.


### Era 3 — Machine learning and the Amazon turn (2010s)


A clearer view of the next step comes from Amazon's demand forecasting team. They started with classical statistical models, then added specialized components on top — seasonal pattern modules, a Distribution Engine for forecast-error estimation, separate logic for new products. Over time, this layered approach became hard to maintain.[Amazon Science has the full history](https://www.amazon.science/latest-news/the-history-of-amazons-forecasting-algorithm) ; it is the most public case study the field has.


Kari Torkkola, a senior principal research scientist on the team, put it in one sentence: *"There are products across multiple categories that behave the same way."* The team built **Sparse Quantile Random Forests** — global tree models trained across the entire catalog, producing quantile forecasts instead of point estimates. One model now covered the whole catalog, and a new product inherited the shape of the products it resembled.


Where the knowledge lives


One model per series


ARIMA, ETS — Era II


series 1


series 2


series 3


series 4


series 5


series 6


Each series gets its own model. Knowledge does not transfer.


One model for all series


SQRF, DeepAR, foundation models — Era III+


Global model


s1


s2


s3


s4


s5


s6


One model pools across all series; patterns transfer.


The move from one model per series to one model across series is the highest-leverage change in the field's history.


In parallel, the time series community was working out the same thing on a smaller scale, on Kaggle. The **M5 competition** (Walmart, 2020) was won by gradient-boosted trees — LightGBM, mostly — running on heavy feature engineering. M5 also ran a parallel Uncertainty track, scored on quantile forecasts, and gradient-boosted trees won that too, with the top teams all using machine learning. Even probabilistic forecasting, deep learning's supposed home turf, belonged to LightGBM in 2020. For a long time, GBTs over carefully crafted lag and calendar features were *the* state of the art. On stable, single-frequency retail data, they are still the production standard, although that is changing.


The trade-off is real, though:


- Feature engineering is a **tax** . Every new business question — *"what if we ran a promo here?"* — becomes a feature design exercise. Data science teams get bottlenecked. Production waits.
- Tree-based global models still **struggle to extrapolate** trends or seasonalities they have not literally seen in training.
- The "global" of a GBT is global in the statistical sense, not the **transferable** sense. The model that learned Walmart's SKUs does not generalize to your business. It is stuck on the features you engineered for it.


### Era 4 — Deep learning (2017–2023)


By 2017, Amazon's team, and many others, were working to remove the feature-engineering tax.[DeepAR](https://arxiv.org/abs/1704.04110) (Amazon, 2017) trained an RNN jointly across all series in a catalog, outputting a probability distribution at each future step.[MQ-RNN](https://arxiv.org/abs/1711.11053) (2017) and[MQ-Transformer](https://arxiv.org/abs/2009.14799) (Amazon, 2020) pushed further, using attention to learn relevant features automatically. Amazon has reported a **15× improvement in forecast accuracy** over its previous stack, and was able to retire much of the legacy machinery. Treat that number as directional rather than as a measurement: it is a company-reported figure with no public metric, horizon, or baseline attached.


Outside Amazon:[N-BEATS](https://arxiv.org/abs/1905.10437) (Oreshkin et al., 2019), the[Temporal Fusion Transformer](https://arxiv.org/abs/1912.09363) (Google, 2019),[Informer](https://arxiv.org/abs/2012.07436) ,[PatchTST](https://arxiv.org/abs/2211.14730) . Lots of architectures, lots of papers, lots of leaderboards.


One footnote operational forecasters will recognize: this is also the era when[Prophet](https://facebook.github.io/prophet/) (Facebook, 2017) became the default open-source forecasting tool — a generalized additive model with built-in seasonality, holidays, and changepoints, fit by MAP rather than full posterior sampling. Prophet was useful: sensible defaults, decent uncertainty, a one-line API. It was also a problem. The same defaults produced confident-looking but often mediocre forecasts, and independent benchmarks on M-competition data repeatedly showed Prophet underperforming both simple statistical baselines and well-tuned GBTs. A lot of teams shipped Prophet because it was easy, not because it was good. Proper Bayesian models ([PyMC](https://www.pymc.io/) ,[Stan](https://mc-stan.org/) ,[NumPyro](https://num.pyro.ai/) , and Google's[Bayesian structural time series](https://research.google/pubs/inferring-causal-impact-using-bayesian-structural-time-series-models/) ) remained valuable when you needed posterior uncertainty over latent dynamics — trend, seasonality, and the effect of an intervention, each with credible intervals you could defend to stakeholders.


The Bayesian tradition also planted an idea the rest of the field would spend a decade rediscovering: the **prior** . A hierarchical Bayesian model can *partially pool* across series — a brand-new SKU borrows strength from the category, regularized toward a shared distribution until it earns its own parameters. That is exactly the "products behave the same way" intuition behind Amazon's global turn, written in the language of priors and posteriors years earlier. The catch was scale. Full posterior inference over millions of series with MCMC is computationally hopeless, and on raw point accuracy these models rarely beat the global tree models. The idea was right, but the machinery wasn't there. Era 5 is, in large part, what happens when a *learned* prior finally works at scale.


And yet, in many production settings, deep models did not clearly beat well-tuned GBTs. They were finicky to train, data-hungry, harder to interpret, and the wins did not always survive outside the lab. M5 illustrated this directly: LightGBM-based models took most of the documented top positions in the final standings — though an ensemble of 43 LSTM networks took third place, a reminder the gap was narrowing. Into the early 2020s, LightGBM remained a default choice for many production forecasting systems.


The pattern is familiar from other domains: **deep learning needed scale to pay off** , and in time series the data *per task* is small. A single retailer's catalog, even a big one, is tiny compared to the text of the internet. The model couldn't justify its capacity. That is the puzzle the next era is solving.


### Era 5 — Foundation models (late 2023–present)


The move is the one that worked in NLP: stop training one model per task. Train **one very large model on as many time series as you can find** , and let it generalize. The first wave landed in 2023–2024:


- [TimeGPT](https://arxiv.org/abs/2310.03589) (Nixtla, 2023) — the first commercial zero-shot forecasting API, transformer-based.
- [Lag-Llama](https://arxiv.org/abs/2310.08278) (2023) — open-source decoder-only model with lagged-value tokenization.
- [Chronos](https://arxiv.org/abs/2403.07815) (Amazon, 2024) — discretizes values into tokens and uses a standard language-model architecture, trained on a large mix of public datasets and synthetic series.
- [Moirai](https://arxiv.org/abs/2402.02592) (Salesforce, 2024) — multivariate and any-frequency from the start.
- [TimesFM](https://arxiv.org/abs/2310.10688) (Google, 2023) — 200M-parameter decoder-only model, trained on 100B time points.
- [Moment](https://arxiv.org/abs/2402.03885) (CMU, 2024) — encoder-style, multi-task.


We started talking about founding The Forecasting Company in Summer 2022, because we knew this was where the field was headed. We incorporated in July 2024, at a time when foundation models were still below par on customer data.


In late 2024, the nascent field got a first benchmark:[GIFT-Eval](https://arxiv.org/abs/2410.10393) (Salesforce) — 23 datasets, 144k+ series, 97 evaluation configs, with a[live leaderboard](https://huggingface.co/spaces/Salesforce/GIFT-Eval) that anyone can submit to. No single model dominates, and strong statistical and ML baselines remain competitive on parts of the benchmark — but foundation models occupy most of the top positions. Read the ordering as coarse: submissions are self-reported, and every zero-shot number rests on a claim not always audited: that the evaluation series were not in the pretraining corpus. The rankings are not always statistically significant, but there is some signal.


The second wave, from 2025, is where covariates and multivariate support arrive:


The second wave · 2025–2026


Chronos-2


2025


Amazon


Univariate + multivariate + covariates in a single zero-shot model.


Sundial


2025


THUML


Trained on a trillion-point corpus with a flow-matching loss. ICML 2025.


Toto 2.0


2026


Datadog


Decoder-only with alternating time/variate attention. Five sizes from 4M to 2.5B.


Moirai 2.0


2025


Salesforce


Dropped multivariate support — an explicit bet on univariate simplicity.


TimesFM 2.5


2025


Google


200M params, 16k-token context, continuous quantile forecasts to 1k-step horizon.


t0-alpha


2026


The Forecasting Company


102M params, open weights under Apache-2.0, historical and future-known covariates.


Six of the past year's releases, and not the whole field — the GIFT-Eval leaderboard lists many more. Time series went from zero deployed foundation models in 2023 to a crowded market of them.


Most of the models above are *pre-trained univariate* : pre-trained on many series, but at inference they look at one series at a time. The natural next step is the **pre-trained global** model — pre-trained, *and* multi-series at inference, transferring information between related series in a single forward pass. Moirai and TimeGPT took related series and covariates before it; Chronos-2 is the first where it works well enough to build on.


That this is hard, rather than merely undone, is easiest to see in who has walked away from it: Moirai shipped multivariate support in 2024, and **Moirai 2.0 dropped it** — an explicit bet that univariate simplicity pays better than multivariate generality. We think that trade is the wrong way round for operational forecasting, where the related series and the covariates *are* the signal. It is one of the bets at the heart of what we are building.


The mechanism this rides on is[in-context learning](https://arxiv.org/abs/2005.14165) . The same trick that lets GPT-3 pick up a new task from a few examples in its prompt — without any gradient updates — is what lets a pre-trained TSFM produce a sensible forecast for a series it has never seen.


This is the Bayesian dream from Era 4, finally made to scale. Pretraining on millions of series is how the model *learns its prior* — what trends, seasonalities, and shocks look like across the whole universe of time series. At inference, conditioning on your history is the **update** : the forecast is, in effect, an approximate posterior predictive, computed in a single forward pass instead of by MCMC. The field even has a name for it — the model is trained to perform[amortized, or in-context , Bayesian inference](https://arxiv.org/abs/2112.10510) : it pays the cost of inference once, during pretraining, and reuses it for free on every new series. Hierarchical Bayes wanted to pool strength across series and quantify uncertainty honestly; it just couldn't afford the compute. A foundation model gets both — a prior learned from far more data than any single hierarchy ever saw, and probabilistic forecasts by default.


For the pre-trained global case, the context window carries *several* related series and any known covariates, and the model learns the relationships between them on the fly. No fine-tuning, no retraining, no MLOps pipeline per customer. The context is the API.


The series is the prompt


← context (prompt)


forecast (continuation) →


context length


80 / 140


weekly seasonality + slow trend. No fine-tuning. No per-series training. The only knob is how much of the series you show the model


— exactly like passing more tokens to an LLM.


The context is the API. The same model handles retail demand, electricity load, and traffic spikes — the series itself tells it what to do.


## Why this is finally a step change for practitioners


The first four eras left the same job in your lap: bring the data, engineer the features, pick the model, train it, tune it, deploy it, monitor it, re-tune it. Foundation models change the contract.


### 1. No training, day-one forecasts


Point a TSFM at a series it has never seen, and you get a probabilistic forecast in seconds. The pretraining did the work.


Pretraining alone does not solve cold-start. A model handed a brand-new series and nothing else has nothing to condition on — zero history is zero history, however large the prior. What solves cold-start is two things the model needs *in its context* : **global conditioning** , meaning related series — the rest of the category, the same item in other stores, the neighboring sensor — and **static covariates** , the product attributes, category, store format and metadata that tell the model which analogues to borrow from. Supply both and a new SKU inherits the behavior of the items it resembles on day one. Withhold them and you are extrapolating from nothing, foundation model or not.


What Era 5 changes is the breadth of what gets borrowed. Era 3 and 4 global models could already pool across your own catalog. A foundation model brings a prior learned from millions of series outside it, and drops the fitting step — so cold-start moves from the worst case to a routine one, provided the context actually carries the analogues.


### 2. Cross-series knowledge is free


You no longer have to *teach* the model that retail spikes in November, that energy load follows temperature, or that intraday traffic has a lunch peak. It has already seen millions of analogous patterns. Your data only needs to **fine-tune the prior** , not invent it.


### 3. Probabilistic by default


Emitting quantiles was never the hard part.[Quantile regression](https://en.wikipedia.org/wiki/Quantile_regression) dates to Koenker and Bassett in 1978, ETS state-space models have produced predictive distributions for decades, and Amazon was shipping quantile forests in the early 2010s. What was hard, and expensive, was getting quantiles that are *calibrated* on your data, *coherent* across your hierarchy, and that **stay** calibrated when the world moves. That usually meant a bespoke modeling effort per problem.


What modern TSFMs change is that the full distribution is the default output rather than something you build toward. That matters because a decision layer cannot use a point estimate: *"How many units should we order to hit a 95% service level?"* requires a quantile, not a mean. It also raises the evaluation bar — a quantile you can read off is not the same as a quantile you can trust, and the difference only shows up in coverage on held-out data.


### 4. Less engineering, more iteration


Feature engineering does not disappear, but it stops being the bottleneck. Adding a new business question stops being a model design exercise and becomes a prompt-and-iterate loop: add the raw signal you think is relevant, and let the model learn what to do. The slow part of building a forecast becomes thinking about *information* , and *decisions* .


### 5. The infrastructure dividend


The biggest, quietest win: **forecasting becomes infrastructure** . One model — versioned, monitored, governed — can serve replenishment, finance, ops, pricing, and headcount planning. The bespoke pipelines that classical workflows accumulate can be consolidated, the way Amazon eventually consolidated its earlier stack.


## NLP's playbook, replayed


Time series is following NLP's playbook three to four years behind, and the gap is closing.


NLP and time series, on the same axis


era


NLP · 2020


GPT-3 — in-context learning


Time series · 2023


Chronos, Moirai, TimesFM — first foundation models


Three to four years behind, and closing. Everything NLP figured out about pretraining, attention, and in-context learning has been compressed into time series in under a decade.


It took NLP nearly sixty years to get from rule-based parsers to GPT-4. Time series has had that path compressed by everything NLP worked out along the way: attention, pretraining, instruction tuning, agent integration. What does not come for free is everything specific to time series — covariates, hierarchies, irregular sampling, intermittency, and the information-cutoff discipline from Part 2. That part is still open work, and it is what we spend our days on. **We do not expect the next leap to take a decade.**


## What's still hard


Nothing above says the problem is solved.


- **The univariate gap.** Many published TSFMs still see one series at a time. They cannot natively use weather, promotions, competitor pricing, macroeconomic context, or related series. For many real businesses, those signals *are* the signal. Chronos-2 takes covariates of both kinds, historical and future-known, and so does our own` t0-alpha` . The set of models that do is still small, and much of the open ecosystem is still catching up.
- **The covariate gap.** Most models that take exogenous inputs make them awkward to supply, and the gains are inconsistent. Real forecasting systems live and die on covariates and known-future variables (holidays, scheduled promos, weather).
- **Specialists still win on the problem they are working on.** A great forecaster with a well-tuned LightGBM on three years of clean retail data often still beats a zero-shot model. The gap is closing fast: when our founding data scientist joined, he fit custom CatBoost models as baselines on every customer backtest, and would often win. He has stopped, because it became a waste of time. What he works on now is the problem, and which data carries signal. What does not scale in production is the specialist. The model is not the asset — the person is, along with the feature set, the outlier rules, and the knowledge that store 4471 reopened in March. That person can only point at a handful of problems at a time, so the top few percent of series by revenue get real attention and the long tail gets a default nobody has looked at in two years. We are a force multiplier for the few specialists a company can afford, not a replacement for them.
- **Coherence across the hierarchy.**` t0-alpha` emits marginal quantiles, one series at a time. Marginals do not reconcile:[the P90 of a sum is not the sum of the P90s](https://www.theforecastingcompany.com/en/blog/time-series-are-not-tables) . Coherent quantiles today need a reconciliation step on top of the model. We will ship that step.` t0-alpha` attends across related series, so the conditioning is there. The output head is not: it emits one marginal per series, and marginals carry no correlation between them.
- **Intermittency.** At the bottom of a retail or spare-parts hierarchy most series are zero most days, and demand is integer-valued rather than continuous. Both break the quantile story: the median is identically zero, aggregate CRPS is dominated by the zero mass so a headline score can look fine while the non-zero days are wrong, and choosing which quantile to order on stops being obvious.
- **Trust and explainability.** A point estimate from ETS is auditable. A foundation model's forecast is, today, less so. What closes that gap is not the model but the evidence around it: backtests that stay stable across information cutoffs, calibration reports that show whether the quantiles are honest, and benchmarking on your data rather than on a public leaderboard. That is auditable today, and it is what we hand a customer. We are working on interpretability, and we expect this to change in the future.


Part of why we[benchmark every major TSFM](https://www.theforecastingcompany.com/en/blog/the-automated-forecaster) under a single harness — and publish the results to our customers — is that the right model still depends on the problem. The field is in the part of the S-curve where that is true.


## What needs to exist


Take the trajectory seriously and a few things follow.


- **True pre-trained global forecasters become the default.** Today's TSFMs are mostly pre-trained univariate — they see one series at a time. The next step is a single model that ingests your whole context: your sales history, your weather, your event calendar, your product text, your store metadata, your competitor signals — in one forward pass. The same way GPT reads images and code, the next forecaster reads everything around a time series.
- **Forecasting becomes a primitive, not a project.** Like an embedding model or an LLM, you call it and it works. The two-quarter *"build a model for X"* engagements that consume forecasting teams disappear.
- **Foundation models meet agents.** The forecaster is one tool inside a larger system that searches for context, joins it temporally, runs backtests, and produces a **decision** , not a number.
- **Specialists become fine-tunes, not bespoke builds.** Domain-specific models still exist, but they start from a foundation model, not from scratch. The lift to add a new vertical drops by an order of magnitude.


## What we're building


Our model is built on two years of open-source and open-science work. What is ours is the model and the data layer underneath it. Architecturally, it is a **causal decoder** in the spirit of Toto, and it uses **grouped attention** — alternating time-axis attention with group-axis attention so the model can reason about multiple related series jointly — a pattern shared with Toto 2.0 and Chronos-2.


It is trained with **contiguous patch masking** , and it sits on **Tolars** ,[the temporal data layer from Part 2](https://www.theforecastingcompany.com/en/blog/time-series-are-not-tables) . A time series is a record of what was known when, not a table you can slice arbitrarily, and a model is only as good as the history its data layer can faithfully reconstruct.


From day one we designed it to handle covariates of both kinds: **historical covariates** (past observed signals — weather, traffic, competitor activity) and **future-known covariates** (holidays, scheduled promotions, planned store openings, official calendars), which only a couple of models can handle. For most operational forecasting problems, those are the inputs that move the forecast. Static metadata is not among them yet; it is coming.


Univariate vs. global


target history


ground truth


forecast


related series


The model sees only the target's own past. It extrapolates the trend and weekly seasonality cleanly — and walks straight through the promotion


that everyone in the room already knows is coming.


Pre-trained univariate sees one series at a time. Pre-trained global reads the whole table — related series, covariates, known-future signals — and reasons about them jointly. Schematic illustration on synthetic data, not model output.


All of it runs inside the agentic harness from Part 1. Without the data layer it cannot ingest the world. Without the agentic harness it cannot search, reason, or recommend. Without the decision layer it cannot create value.


The first public iteration is already out:` t0-alpha` , 102M parameters, with open weights under Apache-2.0 on[Hugging Face](https://huggingface.co/theforecastingcompany/t0-alpha) . A post focused on` t0-alpha` is coming, with the architecture in full, the benchmark results, and the evaluation methodology behind them.


A later post covers the harness around the model — and what changes when forecasting stops being a number on a slide and becomes a decision in your operating system.


[Previously in The Future of Forecasting · Part 2 Time Series Are Not Tables](https://www.theforecastingcompany.com/en/blog/time-series-are-not-tables) Share
