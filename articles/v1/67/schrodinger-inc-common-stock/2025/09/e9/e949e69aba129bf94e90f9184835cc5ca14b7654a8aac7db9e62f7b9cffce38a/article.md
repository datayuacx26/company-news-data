---
schema_version: "1.0.0"
document_id: "e949e69aba129bf94e90f9184835cc5ca14b7654a8aac7db9e62f7b9cffce38a"
company_key: "schrodinger-inc-common-stock"
company: "Schrodinger Inc."
source_id: "schrodinger-inc-common-stock-rss-d7bf526e9e3e"
canonical_url: "https://extrapolations.com/cracking-the-selectivity-challenge-in-kinase-drug-discovery/"
published_at: "2025-09-09T15:06:34+00:00"
first_seen_at: "2026-07-20T23:18:31.059214+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:1ebca9d0139adfb95647d53c28267f1db7071f7d9b9f345ed3fa3b3b491bebb3"
---

# Cracking the Selectivity Challenge in Kinase Drug Discovery

[Life Science](https://extrapolations.com/category/life-science/)


# Cracking the Selectivity Challenge in Kinase Drug Discovery


BY Aleksey I. Gerasyuto, Ph.D.


& Jennifer Knight, Ph.D.


Sep 9, 2025


In drug discovery, pivotal advances often emerge not solely from success, but from confronting significant obstacles. This was the case in our Wee1 inhibitor program. Initial studies using our platform identified multiple novel chemical series with strong activity against Wee1, a kinase critical to cell cycle regulation. However, comprehensive profiling revealed a substantial challenge: the compounds exhibited activity across numerous kinases, demonstrating a lack of selectivity. This promiscuity posed a serious liability. Without a clear path to overcoming this problem, we risked investing time and resources into compounds that might not be suitable for therapeutic development.


Faced with this challenge, we explored whether physics-based computational modeling could be leveraged to prospectively predict — and ultimately mitigate — selectivity issues prior to synthesizing and experimentally testing large numbers of compounds.


**The Kinase Selectivity Challenge**


Designing selective kinase inhibitors is challenging because the ATP-binding site, where many drugs and ATP both bind, is very similar across hundreds of kinases. While experimental testing of many compounds across hundreds of kinases is feasible, it is resource-intensive and time-consuming. Machine learning methods offer some assistance prior to synthesis, but their reliance on incomplete training data limits their ability to reliably extrapolate to novel chemical space.


What we needed was a better way to predict selectivity computationally — one that could be applied prospectively at scale, before we invested heavily in synthesis and assays.


**The Method: Pairing Ligand and Protein FEP**


In our recent


*Nature Communications* paper,


[Harnessing free energy calculations for kinome-wide selectivity in drug discovery campaigns with a Wee1 case study](https://rdcu.be/eCwtc) , we describe a novel computational strategy that combines two types of free energy calculations available in Schrödinger’s platform:


- **Ligand-based relative binding free energy (L-RB-FEP+)** : Predicts how strongly a new compound binds to Wee1 compared to reference compounds, while also estimating potency against specific off-target like PLK1.


- **Protein residue mutation free energy (PRM-FEP+)** : Predicts how swapping a single amino acid or “selectivity handle” in the binding pocket — such as Wee1’s unusual asparagine “gatekeeper” residue — affects binding. This allows researchers to extrapolate selectivity across the entire kinome without modeling each kinase individually.


In other words, L-RB-FEP+ tells us which compounds are potent and PRM-FEP+ tells us which compounds are selective. Together, they form a powerful toolkit for navigating one of the toughest challenges in kinase drug discovery.


**The Results: From Billions of Ideas to a Selective Candidate**


Using this approach, we computationally explored 445 million design ideas and narrowed them down to just 42 compounds worth synthesizing in the lab — a funnel that would have been unimaginable without computation.


The results were striking:


- **Multiple novel scaffolds** with nanomolar potency against Wee1


- Up to


**1,000-fold selectivity over PLK1** , a critical off-target


- **Three distinct chemical series** optimized for both potency and selectivity


- **Accuracy** in predicting kinome-wide selectivity patterns, later validated in broad experimental panels


Even more importantly, these predictions gave us the confidence to pursue complex chemical syntheses for promising compounds we might otherwise have abandoned. In fact, this strategy directly enabled the design of our clinical candidate that wouldn’t exist today without these advanced computational tools.


**The Bigger Picture**


This is the first time PRM-FEP+ has been used prospectively to model kinome-wide selectivity, and it worked. Not only did it rescue a drug discovery program that was on the verge of shutdown, but it also points to a new way forward for the entire field.


The implications extend beyond kinases. Any protein family with conserved binding sites but subtle sequence differences, including GPCRs, nuclear hormone receptors, and ion channels, could potentially benefit from this method. By reducing the need for compound synthesis and exhaustive experimental panels and de-risking programs earlier, we can accelerate the path to higher-quality, more selective drugs with a greater chance of clinical success.


**Turning Insight into Action**


Selectivity is one of the hardest problems in drug discovery, and one of the biggest barriers to developing safe, effective medicines. With this work, we’ve shown that physics-based free energy methods can be a game-changer for solving selectivity issues — not just retrospectively, but in real time, guiding critical program decisions.


If you’re already using Schrödinger’s platform and struggling with selectivity in your own project, we encourage you to identify a selectivity handle and explore this combined FEP strategy. If you’re not yet a customer but want to learn more, request a demo or reach out to discuss how these tools could help your team.


Drug discovery is full of roadblocks. The real breakthroughs come when we find smarter ways to get past them.


Aleksey I. Gerasyuto, Ph.D.


Dr. Gerasyuto is a seasoned drug hunter with extensive experience in computationally-driven drug discovery. In his current role at Schrödinger as vice president, Drug Discovery, Dr. Gerasyuto is responsible for medicinal chemistry of the early stage portfolio and co-leads multidisciplinary efforts to shape the drug discovery team’s internal and collaborative pipeline. He has contributed to the establishment of several successful and productive collaborations including with Morphic Therapeutic, Petra Pharma, BMS and Zai Lab. Prior to joining Schrödinger, Dr. Gerasyuto worked at PTC Therapeutics, one of the pioneers of targeting RNA biology with small molecules. Dr. Gerasyuto contributed to the discovery of several clinical candidates including PTC518 for Huntington disease (Phase 2), MORF-057 for ulcerative colitis (Phase 2) and LOXO-783 for breast cancer and other solid tumors (Phase 1). Dr. Gerasyuto received his Ph.D. from University of Wisconsin, Madison and B.S./M.S. from Higher Chemical College of Russian Academy of Sciences.


Jennifer Knight, Ph.D.


Jen Knight is a director in Schrödinger’s therapeutics group. She has been at Schrödinger since 2012 and has been a modeling lead on internal projects and collaborations. She specializes in free-energy methods, LiveDesign workflow optimization and machine learning applications.


### Sign Up Today


Sign up to receive quarterly updates with the latest from Extrapolations.


### Up Next


[Life Science](https://extrapolations.com/category/life-science/)[Redesigning the Rules: How FEP+ Helped Us Design a Differentiated SOS1 Inhibitor FEP+ allowed us to break into new chemical space, ultimately resulting in the discovery of SGR-4174, our IND-ready SOS1 inhibitor. BY Abba E. Leffler, Ph.D.](https://extrapolations.com/redesigning-the-rules-how-fep-helped-us-design-a-differentiated-sos1-inhibitor/)
