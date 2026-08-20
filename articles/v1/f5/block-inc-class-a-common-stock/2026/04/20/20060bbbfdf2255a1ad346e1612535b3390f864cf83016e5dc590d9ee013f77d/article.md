---
schema_version: "1.0.0"
document_id: "20060bbbfdf2255a1ad346e1612535b3390f864cf83016e5dc590d9ee013f77d"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/proof-not-promises-evaluating-code-scanner-efficacy"
published_at: "2026-04-20T12:00:00+00:00"
first_seen_at: "2026-07-20T03:32:06.876214+00:00"
fetched_at: "2026-07-28T20:52:31.063760+00:00"
content_hash: "sha256:0f5ab326679a2f9fdc74ef6ebd01c0c479498e612eaa67b60a8a4c3b3516d3e7"
---

# Proof, Not Promises: Evaluating Code Scanner Efficacy

Every static analysis security testing (SAST) scanner on the market promises to find your bugs, whether it's Semgrep, CodeQL, your commercial suite, or your large language model (LLM)-powered newcomer. The hard part is proving which one deserves CI time, reviewer attention, and budget. A scanner that catches real bugs but drowns you in junk may be worse in practice than a quieter tool. A scanner that looks sharp in one run but flakes in the next is not reliable. And a vendor benchmark on a corpus you cannot inspect is not enough evidence on its own.


To evaluate a scanner properly, you need the whole confusion matrix: true positives, false positives, false negatives, and true negatives. In plainer language: what it caught, what it invented, what it missed, and what it correctly ignored. Most SAST comparisons obsess over the first number, hand-wave the second, and skip the last entirely because they never annotate convincing non-bugs in the first place. That omission matters: alert fatigue is a measurable cost, and not every miss costs the same. Missing an` alg:none` JWT bypass is not equivalent to missing a verbose error message.


You also need to know what the summary numbers are actually saying. Precision tells you how much of the scanner's output is real. Recall tells you how much of the real vulnerability set it found. F1 combines precision and recall into one score, which makes it useful for quick comparisons. But it is not the whole story: a respectable F1 can still hide an operationally painful false-positive rate, or a miss on one` must` -find bug that you really cared about.


