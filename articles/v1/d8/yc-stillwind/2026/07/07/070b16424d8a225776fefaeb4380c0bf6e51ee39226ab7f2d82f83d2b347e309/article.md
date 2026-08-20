---
schema_version: "1.0.0"
document_id: "070b16424d8a225776fefaeb4380c0bf6e51ee39226ab7f2d82f83d2b347e309"
company_key: "yc-stillwind"
company: "Stillwind"
source_id: "yc-stillwind-news-import-51d46c6f1f6f"
canonical_url: "https://stillwind.ai/blog/stillwind-search-launch"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-22T14:57:19.688518+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:f67657e708c6be48016168cdadb52d2da4c5fc049cf3aa27d681ef4beb2019f3"
---

# Stillwind Search - High resolution component search for PCBs

It is an exciting day for us because today we are happy to announce that our first product, Stillwind Search, is ready for public use.


It has been a significant effort to get to this point, so we want to take the time to go over what problem we are trying to solve and how we went about solving it.


After talking to several electrical engineers, we’ve identified a common pain point: finding the correct components for a project takes up a lot of time and is very tedious. So we decided to come up with a solution for that.


The current way to find components is to go on a distributor’s website, find the category you are looking for, hope the filters cover what you want, and then read through the datasheets of what remains.


In a perfect world, there would be one filter for every single property you might be interested in, uniformly and perfectly filled out for every part that is out there. So we built just that: a filter for basically every single property, for every category of part that is out there. We will go over how we did that in a future blog article, but for now let’s just say that, in the end, we had over 130k properties and we extracted all information about a part into these properties.


We put this data into a PostgreSQL database that is then queried by an AI agent, which you can interactively text with to find the correct part for your project. We also verify that the information we show you is correct and relevant to your search.


We invite you to try this out today, and please tell us your thoughts about it. You can contact us at contact\[@\]stillwind.ai


By the way, we’re already working on a new agent that helps you find combinations of parts, so you can find the optimal set of parts to build complex, complete systems. Stay tuned!
