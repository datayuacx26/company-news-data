---
schema_version: "1.0.0"
document_id: "ce38f065a4c5634bfb139ca76c069c1a31f3ab4feced70a0a4b999a0c3c6db9f"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/the-end-of-push-wait-guess-ci"
published_at: "2026-03-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:56:52.525405+00:00"
content_hash: "sha256:cca4b3545348bb9b2374a6138820f15773ee1ca54207c347081481a11d984e5e"
---

# The end of push-wait-guess CI

## What it was like


Debugging CI feels like mailing a car mechanic instructions for a single turn of a wrench, waiting 3 days, and getting mailed back a Polaroid of a burning car with the caption:` Exit code 1`


The old CI debugging experience, visualized


I'd edit a workflow file, hope I hadn't committed some ancient YAML war crime, commit, push, wait, and then squint at the logs to parse the useful from the haystack. Why is so much of my life as an engineer relegated to YAML files? Great question. No idea. Anyways, here's some questions I would continually hold in my brain:


- Was I in the right working directory?
- Did checkout happen the way I thought?
- Did the file actually exist? Did the variable get set in this context?
- Did the artifact land where I assumed it would?


Who knows.


Certainly not me.


If the failing step sat at the end of a long job, even better. Now I get to wait 15 minutes to learn that my latest guess was also wrong. Sick!


That old loop was more hope-filled finger crossing than debugging.


## What it's like now


Here's the deal. I work for Depot. Depot paychecks pay for my wife's unyielding desire for travel. And thusly, I am biased. So, feel free to disregard the sincerity of this statement: Depot CI with agents has fundamentally changed how I interact with and think about CI. No bullshit.


It turns CI from a remote black box into a local closed loop I can actually drive from my IDE or terminal. I can run a workflow against my current local diff. I can scope it down to one job. I can inspect status and logs. I can stop after a specific step. I can SSH into the actual machine. I can fix the problem locally and rerun until it passes.


I don't have to live in the land of guesses, hopes, and finger crosses because I can almost completely control the loop while having access to all the context and tools I need to fix and iterate.


The first thing that made this click for me was stupidly simple:


```text
depot   ci   run   --workflow   .depot/workflows/ci.yml
```


That command looked ordinary until I realized what it meant. I am not committing and pushing every tiny hypothesis just to see if the thing explodes again. Depot CLI will take my local uncommitted changes, upload them as a patch, and apply them during the run. That alone removes an annoying amount of ceremony. No more polluting my git history with a graveyard of` fix: typo, fix: typo again,` and` fix: asdfasdl` . I can now edit, run, and get a freaking answer.


Then there are the rest of the loop primitives:


```text
depot   ci   status   <  run-i  d  >
depot   ci   logs   <  run-i  d  >
```


And when logs stop being enough:


```text
depot   ci   run   --workflow   .depot/workflows/ci.yml   --ssh-after-step   3
depot   ci   ssh   <  run-i  d  >
```


That last bit is the super fun bit for me. If the thing that keeps failing sits behind 10 or 15 minutes of setup, I don't have to keep reliving Groundhog Day. I can stop after the expensive part and open the hood. I can treat the sandbox the workflow is running in like my own machine!


## Open the hood


Say I have a workflow with a slow setup step and one dumb little failing step at the end.


The setup step creates` demo-artifacts/hello.txt` . The final step tries to read` demo-output/hello.txt` . Deeply sophisticated bug. Extremely enterprise.


In the old loop, I'd push the workflow, wait for it to fail, read` No such file or directory` , and then start inventing machine state in my own head. Maybe the file never got created. Maybe I'm in the wrong working directory. Maybe checkout landed somewhere unexpected. Maybe the artifact name is wrong. Maybe the shell is doing some little goblin thing. Hard to know, because the one thing I actually want to do, which is inspect the machine, is the one thing I'm not allowed to do.


In Depot CI, that same failure looks completely different.


First, I run the workflow against my local changes and scope it down to the one job I care about:


```text
depot   ci   run   --workflow   .depot/workflows/demo.yml   --job   inspectable-demo
```


Then I check status and pull the logs:


```text
depot   ci   status   <  run-i  d  >
depot   ci   logs   <  run-i  d  >   --job   inspectable-demo
```


If the logs are enough, great. If not, I rerun the job with a stop after the expensive setup step:


```text
depot   ci   run   --workflow   .depot/workflows/demo.yml   --job   inspectable-demo   --ssh-after-step   2
```


That's the move.


