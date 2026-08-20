---
schema_version: "1.0.0"
document_id: "637b1b3d279add997ba772db4c977a75845f8b921755992a972e4308b451dff9"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/cmov-vs-branch-perf/"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-23T21:54:08.144992+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:2db2f26717b3a4375cc13a33be7540528c48b9127f0f484322f0566750730c76"
---

# The Most Expensive Instruction Might Be… cmov

*Or: how I fact-checked my own LinkedIn comment and found a compiler heuristic that measures the wrong thing.*


It started with a[LinkedIn comment](https://www.linkedin.com/posts/tomaspitner_the-most-expensive-instruction-might-be-share-7482455564745363456-NoZe/) .[Tomáš Pitner](https://www.linkedin.com/in/tomaspitner/) published a nice[article](https://medium.com/@tomas.pitner/the-most-expensive-instruction-might-be-if-92d8f2864c35) about branch mispredictions and how Clang turns` if` into conditional instructions on ARM64. I did what the platform is optimized for: replied instantly, with my brain on auto-pilot:


> "\[...\] on x86-64 conditional instructions win if and only if a branch is unpredictable. When a branch predictor wins then a branch is cheap and beats cmov & friends. JIT compilers have an advantage here - they can emit different code for different patterns observed."


Then I re-read what I wrote and it made me slightly uneasy: *JIT compilers can emit different code for different patterns observed.* Can they? And do they? What does HotSpot actually observe about a branch? Tomáš is a university professor after all, I don't want to be wrong under HIS status!


I decided to find out: analyze Hotspot source code and then measure. Spoiler: my comment was wrong. HotSpot doesn't observe branch *patterns* at all. It observes something that looks deceptively similar and in a hot loop the difference can be worth 2.9×.


## Bias is not predictability


Take a branch in a loop. There are two different questions you can ask about it:


1. Bias: How often does it go each way? A 90/10 branch is heavily biased; a 50/50 branch is not.
2. Predictability: Can the CPU's branch predictor guess the next outcome?


These two properties are independent. Consider four data patterns driving the same` value > 0` test:


50/50 bias 90/10 bias


**predictable**` TNTNTNTN…` (strict alternation)` TTTTTTTTTN` repeating


**unpredictable** random, 50% positive random, 90% positive


A modern branch predictor has no problem with the alternating pattern at all: It has near-zero mispredictions despite the 50/50 split. The random 90%-positive data, despite its comfortable 90/10 bias, still mispredicts about 10% of the time.


Why does the compiler care anyway? Because it has a choice to make! It can keep the branch, or it can flatten the whole if/else shape (a "diamond", in compiler speak) into a conditional move,` cmov` on x86. The trade:


- A **branch** is a guess. The CPU doesn't wait for the condition. Instead, it speculates on the outcome and races ahead. So a correctly predicted branch costs almost nothing. A misprediction throws away the speculated work and restarts, and that is expensive.
- A **cmov** is a wait, but only for whoever uses its result. It can never mispredict, and independent work still runs ahead freely; but anything that consumes the chosen value has to wait for the condition *and both* inputs to be computed. If each iteration's result feeds the next one, you pay that latency every iteration, regardless of how predictable the data was.


So the right choice depends on several things: how much work each side of the` if` does, whether anything downstream waits on the result and how much other work the loop offers to hide latency behind.


## What the profile actually contains


HotSpot's strength is profile-guided compilation: code starts in the interpreter and the quick C1 compiler, both of which record statistics as they go, and the optimizing C2 compiler later uses those statistics. For an ordinary two-way branch, here is what they record. In` MethodData` , every such branch gets a[BranchData](https://github.com/openjdk/jdk/blob/723826295aa5/src/hotspot/share/oops/methodData.hpp#L1515) record:


```text
class JumpData : public ProfileData {     enum {       taken_off_set,          // the "taken" counter       displacement_off_set,       jump_cell_count         // number of slots above     };
// It consists of taken and not_taken counts as well as a data displacement   class BranchData : public JumpData {     enum {       // the "not taken" counter, appended after JumpData's slots       not_taken_off_set = jump_cell_count,
```


Two aggregate outcome counters. The[x86 interpreter](https://github.com/openjdk/jdk/blob/723826295aa5/src/hotspot/cpu/x86/interp_masm_x86.cpp#L1324) bumps one of them per profiled execution:


```text
increment_mdp_data_at(mdp, in_bytes(JumpData::taken_offset()));   // ...   increment_mdp_data_at(mdp, in_bytes(BranchData::not_taken_offset()));
```


C1 does the equivalent. *(Pedantic footnote: the C++ increment helpers saturate, but the x86 interpreter's fast path is an unchecked` addptr` , a discrepancy that matters to approximately nobody.)*


When C2 compiles the method,[Parse::dynamic_branch_prediction](https://github.com/openjdk/jdk/blob/723826295aa5/src/hotspot/share/opto/parse2.cpp#L1222) reads a snapshot of the two counts (the snapshot itself is explicitly approximate and racy), requires at least[40 combined observations](https://github.com/openjdk/jdk/blob/723826295aa5/src/hotspot/share/opto/parse2.cpp#L1262-L1267) , and[computes their ratio](https://github.com/openjdk/jdk/blob/723826295aa5/src/hotspot/share/opto/parse2.cpp#L1285) :


```text
prob = (float)taken / (float)(taken + not_taken);
```


That ratio is all the conversion heuristic will see. The` MethodData` profile this decision consumes does not contain outcome ordering, branch history or hardware misprediction counts. A strictly alternating branch and a randomly driven branch with equal counts are indistinguishable to everything downstream.


So the branch-outcome signal that reaches C2's cmov decision measures bias. Where does it get used?


## The comment vs. the code


The branch-to-cmov conversion lives in[PhaseIdealLoop::conditional_move](https://github.com/openjdk/jdk/blob/723826295aa5/src/hotspot/share/opto/loopopts.cpp#L722) in` loopopts.cpp` . Before the profile even matters, the candidate has to pass a pile of structural checks: the if/else must form a clean diamond, both sides must be cheap enough (` ConditionalMoveLimit` , default 3 on x86), and the value types must be supported. Then the[decision we care about](https://github.com/openjdk/jdk/blob/723826295aa5/src/hotspot/share/opto/loopopts.cpp#L859) :


```text
float infrequent_prob = PROB_UNLIKELY_MAG(3); // = 0.001   // ... raised for branches inside loops (with default flags):   infrequent_prob = MAX2(infrequent_prob, (float)BlockLayoutMinDiamondPercentage/110.0f);   // ...   // Check for highly predictable branch.   } else if (iff->_prob < infrequent_prob ||       iff->_prob > (1.0f - infrequent_prob))
```


The comment says *highly predictable branch* , but the code tests` _prob` , the taken ratio. C2 counts (approximately) every profiled execution of the branch and observes nothing about their order.


Notice the threshold: with the default` BlockLayoutMinDiamondPercentage = 20` , the branch has to be more one-sided than` 20/110 ≈ 0.1818` to stay a branch, not the 0.001 the first line suggests. So the effective rule is:


> For an eligible integer diamond whose result is used inside the loop, with x86 defaults, C2 permits the cmov conversion only when the taken probability is within **\[18.2%, 81.8%\]** .


Concrete predictions for our four patterns: both 50/50 patterns get cmov, both 90/10 patterns keep the branch, order be damned. For a loop where the selected value feeds the next iteration, the hypothesis says that's the wrong call in two of the four cells; whether the cost difference is big enough to matter is what benchmarks are for.


## The experiment


Everything below ran on my Ryzen 9950X, Linux, a release build of OpenJDK mainline (commit` 7238262` ), pinned to one core, performance governor.


[Two loops](https://github.com/jerrinot/c2-cmov-bias/blob/master/src/main/java/org/openjdk/experiments/CMoveBiasBench.java) over a 1M-element` int\[\]` . Both keep a running value where each iteration depends on the previous one, which also keeps C2's auto-vectorizer away.


```text
// "carried": the selected value feeds the next iteration.   // The select sits on the loop-carried critical path.   long s = SEED;   for (int value : data) {       s = value > 0 ? s ^ mix : s;   }
// "independent": the select result is consumed,   // but doesn't gate the next iteration's select.   long s = SEED;   for (int value : data) {       long selected = value > 0 ? c1 : c2;       s = Long.rotateLeft(s, 13) ^ selected;   }
```


The two loops differ in what happens with the chosen value. In the **carried** loop, it becomes the input of the next iteration: if the choice becomes a cmov, its wait-for-everything latency chains across all million iterations. This is the worst case from the trade-off above. In the **independent** loop, the choice is between two constants; the result is mixed into` s` , but the choice itself doesn't depend on anything from the previous iteration. The running value still chains iterations together (rotate, xor), but the cmov is not part of that chain: the CPU can compute the choices for upcoming iterations ahead of time, so their latency is hidden. If the theory holds, the branch-vs-cmov choice should matter a lot in the first loop and barely at all in the second.


We test with three compiler configurations per each pattern:


1. default: let C2 decide whether to use cmov or branch
2. branch-forced:` -XX:ConditionalMoveLimit=0`
3. cmov-forced: there is a product flag literally named` -XX:+UseCMoveUnconditionally` , documented as "use CMove (scalar and vector) ignoring profitability test." Inside` conditional_move` , its uses are float/double-specific: one affects the float result-type cost, the other bypasses the probability gate only for float/double *compares* . For this integer-compare, integer-result loop it forces nothing: with the flag alone, the 90/10 run still emitted zero select cmovs. The magical combination that worked here is` -XX:+UseCMoveUnconditionally -XX:BlockLayoutMinDiamondPercentage=0` , which widens the probability window.


## Results


Carried loop, 1,048,576 elements per op, 3 forks; errors are JMH's 99.9% confidence intervals. Each pattern shows the default plus only the forced configuration that flips C2's choice. Forcing the choice C2 already made produces the same code, so those rows are omitted:


pattern config select cmov? ns/op branch misses/op


ALT 50/50 default yes 387,783 ± 225 168


ALT 50/50 branch-forced no **135,407 ± 16,415** 149


RAND 50/50 default yes **387,906 ± 539** 181


RAND 50/50 branch-forced no 2,686,656 ± 16,586 511,417


RAND 90/10 default **no** 725,786 ± 11,439 107,446


RAND 90/10 cmov-forced yes **387,716 ± 92** 196


ALT 90/10 default **no** **177,705 ± 171** 171


ALT 90/10 cmov-forced yes 387,912 ± 960 196


The results show two things I find interesting.


First, cmov is a flat line. ~388µs in every pattern: about 2.0 cycles per element (CPU cycles from perf counters divided by the 1,048,576 elements), consistent with the loop spending its time in the` cmov → xor` chain, where each iteration waits for the previous one (the counter also includes the load, compare, and loop control). That's a common trade-off with branchless code: you buy immunity to the worst case at the price of never seeing the perfect one. The branch versions range from 135µs (prediction working) to 2.7ms (48.8% miss rate on random 50/50 data).


Second, arrange the defaults into the 2×2 and the mismatches land exactly where the source reading predicted:


50/50 → C2 picks cmov 90/10 → C2 picks branch


**predictable** ❌ branch wins **2.9×** ✅ correct


**unpredictable** ✅ correct ❌ cmov wins **1.9×**


C2's decision is constant along columns (bias). The hardware's verdict is constant along rows (predictability). The two cells C2 gets right are exactly the ones where the two properties happen to point the same way; in the other two it leaves 2.9× and 1.9× on the table. The heuristic consumes a different quantity that merely correlates with the right one on typical data.


Running the same patterns through the independent loop shows the third effect: in this benchmark, the large cmov penalty appears only when the chosen value feeds the next iteration


pattern config select cmov? ns/op branch misses/op


ALT 50/50 default yes 389,438 ± 416 188


ALT 50/50 branch-forced no 390,858 ± 229 306


RAND 50/50 default yes 389,491 ± 234 181


RAND 50/50 branch-forced no 2,874,266 ± 25,371 521,665


RAND 90/10 default no 766,716 ± 6,818 105,507


RAND 90/10 cmov-forced yes 389,155 ± 137 173


ALT 90/10 default no 390,860 ± 283 388


ALT 90/10 cmov-forced yes 389,455 ± 403 193


ALT 50/50 is now a tie (389µs cmov vs. 391µs branch): out-of-order execution hides the cmov latency when nothing downstream waits on it. Mispredictions still hurt, chain or no chain: random data in branch form stays slow. But cmov's downside is gone. The 2.9× regression only shows up when each iteration's result feeds the next one, because there's no independent work left to hide the latency.


## Could it do better?


I don't understand compilers well enough to propose a fix. CPUs can report which branches mispredict (performance counters, etc), but I assume collecting that from a JIT is awkward and expensive. While looking for how other compilers handle this, I found that LLVM has a dedicated[SelectOptimize pass](https://github.com/llvm/llvm-project/blob/main/llvm/lib/CodeGen/SelectOptimize.cpp) for the cmov-vs-branch question, including the cost of a cmov whose result feeds the next iteration. Its branch input is still the taken/not-taken counts (see? bias again!) and in the[review discussion](https://reviews.llvm.org/D120230) the reviewers note themselves that those counts are only a proxy, and that an unbiased branch may still predict well.


## Takeaways


- The only record of an ordinary branch's runtime behavior that reaches C2's cmov decision is two counters, taken and not taken, and the decision uses only their ratio. That measures **bias** . The hardware charges for **predictability** (or lack thereof). These coincide on typical data and diverge on adversarial data.
- For eligible integer diamonds used inside a loop, the cmov conversion is permitted only for taken-probability in **\[18.2%, 81.8%\]** (x86 defaults).
- When the selected value feeds the next iteration, C2 gets it wrong in both directions on my hardware: **2.9× slow** on predictable 50/50 data (cmov where a branch would be ideal), **1.9× slow** on random 90/10 data (branch where a cmov belongs). Without that dependency, the gap largely disappears in this benchmark.


**Bonus** : Linus Torvalds has this[beautiful rant against CMOV](https://yarchive.net/comp/linux/cmov.html) . The rant is almost 20 years old, but the principles still apply today.


---


*Thanks to Tomáš Pitner, whose article set this off and whose next issue I hereby owe a benchmark :)*
