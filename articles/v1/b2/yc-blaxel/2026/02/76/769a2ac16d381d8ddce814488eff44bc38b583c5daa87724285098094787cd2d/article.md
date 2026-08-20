---
schema_version: "1.0.0"
document_id: "769a2ac16d381d8ddce814488eff44bc38b583c5daa87724285098094787cd2d"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/give-your-coding-agent-blaxel-superpowers"
published_at: "2026-02-26T18:57:02+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:53:46.439852+00:00"
content_hash: "sha256:b7a7e6d764f8615a5ddc6baa94aeb75dd52543919a4b1f208f3cae87a938dd65"
---

# Give your coding agent Blaxel superpowers

Creating a Blaxel sandbox is already easy: you can[write code](https://docs.blaxel.ai/Sandboxes/Overview#using-the-sdks) (in[Python](https://github.com/blaxel-ai/sdk-python/) ,[TypeScript](https://github.com/blaxel-ai/sdk-typescript/) or[Go](https://github.com/blaxel-ai/sdk-go/) ); you can[use the CLI](https://docs.blaxel.ai/Sandboxes/Overview#using-the-cli-and-console) ; or you can point and click your way through the[Blaxel Console](https://app.blaxel.ai/) .


But we wondered: could we make it even easier?


Enter the[Blaxel Agent Skill](https://github.com/blaxel-ai/agent-skills) .


With the Blaxel Agent Skill, we're giving your coding agent everything it needs to autonomously spin up and manage sandboxes on Blaxel. No more writing code - just tell your agent to "create a sandbox" and within a few seconds, it's up and running!


It's not just sandboxes either. Our Blaxel Skill also makes your agent an expert in deploying other resources on Blaxel. That means you (or your agent) can also deploy[agents](https://docs.blaxel.ai/Agents/Overview) ,[MCP servers](https://docs.blaxel.ai/Jobs/Overview) ,[batch jobs](https://docs.blaxel.ai/Jobs/Overview) and[volumes](https://docs.blaxel.ai/Sandboxes/Volumes) , all with the same zero-code approach.


## How it works


Remember[Neo in The Matrix](https://en.wikipedia.org/wiki/Neo_%28The_Matrix%29) , downloading kung-fu skills into his mind? Agent Skills work in much the same way.


Conceptually, Agent Skills are simple to understand. They are instruction sets to extend coding agents with additional knowledge and tools. An agent can load these instructions into its context and use them to complete the tasks assigned to it. This idea was[first proposed in a blog post by Anthropic](https://claude.com/blog/equipping-agents-for-the-real-world-with-agent-skills) and was subsequently[published as an open standard](https://agentskills.io/specification) .


One of the most interesting[features of Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) is that they are written in natural language - not code - and stored in Markdown format. This makes them easy to create, maintain and share even for non-technical users. They can also include references to other information sources, such as external documentation, SDK and API references, code in Git repositories, and so on.[Look at the Markdown for our Blaxel Skill](https://github.com/blaxel-ai/agent-skills/blob/main/SKILL.md) for an example.


When an agent loads the Blaxel Skill, it receives all the information it needs to:


- create perpetual sandboxes to run code and execute commands
- start servers and generate URLs for real-time application previews
- create and deploy AI agents
- create and deploy MCP servers
- deploy and run batch jobs


## Get started


The easiest way to get started with the Blaxel Skill is to add it directly from our GitHub repository to your coding agent with the[open source skills CLI](https://github.com/vercel-labs/skills) :


shell


` npx skills


add


blaxel-ai/agent-skills


`


You'll be prompted to select your coding agent and installation location. The` skills` CLI supports[all popular coding agents](https://github.com/vercel-labs/skills?tab=readme-ov-file#supported-agents) and will automatically add and configure the skill for your selected agent.


Make sure that you're logged in to Blaxel:


` bl login`


Then, fire up your coding agent and give it a prompt - maybe something like this:


` Create a Blaxel cloud sandbox from a minimal base image. Install a complete Go dev environment in it. Show me the installed Go version and OS version in the sandbox.`


Your agent should automatically identify and load the Blaxel Skill, then use the information in it to spin up a sandbox, install packages, execute commands, and return the requested details.


How about something more complex?


` Your mission is - Create a Blaxel cloud sandbox for Next.js web app development. - Within this sandbox, develop a modern Next.js web app for a three course dinner featuring all your favorite recipes. - When you're done, start the dev server and give me a preview URL to see your work. - Make it yummy!`


Hmm, that actually sounds pretty good...


... and looks it too!


Our Blaxel Skill is[open source](https://github.com/blaxel-ai/agent-skills/blob/main/LICENSE) , so you can try it, give us feedback in[Discord](https://discord.gg/enAfyZFWHW) , or[suggest improvements](https://github.com/blaxel-ai/agent-skills/pulls)
