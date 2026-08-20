---
schema_version: "1.0.0"
document_id: "c0482b78d72355be04ea04758f2a48f8a9b70364bdc9b454193bad610e09e64c"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-remote-agent-sandboxes"
published_at: "2025-08-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:52f4f97810cdf91274742a9289f33f0e16780b3f7df7bb981ee11cc42e2a2a59"
---

# Now available: Remote agent sandboxes on Depot

Just last month, we launched the ability for Depot to persist your Claude Code sessions, allowing you to share and resume them across machines and environments. This has been a game changer for our team, enabling us to collaborate more effectively with AI coding agents.


After heavy use, it became increasingly obvious that we needed to take the next step: to equip our AI agents with a persistent, remote sandbox, with full access to a filesystem and project context. This would help us leverage the power of AI coding agents in a way that feels natural and seamlessly embedded into our existing workflows.


So, today, we're rolling out remote agent sandboxes on Depot, starting with remote Claude Code.


## What we were still missing


One of the biggest issues we've had adopting Claude Code, particularly in async or remote settings like[running Claude Code via GitHub Actions](https://depot.dev/blog/claude-code-in-github-actions) , is that the agent *always* starts from scratch. Why? Because CI environments are ephemeral by default, making it difficult to preserve work across sessions.


Resuming a session in CI restores the conversation in a fresh environment. Important tools need to be reinstalled. Access to remote infrastructure needs to be reconfigured. Code needs to be cloned and checked out again. All of this reinitialization is necessary but tedious, and ultimately amounts to wasted effort.


In pursuit of a simpler workflow, we've taken the architecture used to power the rest of Depot and brought it to remote agent sandboxes, with immediate support for remote Claude Code agents.


## Introducing remote Claude Code sandboxes


By default,` depot claude` will now launch a remote agent sandbox. If you prefer to keep your agent locally, pass` --local` to persist the Claude Code session to Depot, but note that all execution will occur locally on your machine.


There are a few key benefits and features that power our remote agent sandboxes:


1. **Isolated environments:** Each agent sandbox runs in its own isolated container backed with **2 vCPUs and 4GB of RAM** and typically starts in under 5 seconds.
2. **Persistent filesystems:** Each agent sandbox has a persistent filesystem that is automatically mounted with the codebase and context of your project. This means the agent can pick up where it left off, with important tools and code changes restored whenever a session is resumed.
3. **Git integration:** The agent sandbox automatically integrates with your Git provider, allowing agents to check out code, commit changes, and open pull requests. This means you can have agents work on features, fix bugs, and even review pull requests without needing to manually manage the code.


With Depot remote sandboxes, agents can run in the background, with dedicated resources, and can be granted access to your code, all while maintaining the context and filesystem of your project. On top of that, you get the centralized session management for Claude Code, so you can create, resume, manage, and share your code sessions across your entire organization.


## Getting started with remote Claude Code


To get started with remote Claude Code, you first need to connect Depot agent sandboxes to your Anthropic account & grant sandboxes access to your code in GitHub (or other Git providers).


### Configuring your Anthropic account


Depot agent sandboxes don't come pre-configured with their own Claude credentials, so you will need to provide your own.


There are two potential keys you can use:


