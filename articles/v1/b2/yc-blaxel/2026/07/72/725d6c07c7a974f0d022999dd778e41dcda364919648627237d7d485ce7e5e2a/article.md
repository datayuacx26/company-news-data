---
schema_version: "1.0.0"
document_id: "725d6c07c7a974f0d022999dd778e41dcda364919648627237d7d485ce7e5e2a"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/now-available-blaxel-and-sapiom-team-up-to-let-agents-buy-their-own-sandboxes"
published_at: "2026-07-21T18:37:09+00:00"
first_seen_at: "2026-07-21T21:36:33.905253+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:933d658fa4e4aef41129f8964b41814545c431f1df6d2302fe4d0afa1cc5d2cf"
---

# Now available: Blaxel and Sapiom team up to let agents buy their own sandboxes

## The need for an agent-native payment experience


Today's AI-native companies prefer speed over control. They want their agents to implement complex applications independently, right from planning to deployment.


But there's still a major bottleneck. An agent can plan, write code, run tests, and simulate execution paths ... but it can’t independently pay for, and provision, infrastructure. This task still usually devolves to a human administrator, who needs to set up billing, generate API keys, and define governance and quotas.


For AI-native companies looking to scale their agentic operations, this adds unnecessary friction. In order for agents to become truly independent, they need a way to pay for the services they run, on their own.


## Use cases


Here are a few examples of when agents need access to additional payment-gated services:


- Code generation: To write code to create and test new features, AI agents need to spin up isolated sandboxes with all application source code repositories and dependencies installed.
- Support and bug fixes: To test user flows, reproduce bug reports, and validate bug fixes, AI agents should be able to dynamically recreate development, staging or production environments.
- Always-on research: To perform ongoing research, AI agents need to provision persistent databases, run continuously to perform searches and scrape web pages, prompt LLMs to analyze findings, and store or output results using various channels.
- Secure data processing: To ensure complete isolation and security when dealing with sensitive data, AI agents need to be able to create isolated environments for data transformation and analysis, then destroy them once done.
- Video processing: To perform processor- and memory-intensive video conversion and optimization, AI agents need the ability to provision compute and data storage infrastructure of different sizes.


## How it works


Blaxel’s platform is built for speed and security, offering agent-native compute environments – called sandboxes — with millisecond-boot times and full state persistence.


Think of sandboxes like a laptop for agents: close the lid, it suspends; open it, it resumes instantly where it left off, with full filesystem and memory preserved. Agents can do anything inside sandboxes, from running code and managing context, to deploying services and applications.


By integrating with Sapiom's gateway, agents can pay with their Sapiom wallets to spin up new environments, process batch jobs, deploy apps, and execute arbitrary code on Blaxel's infrastructure.


## Authentication and observability


Sapiom handles all authentication and payment transparently. When an agent makes a request for a Blaxel sandbox, the Sapiom SDK automatically performs authentication and negotiates access with Blaxel, including attaching the necessary payment headers with the request. There’s no need for a separate Blaxel account or API key - the Sapiom API key gives your agent access to Blaxel, as well as all other Sapiom-enabled service providers.


Sapiom also provides real-time checks and balances on agentic spend. You can define how much your agents spend on Blaxel services - by run, by agent or by time. All transactions are also logged and visible in the Sapiom dashboard. This ensures that when agents provision new sandboxes or start new batch workloads, it's done with complete transparency and control.


## Get started


Once a sandbox is running, Sapiom proxies both[Blaxel's sandbox API](https://docs.blaxel.ai/api-reference/fastapply/apply-code-edit) and[sandbox MCP server](https://docs.blaxel.ai/Sandboxes/MCP) for direct sandbox manipulation. This enables agents to perform all the key functions they need, including[process management](https://docs.blaxel.ai/Sandboxes/Processes) ,[filesystem operations](https://docs.blaxel.ai/Sandboxes/Filesystem) ,[code generation](https://docs.blaxel.ai/Sandboxes/Codegen-tools) , and[real-time public/private preview management](https://docs.blaxel.ai/Sandboxes/Preview-url) .


Get started today with[Blaxel](https://app.blaxel.ai/) and[Sapiom](https://sapiom.ai/) , or[visit our documentation](https://docs.blaxel.ai/) to learn more.
