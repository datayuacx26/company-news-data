---
schema_version: "1.0.0"
document_id: "62ee8c585f6d9a60509dc76ee4e3d4ecc13678e42b7df187021d008053ff3e4a"
company_key: "yc-the-forecasting-company"
company: "The Forecasting Company"
source_id: "yc-the-forecasting-company-news-import-6163f6ce3bb7"
canonical_url: "https://www.theforecastingcompany.com/en/blog/time-series-are-not-tables"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-24T04:24:30.097615+00:00"
fetched_at: "2026-07-28T22:13:05.421454+00:00"
content_hash: "sha256:adea0b8eac6aa5ac1eb9c959ca83c108759524663dfc782c514cb86b15f7d881"
---

# Time Series Are Not Tables

# Time Series Are Not Tables


By Geoff · May 18, 2026


Share


Part 2 of 5


·


The Future of Forecasting


Open any data science textbook and you'll find time series data stored in CSV files, loaded into DataFrames, and treated as rows in a table. This seems natural at first glance — after all, a time series is just a list of (timestamp, value) pairs, right?


Wrong. And this misunderstanding is costing organizations real money. Hedge funds and Amazon know this already: **time series are not tables.**


## The table illusion


The recent wave of *tabular foundation models* — models like[TabPFN](https://arxiv.org/abs/2207.01848) or[TabICL](https://arxiv.org/abs/2502.05564) that are pretrained on diverse tabular datasets and can perform classification and regression zero-shot — have been remarkably useful for general-purpose prediction tasks. People have long used tree-based regressors like Catboost or LightGBM for time-series forecasts, so it seemed reasonable to use the new shiny tabular models on the block for this too, right?


On the surface, it works. You create (a *lot* of) columns for lag features, rolling averages, calendar variables, and feed the table to a model. Gradient boosted trees dominated the M5 forecasting competition (Walmart, 2020) using exactly this approach. More recently,[TabPFN-TS](https://arxiv.org/abs/2501.02945) reformulates forecasting as tabular regression and achieves strong results on covariate-informed benchmarks — with zero time-series-specific pretraining.


This surface-level compatibility hides deep structural differences. Tabular foundation models treat rows as exchangeable (the independent, and identically distributed assumption of tabular data), which means it *cannot natively encode the arrow of time* . It fails to extrapolate simple linear trends. Its confidence intervals don't account for temporal autocorrelation. It doesn't understand regime changes. You also cannot batch different time series without mixing information, which makes predictions very slow across the large volumes that are typical in forecasting setups.


The cleanest way to feel the difference is to look at the same data both ways, and then shuffle the rows.


As a table


timestamp


value


- 2026-Jan-01


20.00


- 2026-Jan-02


21.78


- 2026-Jan-03


27.69


- 2026-Jan-04


33.70


- 2026-Jan-05


34.51


- 2026-Jan-06


32.38


- 2026-Jan-07


32.05


- 2026-Jan-08


32.70


… 40 more rows


The table looks the same shuffled or sorted. Rows look exchangeable.


As a trajectory


Trend and weekly seasonality, clearly readable from order alone.


Same rows, two stories. Tabular models treat rows as exchangeable; in a time series, order *is* the signal.


A table you sort by` timestamp` looks identical to a table you sort by anything else. A trajectory does not. Everything below follows from that asymmetry.


## What makes time series special


### 1. Time is not just another column


In a table, all columns are created equal. But in time series data, the time dimension is *structurally different* from all others. It implies ordering, local correlation in time, and correlation across simultaneous events.


Tables have rows. Time series are **trajectories** .


### 2. Time is irregular by convention


If nothing about a business changes, should you expect the same monthly revenue in February and March? No — February has 28 days, March has 31. That's a 10% difference. Holidays shift between weekdays and weekends. Daylight saving time means some days have 23 hours and others have 25 (and this depends on where you are!).


Monthly revenue (same daily rate)


28


30


31


╱╱


31


Jan


28


Feb


31


Mar


30


Apr


31


May


30


Jun


31


Jul


31


Aug


30


Sep


31


Oct


30


Nov


31


Dec


Y-axis truncated to 25–32 days to make the small but real differences readable. Bars are proportional within that window.


Same business, identical daily rate. February still looks 10% smaller


just because it has fewer days. A naive YoY comparison will treat this as a real drop.


Calendars are messy on purpose. A correct temporal layer normalizes before the model ever sees the data.


### 3. Two kinds of temporal data


Physics distinguishes between **extensive quantities (volumes)** — revenue, transactions, production output — and **intensive quantities (rates / levels)** — temperature, pressure, exchange rates.


Missing data means completely different things for each type:


- A missing day of **revenue** likely means *zero* (the store was closed). You add it as zero, or you don't add anything at all when aggregating.
- A missing day of **temperature** does *not* mean it was 0°C. You interpolate. You never sum.


A tabular schema with a single "value" column can't carry this distinction. A temporal type system can.


### 4. The hierarchy problem


Most real-world forecasting happens in hierarchies. A retailer needs forecasts at the level of SKU × store × day, but also at the level of category × region × week, and at the level of total revenue × country × month. Every level matters: SKU-day for replenishment, category-week for buying, total-month for finance.


The catch: these forecasts must be **coherent** : correlations must be accounted for. If the SKU forecasts sum to one number and the category forecast says something different, your operations team and your CFO are looking at incompatible plans.


Forecast hierarchy


National


430


Region A


240


Region B


175


Store 1


100


Store 2


120


Store 3


80


Store 4


110


Three forecasts produced by three independent models. Sum of stores = 410


, but the national forecast says 430


. Coherence is broken


.


Independent forecasts don't add up. CLOVER enforces probabilistic coherence across the lattice by construction.


You cannot simply add quantiles. The P90 of a sum is not the sum of the P90s. We've[studied this problem formally](https://arxiv.org/abs/2307.09797) — the CLOVER method (co-developed when I was at Amazon Forecasting Science) achieves probabilistic coherence by construction.


### 5. Three kinds of input, not one


Time series forecasting has **three distinct types of input** :


- **Historical variables** — the past of the target series and other observed series.
- **Future-known variables** — holidays, scheduled promotions, store openings, official calendars.
- **Static variables** — product descriptions, store coordinates, category metadata.


Three kinds of input


Past variables stop at` t₀` — everything to its right is unknown. Future-known variables continue across the boundary because we already know them on both sides. Static features aren't on the time axis at all — they're *metadata* attached to the series (a product SKU, a store location, a region). A table treats all three as identical columns; a temporal system reasons about where each one lives in time


.


Three input types, three different roles. Flattening them loses the thing that makes them useful.


Tables flatten all three into identical columns, losing critical semantic distinctions. The model has no way to know that "is_holiday" extends into the future but "yesterday's revenue" does not. Either you leak future information into training, or you starve the model of legitimately knowable context. Both are common; both are silently wrong.


### 6. The two-dates problem


Every observation has two temporal coordinates: the **event date** and the **information date** . A hotel booking made March 1 for April 15 is *information from March 1 about demand on April 15* .


Booking date × stay date


Each cell is a count of bookings made on row Y for the stay on column X. Bookings can only be made before the stay, so half the matrix is empty by construction. Above the today


line: the bookings you actually know about. Below it: future bookings that haven't happened yet. A table that collapses this to one number per stay date silently mixes the two — and trains a model that knows things it couldn't have known at forecast time. Hover any cell to inspect its lead time.


Two date coordinates per fact: when it was learned, and what it was about. Inspired by Harrison Katz's[analysis of Airbnb booking dynamics through COVID](https://medium.com/airbnb-engineering/what-covid-did-to-our-forecasting-models-and-what-we-built-to-handle-the-next-shock-ccbf0e1f7fa9) .


This leads to the concept of the **information cutoff date** — and it can be different for different variables. Sales arrive overnight; web traffic arrives in seconds; macroeconomic indicators arrive months late and get revised.


### 7. Temporal joins are hard


Joining weather data with restaurant traffic looks like a one-line operation. It is not. You need to decide which hours matter (lunch vs. dinner peaks differ by city), what geographic resolution to align (airport-level weather, store-level traffic), whether to lag or lead (a forecasted afternoon thunderstorm shifts lunch one way, an observed one shifts it the other), and how to handle different reporting cadences.


A table model treats all features as contemporaneous. A temporal system reasons about *alignment* : which value of` weather` was knowable, in which geography, at the moment a customer decided whether to walk to the restaurant.


### 8. Data isn't just missing — it's corrupted in structured ways


**Censored data** is everywhere. Demand capped by inventory. Restaurants turning away guests when full. Call centers dropping calls. Energy demand throttled by the grid.


Demand vs. inventory cap


Cap


72


true demand


observed (sales)


0.0%


of demand lost to the cap


Train on the blue line and the model learns the cap. Train on the dashed line and it learns the business.


Training on censored data as if it were ground truth creates a vicious cycle of underforecasting: the model learns the cap, the planner under-orders, demand gets censored harder, the model learns a lower cap.


## Building a temporal data layer


These eight problems are not edge cases. They are the everyday substrate of operational forecasting. Solving them in glue code, dataset by dataset, is how forecasting teams end up with thousands of bespoke pipelines that nobody trusts.


Our temporal engine, **Tolars** , addresses them in a single layer. It is written in Rust with Apache Arrow integration.


Instants vs. durations


Levels live on instants; volumes live on intervals. The type system tracks the difference.


Calendar-aware arithmetic


Three axes — months, days, microseconds — composed correctly, so "+1 month" and DST do the right thing.


Frequencies as tilings


Frequencies are mathematical objects, not strings. Resampling becomes a typed operation, not a heuristic.


Hierarchies as lattices


Hierarchical dimensions are first-class lattices, so coherence and aggregation are guarantees, not afterthoughts.


A few of the abstractions Tolars makes first-class. Each one is a problem that, in tabular land, becomes a slow correctness bug.


Stay tuned for more.


## Why all of this points to decoder models


Everything described above leads to an architectural conclusion: **time series models should be causal decoders, not encoders.**


A causal decoder can only look backward — the correct inductive bias for temporal data. It makes backtesting at scale possible via teacher forcing, because every position in the sequence is simultaneously a training example *and* a held-out evaluation point. And it makes information leakage from the future structurally impossible, not just a thing you remember to check for.


Attention mask


In a causal decoder


, every position can only attend to itself and the past. Information leakage from the future is structurally impossible


— exactly the inductive bias time has. Hover a row to see what that token attends to.


Causality isn't a regularizer — it's the shape of the problem. Decoders bake it into the architecture.


Encoder-style models — and tabular models, which are encoders by another name — have no notion of "past" baked into their architecture. They rely on the practitioner to mask carefully, to engineer features without peeking, to validate on the right splits. Sometimes the practitioner gets it right. More often, time leaks in quietly and the offline metrics look great until production starts forecasting tomorrow.


That's why our foundation model — and the temporal engine underneath it — are built decoder-first, on top of a data layer that takes time seriously. The next post in this series digs into what's inside that model, and how it got there. More coming in part 3.


[Previously in The Future of Forecasting · Part 1 The Automated Forecaster](https://www.theforecastingcompany.com/en/blog/the-automated-forecaster) Next in The Future of Forecasting · Part 3


From ARIMA to Foundation Models


Coming soon


Share
