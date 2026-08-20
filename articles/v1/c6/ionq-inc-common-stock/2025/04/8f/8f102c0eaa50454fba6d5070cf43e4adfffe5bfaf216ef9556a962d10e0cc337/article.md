---
schema_version: "1.0.0"
document_id: "8f102c0eaa50454fba6d5070cf43e4adfffe5bfaf216ef9556a962d10e0cc337"
company_key: "ionq-inc-common-stock"
company: "IonQ Inc."
source_id: "ionq-inc-common-stock-news-import-8f7ead1ae5e6"
canonical_url: "https://www.ionq.com/blog/ionq-and-leading-global-automotive-manufacturer-collaborate-to-advance"
published_at: "2025-04-30T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:28.377709+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:1e26803d695e7c2f93b1e1ad71af5c165d201a55e7fa7fe4324b6c3fb297b634"
---

# IonQ and Leading Global Automotive Manufacturer Collaborate to Advance Materials Science and Vehicle Durability Using Quantum Generative AI

## Introduction


IonQ has worked with a top Fortune 500 global automotive manufacturer to advance multiple quantum computing research initiatives, exploring real-world applications of quantum computing across areas like battery chemistry and object detection for autonomous vehicles. In earlier work, IonQ developed quantum algorithms for the company that model lithium compounds and their reactions—critical to advancing next-generation battery technologies. IonQ has also applied quantum machine learning to 3D object detection for smarter, more accurate detection in future vehicle models.


