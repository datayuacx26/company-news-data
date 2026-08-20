---
schema_version: "1.0.0"
document_id: "a7bb43abfa34443dce37779c895ab68f3b52bb0f9f7068afa9d67ec9fd22bcf9"
company_key: "schrodinger-inc-common-stock"
company: "Schrodinger Inc."
source_id: "schrodinger-inc-common-stock-rss-d7bf526e9e3e"
canonical_url: "https://extrapolations.com/integrating-physics-based-modeling-and-machine-learning-to-advance-virtual-screening/"
published_at: "2025-11-24T17:47:38+00:00"
first_seen_at: "2026-07-20T23:18:31.059214+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:ccbbaa5b76ff54d4b925d02ebb4f365013e40e43be323ec9dfb11a7701d71275"
---

# Integrating Physics-Based Modeling and Machine Learning to Advance Virtual Screening

[Education](https://extrapolations.com/category/education/)


# Integrating Physics-Based Modeling and Machine Learning to Advance Virtual Screening


BY Schrödinger Editorial Team


Nov 24, 2025


*This is the final blog of a three-part series highlighting essential computational chemistry topics covered in the newly released*[Schrödinger Online Course bundle](https://www.schrodinger.com/life-science/learn/education/courses/hitdiscoverybundle/?__hstc=57782773.388b31a5af94c4afbb2ae01a0cdef66d.1739827563809.1763471823406.1763663063697.91&__hssc=57782773.1.1763663063697&__hsfp=1750239613) *. Read on to learn how to combine multiple virtual screening techniques to create a discovery strategy with tangible experimental impact.*


Finding new medicines has traditionally been a slow, costly, and uncertain process defined by trial-and-error. However, this paradigm is changing rapidly thanks to new technologies and workflows, including virtual screening, where drug hunters use computation to search massive libraries of molecules and predict their viability as a hit before entering the lab.


Virtual screening is a foundational early-stage triage method in hit discovery. It enables the efficient evaluation of millions, and sometimes billions of synthetically accessible compounds by applying a funnel-like workflow. This funnel often uses progressively more rigorous and discriminative computational techniques at each stage, as illustrated in the simplified figure below.


*The ultimate goal of virtual screening is to deliver hits that combine potency, novelty, and a realistic path to development.*


Rather than a “one size fits all” process, virtual screening is modular, incorporating a series of independent components and complementary methods that can be easily combined, interchanged, and reused to fit a project’s needs.


Physics-based methods continue to be the backbone of virtual screening. However, while these methods are incredibly accurate, they can be slow and don’t easily scale larger purchasable ligand libraries due to high compute requirements. By integrating AI/machine learning (ML) workflows into virtual screens, these highly accurate physics-based screening methods can be scaled dramatically, enabling the rapid evaluation of billions of potential hits.


**Physics-Based Foundations**


“Physics-based” methods refers to techniques such as


[Shape Screening](https://www.schrodinger.com/platform/products/shape-screening/) , ligand docking with


[Glide](https://www.schrodinger.com/platform/products/glide/) ,


[Glide WS](https://www.schrodinger.com/life-science/learn/white-paper/20-years-of-glide-a-legacy-of-docking-innovation-and-the-next-frontier-with-glide-ws/) and Absolute Binding Free Energy Perturbation (


[ABFEP+](https://www.schrodinger.com/life-science/learn/white-papers/dramatically-improving-hit-rates-modern-virtual-screening-workflow/) ).


Shape screening is an example of a ligand-based virtual screening technique that is typically implemented early, at the very top of the funnel. In shape screening, spheres are generated to represent the shape of active reference molecules. Ligands are then scored based on the quality of the shape overlap against reference ligands.


The


[Phase](https://www.schrodinger.com/platform/products/phase/) product within Schrödiner’s platform adds additional rigor to the shape screening method by projecting any atom type or pharmacophore feature found within a sphere onto the sphere as a color. Combining these ligand-based methods allows you to filter by key features of known actives, while ensuring candidates fit into a desired volume in the binding site. Together, they improve the relevance of proposed hits while still preserving diversity in the library.


Ligand docking is a structure-based technique that treats the protein as static, and takes into account protein-ligand interactions in the binding site when scoring and producing a ligand pose. Ligand docking is typically used to eliminate ligands that will clearly not bind.


[Glide WS](https://www.schrodinger.com/life-science/learn/white-paper/20-years-of-glide-a-legacy-of-docking-innovation-and-the-next-frontier-with-glide-ws/) is an advanced screening tool within Schrödinger’s industry-leading docking method,


[Glide](https://www.schrodinger.com/platform/products/glide/) . Glide WS incorporates


[WaterMap](https://www.schrodinger.com/platform/products/watermap/) to account for the precise role of water molecules in binding, Molecular Mechanics with Generalized Born and Surface Area,


[MM-GBSA](https://www.schrodinger.com/platform/products/prime/) , to evaluate the delocalized strain energies, and a robust scoring function calibrated by free energy perturbation (


[FEP+](https://www.schrodinger.com/platform/products/fep/?utm_source=google&utm_medium=cpc&utm_campaign=dgtl-ls-prod-fep&utm_content=text+ad&utm_term=fep%2B&utm_campaign=dgtl-ls-prod-fep&utm_source=adwords&utm_medium=ppc&hsa_acc=2584137415&hsa_cam=21698542972&hsa_grp=167310844557&hsa_ad=713306080283&hsa_src=g&hsa_tgt=kwd-319497079856&hsa_kw=fep%2B&hsa_mt=b&hsa_net=adwords&hsa_ver=3&gad_source=1&gad_campaignid=21698542972&gbraid=0AAAAAoiC9DOfMwBgNML6VwfB3JCRERVOq&gclid=CjwKCAiAuIDJBhBoEiwAxhgyFqhFfU-LMr3eSp_Eij4luuIhXcktS5LQG8eNGaEUG9IOau32QS65AhoCFXcQAvD_BwE) ) technology. Although Glide WS requires more time to run, it provides more accurate pose prediction and scoring. Glide WS can be used as a rescoring filter for your best candidates, or to predict accurate input poses for FEP+ methods.


Our final example of a highly accurate physics-based screening method is Absolute Binding Free Energy Perturbation, or ABFEP+. ABFEP+ is particularly powerful because it calculates absolute binding free energies without requiring a known reference ligand. This capability enables extrapolation beyond existing chemical matter, making it well suited for identifying novel chemotypes and scaffolds. While these simulations provide highly reliable predictions, their computational expense can limit throughput. Running a single ABFEP+ calculation often takes several hours and does not scale well. This is where AI/ML has had a significant impact.


**Scalable Screening Campaigns through Active Learning**


Active learning is an implementation of ML in a virtual screening cascade. Active learning introduces adaptability to the screening process. Rather than evaluating every compound in a library, the algorithm strategically selects only the most informative compounds. It does this by balancing two goals: quickly pursuing the best predicted drug-like molecules (exploitation) while also deliberately exploring chemically unique or uncertain molecules (diverse exploration).


In practice, three rounds of active learning often provide the most efficient trade-off between model accuracy and computational cost. After each round, the model is retrained on new results, continually improving its predictive performance. ML models can also be re-run when additional data becomes available or when project goals shift, keeping the screening process both current and context-aware.


This iterative cycle enhances efficiency, reduces false positives, and enables teams to identify promising hits or fragments with fewer calculations while maintaining confidence in the results. Active-learning Glide calculations can now feasibly scale into the millions while Active Learning ABFEP++ calculations can scale to the hundreds of thousands.


Active learning does not replace physics-based rigor, it amplifies it. Physics-based simulations supply data, or scores that enable effective model training. This integration allows the model to rapidly extrapolate highly accurate predictions across billions of novel compounds, significantly accelerating the entire screening process while maintaining quality.


**Building a Robust Screening Workflow**


Effective virtual screening requires a well-structured workflow sequence that reflects project-specific objectives. For instance, while chemical diversity can be important for discovering new scaffolds, it is not always the exclusive goal. Campaigns often choose to also pursue the chemical space of known actives in parallel. Regardless of strategy, robust screening depends on:


- Reliable target enablement and structure preparation


- Thoughtful library design reflecting project priorities


- Accurate scoring and pose prediction for meaningful rank ordering


- Clustering and post-processing to identify unique, actionable candidates


- Feasibility considerations to ensure that final hits are purchasable or synthetically accessible


When combined thoughtfully, these elements transform screening from purely a computational exercise into a discovery strategy with tangible experimental impact.


Virtual screening is most powerful when it reveals something new, like expanding our view of what is possible, rather than merely filtering what already exists. Achieving that goal requires the full integration of predictive modeling, careful planning, and collaboration across disciplines. The result is not only faster hit identification, but also a more complete understanding of the binding site and overall system.


**Next Steps**


If you are interested in diving deeper, Schrödinger has recently launched an online certification course,


[Virtual Screening with Integrated Physics and Machine Learning](https://www.schrodinger.com/life-science/learn/education/courses/virtualscreening/) , which combines in-depth learning with hands-on exercises to explore all aspects of virtual screening.


Schrödinger Editorial Team


### Sign Up Today


Sign up to receive quarterly updates with the latest from Extrapolations.


### Up Next


[Education](https://extrapolations.com/category/education/)[Enabling Targets for Structure-Based Drug Discovery Scientists rely on experimental and computational techniques to reveal, refine, and validate molecular structures, ensuring targets are ready for structure-based modeling and enabling the translation of molecular insight into new medicines. BY Schrödinger Editorial Team](https://extrapolations.com/enabling-targets-for-structure-based-drug-discovery/)
