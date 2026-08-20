---
schema_version: "1.0.0"
document_id: "26d0c1ff0ff4b5349cd7304f614f17e7041879fdf85ffdfcd9cf0b2d8d33eb82"
company_key: "yc-doppler"
company: "Doppler"
source_id: "yc-doppler-news-import-86fd2de2d678"
canonical_url: "https://www.doppler.com/blog/how-to-secure-mcp-servers-for-ai-agents"
published_at: null
first_seen_at: "2026-07-25T01:53:43.668103+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:a2ce9125345618fb1dcfeb6593e718b4df80093e86844e0703f75bd9c5915e0a"
---

# How to secure MCP servers for AI agents

Every Model Context Protocol (MCP) quickstart guide gives you the same instructions. Add an env block to mcp.json


, paste in your API key, and run the server. Since everything works immediately, most developers move on without questioning whether storing secrets this way is actually safe or how it impacts their overall security posture.


It's only the first half of 2026, and GitGuardian has already found[24,008 unique secrets](https://blog.gitguardian.com/the-state-of-secrets-sprawl-2026/)


exposed in MCP configuration files. AI-service credential leaks are up 81% year-on-year. If you followed the quickstart docs, there's a real chance you're contributing to that number without knowing it.


You do not need to rebuild your entire agent infrastructure to fix this issue. Implementing fundamental security controls gets you most of the way there: credentials out of config files, each agent operating under its own limited token, and proper authentication in front of your MCP server so requests aren't trusted by default.


This article walks through each of those steps in detail, explains why moving credentials into a .env


file is not the fix, and outlines what a properly secured MCP setup should look like.


## Why everything your agent touches is a credential


