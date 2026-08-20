---
schema_version: "1.0.0"
document_id: "12667db1fe5f588fdc6ddf6773b001de49dacda271107e28f171cd34a72805bf"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/does-weather-affect-exercise"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-26T02:41:58.986769+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:f5910e137c716822b4cfb398731aa081adfae2a75460130a0236b44e91b601a5"
---

# Does Hot Weather Change How Londoners Exercise? | Terra

## The Results


For temperature, each extra 1°C on the day’s maximum was linked to slightly lower odds of going out: walking showed the strongest effect (odds ratio 0.98 per °C, highly significant), running a similar but smaller significant effect (0.98, *p* ≈ 0.0002), while cycling showed no clear daily temperature relationship (OR 1.00, not significant). Results were similar using the day’s minimum temperature instead of the max.


Quick notes on odds ratio: Any number below 1 means that the thing is less likely to happen, and anything above 1 means it’s more likely to happen. Here, 0.98 means a person is about 2% less likely to go for a walk for each 1-degree increase in the maximum temperature.


Of course, looking at the impact of the max temp is a fairly blunt approach, so below you can see the impact on activity across various hourly temperature bins, as shown in charts 2-4.


I treated a day as rainy if precipitation was ≥ 1 mm, and only cycling had a clear, significant result: odds of a ride day were about 14% lower on rainy days (OR 0.86, *p* ≈ 0.02). Amazingly, running did not differ significantly on rainy days (OR 1.03, *p* ≈ 0.6), and walking showed only a weak, non-significant dip (OR 1.04, *p* ≈ 0.09). Having been a cyclist all my life, I’m glad to report that the evidence here backs up my own experience. Cycling in the rain is not much fun.


In short, heat is associated with fewer walk and run days; rain is associated with fewer bike days, with walking showing the largest temperature response in this sample.


***Figure 1:** Does a hot day change whether people go out? Note Walking relates to the left hand axis, running and cycling to the right. The trends are messy, but all three activities see flat activity and a small peak in the low-mid twenties, and then decline.*


Walking has the clearest temperature story. When the day’s maximum temperature fell within the 12–16°C range, about 24% of user–days included a walk. When the max was 32–36°C, that fell to about 15%, a large drop in how often people walked at all. In the regression (with day-of-week controls), each additional degree above the day’s maximum temperature was associated with lower odds of a walk day; the effect was strong and statistically significant.


Running showed a gentler pattern: participation peaked around 20–24°C max (~3.5% of user–days) and fell on the hottest days (~2% at 32–36°C). The temperature link was significant but small. Heat matters, but running is relatively infrequent in this data, so the day-to-day shift is modest in percentage points.


Cycling was much flatter across temperature bands in the chart, and the daily temperature regression was not significant. This is interesting because I’d expect cycling to be the most temperature-dependent, and I wonder if this is because the sample is heavily dominated by commuters who ride in most weather conditions.


Comparing activity to max and min daily temperature is a relatively “blunt” analysis. What about humidity, what about sun strength, etc. Next I decided to investigate if exercise timings moved during hotter weather.


***Figure 2:** When do people go running, and does timing shift with temperature? Most runs still cluster in the morning and late afternoon. On days when the daily max was ≥ 24°C, the peak hour for starting a run moved earlier (around 6:00 vs 8:00 on cooler days), and activity in the 7–8 a.m. window was lower on hot days. That fits people avoiding mid-morning heat, even when they still run.* ***Figure 3:** When do people go cycling, and does timing shift with temperature? Ride starts still peak around 7 a.m. on both cool and hot days. Hourly lines cross by temperature band without a simple “only cycle when it’s cool” picture. It’s consistent with Chart 1, where daily heat did not strongly predict a ride day. Again, I wonder if we are seeing a strong “commuter” effect here. A future analysis should do a detailed day of the week analysis here.* ***Figure 4:** When do people go walking, and does timing shift with temperature? Walking holds its morning and afternoon peaks across temperatures, but hot days flatten the activity throughout the day.*


Walking dominates volume and keeps a strong morning and afternoon pattern. Hourly temperature lines are closer together than the daily chart suggests; the big heat effect shows up more in whether people walk that day than in a dramatic reshaping of the activity by the hour.


This is an interesting finding, but it probably points to people using walking as a utility form of transport.


#### What did we learn?


**Finding** **Strength**


Hotter days → fewer walk days Strong


Hotter days → fewer run days Significant but smaller effect


Hotter days → cycle days Not significant


Rain → fewer cycle days Significant


Rain → run / walk Not significant (walk borderline)


Runners start earlier on hot days Descriptive (hourly), plausible/probable!


## Conclusion


For this cohort, weather is most evident in whether people go for an outdoor walk on a given day, with a meaningful drop on the hottest days. Running follows a similar direction but with weaker intensity. Cycling is less about temperature in May and more about rain reducing the chance of a ride day, unsurprisingly!


Hourly data confirm what we’d expect for runners: on hot days, they not only skip more often at certain times but, when they do go, they tend to start earlier. Although I can’t draw fixed conclusions from this analysis we can say there are strong signals to suggest that heat and rain are linked to participation in London


What’s next? Length and intensity?


This analysis only asks if a session started. It does not yet ask how much people did on days they went out; distance, duration, heart rate, pace, or effort. Next, I am going to look at how the weather affects the activities people engage in in London, and maybe even see if there is a detectable physiological impact.


### References


- García-Witulski C, Rabassa M, Melo O, Sarmiento JH. Effects of climate change on physical inactivity: a panel data study across 156 countries from 2000 to 2022. *The Lancet Global Health* . 2026;14(4):e500-e511. doi:10.1016/S2214-109X(25)00472-3[Full text](https://www.thelancet.com/journals/langlo/article/PIIS2214-109X(25)00472-3/fulltext)
- Wagner AL, Keusch F, Yan T, Clarke PJ. The impact of weather on summer and winter exercise behaviours. *Journal of Sport and Health Science* . 2019;8(1):39-45. doi:10.1016/j.jshs.2016.07.007[Full text / PubMed](https://pubmed.ncbi.nlm.nih.gov/30719382/)
- Ho JY, Lam HYC, Huang Z, et al. Factors affecting outdoor physical activity in extreme temperatures in a sub-tropical Chinese urban population: an exploratory telephone survey. *BMC Public Health* . 2023;23:123. doi:10.1186/s12889-022-14788-0[Full text](https://link.springer.com/article/10.1186/s12889-022-14788-0)
- Périard JD, Eijsvogels TMH, Daanen HAM. Exercise under heat stress: thermoregulation, hydration, performance implications, and mitigation strategies. *Physiological Reviews* . 2021;101(4):1873-1979. doi:10.1152/physrev.00038.2020[Full text / PubMed](https://pubmed.ncbi.nlm.nih.gov/33829868/)
- Tucker P, Gilliland J. The effect of season and weather on physical activity: a systematic review. *Public Health* . 2007;121(12):909-922. doi:10.1016/j.puhe.2007.03.009[Full text / PubMed](https://pubmed.ncbi.nlm.nih.gov/17920646/)
