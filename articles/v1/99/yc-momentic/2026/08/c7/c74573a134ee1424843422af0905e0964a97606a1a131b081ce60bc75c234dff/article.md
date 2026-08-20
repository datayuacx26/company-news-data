---
schema_version: "1.0.0"
document_id: "c74573a134ee1424843422af0905e0964a97606a1a131b081ce60bc75c234dff"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/the-harness-will-not-save-you"
published_at: "2026-08-06T21:58:00+00:00"
first_seen_at: "2026-08-07T06:38:48.799299+00:00"
fetched_at: "2026-08-07T06:38:49.826762+00:00"
content_hash: "sha256:40972ec9027ff179879d2294468497b1f9ac11edd50b72387bea8837f4f721bb"
---

# The Harness Will Not Save You

Models will make mistakes. Right now, Fable, K3, Sol – all of them – do stuff wrong. It is inevitable.


Model makers work on reducing that mistake space. But developers also have to work to reduce the mistake space around the model. Better guardrails, better context, better tools, better sandboxes, better checks the agent runs before it calls the work done. All of that is the harness, and it works. The mistake rate really does go down.


But the harness will not save you. Harnesses build better code; they don't build a better product. To build a better product, you have to work with the model and the harness on verification.


## What is a Harness?


Rules and walls.


The rules are everything the agent is told:


- The conventions in CLAUDE.md are rules.
- The prompt templates and plan formats are rules.
- The instruction to run the test suite before opening a PR is a rule.
- Even tool descriptions are rules about when and how to act.


The rest of a harness is walls. A sandbox doesn't ask the agent to stay inside it; there's nothing outside it to reach. Permission gates, egress controls, read-only mounts, and protected branches work the same way. They hold whether or not the model cooperates, which makes them the most dependable part of the whole setup.


[Harness engineering](https://openai.com/index/harness-engineering/) is the work of improving both halves. Neither half examines the thing the agent produced. A rule can't confirm it was followed, and a wall can only tell you what didn't happen.


[Endor Labs put numbers on all of this](https://www.endorlabs.com/learn/gpt-5-5-sets-a-new-code-security-record-with-cursor-not-codex-in-agent-security-league) by running the same models through two different harnesses in the same week:


- GPT-5.5 scored 61.5% on functional correctness in its own Codex harness and 87.2% in Cursor's, a 25.7-point swing from the harness alone
- Opus 4.7 went from 87.2% in Claude Code to 91.1% in Cursor


Even the best pairing still failed roughly one task in eleven. And the only reason anyone knows that rate is that the benchmark grades the work from outside the harness. Production has no equivalent grader unless you build one.


## The Harness's Relationship with Testing


Ask a team where verification lives in their setup, and they'll usually point at the harness. The rules file says to write tests and run them, the agent does, and the PR arrives green. Testing gets handled as one more rule, so it gets the same treatment as every other input.


[Anthropic’s guidance on verification loops](https://claude.com/blog/building-verification-loops-in-claude-code-with-skills) defines the pattern as Claude checking its own work and fixing what fails, and shows how to encode your manual checks as skills so the agent closes its own feedback loop. There's real tooling behind it:


- A /verify skill builds and runs the app to observe the changes.
- Spec validation checks each change against a markdown spec in the repo.
- Rubric graders run as a separate agent and loop failures back for rework.


The Claude Code team chains them in its own work: code review, then simplify, then verify, then a design check. Every check is instructions the agent reads. The spec sits in the repo where the producing agent can edit it. The rubric is visible to the side being graded.


The important part is this: **the loop is closed by the thing the loop is checking** .


Why that matters starts at training time. Coding models are reinforcement-trained against pass/fail test rewards, and graders already discard agents' edits to test files because models comment out failing tests and splice in mocks. The reward is passing the visible check. Nothing in it asks whether a user can complete the workflow.


The same behavior shows up at inference.[SpecBench](https://arxiv.org/abs/2605.21384) found every frontier model saturates the test suite it can see, while the gap against held-out end-to-end tests widens with scale, roughly 28 points for every tenfold increase in code size. One agent submitted a 2,900-line compiler that memorized the test inputs.


Models align with incentives. If the incentive is passing the test, a capable model has many avenues for that, and writing correct code is only one of them. Just look at what just happened with an unreleased OpenAI model[escaping its sandbox and breaking into Hugging Face's production database](https://huggingface.co/blog/agent-intrusion-technical-timeline) for the answer key. Models align with incentives.


A green suite the agent wrote, ran, and reported tells you the agent satisfied its checks. Whether the software works is a separate question, and answering it takes an oracle that sits outside the agent and that the agent can't edit. Everything in the harness sits inside.


## Build the verification layer yourself


The gap is already showing up in production:


- [Faros AI's report](https://www.faros.ai/research/ai-acceleration-whiplash) on the months since coding agents went mainstream shows monthly incidents up nearly 58%, bugs per developer up 54%, and roughly a third of PRs merging with no review at all.
- A[May 2026 CloudBees study](https://www.cloudbees.com/lp/2026-state-of-code-abundance-report) found 81% of enterprise technology leaders reporting a direct increase in production issues tied to AI-generated code.


Those are correctness failures, the exact thing all those green checks were supposed to catch. Verification has to be built as its own layer, with three properties no harness provides:


- **It's independent** . The checks are authored and run outside the coding agent, and a separate grader doesn't clear the bar if it reads a rubric the producing side can see and edit.
- **It's behavioral** . It exercises the running software the way a user would and judges the outcome rather than the diff.
- **It's in the loop** . It gates every merge at the speed agents ship, because the mistakes arrive continuously now.


The execution of this should be fully agentic.[Agents author the tests](https://momentic.ai/blog/agentic-testing-guide) from plain-language intent, and on every PR an exploration agent reads the diff and adds or updates coverage for the flows that changed. The runs then happen against the preview deployment, and the checks judge outcomes through the signals a user's browser produces: the DOM, the accessibility tree, screenshots, network traffic, the console. Finally, failures get triaged by an agent into real bugs, intentional changes, setup problems, or flakes. It repairs the tests behind intentional changes and quarantines the flaky ones.


Nobody writes scripts, nobody maintains them, nobody manually tests. The only failures that reach a person are the ones classified as real.


The judgment stays with the humans:


- You state the intent: what the product is supposed to do, in plain language, before any agent turns it into a test.
- You keep the[knowledge base](https://momentic.ai/docs/ai/knowledge-base) that records how the product is supposed to behave and what counts as a bug versus an intentional change.
- You make the final call on what verification finds and whether a change ships.


That definition is the oracle every check runs against, and it belongs to the team rather than the agent being graded. Most teams got the order backward. They automated the building and left verification to the builder. Build the layer, hand it your definition of correct, and then let the agents run.


The harness is worth every bit of investment it's getting, and it will keep improving. It embeds rules. Verification is something you have to build.


If you want to see what that layer looks like on your own app, the[setup wizard](https://momentic.ai/docs/quickstart/web) gets you from install to a passing test in a few minutes: run` npx @momentic/wizard@latest` in your project. Or[contact the team](https://momentic.ai/sales) to walk through it.
