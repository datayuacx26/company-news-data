---
schema_version: "1.0.0"
document_id: "9c3a38306a123b68050482fcf7761b492d355597e5b4ae636bbd60d557f02c1c"
company_key: "yc-silurian"
company: "Silurian"
source_id: "yc-silurian-news-import-778c8a4f4a8c"
canonical_url: "https://silurian.ai/blog/gft-for-the-power-grid"
published_at: "2025-10-08T16:00:00+00:00"
first_seen_at: "2026-07-24T00:50:25.898192+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:689bcff778cb9ead80876abf42a46147a6793c41666339b1995d115f7a4f8417"
---

# GFT for the Power Grid

## The problem: weather forecasts are often far from decisions


Traditional weather forecasts, while informative about general conditions like freezing temperatures, often fall short of providing the critical, actionable insights needed by utility operators. The gap between generic weather data and the operational realities of a power grid, such as predicting if a transmission line is at risk of an ice-induced outage, represents a significant vulnerability. A missed forecast can result in substantial damage, widespread service disruptions, and extensive blackouts.


Silurian’s innovative approach bridges this gap. Together with Hydro-Québec, we have demonstrated how GFT transcends basic weather prediction, focusing instead on forecasting the precise impact of weather phenomena on individual assets within a power grid. By post-training our GFT with direct access to sensor data and historical outcomes specific to a utility's infrastructure, we transform general forecasts into highly refined, asset-aware operational intelligence.


Instead of simply producing "better weather information," we deliver probability-calibrated thresholds and specific actions tailored to the unique characteristics of each asset.


### Surface Temperature


15%


MAE reduced


vs. legacy baseline


Day-ahead temperature now land 15% closer to observed conditions, giving operators tighter look into expected load demand.


### Hub height windspeed


20%


MAE reduced


vs. legacy baseline


Windspeed forecasts now 20% lower uncertainty, giving operators tighter dispatch control.


### Total Precipitation


35%


MAE reduced


vs. legacy baseline


Forecast mean absolute error reduced by 35% for better hydro-flow decision making.


### Rime ice detection


0.72


AP


vs. climatology


Several hours of advance warning for potential rime-ice outages keeps transmission operators ahead of cascading failures.


Performance deltas benchmarked for Jan 2024 to March 2025.


## Why post-training (not post-processing) is the unlock


The typical approach of the existing services is to interpret weather forecasts *after* they have been generated. This method, known as **post-processing** , relies on educated guesses to infer impacts or derived products. Our methodology, termed **post-training** , represents a fundamental paradigm shift.


### Post-processing


Like providing a generic weather map to an analyst and tasking them with speculating on what it might do to a specific power line. Interpretation lives outside the model and depends on broad, situation-agnostic heuristics.


Interpretation lives outside the model.


### Post-training


Like embedding an engineer with decades of domain knowledge about your exact hardware directly into the model. Expertise lives inside the model and captures the nuanced links between weather states and asset outcomes.


Expertise is baked into the model.


High-resolution weather grids may be a commodity, but a probability-calibrated, asset-specific icing forecast for your most critical corridors is not. This requires post-training on your data. Historically, weather has been a liability for grid operators—information without agency. You know a storm is coming, but not precisely which assets are vulnerable, when to act, or what actions to take. Our approach fundamentally changes this equation transforming weather from a liability into an operational advantage through the true power of GFT.


Please read[the pre-print](https://www.arxiv.org/abs/2509.25268) for more details.
