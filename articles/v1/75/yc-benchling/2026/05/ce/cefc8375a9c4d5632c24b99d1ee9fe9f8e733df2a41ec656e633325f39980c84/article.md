---
schema_version: "1.0.0"
document_id: "cefc8375a9c4d5632c24b99d1ee9fe9f8e733df2a41ec656e633325f39980c84"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/behind-the-model-with-sal-candido-of-biohub"
published_at: "2026-05-27T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:c77b4f805ba24b6b7daa25e4794611084870cfb0e81008494256eb73b1ba1b02"
---

# Behind the Model: Building A World Model for Protein Biology with Sal Candido of Biohub

*Q&A with Mihir Trivedi, product manager of scientific models at Benchling, and Sal Candido, engineering fellow and vice president of Biohub*


The "Behind the Model" series brings scientists closer to the researchers building the tools they use, including the decisions, tradeoffs, and thinking that don't always make it into a paper. In this installment, we go inside the latest from the[ESM](https://biohub.ai/esm/protein/about) ecosystem: a suite of models and resources from Biohub that includes a frontier protein language model, a fast and accurate folding model, interpretability features, and the ESM Atlas, which maps functional signals across 6.8 billion proteins.


[Sal Candido](https://www.linkedin.com/in/salcandido/) , engineering fellow and vice president at CZI Biohub, and[Mihir Trivedi](https://www.linkedin.com/in/mihirt) , leader on Benchling's scientific AI team, discuss what it means to build a world model for protein biology, what the Atlas makes visible that wasn't before, and how scientists can start using these tools. ESMFold2 will be available on Benchling's Model Hub soon.


**Mihir: You’re positioning the latest ESM release as a state-of-the-art world model for protein biology. What does that mean, and what components of this release enable it?**


**Sal:** The ESM team pioneered protein language modeling, and we’ve been at it for a while – having released a series of these models to the community that tracked the evolution of language models like GPT or Claude over the past 7 years.[ESMC](https://biohub.ai/models/esmc) is the most powerful of these models to date.


We actually built ESMC about a year and a half ago and have spent the time since then doing a deep dive into what the model understands about proteins, and how it understands it. We’ve used that new understanding about the inner workings of a frontier protein language model to build a system of models and resources around it: a state-of-the-art protein folding model; a design protocol that searches the model's latent space to design protein-protein interactions (the key task in computational design of therapeutics); interpretability tools that can be used to understand uncharacterized proteins; and the ESM Atlas, a map of protein biology at unprecedented scale that we hope will help scientists make new discoveries!


**Mihir: The ESM series goes back to 2019. What's the arc from ESM-1 to here? What got meaningfully better, and when did the team realize this had crossed into something qualitatively different?**


**Sal:** ESM1 and ESM1b, our earliest models, really established the basics of language modeling in this space. The team found that not only are the models useful for computationally suggesting modifications to existing proteins (variant prediction), the models begin to understand the chemical similarities between different amino acid types and make a coarse-grained prediction of how proteins fold. ESM2 models really put more compute into the models to create workhorse models that people are still using today for a lot of computational biology and design tasks. We introduced the first ESMFold alongside ESM2, which demonstrated that atomic resolution structure predictions could be extracted from the representations of a language model. And then ESM3 was our first generative model, which was capable of creating a functional fluorescent protein that was distant from anything created in nature.


ESMC is the newest model in the series. We really focused on building the best protein language model we knew how to build based on all the experience the team has accumulated making these models over the years. We added significantly more data and utilized a large scale of compute to produce a truly frontier language model.


**Mihir: ESMFold2 is dramatically faster than other folding models. What does that speed unlock for scientists? For researchers using a platform like Benchling, what becomes practical that simply wasn't before?**


**Sal:** I’m very excited about ESMFold2 because not only is it fast, it is also the most accurate model out there for protein folding — particularly on hard tasks like antibody-antigen docking. ESMFold2 is probably the best model to use if one needs to sink a lot of compute into making one highly accurate prediction or use a lot of compute to process a huge dataset.


For researchers using a platform like Benchling, I think design is the typical reason one would want to do a huge number of folds. Whether pre-screening an existing library computationally instead of in a physical lab or generating de novo binders, being faster allows a researcher to achieve the scale necessary to really start to do virtual experiments that meaningfully accelerate validation of designs in the lab.


But also, don’t sleep on huge datasets, like the ESM Atlas! I often hear about folks wanting to fold a large number of proteins, or use a folding model to understand an interactome better. ESMFold2 is efficient enough to allow those with a moderate compute budget to start looking at this scale of dataset.


**Mihir: Can you walk us through what using the ESM Atlas like in practice? What kind of connection might a scientist surface that would have been invisible before?**


**Sal:** A great example is the one in section 5 of our preprint. People are obviously very interested in gene editing systems because of their potential applications in therapeutics, synthetic biology, and the engineering of entirely new biological functions. In that atlas, CRISPR systems group together with Fanzor systems (which is a more recently discovered eukaryotic gene editor) and a bunch of new systems. The atlas makes these connections because ESMC considers these proteins to have a lot of similar parts, and a scientist can look not only at what proteins are considered to be “in the neighborhood,” but also examine the mechanistic hypothesis as to why ESMC places them there.


CRISPR is a really interesting example because it was a bacterial immune system protein that just happened to have capabilities scientists have leveraged to create a new era of genetic engineering! There are a lot of uncharacterized proteins in the atlas that ESMC thinks are distinct from proteins that are well characterized and understood. We think there is a lot of biology to be discovered in the atlas.


Another example is TMEM65, an integral membrane protein linked to mitochondrial disease but with previously unknown function. Analyzing it in the Atlas revealed similarities to organellar ion transporters and antiporters — a connection that recent studies have since confirmed experimentally, identifying it as an Na+/Ca2+ exchanger essential for mitochondrial homeostasis. This is exactly the kind of discovery the Atlas is designed to enable: surfacing functional signals that traditional homology search simply can’t reach.


**Mihir: You used Sparse Autoencoders to surface interpretable features from ESMC's internal representations. How does that change the way a scientist can interact with the model's outputs? And what does it open up that wasn’t possible before?**


**Sal:** If there is one thing I’ve learned, people will find interesting new ways to utilize all new models. In that sense, I don’t think we fully know how people will benefit from this technology yet. One thing I’m very interested in is how interpretable features can provide a natural language bridge between world models (like ESMC) and reasoning models. ESMC has always been a foundation model, in the sense that the most powerful uses are building another model on top of it. But, in a really interesting way, this makes ESMC an end-user model.


For example, we’ve been noticing that if one gives an agent the descriptions of the features that activate in uncharacterized proteins, the conjunction of models can make pretty good hypotheses / predictions of the protein’s function. We don’t dive into this in the paper but we’re seeing really great results with this, and we provide a tool for people to explore this on the ESM Atlas that just starts to imagine how it will be possible to map and reduce all of protein biology computationally sometime soon.


**Mihir: ESMFold2, ESMC, and ESM Atlas together form a unique open ecosystem. What was the thinking behind making all of this available to every scientist, and what does a shared foundation make possible that other tools can't?**


**Sal:** Biohub is a nonprofit focused on curing or preventing all disease, so there is no reason for us to keep it proprietary. We didn’t have to think too hard about open sourcing this!


But in all seriousness, we have a big mission and we won’t be able to accomplish it ourselves. We believe the fastest way to cure disease is to advance what is possible for everyone, and push the frontier together with the whole scientific community.


I don’t know if I’d say you can’t do it with proprietary tools, but not everyone has access to those tools or economic incentives to use them. With open tools everyone using a platform like Benchling can have access to the models that are state-of-the-art, and we strongly believe putting these tools in everyone’s hands will create shared foundations for the scientific community to advance in ways that can be reproducible, open, and groundbreaking.


**Mihir: There's a bigger claim underneath all of this that protein language models represent a foundational substrate for understanding all of protein biology. How do you think about that, and what would it mean for biology if it's true?**


**Sal:** Protein language models learn the structures and functions of proteins just by looking at their sequences, without access to the knowledge bases of empirical science like biology textbooks, experiment results, or research papers. It is pretty remarkable how concordant they are with experimentally-derived knowledge. For obvious reasons, in our paper we spend a lot of time talking about how the representations are aligned with what is known about proteins. But the flip side of that coin is that it’s likely these models have learned things in their world model of biology that scientists don’t know yet, and we likely have things to learn about protein biology from these models. I feel quite lucky I get an opportunity to be among those first in line to explore this alien intelligence!


**Mihir: Now that you have a foundation model, fast folding model, and a billion-scale atlas — what's the next constraint you're trying to break?**


**Sal:** We believe that building agentic scientific reasoning systems with access to the intelligence encoded in biological world models is the path to accelerated scientific discovery, in which hypotheses can be generated, evaluated, and refined in silico at scale. We take a first step towards this by building agents into the ESM Atlas, powering automated discovery workflows and providing a more intuitive way to query the knowledge contained in the outcomes of billions of evolutionary experiments.


Scientific reasoning agents can continuously improve our understanding of both the world model and biology by iteratively refining our understanding of both proteins and the machine reduction of proteins through which ESMC understands proteins. Furthermore, better understanding and curation of the data used to train ESMC can result in a stronger model, leading to a virtuous cycle of self-improvement that can rapidly enable new discovery and understanding that can be used to make biology legible and programmable – which is a key step towards rapid progress towards curing disease.


Our goal is to scale this intelligence across all of biology.
