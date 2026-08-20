---
schema_version: "1.0.0"
document_id: "cea4145aa73551c8dbc6f12884432ee6b4223f2068fb82a7718ba709ea8991ea"
company_key: "yc-whatnot"
company: "Whatnot"
source_id: "yc-whatnot-rss-30861744a6f8"
canonical_url: "https://medium.com/whatnot-engineering/how-whatnots-taxonomy-keeps-pace-with-a-marketplace-that-won-t-sit-still-ba934c2e4a8d"
published_at: "2026-03-25T14:22:19+00:00"
first_seen_at: "2026-07-24T07:07:55.027426+00:00"
fetched_at: "2026-07-28T22:17:51.657377+00:00"
content_hash: "sha256:fb72b220fd00a03a887a4eb9347619816c7acbf57074decd7ad029b23baba99f"
---

# How Whatnot’s Taxonomy Keeps Pace with a Marketplace That Won’t Sit Still

Featured


Taxonomy


Recommendation System


AI


Ecommerce


Machine Learning


# ***How Whatnot’s Taxonomy Keeps Pace with a Marketplace That Won’t Sit Still***


[Whatnot Engineering](https://medium.com/@whatnotengineering?source=post_page---byline--ba934c2e4a8d---------------------------------------)


6 min read


·


Mar 25, 2026


--


Press enter or click to view image in full size


[Marcus Gee](https://www.linkedin.com/in/marcus-gee/) **| Discovery**


The hottest trends don’t happen over months, but start suddenly. At Whatnot, our best sellers update their strategies in real time to stay in step with the market. Connecting buyers and sellers in this fast-changing landscape is one of the most important problems Whatnot solves, but in 2025, it was a major challenge.


In this post, we’re going to walk through how we overhauled the taxonomy system to evolve more rapidly, so our team could help new communities grow.


## There is No Single Taxonomy


Whatnot supports over 200 categories across seven countries, and that number increases weekly. Our taxonomy is a platform primitive that underpins nearly every discovery experience. It shapes onboarding (what categories new users see), browse (how they navigate), search (how results are ranked), and recommendations (what their feed looks like). A bug in taxonomy doesn’t break one feature; it degrades the entire discovery surface. That makes every change high-stakes.


Here’s what we learned the hard way: maintaining a single taxonomy doesn’t work. A single taxonomy can’t optimally serve onboarding, browse, and search at the same time, because these surfaces have different goals. Let’s look at two examples to understand why:


When new users join Whatnot, they choose categories they’re interested in. That choice shapes the first feed they see. If a user selects a niche category with limited supply, their initial experience may feel sparse or low quality. For this reason, onboarding taxonomies are country-specific and supply-aware. We fold categories into broader parents, reorder them, or exclude them based on local marketplace density.


Press enter or click to view image in full size


Categories play a critical role in connecting enthusiasts and sellers.


When Labubu surged in 2025, it was subcategorized under Toys, and new users struggled to find relevant shows. When we elevated Labubu to the first page of onboarding (a similar treatment given to other big categories like Pokémon Cards), it reduced this friction and helped the category explode.


Browse has a different job: maximizing reach and discoverability. Categories are grouped logically to help buyers narrow from broad interests into more specific subgroups of shows and products. Subcategories may be cross-listed under multiple parents when buyer segments overlap. For example, the “Jewelry & Watches” subcategory “Watches” can also be found under “Men’s Fashion”. While the underlying category is the same, its placement reflects how different buyer journeys intersect.


Although these taxonomies represent the same marketplace, their structures differ based on product goals. This means we maintain multiple overlapping taxonomies, each optimized for a different surface. Managing this is the hard part.


## The problem: customization, speed, and safety


The bottleneck to scaling category growth on Whatnot now became a tooling issue, not a supply issue. We needed to be able to evolve multiple purpose-built taxonomies–but quickly and safely. Drawn-out processes would miss emerging trends, and even minor bugs could have a widespread impact on the product experience.


Within a taxonomy, relationships are modeled as a collection of (subject, predicate, object) triples (e.g., (“football_cards”, “child_of”, “sports_cards”)).


The original system for updating taxonomies was built so that relationships in a taxonomy were modified at the record level. That meant manually adding or removing triples or modifying the subject/object of existing sets, one taxonomy at a time.


As Whatnot scaled to new communities and countries, this became increasingly difficult to manage. More communities meant more categories to update. More countries meant more taxonomies to maintain. The amount of work multiplied quickly. Taxonomy updates took days of engineering effort to create and roll out.


Engineers had to hold the entire graph in their heads. Small mistakes had real consequences. When we made large batches of manual, repetitive changes, it was easy to miss a step and forget to add or remove categories. Those mistakes caused taxonomies to drift out of sync and reduced category visibility. Because every step was manual, we didn’t catch those mistakes quickly and sometimes left them in place for months.


Press enter or click to view image in full size


Managing category relationships independently means engineers must keep the whole graph in their head. Taxonomy migrations make it much simpler to make foundational changes.


## Shifting from record updates to taxonomy migrations


To solve this, we stopped treating taxonomy updates as record edits and began treating them as operations on subtrees. Across taxonomies, structures differed. Some allowed multiple parents, others enforced strict hierarchies. But every update could be decomposed into three primitives: add, move, or delete.


Take this recent taxonomy change, where we separated some of our Trading Card Games subcategories into an Entertainment cards category:


Press enter or click to view image in full size


Under our old system, this would require the following seven changes for every taxonomy we want to update.


Press enter or click to view image in full size


Six of those changes are all applying the same update. From this, it’s clear to see that we could group changes by the operation type and resulting object, so that they could be performed across multiple subjects at once. Taking it another step further, any taxonomies having this same subtree of TCG could all be batched together as well.


So in all, changes could be expressed as a transformation of categories from four inputs:


- the operation to perform (add, move, delete)
- the categories to apply the operation on (i.e source nodes)
- the new parent category of the source nodes (i.e., target node)
- the taxonomies to update


Now, under our new system we just need to define two changes!


Press enter or click to view image in full size


## Continuously evolving taxonomies


With this new approach, we had the opportunity to move to a continuous deployment model by introducing three process improvements: batching, scheduling, and validation.


**Batching.** Through abstraction, we introduced batching. Once updates were defined as composable primitives, repetitive record-level edits were replaced with coordinated operations across taxonomies.


**Scheduling.** We also introduced a cron-driven workflow to automate change execution. Now, updates can be sequenced and scheduled in advance.


**Validation.** We added dry runs. These output a JSON representation of the taxonomy, that we could visualize as a tree. In addition to manual verification of results, we added programmatic validations to detect invalid states such as loops, duplicate children, and orphaned subtrees. If validation fails, we roll back the change and alert on the offending portions of the taxonomy. These steps allow us to get ahead of any mistakes that may have been made.


These guardrails provide us with guarantees we never had in the old system. Now, every change is observable, reviewable, and predictable — allowing us to ship taxonomy updates quickly and confidently.


## Ready for anything


The marketplace never stops moving. New trends will emerge quickly, buyer demand will shift, and the categories that matter most will not wait for a quarterly migration plan. Building for that reality meant changing more than our taxonomy data. It meant changing the system around it.


By moving from manual record edits to validated, schedulable taxonomy migrations, we made taxonomy updates faster, safer, and easier to scale across surfaces and markets. That gives us a much shorter path from marketplace signal to product change.


More importantly, it sets us up for a future where taxonomy evolution is no longer a bottleneck for growth. As new communities form and new demand appears, we can adapt the marketplace structure with the same speed and confidence that our sellers already operate with.
