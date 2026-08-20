---
schema_version: "1.0.0"
document_id: "c3d7a4e1e322337c1470b5ec8e9852f34d857f3f3c4305441f2f1004c0a3b814"
company_key: "ionq-inc-common-stock"
company: "IonQ Inc."
source_id: "ionq-inc-common-stock-news-import-8f7ead1ae5e6"
canonical_url: "https://www.ionq.com/blog/quantum-computing-meets-wall-street-running-real-portfolio-optimization-on-trapped-ion-hardware"
published_at: "2026-03-23T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:28.377709+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:d0f5ccda410a65815528df2ad26ecbeb8207fe2ad3ad9c50769e9ddd83ca892f"
---

# Quantum Computing Meets Wall Street: Running Real Portfolio Optimization on Trapped-Ion Hardware

Every additional qubit matters. In our benchmark on 250 assets drawn from the S&P 500, moving from a 36-qubit to a 64-qubit subproblem size systematically improved portfolio quality across every configuration tested — not as a theoretical projection, but as a measured result on deployed hardware. The relationship is direct: larger executable subproblems preserve more of the asset correlation structure that drives portfolio construction, fewer approximations accumulate, and the final portfolio moves closer to the global optimum. That scaling relationship is hardware-driven, and it holds within the same pipeline without redesign.


