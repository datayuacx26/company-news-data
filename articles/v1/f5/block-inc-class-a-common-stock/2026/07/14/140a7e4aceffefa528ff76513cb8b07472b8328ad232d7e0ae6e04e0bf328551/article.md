---
schema_version: "1.0.0"
document_id: "140a7e4aceffefa528ff76513cb8b07472b8328ad232d7e0ae6e04e0bf328551"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/codecrucible-a-blueprint-for-llm-driven-sast"
published_at: "2026-07-01T12:00:00+00:00"
first_seen_at: "2026-07-27T09:11:27.179122+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:a835f3552680dab48e2e9c8a7f163d7b1d024df11d43b65618ab3103e969751b"
---

# CodeCrucible: A blueprint for LLM-driven SAST

Today we're releasing[CodeCrucible](https://github.com/block/codecrucible) , a new LLM-driven static application security testing (SAST) tool for vulnerability discovery. That release is not really why this post exists. "LLM-driven SAST" is an established category, and this particular tool will probably age. What will last longer is the blueprint: the design choices, tradeoffs, and reference codebase that let other teams adapt the approach to their own pipelines and keep iterating on it. That is the part we hope other teams can reuse.


## Why this post exists


Once a scanner finds useful issues, the hard part becomes integration: how it is invoked, where it runs, and how findings surface. Those choices depend on the environment. A finished black-box tool will often need adaptation before it fits another team's environment. It leaves you working around architectural decisions you may not have made yourself. The more reusable part is the design itself: the tradeoffs, failure modes, and implementation patterns that carry across environments.


