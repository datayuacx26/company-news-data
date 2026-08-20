---
schema_version: "1.0.0"
document_id: "669557aee64f4d92f042a502739946e2e9847ffa88245993b8a4011be1a650e4"
company_key: "yc-wyvern"
company: "Wyvern"
source_id: "yc-wyvern-news-import-543bd8ef8708"
canonical_url: "https://www.wyvern.space/blog/open-data-program-feature-harmful-algal-bloom"
published_at: "2025-06-23T00:00:00+00:00"
first_seen_at: "2026-07-22T20:40:14.439396+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:0ebe2dd49f6a0175197c1d9860429670388fe0ef76f743e1a8c79c2aef0e29ab"
---

# Open Data Program Feature: Harmful algal bloom detection with Wyvern hyperspectral imagery

In Part 1 of his new series, Alex uses Wyvern’s Open Data Program to run exploratory analysis on chlorophyll-a as a proxy for detecting harmful algal blooms (HABs). Using Python notebooks and interactive maps, he walks through how hyperspectral indices like NDWI and NDCI can help isolate water bodies and signal algal activity using cloud-native, open-source tools every step of the way.


## An excerpt from Alex’s blog


> Let’s embark on some exploratory data analysis using Wyvern’s hyperspectral imagery Open Data Program. Harmful Algal Bloom (HAB) detection is attempted using selected band wavelengths and raster analysis techniques. This is part 1 of 2 of a series about modern GIS pipelines leveraging Python and the open-source, cloud-native geospatial tech stack…


#### Part 1 of 2


Data Engineering is \[a\] Software Engineering discipline, and one of the things data engineers do is help scientists get their code into production, make it reproducible, and make it fast.


In this post, I put on my Data Scientist hat and try some exploratory data analysis (EDA) using: Python, RasterIO, NumPy. Shapely, GeoPandas, MatplotLib, Ipyleaflet, and Jupyter.


In an upcoming post I will wear my Data Engineer hat and port the Python code from the notebook environment into Dagster, a production-grade pipeline/workflow orchestrator.


#### Harmful Algal Bloom (HAB) in the news


The Guardian recently published this article with related facts, photos and interviews:


As the Earth heats up, the amount of algae in our waterways is rapidly increasing, transforming the colour of lakes and killing entire ecosystems. source:[Patrick Greenfield, The Guardian, 2025-06-10](https://www.theguardian.com/environment/2025/jun/10/toxic-blooms-algae-marine-dead-zones-lakes-ecosystems-dead-elephants-fish-sea-lions-aoe)


*…*


[Read the full blog here.](https://ricegeo.dev/hab-detection-wyvern-hsi/)


Big thanks to Alex for showing what’s possible when transparency meets technical creativity. We’re excited to keep amplifying community-driven work like this.
