---
schema_version: "1.0.0"
document_id: "efd8f8a6bc1aa4d320115fb519029985f9119099bb58755b65f88ec5f137ee75"
company_key: "xanadu-quantum-technologies-limited-class-b-subordinate-voting-shares"
company: "Xanadu Quantum Technologies Limited"
source_id: "xanadu-quantum-technologies-limited-class-b-subordinate-voting-shares-news-import-9a24fb0a26c8"
canonical_url: "https://pennylane.ai/blog/2026/06/benchmarking-quantum-machine-learning-with-matchcake"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-26T05:56:13.943329+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:8fd14199ec21c113b79af95c4606f9093ef280825ecda35a6cb20e23ae066fe7"
---

# Benchmarking Quantum Machine Learning with MatchCake | PennyLane Blog

1. [Blog](https://pennylane.ai/blog) /


2. [Plugins](https://pennylane.ai/search?categories=plugins&sort=publication_date&contentType=BLOG) /


3.


Benchmarking Quantum Machine Learning with MatchCake


June 12, 2026


# Benchmarking Quantum Machine Learning with MatchCake


[Jérémie Gince](https://pennylane.ai/profile/JeremieGince)


> The following is a guest post by Jérémie Gince from the Institut quantique at Université de Sherbrooke, showcasing[MatchCake](https://github.com/MatchCake/MatchCake) , an open-source PennyLane device for simulating Matchgate circuits, which can be used for benchmarking quantum machine learning models.


Before claiming quantum advantage in QML, you need to check whether a classically simulable free-fermionic model can already do the job. MatchCake makes that comparison easy.


## Research in Quantum Machine Learning (QML)


QML is moving fast. New quantum models, new circuits, new claims of advantage. But there's a question that every new QML paper should answer before anything else: *does your model actually need quantum resources to perform well?*


QML is a relatively new field of research that is still trying to prove its value compared to classical machine learning (ML). To do so, QML algorithms are often compared with ML models on[benchmark datasets](https://pennylane.ai/datasets/collection/qml-benchmarks) where ML has either performed exceptionally well for many years, or on datasets where ML struggles significantly for various reasons. The conclusions are almost always practically the same:


- QML models perform worse than ML, but not by much, so it's fine; we're making progress.
- QML models are not statistically significantly better than ML models.
- QML models are completely outperformed by ML models, but it's not a fair comparison. What matters is that QML models are better than they were before.


These recurring conclusions are often accompanied by problems such as **[barren plateaus](https://pennylane.ai/demos/tutorial_barren_plateaus/)** and a **lack of scalability** , which largely account for the difficulty of training QML models and the challenge of tackling larger, real-world, and therefore more relevant datasets. Beyond this, critiques of these papers frequently come down to questioning the[relevance of QML](https://pennylane.ai/blog/2026/03/quantum-computing-useful-for-machine-learning) , given that no quantum advantage has yet been proven, as well as questioning whether the[comparison with ML](https://pennylane.ai/blog/2022/03/why-measuring-performance-is-our-biggest-blind-spot-in-quantum-machine-learning) is even meaningful.


On top of these recurring conclusions, it is not uncommon to see articles published with the famous word " *advantage* ". This word, which once sparked a flicker of curiosity before reading the article, now triggers an immediate sense of disdain. A disdain justified by the sheer number of bold, unproven claims of quantum advantage in machine learning. That said, these claims are not made in bad faith. Rather, they stem from a lack of tools to verify whether the quantum models in question actually require their quantum resources, and therefore genuinely need to be executed on a quantum computer in order to perform.


It goes without saying, then, that the field of QML is in great need of additional methods to verify whether the quantum resources used by QML models are truly necessary for a given problem. Naturally, this kind of verification is a difficult task, and one that can certainly be approached in a variety of ways.


## MatchCake comes into play


A fairly straightforward way to verify whether a QML model actually needs its genuine quantum resources is simply to strip them away, keeping the same architecture, but constraining it to a space where it can be executed on a classical computer. This way, we can compare the model with its classically simulable "quantum-inspired" counterpart and check whether there is any meaningful difference in performance.


This is where[MatchCake](https://github.com/MatchCake/MatchCake) comes in. MatchCake is a PennyLane-integrated Python simulator for **matchgate circuits** , a restricted class of quantum circuits with a remarkable connection to a physical system of free fermions in 1D. This connection means matchgate circuits can be simulated efficiently in polynomial time on a classical computer.


To get a sense of how this tool works, let's take a look at where matchgates come from. The starting point is a free-fermionic Hamiltonian: a quantum system where fermions don't interact with each other, only evolve independently under a quadratic Hamiltonian of the form


H = i \\sum_{\\mu,\\nu}^{2N} h_{\\mu\\nu}\\, c_\\mu c_\\nu


where c_\\mu


are Majorana operators and h


is a real antisymmetric 2N \\times 2N


matrix. Such systems are exactly solvable and lie at the heart of a large class of tractable quantum models.


The Jordan–Wigner transformation links the fermionic description to the qubit language of quantum circuits: it sends fermionic creation/annihilation operators to Pauli operators on a qubit chain, turning free-fermion evolution into a family of nearest-neighbor two-qubit gates—the matchgates. Therefore matchgate circuits are exactly the qubit representation of free-fermion dynamics.


Concretely, a matchgate acting on two neighboring qubits takes the form


M(A, W) = \\begin{pmatrix} a & 0 & 0 & b \\\\ 0 & w & x & 0 \\\\ 0 & y & z & 0 \\\\ c & 0 & 0 & d \\end{pmatrix}


where A = \\bigl(\\begin{smallmatrix} a & b \\\\ c & d \\end{smallmatrix}\\bigr)


and W = \\bigl(\\begin{smallmatrix} w & x \\\\ y & z \\end{smallmatrix}\\bigr)


satisfy \\det(A) = \\det(W)


. The structure directly reflects parity conservation: states with an even number of excitations only mix with each other, as do states with an odd number, which is the fingerprint of free-fermion dynamics in the qubit picture.


This fermionic structure unlocks two key tools that keep the simulation polynomial-time. First, the matchgates inside a circuit can be compiled into a single-particle transition matrix, a polynomial-size object that captures how each fermionic mode transforms under the full circuit evolution. Rather than tracking an exponentially large quantum state, you only need to track a 2N \\times 2N


matrix R


, built gate by gate by translating each matchgate back into its fermionic action via


R_{\\mu\\nu} = \\frac{1}{4} \\operatorname{Tr}\\!\\left\[ M\\, c_\\mu\\, M^\\dagger\\, c_\\nu \\right\]


where the c_\\mu


are Majorana operators. Equivalently, when the matchgate M


is generated by a free-fermionic Hamiltonian with antisymmetric matrix h


, this reduces to R = e^{4h}


, making the link to the underlying fermionic dynamics explicit. Each gate contributes one such matrix, and the transition matrix of the full circuit is simply their product, a computation that stays polynomial regardless of circuit depth. Second, because the system is free-fermionic, Wick's theorem applies: any many-body expectation value decomposes into a sum of products of two-point correlators. This means the full output of the circuit, which would normally require exponential work to compute, reduces to the Pfaffian of a matrix whose entries are drawn from R


and whose size scales polynomially with the number of qubits. What would otherwise require exponential resources collapses into a tractable calculation.


Importantly, this efficiency does not come from restricting entanglement. Unlike tensor-network methods, which work by keeping entanglement low, matchgate circuits can produce arbitrarily high entanglement. And unlike Clifford circuits, which are efficiently simulable due to a discrete gate set, matchgates are continuously parameterized, making them natural building blocks for variational and kernel-based machine learning models. This is a rare combination: capacity to create highly entangled states, continuous parameters for optimization, and classical tractability.


This makes matchgate circuits a uniquely honest yardstick for QML. If a proposed model can't outperform its free-fermionic counterpart, the quantum resources it claims to exploit aren't actually driving its performance.


An example of such a comparison is presented in the article[Fermionic Machine Learning (Gince et al., 2024)](https://arxiv.org/pdf/2404.19032) where it is shown that on two toy datasets very popular in QML, certain parametrized quantum circuits (PQCs) are unable to outperform their free-fermionic counterparts. Fermionic models matched or exceeded unrestricted PQCs throughout, which raises an uncomfortable but necessary question about what those PQCs were actually contributing (a concern also raised by[Bowles, Ahmed & Schuld (2024)](https://pennylane.ai/blog/2024/03/benchmarking-near-term-quantum-algorithms-is-an-art/) in the context of quantum model benchmarking).


MatchCake is the tool that makes this kind of benchmark possible and practical. It implements the full matchgate simulation pipeline within PennyLane and PyTorch, with automatic differentiation support, scaling as \\mathcal{O}(N^3)


. The practical implication is direct: **any QML paper should include a free-fermionic baseline.** If your model can't beat a free-fermionic counterpart, its quantum structure isn't contributing anything that classical computation doesn't already provide in terms of performance.


## Conclusion


The bar for claiming genuine quantum advantage in machine learning should be uncompromisingly high. If the performance of your model can be matched, or worse, beaten, by a classically simulable free-fermionic baseline, it's not meaningfully quantum. It's just another model with extra overhead. **[MatchCake](https://github.com/MatchCake/MatchCake) changes the standard.** It gives the QML community a scalable way to *actually test* quantum claims.


**Run the benchmark.**


- If your model passes, the claim holds.
- If it doesn't, the real question isn't about quantum advantage, it's: *what is this model actually offering?*


## About the author


[Jérémie Gince](https://pennylane.ai/profile/JeremieGince)


PhD Student in Physics specializing in quantum machine learning and free-fermionic computation. Creator of MatchCake.


*Last modified:* *June 12, 2026*
