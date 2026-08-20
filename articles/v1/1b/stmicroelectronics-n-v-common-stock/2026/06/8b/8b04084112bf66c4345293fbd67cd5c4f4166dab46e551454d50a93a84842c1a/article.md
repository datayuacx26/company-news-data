---
schema_version: "1.0.0"
document_id: "8b04084112bf66c4345293fbd67cd5c4f4166dab46e551454d50a93a84842c1a"
company_key: "stmicroelectronics-n-v-common-stock"
company: "STMicroelectronics N.V."
source_id: "stmicroelectronics-n-v-common-stock-rss-811b8a53447b"
canonical_url: "https://blog.st.com/pqc-post-quantum-cryptography/"
published_at: "2026-06-24T08:12:27+00:00"
first_seen_at: "2026-07-24T02:26:40.894294+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:46c0cac21047fe0f52c051063e69eb41e0a64fbd8dc93ac8d78e59f3cecab499"
---

# Update: New ST54M and PQC, Everything you wanted to know about post-quantum cryptography but were afraid to ask

*Editor’s Note: We updated the section on “How is ST addressing PQC challenges?” to reflect the announcement of our NesLib-PQML libraries and the launch of the ST54M.*


**PQC, or post-quantum cryptography, is now part of the public discourse at numerous tech companies.** Yet many in our industry still struggle to grasp its intricacies, and too few understand why moving to PQC is already necessary. Hence, let’s look at how to think about it and explore what it means for decision-makers today. To intelligently talk about post-quantum cryptography, we must lay down some fundamental principles about quantum computing, which means looking, even superficially, into quantum mechanics. The topic itself is highly complex and broad, so a blog post can’t even begin to scratch the surface. Hence, we simply aim to clarify some of the most basic principles in the hope of demystifying post-quantum cryptography.


## The fundamental principles


### What is quantum computing?


Classical computers store and process information using electrical signals, which can be in only two basic states: high or low voltage. Consequently, we represent these states using a binary language made of zeroes and ones. Everything a classical computer does, from the program it runs or the images and videos it displays, is ultimately a long string of these zeroes and ones. **A quantum computer is similar in that it also uses a physical system as the foundation for all its calculations, but instead of using classical electrical signals, it uses quantum particles, such as electrons or photons.** And these elements behave according to the laws of quantum mechanics.


One famous example of this is the double-slit experiment conducted by Thomas Young in the early 1800s. When light passes through two slits, it creates an interference pattern, as if it were a wave going through both slits at once. However, if one takes a measurement device to determine which slit the light goes through, the interference pattern disappears. In that instance, light behaves as if it were made of particles. **This is what quantum mechanics calls the wave–particle duality.** This behavior has been reproduced numerous times over the years with a variety of photons or matter, and it is one of the key ideas behind quantum computing.


### What is a qubit?


A close-up of a quantum computer


What is the relationship between the double-slit experiment and quantum computers? Put simply, quantum computers use the quantum state of quantum particles as a representation for the fundamental information at the heart of quantum computing. **If classical computers use bits, quantum computers use quantum bits, or qubits.** Just like a bit represents a voltage range, a qubit may symbolize a photon’s polarization or the spin of an electron. In both classical and quantum computing, the system must apply a measurement to determine the value of a bit or qubit. However, unlike in classical mechanics, the double-slit experiment shows that quantum particles behave very differently before and after measurement.


Similar to classical computing, where bits are in a binary state represented by either 0 or 1, quantum computing frames qubits within two states, such as two energy levels or two angular momenta, among others. **However, unlike classical bits, in which states are mutually exclusive, meaning that it’s either a 1 or a 0, qubits are in a superposition of two states, meaning that they behave as if they are in a combination of the 0 and 1 states, like the wave going through both slits at once.** As a result, the 0 and 1 states in a qubit exist in a probability amplitude that defines the probability to actually obtain a 0 or a 1 when measured.


Once the system applies a measurement, the qubit’s superposition collapses to 0 or 1, thereby ending the quantum computation. It’s exactly like when measuring which slit the light passes through in the double-slit experiment mentioned above. As a result, quantum algorithms use carefully designed operations to modify the probability amplitudes of the qubits. **Put simply, a quantum algorithm will manipulate the probability amplitudes to amplify correct results while suppressing incorrect ones, so that applying a measurement yields a correct outcome.** Behind the scenes, it involves complex mathematical notions that we can’t possibly do justice to in a blog post like this one. However, this overview helps understand why quantum computing is such a hard problem to solve.


