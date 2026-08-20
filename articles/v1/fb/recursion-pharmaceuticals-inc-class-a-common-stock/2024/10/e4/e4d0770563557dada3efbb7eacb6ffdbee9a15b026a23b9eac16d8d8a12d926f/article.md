---
schema_version: "1.0.0"
document_id: "e4d0770563557dada3efbb7eacb6ffdbee9a15b026a23b9eac16d8d8a12d926f"
company_key: "recursion-pharmaceuticals-inc-class-a-common-stock"
company: "Recursion Pharmaceuticals Inc."
source_id: "recursion-pharmaceuticals-inc-class-a-common-stock-news-import-f0ced5d1ab91"
canonical_url: "https://www.recursion.com/news/learning-the-effects-of-molecules-on-cells-with-molphenix"
published_at: "2024-10-01T00:00:00+00:00"
first_seen_at: "2026-07-22T11:05:14.614366+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:0ad06913ac7192b1c1d8041f436ea57378dc6d2e1aa4f2a1005222f7ccd9201c"
---

# Learning the effects of molecules on cells with MolPhenix

Today, we’re excited to introduce MolPhenix, a foundation model that can predict the effect of any given molecule and concentration pair on phenotypic cell assays and cell morphology.[Read the paper here.](http://www.arxiv.org/abs/2409.08302)


Images are a powerful tool for unraveling the complex, interconnected network of relationships across biological systems. Contrary to regular assays that focus on a single readout, images of cells provide an extraordinary amount of information about how a cell is functioning. These information-rich representations make them much richer for subsequent ML applications. Moreover, with the help of high-throughput screening, images can be standardized and scaled to capture the cell’s responses to millions of chemical and genetic perturbations at a much cheaper and faster rate.[Phenomics](https://www.recursion.com/news/nothing-short-of-phenomenal-new-deep-learning-model-available-on-nvidias-bionemo-platform#:~:text=At%20Recursion%2C%20we%20call%20this,different%20chemical%20or%20genetic%20perturbations.) refers to the systematic study of a cell's phenotype—encompassing its observable and functional characteristics—in response to various perturbations. Using microscopy images from phenomics experiments, we can capture high-dimensional cellular information through cellular morphology, such as cell shape, size, texture, organelle organization, and subcellular structural changes. It is a rapidly growing field that equips researchers and practitioners with an additional modality to assist us in our journey of decoding biology. With MolPhenix, we leverage recent advances in contrastive machine learning (ML) to develop a novel multi-modal approach for understanding the relationship between molecules and phenomics images. Our approach allows us to map molecules and phenomics into a common space, ensuring that we can learn a rich representation of how a molecule affects the cell’s morphology, achieving a 10X improvement over prior baselines.


### **Why Cell Morphology?**


To leverage the full potential of ML in drug discovery, we must develop models that can leverage much larger and richer black box datasets. Phenomics images capture similar effects to running hundreds or thousands of assays, at a fraction of the cost, just by looking at changes in the morphology of the cells. This concept is what Prof. Michael Bronstein defined as the “Gen-3 biotech” in his blog “[The Road to Biology 2.0 Will Pass Through Black-Box Data](https://towardsdatascience.com/the-road-to-biology-2-0-will-pass-through-black-box-data-bbd00fabf959) ”.


A big part of the ML for drug discovery community focuses on building predictive models, i.e. models that are trained on numbers from a single assay such as inhibition, toxicity, solubility, etc. There are a few challenges with these assays: (1) they are very costly and thus difficult to scale, and (2) they provide only a single indication about the molecules, which do not capture intricate biological details. Although some works try to harness thousands of assays from publicly available data, there are inherent limitations to this approach. These datasets cannot be scaled continuously, and often contain a significant amount of noise due to varying experimental conditions.


Over the past decade,[Recursion](https://www.linkedin.com/company/recursion-pharmaceuticals/) has generated billions of phenomics images through[automated, high-throughput experiments](https://www.recursion.com/technology) , laying the foundation for a new path forward. Paired with new phenomics foundation models like[Phenom-1](https://arxiv.org/pdf/2404.10242) , Recursion can extract meaningful representations from these high-dimensional images to build a map a biology: a guide that allows us to navigate which molecules and genes map to the same space of morphological changes. The richness of the features captured by phenomics images and foundational models holds promise to revolutionize drug discovery.


### **Unlocking Multi-Modality in Molecular Biology**


Multi-modal approaches (i.e. Dall-E, Sora, Gemini, etc.) have unlocked incredible new capabilities in NLP and computer vision. We’ve seen shocking demos of people transforming text into images, short videos, and even movies. However, multi-modal data in molecular biology is complicated due to the interventional nature of experimental results making experimental results difficult to pair, as our colleagues have[explored in previous work](https://youtu.be/7a29mGz9LXI) .


Instead, data can be paired by linking specific molecular perturbations with corresponding high-throughput morphology experiments. This approach allows for training a model which learns how molecular changes affect cellular function. However, unique challenges arise when learning relationships between biological and chemical domains, making it more difficult than working with human-interpretable data. To address this, we identified several key issues that must be resolved to develop models leveraging multi-modality:


🎯 Generalization across microscopy images is inherently very challenging.


The phenomics experimental results are very large images of 4096x4096 pixels that contain hundreds of small cells in different configurations and are very difficult to read and understand by a human. In addition, experimental batch effects figure prominently in the readout. This is a natural consequence of trying to capture all the important aspects of variation of a non-human interpretable medium, such as cell state.


💡 Imagine camera technology a century earlier, where images contained noisy artifacts from low-quality film, overexposure, or the development process. Further, the object of interest is very small, and the background takes up most of the image.


✔️Pre-train your phenomics encoder: By leveraging foundational models like Phenom-1of phenomics, one can simplify the images into vectors, remove batch effects by normalizing with controls, and further reduce noise levels by averaging multiple replicates of the same experiments.


🎯 Most molecules are inactive


At non-toxic concentrations, most randomly sampled molecules will have no noticeable effect, meaning that the observable signal is too small for the noise.


💡 Imagine when training Dall-E to generate images, 90% of the images have a random caption. This would make it much harder for the model to learn the right signal, and standard contrastive approaches would fail.


✔️ Leverage the activity landscape: With the help of a[foundational phenomics model](https://openaccess.thecvf.com/content/CVPR2024/papers/Kraus_Masked_Autoencoders_for_Microscopy_are_Scalable_Learners_of_Cellular_Biology_CVPR_2024_paper.pdf) and the proposed S2L loss, we define an activity landscape to help over-sample the active molecules and adapt the loss function. These training modifications allow us to consider pairs to be negative if their activity landscape is different.


🎯 Molecules can be added at any concentration


Concentration plays a fundamental role in understanding how molecules interact with biological systems. To cite a famous quote from the Swiss physician Paracelsus, “All things are poison, and nothing is without poison; the dosage alone makes it, so a thing is not a poison”.


💡 To provide an intuitive analogy, in speech, meaning changes with the volume and tone. Information is encoded in how loudly a person speaks and provides important context for a situation. To encode the message of a phrase, we must also capture the associated intensity of a phrase: higher volume (yelling) or lower volume (whispering). That’s what we have to deal with in drug discovery when we vary the concentration of molecular perturbations.


✔️ Condition the loss and the molecular model on the dose: Provide the concentration as an input embedding to the model via an explicit input, but also condition the loss on also including the concentration information.


### **Through the lens of a giant**


Let’s rephrase the challenges mentioned in the previous section with the following analogy: Instead of an image of cells, imagine a giant looking at a snapshot of human society through a microscope. Just as we interact with cells via molecules, the giant interacts with humans via sound.


Phenomics is the equivalent of looking at the effect of different tunes or sounds on this image. Just like with Phenomics, the images are not taken at the same time or place, so there is variability in the population.


When playing sound, most tunes will not have any impact, and the image will only change due to the randomness of when/where the picture is taken - basically batch effects. Analogous to concentration, some tunes will have an effect at low volume, such as alarms, and others only at high volume by gathering dancing crowds. But one thing is certain: no matter the tune, increase the volume too much, and everyone will lie on the floor blocking their ears.


Let’s try some PhenoMusical retrieval. Based on this image, can you guess which music was playing? It seems impossible at first glance, but seeing that it is mostly kids that are dancing and jumping happily and that there’s a piano, we could select the top 10 jumping songs for kids, with a piano instrumental, and we would very likely find the right music.


### **MolPhenix: Contrastive PhenoMolecular Retrieval**


Contrastive PhenoMolecular Retrieval answers the question: given a phenomics experiment, can we guess which molecule it was? And vice-versa. Answering these questions will enable us to virtually screen for molecules with a desired phenomics effect.


Despite all the challenges above, we know the gold-mine of understanding the effect of drugs on cells is in front of us, and we need to start mining it! This is why we have built MolPhenix with all the design decisions needed to thrive and enable us to achieve 10X improvement over previous methods. Yes, 10X, going from 7.9% to 77.3% on the Top-1% recall of active molecules! An aligned training objective, thorough understanding of the problem, and leveraging foundation models like Phenom-1 and[MolGPS](https://www.youtube.com/watch?v=VZGgycG29Ag) , helped unlock a new area of application for ML in cell biology.


What is this Top-1% recall?


To empirically assess the quality of MolPhenix we measure Contrastive PhenoMolecular Retrieval via a recall metric. This provides a way to assess the quality of the latent space without relying on time-consuming experimental validation. In practice, we evaluate this phenomolecular retrieval using the Top-1% recall, i.e. based on a phenomics experiment, if you are allowed to select a bag of molecules comprising of 1% of the test set, how often do we find the true molecule in that bag?


### **A powerful embedding!**


Can the embeddings be used out-of-distribution to do virtual screening against other biological assays? We used KNNs (K-nearest neighbors) to virtual screen against 34 diverse assays, ranging from toxicity, to protein binding, to solubility, and found that MolPhenix embeddings are vastly superior to traditional fingerprints.


It doesn’t stop there! We also evaluated the out-of-distribution capacity of MolPhenix embeddings to predict molecular-gene interactions by inputting phenomics images of gene knockouts instead of images. We found that MolPhenix performs roughly 30-50% of real phenomics experiments despite not being trained on Gene KO.


We invite you to learn more about these results in[our paper](http://www.arxiv.org/abs/2409.08302) .


### **What’s next?**


This work lays out a future vision of how foundation models can be used to unite previously disparate modalities. By identifying domain-specific inductive biases along with powerful pre-trained foundation models, it becomes possible to unify biochemical modalities and build a unified representation. This increased flexibility unlocks the door for in-silico screening and in-silico dose-response curve estimations of phenomics readouts or other abstract biological representations. We are excited to continue the work and grow model capabilities across additional modalities and capabilities!


*Authors: Dominique Beaini, Philip Fradkin & Jonathan Hsu, MD, MAS, FACC, FAHA, FHRS*


‍