Building on this strong track record of successful collaborations, this groundbreaking new research initiative further advances quantum computing innovation. The joint[work](https://arxiv.org/abs/2504.08728) successfully demonstrated an end-to-end implementation of a quantum-classical hybrid Generative Adversarial Networks (GANs) to generate high-fidelity steel microstructure images. GANs are prominent machine learning tools and have revolutionized data augmentation and synthetic image generation, particularly in fields where acquiring large, high-quality datasets is challenging.


By integrating quantum computing with classical machine learning, the research not only explores the advantages of quantum-enhanced generative modeling but also demonstrates that the classical-quantum combination can produce superior results in image generation and data augmentation. The results demonstrate that our novel quantum hybrid approach delivers a meaningful improvement in image generation accuracy compared to the baseline classical solution: The microstructure images produced using IonQ’s hybrid QGAN method achieved a higher quality score in up to 70% of cases when compared to images produced using classical generative models.


The complete paper describing the research can be viewed on Arxiv[here](https://arxiv.org/abs/2504.08728) .


## Why Synthetic Data Matters for Steel Microstructure


Steel microstructures determine the material’s mechanical properties, including strength, ductility, and toughness. Understanding these structures is critical for designing high-performance materials in the automotive, aerospace, and other advanced manufacturing industries. However, collecting and labeling large datasets of electron backscatter diffraction (EBSD) images — a primary method for analyzing steel microstructures — is expensive, time-consuming, and in some cases, simply not possible to obtain due to material constraints or limitations in imaging technology.


Synthetic image generation can augment these datasets, improving downstream tasks such as segmentation to differentiate between different phases or grain structures and classification to identify microstructures like bainite or ferrite. A microstructure refers to a small-scale structure of a material, in this case the specific arrangements of atoms and phases within steel.


Traditional materials development can take up to 20 years to progress from initial discovery to commercialization, while the steel industry is estimated to lose billions annually due to design inefficiencies and defects. Applying quantum computing to these challenges has the potential to deliver significant cost and time savings, helping manufacturers accelerate innovation and optimize performance.


## The Automotive Manufacturing Opportunity


In automotive manufacturing, ensuring the reliability of critical engine components is essential. A machine learning model can be trained to classify “desired” or “undesired” parts based on their steel micrographs. However, a key hurdle is the limited availability of training data—the dataset is highly imbalanced, with 47,000 images of good, or desired, parts but only 3,000 images of undesired ones. This scarcity limits the model’s ability to learn accurate decision boundaries, reducing its effectiveness in real-world applications. Moreover, using classical computing to augment experimental data with synthetic generation can be expensive and time-consuming.


IonQ’s quantum hardware can help overcome these challenges with quantum-enhanced generative models to artificially generate new, high-quality images. Using a hybrid quantum-classical Generative Adversarial Network (GAN), the team created synthetic micrographs that expand the dataset while preserving the unique characteristics of ferrite and bainite steel classes. This approach not only improves model performance but also demonstrates the power of quantum computing in tackling complex data scarcity issues in industrial applications.


## The Quantum-Classical GAN Approach


Generative Adversarial Networks algorithms (or GANs) use two competing neural networks—a generator that creates synthetic data and a discriminator that evaluates its authenticity—to improve the realism of generated images. Traditional GANs rely on a fixed probability distribution for their random inputs, which can limit the diversity and accuracy of the generated samples. To overcome this limitation, the research team incorporated a Quantum-Circuit Born Machine (QCBM) algorithm to generate a learned probability distribution. This was used to modify the input of a Wasserstein GAN (WGAN), which improves training stability and image quality by using the Wasserstein distance as a loss function. The result was a quantum-enhanced GAN capable of generating high-fidelity 5-channel EBSD images of two steel phases: bainite and ferrite.


In this study, the classical computer was used to handle most of the GAN training, including optimizing the discriminator and generator networks within the Wasserstein GAN (WGAN). The Quantum-Circuit Born Machine (QCBM), executed on IonQ’s Aria trapped-ion quantum processor, was responsible for generating a learned probability distribution, which was then used as input to the WGAN. This quantum-enhanced input allowed for greater expressivity and diversity in the generated microstructure images. The quantum circuit’s parameters were optimized to further improve the generator’s performance, achieving lower Maximum Mean Discrepancy (MMD) scores compared to purely classical models, indicating superior image quality.


## Benefits of a Quantum-Hybrid Approach with IonQ’s Trapped-Ion Quantum Computer


The quantum portion of hybrid quantum-classical GAN was modeled on IonQ’s Aria trapped-ion quantum processor for its high-fidelity qubits. The quantum system parameters were tuned to balance performance and noise reduction, ensuring the efficient execution of complex quantum circuits. IonQ trapped-ion systems offer longer coherence times and lower error rates compared to some other quantum computing platforms, making them well-suited for machine learning tasks that require precise probabilistic sampling. This architecture allowed the research team to implement and test through numerical experiments the quantum component of the GAN effectively, demonstrating consistent and clear advantages over purely classical approaches. The research team validated the robustness of the methodology by integrating IonQ Aria in the end-to-end training of the entire model.


## Advancing Quantum Machine Learning with Hardware Innovation and Expertise


Quantum machine learning holds immense promise, but unlocking its full potential requires innovative approaches to maximize performance on today’s quantum hardware. This research introduces key advancements that improve the performance of quantum-enhanced generative models:


- **Optimized Quantum Circuit Architecture:** By carefully designing circuits to reduce the number of two-qubit gates, the team improves quantum state fidelity, making computations more reliable.
- **Efficient Gradient-Free Optimization:** Leveraging Simultaneous Perturbation Stochastic Approximation (SPSA), the model efficiently fine-tunes quantum parameters with minimal function calls, accelerating convergence while mitigating noise.
- **Hybrid Quantum-Classical Training:** By alternating between quantum circuit updates and classical neural network training, the approach balances the strengths of both computing paradigms, leading to more stable and scalable training.


## Key Research Findings


1. **Higher Image Quality** : The quantum-classical GAN outperformed a baseline classical GAN, producing more realistic microstructure images by improving fidelity and reducing artifacts in the generated outputs. This was evaluated using Maximum Mean Discrepancy (MMD) scores, where the quantum-enhanced model showed an improvement in image generation accuracy overall. The results varied based on dataset size and model parameters, and demonstrated higher image quality and diversity. Specifically, results demonstrated that the microstructure images produced using IonQ’s hybrid QGAN method achieved a higher quality score in up to 70% of cases when compared to images produced using classical generative models.
2. **Hardware Implementation Significance** : This marks one of the first demonstrations of a practical quantum generative model applied to real-world scientific data, showcasing the potential for quantum computing to enhance machine learning applications.


**Potential Applications**


This research shows how the quantum-classical GAN model can be used across industries to generate high-quality synthetic data, especially when real data is limited or hard to obtain. This research provides insights that could support efforts to explore how synthetic microstructure data enhances AI-driven models for material property prediction and manufacturing optimization. More broadly, this can potentially apply to:


- **Materials Science:** Creating more images of steel microstructures to better train AI models that predict material properties.
- **Medical Imaging:** Generating synthetic medical images for training AI to detect diseases, useful when patient data is scarce.
- **Astronomy:** Improving images of galaxies by filling in missing details, going beyond current deconvolution limits.
- **Particle Physics:** Creating simulated particle physics data to train AI models, helping to analyze experiments.
- **Financial Forecasting:** Improving the accuracy of financial predictions using quantum machine learning.


In the future, this model could be used to create different kinds of images or data, and to classify images using quantum computers, potentially enhancing image augmentation to mitigate data scarcity and streamline the preparation of datasets for robust high-throughput analysis.


**Conclusion**


This successful implementation of a hybrid quantum-classical GAN for steel microstructure analysis represents a significant milestone in quantum machine learning. The ability to generate high-fidelity synthetic microstructure images can enhance materials science research, accelerate AI model training, and open new avenues for quantum-enhanced machine learning.


This research highlights a practical, real-world application of quantum computing in generative modeling, achieving results that surpass previous benchmarks from other quantum modalities for this type of GAN application. As quantum hardware continues to advance, hybrid approaches like this one will unlock new capabilities in data generation, scientific computing, and industrial research.


The paper was authored by IonQ scientists Samwel Sekwao, Jason Iaconis, Claudio Girotto, and Martin Roetteler together with our collaborators. For more details, read the full technical paper here:[https://arxiv.org/abs/2504.08728](https://arxiv.org/abs/2504.08728)
