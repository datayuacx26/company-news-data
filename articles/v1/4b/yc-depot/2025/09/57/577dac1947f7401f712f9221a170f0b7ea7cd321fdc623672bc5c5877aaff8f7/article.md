---
schema_version: "1.0.0"
document_id: "577dac1947f7401f712f9221a170f0b7ea7cd321fdc623672bc5c5877aaff8f7"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/collaborate-with-claude-on-docs"
published_at: "2025-09-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:af1402c92be6115b0b3a7d764988e927a87149b669364d84b35d449ce2593953"
---

# Collaborating with Claude on docs

You've built a new feature, tested it, and are ready to finally ship it. You check in with the team to make sure everything is good to go before launching it for customers and then someone asks “Do we have docs for this?”


Documentation often becomes an afterthought in software development, so I want to explore using a Claude agent to help me identify documentation gaps and generate initial drafts for the missing documentation.


I’m using the Depot Claude remote sandboxes, because I think being able to return to my session and share it with the team will be useful. For more information on these, check out this[quickstart guide](https://depot.dev/docs/agents/claude-code/quickstart) .


## The workflow


### Understanding the change


I want to start by making sure that Claude understands the change being made. For the simplicity of this example, I’ll choose a small change represented by a single commit to try out the workflow. I’m going to start a Depot Claude session and provide my own session id so it’s easy to reference again when I next resume it.


```text
depot   claude   \
--org   ORG_ID   \
--repository   https://github.com/depot/cli.git   \
--branch   main   \
--session-id   doc-hiding-summaries   \
"Can you explain the changes from this commit: 4febf18142a6bfafaaad7258b9c34f2cb251cb07"


✓   Claude   sandbox   started!
Session   ID:   doc-hiding-summaries
Link:   https://depot.dev/orgs/ORG_ID/claude/doc-hiding-summaries
```


Going to the provided link, I can see that Claude has a good understanding of this change. For larger changes, you might need more back-and-forth to ensure alignment, but this single commit was straightforward enough.


### Check for documentation gaps


From here, I basically want to know whether or not this is already documented. If so, I may want to expand the documentation given this new change. If it’s not present, I want to add it in general. So we can resume the session and ask it to pull down another repo that hosts our docs and check for us.


```text
depot   claude   \
--org   ORG_ID   \
--resume   doc-hiding-summaries   \
"I want to check if there is already documentation for this feature in our docs repo. \
Can you pull down the docs repo's main branch and check? \
The repository is here: https://github.com/depot/docs.git"
```


It looks like we need to make sure the agent has permission to clone the repository. I’ll ask it to do this again but this time provide the` --allowedTools` flag to specify the tools it’ll be allowed to use so that we don’t run into access issues.


```text
depot   claude   \
--org   ORG_ID   \
--resume   doc-hiding-summaries   \
--allowedTools   "Bash(git:*)"   \
Can you pull down the docs repo's main branch and check? \
The repository is here: https://github.com/depot/docs.git"
```


The` --allowedTools "Bash(git:*)"` flag grants Claude permission to use git commands, which is safer than bypassing all permission checks.


Based on the output, I can see that it successfully cloned the docs repo and checked to see if this feature was already documented (it was not). Great, so we should probably add some new documentation about this environment variable; not just this particular change, but for its usage in general.


### Generate a draft


At this point, I think we know enough to start generating some documentation. I’m going to ask Claude to look at holistically how that environment variable is used to get a full understanding of it. I then want it to pick a place in the docs to add some of this documentation and pitch the change it would make. I’m still pretty hands on when it comes to working with AI as it’s still a newer workflow for me, so I like to see the plan before we make changes.


```text
depot   claude   \
--org   ORG_ID   \
--resume   doc-hiding-summaries   \
"Since there is no documentation for this environment variable at all, can you look at more generally how it's used and what it can do? \
Then I'd like you to find a good place in the docs repo where you think it would make the most sense to add this documentation. \
And finally pitch the documentation change you think should be made based on your findings."
```


I can see that Claude was able to successfully get a more holistic picture of how to use this environment variable, and it was able to find a good place for it. I disagree a bit with where Claude placed it on the page, but overall that seems mostly good to me. I’m going to provide a bit of feedback before doing anything else.


```text
depot   claude   \
--org   ORG_ID   \
--resume   doc-hiding-summaries   \
"I like what you've done. \
Instead of an Environment Variables section after authentication, can you create an FAQ section at the bottom of the page where you generally answer how to minimize the build output and rewrite this in that format"
```


This seems reasonable to me as a starting point and now I want to get feedback from others.


### Back into the codebase


At this point, I will ask Claude to checkout a branch, commit this change, and then create a draft pull request to get this back into the codebase.


```text
depot   claude   \
--org   ORG_ID   \
--resume   doc-hiding-summaries   \
--allowedTools   "Edit"   "Bash(git:*)"   "Bash(gh pr:*)"   \
"Can you create a branch, commit this change, and then create a pull request in the docs repo with this. \
I'd like you to make sure the link to this sandbox is in the description: https://depot.dev/orgs/ORG_ID/claude/doc-hiding-summaries in case anyone needs further context."
```


I have a pull request now which is great and I can see the changes. This is also the point where CI checks are running and I can see that there are some formatting and lint errors. I’ll ask the agent to use our package json scripts to format these changes.


```text
depot   claude   \
--org   ORG_ID   \
--resume   doc-hiding-summaries   \
--allowedTools   "Edit"   "Bash(git:*)"   \
"Can you check the package.json and run the formatter"
```


I officially have a PR with these changes and they're passing all the checks, ready for iterating on and formal review.


A GitHub interface showing a draft PR A GitHub interface showing the file changes from a draft pull request


Now it's time to work with the team to make sure the docs are right and follow our normal standards.


### Team collaboration and context sharing


This is where the collaborative aspect of Depot sandboxes becomes really valuable. When a teammate needs context about the code I'm documenting, they can access the same Claude session via the shared URL. All the context, the original change analysis, repository exploration, and reasoning behind documentation decisions are preserved and easily viewable by others.


For example, if a reviewer questions why we placed the documentation in the FAQ section rather than a dedicated environment variables page, they can ask Claude directly in the same session. Claude has full context of the decision-making process and can explain the reasoning or help iterate on alternatives.


This collaborative aspect eliminates the typical back-and-forth of "Why did you document it this way?" or "Can you explain what this feature actually does?"


## A more complex experiment


The first workflow I tried used a very tiny feature that was entirely undocumented. I want to try this out on a larger feature that was built with a lot more changes and iterations and has some partial documentation. My workflows tend to be more hands-on as I dip my toes into incorporating agents into my workflow, but if you wanted to experiment with more free reign and complex changes you could try a prompt like this:


```text
depot   claude   \
--org   ORG_ID   \
--repository   https://github.com/depot/cli.git   \
--branch   main   \
--session-id   docs-claude-sandboxes   \
--allowedTools   "Edit"   "Bash(git:*)"   "Bash(gh pr:*)"   \
"We have recently added changes to the depot claude command. \
It now defaults to starting claude in a remote sandbox rather than using a local claude on your local machine. \
Can you validate that we have docs for this in the docs repo: https://github.com/depot/docs.git and then add or update them and generally make sure they are up to date. \
I want to show examples both with how to start the depot claude locally and remotely. \
Then please create a branch and commit this change and then create a draft pull request so that i can review it"
```


I tried it, and ultimately this ended up being faster than manually looking through and writing these docs myself. I have tweaks I want to make and this provides a great starting point that I can iterate from. The hardest part of most of these workflows was making sure I had the correct` --allowedTools` configured.


## When this workflow shines


Based on this experiment, I've found this approach works particularly well for:


- **Complex features spanning multiple files** : Claude can analyze the entire codebase to understand feature scope and usage patterns.
- **Cross-repository documentation** : When your docs live in a separate repo from your code, Claude can work across both seamlessly.
- **Post-development documentation** : Great for that common scenario where features are built first and docs come later.


## Limitations


Currently this interaction is limited to using the CLI so there’s no way to kick off these remote sessions via the UI; there’s a lot of back and forth between the terminal and the UI. This may be improved upon in the future, but it’s the current state of the world.


Additionally, the workflow requires some familiarity with the` --allowedTools` syntax to grant appropriate permissions safely. You can use the` --dangerously-skip-permissions` as an alternative here but, as the flag name implies, this may be risky. You'll need to experiment with permissions based on what actions you want Claude to take.


It’s a great starting off point but at this stage, you likely still need human intervention to make sure the docs created are in the right place and capture the right context for what you want documented. These models do a great job but it’s still a good idea to double check everything and push back if something seems incorrect.


## Final thoughts


This is my first time experimenting with this workflow and I wanted to see how I could use these sandboxes to improve my process of adding docs after I’ve finished with a feature. I could see this being especially useful with not-so-tiny changes or when you have different people working on the features than writing the documentation.


There are a lot more iterations of this workflow that can be explored. Some interesting possibilities:


- make this a part of PR process that gets kicked off by a code change
- add context to a particular session over time during development
- point the agent to an existing style guide to ensure consistent documentation


Want to try this yourself? Pick a small feature that needs docs,[set up a Claude session with depot claude](https://depot.dev/docs/agents/claude-code/quickstart) , and walk through the workflow.


## FAQ


What happens to my sandbox session if I have to wait a few hours or days for team feedback?


Depot sandboxes persist between sessions, which is exactly why they work well for documentation workflows. You can resume your session anytime using the same --session-id and all your context, cloned repositories, and conversation history will still be there. This is particularly useful for documentation work that often gets interrupted by other tasks or needs to span multiple days as you wait for team feedback.


How do I handle documentation that requires screenshots or diagrams?


Claude can identify where visual aids would be helpful and create placeholder notes for them, but you'll need to generate the actual images separately. It's useful to have Claude mark these spots with comments like \[TODO: Add screenshot of build output\] so you can easily find them later. Claude can also generate Mermaid syntax for diagrams if your docs support it.


What happens if Claude suggests documentation changes that conflict with our existing style guide?


You can add your style guide to the session context by having Claude read it from your docs repo or by providing it directly. I'd recommend doing this before asking Claude to generate drafts. For example, you can use a prompt like "Please review our style guide at docs/STYLE.md before making any documentation changes." This way Claude can follow your conventions from the start rather than requiring multiple revision rounds.


## Related posts


- [Now available: Remote agent sandboxes on Depot](https://depot.dev/blog/now-available-remote-agent-sandboxes)
- [Using AI as my engineering copilot (not autopilot)](https://depot.dev/blog/using-ai-as-my-engineering-copilot-not-autopilot)
- [How we automated GitHub Actions Runner updates with Claude](https://depot.dev/blog/how-we-automated-github-actions-runner-updates-with-claude)
- [Depot documentation is now open source](https://depot.dev/blog/depot-docs-now-open-source)


Iris Scholten


Staff Engineer at Depot
