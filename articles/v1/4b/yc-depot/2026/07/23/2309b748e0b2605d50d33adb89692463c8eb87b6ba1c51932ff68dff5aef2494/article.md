---
schema_version: "1.0.0"
document_id: "2309b748e0b2605d50d33adb89692463c8eb87b6ba1c51932ff68dff5aef2494"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/tla-verification"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:9f878809be4c4f442269af2fa5b20c80fb3db6a3c0cbb4aff70fa036b9a9b7b4"
---

# Improving system safety with Temporal Logic of Actions (TLA+)

The hardest distributed systems bugs to find aren't in any single operation. Two processes each do the right thing, but in an order nobody thought of, and data disappears. Tests don't catch these bugs because tests run the interleavings you imagined, while the bug is in the one you didn't.


At the scale of Depot Registry, this is no longer hypothetical. Once you reach a certain request volume, a one-in-a-million interleaving becomes a reality and happens on a schedule you don't control.


So when we rebuilt the garbage collector for[Depot Registry](https://depot.dev/blog/now-available-depot-registry-v2) , we model checked it with TLA+. The model checker found a real bug that our tests and reviews had missed. It also forced us to be precise about a design decision that sounds absurd on first read: our registry stores immutable, content-addressed blobs (that by definition never change), and it depends on S3 bucket versioning anyway.


## What TLA+ is


**TLA+** is a language for describing a system as states and transitions. **TLC** is a model checker that explores every reachable state and every possible interleaving, then tells you whether your invariants hold in all of them. You don't write the implementation in TLA+. You write a simplified model of it that's small enough for the checker to explore in full, but close enough to the real thing that a bug in the model points at a bug in your system.


Here's a trivial model: two clients withdrawing from a shared wallet, each doing a read-then-write without locking.


```text
---------------- MODULE Wallet ----------------
EXTENDS Integers


VARIABLES balance, read


Init == balance = 10 /\ read = [c \in {"a", "b"} |-> -1]


Check(c) == read[c] = -1
/\ read' = [read EXCEPT ![c] = balance]
/\ UNCHANGED balance


Withdraw(c) == read[c] >= 8
/\ balance' = balance - 8
/\ read' = [read EXCEPT ![c] = -2]


Next == \E c \in {"a", "b"}: Check(c) \/ Withdraw(c)


NoOverdraft == balance >= 0
================================================
```


Each definition is a transition:` Check(c)` reads the balance,` Withdraw(c)` subtracts 8 if the client sees enough money.` /\\` means "and," while` \\/` means "or." The primed variable` balance'` is the value in the next state, and` NoOverdraft` is the invariant we want to hold everywhere.


TLC breaks it in four steps: client` a` checks and sees 10, client` b` checks and sees 10, both withdraw, and the balance is -6. That's the classic check-then-act race, found mechanically, with a step-by-step trace showing exactly how to reproduce it. No test run was unlucky. The checker simply tried every ordering.


The wallet is a toy example. Here's the same idea in one of the real invariants from our registry GC model:


```text
ManifestNeedsData ==
\A p \in PusherIDs: manifestExists[p] => s3Versions /= {}
```


Read it from left to right:


- ` ManifestNeedsData` is the name, and` ==` means "is defined as."
- ` \\A` means "for all."
- ` p \\in PusherIDs` means` p` ranges over every pusher in the model.
- ` manifestExists\[p\]` asks whether pusher` p` has a committed manifest.
- ` =>` means "if the left side is true, the right side must be true."
- ` s3Versions /= {}` means the set of S3 versions is not empty.


Put together, the invariant is roughly this pseudocode:


```text
for each p in PusherIDs:
if manifestExists[p]:
assert s3Versions is not empty
```


The last clause might look too weak: it says *some* S3 version exists, not that the *right* blob exists. That's intentional. This model has a single blob digest, because the race we care about is whether GC can delete that blob while a manifest still needs it. In a multi-digest model, the invariant would need to be indexed by digest:` s3Versions\[manifestDigest\[p\]\] /= {}` . Reviewing a model means asking whether choices like this preserve the question you wanted to answer.


Model the moving parts (uploads, database transactions, GC workers), state what must always be true, and let the checker do to your design what production traffic eventually will.


## Why we can afford TLA+ now


TLA+ has been around for decades, and it has a reputation problem: everyone agrees it's powerful, but almost nobody budgets the weeks it takes to write and maintain a faithful model next to a moving implementation. That was our position too. Before this year, a spec for our GC would have lost the prioritization fight every single time.


What changed is that we don't write the model by hand anymore. An agent reads the implementation, the Go transactions and SQL and S3 calls, and translates it into a spec. We review the rest: do the invariants say what we mean, and does the model abstract away the right things? Writing TLA+ was the expensive part. Deciding what must always be true was always the cheap part, and it's the part that stays human.


The result is a spec of the registry's three-tier garbage collector that models concurrent pushers, two GC domains, a counter reconciler, and injected counter drift, all interleaved. It's rooted in the real implementation, transaction by transaction. TLC explores 14,290,224 distinct states in about 21 minutes and proves 10 safety invariants and 2 liveness properties. The most important invariant is the first one: a committed manifest never loses its blob data.


We now use AI to ship faster like everyone else. But we're also using it to build systems that are more correct than what we had the time to verify before.


## The race: why our immutable blobs use S3 versioning


OCI registries are content-addressable. Within Depot Registry, blobs live at` blobs/sha256/<digest>` , and the digest is the hash of the content. Upload the same blob twice and you get byte-identical data at the same key. Nothing ever changes in place. Under that model, S3 bucket versioning looks pointless: every version of an object would be identical.


But the problem is the deletes.


Garbage collection has to remove blobs that nothing references anymore. The GC worker marks a blob with zero references, waits out a grace period, re-verifies, and deletes. But the references live in MySQL and the bytes live in S3, and there is no transaction that spans both systems. Which opens a gap:


1. GC verifies the blob has zero references and decides to delete it.
2. Concurrently, a client pushes an image containing that exact blob. Same digest, same key. The upload writes to S3 and commits a new reference.
3. GC's delete lands and removes the object that a just-committed manifest now points to.


Every individual step is correct. Yet the interleaving deletes live data. And "a client re-pushes a blob right as it becomes garbage" is not exotic: it's what happens when a popular base image cycles out of use and back in.


You could try to fix this with locks or with ever-more-careful re-checking, but you can't re-check S3 and delete in one atomic step. So instead we made the delete itself precise. The bucket has **versioning enabled** : a re-upload of the same key becomes a new version instead of overwriting. When GC marks a blob, it records the specific S3 version ID it saw. When it deletes, it deletes only that version:


1. GC marks the blob and captures version` v1` .
2. The concurrent push writes the same bytes as version` v2` and commits its reference.
3. GC deletes` v1` , and only` v1` .` v2` , the version the new manifest was built on, is untouched.


We don't use versioning to keep history. Every version of a blob is byte-for-byte identical, so there's no history to keep. We use it as a delete fence: it turns "delete this key" into "delete exactly the bytes I inspected," which makes a delete safe to race against a write. That's the answer to the puzzle in the title of this section, and it's a good pattern for any content-addressable store that garbage collects: versioning, not immutable content, makes deletion safe.


## Turning a doubt into a line of TLA+


One concern in the registry design was how its reference counters behave under failure. To reduce contention on hot blobs, we use a lightweight saga: increment a reference counter, do the work, and compensate with a decrement on failure. A reconciler repairs counters when a crash prevents that compensation from completing. The important detail is that a wrong counter is not symmetric. Overcounting delays GC, while undercounting can make GC treat a referenced blob as garbage and delete live data.


That asymmetry was our doubt, so we told the agent to verify exactly that. It came back with an invariant:


```text
ManifestCountNeverUndercounts ==
blobActive => blobManifestCount >= TrueGlobalManifestCount
```


Whenever the blob's row is active, the *stored* counter must be at least the *true* count derived from the physical link rows. The` >=` , rather than` =` , captures the asymmetry directly: overcounts are tolerated, undercounts are a violation.


For that invariant to tell us anything useful, the model also has to include incorrect counters. We added a dedicated drift process that changes them in the same directions as failed compensations and historical desynchronization:


```text
process DriftInjector = "drift"
begin
Drift:
await driftBudget > 0;
either
await blobActive /\ blobLinkCount > 0;
blobLinkCount := blobLinkCount - 1;   \* undercount
or
await blobActive /\ blobLinkCount < N + DRIFT;
blobLinkCount := blobLinkCount + 1;   \* overcount
end either;
driftBudget := driftBudget - 1;
goto Drift;
end process;
```


This part of the spec is written in PlusCal, a front-end syntax that compiles down to TLA+, which is why it reads like pseudocode.` await` blocks the step until its condition holds, and` either/or` is nondeterministic choice: TLC explores both branches wherever this process could run, interleaved with the pushers, GC workers, and reconciler. Rather than representing one specific bug, the process represents the broader condition that a counter may be wrong in either direction when another operation runs. The checker can then verify that destructive steps recount the physical rows instead of relying on a stale counter.


The model also has invariants for its own internal bookkeeping:


```text
S3HeadOK ==
/\ (s3Versions = {}) <=> (s3Current = 0)
/\ (s3Versions /= {}) =>
/\ s3Current \in s3Versions
/\ s3Current = MaxVersion(s3Versions)
```


` S3HeadOK` says the "current version" pointer is empty exactly when the version set is empty, and otherwise points at the newest version. It does not express a product guarantee; it checks that the model's S3 abstraction remains internally consistent. These checks help distinguish a failure in the system being modeled from a mistake in the model itself.


Tests and model checking cover different ground. Tests exercise concrete implementations, while the model explores interleavings that would be difficult to reproduce deliberately.


## Tips and tricks on getting started yourself


Formal verification used to be a luxury reserved for teams with time to burn. That constraint is gone. The tedious part, faithfully translating an implementation into a spec, is now something you can delegate to an agent, while you make the judgment calls. Here's what we wish we knew six months ago.


**Pick your battles:** Don't throw TLA+ at everything. Good candidates are competing processes, tricky transactional boundaries, autonomous workers racing on timing, or work spanning multiple systems with no shared transactions. For example, a CRUD endpoint doesn't need a model checker.


**Have a bias for action:** While you're still learning TLA+, the goal is not a correctness proof. That comes later, if ever. When people hear "TLA+," the first objection is always "but what if the model doesn't match reality?" That's not the point: testing and formal verification both exist to increase trust, and a model checker is one more trust dial. So don't wait until you understand every line of a generated spec. Run it and see what falls out. Worst case you lose an afternoon; best case you've found a thread to pull. If the cost of a mistake is high, that's when you invest real time hunting for flaws in the spec itself.


**Steal this workflow:** Use cheaper models to generate sequence diagrams of the interleaving procedures, either at design time or from an existing implementation. Review them, simplify, and abstract away steps that don't matter. I like Codex for this exploration: it renders the diagrams nicely, and side-chats make it easy to pull on a topic without derailing the main thread. Once the diagrams say what you mean, hand them to a frontier model (Fable at extra high effort, in our case) to generate the TLA+ spec. That first spec will be a black box: you can't read TLA+ yet, so all you can judge is what goes in and what comes out. That's fine. Start there.


**Refine the workflow:** Now make the black box transparent, one piece at a time. The entry point is the invariants, because they're the readable part: short statements about what must always be true. There are usually 1–3 obvious ones. Use agents to propose more; then cut ruthlessly. After a few iterations the rest of the spec stops being opaque too: you start recognizing the transitions, then questioning them. Is this really how our retry behaves? Does the model even allow two workers here? Now you're reading the spec whitebox, finding problems in the model itself instead of just trusting its output.


**Turn doubts into invariants:** Describe what you're *not confident* about, since that's exactly the confidence a checker can buy you. For us it was an asymmetry in our GC counters — overcounting is always safe, undercounting never is — and the[previous section](https://depot.dev/blog/tla-verification#turning-a-doubt-into-a-line-of-tla) explained what the agent did with that doubt. Your doubts are the spec's best requirements.


**Treat findings as leads, not verdicts:** The model might not match reality, so a violated invariant is a starting point, a thread to keep pulling. Take the counterexample trace, turn it into a sequence diagram, and zoom in until you either fully understand the race and fix it, or find the mismatch between model and implementation. Either outcome is progress: you've gone from unknown unknown to something you can point at.


You now have another tool in your toolbox: tests check the interleavings you thought of; TLA+ explores the ones you didn’t.


## FAQ


What is TLA+ and what does the TLC model checker do?


TLA+ is a language for describing a system as states and transitions. TLC is the model checker that explores every reachable state and every possible interleaving, then tells you whether your invariants hold in all of them. You don't write the implementation in TLA+. You write a simplified model that's small enough for the checker to explore in full, but close enough to the real thing that a bug in the model points at a bug in your system.


Why does a content-addressable registry with immutable blobs need S3 bucket versioning?


Versioning isn't there to track changes to a blob, since the bytes never change in place. It's there to make deletion safe. Garbage collection has to remove unreferenced blobs, but references live in MySQL while the bytes live in S3, and no transaction spans both. That gap lets a client re-push a blob at the same moment GC decides to delete it. Versioning turns "delete this key" into "delete exactly the version I inspected," so GC removes the old version and leaves the newly pushed one untouched.


If an agent writes the TLA+ spec, how do I trust that the model matches my implementation?


You don't, not fully, and that's fine at the start. A model checker is one more trust dial, not a correctness proof. Begin with the invariants, since they're the readable part, and ask whether they say what you mean. Then work outward until you're reading the transitions whitebox and questioning them: does the model really allow two workers here, does this match how our retry behaves? If a mismatch would be expensive, that's when you invest real time hunting for flaws in the spec itself.


When is a system actually worth modeling in TLA+?


Use it when you have competing processes, tricky transactional boundaries, autonomous workers racing on timing, or work that spans multiple systems with no shared transaction. That's where the unique interleaving is, and it's exactly what tests miss because tests only run the orderings you thought of. A plain CRUD endpoint doesn't need a model checker.


## Related posts


- [Now available: OCI-compliant Depot Registry](https://depot.dev/blog/now-available-depot-registry-v2)
- [How simulations reduce the guesswork in our infrastructure decisions](https://depot.dev/blog/how-simulations-reduce-guesswork)
- [Scaling Depot: Solving a thundering herd problem](https://depot.dev/blog/planetscale-to-reduce-the-thundering-herd)


Wito Delnat


Staff Engineer at Depot