CodeCrucible itself is an example of the blueprint approach. We built the original version inside Block as a Python and Node.js prototype, using[Repomix](https://github.com/yamadashy/repomix) for repository packing, agentic analysis for vulnerability discovery, and a web UI for scan requests and reports. The tool we are releasing today is that same blueprint adapted for production use in our pipeline: a Go-native CLI with no web UI and no guided report workflow.


## How we approached the problem


### The design space


We believe that LLM-driven SAST tools have to answer four questions, and those answers usually matter more than the choice of model:


1.


**Compaction.** How do you get the codebase into the model's context window? Common options include abstract syntax tree (AST) summarization, embedding-based retrieval, agentic exploration through tool use, and finding-anchored snippet analysis. They trade coverage across the codebase for speed and token cost. Our approach aims to maximize context-window fill and finding quality, but it is not the most cost-efficient method.


2.


**Identification.** How do you ask the model to find vulnerabilities? Options range from open-ended prompts ("find the bugs") to passes targeted to specific Common Weakness Enumerations (CWEs) and self-critique loops. The tradeoff is recall versus precision versus token cost. We split this into two stages: open-ended discovery in the main pass, then CWE-specific deep analysis in audit.


3.


**Relevance screening.** How do you separate real findings from confidently stated but unsupported ones? Common answers include a second-pass LLM review, deterministic filters, taint-style verification, and human triage. The tradeoff is automation versus trust. We use a second-pass LLM review with confidence thresholds plus deterministic post-processing such as deduplication and source-file prioritization.


4.


**Determinism.** How do you make the output of a nondeterministic process usable in a pipeline when the backend is designed for consistent outputs? Options include temperature-zero generation, multi-sample voting, structured output constrained by a schema, and deterministic post-processing. The tradeoff is stability versus coverage. We use JSON Schema to constrain structured responses, with multi-tier JSON repair and deterministic cleanup after the model call.


These choices are not independent. If you choose AST summarization for compaction, you will probably make different choices everywhere else too. Laying them out together makes the tradeoffs visible before getting into the implementation.


### What we did


Whole-repo concatenation means packing the repository into a single LLM call, adding directional prompts, and letting the model reason across the whole thing. In theory it should not have worked. In practice it worked better than we expected, and it caught vulnerability classes that snippet-anchored workflows struggle to see.


Most shipping LLM-driven SAST systems put a traditional analyzer in front of the model. CodeQL, Semgrep, or a custom dataflow engine nominates candidate findings, and the LLM validates the nominated snippet. The LLM is the reviewer, not the primary analyzer. Whole-repo concatenation flips that. The LLM does the primary analysis, and you pay in tokens instead of static-analysis engineering. Both architectures have access to cross-file context in principle, but they pay for it differently.


That matters because snippet-anchored systems only show the model what the upstream engine already knows to look at. If no rule fires, or the engine fails to trace the path, the LLM never sees the code. Whole-repo analysis removes that limitation. If the code is in the packed repository, the model can reason over it whether or not a rule already exists.


It worked for three reasons we did not fully appreciate going in. First, modern reasoning models hold long code contexts better than the field assumed when the snippet-first convention took hold; a 200K-token window evaluating a 50K-token repository is operating well within its comfort range. Second, code compresses well once you strip low-value content such as binaries, lockfiles, vendored dependencies, and generated artifacts. It compresses much more than prose does, which is what most retrieval-augmented generation (RAG) systems were tuned for. Third, cross-file reasoning is one of the places where LLMs are genuinely better than deterministic SAST, so each additional file in context can add real value.


After we saw it work, we found plenty of writing that had already ruled the idea out. The clearest example is[Snyk's CodeReduce paper](https://arxiv.org/abs/2402.13291) . It uses hierarchical delta debugging on the AST to compress code by 20 to 23 times before sending it to an LLM. Its premise is that the model should see only the reported defect and the context required to understand it. Other vendors and academic groups make similar arguments, usually on three grounds: token cost at enterprise scale, hallucination on long contexts, and the fact that most code in a repository is irrelevant to any one vulnerability. Those concerns are real. We think they are engineering problems: filtering, chunking, two-pass review, and cost controls. They are not reasons to reject the architecture outright.


None of this was obvious to us at the start. The prototype was built on a hunch: older advice about aggressively restricting inputs was still being applied in a world with much larger context windows. The version that worked surprised us.


Figure 1: The scan pipeline scopes the repository, analyzes chunks, audits findings, and emits SARIF.


## Making it work at scale


The lesson is not "wait for larger context windows." It is "treat whole-repo analysis as the default, then make the compromises explicit when scale breaks it." The gap between repository size and context window is structural. Real repositories, especially monorepos, will keep outgrowing what a single call can hold comfortably. Scale is not a future feature request. It is part of the design from the beginning.


In practice, that becomes a budgeting problem before it becomes a chunking problem. You need to know how much repository content remains after filtering. You also need to account for prompt overhead, reserved output tokens, tokenizer error margin, and whether a cheap gating pass can buy some of that budget back. CodeCrucible does that before the main analysis pass. It estimates worst-case prompt overhead, skips feature detection when a single chunk still fits, reruns the overhead calculation when feature detection trims the prompt, and only then computes the chunk budget. For the reference implementation, see[internal/cli/scan.go:356-555](https://github.com/block/codecrucible/blob/main/internal/cli/scan.go#L356-L555) .


### Chunking


Our chunking strategy is to chunk as late as possible and as semantically as possible. If the repository still fits, do not chunk it. If it does not, keep file boundaries intact and related files together. Duplicate a little shared context when it buys clarity, and merge undersized chunks so you do not pay prompt overhead more times than necessary. Chunking is a fallback strategy, not the core idea.


That is the shape CodeCrucible follows. It resolves local imports first and takes a single-chunk fast path when the fully flattened repository already fits. It scores files, duplicates a few small high-priority shared files, grows chunks by local import graph where possible, and falls back to directory proximity when it cannot. It also merges small chunks afterward and carries summaries of related files that landed elsewhere. Importantly, it never slices files to make the budget work. If a file is too large, it gets skipped with an explicit warning. The point is not the exact scoring function. Chunking should preserve related code and trust-boundary context rather than merely satisfy a token ceiling. See[internal/ingest/imports.go:147-170](https://github.com/block/codecrucible/blob/main/internal/ingest/imports.go#L147-L170) and[internal/chunk/chunker.go:64-398](https://github.com/block/codecrucible/blob/main/internal/chunk/chunker.go#L64-L398) .


Figure 2: The chunker keeps a single-chunk fast path, then groups larger repositories by priority, imports, and directory proximity.


### Filtering and prompts


Filter in layers before you make architectural decisions. Token budget is the scarce resource. If you spend it on symlinks, binaries, vendored dependencies, CI metadata, notebooks, lockfiles, and test scaffolding, the scanner will look comprehensive while seeing less of the code that matters. In CodeCrucible, filtering starts at the filesystem walk, which honors` .gitignore` and skips obvious non-source content, then continues with heuristic filtering for tests, documentation, vendor code libraries, low-value operational files, explicit includes and excludes, and large files. The point is not the exact exclusion list. It is that the scanner makes the repository smaller before it decides whether chunking is necessary, how much the scan will cost, or what prompt shape to use.


Filtering also has to preserve structure, not only remove bytes. Some small files matter disproportionately because they define trust boundaries: schema files, API contracts, query definitions. In CodeCrucible this manifests as always-include handling for` .proto` ,` .sql` , and GraphQL files, even when they sit in places that would otherwise be filtered out, plus a[local import graph](https://github.com/block/codecrucible/blob/main/internal/ingest/imports.go#L147-L170) and[one-line export summaries](https://github.com/block/codecrucible/blob/main/internal/cli/scan_ingest.go#L98-L166) once whole-repo analysis stops fitting. Import graphs matter here because they help preserve related code once the repository no longer fits in one call.


Prompts need the same discipline. Rather than relying on a single universal prompt, split prompts by phase and make each phase earn its tokens. CodeCrucible has separate prompts for feature detection, main analysis, audit, CWE-specific deep review, and optional context compression. On smaller repositories the analysis prompt can include every section. On larger ones the tool first runs a cheap feature-detection pass over the manifest and representative manifests or entrypoints, then includes only the sections that match the codebase. That lightweight pass buys prompt budget back for the expensive pass.


One useful surprise was that elaborate prompt scaffolding mattered less than we expected. In recent experiments, our heavily steered prompt sets did not materially outperform a much shorter[Carlini-style CTF prompt](https://www.youtube.com/watch?v=1sd26pWhfmg) that frames the task as a direct "find the bugs" security challenge. The shorter version was cheaper and faster. That is a useful blueprint lesson in its own right: prompt complexity is not the same thing as prompt quality, and every extra section has to justify its token cost.


For the reference implementation, we left those prompt sets in the repository rather than treating them as temporary tuning files. The original detailed prompts are preserved in[prompts/default/](https://github.com/block/codecrucible/tree/main/prompts/default) , the shorter Carlini-style set lives in[prompts/carlini/](https://github.com/block/codecrucible/tree/main/prompts/carlini) , and a slightly modified exploit-focused variant lives in[prompts/exploit-proof/](https://github.com/block/codecrucible/tree/main/prompts/exploit-proof) . Although these references are provided and work out of the box, experimenting with prompt sets is cheap. Consider[benchmrk](https://github.com/block/benchmrk) for[this purpose](https://engineering.block.xyz/blog/proof-not-promises-evaluating-code-scanner-efficacy) .


Once chunking enters the picture, prompts have to compensate for the loss of global visibility. In CodeCrucible that means telling the model which chunk it is reviewing, showing it a limited view of the rest of the repository, carrying over related context when configured, and attaching short summaries of related files that landed in other chunks. This is practical work required to preserve the original design goal - whole-repo reasoning - as the repository outgrows a single call. Filtering and prompting are really the same problem from two angles: deciding which bytes get to represent the repository inside the model.


Any tool in this category also needs a budget gate. Once cost scales with token volume, "scan the repo" stops being a harmless command. In CodeCrucible, cost is estimated after filtering and token counting, surfaced through` --dry-run` , and checked against` --max-cost` before any model call is made, with a conservative default threshold in place unless the operator opts out. That is not the detection architecture, but it is part of making the tool safe to operate. For the reference implementation, see[internal/ingest/walker.go:185-307](https://github.com/block/codecrucible/blob/main/internal/ingest/walker.go#L185-L307) ,[internal/ingest/filter.go:175-296](https://github.com/block/codecrucible/blob/main/internal/ingest/filter.go#L175-L296) ,[internal/cli/scan.go:233-321](https://github.com/block/codecrucible/blob/main/internal/cli/scan.go#L233-L321) ,[internal/cli/scan_analyze.go:283-375](https://github.com/block/codecrucible/blob/main/internal/cli/scan_analyze.go#L283-L375) , and[internal/llm/prompt.go:174-404](https://github.com/block/codecrucible/blob/main/internal/llm/prompt.go#L174-L404) .


Figure 3: The analysis phase assembles prompts, repairs malformed JSON, handles overflow, and post-processes merged SARIF. Figure 4: The budget gate filters the repository, estimates scan cost, and runs feature detection only when chunking is needed.


### Quality gates


The scanner becomes useful only when the quality gates are at least as serious as the scanning pass. Without them, you get a lot of findings and very little trust. The predictable failure mode is a recall-tuned first pass that reports the same underlying issue multiple times, describes secondary manifestations as separate defects, and emits output in enough different shapes to make pipeline integration difficult. A scanner is only useful if people trust it enough to keep it turned on.


Deduplication is the first gate, and near-duplicates are the real problem. The same SQL injection can show up from multiple chunks with different prose, and the same missing authorization check can be reported once per endpoint instead of once per resource type. CodeCrucible handles this in stages: merge chunk outputs, remove exact repeats, then key findings by location and CWE and keep the highest-severity instance. It also drops low-severity findings in non-source files and cleans up stale Static Analysis Results Interchange Format (SARIF) metadata. The goal is to collapse symptoms back into causes before a human sees the result.


The second gate is a dedicated validation review. A single "please validate these findings" prompt is usually too generic to be useful. CodeCrucible batches findings, carries forward the specific source files they mention, and injects the matching CWE-specific validation guidance. That separation of jobs matters. The first pass can be generous because the second is explicitly precision-oriented. In the default prompts, the auditor is told to assume the first pass may over-report, prove reachability and lack of mitigation, collapse duplicate manifestations to one root cause, and, when tests are excluded, prove production reachability before doing anything else.


The final gate is output discipline. If the results are not stable enough to land in SARIF and show up cleanly in code scanning, the rest of the pipeline does not matter. CodeCrucible uses strict structured output, a repair ladder when the model drifts, explicit failure notifications when a chunk still cannot be parsed, and a SARIF rebuild step so severities, snippets, and CWE links stay aligned with the findings that survived review. Quality gates have to shape the output into something trustworthy in production. Otherwise the scanner is producing text, and text is the easiest part of the problem. See[internal/sarif/merge.go:5-167](https://github.com/block/codecrucible/blob/main/internal/sarif/merge.go#L5-L167) ,[internal/sarif/postprocess.go:35-181](https://github.com/block/codecrucible/blob/main/internal/sarif/postprocess.go#L35-L181) ,[internal/cli/scan_audit.go:58-493](https://github.com/block/codecrucible/blob/main/internal/cli/scan_audit.go#L58-L493) ,[internal/cli/scan_analyze.go:192-280](https://github.com/block/codecrucible/blob/main/internal/cli/scan_analyze.go#L192-L280) , and[internal/llm/schema.go:8-282](https://github.com/block/codecrucible/blob/main/internal/llm/schema.go#L8-L282) .


Figure 5: The audit phase batches deduplicated findings, applies CWE-specific review, and rebuilds the final SARIF output.


## What it found: Copy Fail case study


We have identified internal bugs with CodeCrucible, but those examples are not useful case studies because readers cannot reproduce them without access to our application code. The recent[Copy Fail](https://copy.fail/) bug (CVE-2026-31431) in the Linux Crypto subsystem gave us a public test case for a high-impact AI-discovered vulnerability.


### The initial test: failing at copying Copy Fail


To begin testing, we used the most direct method before adding reasoning from the published vulnerability discovery process.


We cloned a Linux commit from before[the patch](https://github.com/torvalds/linux/commit/a664bf3d603d) , set up CodeCrucible with GPT 5.5 Cyber and Claude Opus 4.7, and wrote C-specific prompts. Then we pointed it at the crypto directory and ran a scan.


After multiple attempts, we concluded that our harness could not detect this vulnerability on its own.


So was that the end of the test?


### The re-test: unfailing at Copy Fail


Not quite. Our first attempt did not follow the method the Xint researchers used. In their blog post, they described an operator prompt that gave the model a "steer":


*"In this case, the operator prompt was quite simple:*


*This is the linux crypto/ subsystem. Please examine all codepaths reachable from userspace syscalls. Note one key observation: splice() can deliver page-cache references of read-only files (including setuid binaries) to crypto TX scatterlists." ([https://xint.io/blog/copy-fail-linux-distributions](https://xint.io/blog/copy-fail-linux-distributions) )*


We replicated the test using the same steering[prompt as the Xint team](https://xint.io/blog/copy-fail-linux-distributions) and added[kernel exploitation guidance](https://github.com/block/codecrucible/tree/main/prompts/exploit-proof-c-kernel) .


```text
shell    1  ./codecrucible scan   ..  /linux/crypto --provider databricks --model gpt-5.5-cyber --output results.sarif --custom-requirements   "This is the linux crypto/ subsystem. Please examine all codepaths reachable from userspace syscalls. Note one key observation: splice() can deliver page-cache references of read-only files (including setuid binaries) to crypto TX scatterlists."   --prompts-dir prompts/exploit-proof-c-kernel
```


Seven minutes and $9 in token spend later (note: costs and times may vary), we had a SARIF file with the following finding at the top of the results (edited for readability).


```text
yaml    1  Finding  :   AF_ALG accepts read  -  only pages into TX scatterlists without validating writability
2   Summary  :     |
3    In the MSG_SPLICE_PAGES path of af_alg_sendmsg(), pages from the attacker-controlled msg->msg_iter are passed directly into a scatterlist via extract_iter_to_sg() (af_alg.c:1058-1059). The only subsequent processing is a get_page() reference increment for each extracted page (af_alg.c:1065-1066). No validation is performed to ensure the pages are writable, anonymous, private, or otherwise safe for later use in crypto scatterlists. There are no checks that would break COW, require FOLL_WRITE, reject page-cache-backed pages, or copy data into private kernel-owned pages before the pages are retained in ctx->tsgl_list.
4    The review also identified a propagation path through af_alg_pull_tsgl() (af_alg.c:575-580), which can reassign these retained TX pages into a caller-provided scatterlist using get_page(page) and sg_set_page(...). This demonstrates that splice-provided pages can be propagated from the AF_ALG send path into downstream crypto request scatterlists.
5    The reviewed code therefore supports the source-side defect: AF_ALG accepts MSG_SPLICE_PAGES pages and retains them without validating whether they are writable or appropriate for later use as crypto buffers. However, the final exploitation claim-that crypto operations can subsequently write into read-only page-cache pages and modify file-backed cache contents-cannot be confirmed from this source file alone. Verification would require analysis of the AF_ALG skcipher receive path and downstream crypto implementations, which are outside the scope of the reviewed code. As a result, the page-propagation primitive is supported, while the downstream writable-page-cache corruption impact remains unverified.
6   Confidence  :   78%
7   Location  :   af_alg.c  :  1058  -  1066
```


It did not capture the full exploit chain because it missed the exploitable sink and the 4-byte write primitive. Even so, it is the kind of finding that could lead a skilled security researcher or agentic pipeline to the full attack sequence.


### Limitations to the approach


The first limitation is that whole-repo reasoning degrades once the repository no longer fits comfortably in one call. At that point CodeCrucible has to rely on feature detection, chunking, import-graph heuristics, and cross-chunk summaries to preserve enough related code for the model to reason about. That works better than naive chunking, but it is still a heuristic substitute for true global context, and it works better for some repository shapes and languages than others.


The second limitation is that coverage is intentionally shaped by filtering. We respect` .gitignore` , skip tests and docs by default, exclude vendor SDKs and low-value operational files, and drop oversized files rather than slicing them to fit. That keeps cost under control and improves average signal, but it also means CodeCrucible is not trying to be a literal model of everything in the tree. Security-relevant context can still hide in excluded build wiring, generated code, or large files that were not worth the token budget.


The third limitation is that the pipeline is not iterative. The main analysis pass gets one shot to surface candidate findings, and the audit phase is scoped to reviewing those findings in batches with only the files they already mention. That makes the system cheaper, faster, and easier to operationalize, but it also means recall is bounded by what the first pass notices and precision is bounded by what that batch-local audit can prove.


Fourth, the current tool is still mostly code-centric. Even with a dedicated audit pass, it does not validate runtime or environmental controls unless you explicitly feed that context in. In practice that means some findings are true in code but not exploitable in the deployed system because a mitigation lives elsewhere. If you are adapting this pattern for your own environment, the most promising place to improve precision is probably not a more elaborate discovery prompt, but better audit-time context from the systems that actually enforce those controls.


Finally, this approach is also limited by the same constraints as all other AI-centric scanners vs. their deterministic counterparts. Namely, there is no built-in way to rerun scans repeatedly at scale, and there is no solution for deduplication of findings between scans, especially given the scanner will format the same finding differently each scan. These problems, we believe, are shared across this class of scanner, and we aim to share how we are approaching those limitations in future blogs.


## Conclusion


Getting a scanner to run once is easier than fitting it into a real remediation workflow. A scanner that does not fit your environment will remain difficult to adopt no matter how impressive its findings look in a benchmark. That is why we are publishing the tradeoffs, the approach, and the reference codebase rather than treating the binary itself as the main point.


AI security tooling is in the phase where many teams are privately building close variants of the same thing, and the differences that matter most are operational: your iteration speed, how well you tune prompts and filters to your codebase, how safely you bound cost, and how tightly the output fits your remediation workflow.


We believe publishing the blueprint raises the floor without lowering anyone's ceiling.


[Here is the codebase](https://github.com/block/codecrucible) , here is the argument, and here are the rough edges we hit while making it work. Take what fits your stack. The version you should build will not look exactly like ours and that's the point.


## Credits


Special thanks to Hunter Stanton for his technical contributions to this project ([GitHub](https://github.com/HunterStanton) ,[X](https://x.com/hun10sta) )
