---
schema_version: "1.0.0"
document_id: "b1ba86dc0f1dcc79a92272bb3b8ac3b259688b02fc05c7542b5cdb7181584462"
company_key: "recursion-pharmaceuticals-inc-class-a-common-stock"
company: "Recursion Pharmaceuticals Inc."
source_id: "recursion-pharmaceuticals-inc-class-a-common-stock-news-import-f0ced5d1ab91"
canonical_url: "https://www.recursion.com/news/since-its-inception-recursion-has-been-building-the-foundation-for-the-first-virtual-cell"
published_at: "2025-12-04T00:00:00+00:00"
first_seen_at: "2026-07-22T11:05:14.614366+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:495162f5c6e951abdf6880b4a83da0a265e1ca543d58bd2451fd11ae5dd187fc"
---

# Since Its Inception, Recursion has Been Building the Foundation for the First Virtual Cell

Since Recursion was founded in 2013, the company has focused on fundamentally shifting the way drugs are made using data and AI – from an emphasis on generating proprietary data using real-world experimentation, to an increasing reliance on *in silico* modeling and simulation. From those early days, Recursion has been building the necessary components to virtualize key stages of the drug discovery process. So-called virtual cells *,* computational systems that can accurately simulate cellular and patient-level responses to therapeutic interventions, are core to this vision and built on top of the massive, proprietary biological and chemical datasets, AI models, and one of the industry’s most powerful supercomputers, BioHive-2, available to us at Recursion.


Why are virtual cells needed? Because traditional drug discovery is still plagued by high uncertainty, high costs, long timelines, and 90% failure rates. Virtualizing key components of the drug discovery process would allow us to navigate biology’s enormous complexity – to better understand what is driving diseases and to identify completely new ways to treat them – with maximum speed and efficiency.


First, we needed better biological data to train patient-relevant AI models. There are a number of public datasets, of course, and many companies building AI models rely on them. These include the Cancer Genome Atlas, the UK Biobank, ChEMBL, and the Protein Data Bank. While all of these datasets provide important insights, they lack standardization and are often biased toward certain populations or protein types.


To train the best models able to generate novel biological insights that could meaningfully shift outcomes in drug discovery and create a true competitive moat, we needed to create our own datasets in an automated, standardized, and repeatable fashion.


### Building the Data Moat, From Phenomics to Patient Data


Initially, Recursion’s efforts focused on phenomics – capturing images of different types of human cells in different states of perturbation.


Using automated labs, researchers at Recursion developed new ways to grow, freeze, thaw, and experiment with cells in large quantities. Over the course of more than a decade, we have produced hundreds of billions of cells across more than 50 different cell types.


