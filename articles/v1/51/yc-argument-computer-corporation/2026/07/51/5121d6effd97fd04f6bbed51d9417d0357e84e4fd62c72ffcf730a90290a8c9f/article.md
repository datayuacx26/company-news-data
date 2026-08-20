---
schema_version: "1.0.0"
document_id: "5121d6effd97fd04f6bbed51d9417d0357e84e4fd62c72ffcf730a90290a8c9f"
company_key: "yc-argument-computer-corporation"
company: "Argument Computer Corporation"
source_id: "yc-argument-computer-corporation-news-import-6d03b0bcc333"
canonical_url: "https://argument.xyz/blog/lookup-part-2/"
published_at: null
first_seen_at: "2026-07-21T07:37:57.416159+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:1a0d55aaf24ae704f6b2f415bf7373b9e573df199ac1b08758f30dd46b17e6e2"
---

# Lookups in Lurk: Part 2

This is a continuation of the first blog post about the architecture of lookups in Lurk. In part 1 we discussed the use of the logUp lookup technique for different shards of the Lurk execution to communicate their computed values. We continue in this post by noting a soundness issue if one were to apply the logUp technique naively to Lurk and SP1.


## Overflowing multiplicities


As stated in[part 1](https://argument.xyz/blog/lookup-part-1) , the soundness of logUp is restricted to the case when the number of lookups is less than the order of the field ∣F∣\\left| \\FF \\right|


∣


F


∣


. For small fields like BabyBear, and reasonably-sized programs like the Tendermint light client example in SP1, this upper bound on the soundness is met in practice.


### The problem


The soundness of logUp being dependent on the number of lookups amounts to the issue of overflowing multiplicities. Recall that a verifier should be confident of a lookup argument provided by the prover if the total summed accumulator across all shards equals zero.


In the context of SP1’s VM, the main CPU chip checks the state transitions between every consecutive clock cycles. Depending on the current state of the registers, the table will dispatch an operation to be performed by a separate and specialized chip, such as a 32-bit arithmetic operation, or a call to one of the available precompiles. These operations are encoded as queries which contain both the input and result, which the prover then` send` s via the lookup argument to be` received` and constrained by the specialized chip. We refer the reader to[part 1](https://argument.xyz/blog/lookup-part-1) of this series on the details about` send` and` receive` . Briefly,` send` and` receive` can add or subtract the quantity.


mr+rlc(q;γ)\\frac{m}{r + \\rlc(q; \\gamma)}


r


+


rlc


(


q


;


γ


)


m


​


from the Memoset accumulator. Here mm


m


is the multiplicity of the query qq


q


, and rr


r


and γ\\gamma


γ


is randomness provided by the verifier (or a random oracle in the non-interactive setting).


Concretely, a query is always sent using a multiplicity equal to 1, though it is possible to receive it with an arbitrary multiplicity if it was sent multiple times. This is the case when the CPU reads an instruction from the fixed program chip, or when a byte operation is requested from a pre-processed table containing all possible input/output pairs.


Since the logUp accumulator counts the number of occurrences of a query using an element in F\\FF


F


, an issue emerges when we consider what happens when a query was sent exactly ∣F∣|\\FF|


∣


F


∣


times, since the contribution to the sum would be


∣F∣⋅1r−t=0 in F.\\left|\\FF\\right| \\cdot \\frac{1}{r - t} = 0 \\;\\; \\mathrm{in} \\;\\; \\FF.


∣


F


∣


⋅


r


−


t


1


​


=


0


in


F


.


A cheating prover would be able to avoid sending the value entirely, while still convincing the verifier that the logUp accumulator summed to zero.


This was an issue in previous versions of SP1’s VM where it would’ve theoretically been possible to return an incorrect result to the CPU chip and then run the same query in a loop until it would effectively be nullified and not need to be proved. This issue was solved by including the CPU’s clock as part of the query, thereby ensuring that queries occurring at different cycles would always be different. While it is still possible for the same query to be sent and received within the same cycle, it is easy to count the maximum possible number of interactions within a cycle and check that it is less than ∣F∣|\\FF|


∣


F


∣


.


The main drawback of this approach is that the specialized chips may have to prove the same operation multiple times since each query will be matched up with a separate` receive` . In essence, we are forced to always use multiplicity equal to 1. We refer the reader to the excellent presentation on shared randomness in distributed proving for more details1 .


### The solution


The fix for this overflow issue implemented in Lair (a field-oriented intermediary representation used by Lurk) is surprisingly simple, and the original inspiration again comes from the memory checking work of2 . An especially attractive feature of this fix is that it does not require any changes to the[Sphinx prover](https://argument.xyz/blog/sphinx-oss) as it is built on top of the` send` and` receive` primitives already available. Similar memory checking principles underlie the Nexus and Jolt ZKVM memory arguments34 .


Taking inspiration from the fix implemented in SP1, we add a requirement that whenever a query is inserted into the multi-set, the prover must prepend it with a unique *nonce* . The goal is to ensure that if the same query was inserted from another location in the execution trace, the elements added to the multi-set would be different, and therefore both have multiplicity equal to 1. As a side-effect, whenever the prover wants to remove an element from the set, they must non-deterministically supply the nonce used during insertion.


For simplicity, we can define the nonce as the index of the row within the trace where the insertion occurred, as this is both easy to constrain and to keep track of during proving. Note though that this may allow some queries to be duplicated in the set, and it can be extended with additional information such as the shard or trace index within a proof.


The Lair lookup argument builds upon the above abstraction and removes the restriction that queries may only be read once. The goal is to be able to prove queries once, and read them as many times as it is requested across the entire proof. The mechanism which enables this is a form of memory-checking argument, though simplified to the case where items from memory are written once but are immutable.


#### More details


We begin by introducing the following small wrapping functions that restrict the usage of the primitives to only allow singular multiplicity.


```text
fn   insert  (  &mut   acc, q  :   &  [  F  ]) {
// equivalent to
//   acc += 1 / (r + rlc(q, gamma))
send  (acc, q, multiplicity   =   1  )
}


fn   remove  (  &mut   acc, q  :   &  [  F  ]) {
// equivalent to
//   acc -= 1 / (r + rlc(q, gamma))
receive  (acc, q, multiplicity   =   1  )
}


```


*The comments above describe the update that is performed over the logUp accumulator, using the verifier’s random challenges` r` and` gamma` . The function` rlc(q, gamma)` evaluates the random linear combination of the query` q` with powers of` gamma` (this is the fingerprint from above).*


These wrappers establish an interface for dealing with abstract multiset, where elements (which take the form of queries) can only be inserted and removed one by one. This abstraction only allows us to prove that a query was copied from one position to another, since each inserted query must be removed exactly once to ensure the multi-set is empty across all traces in a proof. This in itself does not prevent a malicious prover from performing the same set operation ∣F∣|\\FF|


∣


F


∣


times, and it remains the responsibility of the caller to ensure that each query can only be included a bounded number of times.


More precisely, the following pseudocode implements the new` provide` and` require` methods described above:


```text
fn   provide  (  &mut   acc  :   F  , final_nonce  :   F  , final_count  :   F  , query  :   [  F  ; m]) {
// `receive` the last query with its nonce and counter
remove  (acc, [final_nonce, final_count]   ++   query);


// `send` the query back with nonce = counter = 0
insert  (acc, [  0  ,   0  ]   ++   query);
}


fn   require  (  &mut   acc  :   F  , prev_nonce  :   Option  <  F  >, prev_count  :   Option  <  F  >, query  :   [  F  ; m]) {
// increment the counter
let   new_count   =   prev_count  .  unwrap_or  (  0  )   +   1  ;
// Assert that the `new_count` is non-zero
assert_ne  (new_count,   0  );
// Generate a new nonce using the `row_id`, or return 0 on the first `require`
let   new_nonce   =   if   prev_nonce  .  is_some  () {   get_row_id  () }   else   {   0   };


// `receive` the previous query with its nonce and counter
remove  (acc, [prev_nonce  .  unwrap_or  (  0  ), prev_count  .  unwrap_or  (  0  )]   ++   values);


// `send` the query back with new nonce and counter
insert  (acc, [new_nonce, new_count]   ++   values);
}


```


A few notes are in order:


1. ` new_count` in` require` is **not** constrained to be` last_count` incremented. The only constraint on` new_count` is that it is non-zero. This constraint is implemented by adding an additional column of` new_count⁻¹` and constraint that` new_count * new_count⁻¹ = 1` . We mention this for when we eventually discuss the performance impact of these changes.
2. The value of` new_nonce` is determined by the` row_id` for the lookup query. In practice, this is implemented by adding an additional incrementing counter to the main execution trace with a constraint ensuring the` row_id` increments for adjacent rows. Eventually, using powers of the multiplicative generator of the 2-adic subgroup of F\\FF


F


as a row identifier could remove the cost for the prover committing to the` row_id` column.
3. Finally, the` final_nonce` ,` final_count` ,` prev_nonce` , and` prev_count` inputs are all kept track of by the prover. There are no guarantees that these are **actually** the corresponding quantities. The soundness guarantee does not assume the prover is honest though, so only if the prover behaves rationally they will be able to generate a verifiable lookup argument.


The way the prover and verifier use the above constructions is simple: In the original[Sphinx](https://argument.xyz/blog/sphinx-oss) lookup argument wherever one originally used a` require` or` send` , the claim is that replacing them with a` receive` or` provide` will yield a correct and sound lookup argument which avoids the issues of overflowing multiplicities.


#### Completeness and soundness


We now give informal arguments for the completeness and soundness of the above protocol. An analysis of the protocol can be performed on the level of the global logUp accumulator, or the multiset abstraction.


For completeness, a prover can simply follow the pseudocode and recommendations above for the lookup queries: Incrementing the counter and using the` row_id` as the nonce. As long as the same query is not` require` d more than ∣F∣\\left| \\FF \\right|


∣


F


∣


for the same` row_id` then the constraint that` new_counter ≠ 0` will not restrict the prover from generating a permutation trace satisfying the constraints. The state of the accumulator after all the require calls for a fixed lookup value zz


z


is given as:


∑iN(1r+rlc(noncei+1,counti+1,z;γ)−1r+rlc(noncei,counti,z;γ))\\sum_i^N \\left(\\frac{1}{r + \\rlc(nonce_{i + 1}, count_{i + 1}, z;\\gamma)} - \\frac{1}{r + \\rlc(nonce_{i}, count_{i}, z;\\gamma)}\\right)


i


∑


N


​


(


r


+


rlc


(


n


o


n


c


e


i


+


1


​


,


co


u


n


t


i


+


1


​


,


z


;


γ


)


1


​


−


r


+


rlc


(


n


o


n


c


e


i


​


,


co


u


n


t


i


​


,


z


;


γ


)


1


​


)


which is a telescoping sum simplifying to


1r+rlc(nonceN,countN,z;γ)−1r+rlc(0,0,z;γ).\\frac{1}{r + \\rlc(nonce_{N}, count_{N}, z;\\gamma)} - \\frac{1}{r + \\rlc(0, 0, z;\\gamma)}.


r


+


rlc


(


n


o


n


c


e


N


​


,


co


u


n


t


N


​


,


z


;


γ


)


1


​


−


r


+


rlc


(


0


,


0


,


z


;


γ


)


1


​


.


After all the` require` queries are filled in non-deterministically by the prover, they can generate the` provide` permutation traces by utilizing the` final_nonce` and` final_count` values (denoted nonceNnonce_N


n


o


n


c


e


N


​


and countNcount_N


co


u


n


t


N


​


above). The term added to the accumulator is exactly the inverse of the above telescoped value, and the final accumulator can be verified to equal zero.


Soundness follows by noting that if the final accumulator sum is zero (and that there were a nonzero number of lookups) then the first` require` would contribute a summand of the form


…−1r+rlc(0,0,z;γ).\\ldots -\\frac{1}{r + \\rlc(0, 0, z;\\gamma)}.


…


−


r


+


rlc


(


0


,


0


,


z


;


γ


)


1


​


.


Because a` require` call asserts that` new_count` be non-zero, the only way an honest prover can eliminate this term from the accumulator is by` provide` ing the appropriate query with its` final_count` and` final_nonce` .


Recall that the only constraint on` new_count` is that it be non-zero, so the prover may supply whatever nonzero value they want for` new_count` in a` require` call. A cheating prover may therefore try to overflow the accumulator by` require` ing the same query a large number of times. If we further assume that the number of traces and lookup queries per row are bounded (so that their product is less than ∣F∣\\left| \\FF \\right|


∣


F


∣


) then this term does not run the risk of overflowing.5


Another avenue a cheating prover can take is by` provide` ing a special query with the same fingerprint as one that was being` require` d (but not being equal to it). But an appeal to the Schwartz-Zippel bound similar to the argument found in6 ensures that it is exceedingly unlikely a cheating prover can cook up this cheating query.


#### Cost


In an ideal world, each lookup query could be handled by a single` send` or` receive` , but there is additional computational overhead by moving to the` provide` and` require` semantics. Though this extra cost is unfortunate, you cannot put a price on security.


Because` provide` requires a call to` send` and` receive` under the hood, there is a 2×2 \\times


2


×


overhead on the number of additional lookup columns. Just as for` provide` , a call to` require` also entails two columns for each call. There is also an additional column and constraint corresponding to the extra check that` new_counter ≠ 0` . Finally, there is an additional` row_id` column with additional constraint that is added to all of the traces participating in the lookup argument.


Because of the batching of constraints for two interactions, this means that every call to` provide` will add an additional permutation trace column and every` receive` has two. Overall this means there is roughly a 2×2 \\times


2


×


overhead for the new` provide` and` require` semantics. Despite this we have found this overhead to be manageable in practice. See the recent[performance benchmarks](https://argument.xyz/blog/perf-2024) to see the benefits.


## Try it out!


Try out the Lurk VM using the current implementation of[Lurk](https://github.com/argumentcomputer/lurk) . Also come join us on our public[Zulip](https://zulip.argument.xyz/) to chat with the team and ask any questions you may have. Let us know if you’d like to see any other explainers like this, or if you have any other Lurk content you’re interested in. Thanks for reading!


\\gdef\\FF{\\mathbb F} \\gdef\\rlc{\\mathrm{rlc}}


## Footnotes


1.


ZK11: Sharing Randomness in Distributed Proving - Tamir Hemo (Succinct) ([https://www.youtube.com/watch?v=S5HFiI0KspM](https://www.youtube.com/watch?v=S5HFiI0KspM) )↩


2.


M. Blum, W. Evans, P. Gemmell, S. Kannan, and M. Naor, “Checking the correctness of memories,” in \[1991\] Proceedings 32nd Annual Symposium of Foundations of Computer Science, 1991, pp. 90–99. doi: 10.1109/SFCS.1991.185352.↩


3.


Keynote: Memory checking in folding-based zkVMs - Jens Groth (Nexus) ([https://www.youtube.com/watch?v=xnzjC_9vUzs](https://www.youtube.com/watch?v=xnzjC_9vUzs) )↩


4.


A. Arun, S. Setty, and J. Thaler, “Jolt: SNARKs for Virtual Machines via Lookups,” 2023. Accessed: Aug. 11, 2023. \[Online\]. Available:[https://eprint.iacr.org/2023/1217](https://eprint.iacr.org/2023/1217)↩


5.


This is not an onerous requirement as the bound is far from being met in practice.↩


6.


U. Haböck, “Multivariate lookups based on logarithmic derivatives,” 2022, 2022/1530. Available:[https://eprint.iacr.org/2022/1530](https://eprint.iacr.org/2022/1530)↩
