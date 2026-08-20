---
schema_version: "1.0.0"
document_id: "2604566f100cfe2ae6e291b864a8058df12629bf4dad6db0372fb94486ed3dbb"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-news-import-e250f7226063"
canonical_url: "https://www.docker.com/press-release/nanoclaw-partners-with-docker-to-run-ai-agents-safely/"
published_at: "2026-03-13T17:04:36+00:00"
first_seen_at: "2026-07-25T01:48:04.001658+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:5036c6f913cf9cc46edf769bd1a8fc298ce6259ca9030509724fbce7fe1e51aa"
---

# NanoClaw Partners with Docker to Help Teams Run AI Agents Safely Everywhere | Docker

Press Release


# NanoClaw Partners with Docker to Help Teams Run AI Agents Safely, Everywhere


***Secure sandbox environments allow AI agents to run freely without risk to host systems***


**TEL AVIV/PALO ALTO, CA – March 13, 2026** — NanoCo, creators of[NanoClaw](https://nanoclaw.dev/) , the secure open-source AI agent platform, and[Docker, Inc.](https://www.docker.com/) , the platform used by millions of developers to build, ship, and run software, today announced that they are integrating NanoClaw with Docker Sandboxes. The integration marks the first time any claw-based agent platform can be deployed inside Docker’s MicroVM-based sandbox infrastructure with a single command. NanoClaw has surpassed 20,000 GitHub stars and 100,000 downloads since launching last month, reflecting growing demand for a production-ready, security-first alternative to OpenClaw.


AI agents don’t just answer questions. They connect to live data, run code, and take actions on behalf of the people and teams that deploy them. That shift creates a security problem most organizations have not solved. The core issue is isolation: solutions like OpenClaw run directly on the host machine, without hard boundaries separating agents from each other or from the underlying system. A single compromised agent can access credentials, read session histories, and reach data belonging to entirely separate agents. Application-level permission checks don’t offer sufficient protection. What is required is OS-enforced isolation: each agent in its own safe environment, with its own filesystem and session history, invisible to every other agent running alongside it.


NanoClaw is built on top of Claude Code, Anthropic’s powerful general-purpose coding agent, which ships with sophisticated context management, memory, and tool-use capabilities already built in. NanoClaw wraps this core engine with an orchestration layer that handles scheduled tasks, persistent memory, channel integrations (WhatsApp, Telegram, Slack, Discord), and routing each message to the correct agent, all inside isolated Docker containers running with Docker Sandboxes. The result is an agent that users can configure, extend, and delegate work to entirely from their phone, without writing custom agent code.


With Docker Sandboxes, agents run in MicroVM-based, disposable isolation zones. If an agent were to attempt a container escape, exploiting a zero-day vulnerability for example, it would still be contained. Combined with NanoClaw’s minimal attack surface and auditable codebase, the integrated stack is designed to meet the scrutiny of enterprise security teams.


“Every organization wants to put AI agents to work, but the barrier is control: what those agents can access, where they can connect, and what they can change,” said Mark Cavage, President and Chief Operating Officer at Docker, Inc. “Docker Sandboxes provide the secure execution layer for running agents safely, and NanoClaw shows what’s possible when that foundation is in place.”


“We are at the beginning of a shift where every team, in every organization, will have their own team of AI agents doing real work on its behalf,” said Gavriel Cohen, creator of NanoClaw. “NanoClaw was built to make that a reality today, and Docker Sandboxes is what makes it a reality that organizations can actually trust. Docker has been ahead of the curve in understanding what agent infrastructure needs to look like at scale, and the combination of NanoClaw’s orchestration layer with Docker Sandboxes’s isolation and network controls gives every team the foundation to deploy agents in production with confidence.”


To get started with NanoClaw in Docker Sandboxes, visit[github.com/qwibitai/nanoclaw](https://github.com/qwibitai/nanoclaw) . More information about Docker Sandboxes is[available here](https://www.docker.com/products/docker-sandboxes/) .


**Resources**


- **Docker Blog** :[Secure Agent Execution with NanoClaw and Docker Sandboxes](https://www.docker.com/blog/nanoclaw-docker-sandboxes-agent-security/)
- **NanoClaw Blog:**[Run NanoClaw in Docker Sandboxes with One Command](https://nanoclaw.dev/blog/nanoclaw-docker-sandboxes)
- **GitHub Repository:**[github.com/qwibitai/nanoclaw](https://github.com/qwibitai/nanoclaw)
- **Product Page:**[Docker Sandboxes](https://www.docker.com/products/docker-sandboxes/)


**Media Contacts**


press@docker.com
lazer@qwibit.ai
