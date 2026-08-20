---
schema_version: "1.0.0"
document_id: "0cafac66ff14e486a8ed89f251370ef0a05a48bdfd0b55df0a92dba63f7f0c8d"
company_key: "d-wave-quantum-inc-common-shares"
company: "D-Wave Quantum Inc. Common Shares"
source_id: "d-wave-quantum-inc-common-shares-news-import-84eae6d0b7ca"
canonical_url: "https://www.dwavequantum.com/learn/blog/posts/why-d-wave-s-new-two-qubit-gate-is-a-breakthrough-for-quantum-error-correction/"
published_at: null
first_seen_at: "2026-08-08T01:31:38.618718+00:00"
fetched_at: "2026-08-08T01:31:40.095850+00:00"
content_hash: "sha256:2b5765901254e19ce244255d4231448ca91c3ccb917cd840624f54b88eab22af"
---

# Why D-Wave's Two-Qubit Gate Matters for Quantum Error Correction

## Article Highlights


New peer-reviewed research published in Nature demonstrates that the error-correction advantages of D-Wave's dual-rail qubit architecture are preserved during a fast, high-fidelity two-qubit entangling gate. Here’s why it matters:


- **Quantum error correction is one of the greatest challenges** to building utility-scale gate-model quantum computers.


- **The research advances the path to practical, fault-tolerant gate-model quantum computing,** reducing the immense quantum and classical hardware overhead typically required to detect and correct quantum errors as systems scale.


- **In D-Wave's dual-rail architecture, the most common errors are also the easiest to correct.** This favorable error hierarchy is preserved during fast, high-fidelity two-qubit operations, combining the high-speed operations that superconducting systems are known for with the high fidelity required for scalable fault-tolerant systems.


**The entangling gate demonstrated in this research is already integrated into D-Wave's gate-model systems** and is a foundational component of the company’s development roadmap. To begin exploring the error-aware programming capabilities of dual-rail qubits,[request access](https://explore.dwavequantum.com/simulator?utm_source=event&utm_medium=postcard&utm_campaign=qubitseurope26&utm_content=simulatorlp&_gl=1*jdhof6*_gcl_au*MTA0MTE1MDUxNi4xNzg0MDU1MDY5*_ga*NDMwMDYxNDA5LjE3NzYxOTg1NTU.*_ga_DXNKH9HE3W*czE3ODU1MDk2NDMkbzEyMyRnMCR0MTc4NTUwOTY0MyRqNjAkbDAkaDA.) to D-Wave's forthcoming gate-model simulator.


*By Kevin Chou, James Teoh, and Nitish Mehta*


In a new peer-reviewed paper published in


*Nature* , our team details a fast, high-fidelity two-qubit entangling gate for D-Wave's superconducting dual-rail qubits, a fundamental building block of quantum computation. The research marks a major milestone in supporting efficient quantum error correction (QEC), a central challenge to scaling fault-tolerant gate-model quantum computers.


The paper, "[An entangling gate for dual-rail erasure qubits](https://www.nature.com/articles/s41586-026-10822-y) ," demonstrates approximately 99.9% fidelity for two-qubit operations with fast gate times of about 500 nanoseconds, enabled by the native hardware-level error detection of the dual-rail qubit. These peer-reviewed results validate that D-Wave's dual-rail architecture combines the fast operation speeds that superconducting systems are known for with the high fidelity required for scalable fault-tolerant systems.


In addition, our results show that the favorable error hierarchy of D-Wave's dual-rail qubits is preserved during fast, two-qubit operations, meaning that the most common errors are also the easiest to correct. Simulations leveraging the published results indicate that this favorable error hierarchy could substantially improve QEC efficiency and reduce the physical and classical hardware overhead required as systems scale.


This work matters because efficient QEC is a critical challenge to scaling fault-tolerant gate-model quantum computers. Not only do operations need to be conducted quickly and accurately, but the errors generated during those operations need to remain detectable and efficiently correctable.


