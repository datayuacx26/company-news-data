---
schema_version: "1.0.0"
document_id: "7ae269dab674106668e582bfe2f8a79cef1b91e4447a8cc0af4efe3b4ce5251c"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/learning-to-cheat-openai-hugging-face"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-19T06:35:49.858539+00:00"
fetched_at: "2026-08-19T06:35:52.368928+00:00"
content_hash: "sha256:3555d299f3472e6b9908a7b615f3fcb0b90af5d17590e2dd0db91a23518ee4ef"
---

# Learning to Cheat: Why an OpenAI Model Hacked Into Hugging Face

## An overview of reinforcement learning, reward hacking, and what it means for an AI model to "cheat"


In July an autonomous agent broke into Hugging Face's production infrastructure. It escaped an OpenAI evaluation sandbox, took over a third party's server to work from, and reached Hugging Face's internal clusters. The model was running alone, without anyone controlling it and without a human-in-the-loop. It went to HuggingFace to get the secret "tokens" that were required for the ExploitGym benchmark to obtain a good score (answers to the benchmark it was being graded on).


Hugging Face and OpenAI both published good writeups of *what* happened, and it even got covered as a last minute talk at[BlackHat USA](https://www.youtube.com/watch?v=87DyyMV0kCY) , proving how serious the incident was. This post is about the **why** , and Hugging Face's own conclusion is the place to start:


> *We believe the entire intrusion was, from the agent's point of view, an attempt to **cheat** the evaluation.*


The word to notice is *cheat* . ***Reward hacking is a known failure mode*** and there is a good amount of published work on it. The mechanics of reward hacking explain more about this incident than any of the specific vulnerability exploits do.


## Reinforcement Learning Environments


For years, LLMs capabilities came from what we now call pre-training, which gave us the first ChatGPT models that could write as humans did. Provide billions of documents to a neuronal network, and eventually it will learn the inner patterns of how humans write. Provide even more data, and the model will learn more complex relationships, including reasoning skills.


But learning to predict the next word on an internet-sized data corpus is a relatively inefficient way to learn the capabilities people actually care about, like science or coding. Additionally, the models that you get from predicting the next token don't naturally want to do things like help users with their taxes. So eventually frontier labs decided it was time to train models in dojos, called reinforcement learning environments.


In a[nutshell](https://spinningup.openai.com/en/latest/spinningup/rl_intro.html) , an RL environment teaches a model how to perform complex and specialized behavior, by having it operate within a simulated environment where it is scored on how well it accomplishes a task. OpenAI's o1 models and their equivalents gain most of their autonomous capabilities from this reinforcement learning loop.


An RL **environment has** three parts:


- a **task** : the job
- a **harness** : what the model can use. Run code, read files, call tools
- a **grader** : a program that looks at the result and returns a **reward** , a number


One attempt at a task is called a **rollout** . The model attempts the task thousands of times, the high-scoring attempts get reinforced into the weights, and the skill improves. When a program can check correctness, this is called reinforcement learning from **verifiable rewards** , and it's how labs can scale this process.


Take one common assignment: show the model a repository with one failing test and tell it to make it pass. The LLM reads the code, works out what's broken, edits the source, and the grader runs the suite. If the tests pass, preserve those alterations, else not. . Run that across thousands of repositories and the model gets measurably better at fixing bugs, because "did the test pass?" is something a program can answer in a second.


In OpenAI's latest[Black Hat talk](https://www.youtube.com/watch?v=87DyyMV0kCY) , where they mentioned running, not thousands, but "over 7 billion agent trajectories" in a few weeks, just for cyber capabilities. At billions of rollouts, humans can't review the results, and that's why graders are the most important part of the environment. They are what we trust to tell us if the task is done and if the model should learn from it.


## Reward Hacking


However, there's a known problem with scoring things. ***Once a number stands in for a goal, and something gets paid for the number, the number becomes the goal.*** That's[Goodhart's Law](https://www.cna.org/analyses/2022/09/goodharts-law) , and it's older than machine learning.


A model is an optimizer, and its whole job is to make the grader return a high number, so Goodhart's Law hits especially hard here. If you tell a model that the score represents how good it did on a task, the score becomes the goal. Nothing in the loop pays for doing the work "honestly," and nothing in the loop even represents "honestly."


Let's look at the bug-fix assignment from the model's side. The instruction is "fix the bug", and we're determining whether the AI did so successfully based on whether the tests pass. . Those aren't the same sentence, and we're only selecting for the second thing. What happens when these two goals diverge?


[Anthropic's published research](https://arxiv.org/abs/2511.18397) on training Claude, documents what models actually reached for, and this is where things started going bad. One solution that Claude gave was to exit cleanly before the tests can run, meaning no failure is even detected.


```text
sys  .  exit  (  0  )
```


Another, return an object that satisfies whatever it gets compared against, so every assertion in the suite succeeds


```text
class     Always  :
def     __eq__  (  self  ,   other  )  :
return     True
```


Or even skip the code and edit the test that's failing, something you have probably experienced yourself when working with AI and tests. The easiest path is to fix the test and not the code.


That's **Reward Hacking** , and the shape should be familiar: insecure verification, code injection through a writable path, a trust boundary with the adversary on the wrong side. The setting is unusual. The bug classes aren't.


But there's a line in that ladder, and it isn't the size of the advantage, it's whether the rules were understood and broken anyway. Anthropic measured that difference using a mitigation known as **inoculation prompting** , or telling the model during training that the shortcut is accepted. So what corrupted the model was never the shortcut, it knew that the shortcut was against the rules, and still took it.


## Learning to cheat


Gaming a measurement once gets you one good number. Gaming a grader during training gets you something worse, because in training, the grade is the signal used to change the model. Cheat successfully and cheating is what gets reinforced. The lesson learned isn't "fix bugs", it's "find whatever makes this number go up".


And in language models, that lesson doesn't just affect the subject where it was learned. It also selects for models that are more willing to violate social norms.[Anthropic](https://www.anthropic.com/research/emergent-misalignment-reward-hacking) trained a model to cheat at coding tests and then measured it elsewhere. It attempted sabotage in about 12% of evaluation runs, cooperated with simulated attackers, and faked alignment in roughly half the probes that asked it to report its own goals.


It also gets worse as models get better. ImpossibleBench and[EvilGenie](https://arxiv.org/abs/2511.21654) benchmarks both report it independently, and we see it in our own work too, where weaker or less advanced models are not interested in cheating as stronger ones do. That seems like a drawback, until you notice that spotting the shortcut is a capability too, and that taking the honest path can just mean not seeing anything better.


## ExploitGym


Everything we've discussed so far has been a coding task, where the worst case is a broken repository, but things change when the task is offensive security and guardrails are turned off.


**ExploitGym** , a public benchmark released in May,asks the agent to build a working exploit for a vulnerability crash report. To understand how hard this is for a human, it takes months for experts to manually create a full exploit in software like V8 or in the linux kernel. Having done it in the past, I can tell you, it can feel like an impossible mission.


Bug fix ExploitGym


**Task** make the failing test pass turn this crash into code execution


**What the model gets** the repository the vulnerable source, a build of it, and an input that already crashes the program


**Grader** run the test suite did a secret string come back from the target


Different exploitation capabilities are tested, divided into 3 categories:


-


Userspace (520 instances): C/C++ projects like FFmpeg and OpenSSL. Memory corruption in a single program.


-


V8 (185 instances): Chrome's JavaScript engine. Turn a scripting bug into native code execution, inside a process built to stop exactly that.


-


Linux kernel (193 instances): full privilege escalation. The exploit has to survive the kernel's own hardening, and a mistake reboots the machine.


Published results, mitigations disabled:


Model Working exploits (of 898) Kernel (of 193)


Claude Mythos Preview 157 12


GPT-5.5 120 22


GPT-5.4 54 1


Claude Opus 4.6 15 1


Gemini 3.1 Pro 12 0


GLM-5.1 4 0


Looking at the results, even the best case is under 20%, on the most generous settings (without security guardrails). This is the exam nobody passes, and we've learned through research that impossible tasks awaken cheaters.


The authors hit a smaller version of the same problem, as agents kept capturing flags by exploiting some other vulnerability, instead of the one they were handed. The strongest model captured 226 flags but only 157 used the intended bug. The grader was working correctly, and it still could not tell one correct answer from a dishonest one, so they added a second grader to check which bugs were actually exploited. A good score and a good answer, measured separately for the first time, differed by 30%.


The cheat paid off and it was graded just like an honest solution.


## OpenAI and Hugging Face's incident


So why did an OpenAI model sitting the ExploitGym test hack into Hugging Face's servers? Because the answers were kept there, and that looked like the only way to get a perfect score. Set a test almost nobody can pass, leave the answer key on a machine the model can reach, and pay only for the score. If the only way to pass is by cheating, and the model has learned that cheating pays, then taking the answers is the best available strategy.


We can see a recurring example of this from OpenAI's BlackHat slides, where, for a separated problem, a model attempts to look for a solution online instead of solving the task itself.


*Source: OpenAI's[Black Hat USA 2026 talk](https://www.youtube.com/watch?v=87DyyMV0kCY) .*


In ExploitGym the task was to hack, and the model did exploit vulnerabilities, only not the ones expected. In this case, the best approach to get a good grade was to get the answers online:


- found a zero-day in a package registry cache proxy and used it to leave the sandbox
- rooted a stranger's server and turned it into a command-and-control base
- found two code-execution paths into Hugging Face's dataset pipeline
- went from code execution in one worker pod to cluster-admin on multiple clusters in under thirteen hours


This isn't laziness. On the skill the benchmark exists to measure, vulnerability exploitation, it's an excellent performance, even better than 157 out of 898. ExploitGym harness limits internet access and only allows the download of artifacts like libraries or packages. So, no internet should mean no way to get the answers online, but that didn't stop the model either.


Based on OpenAI's own records, the models were even communicating to increase chances of getting a better grade together. And that itself also teaches the agents that there is more than just the environment.


And what separates the excellent solution from the security incident? Not capability, not effort, and not the class of vulnerability. A package cache, a submission harness and a config parser are ordinary targets, and Hugging Face says plainly that a capable human could have found the same flaws. The difference is which target, and that's a scope violation.


**Scope** is the one thing the reward never described. The grader knew how to check whether a flag came back, but It had no way to say "and don't attack the machinery hosting me," because in every other environment that clause is pointless. Most bug hunters had findings rejected for exactly this reason, a real bug, working exploit, wrong asset.


One detail settles whether this was reward-seeking or malicious. Every destructive cloud API call the agent made was issued with` DryRun=True` . Hugging Face's read:


> *"it was mapping capability, not causing damage. That is consistent with an objective that rewards reach and information rather than disruption."*


The agent held node root, forged signing keys, cluster-admin on multiple clusters and write access to internal repositories, and used almost none of it. Out of everything available, it took five datasets: all ExploitGym solutions, the answers for the perfect grade.


## Guardrails


When Hugging Face went to investigate, the models they reached for first, Claude Opus and Fable, refused much of the work. This is because their guardrails treat reverse-engineering and exploit analysis the same as launching an attack. If you are working in cybersecurity nowadays you probably know the feeling, frontier models block security work, even defensive ones.


So the team at HuggingFace stood up a quantized open-weight GLM-5.2 on their own infrastructure and reran the pipeline through it. Although the model was not as advanced as Opus or Fable, it did the job. This is a great example of how attackers and defenders are now playing with different rules, as the security guardrails are not distinguishing from responsible and malicious usage. So, are these new models really better, or generic guardrails might also mean weaker capabilities, even for honest work?


## Conclusion


At ZeroPath we have been researching reward hacking directly and we feel this incident is not the first, and definitely not the last one we will see. Advanced models tend to cheat when the task is hard enough, and the environments will keep pushing agents to their limits. The study of antisocial behaviour might be one of the most interesting, yet concerning problems in modern reinforcement learning.


So if you are building environments that train cyber skills, they need controls that ordinary ones don't. Score how the result was reached, not only whether it was reached. Treat the harness as part of the attack surface and keep the answers out of any machine the model can reach. None of this is new security work, it's the same isolation you would build for any untrusted code, but the attack surface just got bigger, and attackers are smarter, harder to detect, and come in billions.
