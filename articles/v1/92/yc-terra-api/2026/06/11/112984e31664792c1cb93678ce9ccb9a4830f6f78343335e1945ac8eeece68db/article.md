---
schema_version: "1.0.0"
document_id: "112984e31664792c1cb93678ce9ccb9a4830f6f78343335e1945ac8eeece68db"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/does-heat-make-you-run-slower"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-26T02:41:58.986769+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:92a048722d01983e47d9960ff0a30dfad9f8aafea8141f2a2c222b5deb4a60ab"
---

# Does Heat Make You Slow Down, or Tell You to Slow Down? | Terra

## What changes in the heat?


My comparison splits sessions into cool (starts below 20°C) and hot (starts at or above 20°C.) Median pace shifts from 6.19 to 6.27 minutes per kilometers. Median distance falls from 5.59 to 5.17 km. Median heart rate moves from 154 to 152 beats per minute: essentially flat.


The ridgeline distributions below show more than medians alone. I binned temperature into 4 bins (below 10°C, 10–20°C, 20–30°C, and above 30°C). The pace distribution drifts rightward and runs in clusters at slightly slower speeds. Distance distributions tell a clearer story: the long-run tail that stretches past fifteen kilometers in cool weather shrinks in warmer bins. heart rate distributions overlap heavily across moderate bands.


I think the widening of the curve is interesting here, and could lead to a wider range of user abilities going running these days. That was why I decided to move on to the person-level analysis.


***Figures 1-3:** Ridgeline distributions to visualize how the user population changes their Pace, Distance and Average Hr of runs in different temperature bins.*


## Same runners


Perhaps slower people simply run on hot days, creating a false association. I tested this extensively. Pooled regression and within-person demeaned models produce nearly identical pace coefficients, around 0.006 min/km per °C, so the link is not driven by who runs when.


Among sixty-nine runners with enough cool and hot sessions, sixty-one per cent were slower in the heat. Linear mixed-effects models give each runner their own baseline: pace rises by 0.009 min/km per °C (p < 0.001) and distance falls by 0.067 km per °C (p < 1×10⁻⁶).


The effect is real but modest. But, temperature explains less than 1% of the variance in pace. A twenty-degree swing might add roughly seventeen seconds per kilometers, a noticeable, but small, relative to day-to-day variation. Non-linear models do not outperform a simple linear fit, although data above 30°C are too sparse to draw firm conclusions. It is London data, after all!


***Figures 4-6:** Linear Mixed effect Model charts to show how pace, distance, and average heart rate change with start temperature when each runner is allowed their own baseline (random intercept), so the slope is a within-person effect rather than “slow people run on hot days.” Pace creeps up and distance falls as it warms (both significant) while HR stays essentially flat, meaning runners slow down and cut distance rather than hold speed with a higher heart rate.*


## What does not change: heart rate at effort


When I pooled sessions into narrow pace bands, heart rate in cool and hot conditions is indistinguishable (p > 0.85). Within-person pace-band comparisons show no significant increase in HR with heat. After removing pace and distance effects from heart rate, residuals show no meaningful elevation in warmer bins. Hot sessions do not cluster in a "high HR plus slow pace" corner. Although hard efforts are under-represented on hot days.


Mixed-effects models agree: HR declines by 0.064 bpm per °C, but this is not significant (p = 0.09) and is tiny. With pace and distance controlled, the temperature coefficient on HR is effectively zero (p = 0.75). Runners do not seem to be maintaining speed and paying with a higher heart rate. They ease off, and their hearts reflect the lower intensity. Cardiac cost, defined as heart rate divided by speed, is likewise unchanged across cool and hot conditions.


## The real finding: Self-regulation


I think this is a genuinely interesting finding. The analyses describe behavioral change to thermoregulate rather than increase cardiovascular cost. Runners in London's spring climate appear to anticipate thermal discomfort and respond by running shorter, slower runs.


The LLM tells us that we run ~0.009 min/km slower (~0.5 sec/km) for each extra °C, and ~67 m shorter per run for each extra °C at session start. The difference between a 25°C and a 5°C run would amount to 1.3 km shorter and running 10sec/km slower.


Per-runner plots and within-user correlations reinforce this. Most individuals show a slight positive association between temperature and pace, but individual slopes are noisy and rarely significant on their own. The population-level pattern is a gentle average of many small, self-directed adjustments rather than a uniform physiological collapse. It’s the heat that slows you down, not your heart.


#### References


Cheuvront, S.N., Kenefick, R.W., Montain, S.J. and Sawka, M.N. (2010) ‘Mechanisms of aerobic performance impairment with heat stress and dehydration’, *Journal of Applied Physiology* , 109(6), pp. 1989–1995.[https://doi.org/10.1152/japplphysiol.00367.2010](https://doi.org/10.1152/japplphysiol.00367.2010)


Corbett, J., White, D.K., Barwood, M.J., Wagstaff, C.R.D., Tipton, M.J., McMorris, T. and Costello, J.T. (2018) ‘The effect of head-to-head competition on behavioural thermoregulation, thermophysiological strain and performance during exercise in the heat’, *Sports Medicine* , 48, pp. 1269–1279.[https://doi.org/10.1007/s40279-017-0816-x](https://doi.org/10.1007/s40279-017-0816-x)


El Helou, N., Tafflet, M., Berthelot, G., Tolaini, J., Marc, A., Guillaume, M., Hausswirth, C. and Toussaint, J.F. (2012) ‘Impact of environmental parameters on marathon running performance’, *PLOS ONE* , 7(5), e37407.[https://doi.org/10.1371/journal.pone.0037407](https://doi.org/10.1371/journal.pone.0037407)


Ely, M.R., Cheuvront, S.N., Roberts, W.O. and Montain, S.J. (2007) ‘Impact of weather on marathon-running performance’, *Medicine & Science in Sports & Exercise* , 39(3), pp. 487–493.[https://pubmed.ncbi.nlm.nih.gov/17473775/](https://pubmed.ncbi.nlm.nih.gov/17473775/)


Ely, M.R., Martin, D.E., Cheuvront, S.N. and Montain, S.J. (2008) ‘Effect of ambient temperature on marathon pacing is dependent on runner ability’, *Medicine & Science in Sports & Exercise* , 40(9), pp. 1675–1680.[https://pubmed.ncbi.nlm.nih.gov/18685522/](https://pubmed.ncbi.nlm.nih.gov/18685522/)


Rodrigues Júnior, J.F.C., Mendes, T.T., Gomes, P.F., Silami-Garcia, E., Amorim, F.T., Sevilio Junior, M.N.O., Rossi, F.E. and Wanner, S.P. (2023) ‘Reduced running performance and greater perceived exertion, but similar post-exercise neuromuscular fatigue in tropical natives subjected to a 10 km self-paced run in a hot compared to a temperate environment’, *PLOS ONE* , 18(8), e0290081.[https://doi.org/10.1371/journal.pone.0290081](https://doi.org/10.1371/journal.pone.0290081)


Tucker, R., Marle, T., Lambert, E.V. and Noakes, T.D. (2006) ‘The rate of heat storage mediates an anticipatory reduction in exercise intensity during cycling at a fixed rating of perceived exertion’, *The Journal of Physiology* , 574(3), pp. 905–915.[https://doi.org/10.1113/jphysiol.2005.101733](https://doi.org/10.1113/jphysiol.2005.101733)


Vihma, T. (2010) ‘Effects of weather on the performance of marathon runners’, *International Journal of Biometeorology* , 54, pp. 297–306.[https://doi.org/10.1007/s00484-009-0280-x](https://doi.org/10.1007/s00484-009-0280-x)
