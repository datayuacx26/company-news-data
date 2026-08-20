---
schema_version: "1.0.0"
document_id: "bc58c4a17397b92f2633b26bbecb26dea1fc637ee197175db475b420feb532c1"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-news-import-3c8571c35bcb"
canonical_url: "https://www.adaptyvbio.com/blog/pro1"
published_at: "2025-06-16T00:00:00+00:00"
first_seen_at: "2026-07-24T14:21:15.816567+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:c7dafe590195010689ae058725893a75eab631079975d0c8061765d62abc6e8e"
---

# Designer Spotlight: Can a language model reason about protein design?

/


TL;DR


- -


Michael Hla built Pro-1, a language-based reasoning model able not just to design proteins, but also reason about how it’s designing them.


- -


We offered him free binding affinity and thermostability testing in the Adaptyv Foundry for 19 of his FGF-1 (Fibroblast growth factor 1) sequences optimized by Pro-1.


#### Pro-1: the protein reasoning model


In March, Michael Hla


[took the protein design community by storm](https://x.com/hla_michael/status/1898106485005336988)


when he released


[Pro-1](https://michaelhla.com/blog/pro1.html)


- the first protein reasoning language model. His proposal is simple: let a language model distil biochemical intuition so it can do all the protein optimization you want. What this means more precisely: use a complex training scheme, with recent innovations in large language model reasoning used by AI labs like OpenAI and DeepSeek, to have a model that both proposes mutations and explains why it chose them.


Michael makes several good points for why protein design should be delegated to language models in his


[blog post](https://michaelhla.com/blog/pro1.html)


. Some of the more interesting ones we found are


**interpretability**


- the model argues for each mutation it makes, pointing to relevant (or hallucinated) biochemical motives and paper references; and


**flexibility**


— he mentions Pro-1 can be prompted with sequences, PDB structures, even experimental results.


#### How to train your protein thinker


We found his training scheme incredibly unique. Michael combines biochemical intuition with synthetic data generation from specialized protein language models, a training framework from reinforcement learning, and a physics-based representation of protein stability. We will briefly describe it, but you should check out Michael’s


[blog post](https://michaelhla.com/blog/pro1.html)


and


[thread](https://x.com/hla_michael/status/1898106485005336988)


for more info!


1. **Fine-tuning on synthetic reasoning traces**


2. **Reinforcement learning with the Rosetta energy function**


3. **Creativity rewards**


We were all impressed by Pro-1, so we wanted to put it to the ultimate test:


**lab validation**


. We gave Michael some free binding affinity and thermostability assays for any designs he wanted. He chose to optimize the fibroblast growth factor 1 (FGF-1).


#### Why FGF-1?


As a growth factor, FGF-1 is one of the most


[versatile proteins](https://www.nature.com/articles/s41392-020-00222-7)


. It regulates the fate of bone marrow cells and may promote bone repair, the development of lung epithelial cells with a therapeutic effect on pulmonary fibrosis, and is highly expressed in inflammatory cells. Its


[role in type 2 diabetes](https://www.nature.com/articles/nrendo.2017.78)


is becoming better understood, with experiments showing FGF-1 injections


[reduced the levels of glucose and increased the sensitivity to insulin](https://www.nature.com/articles/nature13540)


in mice.


It binds to plenty of targets, including the fibroblast growth factor receptor 1 (FGFR1) and FGFR2. FGFR1 aberrations occur in


[several types of cancer](https://aacrjournals.org/clincancerres/article/22/1/259/248480/The-FGFR-Landscape-in-Cancer-Analysis-of-4-853)


and there are already FGFR1-inhibiting drugs like


[Pemigatinib](https://www.nature.com/articles/s41571-024-00869-z)


[for bile duct cancer treatment](https://www.nature.com/articles/s41571-024-00869-z)


. FGF-like binders to FGFR1, especially


[when conjugated with cytotoxic drugs](https://pubmed.ncbi.nlm.nih.gov/34867353/)


, could be a potent cancer therapeutic.


Michael mentioned another interesting fact about FGF-1: it has a


[pretty low denaturation temperature](https://x.com/hla_michael/status/1926750321079886144)


. Maintaining its binding while also increasing the melting temperature is a worthwhile task for Pro-1.


#### How we are measuring melting temperatures and binding affinities


We ran our standard automated assay for


[affinity characterization](https://docs.adaptyvbio.com/experiment-types/affinity-characterization)


and


[thermostability](https://docs.adaptyvbio.com/experiment-types/thermostability)


. Proteins were expressed with a


[cell-free system](https://www.nature.com/articles/s43586-021-00046-x)


, followed by affinity characterization via


[bio-layer interferometry (BLI)](https://docs.adaptyvbio.com/technology/biolayer-interferometry)


with the FGFR-1 target, FGF-1 wild-type control, and the designs Michael uploaded on our Foundry Portal.


[BLI](https://en.wikipedia.org/wiki/Bio-layer_interferometry)


measures the binder association and dissociation kinetics via the interference pattern of light reflected from a sensor surface. With these measurements and our in-house post-processing and curve-fitting software, we can calculate the binding affinity ($K_D$) of a protein to its target.


To measure the melting temperature of Michael’s designs, we used our


[newly-developed thermostability assay](https://docs.adaptyvbio.com/experiment-types/thermostability)


. The melting temperature (or $T_m$) represents the temperature at which 50% of a protein is in its unfolded state. This is around


[49 °C](https://pmc.ncbi.nlm.nih.gov/articles/PMC11113989/)


for the wild-type FGF-1


. We are using an automated


[nanoDSF](https://en.wikipedia.org/wiki/Nano_differential_scanning_fluorimetry)


(nano differential scanning fluorimetry) protocol: proteins are heated up and we measure the fluorescence shift of the tryptophan and tyrosine amino acids as they get more exposed from the protein’s core. We normalize these values and quantify the melting temperatures in our post-processing pipeline.


#### Pro-1 yields more stable binders


Most of the variants tested were expressed. Out of these, only 6 maintained binding to their target in the same range as the wild-type. In the figure above, we have highlighted the 3 variants with a $T_m$ higher than the measured control - the wild-type FGF-1 with 50.8 °C - that also bind to FGFR1.


What is more impressive is that a single-point mutant (K116E) reached a melting temperature improvement of 24 °C over the wild-type, and that Pro-1 even suggested this variant. When we consider it was trained on synthetic “perturbed” data and reasoning traces and an


*in silico*


objective (the Rosetta energy function), these are spectacular results! Most other Rosetta-based thermostability optimization studies also reach an


[improvement of 20 °C](https://onlinelibrary.wiley.com/doi/full/10.1002/pro.4428)


, yet none of them have a model able to explain in writing why it chose those mutations.


Michael showed


[an example](https://x.com/hla_michael/status/1926750332475920403)


of Pro-1’s reasoning trace for v37 variant (the 7-mutant in our bar plot). We found it interesting how it knows FGF-1’s binding partners and some plausible biochemical interpretations (e.g., mutations that reduce flexibility should increase stability, targeting hydrophobic patches to reduce the chance of aggregation). However do not attempt to fact-check its references: we tried that and we could not find any “Wu et al., 2017” that suggested the K127E mutation could increase stability of the FGF2 heterodimer, nor any “Kim et al., 2015). But this should not diminish Pro-1’s success - who knows, maybe the Pro-2 will align its reasoning with verified references. If OpenAI’s Deep Research can do it, so could Pro-1.


#### Resources and links


- You can find all thermostability data


[here](https://www.adaptyvbio.com/1f35ca69e7be8077994fc9ce4578318c)


and the binding affinity data


[here](https://docs.google.com/spreadsheets/d/1ly4xdTXLgPQtKYMMJkpEreyyy5P7D-NLvJaUeeA_3Jo/edit?usp=sharing)


- Try out Pro-1


[here](https://huggingface.co/mhla/pro-1)


- Say hi to Michael Hla:


[Website](https://michaelhla.com/)


,


[X](https://x.com/hla_michael)


,


[LinkedIn](https://www.linkedin.com/in/michael-hla-58a468167/)


- **Have some novel proteins you want to test in the lab?**


****


Come talk to us


— we’d like to run many more of those protein designer spotlights, so if you have a cool new hypothesis or model to test we’d love to hear from you!
