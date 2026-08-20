---
schema_version: "1.0.0"
document_id: "d4d69036f9d1af42fb76c08eeb367fa953b0b7659135c27876591d39025ff3cc"
company_key: "yc-warpbuild"
company: "WarpBuild"
source_id: "yc-warpbuild-news-import-6421ae0a6624"
canonical_url: "https://warpbuild.com/blog/claude-managed-agents"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-18T16:01:25.541743+00:00"
fetched_at: "2026-08-18T16:01:28.163940+00:00"
content_hash: "sha256:a88047c3c98ef553fbb4985b52caf3673f6dd9462155d88948236dbb1e36f380"
---

# WarpBuild 🤝 Claude Managed Agents

WarpBuild provides full-featured runners that mirror GitHub runners to run your CI workloads. They come in a range of operating systems (` ubuntu-22` ,` ubuntu-26` ,` macos-15` ,` macos-26` ), architectures (x86-64 / arm64), and core sizes (2x to 32x). See the[full list](https://www.warpbuild.com/docs/ci/cloud-runners) . These same runners can now be the execution environment for Claude Managed Agents. We support Linux and macOS runners today; Windows support is coming soon.


## Claude Managed Agents


Anthropic recently introduced[Claude Managed Agents](https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes) , which let you run Claude as an autonomous agent, well suited to long-running and asynchronous tasks. An agent has two main components: the *harness* that drives it, and an *environment* that executes the commands the harness wants to run.


The harness lives on Anthropic's side and can be customised on their platform with custom instructions, a toolset, and more. The execution environment can either be hosted by Anthropic or be` self_hosted` . WarpBuild can now be that` self_hosted` execution environment. We currently support stock runners; support for custom runners and images will be added in the future.


## Why WarpBuild?


We use the exact same full-featured runner VMs for this feature that we use to run your CI jobs. That means every runner comes with the common software preinstalled at its latest version, and every agent gets VM-level isolation. You can find out what's on the latest images (as at the time of this blog) in our[Changelog](https://www.warpbuild.com/docs/ci/changelog/2026-august#august-12-2026) .


When it finishes, your agent should write its output artifacts to the designated` /mnt/session/outputs` directory.


Anthropic omits the outputs directory from the system prompt for` self_hosted` environments, so make it part of your own system prompt. Instruct your agent to write deliverables to` /mnt/session/outputs` explicitly. See the[sandbox filesystem docs](https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes#sandbox-filesystem) .


When the session ends, WarpBuild persists that directory and makes it available on your dashboard as a downloadable cache artifact. Outputs are treated as CI cache artefacts; they have a TTL of 7 days post last access, and are billed as such; see[cache rates](https://www.warpbuild.com/pricing) . The agent runner VMs are billed by the second, at the same[rates](https://www.warpbuild.com/pricing) as the CI runners.


## Onboarding


Head over to the **Agent Runners** section of your[WarpBuild dashboard](https://app.warpbuild.com/ci/agent-runners/claude) and connect your Anthropic organisation:


1.


**Connect your environment.** Paste your Anthropic API key to link your organisation. WarpBuild provisions a Claude environment and hands you a per-organisation webhook URL to finish setup.


2.


**Register the webhook with Anthropic.** Add that per-organisation URL as a webhook endpoint in your Anthropic settings, so session events (run started, idled, terminated) route to WarpBuild. Paste the signing secret Anthropic returns back into WarpBuild, along with an environment key from your Anthropic environment settings.


3.


**Start a session.** Head over to[Sessions](https://app.warpbuild.com/ci/agent-runners) , choose the runner label (OS, architecture, and core size) that your agent should run on. Any stock Linux or macOS runner works. WarpBuild spins up a fresh runner, runs the agent inside it, and tears it down when the session ends. You can download anything your agent wrote to` /mnt/session/outputs` from the dashboard.


Give your agents a real environment to work in.


- [Agent Runners](https://app.warpbuild.com/ci/agent-runners)
- [Cloud runners](https://www.warpbuild.com/docs/ci/cloud-runners)
- [Pricing](https://www.warpbuild.com/pricing)


---
