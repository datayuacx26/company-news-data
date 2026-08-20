---
schema_version: "1.0.0"
document_id: "5a39adf05700b49bf982ba16ff093a135a1af6f22e01ab6e0f898c55be1b3da6"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-5eeb9d1ed7ff"
canonical_url: "https://devblogs.microsoft.com/blog/building-ax-evals-that-actually-work/"
published_at: "2026-07-15T12:53:13+00:00"
first_seen_at: "2026-07-20T04:34:25.944233+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:a418894098dc242f05d5f37b6df31c13f735cad30deccb42f6e2d0a5cdef0f2a"
---

# Building AX evals that actually work

*This is the eighth and final article in a series about Agent Experience (AX): the practice of making AI coding agents work correctly with your technology. The series covers what you can and can’t control in the agent stack, how to measure whether your extensions are helping or hurting, and how to iterate toward better outcomes.*


You’ve read seven articles about what to measure, why benchmarks don’t transfer, and what hidden variables can do to your results. Now you actually have to build the thing. Most teams build an eval, run it, get scores, and feel good about the numbers. The trouble is that these evals produce confident, consistent, and unfortunately meaningless results. Contaminated data, scenarios that don’t represent real usage, criteria that check the wrong thing, and scores that go up while developer experience stays flat.


Why does this matter? Because evals are means to an end. You evaluate to find where models have gaps in using your technology, so you can fill those gaps and improve the agent experience for your users. The eval tells you what to fix. If it tells you the wrong thing, you spend weeks optimizing something that doesn’t matter while the real problems stay invisible. Getting accurate signal from your eval is the difference between fixing the right thing and wasting your time.