Now I'm not guessing anymore. I'm on the machine. I can run` pwd` . I can run` ls` . I can look around like a normal human trying to understand a real computer. The answer is right there. I'm in` /home/runner/work/e2e/e2e` . The` demo-artifacts` directory exists.` hello.txt` is sitting right there. The workflow didn't fail because the file wasn't created. It failed because the last step was looking in the wrong place.


So I fix the path locally, rerun the job, and it goes green.


That's the whole loop:


Run. Inspect status. Read logs. Stop at the interesting step. SSH in. Check reality. Fix locally. Rerun. Green.


All from the comfort of my terminal.


I didn't appreciate how much the old way was frying my brain until I stopped doing it. Half the pain of CI debugging isn't even the bug. It's the Tab Olympics. IDE, terminal, browser, logs page, back to code, back to the run, back to the logs because the UI forgot where I was. By the time I had enough signal to form a decent hypothesis, I was already cognitively cooked. Fried, I tell ya.


This is what changes when I can finally open the hood.


TL;DR chart:


Step The Old Way The Depot Way


**Change** Commit & Push Local Patch (No Commit)


**Wait** 15 mins (Full Job) 2 mins (Targeted Job)


**Debug** Guessing at Logs ssh into the machine


## The Art of Letting Go


After debugging CI like this a few times and seeing it work on actual production workflows I started to get that peculiar itch. An itch that occurs in many lazy individuals like myself: Why am I doing this manually when I can automate it?


Depot CI turns the "black box" into a controllable loop of CLI commands. That makes CI debugging **delegable** . In the old world, an agent couldn't really fix my CI because it couldn't inspect the ephemeral runner. In this one, it has a terminal and an SSH key. Let's go!


### Putting the agent to work


I no longer stand there watching circles turn orange while I wonder what I want for lunch. I hand the loop to a` /fix-ci` command backed by a[Depot CI skill](https://github.com/depot/skills/blob/main/skills/depot-ci/SKILL.md) .


The agent runs the same loop I would:


1. Read the logs.
2. Rerun the job with` --ssh-after-step` .
3. SSH into the runner and check reality with` pwd` ,` ls` , and whatever else the moment requires.
4. Apply a local fix, rerun, and keep going until the job turns green.


The agent has something I lack during a 6:00 PM debugging session: patience. It doesn't get fried or bored. It just methodically checks reality against the YAML until they match.


## CI is not the work


I don't build CI pipelines because YAML is my true calling.


CI exists to verify code, run tests, lint, scan, build artifacts, deploy software, run migrations, and cut releases. The point of CI isn't to implement CI. The point is to get reliable outcomes so I can move on.


Doing CI this way moves me from implementing the thing I care about to experiencing the thing I care about. It brings me from idea to actuality. It makes things exist.


And once an agent owns the loop, my role shifts up from the weeds into the canopy of cognitive hierarchy: describe the outcome, review the work, approve the result, move to the next thing.


That's time back *and* my brain back.


## Try it yourself


If you want to feel the difference, start small.


Create a tiny workflow. Break it on purpose. Run it with` depot ci run` . Scope it to one job. Stop it after the interesting step. SSH in. Look around. Fix it. Rerun it.


Once you trust the loop, hand it off to an agent… and let it do your work for you.


No more guessing. No more finger-crossing. Just edit, run, green.


For more details, see the very serious[documentation for this agent loop pattern](https://depot.dev/docs/ci/how-to-guides/coding-agents) .


## FAQ


Can I run a Depot CI workflow against uncommitted local changes?


Yes.` depot ci run --workflow .depot/workflows/ci.yml` uploads your local uncommitted changes as a patch and applies them during the run. No more committing throwaway fixes just to test a hypothesis.


How do I SSH into a running Depot CI job to debug it?


Use` --ssh-after-step` to pause the job after a specific step, then` depot ci ssh <run-id>` to open a shell on the runner. You can poke around with` pwd` ,` ls` , check env vars, whatever you need to understand the actual machine state instead of guessing from logs.


Can an AI agent debug CI failures for me?


Once CI debugging is a loop of CLI commands (run, check logs, SSH in, fix, rerun), it becomes something an agent can handle. A` /fix-ci` command backed by a Depot CI skill runs that same loop: reads logs, SSHs into the runner, checks reality, applies fixes, and reruns until green.


## Related posts


- [Now available: Depot CI](https://depot.dev/blog/now-available-depot-ci)
- [How simulations reduce the guesswork in our infrastructure decisions](https://depot.dev/blog/how-simulations-reduce-guesswork)
- [Sherlock can analyze your builds and CI](https://depot.dev/blog/teaching-sherlock-new-tricks)


Andrew "Watts" Watkins


Engineering Manager at Depot
