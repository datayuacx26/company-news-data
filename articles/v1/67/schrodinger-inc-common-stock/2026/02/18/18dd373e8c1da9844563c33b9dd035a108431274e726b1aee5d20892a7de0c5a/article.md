---
schema_version: "1.0.0"
document_id: "18dd373e8c1da9844563c33b9dd035a108431274e726b1aee5d20892a7de0c5a"
company_key: "schrodinger-inc-common-stock"
company: "Schrodinger Inc."
source_id: "schrodinger-inc-common-stock-rss-d7bf526e9e3e"
canonical_url: "https://extrapolations.com/how-innovations-in-virtual-screening-are-revolutionizing-drug-discovery/"
published_at: "2026-02-02T14:09:00+00:00"
first_seen_at: "2026-07-20T23:18:31.059214+00:00"
fetched_at: "2026-07-28T22:25:05.780563+00:00"
content_hash: "sha256:334ed99ca99917342b26b3e4dd4579f5db3f0af413bdb338aff7e6564a0ad095"
---

# How Innovations in Virtual Screening are Revolutionizing Drug Discovery

[Life Science](https://extrapolations.com/category/life-science/)


# How Innovations in Virtual Screening are Revolutionizing Drug Discovery


BY Steven Jerome, Ph.D.


& Paraskevi Gkeka, Ph.D.


Feb 2, 2026


Finding new medicines is traditionally slow, costly, and uncertain. However, that paradigm is changing rapidly thanks to new technologies, including virtual screening, where drug hunters use computation to search massive libraries of potential drug molecules and predict which ones might work — before ever stepping into a lab.


While this technique has been around for years, new breakthroughs have supercharged its power. Today’s tools can search billions of make-on-demand compounds using machine learning, advanced simulations, and detailed 3D protein models. This means scientists can explore more ideas and find better quality molecules much more efficiently than ever before. These advances could lead to better, safer treatments that reach patients sooner.


In a new perspective published in the


*Journal of Medicinal Chemistry* , industry experts share how these technologies are reshaping drug discovery and opening doors to treat diseases once thought undruggable. We sat down with two co-authors of the paper — Steven Jerome, Executive Director at Schrödinger and Paraskevi Gkeka, Group Leader of CADD France, Integrated Drug Discovery at Sanofi — to explore how the age of trial-and-error is giving way to a smarter, more computationally-driven approach to drug discovery.


**What motivated you to co-author this perspective on virtual screening now?**


This perspective emerged from a panel discussion we had on virtual screening at the 21st Schrödinger European User Group Meeting in Lisbon. During the discussion, it was clear that the panelists, along with many audience members, each had valuable insights to share from their own projects — both failures and successes. We decided to pull all of these learnings together and share them with the wider community of chemists and modelers.


**From your perspectives, what’s the single most transformative innovation in computational hit-finding over the last five years?**


While breakthroughs such as AlphaFold2 and other deep-learning methods hold tremendous promise to shake up the computational hit finding landscape, their potential has yet to be fully realized on real-world drug discovery projects. However, the emergence of ultra-large libraries for screening have completely changed the landscape of hit finding. Not that long ago a large campaign might screen a million compounds. Now, billions are truly state of the art.


The application of active learning approaches to structure-based approaches, e.g. docking, FEP, as well as the exponential size increase of the available virtual chemical spaces are definitely the two of the most impactful innovations of the last five years.


**The paper highlights ultra-large libraries. Do you believe “bigger” will always mean “better” when it comes to chemical space?**


We’ve come to realize that large numbers alone don’t guarantee success. Different vendor libraries have different characteristics that can impact the random hit rate (fraction of molecules in the library that will be active), and consequently have enormous influence on the outcome of the hit finding campaign. If you’re seeking Type II kinase inhibitors, for example, checking first for known Type II molecules in the library and highly-similar analogues is a great way to choose a good library to screen. Pooling different libraries together and filtering them based on specific criteria — so that you’re only screening molecules you’d actually consider buying — is a great way to boost both diversity and the hit rate.


Bigger does not necessarily mean better. In fact, hands-on experience has shown us that tailored pre-filtering is a great way to ensure a project benefits most from the available ultra-large libraries.


**Can you walk us through an example where machine learning meaningfully improved a hit-finding campaign?**


Now that libraries have reached well into the billions of compounds, docking each compound has become cost-prohibitive. How do you find compounds with the best docking scores if you can’t dock them all? That’s where state-of-the art sampling algorithms like active learning are critical. Active Learning docking (AL-Glide) has allowed us to find hits in massive libraries that did not exist in smaller ones.


We recently screened the full Wuxi Galaxy and Enamine Real virtual libraries (a total of more than 36 billion molecules), using the 1D similarity approach proposed by


[Merz et al.](https://pubs.acs.org/doi/abs/10.1021/jm010137f) that was recently incorporated into the Schrödinger Suite. Restrained docking based on the project’s front-runner interactions was then used to filter the similar molecules. This process was followed by AL-ABFEP with further filtering based on known DG values for the specific target and known binders. Using active learning was really a game changer because without it, it would have been prohibitively expensive and slow to screen the millions of molecules coming out from the previous filtering step, i.e. restrained docking. With this pipeline, we managed to go from 36 billion molecules to a few hundred molecules that were purchased. We have identified several confirmed binders and managed to get a co-crystal structure for one of them, which we will try to optimize to improve its activity.


**How does the landscape of virtual screening tools look for non-protein targets like RNA or DNA?**


Nucleic acids as targets for small molecule drug discovery is an exciting and emerging area. Most modeling tools were developed for protein targets and perform poorly or possibly don’t run at all on nucleic acids. Recently, we published a preprint on customizing our core hit finding tools like Glide and SiteMap for RNA targets. Because the physical properties of an RNA binding site are much different than that of a protein (highly flexible, highly charged), it was necessary to reparameterize Glide and SiteMap to get better performance on RNAs. We’ve also made it possible to run ABFEP (absolute FEP+) calculations on RNAs, which was not possible before.


RNA targeting with small molecules is potentially a game changer for drug discovery. However, the lack of structures and related binding affinity and activity data greatly limit the evaluation of existing structure-based virtual screening (SBVS) tools that were originally developed for proteins, as well as the development of new tools. Moreover, ligand-based virtual screening approaches when a hit is known are limited by the lack of a clear view of the chemical space of RNA targeting molecules.


**How do you see virtual screening fitting alongside experimental approaches such as fragment screening or DNA-encoded library (DEL) screening?**


We expect adoption of structure-based virtual screening to continue as companies like Schrödinger and others lower the barriers of running high-quality screens. Experimental methods will likely continue to play an important role, especially for targets without available structures.


SBVS will always have a place in drug discovery. The key reason for this is that such approaches provide a microscopic view of the systems of interest and their results are interpretable. The same is true for ligand-based virtual screening algorithms, especially the ones that include 3D conformational information of the ligands.


**What strategies do you recommend for tackling difficult or undrugged targets that lack high-quality structural data?**


At the moment, structure-based modeling for these types of targets is inherently more difficult than those with ligand structures. We would recommend using a diversity of methods for generating starting structures, including homology models and various co-folding methods, varying parameters to generate many models. Finally, physics-based refinement of these models is critical for their use in predictive modeling. We’ve seen encouraging examples of IFD-MD refinement restoring good virtual screening performance to AlphaFold2 structures.


In the case of limited or a complete lack of structural data, homology modeling, AlphaFold2/AlphaFold3, Openfold, Boltz2, Rosetta or other AI structure prediction tools offer great alternatives. A good practice is to construct several models with different parameters and thoroughly study them before initiating a virtual screening campaign. Due to the underlying uncertainty of the predictions, the models should ideally be studied by MD simulations, or even AI generative methods like normalizing flows or diffusion maps.


**What do you think remains the most under-appreciated challenge in structure-based virtual screening?**


Despite technological breakthroughs, working with large libraries is still enormously complex and requires access to massive compute power. Even similarity searches on large databases require specialized cloud databases in order to be practical.


Additionally, capturing the full conformational landscape with the related free energies remains a difficult task, particularly for large biomolecules, which are the common targets in drug discovery. Despite the plethora of techniques for intelligent biasing or for ML generation of conformations, the accuracy of results is often hampered by the inaccuracies of the force fields used. Understanding the dynamical behavior and interpreting this behavior as a mode of action of a biomolecule is important, especially in a pharmaceutical context, not only for research purposes, but also during the development of a drug.


**How should scientists outside of computational chemistry think about synthetic tractability when reviewing virtual screening hits?**


Synthetic feasibility tools seem to be most reliable when they actually perform retrosynthesis. This is an area Schrödinger and many others are working on, but the existing solutions are far from perfect. Thankfully, in hit discovery, there are multiple suppliers of billion+ libraries with better than 80 percent success rate in synthesis.


*In silico* tools work quite well, but we often must resort to alternatives, e.g. identification of close analogs, in the cases where a molecule is not synthetically tractable.


**Looking ahead, what’s one trend or technology you’re keeping a close eye on that could reshape hit discovery in the next 3–5 years?**


We are closely tracking new developments in the area of protein structure prediction that can more accurately represent the conformational ensemble of target proteins. Currently, the most interesting technique in this area leverages a combination of physics and ML approaches.


We’re also excited about the use of large language models for proteins and RNA applied to different tasks, as well as serving as agents for project decision making.


Steven Jerome, Ph.D.


Dr. Steven Jerome completed his Ph.D. at Columbia University in the Chemistry Department under the supervision of Richard Friesner. While at Columbia, he contributed to tools for molecular docking and protein structure refinement. After Columbia, he joined Schrödinger as a scientific developer working on the Glide team, before transitioning to product management. He has since advanced through several roles at Schrödinger to his current dual role as Executive Director of Hit Discovery, directing the development of a broad portfolio of state-of-the-art computational tools for small molecule hit identification and head of EU+ Application Science, supporting greater adoption of the Schrödinger platform within Europe.


Paraskevi Gkeka, Ph.D.


Dr. Paraskevi Gkeka holds an M.Eng. in Applied Mathematics, an M.Sc. in Mathematical Modeling, and a PhD in Chemical Engineering from the University of Edinburgh, where she studied the interactions of peptides and nanoparticles with membranes using coarse-grained models. Following postdoctoral research in computer-aided drug design and molecular modeling, she joined Sanofi in 2016 as a Principal Scientist, contributing to the development of AI-driven enhanced sampling algorithms, binding free energy calculations, and machine learning applications in drug discovery. She has since advanced to Group Head of SMIRNA CADD (Small Molecules Interfering with RNA), where she leads AI-enhanced drug discovery initiatives with a focus on library design and molecular dynamics simulations.


### Sign Up Today


Sign up to receive quarterly updates with the latest from Extrapolations.


### Up Next


[Life Science](https://extrapolations.com/category/life-science/)[デジタルデザインはどのように抗ウイルス薬開発を変革しているのか 新型コロナウイルス感染症（COVID-19）が世界的なパンデミックとして出現した際、世界は前例のないスピードでワクチンと治療薬の開発・供給に尽力しました。この偉業は何百万もの命を救い、世界的な科学協力の力を証明しました。しかし、SARS-CoV-2ウイルスは進化を続けており、パンデミックを引き起こす可能性のある他のコロナウイルスも依然として懸念材料となっています。 BY 岡部 篤俊 , 落田 温子博士 & アバ・E・レフラー博士](https://extrapolations.com/%e3%83%87%e3%82%b8%e3%82%bf%e3%83%ab%e3%83%87%e3%82%b6%e3%82%a4%e3%83%b3%e3%81%af%e3%81%a9%e3%81%ae%e3%82%88%e3%81%86%e3%81%ab%e6%8a%97%e3%82%a6%e3%82%a4%e3%83%ab%e3%82%b9%e8%96%ac%e9%96%8b%e7%99%ba/)
