---
schema_version: "1.0.0"
document_id: "e4dd6156d27eb332765249264fa472776e5b2c93b425f99efb76be00bc85a53d"
company_key: "yc-butter"
company: "Butter"
source_id: "yc-butter-rss-cfe99054af6a"
canonical_url: "https://blog.butter.dev/changelog-0008"
published_at: "2025-12-06T00:21:48+00:00"
first_seen_at: "2026-07-24T22:18:01.434032+00:00"
fetched_at: "2026-07-28T20:55:05.658267+00:00"
content_hash: "sha256:f304af677b639071953523d5027cbdd98278357d0bf41313433f8c61cf57ee30"
---

# Changelog #0008

Welcome to Butter’s eighth changelog! Just like grandma on Thanksgiving morning, we’ve spent a whole lot of time cooking.


Starting with:


## Gramma’s Recipe Book


As a Thanksgiving treat, we launched[cookwithbutter.com](https://cookwithbutter.com/) , which uses Butter’s LLM response caching to help generate, and cache, popular holiday cooking recipes.


Look up popular pre-computed recipes, or try your own! Gramma wants all your favorite recipes, from standard[stuffing](https://cookwithbutter.com/?input=stuffing) to creative[butter, covered in butter, with a butter garnish](http://cookwithbutter.com/?input=butter%2C%20covered%20in%20butter%2C%20with%20a%20butter%20garnish) .


The remainder of our time has been focused on internal tech improvements, which are still in-progress so we’ll only briefly highlight them below:


## Data Engine Rewrite


Part necessity and part premature optimization, we’ve been rewriting our data backend to structure the cache tree in a much more efficient, S3-centric way.


This project, once completed, will include:


-


As low as 0 round trips to S3 for serving hot-path responses.


-


Truly stateless servers able to horizontally replicate and achieve high availability.


-


The ability to say we “rewrote in Rust” (meme).


## Automatic Template Induction


The goal of[template-aware caching](https://blog.butter.dev/template-aware-caching) is to expand the generalizability of cache entries by converting literal text messages into a more powerful composition of templates and dynamic variables.


Currently, this is a manual process, where users must explicitly flag their dynamic content in the[butter-bindings](https://docs.butter.dev/concepts/bindings) request headers in order for those variables to be stripped into template placeholders. Powerful, but cumbersome, especially when agent intent isn’t known in at the time of programming.


Our goal is to make the process automatic, using hints such as attention values to map out which parts of the context window are noise (ignored), which are dynamic (templated), and which are structural (cached).


Two days ago, thanks to hard work from teammate Raymond, we’ve got our first proof-of-concept working end to end:


That’s it for this week! We’re excited to get this above work out in a more public form so you can see the magic of it.


Until then, keep on cooking!
