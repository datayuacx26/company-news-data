---
schema_version: "1.0.0"
document_id: "d45724466991ac48c4c84090e2d908ce729fbe85f2afcde21c0a26618f3bacbd"
company_key: "yc-strand-ai"
company: "Strand AI"
source_id: "yc-strand-ai-news-import-ee8f1008ced1"
canonical_url: "https://strandai.com/blog/1000g-variantformer"
published_at: "2026-01-15T00:00:00+00:00"
first_seen_at: "2026-07-22T15:01:43.907826+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:8ab02ec307df6f803956c9cdd1521e543776ff988d5d94790ee4474d19b3e057"
---

# VariantFormer × 1000 Genomes: Imputed Gene Expression for the Community

# VariantFormer × 1000 Genomes: Imputed Gene Expression for the Community


January 2026 Oded & Yue


We started Strand AI to build modality transformation models for biology, tools that fill in the missing pieces across patient datasets so researchers can do better science.


While we're developing our own models, we thought it'd be fun to run **VariantFormer** (CZI Biohub's 1.2B-parameter DNA-to-RNA model) on new data and give the results back to the community.


## What we did


We took the **1000 Genomes Project expansion pack** , over 500 individuals that weren't in the original training set, and generated imputed RNA-seq expression for samples that never had expression measured.


That gave us imputed gene expression across **4,500 genes** , **45 tissues** , and **538 samples** .


We also tuned the inference pipeline to run **37× faster** on cheaper A100s instead of H100s. Way cheaper, still fast.


## Explore the data


We built an interactive visualizer so you can poke around. Filter by tissue, gene, or population and explore expression patterns across the 1000 Genomes cohort.


You can[explore the data here](https://strandai.com/1000g-variantformer) or grab the full dataset download from the explorer.


## Why we're sharing this


Our main business is licensing multimodal biological datasets, but this one's free. It's a nice way to give back while showing what modality transformation can do.


Have ideas for what models or datasets we should run next? Reach out at[\[email protected\]](https://strandai.com/cdn-cgi/l/email-protection#35535a405b5150474675464147545b51545c1b565a58) .


Thanks to the team at **Chan Zuckerberg Initiative / Biohub** and the **1000 Genomes Project** for the groundwork that made this possible.
