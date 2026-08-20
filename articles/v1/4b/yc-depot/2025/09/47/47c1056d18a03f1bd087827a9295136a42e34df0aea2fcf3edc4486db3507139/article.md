---
schema_version: "1.0.0"
document_id: "47c1056d18a03f1bd087827a9295136a42e34df0aea2fcf3edc4486db3507139"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/using-ai-as-my-engineering-copilot-not-autopilot"
published_at: "2025-09-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:cd924f1c4631a62b7b57b540218b784b4171f16f6c8cf71963b7816807e094a9"
---

# Using AI as my engineering copilot (not autopilot)

AI has suddenly become accessible in ways that actually matter for my day-to-day engineering work. I'm not just telling AI to build something and hoping for the best. I still want control over how things are implemented, especially when I'm the one who has to maintain the code.


Let me share how I've integrated AI (primarily Claude) into my workflow while keeping my hands firmly on the wheel.


## The problem with "just make it work"


I recently asked Claude to help with a feature involving switching between table and filtered views on a GitHub Actions log detail page. Technically, it worked. But the implementation? Lots of` useEffect` hooks that would trigger each other to achieve the final result. While it worked, it was a lot more complex to read and reason through which can make it easier to introduce unexpected bugs and behaviors in future changes. If I was reviewing this code in a PR, I would leave comments to see if there was a more straightforward way of achieving this result. There was and with the guidance of "let's do this in a more declarative way that doesn't rely so heavily on useEffects ...", I was able to get a much cleaner solution.


Making something work isn't always enough. As engineers most of our time is spent reading code, not just writing it. If the logic is convoluted and hard to follow, we've solved one problem while creating another.


## How I actually use AI


### Navigating unfamiliar territory


At Depot, we have multiple codebases, and I'm definitely more familiar with some than others. When I need to dive into unfamiliar code, I don't ask Claude to "explain this entire codebase" since that's too broad to be useful. Instead, I focus my questions:


- "I'm looking for where validation happens for this specific type of request. What are all the different contexts?"
- "I found something in this file. Is this relevant to what I'm looking for? What does it actually do?"


While Claude is super powerful, it's not 100% always right, and I often find myself pushing back: "You said this, but I found this other thing that seems to contradict it. Am I misunderstanding?" About half the time, Claude responds with some version of "You know what, you're right." It can be a great starting point while you find your bearings in an unfamiliar codebase allowing you both to learn as you go, but it's always useful to double check.


### Pairing with a very fast typist


I come from a background of formal pair programming where someone is the driver who primarily handles typing and making the changes, and the other is the navigator who guides the direction of what changes are made. With Claude's interactive mode, I've adapted this approach. I'm the navigator; Claude is the driver.


Here's how it works: I'll start with "Here's a page I want as a boilerplate. Create a new one based on this but just render hello world." Then: "Now fetch this specific data from our database" (with details about the where clause and which table to use).


Every step, I'm reviewing the code, making sure it makes sense. The difference from human pairing is the 30-second wait while Claude types, but the principle is the same. I'm guiding the implementation based on how I would build it, just faster.


Sometimes Claude tries to be "helpful" and re-adds things I deliberately removed. So I've learned to say: "Hey, I've made some changes. Start from here and move forward."


In general, I've found using interactive mode to review each change along the way to be useful in making sure Claude is on the right track. This gives me opportunities to tweak direction and also try different implementations out along the way.


### The tedious stuff


AI really shines here:


- Comparing log files to spot timing differences or anomalies (so much faster than my human eyes)
- Making a specific component generic so I can reuse it
- Finding every place in the codebase where we update certain records so I can add a new field consistently


For verification, I'll implement something myself, then ask: "I intended to do X. Did I miss anywhere?" It's like having a second pair of eyes that never gets tired.


### Data visualization and queries


We've set up MCP to give Claude read-only access to some databases. Now I can ask: "Have we seen an increase in build failures in the last hour?" Claude figures out which tables to query, writes the SQL, executes it, and gives me a graph with the most recent failing builds.


There are still times when I'm writing one off queries, but Claude does a great job especially across multiple databases pulling relevant data to help me identify and visualize any trends that may be relevant.


### Workflow intelligence


These agents help with:


- Providing insight into the key changes made since the last time I looked at some code.
- Acting as an automated code reviewer that catches when my PR description says one thing but the implementation does another (great at finding typos, occasionally catches real issues)
- Generating test cases based on existing code (though you still need human judgment about what's worth testing)


All of these workflows work great, but they have one major limitation.


### Remote agents for holding context


One of the limitations with using Claude sessions is losing context from session to session. Switching machines and switching context can result in starting from scratch since it doesn't remember the previous conversations or decisions made along the way.


This is where Depot's[remote agent sandboxes](https://depot.dev/docs/agents/overview) can help. Instead of running Claude locally, it runs in a persistent cloud environment with full access to your project files, git history, and terminal.


This can be useful when I want to tell it to periodically do something without having to keep providing the same context, and I don't need the interactive mode. You can get started with our[Claude Code quickstart guide](https://depot.dev/docs/agents/claude-code/quickstart) .


## The bottom line


AI has been a very powerful tool to accelerate my work, not replace my thinking. It's been helpful with understanding unfamiliar concepts and codebases while also still enabling me to have control over the architecture and implementation of changes I make. It can do a lot of the tedious work for you and implement a surprising amount of changes on its own, but it can work even better to create maintainable solutions when you provide insight and guidance along the way.


The key is maintaining that balance: letting AI handle the tedious parts while keeping control over the architecture, patterns, and maintainability of the code. Because at the end of the day, I'm not just trying to make something work. I'm building something that my team and I will need to understand, modify, and maintain for months or years to come.


## FAQ


What's the difference between AI as a copilot vs autopilot?


Using AI as an autopilot means letting it make architectural decisions and write code without oversight. As a copilot, you guide the implementation while AI handles the mechanical work—like having a very fast typist who follows your directions.


How can I share Claude Code sessions with my team?


Depot's Claude Code sessions allow you to share and resume AI coding sessions across your entire team. Anyone in your organization can pick up where you left off, making collaboration with AI agents seamless.


Can I run Claude Code in CI/CD pipelines?


Yes! With Depot's remote agent sandboxes and Claude Code in GitHub Actions, you can run AI coding agents in your CI workflows. This enables automated code reviews, updates, and maintenance tasks directly in your development pipeline.


What are remote agent sandboxes?


Remote agent sandboxes provide persistent cloud environments where AI agents like Claude Code can work with full filesystem context, Git integration, and project history. No more starting from scratch—your AI agent maintains context across sessions.


## Related posts


- [Now available: Remote agent sandboxes on Depot](https://depot.dev/blog/now-available-remote-agent-sandboxes)
- [Now available: Claude Code sessions in Depot](https://depot.dev/blog/now-available-claude-code-sessions-in-depot)
- [Faster Claude Code agents in GitHub Actions](https://depot.dev/blog/claude-code-in-github-actions)
- [How we automated GitHub Actions Runner updates with Claude](https://depot.dev/blog/how-we-automated-github-actions-runner-updates-with-claude)


Iris Scholten


Staff Engineer at Depot
