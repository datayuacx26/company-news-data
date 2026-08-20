---
schema_version: "1.0.0"
document_id: "b32f94a004b62459e206336c84d2481f3b355066763afa57911e5c00d420ca09"
company_key: "international-business-machines-corporation-common-stock"
company: "International Business Machines Corporation"
source_id: "international-business-machines-corporation-common-stock-news-import-da8ebfcd6a45"
canonical_url: "https://www.ibm.com/quantum/blog/qiskit-paulice"
published_at: null
first_seen_at: "2026-07-22T00:20:11.053223+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:5fc19d5a6aa4be34e66107895fd8d141bc0ca08af16695b2a913e0394a5b1966"
---

# Qiskit Paulice: postselected quantum error correction for near-term hardware

**Key takeaways:**


- Significant advances have been made toward full fault-tolerant quantum computing by improving the reliability of current quantum systems with different error-handling methods.
- Qiskit Paulice is a new tool that embeds “spacetime Pauli checks” directly into quantum circuits to detect errors during circuit execution with minimal overhead.
- Error detection is a foundational component of error correction that verifies whether a run of a quantum circuit was affected by errors.
- Spacetime Pauli checks make error detection more practical by placing checks across qubits and specific locations in time within the circuit’s execution.
- Access the Paulice documentation and tutorial[here](https://quantum.cloud.ibm.com/docs/addons/qiskit-addon-paulice) .


Achieving useful quantum computing will require progress across two dimensions: (1) developing new algorithms to enable more resource-efficient quantum computations, and (2) extending the reach of quantum hardware through methods for handling errors. We’ve seen real momentum this year in algorithm discovery, with new methods for[large-scale molecular simulations](https://www.ibm.com/quantum/blog/cleveland-clinic-riken-chemistry) ,[multi-objective optimization](https://www.ibm.com/quantum/blog/multi-objective-optimization) , and[stochastic differential equations](https://research.ibm.com/blog/differential-equations) . At the same time, we’re expanding the toolkit for dealing with errors—critical work as we look to scale quantum computers toward practical applications.


Error handling techniques we develop today will help enable large-scale,[fault-tolerant quantum computing](https://www.ibm.com/quantum/blog/what-is-ftqc) —where errors are corrected as they occur during computation—[by 2029](https://www.ibm.com/quantum/blog/large-scale-ftqc) . But we aren’t waiting to share that progress. Our goal is to deliver new capabilities as they become available, equipping you with the tools to improve the reliability of computations at every stage of the journey to fault tolerance. Today, we add another tool to that toolkit:[Qiskit Paulice](https://quantum.cloud.ibm.com/docs/addons/qiskit-addon-paulice) , a new Qiskit addon that integrates low-overhead quantum error detection directly into your quantum circuits.


One of the central challenges in quantum error handling is its computational cost. The most powerful methods usually involve a tradeoff between “time”—the number of samples or computational steps required—and “space”—the number of qubits needed to execute the method.


On one end of this spectrum are error mitigation methods like probabilistic error cancellation (PEC), which have relatively low hardware overhead but require an exponential increase in sampling time, limiting their scalability. On the other end, fault-tolerant architectures like the one IBM is building toward will enable time-efficient computation, but will also require many qubits and complex hardware.


As quantum circuits grow larger and more costly to run, how can we bridge this gap? How can we improve the reliability and resource costs of modern error mitigation while continuing to make meaningful progress toward fault tolerance? Qiskit Paulice provides a practical path forward.


***Explore and test the Qiskit Paulice addon today[by following our tutorial](https://quantum.cloud.ibm.com/docs/addons/qiskit-addon-paulice/guides/low-overhead-error-detection-using-spacetime-codes) .***


## Quantum error detection with Paulice


Quantum error handling methods fall into three categories:error suppression, error mitigation, and error correction. **What are error suppression, error mitigation, and error correction?** Quantum error handling methods are commonly grouped into three categories. Error suppression anticipates and prevents the occurrence of some errors at the hardware level. Error mitigation uses the noisy outputs of multiple circuit executions to estimate the circuit’s error-free results. Error correction refers to a more advanced and not yet fully realized approach in which we encode quantum information redundantly across multiple qubits so that we can, in principle, detect and correct errors during computations by checking for inconsistencies.


**Error detection** isn’t a separate category, but rather a foundational component of error correction and some error mitigation techniques. It verifies whether a run of a quantum circuit was affected by errors. It doesn’t demand the large qubit overhead of full error correction, and unlike other mitigation techniques, it doesn’t require exponentially more samples as circuit size grows.


Typically, error detection involves designating qubits in a system as either **data qubits** or **ancilla qubits** . Data qubits perform the computations, and they are connected to ancilla qubits that catch errors in the data qubits. The challenge is that bringing those ancilla qubits into the computation can add so much depth to the circuit that they introduce more errors than they catch. This is the problem[Qiskit Paulice](https://quantum.cloud.ibm.com/docs/addons/qiskit-addon-paulice) (` qiskit-paulice` ) was built to solve.


Paulice strategically embeds error-detection checks known as[spacetime Pauli checks](https://arxiv.org/abs/2504.15725) directly into your circuits, allowing you to detect errors during circuit execution and filter out errored results. This improves the reliability of computations while adding minimal overhead.


It's a flexible, hardware-efficient approach that integrates smoothly into compatible workflows. Once errors are detected, you can postselect only the circuit runs where no error was observed and discard runs where errors were detected, turning Paulice into a form of postselected error correction. You can also use the syndrome information alongside other techniques, such as error mitigation or error correction workflows, to further reduce the impact of noise.


## Understanding spacetime Pauli checks


Traditional Pauli checks work by entangling ancilla qubits with the data qubits in a circuit. When we measure the ancilla qubits, they output a **syndrome** —a string of bits indicating whether an error was detected during execution. If the syndrome is all` 0` s, no error was detected. If any entry is` 1` , an error occurred.


The layout of qubits on the physical hardware. The payload is supported on the blue qubits and checks on the purple qubits.


At a high level, each Pauli check corresponds to a constraint that should hold throughout the circuit’s execution. If an error disrupts that constraint, it is flagged by the measured syndrome. This allows us to identify corrupted results and isolate circuit runs that are more likely to be correct.


Spacetime Pauli checks are more efficient because they implement these constraints as a[spacetime code](https://arxiv.org/abs/2504.15725) . Checks are defined not just across qubits in physical space, but also across specific locations in time within the circuit’s execution. This allows the checks to detect errors across extended regions of the computation.


That’s part of what makes spacetime Pauli checks practical on today’s hardware. Standard Pauli checks1 often require measuring high-weight operators, which can introduce significant circuit depth—especially on devices with limited qubit connectivity, where additional SWAP gates may be needed. Spacetime Pauli checks avoid this by placing checks only where they are most effective, enabling a hardware-efficient encoding that can be implemented with relatively low overhead.


A quantum circuit demonstrating the structure of a singular spacetime Pauli check. The ancilla is represented by the purple qubit/top wire, and four target/payload qubits are represented in blue.


Not all checks are equally useful. Each one adds operations to the circuit that introduce some additional noise. A “good” set of checks detects more error than it creates, balancing detection ability with minimal overhead. To achieve this, Paulice uses a noise model and the device’s connectivity constraints to automatically identify checks that are valid, low-weight, and effective at detecting errors across the circuit.


The` qiskit-paulice` package automatically identifies and inserts these spacetime Pauli checks in your quantum circuits. This allows you to maximize error detection while minimizing additional qubit and circuit costs, making error detection a practical tool for improving computations. These methods are particularly well suited to Clifford and Clifford-dominated circuits,where efficient checks can be more readily constructed. **What are Clifford circuits?** Clifford circuits are quantum circuits built from a specific set of gates—e.g. Hadamard, Phase, and CNOT gates—that map basic operations known as Pauli operators to other Pauli operators. Their behavior is mathematically well understood and efficiently simulated on classical computers, making them a natural entry point for developing and testing error-handling methods.


Once the circuit is executed, you can use the resulting syndromes in different ways depending on your goal. In the simplest case, you can postselect only the runs in which no errors were detected, improving the fidelity of the remaining results. Additionally, you can use the syndrome information alongside other techniques, such as error mitigation or error correction workflows, to further reduce the impact of noise.


The concepts that power Qiskit Paulice are already beginning to play a role in cutting-edge, advantage-candidate experiments. For example,[a recent Quantum Advantage Tracker submission](https://quantum-advantage-tracker.github.io/trackers/classically-verifiable-problems?instance=random_graph_sampling_nq70_depth70_checks27_basis_fstate) from IBM and the University of Chicago applies spacetime Pauli checks in large-scale random graph state sampling experiments. These experiments fall under the broader class of **random circuit sampling** , a leading benchmark for early demonstrations of quantum advantage. The research underscores how low-overhead error detection and syndrome-based filtering techniques can help scale quantum computations towards regimes that become increasingly difficult to simulate classically.


## Getting started with Qiskit Paulice


Let's walk through an example of how to use Paulice to improve the fidelity of a Clifford circuit’s measured distribution with spacetime checks.


First, install the` qiskit-paulice` package:


```text
pip install qiskit  -  paulice
```


Copy to clipboard


Next, create a Clifford circuit. For this example, we’ll construct a 12-qubit circuit with four layers of entangling gates placed in a brickwork pattern interleaving layers of single-qubit Clifford gates:


```text
from   qiskit   import   QuantumCircuit
circuit   =   QuantumCircuit  (...)
```


Copy to clipboard


To add Pauli checks, you’ll need to identify which qubits in your circuit can serve as targets and which nearby qubits can act as ancillas. In practice, this involves selecting data qubit–ancillary pairs based on device connectivity so checks can be implemented with minimal overhead. For simplicity, we’ll represent that process here as:


```text
from   qiskit_ibm_runtime   import   QiskitRuntimeService
from   qiskit_paulice  .  layout   import   get_check_qubits


service   =   QiskitRuntimeService  ()
backend   =   service  .  backend  (  "ibm_boston"  )


# Choose a layout for the circuit
layout   =   [  68  ,   69  ,   78  ,   89  ,   90  ,   91  ,   98  ,   111  ,   112  ,   113  ,   119  ,   133  ]


# Find available ancillas and their adjacent target qubits
target_qubits  ,   ancilla_qubits   =   get_check_qubits  (backend.coupling_map, layout)
```


Copy to clipboard


Next, construct a noise model, which we'll use to score the effectiveness of Pauli checks that are valid for the circuit. A basic depolarizing noise model, which assumes every gate and measurement has roughly the same chance of error, is usually sufficient, and you can build it from backend data:


```text
from   qiskit_paulice  .  noise_models   import   NoiseModel


noise_model   =   NoiseModel  .  from_backend  (backend, layout)
```


Copy to clipboard


Together with the target qubits and noise model, we use Qiskit Paulice to search for effective, low-weight checks and add them to the circuit:


```text
from   qiskit_paulice   import   add_pauli_checks


checked_circuit   =   add_pauli_checks  (
circuit, [layout.  index  (q)   for   q   in   target_qubits], noise_model
)  [  -  1  ]
```


Copy to clipboard


You now have a circuit with 7 checks!


To continue the workflow, see[the Paulice documentation](https://quantum.cloud.ibm.com/docs/addons/qiskit-addon-paulice) . The full tutorial walks through the process of running the checked circuit, sampling results, inspecting syndromes, postselecting error-free samples, and comparing fidelities before and after error detection.


## Try it today


With Paulice, we're taking the next step toward practical quantum error correction and making error detection accessible across the quantum community.


The Paulice team is already working to expand the package with planned enhancements, including support for handling non-Clifford systems, analysis of postselected noise channels, and improved handling of idling noise during check selection.


Learn more about Qiskit Paulice with these resources.


- **[Paulice documentation site](https://quantum.cloud.ibm.com/docs/addons/qiskit-addon-paulice) :** Access Paulice installation instructions, tutorials, and API references.
- **[qiskit-paulice GitHub repo](https://github.com/Qiskit/qiskit-paulice) :** Download the open source Qiskit Paulice addon.
- **[Low-overhead error detection with spacetime codes](https://arxiv.org/abs/2504.15725) :** The research behind Qiskit Paulice.


We're excited to see what you build with Paulice. Try it out and let us know what you discover.
