---
schema_version: "1.0.0"
document_id: "c1b19b3616a8fc36f236d8112e664bbab12d04a251609b451f9a30655095a1cb"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-news-import-3c8571c35bcb"
canonical_url: "https://www.adaptyvbio.com/blog/adaptyv-api"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-24T14:21:15.816567+00:00"
fetched_at: "2026-07-28T21:56:50.434513+00:00"
content_hash: "sha256:e8884ba83056fd7d7066001dfcbbebdd278ba0a53e3d19a14b3007f694b97118"
---

# Adaptyv API: Giving everyone access to a real lab

/


TL;DR


- -


• We're releasing the Adaptyv API, giving everyone programmatic access to our wet-lab infrastructure


- -


• Query our target catalogue, create experiments, track pipeline status, get cost estimates, and pull structured results via simple API calls


We're officially releasing the


[Adaptyv API](http://agents.adaptyvbio.com/)


, which gives you and your AI agents access to our wet-lab.


Over the past three years we've tested tens of thousands of proteins from pharmas, AI for protein design companies, academic labs, alongside dozens of early-stage startups and individual researchers. Until now, all of that went through our


[Foundry portal](https://foundry.adaptyvbio.com/)


or also email threads and Slack channels.


**We think the process of testing a designed protein should be as simple as calling an endpoint**


, so we built an API around the same infrastructure those teams already use, to make everything as accessible as possible. We're thus


**allowing AI to program wet-lab experiments**


.


You can query our target catalogue, create experiments, track them through the full pipeline, get cost estimates, and pull structured results when they're ready. AI agents can run the same workflow overnight and parse results in the morning, without anyone supervising.


### What the API covers


**Creating an experiment**


is one API call. You pick an experiment type, upload your sequences and we handle gene synthesis, expression, purification, and characterization from there.


Additionally,


**cost estimates**


are available before you commit. Pass in your experiment parameters and you get per-sequence pricing back, with full quotes and PDF export. If you're running larger campaigns, you can use this to set experimental budgets programmatically.


After you confirmed your experiment, you can check pipeline status at any point, so you always know where your proteins are in the process.


The


**target catalogue**


is searchable by name, organism, or gene, and each target includes available assays and pricing. Your script or agent can plan a full campaign before you generate a single candidate, which is something that used to require weeks of extensive research and planning.


When experiments finish, you get


**structured protein data**


back: KD values, binding classifications, kinetic parameters, melting temperatures, expression values, all as JSON. If you're running an optimization loop, your model can ingest the results directly and use them to inform the next round of designs.


For


**agent workflows**


, you can create


**scoped API keys**


that are read-only, org-specific, or time-limited, and hand those to your agent. A restricted token lets it browse targets and pull results, but it can't place orders until you allow it. We built this with the assumption that in many cases the agent will be the one interacting with the API, not the user directly, and you want to control how much agency our agent gets.


### Why we built this


We started Adaptyv with the idea that anyone should be able to test a designed protein, whether they have their own lab or not. We've seen this play out over the last two years: teams start with a few sequences, get data back, iterate on their models, and come back with better designs. The ones running fast


[design-build-test-learn](https://www.adaptyvbio.com/blog/po101)


cycles are the ones whose models improve the fastest.


The API is the latest step in making that cycle accessible to more people. A researcher validating their first


[BindCraft](https://github.com/martinpacesa/BindCraft)


or


[design-a-protein](https://design-a-protein.com/)


design can now use the same infrastructure as a frontier AI lab running thousands of iterations. An agent running overnight can submit proteins and retrieve results without waiting for anyone.


### What's next


We're iterating fast on the API based on partner feedback. We're also working on webhook support for real-time experiment status updates, richer result formats, and tighter integration with


[Proteinbase](https://proteinbase.com/)


for published experimental data. If there's something you need that we don't support yet, we want to hear about it.


### Who's already building on it


[Tamarind Bio](https://www.tamarind.bio/)


[integrated the API into their computational design platform](https://www.tamarind.bio/blog/introducing-the-tamarind-bio-assay-portal)


for proteins and antibodies. Their users can now order binding, expression, and stability assays without leaving Tamarind.


[Phylo](https://phylo.bio/)


is building a biology IDE where agents plan and execute experiments and data analysis. They integrated the API so their agents can submit experimental campaigns, retrieve results, and reason over them to plan the next round of designs, all within the same environment their users already work in.


More integrations coming soon - stay tuned!


### Getting started


The docs are here:


[docs.adaptyvbio.com/api-reference/api-introduction](http://docs.adaptyvbio.com/api-reference/api-introduction)


The spec lives at the endpoint itself:


[foundry-api-public.adaptyvbio.com/api/v1/openapi.json](http://foundry-api-public.adaptyvbio.com/api/v1/openapi.json)


We also built an interactive showcase where you can try each part of the API with live request/response panels:


[agents.adaptyvbio.com](http://agents.adaptyvbio.com/)


Email us at


support@adaptyvbio.com


if you're building something that could use a wet-lab backend so you can get an API key. Our customers can obtain API keys directly from their


[Foundry Portal accounts](https://foundry.adaptyvbio.com/)


.
