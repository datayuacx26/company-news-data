---
schema_version: "1.0.0"
document_id: "da45b3e1b3c5f44d09e8dcc14b8abdfec676d34c42bb8c1028120c6b82989083"
company_key: "yc-argument-computer-corporation"
company: "Argument Computer Corporation"
source_id: "yc-argument-computer-corporation-news-import-6d03b0bcc333"
canonical_url: "https://argument.xyz/blog/arecibo-supernova/"
published_at: null
first_seen_at: "2026-07-21T07:37:57.416159+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:d35e9d4ab128a942ec066747d36f2d6508adc72e77d26cd1cfc39ba033c47c67"
---

# Announcing the first Arecibo release

We’re happy to announce the first crate release of[Arecibo](https://github.com/argumentcomputer/arecibo) , our fork of[Nova](https://github.com/microsoft/Nova) . Arecibo is intended to be an “incubation project” for Nova, testing out implementations of improvements to the Nova folding framework, and back-contributing these changes to the upstream project. Please consider that as a project meant to evolve quickly, it makes few guarantees of backward-compatibility, and that the Arecibo-specific changes have not been reviewed by the Nova maintainers.


However, we are building our main project,[Lurk](https://github.com/argumentcomputer/lurk-rs) on top of Nova, and we need a crate release for Arecibo to support the Lurk release. This note is meant to describe the main feature you can find in that crate: our implementation of[SuperNova](https://eprint.iacr.org/2022/1758) .


# Technical Release Note: SuperNova Protocol Integration into Nova


## Introduction


This release note highlights the recent integration of the SuperNova protocol into Arecibo’s cryptographic suite, marking a significant advancement in the field of cryptographic proofs, particularly in the area of folding schemes. SuperNova, by Abhiram Kothapalli and Srinath Setty, introduces a groundbreaking approach to proof statements in virtual machine modelization and customizable circuit operations, significantly reducing the computational costs and complexities traditionally associated with universal circuits in Incrementally Verifiable Computation (IVC).


Nova’s existing folding scheme, effective in handling repeated execution of a universal circuit, faces limitations due to costs proportional to the size of the universal circuit. SuperNova addresses this by allowing the breakdown of the universal circuit into smaller sub-circuits for each alternative instruction, thus reducing the complexity and cost of the folding operation. This integration is especially beneficial for virtual machine modeling and extends to cases where a core set of instructions is augmented with user-defined operations, as seen in languages like Lurk.


Initial testing of this integration within the Lurk proof language, particularly its coprocessor feature in the current Beta release, has shown promising results. The primary audience for this significant development includes developers and researchers in the field of cryptography, who stand to benefit from the enhanced efficiency and reduced costs in proof statement processing.


The overarching goal of integrating SuperNova into Nova is to establish the Non-Uniform Incremental Verifiable Computation (NIVC) paradigm as a standard in folding scheme implementations. By back-contributing this stable contribution to the Nova repository, we aim to incubate SuperNova’s functionalities, ensuring wider adoption and recognition in the cryptographic community


## Technical Overview


SuperNova enhances Nova’s folding schemes by introducing a more efficient approach to non-uniform Incremental Verifiable Computation (NIVC). It leverages cryptographic primitives that enable a prover and a verifier to fold two N-sized NP instances into a single N-sized instance. This folding is such that the resulting instance is only satisfiable if the original instances are satisfiable, and it specifically applies to a variant of the R1CS ( *Relaxed* Rank-1 Constraint Systems).


In each step, SuperNova’s prover folds an R1CS instance, representing the prior step of program execution, into a running instance with an associated witness. This instance, along with advice from the folding scheme, is then fed into an augmented circuit. This circuit comprises a verifier circuit and one of the kk


k


functions from the set F1,...,Fk{F_1, ..., F_k}


F


1


​


,


...


,


F


k


​


. The verifier circuit itself includes two key components: one for verifying the non-interactive folding scheme for R1CS, and another for computing a transition function ϕϕ


ϕ


.


SuperNova distinguishes itself by using multiple running instances, one for each instruction supported by the machine. The verifier circuit determines which running instance to fold into, guided by the transition function ϕϕ


ϕ


embedded within it. In its natural design, the size of the verifier circuit scales linearly with kk


k


. However, SuperNova employs offline memory checking techniques to render the verifier circuit size independent of kk


k


.


After NN


N


steps, the prover is left with kk


k


running instances and an R1CS instance representing the final step of the program execution. The prover can prove these by sending associated witnesses for verification. While the proof size is not dependent on the number of steps in the program execution, it is proportional to the sum of sizes of circuits for functions in F1,...,Fk,ϕ{F_1, ..., F_k, ϕ}


F


1


​


,


...


,


F


k


​


,


ϕ


1 . Although initially not zero-knowledge, this issue is resolved similarly to Nova. Compressed proofs and zero-knowledge are achieved by invoking a general-purpose zkSNARK, such as Spartan, making SuperNova a powerful addition to the Nova framework in handling complex cryptographic proofs.


## Contributions and Improvements


### Original protocol sketches by the community.


Cryptographic protocols take a village, and this integration of SuperNova is no different. Foundational contributions came from various community researchers:


The project was kicked-off with Wyatt Benno’s[foundational kick-off](https://github.com/microsoft/Nova/issues/146#issuecomment-1631883880) , laying down the groundwork for integrating the SuperNova protocol with Nova.


Later on, Sun Ming provided a[landmark PR](https://github.com/argumentcomputer/arecibo/pull/2) with an initial version of the complete folding argument, which kick-started the work incubating it in Arecibo.


Srinath Setty’s improvement in[PR 250](https://github.com/microsoft/Nova/pull/250) , while not directly related to SuperNova, were also incredibly helpful to us for implementing a compressing SNARK. The main feature was the refactor of the pre-processing variant of Spartan, replacing the “grand product argument” from[\[ Quarks §5 \]](https://eprint.iacr.org/2020/1275.pdf) with argument based on log-derivates popularized by[\[ Multivariate lookups based on logarithmic derivatives \]](https://eprint.iacr.org/2022/1530.pdf) .


---


> A note on how this new argument for the pre-processing SNARK works: If a prover claims that two multilinear polynomials P(X‾)P(\\underline X)
>
>
> P
>
>
> (
>
>
> X
>
>
> ​
>
>
> )
>
>
> and Q(X‾)Q(\\underline X)
>
>
> Q
>
>
> (
>
>
> X
>
>
> ​
>
>
> )
>
>
> are permutations of each other, they would produce a proof that the following products are equal, for a randomly sampled challenge rr
>
>
> r
>
>
> ∏x‾∈{0,1}k(P(x‾)+r)=∏x‾∈{0,1}k(Q(x‾)+r).\\prod_{\\underline x \\in \\{0, 1\\}^k} \\big( P(\\underline x) + r \\big) = \\prod_{\\underline x \\in \\{0, 1\\}^k} \\big( Q(\\underline x) + r \\big).
>
>
> x
>
>
> ​
>
>
> ∈
>
>
> {
>
>
> 0
>
>
> ,
>
>
> 1
>
>
> }
>
>
> k
>
>
> ∏
>
>
> ​
>
>
> (
>
>
> P
>
>
> (
>
>
> x
>
>
> ​
>
>
> )
>
>
> +
>
>
> r
>
>
> )
>
>
> =
>
>
> x
>
>
> ​
>
>
> ∈
>
>
> {
>
>
> 0
>
>
> ,
>
>
> 1
>
>
> }
>
>
> k
>
>
> ∏
>
>
> ​
>
>
> (
>
>
> Q
>
>
> (
>
>
> x
>
>
> ​
>
>
> )
>
>
> +
>
>
> r
>
>
> )
>
>
> .
>
>
> Taking the log-derivate on each side, the claim is equivalent to showing that the following sums are equal instead:
>
>
> ∑x‾∈{0,1}k1P(x‾)+r=∑x‾∈{0,1}k1Q(x‾)+r.\\sum_{\\underline x \\in \\{0, 1\\}^k} \\frac{1}{ P(\\underline x) + r } = \\sum_{\\underline x \\in \\{0, 1\\}^k} \\frac{1} {Q(\\underline x) + r }.
>
>
> x
>
>
> ​
>
>
> ∈
>
>
> {
>
>
> 0
>
>
> ,
>
>
> 1
>
>
> }
>
>
> k
>
>
> ∑
>
>
> ​
>
>
> P
>
>
> (
>
>
> x
>
>
> ​
>
>
> )
>
>
> +
>
>
> r
>
>
> 1
>
>
> ​
>
>
> =
>
>
> x
>
>
> ​
>
>
> ∈
>
>
> {
>
>
> 0
>
>
> ,
>
>
> 1
>
>
> }
>
>
> k
>
>
> ∑
>
>
> ​
>
>
> Q
>
>
> (
>
>
> x
>
>
> ​
>
>
> )
>
>
> +
>
>
> r
>
>
> 1
>
>
> ​
>
>
> .
>
>
> Given that Spartan is based on Sumcheck, the second claim is much easier to work with. The key benefits of this new approach are:
>
>
> - Reducing proof size by 40 field elements and 4 commitments
> - Removing the need for a second Sumcheck instance by combining all claims into a single instance, saving ~ log⁡(N)\\log(N)
>
>
> lo g
>
>
> (
>
>
> N
>
>
> )
>
>
> hashes for the verifier
>
>
> While these improvements are impressive on their own, they are even more beneficial to SuperNova where the savings scale with the number of instances. Moreover, we were able to reuse the existing Sumcheck batching functionality to handle multiple instances rather seamlessly.


---


### Outline of our contributions:


With the amazing work from the original contributors, a lot of our work was already cut out for us.


After some light refactoring, we were able to reduce the number of variables and constraints in the recursive verification circuit, and to streamline the API for dealing with the “program counter” (also represented as ϕ\\phi


ϕ


in the literature).


We also resolved some correctness issues in the recursive SNARK verification, which is primarily used in our test suite to validate the folding logic, rather than computing a full compressed SNARK.


While working on the batched pre-processing SNARK implementation, we discovered a bug in the original protocol that would have allowed a malicious prover to craft a valid proof for arbitrary public inputs. The vulnerability is described in[this note](https://hackmd.io/ZAVNUKOeSqKbIx8NK-8krQ) and was disclosed to Setty who promptly addressed and fixed it in[Nova PR 274](https://github.com/microsoft/Nova/pull/274) . This fix unfortunately would not have scaled to the batched setting and would have resulted in a large efficiency loss. We therefore opted to implement a more efficient counter-measure that applies to both the direct and batched setting, ensuring uniformity between both variants (called *Bounded Witness Sumcheck* in our note).


Finally, the bigger chunk of our contribution was to implement a final SNARK that made sense for folded SuperNova proofs.


## Compressing SuperNova proofs


The current implementation of Nova uses a modified version of the Spartan SNARK protocol as a final compressing SNARK for relaxed R1CS instances. In the move to SuperNova, we implemented a batched version of Spartan that can prove the satisfiability of n≥1n \\geq 1


n


≥


1


relaxed R1CS instances in a single, efficiently verifiable SNARK proof.


A naive solution to compressing SuperNova proofs would be for the prover to provide a SNARK proof for each individual circuit in the SuperNova NIVC instance. In the case of SuperNova implemented on a cycle of curves, there will be an additional SNARK proof for the secondary circuit as well. In total this implies that the prover and verifier costs would both scale linearly in the number of circuits.


As currently implemented in compressed SNARK feature[pull request](https://github.com/argumentcomputer/arecibo/pull/131) , we implement an alternative construction wherein the multiple Spartan proofs can be batched and proved/verified together. This reduces the cost of the verifier to the complexity of verifying a single (or two, in the case of a cycle of curves) Spartan proofs.


We will not go into the specifics of the Spartan or the Sumcheck protocols. For that we refer the reader to the fantastic resources linked below. Instead we aim to give a rough idea of the implementation---focusing on some of the subtleties related to batching proofs for circuits of different sizes.


For full details, see[SuperNova CompressedSNARK description](https://hackmd.io/@adr1anh/BJw1g0aBT) .


### Batching Sumchecks and Spartan


Given two (we will restrict to the case of n=2n = 2


n


=


2


Sumcheck instances, but the arguments are readily generalizable) multivariate polynomials P1(x1,…,xk)P_1(x_1, \\ldots, x_k)


P


1


​


(


x


1


​


,


…


,


x


k


​


)


and P2(x1,…,xk)P_2(x_1, \\ldots, x_k)


P


2


​


(


x


1


​


,


…


,


x


k


​


)


together with two claims of the form


Hi=∑x‾∈{0,1}kPi(x‾)H_i = \\sum_{\\underline x \\in \\{0, 1\\}^k} P_i(\\underline x)


H


i


​


=


x


​


∈


{


0


,


1


}


k


∑


​


P


i


​


(


x


​


)


there is a well-known technique to batch the two Sumcheck proofs into one. First the verifier provides one extra field element rr


r


as a challenge to the prover (or in the non-interactive model, the prover queries the random oracle for rr


r


) and the prover then proceeds with a single Sumcheck for a claim of the form


H1+rH2=∑x‾∈{0,1}kP1(x‾)+rP2(x‾).H_1 + r H_2 = \\sum_{\\underline x \\in \\{0, 1\\}^k} P_1(\\underline x) + r P_2(\\underline x).


H


1


​


+


r


H


2


​


=


x


​


∈


{


0


,


1


}


k


∑


​


P


1


​


(


x


​


)


+


r


P


2


​


(


x


​


)


.


Note here we are using the notation x‾\\underline x


x


​


as shorthand for the list of inputs x‾=(x1,x2,…,xk)\\underline x = (x_1, x_2, \\ldots, x_k)


x


​


=


(


x


1


​


,


x


2


​


,


…


,


x


k


​


)


.


The Spartan protocol, naively understood as being built out of two Sumchecks, can similarly be batched. Consider two relaxed R1CS shapes (Ai,Bi,Ci)(A_i, B_i, C_i)


(


A


i


​


,


B


i


​


,


C


i


​


)


, instances Ui=(W‾i,E‾i,ui,xi)\\mathbb U_i = (\\overline W_i, \\overline E_i, u_i, \\mathsf x_i)


U


i


​


=


(


W


i


​


,


E


i


​


,


u


i


​


,


x


i


​


)


, and witnesses Wi=(Wi,Ei)\\mathbb W_i = (W_i, E_i)


W


i


​


=


(


W


i


​


,


E


i


​


)


( i=1,2i = 1, 2


i


=


1


,


2


). Suppose AiA_i


A


i


​


are square s×ss \\times s


s


×


s


matrices with s=2ks = 2 ^ k


s


=


2


k


. Let Zi=(Wi,ui,xi)Z_i = (W_i, u_i, \\mathsf x_i)


Z


i


​


=


(


W


i


​


,


u


i


​


,


x


i


​


)


be the satisfying relaxed R1CS assignment. Denote A~i\\widetilde A_i


A


i


​


, W~i\\widetilde W_i


W


i


​


, Z~i\\widetilde Z_i


Z


i


​


, etc… the multilinear polynomial extensions of the matrices and vectors above.


Recall that the outer Sumcheck of the Spartan protocol involves an additional multilinear extension polynomial eq~(r‾,x‾)\\widetilde{\\mathrm{eq}}(\\underline r, \\underline x)


eq


​


(


r


​


,


x


​


)


which is partially evaluated at a random challenge τ‾=(τ1,τ2,…,τk)\\underline \\tau = (\\tau_1, \\tau_2, \\ldots, \\tau_k)


τ


​


=


(


τ


1


​


,


τ


2


​


,


…


,


τ


k


​


)


provided by the verifier (in practice turning a polynomial zero-check into a polynomial Sumcheck).


Proceeding with the above notation, the rough idea of the batched Spartan work is to take the two outer Sumcheck claims


0=∑x‾∈{0,1}keq~(τ‾,x‾)\[(∑y‾∈{0,1}sA~i(x‾,y‾)Z~i(y‾))(∑y‾∈{0,1}sB~i(x‾,y‾)Z~i(y‾))−(ui⋅∑y‾∈{0,1}sC~i(x‾,y‾)Z~i(y‾)+E~i(x‾))\]0 = \\sum_{\\underline x \\in \\{0, 1\\}^k} \\widetilde{\\mathrm{eq}}(\\underline \\tau, \\underline x) \\left\[\\left(\\sum_{\\underline y \\in \\{0, 1\\}^s} \\widetilde A_i(\\underline x, \\underline y) \\widetilde Z_i(\\underline y)\\right)\\left(\\sum_{\\underline y \\in \\{0, 1\\}^s} \\widetilde B_i(\\underline x, \\underline y) \\widetilde Z_i(\\underline y)\\right) - \\left(u_i \\cdot \\sum_{\\underline y \\in \\{0, 1\\}^s} \\widetilde C_i(\\underline x, \\underline y) \\widetilde Z_i(\\underline y) + \\widetilde E_i(\\underline x)\\right)\\right\]


0


=


x


​


∈


{


0


,


1


}


k


∑


​


eq


​


(


τ


​


,


x


​


)


​


​


y


​


∈


{


0


,


1


}


s


∑


​


A


i


​


(


x


​


,


y


​


)


Z


i


​


(


y


​


)


​


​


y


​


∈


{


0


,


1


}


s


∑


​


B


i


​


(


x


​


,


y


​


)


Z


i


​


(


y


​


)


​


−


​


u


i


​


⋅


y


​


∈


{


0


,


1


}


s


∑


​


C


i


​


(


x


​


,


y


​


)


Z


i


​


(


y


​


)


+


E


i


​


(


x


​


)


​


​


and batch them into a single claim of the form


0=∑x‾∈{0,1}keq~(τ‾,x‾)(G1(x‾)+rG2(x‾))0 = \\sum_{\\underline x \\in \\{0, 1\\}^k} \\widetilde{\\mathrm{eq}}(\\underline \\tau, \\underline x) \\left(G_1(\\underline x) + r G_2(\\underline x)\\right)


0


=


x


​


∈


{


0


,


1


}


k


∑


​


eq


​


(


τ


​


,


x


​


)


(


G


1


​


(


x


​


)


+


r


G


2


​


(


x


​


)


)


where we take GiG_i


G


i


​


to be the inner expression of the above summands. This reduces the prover’s claim of the above sum to an evaluation of the GiG_i


G


i


​


at a random challenge r‾x=(r1,x,r2,x,…,rk,x)\\underline r_x = (r_{1,x}, r_{2, x}, \\ldots, r_{k, x})


r


​


x


​


=


(


r


1


,


x


​


,


r


2


,


x


​


,


…


,


r


k


,


x


​


)


. These claimed evaluations are of the form


∑y‾∈{0,1}kA~(r‾x,y‾)Z~(y‾)\\sum_{\\underline y \\in \\{0, 1\\}^k} \\widetilde A(\\underline r_x, \\underline y) \\widetilde Z(\\underline y)


y


​


∈


{


0


,


1


}


k


∑


​


A


(


r


​


x


​


,


y


​


)


Z


(


y


​


)


together with a similar expression with B~\\widetilde B


B


and C~\\widetilde C


C


. They and can be proven with another collection of Sumcheck proofs. The evaluations of E~i(r‾x)\\widetilde E_i(\\underline r_x)


E


i


​


(


r


​


x


​


)


will be handled at the end in a batched opening to the polynomial commitments.


In practice, the current implementation of Spartan for a single relaxed R1CS instance already batches these “inner” Sumchecks. The batched Spartan protocol simply has a larger set of Sumcheck claims to batch.


A subtlety not yet mentioned has to do with the case when the polynomials entering the Sumcheck claims have a different number of variables. This will happen in practice when batching Spartan proofs corresponding to R1CS shapes with numbers of constraints that differ by a multiplicative factor of 2 or more. Suppose for example P1P_1


P


1


​


is a polynomial in k1k_1


k


1


​


variables, and P2P_2


P


2


​


is a polynomial in k2k_2


k


2


​


variables. To account for this difference, we simply consider both P1′P'_1


P


1


′


​


and P2′P'_2


P


2


′


​


as polynomials in n=max{k1,k2}n = \\mathrm{max}\\{k_1, k_2\\}


n


=


max


{


k


1


​


,


k


2


​


}


variables by introducing new free variables to the original polynomials.


With this perspective, the initial claim HiH_i


H


i


​


is scaled by 2n−ki2^{n-k_i}


2


n


−


k


i


​


to account for the summation over the new free variables, since each one doubles the number of occurrences the terms in the original sum.


∑x‾∈{0,1}nPi′(x‾)=∑y‾∈{0,1}n−ki∑x‾∈{0,1}kiPi′(y‾,x‾)=∑y‾∈{0,1}n−ki∑x‾∈{0,1}kiPi(x‾)=2n−ki⋅Hi. \\sum_{\\underline x \\in \\{0, 1\\}^n} P'_i(\\underline x) = \\sum_{\\underline y \\in \\{0, 1\\}^{n-k_i}} \\sum_{\\underline x \\in \\{0, 1\\}^{k_i}} P'_i(\\underline y,\\underline x) = \\sum_{\\underline y \\in \\{0, 1\\}^{n-k_i}} \\sum_{\\underline x \\in \\{0, 1\\}^{k_i}} P_i(\\underline x) = 2^{n-k_i} \\cdot H_i.


x


​


∈


{


0


,


1


}


n


∑


​


P


i


′


​


(


x


​


)


=


y


​


∈


{


0


,


1


}


n


−


k


i


​


∑


​


x


​


∈


{


0


,


1


}


k


i


​


∑


​


P


i


′


​


(


y


​


,


x


​


)


=


y


​


∈


{


0


,


1


}


n


−


k


i


​


∑


​


x


​


∈


{


0


,


1


}


k


i


​


∑


​


P


i


​


(


x


​


)


=


2


n


−


k


i


​


⋅


H


i


​


.


As an optimization, we can avoid any unnecessary computation due to the extra free variables using the following observation. Let Si(j)(Xj)S^{(j)}_i(X_j)


S


i


(


j


)


​


(


X


j


​


)


denote the univariate polynomial computed by the Sumcheck prover in round 0≤j<n0 \\leq j < n


0


≤


j


<


n


for the ii


i


-th claim. In the first n−kin - k_i


n


−


k


i


​


rounds of the protocol, this polynomial is constant and equal to 2n−ki−j−1⋅Hi2^{n-k_i-j-1}\\cdot H_i


2


n


−


k


i


​


−


j


−


1


⋅


H


i


​


. At this point there are kik_i


k


i


​


rounds remaining, we will have recovered the original claim since Si(n−ki−1)(rn−ki−1)=HiS^{(n-k_i-1)}_i(r_{n-k_i-1}) = H_i


S


i


(


n


−


k


i


​


−


1


)


​


(


r


n


−


k


i


​


−


1


​


)


=


H


i


​


.


This approach to Sumcheck batching avoids having to pad the polynomials in different sized claims thereby saving memory. Moreover, the optimization we describe above ensure the prover performs the same amount of work as it would the individual claims were proved using independent Sumcheck instances of the respective sizes.


### Implementation details


To summarize, our main contributions in this respect fall into three categories:


1.


We implement a new method` SumcheckProof::prove_cubic_with_additive_term_batch` as a combination of` prove_quad_batch` and` prove_cubic_with_additive_term` . The former is used to batch the inner Sumchecks of the already existing Spartan implementation and the latter is used for the outer Sumcheck.


2.


We introduce a new trait,` BatchedRelaxedR1CSSNARKTrait` , mirroring the already existing` RelaxedR1CSSNARKTrait` . The difference being that the batched version accepts a list of relaxed R1CS shapes, instances, and witnesses for the prover and verifier. We then implement this trait using the above batched Spartan protocol.


3.


We define a` supernova::CompressedSNARK` type that is generic in the above` BatchedRelaxedR1CSSNARKTrait` . When instantiated with the batched Spartan implementation it yields a final compressing SNARK for supernova proofs with an efficient verifier. Included are benchmarks and tests of correctness. In particular, there are tests which check the correctness for NIVC instances where the different number of constraints between the circuits will result in Sumchecks over polynomials with different numbers of variables.


## Organization of SuperNova contributions


A lot of the SuperNova contributions provide NIVC versions of already existing code meant for Nova IVC. In this section we provide a table to elucidate some of the structure of the SuperNova code by comparing it to the locations of already existing Nova implementations.


Nova SuperNova


` RecursiveSNARK`` lib.rs`` supernova/mod.rs`


` CompressedSNARK`` lib.rs`` supernova/snark.rs`


` StepCircuit`` traits/circuit.rs`` traits/circuit_supernova.rs`


Augmented Circuit` circuit.rs`` supernova/circuit.rs`


` (batched)RelaxedR1CSSNARKTrait`` traits/snark.rs`` traits/snark.rs`


Direct Spartan` spartan/snark.rs`` spartan/batched.rs`


Spartan with Spark preprocessing` spartan/ppsnark.rs`` spartan/batched_ppsnark.rs`


(batched) Sumcheck primitives` spartan/Sumcheck.rs`` spartan/Sumcheck.rs`


## Benchmarks


We have a few[preliminary benchmarks available for our SuperNova implementation](https://gist.github.com/huitseeker/0342a88f350e0345acf530b4091084cb) . While these benchmarks have a limited scope, they show a nice improvement in performance in favor of our approach to batched sumchecks compared to the standard Nova, on the pre-processing SNARK. Our integration with Lurk, on more sizeable circuits, should show results that are more reflective of the real world soon™.


## Closing Thoughts


In closing, the successful implementation of the Arecibo project truly exemplifies the adage that it takes a village to achieve greatness. Our heartfelt gratitude goes to all the contributors, including Srinath Setty, Abhiram Kothapalli, Sun Ming, Wyatt Benno, and others, whose collective expertise has been instrumental in this endeavor. The rapid iterations and collaborative efforts in the Arecibo repository have been key to the advancement of this implementation. It is now time for us to work on contributing this back to Nova, and work in the next stages of our project. Feel free to explore Arecibo, we would be greateful for your feedback!


## Resources


Prior references on Nova, SuperNova, the sumcheck protocol, and the LogUp sub-protocol used as a sub-part of the pre-processing SNARK.


- [Proofs, Arguments, and Zero-Knowledge](https://people.cs.georgetown.edu/jthaler/ProofsArgsAndZK.html) by Justin Thaler
- [SuperNova: Proving universal machine executions without universal circuits](https://eprint.iacr.org/2022/1758) by Abhiram Kothapalli, Srinath Setty
- [Spartan: Efficient and general-purpose zkSNARKs without trusted setup](https://eprint.iacr.org/2019/550) by Srinath Setty
- [The Spartan SumCheck-Trick](https://hackmd.io/@mprashker12/rJkM_JPQh) , Matthew Prashker
- [Multivariate lookups based on logarithmic derivatives](https://eprint.iacr.org/2022/1530) by Ulrich Haböck


## Footnotes


1.


There is a nuance: arguably our implementation of ϕ\\phi


ϕ


is the one implied by the paper, since that requires access to the whole witness. In that case, the cost of ϕ\\phi


ϕ


is actually the sum of k distinct ϕ\\phi


ϕ


circuits.↩
