---
schema_version: "1.0.0"
document_id: "a367dd715c40d32840b73a6f0f66c99ac482a22c70eff25adb3f7a174689324a"
company_key: "ionq-inc-common-stock"
company: "IonQ Inc."
source_id: "ionq-inc-common-stock-news-import-8f7ead1ae5e6"
canonical_url: "https://www.ionq.com/blog/breaking-the-decoding-bottleneck-fast-and-accurate-software-decoding-for-quantum-ldpc-codes"
published_at: "2026-01-08T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:28.377709+00:00"
fetched_at: "2026-07-28T21:58:18.576112+00:00"
content_hash: "sha256:621497c230a99e0265b804a952ae4350da9133b22dc35216fc1902cf61a20d43"
---

# Breaking the decoding bottleneck: Fast and accurate software decoding for Quantum LDPC codes

At IonQ, our quantum error correction team is laser-focused on the ultimate destination: building a large-scale fault-tolerant quantum computer.


To get there, we need **Quantum Error Correction (QEC)** . Recall that QEC protects quantum information by forming[logical qubits](https://www.ionq.com/blog/demystifying-logical-qubits-and-fault-tolerance) , spread across many physical qubits, allowing us to detect and correct errors without destroying the calculation. However, building a fault-tolerant computer isn't just about having good qubits—it’s about having a "brain" orchestrating the correction, one that is fast enough to fix errors before they build up. This "brain" is the **decoder** .


Today, we are releasing[new research](https://arxiv.org/abs/2512.07057) that challenges a long-standing assumption in the industry: that building a large-scale fault-tolerant quantum computer would require a supercomputer to execute the decoder fast enough to correct thousands of logical qubits simultaneously. We demonstrate a new **Beam Search Decoder** that is not only simpler and more accurate than existing standards, but also proves that IonQ’s trapped ion architecture can handle fault-tolerant decoding using only standard CPUs—avoiding the need for the expensive, custom supercomputing hardware required by competing architectures.


## The challenge: The "decoding bottleneck"


In QEC, the decoder must look at a stream of error symptoms (syndromes) and deduce exactly which errors occurred on the qubits. It’s a complex logic puzzle that must be solved continuously and incredibly fast.


For years, "folklore" in the quantum industry has suggested that as we scale up, decoding will become a massive bottleneck. This is particularly true for superconducting quantum computers. Because their qubits operate on nanosecond timescales, they require decoders to make decisions in microseconds. This forces engineers to build complex, power-hungry, custom hardware (FPGAs or ASICs) just to keep up.


But at IonQ, our trapped ions operate on a different clock—typically in the millisecond regime. While often viewed as a difference in speed, this is actually a **strategic advantage** for control engineering. Our classical control software gets orders of magnitude more time to make high-quality decisions. To illustrate this advantage, our research investigates the following question.


*Could we build a decoder accurate enough for next-gen quantum error-correction codes, yet fast enough and running purely in software?*


## Beam search decoder


For the past six years, the most popular decoder has been the so-called **BP-OSD decoder** (Belief Propagation with Ordered Statistics Decoding). While accurate, BP-OSD is computationally heavy. It consumes the order of N^3 operations to decode N physical qubits, making it difficult to scale.


Our team has developed a new approach: a **Beam Search Decoder** . Instead of blindly following one guess, this algorithm explores multiple "most likely" error paths (beams) in parallel, keeping only the best candidates at each step. It combines the speed of heuristic searches with the rigorous accuracy required for fault tolerance.


Figure 1: Illustration of the Beam Search Decoder workflow. The algorithm explores multiple candidate solutions in parallel (beams), pruning unlikely paths at each step to efficiently find the most probable error correction.


## Unlocking record performance


We benchmarked this new decoder against the industry standard (BP-OSD) on[Bivariate Bicycle (BB) codes](https://arxiv.org/abs/2308.07915) —a promising class of[Quantum LDPC codes](https://www.ionq.com/blog/reimagining-error-correction-for-modular-quantum-computing) that are far more efficient than the Surface Code. The results, detailed in our latest technical paper, were transformative:


- **Higher Accuracy:** Our Beam Search decoder achieved a 17x reduction in logical error rate compared to the standard BP-OSD decoder. This is a **17x increase in logical qubit quality** .
- **Suppressing stalling events:** In real-time systems, average speed isn't enough; you need to avoid the rare "slow" cases that stall the whole system. Our decoder reduced these worst-case (99.9th percentile) runtimes by **26x** .
- **Practical decoder:** Most importantly, we demonstrated that a specific configuration (Beam Width 32) simultaneously achieves a **better accuracy** than BP-OSD and has a runtime (99.9th percentile) **under 1 millisecond** on a single core of a standard commercial CPU (Apple M3 Pro).


## The IonQ advantage: Software-defined fault tolerance


This result validates a key pillar of IonQ’s scaling strategy. Because our trap cycle times align perfectly with the 1ms runtime of this high-accuracy decoder, **IonQ can drive fault-tolerant correction using standard, flexible software.**


While most other technologies must pour resources into developing custom micro-architectures and single-purpose decoding chips just to handle their data streams, IonQ can leverage off-the-shelf CPUs. This simplifies our stack, reduces cost, and allows us to rapidly iterate on our error correction algorithms without redesigning hardware.


Based on this work, we estimate only three CPUs with 32 cores each could be enough to correct 1000 logical qubits fast enough to keep up with errors in our trapped ion qubits. This is made possible by the record fidelity of our physical qubits, which were demonstrated to achieve a fidelity > 99.99% in a[recent preprint](https://arxiv.org/abs/2510.17286v1) . Starting from better physical qubits, we inherently have fewer errors to correct, simplifying the decoder’s job. For comparison, one might need 1000 hardware (FPGA or ASIC) decoders to correct 1000 logical qubits in a surface code architecture with superconducting qubits. If one manages to build superconducting qubits equipped with many long-range couplers, one could replace surface codes by more efficient LDPC code. Then, we would still require 84 hardware decoders to correct 1000 logical qubits.


# Logical qubits Code # Code blocks # Decoders


IonQ


1,000 LDPC
\[\[144,12,12\]\] 84 3 CPUs


Superconducting qubits


1,000 Surface code 1,000 1,000 FPGAs


Superconducting qubits + long-range couplers


1,000 LDPC \[\[144,12,12\]\] 84 84 FPGAs


## Looking ahead


Efficient decoding is the unsung hero of the fault-tolerant stack. By introducing a decoder that is simultaneously simpler, faster, and more accurate than the status quo, we are removing one of the biggest roadblocks to large-scale QEC.


This research confirms that our choice of trapped-ion physics doesn't just give us better qubits—it gives us a smarter, more manageable path to engineering the control systems that will run them.


*Blog post based on*[pre-print article](https://arxiv.org/abs/2512.07057) *and the*[software implementation.](https://github.com/ionq-publications/BeamSearchDecoder)