In the[previous article](https://developer.microsoft.com/blog/the-hidden-variables-in-your-agent-eval) , we covered the environmental factors that silently steer results. This article is about the structural decisions that determine whether your eval produces signal or noise.


## What a solid eval requires


A solid eval is a combination of six things. Miss any one of them and you get scores that look right but mean nothing.


1. **Representative prompts.** The scenario instruction should be something a developer would reasonably type into a coding agent. Nothing more. No evaluation metadata, no scoring rubric mixed in. How specific the prompt is depends on what you’re measuring: a vague prompt tests discovery (propensity), a prompt that names the library tests quality (efficacy).
2. **Accurate criteria.** Your evaluation criteria must be capable of deciding whether the agent’s output correctly reflects the task. That means checking for correct *usage* , not just presence. And it means verifying that the code actually works, not just that it looks right.
3. **Unambiguous criteria.** The same criterion, evaluated against the same output multiple times, should produce the same verdict every time. If it doesn’t, you’re measuring judge variance, not agent quality.
4. **Multiple runs.** Wherever you use LLMs, whether for coding or judging, you’re subject to variance. A single run can be an outlier. Run each scenario at least 5 times to establish whether a result is signal or noise.
5. **A clean environment.** User names, workspace paths, directory names, and other semantic hints in the eval environment can skew agent behavior in ways that have nothing to do with your extension. Article 7 covered this in depth. Your eval must control for it.
6. **A representative environment.** If the majority of your users are on Windows and you’re testing on Linux, your results and the optimizations you derive from them might not be relevant to the majority of your audience.


## Anatomy of a reliable criterion


Most criteria are written like this: “The code should use proper error handling.” Two reviewers would score that differently because neither “proper” nor “error handling” has a shared definition. You’ve defined what you’re checking, but not what passes or what fails.


Compare that with: “All API calls to external services are wrapped in try-catch blocks that handle network errors separately from application errors. Network errors retry with exponential backoff. Application errors propagate to the caller with the original error code preserved.” Two reviewers scoring against that criterion will agree, because there’s nothing left to interpret.


Writing criteria at this level takes longer. You’ll feel the urge to write “uses proper error handling” and move on. Resist it. Vague criteria don’t just produce inconsistent scores. They produce scores that *look* consistent while meaning different things across runs. You trust them, and that trust is what makes them dangerous.


### Checking usage, not presence


Your criteria need to verify correct *usage* , not just that a name appears somewhere in the output. And static analysis alone isn’t enough: you need to build and run the project to know whether the code actually works. We covered both of these failure modes in article 3’s[common measurement mistakes](https://developer.microsoft.com/blog/is-your-agent-extension-actually-working) , and they remain the most frequent source of false confidence we see in many evals.


### The skip condition


Not every criterion applies to every scenario output. If you’re checking whether the code implements authentication correctly, but the scenario didn’t produce any authentication code (maybe the agent solved the problem differently), forcing a pass/fail verdict corrupts your data. You need a third state: skipped.


The skip condition defines when a criterion doesn’t apply. “Skipped: no authentication flow is present in the generated code.” This keeps your scoring honest. Without it, your judge has to make a call on code that doesn’t exist, and that call will be inconsistent.


The balance is important though. Skip conditions that trigger too often leave you with sparse data, and criteria that never skip force verdicts on code that isn’t relevant to what you’re measuring. Design skip conditions for the cases where evaluating would be meaningless, not for cases where evaluating would be hard.


## Writing criteria for LLM judges


Writing precise criteria is harder than it sounds, and not for technical reasons. People write vague checks because that’s how they think. They approach criteria from their own mental model, using their own knowledge as context, and assume the model understands what they mean. “Uses the SDK correctly” feels complete when you know what correct usage looks like. The problem is that the model might not.


For most teams, AX isn’t the job. They’re building a product, and AX is one more thing they need to take care of to ensure adoption, the same way they used to care about DX. Asking them to write detailed, structured evaluation criteria with explicit pass/fail/skip conditions feels like a heavy ask on top of everything else. But vague criteria don’t save time. They produce scores you can’t act on, which means you spend the time anyway, just later, debugging why your “improvements” didn’t improve anything.


There’s a deeper problem with LLM judges specifically. You’re using an LLM to write code, then asking another LLM to judge that code. If the coding model has knowledge gaps about your technology, it produces incomplete or incorrect output. But the judge model likely has the same gaps. It can’t catch what’s wrong if it doesn’t know what right looks like either. Vague criteria like “uses the SDK correctly” let the judge fill in its own (partial) understanding of “correctly,” and a model with incomplete knowledge will score incomplete code as correct. The result: great scores, broken code. Your criteria must be specific enough to fill the judge’s knowledge gaps. The pass/fail conditions need to spell out exactly what correct usage looks like so the judge doesn’t need domain expertise it doesn’t have.


The same applies if you use an LLM to write your evaluation criteria. If the model doesn’t know your technology, it can’t write criteria that check for correct usage of it. It’ll produce plausible-sounding checks based on whatever it thinks your SDK resembles, and those checks will pass code that uses the wrong patterns. You need domain knowledge to write criteria, whether that knowledge comes from a human or from an LLM that’s been given enough context to understand what correct looks like. For proprietary technology, that almost always means a human writes the criteria, or at minimum reviews and corrects what an LLM drafts.


This also determines your model choice. The weaker the judge model, the more detailed your evaluation instructions need to be to compensate. Why would you use a cheaper model? Because you’ll be running it thousands of times across scenarios and runs, and you might not need full reasoning capability for every criterion. But the model does need solid domain understanding to score accurately. A smaller model with explicit, gap-filling criteria can match a frontier model with vague ones, at a fraction of the cost. The tradeoff is in criteria authoring effort, not in score quality.


What works is a checklist of concrete checks, each with its own pass, fail, and skip conditions. Precise enough for the judge to score unambiguously, but still scannable when your team reviews it in a PR.


```text
## Secure credential handling


Check that the application handles credentials securely.


- API keys are not hardcoded in source files
- skipped: Application does not use API keys
- passes: Keys are loaded from environment variables, config files
excluded from version control, or a secrets manager
- fails: Keys appear as string literals in source code


- Secrets are not logged or exposed in error messages
- skipped: No logging or error handling present
- passes: Log statements and error messages do not include
credential values, connection strings, or tokens
- fails: Any secret value appears in a log statement,
error message, or console output


```


Each check is independently scorable. The judge evaluates them one at a time and reports which passed, which failed, and which were skipped. You get granular signal instead of a single opaque score for the whole criterion.


### Calibrating the judge


Writing criteria isn’t enough. You need to verify that the judge interprets them the way you intend. Calibration checks two things: accuracy (does the judge get the right answer?) and consistency (does it get the same answer every time?).


For accuracy, take 5-10 outputs where you already know the correct verdict (you scored them manually), run the judge against them, and check whether it agrees. When the judge disagrees with your manual verdict, the criterion is almost always ambiguous. Tighten the wording, run calibration again, iterate until the judge matches your intent on all calibration cases. One important detail: calibrate against real agent output, not hand-crafted examples. Agents do surprising things that your test cases won’t anticipate. They put authentication in a middleware instead of inline. They split one file into three. They use a library you didn’t expect. Those surprises are exactly what your criteria need to handle.


For consistency, run the judge against the same output multiple times. Even with temperature set to zero, you can get variation if your criteria are ambiguous. A criterion that flips between pass and fail on identical input isn’t reliable enough to measure with. This is especially common with smaller models: they’re more sensitive to ambiguous wording and more likely to produce inconsistent verdicts on the same input. If the judge is inconsistent, the criterion needs to be more specific, not the model more capable.


How often should you calibrate? Every time you add or modify a criterion. Every time you change the judge model. Every time you change how you present the code to the judge. And every time the harness changes: if you’re using your own, that means system prompt updates; if you’re using an off-the-shelf one, that means harness version upgrades, because either can change how context gets assembled and what the judge sees.


### Judge failure modes


To give you an example, we hit two issues while building our own evaluation platform. Each one cost us a week of bad data before we figured out what was happening.


**Semver reasoning.** We wanted to check whether generated projects used recent SDK versions by verifying the latest major and minor versions (two checks, scored independently). Our mini-class judge model couldn’t reason about semver. It would discover the dependencies and their versions from the generated code, but couldn’t reliably determine whether the used version matched the latest major or minor. Semver comparison requires understanding caret ranges, tilde ranges, and pre-release semantics, and no amount of criteria rewording made the judge reliable at it. We ended up building a custom tool that takes the discovered version and compares it against the latest available version. The judge identifies what’s installed, the tool does the semver comparison, and the judge gets back a simple` true` or` false` .


**Score calculation.** Early on, we asked the judge to calculate aggregate scores following a simple formula. We’d get scores that looked plausible, but on closer inspection, they were wrong, and debugging meant re-reading judge output to figure out where the arithmetic went sideways. We solved this by narrowing what we asked the judge to do: return a pass, fail, or skip verdict for each individual check, along with the supporting evidence. Nothing else. We calculated the aggregate scores deterministically in code. The judge got more consistent at the one thing we still asked of it (making a judgment call), and we stopped debugging arithmetic errors entirely.


## Statistical rigor without a statistics degree


LLMs are non-deterministic. The same prompt, same model, same extension can produce different output on every run. One run tells you almost nothing about true performance, and you need multiple runs to know whether a result is real or an outlier.


### How many runs do you need?


In[our experience](https://developer.microsoft.com/blog/is-your-agent-extension-actually-working) , 5 runs per scenario is where results start collapsing into a stable pattern. You’ll get the occasional outlier, but 5 runs is enough to see whether a scenario reliably passes, reliably fails, or is genuinely unstable. For most decisions (should we ship this change? is this extension helping?), that’s sufficient signal.


How noisy are your scenarios? A scenario that passes 5/5 with your extension and 1/5 without it doesn’t need sophisticated statistics. But 3/5 with and 2/5 without? That’s ambiguous. Run a few more to see if the pattern holds, or accept that the effect is small enough that the scenario might not be the right one to measure it.


### Variance tells you something


High variance in a scenario isn’t just a statistical inconvenience. It’s information. If the same scenario passes 90% of the time with your extension and 50% of the time without, you’ve found a case where your extension reliably helps. If it passes 70% of the time with your extension and also 70% of the time without, the variance is telling you the extension isn’t doing anything for this specific task.


But there’s a subtler case. If a scenario passes 90% with your extension on Monday and 50% with your extension on Friday, something changed in the environment. Could be a model update. Could be a harness change or an extension that auto-updated beneath you without logging it. High *within-condition* variance over time is a signal that you have an uncontrolled variable from article 7, not that your extension is inconsistent.


Track run-over-run variance separately from between-condition variance. Run-over-run variance reveals stability problems. Between-condition variance is where you find the actual impact of your extension. You want both numbers, because they answer different questions.


## Keep it practical


When you consider everything that influences agent output, the permutations are endless. Different models, harnesses, extensions, operating systems, not to mention variations in how developers phrase the same request. You could try and test all of them, producing more data than you could ever analyze, and you’d never ship an improvement because you’d still be running scenarios and analyzing the results.


If you’re reading this, you’re likely not doing theoretical research. You’re using evals to improve how developers use your technology through AI coding agents. Start from where the majority of your users are and fix AX for them first. Pick the model and harness most of your developers use, run on the OS they run, and write prompts that reflect their most common tasks. That’s your first eval. Expand later if the data tells you to.


### Prompt design


Whether you’re testing discovery of your technology or its application, one rule is absolute: the scenario instruction should contain *only* the developer prompt. No evaluation criteria mixed in, no scoring rubric alongside the task, no metadata about what you’re testing. We’ve seen folks embed their rubric in the same prompt they send to the agent, combining the instruction with the scoring criteria. That creates interference you can’t measure. The agent sees the rubric, adjusts its behavior to match it, and you end up evaluating whether the agent can read your grading sheet, not whether it can do the task. Keep the prompt and the rubric completely separate.


### Workspace fidelity


An empty workspace tests from-scratch generation. A realistic workspace tests integration into existing code. Both are valid measurements of different things, but mixing them in a single eval run produces confusing results because the agent behaves fundamentally differently in each context.


Group your scenarios by workspace type and analyze results separately. “Our extension lifts from-scratch scenarios by 20% and integration scenarios by 8%” is more actionable than “our extension improves scenarios by 14% on average.” The average hides the fact that you’re great at one thing and mediocre at another.


## The environment you run in


Article 7 covered hidden variables in depth. Two of them matter enough to repeat here because they’re requirements, not nice-to-haves.


### Clean environments


Your eval environment should be free of semantic hints that could influence the agent’s decisions. A workspace path like` /home/azureuser/test-workspace/poc-api` leaks three signals: a cloud provider preference, a “this is a test” framing, and a “this is a proof of concept” quality expectation. Each one can nudge the agent’s behavior in ways that have nothing to do with your extension.


Run your evals from a semantically neutral path. Use a generic user identity. Don’t name the workspace after the thing you’re testing. The goal is to ensure that the environment doesn’t influence your results.


### Representative environments


The environment your eval runs in should match where your users actually work. This sounds obvious, but the default choice is almost always wrong. Teams gravitate toward Linux containers and CLI-based agents because they integrate easily into CI pipelines. But if your developers use Visual Studio Code on Windows, you’re measuring a different experience. The harness is different. The shell is different. The agent’s default technology choices are different. A model that picks Python on Linux might pick .NET on Windows for the same prompt.


The investment in running evals on the right platform pays for itself the first time it prevents you from optimizing for an environment your users don’t have.


## Getting started for real


If you’re building your first eval system based on this series, here’s the practical sequence:


1.


**Start with 3-5 scenarios** that represent your most common developer tasks. Write prompts that sound like real developer requests, not optimized instructions.


2.


**Write specific criteria** using the checklist format. For each check, define pass, fail, and skip conditions. Lean toward over-specifying rather than under-specifying.


3.


**Calibrate your judge** on real agent output where you know the correct verdict. Iterate the criteria until the judge agrees with you consistently.


4.


**Run 5 times per condition** (baseline and with-extension). If results are unanimous, you have your answer. If they’re split, run more.


5.


**Pin your environment.** Record the OS, tool versions, model version, and harness version for every run. When scores shift, check the manifest before blaming your extension.


6.


**Version your rubric.** When you change criteria, treat it as a new measurement instrument. Don’t compare scores across rubric versions without re-running calibration.


7.


**Run regularly.** Some AI coding changes update multiple times a day. Models change without notice. Pick a cadence that matches how fast things move in your stack, but also one that gives you time to act on the results. Running daily is pointless if you can’t investigate a regression until next week.


## Summary


Building AX evals that hold up requires discipline in rubric design (specific enough that two evaluators agree) and statistical practice (enough runs to distinguish signal from noise). Layer system hygiene on top: version pinning, regression detection, calibration loops, cost tracking. None of these are individually hard. Maintaining all of them simultaneously as your scenarios grow is where teams fall apart.


The payoff is confidence. When you see a score change, you know it reflects a real change in capability. When leadership asks whether your AX investment is working, you have data instead of anecdotes.


If you shipped an extension without measuring its impact before reading this, you now have everything you need to start. Pick a scenario, write the criteria, run it five times, and see what you learn. If you get stuck, reach out. I’d love to hear what you’re building.
