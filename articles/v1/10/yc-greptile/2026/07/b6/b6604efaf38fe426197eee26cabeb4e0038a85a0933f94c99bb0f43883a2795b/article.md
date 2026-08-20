---
schema_version: "1.0.0"
document_id: "b6604efaf38fe426197eee26cabeb4e0038a85a0933f94c99bb0f43883a2795b"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-f703c271ba19"
canonical_url: "https://www.greptile.com/blog/model-inversion"
published_at: "2026-07-21T12:00:00+00:00"
first_seen_at: "2026-07-21T22:07:23.426026+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:79c7cb98e593fc6a8339fa4e0093cf56655cde75ae2eafd89f525e263e631bae"
---

# Models are worse at reviewing their own code

## Introduction


I'm Rodrigo, I work on the research team at Greptile, the AI code review agent.


Greptile uses a variety of models under the hood, but the main review agent is usually a frontier model from either OpenAI or Anthropic.


Most of the code that Greptile reviews is also written by models from OpenAI and Anthropic. It got me thinking - are models good at finding bugs in their own code?


## Methodology


- I curated two datasets of 500 PRs each, one authored by Claude Code and one by Codex.
- Model authorship was determined via commit trails such as` Co-authored by: Claude Opus 4.7` , PR title prefixes like` \[codex\]` , and branch prefixes like` codex/` .
- Using sentiment analysis, upvote/downvote ratios, and git archaeology, I built a ground truth dataset of verified bugs. The two datasets contain roughly 1,500 ground truth comments in total.
- I ran both Codex and Claude Code's` /review` feature 3 times per PR, measured recall by matching comments against ground truth with an LLM-as-a-judge, and averaged the results.
- Stylistic comments, praise, and documentation suggestions were excluded. Final results reflect recall on high-severity bugs only.


## Findings


### 1. Models are worse at finding bugs in their own code


The data shows that both models find more bugs in code written by the other model than in code they wrote themselves.


\[ FIG. 01 / RECALL OF HIGH SEVERITY BUGS \]


### Each model finds more bugs in the *other* model's code


64%


60%


56%


52%


48%


60.0%


62.0%


53.7%


50.5%


Claude Authored PRs


Codex Authored PRs


Claude Opus 4.7


GPT 5.5


same-model


cross-model


P0/P1 recall, %


In Claude Code authored PRs, GPT caught a higher share of high severity bugs than Opus. The opposite was true for Codex authored PRs - Opus beats GPT.


### 2. Models produce the same types of bugs that they are most likely to miss in review


I wanted to find out why this was. I decided to categorize the types of bugs that each model was producing into categories like "Performance" and "UI".


This revealed a fascinating pattern: the types of bugs a model introduces most often are the same types it's more likely to miss during review.


\[ FIG. 02 / BUG FREQUENCY PER DATASET \]


### Where each model's bugs cluster


Missing behavior


35.1%


24.2%


Semantic intent


21.4%


27.4%


Error handling


18.4%


22.6%


Security


9.4%


6.7%


Performance


5.0%


7.5%


Contract & schema


4.7%


5.2%


UI behavior


4.3%


3.6%


Build breakage


1.7%


2.8%


Claude PRs


Codex PRs


share of bugs in dataset


\[ FIG. 03 / HIGH-SEVERITY RECALL PER CATEGORY \]


### Each is best at catching the *other's* bugs


Missing behavior


63.3


69.0


Semantic intent


40.4


33.9


Error handling


59.4


55.2


Security


68.4


72.4


Performance


57.1


51.1


Contract & schema


46.6


50.2


UI behavior


57.9


59.3


Build breakage


58.8


82.4


30%


45%


60%


75%


90%


Opus 4.7


GPT 5.5


loser


P0/P1 recall, %


Claude-authored PRs have a higher share of "wrong data or missing behavior" bugs compared to Codex. That's a category where GPT outperforms Claude as a reviewer. Meanwhile, Codex-authored PRs skew more toward speculative semantic issues and error handling failures - categories where Claude's recall is stronger.


Each model seems to have its own mental checklist, its own design instincts. The bugs it introduces are reflections of the things it doesn't naturally worry about, and those are exactly the things it fails to flag during review. A different model, with different instincts, helps you fill in the gaps.


### 3. Claude models go wide, GPT models go deep


Researching this question meant reading a lot of model traces, and that's where things got interesting.