We perturb these cells using CRISPR-Cas9 editing and by adding chemical compounds, and run many replicates of each combination to create a more robust experimental signal. Each week, Recursion’s automated labs run up to 2.2 million of these experiments – collecting not just one data type, but multiple types of data needed to help reconstruct what’s happening at a cellular level. In addition to[Cell Painting and Brightfield imaging data](https://www.recursion.com/news/brightfield-is-back-a-17th-century-cell-imaging-technique-is-making-a-comeback-thanks-to-machine-learning) , this includes:


- **Transcriptomics** – We use Trekseq, Recursion’s industrialized transcriptomics platform, to generate high-throughput, cost-effective RNA expression data at scale. Trekseq provides a robust, standardized, and reproducible method for measuring gene expression across thousands of samples, supporting drug discovery and development workflows.


- **Chemical and Molecular Data:** We have generated extensive data on chemical compounds and their interactions with biological systems. This includes a proprietary absorption, distribution, metabolism, excretion, and toxicity (ADMET) dataset and atomistic data, including detailed chemical property measurements and chemical-protein interaction data. This gives us molecular-level insights crucial for advancing drug candidates.


- **Real-World, Patient Data** - We partner with companies like Tempus (for oncology) and Helix (for non-oncology) to gain access to high dimensional, unbiased, high throughput patient genomics data along with longitudinal medical record data. Having real-world human data is critical. This layer allows us to[do both forward and reverse genetics](https://www.recursion.com/news/leveraging-the-power-of-patient-data-in-ai-drug-discovery) to build causal AI models. We can use our phenomics maps to surface signals from the patient data and we can use the patient data to discover disease associations in our maps.


### From AI Models to a Virtual Cell


To turn these data layers into real insights about diseases and how to treat them, we need AI models that are capable of processing and analyzing multiple data types at massive scale and finding connections. AI models help us create cell-specific virtual maps for our partners in areas[like neuroscience](https://www.recursion.com/news/how-a-first-of-its-kind-microglia-map-from-recursion-and-roche-and-genentech-could-unlock-new-treatments-for-neurodegenerative-diseases) that can quickly surface novel targets and hits. They also, ultimately, are helping us to build a true virtual cell.


First, we use our deep resources of phenomics, transcriptomics, and other data to train models. Over time, those models are able to simulate the outcomes of phenomic and transcriptomic experiments across relevant cell types. Already, our models can accurately simulate phenomics experiments and the outcomes of large-scale drug screening. Teams at Recursion are now actively working on the next step in the evolution – the “explanation” function.


This explanation piece is a defining feature of how Recursion is approaching the virtual cell, as outlined in a recent paper,[Virtual Cells: Predict, Explain, Discover](https://arxiv.org/abs/2505.14613) . We don’t want a virtual cell to only be able to predict how human cells will respond to perturbations – we want them to explain to us, mechanistically, the reasoning behind their predictions. This will help expand our understanding of biology and build trust in the model.


Some of the models that Recursion is using to lay the foundation for the virtual cell include:


- [MolE](https://www.recursion.com/news/introducing-mole-a-new-model-for-predicting-molecular-properties-for-ai-drug-design-and-beyond) - a foundation model for chemistry trained on over 842 million molecular graphs and further fine-tuned on a set of downstream ADMET tasks that can help guide chemical property prediction and the design of highly optimized drug-like molecules.


- [Molphenix](https://www.recursion.com/news/learning-the-effects-of-molecules-on-cells-with-molphenix) - a foundation model that can predict the effect of any given molecule and concentration pair on phenotypic cell assays and cell morphology.


- [Boltz-2](https://www.rxrx.ai/boltz-2) - an open-source model that Recursion developed in partnership with MIT, that predicts both the 3D structure of protein-ligand complexes and their binding affinity — two critical factors in early-stage drug discovery – far more efficiently than traditional physics-based approaches.


### Making the Most of Experimental Validation


Lab experiments still play a critical role – but primarily in instances where Recursion needs additional confidence in the model’s predictions.


This active learning paradigm creates a constant, iterative feedback loop:


- The model makes predictions and determines its level of uncertainty.
- Experiments are selected and run where uncertainty is highest.
- The results of these experiments are fed back into the model, retraining it and reducing uncertainty for future predictions.


We employ the same resource-efficient approach when it comes to our predictive chemistry workflows. Our models are equipped with uncertainty estimation – and during the closed-loop design-make-test-analyze (DMTA) workflows, the model autonomously suggests new compounds to test, prioritizing those that will most improve its understanding and performance.


Only the molecules that are most likely to yield the most learning are selected for experimental validation, maximizing the value of each experiment.


“Ideally, our computational models are giving us enough confidence so that we can rule experiments in or out based on those predictions,” says Dan Cohen, President of Valence Labs, Recursion’s AI research engine.


### What’s Next for Virtual Cells


Recursion sees virtual cells as the first step toward even more complex predictions – ultimately scaling up to virtual organs and patients. It’s an ambitious goal, but by using the same approach – predict, explain, discover – there are already signs that we can create simulations that not only allow us to better understand diseases and how they arise – but can point us to more effective ways to treat them.


‍


------------------------


### KEY QUESTIONS:


**1** . **How does Recursion define the virtual cell?** Recursion defines the virtual cell as a computational system that can accurately simulate patients’ responses to therapeutic interventions.


**2. Why did Recursion choose to build its own datasets instead of relying on public ones?** While public datasets provide valuable insights, they often lack standardization and can be biased toward certain populations or protein types. To train AI models capable of generating novel biological insights and create a competitive moat, Recursion needed fit-for-purpose data that was relatable, standardized, and repeatable. This led to the creation of their own massive, proprietary datasets, starting with phenomics and expanding to other layers, including transcriptomics, ADME, and patient data.


**3. What are the different data layers Recursion is using to build a virtual cell?** Recursion collects multiple layers of data to reconstruct cellular activity. These include:


- **Phenomics (Cell Painting and Brightfield imaging):** Images capturing the physical changes in cells under perturbation.
- **Transcriptomics:** High-throughput RNA expression data generated via their Trekseq platform.
- **Real-World Patient Data:** Genomic and longitudinal medical record data obtained through partnerships with companies like Tempus and Helix.


**4. How does Recursion define a successful virtual cell?** According to their recent paper, a virtual cell shouldn't just *predict* how cells respond to perturbations; it must also *explain* the reasoning behind those predictions in mechanistic terms. This "Predict, Explain, Discover" framework is crucial for building trust in the model and expanding biological understanding, moving beyond simple black-box predictions.


**5. What role do lab experiments play if the goal is virtual modeling?** Lab experiments remain critical for validating predictions but are used more strategically in an "active learning" loop. Instead of testing everything, the AI models identify areas where their predictions are uncertain. Experiments are then run specifically to address those uncertainties. The results are fed back into the model to retrain it, making it smarter and reducing the need for future physical testing.


‍


------------------------


*Author:*[Brita Belli](https://www.linkedin.com/in/brita-belli/) *, Senior Communications Manager, Recursion*


‍
