---
schema_version: "1.0.0"
document_id: "343a333ba182d6747e58fae17bb16e67ff0f2c3aec5eb832c4c036873d81d30f"
company_key: "adtran-holdings-inc-common-stock"
company: "ADTRAN Holdings Inc."
source_id: "adtran-holdings-inc-common-stock-rss-2a55922dc4ce"
canonical_url: "https://www.blog.adtran.com/en/from-zero-to-quantum-hero"
published_at: "2025-08-27T08:13:00+00:00"
first_seen_at: "2026-07-20T23:22:01.904129+00:00"
fetched_at: "2026-07-28T22:25:47.581815+00:00"
content_hash: "sha256:70ed8aef91ad303c2fe3b88d73d3c0cd8586b36eeedae01c97ec48eefb3f7c26"
---

# From zero to quantum hero

With quantum computing now grabbing headlines and fast becoming a reality, the question isn’t if it will disrupt encryption – it’s when. This step-change in computing power will lead to a fundamental change in how computers are used to solve problems, analyze information and protect data. We’re already seeing early proofs in the wild, for example, Volkswagen teamed up with D-Wave’s quantum computer to optimize the routing of taxis in Beijing in real time1, and Airbus is using quantum computing to model new materials for aircraft2.


Should Adtran be concerned about this development? Well, yes. The immediate risk is the impact on encryption. Shor’s algorithm3, developed in 1994, shows how a quantum computer could factor large integers and compute discrete logarithms efficiently, which would undermine RSA and ECC. In practice, attackers can harvest encrypted data now and decrypt it later, so our priority is to adopt quantum‑safe protections ahead of time.


Adtran has a long history of providing customers with the ability to encrypt traffic, and we’ve always taken a proactive approach to ensure that the encryption algorithms we employ exceed the minimum security demands at the time each hardware platform is released to customers.


**Qubits, superposition and the quantum leap**


To put it simply, quantum computing applies the principles of quantum mechanics to process data. Unlike traditional computers, which process 0 and 1 bits, a quantum computer processes qubits, a quantum mechanical bit that exists in a mixed 0 and 1 state due to superposition. This introduces the advantage of parallel processing, where the quantum computer can evaluate both the 0 and 1 states simultaneously. The level of parallel processing increases exponentially as qubits are added, which are mixtures of multiple states or symbols, beyond the 0 or 1 mixed state mentioned above. For example, a three-qubit register can be in a superposition of 000, 001, 010, 011, 100, 101, 110 and 111 – hence, the incredible computing power available from the 1,225-qubit quantum computer from Atom Computing or IBM’s 1,121-qubit quantum computer4. This additional computing power can provide faster solutions to most problems that would take a classical computer years to solve, such as factoring large numbers. Because many encryption schemes rely on the difficulty of factoring large integers, the threat to encrypted data is clear.


**Why quantum computing isn’t quite ready for prime time**


It’s not plain sailing, though, as there are many challenges to the commercial application of quantum computing, such as computer stability. Qubits are extremely fragile entities, and any interaction with the surrounding ‘real’ world resolves the qubit into one of its possible states, for instance, 010 in the case of a three-qubit register (turning it into a classical symbol). This collapse leads to the loss of the parallel processing potential and introduces errors. To reduce these errors, quantum computers must operate at extremely cold temperatures. Ideally, they need to operate at absolute zero (-273.15°C, nature not being a respecter of whole numbers), but as this is unattainable5, any calculation performed by a quantum computer has to be performed as near to absolute zero as possible and repeated more than once so that statistics can be used to provide the final result.


There are available error correction methods, akin to forward error correction, where redundant qubits are used to check information integrity. Other challenges include the need for greater control effort as qubit counts rise, increased complexity in presenting problems to the quantum computer, the need for specialized quantum measurement devices, the necessity for highly sensitive amplifiers to convert the quantum signals into readable outputs and, ultimately, the requirement for a lot of electrical power.


Also, not all problems can be solved with the full benefit of the exponentially increased computing power of the parallel computing provided by a quantum computer. Tasks such as machine learning and certain optimization problems (for example, the traveling salesman problem) don’t benefit from it.


> Crypto-agility ensures we can stay ahead of any algorithm that’s broken.


