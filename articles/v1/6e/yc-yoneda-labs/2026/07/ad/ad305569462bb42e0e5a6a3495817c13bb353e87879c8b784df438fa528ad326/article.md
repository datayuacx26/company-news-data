---
schema_version: "1.0.0"
document_id: "ad305569462bb42e0e5a6a3495817c13bb353e87879c8b784df438fa528ad326"
company_key: "yc-yoneda-labs"
company: "Yoneda Labs"
source_id: "yc-yoneda-labs-news-import-4aef20936c75"
canonical_url: "https://www.yonedalabs.com/post/suzuki-miyaura-cross-coupling-practical-guide/"
published_at: null
first_seen_at: "2026-07-26T06:14:18.532936+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:4620527d4b91e35198ec8c338c16e44c77603e04b6b98a3dd54b0018e8c103fa"
---

# Suzuki-Miyaura cross-coupling: Practical Guide

# Suzuki-Miyaura cross-coupling: Practical Guide


Suzuki-Miyaura cross-coupling is palladium catalyzed reaction for forming carbon-carbon bonds. Its development originated in the 1980s, and in 2010 Akira Suzuki was awarded the Nobel Prize for chemistry together with inventors of two other cross-coupling reactions, Richard F. Heck and Ei-ichi Negishi \[[Suzuki 1979](https://doi.org/10.1016/S0040-4039(01)95429-2) ,[Nobel prize 2010](https://www.nobelprize.org/prizes/chemistry/2010/illustrated-information/#:~:text=The%20Royal%20Swedish%20Academy%20of,cross%20couplings%20in%20organic%20synthesis%E2%80%9C.) \].


There are quite a few components to a typical Suzuki-Miyaura reaction:


- Nucleophile - boronate - typically boronic acid, ester, or boronate salt
- Electrophile - (pseudo)halide - typically bromide, iodide, chloride, tosylate, or other leaving groups
- Palladium catalyst - source of Pd metal and ligand that stabilizes and solubilizes it
- Base - usually carbonates, phosphates, alcoholates, fluorides, or organic bases such as amines
- Solvent - many organic solvents are possible, including dioxane, THF, DMF, or toluene. A small amount of water is usually added


During the reaction, the carbon skelets of nucleophile and electrophile are coupled together, creating a new C-C bond.


Over the years, Suzuki-Miyaura cross-coupling has become very popular for its versatility, and now it is one of the most common reactions in synthetic organic chemistry.


Despite its popularity, the reaction is rather complex. The unusual chemistry of palladium and boronates yields not only an intriguing reaction mechanism but also many possible side reactions. Cross-coupling reactions come with many unexpected quirks, and it is a life-long journey to understand the greater picture as well as all the intricacies.


This article is a great starting point for younger chemists in their undergrad who just started working in a research lab, as well as for medicinal chemistry veterans who might find some of the mechanistic insights interesting or surprising.


### The catalytic cycle


The mechanism of Suzuki-Miyaura cross-coupling is centered around a square-planar Pd complex which exists in two oxidation states, Pd(0) and Pd(II), and usually has 14 or 16 electrons in its coordination sphere.


The general steps are outlined below:


1. **Pd source:** Pd complex can enter the catalytic cycle only as Pd(0) with at most 2 ligands
2. **Oxidative addition:** Pd inserts itself into the C-X bond of the electrophile, such as aryl bromide
3. **Ligand exchange:** The halide leaves and is replaced either by boronate anion, or hydroxide that then binds boronic acid
4. **Transmetalation:** Using the hydroxide bridge, the aryl is transferred from boron to palladium
5. **Reductive elimination:** The two aryl groups get cis relative to each other and eliminate from the palladium, forming a C-C bond


After reductive elimination, the active Pd(0) species is regenerated and re-enters the catalytic cycle.


Generally accepted mechanism for Suzuki-Miyaura cross-coupling reaction. The ligation state throughout the catalytic cycle is not well understood for most ligands, but the palladium atom is usually coordinated to one or two phosphine atoms \[[Joshi 2022](https://doi.org/10.1021/acscatal.1c05802) \].


### Generating active catalysts


During the catalytic cycle, palladium exists mostly as a square planar complex, switching between Pd(0) and Pd(II). The palladium complex can enter the catalytic cycle only as Pd(0). Soluble compounds with Pd(0) are often unstable on air, so Pd(II) sources are preferred, but the palladium needs to undergo initial reduction.


### Pd(0) sources


The most common sources of Pd(0) are tetrakis(triphenylphosphine)palladium Pd(PPh3)4, and complexes with dibenzylidene acetone: Pd2(dba)3 and Pd(dba)2.


Tetrakis is a great palladium source which comes with triphenylphosphine as a ligand.


DBA complexes are used together with the phosphine ligand of choice and serve solely as a source of Pd. The disadvantage is that DBA complexes decay, forming palladium black - nanoparticles of metallic palladium. This can lead to a nearly complete loss of catalytic activity \[[Weber 2019](https://doi.org/10.1021/acs.oprd.9b00214) \].


3D structure of Pd2(dba)3 \[Wiki\].


### Pd(II) sources


Pd(II) salts are common and often bench-stable sources of palladium. Palladium acetate, Pd(OAc)2, is often used together with phosphine ligands added separately, or some bidentate ligands come as chloride complexes, for example, Pd(dppf)Cl2.


For the catalytic cycle to start, Pd(II) needs to be reduced to Pd(0) - this usually happens by oxidizing some of the phosphine ligands, or by homocoupling two boronic acids.


Pd(II) needs to be reduced to produce catalytically active Pd(0) complex. This most commonly happens by oxidizing phosphine ligand, or homocoupling.


### Precatalysts


Palladium precatalysts are Pd(II) complexes that contain the desired ligand and are designed in such a way that the complex can easily dissociate, producing active Pd catalyst in a clean and efficient way.


**Buchwald** precatalysts come in many generations (G2, G3 and G4 being most popular now). They feature a bidentate ligand with an amine that can reductively eliminate, producing a Pd(0) catalyst.


**Allyl precatalysts** feature η3 coordinated allyl, under basic conditions the complex can decompose in different ways, releasing active Pd(0).


Activation routes for Pd(allyl) precatalysts \[[Shaughnessy 2019](https://doi.org/10.1002/ijch.201900067) \].


**PEPPSI type precatalysts** are usually used together with NHC (N-heterocyclic carbene) ligands based on sterically hindered imidazolium \[[Valente 2012](https://doi.org/10.1002/anie.201106131) \]. Note that PEPPSI type precatalysts produce Pd(II) species that need to be reduced, likely by homocoupling \[[O'Brien 2006](https://doi.org/10.1002/chem.200600251) \].


Example of NHC PEPPSI catalyst, PEPPSI-iPr.


**Palladium chlorides with bidentate ligands** are popular precatalysts for ligands such as dppf, dtbpf, XantPhos, or dppe. Under basic conditions, one of the phosphines is oxidized, producing an active catalyst with Pd(0).


Oxidizing one of the phosphorus atoms in bidentate ligands has been shown to be necessary for catalytic activity for Suzuki cross-coupling or C-H activation. The addition of unoxidized bidentate phosphine binds the palladium, making it catalytically inactive \[[Murray 2022](https://doi.org/10.1021/acs.oprd.2c00332) ,[Ji 2015](https://doi.org/10.1021/jacs.5b01913) \].


### Side reactions


The relatively harsh conditions together with the catalytic activity of Pd and unstable boronates often lead to the formation of various side products. These not only reduce the reaction yield but also make the purification more difficult.


Understanding the side reactions can be useful in choosing or adjusting reaction conditions in a way that prevents them.


### Protodeborylation


Boronic acids are prone to hydrolysis, this is especially true for heteroaryl boronic acids. Protodeborylation can proceed via multiple mechanisms depending on the electronic properties and protonation state.


There are some structural motives that accelerate protodeborylation, among the most common are 2-heteroaryls and protonable heteroatoms in the aromatic ring \[[Cox 2016](https://doi.org/10.1021/jacs.6b03283) ,[Hayes 2021](https://doi.org/10.1021/jacs.1c06863) \].


Common motives that accelerate protodeborylation in aqueous conditions. In these cases, protodeborylation does not require a Pd catalyst \[[Cox 2016](https://doi.org/10.1021/jacs.6b03283) ,[Hayes 2021](https://doi.org/10.1021/jacs.1c06863) \].


### Dehalogenation


Dehalogenation of the aryl halide substrate is another common side reaction. After oxidative addition, the Pd complex might oxidize something else in the reaction mixture (often amine base or alcoholic solvent) to gain hydride ligand. The aryl and hydride can then reductively eliminate, forming deshalo side product.


Possible mechanism for dehalogenation of aryl halides \[[Navarro 2005](https://doi.org/10.1021/jo0521201) \].


### Homocoupling


If the reaction mixture contains Pd(II) species without aryl/vinyl/alkyl ligands, the palladium complex will likely cause homocoupling of two boronic acids. These species usually occur because a Pd(II) source was used, the reaction mixture wasn’t properly degassed, or other oxidative processes.


Possible mechanism for homocoupling of phenylboronic acid.


During the homocoupling reaction, two boronic acids are coupled together and the Pd(II) species is reduced to Pd(0).


It has been shown that higher oxygen levels lead to an increase in homocoupling \[[Miller 2007](https://doi.org/10.1021/op060180i) ,[Adamo 2006](https://doi.org/10.1021/ja0569959) \].


### β-hydride elimination


If one of the substrates, typically boronate, is aliphatic and contains β-hydrogens, the hydrogen can be transferred to Pd, forming alkene.


To prevent β-hydride elimination, it might be advantageous to increase the rate of oxidative addition by using aryl iodide as electrophile and adding silver salt.


### Electrophiles - halides and pseudohalides


Common electrophiles for Suzuki cross-coupling include bromides, iodides, chlorides, triflates, and less commonly also tosylates and mesylates. The choice of electrophile has numerous effects.


**Electron poor electrophiles with a good leaving group undergo oxidative addition easier**
Bromides, iodides and triflates usually undergo oxidative addition faster than the rate determining step. Chlorides, tosylates and mesylates will react slower and might require more electron rich ligands.


An important factor is also the electron richness of the aromatic system - pyridines or benzenes with electron withdrawing groups will react more readily than electron rich aryls, such as anisole.


**Iodide can poison the palladium**
Despite very fast oxidative addition, iodides are soft ligands that can strongly bind the palladium complex and slow down the catalytic cycle \[[Ho 2017](https://doi.org/10.1021/acs.organomet.8b00189) \]. In some cases, iodide can act as μ-bridging ligand, forming inactive palladium dimers \[[reddit](https://www.reddit.com/r/chemistry/comments/60dcqr/ive_heard_about_organic_iodides_being_unsuitable/) \].


Inactive Pd dimer with μ-bridging iodide ligands.


### Nucleophiles - boronic acids and esters


Boronates are a key part of the Suzuki-Miyaura cross-coupling reaction, but their instability is the most common reason for low yields. Some of the most common and useful boronic esters are shown below.


Examples of common boronic acid derivatives used for Suzuki cross-coupling.


Let's talk about the different classes of boronates and the rationale behind their design.


**Boronic acid**
Boronic acid and its trimers, boroxines, can be a safe default. They are easy to prepare, often commercially available, and work well if the boronic acid does not decompose.


**Pinacol and similar esters**
Boronic esters with simple diols such as pinacol, catechol, or more recently neopentyl glycol improve the stability of the boronate, preventing protodeborylation.


Catechol esters were used in the original work of Suzuki \[[Miyaura 1979](https://doi.org/10.1016/S0040-4039(01)95429-2) \]. Pinacol esters are the most popular and can be prepared using Miyaura borylation reaction - coupling B2pin2 onto aryl halide using Pd catalyst \[[Organic Chemistry Portal](https://www.organic-chemistry.org/namedreactions/miyaura-borylation-reaction.shtm) \]. Neopentyl esters were popularized in 2021 by Denmark for their good reactivity in anhydrous conditions \[[Kassel 2021](https://doi.org/10.1021/jacs.1c06419) \].


Depending on the ester and on the reaction conditions, the ester usually needs to be hydrolyzed to boronic acid before undergoing transmetalation \[[Thomas 2016](https://doi.org/10.1126/science.aad6981) ,[Thomas 2018](https://doi.org/10.1021/jacs.8b00400) \] - but see the section “Is water needed?”.


**Trifluoroborates**
Trifluoroborates can be prepared from boronic acid or its esters using KHF2 \[[Molander 2009](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2908178/) \]. Similar to boronic esters, the trifluoroboronate is slowly hydrolyzed in alkaline aqueous conditions to produce boronic acid.


Trifluoroboronates can be poorly soluble in organic solvents, and it is not practical to purify them using typical column chromatography.


**Coordinating esters such as MIDA or Epin**
These esters contain groups that can donate into the empty orbital on the boron atom, making the esters more stable and easier to work with (especially for chromatography with silica or TLC) \[[Aich 2022](https://doi.org/10.1039/D2CC04893K) \].


Unlike B(pin), B(Epin) esters can be easily purified on silica \[[Oka 2022](https://doi.org/10.1021/acs.orglett.2c01174) \].


Coordinating esters need to be hydrolyzed into boronic acid to undergo transmetalation, this can be done before or during the cross-coupling reaction.


While Epin has very similar properties to pinacol ester \[Oka 2022\], amines such as MIDA, DEA, or N-phenylethanolamine are significantly more stable. If a molecule has two boronate centers, one as boronic acid and the other one protected by a suitable amine, the boronates can be sequentially coupled to two different substrates.


MIDA boronate allows is stable during anhydrous Suzuki cross-coupling and can be used for sequential coupling reactions \[[Sigma-Aldrich article](https://www.sigmaaldrich.com/US/en/technical-documents/technical-article/chemistry-and-synthesis/cross-coupling/mida-boronates) \].


### Common catalysts and their properties


Palladium ligands are a key component in most cross-coupling reactions. They fulfil many roles, including solubilizing and stabilizing the Pd(0) species, preventing the formation of palladium black, and modulating the electronic and steric properties of the reactive palladium center.


Structures of some ligands commonly used for Suzuki-Miyaura cross-coupling.


Choosing the correct ligand often requires screening or trial-and-error, although there is some understanding of how ligand properties affect the reaction:


- Electron rich ligands (alkylphosphines, ferrocenes, NHC) promote oxidative addition, this is important, especially for chlorides or electron rich electrophiles
- Bulky ligands (tert-butyl, adamantyl, some NHC ligands) promote reductive elimination


Many ligands can also provide a second hemilabile coordinating atom - this is useful to prevent the palladium from being inactivated by binding two ligands, and increases the stability of the complex.


Secondary hemilabile coordination of Pd atom to aryl ring in SPhos and phosphine oxide in oxidized XantPhos \[[Barder 2007](https://doi.org/10.1021/om0701017) ,[Ji 2015](https://doi.org/10.1021/jacs.5b01913) \].


There have been many attempts to quantify electronic and steric properties of ligands, and use them to predict reaction outcomes - these are often referred to as quantitative structure reactivity relationships (QSRR). These approaches are generally more successful for predicting regio- and stereo-selectivity rather than yield \[[Durand 2019](https://doi.org/10.1021/acs.chemrev.8b00588) ,[Niemeyer 2016](https://doi.org/10.1038/nchem.2501) ,[Dijk 2023](https://doi.org/10.1021/jacs.3c06674) ,[Newman-Stonebraker 2022](https://doi.org/10.1021/jacs.2c09840) \].


### Solvents


There are many types of solvents that are used for Suzuki cross-coupling reaction:


- **Ethers:** dioxane, THF, 2Me-THF, DME
- **Esters:** BuOAc, iPrOAc
- **Aromatics:** toluene, xylenes
- **Amides:** DMF, DMAc, NPM
- **Alcohols and glycols:** MeOH, EtOH, n-BuOH, diglycol
- **Sulfoxides:** DMSO, sulfolane


More often than not, water is added to the organic solvent to activate the boronic species and to dissolve the inorganic base. Common ratios range from 2:1 to 10:1 solvent:water, but it is relatively common to use anhydrous conditions too.


Depending on your choice of solvent system and reactants, the reaction might be homogeneous, biphasic emulsion, suspension, or anything in between. Strong stirring is often a necessity.


### Is water needed?


This is a difficult and controversial question.


The generally accepted mechanism for transmetalation features a μ-bridging hydroxy group between Pd and B atoms - this would suggest that water is needed for the catalytic cycle. In addition, most boronic esters are thought to hydrolyze to boronic acids before reacting \[[Thomas 2016](https://www.science.org/doi/10.1126/science.aad6981) \].


Transmetalation of boronate onto palladium proceeds via μ-bridging OH group.


But in 2018, the Denmark group published another paper \[[Thomas 2018](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6008789/) \] showing that even some boronic esters can undergo transmetalation without ester hydrolysis, just with stochiometric CsOH to form the bridge between B and Pd. The paper wasn't conclusive about pinacol and neopetyl esters though.


In 2021, the Denmark group published another paper \[[Kassel 2021](https://pubs.acs.org/doi/full/10.1021/jacs.1c06419) \] featuring Suzuki cross-coupling in completely anhydrous conditions, where reactions go to completion in minutes at r.t. They used TMSOK as a base, and the trimethylsilonate anion most likely acts as μ-bridging ligand \[[Delaney 2022](https://doi.org/10.1021/jacs.1c08283) \]. The paper also shows that these conditions are very sensitive to Lewis bases (pyridines and other heterocycles) which poison the palladium. However, that can apparently be solved by adding Lewis acidic B(OMe)3 that binds the Lewis bases, restoring Pd activity.


In addition, boronic acids are known to trimerize to form boroxines, releasing water. Even in an anhydrous solvent, the released water can generate hydroxide ions that then participate in the catalytic cycle.


### Bases


Similar to solvents, there are many bases that are commonly used - both inorganic and organic.


- **Carbonates:** K2CO3, Cs2CO3, …
- **Phosphates:** K3PO4
- **Bicarbonates:** KHCO3, NaHCO3
- **Fluorides:** KF, CsF
- **Hydroxides:** KOH, NaOH, LiOH
- **Alkoxides:** NaOMe, NaOEt, KOtBu, LiOiPr, …
- **Amines:** NaOMe, NaOEt, KOtBu, LiOiPr, …
- **Modern organic bases:** DBU, 7Me-TBD, P2-Et, BEMP


There isn't a straightforward way to choose a base, but one should consider compatibility with the solvent, any functional groups on the substrates, and solubility.


### Approaches for choosing reaction conditions


There are many parameters that can be easily changed and that can have a large effect on the outcome of the cross-coupling reaction. To name just a few:


- Catalyst system - source of Pd and ligand
- Solvent, amount of solvent (concentration of substrates)
- Base, equivalents of base
- Temperature


All these parameters form an exponentially growing combinatoric space, and finding the best, or at least good enough reaction conditions can be challenging.


Let's look at some of the most common methods for finding reaction conditions!


### Method 1: Try and see


This is a very common approach in many academic or medicinal chemistry labs - try some go-to reaction conditions, or reaction conditions that have been published for similar substrates, and see what happens. If it works, great - if not, try something else.


If you cannot run reactions in parallel, or you have very limited starting material, this is a reasonable approach.


### Method 2: Design of Experiments


Design of Experiments (DoE) is a statistical tool that lets you systematically explore the reaction space without having to try all combinations.


DoE can seem daunting if you've never encountered it before, but it's actually really simple. The easiest way to start is with Taguchi orthogonal arrays \[[NightHawkInLight](https://youtu.be/5oULEuOoRd0?si=3Y0cIBA4dNU1Fux4) ,[York Maths Dept.](https://www.york.ac.uk/depts/maths/tables/orthogonal.htm,) ,[Wiki](https://en.wikipedia.org/wiki/Fractional_factorial_design) \], or using simple online tools \[[YonedaLabs](https://www.yonedalabs.com/#optimize) ,[Sskki-exe GitHub](https://github.com/Sskki-exe/TaguchiArrayGenerator) \].


For example, if you're considering 4 ligands, 4 bases and 4 solvents, instead of testing all 64 combinations, you can still efficiently map out the reaction space using the L16 orthogonal table \[[York Maths Dept.](https://www.york.ac.uk/depts/maths/tables/l16b.htm) \].


### Method 3: Pd screening plates


Many vendors offer Pd screening kits - these usually come with pre-weighted Pd precatalysts, degassed solvents and a selection of bases. Using a syringe and N2 balloon, these kits let you easily run 24 reactions in parallel! That being said, the kits are often quite pricey \[[HepatoChem](https://hepatochem.com/suzuki-miyaura-cross-coupling-screening-kits/) ,[Sigma-Aldrich](https://www.sigmaaldrich.com/US/en/product/aldrich/kitalysis24pd) ,[Krackeler](https://www.krackeler.com/catalog/sigma/ALDRICH/KITALYSIS-SM) \].


### Method 4: Testing all combinations


Also known as full-factorial design, this approach is usually done using HTE (high-throughput experimentation) set-ups that can run 96 or more reactions in parallel. This would allow to screen combinations such as 12 ligands + 4 bases + 2 solvents, 8 ligands + 4 bases + 3 solvents, or similar.


### Method 5: Bayesian Optimization


You can think of Bayesian Optimization (BO) like DoE on steroids. It starts by recommending 10–20 smart experiments to get you going. As you feed in results from your wet lab, the machine learning engine behind BO continuously learns and adapts, proposing increasingly better experiments for your next round. This iterative approach allows you to home in on optimal reaction conditions within days — even when you're navigating thousands of possible combinations.


At Yoneda Labs, we've developed software that lets you use BO without any prior knowledge of statistics or programming required. If you'd like to try it out in your lab, feel free to schedule a demo with us on[Calendly](https://calendly.com/michal-yonedalabs/chat-with-yoneda-labs) .


### Conclusion


Congratulations if you made it all the way to here! I hope that you found the article interesting and enjoyable. I tried to keep the article concise and with lots of figures - I know, everyone loves figures.


If you want to dive deeper into any of the topics, the references should be a great place to start! I can also recommend a guide from Sigma-Aldrich \[[Bruno 2017](https://www.sigmaaldrich.com/US/en/campaigns/cross-coupling-reaction-guide) \].


Many aspects of transition metal chemistry are still not fully understood, and researchers can often have different opinions. If you think that any part of this article is not entirely accurate, or missing some important details, please let me know at jan@yonedalabs.com and I'll update the article!


### Get in touch


At Yoneda Labs, we built software that can help you effortlessly use Bayesian Optimization, automatically analyze your LC-MS data, and manage your HTE lab. We're also running our own HTE lab to create a model that will predict high-yielding conditions for organic reactions without any wet lab experimentation. If you're interested in our software or just want to chat about HTE or organic synthesis, feel free to schedule a meeting with us on[Calendly](https://calendly.com/michal-yonedalabs/chat-with-yoneda-labs) .
