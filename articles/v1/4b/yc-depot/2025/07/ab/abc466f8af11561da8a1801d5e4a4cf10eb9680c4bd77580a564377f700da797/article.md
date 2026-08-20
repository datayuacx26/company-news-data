---
schema_version: "1.0.0"
document_id: "abc466f8af11561da8a1801d5e4a4cf10eb9680c4bd77580a564377f700da797"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-claude-code-sessions-in-depot"
published_at: "2025-07-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:a531bdb5d8cf43a0724b8e1b08c128dcb1757e66fd17385731e54d0e91de40a8"
---

# Now available: Claude Code sessions in Depot

We've been using Claude Code at Depot since pretty much the moment it dropped. It's been a game changer for everything from our day to day development to debugging production issues. However, after using it for a while, we realized that we were all getting really annoyed by how challenging it was to share sessions with each other or resume them in ephemeral environments (like CI jobs!).


So, in traditional Depot fashion, we went and fixed our own problem, and today we're releasing Claude Code sessions in Depot. Now available to all Depot users on any of our pricing plans.


## So, what are Claude Code sessions in Depot?


We built a new command for the Depot CLI,` depot claude` , that allows you to create, resume, and manage Claude Code sessions across your entire organization. This means you can now save your AI coding sessions in Depot, share them with your team, and pick up where you left off—no matter which machine or environment you're working in.


With Claude Code sessions in Depot, you can now get all of the following right out of the box:


- **Developers and AI agents** - Hand off work between human developers and automated Claude agents
- **Team members** - Share complex problem-solving sessions across time zones and teams
- **Local and CI environments** - Start debugging locally and continue in CI, or vice versa
- **Different stages of development** - Maintain context from design through implementation to review


### How do you use Claude Code sessions in Depot?


To make use of Claude Code sessions in Depot, you first need to make sure you have the[Depot CLI installed](https://depot.dev/docs/cli/installation) .


With the CLI installed, run` depot login` to authenticate your local CLI with your Depot organization. This is required to ensure your sessions are securely stored and accessible across your team.


With that, you're ready to start using Claude Code sessions in Depot. To create and manage these sessions, you simply use the` depot claude` command instead of the regular` claude` command. This command automatically handles session persistence, allowing you to create and resume sessions with ease.


```text
# Start a new session with a custom ID
depot   claude   --session-id   feature-auth-redesign
```


This opens Claude Code, but it is wrapped in Depot's session management. You can interact with Claude as you normally would, but your session state is now saved to your Depot organization. Every interaction you have with Claude will be stored securely in your organization, allowing you to resume it later from any machine or environment.


If you don't specify a session ID, Depot will generate one automatically for you. You can also resume existing sessions by using the` --resume` flag with the session ID.


```text
# Resume an existing session by ID
depot   claude   --resume   feature-auth-redesign
```


You can pass any prompts or parameters you want to Claude, just like you would with the regular` claude` command. For example, you can specify a prompt to continue working on a specific task:


```text
# Resume a session and provide a prompt
depot   claude   --resume   feature-auth-redesign   --model   opus   -p   "continue implementing the authentication flow"
```


You can even use this from CI as well! Install Claude Code in your CI environment, authenticate with Depot, and then run the` depot claude` command to resume a session and continue working on it.


```text
# In your GitHub Actions workflow
-   uses  :   depot/setup-action@v1
-   run  :   npm install @anthropic-ai/claude-code
-   run  :   |
depot claude --resume pr-${{ github.event.pull_request.number }} \
-p "review this PR for security issues and best practices"
```


### Uhhh, I don't remember my session?


Yeah, we had the same problem. So we added a way to list all of your sessions so you can see what you have available to resume. You can list all available sessions in your organization using the` list-sessions` command:


```text
depot   claude   list-sessions
```


From there, you can choose to resume any sessions by selecting the session from the list:


When you resume a session, Depot will automatically load the last state of that session, including any code, prompts, and context you had previously set up. This means you can seamlessly continue your work without losing any progress. Once you exit again, your session state is saved back to Depot, ready for you or your teammates to pick up later.


### Where could this be better?


There are a few areas worth being aware of as you begin using Depot's Claude Code sessions. Here is a quick list of things we know about that we are working on improving:


-


Work produced in sessions are not automatically committed to your git repository. So, if you start a session, we don't automatically create a branch for your session that Claude will work on. This is something we are looking to improve in the future, but for now, we recommend creating a branch for your session and naming your session after that branch. This way, you can easily find the work done in that session.


-


Claude Code must still be installed in your environment. Depot's Claude Code sessions are a wrapper around the existing Claude Code functionality, so you still need to have the[Claude Code CLI installed](https://docs.anthropic.com/en/docs/claude-code/overview) and configured in your environment. Depot's sessions make it easier to manage and resume those sessions across different machines and environments.


## How does it actually work?


The` claude` CLI already saves each of its session in a line-delimited JSON file in` $HOME/.claude` . To enable cross-machine session sharing then, the` depot claude` command executes the` claude` binary already on your machine and watches for changes to that session file on disk, saving any changes in it via the Depot API.


To resume a session, the` depot` CLI checks the name of the session being resumed, fetches its contents from the Depot API and writes them to disk, then executes the` claude` CLI.


In this way sessions can be named, shared, and resumed from anywhere!


## Why we built this


Honestly? Because it was fun and we needed it ourselves. But also because we believe that AI coding agents are going to fundamentally change how we build software. They are already helping us write code faster, debug issues more effectively, and even design new features.


This makes our own use of Claude Code significantly more powerful. By allowing AI agents to maintain context across sessions, we can leverage their capabilities in a way that feels natural and integrated into our existing workflows.


We can have a Claude Code agent start working on a complex feature, hand it off to a human developer for review, and then have the agent pick up where it left off—all while maintaining full context of the conversation and code changes. That feature can then be submitted as a pull request, where other agents review it, and we can pick up the session of that review agent to address any feedback.


It's powerful stuff, and we think it will be a helpful tool to accelerate teams even more.


## Conclusion


Depot is focused on building tools and services that accelerate delivery pipelines. We started by accelerating Docker image builds, then added our technology and expertise to GitHub Actions, and followed up with our own remote cache service, Depot Cache.


The combo of all these tools is helping teams build and ship software faster than ever before.


With the addition of Claude Code sessions, we're taking another step towards accelerating a new facet of the software delivery pipeline by making it faster & simpler for engineers and coding agents to interact on the same codebase.


We hope you enjoy this as much as we do. If you have any questions, feedback, or ideas for how we can make this even better, please hop into our[Community Discord](https://discord.gg/XpTfcVrr46) and let us know!


## Related posts


- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)
- [Now available: Egress filtering for GitHub Actions Runners](https://depot.dev/blog/now-available-egress-filtering-for-github-actions-runners)
- [How to calculate your real GitHub Actions usage in minutes](https://depot.dev/blog/how-to-calculate-github-actions-usage)
- [Faster Claude Code agents in GitHub Actions](https://depot.dev/blog/claude-code-in-github-actions)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
