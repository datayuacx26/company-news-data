---
schema_version: "1.0.0"
document_id: "29656cfec59320d1c64c3196779cfd2a1b95aa5a3a0b97be37ec093f1ce31228"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/best-time-to-workout-for-sleep"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-26T02:41:58.986769+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:02bde4922bf995cfc8349007243c420d4c3e3813d98b83610e7fa17ca5bfe595"
---

# Your Evening Workout Is Costing You Sleep | Terra

A question we’ve tried to answer in the past is whether or not how much you exercise during the day affects how you sleep. We had pretty interesting results when we did this, notably that higher number of burned calories was linked with lower sleep heart rates, slightly higher HRV, and tiny changes in sleep duration or stages.


A common limiting factor of many large-scale investigations we run at *Terra Research* is that of confounding factors. Having access to data that belongs to real people living real lives is messy and doesn’t allow much space for control.


These confounders are an artefact of this reality. Yet, as researchers, we recognize the importance of investigating these confounding factors. And so this is what we will do here, instead of just looking at how calorie burn affects sleep, this blog will attempt to look into how the timing of activity affects sleep.


To do this, we ran activity and sleep data from the same day through a model with fixed effects and terms for activity time, the activity-sleep gap, and the interaction between these terms. We looked at how the data we collected from users sleeps was affected by when they decided to exercise. This work only focused on cardiovascular exercises, specifically, biking (outdoor & stationary), walking, running (outdoor & treadmill), and hiking.


## Earlier Workouts Lead to Better Recovery


There were a couple effects that were very clear (p < 0.05). The first had to do with time spent awake in bed. Both the linear model and the plot show that the later you exercise, the less time you spend awake in bed.


***Figure 1:** Time spent awake in bed based on the gap between activity time and bedtime. This is a small effect which is apparent by the error presented.*


However, this is not an indicator that you sleep *better.* Despite taking longer to fall asleep, we found a increased number of REM events for activities done much earlier during the day.


***Figure 2:** REM event counting based on the gap between activity time and bedtime. Once again, this effect has a small magnitude. Gap distributions between plots can differ based on how much missing data was present for each sleep column.*


Continuing on this theme that earlier activity was better for your sleep, we found that heart rate was lower when activity was done earlier during the day.


***Figure 3:** Sleep heart rate based on the gap between activity time and bedtime. Interestingly, this plot resembles a mirror image of the previous ones.*


Since sleep is essential for recovery, we looked at HRV, which is one the best indicators for recovery.


***Figure 4:** RMSSD, a measure of your HRV or heart rate variability drops when you have a late workout.*


With the drop in REM events as well as HRV, we once again reached the same result. Your recovery was better when you worked out earlier in the day.
