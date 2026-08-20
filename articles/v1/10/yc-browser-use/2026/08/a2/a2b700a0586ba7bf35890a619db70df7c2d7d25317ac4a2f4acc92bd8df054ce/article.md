---
schema_version: "1.0.0"
document_id: "a2b700a0586ba7bf35890a619db70df7c2d7d25317ac4a2f4acc92bd8df054ce"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-3219c37ba697"
canonical_url: "https://browser-use.com/posts/benchmark-behind-the-benchmark"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T12:21:49.678271+00:00"
fetched_at: "2026-08-06T12:21:50.243826+00:00"
content_hash: "sha256:6e7d70597011c065b551796ef7bcd19c332b49e06b9a962cf52f3bc59ef41281"
---

# The Benchmark Behind the Benchmark

A benchmark is **two things** : the tasks, and the judge that decides whether you succeeded. Everyone treats the judge as an afterthought, but it decides every number you publish, and a weak one makes the whole thing **reward-hackable** .


So we spent a long time building ours. An aligned judge has to be consistent in four ways:


- **The same trace twice.** Judge one finished run again and get the same score.
- **Across tasks.** Task 90 is held to the same bar as task 3.
- **Across models and harnesses.** Browser Use and Codex get graded the same way.
- **At the run level.** If model A beats model B, that shouldn't flip when you judge it again.


None of that is free. Start with how much of the score is even real.


## What moves the score


Two things. Only one of them is the model.


Var


(


score


)


=


can’t fix


σ


agent


2


​


​


​


+


ours


σ


judge


2


​


​


​


**Agent noise** is the web and the model. A site blocks you. A wrong turn on step 12. You measure it. You can't remove it.


**Judge noise** is the same finished run, graded twice, coming back with two different verdicts. That one we can kill.


### Agent noise


We ran GPT-5.5 five times on the same 106 tasks. Same model. Same harness. Same config.


Each run solved about 89. Never the same 89.


Every column is one task. Every row is one run. Green means it worked.


**64 tasks work every time. 2 never work.** The 40 in the middle are the whole problem. Same model, same task, passing and failing at random. A misread filter. A wrong click. Giving up one step early.


Run once and you score 85%. Give it five tries and 104 of the 106 tasks work at least once.


So a single run doesn't measure what a model *can* do. It measures how often it does it.


### Judge noise


Now freeze the agent. We took 104 finished runs from that set, the ones we have human labels for, and changed only the model doing the grading.


Same idea: every column a task, every row a judge, all reading identical work.


**45% of the benchmark depends on which judge you asked.** The most lenient reports 83.7%, the strictest 62.5%. Twenty-one points, wider than the gap between most models people compare.


If the judge moves the score more than the model does, you can't tell whether a change helped.


## Building the judge


### One prompt with the whole trace


The first judge was a single LLM call. Whole trace in, verdict out.


Fine with one harness. Then we added more. Traces look nothing alike, run hundreds of steps, and compaction eats the part you need.


### The evidence is one thing, buried


Task: extract listings under $2,000. On step 34 the agent sets the filter to $20,000. Everything after that is clean, confident, and wrong.


You can't fit that in a prompt. And one pass gives one shot at finding it.


So the judge became an agent. It gets the trace and the workspace and goes hunting, as Codex or Claude Code with read access. It can open the CSV, scroll back to step 34, check the number against the page it came from.


### What the judge can see


An agent judge can only verify what it can open. Easy to break, though. Screenshots are heavy. Page text is heavy. Something truncates to fit a budget.


Cut the wrong thing and a true claim becomes unverifiable, which to the judge looks identical to an invented one. It starts calling correct answers hallucinations. Get the evidence contract right before you touch the prompt.


## Why we stopped asking for pass/fail


The judge has to output something, and pass or fail is the obvious choice. It's also the wrong one, because most tasks aren't binary.


Real prompts are loose. *Check one of the top contributors.* *Open the trending puzzle.* Nobody can verify which one was right, including the judge. And agents fail partially: ask for JSON, get a CSV with all the right data in it. After 400 correct steps that isn't a zero.


One from our set:


> Go to spaces and navigate to one of the recommended spaces to view. Check the profile of one of the top contributors in this space and return how many followers they have.


The agent verified **227 followers** . But the recommended-spaces page was login-gated, so it used a publicly reachable Space instead.


**Five of our eleven judges passed it. Six failed it.** Same screenshots, same follower count. They split on whether a substituted Space counts.


So it isn't 0 or 1. Call it a 60. You can argue for 45, and that argument is the point: the score doesn't remove the judgment call, it puts it somewhere you can see.


So we ask for a number instead: what percentage of the requested outcome was delivered, correct and backed by evidence. Here is the same judge on the same traces, run four times, changing only a setting that shouldn't move the answer at all.