The problem that motivates this work is cardinality-constrained portfolio selection — choosing exactly K assets from a large universe while jointly minimizing risk and maximizing return. This classical problem is NP-hard, and at the scale of a real equity universe, classical solvers rely on heuristics. Our latest manuscript,[Large-Scale Portfolio Optimization on a Trapped-Ion Quantum Computer](https://arxiv.org/abs/2602.23976) , co-authored with Kipu Quantum, presents an end-to-end quantum-classical pipeline that treats the hardware qubit limit as the primary design constraint and benchmarks the result directly against Gurobi on real S&P 500 data. We ran quantum subproblems on both IonQ Forte and a 64-qubit Barium development system similar to the forthcoming IonQ Tempo line.


## **From QUBO to Quantum Hardware: The Decomposition Problem**


Cardinality-constrained portfolio selection, formalized based on the Markowitz mean-variance framework, maps directly to a quadratic unconstrained binary optimization problem, or QUBO — a mathematical form in which each asset is represented as a binary variable (included or excluded) and the objective encodes both covariance risk and expected return in a single quadratic expression. QUBO problems convert to Ising Hamiltonians — energy functions defined over interacting binary spins — which is the native computational language of gate-based quantum hardware.


A 250-variable QUBO cannot execute on any current quantum system in one shot. The result is a hybrid approach in which quantum resources handle subproblems and classical computing manages correlation preprocessing, global recombination, and constraint enforcement.


Our pipeline has four stages.


**Stage 1 — Data Denoising.** We apply Random Matrix Theory (RMT) denoising to the empirical asset correlation matrix before clustering. Raw correlation matrices from finite return histories carry noise eigenmodes — spurious statistical patterns that are statistically indistinguishable from the random baseline predicted by the Marchenko-Pastur law. This is a theoretical distribution describing how eigenvalues of purely random matrices are distributed, and it depends on matrix dimensions — in financial applications, the number of assets and the number of observations. Clustering on these modes produces groupings driven by data artifacts, not meaningful correlation structure. RMT separates the matrix into three components: noise, a dominant global market mode, and the structured informative residual. Community detection runs only on the structured component, ensuring that the clusters fed to the quantum optimizer reflect real asset correlations.


**Stage 2 — Hardware-Capped Partitioning.** Community detection (Louvain) on the denoised matrix produces an initial partition of assets into correlated groups. Communities that exceed the hardware qubit limit Qmax are recursively split using a correlation-guided greedy rule: within each oversized community, the highest-degree node in the absolute-correlation graph seeds a new cluster of exactly Qmax assets, drawn from its strongest correlates. The process repeats on the remainder until all clusters satisfy the budget. Every asset is assigned to exactly one cluster, meaning no assets are discarded.


For Qmax = 36 (IonQ Forte): 14 clusters, three at the maximum size of 36. For Qmax = 64 (Barium development system): 11 clusters, with the three largest at 41, 55, and 60 assets. Fewer clusters at larger sizes means fewer cross-cluster asset correlations are severed — and less decomposition-induced error in the final portfolio.


**Stage 3 — BF-DCQO Execution.** Each cluster maps to an independent Ising subproblem solved on quantum hardware using Bias-Field Digitized Counterdiabatic Quantum Optimization (BF-DCQO), developed by Kipu Quantum. BF-DCQO is non-variational: circuit parameters are derived analytically from the problem Hamiltonian rather than tuned through a classical training loop, making it practical for near-term hardware where variational approaches are expensive and noise-sensitive. The algorithm runs iteratively — each round uses the lowest-energy measurement outcomes to update the quantum initial state for the next round, progressively concentrating samples toward favorable solutions. A gate pruning step controls circuit depth without discarding the dominant cost structure.


**Stage 4 — Hybrid Refinement.** BF-DCQO circuits do not enforce the cardinality constraint natively, so post-processing applies in two phases: a gradient-guided pass that repairs each output to exactly K assets, followed by a cardinality-preserving swap local search that refines solution quality. Cluster-level solutions concatenate into global 250-asset portfolios, and the local search runs again at the global level — where cross-cluster swaps become available for the first time, recovering inter-asset relationships that per-cluster optimization cannot access.


## **Hardware: Why Trapped-Ion Shines for Dense Ising Problems**


[IonQ Forte Enterprise](https://www.ionq.com/quantum-systems/forte-enterprise) class systems use 36 qubits and deliver industry-leading two-qubit and single-qubit gate fidelities. The 64-qubit Barium development system extends this architecture with a longer ion chain and adds leakage checks to filter samples where qubits have interacted with the environment.


Our trapped-ion hardware has a structural advantage for dense QUBO objectives. Superconducting processors implement two-qubit gates only between physically adjacent qubits; a fully connected Ising graph requires extensive SWAP routing, which multiplies circuit depth and error. Trapped-ion systems realize two-qubit interactions through the collective vibrational modes of the ion chain — shared phonon modes that act as a communication bus between any pair of ions regardless of their physical separation — giving near-arbitrary qubit connectivity without SWAP overhead. A 36-qubit fully connected Ising Hamiltonian runs at its natural circuit depth on Forte; the coupling graph imposes no additional penalty.


Circuit pruning removes gates below a fixed rotation-angle threshold before execution. These gates contribute negligibly to the implemented evolution but accumulate gate errors. On the 36-qubit Forte runs, two pruning levels were tested: high (~60% of gates removed) and medium (~40% removed). On the 64-qubit system, medium pruning removed 50–70% of gates.


## **Results**


**Qubit budget determines portfolio quality.** The 250-asset universe was benchmarked with a fixed cardinality of K = 125 — the portfolio must hold exactly half the universe. Moving from Qmax = 36 to Qmax = 64 systematically improved post-processed objective values across all configurations. Larger subproblems preserve more inter-asset correlation structure inside each cluster. Correlations that cross cluster boundaries are invisible to the optimizer and introduce approximation error at recombination; the 64-qubit decomposition severs fewer of them, and the final global portfolios are correspondingly closer to the Gurobi-computed global optimum.


**BF-DCQO initializations outperform random baselines.** Under identical post-processing budgets, quantum-initialized candidate pools consistently produced lower-energy portfolios than random selection followed by the same local search. The quantum circuits concentrate samples in lower-energy regions of the Ising landscape; the classical refinement then operates from a structurally better starting point.


**Global local search recovers value that per-cluster optimization cannot.** Some candidates that were suboptimal at the cluster level — before any post-processing — surpassed the recombined reference (the portfolio built from individually optimal per-cluster solutions) after global refinement. Cross-cluster swap moves at the global level identify improvements that are structurally unavailable to any per-cluster solver. The recombined reference is a baseline for comparison, not an upper bound on achievable solution quality.


**Pruning level has limited impact on end-to-end solution quality.** At 36 qubits, high and medium pruning produced comparable end-to-end results; at 64 qubits, medium pruning consistently outperformed both 36-qubit configurations. At fixed qubit count, reducing circuit complexity by 20 additional percentage points of gates leaves solution quality essentially unchanged. Circuit cost can be cut substantially on near-term hardware without paying an equivalent optimization penalty.


**Solution diversity spans the risk-return frontier.** Candidate portfolios examined in the annualized risk-return plane show a continuous, broad distribution of feasible solutions. Near-degenerate objective values correspond to portfolios with substantially different risk and return profiles. The pipeline produces a set of high-quality candidates across the feasible interior, not a single-point solution.


## **Applications in Financial Optimization**


Our decomposition structure is not specific to the 250-asset test case. Portfolio rebalancing under cardinality constraints — where a fund must hold exactly N positions after a rebalance — index tracking with limited holdings, and regulatory capital allocation problems that require selecting a fixed number of instruments all reduce to the same QUBO form. In each case, the hardware qubit budget determines the maximum cluster size. As that budget increases with each hardware generation, fewer decomposition approximations accumulate and solution quality improves within the same pipeline architecture, without redesign.


Our workflow is naturally parallel. Cluster subproblems are independent and can be dispatched simultaneously to multiple QPUs. At current cloud access latencies, this is already feasible for moderate cluster counts; at larger qubit counts and faster execution, it becomes a practical model for production deployment.


## **Next Steps**


The direct path to further improvement is larger subproblem execution. The 64-qubit Barium development system used here is similar to the forthcoming IonQ Tempo line., and each increase in executable qubit count reduces cluster count, shrinks decomposition error, and pushes achievable solution quality toward the global optimum within the same pipeline. Incorporating cross-cluster coupling terms more explicitly during recombination — rather than relying entirely on post-processing swap moves to recover that structure — is an open algorithmic direction we are actively exploring. Multi-QPU parallel execution of independent subproblems is the natural scaling model as quantum cloud infrastructure matures.


## **Conclusion**


The 250-asset S&P 500 test case used here is the scale where classical heuristics currently operate. Running quantum optimization on this problem, on deployed hardware, with results benchmarked directly against Gurobi, places quantum and classical methods on the same playing field at a scale financial practitioners recognize.


The scaling relationship is hardware-driven and holds within our existing pipeline. Each generation of larger qubit-count hardware directly extends the executable subproblem size, reduces decomposition error, and produces better portfolios without requiring a new algorithmic approach. Just as quantitative finance was transformed by the availability of faster classical computing — enabling strategies that were theoretically understood but computationally intractable — the arrival of larger, higher-fidelity quantum processors is set to open the same door for problems that classical optimization has not been able to solve exactly. The path from our current results to production-relevant performance is a function of qubit count, and qubit counts are increasing on a defined roadmap.


The paper was co-authored by Alejandro Gomez Cadavid, Ananth Kaushik, Pranav Chandarana, Miguel Angel Lopez-Ruiz, Gaurav Dev, Willie Aboumrad, Qi Zhang, Claudio Girotto, Sebastian V. Romero, Martin Roetteler, Enrique Solano, Marco Pistoia, and Narendra N. Hegade. Full technical details:[https://arxiv.org/abs/2602.23976](https://arxiv.org/abs/2602.23976)
