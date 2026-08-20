---
schema_version: "1.0.0"
document_id: "8bcadfade1fd7a4889f7b6a32c081f6a26465e90abfb95ab80d2858756e11f1c"
company_key: "airbnb-inc-class-a-common-stock"
company: "Airbnb Inc."
source_id: "airbnb-inc-class-a-common-stock-rss-d1a7958eece3"
canonical_url: "https://medium.com/airbnb-engineering/how-we-knew-covid-was-over-and-what-our-models-had-to-unlearn-c606b9bdb0ab"
published_at: "2026-08-19T17:01:03+00:00"
first_seen_at: "2026-08-19T17:16:01.399741+00:00"
fetched_at: "2026-08-19T17:16:03.369600+00:00"
content_hash: "sha256:860ba053736cf6b485701276a3e348f3ad2fff53e4bcb8bbe60d648b3c0467be"
---

# How we knew COVID was over (and what our models had to unlearn)

Data


Data Modeling


Data Science


Technology


Machine Learning


# **How we knew COVID was over (and what our models had to unlearn)**


[Harrison Katz](https://medium.com/@harrison.katz?source=post_page---byline--c606b9bdb0ab---------------------------------------)


12 min read


·


Just now


--


*When we retrain, when we rebuild, and when we leave a model alone.*


Press enter or click to view image in full size


**By:**[Harrison Katz](https://harrisonekatz.com/)


## A forecast that carries weight


The Forecasting Data Science team at Airbnb produces many of the forecasts the rest of the company plans around: demand, bookings, cancellations, and a range of finer cuts by market and segment, refreshed continuously across thousands of markets. The targets differ, and the models differ, but they have one thing in common: Other teams build on top of them.


This means a forecast that is casually wrong is not a clean miss, as it might be in an academic setting. That’s because a small bias does not stay small once a lot of decisions are riding on it. So when one of those forecasts starts to drift, what to do about it is not really a methods question. It is a risk question, and an easy one to get wrong, which we have from time to time.


One of these forecasts had been missing, compared to what actually happened after the forecast was released, in the same direction for a couple of quarters. This bias persisted after several routine refreshes. The usual solution would be to fully retrain the model: pull in the recent data, refit the model again, and ship. But we wanted to understand the source of the bias, rather than simply hoping an update would eliminate it.


If you’re interested in other posts on this topic, you can learn more about[how COVID impacted Airbnb’s financial models](https://medium.com/airbnb-engineering/what-covid-did-to-our-forecasting-models-and-what-we-built-to-handle-the-next-shock-ccbf0e1f7fa9) or[how we dealt with disruption to our models during the pandemic](https://medium.com/airbnb-engineering/when-history-fails-you-borrow-from-geography-915a72b91b5c) . This post is about the discipline that came out of both: how we now decide whether a struggling forecast needs new data, a new model, or no changes at all.


## One word, three decisions


The easy mistake is treating the choice to “retrain” a model as a single action. It is three separate actions — refitting, respecifying, or holding — and none of them is particularly similar to the others.


Refitting is the cheaper option. Same model, same structure, same features, updated with newer data. This is what most people mean by “retrain.” For the ordinary drift that builds up in a model as the world moves on, it’s usually the right choice.


Saying that this is the cheaper option is not the same as saying that it’s cheap. A refit still has to be validated and shipped. The refitting process can degrade a model that was actually fine if the recent data happens to be unusual. And, given the importance of the production forecast, every refresh is a small risk you are choosing to take. It is the least expensive of the three options, but not a free one. None of this is an argument against refitting. On most cycles it is the right move, and if we had to run one of the three blind, it would be this one. The claim is narrower. A refit is a decision with a price, and pricing it is what lets you notice the cycles where a different option was worth more.


Respecifying is a different animal. You change the model itself: add a feature, drop one, change the structure, the priors, the likelihood. On the production forecast, which so many of our teams plan around, this is a real commitment; you are replacing something you understand, and have watched the behavior of for years, with something partly new that you have not. It is also where almost all the actual improvement lives. Refitting keeps a good model current. Respecifying is how a model that is in some way wrong in its operations is made right.


Holding is the one nobody likes. You look at the miss, decide it does not warrant action, and leave the model alone. This takes the most nerve, because to anyone watching the forecast it looks like you are ignoring a problem. On a forecast that people are relying on, “We decided to do nothing” is a hard sentence to say out loud. It is also, more often than you would think, the correct call.


What we have learned from years of running these models is that the trouble comes from filing all three under one verb. “We should retrain” gets said when any of the three is the actual answer, and the word drags everyone toward the one that feels cheapest, because it is the one with a name.


Most of the time, though, nobody says it at all. Retraining is rarely a decision someone makes because a model is drifting. Retraining runs on a schedule, monthly or quarterly, by convention, because a standing cadence is one less thing to think about. Which means the choice among the three is hardly ever made on purpose. The cadence makes the choice in advance, every cycle, and the cadence always picks refit.


## Three ways to hold on too long


Each of the options contained in the term “retrain” — refit, respecify, or hold — has a matching failure, and underneath each sits the same mistake: a model holding onto a shock, or a surprise, after the cause of the shock or surprise is no longer relevant to forecasts. That mistake can grab a blip and treat it as the new normal; keep a crisis assumption after the crisis is over; or build the shock so far into a new model that the model can never move past the shock.


**Chasing noise.** A forecast misses for a quarter, the residuals look alarming, and someone asks whether we should update the model now rather than wait for the next scheduled run. That impulse is the failure, and it is a specific one. What triggers an off-cycle refit is always a surprise, which means the window you are rushing to absorb is the window you understand least.


A couple of years ago one of our markets ran hot for a quarter against the published forecast, because a large event landed on the calendar and pulled a wave of bookings forward. We refit early. The model took the unusual quarter as the new level, the next two forecasts came in high, and it settled only once the event aged out of the training window. Waiting for the cadence would have meant fitting that spike alongside the quarters that came after it, rather than as the last thing the model saw, which is a much weaker pull. (Figure 1)


Press enter or click to view image in full size


*Figure 1. Illustrative shape of the failure. A one-off spike sits inside the model’s normal range, so holding keeps the forecast on track, while refitting on the spike pulls the model toward it and gives back several quarters of accuracy. The same refit on an ordinary quarter would have cost nothing, which is what makes the reflex hard to see. Synthetic data.*


**Carrying ghosts.** The quieter error, and the opposite mistake. A structural assumption that was true during a shock stays switched on long after the shock has passed, hidden behind a run of refits that all look like routine maintenance. We have had one of these hide in plain sight. During the worst of the COVID disruption, cancellation timing shifted in a way it never previously had, and the model learned from it. Market behavior came back to normal well before the assumption did, and since every refit looked clean, the stale piece sat there until the forecast had leaned the same way for long enough that someone went looking, found the lean baked into the model, and respecified the model to wring out the now-faulty assumption. (Figure 2)


Press enter or click to view image in full size


*Figure 2. When the structure cannot anticipate a turn, refitting only chases it: each quarterly refit lands below the new trend, and the residuals stay one-sided, the signature of a structural problem a refit cannot fix.*


**Respec-as-panic.** The failure of overcorrecting. Something genuinely moves — a foreign exchange rate (FX) swing from a major source market, a regional disruption that reroutes demand — and the reflex is to rebuild. A fresh model, new structure, stood up under deadline.


We have caught ourselves reaching for this solution. An FX move on a major source market threw one of our forecasts off for a couple of months, and the first instinct was to stand up a new model around the new regime. We widened the priors instead, the existing model rode it out, and we avoided a move to a more fragile model, tuned in response to a shock that was already on its way out. (Figure 3)


Press enter or click to view image in full size


*Figure 3. A temporary shock clears on its own; widening the prior lets the existing model ride through it, while a full rebuild fits the shock and carries new volatility forward.*


## How we decide now


The rule we now hold to is easy to state and hard to follow. On a standing cadence, the question of whether to retrain mostly answers itself. So the question that matters is which of the three options a given miss calls for: refit, respecify, or hold? And the right answers turns on what changed.


If the process generating the data is still the one the model assumes, and the parameters have just drifted, refit. The structure is fine, it just needs current numbers.


If the process has changed in a way the model cannot represent no matter how you estimate it, respecify, because fresh data cannot help a model being asked to describe a world it has no language for. The tell is direction: a misspecified model misses the same way over and over.


A respecify can cut both ways. It can add structure the model was missing, the way borrowing across geography did after 2020, or it can take structure out, retiring an assumption the world has outgrown.


The second kind is the forgetting, and it is the one teams skip, because adding feels like progress and removing feels like giving something up. But it’s an important tool to remember, and to use, whenever needed.


And if the miss sits inside the range the model already calls normal, hold. In practice this usually means declining to refit off-cycle, on the theory that a surprise you cannot yet explain is the worst possible reason to move a model early. The cadence will get to it. Skipping a scheduled refit is the rarer and more expensive version, since stale parameters have a price of their own, and it is worth it only when you can name the thing in the window that you do not want the model to absorb.


Press enter or click to view image in full size


*Figure 4. The same decision as a quick triage. The question is never whether to act, but which of the three moves the situation calls for.*


There is a more formal version of this that we have written up separately \[[1](https://arxiv.org/abs/2604.06438) \]\[[2](https://arxiv.org/abs/2606.06670) \]. The one-line version is that updating is not free: moving a model toward new data costs something, staying put costs something, and the call is a comparison of the two, at the level of parameters and again at the level of structure.


In production, running real forecasts, the math is not what we reach for. We reach for the three questions above, in order.


## The time we got it right


The clearest case we have of making this call correctly is the one from[our second post on the Covid era](https://medium.com/airbnb-engineering/when-history-fails-you-borrow-from-geography-915a72b91b5c) , seen through this lens.


Before 2020, our destination-market forecasts leaned on a hierarchy. Markets with long, stable histories anchored the estimates, and other, “thinner” markets borrowed strength from them, on the assumption that a destination behaves like comparable destinations elsewhere. The borrowing was the structure, and for years it was true enough to be both invisible and unquestioned.


COVID broke the assumption, not the parameters. The shock was larger than anything we had previously experienced. Recovery did not arrive everywhere at once, or in the same shape. Some markets came back fast, while others stayed flat for quarters. Markets s that used to move together were suddenly on different paths. As a result, the “borrowing” of one market’s stability, by other markets that had previously tracked it, which had previously stabilized the forecasts, was now contaminating them. Markets were being pulled toward a kind of average that no longer described any of them.


The instinct was to refit our models with new data, and we tried that. It failed in a way that turned out to be the whole lesson: the estimates did not just shift, they went unstable, swinging quarter to quarter as the model tried to reconcile markets that no longer belonged in the same pool. Over the recovery window, the refit-only error ran about three times the pre-shock baseline, and it would not settle as new data came in.


That instability was the signal. A parameter problem gets noisier at the edges, but holds its shape; this was the shape itself coming apart, which is what a structural problem looks like. (Figure 5)


Press enter or click to view image in full size


*Figure 5. Why pooling broke, and why respecifying fixed it. Left: two markets that used to move together take different recovery paths, so borrowing strength across them now hurts. Right: forecast error over the recovery window; refitting stays high & unstable; changing what the model borrows across brings error back toward the pre-shock baseline.*


The fix was a respecification, which is the subject of the previous post, so in one line: we changed what the model borrows across. Instead of pooling by a fixed hierarchy, the prior borrows along geographic adjacency and shared recovery dynamics, so a market draws strength from places actually behaving like it today, rather than places it used to resemble before the crisis. On a held-out recovery window, the respecification cut error by a little over half against refitting, and brought it back to within a couple of points of the pre-shock baseline.


The point for this post is that our new framework, which gives a prominent role to holding, would have told us not to refit. That would have prevented the failed refit that generated so many errors.


The miss was directional, not noisy, so it was not a hold. Fresh data made it worse, not better, which is the signature of a problem in structure and not in parameters, which indicated that the remedy was not a refit. What was left was to respecify.


We got there by trial, and errors, that time. The reason to write the decision down is to get there without the trial the next time, on a forecast where the trial costs a quarter of accuracy that other teams feel downstream.


**Why this is hard on forecasts that carry weight**


None of this is hard to understand. It is hard to do, and it is hardest precisely on the forecasts that matter most, for three reasons that we keep running into.


Holding looks like negligence. When a forecast a lot of people watch is missing, “We are choosing not to act” is an unpopular thing to say, and the pressure is always toward visible activity. Instead, when we hold, we use the modern, Buddhist-adjacent maxim: “Don’t just do something; sit there.” Refitting, on the other hand, is visible activity. It photographs well even when it is wrong.


Respecifying is expensive, and a little frightening. Replacing the structure of a model that has produced reliable numbers for years means giving up something you understand for something you do not understand yet, and the cost of being wrong is not abstract. That fear is healthy, and it is also why teams under-respecify and let ghosts accumulate.


And the last few years trained the wrong reflex. Forecasting through COVID meant changing models under real pressure, often correctly. Coming out the other side of the pandemic, a lot of teams kept reaching for the rebuild when a refit would do, while leaving pandemic-era assumptions switched on because nobody wanted to touch a model that had survived.


The same period produced both the panic and the ghosts. The discipline now is not how to react fast. It is how to tell the three options apart again, with time to think, on models where being repeatedly wrong is expensive.


**Learning to forget**


Refitting keeps the model current. It does not keep it honest. What goes stale is not the numbers but the assumptions underneath them, and those are exactly what a refit leaves alone.


A shock makes you add structure to cope: an elevated risk, a wider band, a special case for a world that has stopped behaving. Taking that structure back down once the shock has passed, before it quietly biases everything downstream, is harder than any retrain. It rarely has a deadline. Nobody schedules it.


A good forecasting team doesn’t just learn from shocks. It learns to forget them, when needed, on purpose.


If this type of work interests you, check out some of our[related positions](https://careers.airbnb.com/) .


**Acknowledgments**


Thanks to Liz Medina, Jess Needleman, Linhan Liu, and Flavio Stanchi for building and improving the forecasting systems described here, and to Peter Coles, Yuanyuan Cui, and Adam Liss for their support of this work and its publication.


*Harrison Katz leads Finance Data Science & Strategy at Airbnb. His research focuses on Bayesian methods for compositional and hierarchical time series, Bayesian decision theory, & forecast governance.*


*All product names, logos, and brands are property of their respective owners. All company, product, and service names used in this website are for identification purposes only. Use of these names, logos, and brands does not imply endorsement.*
