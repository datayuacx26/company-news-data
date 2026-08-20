---
schema_version: "1.0.0"
document_id: "edb1cc2113d005c85f9f3c98994eeeac9a1309588565b7ff8db8c87276dec138"
company_key: "ionq-inc-common-stock"
company: "IonQ Inc."
source_id: "ionq-inc-common-stock-news-import-8f7ead1ae5e6"
canonical_url: "https://www.ionq.com/blog/demystifying-logical-qubits-and-fault-tolerance"
published_at: "2025-11-20T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:28.377709+00:00"
fetched_at: "2026-07-28T22:25:11.092082+00:00"
content_hash: "sha256:5a96ee8a7951d8c183ca63291454f3b94bb11d4f378fd125ed14d68c5171d9ad"
---

# Demystifying Logical Qubits and Fault Tolerance

## Introduction


Emerging industries often race ahead so quickly that they forget to clearly define their own terms. Quantum computing is no exception. Two of the most important, yet most misunderstood terms are **logical qubits** and **fault tolerance** . These concepts are central to building truly useful quantum computers, yet confusion around their meaning slows understanding and adoption.


This article explains what logical qubits are, why they are difficult to create, how they relate to fault tolerance, and how IonQ is approaching this challenge.


The main takeaways from this article are:


- Logical qubits are essential for fault-tolerant quantum computing, but they are hard to build well.
- Logical qubits differ in efficiency, quality, and usefulness — not all are equal.
- Poor physical qubits lead to poor logical qubits; rushing ahead can backfire.
- Improving physical qubits multiplies the benefits of logical ones.
- IonQ’s focus on ultra-high-fidelity trapped-ion qubits provides a strong foundation for logical qubits and fault tolerance.


## What are logical qubits?


The idea dates to[Peter Shor’s 1995 paper](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.52.R2493) on quantum error correction: by grouping several **physical qubits** together, we can form a more reliable **logical qubit** that resists errors in storing information (memory) and performing operations (gates). Logical qubits are essentially “software-defined” qubits built from relatively noisy hardware.


A year later, Shor also[introduced fault tolerance](https://arxiv.org/pdf/quant-ph/9605011) — computation that still works correctly even when errors occur. Later proofs, like the[threshold theorem](https://arxiv.org/abs/quant-ph/9906129) , showed that as long as error rates are below a certain limit, large-scale error-corrected computation is possible.


Today, physical quantum computers exist worldwide, each with different hardware approaches. All aim toward fault tolerance, but the terms are often used loosely, fueling hype and confusion. Recent announcements of “first logical qubits” have made it seem like error-free computation is already here, but the reality is more nuanced.


Today’s quantum systems are still in the[NISQ era](https://arxiv.org/abs/1801.00862) (Noisy Intermediate-Scale Quantum), where we have relatively few qubits and noise is ever-present. Logical qubits promise a path beyond NISQ, but early implementations may be expensive, slow, or even perform worse than the best physical qubits.


## Not all logical qubits are the same


It’s easy to assume that any logical qubit is automatically better than a physical one. But implementations vary widely, and many logical qubit demonstrations showcase only a single aspect rather than a balanced, useful system.


To truly assess logical qubits, five attributes should be considered:


1. **Overhead (physical-to-logical ratio):** How many physical qubits are needed per logical qubit? Lower overhead means better efficiency and lower energy use. With high-quality physical qubits, fewer are needed.
2. **Idle logical error rate:** How likely a logical qubit is to fail while storing information. This depends directly on the fidelity of the underlying physical qubits.
3. **Logical gate fidelity:** Accuracy of operations on logical qubits. High fidelity is critical for useful algorithms.
4. **Logical gate speed:** How quickly logical operations can be performed. Speed affects the range of feasible applications.
5. **Logical gate set (universality):** Whether the system supports a complete set of quantum operations (e.g., Clifford + T gates). Without universality, applications are very limited.


A logical qubit can therefore be worse than the physical ones it’s built from — more error-prone, slower, or restricted to a narrow set of gates. Many current demonstrations fall into this category, showing progress but not yet providing practical utility. As can be seen in IonQ’s technology roadmap (below), it is well-positioned to get to fully functional logical qubits at a higher scale significantly sooner than others, since the underlying foundation is more suited to this application.


## Achieving fault tolerance


A common misconception is that fault tolerance is binary — that you simply “turn it on” and errors disappear. In reality, it’s a spectrum. Different applications can tolerate different amounts of noise, and “enough” fault tolerance depends on the task.


For example, some optimization algorithms actually benefit from small amounts of randomness, while large-scale simulations of chemistry may require error rates near 10⁻¹⁵ per operation.


A more useful way to think about fault tolerance is as a **noise budget** : each application requires a target logical error rate. Architects can then adjust encoding schemes to balance overhead, fidelity, and performance to meet that budget.


Fault tolerance, then, is not a single milestone but a gradient of progress. Early logical qubits will not suddenly end the NISQ era — but as physical qubits improve, the efficiency and performance of logical qubits will rise, eventually enabling large-scale fault-tolerant algorithms.


## IonQ's natural advantages


IonQ has built its roadmap on a simple principle: **start with the best possible physical qubits.**


Trapped atomic ions are “nature’s qubit”: identical, stable, and naturally isolated from noise when suspended in electromagnetic fields inside an ultra-high vacuum. Among ion species, IonQ chose **Barium** , which offers advantages like reduced photon scattering, lower heating, and simpler state preparation and measurement.


Through its acquisition of Oxford Ionics, IonQ now has access to record-high two-qubit gate fidelities of **99.99%** — higher than any logical qubit demonstrated so far. This means that even a 100-qubit physical system can outperform much larger systems built from thousands of lower-quality qubits arranged into 100 logical ones.


Logical qubits built on such a foundation multiply the advantages: fewer resources, faster operation, lower energy, and better universality.


## IonQ's approach to logical qubits


IonQ’s strategy for scaling logical qubits rests on four key technical choices:


1. **Maximize physical qubit quality** to minimize overhead.
2. **Use 2D arrays of ions** with all-to-all connectivity.
3. **Build modular systems,** where each array forms a unit.
4. **Link modules** to achieve the desired computational size.


A recent[IonQ paper](https://arxiv.org/abs/2503.22071) introduced new error correction codes called **BB5 codes** , variants of[bivariate bicycle (BB) codes](https://arxiv.org/abs/2308.07915) . These codes demonstrated logical error rates four times lower than earlier versions, with far fewer qubits required than surface codes. For example, BB5 achieved the same logical error rate as a distance-7 surface code while using only about a quarter of the qubits.


If paired with Barium ions’ full potential fidelity, BB5 could reach effective fidelities above **99.9995%** . In quantum computation, small margins compound across millions of operations, determining whether algorithms succeed or fail.


IonQ continues to refine error-correction schemes, publishing advances in noise reduction and logical code efficiency. The goal is not to chase one metric but to balance fidelity, connectivity, shuttling, and gate speeds — the perfect combination for supporting real industry applications.


Logical qubits are incredible, but they come with costs and trade-offs, and their usefulness depends heavily on the quality of the physical qubits beneath them. Fault tolerance is not a switch but a spectrum. The industry will pass through many intermediate steps where different applications become practical at different levels of error suppression, opening new opportunities for quantum computing to make a giant impact.


IonQ is starting with the world’s best physical qubits — high-fidelity Barium ions — and uses precision microwaves to control them. These choices will make every technical step more efficient, ultimately leading to scalable, fault-tolerant quantum computing sooner than competing approaches.


Read more logical qubits and fault tolerance in our Resource Center:
