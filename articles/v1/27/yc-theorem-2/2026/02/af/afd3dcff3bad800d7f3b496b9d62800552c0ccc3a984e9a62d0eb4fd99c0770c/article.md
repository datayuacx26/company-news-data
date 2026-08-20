---
schema_version: "1.0.0"
document_id: "afd3dcff3bad800d7f3b496b9d62800552c0ccc3a984e9a62d0eb4fd99c0770c"
company_key: "yc-theorem-2"
company: "Theorem"
source_id: "yc-theorem-2-rss-ef0b9cf422a4"
canonical_url: "https://theorem.dev/blog/lf-lean/"
published_at: "2026-02-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:37.219761+00:00"
fetched_at: "2026-07-28T22:21:35.013216+00:00"
content_hash: "sha256:a821e36b139f1b3ffa2b2ffdfe78e95b8c13457af4d8a6b1bdfc37613586e3b8"
---

# `lf-lean`: The frontier of verified software engineering

## Introduction


As AIs automate increasingly complex software tasks, a fundamental tension emerges: how do we know the code they produce is correct? The standard approach of reviewing AI-generated code and its tests doesn’t scale. Human review effort grows proportionally with code volume, while AI code generation capacity grows exponentially.1 1


Moreover, frontier software engineering typically results in multiplicative bug growth; many bugs are subtle and interaction-driven, and their count grows super-linearly with code length and complexity. SeeAppendix C .


If this trend continues, we’re headed toward a world where code deployment is bottlenecked on human review capacity, or just deployed without review.


This post introduces a different approach: *task-level specification generators* . Instead of reviewing each piece of generated code individually, we review a correctness specification once for an entire class of tasks, and the correctness specification covers all instances of the task across any codebase. The key insight is that for many important software transformations (translation, optimization, refactoring), we can define what “correct” means generically, without reference to any specific codebase. Moreover, as codebase complexity increases, a lot of feature implementation is a semantics-preserving transformation of existing features, so the correctness specification for the new feature is a simple corollary of the correctness specification for the existing features. Thus, building task-level specification generators helps scale oversight in two ways: (1) you can easily work on many more codebases simultaneously, and (2) what works on simple codebases extends to arbitrarily complex ones.


We prototype task-level specification generation with` rocq-dove` , building a scalable environment for verified translation2 2


A verified translation is a Lean translation and a proof of correctness.


from Rocq3 3


Formerly known as Coq.


to Lean. Given any Rocq source code,` rocq-dove` automatically generates a correctness specification (a theorem that any valid translation must satisfy) and grades model-generated translations and proofs against it. Because the specification is derived from the source code itself,` rocq-dove` scales to arbitrary codebases without additional human effort.


We use` rocq-dove` to generate an environment for a verified translation of all 1,276 statements4 4


Each statement is a logical proposition or a claim that can be proven to be true.


in *Logical Foundations* 5 5


*Logical Foundations* is an interactive textbook covering functional programming, type theory, and formal program reasoning.


\[1\]


. Using our scaffold, frontier AI models6 6


We use a mixture of frontier models as AI agents, given that they have somewhat orthogonal competencies at software verification.


autonomously produced verified translations of 97% of the statements.7 7


While all statements of *Logical Foundations* are likely in the training data, proving that a translation is correct is a different task than proving the original statement.


We solved 6 extreme-difficulty statements manually (~2 person-days), after which models completed the remaining 2.5%. For context, we estimate the full task would have taken 3 person-months for translation plus 2.5 person-years for verification if done entirely by humans.8 8


Translation estimates are based on industry SWE productivity data; verification estimates use the historical median of ~2,558 verified LoC/person-year from landmark projects. SeeAppendix B for details.


Two key world-view updates:


1.


**Verified software engineering can scale without marginal human oversight.**` rocq-dove` automatically generated correctness specifications for the 1,276 translations in` lf-lean` . No human judgment was needed to define “correct” for each case. Combined with rich feedback from the proof assistant, this creates a dense reward signal that lets AI iterate toward correct solutions autonomously.


2.


**AI capabilities on software verification are advancing faster than expected.** We developed` lf-lean` , the first sufficiently complex evaluation to demonstrate this. Compared to METR’s time horizon trend\[2\]


on unverified software engineering, verified software engineering appears to be catching up.


The implication is that software verification, long considered too expensive for practical software development, may become the more scalable approach in an era of AI-assisted programming. When humans can’t review all the code AI produces, having machines verify correctness automatically becomes not just nice to have, but necessary.


In what follows, we explain *task-level specification generators* , the conceptual framework that lets correctness checking scale without marginal human effort. We then describe` rocq-dove` , our prototype environment for verified translation from Rocq to Lean, and how we applied it to produce` lf-lean` . We present details of how we achieved a 350× speedup over manual verification, and make the case for scaling verified software engineering.


## Task-Level Specification Generators


Figure 2:


Constant oversight for arbitrarily scaling codebases. Human oversight is O(1)—define the task-level specification generator once—while verified task completions scale to O(n).


The scaling problem with AI coding is that humans can only review so much. As models generate more code more quickly, human oversight becomes the bottleneck.


One response is to make review faster—better tooling, smarter sampling, and more efficient workflows. This can mitigate the immediate issue, but doesn’t change the fundamental scaling problem: a human must understand each codebase and write codebase-specific verification. Oversight effort remains linear: $\\mathcal{O}(n)$ where $n$ is the number of tasks. This is akin to writing a formal specification for every codebase.


The other response is to make review amortizable. For certain task classes $\\mathcal{T}$ (e.g., translation, optimization, parallelization, refactoring), correctness can be defined generically, without reference to individual task details. A human reviews the specification generator for $\\mathcal{T}$ once; the system then automatically verifies any task instance. Human effort becomes $\\mathcal{O}(1)$ with respect to codebase count. Task-level specification involves defining a specification generator, which is a function that takes a particular instance of a task, or a description thereof, and outputs a specification for that specific instance.


When building a new toolchain, the ideal problem is useful, simple, and representative. We started with translation, which hits all three; there is real demand for porting codebases between languages\[3\]


, it is the simplest semantics-preserving transformation, and it is paradigmatic for the generalized challenge of software verification—establishing semantic equivalence. Semantic equivalence often looks the same regardless of the exact specification or implementation, and verified translation captures this core difficulty, making it an effective task for training on software verification.


---


## ` rocq-dove` : A Task-Level Specification Generator for Translating from Rocq to Lean


We developed a prototype task-level specification generator for the task of verified software translation from Rocq to Lean. Unlike an unverified software translation which yields only a translated version of the code, a *verified translation* produces a` (translation, proof)` pair. The` proof` then serves as a guarantee that the` translation` is semantically equivalent to the` source` . Even if the` translation` is correct, the pair is rejected if the` proof` does not verify its correctness. In this section, we describe the environment setup used for every codebase and the details of how we build *typewise isomorphism* .


### Environment


We describe the general environment for verified translation from Rocq to Lean.


Figure 3:


Pipeline structure for verified translation. The task-level specification generator is applied to the Rocq source and provided to the AI agent, which produces a Lean translation and Rocq proof. The grader validates the output above the trust boundary.


1.


**Specification Generation** :` rocq-dove` derives a theorem statement $\\texttt{Thm}_{\\texttt{Rocq}}$ directly from the Rocq source $\\texttt{Src}_{\\texttt{Rocq}}$. The theorem states that the translation is *typewise isomorphic* to the source; we describe nuances of the definition in theTypewise Isomorphism section.9 9


The theorem statement is the type of the` (translation, proof)` pair. The complicated control flow of having the round-trip compilation be part of the specification is a result of the fact that this is a[dependent type](https://ncatlab.org/nlab/show/dependent+type) . Specifically, it is known as a[dependent sum type](https://ncatlab.org/nlab/show/dependent+sum) or *$\\Sigma$-type* (which, confusingly, is identical to the cartesian product type when it is non-dependent). It is variously written as $\\sum_{a:A} B(a)$ or $\\sum (a:A). B(a)$ or $(a : A) \\times B(a)$ or $\\{a : A~|~B(a)\\}$. In our case, the translation $\\texttt{Target}_{\\texttt{Lean}}$ is $a$ (of type $A$), and the theorem statement $\\texttt{Src}_{\\texttt{Rocq}} \\cong \\texttt{rocq-lean-import}(\\texttt{Target}_{\\texttt{Lean}})$ is $B(a)$.


2.


**Translation** : The AI agent translates $\\texttt{Src}_{\\texttt{Rocq}}$ into $\\texttt{Target}_{\\texttt{Lean}}$ using $\\texttt{Thm}$ as a guide.


3.


**Round-Trip Compilation** : $\\texttt{Target}_{\\texttt{Lean}}$ is compiled back into Rocq using rocq-lean-import10 10


Lean’s compiled` .olean` files can be exported into a textual format with lean4export\[5\]


. rocq-lean-import is a tool that parses this textual format and constructs equivalent definitions in Rocq, preserving the type structure and allowing Rocq to reason about Lean code. The reason we have a tool that directly translates from Lean to Rocq (lean4export + rocq-lean-import) and don’t have a tool in the other direction is a bit of a historical accident: Lean was designed to have a small core with a very powerful elaborator, while Rocq is more permissive in the sort of type theory it supports. As a result, the two most complicated features of type theory are explicit and simple in Lean but implemented in a more complex way in Rocq. (Lean elaborates recursive functions into eliminators and avoids universe polymorphism, while Rocq has a complicated guard checker for recursive functions and supports implicit universe polymorphism with cumulativity.)


\[4\]


, yielding $\\texttt{RoundTrip}_{\\texttt{Rocq}}$. This step is necessary because the correctness specification $\\texttt{Thm}$ is stated as an equivalence between two Rocq terms.11 11


You might wonder why rocq-lean-import doesn’t trivialize the problem. The reason is that the slight differences between Lean and Rocq impose constraints on the translation that rule out a trivial text-to-text source mapping. Moreover, these same differences force the isomorphism proofs to reason about program structure recursively rather than syntactically, which is what turns a trivial-at-first-sight task into something that is actually representative of software verification.


4.


**Proof Generation** : The AI agent constructs a proof $\\texttt{Proof}_{\\texttt{Rocq}}$ of $\\texttt{Thm}$ demonstrating the equivalence $\\texttt{Src}_{\\texttt{Rocq}} \\cong \\texttt{RoundTrip}_{\\texttt{Rocq}}$.


5.


**Grading** : The` rocq-dove` grader checks that the proofs have three properties: (1) proofs are valid, (2) adhere to the specified theorem statements, and (3) avoid relying on additional unsupported assumptions.12 12


Technically the grader also checks a fourth property that is usually not relevant but is sometimes essential: the Lean translation should not inconsistently fill in details that are deliberately held abstract in the Rocq source. As an example, if the Rocq source axiomatizes the real numbers and their properties, the AI should not be permitted to define the type of real numbers to be the empty set while still axiomatizing that the reals are a (nonempty) complete ordered field. If we allowed this, the AI could leverage the resulting contradiction to trivially prove all isomorphisms.


If checking fails, the agent iterates on the translation or the proof until it passes.


### Typewise Isomorphism


Since Rocq and Lean differ slightly in behavior, we require a notion of “equivalence” that’s neither too strict nor too loose. We cannot use strict equality, as writing the same source code for a datatype definition in different files results in datatypes that are not technically “equal”. As such, we define *typewise isomorphism* as follows:


#### Dealing with Types and Values


For datatypes, the standard relaxation of equality13 13


Note that here we mean the[intensional propositional equality](https://ncatlab.org/nlab/show/propositional+equality) of type theory.


is isomorphism: $A$ and $B$ are isomorphic ($A \\cong B$) when we have functions $\\{\\text{to} : A \\to B\\}$ and $\\{\\text{from} : B \\to A\\}$ such that $\\{\\text{to} \\circ \\text{from} = \\text{id}\\}$ and $\\{\\text{from} \\circ \\text{to} = \\text{id}\\}$.14 14


Note that we are defining isomorphism like[category theorists](https://ncatlab.org/nlab/show/isomorphism#definitions) . All functions definable in type theory are structure-preserving.


We can then define equivalence (“valid translation”) for values via[transport](https://ncatlab.org/nlab/show/transport#transport_across_paths) :


$$ a \\cong b := \\text{iso.to}(a) = b $$


where $a : A$, $b : B$, and $\\text{iso} : A \\cong B$.


#### The Problem with Propositions


Unfortunately, this isn’t enough. Since isomorphism is defined as reversible mapping, all true propositions are isomorphic. For example, $1 = 1$ and $2 = 2$ are both true; in a proof-irrelevant setting like Lean, simply mapping any proof of one to any proof of the other yields an isomorphism. We would not want to say that “True” is a valid translation of “my C++ compiler is correct” just because both statements are provable.


#### Dealing with Type-Level Functions


We need to instead enforce that the types line up structurally, which we can do by relationally lifting isomorphism along type-level functions. For example, consider translation of a type-level function $F : \\texttt{Type} \\to \\texttt{Type} \\to \\texttt{Type}$. Instead of saying that a valid translation of the type $F(A, B)$ is any type $X$ with $F(A, B) \\cong X$, we say that a valid translation of $F(A, B)$ is any $F'(A', B')$ satisfying $A \\cong A'$, $B \\cong B'$, and for all $C$, $C'$, $D$, and $D'$,15 15


Note that arrows ($\\to$) in type theory are always right associative; $A \\to B \\to C$ means $A \\to (B \\to C)$. This convention is the one which plays well with currying; $(A \\to (B \\to C)) \\cong ((A \\times B) \\to C)$


$$ (C \\cong C') \\to (D \\cong D') \\to (F(C, D) \\cong F'(C', D')) $$


#### Dealing with Polymorphic Functions


Sometimes, we must apply the appropriate kind of equivalence based on what the object in question is. For example, consider the function` if_then_else_ : ∀ {ρ}, bool → ρ → ρ → ρ` that returns its first or second argument of type` ρ` depending on whether its boolean argument is true or false. Then the relation` if_then_else_ ≅ if_then_else_'` would involve` =` if` ρ` is` int` , but would involve` ≅` if` ρ` is` Type` instead.


#### Prop vs. SProp and Universes


Most projects in Rocq prove theorems whose types are[Prop](https://rocq-prover.org/doc/V9.1.0/refman/language/core/sorts.html) s, meaning that we cannot prove that all proofs of these theorems are equal. However, because Lean only contains proof-irrelevant propositions, the rocq-lean-importer imports[Prop](https://lean-lang.org/theorem_proving_in_lean4/Propositions-and-Proofs/) in Lean to[SProp](https://rocq-prover.org/doc/V9.1.0/refman/addendum/sprop.html) , which is like` Prop` , but with computational proof irrelevance. Luckily, axiomatizing` Prop = SProp` is consistent with software verification projects, and sufficient to deal with such issues. There are also subtleties around universe levels and cumulativity which we elide in this post.


---


## Making` lf-lean`


Loading Translation Explorer…


Interact with explorer


Figure 4.


Interactive translation explorer for Logical Foundations statements in Lean. Each node represents a theorem statement, colored by source module. Click any node to view its direct dependencies and dependents.


While there has been tremendous progress in AI theorem-proving focused on mathematics\[6\]


\[7\]


\[8\]


, our focus is on reasoning about the correctness of software, e.g., programs involving data structures, interpreters, and operational semantics. Software verification differs from formal mathematics in important ways: abstractions are more porous, requiring reasoning thoroughly about codebases with significantly more dependencies. The proof methods required are also different. Brute-force case analysis is more commonly used, and there is a much tighter tie between theorem statement, proof generation methodology, and proof. Yet precisely because verification has historically been so expensive, very few real-world projects have been verified—and correspondingly few benchmarks exist for evaluating AI capabilities in this domain.


The translation task of` rocq-dove` addresses this gap. Because it can generate correctness specifications from any Rocq source, it turns existing codebases into evaluation and training environments for AI software verification without additional human effort. We applied it to the 1,276 Rocq theorem statements from *Logical Foundations* 16 16


For theorems closed with[Qed](https://rocq-prover.org/doc/V9.1.0/refman/proofs/writing-proofs/proof-mode.html#coq:cmd.Qed) (rather than` Defined` ), we include only the statements of theorems and not the proofs. This is safe because Rocq is set up so that changing the code between` Proof.` and` Qed.` cannot change the behavior of the rest of the codebase, with a couple of minor exceptions, of which universe constraints are the most significant. Program verification generally does not care about universe constraints beyond avoiding inconsistency.


\[1\]


, an interactive textbook covering functional programming, type theory, and formal program reasoning. While textbook problems are not real-world software engineering, the textbook’s pedagogical progression—from simple definitions to deeply interconnected proofs—makes it a natural starting point for measuring how AI handles increasing complexity.


The statements in *Logical Foundations* are not standalone problems. The textbook comprises 17 modules forming a deep dependency graph:


- Over 90% of statements depend on at least one earlier statement.
- Dependency chains reach up to 61 total dependencies.


To our knowledge, this makes` lf-lean` the first AI software verification result that includes software with dependencies.


Figure 5:


Distribution of dependency counts across the 1,276 statements in Logical Foundations. Nearly half (49%) depend on 1–5 prior statements, while a long tail extends to 21+ dependencies (3%), illustrating the progression from self-contained definitions to deeply interconnected proofs.


This dependency structure creates a practical problem. A natural but brittle approach would be monolithic translation: asking the model to translate and verify a target statement plus its entire dependency tree in one pass. Empirically, this fails:


- We estimate monolithic translation would yield less than 20% success, as the required context quickly exceeds what any model can reliably hold and manipulate.
- Rollouts where a model solved more dependencies were exponentially rarer, with roughly a 3× reduction in successful problem count for every additional dependency solved.
- We observed 0% performance when a model attempted more than 9 dependencies at once.


The fix mirrors how large software systems stay manageable: modular decomposition.17 17


This bounds verification context to $\\mathcal{O}(\\max_i |c_i|)$ rather than $\\mathcal{O}(|P|)$. We call this *compositional verification* . SeeAppendix D .


Instead of forcing the model to re-derive the whole dependency tree, we treat previously verified dependencies as trusted interfaces. We provide the model with dependency files that have already been translated and verified. Then the model only needs to translate and verify the marginal statement (the new component) against its interface. The proof assistant enforces that interfaces between components match exactly. If each component verifies, the whole dependency graph is correct by construction.


This is just standard software engineering practice applied to verification: simple components, low coupling between them, and cohesive proof obligations that can be discharged independently.18 18


These heuristics map directly to proof tractability: low coupling allows proofs to proceed independently, while high cohesion ensures unrelated functionality has disjoint proof obligations. SeeAppendix D .


Beyond evaluation, we hope the translation itself proves directly useful. With the rising demand for Lean, we hope that our translation will be valuable to PL students. Moreover, we note that since we worked on statement translation, not proof translation, the Lean statements serve as an additional benchmark for evaluating AIs.


---


## Verified Translation at Scale


Without any human intervention, AI agents autonomously generated verified translations of **97%** (1,237 / 1,276) of the theorem statements. The remaining 39 statements were blocked by just 6 “extreme” difficulty statements (0.5% of the total). These 6 required manual solving, taking approximately 15 hours of human effort. After being provided with those six solutions, models autonomously completed the remaining 33 statements, achieving 100% coverage of the benchmark.


To put this in perspective: we estimate that manually translating *Logical Foundations* would have taken 3 person-months, and formally proving the correctness of those translations would have taken an additional 2.5 person-years. Instead, in this workflow, the human effort required was ~15 hours—an over 350× speedup on verification effort alone. We annotated each statement by difficulty based on inference compute required; LoC is the strongest predictor, with a sharp cliff at 17+ LoC where hard and extreme problems dominate (seeAppendix E ).


To situate these results in the broader trajectory of AI software engineering, we adapt METR’s Time Horizon framework, which tracks task complexity with how long it would take a human to complete the task.


Loading chart...


release date →


Figure 1: Closing the Gap Between Verified and Unverified Software Engineering. Adapted from METR's Time Horizon plot, including software verification benchmarks where AIs write code and then prove it correct. We plot only the time horizon for implementation (not verification).` lf-lean` gives us our first measurement of where verified software engineering capability actually is, and the early signal is surprisingly encouraging.


In Figure 1, **circles** represent AI model releases evaluated on unverified software engineering tasks (METR’s time horizon methodology), while **diamonds** represent software verification benchmarks where AIs must both implement and formally prove correctness. We include AlphaVerus\[13\]


, FVAPPS\[14\]


, VerinaBench\[15\]


, and` lf-lean` , which are benchmarks where AIs succeed at not just program implementation, but also software verification.


Note that the plot indicates the time horizon for just the program implementation component, rather than both implementation and translation, as we want a direct comparison of the human labor that can be automated. To approximate their respective time horizons, we conducted human baseline tests on the hardest solved problems of each of the formal verification projects. 19 19


We proxied difficulty for each project via annotations from the authors of FVAPPS and VerinaBench, lines of code, and our own best judgment.


We note that the comparison of verified engineering is still a slight overestimate w.r.t. feasibility given that humans are slower at programming in languages amenable to verification.


We used OLS on the three preexisting verification benchmarks to fit the y-intercept of METR’s time horizon exponential curve to get the lower curve. We used OLS to fit an exponential to the four verification benchmarks (including` lf-lean` ) to get the improved curve.


---


## Conclusion


Conventional wisdom is that software verification is infeasible. But *infeasible* has always meant *too expensive* , not impossible. The cost is measured in LoC: verification artifacts—specifications, proofs, and auxiliary infrastructure—typically dwarf the implementation itself by almost an order of magnitude. Across landmark projects from seL4\[9\]


to CompCert\[10\]


, the median productivity is roughly 2,558 verified LoC per person-year (Appendix B ), and our own translation required approximately 215k lines of isomorphism proofs to verify the translation of 6k lines of Rocq statements into 25k lines of Lean (Appendix A ). Verification has never failed because it is conceptually hard; it has failed because it multiplies the volume of code that humans must write and maintain.


We are now entering a regime with a new calculus of feasibility. LoC generated are becoming effectively free, while human review is the bottleneck. In this regime, verified software engineering may actually be *more scalable* than unverified software engineering, because the oversight itself is automated. Models are not yet good enough at verification to fully realize this vision. But` lf-lean` demonstrates that the gap is closing.


The oversight benefit is realizable by scaling RL on software verification. Most RL environments for code generation rely on programmatic rewards or model-grading. Programmatically written unit tests provide sparse feedback, because test coverage isn’t meaningfully correlated with the number of bugs found (Appendix C ).20 20


After controlling for codebase length and complexity, test coverage has low to moderate correlation with the number of bugs found.


And model-graded rewards are not robust against goodharting or spurious correlations resulting in catastrophic out-of-distribution generalization.


Given the appetite for scaling software complexity, the only scaling feedback on correctness leverages model capability. In` rocq-dove` , we leverage model-generated proofs, guided by the rich feedback of proof assistants. Moreover, by building task-level specification generators, we significantly amortize the investment in the task specification, reusing the same environment for scalably many codebases. We are optimistic that this type of RL environment—dense signal, unbounded task complexity, scalable reuse—can drive rapid capability gains in verified software engineering.


SeeAppendix C .


## Appendix


### A.` lf-lean` by the Numbers


Logical Foundations contains approximately 6k lines of Rocq statements. On average, each statement has 6.4 dependencies with a total of 24.8 LoC (61 dependencies with a total of 382 LoC at maximum). We translated this to approximately 25k lines of Lean code21 21


Our published dataset contains 59,868 lines of Lean spanning 3018 total translations of 1276 distinct statements, as well as 727,111 lines of Rocq (including 505k lines of proof) to prove said translations. Multiplying the total LoC by 1276/3018 yields a low estimate of 25k lines of Lean and 215k lines of Rocq proof that would result from deduplication (simpler questions are duplicated more often). We hypothesize that the 4× blowup results from two incentives in our translation process: (1) the model had an easier time regenerating rather than reusing standard library definitions, whose counts are not included in the LF source; and (2) the model was often incentivized to use longer names and more awkward constructions to exactly match with the Rocq source.


(≈ 3 person-months of work22 22


~3 person-months for translation (assuming 100 kLoC/person-year) plus ~2.5 person-years for verification (at the historical median of ~2,558 verified LoC/person-year). SeeAppendix B for the full derivation and caveats.


) and about 215k lines of Rocq isomorphism proofs (≈ 2.5 person-years of work). Our models produced 1.5M lines of Lean and 7M lines of Rocq proofs in the process of doing the translation.


Our environment,` rocq-dove` , ensures that all isomorphism proof tasks are essentially independent and parallelizable, having interdependencies only through the constraints imposed on the Lean translation. As a result, while the time horizon on verified software engineering ranges from 5 hours (METR) to 3 person-months (` lf-lean` ), the time horizon on the verification proofs themselves is best measured in years.


### B. Human Productivity in Software Verification


To estimate how long the` lf-lean` project would have taken a human team, we need reliable baselines for two distinct activities: writing translation code and writing formal proofs of correctness.


**Translation productivity.** For the code-writing component (translating Rocq definitions and theorem statements into Lean), we draw on unverified software engineering productivity estimates. Published industry figures typically place productive output at 50–100 kLoC/person-year for a developer working in a familiar language and codebase (a range that likely already reflects AI-assisted workflows). We calibrated against internal time trials on` lf-lean` statements and cross-referenced with these published figures, adopting the upper end of the range (100 kLoC/year) to produce a conservative lower bound on human time. Since our Lean translation comprises approximately 25 kLoC, this yields an estimate of roughly 3 person-months for translation alone. We note this likely underestimates the true effort: Lean is less widely known than mainstream languages, the translation requires understanding both Rocq and Lean type theory, and the translator must make non-trivial design decisions about how to represent Rocq idioms in Lean.


**Verification productivity.** For the proof-writing component, no single industry benchmark exists, so we surveyed representative large-scale mechanized verification projects to establish a historical baseline. Projects were included if they:


1. reported non-trivial mechanized verification artifacts (e.g., in Rocq, Isabelle/HOL, F*, or Dafny);
2. provided sufficient quantitative data in peer-reviewed papers, technical reports, or authoritative project documentation; and
3. corresponded to a clearly identifiable verification effort rather than tooling-only or purely theoretical work.


Multiple entries for the same codebase (e.g., original vs. extended CompCert) were treated as distinct data points when reported separately in the literature.


Across these projects, we measured verified LoC per person-year—that is, how many LoC of implementation a team could formally verify per person-year of combined effort on code, specifications, and proofs. The resulting median is approximately 2,558 LoC/person-year, reflecting the PhD-level expertise and intensive manual effort traditionally required. Canonical examples include seL4\[9\]


, the first formally verified OS microkernel, and CompCert\[10\]


, a verified optimizing C compiler (see Figure 7). We note that the surveyed projects vary in domain and proof assistant, introducing additional uncertainty into this median estimate.


Figure 6:


Historical Productivity in Software Verification. Verified LoC per person-year across landmark software verification projects from 2008 to 2022. The median of approximately 2,558 LoC/person-year reflects the PhD-level expertise and extensive manual effort traditionally required.


**Applying these baselines to` lf-lean` .** The` lf-lean` release includes approximately 215 kLoC of Rocq isomorphism proofs covering the 1,276 translated statements. At the historical median verification rate, this volume of proof would require roughly 2.5 person-years of effort. Combined with the ~3 person-months for translation, we estimate the full verified translation would have taken approximately 2.75 person-years. In contrast, our actual human effort was approximately 15 hours (2 person-days), spent on 6 extreme-difficulty statements that the models could not solve autonomously. This represents a speedup of over 350× on the verification effort and roughly 30× on the translation effort alone.


### C. Unit Testing Is Insufficient for Rigorous Analysis


Unit tests are generally insufficient for frontier software engineering tasks, which typically result in non-linear bug count growth in codebase size and complexity. Bugs often result from subtle / high-interaction causes, which can be challenging for unit tests to cover. Expanding test suite size is insufficient, as studies have found a low to moderate correlation between bugs found and code coverage with unit tests\[16\]


. Additionally, after controlling for codebase size and complexity, there is no correlation between code coverage and bugs found\[17\]


.


Figure 7:


Test Coverage vs. Bug Density. Analysis of ~100 large, mature open-source Java systems shows unit-test coverage has no meaningful relationship with the number of bugs reported after release, even when controlling for LoC and complexity. High-coverage systems often still exhibit substantial defect counts, while lower-coverage systems are not consistently worse. (Kochhar et al. 2017)


More anecdotally, Google’s OSS-Fuzz\[18\]


tool continuously tests popular and well-maintained open source libraries and has found thousands of bugs and vulnerabilities.


### D. Compositional Verification


In a monolithic approach, verifying a program $P$ requires fitting the entire codebase into a single context and reasoning about it all at once. As program size grows, this becomes intractable: verification effort scales with total complexity and the required context exceeds any model’s window. We call this monolithic verification.


For programs with modular structure, correctness can be established piecewise. Decompose $P$ into components $c_1, c_2, \\ldots, c_k$ with explicit interfaces. Verify each $c_i$ independently; the proof assistant checks that interfaces between dependent components match exactly. If all components verify, the whole is correct by construction. This bounds verification context to $\\mathcal{O}(\\max_i |c_i|)$ rather than $\\mathcal{O}(|P|)$. We call this compositional verification. We move from verifying whole programs to verifying components against interfaces.


Classical heuristics for well-designed software map directly onto proof tractability. Let $\\text{LoC}_{\\text{proof}}(c)$ denote lines of proof required for component $c$:


- Simplicity → low $\\text{LoC}_{\\text{proof}}(c_i)$ for each component
- Low coupling → proofs for $c_i$ and $c_j$ proceed independently
- High cohesion → unrelated functionality has disjoint proof obligations


In` lf-lean` , 90% of statements depend on at least one prior statement. Rather than requiring the model to verify the full dependency tree simultaneously, we provide previously verified dependencies as trusted interfaces. The task reduces to checking that the new component matches the existing interface, compositionally building toward the full solution.


### E. Measuring Difficulty and Difficulty Annotations


While dependency count captures one axis of difficulty, Lines of Code (LoC) provides a more universal and fine-grained measure. LoC has many nice properties: it is objective, easy to measure, and strongly correlated with token count on model-generated code. Higher LoC problems consume the context window and introduce more opportunities for mistakes in the solution.


In our modular setup, we refine this to marginal LoC: the new code the model must understand and translate to solve a given statement, given that its dependencies are already verified.


Figure 8:


Marginal LoC vs. # Dependencies. Each bubble represents a unique (dependency count, LoC) pair; size and color intensity encode the number of statements at that point. The weak positive trend reflects that higher-dependency statements tend to be longer, but the wide vertical spread shows that LoC and dependency count capture largely independent axes of difficulty.


We did not run clean evaluations specifically for difficulty annotations. Instead, our easy, medium, hard, and extreme ratings are based on how much inference compute was necessary to solve each statement. Concretely, we considered (1) how many best-of-k runs were needed to obtain a successful verified translation, and (2) how many different evaluation setups we had to try before hitting these numbers. Extreme problems were solved by a human.


Figure 9:


Difficulty by marginal LoC. Percentage breakdown of difficulty levels within each marginal LoC bin. As marginal LoC increases, the proportion of easy problems decreases monotonically: 93% → 88% → 85% → 81% → 79% → 50% → 13% → 0%. The difficulty cliff is sharp at 17+ marginal LoC, where hard and extreme problems become dominant.


---


## Acknowledgements


We are grateful to Jake Mendel for encouraging us to work on hard problems.


This work builds on impressive projects and ideas. Gaëtan Gilbert made rocq-lean-import, and Benjamin Pierce wrote the *Software Foundations* textbooks to educate generations of PL students. Kaiyu Yang and Quinn Dougherty generously provided hard problems from VerinaBench and FVAPPS, respectively, for our time horizon baseline. Thomas Kwa helped us understand the METR result methodology.


## Authors


- Vishesh Saraswat* Theorem, MARS, MATS


- Jeffrey Chang* Theorem


- Harshikaa Agrawal Theorem


- Vaidehi Agarwalla Theorem


- Robert Zhang Theorem


- Twm Stone MARS


- Jacob Green MARS


- Shiki Vaahan MARS


- Lawrence Chan METR


- Rajashree Agrawal Theorem


- Jason Gross Theorem


*Equal contribution


Vishesh and Jeffrey co-developed the AI agent harness; Vishesh contributed to the interactive visualizer, and Jeffrey contributed to the blog post. Harshikaa built infrastructure for analyzing agent performance and contributed to the AI agent harness. Vaidehi designed diagrams and collected data for the appendix baselines. Robert developed the human-generated solutions for extreme-difficulty statements, and developed the time horizon study. Twm, Shiki, and Jacob extracted the initial dataset of Rocq problems from the *Software Foundations* textbook. Lawrence advised on blog post writing and presentation of the results. Rajashree co-led the project, and developed the task-level specification generator framework and wrote the blog post. Jason led the project, and developed the specification generator and grader in` rocq-dove` .
