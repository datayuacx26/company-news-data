---
schema_version: "1.0.0"
document_id: "568892d5339a293b6879de0dc418c72717722c639eae53aedf27faa3ba593c6f"
company_key: "yc-ionworks"
company: "Ionworks"
source_id: "yc-ionworks-news-import-f340ab61a31b"
canonical_url: "https://ionworks.com/blog/mechanics"
published_at: "2025-04-20T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:30.651124+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:9b67cfadfe6ca2caf402746fa2b750ae4263b28d575973e739a7b406e34ad2a8"
---

# Under Pressure: The Role of Mechanics in Battery Degradation

In our previous posts, we explored[SEI growth](https://ionworks.com/blog/sei) and[lithium plating](https://ionworks.com/blog/lithium-plating) ,two major contributors to battery aging. However, another critical factor influences these degradation mechanisms: mechanics. Unlike SEI formation or lithium plating, mechanical effects do not directly degrade the battery. Instead, they create the conditions that accelerate other degradation pathways, making them a central aspect of battery modeling.


## Why electrodes expand and contract


During each charge and discharge cycle, lithium ions move in and out of the active material particles in each electrode. This insertion and extraction cause the particles to expand and contract, introducing mechanical stresses and deformations. The extent of this expansion varies depending on the material. For example, silicon is well known for its extreme expansion, swelling up to four times its original volume during lithiation. While silicon offers a much higher capacity than conventional graphite anodes, this significant volume change presents a major challenge, leading to mechanical instability and faster degradation. Therefore understanding the mechanics of batteries is not only important to extend its life but also to enable new chemistries.


## How mechanical stress accelerates other aging modes


Although mechanical effects do not directly degrade the battery, they set the stage for other degradation mechanisms. Repeated expansion and contraction can lead to cracking, either on the surface of active material particles or within the binder that holds them together. Surface cracks expose fresh material to the electrolyte, promoting additional SEI formation and increasing resistance. Cracking within the binder reduces electrical conductivity and can lead to the loss of active material, as sections of the electrode become electrically isolated and no longer contribute to charge storage.


## Modelling mechanical effects across scales


Current modeling approaches primarily focus on the particle level, often assuming spherical elastic particles and applying linear elasticity theory to describe the stresses. These stress models are then coupled with degradation models to understand how mechanical strain influences other aging mechanisms. However, a major challenge remains: how do localized stresses at the particle level translate to stress distribution at the electrode and battery levels? Addressing this issue is an active area of research, as accurately modeling mechanical effects across different scales is essential for predicting battery behavior and improving durability.


Understanding mechanical effects is essential for extending battery lifespan. While SEI growth and lithium plating are well-established degradation pathways, mechanical stress plays a silent but significant role in shaping both. By integrating mechanical modeling with electrochemical and thermal insights, researchers can develop better materials and electrode designs that minimize cracking, preserve active material, and ultimately enhance battery performance.
