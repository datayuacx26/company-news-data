---
schema_version: "1.0.0"
document_id: "9451498d8c5418f1540eb57292b3278e526fb0b839f2a12dbe4dcfaf806a96d5"
company_key: "doubleverify-holdings-inc-common-stock"
company: "DoubleVerify Holdings Inc."
source_id: "doubleverify-holdings-inc-common-stock-news-import-e3fd1e38d821"
canonical_url: "https://doubleverify.com/blog/web/prove/hot-take-in-incrementality-testing-statistical-significance-doesnt-matter.-hear-us-out"
published_at: "2026-08-10T10:00:00+00:00"
first_seen_at: "2026-08-10T18:52:17.195539+00:00"
fetched_at: "2026-08-10T18:52:17.895964+00:00"
content_hash: "sha256:a27a95cdeaad66012f120a3957b1ecf3602dfa75d59a032c019a59e510d21f78"
---

# Hot Take: In Incrementality Testing, Statistical Significance Doesn’t Matter. (Hear Us Out.)

**By Will Burghes, Head of Professional Services, DV Rockerbox™**


You ran an incrementality test. The results are in. Now, how do you interpret them?


Most marketers look for one thing: the plausible value, or p-value. Did the test reach statistical significance? If yes, the results are real and the channel works. If no, they assume the results are random and the test was a waste.


It’s instinctive to look for such an easy answer. But by doing so, you throw away most of the information the test actually produced, which can lead you to waste money on ineffective channels or prematurely pull the plug on promising ones. To avoid these risks, you need to key in on what’s known as the


**confidence interval** . Once you know how to read it, the results become more actionable and can lead to greater success.


## What Is the Confidence Interval, and Why Should You Rely on It?


When you run an incrementality test, the relevant output isn't just the single p-value. It's the


**range of plausible values** that gives you the true incremental effect. This range, called the


**confidence interval,** accounts for the noise in your data, including timing, audience behavior and other outside factors. It tells you: Given those variables and what we observed, the true lift is most likely somewhere between X and Y.


Understanding that range and interpreting it correctly is critical to deciding whether a channel is worth the investment.


## How to Interpret the Confidence Interval


Statistical significance is just one property of the confidence interval. If the entire confidence interval is above zero, the result is statistically significant; you can be confident the media is driving


*some* lift. If the interval includes zero, the result is not significant; the data is consistent with the possibility that the lift is zero (though that possibility may be very small).


But the interval tells you much more than that binary.


Consider two results for an apparel retailer trying to understand the incrementality of brand search and paid social. Neither result is statistically significant. But they tell you completely different things. And they should lead to completely different decisions.


**Brand Search:** Estimated iROAS of 0.1, with a 90% confidence interval of -0.3 to 0.5. The range is tight and clustered around zero. What it tells you: Even in the best case, this channel is barely moving the needle.


**Paid Social:** Estimated iROAS of 1.8, with a 90% confidence interval of -0.2 to 3.8. The point estimate is promising, but the range is wide. What it tells you: There may be a real effect here, but you don't have enough data to be sure.


## Homing in on the Most Likely Value


A confidence interval isn't a flat range where every value is equally likely. It's shaped like a bell curve. The


**point estimate** at the center is the single most probable value for the true effect, and the probability tapers off as you move toward the edges.


Think of it this way: If your test produced an iROAS point estimate of 1.5 with a 90% confidence interval of 0.3 to 2.7, the most likely answer is 1.5. An iROAS of 0.3 or 2.7 is possible, but much less probable. The edges of the interval are the boundaries of what's plausible, not equally good guesses.


This matters because it can be tempting to point to the upper bound as proof that the channel works, or to the lower bound as proof it doesn't. What the data is telling you: It is more likely that the true lift is closer to the center than to the ends.


Even when a confidence interval technically includes zero, a point estimate well above zero is still informative. Take this example of a DTC ecommerce brand testing the impact of their display spend.


The test result is saying: "We think the most likely answer is positive, but we can't fully rule out zero."


That's a different message than a point estimate at zero, and it should lead to a different decision. Most importantly, if the confidence interval includes zero (and therefore the test did not reach statistical significance), that doesn’t mean that the test hasn’t provided any insight.


## What Different Intervals Can Tell You


Here’s how to read different testing outcomes:


**The interval is entirely above zero.** The media is delivering measurable lift. Statistical significance tells you the effect is real. Material significance tells you whether it matters. Ask yourself, is the iROAS or iCPA good enough to justify spending at the level you were for the test?


**The interval is tight and centered near zero.** The media as deployed is not delivering meaningful lift. This doesn't mean the channel will never work. But at this spend level, with this creative and these contextual segments, it isn't working now. That's a finding worth acting on. Our suggestion: Reallocate the budget or fundamentally change the strategy before retesting.


**The interval is wide and includes both zero and meaningfully positive values.** There may be a real effect here, but you might need more data to find it. Options would include retesting with a larger budget, bigger test cell or longer window; or turning to multi-mix modeling (MMM) or multi-touch attribution (MTA) for another data point.


## The Bottom Line


Once you stop viewing incrementality testing results as a pass/fail verdict and see them as a guide, every data point suggests insights for better decisions and a clearer course of action.


The only way to waste a result is to ignore it.


**Next Steps**


[Contact](https://doubleverify.com/lp/contact) a DV representative to learn more about DV Rockerbox.