Then there is stability. Semgrep ruleset updates can regress coverage silently. Commercial vendors publish F1 scores against corpora you cannot audit. LLM-based scanners can disagree with themselves between runs on the same code, even with temperature pinned. We built[benchmrk](https://github.com/block/benchmrk) because we wanted a repeatable answer to all of that: a harness that tells you what your scanner catches, misses, and invents.


## What` benchmrk` is


` benchmrk` is a harness for measuring SAST scanner efficacy against ground truth you control. It is not a scanner itself and it does not find bugs. It runs your scanners against a corpus of known-true and known-decoy vulnerabilities you provide, and tells you how each scanner performed. The goal is to replace *"how is our SAST doing?"* as a gut call with a repeatable answer that survives a re-read six months later with different reviewers in the room.


If you can annotate real bugs and real decoys, you can measure what your scanners actually do.


## How it works


` benchmrk` 's ground truth is an **annotation file** : a JSON document you (or a team of reviewers) maintain separately from the target codebase, describing every known vulnerability the codebase contains and every intentional false-positive decoy worth scoring. The top-level shape is` {"vulnerabilities": \[...\]}` . Each entry is a single *vulnerability* , not a single location. Real bugs often span multiple files (source and sink of a data flow, or the same authorisation gap repeated across many endpoints), and the format captures that shape directly. Each vulnerability comprises:


- **` cwes`** : a *set* of Common Weakness Enumeration (CWEs) a reasonable scanner might assign. The matcher accepts any member as an exact match, rather than forcing reviewers to commit to one canonical label for a bug two engineers might legitimately classify differently.
- **` evidence`** : one or more evidence rows where the bug manifests. Each row carries a` file` , a` line` , optional` end` , optional` role` , and the per-location` category` and` severity` . A scanner finding matching *any* evidence row satisfies the whole vulnerability.
- **` criticality`** : determines the expectation from a scanner discovering the vulnerability.` must` ,` should` , and` may` are the three defined tiers (seeHow it avoids cheating itself ).
- **` status`** :` valid` for real bugs;` invalid` for intentional decoys (code that *looks* vulnerable but isn't).
- **` annotated_by`** : the reviewers who co-signed the annotation, giving you a consensus signal for later scoring (seeHow it avoids cheating itself ).


A minimal current-format annotation for an IDOR that manifests across two files looks like this:


```text
json    1  {
2      "vulnerabilities"  :     [
3        {
4          "name"  :     "idor-users-endpoint"  ,
5          "description"  :     "User records can be fetched without an ownership check"  ,
6          "cwes"  :     [  "CWE-639"  ,     "CWE-862"  ,     "CWE-863"  ]  ,
7          "status"  :     "valid"  ,
8          "criticality"  :     "must"  ,
9          "annotated_by"  :     [  "jack@example.com"  ,     "jill@example.com"  ]  ,
10          "evidence"  :     [
11            {
12              "file"  :     "routes/api/users.js"  ,
13              "line"  :     42  ,
14              "end"  :     55  ,
15              "role"  :     "sink"  ,
16              "category"  :     "broken-access-control"  ,
17              "severity"  :     "high"
18            }  ,
19            {
20              "file"  :     "services/user_svc.go"  ,
21              "line"  :     88  ,
22              "end"  :     102  ,
23              "role"  :     "sink"  ,
24              "category"  :     "broken-access-control"  ,
25              "severity"  :     "high"
26            }
27          ]
28        }
29      ]
30   }
```


Write annotations in that shape: one vulnerability entry, many evidence rows, one acceptable CWE set.


One operational rule matters here: keep annotation files outside the target codebase the scanner is analyzing.` benchmrk` imports ground truth separately from the corpus path you register and scan. If a scanner can read the annotations directly, especially an LLM-based scanner, you have effectively handed it the answer sheet.


Annotations are the artefact you maintain by hand (or grow through the triage loop described later). Everything else` benchmrk` produces, from F1 scores to coverage-overlap tables, is computed from them. If you can annotate it, you can measure it.


The unit of *analysis* is an **experiment** : a matrix of scanners × projects × iterations, persisted to SQLite, re-runnable and re-scorable at any point. A parameter sweep is an experiment with multiple scanner configurations. A stability check is one with high iteration count. Both flow through the same` compare` invocation. Pre-existing scan results can also be imported directly, enabling point-in-time assessments without re-executing the scanner.


Figure 1: the` benchmrk` pipeline. Annotated ground truth and scanner findings (via SARIF import or in-harness runs) feed a deterministic matcher; results are persisted to SQLite and re-scorable at any time without re-running the scanner.


## How it avoids cheating itself


A benchmark system has three places it can quietly lie: matching logic, ground truth, and provenance across runs.


**Deterministic matching.** There is no LLM in the matching or scoring loop. We were not going to evaluate a stochastic scanner with a stochastic evaluator. The matcher (` internal/analysis/matcher.go` ) is a tiered Go algorithm and assigns each direct finding-to-evidence comparison one of five tiers in descending order of strength:


1. ` exact` : overlapping lines, identical CWE. Confidence 1.0.
2. ` hierarchy` : overlapping lines, CWEs related in the MITRE tree. 0.75–0.95, decaying with hop count.
3. ` fuzzy` : nearby lines, CWE-related. 0.5–0.9.
4. ` category` : wider line window, CWE-related. 0.3–0.5.
5. ` same_line` : overlapping lines, unrelated CWEs. 0.2. Last-resort fallback.


CWE hierarchy walking is generated from MITRE CWE and capped at depth 3, because otherwise everything converges at root concepts like *"improper input validation."* The CWE tree can be updated at any time with the latest CWE database. Hand-maintained "related CWEs" tables are the usual workaround; they become unmaintainable the moment a scanner emits a CWE label that isn't in the table.


**CWE accept-sets.** A single vulnerability can legitimately carry several CWEs. An SQL injection is CWE-89 (Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')) to some scanners, CWE-943 (Improper Neutralization of Special Elements in Data Query Logic) to others. Most CWE-aware matchers pick one and punish the alternatives.` benchmrk` 's matcher takes a *set* and accepts any member. A scanner reporting any label in the set gets an` exact` match, not a` hierarchy` match. Two scanners reviewing the same bug under a different lens get rewarded, not punished.


**Evidence annotations for source-to-sink bugs.** The vulnerability, not the location, is the unit of accounting. One bug is often many lines across many files. An IDOR might span route registration, a missing authorisation check in the handler, and a database call that exposes the object. A naive matcher that scores each location independently would punish a scanner for reporting one consolidated finding: five out of six endpoints would count as FNs even though the scanner correctly identified the systemic bug.` benchmrk` inverts this. A vulnerability carries one or more evidence rows (source and sink locations), and a single scanner finding matching any one of them satisfies the whole vulnerability: one TP, not one-TP-plus-five-FNs. You are measuring *"did the scanner find this bug?"* , not *"did the scanner enumerate every symptom?"*


**Plural ground truth.** Tiered criticality is what lets` compare` stratify recall by how much the miss actually costs. Annotations have three criticality ratings:` must` ,` should` , and` may` .` must` marks non-negotiable vulnerabilities a competent scanner needs to find.` should` marks bugs the scanner ought to catch but that are not blockers.` may` marks things that noisy scanners might detect but a miss is a non-issue operationally. Separately, every annotation carries an` annotated_by` list of reviewers who co-signed it, and` compare --min-consensus N` restricts scoring to annotations with at least N names. This means that you can compare at N=1 and N=2 and watch whether the ranking shifts. If it does, your F1 was riding on one person's perspective on the code.


**Scorer pinning.** Every scored run is stamped with` matcher_version` and` annotation_hash` (a digest of ground truth at scoring time).` compare` warns when runs carry different stamps, because their numbers are not comparable.` rescore` clears derived matches and re-runs the matcher against current ground truth, preserving raw findings and experiment IDs. External references (blog posts, decision records, slide decks) still resolve; only the evaluation updates.


## Ground truth as a living artefact


Matchers are wrong sometimes.` benchmrk triage` lists unmatched findings and lets you assign dispositions: TP, FP, or needs-review.` triage --promote` writes those dispositions back: true positives become new vulnerabilities or new evidence on existing ones (` --attach-to <vuln>` ); false positives become` invalid` decoys that future scanners are scored against on the TN axis.


` rescore` makes the loop cheap. Without it, every annotation change requires re-running every scanner (minutes at best, dollars at worst), and in practice nobody updates the annotations. With it, updating annotations and recomputing matches is seconds against the historical finding store. Ground truth iterates at code-review speed rather than scan speed. That's the difference between an annotation file people maintain and one they don't.


Figure 2: the ground-truth resolution loop. Triaged findings flow back into the annotation file — TPs as new evidence rows, FPs as` invalid` decoys — and` rescore` re-matches every historical run in seconds, without re-executing any scanner.


` benchmrk review run <run-id>` opens the HTML triage UI for the human-in-the-loop step. The realistic alternative is usually a spreadsheet, and spreadsheet-based triage loops rarely survive repeated use.


## What` benchmrk` Lets You Answer


Three scenarios, three core use cases the tool is built around:


- **A/B testing** one scanner's configuration or prompt variants.
- **Composition analysis** across similar tools or an entire SAST stack: who catches what, who's redundant.
- **Post-hoc incident analysis** : given a bug that made it through the scanner suite, figuring out whether the scanner actually missed it and how to close the gap.


### A/B testing: prompt variants on an LLM-based scanner


One straightforward use for` benchmrk` is prompt A/B testing. After Nicholas Carlini's[Unprompted (2026)](https://www.youtube.com/watch?v=1sd26pWhfmg) talk, we wanted to test whether that style of prompting would actually improve an LLM-based code scanner on a fixed benchmark. The idea was simple enough to test: if you prompt the model more like an adversarial solver than a passive reviewer, does it find more of the bugs that matter?


We compared three prompt variants:` default` (our baseline scaffolded prompt set),` carlini-slim` (a minimal prompt set using the Carlini "CTF" framing), and` exploit-proof` (a modified version of` carlini-slim` that adds explicit exploitability gates). Holding the model constant (` claude-opus-4-6` ) along with thinking and output parameters, we ran the three variants across two annotated benchmark targets for three iterations each: 18 runs total. One of those targets was Go-DocVault, an intentionally vulnerable Go application with full` benchmrk` annotations, used as a controlled benchmark corpus. After about two hours of wall clock and $18 in API spend we had some answers.


**Go-DocVault (Go, 34 annotations), aggregate:**


From the output below we can see that` opus-default` wins on aggregate F1 score. However, stratifying by criticality inverts the story:


```text
1  METRIC             opus-default    opus-exploit-proof  opus-carlini-slim  BEST
2  TP                 29              27                  29                 opus-default
3  FP                 3               7                   10                 opus-default
4  FN                 5               7                   5                  opus-default
5  TN                 7               6                   6                  opus-default
6  Precision          0.9062 ±0.0474  0.7941 ±0.0451      0.7436 ±0.0112     opus-default
7  Recall             0.8529 ±0.0139  0.7941 ±0.0240      0.8529 ±0.0139     opus-default (within σ)
8    Recall (must)    0.9231          1.0000              1.0000             opus-exploit-proof
9    Recall (should)  0.8000          0.8000              0.7333             opus-default
10    Recall (may)     0.8333          0.5000              0.6667             opus-default
11  F1                 0.8788 ±0.0183  0.7941 ±0.0218      0.7945 ±0.0114     opus-default
12  Accuracy           0.8182 ±0.0275  0.7021 ±0.0293      0.7000 ±0.0141     opus-default
13  Duration (ms)      730660          378455              394022             opus-exploit-proof
14
15  COVERAGE OVERLAP  (valid vulnerabilities only)
16    Union recall:       0.9118   (best single: 0.9118, opus-default)
17    → running all 3 gains nothing over opus-default alone
18    Union FP ceiling:   ≤14
19
20    Caught by all     28   [must:13 should:12 may:3]
21    Caught by none     3   [should:2 may:1]   ← blind spots regardless of which you pick
22
23  MARGINAL CONTRIBUTION
24    opus-default        1  [should:1]  no-bruteforce-limit
25    opus-exploit-proof  0              ← redundant given the others
26    opus-carlini-slim   0              ← redundant given the others
27
28  FLAKY COVERAGE
29    opus-carlini-slim   avatar-no-integrity  1/3 iterations
30    opus-carlini-slim   user-enum-timing     1/3 iterations
31    opus-default        jwt-alg-none         1/3 iterations
32    opus-default        pinned-old-jwt-lib   2/3 iterations
33    opus-default        user-enum-timing     1/3 iterations
34    opus-exploit-proof  jwt-alg-none         2/3 iterations
```


Figure 3: raw` benchmrk compare` output for the three prompt variants (` opus-default` ,` opus-exploit-proof` ,` opus-carlini-slim` ) on Go-DocVault, three iterations each. Aggregate metrics, tier-stratified recall, coverage overlap, marginal contribution, and per-vulnerability flake data all land in a single report.


` opus-default` has the best aggregate F1 score and the *worst* must-tier recall. The gap has a mechanism:` opus-default` misses` jwt-alg-none` in 1 out of 3 iterations. Both` slim` variants catch it 3/3. Single-iteration evaluation, the way most scanners get spot-checked, would have shown` default` catching the bug and called it reliable. Three iterations revealed the flake; one iteration would have hidden it.


Figure 4: A/B testing flow in` benchmrk` . One scanner is registered N times under different variant axes — configuration, profile, model, prompt, ruleset — and run against the same corpus for the same iteration count;` compare` then attributes any metric shift to the single variable you changed.


### Composition analysis: do I need all of these tools?


Headline F1 score tells you which tool is best. It doesn't tell you whether running *all of them* catches more.` compare --coverage` does. Here is what the output looks like in a hypothetical three-scanner composition (Semgrep, CodeQL, and Horusec open-source scanners) against a codebase with 31 annotated vulnerabilities:


```text
1  COVERAGE OVERLAP  (valid vulnerabilities only)
2
3    Union recall:       0.7419   (best single: 0.6452, semgrep)
4    → running all 3 scanners gains +0.0967 recall over the best one alone
5
6    Caught by all      18   [must:3 should:14 may:1]
7    Caught by none      8   [must:0 should:6 may:2]   ← blind spots regardless
8
9  MARGINAL CONTRIBUTION
10    semgrep   2   [must:1 should:1]   jwt-alg-none, csrf-token-missing
11    codeql    1   [should:1]          mass-assignment-role
12    horusec   0                       ← redundant given the others
```


In this hypothetical, three things fall out immediately. Horusec adds nothing the other two don't already find. Dropping Semgrep loses a` must` -tier bug. The eight blind spots are` should` /` may` only, so the suite isn't missing anything critical. Without this output, *"should I keep running Horusec in CI?"* is a gut call. With it, it's an evidence-backed decision - giving time back to those who maintain or review the tool.


The actual prompt-variant experiment detailed inA/B testing: prompt variants on an LLM-based scanner produced the same shape of answer with real numbers: on JS-TodoApp, running all three opus variants gains ~0.04 recall over the best single at triple the cost. On Go-DocVault, zero gain. Three bugs are missed by every variant in every iteration. Shared blind spots are the strongest signal you'll get that you've hit a model ceiling.


Figure 5: composition analysis. Union recall measures what a scanner suite catches *together* ; marginal contribution identifies which scanners still pull their weight and which are redundant given the rest — the basis for evidence-backed "do I keep running this tool?" decisions.


### Post-hoc incident analysis: why did we miss this?


An anonymized bug-bounty report landed: reflected XSS in a TypeScript web app. A` return_to` query parameter flowed through` decodeURIComponent()` into a client-side navigation API; a` javascript:` URI executed in the victim's session. The fix added a host allowlist at all four caller sites.


We had CodeQL in three tiers: an extended suite, a CI suite, and the upstream default. We needed to answer the question: should this vulnerability have been detected?


**Step 1, annotate the pre-fix commit.** One` benchmrk` vulnerability,` return-to-xss` , with eight evidence rows: source (` decodeURIComponent` ) and sink (navigation API) at each of the four sites. Accept-set` {CWE-79, CWE-601, CWE-20}` . Criticality` must` .


**Step 2, register the three configurations.** Register three scanners with` benchmrk` using each of the production CodeQL configurations (that is, rule sets). An agent can automate this setup.


**Step 3, score:**


Configuration TP FP FN Recall F1


codeql-default 0 4 1 0.00 0.00


codeql-ci 0 7 1 0.00 0.00


codeql-internal 0 8 1 0.00 0.00


A complete miss. The tempting conclusion: CodeQL cannot find this XSS. The review phase said otherwise.


**Step 4, near-miss review.**` benchmrk review run <run-id>` renders a card per finding showing source context and, for unmatched findings, the nearest ground-truth evidence and why it didn't match. Three findings across all three configs pointed at the *same two lines* in a small utility module,` utils/view.ts` :


Rule File Line Disposition


` js/xss`` utils/view.ts` 25 TP (wrapper site)


` js/xss`` utils/view.ts` 48 TP


` js/client-side-unvalidated-url-redirection`` utils/view.ts` 25 TP


` js/incomplete-sanitization`` api/v1/export.ts` 25 FP


CodeQL's taint analysis had traced dataflow back through the wrapper and reported at the definition, not the caller sites. Those definition sites were not captured in our evidence set, so the scanner appeared to be "missing" the issue. Effectively this was a case of *"same bug, different coordinates"* .


**Step 5, promote and rescore:**


```text
shell    1  benchmrk triage   <  run-id  >   --promote --criticality must --attach-to return-to-xss
2  benchmrk rescore   <  project-name  >
```


The TPs become new evidence rows on` return-to-xss` . The FP becomes an` invalid` decoy.` rescore` re-runs the matcher against updated annotations in under a second. No scanners re-execute.


**Step 6, post-rescore:**


Configuration TP FP FN Recall (must) Precision F1


codeql-default 1 1 0 1.00 0.50 0.67


codeql-ci 1 4 0 1.00 0.20 0.33


codeql-internal 1 5 0 1.00 0.17 0.29


All three configurations now catch the bug. Recall on the` must` -tier vulnerability is 1.00 across the board.


So back to the original question. Why was this missed? The honest answer is also the simplest; it was never scanned in production and therefore the vulnerability never surfaced. Further investigation identified a coverage gap in our GitHub security policy that left the repository exposed.


From this postmortem we were able to resolve the coverage gap and validate that our production ruleset would have raised the issue in the first place.


Figure 6: the post-hoc incident analysis loop. Annotate the pre-fix commit, score against the production scanner configurations, use` benchmrk review` to surface near-misses reported at different coordinates, promote the real-but-mislocated findings back into ground truth, and rescore — turning a bug-bounty report into a permanent regression test.


## Why not OWASP Benchmark, Juliet, or SARD?


Those corpora built the approach` benchmrk` extends. However, three of their original assumptions are challenged by modern tooling:


- **Shape.** Juliet and SARD are one-bug-per-line by design. Real bugs span many lines across many files; they do not exercise a vulnerability model built around one entry with multiple evidence locations.
- **Contamination.** Juliet, SARD, juice-shop, DVWA, and OWASP Benchmark are likely in every modern LLM's training set. Any LLM-based scanner will look artificially strong on them where the model has already seen the answers.
- **Age.** Juliet's last major update predates LLM-based SAST. The landscape has moved.


These classical corpora are still useful for regex tools and for regression testing inside a stable scanner. They are the wrong harness for *"does this scanner work on real code?"* in 2026. We need corpora the model hasn't graduated from, and bugs with the shape real bugs have.


Ultimately,` benchmrk` exists to be corpus-agnostic. The goal is to let you bring your own corpus and annotations so you can test your scanners against your use cases, not something curated by others.


## Caveats


**Annotations are labour.**` benchmrk` is only as effective as the corpus and annotation work that precedes it. Someone reads the codebase, identifies real vulnerabilities and intentional decoys, writes CWE accept-sets, evidence locations, criticality tiers, and gets reviewers to co-sign. Weeks per meaningful corpus, not hours. The triage-promote loop amortises; the first pass does not.


**Consensus is a feature you have to stock.**` --min-consensus` only works when multiple reviewers have signed the same annotations. The annotations used in this post were single-author today.


**Input formats.** At present` benchmrk` only supports SARIF (2.1.0 at the time of writing) and` semgrep-json` . Anything else needs a custom normaliser in` internal/normalise/` or an upstream transform.


**Out of scope.** Duration is captured but this is not specifically a speed benchmark; the question is whether the scanner is right. Knowing if it's fast is a bonus. Adversarial scanner testing (can the scanner be fooled by obfuscation?) is out of scope, because` benchmrk` scores what the scanner produces, not what it failed to consider.


### Points of degradation


- **CWE is not forever.** The industry is shifting toward MITRE ATT&CK mappings and OWASP ASVS references. Support for alternative vulnerability categorising models requires further investigation.
- **Ground-truth drift.** When a file is refactored out of existence, its annotations decay silently. This is an open problem.


## Conclusion


Scanner efficacy, across any scanner (deterministic or stochastic, commercial or open source), is a measurable property.` benchmrk` gives you the power to measure.


Two hours of wall clock and $18 in API spend provided concrete answers to questions we had about one LLM-based scanner configuration. One afternoon of triage turned a bug-bounty postmortem into a permanent test case and surfaced a gap in our visibility of CodeQL.` compare --coverage` told us that running all three prompt variants in parallel on Go-DocVault gained exactly zero recall over the best single variant alone. All three results are in SQLite files anyone on the team can re-score tomorrow.


One thing we will not do, no matter how tempting: add LLM-based matching. Determinism is the product. The day the matcher asks an LLM whether two findings are *"the same bug"* is the day the tool becomes exactly the thing it was built to measure.


Practical note:` benchmrk` fits naturally into agent workflows. Agents are good at the mechanical parts of the loop, setting up experiments, running scans, triaging findings, and re-scoring after annotation updates, while the benchmark itself stays deterministic.


---


## Trying` benchmrk` Yourself


Prerequisites: Go 1.25+,` semgrep` ,` git` .


```text
1  git clone https://github.com/block/benchmrk.git
2  cd benchmrk
3  ./examples/quick-run.sh                        # public vulnerable app
```


` JS-TodoApp` and` Go-DocVault` are purpose-built corpora chosen so the LLM-based scanners we tested would not have seen them in training. The quick-run script uses a public app instead; the workflow is the same, but the numbers will differ. To try the same process against a novel corpus ***for testing purposes*** , a simple method is to generate one on demand: ask an agent to write a CRUD app with intentional vulnerabilities and decoys plus a` benchmrk` annotation file in the` {"vulnerabilities": \[...\]}` format, including` cwes` ,` criticality` ,` status` ,` annotated_by` , and per-evidence` file` ,` line` , optional` end` ,` category` , and` severity` fields.
