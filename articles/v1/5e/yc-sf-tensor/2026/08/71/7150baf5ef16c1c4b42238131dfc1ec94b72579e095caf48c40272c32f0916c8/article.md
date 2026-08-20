---
schema_version: "1.0.0"
document_id: "7150baf5ef16c1c4b42238131dfc1ec94b72579e095caf48c40272c32f0916c8"
company_key: "yc-sf-tensor"
company: "SF Tensor"
source_id: "yc-sf-tensor-news-import-88ed66988698"
canonical_url: "https://sf-tensor.com/news/formal-correctness"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T08:45:53.460276+00:00"
fetched_at: "2026-08-05T08:45:55.721043+00:00"
content_hash: "sha256:804f88a87a7b4670880302ddbd7bab394129628c403670995f0e8b8c72a28ebc"
---

# Proving Kernels Correct Instead of Testing Them

A single FlashAttention tile has more distinct input assignments than every test suite ever written will cover. Therefore you really only have three options: (1) test based on a small suite of hand-selected tests, (2) perform probabilistic equivalence verification or (3) perform formal correctness / equivalence verification. Given that at times we may generate a million+ kernels in a single day at SF Tensor, manually writing a test suite for each base variant is not feasible, therefore for the longest time we've been limiting our optimizations to cases where we can be certain of equivalence (e.g., algebraically equal rewrites) or performing probabilistic verification.


Maybe it's because I'm old fashioned and like to be confident that what we are shipping is correct, but I've always felt uneasy with probabilistic verification, especially as we rolled out agentic optimization for certain kernels and hardware vendors, therefore we've spent a lot of time thinking about and working on a system to formally prove the correctness of kernels before we release them into production (and now, as a tool that we use to fight back against agents' reward hacking and to help them determine why things don't work when they make mistakes).


Just to further demonstrate the scale of testing that would be required to verify the full search space of just a small FlashAttention tile with 20 scalar inputs: 4 query values, 8 key values and 8 value values. In BF16 that's 23202^{320}


2


320


