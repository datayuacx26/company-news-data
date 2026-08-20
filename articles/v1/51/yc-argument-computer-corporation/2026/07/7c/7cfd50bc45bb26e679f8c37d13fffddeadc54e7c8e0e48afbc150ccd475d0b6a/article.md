---
schema_version: "1.0.0"
document_id: "7cfd50bc45bb26e679f8c37d13fffddeadc54e7c8e0e48afbc150ccd475d0b6a"
company_key: "yc-argument-computer-corporation"
company: "Argument Computer Corporation"
source_id: "yc-argument-computer-corporation-news-import-6d03b0bcc333"
canonical_url: "https://argument.xyz/blog/sphinx-oss/"
published_at: null
first_seen_at: "2026-07-21T07:37:57.416159+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:c0b584d0d4952b268af4c8b8e0386c6e69a579246b293d4c6e2c1f0932fb2cae"
---

# Argument is Open-Sourcing Sphinx

Argument Computer Corporation (Argument) is open-sourcing[Sphinx](https://github.com/argumentcomputer/sphinx) , our fork of[Succinct](https://succinct.xyz/) Labs’[SP1 zero-knowledge virtual machine (ZKVM)](https://github.com/succinctlabs/sp1) .


Sphinx underpins critical elements of our zero-knowledge proof efforts, including light clients in collaboration with[Wormhole](https://wormhole.foundation/blog/wormhole-foundation-awards-contributor-grant-to-lurk-lab-to-bring-trustless-transfers-to-wormhole-with-zk-proofs) and[Kadena](https://www.kadena.io/blog/kadena-announces-partnership-with-lurk-lab-to-build-zk-bridge) . It also drives the forthcoming STARK engine of[Lurk](https://github.com/argumentcomputer/lurk-rs) , Argument’s next-generation zero-knowledge virtual machine


[Sphinx](https://github.com/argumentcomputer/sphinx) retains the same architecture as[SP1](https://github.com/succinctlabs/sp1) , which integrates the[Plonky3](https://github.com/Plonky3/Plonky3) prover toolkit, as well as other[prior work](https://github.com/succinctlabs/sp1/blob/v1.0.1/README.md#acknowledgements) . We are immensely grateful to Succinct Labs for their work on SP1, which continues to accelerate advancements in the proof ecosystem in a fully open-source manner. The results we’ll announce in the coming days would not have been possible without their contributions.


The[Sphinx VM](https://github.com/argumentcomputer/sphinx) can generate cryptographic proofs of RISC-V assembly (specifically RV32IM). It has provided us with a solid foundation from which we’ve designed the next generation of the Lurk ZKVM, to replace our[previous backend](https://argument.xyz/blog/arecibo-supernova) based on[Nova](https://github.com/microsoft/Nova) and[SuperNova](https://eprint.iacr.org/2022/1758) . We developed Sphinx privately until we were confident that its performance and characteristics met our technical goals and those of our customers. We are pleased to report that this approach was highly successful, as our upcoming blog posts will detail. Sphinx is named after the[Sphinx observatory](https://en.wikipedia.org/wiki/Sphinx_Observatory) , marking the next iteration of our star-gazing adventure!


Building on this success, we now plan to contribute our enhancements back to[SP1](https://github.com/succinctlabs/sp1) , just as we previously did with[Nova](https://github.com/microsoft/Nova/pulls?q=+is%3Apr+author%3Ahuitseeker+) .


## What’s in Sphinx Today?


We are particularly excited by SP1’s[precompile-centric architecture](https://www.youtube.com/watch?v=vKZWHtWFlJ0) , which allows developers to add custom circuits for certain application-specific operations in the VM with minimal overhead. We had adopted a similar principle[in the prior version of Lurk](https://argument.xyz/blog/lurk-beta) and are convinced this approach bridges the gap between ZKVMs and custom ZK, leveraging the best of both worlds. The lookup-based communication bus used by precompiles also enables us to implement[new features](https://www.youtube.com/watch?v=qih97RQNGFk) , and we certainly intend to make those lookups shine in the next version of Lurk.


Sphinx offers several features for light client developers, including:


- Chips for field operations defined on quadratic field extensions,
- Precompiles for the G1 and G2 groups of the BLS12-381 curve, leveraging the above,
- A sign-agnostic square root chip,
- Decompression precompiles for Secp256k1 and BLS12-381 that respect their curve-specific notions of sign.


We have integrated these precompiles into our fork of[bls12_381](https://github.com/argumentcomputer/bls12_381/tree/zkvm) , demonstrating significant speed improvements.


There are additional changes1 in development, aimed at making the prover more adaptable to various input instructions, given that the design of[Lurk](https://eprint.iacr.org/2023/369) directly interprets programs without emulating a hardware-oriented[instruction-set-architecture](https://en.wikipedia.org/wiki/Instruction_set_architecture) like RISC-V.


Sphinx is currently undergoing rapid development and has not yet completed an audit (unlike SP1).


## Why Are We Open Sourcing?


We believe that zero-knowledge doesn’t have to be zero-sum. Open-source software moves the whole field forward, and drives standards that will enable the next generation of builders. The fully open and permissive licensing approach taken by projects like Succinct’s SP1 and Polygon’s Plonky3 is not only developer-friendly, it’s also in our opinion the best way to build high-quality cryptographic software. As[Linus’ Law](https://en.wikipedia.org/wiki/Linus's_law) says: “many eyes make all bugs shallow”.


We’re excited to contribute to the fast-growing SP1 ecosystem with our release of Sphinx, and we look forward to continuing to share and upstream our work as we build upon this foundation for future projects.


## Stay Tuned!


In the coming days, we will provide more details about our work with Sphinx, including exciting[updates on our light client projects](https://argument.xyz/blog/aptos-eth) and the next version of Lurk!


## Footnotes


1.


For example, a static approach to generic column management using[hybrid-array](https://github.com/RustCrypto/hybrid-array/tree/master) , and a slightly more flexible` MachineAir` trait under development.↩