Each model has its own quirks. Some are noticeable personality traits, like GPT's obsession with[goblins](https://openai.com/index/where-the-goblins-came-from/) , or Opus' care for[animal welfare](https://assets.anthropic.com/m/983c85a201a962f/original/Alignment-Faking-in-Large-Language-Models-full-paper.pdf) . But others are more subtle, and directly affect how each model approaches the tasks we care about.


I spent a lot of time analyzing how each model behaves when tasked with a code review: the evidence that it gathers, where it spends its reasoning tokens, and what it decides to flag or ignore. Here's what stood out.


During initial testing, I observed a stark contrast in the number of comments each model would post per review. The average Codex review would land at around 1 to 2 comments, while Opus would post around 7 to 8. I considered two possible explanations:


- GPT comments *less* than it should.
- Opus comments *more* than it should.


Both turned out to be true. GPT searches for bugs depth-first. Opus goes breadth-first.


Across the board, LLM code review traces can be split into three phases:


1. **Scope** : The model reads the diff and understands the changes the PR introduces. By this point, models tend to identify *potential* issues that could be considered bugs.
2. **Investigate** : The model searches the codebase with the goal of gathering evidence to confirm the existence of such bugs.
3. **Summarize** : The model summarizes its findings and produces the final review artifacts.


In the first two phases, the results of each tool call progressively build the context window that will ultimately be used to produce the final result. To understand what was going on, I looked at what share of the context window was built during each phase.


\[ FIG. 04 / TRACE COMPOSITION BY PHASE \]


### Opus *scopes* broadly. GPT *tunnels* straight in.


01


Scope


read the diff


Opus


59.4%


31.5 KB


GPT


6.1%


2.1 KB


02


Investigate


grep, gather evidence


Opus


31.2%


16.6 KB


GPT


82.5%


28.5 KB


03


Summarize


write the review


Opus


9.4%


5.0 KB


GPT


11.4%


3.9 KB


59.4%


31.5 KB


6.1%


2.1 KB


31.2%


16.6 KB


82.5%


28.5 KB


9.4%


5.0 KB


11.4%


3.9 KB


01


Scope


read the diff


02


Investigate


grep, gather evidence


03


Summarize


write the review


Opus 4.7


GPT 5.5


share of visible evidence


The difference was significant. Opus gets most of its context during the **Scope** phase. GPT gets most of it from the **Investigate** phase. I theorized the following:


- Opus takes a preventive approach - willing to comment on things that *could* be bugs.
- GPT places a big emphasis on verifying that what looks wrong is *actually* wrong.


### 4. GPT leaves bugs behind


You would think that a strong emphasis on verification wouldn't hurt results. When curating the dataset, we built our ground truth from high-severity, verified bugs.


Yet initially, GPT had very low recall. It would be easy to blame the model's capabilities, but GPT 5.5 is a frontier model. I was convinced that this wasn't a skill issue.


I tried adding an extra instruction explicitly telling the model to target around 7 to 10 comments per review. Suddenly, it recovered. Not an elegant solution, but it confirmed my suspicion: GPT was leaving bugs behind.


\[ FIG. 05 / TUNNELING FAILURE MODE \]


### GPT *sees the bug* , then *drops it.*


REASONING ARTIFACTS


1. 01


I'm thinking about hash collisions ... This raises the question of deadlocks on numeric locks ...


2. 02


Right now, I have two key issues: 1. A deadlock in batchSet due to per-item advisory locks. 2. A misclassification of primary key constraints related to long custom table names.


3. 03


A deadlock in batchSet can occur under concurrent writes when lock ordering varies across items.


deadlock discovered


4. 04


Examining unique-index issues: nondeterministic LIMIT 1 fallback, replace-delete-insert races on shared documents, assertUniqueIndexes idempotency ...


focuses on unique indices


FINAL REVIEW


1. 01


deadlock in batchSet, not posted


omitted finding


2. 02


\[P2\]


Handle truncated primary-key constraint names ...


posted


REASONING SHARE BY BUG


% of trace tokens


deadlock


20.8%


PK issue


79.2%


reasoning


posted review


omitted finding


example trace from an open source PR


Looking at the reasoning traces made it even clearer. The model would often identify the bugs I expected it to post, explicitly mentioning them as potential issues in its reasoning summaries. Yet it wouldn't post them.


After multiple iterations of tuning extra instructions, I started to see both improvement and the potential root cause: conflicting instructions and post-training reinforced behavior.


\[ FIG. 06 / GUIDANCE RECOVERY \]


### Caught between voices, GPT *posts both* .


D


"only what the author would fix"


vs


U


"be thorough, maximalist"


REASONING ARTIFACTS


1. 01


Developer wants narrow scope, user wants broad. I can be thorough and still valid.


weighs developer


2. 02


Could ignore the user's noisy requests. But if maximalist is wanted, I can include lower priority bugs ...


weighs user


3. 03


Finalizing. Two key issues: 1. Deadlock in batchSet from per-item advisory locks. 2. Misclassified primary-key constraints from long custom table names.


considers all issues


FINAL REVIEW


1. 01


\[P1\]


Deadlock in batchSet under concurrent writes


posted


recovered


2. 02


\[P2\]


Handle truncated primary-key constraint names ...


posted


REASONING SHARE BY BUG


% of trace tokens


deadlock


37.6%


PK issue


62.4%


reasoning


posted review


same trace, with extra guidance


Instructions like "be thorough" or "output all the bugs you can find" would often lead the model into internal debates about whether to follow the *developer* or the *user* instructions. The language of OpenAI's` /review` system prompt leads GPT 5.5 to aggressively narrow the scope of its reviews with the goal minimizing noise.


The performance eventually improved. Nonetheless, I couldn't help but feel that prompting it away from that behavior felt less like crafting a request and more like trying to jailbreak the model.


### 5. Opus wants you to be careful


Opus had the opposite problem. Each review produced far more comments than it should have. At first I assumed the model was just noisy, but the traces told a different story.


Opus tries to review code the way a human would. Instead of focusing strictly on bugs, it concerns itself with intent - hedging on whether something is a bug or a deliberate choice, trying to foresee potential problems, making stylistic suggestions, and even praising code it finds well-written.


\[ FIG. 07 / OPUS COMMENT TYPES \]


### Opus *hedges* , *compliments* , and *warns* .


?


Conditional on intent


this is only a problem depending on what you meant


?


"If the design intent is to spread evenly across full width, auto-fit should be used instead."


?


"Worth confirming this is intentional UX."


?


"If the intent is a 10 ms hold before the exponential ramp, fine, but it reads as duplication."


✓


Structural praise


compliments the shape of the change


✓


"Symmetric write path. Every mutation is paired with the matching add/remove feed call."


✓


"Clear separation of concerns: adapter advertises capability, integration wires the config, infra attaches the layer."


✓


"Migration is purely additive, with a documented manual rollback."


→


Future risk


predicts drift, breakage, aging badly


→


"Consistent with prior style, but a footgun if a future contributor copy-pastes the snippet."


→


"Without a test, the next refactor can silently regress this branch."


→


"If the cache expires while traffic is high, multiple requests will simultaneously run the expensive GROUP BY."


intent


praise


risk


selected from open source PR reviews


Whether this style is useful or annoying is a matter of taste. But a holistic approach without proper verification produces false positives, and false positives are not free. Each one costs an engineer's attention, and in an agentic workflow, each one costs compute. One of the promises of AI code review is that they make verification cheap. The challenge is knowing when to cast a wide net and when to go deep.


## Shipping Model Inversion


The most interesting finding was that Codex catches more bugs in Claude-authored code, and vice versa. Knowing this, we built a small feature we're calling Model Inversion.


It detects which coding agent authored a PR - based on commit trails, branch prefixes, and PR titles - and routes the review to a different model. If Claude wrote it, GPT reviews it, and vice versa.


\[ FIG. 08 / MODEL INVERSION ROUTING \]


### Greptile reads the trail. The reviewer is never the *author* .


01


SIGNAL


read from the PR


commit trail


` Co-authored-by: Claude Opus 4.7`


Opus


PR title


` \[codex\] model inversion`


GPT


branch prefix


` codex/blog-post`


GPT


02


DETECT


who wrote it


Greptile


matches


03


AUTHOR


identified model


authored by


Opus 4.7


authored by


GPT 5.5


04


INVERT


route to the other model


authored by


Opus 4.7


reviewed by


GPT 5.5


authored by


GPT 5.5


reviewed by


Opus 4.7


Opus 4.7


GPT 5.5


**Model inversion is experimental** , and we're still learning how far the effect goes as models improve. Based on our initial research, we're optimistic that it will help our users catch more bugs.


## Closing Thoughts


One thing that stood out from the GPT work: models weight their system instructions heavily, and post-training techniques like[Deliberate Alignment](https://arxiv.org/pdf/2412.16339) encourage them to reason about user intent before acting. In practice, this meant that getting GPT to simply report all the bugs it found was surprisingly difficult.


The model was not disobedient - it was doing exactly what it was trained to do. I believe that it is worth paying attention to the tension between alignment and usefulness.


But setting post-training quirks aside, it is undeniable that models are getting smarter every month. A year ago, the performance difference in the opening figure would likely have been larger.


It is tempting to ask whether the capabilities of frontier models will eventually converge. Two superintelligent forms - gods, knowers of all things - are indistinguishable from each other. But the current landscape looks more like Ancient Greece. Instead of a god of thunder, a god of fire, and a god of the seas, we have a god of race conditions, a god of SQL injections, and a god of user experience.


While that remains the case, we will continue evaluating every model to ensure PRs are routed to the agent most capable of reviewing them.


If you have any feedback on the methodology, questions about the feature, or want to share interesting findings like these with us, send me an email:rodrigo@greptile.com .