With this demonstration, we complete the core set of operations required for computation with our dual-rail qubits. The result enables D-Wave scientists to investigate increasingly complex circuits, real-time erasure detection and full quantum-error correction protocols using dual-rail qubits.


## **Why is Quantum Error Correction Critical to Scaling Gate-Model Quantum Computers?**


While quantum computers have the potential to be incredibly powerful, they are also highly sensitive to noise. These small disturbances can result in errors that, if left unchecked, cause the entire computation to fail. Fortunately, with QEC, we can detect and correct these errors.


In QEC, quantum information is not stored in a single physical qubit. Instead, it is redundantly encoded across many physical qubits to form a logical qubit. This redundancy allows the system to identify and correct a certain number of physical-qubit errors while preserving the encoded quantum information.


However, this method for QEC requires a substantial increase in the number of physical qubits and classical hardware needed to construct even a single logical qubit. That additional overhead increases the complexity, cost, and performance demands of the overall system, making efficient error correction a central challenge in scaling fault-tolerant gate-model quantum computers.


D-Wave's dual-rail qubits address this challenge differently, detecting the most common quantum errors at the hardware level. This built-in error detection makes QEC more efficient by reducing the quantum and classical hardware resources needed to identify and correct errors as systems scale, advancing the path to practical, fault-tolerant gate-model quantum computing.


## **Why Dual-Rail Qubits are a Powerful New Paradigm for Quantum Error Correction**


To understand why this research represents a breakthrough for QEC, it helps to understand how dual-rail qubits work.


D-Wave's dual-rail qubits are superconducting quantum devices that create, store, and manipulate quantum information to perform computations. Unlike other quantum computing architectures, the dual-rail qubit is designed to detect errors at the hardware level.


D-Wave's dual-rail


qubits


store quantum


information using two superconducting microwave cavities that together hold a single photon between them.


The


computational


states are either a photon in the


left cavity (and no photon in the


right),


represented by


∣0⟩


,


or a photon in


the right cavity (and no photon in the


left) represent


ed by


∣1⟩


.


Like other qubits,


a


dual-rail qubit can also be placed in a superposition, or a combination of


∣0⟩


and


∣1⟩


at the same time, labeled


∣0⟩


+∣1⟩


in Figure 1 below.


Figure 1 — The location of the photon determines the computational state of the dual-rail qubit. If the photon is in neither cavity, the qubit will flag an error known as an erasure.


A key advantage of D-Wave's dual-rail qubits is their ability to detect the most common errors as they occur. The photon carrying the quantum information can occasionally be lost because of tiny imperfections in the hardware or interactions with the surrounding environment, causing errors. When that happens, neither cavity contains a photon. D-Wave's dual-rail qubit can efficiently detect if the photon is lost and flag that an error has occurred. This detected photon loss is known as an *erasure* .


Erasures are the dominant error channel in the dual-rail qubit, accounting for approximately 90% of errors when the qubit is idling. By contrast, harder-to-detect errors, such as phase-flip errors (|0⟩+|1⟩↔|0⟩-|1⟩) or bit-flip errors (|0⟩↔|1⟩),


occur much less frequently. This creates a favorable error hierarchy for QEC.


Noise is inevitable in quantum systems, but erasures are generally easier to manage than errors whose locations are unknown. A simple classical repetition code shows why.


Figure 2 — Comparing bit-flip errors and erasures in a classical repetition code. While it takes 3 bit-flip errors to cause a logical error, we require 5 erasures to induce a logical error in the case of erasures.


A bit-flip error changes a bit of information from a 0 to a 1, or vice versa. To protect the information, we encode it redundantly as a logical bit using multiple physical bits. For example, Figure 2 uses "00000" and "11111" to encode logical 0 and 1, respectively (this is not a full error correction code, just a toy example). This encoding tolerates up to two bit-flips and still decodes correctly (Figure 2a), but three flips cause a decoding error (Figure 2b).


Now consider erasures, in which the location of each corrupted bit is known (represented as * in Figure 2c). Here, even three erasures can still be decoded correctly. In this five-bit example, all five bits would need to be erased before that information is lost (Figure 2d). In general, a code that corrects up to


