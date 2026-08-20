---
schema_version: "1.0.0"
document_id: "fba4b11e204928daeffd787353f19d259ab5f4b5f3dc097e4c25298c07a4b7c0"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/how-to-handle-missing-wearable-data"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T18:14:50.625507+00:00"
fetched_at: "2026-08-18T18:14:51.283616+00:00"
content_hash: "sha256:b5994413b838c01fcade9615ca05d5b844648fff6ac5c94953b9313f5dfd74d3"
---

# Terra Smart Fill: Solving Missing Health Data | Terra

Wearable data is a data engineering nightmare. Charge on device, position on body, software settings or cross-API technicalities leads to data being partly or even completely missing. Naturally, this affects every bit of downstream analysis or presentation using this data.


But data doesn’t exist in isolation. Certain values can be derived from others, while some other values almost never move, or move in entirely predictable ways from day-to-day. This led us to ask the question; to what extent can we predict missing health information from information that already exists?


At Terra, we’ve noticed that machine learning techniques do extremely well when trying to uncover patterns within high-dimensional data, exactly the kind of data health devices report. How does HR relate to HRV, and how would that relate to the breathing rate at night? ML algorithms do extremely well at this kind of task.


So we decided to build it. A tabular model that fills in health information that a wearable never gave at all. We considered using tabular foundation models but felt our data had very specific tendencies that we wanted to build around.


Much of our inspiration came from a 2020 NeurIPS paper titled *'*


[Handling Missing Data with Graph Representation Learning](https://arxiv.org/abs/2010.16418) *'*


that details a framework where tabular data is modeled as a graph, column identities are nodes and the values they hold are edges. Then, a missing value simply becomes an edge regression task.


A couple tweaks to this adding in personalization as well as a method to accommodate seen and unseen devices, and you end up with a model that works fairly well.


How well, you ask? Well. Against the strongest simple alternative, which is assuming you are at your own average, error drops 9% on overnight HRV, 18% on daily heart rate and 28% on workout heart rate. Against a properly tuned classical setup on the same data, 5.91 against 7.08 ms.


It also holds up on devices it was never trained on: drop a whole device family out of training and error rises by at most 1.2 ms of HRV, still matching or beating that person's own average, which never lost access to the device. A new integration does not mean waiting for a device-inclusive retrain.


To check how important personalization is to this, we also looked at how error drops as amount of data increases—which showed us that a simple average’s error is somewhat U-shaped, whereas SmartFill error continues to decrease with increased datapoints. Predictions based on no history remained at a constant error.


We also checked whether these imputations helped our own ML models’ abilities to predict certain conditions and found an increased accuracy across conditions like sleep-apnea as well as improved representation qualities in more general-purpose models. One of the failures we saw while doing this was that of conditions related to menstruation; it turns out that predicting temperate offsets, the primary signal for menstruation, from surrounding information is a pretty difficult task.


We quantify the uncertainty in every prediction, so the accuracy of anything you choose to use is known rather than assumed. Each filled value comes back with a range, and we report a confidence tier alongside it: high, medium or low. The tier comes from how wide that value's own range turned out to be, compared to the ranges we typically produce for that device. The narrowest third are high confidence, the middle third medium, the widest third low. Values we can derive exactly from others are always high, and a device whose accuracy we have not been able to confirm never reports as high.


None of this makes a filled value a measurement. It is an estimate, it says so, and it comes with a range and a rating so you can decide per value whether to use it, show it, or ignore it. That distinction matters more to us than the error numbers: a model that quietly guesses is worse than a gap, because a gap is at least honest about itself.


What we would rather not do is pretend missingness is an edge case. It is the normal condition of wearable data, and every product built on it is already making an implicit decision about how to handle the holes, usually by dropping the row or carrying the last value forward. SmartFill is our attempt to make that decision explicit, measured, and yours.


SmartFill is live in the Terra dashboard. If you are working around missing data today, we would like to hear which gaps hurt most, because that is what decides where this goes next.
