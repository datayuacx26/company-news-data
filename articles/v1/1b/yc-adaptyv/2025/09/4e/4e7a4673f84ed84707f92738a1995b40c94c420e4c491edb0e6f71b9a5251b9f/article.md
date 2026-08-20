---
schema_version: "1.0.0"
document_id: "4e7a4673f84ed84707f92738a1995b40c94c420e4c491edb0e6f71b9a5251b9f"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-news-import-3c8571c35bcb"
canonical_url: "https://www.adaptyvbio.com/blog/mbp"
published_at: "2025-09-01T00:00:00+00:00"
first_seen_at: "2026-07-24T14:21:15.816567+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:caa923c4d2857e8f6382d728e923aa3d4990e096512a66d8f1efd1b6eb0eec88"
---

# Case study: Designing modular protein sensors using BindCraft

/


TL;DR


- -


AI models like BindCraft make it possible to design novel protein binders, conditioned only on the target’s structure and a few hotspot residues.


- -


We’re presenting two de novo biosensors we have designed with BindCraft


#### What do we mean by conditional binders and biosensors?


In simple terms, a conditional binder binds to its target only in the presence of another molecule, be that a protein, nucleic acid, small molecule. If you couple this conditional binding to a reporter assay (e.g., link the binder and target to parts of a split green fluorescent protein - a splitGFP system), then you have a cheap and easy way to measure the presence of your inducer molecule.


#### Why and how can we make binders conditional?


