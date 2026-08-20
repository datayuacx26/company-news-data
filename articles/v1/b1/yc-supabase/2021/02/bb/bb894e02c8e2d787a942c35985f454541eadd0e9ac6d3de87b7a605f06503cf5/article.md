---
schema_version: "1.0.0"
document_id: "bb894e02c8e2d787a942c35985f454541eadd0e9ac6d3de87b7a605f06503cf5"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/case-study-roboflow"
published_at: "2021-02-09T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:73255864462917c72481a06933eb07f73bd28cefce844d05a50f8c394da05925"
---

# Roboflow.com choose Supabase to power Paint.wtf leaderboard

Brad Dwyer is the founder of[Roboflow](https://roboflow.com/?ref=supabase) , a startup helping developers build computer vision into their applications. Their solution allows developers, with zero computer vision experience, to go from images to a trained computer vision model in minutes.


Learn how Brad and Roboflow used Supabase to launch[Paint.wtf](https://paint.wtf/) , a product which survived traffic generated from the front page of Hacker News, Reddit, and Product Hunt.


## From idea to a launch in a weekend#


Brad and a friend ([Erik Dunteman](https://twitter.com/erikdoingthings) ) wanted to experiment with OpenAI's new[CLIP](https://openai.com/blog/clip/) model through a side project they could build in a weekend. CLIP classifies a wide range of images by flipping image classification into a text similarity task. Current image classifiers are limited because they are trained on a fixed number of categories. In contrast, CLIP learns from the raw text describing the images meaning the classifier isn't limited by labels and supervised learning. The team recognised that CLIP opens up a vast range of use cases that have been difficult previously due to the time required to collect images and train the model.


Roboflow settled on a straightforward concept: they prompt users to draw an image which is then fed into CLIP. The AI then judges how close the drawing is to the given prompt and assigns it a score. Users' performance is tracked on a leaderboard for each prompt and users can see how well they performed against their peers according to the model.


## Leaderboards that Count#


Brad and the team needed a leaderboard to make this idea work. While their first intuition was to use Firebase, it lacks the built-in functionality for counting the number of documents in a collection - a critical function for implementing their leaderboard design.


You might export data to BigQuery, or implement an increment function on collection change, however PostgreSQL has great built in support for counting and Brad felt like this would be the perfect opportunity to test out Supabase and get the functionality he needed for the leaderboards to work reliably and without implementing add-ons.


## One weekend to break the internet#


Brad and the team built the product overnight and launched it on Hacker News. Paint.wtf went on to make it to Hacker News and Reddit's first page, and getting traction on Product Hunt. As a result, they had to handle serious user volumes; at its peak, users were submitting over 2 new drawings every second.


Over 100K users submitted drawings in a 24 hour span. At this point, Brad knew he had picked the right setup for his leaderboard as even with this massive spike in usage it continued to perform reliably so his users could get accurate and up to date rankings for their submissions.


Paint.Wtf has continued to get sustained coverage in the media and has continued to pick up new and unexpected use cases. For example, remote teams are using Paint.wtf as part of their daily ice breaker activities during COVID to keep up team social cohesion.


## Ship fast, and carry on scaling#


Thanks to Supabase, Roboflow could launch quickly and keep scaling despite the huge user volumes their innovative project generated overnight.
