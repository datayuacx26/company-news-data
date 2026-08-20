---
schema_version: "1.0.0"
document_id: "1585af2a45f7f58b35c4ddd190454bb88ff1deafee9b1b061c172dbb0e15a663"
company_key: "stitch-fix-inc-class-a-common-stock"
company: "Stitch Fix Inc."
source_id: "stitch-fix-inc-class-a-common-stock-rss-75294c3181e6"
canonical_url: "https://multithreaded.stitchfix.com/blog/2022/09/02/stylists-in-the-loop/"
published_at: "2022-09-02T09:00:00+00:00"
first_seen_at: "2026-07-20T23:19:28.351732+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:bb838cbda505bfe56799f5b4539361cff45d5165123685b825d0a92267b5114f"
---

# Experts in-the-Loop at Stitch Fix

Imagine your job is to personalize search results on an e-commerce site for returning customers, classify the presence or absence of pedestrians in street photos, or develop an app that translates languages. In all of these cases, a basic ingredient is a dataset of annotations provided by a human. For any company seeking to personalize experience for its customers, combining[human](https://en.wikipedia.org/wiki/Human-based_computation)[computation](https://www.technologyreview.com/2015/06/04/248690/the-emerging-science-of-human-computation/) with algorithmic computation is essential. This is also true for Stitch Fix.


At Stitch Fix, we recently launched Stitch Fix[Freestyle](https://newsroom.stitchfix.com/blog/launch-of-stitch-fix-freestyle/) , our direct-shopping experience, where our algorithmic recommendations are now directly shared with clients in their own personal shopping feed – a different approach from our original Fix experience, where a team of expert stylists determined what should go in the client’s Fix. Central to the Freestyle experience for clients is showing individual items as part of complete outfits, where both items and outfits are personalized based on our clients unique size, fit, style, and price preferences.


#### But, what makes a good outfit? And, how do we balance personalization to a customer with a given level of outfit quality or a particular stylistic slant?


In order to bootstrap a new product like this, to offer personalized outfit recommendations at scale to our around 4m clients, we need some high-quality training data. In particular, we need data specific to what it means to have a good client experience - or what makes a good outfit for a given client at Stitch Fix. Datasets like this aren’t exactly floating around: assembling them requires an intentional, large-scale effort. The best way to get high-quality data sets is to work with our in-house experts: our stylists.


In this post, you’ll learn more about the purpose and impact of stylist-in-the-loop projects, and the powerful impact that our stylists have in building the future of personalized shopping.


#### Bringing in our “experts in-the-loop”


Working with our expert stylists helps us improve our clients’ experience in a number of ways. In building this new Freestyle experience from scratch, we had to make decisions about how we would measure the value added for our clients. This was especially important, because if we’re showing recommendations to clients without stylists’ judgment beforehand (like in the Fix experience), how would we know if our algorithmic recommendations were good? We know we could measure indirect customer metrics like engagement and revenue, but we also recognized we could measure this by tapping into our expert stylists, and asking them to give their opinion.


To give an example of how we bring our experts in-the-loop with our algorithms, I’ll illustrate how we do this in the context of building outfits for your “Trending for You” and “[Complete Your Looks](https://newsroom.stitchfix.com/blog/introducing-the-latest-ways-we-are-making-it-even-easier-for-you-to-discover-the-things-you-love-with-shop/) ” shops in Freestyle. The first step is **Outfit QA** .


Each day, a random sample of the day’s algorithmically-generated outfits shared with clients is collected and stored. Then, our styling experts evaluate - against a developed set of criteria and using a specific labeling tool - what makes a good outfit. The evaluation criteria was developed in conjunction with members of our styling field, to ensure that we were maximizing the expertise of our Styling Team Leads and their stylistic judgment based on their own expertise and deep understanding of our clients.


#### Measuring the impact


As we reviewed the impact of our Outfit QA project, we saw three key areas of opportunity:


1. It gave us a better understanding of loss patterns, for example:


2.


It helped us understand and prioritize model improvements.


3. Importantly, it gave us training data to build a predictive model to estimate the likelihood of a given outfit having a QA problem. This is a tool we are able to use to help evaluate outfits before showing them to clients.


#### Outfit Creation at Scale


Now that we know what problem areas to improve, where should the training data come from? Our inventory is changing constantly – seasons change, tastes change – so our training data needs to stay fresh, relevant, and on-brand. We again turn to our styling experts, working in conjunction with an algorithmic process, to keep us informed. This is the second step for our outfit-building project: **Stylist-Generated Outfits** .


Our initial goal with stylist-generated outfits was to build a process for a continually refreshing dataset to feed into the outfits we show in “Shop Your Looks” and “Trending for You”. As trends in fashion change often, and our inventory changes to reflect them, we needed to build an outfit model that would easily adapt and not risk becoming stale.


We built a tool that allows our stylists to sample available inventory and create outfits. This new tool enabled us to scale our effort beyond what was originally envisioned, and bring new value to our clients by sharing outfit inspiration, or “Ways to Wear It”, for individual items in their curated Freestyle Shops. Because of the scale at which stylists were able to create outfits using this tool, we were also able to ask an unconventional question: what if we recommend stylist-generated outfits alongside our algorithmically-generated ones?


In an early test, the stylist outfits were very well-received by clients. We also tested our original hypothesis of using the stylist outfits as training data to improve the model directly. This resulted in a lift to observed Freestyle customer metrics, including revenue, as well as a 14% increase in our internal quality measure, as enabled by the Outfit QA project above.


#### What’s next?


The experimentation with Outfit QA and Stylist-Generated Outfits project created new opportunities for showcasing stylist expertise in our Freestyle experience. By bringing our style experts together with our algorithms, we were able to improve the client experience.


Our stylist expertise is a unique differentiator in terms of the algorithmic products we are able to bootstrap in this way. It takes time to develop an organic and nuanced understanding of a domain, and our stylists have built-in, relevant knowledge they bring to these systems: they know style *and* they know their clients.


We’ve only started to scratch the surface with these kinds of human-in-the-loop machine learning experiments. More updates and initiatives to come!
