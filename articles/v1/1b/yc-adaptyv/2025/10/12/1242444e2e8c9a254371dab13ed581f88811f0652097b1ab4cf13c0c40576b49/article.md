---
schema_version: "1.0.0"
document_id: "1242444e2e8c9a254371dab13ed581f88811f0652097b1ab4cf13c0c40576b49"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-news-import-3c8571c35bcb"
canonical_url: "https://www.adaptyvbio.com/blog/proteinbase"
published_at: "2025-10-06T00:00:00+00:00"
first_seen_at: "2026-07-24T14:21:15.816567+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:b61df0a0f7ee8985516280290f65d2a9fbb9fe86b48a2f3c8072e051224a68e3"
---

# Proteinbase — the home of protein design data

Today we’re launching


[Proteinbase](https://proteinbase.com/)


, a single hub for experimental protein design data. Over 1,000 novel proteins are already live, each with computational predictions, experimental validation, and the method used to design them. Everything comes from the Adaptyv lab under standardized protocols, which means the results are reproducible, comparable, and include negative data that usually never gets shared.


To encourage open science, we’re also offering a


[20% discount on Adaptyv lab validation services](https://start.adaptyvbio.com/)


if you open source your results on Proteinbase!


#### What are we trying to fix?


**📊 Lack of open, high-quality protein experimental data (including negative data)**


*How we're fixing it:*


We’ll periodically release thousands of experimental data points generated in the Adaptyv lab, from customers who choose to open source their result and from our internal benchmarking campaigns.


**⚖️ Lack of real-world benchmarks for protein design pipelines**


*How we're fixing it:*


Proteinbase links every protein to the design method that created it. As data accumulates, you can see how each model performs across different tasks. We're also introducing standardized benchmarks like


[BenchBB](https://beta.adaptyvbio.com/benchbb)


to establish clear performance metrics.


**🧑‍🔬 Lack of standardisation in experimental protocols which makes the data hard to compare**


*How we're fixing it:*


All data on Proteinbase comes from the Adaptyv Lab, using standardized protocols. Every result


can be traced back to its exact experimental conditions.


**🧠 Lack of experimental validation opportunities, which makes it hard to see novel ideas emerge**


*How we're fixing it:*


We will organize regular protein design competitions that are free to enter, with testing fully funded by Adaptyv or partner organizations.


#### How


does Proteinbase work


?


**Proteins**


The core elements of Proteinbase are


[proteins](https://proteinbase.com/proteins)


. For each protein published on proteinbase, we’ll show both computational predictions and experimental measurements. All submissions on Proteinbase go through our structure folding and annotation pipeline: they get assigned a folded structure via the recent


[Boltz-2](https://github.com/jwohlwend/boltz)


model, several metrics to characterize their designs, and structural domain annotations. Once the protein has been validated in the lab, all experimental information will be available on the protein’s page, from BLI curves to expression measurements or thermostability.


**Collections**


[Collections](https://proteinbase.com/collections)


group related proteins together, like a curated showroom or a playlist. These can be organized around a hypothesis, model launch, benchmark, or any theme that makes sense for your work. Collections can contain binders against a target, optimized enzyme variants,


*de novo*


proteins from a model like RFdiffusion, or any other designed protein.


**Design Methods**


[Design Methods](https://proteinbase.com/design-methods)


are the tools and approaches used to create proteins. These range from multi-step pipelines like


[BindCraft](https://proteinbase.com/design-methods/bindcraft)


to single models like


[EvoDiff](https://proteinbase.com/design-methods/evodiff)


. Each protein links back to its design method whenever possible. The most successful methods (or state-of-the-art) can be easily retrieved within Proteinbase, each containing expression and hit-rates.


**Targets**


[Targets](https://proteinbase.com/targets)


are the molecules that designed proteins aim to bind. Each target has its own page showing general information and performance statistics across all tested proteins. We’re making it easy to find which methods performs best against any chosen target or which targets are the most popular.


**Designers Profiles**


All personal collections and designs can be showcased in a Designer Profile. We’ll also keep track of the designers’ favourite targets, success rates, and more, making it easy to quantify their progress. We’re expanding the Profiles even more in the future updates.


**Contributing / Downloading data**


All current data on Proteinbase is under ODC-BY license, meaning it’s free to explore,


[download](https://proteinbase.com/download)


, and use for anything the designers would want. This includes searching for leads to improve against a difficult target difficult or training some machine learning models to predict binding affinity. Any protein tested on Adaptyv can be published on Proteinbase. Learn more here:


[https://proteinbase.com/publish](https://proteinbase.com/publish)


#### What’s coming up next?


**Collection drops:**


We'll release new experimental data every week, be those from our own experiments, from community contributors, or from competitions. We’re open-sourcing the experimental measurements, so designers can use this data however they want and keep designing!


**Designer profiles:**


We’re putting the


*designer*


in protein designer. We’ll enhance the profiles to include achievements, track metrics across proteins, and showcase rankings in the recent competitions. As people contribute and run more experiments, they’ll build a track record, and we’re making sure they’re


*rewarded*


for that


.


**Competitions**


: We’ll launch a new Protein Design Competition soon! But isolated competitions, albeit helpful, won’t help the field progress at the exponential rate we imagine it could. So we’ll launch multiple, constantly, gauging any shifts in the meta or any problems that appear to be “solved”. And, on top of that, we’re having ad-hoc research sprints and crowdsourced challenges - imagine a bio-hackathon every week. These are the fast design-build-test cycles we imagine, but open for everyone to contribute and


*engineer*


biology.


#### Why now


The protein design field is advancing at an incredible pace. New models, tools, and communities are emerging every month, pushing the boundaries of what can be designed and tested.


However, the protein design landscape is still very fragmented.


We’ve seen this expansion along three major axes


:


1. **Open-source models and communities.**


Over the past year, open-source innovation has accelerated. Teams like Boltz, BindCraft, Mosaic, and Germinal have released powerful models with flexible licenses, allowing anyone to explore, modify, and build on them. This has created a thriving ecosystem of variants, forks, and experimental methods built from shared foundations. Proteinbase provides a common space for these open models to be tested, compared, and documented, turning scattered efforts into a cohesive public resource.


2. **Industry-led research and proprietary models.**


At the same time, established companies are driving rapid progress with their own models and datasets. Chai Discovery, LatentLabs, DeepMind, Nabla Bio, Microsoft, and Cradle are releasing new design methods and publishing validation results at an unprecedented pace. These groups often set benchmarks and define new frontiers for what’s possible in protein design. Proteinbase complements this by offering a transparent reference point where results from both open and proprietary efforts can coexist and be compared.


3. **Independent researchers and new entrants.**


The field is also attracting a wave of new participants, from academics and small startups to individual enthusiasts, who are eager to design proteins but often face fragmented tools and information. Many rely on incomplete information, online communities, and trial-and-error workflows. Proteinbase serves as an entry point for this growing group, helping them discover methods, learn from real experimental data, and connect their work to the wider ecosystem.


We’ve seen a massive momentum across all three axes, and we’re sure this is just the beginning! With Proteinbase, we’re building the home of protein design data, where protein designs, experimental results and design methods come together to be shared, compared and learned from.


Go check out


**Proteinbase**


now:


[proteinbase.com](http://proteinbase.com/)
