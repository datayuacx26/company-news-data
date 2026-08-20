---
schema_version: "1.0.0"
document_id: "bda5a30e45993b7b9bca94dcb3069d463a93d5b4f5a3b5a0409512a0255d6bd7"
company_key: "yc-whatnot"
company: "Whatnot"
source_id: "yc-whatnot-rss-30861744a6f8"
canonical_url: "https://medium.com/whatnot-engineering/the-souffl%C3%A9-problem-one-regex-30-fewer-tokens-5026e6841cf4"
published_at: "2026-06-23T16:01:03+00:00"
first_seen_at: "2026-07-24T07:07:55.027426+00:00"
fetched_at: "2026-08-20T03:40:21.870140+00:00"
content_hash: "sha256:78cb40e7d42c0bbded63514623c0855c35ea7575a5e5eea5121aea972194d416"
---

# The Soufflé Problem: One Regex, 30% Fewer Tokens

***Our AI agent was burning tokens on every turn by processing build noise that provided no signal. Context windows clogged with irrelevant logs, forcing the agent to waste cycles and thrash through unnecessary tool calls.***


***We solved this with one line of regex; a filter that cut token consumption by up to 30% and, more importantly, stopped the agent from flailing. It’s not just a cost-saving trick; it makes the agent’s behavior more predictable, spending its budget on the fix instead of the hunt.***


To grok this, imagine a chef tasked with making a soufflé with ingredients but no recipe.


An expert chef, using school-honed skills, might succeed immediately or through a few refined attempts. Success is inevitable because they possess the competence to iterate. A mediocre chef eventually succeeds too, but wastes far more food in the process.


AI coding agents operate similarly when facing unknowns. They iterate, try, and fail before reaching an answer. They are capable enough to succeed, but they waste significant compute and tokens during the process. The solution isn’t rocket science, but just really good filtering that is specific to tasks that can be token-heavy. Here’s an example of that below.


For the rest of the post we will do a deep dive into what we did, how it works, and you can ultimately use the recipe for your coding agents.


### The asymmetry


Coding agents with capable LLMs are good at knowing **which tool to call** . They pick the right command as they work towards their objective. But they are bad at **predicting what the output will look like** and where in that output to focus.


The agent fires off a build command. Hundreds of lines of build output land in its context window. The relevant compiler error is buried somewhere in the middle.


Reading through a log is second nature to you; you scan the text without a second thought. You scroll. Your eye snags on red text, stack traces, the word FAILED. You find what matters in seconds, and the rest of the output costs you nothing. It sits in your scrollback, and you forget it was ever there.


For an agent, there is no such thing as free output. Every line lands in the context window. Every turn after that, the harness sends the whole context back to the model. The noise adds up and burns tokens on every turn. In a bash-heavy environment (like Android development), where a single task can fire multiple commands, you get the compounding effect of wasted tokens. Context fills up. Token consumption grows on every turn. The lines you would have ignored, the agent goes on paying for, again and again, for the rest of the session.


It will improvise. tail -20. Then tail -50. Then head -30. Then a grep with a pattern it invented on the spot. Then it dumps the output to a file and starts reading it in chunks from the end.


The smarter the agent gets, the more confidently it improvises, and the more it can waste doing it. A weaker agent will just keep cycling. tail, head, grep, different tail, different grep, a fresh approach every minute, none of them working, until you notice and stop it.


Just like the chef without the recipe, you don’t know in advance how many tries the agent will take to tackle its task. If you provide a clear recipe for the domain space it is working in, then you get two things: (1) More deterministic behaviors for each run and (2) reduced token burn!


### The recipe


The solution rests on a single core principle: **eliminating output ambiguity** . By predefining the expected command output and highlighting the critical data points, you eliminate the need for agent improvisation. Instead of forcing the agent to devise its own filtering logic on the fly, you provide a pre-built filter to guide it.


In our implementation, we achieved this by documenting the “recipe” within AGENTS.md (or your preferred config file). This includes a specific named filter and concise guidelines on when to apply it. *To strengthen this further, a pre-tool hook could be used to block any build requests that lack a filter, making errors significantly less likely* . For our purposes, however, maintaining these instructions in the markdown files has proven effective.


The concept is straightforward: **route the command output through a filter designed to retain only the essential details of progress (such as errors, failures, and final results) while discarding the surrounding noise** .


```text
<build-or-test command> 2>&1 | rg "<the lines that signal success or failure in your stack>"
```


### Real-world example


