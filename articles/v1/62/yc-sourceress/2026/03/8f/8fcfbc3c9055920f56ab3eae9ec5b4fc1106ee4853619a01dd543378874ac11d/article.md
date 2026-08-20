---
schema_version: "1.0.0"
document_id: "8fcfbc3c9055920f56ab3eae9ec5b4fc1106ee4853619a01dd543378874ac11d"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/keystone"
published_at: "2026-03-25T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:095279d7cbe9967c34d6148df910b38b7937980d91382652743389f381a72241"
---

# Introducing Keystone: Building self-configuring agents

At Imbue, we want coding agents that work for everyone, on every project, so we built Keystone: a standalone tool that drives a coding agent to create Dockerfiles and dev containers for arbitrary repositories, with all the work happening safely inside a[Modal](https://modal.com/) sandbox.


All coders have had this problem: “I just cloned a git repo, how do I actually get it running?”


One approach is to ask Claude Code to figure it out, installing dependencies on your system to make the repo run. But when Claude and other coding agents step into sys admin territory, they can make dangerous decisions. We’ve seen them dangerously install, remove, and downgrade packages, modify configuration files in our home directories, and try to change kernel parameters or other system settings.


Containers provide a partial solution to the “get this code running” problem. Dockerfiles give you a way to describe a reproducible execution environment with appropriate dependencies pre-installed and configured, and[dev containers](https://containers.dev/) provide a standardized way to package them inside a code repo, making it “self describe” its own execution environment.


But unfortunately, most code repos don’t already have a dev container pre-configured, which means you or your coding agent is stuck figuring that part out on its own.


**Until Keystone. Agents doing sys admin can go rogue, but Keystone runs them safely in a Modal sandbox while they autonomously configure your Docker container.**


## How Keystone works


[Keystone](https://github.com/imbue-ai/keystone) is a small, open-source Python CLI tool that takes any code repository as input, and augments it with a working Dockerfile and dev container, together with an entrypoint demonstrating that the project’s tests run in this Docker environment.


It does this by shipping the target code repository to a special Modal sandbox with its own Docker daemon, capable of running normal docker build and run commands. Kudos to the Modal team for pulling off this sandbox-in-a-sandbox engineering feat!


Within the confines of the Modal sandbox, Keystone starts a normal coding agent, like Claude Code or Codex and prompts it to explore the repo and create a Dockerfile in which as many tests as possible can pass.


Specifically, the agent is prompted to identify all of the repo’s automated tests, and construct a unified test entrypoint that’s capable of executing them and providing evidence of successful test execution in the form of JUnit XML reports.


Depending on the target project, the coding agent working inside Keystone can have a tough job. Legacy or complex software might need a very particular environment in which to run.


We open-sourced[Keystone on GitHub](https://github.com/imbue-ai/keystone) ! Share feedback with us there, or[join our Discord server](https://discord.gg/GvK8MsCVgk) to chat with our team.
