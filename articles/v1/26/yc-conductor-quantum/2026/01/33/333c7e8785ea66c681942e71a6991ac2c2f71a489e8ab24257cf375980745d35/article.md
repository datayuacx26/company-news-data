---
schema_version: "1.0.0"
document_id: "333c7e8785ea66c681942e71a6991ac2c2f71a489e8ab24257cf375980745d35"
company_key: "yc-conductor-quantum"
company: "Conductor Quantum"
source_id: "yc-conductor-quantum-rss-fd18baa21fa7"
canonical_url: "https://blog.conductorquantum.com/p/coda-natural-language-quantum-computing"
published_at: "2026-01-22T00:16:33+00:00"
first_seen_at: "2026-07-24T23:15:28.367668+00:00"
fetched_at: "2026-07-28T20:54:41.720106+00:00"
content_hash: "sha256:37ccf8021027587bce14d1688dc19178287155813e2502f4775c6253dfa28e7c"
---

# Coda — Natural language quantum computing

# Coda — Natural language quantum computing


### Describe your problem and execute it on real quantum computers.


Jan 22, 2026


***TL;DR Qubit counts are increasing quickly. To keep pace, we need better software that makes quantum computers usable by more people, earlier. That is why we built Coda.***


Hi. This is Brandon, Joel and Ray from Conductor Quantum.


Today, we are announcing Coda, a natural language interface for running real quantum programs on real quantum hardware.


## What Coda does


Coda lets you interact with a quantum computer using natural language.


You describe the problem you want to run. Coda converts that intent into a quantum circuit, checks it for correctness, and executes it on available hardware.


The goal is not to hide quantum mechanics, but to remove unnecessary friction between intent and execution.


## How this fits into the current state of quantum computing


Quantum computers already exist and are improving steadily. Qubit counts are rising, hardware reliability is improving, and access to cloud hosted quantum systems is becoming more common.


What has not kept pace is software.


Most quantum tools still assume deep prior knowledge, low level programming, and significant setup overhead. This slows down experimentation and limits who can realistically use the hardware.


Coda is designed to sit above existing quantum SDKs and hardware providers, translating high level intent into executable programs without requiring users to manage the entire stack themselves.


## Learning and exploration


For users who are new to quantum computing, Coda includes a learn mode.


Instead of treating the system as a black box, learn mode explains what circuit is being generated, why specific operations are used, and what the results mean. The goal is to let users build intuition while still running real programs.


One of the first quantum computers available on the platform is Rigetti’s 84 qubit system. If users want to iterate quickly before using quantum hardware Coda also supports simulations up to 34 qubits supported by the


[NVIDIA cuQuantum libraries](https://developer.nvidia.com/cuquantum-sdk) and


[NVIDIA CUDA-Q platform](https://developer.nvidia.com/cuda-q) for hybrid quantum-classical computing.


## Why we are building this


We believe quantum computing will only be useful if people can experiment quickly and cheaply.


As hardware scales, the primary constraint will shift from qubit count to orchestration, usability, and integration with classical computation. Software needs to evolve to meet that shift.


## What we have built so far


Coda builds on several years of work at lower levels of the quantum stack.


We have:


-


Built the first API for low-level silicon quantum chip control.


-


Partnered with quantum chip maker SemiQon and ran our software across 64 of their quantum devices.


-


Shipped quantum control software for companies including Quobly and EeroQ.


This experience informs how Coda generates circuits, validates execution, and interacts with hardware constraints.


## What is next


We are continuing to expand Coda in three directions:


-


End to end GPU plus QPU orchestration so classical and quantum computation can run together in a single workflow


-


Connecting Coda to our lower level control and tune up software, enabling a path from high level intent to device level execution


-


Scaling our control software as quantum hardware moves to larger systems


Our aim is to reduce the distance between intent and execution as much as possible.


## Try it


Coda is live and evolving. Try it here:


[https://coda.conductorquantum.com/](https://coda.conductorquantum.com/)


If you try it, we would love feedback on what works, what is confusing, and what you want to do that you cannot yet do.


— Brandon, Joel and Ray, Conductor Quantum


Thanks for reading! Subscribe for free to receive new posts and support my work.