**20 points versus half a point.** The binary judge isn't more wrong, it just amplifies. Every borderline task is a coin flip, and every coin flip becomes a whole task in the final score.


## Ensembles


The other lever: run the judge several times and combine the answers.


It works because judge noise is random. Every run is the real score plus some error, and those errors don't agree with each other, so putting several together cancels most of them out. For


n


independent jurors each with variance


σ


2


:


Var


(


s


ˉ


)


=


n


σ


2


​


Three jurors cut the spread by


3


​


, five by


5


​


.


In practice you get a bit less than that. Measured across repeat runs, our whole-run number moves by about a point.


### How you combine them matters


Averaging seems right and isn't. Finding a buried defect is a search, so sometimes one juror digs into the right file and comes back with a 10 while the other two sit at 96. That single outlier drags the average down 30 points.


The median survives that. It also throws away the case where the lone juror was *right* , and it can't tell the two apart.


We tried to fix that. When the three scores are far apart, send all three audits to a fourth model and let it decide who's right.


**Dead end.** The arbitrator is another LLM, so it answers differently on different runs. We had swapped a stable median for an unstable tiebreak, and the final number moved around more than before.


So we let the lone-needle case go. We miss a few real bugs and the number stops moving.


> **Side quest: how universities do it**
>
>
> Marking a stack of exams is the same problem. Nobody wants two TAs handing the same paper wildly different grades, so they don't let each TA decide from scratch. They get a marking scheme: the full proof is 50 points, and the one genuinely hard step is worth 40 even if you only got a tenth of the way through it.
>
>
> We do the same thing with failure modes. Run a task twenty times across different models, collect every way it goes wrong, and hand the judge that list. We have 5,692 of them.
>
>
> It only covers what you've already seen, though. The first time an agent skips the browser entirely and reverse-engineers the site's API, there's no line for it and the judge is guessing again.


## Comparing models


You have a score for every task. Three ways to turn that into "A beats B", and they get better in that order.


### Threshold and count


Pick a cut, call everything above it a pass, add them up. This is what almost every benchmark reports.


It throws away everything the score just told you. A 52 and a 48 are the same run for any practical purpose, and they end up on opposite sides. You are back to the amplification problem from before.


### Task by task


Line the two models up on the *same* task, decide who won that one task, then count. On scores you leave a tie band in the middle, five points wide, so small gaps don't count as a result.


Every row is one task both models attempted. This pair predates the continuous judge, so the verdicts are pass/fail and a tie means both did the same thing.


More useful than "84 versus 78": the models mostly agree, and Opus is ahead on fourteen specific tasks you can go read.


Pairing also kills most of the noise, because both models faced the same task:


Δ


ˉ


=


n


1


​


i


=


1


∑


n


​


(


A


i


​


−


B


i


​


)


SE


=


n


​


s


Δ


​


​


Counting wins throws away the margins too. That's a threshold, and I just attacked thresholds. The difference: at the task level magnitude is signal, here it's usually artifact.


It changes answers. Opus beat Grok on 44 tasks by 12.3 points, Grok beat Opus on 28 by 26.7. On


Δ


ˉ


, **Grok wins** . On win counts, **Opus takes it 56.6 to 43.4** . We believe Opus: a 90-point gap on one task is nearly always the judge being strange.


### Elo


Two models at a time doesn't scale to twenty, and not every model has run every dataset.


So treat each task as a game. A beats B if it scores five points higher, draw inside that. Five is roughly how much the same judge moves on the same task, so anything smaller is noise. 106 tasks across six configurations gives 1,590 matches.


The top four sit within 56 points of each other. That's close enough that I wouldn't call it a ranking.


The gap that does matter: Opus 5 at low effort is 88 points below Opus 5 at high. Turning up reasoning moves a model further than switching vendors does.


Elo aggregates the judgments you already have. It can't repair them.


## Where this leaves us


Report agent noise. Run more than once and show best-of-N.


Judge noise you can actually kill. Give the judge every piece of evidence, ask for a score instead of a verdict, run it three times, take the median. That makes it consistent, not correct, and consistent is the part you can measure.


And publish the judge next to the dataset.


Once it holds still, you get charts like this:


Same 106 tasks, every model judged the same way. **Luna xhigh lands two tasks behind Opus 5 at a sixteenth of the cost.** That comparison only means something because the judge didn't move between them.


Every model on that chart runs on[Browser Use Cloud](https://cloud.browser-use.com/) , if you want to point them at your own tasks.


## TL;DR


Swap only the judge and the same runs score 62.5% or 83.7%. Give it the whole workspace, ask for a score instead of a verdict, run it three times, take the median.