In our Android repo,[Gradle](https://gradle.org/) was the worst offender. Android builds spit out logs hundreds (if not thousands) of lines long, with the relevant lines being buried somewhere along the way. Our filter, defined once in the docs and referenced everywhere by name, looks like this:


```text
2>&1 | rg "^e: |FAILED|FAILURE:|Cannot locate|tests completed|BUILD (SUCCESSFUL|FAILED)|problems-report|AssertionFailedError|AssertionError|Exception( at|:)"
```


The output is distilled from hundreds of lines down to a mere handful … twenty at most.


By removing the surrounding noise, such as JVM warm-up sequences, dependency resolutions, configuration phases, and plugin chatter, the agent is left only with what truly matters: compiler errors, test failures, exception classes, and the final build outcome.


Each token included in the filter was selected to address a particular type of failure:


- ^e: is anchored because the unanchored version was matching e: fragments that appear in normal Gradle task output. The agent treated those matches as compile errors and burned cycles analysing a build that had compiled fine.
- Cannot locate got added because misnamed tasks amplified the noise problem. The agent would typo a task, see FAILED at the tail of the output, assume compile errors, and start fixing code that wasn't broken. Gradle had already said the task didn't exist. That line just sat higher up than tail -10 reaches.
- AssertionFailedError is there to surface why a test failed, not just that it failed. Without it, the agent would see FAILED next to a test name and have to guess at the fix from the name alone.
- … and so on for every other token. Each one *earned* by a specific failure and protecting the agent from a specific kind of confusion.


> *You’ll notice the filter mixes* ***test*** *failure tokens with* ***compile*** *output ones. That’s deliberate. We use one filter for everything, not one for compiles and another for tests, because conditional rules are exactly the kind of thing agents quietly forget. The* ***test*** *tokens just don’t match anything when the filter runs on* ***compile*** *output.*


The following comparison illustrates the impact of applying this recipe. While the specific implementation focuses on Android and Gradle, the underlying principle is universal: **any tool generating excessive output for an agent to process efficiently can be improved by the same strategy. By defining expected output and highlighting critical segments beforehand, you empower the agent to identify which parts of the stream truly require its attention.**


> **The regex is the recipe. The chef is the agent. Hand them the recipe, and even a mediocre one ships a soufflé.**


### The proof is in the pudding


Does the filter actually work, or is it just theater? A shorter log is obviously cheaper, but if the agent loses the plot because it lacks context, the savings are a trap. We ran the numbers to find out.


Across sixty-plus runs on two constructed Android fix tasks (seeded errors, not observed production sessions), the filtered agent and the unfiltered agent reached the same finish line every single time. Accuracy didn’t budge. But the path they took to get there was radically different.


The real win isn’t just shrinking the payload; it’s stopping the thrash. When an agent gets a thousand lines of noise, it panics. It starts guessing. It fires off a tail, then a grep, then reads the file in chunks. Each new tool call drags the old noise along and burns tokens on it again. That is the **compounding effect of wasted tokens** : you do not pay for the junk once, you re-pay for it every turn after.


By handing the agent the signal on a silver platter, we cut transcript size by 20% on the simpler fix and ~30% on the cascade-style failure. The more complex the build failure, the more the unfiltered agent flails, and the more the recipe saves. You’re not just saving pennies on tokens; you’re buying back the rounds that never should have happened in the first place.


### Four lessons


Months into production, four lessons stand out.


**Most of the work is around the rule, not in it.** The regex is small. Most changes happen to the rules around it: when to use it, what never to do with it, how to apply it across tools. The filter is the mechanism, but the instructions are what make it reliable.


**There is no final form.** The filter will keep changing as the development environment changes. In a few months, it will look different, with new patterns added and others dropped. It’s a living document, not a finished artifact.


**One filter, not two.** If your docs carry two slightly different versions of the same rule, the agent latches onto whichever it hits first and uses it everywhere, forgetting the conditional logic in AGENTS.md about which belongs where. We had exactly that: two patterns drifting in two corners of the docs, one excluding test failures, one not. Both sort of worked … until they didn’t. The fix was a single pattern, defined once and referenced by name. Nothing to choose between, nothing to remember.


**Forbid mutation.** Given the slightest opening, agents will try to “improve” your filter for the one case they’re working on. So we added this line to the doc:


> Append this filter exactly as-is. Do not add, remove, or change any patterns. Not even “just one more thing” for a specific situation. If the filter misses something, run the command without a filter as a separate follow-up call.


The phrase “not even just one more thing” is load-bearing. The regex does the filtering; the sentence keeps it from being quietly rewritten into something useless.


Smarter models will keep improving how agents pick tools and reason about problems. Recipes are what improve the parts in between, and recipes compound across every agent that touches your codebase. An afternoon spent writing one good filter outlives many model upgrades.


---


[The Soufflé Problem: One Regex, 30% Fewer Tokens](https://medium.com/whatnot-engineering/the-souffl%C3%A9-problem-one-regex-30-fewer-tokens-5026e6841cf4) was originally published in[Whatnot Engineering](https://medium.com/whatnot-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
