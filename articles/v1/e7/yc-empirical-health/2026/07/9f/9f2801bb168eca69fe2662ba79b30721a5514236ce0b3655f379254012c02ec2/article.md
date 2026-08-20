---
schema_version: "1.0.0"
document_id: "9f2801bb168eca69fe2662ba79b30721a5514236ce0b3655f379254012c02ec2"
company_key: "yc-empirical-health"
company: "Empirical Health"
source_id: "yc-empirical-health-news-import-485d82c6bcdb"
canonical_url: "https://www.empirical.health/blog/macrocyclic-peptides/"
published_at: "2026-07-26T00:00:00+00:00"
first_seen_at: "2026-07-30T15:53:04.847099+00:00"
fetched_at: "2026-07-30T15:53:06.877444+00:00"
content_hash: "sha256:500e96915946ec5cdcd95a17128d3a8e33612e0b78fa5c1f49f93fb2bf00e83f"
---

# Macrocyclic peptides: a new third drug class?

# Macrocyclic peptides: a new "Goldilocks" drug class?


[Brandon Ballinger](https://www.empirical.health/blog/author/bballinger) ·


Jul 26, 2026


Traditionally, medicines were either small molecules or biologics. 90% of marketed drugs are small molecules like aspirin, atorvastatin, metformin. Small molecules are cheap to manufacture, chemically stable, and small enough (<500 daltons) to cross the wall of your intestine (so they can be taken as pills). Monoclonal antibodies like Repatha or synthetic insulin (made famous by Genentech in 1982) are biologics. Biologics are large, often >150,000 daltons, which makes them able to cover an entire protein surface, but they have to be injected since otherwise they would be destroyed by your digestive enzymes.


(A *dalton* is just a small unit of mass, about 1/12 the mass of a carbon atom.)


What if there were a middle category? Now there is. *Macrocyclic peptides* are large peptides (like GLP-1s) folded into a ring structure.


The first oral macrocyclic peptide was approved by the FDA last week. It’s a[pill form of PCSK9 inhibitor](https://www.empirical.health/blog/oral-pcsk9-inhibitor-lipfendra) that lowers LDL cholesterol by about 60%, comparable to injected PCSK9. This means we now have the precise targeting of a biologic with the stability of pills.


*Enlicitide diagram. 6 of the 8 amino acids don’t occur in nature. Rings are used to stabilize the structure. Source:[American Peptide Society](https://americanpeptidesociety.org/research/enlicitide-decanoate/) ,[Lipfendra prescribing information](https://www.merck.com/product/usa/pi_circulars/l/lipfendra/lipfendra_pi.pdf) .*


Macrocyclic peptides violate the traditional **Rule of Five** , the 1997 guideline that predicts whether a compound will be absorbed when swallowed: under 500 daltons, fewer than five hydrogen bond donors, fewer than ten acceptors. Medicinal chemists now describe this territory as “beyond Rule of Five” space, which is a polite way of saying the old predictions stop working and you have to measure everything yourself.


This post covers why PCSK9 was such a tough target for small-molecule chemistry, how a screening technology from a startup found the molecule, the difficult parts of the 13 year timeline, and whether AI would have helped each phase.


## Why PCSK9 is a hard target for small molecule drugs


PCSK9 destroys the liver’s LDL receptors, which pull cholesterol out of your blood. Blocking PCSK9 works spectacularly well: drugs like Repatha can lower LDL cholesterol by 60% alone or 85% as part of triple therapy.


But three problems make PCSK9 an unusually cruel target for a pill:


- **Its enzyme activity is actually a red herring.** PCSK9 is a protease, so the obvious move is to jam its active site. But[McNutt and colleagues](https://pubmed.ncbi.nlm.nih.gov/17537735/) built a catalytically dead version in 2007, and found it still destroyed LDL receptors. This means PCSK9 works more like an escort than a pair of scissors.
- Its natural **deep, well-shaped cavity is already plugged** by its own prodomain.
- The obvious target is **flat** . The surface where PCSK9 grips the LDL receptor sits more than 20 angstroms from the natural pocket. Merck’s own chemists called both surfaces “flat, featureless.”


In 2019, Merck published a paper describing its own failure to find a conventional pill for this target. In contrast, targeting PCSK9 is “[great for antibodies because they’re huge](https://www.drugdiscoverynews.com/a-new-goldilocks-drug-class-macrocyclic-peptides-15931) (as explained by Douglas Johns, a clinical director on the team that developed enlicitide.)


## Lipfendra took 13 years to develop. Would AI have sped it up?


Development of Lipfendra took 13 years divided across four phases: 1. find a molecule that binds a flat surface, 2. increase binding potentancy, 3. get it across the gut wall 4. and manufacture it at scale.


*Four stages of enlicitide’s development.*


For the most part, this was all done using traditional biotech tools (e.g., in vitro screening techniques). In 2026, an obvious question is: how much would AI help with each of these four stages? Let’s go stage by stage.


## Stage 1: how mRNA display found the starting molecule


Merck licensed an mRNA display technology from a startup, Ra Pharmaceuticals, in 2013.[Ra](https://www.fiercebiotech.com/biotech/ra-pharmaceuticals-announces-broad-collaboration-merck-to-develop-cyclomimetics%E2%84%A2-0) isa Cambridge company built around an mRNA display platform derived from Nobel laureate Jack Szostak’s work. The terms were $4.5 million up front and up to $56 million in milestones, which for a drug now forecast at multibillion-dollar peak sales is one of the better options anyone has bought. UCB ultimately acquired Ra for $2.1 billion in 2020.


mRNA display is an *in vitro* technology. It physically tethers each peptide to the strand of mRNA that encodes it, using a small molecule (puromycin) as a sort of molecular “leash.” The process is repeated iteratively in a test tube, which lets the screen test trillions of potential DNA sequences.


*How mRNA display works: peptides stay tethered to the mRNA that encodes them, so survivors can be sequenced and amplified.*


**Would AI have sped this up?** More than any later stage, yes. De novo macrocycle design genuinely works now. Baker’s lab published[RFpeptides](https://doi.org/10.1038/s41589-025-01929-w) in June 2025, getting 12 binders from 39 molecules synthesized, including 9.4 nM against a bacterial protein the authors call “considerably flatter and difficult to target.” Latent Labs reports hit rates of 91% to 100% against standard benchmarks. So you can now run stage 1 a laptop from a few dozen molecules instead of trillions in a test tube.


One limitation of today’s generative methods is that they only the 20 natural amino acids joined head to tail, while enlicitide needed six non-natural residues. mRNA display handles that today and tellingly, the AI-first shops like Unnatural Products still start by screening with display, then apply computation to optimize the resulting hits. As yet, no AI-designed macrocyclic peptide has entered a clinical trial.


## Stage 2: 190,000x tighter binding


The molecule that came out of that screen was a good start, but it wasn’t an effective drug on its own. It bound PCSK9 only weakly, and its half life in whole blood was only an hour. The approved drug binds 190,000x more tightly and has a half-life closer to 8 hours.


mRNA display hit Optimized lead Enlicitide


Binding affinity (Kᵢ) 956 nM 0.0024 nM 0.005 nM


Surviving 1 hr of trypsin 1.2% 96.8% —


Half-life in whole blood 63 min 7.6 hr —


*Data from[ACS Med Chem Lett 2022](https://doi.org/10.1021/acsmedchemlett.2c00319) and[Circulation 2023](https://doi.org/10.1161/CIRCULATIONAHA.122.063372) .*


That potency change didn’t come from finding new points of contact with PCSK9. The chemists added a second and third ring, which improved potency roughly 500-fold by taking away the molecule’s freedom to wobble. (A floppy chain has to “freeze” into one shape to bind, and pays for that out of its binding energy, whereas a ring is more rigid.) Jan Kihlberg, a medicinal chemist at Uppsala University,[puts it simply](https://www.drugdiscoverynews.com/a-new-goldilocks-drug-class-macrocyclic-peptides-15931) : “Instead of being a floppy piece of string, you make it into a donut.”


The rest of the campaign was a sequence of hard-won small victories. For example, two protease cuts improved rigidity (which improves potenncy). Using 5-fluorotryptophan (a non-natural residue) to sticks its fluorine into a shallow dent on PCSK9 increased potency further. And then and a series of changes reduced oxidation from 98% to 2.9%, making the drug last longer in blood.


The first human data was presented at an American Heart Association meeting. A single dose wiped out more than 93% of the free PCSK9 in people’s blood. At the time one of the researchers[said](https://www.drugdiscoverynews.com/a-new-goldilocks-drug-class-macrocyclic-peptides-15931) : “That’s when I got goosebumps. We reduced free PCSK9 to levels that were basically to zero, which is what the antibodies do. And then we knew we had something.”


**Would AI have sped this up?** Less than you’d hope. Latent-X’s affinities against protein-protein targets are 5-72 micromolar, RFpeptides’ is 6 nanomolar, and enlicitide is 5 picomolar. So we’re still three orders of magnitude short.


## Stage 3: making enlicitide get absorbed


The next challenge is absorption. Every Lipfendra tablet contains sodium caprate, which loosens the tight junctions in your intestinal cells. So drug doesn’t “cross” through gut cells, it squeezes between them. The other lever for absorbsion is just sheer potency: at 5 picomolar affinity, absorbing 1% of a 20 mg dose is plenty.


This is partly why Lipfendra has to be taken on an empty stomach with water, black coffee, or plain tea, and requires you to wait at least 30 minutes before eating. Food interferes with the enhancer. (The rules may not be permanent, though: David Baker’s group has designed macrocycles that reach 40% oral bioavailability in mice with no enhancer at all.)


**Would AI have sped this up?** Not really. AstraZeneca’s Molecular AI group[reported in 2026](https://arxiv.org/abs/2605.05770) that when generative models wander outside their training data, permeability predictions “reduce to random outputs.” And no model handles the paracellular absorption which worked here.


## Stage 4: scaling manufacturing


A 1,722-dalton ring with eight amino acids (six of which are non-natural) isn’t easy to make in bulk. The first synthetic route took 63 steps and leaned on chromatography, so producing a single 100 kg batch would have required more than 170 metric tons of intermediates (this makes even node_modules look efficient). In May 2026, Merck’s process chemists published a[route in Science](https://doi.org/10.1126/science.aed8713) using 13 engineered enzymes, which cut the number of steps in half.


**Would AI have sped this up?** Yes, in part. Merck’s route depends on 13 enzymes engineered to catalyze reactions they didn’t evolve to run, and ML-guided protein engineering is genuinely routine for that job. The one clear computational win in enlicitide’s story is in the factory / process scale-up.


## What other targets could macrocyclic peptides reach?


Most disease biology inolves proteins-protein interactions, and many of those interfaces look like PCSK9’s: broad, flat, and pocketless. Taking a target that only an injected antibody could hit and turning it into a pill has now happened twice in a year (enlicitide, and icotrokinra four months earlier). A sampling of the pipeline:


- **Immunology:** icotrokinra is already oral, blocking the IL-23 receptor for plaque psoriasis, a target that used to belong to injected antibodies like Stelara, Skyrizi, and Tremfya. Protagonist has an oral IL-17 peptide entering Phase 2.
- **Neuromuscular disease:** UCB’s zilucoplan (complement C5) has treated myasthenia gravis since 2023, from the same Ra Pharmaceuticals platform as enlicitide, though it is still injected.
- **Infectious disease:** Roche’s zosurabalpin is in Phase 3 against carbapenem-resistant *Acinetobacter baumannii* , and would be the first new class for that pathogen in about 50 years.
- **Oncology:** the hard frontier, because these targets are all inside the cell. Chugai’s LUNA18 (KRAS–SOS1) hit the best oral bioavailability in the class (21% to 47% across four species, no enhancer), but Roche dropped it in July 2025 over a narrow therapeutic window, and a successor, AUBE00, is next. Circle Pharma has a cyclin A/B inhibitor in Phase 1.


The money is following. Unnatural Products has macrocycle deals with Merck, argenx, and Novartis (a February 2026 cardiovascular target), and Merck paid up to $493 million for Cyprumed’s oral-delivery technology, presumably to beat sodium caprate next time.


## One complication: a small molecule may get there anyway


AstraZeneca’s AZD0780 is an oral small molecule aimed at PCSK9. It lowered LDL by about 51% in Phase 2b and entered Phase 3 in June 2025. So small-molecule chemistry may reach this target after all.


But for now, macrocyclic peptides do genuinely seem like a new and interesting category of medications that can give capabilities of biologic in the form factor of a pill.


---


*See also:[Lipfendra, the first oral PCSK9 inhibitor](https://www.empirical.health/blog/oral-pcsk9-inhibitor-lipfendra) ,[Lipfendra vs Repatha](https://www.empirical.health/blog/lipfendra-vs-repatha) , and[PCSK9 inhibitors vs statins](https://www.empirical.health/blog/statins-vs-pcsk9-inhibitors) .*


**Whether you need a PCSK9 inhibitor depends on your numbers.** Empirical Health's[heart health panel](https://www.empirical.health/product/heart-health-panel?utm_source=macrocyclic_peptides_blog) measures LDL, ApoB, and Lp(a), so you and your doctor can see how far your current treatment is getting you.


## Get your free 30-day heart health guide


Evidence-based steps to optimize your heart health.
