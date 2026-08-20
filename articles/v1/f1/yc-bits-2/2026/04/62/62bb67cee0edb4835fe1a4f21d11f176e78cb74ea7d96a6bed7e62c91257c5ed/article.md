---
schema_version: "1.0.0"
document_id: "62bb67cee0edb4835fe1a4f21d11f176e78cb74ea7d96a6bed7e62c91257c5ed"
company_key: "yc-bits-2"
company: "Bits"
source_id: "yc-bits-2-news-import-8ae56e6aae1c"
canonical_url: "https://klausai.com/blog/running-openclaw-in-docker-complete-setup-guide/"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-24T21:07:46.493176+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:e1fdf708d2b23b0c7cd3474840082e9bde3e632acc5fe44616f1e318483df2a7"
---

# Running OpenClaw in Docker: The Complete Setup Guide

# Running OpenClaw in Docker: The Complete Setup Guide


I run a managed hosting service, so I have opinions about whether Docker self-hosting is worth the effort. I’ll be honest about where it gets painful. But this is a real setup guide, not a sales pitch. If you want to run OpenClaw in Docker, here’s how to do it right.


## What You Need Before You Start


Docker adds a layer of complexity on top of OpenClaw’s own setup process. Before you start, make sure you have the basics covered.


**Hardware:**


- 2 GB RAM absolute minimum. The Docker image build gets OOM-killed on 1 GB hosts with exit code 137 ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ). 4 GB is what you actually want for anything beyond basic use.
- 20 GB SSD. Session logs, media files, and cron run outputs accumulate faster than you’d expect.


**Software:**


- Docker Engine + Docker Compose v2 ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) )
- Ubuntu 22.04 or 24.04 if you’re on a VPS


**Other:**


- An API key from a model provider (OpenRouter, Anthropic, OpenAI, etc.)
- A domain or static IP if you want remote access (optional for local Docker setups)


If Docker is new to you, this is probably not the right starting point. The[getting started guide](https://klausai.com/blog/how-to-set-up-openclaw-complete-getting-started-guide) covers simpler paths, including local install and managed hosting.


**What this guide does not cover:** Kubernetes deployments, multi-node setups, or running OpenClaw behind a reverse proxy. Those are separate topics with their own failure modes. This guide gets one OpenClaw instance running in Docker on one machine, properly secured and maintainable.


## How to Set Up OpenClaw in Docker


Two approaches: the setup script (fast, opinionated) or the manual path (full control over each step).


### The Quick Way: Setup Script


Clone the repo and run the script:


```text
git   clone   https://github.com/openclaw/openclaw.git
cd   openclaw
./scripts/docker/setup.sh
```


That single command builds the Docker image from the included Dockerfile, runs the onboarding wizard (which asks for your API key and model provider), generates a gateway token for the Control UI, and starts the full compose stack ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ).


If you’d rather skip the local build, pull a pre-built image from the GitHub Container Registry:


```text
export   OPENCLAW_IMAGE  =  "ghcr.io/openclaw/openclaw:latest"
./scripts/docker/setup.sh
```


