---
schema_version: "1.0.0"
document_id: "8a78a01f8958fbc04f093c214e1923990331ffafb9f14feadeedf1c93dcad8c9"
company_key: "yc-silurian"
company: "Silurian"
source_id: "yc-silurian-news-import-778c8a4f4a8c"
canonical_url: "https://silurian.ai/blog/day-ahead-rime-ice-from-reactive-to-preventive"
published_at: "2025-10-24T06:30:26.698+00:00"
first_seen_at: "2026-07-24T00:50:25.898192+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:ca81d0fed735382f882e2db821c3c59bb234105a7be3dbad5c8b8854559e333c"
---

# Day-Ahead Rime-Ice: From Reactive to Preventive

When rime ice builds up on a transmission line, it’s already too late. The escalating weight and drag can lead to line clashing, flashovers, and catastrophic outages. For decades, utility companies have been forced to *react* to these events, often scrambling crews, managing costly repairs, and dealing with grid instability.


What if you could move from reaction to prevention?


## **The Gap: Why No One Offers This Operationally**


Reliable, asset-level rime-ice forecasts have been missing from the operator's toolkit for two simple reasons:


1. **NWP Models lack sharpness** : Traditional numerical weather prediction (NWP) models forecast at a multi-kilometer scale. This "smeared out" view misses the micro-scale physics that create ice on a specific transmission line. The result is vague guidance for a region, not actionable intelligence for an asset.
2. **You Can't "Tack On" Precision** : Many localized AI/ML solutions merely post-process the imprecise NWP forecasts. This approach is unable to generate a dependable rime-ice forecast that was absent from the core model. You’re correcting a signal that was never there.


## **A New Capability: GFT-HQ Learning Rime-Ice Natively**


Our collaboration with Hydro-Québec validates this new approach. By post-training Silurian’s GFT weather foundation model with data from Hydro-Québec’s own network of icing sensors, we created GFT-HQ.


This model learns to natively forecast rime-ice, rather than just post-processing a NWP forecast. The result is a new, *operational* capability: GFT-HQ achieves an Average Precision of 0.73 for day-ahead rime-ice detection. This provides operators with a reliable, full-day warning that aligns directly with their existing planning windows.


Climatology


3× lift


AP = 0.37


GFT-HQ


8× lift


AP = 0.72


Figure: 8x Lift over Random Chance in predicting rime-ice risk in the next 24h


Beyond aggregate skill, we also analyzed the model's ability to capture high-impact episodes that posed particular challenges for Hydro-Québec’s operations.


Figure: Romaine rime-ice event (Nov 2024) at two nearby Sygivre stations 11 km apart. The black strip labelled “Obs” is the observed binary occurrence. ERIC_C (left): Stable 3-day advance warning before Romaine transmission line collapse. MONTAG_C (right): Weak signal 11km away. GFT captures the strong micro-scale differences despite close proximity of the locations.


## **What This Unlocks: From Reactive to Proactive**


-


01


Prevent false dispatches


### Targeted De-icing


Shift from reacting to surprise accretion events to scheduling proactive crew runs on priority spans with quantified false-dispatch risk.


-


02


Derate with context


### Hazard-Aware DLR


Fuse probabilistic icing trajectories with your DLR engine so you can preemptively derate corridors before load threatens system stability.


-


03


Day-ahead foresight


### Smarter Market Decisions


Fold weather-driven outage probabilities into hedging, unit commitment, and congestion planning to improve revenue capture.


-


04


Unified forecasting


### Single, Coherent Model


Leverage the same GFT backbone to enhance wind, temperature, and precipitation guidance wherever sensors report.


As atmospheric volatility increases, the cost of a failed forecast is amplifying. To move beyond simple precision to true decision optimization, a new approach is required.


The GFT-HQ model proves this new capability is unlocked by post-training weather foundation models with high-value, operational sensor data.


**Don't just monitor. Predict.** See how your sensor network can power a new class of high-precision forecasts. Read more in the preprint[here](https://arxiv.org/html/2509.25268v1) .
