---
schema_version: "1.0.0"
document_id: "5d0aad800b7409a90e00b6417897573431ea62f59e11fbd8cb99d3ef31493ab8"
company_key: "yc-stagewise"
company: "stagewise"
source_id: "yc-stagewise-news-import-0e327623c986"
canonical_url: "https://stagewise.io/news/release-week-april-20-26"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-22T14:44:14.475585+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:36a3a8ce88928071dc16b4548139ef84b03c8004dea51d2623141b86709d5bfe"
---

# Release Week: April 20-26 · stagewise Newsroom

## What Shipped Last Week


During the last week, we shipped **Alpha 64** through **Alpha 72** . Let's talk about the biggest changes!


### Persistent shell support


We shipped support for long-running shell sessions for stagewise agents, allowing our agent to not only execute one-shot commands, but also own and control **long-running shell commands like dev servers, build processes** and much more. The agent is now also able to navigate interactive apps that require keyboard navigation.


This marks the first step of stagewise integrating more terminal-based capabilities for both the agent and the user.


### First-Day Support for Kimi K2.6


We shipped support for **Kimi K2.6** on its release day, giving users access to **Moonshot.AI** 's latest and greatest model, combining strong agentic capabilities with a strong pricing model. We're excited to get feedback on what your experience with the latest Kimi model inside stagewise looks like!


### Renamable agents


As part of making it easier to keep control of all agent instances, stagewise now allows you to rename individual agents. Simply click on the title of an agent in the active agents section to enable the edit mode.


Changing the name of an agent also permanently disables the continuous generation of fresh agent titles, allowing you to easily remember which agent did what.


### Performance improvements


Users with large chat histories with the agent might have noticed significant drops in performance. We found and fixed the underlying issue, which should result in a much smoother experience especially for our power users.


Additionally, we identified and resolved a performance bottleneck with the agent-internal file diff service, which led to lags in the application on edits of larger files.


### Advanced source file metadata for agents


Release **Alpha 70** shipped an AST preview for several source file types, allowing the agent to make more precise and efficient read calls of large source files. Compared to the previous version that only shipped file-type agnostic information like file size or line count, the agent now immediately receives information about the location of function bodies etc., helping to make more targeted changes.