From our perspective, though, the factorizing of large prime numbers, which underpins cryptography, does benefit from quantum-accelerated calculation. Although this isn’t yet practical, the scale of the research investment around the world means it’s only a matter of time before it becomes a real threat. There’s also the risk that attackers could harvest sensitive data now and decrypt it later when quantum capabilities mature.


Companies such as IBM, Google, Microsoft and startups like Atom Computing and D-Wave are in an intensely competitive battle to produce a quantum computer with the highest qubit count. At present, this competition is being ‘won’ by Atom Computing with their quantum processor featuring a 1,225-site array and a 1,180 active qubit count. In the future, IBM plans a 4,158+ qubit quantum computer targeted for later this year and a 100,000+ qubit quantum computer by 2033 (perhaps to be taken with a quantum of salt), with similar plans for Atom Computing and Microsoft.


**Adtran’s quantum-safe strategy: ready before the revolution**


So, what’s Adtran’s response? Well, via[Adva Network Security](https://www.advasecurity.com/en) , post-quantum cryptography (PQC) has been adopted across the entire optical networking product portfolio and is being introduced into the Ethernet access product line. The National Institute of Standards and Technology (NIST) has now selected and standardized a PQC algorithm, the CRYSTALS-Kyber (2022) algorithm6 (FIPS 203), with another one to follow, the Hamming Quasi-Cyclic (2025) algorithm7. Relying on a single PQC standard isn’t ideal, as there’s always the risk of it being broken, which is why the introduction of a second algorithm – and likely more in the future – is so important. CRYSTALS-Kyber, now called ML-KEM, is being implemented by Adtran in the FSP 3000 AgileConnect™ and CloudConnect™ products.


In fact, Adtran was an early adopter of PQC with the McEliece algorithm back in 2017. Although McEliece has not been standardized by NIST, it remains under consideration by this body and has shown no weakness so far (currently at round four of the submissions process). In parallel, Adtran has provided support for the ETSI GS QKD 0148 specification on both the FSP 3000 AgileConnect™ and CloudConnect™ product lines. This standard describes a communication protocol and data format for QKD equipment to provide key information to an application, enabling interoperability with different QKD vendors via REST APIs.


Furthermore, the development of a so-called ‘crypto sub-module,’ an enclosed daughter board, will provide crypto-agility to ensure cryptographic algorithms can be swapped easily in response to changes in standards or, heaven forbid, the cracking of an algorithm!


References:
1.


[https://www.volkswagen-group.com/en/press-releases/research-project-successful-volkswagen-it-experts-use-quantum-computing-for-traffic-flow-optimization-16498](https://www.volkswagen-group.com/en/press-releases/research-project-successful-volkswagen-it-experts-use-quantum-computing-for-traffic-flow-optimization-16498)
2.


[https://www.airbus.com/en/innovation/digital-transformation/quantum-technologies](https://www.airbus.com/en/innovation/digital-transformation/quantum-technologies)
3.


Shor, P.W. (1994). "Algorithms for quantum computation: Discrete logarithms and factoring". Proceedings 35th Annual Symposium on Foundations of Computer Science. pp. 124–134. doi:10.1109/sfcs.1994.365700. ISBN 978-0-8186-6580-6.
4.


To put the computing advantage of a quantum computer in context as well as the equivalent computing power in terms of bits (the usual measure of the power of a computer): the equivalent for a 1,121-qubit quantum computer would be a 21121-bit classical computer! (Warning: your computer may crash if you try to calculate this value.) Of course, not all this quantum advantage is available to every type of calculation.
5.


[https://phys.org/news/2017-03-physicists-impossible-cool-absolute.html](https://phys.org/news/2017-03-physicists-impossible-cool-absolute.html) . Also, this fact is formally declared in the third law of thermodynamics.
6.


[Module-Lattice-Based Key-Encapsulation Mechanism Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.203.pdf)
7.


[HQC](https://pqc-hqc.org/)
8.


[https://www.etsi.org/deliver/etsi_gs/QKD/001_099/014/01.01.01_60/gs_qkd014v010101p.pdf](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/014/01.01.01_60/gs_qkd014v010101p.pdf)
