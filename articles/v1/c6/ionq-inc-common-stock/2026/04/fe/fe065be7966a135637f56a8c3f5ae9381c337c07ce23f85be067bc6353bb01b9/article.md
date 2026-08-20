---
schema_version: "1.0.0"
document_id: "fe065be7966a135637f56a8c3f5ae9381c337c07ce23f85be067bc6353bb01b9"
company_key: "ionq-inc-common-stock"
company: "IonQ Inc."
source_id: "ionq-inc-common-stock-news-import-8f7ead1ae5e6"
canonical_url: "https://www.ionq.com/blog/mind-the-gaps-quantum-optimization-for-efficient-electric-and-autonomous-freight-dispatch-with-ionq-and-einride"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:28.377709+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:f47e2e0d532afea64bbdfb58361283dd1fc37c68e13440d4a141c378e66e8345"
---

# Mind the Gaps: Quantum Optimization for Efficient Electric and Autonomous Freight Dispatch with IonQ and Einride

The shift to electric freight is accelerating, and its economics hinge on a planning problem that conventional routing software was not designed to handle.[Research conducted by Einride](https://www.einride.tech/press/einride-unveils-first-of-its-kind-study-quantifying-the-impact-of-ai-powered-electrification-planning) , a technology company building the infrastructure for electric and autonomous freight, alongside Fraunhofer and Rewe, found that optimizing electric fleet operations from the ground up reduced fleet-level[total cost of ownership by 8–13%](https://doi.org/10.24406/publica-4574) , compared to roughly 3% for straightforward 1:1 replacement of diesel trucks with EVs. Electric fleets introduce charging schedules, energy constraints, and route interdependencies that make planning substantially more complex than conventional trucking. That complexity, managed well, is where the economic advantage lives.


That optimization advantage depends entirely on how well the plan holds, and how well the system recovers when it doesn't.


Einride operates its fleet through Saga, an AI-powered platform that connects vehicles, infrastructure, and data to manage freight operations at scale. Shipment cancellations are a daily feature of large-scale logistics, leaving idle gaps in pre-optimized vehicle schedules that directly erode the fleet utilization rates that make electric freight economically viable.


Einride's fleet operations team deals with them continuously, using its Electric Vehicle Routing Problem (E-VRP) solver to slot in replacement shipments from a waiting pool. Each gap carries hard constraints: vehicle charging limits, driving time regulations, time windows, and driver shift boundaries. The solver is fast and effective for individual gaps. What it does not naturally account for is the interaction between gaps on different vehicles: how two replacement shipments assigned to nearby routes might conflict operationally, create scheduling dependencies downstream, or compound risk across the fleet when executed together. That interaction between concurrent assignments is where classical gap-filling solvers are structurally limited, and where the quantum formulation is specifically designed to operate.


When two gaps on nearby vehicles are filled simultaneously, the selected shipments create compounded operational risk: geographic proximity between routes, temporal overlap between vehicle schedules, and shared infrastructure constraints all affect how the two assignments perform together, not just individually. That interaction is encoded as a quadratic term in the objective. Mixed-integer solvers like SCIP can represent this quadratic structure natively, but their branch-and-bound search becomes significantly more expensive as the interaction weight grows, and on large high-interaction instances, SCIP consistently hits the 48-hour wall-clock time limit without proving optimality.


Quantum computing is suited to this class of problem for a structural reason. When each gap-filling decision is encoded as a single qubit, the quadratic interactions between concurrent assignments map directly to two-qubit couplings in the circuit, a representation that captures cross-assignment dependencies naturally. Rather than searching through candidate combinations sequentially, the quantum circuit holds multiple assignment possibilities in superposition, using interference to progressively concentrate probability toward the most operationally compatible solutions.


Our work with Einride,[Hybrid Quantum-Classical Optimization Workflows for the Shipment Selection Problem](https://arxiv.org/abs/2604.11758) , presents a hybrid quantum-classical workflow designed to augment Saga's planning capabilities, and validates it using problem instances constructed from anonymized real operational schedules from active customer freight operations. Across all weekly schedules evaluated, the full hybrid workflow, with quantum assignment passed to Einride's operational solver as a warm start, achieves a mean improvement of 1.7% in Shipments Delivered relative to the classical baseline, with best-case individual gains reaching +12.1%. The paper is co-authored by Miguel Angel Lopez-Ruiz, Daiwei Zhu, Claudio Girotto, Willie Aboumrad, Julia Kompalla, Mena Issler, Ananth Kaushik, and Martin Roetteler from IonQ, and Jonas Hatzenbühler, Shudian Zhao, and Jonas Alm from Einride.


## **Key Findings**


- **The hybrid workflow improves classical baseline planning on average:** the quantum warm-start solution achieves a mean improvement of 1.7% in Shipments Delivered across the portfolio of weekly schedules studied, with best-case individual scenario gains reaching +12.1%. The quantum component supplies a structured, compatibility-aware starting point that the classical solver refines into plans it could not reach from a classical baseline.
- **Quantum selection improves schedule compatibility across the fleet:** Both the raw quantum assignment and the warm-start solution show consistent improvement in Schedule Compatibility Score, with best-case scenario-level gains of +0.14. This is the clearest direct signal that the quadratic formulation is identifying geographically and temporally coherent concurrent assignments.
- **The gains come at no material cost penalty:** Across all evaluated weekly schedules, total operational cost remains effectively unchanged relative to the baseline.
- **Iterative-QAOA requires no re-training between problem instances:** Circuit parameters follow a fixed schedule calibrated once via noiseless simulation. No re-training is required when new cancellation scenarios arrive, making the algorithm practical for a planning system that re-solves the same class of problem continuously.


## **Problem Instances from Einride's Live Operations**


The benchmark instances in this study are not synthetic. They are constructed from anonymized real operational schedules from active customer freight operations. The generation process is designed to replicate the structure of a disruption scenario: a baseline schedule is established for a full week of operations, a simulated set of cancellations is applied, and the canceled shipments are returned to a pool of available candidates for re-assignment. A feasibility filter then identifies every valid replacement sequence for each resulting gap of at least two hours. A sequence here means a candidate set of shipments that can be inserted to fill a gap while respecting all operational constraints: charging cycles, driving regulations, time windows, and driver shifts. The value of each feasible sequence is computed as the cost saving from inserting it relative to the original scheduled route.


Each instance covers a one-week planning horizon. Because real operational instances involve far more shipments and gaps than current simulation can address directly, each weekly instance is decomposed into subproblems of tractable size. Smaller subproblems of up to 32 qubits are evaluated using a statevector simulator, while larger subproblems ranging up to 130 qubits are benchmarked via Matrix Product State (MPS) simulation, a classical tensor-network method that approximates quantum circuit behavior at scales approaching near-term hardware capacity.


The classical baseline is SCIP, a mixed-integer quadratic programming solver that integrates branch-and-bound search, cutting-plane separation, and a portfolio of primal heuristics. All SCIP runs were bounded by a 48-hour wall-clock timeout.


## **The Regime Where Classical Branch-and-Bound Stalls**


SCIP's runtime scales with two independent factors: problem size (number of sequences, equivalent to qubit count) and the weight of quadratic interaction terms. For SSP instances with low interaction complexity, SCIP solves efficiently. As the interaction weight increases, reflecting denser or more operationally consequential cross-assignment risk, the branch-and-bound subproblems become harder in a way that scales with the interaction structure. Across large instance families with high quadratic weights, SCIP often hits the 48-hour wall-clock timeout without proving optimality.


The high-interaction regime is where solution quality has the most operational consequence, and where SCIP's branch-and-bound search is already running out of time. A quantum approach is not being asked to solve faster. It needs to produce a better pre-execution schedule in precisely the regime where classical accuracy is already compromised.


On large instances evaluated via MPS simulation, Iterative-QAOA shows more robust behavior relative to SCIP as quadratic weight increases. The ten candidate solutions from the quantum pipeline remain competitive with SCIP's best feasible solutions within time-limited budgets, and across the full benchmark set, quantum solutions are competitive with the Einride Optimization Solver classical baseline on Shipments Delivered. This does not establish quantum advantage universally. At low interaction complexity and small scale, SCIP performs well. The quantum advantage is structural and specific: it emerges in the high-interaction setting that defines the most consequential part of Einride's planning problem.


## **The Quadratic Structure That Changes the Problem**


Standard vehicle routing assigns shipments to vehicles to minimize total cost, with each assignment contributing its value independently. The Shipment Selection Problem (SSP) that Einride faces adds a second layer: pairwise interaction costs between assignments. Two gaps filled simultaneously on nearby vehicles are not independent decisions. Their combined operational risk depends on where those vehicles are, how their schedules overlap, and what shipments they are carrying. That interaction is encoded through a flow matrix and a distance matrix whose product defines the interaction weight between any two concurrent assignments.


The result is a Mixed-Integer Quadratic Program (MIQP) that maps to a Quadratic Unconstrained Binary Optimization (QUBO) problem for execution on quantum hardware, a form in which each binary decision variable maps to a single qubit and the objective becomes an energy function whose ground state encodes the optimal assignment. Three feasibility constraints govern the formulation: each gap receives at most one replacement sequence, each shipment can only be assigned once across the fleet, and the selected sequence must fit within the available time window of the gap it fills. Solutions are evaluated against three operational KPIs: Shipments Delivered (SD), Total Drive Distance (TDD), and Schedule Compatibility Score (SCS), which measures the pairwise compatibility of concurrent assignments, with higher scores reflecting geographically co-located and temporally coherent insertions across the fleet.


## **Iterative-QAOA: Fixed Schedule, Iterative Concentration**


Standard QAOA finds its circuit parameters through a classical variational optimization loop, tuning parameter angles iteratively until the expected energy converges. On near-term hardware, that loop is expensive, noise-sensitive, and impractical for a planning system that must re-solve the same class of problem each time a new set of cancellations arrives. Iterative-QAOA eliminates it. Circuit parameters follow a fixed schedule empirically calibrated via noiseless simulation sweeps across representative SSP instances. No re-training occurs when new problem instances arrive.


An iterative warm-start mechanism takes its place. After each circuit execution, the lowest-energy measurement outcomes update the quantum initial state for the next iteration, progressively concentrating the probability distribution toward favorable solutions. The process runs for 20 iterations with 4,000 shots each. By iteration 9, the distribution is substantially concentrated relative to the starting state. Circuit pruning removes low-magnitude entangling gates before execution to control circuit depth, while all scoring is evaluated against the full unpruned cost structure. This preserves the optimization signal while reducing gate overhead.


## **From Einride's Fleet to Adjacent Scheduling Problems**


SSP's core structure recurs across a broad class of scheduling problems: binary assignment variables, exclusivity constraints, and a quadratic term capturing how concurrent decisions interact. Across all of them, the interaction between simultaneous assignments is what makes the problem hard.


Airport gate assignment faces the same structure: assigning aircraft to gates to minimize passenger transit times, resource load, and turnaround efficiency. Multi-carrier freight coordination, where shipments from different operators share loading docks or regional hubs, creates the same cross-assignment dependencies. Distribution center placement follows the same logic: minimize the number of facilities while reducing total route detours.


Classical solvers handle the linear costs well in every one of these settings, but the interaction structure between concurrent assignments is what strains them at scale. The Iterative-QAOA pipeline developed here, and the non-variational fixed-schedule property that eliminates re-training overhead when new problem instances arrive, extend directly to these domains without reformulation.


## **Next Steps**


The current work evaluates instances up to 130 qubits via simulation. The natural next step is direct hardware execution across this full instance range as available qubit counts and circuit depth expand. The current single-sequence-per-gap restriction is an open extension point: multi-sequence gap insertions would more fully represent Einride's operational flexibility, and the MIQP formulation supports it directly.


## **Conclusion**


We have demonstrated that this algorithm, using problem instances derived from Einride's real operations, produces solutions competitive with classical branch-and-bound. The structural gap between the approaches widens in exactly the operational regime that matters most: large fleets, high interaction complexity, and schedule re-optimization. What this study establishes is a proof of concept grounded in real operational data: the algorithm works, the results are competitive, and the hybrid pipeline delivers gains that neither approach achieves alone.


[https://www.ionq.com/world-quantum-day-2026-videos/cutting-engineering-simulation-costs-today](https://www.ionq.com/world-quantum-day-2026-videos/cutting-engineering-simulation-costs-today)


Technical paper:[https://arxiv.org/abs/2604.11758](https://arxiv.org/abs/2604.11758)


‍
