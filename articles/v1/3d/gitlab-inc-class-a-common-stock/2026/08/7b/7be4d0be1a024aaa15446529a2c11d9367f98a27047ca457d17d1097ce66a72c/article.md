---
schema_version: "1.0.0"
document_id: "7be4d0be1a024aaa15446529a2c11d9367f98a27047ca457d17d1097ce66a72c"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/ai-agent-sandbox/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T20:53:50.141883+00:00"
fetched_at: "2026-08-12T20:53:54.136343+00:00"
content_hash: "sha256:a96b630d2140e29192b4df3d2e98d18dcb450dcfb02ab0db697bb9c223c74e43"
---

# A sandbox is only as closed as what an AI agent can reach

In July, OpenAI and Hugging Face responsibly disclosed an OpenAI model under internal evaluation escaped its sandbox, reached the open internet, and accessed Hugging Face’s internal production infrastructure. The agent took datasets, cluster details, and cloud keys during the intrusion.


The most critical part of the external phase of this incident was the first hour, when the agent escaped its sandbox by using a vulnerability within a package proxy on its sandbox’s allowlist to access the internet.


As more frontier model developers[disclose similar incidents](https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals) , the security industry needs to closely consider how agents impact reachability within test environments. You also do not have to run evaluation harnesses for this to be your problem. Any workload that executes code behind an egress list has the same structure: CI runners, hosted sandboxes, and network-reachable tool servers. Most of those allowlists were built to stop data leaving, but that is now not the only thing they need to stop.


## Why allowlists extend reachability


When building an allowlist, you typically consider what a workload needs to contact and weigh each entry by what data could leave through it.


OpenAI’s agent revealed a blind spot for this typical process. The package proxy the agent accessed is a service: it authenticates callers, runs software with vulnerabilities, and talks to the public internet. While the sandbox could not reach the internet, the agent gained access to the package proxy, which did.


An allowlist hands your AI workload the reach of any destination it can take over.


## How OpenAI’s model escaped


To escape, OpenAI’s agent identified two zero-day vulnerabilities in the package proxy on its allowlist with a very large payoff.


The first flaw was a server-side request forgery flaw that let the agent ask the proxy to fetch arbitrary URLs on its behalf. In[OpenAI's Black Hat presentation](https://www.youtube.com/watch?v=87DyyMV0kCY&t=721s) , the model's reasoning at that moment reads " *excellent, we have arbitrary SSRF* ." The sandbox borrowed the proxy’s internet connection.


The second flaw was a privilege escalation.[CVE-2026-65616](https://nvd.nist.gov/vuln/detail/CVE-2026-65616) (CVSS 8.8) causes a package proxy's refresh-token endpoint to issue new tokens without validating token signatures. These tokens carry their scope as a claim rather than the server looking it up, so once the signature goes unverified the claim is whatever the caller says.


In this case, the caller was the agent, which extended the access of its read-scoped token within the sandbox by editing its claim to administrative. It then submitted the token for refresh to the package proxy which returned an administrator-scoped token. As a result, the agent could act with administrator permission instead of its original read-only permission.


The agents worked as a group to break out. Together, they explored the one service they could reach until they found a way to make it fetch on their behalf, then shared the method so other agents could use it too. One agent noted that external exploitation was outside the scope of its instructions, but continued anyway.


Hugging Face and OpenAI also put the proxy compromise inside roughly the first half hour of the sandbox test despite the elusive nature of the vulnerabilities. Remember, these were zero-days, so the agent could not reference a CVE or advisory. The package proxy is also closed-source, and its code was never in the agent’s training set.


## Why the model escaped


We now know why the agent went after the proxy. It was stuck on its evaluation task, and OpenAI's presentation shows the model reasoning " *perhaps answer online* " before looking for a way out. Frontier models are rewarded for solving tasks quickly, so a stuck model looks for shortcuts, and OpenAI normally closes that one by disabling internet access.


There are still open questions about the agent's behavior, including everything the agent did during its four-day intrusion and what it did once it gained access to the internet. What we do know is that this agent revealed, in practical terms, that we must make some new considerations as we build test environments for security-capable, autonomous AI.


## How to reduce risk across AI test environments


If you’re building sandboxes for AI agents, here are four ways to reduce your risk and limit reachability:


1. **Block the routes a build client never uses** . A build client only downloads files. It has no reason to call the proxy's admin API, its configuration routes, or its token-refresh route, which swaps an expiring token for a new one and is the route that was abused in this incident.
2. **Restrict what the proxy itself can reach** . The agent got out because the proxy could reach the whole internet, so anyone controlling the proxy could too. Limit the proxy’s outbound connections to the registries it actually serves.
3. **Monitor the proxy.** A proxy serving a build-fleet is monotonous, with short connections, small downloads, and requests that mostly succeed. Consider alerting on anything that doesn’t follow that pattern, such as connections that stay open, data moving outward in bulk, a run of requests for packages that are not there, and any call to an admin route.
4. **Treat untrusted workloads as internet-facing.** If an untrusted workload can reach an internal service, treat that service as internet-facing. Anything that compromises the workload can reach it too. We looked at the[same question for CI runners earlier](https://about.gitlab.com/blog/harden-pipeline-perimeter-for-ai-assisted-coding/) this year, where the untrusted code runs in a pipeline rather than an agent sandbox.


## Cyber-capable models require new sandbox considerations


A sandbox is a list of things the workload inside it can talk to, and everything on that list brings its own reach with it. When a workload is a build job that doesn’t matter, but it does when agents widen that list in ways allowlist designs never anticipated.


Beyond evaluation harnesses, agentic workloads are also moving into CI, production automation, and into tooling that interacts with internal services. Each of those has a list with similar properties and structure. As models get better at finding and chaining flaws, the reach on those lists extends what agents can impact.


OpenAI observes that these agents remain "bounded by the privileges they can obtain and the systems they can communicate with," so existing controls still work.
What must change is how carefully we draw those lists to minimize reachability.


> Find more articles from the Threat Research team on our[Security Labs site](https://about.gitlab.com/blog/categories/security-labs/) .