possible input assignments (technically you can reduce that amount by around 1%, but at this scale that's immaterial). At FP32 it's 26402^{640}


2


640


. In FP64 it's 212802^{1280}


2


1280


. No test harness can sample a meaningful fraction of that space, but the bugs that hide in it are exactly the ones testing is worst at finding: a missing rescaling term, a wrong denominator update, a broken causal mask, a synchronization bug that only occurs when` threadIdx % 64 == 0` (yes, we've had that one, no it was not fun). Each is correct on almost every input and wrong on a thin slice you need to hope your test harness will find. The biggest problem here is that the dangerous bugs are input-dependent: a causal mask can be correct for every position except one boundary, a denominator can be correct on every input except the ones that trigger a specific rescaling path. With random sampling, we have to land on that slice to see the bug. We try to increase the probability of that through probabilistic sampling and verification and some other techniques, but it's still just random sampling. Third, concurrency adds interleavings a single test run never explores: a missing barrier produces the right answer most of the time and the wrong answer when the schedule happens to race.


With formal correctness, we don't sample the space at all, instead we create a mathematical model and then formally prove that no input exists that makes the optimized kernel disagree with the reference. The great thing is that if there are such inputs, the system hands back a counterexample (i.e., the exact input that breaks the kernel).


## Proof Over Every Input


The first thing we had to do was determine at what layer our proof engine should operate. Our first instinct was to grab the hardware ISA, because it doesn't care what's going on further upstream and we just validate kernels down there, no matter if it came from Triton, CUDA, manual PTX or our own compiler. At the same time we were working on this though, we were digging a lot deeper into ptxas and its internals and realized that in (most) cases, mapping PTX to SASS is a static 1:n mapping, so we would gain no additional semantic understanding by going deeper, while losing a lot of documentation around understanding the instructions, therefore we decided to target PTX on the NVIDIA side and AMDGPU dialect for AMD.


PTX is the frontend, but the proof itself doesn't operate on a PTX-like AST directly. We lower it into a validated shared IR first, the same IR our[CPU interpreter](https://sf-tensor.com/news/warpsim) runs and the engine works on that. So the flow is: you give it validated shared IR produced from PTX along with a proof contract, we symbolically execute the program to produce a symbolic mathematical model of what is being computed and then ask Z3 whether any input, lane, CTA or legal synchronization case could violate the contract. It either proves the contract holds or returns a counterexample.


The way that this is actually done is by asserting inequality, that is we claim that` kernel != specification` and then ask the proof solver to prove this for us. If it returns UNSAT (unsatisfiable) that means that there is no way for those two to not be equal from which we can derive that they must be equal and it returns us a so-called proof certificate (which is basically the proof showing the steps to prove there is no way for them to be unequal). If on the other hand it is able to find a way to prove that they are unequal, that is find a SAT (satisfiable) result for the inequality, then we have a counterexample showing where they diverge.


Because we are using symbolics and not numeric inputs, the proof is over every input, not some sampled set of inputs, which is the whole point: a passing test in random sampling says "correct on the inputs we tried", whereas a proof says "correct on all possible inputs and here's the proof to show why".


The thing that keeps this from exploding is that we don't unroll every lane or CTA. The engine proves a representative observed output and then separately checks safety using symbolic lanes and blocks, plus, for cross-CTA interference, a second arbitrary alpha-renamed CTA. That's the whole reason the million-lane and million-CTA style tests stay tiny instead of blowing up with the launch geometry. For example, a million-lane shared-memory shift proof comes back in 0.08s, because we never enumerated a million of anything, we reasoned about an arbitrary one.


Toy equivalence checker


Assert that the candidate disagrees with the reference, then ask whether any input exists.


UNSAT


reference


scale = 2


out = n / scale


candidate


k = 0.5


out = n * k


Candidate multiplier


solver query


exists n:


n / 2 != n * 0.5


result = UNSAT


Proven equivalent


No assignment to n can make division by 2 disagree with multiplication by 0.5.


When the candidate multiplier is 0.5, the negated equivalence query is unsatisfiable.


Any other finite value leaves a concrete input where division by 2 and multiplication by k diverge.


A tiny version of the engine loop: encode both programs, assert disagreement, then interpret SAT as a counterexample and UNSAT as a proof.


Before getting to FlashAttention, it's worth being concrete about what the engine actually eats.


It consumes validated shared IR, which means anything we can lower into that IR can be proven, not just a handpicked instruction set. Right now we focus on PTX, but we are extending it to AMDGPU and other silicon.


For anything with real launch geometry, there's a topology-aware kernel proof that understands 3D grids, CTA footprints, clusters, barriers and atomics and a separate synchronization-only verifier for warp sync, votes, shuffles, mbarriers, async copy, cluster barriers and grid barriers, which is the part that lets us reason about collective kernels at all.


## The FlashAttention Proof


A good example of what we can prove today is that an optimized FlashAttention-style kernel is formally equivalent to a naive attention specification and we've pushed this all the way up to a full` d=128` head dimension. The scaled collective proof runs over the entire possible input space and we ask whether any input exists that makes the optimized kernel disagree with the model.


**Query/key/value staging through shared memory.** The kernel copies Q, K and V entries into modeled shared arrays (with explicit allocation lengths and ownership), then later reads from those shared arrays instead of global memory. The verifier tracks the values through the staging so a read after a barrier resolves to the value that was actually written.


**Lane-partial QK score reduction.** Each lane computes one partial term of the query-key dot product, publishes it through shared memory, synchronizes and then lane 0 reduces the published partials back into the full score. The proof has to show the reconstructed score equals the single dot product in the spec, regardless of how the work was split across lanes. We also run this as a stress proof over 4,096 query/key pairs at d=128, which is 1,048,576 scalar Q/K values if you were to enumerate it and it proves in about 0.91s, because the proof is symbolic and parametric rather than one test per pair.


**Synchronization barriers.** Barriers are represented explicitly, modeled as proof phase boundaries. This was one of the most important things we did, because without it any equivalence is useless, but with it if you remove a barrier the proof can no longer show the optimized kernel matches the generated recurrence and the kernel is rejected, ensuring that a missing barrier results in a counterexample.


**Online softmax recurrence.** The kernel maintains a running max, rescales the existing denominator and accumulator when the max changes, adds the new exponentiated term and finally divides the accumulator by the denominator.


**Causal mask variant.** The proof uses a masked naive attention spec where query` q` may only attend to keys` k <= q` . If you run the same optimized kernel against the all-keys spec then the causal proof is rejected, which is the behavior we want: a causal kernel should not match a non-causal reference.


One of the first sub-results we slowed down on is the online softmax one, because it's non-trivial for the system to prove that online softmax is equivalent to recomputed softmax.


To solve this, the verifier proves the invariant that the online denominator equals the stable recomputed denominator at the current max, then proves the final output matches recomputed softmax. The recurrence isn't approximately the same as recompute-everything, but it's calculating the exact same thing and the engine proves so through the rules of exponent and the loop invariant instead of simply asserting it.


Online softmax invariant


Before x_t


m_old = max(x_1 ... x_t-1)


d_old = Σ_i<t exp(x_i - m_old)


a_old = Σ_i<t exp(x_i - m_old) * v_i


After adding x_t


m_new = max(m_old, x_t)


d_new = d_old * exp(m_old - m_new)


+ exp(x_t - m_new)


a_new = a_old * exp(m_old - m_new)


+ exp(x_t - m_new) * v_t


o_new = a_new / d_new


The proof keeps the denominator and accumulator in the same max frame as recomputed softmax at every step.


## The Counterexample


Especially as we roll-out our agentic optimization, the counterexamples are half the value. Because when a kernel is wrong, the engine doesn't simply say "wrong", instead it produces specific inputs on which the optimized kernel diverges from the reference, which is relevant both for humans optimizing kernsl at 2am, as well as agents optimizing kernels or for, say, post-training a model on kernel optimization.


Say we take a working single-row online softmax kernel and introduce a simple bug: we omit the rescaling of the existing denominator when the running max changes. This is the single most common online-softmax mistake and it can pass casual testing because it's correct in cases where the max happens not to move.


The engine rejects it and hands back the input that breaks it:


```text
text  softmax_single_row at index 3:
kernel c[i] = 17/2
spec   c[i] = 10
input a[i] = 3/5
input b[i] = 0
input c[i] = 16
input d[i] = 0


```


So the optimized output comes out at 8.5 against a reference of 10.0, a diff of -1.5, on a completely reproducible input. We can then take that specific counterexample model (the actual values) and hand it to the person or agent optimizing the kernel to help them fix it.


Break the kernel


Flip off the rescale term and the online softmax kernel stops matching recomputed softmax.


UNSAT


online softmax recurrence


m_new = max(m_old, x_i)


den = den * exp(m_old - m_new) + exp(x_i - m_new)


acc = acc * exp(m_old - m_new) + exp(x_i - m_new) * v_i


out = acc / den


solver query


exists row, values:


online_with_rescale(row) != recomputed(row)


result = UNSAT


Rescale existing denominator


The term that moves old values into the new max frame.


Proven equivalent


The denominator stays in the same max frame as recomputed softmax at every step.


The correct recurrence rescales old denominator terms by exp(m_old - m_new) whenever the running max changes.


Removing that one term is enough for Z3 to produce a concrete input where the optimized kernel diverges.


The same SAT/UNSAT loop on a real bug: the proof accepts the recurrence with rescaling and rejects the version without it.


## Reals First, Bits Next


The most important thing to be clear about: we model floating-point values as exact reals, not IEEE-754 bit patterns, which means that the engine proves that two kernels are algebraically equivalent (i.e., that they implement the same mathematical function), but it does not prove they produce bit-identical floating-point output, because float addition isn't associative and reordering a reduction changes the last bits.


The reason for this is simple: the semantic equivalence of a kernel is where the most dangerous bugs actually live. A missing rescale, a wrong denominator, a broken mask, a missing barrier: all of these are algebraic and structural errors and the algebraic layer catches all of them. Given the fact that ~every optimization on floatings produces a result that is not bitwise-identical anyway given that floating-point math is not associative, the question is not really if the result will be different but instead how much different.


Once the kernels' semantics are formally proven, verifying that a new kernel stays within a given relative and absolute error is more so a tractable, probabilistic problem. Verifying that it is doing the right calculation in the first place is not and it is the part that matters most for preventing reward hacking and for trusting aggressive optimization. Once we first get the semantics right, we can then bound the error. For that, we currently use a fairly rigorous system which encompasses various techniques beyond just calculating absolute and relative error, such as entropy and output lattice spacing, to reason about the floating-point precision. At the end of the day though, it's not a formal floating-point model, which is why we are building a version with full IEEE-754 semantics: rounding, NaNs, infinities, signed zero and hardware-specific math behavior, so that we can then formally reason about the error between two kernels. We already have a first proof of concept and a post on reconstructing` tcgen05` behavior bit-for-bit ([link](https://sf-tensor.com/engineering/bitwise-tcgen05) ).


The next step is widening the model in the obvious directions: more PTX instructions, larger kernel shapes, finite floating-point semantics with tolerance or bit-exact contracts, but the important shift is already here, which is that correctness should become a proof obligation, not a sampling exercise.


If you find this type of work interesting and are looking for a job: if you build a z3 system that proves online softmax (which is doable in <100 LOC) and send it to me, I'll interview you for any of our[roles](https://sf-tensor.com/careers) .
