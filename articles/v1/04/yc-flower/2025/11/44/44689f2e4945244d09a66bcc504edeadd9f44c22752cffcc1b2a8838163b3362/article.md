---
schema_version: "1.0.0"
document_id: "44689f2e4945244d09a66bcc504edeadd9f44c22752cffcc1b2a8838163b3362"
company_key: "yc-flower"
company: "Flower"
source_id: "yc-flower-news-import-15689314081b"
canonical_url: "https://flower.ai/blog/2025-11-19-quantum-federated-learning"
published_at: "2025-11-19T00:00:00+00:00"
first_seen_at: "2026-07-21T20:31:29.302072+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:6f3ad9725594f6b7713b862b27687a1f527ffd01b509c067d474615bfc19d6e1"
---

# Quantum Federated Learning using Flower

Quantum computing is reshaping how we think about data representation and learning efficiency. By combining it with federated learning, researchers can explore privacy-preserving collaboration enhanced by quantum models.


### Why Quantum Federated Learning?


Traditional Federated Learning (FL) enables distributed model training without sharing raw data, allowing multiple clients to collaboratively improve a global model while preserving privacy. However, as modern datasets grow in size and complexity, training on classical models can become slow and resource intensive.


Quantum computing offers a promising solution through parameterized quantum circuits (PQCs), which can represent complex data distributions in exponentially larger Hilbert spaces. In a hybrid quantum-classical neural network, one or more classical layers are replaced with PQCs, allowing more expressive feature transformations that can achieve comparable accuracy with fewer parameters in certain tasks.


Quantum Federated Learning (QFL) integrates these principles by combining FL's decentralized training with the expressive power of quantum models. Each client trains a hybrid quantum-classical model locally, and the global server aggregates updates using Flower’s flexible strategies such as FedAvg. This framework preserves data privacy while exploring the quantum advantage in distributed learning systems.


For more details on how to run QFL using Flower try out the[quickstart-pennylane](https://github.com/flwrlabs/flower/tree/main/examples/quickstart-pennylane) Flower example directly.


### QUAFFLE: Quantum U-Net Assisted Federated Learning and Estimation


One practical application of quantum federated learning is in flood analysis, a field that faces significant challenges such as large datasets that are computationally intensive to train, diverse data sources (such as optical, radar, or camera images), and regional variability where models trained in one region often fail to generalize elsewhere.


QUAFFLE, developed by our team at SECQUOIA in Purdue University, addresses these challenges by integrating federated learning, quantum computing, and climate science into a unified framework. View the project on[NASA's gallery](https://www.nasa-beyond-challenge.org/project-gallery/secquoia) .


Traditional neural networks for flood mapping struggle to generalize across regions due to data heterogeneity and access limitations. QUAFFLE introduces a hybrid quantum-classical U-Net, where the vertex layer of the U-Net is replaced with a PQC. Each federated client trains its own local hybrid quantum U-Net on optical and radar imagery, while the Flower server aggregates these model updates using federated averaging. This allows collaboration across regions without sharing sensitive raw data.


This architecture offers several advantages:


- **Flexible** : Leverages data of different types and regions and is easily generalized to other applications.
- **Efficient** : Reduces the computational burden for the central server and uses fewer training parameters.
- **Private** : Federated learning preserves the privacy of these massive datasets, overcoming geographic and jurisdictional barriers.
- **Implementation-ready** : Using a quantum circuit at the vertex, which is the lowest-dimensional layer of the U-Net, requires only a few qubits, making it practical for current NISQ (Noisy Intermediate-Scale Quantum) devices.
- **Future-proof** : Doesn't depend on a specific hardware and is compatible with multiple quantum platforms.


This work was selected as a finalist in NASA's Beyond the Algorithm Challenge, a national competition where researchers develop non-conventional computing approaches for flood mapping and disaster response, showcasing Quantum Federated Learning's potential impact on real-world environmental analysis.


### Federated Learning at SECQUOIA


Beyond flood analysis, our team at[SECQUOIA](https://secquoia.github.io/) has advanced the use of Classical and Quantum Federated Learning across several scientific domains. Building on Flower's adaptable federated architecture, these efforts demonstrate how FL can extend to fields such as secure distributed computing, healthcare, and chemical engineering.


In secure QFL, we introduced Multimodal Quantum Federated Learning with Fully Homomorphic Encryption (MQFL-FHE), a framework that integrates quantum computing with encrypted federated aggregation to preserve privacy while maintaining model performance ([Dutta et al., 2024](https://arxiv.org/abs/2412.01858) ).


In healthcare, we proposed Federated Hierarchical Tensor Networks, a quantum-assisted architecture designed to enhance data efficiency and collaboration in medical imaging and diagnostics ([Bhatia & Bernal Neira, 2024](https://arxiv.org/abs/2405.07735) ). This aligns with Flower's ongoing applications of federated learning in healthcare, as showcased in[Flower's Healthcare Use Cases](https://flower.ai/industries/healthcare) .


In chemical engineering, we have applied Federated Learning to tasks such as manufacturing optimization, multimodal data integration, and drug discovery, and created hands-on tutorials to help engineers adopt frameworks like Flower ([Dutta et al., 2024](https://arxiv.org/abs/2412.01858) ). These efforts build upon our earlier work in privacy-preserving FL frameworks for chemical engineering, featured in the Flower & Chemical Engineering[video tutorial](https://www.youtube.com/watch?v=Iicg-EAqODI&t=362s) .


### The Quantum Federated Learning Example in Flower


In our new[Quickstart Quantum Federated Learning example](https://github.com/flwrlabs/flower/tree/main/examples/quickstart-pennylane) , we demonstrate how to train a hybrid quantum-classical neural network on the CIFAR-10 dataset, containing 60,000 color images across 10 classes, with[PennyLane](https://github.com/PennyLaneAI/pennylane) and Flower. PennyLane is an open-source library for building and training quantum machine learning models. The project runs entirely on a quantum simulator by default (so no quantum hardware is required), and the device can be swapped by changing the PennyLane backend.


The hybrid model used in this tutorial combines classical convolutional feature extraction with a quantum-enhanced decision layer. The network begins with a standard Convolutional Neural Network (CNN) with two convolutional layers followed by fully connected layers to extract visual features from the CIFAR-10 images. The final dense layer reduces these features to match the number of qubits, feeding them into a PQC implemented in PennyLane. In the federated setup, each client trains its own model locally on different CIFAR-10 subsets. The Flower server then aggregates all client updates using FedAvg, forming a global hybrid model that’s redistributed for the next round.


We encourage you to experiment with the tutorial by adjusting the number of clients, qubits, or circuit depth to see how they affect learning performance and convergence!
