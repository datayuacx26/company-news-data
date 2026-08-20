---
schema_version: "1.0.0"
document_id: "712a16dabfc2957d991eb0e65b1253c557b9c25aeaaf8525198eea04f8739b33"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/interactive-proofs-distribution-properties"
published_at: null
first_seen_at: "2026-07-21T07:16:20.305331+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:b4997cda955ba706477f19fc292a747137b0590f7836a40cf603434538f583e0"
---

# Interactive Proofs for General Distribution Properties

[View publication](https://eccc.weizmann.ac.il/report/2024/094/)


Suppose Alice has collected a small number of samples from an unknown distribution, and would like to learn about the distribution. Bob, an untrusted data analyst, claims to have run a sophisticated data analysis on the distribution and makes assertions about its properties. When and how is it possible for Alice to efficiently verify Bob’s claims (using fewer resources than would be needed to run the analysis herself)? We construct interactive proof systems for general distribution properties that can be decided by bounded-depth circuits. Taking N to be an upper bound on the distribution’s support size, and D a bound on the depth of a uniform Boolean circuit that gets a complete description of the distribution and decides the property, the verifier’s sample complexity, running time, and the communication complexity are all bounded by Õ(D+N^0.99). The number of rounds is O(D·log(N)). The proof system is doubly-efficient: the honest prover runs in polynomial time and quasi-linear sample complexity. We also show similar results for properties that can be decided by a bounded-depth Turing machine (that gets as input a complete description of the distribution). We remark that even for simple properties, deciding the property without a prover requires quasi-linear sample complexity and running time. Prior work \[Herman and Rothblum, FOCS 2023\] demonstrated sublinear interactive proof systems, but only for the much more restricted class of label-invariant distribution properties.


- † Weizmann Institute of Science
- ** Work done while at Apple
