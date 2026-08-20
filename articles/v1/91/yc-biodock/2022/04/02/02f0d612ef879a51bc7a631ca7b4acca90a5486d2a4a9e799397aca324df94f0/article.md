---
schema_version: "1.0.0"
document_id: "02f0d612ef879a51bc7a631ca7b4acca90a5486d2a4a9e799397aca324df94f0"
company_key: "yc-biodock"
company: "Biodock"
source_id: "yc-biodock-rss-567b1dbd4991"
canonical_url: "https://blog.biodock.ai/imaging-with-expansion-microscopy/"
published_at: "2022-04-13T14:55:46+00:00"
first_seen_at: "2026-07-24T19:19:16.186046+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:16679979d9438277f32b7a215d87c925a422ae0f2ad47ef4fb81ae4fdbf785f5"
---

# Cheating the Diffraction Limit with Expansion Microscopy

Imagine you are behind the camera framing a picture of your friend. Where you stand, you notice that although you are capturing much of the background, your friend is far away and consequently they appear blurry. To adjust for this, you move in closer and snap the perfect picture. What did you just do? You decreased the distance between the camera sensor and the subject, such that now each pixel occupies less surface area and thus represents finer information. This provides a useful framework for conceptualizing resolution, which measures how many pixels there are and how much information each pixel encodes. A paramount goal in the field of microscopy is to image increasingly smaller things at increasing degrees of resolution. How can we do this?


