---
schema_version: "1.0.0"
document_id: "7ebf37cdede8f5d6ce07e9c5d336fe8ea3b8576b7ce2e1f5dc3071be5653cb07"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/closing-the-loop-how-to-build-self-improving-ai-systems-with-automated-feedback-loops/"
published_at: "2026-07-17T17:43:22+00:00"
first_seen_at: "2026-07-20T03:32:03.151430+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:66fac4314de89320122a784dbbef1acf5a106190618e95a847f23ef53846ea8e"
---

# Closing the Loop: How to Build Self-Improving AI Systems with Automated Feedback Loops

*After six cycles, the system ran out of things to fix.* That sentence probably raises more questions than it answers. Here is how we got there.


The first skill PR took three days to merge. Twelve comments on structure. Six on conventions. The contributor fixed everything, merged, moved on. The next contributor hit the same twelve walls.


If you have reviewed enough[pull requests](https://engineering.salesforce.com/scaling-code-reviews-adapting-to-a-surge-in-ai-generated-code/) , you have probably seen this pattern. An expert writes a thoughtful review comment. That comment fixes one PR. Then the wisdom evaporates. The next person has no way to benefit from it. Multiply that across dozens of contributors and hundreds of PRs, and you have an organization that generates enormous amounts of insight and retains almost none of it.


This is not a tooling gap. It is an architectural one. The signal already exists in the review process. Nobody is collecting it, synthesizing it, or feeding it back into the AI system that produced the artifact in the first place.


##### **We Built a Generator**


The breakthrough was not realizing we needed better reviewers, rather, it was realizing that repeated review comments were not really comments anymore, they were undocumented requirements. Once the same guidance appears often enough, it no longer belongs in a human review. It belongs in the system itself.


So we did the obvious thing: we built a skill that creates other skills. We call it creating-sf-skill, a meta skill that takes a user’s intent and produces a complete skill directory: SKILL.md with frontmatter, reference files, the full structure. It encodes the conventions that reviewers kept repeating: gerund naming, progressive disclosure, scope boundaries, trigger precision, constraint formatting.


Contributors who used to take three days now merged in two days. The repeated comments didn’t vanish entirely, but nature changed. Instead of twelve comments on basic structure, reviewers could focus on genuinely subtle issues, trigger overlap with existing skills, and quality of constraint rationale.


We turned tribal knowledge into executable instructions. But a new question surfaced almost immediately: how do we know the generator itself is improving?


##### **How Do You Know Whether an AI Generator is Actually Improving?**


The generator produces structured artifacts like skill directories with frontmatter, naming conventions, file layouts, and semantic content. How do you prove that one version of the AI system is better than the last? We quickly discovered there was not a single evaluation that could answer that question. Different kinds of failures required different kinds of evidence. What we converged on is a three-tier evaluation framework where each tier catches what the others cannot.


##### **Tier 1: Trigger Accuracy**


Does the skill activate correctly in a crowded environment? Twenty queries, 10 should-trigger, 10 should-not-trigger evaluated by independent Sonnet subagents with the full skill environment loaded with over 80 competing skills active simultaneously. If the generated skill doesn’t fire correctly in a realistic environment, nothing else matters. This is the tier that catches “looks great on paper, never activates in practice.”


##### **Tier 2: Structural Validation**


Does the output pass the same mechanical checks that CI enforces? Pure determinism. The repo’s actual[validate-skills.ts](https://github.com/forcedotcom/sf-skills/blob/main/scripts/validate-skills.ts) contains 21 rules: kebab-case naming, frontmatter format, description length and word count, gerund naming convention, version format, cross-skill conflict detection. Same script, same rules, same binary pass/fail that would block a real PR. Additionally, every subdirectory file must be referenced in SKILL.md. Unreferenced files are dead weight that waste context.


##### **Tier 3: Rubric-Based LLM-as-Judge**


Does the output demonstrate semantic quality? LLM agents grade each generated SKILL.md on dimensions specific to output:


The holistic principle: use deterministic validation wherever quality can be measured objectively, and reserve LLM judgment for what requires semantic understanding. Scripts verify what is mechanically verifiable. LLMs evaluate what requires context and judgment. Neither replaces the other because they are answering different questions. Together, these layers form a production-ready AI evaluation framework, with each layer measuring a different dimension of quality.


*Timeline showing evolution from v1 to v2.*


*Bar chart comparing v1 and v2 performance metrics.*


*Radar chart comparing v1 and v2 rubric dimensions.*


With this framework we could measure whether the generator was getting better. But proving quality and actually improving quality are different problems. The generator’s instructions were still static. Reviewer comments on generated skills were still accumulating. Patterns were repeating across PRs. How do we feed that signal back in automatically?


##### **Closing the Feedback Loop**


The evaluation framework tells us how good the generator is. But the review process tells us *where* it is falling short. The gap between those two things is the feedback loop.


##### **Mining Signal**


The loop runs as a scheduled weekly automated workflow. To prevent overfitting to an isolated reviewer’s style, execution gates on a threshold: at least 15 merged skill-creation PRs, and no open PR already labeled improvement-loop. When triggered:


- Scan multiple PRs to collect inline comments
- Multi-layer filtering discards author/bot replies and CI noise
- LLM deduplicates patterns across PRs and computes priority scores based on frequency x severity


##### **Applying Improvements**


Once gaps are isolated:


- **Gap analysis** : Check whether a rule already covers the issue and enforce the instruction in the skill’s prompt and references
- **Surgical editing** : An editing agent applies changes in programmatic mode. Maximum five improvements and 100 line modifications per loop to bound blast radius
- **Validation gate** : Modified code enters the three-tier evaluation. Any regression aborts immediately
- **Transparent delivery** : Passing configurations generate a description with reference PRs and push a draft PR for human review


##### **Measuring Efficacy**


The efficacy metric is simple: we track the frequency of repeated comment patterns across cycles. A comment pattern that appeared in five PRs in one window and zero in the next is a pattern the generator has internalized. The decline in repeat comments across cycles is the direct measure of whether the loop is working.


##### **The Self-Limiting Nature**


As the generator improves:


1. Skills generated by it get fewer review comments
2. Fewer comments means less signal per cycle
3. Less signal means fewer patterns cross the frequency threshold
4. Fewer patterns means smaller (or no) changes per cycle


The system converges toward steady state. It doesn’t infinitely mutate but stabilizes. This distinguishes it from a simple “collect feedback and apply it” script. It has natural damping that converges rather than oscillates. And when conventions change, the surge in reviewer comments about the new convention provides a fresh signal that restarts the cycle exactly where needed. So does the damping actually hold? Does it actually converge?


##### **The System Learned to Stop Changing**


We ran six cycles. The system applied improvements in only two of them and self-limited in the other four which is the convergence we designed for.


**Cycle 1** : Found three high-frequency patterns from five source PRs that were all structural gaps in core SKILL.md: missing scope-boundary rules, hardcoded repository names, tool-specific language. The patterns reviewers had been writing ten times a week.


**Cycle 6** : The patterns that crossed threshold were different in character. Lower severity. Refinements to reference files: “don’t put maintainer docs in SKILL.md body,” “verify delegation targets exist before naming them.” Not core workflow changes. Cycle 6 didn’t touch SKILL.md at all but only edited peripheral files (best_practices.md, pr_checklist.md, section_guide.md). Core instructions had stabilized.


Three of the six cycles aborted at the gate check because there was not enough new signal to justify a run. The system was running out of things to fix. That is exactly what “convergence toward community standards” looks like when you measure it.


Most people assume self-improving systems mutate forever. This one did not. Early cycles made significant changes. Later cycles made smaller refinements. Eventually runs started aborting because there wasn’t enough signal to act on. The system literally began running out of things to fix.


##### **Cycle 7: The Restart**


Between cycles 6 and 7, frontmatter conventions changed. Previously valid fields were deprecated. The generator’s instructions hadn’t absorbed the change.


The result: 5 PRs flagged the same stale guidance in a single window. A frequency spike we hadn’t seen since Cycle 1.


The system responded without intervention. 34 PRs scanned, 10 patterns identified, 5 applied including corrections to the generator’s source-of-truth for frontmatter validity. No manual audit. No one noticed the drift, but the system did.


This is the restart we designed for. Standards moved. Reviewers commented on the new expectation. Comments crossed the frequency threshold. The loop corrected itself.


Convergence is not permanent. It is conditional on stable standards. When standards shift, damping lifts, the system adapts, and re-converges at the new equilibrium.


##### **A Code Reviewer That Reviews Itself**


We took the same architecture and pointed it at our automated code reviewer, a bot that reviews every skill PR. Same three moves, completely different signal.


Instead of mining human comments about skills, it mines human reactions to the bot’s own output: which suggestions got silently fixed, which ones got disputed, which issues humans caught that the bot missed entirely.


First cycle: four improvements to the reviewer’s best practices reference and severity calibration. The bot learns what humans actually care about and stops flagging what they do not. Same architecture. Different domains. The pattern held.


##### **The Pattern Behind the Pattern**


Everything we have walked through follows the same shape. The key insight: Steps two and three create a system with natural damping. As the generator improves, it produces fewer review comments, which means less signal per cycle, which means smaller changes, which means convergence. The system does not infinitely mutate but stabilizes. And when conventions change, the surge in new reviewer comments restarts the cycle exactly where it is needed.


Flowchart detailing the automated self-improvement process.


##### **What We Are Still Building**


The end state is PRs to[skills/](https://github.com/forcedotcom/sf-skills/tree/main/skills) that merge without a human reviewer. Today, low-blast-radius paths (eval datasets, plugin skills, bot configs) already merge without human intervention when they pass CI + bot review + comment resolution.


However, the core artifact directory[skills/](https://github.com/forcedotcom/sf-skills/tree/main/skills) still requires a human reviewer. The blocker is not the artifact, it is the reviewer. A human still validates the bot’s comments: accepting some, disputing others, catching what it missed. Until the bot is trustworthy enough to stand unsupervised, a human stays in the loop.


That is where the same feedback loop closes the gap. As the bot absorbs signals about what humans actually care about, its false-positive rate drops and its coverage grows. Each cycle brings it closer to the confidence threshold where it earns the right to be the final gate. When it crosses that threshold, the human reviewer becomes optional.


##### **When to Close the Loop**


This architecture is not universal as the following prerequisites must hold.


**Minimum Signal Threshold** : The feedback loop needs volume to avoid overfitting. We gate on 15 or more merged PRs before running a cycle. If your domain doesn’t generate enough review signal, perhaps only two contributors touch the output quarterly, the synthesis step will latch onto one reviewer’s personal style rather than community consensus. Rule of thumb: if you can’t collect 15+ reviewed artifacts within your intended cycle window, the automated loop will produce noise, not signal.


**Structured, Verifiable Output** : The layered eval framework requires that “correct” has a mechanical definition for at least some dimensions. If your agent produces free-form prose with no structural contract, no schema, no naming conventions, Tier 2 collapses and you are relying entirely on LLM judgment. The sweet spot: artifacts with both a structural contract (frontmatter, file layout, naming) and semantic requirements (quality of instructions, clarity of constraints).


**When the pattern breaks down** : Insufficient review culture (rubber-stamped PRs have no signal to mine). Highly subjective output (if “good” is purely taste-driven with no measurable proxies, convergence dissolves).


##### **The Broader Opportunity**


The result is tooling that does not just help contributors today, rather, it gets better every time someone participates in the process. Reviewer wisdom doesn’t evaporate into forgotten comments. It accumulates, automatically, into the system that produces the next artifact.


The architecture works anywhere you have: a generator producing structured output, a review process generating signal, and a way to mechanically verify at least some dimensions of quality. If those three conditions hold, you can close the loop.


If your team already reviews AI-generated artifacts, you may have the most valuable ingredient for a self-improving system: a continuous stream of expert feedback. The challenge is not collecting more data, rather, it is feeding that knowledge back into the system that produced the artifact in the first place.


##### **Learn More**


- Stay connected by joining our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) .
- Explore our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to see how you can get involved.
