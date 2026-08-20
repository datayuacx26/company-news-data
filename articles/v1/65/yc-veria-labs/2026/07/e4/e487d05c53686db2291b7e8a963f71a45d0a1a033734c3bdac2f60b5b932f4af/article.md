---
schema_version: "1.0.0"
document_id: "e487d05c53686db2291b7e8a963f71a45d0a1a033734c3bdac2f60b5b932f4af"
company_key: "yc-veria-labs"
company: "Veria Labs"
source_id: "yc-veria-labs-rss-80fd8934189a"
canonical_url: "https://verialabs.com/blog/hacking-a16z/"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:58.723115+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:e343047b00d361b5247cba1b1b293014ea5e951ecc23d2634efc53ff13879dd2"
---

# Hacking a16z: Breaking Jolt and Dory

**Hacking a16z** Breaking Jolt and Dory


Jul 13, 2026


#zkvm #cryptography


Following on testing our agent against hard codebases like zkVMs, we decided to point our agent at Jolt.[Jolt](https://github.com/a16z/jolt) is a zkVM designed and maintained by Andreessen Horowitz that will be the backbone of[Zero](https://layerzero.network/zero) , an L1 set to release on mainnet by LayerZero.


As our product keeps developing, the next best thing to do is to see if our agent can not only find bugs but also connect and exploit bugs in dependencies. If you want to read the technical breakdown, you can read all the technical details[here](https://verialabs.com/blog/technical-breakdown-of-jolt) .


## About Us


At Veria Labs, we build AI pentesting agents that automatically find and fix security vulnerabilities in your application. Founded by members of the #1 competitive hacking team in the U.S., we’ve found critical vulnerabilities in every company we’ve worked with, from small startups to enterprise giants.


Think we can help secure your systems? We’d love to chat! Book a call[here](https://verialabs.com/contact) .


## Beyond Solo Bug Hunting


Severity scoring in most tooling is done one finding at a time. Anything that looks minor in isolation is left unfixed since there will always be more critical and higher severity findings to fix first. Bug if one critical finding can break a system, what can 2 or more low severity findings chained together do?


### Vulnerability Chaining


Exploits often require stringing multiple observations together at the same time. Surface level bugs that exist near a data entrypoint are found rather “easily.” Most tools evaluate findings on at time, so they miss exploits that only exists after the first layer and rely on slight imperfections above them. Also, because tools evaluate whether a finding is real or not based on how far they get in the codebase, if a vulnerability only exists in the middle of the process and can’t be reached in the first place, it will get filtered out. A finding in the middle of the process will get filtered out since the entrypoint won’t accept those inputs anyways unless another bug gets by that layer first.


If there is a feature in the business logic that gives just enough access to turn a finding from a code level bug to exploitable vulnerability, that has to be marked as well. The entire interaction between different vulnerabilities must be reasoned together and not just scanning files in isolation.


### Downstream Dependencies


All modern software is relies on dependencies and public libraries. A vulnerability in a dependency that directly affects your codebase is your vulnerability, and it has to be surfaced as such. An agent has to be able to follow how third party imports are actually used and called, so it can find the dependency vulnerabilities that genuinely reach the app.


Traditional scanners can tell you a dependency is outdated or that it carries a known CVE. What they can’t tell you is whether you actually need to upgrade, or more importantly, whether the way you call that dependency even exposes you to the vulnerability in the first place. Also, scanning all third party dependencies creates an extremely large surface area. It’s important to be able to select which packages to actually audit more thoroughly and which ones to leave alone.


## Why Run on Jolt?


We will be skipping over what a zkVM is as it was briefly covered in the[last blog](https://verialabs.com/blog/forging-transactions-on-aleo/) .


Jolt is a zkVM for RISC-V, built by a16z crypto as both a general purpose zkVM as well as the backbone of Zero. Zero is a private L1 blockchain under development by LayerZero. It uses zero knowledge proofs to keep transactions confidential while remaining verifiable by anyone on the network. Because it’s also general purpose, it can prove arbitrary computation, which makes any break in it especially critical.


We’ve also heard that security with Jolt has been an enormous effort for a16z. Since the start of the year, the team changed its whole approach and began feeding the entire codebase into every new model as it came out, and burning as many tokens as physically possible.


Jolt itself has also been rewritten two or three times over the past year, with LayerZero also maintaining its own internal fork for Zero. On top of the model driven review, there’s a formal verification effort rewriting parts of Jolt (the bytecode expansions) in Lean which has turned up even more bugs. Along with formal verification and AI, manual audits from an in house and external cryptographers round it out.


But our agent broke it anyway😉


## How Our AI Agent Found It


We’ve poured enormous effort into pushing our agent on the two capabilities above. Chaining small imperfections, and tracing vulnerabilities down into dependencies. Jolt was where both came together and showed a combination of both.


The agent’s first lead looked like the an entire forgery but turned out to be intended behavior on its own, and the real vulnerability was buried a layer inside a dependency. What made the difference was that the agent didn’t stop at the first interesting looking finding, and it didn’t throw that finding away when it turned out to be by design either.


### The Initial Lead: A Suspicious Fiat-Shamir Ordering


Looking through the agent’s traces, the first interesting thing came from Jolt’s ZK Sumcheck stage. The agent noticed that some verifier challenges were being derived before the prover’s hidden output claims were bound into the transcript. In a Fiat-Shamir based proof, challenges are supposed to be unpredictable until the prover has already committed to the data those challenges will check. Seeing that order apparently flipped, the agent’s first conclusion was that this was the vulnerability. A classic Fiat-Shamir ordering flaw that would let a malicious prover choose its claims after seeing the challenges.


If the prover can pick hidden output claims after the batching challenges are fixed, it can nudge those claims to whatever it needs, and the proof appears forgeable on the spot.


### Realizing It Was Intended


The obvious next step for the agent was to actually test that theory and turn it into an actual proof forgery. When it did, it hit a failure, and it revealed that the ordering was by design.


The hidden output claims are literally the evaluations of the trace polynomials at the sumcheck’s random point, and that point is itself a Fiat-Shamir challenge. The claims can’t even exist until the challenges that produce them have already been drawn. The Jolt designers knew this and deferred the binding of those claims to the Stage 8 Dory opening proof. It’s supposed to reject any claim that doesn’t match the real polynomials.


So on its own, the Fiat-Shamir ordering was not exploitable and was an intended consequence of how ZK sumcheck has to work, with a later check. A tool that scores findings one at a time would have marked it as a false positive and moved on.


Instead of discarding the finding, our agent turned decided to look into a little more. The entire security of the ordering was on how the Dory opening proof was implemented. So rather than trash this “finding”, the agent went to the Dory library to verify this.


### The Real Vulnerability: Buried in Dory


Jolt uses[Dory](https://github.com/a16z/dory) , another a16z library, as its polynomial commitment scheme to prove the final batched opening. Instead of treating the dependency as a trusted black box, the agent followed Jolt’s verifier all the way down into Dory’s own code.


In ZK mode, Dory hides the evaluation value behind an evaluation commitment called` y_com` . That is safe only if the proof also cryptographically ties` y_com` to the statement the verifier is checking. The committed polynomial, the verifier’s opening point, and the claimed value.


The agent found that this binding was incomplete. Dory’s ZK verifier would accept a proof whose internal` y_com` was self-consistent, without tying it back to the verifier’s actual opening point. In practical terms, a prover could build a Dory proof at one carefully chosen point while Jolt’s verifier believed it was checking the opening at a completely different point, as long as the hidden commitment lined up with the value the rest of the proof expected.


This was the real vulnerability, and it was in the Dory dependency. It also explained why the Fiat-Shamir observation mattered. The Stage 8 Dory opening which was supposed to prevent the Fiat-Shamir ordering from being exploited was broken.


### Chaining


Now was the part of putting the two together. The intended sumcheck ordering gave the prover the freedom to choose forged hidden claims, and the Dory bug removed the one check that was supposed to catch them.


To prove it end to end, the agent set up a tiny program:


```text
1   #[jolt  ::  provable(heap_size   =   128, max_trace_length   =   1024)]    2   fn     is_seven  (x  :     u64  )   ->     bool   {    3          x   ==     7    4   }
```


The goal was to forge a proof that` is_seven(6)` returns` true` , when the honest answer is obviously` false` .


The first attempt at just using the sumcheck ordering didn’t work. The sumcheck ordering let the prover choose hidden output claims after seeing the batching challenges, but those forged claims still had to pass the final Dory opening proof. With an honest polynomial commitment check, the attack would died there.


Once the ordering let the agent forge hidden BlindFold claims, it used the Dory binding gap to find an alternate opening point where the committed joint polynomial evaluated to the forged claim. It then generated a perfectly honest Dory ZK proof at that alternate point, while Jolt’s unchanged verifier continued to believe it was checking the original, verifier derived point.


The chain looked like this:


1. Use the sumcheck’s Fiat-Shamir ordering to compute the batching coefficients before the hidden output claims are committed.
2. Adjust the hidden claims so the batched BlindFold equations still pass, even though the public output is false.
3. Keep the Pedersen/Hyrax row commitments honest by making those adjustments before committing the rows.
4. Use the Dory ZK binding gap to make the final polynomial opening proof accept the forged joint claim at an alternate point.
5. Hand the resulting proof to Jolt’s normal verifier, with honest preprocessing and no verifier modification.


Veria AI created a valid proof for a program where the expected output for` is_seven(6)` should be false, but the accepted output was true. The verifier was not patched, configured, or given any malicious preprocessing values.


## The Future of Application Security


We think this result is a preview of where all of application security is going. Finding much more hidden bugs and auditing dependencies at real depth is enormous, patient work, the kind of thing a manual pentester might comb through over a two to three week audit.


It requires holding a whole system in context and following leads across boundaries until an exploit either materializes or is ruled out, including leads that look like dead ends at first. That is precisely what our agent did against Jolt, and it’s the capability we believe every serious security program will need going forward.


So what’s next for Veria Labs? We’re going to continuously improve our agent to find more and more complex vulnerabilities.
