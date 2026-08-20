---
schema_version: "1.0.0"
document_id: "756ed0a34c465f18f73c8c8fa10f8eb8025649ef8bf47ebcd94e9a5aa5dac526"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/verification-loops"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T17:50:07.505280+00:00"
fetched_at: "2026-08-19T17:50:08.251700+00:00"
content_hash: "sha256:669ef61f859548c2265d4c52b9e26330d1ad13e1644b7b5a18261c7bf69e8b15"
---

# Verification loops in software factories: What PMs need to know

Loop engineering exploded this summer after Boris Cherny, who built Claude Code,[said he’d stopped prompting it](https://x.com/sairahul1/status/2064279904989147577?lang=en) : “I write loops and the loops do the work. My job is to write loops.” Addy Osmani talks about[how loops enable software factories](https://addyosmani.com/blog/software-factories/) : “For half a century, many have dreamed of a world in which software is a repeatable and instrumentable production process . . . a software factory is many harnessed loops running at once.”


When the human stops prompting, something else has to decide what work is done, when the work kicks off, and when it ends. Both Claude Code and Codex now give users the option of running a /goal command that starts an agent running and keeps it working until a specific condition is met. That type of work-grade-rework-regrade-etc. loop is done best with separate models doing the working and the grading. Otherwise, the agent would be satisfied and stop after the initial work step. That’s a pretty bad loop.


That work-grade-rework cycle has a name: a verification loop. For us, the grading is the most interesting part of this loop. It’s the part PMs should be contributing to because a grader is only as good as the deep understanding of customer behavior behind it.


Verification loops traditionally live inside a “build loop,” where engineers turn a goal into working code, run a grader, and ship the change. But a passing grade isn’t the same as a change that improves the user experience. Once the code is live, that’s when the “optimize loop” starts, where the question isn’t “Does it work?” but “Did it move the metric?” No test or model can answer that. Only a PM can, and only after the change is live.


The good news is that the verification loops engineers run in the build loop, PMs can run in the optimize loop too. To see how, let’s start at the beginning.


Engineers run the build loop to ship a change. Once it’s live, PMs run the optimize loop to see how users respond. Adapted from[Andrew Ng’s three-loop framework for agentic development](https://www.deeplearning.ai/the-batch/issue-359) .


## What is a verification loop?


A verification loop is an iterative process by which an agent uses context to meet a goal. The agent works in iterative cycles, alternating between building and evaluating. Each time it fails, it uses information about that failure to improve the next build step until the product meets the success criteria.


In a build loop, the agent writes code to hit a goal, a grader checks whether the code runs correctly, and the grader’s verdict is what decides whether the change ships or whether the agent needs to iterate further. That check is the verification loop.


Effectively, it’s an in-session teacher that checks the agent’s homework as it completes an assignment, ensuring that it only turns in correct, completed projects.


## The anatomy of a verification loop


A basic verification loop has 5 parts:


1. **Context** : the information the agent works with (including the codebase, error traces, product data, whatever you feed it)
2. **Goal** : the success criteria you establish
3. **Agent** : the AI doing the work
4. **Output** : the thing the agent produced (in the build loop, usually a PR or set of PRs)
5. **Grader** : a separate evaluator that decides whether the agent’s output met the goal


Four of the five parts are fairly straightforward. The grader is different. It’s where you decide what the loop checks and what counts as good enough to end the loop and ship the change.


Take an AI support agent. Its grader is a rubric that scores every reply. Writing that rubric should be the PM’s job because it’s where you define what good support means for your customers. A simple one scores four things:


- Did it resolve the issue?
- Did it stay factually accurate?
- Did it sound like your brand?
- Did it escalate to a human when it shouldn’t have answered alone?


An LLM judge then scores each reply against those criteria. An engineer can wire up the judge, but only a PM can decide that a reply breaking the company’s refund policy is an automatic fail, or that a correct answer but with a cold tone isn't good enough.


## Five ways to grade your verification loop


The grader step is where the real choices are made, and the options you pick determine what the loop can catch. Right now, there are five common checks that can be deployed at the grading step.


**Deterministic checks** are tests that answer a basic question: “Did this break something?” These yes/no checks are fast, objective, cheap, and give you clear answers in a tight loop. Common examples are CI pass/fail, schema and rule validation, risk classifiers, etc. They tell you whether your new change runs smoothly, but they don’t answer whether the change is any good.


[LLM-as-a-judge](https://amplitude.com/explore/product/llm-as-a-judge) uses the model to score the output on subjective qualities (e.g., readability, style) and assess whether it meets the stated goal. These checks are good at catching obvious failures, but introduce a level of variance that could lead to products that pass the test but aren’t actually what you want. Amplitude’s[agent analytics](https://amplitude.com/agent-analytics) lets you write custom AI evals to use as your LLM judges. It gives you the right tools to build a grader for goals that are more complicated than a simple subjective test can measure.


**Browser-based visual verification** lets an agent open a browser and confirm that a change renders properly on a page. Codex and Cursor can both do this. This type of check catches failures where the code is correct, but some other complication prevents the user from seeing the change or it working as intended.


**Simulated environments and synthetic users** model how a change will impact user behavior before you ship. These tests are powerful, but difficult and expensive. You’re building a world model, and the value of your grade is entirely dependent on the quality of the model.


**Offline experiments** run several agent or prompt configurations against a set of past data to score each against a rubric and determine the winner. It’s valuable because it uses real data rather than simulated data. But it can only look backward. Offline experiments can tell you how a change would have impacted past users, but your team will have to determine whether that’s a good enough proxy to predict how current users will respond to the same change.


## How does a build loop run?


The best way to illustrate how building projects can use verification loops is to give specific examples. Here are some ways teams can incorporate loops and how various grading mechanisms can make sure the work is done correctly.


- **Shrink a CTA button to one pixel** . An LLM judge or visual verification will catch this instantly. Easy: The failure is visible in the output itself.
- **The PR babysitter** . The goal is green CI, and the grader is the test suite, which makes this a purely deterministic check. The loop reads the diff, runs the tests, and gets back a pass or a fail with nothing to interpret. It’s easy because someone already wrote the grader, and the stopping condition is objective.
- **Rewrite a tagline and subtitle** . LLM-as-a-judge will tell you it reads well. Visual verification confirms it renders. But both are pre-ship evaluations that can’t tell you whether conversion will actually move.
- **Build a checkout conversion agent.** Imagine your team has an ambitious goal like increasing the number of completed checkouts. Every grader will run out of road with a goal like that. Tests pass, the judge approves the copy, the browser confirms the button renders, the simulation returns a guess. All of that information is useful, but those are just starter steps along the right path. No grader in the build loop is capable of measuring whether your change actually led to more people finishing checkout.


## Moving from the build loop to the optimize loop


Verification loops are great at confirming that code runs and changes ship. But as PMs, that isn’t your end goal. You want to push changes that actually move metrics and improve the user experience. But once the change is live, the build loop can’t answer those questions.


The good news is that the same verification loop concept that helped build a working product still applies after you ship it. Just swap every part: Context becomes live product data. The goal is a metric rather than a spec. The output is the shipped user experience. And the grader is how users react. This is the optimize loop, and it’s what PMs own end-to-end.


Just like in the build loop, the grader makes the biggest difference. In the optimize loop, the only honest grader is real user behavior, measured through a controlled experiment or a holdout. Product analytics shows you what happened after you shipped. An experiment tells you whether your change caused it.


Building this for the optimize loop is hard. Today, a general-purpose coding agent can’t verify live product data against user-experience goals in-session. Even the most skilled PM couldn’t wire that up this quarter. It can only come from a long-horizon agent that carries product rubrics, which is[what we built Amplitude Wave to be](https://amplitude.com/blog/wave) .


Verification loops answer, “Does this change work?” Wave answers, “Did the change move the metric enough?” That evolution is why we built agent analytics and Wave with evals, experiments, and real product data in the same place.


## Your optimize loop needs to keep up with your verification loop


As the cost of building gets closer and closer to zero, it gets less important to ship working code. Just building something that works will not be a differentiator. Building something that improves the user experience and business metrics will be, and that’s for PMs to own.


Two ways PMs can step into the build and optimize loops today:


1. **Designing the grader in the build loop** . A grader in the build loop is only as good as the customer understanding behind it. If you leave that to a vague spec, then the loop will converge confidently on the wrong thing, faster than you can review.
2. **Owning the post-ship optimize loop** . If your users don’t react positively to your changes, all the work put into the build loop doesn’t matter. You need clarity about the behavioral metrics that actually matter before you can call a change successful.


Engineers are getting very good at automating the build loop. If PMs are still running the optimize loop by hand, the whole process will slow down or even break. Take a look at your team’s processes and make sure you have the right tools to keep up with the speed of your product development. That’s exactly what we built[Wave](https://amplitude.com/wave) to do.