Mathematically, the resolution is a function of the numerical aperture of the optical system and the wavelength of the light you are collecting (figure 1). The resolution of this system, however, is limited by its[diffraction limit](https://www.youtube.com/watch?v=sTa-Hn_eisw&ref=blog.biodock.ai) which is a result of how light bends due to its wave properties. In our thought experiment, the subject grew in the frame by becoming closer to the sensor. High-resolution microscopes, however, are already incredibly close to the sample. What if instead, we could literally grow the sample itself like consuming a super mushroom in Mario Bros? Scientists have developed a method whereby this is possible, and it is called Expansion Microscopy (ExM). In this article, I will discuss how it works and how scientists are using it to cheat the diffraction limit and peer into previously inaccessible nanoscopic worlds.


[Figure 1:](https://svi.nl/DiffractionLimit?ref=blog.biodock.ai) Schematic representation of the diffraction limit of an optical lens (a) the numerical aperture (NA) of a lens is a function of the maximal angle that the lens can receive light (b) the diffraction limit is the fraction of the wavelength of the light and the numerical aperture. This determines how close two light sources can be disambiguated.


**Table of Contents**


**1. What is Expansion Microscopy and how does it work?**


**2. How is Expansion Microscopy being used in fields like neuroscience and pathology?**


**3. How can AI integrate into the workflow of high-resolution imaging afforded by Expansion Microscopy?**


***Expansion Microscopy deploys hydrogel chemistry to isotropically expand biological tissues***


[Polyelectrolyte g](https://aip.scitation.org/doi/full/10.1063/5.0048152?ref=blog.biodock.ai) els are composed of crosslinked polymers that form a grid network. One property of these gels is that under certain environmental conditions, like being placed in water, they can swell. At the molecular level, this is caused by the isotropic expansion of the polymer network (Figure 2). The general strategy of ExM, developed by Ed Boyden’s group at MIT, is to apply this principle to biological tissues through a series of chemical infusions and treatments which alter the properties of the tissue while preserving the cellular and protein structure. What are these[steps](https://www.nature.com/articles/s41596-018-0117-3?ref=blog.biodock.ai) and how do they work?


[Figure 2: Polyelectrolyte hydrogels' polymer network expands in the presence of water](https://www.nature.com/articles/s41592-018-0219-4?ref=blog.biodock.ai)


1. Labeling: Attaching an imaging probe to your proteins of interest for eventual visualization during imaging (Figure 3a)


Labeling is typically performed using general[immunostaining methods](https://www.cellsignal.com/applications/immunofluorescence/overview-immunofluorescence-techniques?ref=blog.biodock.ai) . In immunostaining, antibodies are iteratively used to target proteins of interest. Initially, the primary antibody is used to bind to your protein of interest. Next, a secondary antibody that is specific to the primary is added to the sample. The secondary antibody is conjugated to a fluorophore which can be probed during microscopy. This way you can visualize the spatial distribution of that protein. Intermediate steps within labeling include washing out unbound antibodies and ‘[blocking](https://www.ptglab.com/support/immunohistochemistry-protocol/ihc-blocking/?ref=blog.biodock.ai#:~:text=What%20is%20blocking%20in%20Immunohistochemistry,specific%20binding%20to%20other%20molecules.) ’ the sample with protein to minimize nonspecific antibody binding.


2. Anchoring: Covalently linking your imaging probe to the expansion gel itself such that can be embedded in the cross-linked network (Figure 3b)


In the anchoring step, a compound called[Acryloyl-X (AcX)](https://blog.biodock.ai/imaging-with-expansion-microscopy/thermofisher.com/order/catalog/product/A20770) is used to treat the sample. AcX reacts with the amine groups on the amino acids of proteins to create a chemical modification that behaves as an anchor. This will permit linkage to the polymer network in the subsequent polymerization step.


3. Polymerization: Casting the sample in a monomer solution which reacts to form the cross-linked network (Figure 3c)


A monomer solution of sodium acrylate and dimethylacrylamide (DMAA) is added to the sample. This undergoes[free-radical polymerization](https://chem.libretexts.org/Courses/Purdue/Purdue_Chem_26100%3A_Organic_Chemistry_I_(Wenthold)/Chapter_08%3A_Reactions_of_Alkenes/8.7.%09Polymerization/Free_Radical_Polymerization?ref=blog.biodock.ai) to form a gel matrix that links with the anchors from the previous step.


4. Homogenization: Disrupting the mechanical structure of the sample to enable isotropic expansion (Figure 3d)


Since the relative density of protein in the tissue is not uniform, the rate of expansion across regions is inconsistent and could lead to rupturing and damaging of the sample. To circumvent this, the sample has to be homogenized, which is achieved by slicing up the proteins. This is done through a process called digestion which includes treating the sample with a proteolytic enzyme called[proteinase K](https://enzyme.expasy.org/EC/3.4.21.64?ref=blog.biodock.ai) . Another method to homogenize the sample without proteolysis is by heating the sample. This causes the proteins to[denature](https://chem.libretexts.org/Courses/University_of_Arkansas_Little_Rock/Chem_4320/Chem_4320%2F%2F5320%3A_Biochemistry_1/02%3A__Protein_Structure/2.3%3A_Denaturation_of_proteins?ref=blog.biodock.ai) , which overrides the steric interactions which produce the tertiary structure of proteins, causing them to unfold.


5. Expansion: Isotropically expanding the gel matrix and the sample (Figure 3e)


Deionized water is added to the sample which is now encased in a gel. Osmotic force drives the water into the gel which stretches the fabric of the interwoven polyacrylate network and thus isotropically expands your sample.


Now your sample has at least quintupled in size! What this has done is separate fluorophores in space such that features of a cell that are close together are now far enough apart to disambiguate during typical microscopy methods. This means that you can supersede the diffraction limit proportional to your expansion factor. For instance, if your microscope has a diffraction limit of 500 nm, following expansion microscopy you would have an *effective resolution* of under 100 nm.


**Figure 3:[Principle workflow of expansion microscopy](https://www.youtube.com/watch?v=e2zQBuyBhOg&ref=blog.biodock.ai) (a) primary and secondary antibody probes binding to a protein of interest (b) anchoring molecules attached to antibodies and protein (c) formation of a gel matrix (d) homogenization of the gel matrix through proteolysis (e) water-mediated gel and sample expansion**


Although the protocol I described above provides a general scaffold for the strategy of ExM, alternate versions of this method that improve usability, expansion, and versatility have been developed. For example,[iterative expansion microscopy (iExM)](https://www.nature.com/articles/nmeth.4261?ref=blog.biodock.ai) includes two rounds of expansion whereby the polymers in the first gel can be cleaved and a second gel can be introduced. This allows for sub 30 nm resolution. This is small enough to spatially resolve the structure of individual synapses, which are the quantal units of neuronal connections (Figure 4). Furthermore, instead of probing proteins, some scientists have integrated the use of labels for other biological molecules like RNA. This class of ExM uses FISH methods or RNA fluorescence in situ hybridization.[Synergizing FISH methods with ExM](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4965288/?ref=blog.biodock.ai) enables improved subcellular localization and quantification of specified sequences of RNA. How is this marginal improvement in the optical resolution of biological samples being used?


***Expansion Microscopy is being used to identify nanoscopic cellular features in neuroscience and pathology***


Ed Boyden’s group at MIT, the same group which published the original iteration of ExM,[applied this method](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4965288/?ref=blog.biodock.ai) to the field of Pathology.[Kidney Minimal Change Disease (MCD)](https://unckidneycenter.org/kidneyhealthlibrary/glomerular-disease/minimal-change-disease/?ref=blog.biodock.ai) is characterized by proteinuria where the kidney filters out proteins in the blood which would normally remain in circulation. Histologically, this can be diagnosed by examining the foot processes epithelial cells which line the glomeruli of capillaries. Usually, these processes are too small to image optically and require the use of electron microscopy to resolve. With Expansion Pathology (ExPath: coined to denote the use of ExM for pathology), glomeruli can be expanded to achieve high enough resolution to visualize foot processes and diagnose MCD.


[Figure 4: Nanometer resolution imaging of a synapse](https://www.nature.com/articles/s41592-018-0219-4?ref=blog.biodock.ai) **(a) image of a synapse with the presynaptic and postsynaptic terminal. Homer1 is a scaffolding protein located in the postsynaptic terminal. Bassoon is another scaffolding protein located at the active zone of the presynaptic terminal. GluR1 is the receptor for the excitatory neurotransmitter glutamate. (b) the spatial separation of fluorophore intensity demonstrates nanoscale resolution.**


ExM has also proven helpful in the field of neuroscience. In[work](https://www.cell.com/neuron/fulltext/S0896-6273(18)31081-X?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS089662731831081X%3Fshowall%3Dtrue&ref=blog.biodock.ai) from David Fitzpatrick’s group at the Max Planck Institute for Neuroscience, scientists used ExM to visualize the organization of dendritic inputs on neurons in the primary visual cortex. Diffraction-limited optical methods are usually unable to resolve the nanoscale morphology of synapses. Using ExM, the scientists were able to map the spatial organization of dendrites who were receiving input from a specific type of neuron and were able to identify cell type specific patterns. How are scientists capitalizing on ExM by integrating it with machine learning?


***AI-mediated segmentation and classification of expanded tissues open new doors for histological analysis and diagnostics.***


One application is to segment the plasma membranes of cells in sections. In 2D sections, morphometric units for neurons and other cell types like cross-sectional area can be extracted. With multiple sections in series, these segmentations can build three-dimensional reconstructions of the neuron. Following the improved spatial resolution of expanding neuronal tissue,[scientists from Adam Marbelstone and Ed Boyden’s group](https://www.frontiersin.org/articles/10.3389/fncom.2017.00097/full?ref=blog.biodock.ai) trained a[convolutional neural network](https://www.ibm.com/cloud/learn/convolutional-neural-networks?ref=blog.biodock.ai) to classify either pixels as part of the cell boundary or intracellular space. Then, model performance can be visualized by reporting the probability map of all pixels (Figure 5). Another application in the field of pathology is to improve the computational[diagnosis of preinvasive breast lesions](https://www.nature.com/articles/nbt.3892?ref=blog.biodock.ai) . Human classification is heavily inconsistent, and scientists aimed to demonstrate that the improved spatial resolution conferred by ExM combined with computational classification could greatly advance diagnostic efforts. Scientists accomplished this by training a network to segment nuclei and then extract nuclear features such as intensity, texture, and morphology. Armed with this precise information, diagnostic accuracy was an incredibly 95% which for atypical ductal hyperplasia was over 20% better in expanded samples than unexpanded.


**[Figure 5: Cell boundary segmentation of 2D slices of expanded neuronal tissue](https://www.frontiersin.org/articles/10.3389/fncom.2017.00097/full?ref=blog.biodock.ai) (a) segmentation overlay of trained convolutional neural network (b) input image of stained cell boundaries**


If any of the above segmentation and classification methods could be applied to your research, Biodock delivers custom AI modules for fluorescent cell segmentation. Detection includes but is not limited to, nuclei, cell boundaries, and even precise histological features.[Request a brochure](https://www.biodock.ai/brochure/?ref=blog.biodock.ai) to see all the services Biodock has to offer and how we can accelerate your science!


***Conclusions***


Taken together, Expansion Microscopy is an innovative tool that is reshaping our understanding of the world at the nanoscale. Improvements to the method that enhance the expansion factor and broaden its accessibility are currently underway. Critical applications in neuroscience and histology are already being made. Finally, by harnessing the power of artificial intelligence, we will be able to train machines to interpret the high-dimensional information ExM allows us to obtain.
