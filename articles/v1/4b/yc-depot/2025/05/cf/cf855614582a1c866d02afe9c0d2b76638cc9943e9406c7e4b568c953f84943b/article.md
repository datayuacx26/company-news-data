---
schema_version: "1.0.0"
document_id: "cf855614582a1c866d02afe9c0d2b76638cc9943e9406c7e4b568c953f84943b"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/claude-code-in-github-actions"
published_at: "2025-05-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:cc8b45a9436f2a576cd691939dc8a0ae93c728d9d34d3bcb6d58083338ece0ab"
---

# Faster Claude Code agents in GitHub Actions

It's been an exciting week as the news starts rolling in about bringing agentic coding out of the IDE and your own terminal and into other parts of our development workflows. Anthropic has brought my favorite agentic coding interaction, Claude Code, to GitHub Actions.


This means that we can now use Claude Code to write and run code by simply asking Claude in pull requests and issues. Behind the scenes, Claude Code runs inside of GitHub Actions workflows that we control, making it easier to automate tasks and improve our development processes.


In this post, we're going to explore how to use Claude Code in GitHub Actions and how to make it even faster and cheaper with the power of Depot GitHub Actions runners.


## What is Claude Code?


Claude Code is an agentic coding tool that lives in your terminal and has a deep understanding of your codebase. It can help you code faster by giving it specific prompts to go work on in the background. Until now, it's been limited to running on your local machine as the only supported path. However, others have been working on bringing it to more places, like GitHub Actions.


But now, an official Claude Code GitHub App and an official GitHub Action make it even easier to use Claude Code in your GitHub Actions workflows. This is a game changer, as it allows you to use GitHub Actions as a background automation platform. Multiple Claude Code agents work on your codebase in parallel, processing different tasks simultaneously, significantly speeding up your development process.


## Configuring Claude Code in GitHub Actions


To get started, it's recommended that you install the official GitHub app for Claude. This app allows you to tag` @claude` in your issues and PRs with a specific prompt to go work on.


Installing the app can actually be done directly from` claude` in your terminal:


```text
claude   /install-github-app
```


This installs the GitHub App to your repository, adds your Anthropic API key as a secret, and configures a workflow in` .github/workflows/claude.yml` that looks like this:


```text
name  :   Claude Code


on  :
issue_comment  :
types  : [  created  ]
pull_request_review_comment  :
types  : [  created  ]
issues  :
types  : [  opened  ,   assigned  ]
pull_request_review  :
types  : [  submitted  ]


jobs  :
claude  :
if  :   |
(github.event_name == 'issue_comment' && contains(github.event.comment.body, '@claude')) ||
(github.event_name == 'pull_request_review_comment' && contains(github.event.comment.body, '@claude')) ||
(github.event_name == 'pull_request_review' && contains(github.event.review.body, '@claude')) ||
(github.event_name == 'issues' && (contains(github.event.issue.body, '@claude') || contains(github.event.issue.title, '@claude')))
runs-on  :   ubuntu-latest
permissions  :
contents  :   read
id-token  :   write
steps  :
-   name  :   Checkout repository
uses  :   actions/checkout@v4
with  :
fetch-depth  :   1


-   name  :   Run Claude Code
id  :   claude
uses  :   anthropics/claude-code-action@beta
with  :
anthropic_api_key  :   ${{ secrets.ANTHROPIC_API_KEY }}
```


Once you've installed this, you must merge in the pull request Claude opens before you can use it.


### Current gotchas


Because this is a beta action, things are very much in flux and under development. There are a few things to note:


1. You will need to make sure you have the GitHub CLI installed (i.e.,` gh` ) before you run the install command.
2. If you get` Couldn't install GitHub App: gh: Not Found (HTTP 404)` when running` /install-github-app` , it means that your local` gh` auth token doesn't have workflow permissions (i.e., it can't create the new` claude.yml` workflow). You can fix this by running` gh auth refresh -h github.com -s workflow` and then re-running the` /install-github-app` command.
3. The default tools in the action are pretty tightly scoped. They are listed below, and anything you add to` allowed_tools` will be appended to the end of this list:


- Edit
- Glob
- Grep
- LS
- Read
- Write
- mcp__github_file_ops__commit_files
- mcp__github_file_ops__delete_files
- mcp__github__update_issue_comment


## Having Claude Code open pull requests after completing tasks


The initial example workflows are heavily focused on running Claude Code to tag issues, add comments to issues, and add pull request reviews. This is great, but it doesn't really take advantage of the full power of Claude Code.


