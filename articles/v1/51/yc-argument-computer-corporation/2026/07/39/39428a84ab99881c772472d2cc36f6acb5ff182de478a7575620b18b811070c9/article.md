---
schema_version: "1.0.0"
document_id: "39428a84ab99881c772472d2cc36f6acb5ff182de478a7575620b18b811070c9"
company_key: "yc-argument-computer-corporation"
company: "Argument Computer Corporation"
source_id: "yc-argument-computer-corporation-news-import-6d03b0bcc333"
canonical_url: "https://argument.xyz/blog/lookup-part-1/"
published_at: null
first_seen_at: "2026-07-21T07:37:57.416159+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:5b901441f3cdd528e3592917251129a56df605b7f4e6a386df76374b5852a1d4"
---

# Lookups in Lurk: Part 1

This is the first in a series of two blog posts where we discuss the challenges and benefits of introducing lookups to Lurk.


The aim of this first short note is to introduce the lookup argument that underlies the Lurk execution architecture. In the second note we will describe a soundness issue with a naive implementation of the protocol, and the proposed fix that we claim rectifies the issue.


## Introduction


The current iteration of Lurk (which will be open-sourced in the coming months) uses our prover[Sphinx](https://github.com/argumentcomputer/sphinx) which is a[friendly fork](https://github.blog/2022-04-25-the-friend-zone-friendly-forks-101/) of Succinct Labs’[SP1 Prover](https://github.com/succinctlabs/sp1) . Because of this, the core structure of the Lurk lookup argument mirrors the one used in SP1, and as such this note could be of use in understanding the structure of the SP1 lookup argument as well.


Throughout we will use F\\FF


F


to denote a ~32 bit field. In practice both Lurk and SP1 are defined over the BabyBear field, which is the prime field of characteristic p=15⋅227+1p = 15 \\cdot 2^{27} + 1


p


=


15


⋅


2


27


+


1


, but in principle any 32 bit field should suffice. For the purposes of the lookup argument, we will also need a field extension which is roughly 128 bits large. We denote this field K\\mathbb K


K


, and in practice we take the quartic extension of F\\FF


F


.


## Background


### AIR, FRI, STARKs


There are tons of good resources out there for learning about the inner workings of STARKs (in particular1 ), so we will largely take them as a black box with one particular function: Let M∈FT×wM \\in \\FF^{T \\times w}


M


∈


F


T


×


w


be a T×wT \\times w


T


×


w


array with entries in F\\FF


F


, and let P={P(x1,…,xw,y1,…,yw)}\\mathcal P = \\{P(x_1, \\ldots, x_w, y_1, \\ldots, y_w)\\}


P


=


{


P


(


x


1


​


,


…


,


x


w


​


,


y


1


​


,


…


,


y


w


​


)}


be a set of polynomials in 2w2w


2


w


variables. STARK protocols allow a prover to provide a succinctly verifiable proof that the array satisfies the polynomial constraints in the sense that ∀P∈P,P(m→t,m→t+1)=0\\forall P \\in \\mathcal P, P(\\overrightarrow m_t, \\overrightarrow m_{t + 1}) = 0


∀


P


∈


P


,


P


(


m


t


​


,


m


t


+


1


​


)


=


0


where m→t\\overrightarrow m_t


m


t


​


denotes the tt


t


th row of the array MM


M


.


In the context of a ZK VM like Lurk or SP1, we should think of the array MM


M


as the execution trace of the VM. In this case each of the TT


T


rows represent the state of the VM at time tt


t


, where the state is represented in terms of the ww


w


registers of the machine. This is not strictly the case for the SP1 VM, which is based on the RISC-V architecture with its 32 general purpose registers, but it provides a compelling mental model that is not far from the truth.


We should note that in the most general setting, the above polynomial constraints are not the only kinds of constraints that a prover can succinctly attest to. STARKs also support *boundary constraints* , and polynomial constraints over domains more general than adjacent rows.2


### Lookups


A limitation of the above protocol is that the computations verifiable by the STARK protocol are limited to a single trace. These limitations cause a number of issues:


- The length of the trace is limited by the 2-adicity of the field F\\FF


F


. In the case of the BabyBear field this limit is T≤227T \\leq 2^{27}


T


≤


2


27


. This limit can be reached in practice for large computations.
- One of the major computational bottlenecks for STARKs is the cost of committing to the trace. This has the asymptotics of O(Tlog⁡T)O(T \\log T)


O


(


T


lo g


T


)


, being dominated by an FFT and Merkle commitment to the result.


For the above reasons, and others, it is desirable to split the execution trace into multiple smaller tables. The fundamental roadblock to this splitting is the need for different traces to communicate values in a verifiable way. Luckily there are well-known protocols that can achieve this inter-table communication that go under the name of **lookup arguments** .


Opening up the possibility of splitting the execution trace also unlocks the ability to split the execution trace into smaller and more specialized traces. For example, specialized traces that deal specifically with unsigned integer arithmetic. Another particularly promising application is in the realm of memory-checking: having a read and writable memory which all the other tables can access.


#### Range Checking


We end this section with a classical and well-known application of lookups: that of range checking. Adding in a lookup argument on top of the STARK protocol, in addition to supporting inter-table communication and memory checking, also introduces the ability to perform range checks on table entries. In particular consider the following setup:


Table M1M_1


M


1


​


has a column c1c_1


c


1


​


which the prover claims represents 8-bit unsigned integers. If there is a separate pre-processed table BB


B


consisting of the values 0,1,…2550, 1, \\ldots 255


0


,


1


,


…


255


then an inter-table lookup argument between M1M_1


M


1


​


and BB


B


could prove the values c1c_1


c


1


​


are taken from the table BB


B


. This forms an efficient proof that all the values in c1c_1


c


1


​


fall in the range \[0,28−1\]\[0, 2^8 -1\]


\[


0


,


2


8


−


1


\]


without the need for additional bit variables and boolean constraints.


Doing this for a single column c1c_1


c


1


​


may be superfluous. However in the case of a RISC-V based VM like SP1 where all the registers and memory values mimic the operations of a UInt32 based RISC machine, the savings from using range checking far outweigh the additional constraints and complexity needed in incorporating lookup protocols.


## Some lookup protocols


In this section we will go through a few different strategies to provide lookup arguments. These will get progressively more efficient, until we end with the *logUp* argument introduced in3 . For simplicity, throughout we will be specialized to the case of two tables M1M_1


M


1


​


and M2M_2


M


2


​


, where M1M_1


M


1


​


is looking up the values from a single column of table M2M_2


M


2


​


.


In all of the examples we assume that the prover first populates the entries in the tables M1M_1


M


1


​


and M2M_2


M


2


​


, perhaps populating the looked up columns with values non-deterministically. It is then the job of the prover to provide a convincing proof that the values supplied are taken from table M2M_2


M


2


​


.


### Merkle proofs


The first lookup argument we introduce is perhaps the simplest. First the prover Merkle commits to the table M2M_2


M


2


​


, and for every component in the received column of values c1c_1


c


1


​


, the prover provides a Merkle inclusion proof for the inclusion in the sent column c2c_2


c


2


​


of M2M_2


M


2


​


.


The cost profile of this lookup argument is particularly bad: If the table sending values has length TT


T


, and there are NN


N


lookups then the prover has to send O(Nlog⁡T)O(N \\log T)


O


(


N


lo g


T


)


field elements, and the verifier has to perform an equivalent number of hashes.


Because of these poor performance characteristics, this lookup argument is not used in practice to transfer computed results between tables.


### Hash multiset proofs


The rest of the lookup arguments abstract away the requirements of a lookup argument into the correctness of a *multiset* implementation. Recall a multiset is a set together with a natural number multiplicity for each element of the multiset.


In essence if the prover and verifier both had access to an implementation of a multiset which the verifier can be confident behaves like an idealized multiset, then the prover can keep track of all of the received values of the lookup in a multiset. If this multiset of received values matches the multiset of sent values (also kept track of by the prover), then the verifier can be convinced that the lookup argument is valid.


The problem is now to use efficient cryptographic primitives to implement this idealized multiset. This perspective was taken in the memory checking paper4 in the context of memory checking, and is implemented using only an idealized hash function. This was extended in5 to allow concurrent memory access by leveraging a hash-to-curve operation to implement the` SetKV` primitive.


### Grand product


Another way to implement a multiset-based lookup argument is to add an additional round of interactivity between the prover and verifier, wherein the two use extra randomness to turn the multiset correctness into a polynomial relation checked at a random point. This is the perspective taken first in6 and later improved in a follow up series of works. We refer the reader to the survey article7 which has a more thorough treatment of the subject.


In essence, the prover begins by committing to the column in the lookup table M2M_2


M


2


​


(which we will assume is of the form C=\[c1,c2,…,cT\]C = \\left\[c_1, c_2, \\ldots, c_T\\right\]


C


=


\[


c


1


​


,


c


2


​


,


…


,


c


T


​


\]


), the looked up column c1=\[z1,…,zN\]c_1 = \\left\[z_1, \\ldots, z_N\\right\]


c


1


​


=


\[


z


1


​


,


…


,


z


N


​


\]


in M1M_1


M


1


​


, and the multiplicities of the lookups \[m1,…,mT\]\\left\[m_1, \\ldots, m_T\\right\]


\[


m


1


​


,


…


,


m


T


​


\]


. The verifier supplies an extra random element r∈Kr \\in \\mathbb K


r


∈


K


, and the two proceed in an interactive protocol to verify the identity


Z(r)=∏i=1N(r−zi)=∏i=1T(r−ci)mi=L(r).Z(r) = \\prod_{i = 1}^N (r - z_i) = \\prod_{i = 1}^T(r - c_i)^{m_i} = L(r).


Z


(


r


)


=


i


=


1


∏


N


​


(


r


−


z


i


​


)


=


i


=


1


∏


T


​


(


r


−


c


i


​


)


m


i


​


=


L


(


r


)


.


The polynomial on the left hand keeps track of the unordered list of lookups done in column c1c_1


c


1


​


, and the right hand polynomial keeps track of the whole lookup table with the associated lookup multiplicities.


By the Schwartz-Zippel lemma, these products match if the corresponding polynomials Z(X)Z(X)


Z


(


X


)


and L(X)L(X)


L


(


X


)


equal with overwhelmingly high probability (if K\\mathbb K


K


is taken large enough). This is the case (by the fundamental theorem of algebra) when the roots equal with the same multiplicities. In other words: If the lookups performed match the lookup table values with their associated committed multiplicities, then the lookup argument should convince the verifier as to the correctness of the associated values.


### logUp


The logUp paper3 (later improved in8 ) modifies the above lookup argument by transforming the so-called grand product check into an argument amenable to be checked via a sumcheck protocol.


This is done by taking the *logarithmic derivative* of the above expressions. The logarithmic derivative of a polynomial ff


f


is defined as


dlog(f)(x)=f′(x)f(x)\\mathrm{dlog}(f)(x) = \\frac{f'(x)}{f(x)}


dlog


(


f


)


(


x


)


=


f


(


x


)


f


′


(


x


)


​


where f′(x)f'(x)


f


′


(


x


)


is the usual derivative. It can be shown that the logarithmic derivative satisfies


dlog(f⋅g)(x)=dlog(f)(x)+dlog(g)(x).\\mathrm{dlog}(f \\cdot g)(x) = \\mathrm{dlog}(f)(x) + \\mathrm{dlog}(g)(x).


dlog


(


f


⋅


g


)


(


x


)


=


dlog


(


f


)


(


x


)


+


dlog


(


g


)


(


x


)


.


As long as the number of lookups is bounded above by the size of F\\FF


F


9 , then it follows that two polynomials with the same logarithmic derivative differ by a constant. This means that it suffices for a prover and verifier to check the equality of summations


∑i=1N1r−zi=∑i=1Nmir−ci\\sum_{i = 1}^N \\frac{1}{r - z_i} = \\sum_{i = 1}^N \\frac{m_i}{r - c_i}


i


=


1


∑


N


​


r


−


z


i


​


1


​


=


i


=


1


∑


N


​


r


−


c


i


​


m


i


​


​


where rr


r


is a similarly derived random value as in the grand product argument. Though the summands are not themselves polynomials, the paper3 describes a transformation of the above summation into one that is verifiable via a sumcheck protocol between the prover and verifier.


It is this underlying lookup argument that forms a foundation for the Lurk, Sphinx, and SP1 implementation. Though the final verification is not done via sumcheck, the verifier essentially checks the above equality as part of the protocol.


## Sphinx lookup argument


With the above preliminaries in place, we are now ready to describe the actual lookup argument used in Lurk (and Sphinx and SP1). Sphinx uses lookup arguments to allow tables to communicate values between each other. A basic atom of communication between tables is referred to as an` Interaction` , and these interactions are split into two basic forms:` send` s and` receive` s.


These dual forms of communication amount to a table either sending or receiving a linear combination of columns. It is not theoretically necessary that the values being sent or received be linear in the AIR trace columns, but to minimize the degree of the polynomial constraints being verified and support some batching optimizations in Plonky3 this restriction is in place.


As in the last section, for simplicity, let us focus on the example where table 1 has a single` receive` interaction and table 2 has a single` send` interaction (our mental model should be that M2M_2


M


2


​


is sending a column of values to be received by M1M_1


M


1


​


).


1. First the prover populates the tables for all the tables in the execution trace. Any of the columns being` receive` d are populated non-deterministically.
2. The prover commits to each of the tables and sends the roots of the Merkle commitments to the verifier. In the course of a STARK proof this is a necessary step, so there is no extra work involved in this step that would not have to happen otherwise.
3. The verifier sends extra randomness γ∈K\\gamma \\in \\mathbb K


γ


∈


K


and r∈Kr \\in \\mathbb K


r


∈


K


. In practice, in the non-interactive setting, the prover instantiates a random oracle with a concrete hash function, and generates the randomness by hashing the roots of the Merkle commitments.
4. The verifier augments the tables MiM_i


M


i


​


with new tables AiA_i


A


i


​


. These new tables, referred to as the *permutation trace* , have the same height as their associated tables MiM_i


M


i


​


. The width is dependent on the number of interactions in the main trace MiM_i


M


i


​


, and the values are elements of the extension field K\\mathbb K


K


. The columns of the permutation trace can be split up into two forms:


- The first set of columns calculate the reciprocals of the values of the interaction, as in the logUp summation. If a value is a` send` the numerator appears with a +1+1


+


1


, and if it is a` receive` it appears with a −1-1


−


1


. As an optimization, the inverse of two interaction summands can be batched together into a single degree three constraint. This is why it is necessary for the interaction expressions to have degree 1. This means there are ⌈m/2⌉\\lceil m / 2 \\rceil


⌈


m


/2


⌉


columns of this form.
- The last column represents the running total of the logUp accumulator. The constraints on the last column ensure that acci+1=acci+∑im±1r+q(zi;γ) \\mathrm{acc}_{i + 1} = \\mathrm{acc}_i + \\sum^m_i \\frac{\\pm 1}{r + q(z_i;\\gamma)}


acc


i


+


1


​


=


acc


i


​


+


i


∑


m


​


r


+


q


(


z


i


​


;


γ


)


±


1


​


where the *fingerprint* q(zi;γ)q(z_i;\\gamma)


q


(


z


i


​


;


γ


)


of the lookup value ziz_i


z


i


​


depends on the additional randomness γ\\gamma


γ


. (more on the fingerprint later)


5. The prover provides a STARK proof for each shard attesting that all the shard constraints (both on the main trace, and on the permutation trace) are satisfied. In addition to each of these STARK proofs, the prover also sends the final summation in each of the permutation trace accumulation columns.


If the global lookup argument is correct, then the total logUp accumulator should equal zero (the two summands in the logUp check are combined into a single sum with terms appearing with their appropriate sign).


The verifier’s function is two-fold:


1. First they verify each shard’s STARK proof. In addition to verifying that the main trace was calculated correctly, this also verifies the permutation column calculations. In particular, the verifier can be convinced that the final accumulated sum in each permutation trace has been calculated correctly.
2. Finally, to verify the lookup argument’s correctness the verifier simply calculates the sum of all the shard logUp accumulators.


### Memory checking


The above construction is a naive, but ultimately accurate depiction of the Lookup argument used to communicate between trace shards in Lurk. A slightly different approach is used for the memory checking algorithm. In fact the algorithm described in the original reference4 on memory checking forms a basis for the memory checking argument used.


In the case of SP1 where the CPU chip has a clock, it is possible to use the Blum et. al. construction to implement a read and write memory chip. In slightly more detail: in addition to the value stored in each cell, there is an additional *timestamp* that keeps track of the clock cycle of the last read or write.


When a cell is read by an execution shard (the value is` receive` d) the shard` send` s the same value and incremented timestamp. Similarly, when a value is written to memory (it is a` send` value for some execution chip) the value at the cell is first` receive` d with its timestamp, and then the new value is written with a new timestamp given by the current clock via` send` (together with a constraint that the` receive` d value is smaller than the clock).


The final verification of the memory integrity requires the verifier to scan the values in memory to ensure that the value contained in that cell is the last value accessed in the course of execution. This memory-checking algorithm effectively allows a prover to simulate mutable memory using an append-only data structure.


Because there is no natural way to define an incrementing clock cycle in Lurk execution, this exact memory checking algorithm is not used for the Lurk memory chip. Instead, Lurk uses the Poseidon hash function to content-address Lurk data on initial inputs and final outputs. For internally used intermediate data structures, a simple incrementing counter is used to address the value. The data and its *pointer* , its associated content address, are appended to the Lurk memory chip.


Though the specifics of the Lurk memory checking algorithm differ, its introduction here is relevant. In a follow-up blog post we will explore how a similar construction can be used to address a soundness issue in the underlying lookup argument.


## Footnotes


1.


U. Haböck, “A summary on the FRI low degree test,” 2022, 2022/1216. Available:[https://eprint.iacr.org/2022/1216](https://eprint.iacr.org/2022/1216)↩


2.


Plonky3 implements these boundary constraints, but restricts the polynomial constraints to be on adjacent rows, and, for purposes to be discussed later, the degrees of the polynomial constraints are at most 3 in Lurk.↩


3.


U. Haböck, “Multivariate lookups based on logarithmic derivatives,” 2022, 2022/1530. Available:[https://eprint.iacr.org/2022/1530](https://eprint.iacr.org/2022/1530)↩↩2↩3


4.


M. Blum, W. Evans, P. Gemmell, S. Kannan, and M. Naor, “Checking the correctness of memories,” in \[1991\] Proceedings 32nd Annual Symposium of Foundations of Computer Science, 1991, pp. 90–99. doi: 10.1109/SFCS.1991.185352.↩↩2


5.


S. Setty, S. Angel, T. Gupta, and J. Lee, “Proving the correct execution of concurrent services in zero-knowledge,” 2018, 2018/907. Available:[https://eprint.iacr.org/2018/907](https://eprint.iacr.org/2018/907)↩


6.


A. Gabizon and Z. J. Williamson, “plookup: A simplified polynomial protocol for lookup tables,” 2020, 2020/315. Available:[https://eprint.iacr.org/2020/315](https://eprint.iacr.org/2020/315)↩


7.


T. Solberg, ‘A Brief History of Lookup Arguments’.[https://github.com/ingonyama-zk/papers/blob/main/lookups.pdf](https://github.com/ingonyama-zk/papers/blob/main/lookups.pdf)↩


8.


S. Papini and U. Haböck, “Improving logarithmic derivative lookups using GKR,” 2023, 2023/1284. Available:[https://eprint.iacr.org/2023/1284](https://eprint.iacr.org/2023/1284)↩


9.


This will be a very important point in a little bit.↩
