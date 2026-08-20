---
schema_version: "1.0.0"
document_id: "e3f0ab013fd885378de9f14a9dd7188065ba61021f67f8720bef3e4c26d6d98b"
company_key: "ionq-inc-common-stock"
company: "IonQ Inc."
source_id: "ionq-inc-common-stock-news-import-8f7ead1ae5e6"
canonical_url: "https://www.ionq.com/blog/advancing-quantum-computing-faster-mixed-species-gates-for-trapped-ion"
published_at: "2025-03-05T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:28.377709+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:3d285f4942f1390295927bd6501c5803b57f25228f45144926ebbd72b9cf15bc"
---

# Advancing Quantum Computing: Faster Mixed-Species Gates for Trapped-Ion Quantum Networking

The steady execution against IonQ’s technological roadmap does not rely on monumental breakthroughs that are out of reach using today’s science. Rather, planned improvements to speed and efficiency of IonQ’s trapped-ion quantum computers are expected to take place through the continued commercialization of quantum research.


Increasing the speed of quantum gates – without sacrificing fidelity – for quantum computing and networking is a continuous objective for IonQ. New research from IonQ shows significant progress against this important goal, which is expected to ultimately enable quantum networking between and within quantum computers.


In a recent IonQ academic paper, “Fast Mixed-Species Quantum Logic Gates for Trapped-Ion Quantum Networks,” we have achieved a significant milestone in the potential for fast mixed-species trapped-ion qubits. This research, executed in partnership with Australian National University, provides a strong foundation for future experimental validation and practical implementation.


## Why Mixed-Species Gates Matter


Trapped-ion quantum computers store and manipulate quantum information using charged atomic particles held in place by electromagnetic fields. While same-species ion pairs have been used successfully as qubits to perform quantum logic operations, mixed-species ion gates enable more versatile, large-scale, architectures. “Mixed-species gates” refer to quantum operations between ions of different atomic elements or isotopes. Mixed species are especially common in network architectures where the communication qubit species is different from the computation qubit species. In this research, specific pairings include Barium (Ba), Calcium (Ca), Ytterbium (Yb), and Beryllium (Be), with combinations such as 133Ba⁺–138Ba⁺ and 43Ca⁺–88Sr⁺.


By integrating different species, researchers can create hybrid quantum architectures that optimize for scalability, reliability, improved fidelity, and efficient entanglement distribution. Different ion species are chosen strategically and can serve specialized roles: some are ideal for memory storage, others for computation, and others for networking via ion-photon entanglement. Efficiently linking these different qubits while maintaining fast and reliable operations is key to building large-scale quantum systems.


Historically, the so-called Lamb-Dicke regime has supported mixed-species entangling gate speed rates of around 10 kHz. IonQ’s latest research introduces a novel, alternative approach that enables faster speeds using ultrafast state-dependent kicks (SDKs) delivered via nanosecond-scale laser pulses. These SDKs, an industry-first, could allow quantum logic gates to operate at much faster megahertz (MHz) speeds.


## The Science Behind the Speed


By delivering precisely timed SDKs, the researchers propose a method to create entangling operations orders of magnitude faster than conventional methods. Specifically, the study predicts that two-qubit operations could be performed in hundreds of nanoseconds – orders of magnitude faster – than the tens or hundreds of microseconds as seen in previous mixed-species experiments.


A key innovation in this work is the engineering of optimized pulse sequences to counteract motional decoherence and other sources of error. Unlike previously demonstrated mixed-species gates, which require extensive cooling and error correction overhead, this new approach takes advantage of impulsive SDKs to induce state-dependent displacements in ion motion. The SDK mechanism effectively excites and de-excites motional modes, ensuring that entanglement is established without residual motional heating. This innovation by IonQ marks an industry-first for designing SDKs-based gates for mixed species.


## Key Findings and Implications


