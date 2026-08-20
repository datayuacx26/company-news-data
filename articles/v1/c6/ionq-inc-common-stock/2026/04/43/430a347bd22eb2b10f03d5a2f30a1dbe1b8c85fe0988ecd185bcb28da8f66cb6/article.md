---
schema_version: "1.0.0"
document_id: "430a347bd22eb2b10f03d5a2f30a1dbe1b8c85fe0988ecd185bcb28da8f66cb6"
company_key: "ionq-inc-common-stock"
company: "IonQ Inc."
source_id: "ionq-inc-common-stock-news-import-8f7ead1ae5e6"
canonical_url: "https://www.ionq.com/blog/beyond-qubit-counts-introducing-ionqs-application-centric-benchmarking-framework"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:28.377709+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:d5de2c1fbe49b3c48a2bdd265464ec62ddf404d38431f0f27a0f0a88de5658f4"
---

# Beyond Qubit Counts: Introducing IonQ’s Application-Centric Benchmarking Framework

**Quantum computing needs better benchmarks.**


Whether evaluating quantum for today's workloads or tomorrow's, every serious decision in quantum computing eventually comes down to one question: how do you measure real progress, and at what cost? The industry produces no shortage of numbers to answer it. Qubit counts, gate fidelities, circuit depths, coherence times. These figures are published constantly, but none of them actually answers the question. That is the problem this framework is built to solve.


Our Benchmarking White Paper introduces a structured, application-centric framework for evaluating quantum computing systems. The framework is designed for the industry, not just IonQ. It covers 13 benchmarks across optimization, quantum chemistry, machine learning, data loading, simulation, and foundational algorithms. While this paper reports results on IonQ hardware, the framework is built to support evaluation across any quantum system, using metrics that connect directly to the value of the obtained solution.


## A Framework Built on Well-Informed Lessons


The framework is inspired by MLPerf, the established standard for AI benchmarking (managed by NVIDIA, Microsoft, Amazon, Meta, Qualcomm, AMD, Intel, Arm, and many more). The structure is clear yet flexible: “Closed benchmarks” fix the implementation so that cross-platform comparison is a fair test of the system, not the algorithm. “Open benchmarks” fix the success criterion and permit algorithmic innovation, allowing teams to demonstrate advances without disclosing proprietary methods. In both cases, each benchmark has to disclose critical information to give the results the necessary context.


The primary metrics are Time-to-Solution (TTS), Energy-to-Solution (ETS), and solution quality. TTS is the total wall time to reach a result that meets a predefined quality threshold, encompassing pre-processing, compilation, execution, and post-processing. In TTS benchmarks, that quality threshold is the figure of merit for the problem: it defines what constitutes a valid answer, not merely a fast one. ETS often follows TTS, given the hybrid nature of full solutions. If an application uses significant GPU compute capacity, as an example, that would show up in energy consumption and needs to be reported (even as an approximation based on duration).


Component-level metrics like gate fidelity, qubit count, and coherence times are critical. Building and scaling quantum computing systems depends on them. But they describe parts, not systems. When designing quantum computers, architectural choices involve real tradeoffs across these metrics. Higher gate fidelity may come at the expense of slower gate speeds. Increasing logical qubit count through error correction consumes physical qubits. Compiler optimizations interact with hardware constraints in ways that vary by workload. Application-level benchmarks capture how all of these components work in concert across the full problem stack. That is the complete picture component-level metrics alone cannot provide.