To understand why[MCP security](https://www.doppler.com/blog/mcp-server-secure-secrets-management)


is so easy to get wrong, look at what an MCP server actually does. The server sits between your AI client and the external tools or data sources it depends on to connect LLMs to your infrastructure. If your agent queries a database, the MCP server holds the credentials required for that data access. If it calls a search API, the MCP server has the token. If it accesses an LLM endpoint, the MCP server has the API key.


That makes MCP a credential hub by design. And the credentials flowing through it govern your most sensitive business data, making them high-value targets for malicious actors. In a typical MCP-based system, those credentials usually fall into three categories:


- LLM API keys for services like OpenAI, Anthropic, and Google
- Service credentials such as database connection strings, search API tokens, and internal API keys
- Orchestration secrets for tools like LangChain, Hugging Face, Weights & Biases, and vector databases


Trend Micro reviewed a sample of MCP servers and found that around[48% recommended insecure storage methods](https://www.trendmicro.com/vinfo/us/security/news/vulnerabilities-and-exploits/beware-of-mcp-hardcoded-credentials-a-perfect-target-for-threat-actors)


, with most of those storing credentials in plaintext files like .env


or JSON. Nearly half the ecosystem is actively pointing developers toward patterns that open the door to cloud resource compromise, database access, and malicious code injection.


All of these credentials are[non-human identities](https://www.doppler.com/blog/what-is-non-human-identity-nhi)


and should be treated that way. They need scoped permissions, ownership, monitoring, and rotation policies. In practice, most teams treat them like ordinary config values. With credentials spread across every service your agent touches, the MCP threat model looks very different from that of a normal web application.


## Four attack vectors unique to agentic MCP systems


Attack vectors aren't peculiar to any one system, but the ones that emerge in MCP environments fall into four distinct categories.


Attack How it works


Confused Deputy


An attacker tricks the MCP client into performing actions or granting access on their behalf.


Token Passthrough


Tokens are accepted without properly validating where they came from, allowing credentials to be reused across systems.


Server-Side Request Forgery (SSRF)


The agent is manipulated into making requests to internal services or unintended destinations, bypassing standard network access restrictions.


Session Hijacking via Prompt Injection


Malicious prompts cause the agent to expose session tokens or act as another user.


What makes these security risks particularly dangerous is how they operate together. A prompt injection attack can trigger token passthrough to downstream APIs or internal APIs. SSRF can feed into a confused deputy scenario. One foothold compounds into the next because the agent keeps executing, and unlike traditional applications or traditional APIs, there is no human in the loop to notice something is wrong before the damage is done.


Most developers who recognize this land on the same fix. They pull credentials out of the config file and into .env


, and it feels like progress even though it doesn't meaningfully improve anything.


## Why .env isn't the fix


If you move your credentials from mcp.json


into a .env


file, you're only delaying the inevitable. Environment variables[can still leak](https://www.doppler.com/blog/environment-variables-secrets-management)


in several ways.


### Subprocess environment inheritance


When your[MCP server](https://www.doppler.com/blog/secure-mcp-servers-secrets-architecture)


executes tools (shell commands, scripts, or any child process), those processes inherit the full parent environment by default. Every credential loaded from mcp.json


's env block or a .env


file is automatically available to whatever your MCP tool spawns, granting full access to the inherited environment. If a tool's behavior can be influenced by agent input, a prompt injection attack targeting that tool's execution can reach any credential in the inherited environment. Explicitly scoping what each subprocess receives rather than inheriting the full environment is the mitigation, and almost no MCP server configuration does this by default.


### Agent filesystem reads


This is the one most developers haven't thought through. Claude Code, Cursor, and other AI clients have filesystem access. An agent working on a task can read credentials from either file directly into its context window, causing severe data exposure. Even if the file has read-only access, the credential is now in the model's working memory and can surface in outputs, logs, or downstream API calls. OWASP's MCP Top 10 classifies this as[MCP01](https://owasp.org/www-project-mcp-top-10/2025/MCP01-2025-Token-Mismanagement-and-Secret-Exposure)


: contextual secret leakage, where the model or protocol layer itself becomes an unintentional secret repository. Moving to .env


changes nothing about this, because the filesystem access model is the same regardless of filename.


### Lost or stolen machines


Plaintext credentials on disk are accessible to anyone who gains access to the disk; the filename is irrelevant. The .env


migration fixes the visual problem: your credentials aren't sitting in a named config file anymore. But it leaves the actual problem intact. Credentials at rest on the filesystem are readable by any process that knows where to look, including the agent you're trying to secure.


So if files aren't the answer, what is? Getting credentials off disk entirely.


## Best practices for securing MCP interactions


Most MCP security failures come from static secrets and shared identities; both must be removed to make agent systems safe at scale.


### Use runtime injection for credential delivery


Runtime injection replaces the traditional pattern of persisting credentials in configuration files with a just-in-time retrieval model. Instead of storing secrets in mcp.json


or similar files and loading them at startup, the credentials are fetched from a secrets manager at process launch. The credential exists only in memory for the lifetime of the process and is discarded when the process exits. Nothing is written to disk, and no secret is embedded in configuration artifacts.


In a traditional setup, credentials are embedded directly in the configuration file. This makes the config file the authoritative source of the secret and introduces long-lived exposure risk, especially if the file is copied, committed, or shared.


In the runtime injection model, the configuration no longer contains secrets. Instead, it wraps the execution command so that a secrets manager injects credentials at runtime.


With this approach, rotation is handled upstream in the secrets manager. Updating a credential does not require modifying configuration files, and a leaked configuration no longer exposes any usable secrets. Auditability also improves, since credential access is recorded at fetch time and tied to a specific identity rather than being embedded and static.


### Use per-agent tokens, not shared keys


In many agentic systems, multiple agents share a single LLM API key for convenience. This simplifies setup but removes identity boundaries and makes attribution impossible when something goes wrong. If one agent is compromised, the attacker inherits the same credentials used by all other agents. Rotating the key also becomes disruptive since it invalidates access for every agent, including those not involved in the incident.


The right model is a token-based authentication approach with one scoped access token per agent or MCP server. This allows you to enforce role-based access control so each action maps cleanly to a legitimate user or isolated service.


As shown in the diagram above, a shared credential creates a single point of attribution failure while scoped tokens restore per agent identity.


It is also worth noting that agents consume credentials autonomously and at high frequency, which can lead to non-human identity sprawl. Because of this, provisioning and rotation cannot be handled manually and should be managed through a centralized secrets infrastructure with automated lifecycle control.


Once credentials are scoped per agent, the next security layer is the MCP boundary itself.


## How to prove identity to your MCP server


Everything so far has focused on how credentials are stored and scoped within agents. The next question is external trust. How does an MCP server verify that an incoming client is actually allowed to connect and act on its behalf? The table below ties different authentication methods to their appropriate use cases.


Authentication method Use case Minimum requirement


OAuth 2.1 with PKCE


Remote public agents


JWT validation with JWKS caching


Mutual TLS (mTLS)


Trusted internal agents


Certificate rotation and revocation


API key with rotation


Local development only


Strict rotation policy, not production use


The[OAuth flow](https://modelcontextprotocol.io/specification/draft/basic/authorization)


should be your default for remote agents, utilizing dynamic client registration and explicit user approval where applicable. You should validate every request using JWT verification against a cached JWKS set with a short TTL, typically around five minutes, with automatic refresh on validation failure. One thing that gets missed often is audience validation. Every token you accept should explicitly target your MCP server. If a token is valid but was issued for another service, reject it.


In multi-service setups where one MCP server calls another while carrying a user context, preserve the original user identity across services instead of replacing it with a service identity. This keeps attribution intact when requests pass through multiple systems and simplifies incident response.


## Looking forward: Your AI agent secrets checklist


By this point, the pattern should be clear. Keep secrets off disk, scope credentials per agent, and require explicit authentication at the MCP boundary. As a quick audit, your setup should roughly look like this:


- No secrets stored in mcp.json


, .env


, or agent-readable files
- Runtime injection used for credential delivery
- One scoped token per agent or MCP server
- Automated rotation and audit logging
- OAuth 2.1 or mTLS protecting remote MCP servers
- JWT audience validation and tool-level RBAC enabled
- Outbound requests restricted to approved destinations
- Rate limiting enforced per agent identity to prevent resource exhaustion and abuse


The[24,008 exposed secrets](https://blog.gitguardian.com/the-state-of-secrets-sprawl-2026/)


discovered in the MCP configuration files are an ecosystem problem. The documentation told people to store credentials in config files, and they did. What breaks down in agentic systems is scale. Agents create and consume non-human credentials continuously, which turns practices that were merely risky in traditional applications into a serious operational problem.


If you want to implement these security policies and controls without building the infrastructure yourself, Doppler provides runtime injection, secret scoping, and audit logging purpose-built for MCP-based agent workflows. Try a[Doppler demo](https://www.doppler.com/demo)


to see how this maps onto a real setup.
