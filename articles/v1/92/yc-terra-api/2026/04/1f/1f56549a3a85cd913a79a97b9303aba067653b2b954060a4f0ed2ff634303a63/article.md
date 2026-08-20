---
schema_version: "1.0.0"
document_id: "1f56549a3a85cd913a79a97b9303aba067653b2b954060a4f0ed2ff634303a63"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/tube-strikes-london-activity"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-26T02:41:58.986769+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:504f268300e39fc644c661960dbabe7cb5ca157d8d17c6667a605ffee974316a"
---

# Tube Strikes Made Londoners Active | Terra

## The surge of e-bikes


Whether you are team Lime or team Forest, one thing is the same: if it’s a standard UK e-bike, it should stop assisting at around 25 km/h.


To separate likely e-bike rides from normal bike rides, we used unsupervised k-means clustering with k = 2 on sampled weekday commute speed traces. Each ride was represented using cap-sensitive features: median speed, 90th percentile speed, speed variability, maximum speed, time spent above 27 km/h, time spent above 28 km/h, the integrated area above the 25 km/h cap, and median absolute acceleration. All features were z-score standardised before clustering. The feature set was chosen to maximise separation in PCA space while keeping the split interpretable. The cluster with lower upper-tail speeds and much less time above the cap was labelled E-bike, and the other cluster was labelled Normal bike.


The biggest discriminatory feature was not average speed but what happens in the upper tail of speed distributions. That is exactly what you would expect if one group is being constrained by a speed limit and the other is not.


***Figure 2:** Left: the two ride clusters in PCA space. The traces show a typical e-bike-like ride and a typical normal-bike-like ride. Right: the features that most strongly separate them.*


Once those activities were labelled, the strike effect was pretty clear. In the sampled rides:


- on non-strike weekdays, **54.9%** activities were classified as e-bike
- on strike weekdays, **67.5%** activities were classified as e-bike
- median speed on strike days drops from 23.11 km/h to 21.44 km/h


***Figure 3:** Estimated share of e-bike rides in sampled weekday activities. During the strike days, the e-bike share rises from 54.9% to 67.5%.*


## Where do people commute to and from?


The spatial picture is just as interesting. The weekday map includes 1,566 cycling activities from 289 users. Blue circles show likely trip sources (where people go from), and red circles show likely sinks (where people go to). Can you find your local e-bike dock inside a blue circle and your office inside a red circle?


***Figure 4:** Rounded weekday source and sink locations during the April 2026 strike period. Blue marks likely trip origins; red marks likely destinations.*


Another interesting finding, people tend to live close to work and commutes are often quite local. While Central London still acts as a major sink, many patterns also show short-to-medium commutes between nearby residential and work areas. For example, people living around Wimbledon tend to commute further south-west.


***Figure 5:** Smoothed weekday commute field during the strike period. Blue regions act more like trip sources; red regions act more like trip sinks.*


## What this says about London


None of this is especially surprising after experiencing the strike first hand. When the Tube becomes unreliable, London does not stop moving and people cycle more, often relying on e-bikes. TfL strikes may disrupt the network but they also reveal something else about the city: Londoners are very good at finding another way home.