Up until now, most of the binders only had one task - to bind. Irrespective of environmental conditions (e.g., temperature, pH), introduction of other molecules, external triggers or energy sources (light -


[optogenetic](https://en.wikipedia.org/wiki/Optogenetics)


switch), designed binders would always bind to their target the exact same way - unless that target was completely denatured at 90


**°**


C.


*But what if we want a finer control over binding*


.


**First strategy: target a conformational change!**


Extending conventional design algorithms to conditional protein binding where protein-protein interactions depend specifically on the presence of small molecules would allow to vastly extend the use cases and opens up many avenues in synthetic biology. But most tools cannot be conditioned on external factors: they only see the target and its epitope. We now arrive to our first design strategy: we assume the environment change induces a large change in the target, something that would expose an epitope not seen in the normal/unperturbed state. For small molecules, we have the maltose binding protein MBP: crystal structures of its bound and unbound states reveal a significant conformational shift, which we can target with our design algorithm. The advantage of this strategy is straight-forward: the design algorithm doesn’t need to know anything about the small molecule! We used BindCraft as our primary binder design algorithm and a custom script for selecting its target hotspots.


**Second strategy: ligand-induced binding =**


[molecular glues](https://en.wikipedia.org/wiki/Molecular_glue)


**!**


Stay tuned for the next blog post for this method!


***Now, onto the worfklow and results!***


#### Case Study: Maltose-induced protein binding


Maltose-Binding Protein (MBP) from


*E. coli*


(Uniprot ID P0AEX9) is well-characterized for undergoing significant conformational changes upon binding of the disaccharide maltose, transitioning from an open often termed


*“apo”*


to a closed structure often termed


*“holo”*


. This, in turn, exposes a site on the opposite site of where maltose binds (you can see it in the animation below), that we will target with


[BindCraft](https://www.nature.com/articles/s41586-025-09429-6)


. We show this conformational change and the 4 exposed epitopes in the animation below:


We aimed to design proteins specifically targeting the maltose-bound conformation therefore designing a system showing binding only in presence of the ligand.


Design strategy:


To identify hotspots for subsequent binder design the


[solvent accessible surface area (SASA)](https://en.wikipedia.org/wiki/Accessible_surface_area)


and hydrophobicity was calculated for both the bound and unbound states, with the SASA difference between bound and unbound indicating the most accessible regions when maltose is present.


Targeting hydrophobic patches can


[increase the likelihood](https://www.nature.com/articles/s41586-022-04654-9)


of sampling a binder with


*de novo*


algorithms. We then calculated a “hotspot” score using these 2 metrics and selected the top 4 best scoring residues to target for binder design. We visually checked the hotspots in


[ChimeraX](https://www.google.com/search?q=chimerax%C2%A0&client=safari&sca_esv=ff72e1d4abb98f8c&rls=en&ei=T9CxaNq2HJ397_UP1JXj0QQ&ved=0ahUKEwiamsLGs7CPAxWd_rsIHdTKOEoQ4dUDCBE&uact=5&oq=chimerax%C2%A0&gs_lp=Egxnd3Mtd2l6LXNlcnAiCmNoaW1lcmF4wqAyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAEMgUQABiABDIFEAAYgAQyBRAAGIAESOYKULMCWIIJcAF4AJABAJgBX6ABxwSqAQE4uAEDyAEA-AEBmAIEoALdAcICChAAGLADGNYEGEfCAgYQABgWGB7CAggQABiABBiiBMICBRAAGO8FmAMAiAYBkAYFkgcBNKAHixSyBwEzuAfXAcIHAzItNMgHEw&sclient=gws-wiz-serp)


.


Computational generation:


We used our in-house, Rosetta-free BindCraft version with the Default trajectory generation option, only ranging the inter-protein contacts weights (we deviated from the standard 1.0 by adding 0.1 and 0.5, biasing our designs to a smaller number of contacts that would only concentrate binding at the hotspots, avoiding any unwanted non-specific interaction). This way, we only target an exposed epitope when maltose is bound to MBP.


Experimental validation:


Using our


[Bio-layer Interferometry (BLI)](https://docs.adaptyvbio.com/docs/experiment-types/affinity-characterization)


workflow in presence and absence of maltose we were able to measure their affinity for both conformations of maltose-binding protein and hence determine the maltose specific nature of the binding. The designed sequences were expressed and immobilized on a functionalized glass surface and dipped into maltose bound or unbound MBP to obtain precise kinetics measurements.


This assay allowed to identify two designs where maltose increased the binding affinity by several orders of magnitude. You can see them in the sensogram plots above. These designs termed n°19 and n°33 with


K_D


s in the micromolar range in absence of maltose both showed tens of nanomolar affinity in presence of maltose. In both cases maltose caused an orders of magnitude increase in the affinity of the designs to MBP showcasing the ligand specificity of our designs. The affinity values are given below:


#### Practical applications: conditional binding for biosensing and molecular switches


To demonstrate practical utility, our designs were coupled with easily detectable reporter systems allowing


to complement the precise BLI readout with low resource qualitative measurement techniques allowing to transform the previously designed proteins into functioning biosensors.


To achieve this, we employed systems known as


[Protein Complementation Assays (PCA)](https://en.wikipedia.org/wiki/Protein-fragment_complementation_assay)


. These rely on two protein fragments with minimal intrinsic affinity for one another, which regain enzymatic activity only when brought into close proximity - in our case as a result of the binding event - thereby enabling functional dimerization. For a biosensor application this enzymatic activity should be easily measurable to act as a readout for small-molecule presence


To achieve this, we focused on a


[split β-lactamase colorimetric reporter system](https://pubmed.ncbi.nlm.nih.gov/12042868/)


- a divided enzyme that regains activity upon binding-induced reconstitution, converting a colorimetric substrate and producing a visible color change that can be easily interpreted by eye or quantified using absorbance measurements.


The mechanism is described schematically below


Below we show the visual readout possible by fusing our previously showcased anti-maltose · maltose binding protein binders fused to split beta β-lactamase fragments and a quantitative measurement of the ligand specific activity of the reconstituted enzyme. As the two proteins bind together in presence of the ligand, the fragments fuse together and gain enzymatic activity changing the substrate color from yellow to red.


#### Designed proteins on demand


Our case studies demonstrate that computationally designed, conditionally active protein interactions are not just theoretical; they have immediate applications in creating precise biosensors and molecular switches. These proteins can be integrated into diverse fields, from medical diagnostics and therapeutics to environmental monitoring, providing highly targeted, controllable biological tools.


We showed that t


ools like BindCraft


can be used to design ligand specific binders for arbitrary protein targets. Combined with the low-latency automated lab at Adaptyv, we were able to go from design → test → application quite fast,


[and so can you!](https://www.adaptyvbio.com/)


Also check out the original BindCraft paper, now published in


[Nature](https://www.nature.com/articles/s41586-025-09429-6)


, and their super user-friendly


[codebase](https://github.com/martinpacesa/BindCraft)


!