One of the things I'm most excited about is the ability to have Claude Code open pull requests after completing tasks. This is a great way to automate getting code changes reviewed and merged into your codebase.


To do this, you must update the` permissions` section of the workflow to include` pull-request: write` , so Claude can open pull requests. You must also include` mcp__github__create_pull_request` in the` allowed_tools` input.


Here is what the Claude Code workflow looks like with these changes:


```text
name: Claude Code


on:
issue_comment:
types: [created]
pull_request_review_comment:
types: [created]
issues:
types: [opened, assigned]
pull_request_review:
types: [submitted]


jobs:
claude:
if: |
runs-on: ubuntu-latest
permissions:
contents: read
id-token: write
+      pull-requests: write
steps:
- name: Checkout repository
uses: actions/checkout@v4
with:
fetch-depth: 1


- name: Run Claude Code
id: claude
uses: anthropics/claude-code-action@beta
with:
anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
+          allowed_tools: 'mcp__github__create_pull_request'
```


With that change, you can now tag` @claude` in your issues to have it open a pull request for the changes it made. Here is an example from our open-source docs repository,[depot/docs](https://github.com/depot/docs) :


## Speeding up Claude Code with Depot GitHub Actions runners


What's particularly cool about Anthropic making agentic coding backed by GitHub Actions is that it allows you to control where you want the Claude Code agent to run. This means that you can use Depot GitHub Actions runners to run Claude Code in a much faster environment than the default GitHub-hosted runners.


We've built out our GitHub Actions runners to have faster CPUs, ramdisks with faster IO, and network speeds that make caching exponentially faster. They are also half the cost of GitHub-hosted runners, meaning you can run your Claude Code agents for a fraction of the cost.


To move a Claude Code agent to run on Depot GitHub Actions runners, just update the` runs-on` section of the workflow to use` depot-ubuntu-latest` instead of` ubuntu-latest` . Here is what the updated workflow looks like:


```text
name: Claude Code


on:
issue_comment:
types: [created]
pull_request_review_comment:
types: [created]
issues:
types: [opened, assigned]
pull_request_review:
types: [submitted]


jobs:
claude:
if: |
-    runs-on: ubuntu-latest
+    runs-on: depot-ubuntu-latest
permissions:
contents: read
id-token: write
pull-requests: write
steps:
- name: Checkout repository
uses: actions/checkout@v4
with:
fetch-depth: 1


- name: Run Claude Code
id: claude
uses: anthropics/claude-code-action@beta
with:
anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
allowed_tools: 'mcp__github__create_pull_request'
```


Making this switch will make your Claude workflow significantly cheaper and faster. If you have a Claude Code agent that, on average, takes **5 minutes** , it will cost you about **$0.04/session** on GitHub-hosted runners. That same session on Depot GitHub Actions runners will cost you **$0.02/session** on our faster default runners.


But you can also tune this further with any of our runner types. For example, we have a` small` runner with only 2 cores, 2 GB of RAM, and 100 GB of disk at just **$0.002/minute** . This is an excellent option for running Claude Code agents doing small tasks that don't require a lot of resources. This runner type costs **$0.01/session** on Depot GitHub Actions runners.


## Conclusion


So there you have it - Claude Code on Depot runners is a powerful combo. Let's break down why this matters:


- **Save money** : Running Claude on Depot runners costs half what you'd pay on GitHub-hosted runners. If you're pinching pennies, our small runners drop the price to just $0.01 per session for simple tasks.
- **Speed things up** : Depot's faster CPUs, RAM disks, and network make everything run smoother. Your AI agents will thank you.
- **Get more done** : With Claude agents handling routine stuff in parallel, your team can focus on the interesting problems instead of the boring ones.
- **Super simple setup** : You're good to go with a few tweaks to your YAML file. No complex configuration needed.


This whole "AI agents in CI" thing is still pretty new, but is clearly where some portion of development is heading. The coolest part is how easy it is to set up - install the Claude GitHub App, add those permissions for PR creation, and switch to depot-ubuntu-latest. That's literally it.


I'm really excited to see what people build with this setup. Having multiple Claude agents working on different parts of your codebase while running on faster and cheaper infrastructure feels like a glimpse into the future of development.


Want to try it yourself?[Sign up for a 7-day free trial of Depot](https://depot.dev/sign-up) and get access to all our build performance for GitHub Actions, Docker, Bazel, Turborepo, Gradle, and much more.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