Available tags include` latest` ,` main` , and version-pinned tags like` 2026.4.14` ([OpenClaw GitHub Releases](https://github.com/openclaw/openclaw/releases) ). I’ll explain later why you should pin to a specific version instead of using` latest` .


### The Manual Way: Step by Step


For readers who want to understand what the script does, or who need to customize the build.


**Build the image:**


```text
docker   build   -t   openclaw:local   -f   Dockerfile   .
```


**Run onboarding:**


```text
docker   compose   run   --rm   --no-deps   --entrypoint   node   \
openclaw-gateway   dist/index.js   onboard   --mode   local   --no-install-daemon
```


This walks you through API key configuration, model provider selection, and gateway setup. It generates a` .env` file with your` OPENCLAW_GATEWAY_TOKEN` .


**Configure the gateway:**


```text
docker   compose   run   --rm   --no-deps   --entrypoint   node   \
openclaw-gateway   dist/index.js   config   set   --batch-json   \
'[{"path":"gateway.mode","value":"local"},{"path":"gateway.bind","value":"lan"}]'
```


Keep` gateway.bind` on` lan` or` loopback` unless you have a specific reason to expose it more broadly ([OpenClaw VPS Docs](https://docs.openclaw.ai/vps) ).


**Start the gateway:**


```text
docker   compose   up   -d   openclaw-gateway
```


**Verify it’s running:**


```text
curl   -fsS   http://127.0.0.1:18789/healthz
```


A successful response means the gateway is up and accepting connections ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ).


## Docker Compose Configuration Explained


The compose stack runs two containers. Understanding what each does saves debugging time later.


**` openclaw-gateway`** : the long-running process that handles conversations, skill execution, cron jobs, and channel integrations. This is your agent.


**` openclaw-cli`** : a management sidecar for running CLI commands. It shares the gateway’s network namespace via` network_mode: "service:openclaw-gateway"` , which means it can reach the gateway on localhost without exposing additional ports ([OpenClaw GitHub](https://github.com/openclaw/openclaw/blob/main/docker-compose.yml) ).


### Volumes: The Part Most Guides Skip


Docker Compose bind-mounts two critical paths:


Volume Container Path What It Stores


` OPENCLAW_CONFIG_DIR`` /home/node/.openclaw` Config (` openclaw.json` ), credentials, OAuth profiles,` .env` with gateway token, cron jobs


` OPENCLAW_WORKSPACE_DIR`` /home/node/.openclaw/workspace` Memory files, tools, workspace content, session JSONL logs


These directories persist across container restarts and replacements. If you lose them, you lose your agent’s configuration, credentials, and conversation history ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ).


**Permission gotcha** : the container runs as user` node` (uid 1000). Host bind mounts must match:


```text
sudo   chown   -R   1000:1000   /path/to/openclaw-config   /path/to/openclaw-workspace
```


### Environment Variables


Variable Purpose


` OPENCLAW_IMAGE` Pull a remote image instead of building locally


` OPENCLAW_SANDBOX` Enable agent sandboxing (` 1` ,` true` ,` yes` ,` on` )


` OPENCLAW_DOCKER_SOCKET` Custom Docker socket path (for rootless Docker)


` OPENCLAW_EXTRA_MOUNTS` Additional bind mounts (comma-separated` source:target\[:opts\]` )


` OPENCLAW_HOME_VOLUME` Named Docker volume for` /home/node` persistence


Full list in the[official Docker docs](https://docs.openclaw.ai/install/docker) .


### Disk Growth


Three things fill your disk quietly:` media/` (files your agent downloads or creates), session JSONL files (every conversation gets logged), and` cron/runs/*.jsonl` (output from scheduled tasks). Rolling logs also accumulate in` /tmp/openclaw/` inside the container.


On a 20 GB disk, you’ll start noticing pressure within a few weeks if your agent runs scheduled tasks or handles file-heavy workflows. Set up log rotation or at minimum monitor disk usage weekly. A simple cron job that checks` du -sh ~/.openclaw/` and alerts above a threshold saves you from the 3am “disk full, gateway crashed” surprise.


### Running CLI Commands


After the stack is running, execute management commands through the CLI container:


```text
docker   compose   run   --rm   openclaw-cli   [command]
```


For CI/CD or automated scripts without TTY allocation:


```text
docker   compose   run   -T   --rm   openclaw-cli   gateway   probe
```


Common commands you’ll use:` gateway probe` (health check),` config set` (change settings),` channels add` (connect messaging),` dashboard --no-open` (regenerate auth token).


## Security Hardening for Docker Deployments


The default Docker image includes reasonable security defaults. The container runs as non-root user` node` (uid 1000), drops` NET_RAW` and` NET_ADMIN` capabilities, and enables` no-new-privileges` ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ).


That said, there are things the defaults don’t cover.


**Docker bypasses your firewall.** If you’re running UFW or iptables, Docker’s port publishing skips your rules entirely. You need to configure the` DOCKER-USER` chain explicitly to restrict access to published ports ([Docker Security Docs](https://docs.docker.com/engine/security/) ). Most first-time Docker deployers don’t know this.


**Keep the gateway on loopback.** The[official recommendation](https://docs.openclaw.ai/vps) is to never expose port 18789 directly. Access remotely through an SSH tunnel:


```text
ssh   -L   18789:localhost:18789   user@your-vps
```


Or use Tailscale by setting` gateway.bind` to` tailnet` in the gateway config, which authenticates via Tailscale identity headers ([OpenClaw VPS Docs](https://docs.openclaw.ai/vps) ).


**Agent sandboxing for untrusted skills.** If you’re installing community skills from Clawhub, enable sandbox mode:


```text
export   OPENCLAW_SANDBOX  =  1
./scripts/docker/setup.sh
```


This runs tool executions in isolated sub-containers, preventing a malicious skill from accessing your host filesystem ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ).


**The container isolation question.** Standard Docker containers share the host kernel. A kernel vulnerability can allow container escape, giving an attacker host access ([Northflank](https://northflank.com/blog/how-to-sandbox-ai-agents) ). For most self-hosted setups, this risk is acceptable. For high-security deployments running untrusted code, consider Docker Sandbox (which uses microVM isolation with dedicated kernels per workload, booting in roughly 125ms with less than 5 MiB overhead per VM) or gVisor (which intercepts syscalls at the user-space level, adding[10-30% overhead on I/O-heavy workloads](https://northflank.com/blog/how-to-sandbox-ai-agents) ).


The practical question: are you running skills you wrote yourself, or community skills from Clawhub? If it’s your own code, standard Docker isolation is fine. If you’re installing third-party skills that execute arbitrary commands, the stronger isolation is worth the setup effort.


## How to Update OpenClaw in Docker


This is where Docker self-hosting gets honest about its costs.


OpenClaw ships fast. In a 14-day span in March 2026, there were 7 stable releases, and 43% of them included breaking changes ([TryOpenClaw](https://www.tryopenclaw.ai/blog/openclaw-update-treadmill/) ). That’s roughly one release every two days, with nearly half requiring more than a simple restart.


**Basic update process** (for pre-built images):


```text
docker   compose   pull
docker   compose   up   -d
```


For local builds, pull the latest source and rebuild:


```text
git   pull
docker   build   -t   openclaw:local   -f   Dockerfile   .
docker   compose   up   -d
```


**Pin your version.** Use a specific tag instead of` latest` :


```text
image  :   ghcr.io/openclaw/openclaw:2026.4.14
```


This gives you control over when you update. Read the[changelog](https://github.com/openclaw/openclaw/releases) before upgrading, and back up your` ~/.openclaw` directory first.


**After updating** , verify the gateway is healthy:


```text
docker   compose   run   -T   --rm   openclaw-cli   gateway   probe
```


**The maintenance math.** At roughly 20 minutes per update and 2-3 updates per month (if you’re selective about which releases to apply), expect 1-2 hours monthly on updates alone. Add general maintenance (disk monitoring, log rotation, security patches) and you’re looking at 2-5 hours per month ([TryOpenClaw](https://www.tryopenclaw.ai/blog/openclaw-update-treadmill/) ). That’s the real cost of self-hosting, not the server bill.


The worst scenario is not a slow afternoon of planned maintenance. It’s your agent going silent at 2am because a kernel update broke the Docker socket, or a new OpenClaw release changed how config files are parsed and your cron jobs stopped running. You don’t find out until a customer asks why their daily report didn’t arrive.


## Common Docker Setup Problems


**OOM during build (exit 137).** Your host has less than 2 GB RAM. Either increase VM memory or skip the local build entirely by using a pre-built image with` OPENCLAW_IMAGE` ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ).


**Permission denied on /home/node/.openclaw.** Bind mount ownership doesn’t match the container user. Fix it:


```text
```


**“pairing required” or “disconnected (1008).”** Gateway mode or binding is misconfigured. Reset both:


```text
docker   compose   run   --rm   openclaw-cli   config   set   --batch-json   \
```


**Sandbox containers won’t start.** The sandbox image hasn’t been built yet. Run` scripts/sandbox-setup.sh` before enabling sandbox mode ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ).


**Custom tools not found in sandbox.** OpenClaw uses login shells (` sh -lc` ) which reset PATH. Set` docker.env.PATH` in your agent config or add tool definitions to` /etc/profile.d/` in your custom Dockerfile ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ).


**Control UI shows “Unauthorized.”** Your gateway token has expired or wasn’t set. Regenerate it:


```text
docker   compose   run   --rm   openclaw-cli   dashboard   --no-open
```


## Docker Self-Hosting vs Managed Hosting


This isn’t a full comparison (the[managed vs self-hosted article](https://klausai.com/blog/openclaw-hosting-managed-vs-self-hosted) covers that). Just the Docker-specific tradeoffs.


Docker Self-Hosted Managed (Klaus)


**Setup time** 30-60 minutes Under 5 minutes


**Monthly server cost** $5-25 (VPS) $19-200 ([klausai.com/pricing](https://klausai.com/pricing) )


**Monthly maintenance** 2-5 hours Zero


**Update responsibility** You Provider


**Security patches** You Provider


**OAuth integration setup** Per-integration, manual Pre-configured


**Full environment control** Yes No


Docker gives you full control over the environment, no vendor lock-in, and the ability to customize the image. It costs you update maintenance, security responsibility, and the OAuth setup for every integration you want to connect. Each integration (Google Workspace, Slack, Telegram) requires creating a separate OAuth app in that platform’s developer console. Google’s setup alone takes 20-30 minutes if you haven’t done it before, and their test mode silently fails to authorize. You have to publish the app to Production status.


At Klaus, managed hosting starts at $19/month and handles all of the above. Whether that’s worth it depends on how you value your time versus your control. I’m biased, obviously. But the Docker guide above is genuine. I want people to succeed either way.


## Frequently Asked Questions


### How much RAM does OpenClaw need in Docker?


2 GB is the minimum for the image build step. For actually running the agent, 4 GB is what you want. Browser automation skills can spike memory well beyond that ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ). On a 2 GB VPS, complex agent tasks will crash the gateway.


### Should I use Docker or install OpenClaw directly?


Docker adds isolation and reproducibility. Direct install is simpler for single-user local setups. For VPS deployments, Docker is the better choice because it simplifies updates (pull new image, restart) and provides a security boundary between the agent and your host system ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ).


### How often should I update my Docker OpenClaw instance?


Pin to a specific version and update every 2-4 weeks after checking the changelog. Skipping more than a month risks falling behind on security patches. Three security advisories shipped in a single two-week period in March 2026 ([TryOpenClaw](https://www.tryopenclaw.ai/blog/openclaw-update-treadmill/) ).


### Can I run OpenClaw in Docker on a Raspberry Pi?


ARM images exist, but the 2 GB RAM minimum means only Pi 4 or Pi 5 with 4 GB or more will work. Performance will be limited. Enable the Node compilation cache for ARM hosts to reduce cold-start penalties ([OpenClaw VPS Docs](https://docs.openclaw.ai/vps) ).


### What happens to my data if the Docker container crashes?


Your data survives container crashes and restarts because the important state (config, credentials, workspace, session logs) lives in bind-mounted volumes on the host filesystem. The container itself is stateless. As long as your volume directories are intact, you can rebuild or replace the container without losing anything. Back up those directories regularly ([OpenClaw Docker Docs](https://docs.openclaw.ai/install/docker) ).


## Key Takeaways


- Docker is the recommended way to self-host OpenClaw on a VPS. It provides isolation, reproducible deployments, and simpler updates than bare-metal installs.
- Pin your image version. Never run` latest` in production. OpenClaw ships releases every two days, and many include breaking changes.
- The real cost of Docker self-hosting is not the $5-25/month server bill. It’s the 2-5 hours per month of maintenance: updates, security patches, disk monitoring, and debugging.
- Keep the gateway on loopback. Access remotely via SSH tunnel or Tailscale. Never expose port 18789 directly to the internet.
- Back up your` ~/.openclaw` directory before every update. Volumes persist across container restarts, but a bad update can corrupt config files.
- Enable sandbox mode if you install community skills from Clawhub. The[ClawHavoc incident](https://klausai.com/blog/best-openclaw-skills-for-business) showed that malicious skills are a real threat.
- If 2-5 hours of monthly container maintenance sounds like more than you want to spend, managed hosting exists. Klaus starts at $19/month and handles everything covered in this guide.


---


Want to skip the Docker setup?[Sign up at klausai.com](https://klausai.com/) and start talking to your agent in minutes.


For a broader look at all setup paths (managed, VPS, local), see the[complete getting started guide](https://klausai.com/blog/how-to-set-up-openclaw-complete-getting-started-guide) . For a deeper comparison of managed vs self-hosted, see the[managed vs self-hosted article](https://klausai.com/blog/openclaw-hosting-managed-vs-self-hosted) .


## Sources


- OpenClaw. “Docker Installation Guide.” 2026.[https://docs.openclaw.ai/install/docker](https://docs.openclaw.ai/install/docker)
- OpenClaw. “Linux Server (VPS) Deployment.” 2026.[https://docs.openclaw.ai/vps](https://docs.openclaw.ai/vps)
- OpenClaw. “docker-compose.yml.” GitHub.[https://github.com/openclaw/openclaw/blob/main/docker-compose.yml](https://github.com/openclaw/openclaw/blob/main/docker-compose.yml)
- OpenClaw. “Releases.” GitHub.[https://github.com/openclaw/openclaw/releases](https://github.com/openclaw/openclaw/releases)
- TryOpenClaw. “OpenClaw Releases Weekly: Here’s Why That’s a Problem If You Self-Host.” 2026.[https://www.tryopenclaw.ai/blog/openclaw-update-treadmill/](https://www.tryopenclaw.ai/blog/openclaw-update-treadmill/)
- Northflank. “How to Sandbox AI Agents in 2026: MicroVMs, gVisor & Isolation Strategies.” 2026.[https://northflank.com/blog/how-to-sandbox-ai-agents](https://northflank.com/blog/how-to-sandbox-ai-agents)
- Docker. “Docker Engine Security.” 2026.[https://docs.docker.com/engine/security/](https://docs.docker.com/engine/security/)
- Klaus. Pricing.[https://klausai.com/pricing](https://klausai.com/pricing)
