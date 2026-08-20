---
schema_version: "1.0.0"
document_id: "63a2522f2d0d67e32ec6b882170327c86f3f752f937861be1bfe9c9466476c51"
company_key: "yc-formal"
company: "Formal"
source_id: "yc-formal-news-import-a5b088b50d89"
canonical_url: "https://www.formal.ai/blog/allowlisting-bash-commands/"
published_at: "2025-11-05T00:00:00+00:00"
first_seen_at: "2026-07-27T09:22:31.452672+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:3cd42d2cf79ed85c5e4956b2c1a3b2d1acff47ecbee14843ccfd79e12ac2eb34"
---

# Allowlisting Some Bash Commands is Often the Same as Allowlisting All with Claude Code

## Introduction


At Formal , we are heavy users of agentic coding tools for software development, and we’re trying to continue performing local development with these tools on our (admittedly beefy) laptops for as long as possible. One example is Claude Code. These tools feel particularly magical when[verification loops are as fast as possible](https://www.ycombinator.com/library/MW-andrej-karpathy-software-is-changing-again) .


Having to explicitly approve every file edit Claude Code makes is time intensive — and so is having to explicitly approve every command Claude Code wants to run to get feedback on that code change before we review it ourselves. Some examples include running` go build` ,` go test` , restarting a` docker` container, and running our linter!


Claude Code supports[allowlisting Bash commands](https://docs.claude.com/en/docs/claude-code/settings#permission-settings) as well as file edits to your directory without requiring approvals, which can dramatically speed up development. What if, however, we do not want Claude Code to be able to run certain commands on our laptops? Enabling file edits and particular Bash commands often used in software development often enables Claude Code to run *any* command!


We use` TypeScript` and` Go` , so the examples in this post will be specific to those languages.


We’re defining “able” as “could Claude Code perform these actions,” irrespective of the probability that Claude would output text that would cause Claude Code to perform these actions.


## ` go test`


What’s the worst a unit test could do? Well, a unit test could execute arbitrary bash scripts.


If you allowlist running` go test` and editing files without approval,


Claude Code could run any other command without approval via the following flow:


- Edit a test file to use` exec.Command`
- Run` go test`


## ` go generate`


Okay, that makes sense —` go test` is effectively running arbitrary code. What about making sure our code builds? Well, a prerequisite for building is code generation.


We do have some` go generate` directives, however, and running` go generate` as part of your build pipeline allows arbitrary code execution if the coding agent can edit files that will be used by` go generate` .


Running` go generate` produces


## ` go build*`


Okay, so let’s not run` go generate` without manual review. What about making sure your edited code builds correctly? Well, Claude Code can run` formal ls` via` go build` too if Claude Code can specify arguments after` go build` !` go help build` shows that there is a` -toolexec` argument:


Running` go build -toolexec 'formal ls'` produces


It seems like our Formal Desktop fails to connect to the desktop agent when trying to be run by` go build` ! Good thing we won’t be supporting that kind of functionality soon.


## ` eslint`


What about just running our` eslint` linter?` Eslint`[supports JavaScript files as configs](https://eslint.org/docs/latest/use/configure/configuration-files) , so Claude Code could add an` execSync` in an` eslint.config.js` .


Sure enough,` eslint` tries to run` formal ls` on startup:


## ` make` or` pnpm run`


However, allowlisting ***any***` pnpm run` command could enable Claude Code to run any command solely by editing the` package.json` ’s scripts config, no understanding of custom eslint rules required! Allowlisting a` make` command would allow executing any command as well for similar reasons.


## Claude Code May Be Able to Run Any Command


If using file watchers like` next dev –turbopack` or` jest` with` watchman` , Claude Code could still execute any command **without**` Bash` being allowlisted! We perform frontend development using` next dev –turbopack` , which spins up a Next.js server with automatic building and hot reloading when a file is edited. Adding the following code in any API route will have this command be executed at startup when the file is saved:


## ` docker`


What about rebuilding` docker` containers? Since Docker is a tool for executing code, being able to run` docker` commands enables Claude Code to run any command in a container.


To interact with the host, Claude Code could mount the host filesystem and run in privileged mode. In fact, the` docker` daemon by default runs with root, so being able to run` docker` commands may enable Claude Code to run commands as root as well against the host filesystem:


Hardcoded docker commands don’t fare much better: you can configure mounts and privileged settings by editing the[Docker Compose file](https://docs.docker.com/reference/compose-file/) ,[and run privileged commands that interact with the host through the USER and RUN instructions](https://www.docker.com/blog/understanding-the-docker-user-instruction/) in Dockerfiles.


## Allowlisting Bash Commands Is a Fraught Exercise


The combination of running developer tools against your codebase and editing your codebase often allows running any code. Development software is designed to execute developer-provided code, and malicious code provided by a malicious developer was not part of the threat model (if this kind of software had a threat model to begin with!). A lot of tools have some method of running arbitrary code as a configuration ***feature*** , not a bug.


The challenges of allowlisting only some commands but not others is not specific to Claude Code or Cursor:[a lot of Unix binaries](https://gtfobins.github.io/) were not designed to isolate user privileges, and similarly our developer tools for executing code were not designed as if a malicious developer was able to run them.


In fact, even` find` supports a` -exec`[argument that allows for arbitrary code execution](https://github.com/anthropics/claude-code/issues/9998) .


This might be part of the reason[why the Claude Code npm source](https://leehanchung.github.io/blogs/2025/03/07/claude-code/) has a “Glob” tool with the following prompt:


## But Does Command Allowlisting Make Running Unwanted Commands Less Likely?


Sure, Claude Code could perform all of these convoluted code edits or command arguments, but would Claude Code be less likely to emit these kinds of commands and changes than a more conventional Bash command?


Intuitively, we expect this to be true: we would expect that Claude Code would emit a` Bash:(curl)` tool call more often than a` Bash:(go build -toolexec 'curl')` call. We have not found a great way, however, to precisely quantify that reduction likelihood.


Should we worry more about allowlisting` make` and` pnpm run` than` go build` with file edits?


In addition,[active attempts at prompt injection to the inputs we are providing to Claude Code may significantly change our likelihood estimates](https://simonwillison.net/2025/Oct/22/living-dangerously-with-claude/#living-dangerously-with-claude.010.jpeg) . Still, viewing Claude Code as unhindered at the model and prompt level from emitting any kind of command is a simplifying assumption:[the model providers are working on model alignment](https://www.anthropic.com/research#alignment) .


Still, the definition of implicit “wanted” and “unwanted” commands from a prompt is remarkably squishy. In our experience, we have seen Claude Code attempt to run` psql` and AWS CLI commands. At first blush, this may seem alarming — but it depends on what resource these commands are run against! There are likely many Claude Code users who want it to run` psql` and AWS CLI.


In addition, we have a containerized test Postgres database in our compose stack, and running` psql` commands on that database is an expected part of our test and development workflow. Determining the risk profile of the same` psql` or AWS CLI command based on the resource we are interacting with can be tricky for agentic coding tools ([and for humans too](https://about.gitlab.com/blog/postmortem-of-database-outage-of-january-31/) )!


## An Alternative Form of Permissions Restriction: Sandboxing!


Running these tools on a different host means that these agentic tools are limited by the permissions of the host irrespective of what commands they run. We do still want to run these agentic tools on a privileged host, so we’re thrilled to see that Cursor, Claude Code, and Codex have all been releasing sandboxing tools! For OS X users, a lot of these sandboxes are using sandbox-exec under the hood. This is the[same technique Chromium uses](https://www.chromium.org/developers/design-documents/sandbox/osx-sandboxing-design/) despite macOS considering it deprecated since 2017.


- Cursor has a[sandbox mode for all users](https://cursor.com/docs/agent/terminal#sandboxing-beta) that runs terminal commands with sandbox-exec.
- [Claude Code has a Sandbox Bash tool](https://docs.claude.com/en/docs/claude-code/sandboxing)
- Claude Code also[has an experimental sandbox runtime](https://github.com/anthropic-experimental/sandbox-runtime)
- Anthropic provides a[devcontainers](https://docs.claude.com/en/docs/claude-code/devcontainer) template for working with Claude Code
- [The Codex CLI/IDE extension has an agent sandbox as well](https://developers.openai.com/codex/security/)


In addition, we recommend sandboxing those watchman processes as well.