1. **Faster Time-to-Solution:** The development of fast quantum logic gates based on SDKs can enable faster overall time-to-solution for applications as a function of increased gate speeds, improved networking fidelities, and efficient use of qubits.
2. **Theoretical Sub-Microsecond Gate Speeds** – Simulations suggest that mixed-species entangling gates can operate at MHz rates, two to three orders-of-magnitude improvement over traditional spectroscopic gates. This speed increase could enable real-time quantum operations that need error correction steps, significantly enhancing the efficiency of computational workloads.
3. **High Fidelity Potential** – The technique is theoretically robust against pulse timing jitter and mode-frequency drifts, and in simulations, gate infidelities as low as 10⁻⁴ have been achieved. This level of precision is essential for maintaining coherence in large-scale quantum systems, making these gates viable candidates for fault-tolerant quantum computing.
4. **Scalability** – Faster gates could enable deeper quantum circuits and more efficient error correction. The reduced operational time per gate translates to improved overall system performance, making it feasible to execute complex algorithms with fewer decoherence-related constraints.
5. **Error Resilience** – The SDK approach could mitigate common sources of decoherence, such as anomalous heating and motional coupling instabilities. By reducing error sources at the physical level, these gates could improve the reliability of quantum computations and reduce the need for extensive error correction measures.
6. **Quantum Networking and Distributed Computing Enhancement** – If experimentally validated, the rapid transfer of quantum information between network and memory qubits could improve entanglement rates and fidelities between networked nodes. This advancement would facilitate more efficient communication between quantum nodes, laying the groundwork for a robust, high-speed quantum internet.


## A Path Toward Scalable Quantum Networks and Distributed Computing


One of the most exciting applications of this research is next-generation quantum networking and distributed quantum computing. Future IonQ systems are expected to implement photonic interconnects to link multiple quantum processors. These photonic networking entangling operations require gates in the range of 1-10kHz. The newly proposed fast gates theoretically operate well within this limit, allowing rapid and high-fidelity transfer of quantum information between network and memory qubits.


This method also aims to reduce ion-photon entanglement loss, a key step in scaling distributed quantum systems. By enabling near-instantaneous quantum state transfer, the SDK protocol could help minimize network latency and improve error correction across quantum nodes, leading to:


- **More Speed and Reliability:** Faster and more reliable and efficient quantum networking between and within quantum computers across long distances
- **Hybrid Quantum Networks and Architectures:** Successful deployment of hybrid quantum computing systems that connect multiple nodes of different qubit species to address different needs best suited for each species
- **Scalability and Error Correction:** Research motivated by our R&D roadmap and stated scaling strategy, namely networking trapped-ion processors together via photonic interconnects.


## What’s Next for IonQ and Quantum Computing?


While these results remain theoretical, IonQ has already filed a provisional patent covering the invention and expects to run experimental demonstrations soon. Once successfully implemented in hardware, fast mixed-species gates could significantly enhance the speed and efficiency of trapped-ion quantum computers, furthering their advantage in the race toward practical quantum advantage.


IonQ’s roadmap focuses on performance, scalability, and enterprise-grade quantum systems. By improving two-qubit gate speeds and fidelities, these new mixed-species gates align with all three pillars of performance, scale, and enterprise grade solutions.


Moreover, the success of this technique could have significant implications beyond trapped-ion systems, potentially influencing hybrid quantum architectures that incorporate superconducting qubits, neutral atoms, or alternative modalities.


The paper was authored by IonQ scientists Haonan Liu, Alexander K. Ratcliffe, Varun D. Vaidya and C. Ricardo Viteri, along with Australian National University scientists Phoebe Grosser, Simon A. Haine, Joseph J. Hope, Zain Mehdi and Isabelle Savill-Brown. For more details, read the full research paper here:[https://arxiv.org/abs/2412.07185](https://nam12.safelinks.protection.outlook.com/?url=https%3A%2F%2Farxiv.org%2Fabs%2F2412.07185&data=05%7C02%7CPaul.Lesinski%40edelman.com%7C1e2057b402e54727232b08dd420df70f%7Cb824bfb3918e43c2bb1cdcc1ba40a82b%7C0%7C0%7C638739349937912104%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=1mAgxT7BSpsvPurJqXXulsEtW6dZje66bGJxJx8NKRo%3D&reserved=0)
