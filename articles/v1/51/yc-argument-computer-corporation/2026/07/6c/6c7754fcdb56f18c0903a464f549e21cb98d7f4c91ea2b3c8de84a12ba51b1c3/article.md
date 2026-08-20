---
schema_version: "1.0.0"
document_id: "6c7754fcdb56f18c0903a464f549e21cb98d7f4c91ea2b3c8de84a12ba51b1c3"
company_key: "yc-argument-computer-corporation"
company: "Argument Computer Corporation"
source_id: "yc-argument-computer-corporation-news-import-6d03b0bcc333"
canonical_url: "https://argument.xyz/blog/lurk-beta/"
published_at: null
first_seen_at: "2026-07-21T07:37:57.416159+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:e8cee52aafb5e50d799035f463e0666ddb30841a341e7aa70e5f1289f1565abe"
---

# Lurk Beta: The Zero-Knowledge Proof Programming Language

Today, we are thrilled to announce the Beta Release of Lurk, marking a significant milestone in our journey to create the premier Zero-Knowledge Proof Programming Language. This post will recap Lurk’s evolution, showcase its current capabilities, and give a glimpse into its promising future.


## Past (Alpha)


With the[Lurk Alpha](https://argument.xyz/blog/announcing-lurk-alpha) release, we introduced a Turing-complete programming language designed for zero-knowledge proofs. It allowed developers to escape the confines of manually written circuits and embrace a high-level, expressive alternative. Central to Alpha was a universal verifier, capable of handling any computation expressible in Lurk’s language. This version laid the foundation for what Lurk is today, providing a robust, audited platform for early adopters.


## Present (Beta)


### Correctness


In the Beta version, we have significantly enhanced the correctness of Lurk’s reduction circuit by transitioning from hand-written circuits to procedurally generated ones. This advancement is made possible through our[Lurk Evaluation Model](https://github.com/argumentcomputer/lurk-rs/blob/main/src/lem/mod.rs) (LEM) DSL, which also facilitates the[implementation](https://github.com/argumentcomputer/lurk-rs/blob/main/src/lem/eval.rs) of the[Lurk reduction step](https://github.com/argumentcomputer/lurk-rs/blob/main/notes/reduction-notes.md) .


### Customization


Lurk Beta introduces Coprocessors, a major leap in customization. These allow for seamless integration of domain-specific instructions, enhancing program expressiveness and efficiency, especially in cryptographic functions. Additionally, with Circom Scotia, integrating Lurk-compatible Circom circuits into your projects has never been easier.


- Custom instructions are now possible through[Coprocessors](https://github.com/argumentcomputer/lurk-rs/blob/main/src/coprocessor/mod.rs) , enabling applications to define and implement domain-specific instructions that integrate as seamlessly as Lurk’s built-in operators.
- User-defined Coprocessor circuits facilitate efficient proofs for a variety of functions, particularly[cryptographic ones](https://github.com/argumentcomputer/lurk-rs/blob/main/examples/sha256_ivc.rs) .
- With[Circom Scotia](https://github.com/argumentcomputer/circom-scotia) , defining Lurk-compatible circuits in Circom becomes straightforward, and Lurk’s support for[Circom Coprocessors](https://github.com/argumentcomputer/lurk-rs/blob/main/src/coprocessor/circom) enhances this integration.


### Performance


The Beta release transforms Lurk’s performance profile. While in an IVC proof, the addition of coprocessors to the Lurk evaluation circuit could bloat the cost of every folding step, with the integration of SuperNova, this is no longer a concern. Our hybrid model provides a balance between general-purpose programming and optimized arithmetic circuits, ensuring optimal performance.


- The[NIVC support](https://github.com/argumentcomputer/lurk-rs/blob/main/src/proof/supernova.rs) in Lurk Beta ensures that the cost of external circuits is only incurred during actual use, effectively eliminating the performance penalty of Lurk Alpha’s single universal circuit.
- The hybrid performance model equips Lurk with the versatility of a fully general, expressive, data-centric programming language, combined with the efficiency of custom arithmetic circuits authored in Circom or Bellpepper.


### Ecosystem and Stack


Lurk Beta benefits from the full-stack support of Lurk Lab for the proving pipeline. Our commitment to open-source, community-focused projects enhances the agility and responsiveness of our stack, allowing us to quickly adapt and innovate in response to the needs of the ZK and Folding communities.


Lurk is designed to be blockchain-friendly, particularly for efficient verificationperformance on Ethereum. This is evident in its development of low-level components, such as the switch from Pasta to Bn256/Grumpkin curve cycle, enabling the use of Ethereum precompiles for on-chain verification.


#### Arecibo


- Our friendly fork of Nova,[Arecibo](https://github.com/argumentcomputer/arecibo) , plays a crucial role in our ecosystem. Detailed in[yesterday’s blog post](https://argument.xyz/blog/arecibo-supernova) , Arecibo serves as an incubator for new ideas, allowing for API finalization and design reviews, as seen with the adoption and refinement of[SuperNova](https://github.com/argumentcomputer/arecibo/blob/dev/src/supernova/mod.rs) .


#### Bellpepper


- [Bellpepper](https://github.com/argumentcomputer/bellpepper) , a fork of[Bellperson](https://github.com/filecoin-project/bellperson) , focuses on providing a common library for specifying arithmetic circuits, underpinning Neptune, Arecibo/Nova, and Lurk itself. Its development emphasizes broad utility, as seen in Bellpepper’s[Community-Maintained Cryptographic Gadgets Library](https://github.com/argumentcomputer/bellpepper-gadgets/tree/main/crates) .


#### Neptune


- We continue to maintain and optimize[Neptune](https://github.com/argumentcomputer/neptune) , our Poseidon hashing and circuit library, leveraging Bellpepper’s advancements for significant improvements in synthesis time.
- Taking advantage of Bellpepper’s optimized witness-generation circuit, we implemented[optimized witness-generation](https://github.com/argumentcomputer/neptune/blob/main/src/circuit2_witness.rs) .
- This led to optimizations yielding[16x improvements in synthesis time](https://github.com/argumentcomputer/neptune/pull/190) .


## Future (1.0 and beyond)


### Lurk 1.0


-


**ZeroMorph Integration** : We’re integrating the[ZeroMorph](https://eprint.iacr.org/2023/917) polynomial commitment scheme into Nova, promising faster verification processes. This integration, coupled with the[CycleFold protocol](https://eprint.iacr.org/2023/1192) , aims to produce smaller proofs and enhanced efficiency.


-


**LEM-Driven Development** : LEM ensures the equivalence between the Lurk interpreter’s description and the enforcing circuit. It enables faster iterations on algorithmic improvements to reduce Lurk iterations (e.g., more efficient data structures) and the introduction of new language features beyond Alpha’s core (e.g., exception management, or new operators such as the[list](https://github.com/argumentcomputer/lurk-rs/issues/944) and[apply](https://github.com/argumentcomputer/lurk-rs/issues/950) operators from Common Lisp, and[type assertions](https://github.com/argumentcomputer/lurk-rs/issues/658) )


-


**Parallel Folding** : Inspired by Privacy Scaling Explorations’ ParaNova and Lagrange’s contributions, we’re exploring parallel folding to integrate these insights into Lurk. This includes work on[Arecibo Paranova Integration](https://github.com/argumentcomputer/arecibo/pull/86) and expanding to encompass[Binary PCD](https://lagrangelabs.notion.site/Nova-from-IVC-to-general-PCD-for-zkMapReduce-d04afec2d70c47db9dfb67e84d0d796e) .


### Concurrent Lurk (Lurk 2.0?)


- We are envisioning an Actor Model Concurrency for Lurk, enabling proofs of truly distributed computation and opening new horizons in zero-knowledge proof programming.