The main potential advantage of quantum computing over classical computing lies in its ability to exploit superposition to explore many possibilities in parallel. While a classical computer must evaluate possibilities step by step, a quantum computer can, for certain problems, use its quantum behavior to reduce the number of operations needed and achieve significant speedups. However, this performance gain does not apply to all tasks. **Traditionally, quantum computers mostly outperform classical machines on specific classes of problems, such as optimization, simulation of quantum systems, and some cryptographic and search-related applications.**


## The quantum menace


Moving towards PQC: Closeup of circuits shaping qubits


### What is a cryptographically relevant quantum computer (CRQC)?


While there are already quantum computers in various labs around the world, none are actually a threat to classical cryptographic algorithms. As the industry coined it, they are not “cryptographically relevant.” **However, we are clearly marching toward a CRQC, a cryptographically relevant quantum computer, meaning a future quantum computer capable of breaking public-key cryptography by running fault-tolerant quantum algorithms leveraging a high number of qubits.** Some estimate CRQC to be 10 to 15 years away, but it’s very hard to make any sort of reliable prediction. The reason is that to be cryptographically relevant, a quantum computer must not only be able to handle a significant number of qubits, but it must also pass additional requirements.


Quantum computers struggle to recognize and correct errors or corruption. A CRCQ will need to **detect and correct errors in real time** . Today’s quantum computers also tend to generate error-prone logical qubits. To achieve cryptographic relevance, a quantum computer will have to **support hundreds of logical qubits** . A logical qubit is a qubit encoded using numerous physical qubits to protect it against errors. The idea is similar to Error Correction Codes in ECC memory. In 2024,[Google created a logical qubit](https://blog.google/innovation-and-ai/technology/research/google-willow-quantum-chip/) made of 105 qubits. Finally, CRQC will have to process computations across millions of quantum gate operations and **remain stable for at least hours** . Until a quantum computer can satisfy all these criteria, it is not cryptographically relevant.


### What are the quantum threats?


#### Classical algorithms


How would a quantum computer break classical cryptographic algorithms? One of the most commonly cited methods is Shor’s Algorithm, named after the mathematician Peter Shor. To understand what it does, it is important to note that many modern cryptographic algorithms rely on the fact that factoring large integers is extremely difficult and time-consuming on classical computers. **Indeed, while multiplying two numbers is fast and easy, figuring out which prime numbers to multiply together to arrive at a large number takes so much time as to render the computation practically impossible.**


This is, in essence, the heart of encryption algorithms such as RSA. The large number is the encrypted data. The two prime numbers are the public and private keys. If one has both, decrypting the data is very quick and simple. If one only has the public key, brute forcing the private key would be considered impossible. For instance, breaking an RSA encryption using 2048-bit integers would take approximately 300 trillion years. **Comparatively, according to a popular paper published in 2021, the same operation would take eight hours and “[20 million noisy qubits](https://quantum-journal.org/papers/q-2021-04-15-433/) on a quantum computer.**


#### Shor’s and Grover’s algorithms


Behind the scenes, numerous complex mathematical principles explain how a cryptographically relevant quantum computer can very quickly crack RSA encryption. And while we will not delve into number theory and modular arithmetic in this paper, we can mention two popular algorithms that have garnered such attention that they are even cited in mainstream publications: Shor’s and Grover’s algorithms. In essence, both are quantum algorithms that enable quantum computers to perform certain computations at speeds that vastly surpass those of classical computers. Hence, when industries talk about quantum computers threatening today’s cryptographic standards, it’s because, in part, these algorithms have mathematically shown how it could happen.


In the most basic of terms, Shor’s algorithm, developed by Peter Shor1 in 1994, showed how a quantum computer could perform order-finding and other complex routines at an incredible speed. Under the hood, this is possible because quantum computers can manipulate multiple qubits to perform modular arithmetic and other operations at speeds that vastly surpass those of classical computers. As a result, Shor’s algorithm can make it exponentially faster to crack RSA encryption by factoring large numbers with far fewer computational steps, thereby severely undermining its cryptographic relevance.


Similarly, in 1996, Lov Grover presented2 a quantum search algorithm that can be used to brute-force a symmetric key. This applies to ciphers like AES and to keyed constructions such as HMAC-SHA-256. Classical brute-force would require testing almost all possible keys. With a typical key size of 128 bits, testing all possible keys would require 2128 executions of the underlying cryptographic algorithm. Put simply, we theoretically have to test all possible key values. On the other hand, Grover’s algorithm uses quantum computing so that each computation increases the probability amplitude of the correct key, so that after only about 264 computations, the correct key may be measured with very high probability.


#### AES-192 and AES-256


While quantum computers can process an amount of information at once that is simply unfathomable for classical computers, rendering some encryption algorithms vulnerable, the[National Institute of Standards and Technology](https://csrc.nist.gov/projects/post-quantum-cryptography/faqs) of the United States Department of Commerce has also publicly stated that not all current encryption algorithms are lost. **For instance, the organization declared that “AES 192 and AES 256 will still be safe for a very long time”.** That’s because Grover’s algorithm has implementation limitations that restrict its ability to brute-force this type of encryption. Aside from the fact that the algorithm requires substantial quantum computation, its practical advantage is also hindered by the difficulty of efficiently parallelizing it.


#### Preparing for tomorrow


While it is important not to overstate the risk that CRQC poses today, it’s also critical not to underestimate it. **As quantum computers get better, what is fine today simply won’t stand a chance tomorrow.** That’s a problem for some digital signature and certification technologies that must last decades, if not more. Similarly, even if a blockchain (based on classical public-key cryptography) meets all security requirements today, its inability to withstand attacks from a quantum computer 20 years from now will effectively render it useless. Attackers are also adopting a “harvest now, decrypt later” strategy, which leads them to hoard massive amounts of sensitive data, knowing they will eventually crack their encryption keys once quantum computers are powerful enough.


It’s no surprise then that, even if a CRQC is still a way away and some current algorithms still have a long life ahead of them, **many are already upgrading to post-quantum cryptography** , meaning a cryptographic scheme that will resist quantum computers, despite their capabilities. The very popular CDN \[Cloudflare announced a PQC initiative last year\] and[Google](https://cloud.google.com/blog/products/identity-security/announcing-quantum-safe-digital-signatures-in-cloud-kms) just announced an update to its Chrome browser and cloud services to use quantum-safe encryption tools. Similarly,[Apple](https://security.apple.com/blog/imessage-pq3/) explained that it implemented PQC algorithms in its iMessage app to “protect against future threats from quantum computers.”


### When could CRQC become reality?


Government regulators are trying to lead by example and have provided roadmaps for their PQC deployments. Like the[National Security Agency](https://media.defense.gov/2022/Sep/07/2003071836/-1/-1/0/CSI_CNSA_2.0_FAQ_.PDF) (NSA) of the United States, many recognize that they don’t “know when there will be a CRQC.” **However, many have already begun to plan for it because transitions take time, sometimes 10 years or more, from final standardization to full system integration.** Put simply, migrations toward quantum-resistant algorithms are already underway, markets are moving toward PQC, and government agencies have released recommended timelines for PQC migration to anticipate the “Q-day”, when a CRQC becomes a reality.


#### European Commission


In June 2024, the[European Commission](https://digital-strategy.ec.europa.eu/en/library/recommendation-coordinated-implementation-roadmap-transition-post-quantum-cryptography) asked member states to have a roadmap for PQC implementation “after a period of two years following the publication of this Recommendation”, which sets the deadline for the end of 2026. **In its[official roadmap](https://digital-strategy.ec.europa.eu/en/library/coordinated-implementation-roadmap-transition-post-quantum-cryptography) , the Commission set that “high-risk use cases should be transitioned to PQC as soon as possible, no later than the end of 2030” and that by “2035, the transition should be completed for as many systems as practically feasible.”** The European Union Agency for Cybersecurity (also called ENISA because it was originally named the European Network and Information Security Agency), has published a[migration paper](https://www.enisa.europa.eu/sites/default/files/publications/ENISA%20Report%20-%20Post-Quantum%20Cryptography%20Current%20state%20and%20quantum%20mitigation-V2.pdf) and an[integration study](https://www.enisa.europa.eu/sites/default/files/publications/Post%20Quantum%20Cryptography-%20Integration%20Publication.pdf) to help meet those goals.


#### NIST


**Similarly, NIST, as stated in a[presentation given in 2025](https://csrc.nist.gov/csrc/media/Presentations/2025/nist-pqc-the-road-ahead/images-media/rwcpqc-march2025-moody.pdf) , wants quantum-vulnerable algorithms deprecated by 2030 and disallowed by 2035.** The Department of Commerce recognizes that “NIST-approved symmetric primitives providing at least 128 bits of classical security” are still a viable option and that “organizations may continue using public key algorithms at the 112-bit security level as they migrate to post-quantum cryptography.” However, NIST also understands that moving to quantum-resistant algorithms is a tremendous endeavor that requires infrastructure modernization, new hardware and software, library implementation, and more. That’s why the organization began its standardization work years ago and is working to move the industry toward PQC as quickly as possible.


#### NSA


Similarly, the NSA has said that it hopes “all NSS ( *editor’s note: National Security Systems* ) will be quantum resistant by 2035.” To achieve that goal, the agency has drafted version 2.0 of its Commercial National Security Algorithm Suite (CNSA), which lists the quantum-resistant algorithms approved for NSS use. **As the NSA explains, “CNSA 2.0 algorithms will be required for all products that employ public-standard algorithms in NSS”.** By January 1, 2027, “all new acquisitions for NSS will be required to be CNSA 2.0 compliant”, and “by December 31, 2030, all equipment and services that cannot support CNSA 2.0 must be phased out unless otherwise noted, while by December 31, 2031, CNSA 2.0 algorithms are mandated for use.”


## The advent of PQC


### Where is PQC needed?


Not every infrastructure is affected equally by quantum computers. Not every piece of computing equipment needs to migrate to PQC at the same speed. However, it would be wrong to assume that only supercomputers storing national security secrets are prioritized. **In fact, in many instances, the primary factor for deciding how fast a piece of equipment must move to PQC is not necessarily function but its longevity in the field.** Even something as benign as an industrial application becomes a sensitive piece of equipment because it was designed to remain in the field for decades. Indeed, if its secure boot or over-the-air update mechanism is not quantum resistant, that system can become a vulnerability for the whole company.


That’s why the GSMA, the association for the Global System for Mobile Communications,[issued guidelines in 2024](https://www.gsma.com/newsroom/gsma_resources/pq-03-post-quantum-cryptography-guidelines-for-telecom-use-cases/) outlining how telecom operators should move to PQC. **The United States Cybersecurity and Infrastructure Security Agency[published something similar](https://www.cisa.gov/resources-tools/resources/post-quantum-considerations-operational-technology) for all “operational technology (OT) owners and operators,” telling them that they “cannot wait until the advent of a CRQC to develop and implement a plan.”** In fact, the agency explains that it is a fallacy to think that only information technology (IT) is at risk from quantum computers. As the document explains, CRQC allows “the attacker to masquerade as trusted sources, freely tamper with information undetected, or decrypt information used to protect communication channels” in all OT systems.


Consequently, it’s some of the world’s smallest and most innocuous pieces of equipment that are the most at risk, such as trusted platform modules, or TPMs. These little secure elements make the basis for the root of trust responsible for signing firmware, generating or handling keys, and securing a machine’s boot sequence. **All the quantum-resistant TLS certificates in the world protecting the web are meaningless if a machine’s root of trust is compromised.** Similarly, governments, companies, and institutions have resorted to secure IDs and other components to store a person’s information. These IDs last a very long time and are often used to sign documents or verify a person’s identity. A vulnerable ID can thus have catastrophic effects.


### What are the new Post-Quantum cryptographic standards, and how do they differ from classical algorithms?


#### Harder math problems


In preparation to migrate to PQC, many wonder what makes an algorithm quantum resistant. **While the underlying mathematics is quite complex, the basic idea is to use more computationally intensive algorithms than factoring, such as lattice-based cryptography.** Very simply, lattice-based cryptography uses a set of coordinates, which could be crudely represented as dots on a 2D plane, to solve mathematical problems, such as high-dimensional structured algebraic problems, that use this lattice representation. Algorithms then use complex mathematical tools, such as number-theoretic transforms in lattice-based cryptography, to further increase computational intensity, thus strengthening their quantum resistance.


#### More resource intensive


**PQC also uses some obvious but very effective methods to harden security, such as larger key sizes.** As we saw with AES, even if the underlying cryptography may be conceptually vulnerable to a quantum computer, a large enough key can mean that an attack would still take too much time or resources to be viable. Similarly, some quantum-resistant algorithms use statefulness, akin to a random value, like a counter, created each time a signature is generated and then checked to ascertain a message’s origin or if it was intercepted. Classical algorithms such as RSA and ECDSA are stateless. Some PQC mechanisms are stateful, which adds another layer of security to prevent a signature from being reused.


#### A list of PQC algorithms


**In 2024, NIST released[three finalized PQC standards](https://www.nist.gov/news-events/news/2024/08/nist-releases-first-3-finalized-post-quantum-encryption-standards) , ML-KEM (FIPS 203), ML-DSA (FIPS 204), and SLH-DSA (FIPS 205).** ML stands for Module-Lattice. KEM stands for Key Encapsulation Mechanism and refers to how a key is used in an insecure channel. The other two standards are used for a digital signature algorithm (DSA) that can verify authenticity. SLH stands for Stateless Hash-Based and uses hash computations rather than a lattice. These already provide a strong foundation for quantum resistance. For instance, Apple is already using ML-KEM, and Google chose ML-DSA and SLH-DSA, although its cloud key management system supports all three.


**Efforts continue to standardize new algorithms to address edge cases.** For instance, NIST has approved two other stateful hash-based algorithms (LMS or Leighton-Micali Signature and XMSS or eXtended Merkle Signature Scheme) and is working on standardizing another lattice signature, the FN-DSA (FIPS 206), which combines a lattice and a fast Fourier transform to keep keys small while still ensuring quantum-resistance. However, the latter is harder to implement due to its reliance on floating-point calculations and may not be suitable to product targeting resistance to side-channel attacks.


### What are the challenges for PQC migration?


#### Inventory


Too often, teams focus on the quantum-resistant algorithm they will use and fail to realize that migration challenges reside elsewhere. Indeed, when the CISA published its quantum-readiness guideline, it didn’t mention one algorithm by name. **However, it outlined a long list to “prepare a cryptographic inventory”, thus helping organizations assess risks, determine protocols in jeopardy, establish certification constraints, and determine all the dependencies that would be affected by a migration to quantum-resistant algorithms.** For instance, it’s very easy for a company to focus on updating its cloud infrastructure and entirely forget the smart cards, hardware secure modules, or VPNs that its employees use to access those services.


#### Crypto-agility


This is why the industry coined the term cryptographic (crypto) agility, which[NIST](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.39.pdf) defines as the “capabilities needed to replace and adapt cryptographic algorithms in protocols, applications, software, hardware, firmware, and infrastructures while preserving security and ongoing operations.” Put simply, it’s the ability to move to PQC with as little disruption to normal operations as possible. **Concretely, it means having a strong inventory of products that use cryptography, developing infrastructure that abstracts algorithms via APIs, or ensuring that all software is easily and securely updatable.** Crypto-agility also means implementing tests to verify an organization’s ability to use multiple algorithms and switch to a new one when the security community discovers vulnerabilities.


Crypto-agility is a core requirement for any PQC migration. Not only is crypto-agility important for limiting downtime and productivity loss, but it also ensures a secure migration. **For instance, if a migration eschews the principles of crypto-agility to hard-code certificate schema, protocols, key containers, and more, then not only is a company at a greater risk from a bad implementation, but it will make any update to the security implementation extremely difficult, causing further downtime and loss of productivity.** If the migration to quantum-resistant technologies teaches anything, it is that security is constantly evolving, and having a crypto-agile organization is the best way to respond to those evolutions.


#### Interoperability and hybrid deployment


Another tenet of crypto-agility is interoperability, meaning the guarantee that systems can continue to communicate with one another, regardless of the state of their migration. Indeed, if updating one system to a quantum-resistant algorithm prevents it from communicating with the rest of the infrastructure, it may cause serious issues. **It’s why many are adopting hybrid deployments, which use classical and post-quantum algorithms in a system that can switch between them on the fly, thus preserving interoperability.** Hybrid deployment may increase complexity and add computational costs. Yet, in a large organization, it is often the best way to ensure a smooth PQC migration.


#### Resource-constrained systems


Engineers know that PQC migration on some resource-constrained systems can also be challenging. They often have more restrictive RAM budgets and smaller bandwidths. Their microcontrollers may also have limited computational capabilities, and even those with cryptographic capabilities may lack the drivers, middleware, or APIs needed to efficiently execute a quantum-resistant algorithm. That’s why embedded system engineers must already plan for PQC. **While some products in the field may be upgraded to support PQC algorithms, others may find it difficult or even impossible, unless designers planned for PQC from the start.**


### How PQC impacts computing systems?


The migration to post-quantum cryptography (PQC) will have a profound impact on how the teams design, implement, and maintain computing systems. **To address the challenges inherent in PQC migration, engineers will need to work on multiple fronts at both the hardware and software levels.** On the hardware side, some post-quantum schemes require larger key sizes, signatures, and ciphertexts, as well as more compute-intensive operations. As a result, systems will need greater memory and storage to handle larger keys, certificates, and intermediate data. They will also require higher processing capability or dedicated accelerators to keep latency and power consumption within acceptable limits. Finally, some platforms may need new system architectures and security modules to accommodate new cryptographic workflows.


PQC migration is much more than simply using new algorithms. It requires reinforcing protection against physical and implementation-level attacks. The strongest quantum-resistant algorithm will fail if an attacker can exploit its implementation. For example, without proper safeguards, a hacker might recover secret keys or sensitive data by observing power consumption, electromagnetic emissions, timing differences, or faults introduced in the system. **To counter these side-channel attacks, systems must integrate protections and hardware-level countermeasures to ensure that quantum-resistant algorithms are not compromised by non-quantum weaknesses.** In other words, a highly secure PQC algorithm offers illusory protection if its implementation is vulnerable to attacks using affordable equipment.


### How is ST addressing PQC challenges?


The ST33KTPM


ST is rising to the PQC challenge and empowering migration initiatives with comprehensive software libraries and hardware solutions for both standard and secure microcontrollers. For instance, we are offering software cryptographic packages like **[X-CUBE-PQC on STM32](https://blog.st.com/x-cube-pqc/)** or the **[NesLib-PQML](https://www.st.com/content/st_com/en/campaigns/pqc-certified-secure-libraries-z26.html)** certified libraries on ST33K1M5 secure element, the trusted platform modules (TPMs) based on the ST33KTPM, and the NFC/eSE/eSIM ST54M combo solution for mobile, which covers key elements of major PQC algorithms.


**The[ST33KTPM](https://www.st.com/content/st_com/en/campaigns/st33ktpm-new-tpm-for-consumer-and-industrial-z26.html) is the first quantum-resistant TPM, FIPS 140–3 certified, demonstrating that it meets stringent security requirements while underlining its readiness for the post-quantum era.** As a TPM, the ST33KTPM serves as a hardware Root of Trust for servers, personal computers, IoT devices, and other embedded systems. By integrating PQC mechanisms directly into this critical security anchor, ST helps ensure that the integrity, authentication, and key management functions at the heart of these systems remain resilient against future quantum-capable adversaries. For instance, the ST33KTPM supports secure updates using LMS (Leighton–Micali Signature) as a stateful hash-based signature scheme to protect firmware authenticity.


We are also announcing the[ST54M](https://www.st.com/content/st_com/en/campaigns/st54m-pqc-hardware-accelerator-mobile-z26.html) , the latest generation of the ST54 family. It’s an advanced single-die solution designed to support next-generation connected services. **It combines a hardware accelerator for post-quantum cryptography (PQC) with NFC, secure element, and eSIM functionalities, delivering a future-ready solution for secure mobile connectivity and services.**


#### Enabling crypto-agility and PQC-readiness


The ST54M


Beyond new hardware, ST is also investing in software libraries and tools to help existing and future products become PQC-ready. A key objective is to enable crypto‑agility, meaning the ability to deploy, combine, or replace cryptographic algorithms over time without redesigning the entire system. Thanks to these libraries, developers can begin integrating PQC algorithms alongside classical ones (hybrid schemes), enabling a smoother and safer transition. Teams can also switch algorithms or change key sizes through system updates as standards evolve and new threats emerge. **It’s some of the most efficient ways to prepare existing platforms for cryptographically relevant quantum computers, even before such machines are widely available.**


---


1.


P. W. Shor, “Algorithms for quantum computation: discrete logarithms and factoring,” Proceedings 35th Annual Symposium on Foundations of Computer Science, Santa Fe, NM, USA, 1994, pp. 124–134,[doi: 10.1109/SFCS.1994.365700](https://doi.org/10.1109/SFCS.1994.365700) .↩︎


2.


Grover, Lov K. (1996–07–01). “A fast quantum mechanical algorithm for database search”. Proceedings of the twenty-eighth annual ACM symposium on Theory of Computing – STOC ’96. Philadelphia, Pennsylvania, USA: Association for Computing Machinery. pp. 212–219.[arXiv:quant-ph/9605043](https://doi.org/10.48550/arXiv.quant-ph/9605043)↩︎
