---
schema_version: "1.0.0"
document_id: "1964674215aafc1ed9d1583cb37094249b4a81b9ec02713358e94458db69bdb7"
company_key: "yc-formal"
company: "Formal"
source_id: "yc-formal-news-import-a5b088b50d89"
canonical_url: "https://www.formal.ai/blog/permission-pitfall-mcp/"
published_at: "2025-06-03T00:00:00+00:00"
first_seen_at: "2026-07-27T09:22:31.452672+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:0084d8774a38f55133fc86d611ef36d3ae2d10e901b870fd3f07f966093a6432"
---

# The Permission Pitfall: Securing MCP Servers Without Limiting Value

Back in November, Anthropic introduced the[Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) (MCP), as the new (and now) standard way for connecting AI assistants to data sources and tools. The goal was to give LLM models the data context it needs to be effective at scale.


MCP has described as the “[USB-C of AI agents](https://medium.com/data-science-collective/mcp-is-a-security-nightmare-heres-how-the-agent-security-framework-fixes-it-fd419fdfaf4e) ” connecting AI agents to data and tools through a common interface. Instead of one-off integrations, MCP provides a consistent communication pattern, paving the way for flexible tool use and smarter systems.


Yet, with every new technology, there exists fundamental risks that are now being exploited.


## **Pulling on the thread of MCP risks**


Since the release in November, the AI and security community have detailed various risks that arise from MCP – from[rug pulls (silent redefinitions)](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/) to Whatsapp MCP server attacks that allowed for message history exfiltration. With every new technology we see a pattern of old risks surfacing as we saw with smart homes, connected cars, and more.


In the long list of possible attacks (which you can read more on at[vulnerablemcp.info](http://vulnerablemcp.info/) ), there’s a consistent thread tying many risks together – **the lack of authentication (authn) and authorization (authz).**


Across every resource that has come out on securing MCP, you’ll always see the recommendation for access controls, least privilege, service identity authentication, and further access-related precautions.


Fundamentally, the risk is that an agent can both unintentionally or maliciously be used to take action on data beyond its intended scope. At the end of the day, MCP is about data access; data access risks by an LLM is the prevalent theme.


## **How this played out with the Github MCP vulnerability **


On 5/26, Invariant labs discovered a[vulnerability](https://invariantlabs.ai/blog/mcp-github-vulnerability) in the[Github MCP integration](https://github.com/github/github-mcp-server) where an attacker can hijack a user’s agent via a malicious GitHub Issue, and prompt it into leaking data from private repositories.


### How does it work?


In summary, the vulnerability is an example of a **prompt injection **where attackers can hijack model behavior by embedding malicious instructions in prompts, causing the model to execute unintended agent calls.


In this case, an attacker can create a malicious issue on a user’s public GitHub repo, containing a prompt injection. The owner of the repo queries their agent which fetches the malicious issue and coerces the agent to pull private repo data into context. The data is then leaked into the auto-created PR in the public repo.


### Why could it happen?


At its core, what we’re seeing is a lack of implemented granular permissions.


Connecting Claude desktop to Github involves adding this JSON block to your User Settings (JSON) file.


Here, Claude Desktop authenticates to GitHub via a personal access token (PAT). In its current state, PATs are used to authenticate the agent to Github resources. These PATs are scoped on permissions you set to then authorize the agent to take scoped actions.


In the case with the Github MCP vulnerability, if you set your PAT to provide access to every repository in GitHub, a prompt injection can be potentially used to access data from any repo. However, if the PAT only grants access to public repos, a prompt injection to access private repos would fail.


## **Why permissions aren’t enough**


The challenge is that you can only apply permissions to the level of granularity expressed by the base system (e.g., GitHub). At the root of prompt injection is a lack of granular permissions to prevent exploitation.


In Github, when you create a PAT or Github App, you have the option to provide three levels of access: **No access, Read-only, Read and Write**


When you give write access to the **contents** permission, for example, you provide authorization across multiple endpoints.


With most GitHub apps or use cases for PATs (such as connecting to the Github MCP via Claude), you need a combination of permissions across various endpoints. In the case of a security code review bot that commits a fix, opens a PR, and comments a summary of the change, the bot needs read and write to both **contents **and **pull requests. **


The bot gain ability to make commits through the **contents **endpoint


It gets access to create a PR through the **pull requests **endpoint


One of the risks that exists is that you will also have to give access to merge PRs through the **contents **end point


However, given there’s no way to break out these permissions, an organization must accept the risk or implement additional mitigating controls (which often come with limitations).


Applying this back to the GitHub vulnerability, we saw that prompt injection can allow access to data from private repos. The way to prevent this is by correctly permissioning repo access.


The solution to prevent prompt injection that coerces an agent from merging a PRs is not possible through GitHub’s permissioning system without severely limiting its functionality.


## **What can we do?**


A solution to a limited permissioning system is to provide a proxy layer that defines an additional layer of permissions on top of what the system natively gives. This way, you’re not limited by GitHub but can go beyond to fully implement least privilege.


Take for example this PAT which has read and write access to **contents** and **pull requests** .


We set up the Github MCP server for Claude with the following JSON


With the GitHub MCP server configured, we asked Claude chat to create a new PR to update the README with MCP security details (an approved action).


And it was successful!


Then we asked Claude to merge a PR. This is an action we don’t want it to take but by nature of the given permissions it has the ability to do.


## **One proxy to rule them all**


In a normal GitHub MCP set up, Claude would have access to the MCP server which then through the PAT accesses Github to fetch data and take actions.


With Formal, you put a proxy between the GitHub MCP server and Github. In this scenario, the Github MCP server is accessing a GitHub resource in Formal. This allows you to put additional controls on the actions Claude can take through policies and also gain visibility into actions agents are taking.


With this, we built a policy that lets the agent take actions on Github but specifically blocking the ability to merge PRs.


We then prompted Claude to take the same action as before to merge an open PR. This time we see that the agent didn’t have the granular permissions to merge the PR!


The value here is that the agent can still create new PRs since the policy allowed for it. The goal is to not limit the ability of the agent but to provide the right guardrails in which it can operate.


## **Takeaways**


MCP is becoming the standard way for AI agents to obtain access to data sources. Yet, the push for granular permissions doesn’t truly mitigate risks because of the inherent limitation of each system. Even with OAuth,the agent just inherits the permission of the user, which is often too broad.


We believe that there’s a need for a centralized agent permission control system that allows users to granularly implement least privilege beyond the capabilities defined by a system.


This is why our focus at Formal has always been to decouple access from the underlying system itself. We’re excited for the next evolution of data connections for LLMs, and we believe that no matter what shape of data security risks come next, Formal will be able to secure the flow of data.


## **References**


- [MCP Security Research Briefing](https://www.wiz.io/blog/mcp-security-research-briefing)
- [MCP Is a Security Nightmare — Here’s How the Agent Security Framework Fixes It](https://medium.com/data-science-collective/mcp-is-a-security-nightmare-heres-how-the-agent-security-framework-fixes-it-fd419fdfaf4e)
- [Securing MCP: 5 Safeguards for Enterprise Teams](https://www.detectionatscale.com/p/securing-mcp-5-safeguards-for-enterprise)
- [MCP Security Checklist: A Security Guide for the AI Tool Ecosystem](https://github.com/slowmist/MCP-Security-Checklist?tab=readme-ov-file)
- [Model Context Protocol has prompt injection security problems](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/#rug-pulls-and-tool-shadowing)
- [The Vulnerable MCP Project](https://vulnerablemcp.info/)
- [GitHub MCP Exploited: Accessing private repositories via MCP](https://invariantlabs.ai/blog/mcp-github-vulnerability)
- [Github MCP Server](https://github.com/github/github-mcp-server)
