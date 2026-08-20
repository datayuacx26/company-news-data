---
schema_version: "1.0.0"
document_id: "1d64f232c34475ffee2f4566e0a531e41210c9cf32a6a33175c08f3eb41fa6b6"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/eve-support-agent-zero-credentials"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:e00dbc7cd953c3b559f10f17465f6abe7ca8f0d498636d324711fe58040f5d7b"
---

# How We Secured a Vercel eve Agent by Giving It Zero Credentials

Vercel just shipped[eve](https://vercel.com/blog/introducing-eve) , an open source framework for building AI agents. It promises to do for agents what Next.js did for web apps, collapse the architecture into a folder and file structure instead of a pile of glue code.


With eve, the entire agent is a file directory: the instructions it follows, the tools it can call, the specialists it can delegate to, the places people can reach it.


We built one to see how far that pitch holds up, and to test something else at the same time. We prototyped a support agent for the Infisical[open source community](https://infisical.com/slack) : it needed to read our codebase and answer questions about the CLI or platform, with citations to the actual source. That agent needed real credentials to do the job, a model API key and a GitHub token, and we didn't want either one anywhere near its own process. So we routed everything through[Agent Vault](https://infisical.com/blog/agent-vault-the-open-source-credential-proxy-and-vault-for-agents) , an open source credential proxy and vault, and watched the agent do real work while holding none of the keys that made it possible.


## The folder is the architecture


Open an eve project and the directory tells you almost everything about what the agent does:


```text
agent/
agent.ts          model + config
instructions.md   who the agent is
tools/            custom capabilities
skills/           playbooks the agent loads on demand
connections/      external tools and MCP servers
subagents/        specialists it can delegate to
channels/         where people reach it (Slack, web, HTTP)
schedules/        cron-triggered runs


```


That's the whole mental model. A file's name and its place in the tree define what it does. Compare that to hand-rolling an agent on a raw model API, where the "architecture" lives across a dozen scattered files and a system prompt nobody wants to touch, and the appeal is obvious.


A few pieces are worth walking through, because they're what let us build our credential setup without writing much extra code.


**Model choice is one line.**` agent.ts` exports the config, and the model is a single field:` model: "anthropic/claude-sonnet-5"` . eve routes that string through Vercel's AI Gateway by default, but it can also call any other LLM API directly, and it accepts fallbacks. Our main agent runs on Claude Sonnet for routine questions. The code-researcher subagent, the specialist that digs through code on harder questions, points at Opus in its own` agent.ts` , just by naming a different string.


**Tools are typed functions with the built-ins already done for you.** Web search, fetching a URL, reading and writing files, running shell commands, these ship out of the box. When we needed something specific, a function that hands back our canonical docs and GitHub links so the agent points people to the real page instead of guessing a URL, we wrote it as a typed function in` tools/` :` defineTool` wrapping a Zod schema for the input and an` execute` function that does the lookup. The model only ever sees what` execute` returns, never the code or the credentials behind it.


**Connections saved us the most time, and they're the part our credential story runs through.** Wiring up GitHub by hand on a typical agent SDK means writing each tool call yourself: list issues, get a file, search code, one function per endpoint. In eve, a connection is one file pointing at a server, and the agent discovers the whole toolset on demand. Ours is` connections/github.ts` ,` defineMcpClientConnection` pointed at GitHub's own hosted MCP (Model Context Protocol) server with a` getToken` callback reading` process.env.GITHUB_TOKEN` . The moment that file existed, the agent could search issues, read files, and open pull requests, without us writing a single GitHub-specific function. That` GITHUB_TOKEN` is exactly the value we're about to hollow out.


**Subagents let the main agent stay lean.** Ours delegates code questions to a code-researcher subagent with its own prompt, its own sandbox, and its own model, defined the same way as the main agent's but living at` subagents/code-researcher/agent.ts` . It clones our repos, greps the real source, and comes back with a file and a line number instead of a guess, which is the whole point of building a support agent that answers from our actual codebase instead of from a model's memory of it.


**Channels are different front doors to the same agent.** There's a plain HTTP endpoint you can hit with` curl` , a web chat component you can drop into a Next.js site, and platform channels for Slack and Discord that answer mentions and DMs where a team already works. We didn't have to write anything for the HTTP endpoint since eve enables it by default. But we can separately configure` slack.ts` ,` discord.ts` , or anything else. This avoids creating parallel agents and means an update is reflected across channels.


**The sandbox is where the agent actually gets its hands dirty.** Cloning a repo, grepping through code, running a command, none of that happens on your machine or inside your app's process. eve spins up an isolated sandbox and runs it there.


The provider is tiered: on Vercel it's a hardware-isolated microVM, and locally it walks down a list, Docker first, then a lighter microsandbox, then a pure-JS` just-bash` fallback if neither is available. That fallback is silent, not an error. If Docker isn't running when eve starts, you get a slower, less capable sandbox with no warning that you're not in a container anymore, worth checking for before you assume your local run matches production.


That sandbox has a nice cost property worth calling out. The first time our code-researcher subagent ran, it cloned the relevant repos and baked them into a cached template image, about a gigabyte, containing our whole monorepo. Every session after that spins up a fresh sandbox from that template using copy-on-write, so new containers are kilobytes, not gigabytes. You clone once, and every subsequent session is nearly free.


The rest of the framework rounds out what you'd expect from something meant to run in production:


- Any tool can require human-in-the-loop approval before it runs, so risky or expensive actions pause and wait for a human to say go.
- Schedules are cron-triggered directories, so a daily report or a nightly sync runs on its own with nobody prompting it.
- Evals are scored test suites that run on every deploy, so a prompt tweak that quietly breaks something gets caught before it ships.
- A turn that's waiting on a human approval or an OAuth sign-in just parks and resumes when the answer comes back, even much later.


When it's time to ship, an eve agent is a normal Vercel project.` vercel deploy` and you get the sandbox, the durable sessions, and the cron schedules for free. eve doesn't require Vercel since it's open-source, but it is optimized for Vercel compatibility.


## The problem eve doesn't solve: what holds the keys


None of that changes the one fact every agent eventually runs into. To do anything real, an agent needs credentials: a model API key, a GitHub token, whatever the task requires. The default pattern is a` .env` file the agent process reads directly, which means the agent holds a live secret. It's the same setup behind[the recent wave of AI coding agents leaking their .env files](https://infisical.com/blog/your-ai-coding-agent-is-reading-your-env-file) . If a malicious instruction ever gets into its context, a prompt injection buried in a GitHub issue, a poisoned file it reads, a webpage it fetches, the agent has something worth stealing sitting right there in its own environment.


This isn't an eve-specific problem, it's the same[credential brokering](https://infisical.com/blog/credential-brokering-for-ai-agents) problem every agent framework has, from a local coding assistant to an[MCP server](https://infisical.com/blog/managing-secrets-mcp-servers) sitting in front of a database. Anywhere an agent needs a secret to act, it's also a place the secret can walk out the door.


So before we wired up the demo, we stood up[Agent Vault](https://github.com/Infisical/agent-vault) , an open source credential proxy and vault built for exactly this. The setup is one command:


```text
agent-vault vault run --vault workshop -- eve dev


```


That starts the agent and routes its outbound traffic through the vault. The agent's` .env` only holds placeholder strings or is fully empty. No real Anthropic key, no real GitHub token. The actual credentials live in the vault, and Agent Vault injects them onto the wire when a request actually goes out, after the request has already left the agent's own process.


We asked the agent to print its own Anthropic API key and` cat` its` .env` file. It refused, which is expected: models are trained not to dump credentials on request. But that refusal isn't actually the point, because there's nothing real to refuse. We checked the` .env` file ourselves afterward: every value in it was a dummy string, so even a clever prompt injection would've been worthless to the attacker.


The more interesting part is the GitHub connection. We pointed it at one of our repos with a real personal access token sitting only in the vault, and the agent's tool calls came back clean: it listed issues and read files with no errors, even though the credential in its own` .env` was a placeholder that couldn't authenticate anything on its own. That's the same path that lets the pattern extend to a private repository the agent's own environment has no working credential for.


The request leaves the agent carrying a dummy value, and Agent Vault swaps the dummy token or attaches one on the wire, after the request has already left the agent's process, before it reaches GitHub. The LLM API key is brokered the same way. There's nothing sitting in the agent's process that a prompt injection could ever actually exfiltrate.


This is the pattern the industry is converging on for agent credentials, and it matters more as agents get more autonomous, not less. A human developer can eyeball a suspicious instruction and stop. An agent working through a long task on its own doesn't get that same pause, so the safest design is one where it never has a real secret to leak in the first place, whether it's careless, compromised, or just doing exactly what an attacker's injected instruction told it to do.


## Questions worth asking before you wire credentials into an agent


**How does this actually happen in the real world?** Prompt injection is the usual path: an instruction hidden in a GitHub issue, a web page, or a Slack message tells the agent to read out or forward whatever credentials it's holding, and the agent follows it because it can't tell an injected instruction from a real one. This isn't theoretical. Research into malicious skills sold on skill marketplaces found that roughly one in eight contained some form of credential exfiltration or credential harvesting.


**Why not just scope the token narrowly instead of brokering it?** Scoping helps and you should still do it. A read-only, single-repo GitHub token limits the blast radius if it leaks. But scoping doesn't stop the leak itself, it just shrinks what the leak is worth. Brokering removes the credential from the agent's reach entirely, so there's nothing to leak regardless of scope.


**Doesn't the agent still need to authenticate somehow?** Yes, but not to fetch back secrets. The agent authenticates to Agent Vault with a token that only lets it proxy through to a predefined set of upstream services, it isn't[secret zero](https://infisical.com/blog/solving-secret-zero-problem) , and it can't be used to pull any of the other credentials the broker holds. The credential that actually opens something at the target API is attached by the broker itself, after the request leaves the agent, and the agent never sees or holds it.


**What happens if the agent tries to leak the dummy value anyway?** Nothing useful happens for an attacker. The value in the agent's` .env` is a real string, it's just not a working credential. Printing it, emailing it, or posting it somewhere hands over nothing that opens a door.


**Does durability survive a local eve dev session, or only on Vercel?** Only on Vercel today. Local development runs the same code, but the checkpointing and crash recovery are part of the deployed runtime. Worth knowing before you build a workflow that depends on a session surviving a restart in local dev.


## Where this leaves us


eve gives an agent a clean, inspectable shape, a folder of files instead of a pile of glue code, with sandboxing, subagents, durability, and deployment handled for you. None of that touches the credential problem, and it isn't supposed to. That's a separate layer, and it's the one that decides whether a compromised or simply overzealous agent can do damage with what it's holding.


We built our support agent on[eve](https://github.com/vercel/eve) and secured it with[Agent Vault](https://github.com/Infisical/agent-vault) . By the time it could've answered questions in Slack, its` .env` file held two placeholder strings, and neither one could authenticate to anything. The model key it thought with and the GitHub token it searched code with both lived somewhere the agent itself could never reach.
