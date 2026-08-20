---
schema_version: "1.0.0"
document_id: "185a4e0bd9a9357b19b7609508053122fb4a8d30071a900650b973c5c48cd341"
company_key: "yc-veria-labs"
company: "Veria Labs"
source_id: "yc-veria-labs-rss-80fd8934189a"
canonical_url: "https://verialabs.com/blog/technical-breakdown-of-jolt/"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:58.723115+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:37a28d41a1a5ac1aa410059d29e1d76095b93140267a45a8a6c1b018eb35495a"
---

# A Technical Breakdown of Jolt's Cryptography (and the exploit)

**Breaking Down Jolt Cryptography** The Mathematics Behind the Exploit


Jul 13, 2026


#zkvm #cryptography


This is a technical analysis of the Dory vulnerability that was found and poc’d by our agent. This blog goes much deeper into the mathematics behind how the exploit actually worked. If you’ve landed in the wrong blog, you can read how our AI found it[here](https://verialabs.com/blog/hacking-a16z) .


## TL;DR


- Ordering of Fiat-Shamir in Sumcheck
- Missing constraint in the Dory proving library
- Exploiting the constraint and backsolving from the Fiat-Shamir ordering
- ` is_seven(6) == true` (6 == 7)


## About Us


At Veria Labs, we build AI pentesting agents that automatically find and fix security vulnerabilities in your application. Founded by members of the #1 competitive hacking team in the U.S., we’ve found critical vulnerabilities in every company we’ve worked with, from small startups to enterprise giants.


Think we can help secure your systems? We’d love to chat! Book a call[here](https://verialabs.com/contact) .


## What is Jolt?


We’ve covered[Jolt](https://github.com/a16z/jolt) in our other blog but in brief, Jolt is a zkVM for RISC-V built as a general purpose zkVM for proving arbitrary executions. While it can be used as a standalone library, Jolt is expected to be the backbone of the new[Zero](https://layerzero.network/zero) L1 chain currently being developed by LayerZero.


Jolt will facilitate private payments on the Zero network. This allows anyone to verify transactions without seeing details like wallet addresses, amounts, etc.


## Vulnerability Details


This section covers the math, the vulnerabilities, and how they were chained. As always, this may get quite messy.


### Background


**From a program to polynomials** : A zkVM proves “this program ran correctly and produced this output.” Jolt encodes the execution trace as a family of multilinear polynomials {Pi}i∈\[m\] over the BN254 scalar field 𝔽=𝔽r, where each Pi:𝔽n→𝔽 is the multilinear extension of its values on the boolean hypercube,


Pi(𝐱)=∑𝐛∈{0,1}nPi(𝐛)∏k=1n(xkbk+(1−xk)(1−bk)),𝐱∈𝔽n.


Checking the program reduces to checking a system of claims over the {Pi}.


**Sumcheck.** The sumcheck step certifies that a polynomial g=∑iλigi sums to a claimed value C over the hypercube without the verifier recomputing the sum:


C=?∑𝐱∈{0,1}ng(𝐱),λi=ℋ(τ)∈𝔽.


It runs one variable at a time. In round j the prover sends the univariate restriction


gj(X)=∑𝐱∈{0,1}n−jg(r1,…,rj−1,X,𝐱),


the verifier enforces the fold gj(0)+gj(1)=cj−1 against the running claim, samples a challenge rj∈𝔽, and updates cj=gj(rj). After n rounds the protocol collapses to a vector of opening claims at the random point 𝐫=(r1,…,rn).


{Pi(𝐫)=vi}i∈\[m\],vi∈𝔽,


Sumcheck is interactive however. To turn this non-interactive, Jolt applies a Fiat-Shamir transcript across all steps to sample the randomness. Jolt derives every challenge by hashing the running transcript τ of prover messages:


rj=ℋ(τ<j).


Security relies on this binding invariant. Anything that can change whether the proof is accepted must be absorbed into τ before the challenge that depends on it:


∀accept-relevant w:w→τ before r=ℋ(τ).


**Polynomial commitments ([Dory](https://github.com/a16z/dory) ).** The openings Pi(𝐫)=vi are discharged by a polynomial commitment scheme. Jolt batches its polynomials under powers of a challenge γ into one instance (𝒞∗,𝐫,y∗) with 𝒞∗=∑iγi𝒞i, and proves it with Dory.


Dory is a pairing based scheme, also developed by a16z, over 𝔾1,𝔾2,𝔾T with a pairing e:𝔾1×𝔾2→𝔾T. Dory asserts that a committed polynomial 𝒞∈𝔾T satisfies P(𝐳)=y with a final pairing identity of the form ∏ke(Lk(𝐳),Rk)=𝒞⋅e(⋅,e2), where e2∈𝔾2 encodes the claimed value.


### The Ordering of Fiat-Shamir in Sumcheck


The ZK sumcheck fixes its random challenges before the prover commits to the hidden output claims. The batching coefficients λ are drawn up front, the round polynomials are folded to produce 𝐫, and only afterwards are the hidden output claims added into the transcript:


```text
1   let   batching_coeffs   =   transcript  .  challenge_vector  (sumcheck_instances  .  len  ());    // λ ← H(τ)  [early]    2   // … n sumcheck rounds fold r_1..r_n out of τ …    3   let   oc_committed   =   pedersen_gens  .  commit_chunked  (  &  output_claim_values, rng);          // Com(v_i; ρ_i)    4   transcript  .  append_commitments  (  b"output_claims_coms"  ,   &  output_claims_commitments);   // τ += Com(v)  [late]
```


By the time the openings 𝖢𝗈𝗆(vi;ρi) enter τ, every challenge that will test them is fixed. Write 𝝌=(λ,𝐫,…) for those challenges and Φ(𝐯,𝝌)=0 for the BlindFold output constraint the openings must satisfy. There’s no way around this ordering: the output claims are values p(𝐫), and 𝐫 (like λ) is the sumcheck’s challenge stream, so the claims don’t exist until the challenges that produce them are already drawn. Binding the openings is intended to be left entirely to the Stage 8 opening proof.


Technically, that does give a modified prover some help. With 𝝌 known first, and Φ affine in each opening, changing one coordinate vk by δ moves the constraint along a straight line,


Φ(𝐯+δ,𝐞k,,𝝌);=;Φ(𝐯,𝝌)+δ⋅∂vkΦ,∂vkΦ≠0,


so the offset that lands it back on zero is just δ⋆=−,Φ(𝐯,𝝌)/∂vkΦ. The prover shifts the hidden opening and re-commits; the only thing that changes is the freshly-formed 𝖢𝗈𝗆(vk;ρk), which nothing has constrained yet:


```text
1   let   delta_i   =   (target_output_claim   -   base_eval)   *   derivative_i  .  inverse  ()  .  unwrap  ();   // δ* = -Φ/∂Φ    2   output_claim_values[output_index]   +=   delta_i;     // v_k ↦ v_k + δ*    3   *  opening_value             +=   delta_i;
```


But by design this isn’t supposed to work: the Stage 8 Dory opening is supposed to reject any 𝐯 whose openings don’t match the real polynomials Pi at 𝐫.


### The Vulnerability: Missing Constraint in Dory Dependency


Jolt calls the Dory library for all proving/verifying related to Dory. In Dory’s zk verifier, it never ties the proof to the point it is supposed to be evaluated at. Stage 8 first collapses all the committed polynomials into a joint opening (𝒞∗, 𝐫, y∗) with 𝒞∗=∑iγi𝒞i, and hands it to` verify_zk` with the point 𝐫 but no value, since in ZK the evaluation stays concealed as ycom=𝖢𝗈𝗆(y∗;⋅):


```text
1   let   hiding_evaluation_commitment   =     PCS  ::  verify_zk  (    2         &  joint_commitment, pcs_opening_point  .  as_slice  (),    // (C*, r) ;  no y*    3         &  proof  .  joint_opening_proof,   &  preprocessing  .  pcs_setup, transcript)  ?  ;
```


Following this call into the Dory function` dory-pcs::verify_evaluation_proof` in the dependency, the whole thing turns on a single group element e2∈𝔾2 that anchors the final pairing identity. In the transparent (non-ZK) path, e2 is built from the claimed value which ties both the value y and the point 𝐳 into the equation:


(𝐭𝐫𝐚𝐧𝐬𝐩𝐚𝐫𝐞𝐧𝐭)e2=y⋅g2,verify ∏ke(Lk(𝐳),Rk)=𝒞⋅e(⋅,e2).


In ZK mode though, it reads e2 from the proof and discards y entirely:


(𝐳𝐤)e2←π.e2 (prover-chosen),y unused,ycom←π.ycom.


```text
1   #[cfg(feature   =     "zk"  )]    2   let   (e2, is_zk)   =     match   (  &  proof  .  e2,   &  proof  .  y_com) {    3          (  Some  (pe2),   Some  (yc))   =>   {   /* sub-proofs about y_com only */   (  *  pe2,   true  ) }    4          (  None  ,   None  )   =>   (setup  .  g2_0  .  scale  (  &  evaluation),   false  ),    5          _   =>     return     Err  (  DoryError  ::  InvalidProof  ),    6   };    7   #[cfg(not(feature   =     "zk"  ))]    8   let   (e2, _is_zk)   =   (setup  .  g2_0  .  scale  (  &  evaluation),   false  );    // e2 binds `evaluation`
```


The side proofs check that ycom is a well formed commitment, but they never check that it actually commits to P∗(𝐳). So with e2 as a free group element the prover supplies, the acceptance relation stops depending on 𝐳 altogether:


∃π:V(𝒞∗,𝐳,π)=1 for every 𝐳∈𝔽n.


### The Full Exploit


For the PoC, our agent setup a simple Rust program of:


```text
1   fn     is_seven  (x  :     u64  )   ->     bool   {    2          x   ==     7    3   }
```


with the goal of forging a proof to claim` is_seven(6) == true` .


Since the vulnerability allows an attacker to forge a Dory proof, the first observation on the ordering of the sumcheck can be backsolved before abusing the Dory commitment. The attack runs in 2 main steps. Let 𝐲∗∈{0,1}8 be the public output region. In an honest case: 𝗂𝗌_𝗌𝖾𝗏𝖾𝗇(6) produces y0∗=0 (false).


First, flip the public output bit (from false to true) and rewrite the single store instruction that writes it, so the trace claims the flipped value:


y0∗↦y0∗⊕︎1,rewrite the unique 𝖲𝖡/𝖲𝖧/𝖲𝖶/𝖲𝖣→𝚙𝚘𝚜𝚝_𝚟𝚊𝚕𝚞𝚎.


The witness is now internally inconsistent since the BlindFold constraint breaks (Φ(𝐯,𝝌)≠0) An honest prover would abort on 𝐀𝐳∘𝐁𝐳≠𝐂𝐳.


Because the sumcheck never bound the claim values, solve for the offset that puts the broken constraint back to zero and shift the hidden output opening by it:


δ∗=−Φ(𝐯,𝝌)/∂vkΦ,vk↦vk+δ∗ ⇒ Φ=0,Ξ(⋅) fixed.


The prover now holds a coherent but false claim set, with a forged joint value y^∗≠P∗(𝐫).


The forged claim no longer matches the real polynomial at the honest point. But a multilinear polynomial is degree ≤1 in each coordinate, so its value moves linearly as you sweep one coordinate. This means you can solve for a single alternate coordinate rℓ′ at which the real polynomial hits the forged value exactly:


𝐫′=𝐫 with rℓ′=y^∗−P∗(𝐫|zℓ=0)P∗(𝐫|zℓ=1)−P∗(𝐫|zℓ=0) ⟹ P∗(𝐫′)=y^∗.


The prover emits a perfectly honest Dory opening at this wrong point, π←𝗈𝗉𝖾𝗇_𝗓𝗄(P∗,𝐫′). The verifier checks at the honest point 𝐫. Since Dory’s ZK check ignores the point, it accepts anyway (V(𝒞∗,𝐫,π)=1).


This was run against an unchanged verifier and both honest and forged proofs:


𝚑𝚘𝚗𝚎𝚜𝚝.𝚋𝚒𝚗→𝖮𝗄 (y0∗=0),𝚙𝚘𝚌.𝚋𝚒𝚗→𝖮𝗄 (y0∗=1):6=7.


## Impact in the zkVM


This is a total break of soundness in ZK mode. For an arbitrary false statement S there exists an constructible π with 𝖵𝖾𝗋𝗂𝖿𝗒zk(S,π)=1, against the unchanged verifier. The transparent path, binding both y (via e2=y⋅g2) and 𝐳, is unaffected.


Since Jolt is used as a general purpose zkVM as well as being the backbone of the Zero L1 chain, this makes it the highest impact for a zkVM. On the blockchain, depending on how it will be used, can most likely be used to forge false transactions or introduce an infinite mint.


## Remediation


Patching the Dory vulnerability can be done in either the library or Jolt itself. Bind the opening point. The cleanest fix would be to directly patch this on the Dory library side. That said, there is a comment that claims the caller must bind this before calling any Dory functions. So, to fix this on the Jolt side, we can apply the patch:


```text
1   --- a/crates/jolt-dory/src/scheme.rs    2   +++ b/crates/jolt-dory/src/scheme.rs    3   @@ fn open_zk(poly, point, _eval, setup, hint, transcript) {    4               let ark_point: Vec<ArkFr> = point.iter().rev().map(jolt_fr_to_ark).collect();    5   +        // Bind the opening point before Dory derives its challenges.    6   +        transcript.append(&LabelWithCount(b"zk_dory_point_bind", point.len() as u64));    7   +        for p in point {    8   +            p.append_to_transcript(transcript);    9   +        }    10               let mut dory_transcript = JoltToDoryTranscript::new(transcript);
```


```text
1   --- a/crates/jolt-dory/src/scheme.rs    2   +++ b/crates/jolt-dory/src/scheme.rs    3   @@ fn verify_zk(commitment, point, proof, setup, transcript) {    4               let ark_commitment = jolt_gt_to_ark(&commitment.0);    5   +        // Mirror the prover: bind the same point before verification draws challenges.    6   +        transcript.append(&LabelWithCount(b"zk_dory_point_bind", point.len() as u64));    7   +        for p in point {    8   +            p.append_to_transcript(transcript);    9   +        }    10               let mut dory_transcript = JoltToDoryTranscript::new(transcript);
```


With this in place an honest proof still verifies, and a proof produced at a different point is rejected:


```text
verify at correct point = Ok(...)    verify at wrong point   = Err(VerificationFailed)
```


## Conclusion


This is a result of feature and a missing check that were chained together to fulfill the full exploit chain. While each on there own are not exploitable, exploiting both at the same time allows for complete proof forgery. The Dory vulnerability was reported to a16z which was quickly patched.


Fixes from the team are listed here:


On the Jolt side,[https://github.com/a16z/jolt/pull/1664](https://github.com/a16z/jolt/pull/1664) and on the Dory side:[https://github.com/a16z/dory/pull/23](https://github.com/a16z/dory/pull/23)


## PoC


All files related to the PoC can be found in this GitHub repo:


[https://github.com/verialabs/Jolt-PoC](https://github.com/verialabs/Jolt-PoC)