IonQ’s new benchmarking code is publicly[available here](https://github.com/ionq-publications/apps-benchmark) , implemented in Qiskit. Any partner, customer, or third party can run these benchmarks on their own systems for independent comparison.


## What the Results Show


Four results from the white paper illustrate what this kind of benchmarking reveals. Where noted, performance comparisons against other systems are drawn from independently validated testing conducted alongside the white paper. While the details below are fairly technical and nuanced, the bottom line is that IonQ's low noise and all-to-all connectivity not only contribute to high-quality results, but also yield Time-to-Solution and Energy-to-Solution metrics that are commercially meaningful, particularly against architectures where noise floors extend solution time significantly or prevent convergence altogether.


**Optimization at scale.** The Linear Ramp QAOA TTS result makes the framework concrete. On a 36-qubit, 4-regular MaxCut instance at circuit depth p = 9, IonQ Forte achieves a finite TTS at every approximation ratio threshold up to and including the optimal solution. With 5,000 shots, Forte samples 14 bitstrings that achieve the optimal cut (AR = 1.0). At AR ≥ 0.90, Forte's TTS is approximately 34 seconds. TTS is a direct measure of what it costs to obtain a qualifying solution at a given confidence level, the number that matters when quantum computing is evaluated against a real workflow. At the same threshold, the leading superconducting system required approximately 512 seconds. The gap widens further at higher thresholds: above AR ≈ 0.90, that system produces no qualifying samples at all, making its effective TTS infinite.


**A foundational subroutine under real conditions.** The Quantum Fourier Transform benchmark tests one of the most consequential subroutines in quantum computing. QFT sits at the core of Shor's algorithm, HHL, and a range of algorithms expected to deliver long-term computational advantage. IonQ Forte maintains scores approaching the ideal maximum across the Modulated QFT challenge circuits, holding performance across circuit widths where noise-driven degradation typically accelerates. Other systems tested across the same circuit widths degraded significantly faster, with scores approaching the noise floor at sizes where IonQ Forte maintained meaningful accuracy. Benchmarking QFT execution fidelity directly tests whether a system can reliably execute the building blocks of future fault-tolerant algorithms.


**Circuit complexity stress test.** The Hidden Shift benchmark probes a system's ability to execute circuits with systematically increasing entangling gate counts across multiple permutation families. IonQ Forte maintains meaningful benchmark scores across CX ladder, CCX ladder, and MCX permutation challenges up to 36 qubits. The MCX variant is the most gate-intensive challenge in the suite. Unlike application benchmarks tied to specific problem domains, the Hidden Shift problem provides a hardware-agnostic measure of how a system performs as circuit complexity scales, independent of algorithmic framing. On the Time-to-Solution variant of this benchmark, at 36 qubits with an 80-CX random permutation, the leading superconducting system failed to sample a single bitstring within 5 bit-flip errors of the target across 1 million circuit executions. IonQ Forte sampled the correct solution in minutes.


**Quantum chemistry under pressure.** The VQE benchmark tests molecular ground-state energy calculations on hydrogen chains of 2 to 18 atoms. The “solved” criterion is energy accuracy within 1 mHa of the exact solution. This standard currently remains unmet across the quantum computing industry. The benchmark results quantify precisely how far each system size falls short, without obscuring the noise regime behind aggregate scores. On variational algorithms of this class, architectures with faster gate speeds hold a TTS advantage, a tradeoff this framework reports directly rather than omits.


## Designed to Be Honest


A framework that reports only favorable results is not a benchmark. The VQE benchmark publishes a solved criterion that IonQ’s own hardware has not yet reached. That is by design.


Results are presented as a function of problem size and circuit depth. The noise regime of each of IonQ’s current systems is visible at every scale, not flattened into a single score. Performance degrades with circuit depth on NISQ hardware, and this framework shows exactly where and how. That is the information customers and researchers need to make real development, procurement, and deployment decisions.


VQE is included as a rigorous test of hardware fidelity rather than as a production-ready chemistry algorithm. The approach was originally designed around the constraints of early NISQ hardware, where shallow circuits and high noise made deeper alternatives impractical. At scale, VQE faces well-documented algorithmic challenges: barren plateaus make gradient-based optimization increasingly difficult as system size grows, and local minima become harder to escape as the molecular complexity increases. VQE remains a meaningful stress test for current hardware precisely because it is hard to execute well. As hardware and algorithms co-evolve, the framework is designed to retire benchmarks that no longer reflect real application demand and adopt those that do.


The benchmark code is public. Any third party can run the same workloads on any system and report results in the same terms. The comparative results referenced in this post, across both IonQ and non-IonQ systems, were independently validated by Kearney. That is the standard the field needs if quantum computing is going to earn a place in real enterprise decisions.


## What This Framework Makes Possible


Benchmarking frameworks evolve alongside the technology they measure. This one is designed to do the same. Benchmark problems can be proposed as new application domains emerge, and existing problems can be modified or retired as hardware capabilities and business requirements shift.


For customers evaluating quantum systems for real workloads, the question is no longer "how many qubits?" It is "how long does it take, and at what cost, to reach the answer quality my application requires?" This framework is built to answer that question. Detailed results, benchmark descriptions, and system comparisons are[available here](https://github.com/ionq-publications/apps-benchmark) . That is how quantum computing earns a place in decisions that matter.