*n* bit flips can correct up to


*2n* erasures, since an erasure's location is known but a flip's location is not. The same principle applies to QEC. Knowing where an error occurred gives the decoder more information and makes the error easier to correct.


This example illustrates a simple yet powerful design principle: some errors are easier to correct than others. Dual-rail qubits are designed so that detectable erasures dominate, while harder-to-detect errors—such as bit- and phase-flip errors—occur much less frequently. This arrangement is what we call the


*error hierarchy* and is a central property of our dual-rail qubits. It is essential to preserve this error hierarchy across all gates and operations, a property that we confirm in this research.


## **Building a Two-Qubit Gate That Preserves Error Hierarchy**


The gate demonstrated in the


*Nature* paper is called a controlled-Z (CZ) gate. This two-qubit operation applies a phase when both qubits are in a particular computational state, creating the entanglement required for quantum computation. It's closely related to the well-known controlled-NOT (CNOT) gate, which links two qubits in a similar way. Both are considered fundamental building blocks of any gate-based quantum computer.


Engineering a high-performance entangling gate is one of the most difficult challenges for any qubit platform. The difficulty is not necessarily in generating entanglement but doing so quickly and accurately between a selected pair of qubits and only those two qubits. The implementation seen in Figure 3 satisfies this requirement.


Figure 3 — Dual-rail qubits and the Swap-Wait-Swap CZ gate. a. Schematic of two dual-rail qubits used to implement a CZ gate. b. Gate sequence for the Swap-Wait-Swap gate. c. Evolution of each computational state through the gate, noting that only one of the four computational states acquires a phase.


The gate works by turning on and off the entangling interactions by swapping a photon from one of the dual-rail cavities into a coupler that bridges two different dual-rail qubits. We wait for the entangling interaction to perform a CZ gate, and then we swap the photon back into the original cavity to turn off the interaction. We call this sequence *Swap-Wait-Swap* in the paper, and its strength lies in its simplicity.


By industry-standard two-qubit gate benchmarks, our CZ gate works with low error rates and a fast gate time (500 ns). But perhaps more importantly, our CZ gate preserves all the properties of the error hierarchy that make dual-rail qubits good for QEC: erasures form the majority of the total errors at ~0.5%, dephasing errors are lower at 0.1%, and bit-flip errors are vanishingly infrequent, at the parts-per-million level. The


*Nature* paper confirms that our CZ gate preserves the error hierarchy, as erasures are still the most likely error in our system.


## **What Comes Next?**


The entangling gate demonstrated in this research is already integrated into D-Wave ™


gate-model systems, where it is delivering comparable performance. This is significant because the dual-rail architecture, and its ability to make dominant errors directly detectable, is the foundation of our gate-model development program.


The next phase of our research will explore how the dual-rail qubit performs in practice for QEC, in which the team will investigate how this two-qubit gate would perform in an error-corrected quantum computer. Using today’s hardware performance as a benchmark, our simulations predict substantially improved error-correction performance and scaling.


This upcoming research will continue to support D-Wave's recently announced


[gate-model development roadmap](https://www.dwavequantum.com/company/newsroom/press-release/d-wave-charts-a-new-course-to-fault-tolerant-quantum-computing-with-gate-model-roadmap/) , in which we plan to release a system with 100 logical qubits, capable of successfully performing more than 1 million operations, by 2032. The roadmap combines the error awareness of D-Wave's superconducting dual-rail architecture with our integrated cryogenic control technology to support what we believe will be a fast, efficient, and achievable path to commercial gate-model quantum computing.


Our forthcoming gate-model simulator will allow developers to begin exploring error-aware programming. Developers will be able to investigate how applications can respond to erasures and begin building expertise relevant for future dual-rail gate-model quantum systems.


Prepare for Error-Aware Quantum Computing


Request access to D-Wave's forthcoming gate-model simulator and be among the first to experience its error-aware programming capabilities.


[Learn More](https://explore.dwavequantum.com/simulator)


**Share this article**
