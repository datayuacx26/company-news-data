---
schema_version: "1.0.0"
document_id: "bfe19cc885e4c57659e4f16d110e8c7b8a2b92a8eaaf77cba2e9bf45229972c3"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/do-heatwaves-make-you-sleep-less"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-26T02:41:58.986769+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:ee16c059e07d5d6e739842747129ddbc4960b7a95e312cc4b6332e68947f13fa"
---

# Heatwaves and Sleep | Terra

## Cohort and Design


These are wearable-owning, GPS-active users and are not population-representative. Location comes from median activity GPS, collapsed into coarse regions. Weather is matched at region centroids.


I used three lenses: region-night time series, dose–response deciles, and within-person models (each user's hot nights vs their cool nights, day-of-week held fixed). I excluded wakeup counts from this analysis; only 1.7% of nights in the data reported them.


## Warmer Nights = Less Sleep (1.4 minutes lost per degree)


Within-person demeaned regression:


- -1.40 min asleep per +1°C night minimum temperature
- -1.48 min asleep per +1°C night mean WBGT


When I did a hot-vs-cool quartile comparison across 34,360 users with ≥14 nights of data, the median sleep change was −11.1 minutes between each user's hottest and coolest quartile nights.


I presumed that this decrease in time asleep would come from more awake time; that’s what hot nights feel like to me. I go to bed, and the discomfort keeps me uncomfortably awake. But I was wrong.


Metric Within-person effect per +1°C Interpretation


Time asleep −1.40 min Less total sleep


Time in bed −1.49 min Shorter nights overall


Time awake in bed +0.05 min Negligible — not the driver


Asleep ÷ in bed (calculated) not significant Ratio unchanged


Hot nights do not keep people awake longer. People end their nights earlier, or start them later, and total time in bed contracts by almost the same amount as lost sleep. Calculated sleep efficiency (asleep ÷ in bed) is flat within-person: both numerator and denominator shrink together.


The data shows a compressed sleep window, not a particularly restless one. A small but statistically significant rise in awake-in-bed time (+0.05 min/°C; +0.25 min median hot vs cool) exists, but it is an order of magnitude smaller than the sleep loss.


Regular readers will know I’m not a great fan of wearable determined sleep stages, but I still like to report them. Here, REM takes the hardest proportional hit during the shorter night (−0.60 min/°C vs −1.40 min total).


***Figure 1:** Regional time series of mean sleep (blue line), night minimum temperature (red line), and WBGT across seven European regions. Sleep troughs track temperature peaks through the heatwave window (yellow band), with the largest co-movement in France, Benelux/DACH, and the UK; Scandinavia shows a flatter response.*


## Dose–Response


Sleep moves steadily across night-temperature deciles with no visible threshold at 18°C, which was another surprise to me as I presumed there would be a threshold effect. There may still be at an individual level, and here it is just smoothed out by population variation. Awake-in-bed time is flat to slightly rising at the population level; the dominant signal is shorter sleep duration.


***Figure 2:** Population decile dose–response for sleep duration, deep sleep, and awake-in-bed time vs night minimum temperature. Total and deep sleep fall as nights get warmer; awake-in-bed time is essentially flat, heat shortens sleep, not by keeping people awake longer.*


## Regional Differences: Iberia is More Resilient


Regions accustomed to warm summer nights show smaller within-person sleep penalties:


Region Users Median Δsleep (hot − cool)


France 4,427 −15.5 min


UK / Ireland 18,295 −11.9 min


Benelux / DACH 5,775 −10.8 min


Italy 1,964 −10.5 min


Iberia 1,987 −7.0 min


North Europe (control) 1,083 +1.2 min


**Iberians lose roughly half the sleep of the French** on the same hot-vs-cool comparison, despite experiencing similar absolute night temperatures during the heatwave. Baseline Iberian nights already average about 19°C minimum, so we can presume these users are just more used to the conditions: building stock, behavior (later schedules, siesta culture), and physiology may all contribute. France and the UK, where warm nights are exceptional, show the largest hits.


Scandinavia, which swerved the heat wave, shows no meaningful signal and provides a useful null control.


**Figure 3:**


Within-person histograms of hot-minus-cool quartile night deltas for sleep, deep sleep, and awake-in-bed time. Most users lose sleep on their hottest nights (median −11 min); deep sleep falls too; awake-in-bed change clusters around zero.


## Humidity and Autonomic Strain


HRV (RMSSD) associated more clearly with WBGT (−0.019 ms/°C, p = 0.001) than with dry-bulb min temperature alone. Humidity is coming into play here, and it makes sense that muggy nights add autonomic load.


## Conclusion


A week of heatwave nights costs ~1.5 hours of cumulative sleep (eleven minutes × eight nights), drawn disproportionately from REM. The dominant mechanism powering this is behavioral compression of the sleep window (less time in bed), not lying awake for longer. That has practical implications: cooling the bedroom enough to stay in bed may matter more than managing fragmentation once you are there.


The standout sleeping performance of Iberians suggests adaptation is real. Regions facing more frequent warm nights may develop behavioural and infrastructural buffers that northern Europe lacks. As nighttime warming becomes routine, today's France may be tomorrow's normal, but today's Iberia shows the penalty can be smaller for populations already accustomed to heat.
