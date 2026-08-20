---
schema_version: "1.0.0"
document_id: "797f63e1923cb5861245640225481725b7c5cfbe996523697977affd7ab4a4e6"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/london-marathon-data"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-26T02:41:58.986769+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:2bba088cba4f79df70877b33a62f72a79d94ffe9e81b335255ea2bb136fe0bc9"
---

# The Data Behind the London Marathon | Terra

## Faster runners run with higher heart rates


Across the cohort, finish time and average heart rate are negatively correlated:


**Finish band** **Median average HR**


Sub-3:00 167 bpm


3:00–3:30 167


3:30–4:00 165


4:00–4:30 164


4:30–5:00 161


5:00–5:30 155


5:30+ 154 bpm


Sub-3 runners average around 167 bpm for the entire race. Six-hour finishers average around 154. That's a 13-bpm gap, and the direction is the opposite to what you may assume. You might think the faster runners are the fittest and operating well within themselves while the strugglers redline.


Two things drive it. First, faster runners genuinely are working harder in absolute terms. Their fitness allows then to push closer to their maximum for the duration of the race. They're closer to lactate threshold for 26 miles, which sits at higher absolute HR than easy aerobic effort. It’s thought that the very top runners can operate within a few % of this threshold for the whole 26.2 miles. Second, slower runners take walk breaks at aid stations, often more than they realize, and every minute spent walking drags the average down.


I'd love to look at this in %HRmax terms rather than absolute bpm, which would normalize across age and sex differences and probably tell a cleaner story but the user max-HR field was empty in this export. This is probably one for our next dataset.


### Pacing discipline shows up clearly in the data


We don't have splits in this export, so we can't compute classic positive/negative split analysis. But there's a back-door proxy: the ratio of max_speed to avg_speed over the race. A well-paced runner has a low ratio — their fastest moment isn't far above their average. A blow-up has a high ratio.


**Finish band** **Median max/avg speed ratio**


Sub-3 1.26


3–4 hours 1.39


4–5 hours 1.41


5h+ 1.57


Sub-3 runners ran within 26% of their average peak. The slowest band hit pace bursts 57% above their average — start-stop-start-stop pacing instead of holding a steady effort. It's exactly what you'd expect: discipline is the thing that separates a 3-hour runner from a 5-hour runner more than fitness alone.


### A device-level oddity (with a big caveat)


The cohort split roughly 75% Garmin, 21% Coros, with a handful of Polars and a few unknowns. When you compare the two big device populations, most numbers agree — average HR (164 vs 163, statistically indistinguishable), max HR (182 vs 181, indistinguishable), GPS distance (42,734 m vs 42,703 m, indistinguishable).


***Figure 4:** Distributions of the device-reported Calorie expenditure. Same race, same data, two different stories from the watch on your wrist. Coros runs about 332 kcal hotter at the median (a 12% gap) and has a markedly heavier right tail, with some Coros watches reporting over 5,000 kcal for a marathon, a useful reminder that calorie numbers from a wearable are a model output, not a measurement*


The chart tells the headline: Garmin runners are tightly clustered around 2,956 kcal, while the Coros distribution is shifted right (median 3,288 kcal) and has a long tail stretching past 5,000 kcal. A 12% gap at the median, and a much heavier upper tail. The obvious question is whether the high-burning Coros runners are working harder, or simply different.


They're not working harder, but actually slower.


***Figure 5:** Plot of calories expended per kilometer for different activity times on different devices. Each line is the median kcal-per-kilometer reported by that brand's watches across runners in each finish-time band, with the IQR shaded and a dotted line marking the textbook physiological estimate (~1 kcal per kg per km, roughly 70 kcal/km for a 70 kg runner).*


This is the chart that I think is genuinely interesting. On the y-axis, kcal per kilometer. On the x-axis, the finish-time band. The dotted line at 70 kcal/km is the textbook physiological estimate for a 70 kg runner: roughly 1 kcal per kg per km, largely independent of speed. That's what the literature says marathon energetics should look like.


Garmin's line sits almost exactly on it. From sub-3 to 6+ hours, Garmin reports 67 to 73 kcal/km, a small drift that's consistent with the modest economy penalty slow runners pay. The blue line is what physiology predicts.


Coros's line is doing something different. It estimates a 47% increase in reported energy cost per kilometer, which physiology doesn't really support. Running a kilometer slowly costs you a bit more than running it quickly, but not 47% more.


What seems to be happening is that the Coros algorithm is heavily weighting time on feet. Slow runners are out there for five or six hours instead of three, with elevated heart rates and frequent walking sections, and the Coros model keeps accumulating calories through that extra time at a rate that's hard to justify mechanically.


A caveat I have to flag: we still don't have user weight, sex or age in this export. So in principle the Coros cohort could be heavier on average than the Garmin cohort, which would mechanically produce more calories. But a uniform demographic offset would shift the Coros line up by a constant amount, not bend it upward with finish time. It looks more like an algorithmic choice than a population difference.


This is, I think, a genuinely useful test signal for wearable calorie models: kcal/km should be roughly flat across finish-time bands on the same course on the same day. It's a self-contained plausibility check that doesn't need any reference lab data. The practical takeaway: calories from a watch are a model output, not a measurement, and the slower you run, the further from physiology that model can drift.


## What the marathon revealed


Sawe's 1:59:30 will be the headline number for years. But it's worth zooming out for a moment. Bannister's 4-minute mile mattered not because it changed what was physiologically possible; within a few months of Bannister's run, John Landy ran 3:58, and within a decade, dozens of athletes had broken four. The barrier was psychological. Bannister proved it could be done; everyone else then realized they could too.


The two-hour marathon barrier had the same shape. We knew, abstractly, it might be possible. Kipchoge's 2019 Vienna run hinted at it. Kiptum's 2:00:35 sat close.


What I find most fascinating isn't that the record fell. It's that it fell convincingly. The asymptote turned out to be a step we could walk through. This is another principle I find interesting in endurance sport; progress is not linear, but happens in steps.


The 571 runners in this dataset were sharing the streets of London with that moment. Their watches were recording the same air, the same pavement, the same cool (the conditions definitely helped compared to hitter other years) spring morning.


Most of them recorded 1.25% further than the course says they did, with their hearts beating around 165 bpm, and each of them has their own story about where the corral started, where the race finished, and where the wall came up.


A good day for the marathon. A great day for the sport. And a quietly fascinating dataset to sit with the morning after.


**References:**


1. Joyner MJ. Modeling: optimal marathon performance on the basis of physiological factors. J Appl Physiol (1985). 1991;70(2):683–687.[https://journals.physiology.org/doi/abs/10.1152/jappl.1991.70.2.683](https://journals.physiology.org/doi/abs/10.1152/jappl.1991.70.2.683)
2. London Marathon Events. London Marathon Results.[https://www.londonmarathonevents.co.uk/london-marathon/results](https://www.londonmarathonevents.co.uk/london-marathon/results)
3. Olympics.com. London Marathon 2026: All results, times and standings — full list. April 26, 2026.[https://www.olympics.com/en/news/london-marathon-2026-all-results-times-and-standings-full-list](https://www.olympics.com/en/news/london-marathon-2026-all-results-times-and-standings-full-list)
4. The Guardian. London Marathon hails greatest day as Sawe breaks two hours and records tumble. April 26, 2026.[https://www.theguardian.com/sport/2026/apr/26/london-marathon-hails-greatest-day-as-sawe-breaks-two-hours-and-records-tumble](https://www.theguardian.com/sport/2026/apr/26/london-marathon-hails-greatest-day-as-sawe-breaks-two-hours-and-records-tumble)