1. **Claude Code token for Max plan:** If you're on a Max plan with Anthropic, you can generate a Claude Code token by running` claude setup-token` .
2. **Anthropic API key:** If you want to make use of an API key in your Anthropic account, you can generate one by following[this guide](https://docs.anthropic.com/en/api/overview) from Anthropic.


### Set your Anthropic credentials


In the latest version of the[Depot CLI](https://depot.dev/docs/cli/reference) , you can add secrets to your Depot organization that allow you to define environment variables that will be passed through to sandboxes.


Add your Anthropic credentials to your Depot organization:


#### Max plan subscribers


For Max plan subscribers who have run the` claude setup-token` command, you can add your Claude Code token to your Depot organization by running:


```text
depot   claude   secrets   add   CLAUDE_CODE_OAUTH_TOKEN   --value   <  your-toke  n  >
```


#### Anthropic API key users


If you are using an Anthropic API key, you can add it to your Depot organization by running:


```text
depot   claude   secrets   add   ANTHROPIC_API_KEY   --value   <  your-anthropic-api-ke  y  >
```


### Configuring Git access


To allow Depot agent sandboxes and Claude Code to access your code, open pull requests, and commit changes, you need to configure Git access.


#### GitHub App


If your source control lives in GitHub, we've deployed a new app called` Depot Code` that you can install in your GitHub organization and choose the repositories that the remote agent sandboxes should have access to.


1. Login to your Depot account and select your organization
2. Click on the Settings tab in the sidebar
3. Scroll down to the GitHub Code Access section
4. Click Connect to GitHub


#### Other Git providers


If you're not using GitHub for source control, you can still grant remote agents access to your code by generating and adding an access token to your Depot organization.


```text
depot   claude   secrets   add   GIT_CREDENTIALS   --value   <  your-ke  y  >
```


## Getting started with remote Claude Code sandboxes


Once you've set up your Anthropic credentials and Git access, you're ready to start using remote Claude Code sessions in Depot.


### Start a new sandbox


To get started, you can fire up a very basic example of a remote Claude Code agent on Depot. In this example, we'll ask the agent to give us a quick summary of everything that has happened in our public docs repo over the past 30 days.


```text
depot   claude   \
--session-id   repo-summary   \
--repository   https://github.com/depot/docs   \
--branch   main   \
"Give me a summary of the activity against this repo over the past 30 days"
```


Agent sandboxes on Depot today are fully **async** . This command immediately returns a session URL that you can use to view the session and its results in your browser.


```text
✓   Claude   sandbox   started!
Session   ID:   repo-summary
Link:   https://depot.dev/orgs/1219818893183/claude/repo-summary
```


Opening that session url in a browser, we can see the agent results and full session details in one place.


[Remote Claude Code agent on Depot with Tool Error](https://depot.dev/images/remote-claude-code-depot-1.webp)


Here we've successfully started the sandbox with Claude Code processing our prompt. But, we see that Claude wanted to make use of the` Bash` tool to help describe the recent commits against this repository. That failed, but the command to get recent committers worked.


Once the agent was able to log what it was able to get to from the summary, the sandbox then automatically shut down.


### Resume a sandbox


We can resume this session with the same sandbox and filesystem. But, this time we will use the` --allowedTools` flag to grant the agent access to the` Bash` tool, allowing it to run shell commands in the sandbox.


```text
depot   claude   \
--resume   repo-summary   \
"Give me a summary of the activity against this repo over the past 30 days by looking at the git history."   \
--allowedTools   "Bash"
```


This starts the same sandbox as before, so our session url to monitor progress is the same. This time, however, we see that the agent successfully ran the` Bash` tool and retrieved the recent commits against the repository.


[Remote Claude Code agent on Depot with Bash Tool](https://depot.dev/images/remote-claude-code-depot-2.webp)


Now we can see the latest output from the agent doesn't have any tool call errors, and it was able to invoke Bash as expected against our docs repo.


An important thing to note is that the` --repository` and` --branch` flags are **only needed when creating a session** , as that is when the sandbox is initialized. Thereafter, Claude Code will operate against that branch until prompted to checkout something else.


### Fork a sandbox


You can fork an existing session and its sandbox filesystem into a brand new session by using the` --resume` and` --session-id` flags together. Using our example above, we could fork the` repo-summary` session like this:


```text
depot   claude   \
--resume   repo-summary   \
--session-id   repo-summary-fork   \
"When was the last commit against this repository?"   \
--allowedTools   "Bash"
```


Note that a new session url is output, one for the forked session.


```text
✓   Claude   sandbox   started!
Session   ID:   repo-summary-fork
Link:   https://depot.dev/orgs/1219818893183/claude/repo-summary-fork
```


When we open up that new forked session in Depot, we see that it has the previous session's state and filesystem.


[Forking a Remote Claude Code sessions in Depot](https://depot.dev/images/remote-claude-code-depot-3.webp)


#### Forking is a lot like base images


Forking sessions are particularly powerful for creating a reusable session. You can have a "base" session that sets up the environment with all of the developer tools you want or need, configures scripts, and sets up a local development environment at the file system level. You can then use the base session to create new agents operating on different pieces of work.


## Advanced usage


Once you've mastered the basics of starting, resuming, and forking sandboxes, you can dive into integrating remote Claude Code agents into your workflows.


### Commit changes and open pull requests


Let's have Claude Code operate on a branch we create to update the` README` with the general structure of the repository.


Before we start, we first create a branch and push it to upstream called` readme-update` .


Then we can launch an agent with our prompt:


```text
depot   claude   \
--session-id   readme-update   \
--repository   https://github.com/depot/docs   \
--branch   readme-update   \
"Take a look at the general structure of this repo and update the README with a summary of the repo and its general structure. Once done, commit your changes to the branch, push to the upstream, and open a pull request to have your changes reviewed."   \
--allowedTools   "Bash Edit Read Write"
```


This launches a brand new Claude Code sandbox that checks out our new branch, explores the repository, updates the` README.md` file, commits the changes, pushes the branch to upstream, and opens a pull request. We can take a look at the agent session inside Depot to see the full progress.


[Opening a pull request with remote Claude Code in Depot](https://depot.dev/images/remote-claude-code-depot-pr.webp)


We can see it worked and even see the actual PR inside of[depot/docs](https://github.com/depot/docs/pull/134) on GitHub. If you look closely, its first commit wasn't actually perfect, as it failed CI due to some lint rules we have in place.


[CI errors with Claude Code](https://depot.dev/images/remote-claude-code-commits.webp)


However, with the ability to resume any Claude Code sandbox in Depot, we resumed the sandbox via` --resume readme-update` and then asked the agent to fix the lint errors. It was able to do so, commit the changes, and push them to the same branch.


## Pricing


Agent sandboxes are available on all Depot plans. They are priced at **$0.01/minute** , tracked by the second with no one minute minimums. A sandbox is considered "active" when the agent is running and processing your prompts. When the agent exits, the sandbox will automatically shut down.


## What could be better?


There are some limitations to remote agent sandboxes that we are working on improving. Here are a few things to be aware of that we've brushed up against:


1. **Async only:** With our remote agent sandboxes, we are currently only supporting async sessions. This means that the` depot claude` command will return immediately with a session URL, and the agent will run in the background. So this isn't a great fit if you do a lot of interactive development with Claude.
2. **Session monitoring:** Today, you have to go into the Depot UI to monitor the progress of your sessions and their output.
3. **Claude Code only:** At the moment, the first agent we support is remote Claude Code.
4. **Tools support:** Currently, we only support the tools that Claude Code comes pre-configured with. You can specify things like your own MCP servers and the like via the` claude` CLI flags and config files.


Even with those limitations, we've found quite a bit of power in being able to fire off a background task to a Claude Code agent and have it run in a remote sandbox in the background, iterating on its code, and ultimately opening a pull request with the changes it made.


## Conclusion


We built this because, as heavy users of Claude Code ourselves, we were getting tired of "starting from scratch" when running agents in an async environment like GitHub Actions. We had to restore the filesystem state, install dependencies, and reconfigure the agent every time we wanted to run a new prompt. We were persisting the session state and metadata, but not the actual filesystem the agent was working against.


So, in traditional Depot fashion, we went and solved our own problem by launching agent sandboxes on Depot that automatically persist their state, can be resumed, and even forked into brand new sessions reusing the same filesystem.


We know there are limitations at the moment, but we've found it already incredibly powerful to delegate tasks to a Claude Code agent running in a remote sandbox in the background.


We can't wait to see what you do with it! If you have any questions, feedback, or ideas for how we can make this even better, please hop into our[Community Discord](https://discord.gg/XpTfcVrr46) and let us know!


## Related posts


- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)
- [Now available: Egress filtering for GitHub Actions Runners](https://depot.dev/blog/now-available-egress-filtering-for-github-actions-runners)
- [How to calculate your real GitHub Actions usage in minutes](https://depot.dev/blog/how-to-calculate-github-actions-usage)
- [Faster Claude Code agents in GitHub Actions](https://depot.dev/blog/claude-code-in-github-actions)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
