---
schema_version: "1.0.0"
document_id: "b8041b76c1f7f54294379d37e04a321863b79192aca464aa77195e8e86c3fc3d"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-rss-523875a8e4a3"
canonical_url: "https://adaptyvbio.substack.com/p/introducing-benchbb-and-the-community"
published_at: "2025-04-26T17:36:46+00:00"
first_seen_at: "2026-07-24T14:21:19.055100+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:6a901ab1fe0541c53d83e1d64910df0b53299e4b1ad6f789665994915662867f"
---

# Introducing BenchBB and the community paper of the Protein Design Competition

# Introducing BenchBB and the community paper of the Protein Design Competition


### We wrote a community paper about our Protein Design Competition, teaming up with your favourite protein designers from both rounds. We close the paper by creating BenchBB, the Bench-tested Binder Benc


[Adaptyv Bio](https://substack.com/@adaptyvbio)


Apr 26, 2025


> **TL;DR**
>
>
> We wrote a community paper about our
>
>
> [Protein Design Competition](https://foundry.adaptyvbio.com/competition) , teaming up with your favourite protein designers from both rounds.
>
>
> We aimed to explore the competition data in as many ways as we could, take stock of the state of the field, what SotAs have been established across the rounds, and which metric is most predictive of binding and expression.
>
>
> The one thing we kept meeting throughout:
>
>
> **the lack of a standardized benchmark set for protein binder design** , leading to difficult comparisons and a lack of consistent, high quality data. This is why we close the paper by creating
>
>
> **BenchBB** , the
>
>
> **Bench** -tested
>
>
> **B** inder
>
>
> **B** enchmark — a curated set of 7 protein targets designed to capture diverse binder design challenges by remaining accessible enough for wide scale lab validation.
>
>
> Want to read the community paper?
>
>
> **Check it out on [Biorxiv](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2)** !
>
>
> Want to test your protein-design model on BenchBB?
>
>
> **Go to [benchbb.bio](https://beta.adaptyvbio.com/benchbb) and get started** !


### The community paper after the Protein Design Competition


As you might remember, we hosted the


[Protein Design Competition](https://design.adaptyvbio.com/) . Briefly, we called for protein designers to create novel binders for EGFR and received over 1857 total submissions, 600 which we then experimentally tested in our lab, validating a total of 60 novel binders! For details you should check our


[previous](https://www.adaptyvbio.com/blog/po102)[blog](https://www.adaptyvbio.com/blog/po103)


[posts](https://www.adaptyvbio.com/blog/po104) . However, in these posts we could barely scratch the surface of possible data exploration. There was a


[strong demand to do more analyses and collect all the learnings](https://x.com/anthonygitter/status/1846203654144962589) , including those that our participants


[had already been doing](https://blog.booleanbiotech.com/what-we-learned-adaptyv) throughout the rounds.


The spirit of collaboration was strong throughout the competition so it did not seem fitting for us to act as the curators and gatekeepers for what would be included or said in such a writeup. Instead, we decided to form a consortium and launched


[an open call for collaboration](https://www.adaptyvbio.com/blog/po104#:~:text=If%20you%20want%20to%20participate) . To our delight, several of the participants responded, and the excitement to contribute turned into a wide range of analyses, with people applying their expertise in different types of biomolecules (from antibodies to peptides), statistics, and general understanding of the current state of computational protein design, and vision for its future. The discussions in our Slack channel were some as delightful as they were interesting, and we finally distilled them into the preprint


**[Crowdsourced Protein Design: Lessons From The Adaptyv EGFR Binder Competition](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2) .** We will now summarize some of its key takeaways, but we strongly encourage you to read the full paper (honest clickbait: table 4 might surprise you!)


### In summary: More data = better metrics & models = better proteins


We took another look at the data we


[had already](https://www.adaptyvbio.com/blog/po102) analyzed in our


[blog posts](https://www.adaptyvbio.com/blog/po103) , to see whether we can identify patterns and trends on the combined dataset of both rounds.


While there was a sizeable increase in expression success and binder hit rate (see below), there remained many questions about when certain protein design approaches might be better than others and how to predict whether a protein makes a good binder from just the sequence alone. During the competition, we received many more protein designs than we could test in our lab during the timeframe of the contest. We thus had to select which designs to experimentally validate and which not.


Our strategy was the following:


1.


We ranked designs by a set of computational metrics (ipAE, iPTM and ESM2 PLL) and chose the top 100 designs according to this ranking.


2.


We then additionally selected another 300 designs based on whether the protein designers had described a particularly interesting or novel design method.


As we already suspected that the computational metrics might not correlate well with the binding affinity, this approach also rewarded protein designers for proposing new design methods that not just aimed at maximizing the computational score.


In the paper, we run lots of statistical analyses to check the correlation of various surrogates (including iPTM, ipAE, and ESM 2/3/C) with binding strength and found that:


1.


at least on our dataset, ipAE, iPTM and ESM2 PLL (normalized or not) only correlate weakly with KD


*KD* ​, despite some of them being part of the competition target metric.


2.


the good news, as


[Nikhil Haas (BioLM) had already noted](https://x.com/NikhilHaas/status/1849132864866332862) , ESM3 and ESMC, when length normalized


*do* correlate with KD


*KD* ​, at least on our dataset.


We don’t recommend you to rush towards blindly maximising this metric though, because as we show later in the paper,


[specific antibody domains might require different metrics](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.4.5) to inform binder design — huge shoutout to Nikhil and the team at


[BioLM](https://biolm.ai/) for donating both the data and their valuable time for making this analysis possible.


Beyond this, there’s a lot more in the paper and the supplementary, e.g. a detailed study of the specific


[EGFR domains](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.A.5) and their role in the successful binders,


[highlights](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.3.3) of


[methods](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.3.1) used


[throughout](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.3.4) the


[competition](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.3.5) (including


[Cradle’s winning entry](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.3.2) , which they have been further evaluating and explaining


[in more detail in their own series of posts](https://www.cradle.bio/blog/adaptyv2) ) and more context on the competition and the


[community response](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.2.5) . We thank all the authors for the time and resources they to get the paper done this way.


However, the one thing that kept being said as we did all of these analyses and was confirmed with every insignificant p-value our analyses yielded is:


**we need more data.**


Thus, in the


[discussion](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#section.5) we tried to acknowledge the great advances that became obvious as we looked back on the data, but also stress the limitations and challenges the field still faces:


1.


Computational metrics are getting better,


[but are not yet plug-and-play reliable](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.5.1) , largely caused by the extremely non-standardized datasets they are derived from, often using different assays


2.


This lack of standardization extends as far as the


[definition of a binding hit](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.5.2) , making it very difficult to compare results from one report to another


3.


Even if the assays and hit definitions were stable, everyone makes up their


[slightly tweaked sets of targets right now](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2.full.pdf#subsection.5.3) , with only a few if any targets being shared across studies.


Of course, it is too easy to only complain about things, so we also suggest a first step in fixing this situation.


### Introducing


**BenchBB** : the Bench-tested Binder Benchmark


BenchBB is a curated set of 7 protein targets designed to provide a rigorous, consistent, and practical benchmark for computational binder design methods. While recent papers (


[RFdiffusion](https://www.nature.com/articles/s41586-023-06415-8) ,


[AlphaProteo](https://arxiv.org/abs/2409.08022) ,


[BindCraft](https://www.biorxiv.org/content/10.1101/2024.09.30.615802v2) ) have started to partially use a few common targets, the field lacks a standardized minimal set to objectively compare computational approaches. BenchBB directly addresses this gap.


We selected targets by balancing multiple factors:


-


Novel interfaces: the targets should not be heavily represented in standard ML training datasets.


-


Challenging conformations: the targets have significant conformational changes and thus require varied binding mechanisms.


-


Therapeutic relevance: target should have potential translational impact.


-


Accessibility: ease of recombinant expression, primarily in


*E. coli* , enabling broad lab validation.


To ensure consistent, comparable evaluation across studies, we propose the following standardized assay approach for binding measurements:


-


Use label-free sensing methods such as Bio-Layer Interferometry (BLI) or Surface Plasmon Resonance (SPR) to accurately measure kinetic parameters (kon, koff) and compute affinity constants (KD) from them.


-


Whenever feasible, share assay parameters and conditions (e.g., sensor type, immobilization method, analyte concentration range, and fitting methods) as well as the raw kinetic data.


-


Define a binder or “hit” as having a clearly measurable interaction signal with KD ≤ 10 µM.


So let’s meet the 7 target proteins!


-


This one needs no introduction - it was the catalyst that launched our


[entire competition](https://design.adaptyvbio.com/) . But, briefly, EGFR’s extracellular domain (~620 AA) binds EGF and TGF-α; it is frequently overexpressed or mutated in several cancers; and several therapeutic antibodies (e.g. Cetuximab) target it. PDB ID:


[8HGO](https://www.rcsb.org/3d-view/8HGO) .


-


We have accumulated a solid data set thanks to the participants in both competition rounds. Designers can further expand it or compare their tools or results to the data we


[have](https://github.com/adaptyvbio/egfr_competition_1/)


[released](https://github.com/adaptyvbio/egfr_competition_2/) - this is one of the main reasons to include EGFR.


-


[Cao et al. 2022](https://www.nature.com/articles/s41586-022-04654-9) designed 50–65 aa miniproteins that bound EGFR’s Domain I and Domain III, successfully blocking EGF-induced signaling. They reported, however,


[an 0.01% hit-rate](https://www.nature.com/articles/s41586-022-04654-9/tables/2) . We saw in the


[community paper](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v2) how this was significantly improved upon: almost 3% in Round 1, then 13% in Round 2. And let’s not forget about the


[8.2x binding affinity improvement](https://www.cradle.bio/blog/adaptyv-protein-design-competition) over Cetuximab that Cradle achieved.


-


IL7Ra is the alpha subunit of the IL-7 receptor (CD127), a 219 AA cytokine receptor


[critical for T-cell development](https://www.sciencedirect.com/science/article/abs/pii/S1043466622002587) . PDB ID:


[3DI3](https://www.rcsb.org/structure/3DI3) .


-


Other than its therapeutic relevance (blocking IL-7/IL-7R interaction could modulate immune responses), we chose it because its “e


*ctodomain is easily produced in human cells and has been benchmarked in multiple prior studies”* .


-


[RFdiffusion](https://www.nature.com/articles/s41586-023-06415-8) yielded multiple IL-7Rα binders where earlier Rosetta designs yielded almost none (original ~2.2% with AlphaFold selection to a reported ~34% for RFdiffusion). One designed binder showed nanomolar binding and inhibited IL-7 signaling


*in vitro* .


[Cao et al. 2022](https://www.nature.com/articles/s41586-022-04654-9) reported an 0.05% pre-AlphaFold hit-rate for


*de novo* binders.


[AlphaProteo](https://arxiv.org/abs/2409.08022) also generated strong IL-7Rα binders in one round. All these were


*de novo* mini-proteins (~50–60 AA) that expressed well in


*E. coli* and bound IL-7Rα with high affinity (comparable to or better than a natural IL-7:IL-7R interaction). They report a 24.5% success rate, greater than the remeasured RFdiffusion one (16.8% versus the original 34% published).


-


PL-L1 is an immune checkpoint ligand (~290 AA) expressed on cancer cells and APCs. PD-L1 binds PD-1 on T cells, suppressing immune responses. PDB ID:


[4Z18](https://www.rcsb.org/structure/4Z18) .


-


[Gainza et al. 2023](https://www.nature.com/articles/s41586-023-05993-x#:~:text=Surface%20sites%20presenting%20flat%20structural,To%20test) noted PD-L1’s surface


**** *“displays a flat interface considered to be ‘hard to drug’ by small molecules”,* thus making it ideal for testing advanced design methods. We consider it a “


*de facto binder design benchmark target”*


-


[RFdiffusion](https://www.nature.com/articles/s41586-023-06415-8) reported a 12.6% hit-rate. It was additionally benchmarked by


[AlphaProteo](https://arxiv.org/abs/2409.08022) ,


[MaSIF](https://www.nature.com/articles/s41586-023-05993-x) ,


[BindCraft](https://www.biorxiv.org/content/10.1101/2024.09.30.615802v2) , and


[Yang et al., 2025](https://www.nature.com/articles/s41467-025-57192-z) .


-


BBF-14 is a


*de novo* designed 112-residue β-barrel protein (13.8 kDa) with an internal hydrophobic pore. PDB ID:


[9HAG](https://www.rcsb.org/structure/9HAG) .


-


Serves as a stress-test for binder design on a novel, non-natural target. With BBF-14, there are no evolved binders or known epitopes – designers must rely solely on the computed structure. Thus, it “


*can assess generalization beyond natural interfaces”.*


-


It was previously used as a target in the


[BindCraft paper](https://www.biorxiv.org/content/10.1101/2024.09.30.615802v2) , where one design (“binder4”) bound BBF-14 with KD


*KD* ​ of 20.9 nM (SPR). BindCraft achieved a 55% hit-rate (6/11) on BBF-14.


-


BHRF1 is a viral anti-apoptotic protein from Epstein–Barr virus (EBV) that mimics Bcl-2, allowing infected cells to evade apoptosis.


[It is associated with EBV-linked cancers](https://pubmed.ncbi.nlm.nih.gov/24949974/) . PDB ID:


[2WH6](https://www.rcsb.org/structure/2WH6) .


-


Our main reasons for choosing were that it is “


*easily expressed in E. coli and commercially*
*available with many antibody controls”.* Additionally, it has a known


[hydrophobic hotspot - the BH3-binding cleft](https://pubmed.ncbi.nlm.nih.gov/24949974/) , which restricts the search space.


-


Targeted initially by


[Procko et al. 2014](https://www.cell.com/cell/fulltext/S0092-8674(14)00613-8?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS0092867414006138%3Fshowall%3Dtrue) - their


*de novo* 86 AA minibinder (“BINDI”) could bind BHRF1 with 220 pM affinity (PDB ID:


[4OYD](https://www.rcsb.org/structure/4OYD) ). More recently,


[AlphaProteo](https://arxiv.org/abs/2409.08022) reported an 88% experimental hit-rate, far above prior methods, yielding multiple nanomolar binders without any optimization.


-


MBP is a 42-kDa periplasmic binding protein in


*E. coli* that binds maltose/maltodextrins as part of a sugar transport system. It is very stable and well-expressed; commonly used as an N-terminal fusion solubility tag


[to aid recombinant protein expression](https://pubmed.ncbi.nlm.nih.gov/10452611/) . PDB ID:


[1PEB](https://www.rcsb.org/structure/1PEB) .


-


MBP’s abundance and stability make it easy to produce and assay, thus it can be tested in any lab. Another reasons for choosing it is that it f


*eatures “a well-characterized active site allowing straightforward binder screening via elution from amylose resin”.*


-


[Zhou et al. 2025](https://www.sciencedirect.com/science/article/abs/pii/S0006291X25000361) employed


*de novo* design and computational screening to create MBP binders:


*“6 candidate binders targeting MBP”* were identified without any directed evolution. These hits were small folded proteins (≈80–100 aa) that bound MBP with low micromolar to nanomolar affinity.


-


This RNA-guided DNA endonuclease needs to further introduction - it the key in CRISPR gene editing, widely used in gene editing and any genomic biotechnology application. PDB ID:


[4OO8](https://www.rcsb.org/structure/4OO8) .


-


We chose it for its “


*easy structural characterization via cryoEM; stable, easily expressed in E.*
*coli, and with multiple known binding sites and conformations”.* Cas9’s size and moving parts make binder design difficult, but a successful binder can act as an “off-switch” for genome editing (


*de novo* binders regulating the enzyme function). The


[BindCraft](https://www.biorxiv.org/content/10.1101/2024.09.30.615802v2) authors note including “multi-domain nucleases, such as CRISPR-Cas9” as challenging targets.


-


Used as a target for


[BindCraft](https://www.biorxiv.org/content/10.1101/2024.09.30.615802v2) - where a small designed protein binder (~100 AA) bound Cas9 and inhibited its genome editing activity. Surprisingly, it also yielded a 100% hit-rate, with the best binder measuring a KD


*KD* ​ of 267 nM (SPR).


### Next steps


> Want to read the full paper?
>
>
> **Check it out on [Biorxiv](https://www.biorxiv.org/content/10.1101/2025.04.17.648362v1)** !
>
>
> Want to test your protein-design model on BenchBB?
>
>
> **Go to [benchbb.bio](https://beta.adaptyvbio.com/benchbb) and get started** !
>
>
> Got questions about the paper or BenchBB?
>
>
> **Just email us at benchbb@adaptyvbio.com**


### Acknowledgements


Thanks to all the consortium authors for joining us throughout this journey, their contribution to the paper and in-depth discussions over the benchmark targets!


-


**[Filippo Stocco](https://es.linkedin.com/in/stoccofilippo) , [Noelia Ferruz](https://es.linkedin.com/in/noeliaferruz)** from Centre for Genomic Regulation, Pompeu Fabra University


-


**[Anthony Gitter](https://twitter.com/anthonygitter)** from Department of Biostatistics and Medical Informatics, University of Wisconsin–Madison; Morgridge Institute for Research


-


**[Yoichi Kurumida](https://orcid.org/0000-0003-2696-154X)** from School of Frontier Engineering, Kitasato University


-


**[Lucas de Almeida Machado](https://scholar.google.com.br/citations?user=lPjR0pEAAAAJ&hl=pt-BR)** from Instituto Oswaldo Cruz, Fiocruz


-


**[Francesco Paesani](https://scholar.google.com/citations?user=01BwypIAAAAJ) , [Cianna N. Calia](https://www.linkedin.com/in/cianna-calia-092aa7296)** from Department of Chemistry and Biochemistry, University of California San Diego


-


**[Chance A. Challacombe](https://www.linkedin.com/in/chance-challacombe-541aa6240) , [Nikhil Haas](https://www.linkedin.com/in/nikhilhaas) , [Ahmad Qamar](https://www.linkedin.com/in/atqamar)** ** from BioLM


-


**[Bruno E. Correia](https://ch.linkedin.com/in/bruno-correia-23a1aa4) , [Martin Pacesa](https://ch.linkedin.com/in/martin-pacesa-5b6670100) , [Lennart Nickel](https://de.linkedin.com/in/lennart-nickel-aba84a203)** from École Polytechnique Fédérale de Lausanne (EPFL)


-


**[Maxwell J. Campbell](https://maxc.codes/)** from Hearth Industries


-


**[Constance Ferragu](https://ch.linkedin.com/in/constance-ferragu) , [Patrick Kidger](https://kidger.site/)** from Cradle Bio


-


**[Logan Hallee](https://www.linkedin.com/in/logan-hallee)** from Synthyra; Center for Bioinformatics & Computational Biology, University of Delaware


-


**[Christopher W. Wood](https://scholar.google.co.uk/citations?user=uarP_xQAAAAJ&hl=en) , [Michael J. Stam](https://scholar.google.co.uk/citations?user=PCIGd48AAAAJ&hl=en) , [Tadas Kluonis](https://uk.linkedin.com/in/tadas-kluonis) , [Kartic Subr](https://scholar.google.com/citations?user=6FYGyMQAAAAJ&hl=en) , [Süleyman Mert Ünal](https://www.linkedin.com/in/mert-unal-05783019a/?originalSubdomain=tr) , [Leonardo Castorina](https://uk.linkedin.com/in/leonardo-castorina)** from University of Edinburgh


-


**[Elian Belot](https://www.linkedin.com/in/elian-belot/)**


-


**[Alexander Naka](https://www.linkedin.com/in/alex-naka/)** from Science Corporation
