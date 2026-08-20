---
schema_version: "1.0.0"
document_id: "d7b53f67845e2f0feafe73b7f38e2e0747e3a9bfda3b60ed98941224027f1792"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/taste-over-tooling-craftsmanship-in-the-post-execution-era-891fbdcd5fe3"
published_at: "2026-07-13T21:41:11+00:00"
first_seen_at: "2026-07-19T22:15:27.842622+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:056fea3ae617b8906fd4b4cae9787c613769047c2a0516812285ad15fe9268de"
---

# Taste Over Tooling: Craftsmanship in the Post-Execution Era

# Taste Over Tooling: Craftsmanship in the Post-Execution Era


[Valentino](https://medium.com/@codenamev?source=post_page---byline--891fbdcd5fe3---------------------------------------)


9 min read


·


Jul 13, 2026


--


Press enter or click to view image in full size


Photo by[Bozhin Karaivanov](https://unsplash.com/@bkaraivanov?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


*As AI agents commoditize the execution of code, the leverage moves to the two things a machine can’t do for you: deciding what belongs on the menu, and knowing when a finished plate is wrong.*


## Introduction: The Shifting Medium of Craftsmanship


Software craftsmanship has always been an evolving discipline. For the last decade, our industry was shaped by what I think of as the “happy coder” mentality. Evangelized heavily by the Ruby on Rails community and figures like David Heinemeier Hansson (DHH), we treated code primarily as a medium for human-to-human communication. We piled into rooms with whiteboards to map out dreams. We obsessed over tight feedback loops and empathy for the next developer who would read our syntax.


Today, the medium is shifting. With the rise of agentic coding, we are no longer just communicating with peers. We are directing autonomous machines. Because agents can write, refactor, and test code almost instantly, **execution has become a commodity** . The ability to simply perform a task or follow a pre-set plan is no longer where the value sits. For modern craftspeople, value now lives in the judgment, problem-solving, and creative input that precede and guide that execution.


The actual typing of code is slipping from our fingertips, and for many of us that feels terrifying. But craftsmanship hasn’t disappeared. It has moved up the stack.


## The “Golden Path” Trap: The Bitter Lesson of Rigid Context


Faced with near-infinite, instantaneous execution, the industry’s first instinct has been to cage it. Teams are searching for a “golden path” to make agents reliably useful, and the result is a whirlwind of complex tooling: multi-step LLM pipelines, rigid harnesses, and sprawling context files that run to thousands of lines.


I have spent a lot of this past year building exactly that kind of system, and I want to be honest about how it goes.


I work on a domain skill system for a production agent. Each “skill” is a package of instructions, rules, and tool contracts scoped to one domain. The intent is reasonable: give the agent the right context for the task in front of it. The reality is that a single root context file grew to roughly 960 lines, and selecting just one domain skill expands the assembled system prompt from around 230 lines to nearly 3,000. That is a 13x inflation before the agent has read a single line of the user’s actual question.


The problems compound from there. We cap the classifier at three simultaneous skills, but skills from independent teams stack into the same prompt with no collision detection, so their instructions shadow and occasionally contradict each other. Tool binding is global rather than skill-aware, which means the agent can reach for any tool regardless of which skill was selected, so we end up encoding “you must call this tool, you must not call that one” in prose because there is no structural way to enforce it. The tell is in our own test suite: the large majority of our evals exist to check whether the right skill was selected at all, not whether the answer was any good. We write entire eval files to stop one domain’s keyword from wrongly triggering another domain’s skill.


That is the trap. The more context we add to control the agent, the more edge cases, selection conflicts, and slightly-off tool calls we generate, and the more prose we write to patch them.


This is not just my experience. Consider the recent preprint[Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?](https://arxiv.org/html/2602.11988v1) (Gloaguen et al., Feb 2026). The researchers tested the now-standard advice of generating repository context files to guide agents, across two settings: established SWE-bench tasks with generated context files, and a fresh collection of issues from repositories that already ship developer-committed context files. Across multiple agents and models, the context files reduced task success rates compared to providing no repository context, while increasing inference cost by over 20%. Behaviorally, the files encouraged “broader exploration,” more file traversal and more thorough testing, because the agents dutifully respected every instruction they were given. The authors’ conclusion is blunt: human-written context files should describe only minimal requirements.


My reading, after living inside one of these systems, is that comprehensive documentation does not constrain an agent so much as it gives the agent more reasons to wander.


This is the shape of a much older pattern. AI researcher Rich Sutton named it[The Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) : “general methods that leverage computation are ultimately the most effective, and by a large margin.” Every time we try to build in our own hard-won structure to steer a capable model, it helps in the short term and then plateaus. As Sutton puts it, “we have to learn the bitter lesson that building in how we think does not work in the long run.” A sprawling context file is exactly that: our knowledge, encoded as rigid scaffolding, fighting a model that would often do better with less.


## The Agentic Omakase: What the Data Says


If rigid tooling and massive context files are a trap, how do we guide the machine? The answer comes from the era we are leaving behind: **conventions** .


DHH described the Rails philosophy as[omakase](https://rubyonrails.org/doctrine#omakase) . “How do you know what to order in a restaurant when you don’t know what’s good? Well, if you let the chef choose, you can probably assume a good meal, even before you know what ‘good’ is.” Rails won because it made the baseline decisions for you. It chose a small set of strong defaults, and trusted that taste, applied once by the people who curate the stack, beats configuration applied endlessly by everyone downstream.


The agentic version is the same move. We are not the diner here and we are not the line cook. We sit in the chef’s seat. We curate a small, opinionated menu of conventions, and the agent and the developer both work from it. We pick what is on the menu; the agent cooks the meal.


## Get Valentino’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


The data backs this up with surprising force. In[SkillsBench: Benchmarking How Well Agent Skills Work Across Diverse Tasks](https://arxiv.org/abs/2602.12670) (Li et al., Feb 2026), the researchers built a benchmark of 86 tasks across 11 domains, with curated skills and deterministic verifiers, and ran more than 7,300 trajectories. Curated skills raised the average pass rate by 16.2 points. But the headline average hides the real lesson:


- The benefit varies wildly by domain, from 4.5 points for software engineering to 51.9 points for healthcare. Skills are not uniformly good. They pay off where the procedure is genuinely hard to infer.
- In 16 of 84 tasks, skills made things worse. Adding procedural knowledge is not free, and sometimes it actively gets in the way.
- Agents asked to write their own skills saw no benefit on average. Models cannot reliably author the procedural knowledge they benefit from consuming.
- Focused skills with two to three modules outperformed comprehensive documentation, and smaller models equipped with good skills could match larger models without them.


You cannot hand an agent a massive manual, and you cannot expect the agent to write its own conventions. Both fail. What works is a curated handful of strong, focused conventions, chosen by a person with taste.


This is the direction I have been pushed in my own setup, almost against my instincts. My global agent config is 31 lines. My rules directory is a single file. My hooks amount to one hook, and even that one delegates its real logic to a separate binary so the instruction surface stays thin. When I built a review tool that auto-triggered on every diff and found it got in the way more than it helped, I did not rewrite it or expand its instructions. I left it in place and switched it off with a single line of config. The skill is still there when I summon it deliberately, but it no longer crowds the agent’s attention by default. Curate the menu, then have the discipline to keep things off it.


## Quality Backpressure and the New Rigor


If execution is cheap and complex tooling is a liability, what is the engineer’s actual job? I think of it as **quality backpressure** .


When an agent can generate a feature in seconds, the bottleneck is no longer how fast we can type. It is how safely we can verify. The infinite stream of execution needs something pushing back against it, and that something is us. This is a return to the harder sciences of our craft: careful measurement, robust architectural boundaries, and a refusal to accept output just because it looks right.


Here is a concrete example from my own work. An agent shipped code that attached a tag to our observability traces, deep inside the request flow. The unit tests passed. They mocked the tagging call and asserted it was made. A neighboring metric kept flowing, so we knew execution reached that part of the code. Everything was green. But the tag never actually appeared on a single production trace, because at that call site the active tracing scope was empty, and the tagging function silently did nothing when there was no scope. No error. No warning. No failed test.


What caught it was not a better test. It was a habit: open the destination system, find a real sampled request, and look for the attribute with your own eyes. It was absent, sitting right next to another tag, set by the same function from a different point in the code, that showed up fine.


And here is the subtle part. The change had been verified end to end in a non-production environment, and it genuinely worked there. The thing that broke it was that production ran the code differently. The build pipeline and runtime configuration executed the same code along a path the verifier was never set up to mirror, so a change that truly passed its end-to-end check still failed where it counted. The verification gate that mattered was the one that touched production ground truth, not the one that passed locally or in staging.


That is the pattern behind almost every agent mistake I have caught. The output was plausible, consistent with the evidence in front of it, and it cleared every local check, and it was still wrong, because the check never reached the layer where the truth actually lived. Another time, an agent confidently proposed adding retry logic that was “missing.” One question to the agent, “aren’t we already retrying with backoff?”, sent us to the SDK source, where retries with exponential backoff were already running on every call. The metric read zero not because nothing retried, but because the metric could not see the layer where the retries happened.


The rigor you bring to the table is not a context file you downloaded off GitHub. It is your domain expertise, your instinct for which layer to probe, and the specific, probing questions you ask the agent after it confidently hands you a thousand lines of code.


## Conclusion: The Rise of Taste


Execution is becoming a smaller part of what defines good engineering work, and we should welcome that.


The two ideas in this post are really one idea seen from two sides. The agentic omakase is how you set the agent up: a small, curated menu of strong conventions instead of a sprawling manual. Quality backpressure is how you hold it accountable afterward: the personalized rigor that filters an infinite stream of execution through judgment the agent does not have. You curate the menu, and then you taste every plate that comes back. As both SkillsBench and Evaluating AGENTS.md show, over-engineering the agent’s environment hurts more than it helps, so the leverage moves to the two things a machine cannot do for you: deciding what belongs on the menu, and knowing when a finished plate is wrong.


The happy coder of the future is not the one maintaining the most elaborate AI pipeline, nor the one hand-writing the most elegant syntax. Execution has slipped from our fingers. The taste required to guide it is still entirely our own.


*If navigating this shift with strong conventions, personalized rigor, and a real focus on craftsmanship sounds like your kind of work, we would love to hear from you. We are hiring. Learn more at*[gusto.com/careers](https://gusto.com/careers) *.*
