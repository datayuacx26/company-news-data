---
schema_version: "1.0.0"
document_id: "f5d5136ef2fbfeb5c03cd455d5bbfd81f3bec91ad6ae5a9bc1e3686258c7638b"
company_key: "schrodinger-inc-common-stock"
company: "Schrodinger Inc."
source_id: "schrodinger-inc-common-stock-rss-d7bf526e9e3e"
canonical_url: "https://extrapolations.com/enabling-targets-for-structure-based-drug-discovery/"
published_at: "2025-11-17T21:16:19+00:00"
first_seen_at: "2026-07-20T23:18:31.059214+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:8129454ba2722388373db87b83a3200cf9b930fe4a549a3605b4e12f3752b138"
---

# Enabling Targets for Structure-Based Drug Discovery

[Education](https://extrapolations.com/category/education/)


# Enabling Targets for Structure-Based Drug Discovery


BY Schrödinger Editorial Team


Nov 17, 2025


*This is the second in a three-part series highlighting essential computational chemistry topics covered in the newly released[Schrödinger Online Course bundle](https://www.schrodinger.com/life-science/learn/education/courses/hitdiscoverybundle/) . Read on for a review of the various methods for obtaining and refining structures to enable structure-based drug discovery.*


The field of structure-based drug discovery (SBDD) took shape in the 1980s, when advances in X-ray crystallography first allowed scientists to visualize the three-dimensional shapes of biologically important proteins in atomic detail. For the first time, researchers could see exactly how drug molecules fit into their targets, like keys in molecular locks, and use that knowledge to guide the design of new therapeutics. These breakthroughs gave rise to powerful computational techniques such as molecular docking, which predicts how small molecules bind to proteins, and free energy perturbation (FEP), which can estimate binding strength with remarkable precision.


Today, scientists use a range of powerful methods to reveal and refine molecular structures, from X-ray crystallography and cryo-electron microscopy to machine learning-based prediction tools. Computational modeling plays a central role in improving these structures and ensuring they accurately capture how proteins move and interact with potential drugs. Understanding these techniques, and knowing how to validate that a target is ready for structure-based modeling, is essential for translating molecular insight into new medicines.


# Methods for structure determination or prediction


**Experimental structure determination**


X-ray crystallography remains the primary source of structures in the Protein Data Bank and is one of the most established techniques for determining protein structures. Although an X-ray crystal structure can frequently be near-atomic resolution, this process requires significant time and resources, and flexible targets cannot be easily crystallized.


Over the last decade, driven by advances in imaging technology and simplified sample preparation protocols, cryo-EM has emerged as a popular alternative method for protein structure determination at atomic resolution. Cryo-EM structures have an advantage over X-ray crystal structures in that structures can remain in their native states, including membrane-bound states, which is key for enabling G protein–coupled receptor (GPCR) and ion channel targets. Cryo-EM structures are, however, frequently lower resolution and in need of additional refinement to generate a high-confidence atomistic model of the system.


**Template-based structure prediction**


Methods like template-based homology modeling combine knowledge of template structures, utilizing all structural data where the target and query sequences match, with backbone data, which refers to the continuous sequence of atoms that forms a protein’s structural framework. Template-based homology modeling also leverages physics-based prediction methods for anything not explicitly captured in the template. While these models can be valuable, they also have limitations — they are generally more accurate the higher the sequence identity between the query and template sequence.


**Machine learning structure prediction**


Many in the world of drug discovery were introduced to machine learning structure prediction by the “AlphaFold moment” in the 2018 Critical Assessment of Structure Prediction (CASP) competition. In this breakthrough event, DeepMind’s AlphaFold dramatically outperformed all other protein structure prediction methods, signaling a turning point in structural biology. Since then, several generations of both general and specialized structure prediction models have been released, including AlphaFold2/3, Chai1/2, Boltz1/2, and RoseTTAFold-all-atom. Co-folding methods in particular have shown great promise However, the quality of the generated poses can be limited by how similar the target protein is to the structures included in the method’s training set.


Despite their limitations, these models have the potential to vastly expand the number of targets that can be drugged through structure-based drug design methods.


# Structure assessment


The output from any of the methods described above is a three-dimensional structure. An important next step for anyone working with one of these structures would be to evaluate the relevance and quality of the structure. Does it capture the biologically relevant conformation? Does it have a well-resolved ligand with a binding mode similar to what we’d expect for our project’s chemistry? Answering these questions can help differentiate between “a” structure and “the right” structure.


# Structure refinement


Structures obtained from any of these methods are rarely, if ever, ready for immediate use in modeling or drug design without additional refinement and validation. Preparation and refinement methods, such as with Schrödinger’s


[Protein Preparation Workflow](https://www.schrodinger.com/life-science/learn/white-papers/protein-preparation-workflow/) , and


[IFD-MD](https://www.schrodinger.com/platform/products/ifd-md/) , can help fill in missing side chains or refine ligand placement in the binding site. These tools can help fix glaring mistakes and refine models, and are also fundamentally hypothesis generators that allow researchers to generate several models that could be profiled by


[FEP+](https://www.schrodinger.com/platform/products/fep/) , Schrödinger’s industry-leading free energy perturbation technology.


# Enablement through rigorous validation


The final step of structure enablement is to evaluate the predictive power of your structural model. This is done by using a computational assay, such as FEP+, and a series of known binders with experimental binding affinity to test how well the predicted affinities correlate to the experimental values.


Poor correlation indicates that the structural model doesn’t sufficiently capture the biological system, suggesting that it would have limited utility for affinity prediction and other structure-based tasks. Strong correlations are the ultimate validation of structure quality and utility.


# Learn More


If you are interested in diving deeper, Schrödinger has recently launched an online certification course,


[Target enablement, preparation, & validation](https://www.schrodinger.com/life-science/learn/education/courses/target/) , which combines in-depth learning with hands-on exercises to explore all aspects of target enablement.


Schrödinger Editorial Team


### Sign Up Today


Sign up to receive quarterly updates with the latest from Extrapolations.


### Up Next


[Education](https://extrapolations.com/category/education/)[Elevate Your Screening Campaign: Designing Quality Ligand Libraries for Hit Discovery Crafting a high-quality ligand library is essential for exploring chemical space and discovering new hits. While broad, general-purpose libraries exist, focusing on libraries customized for specific screening campaign goals and targets is more effective. BY Schrödinger Editorial Team](https://extrapolations.com/elevate-your-screening-campaign-designing-quality-ligand-libraries-for-hit-discovery/)
