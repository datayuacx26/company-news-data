---
schema_version: "1.0.0"
document_id: "762e929196279d98770c73a6363130946eb337cda5b4754bc5aeca9d21f60dee"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/elasticsearch-mcp-server-oauth-authentication"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T16:58:27.946710+00:00"
fetched_at: "2026-08-13T16:58:29.603066+00:00"
content_hash: "sha256:b5676a6f2a84f1332c91dd3993a5b963de9084641a9c50fbcc863da1b442c8e3"
---

# Your AI agent doesn't need your API key: OAuth 2.1 for Elasticsearch MCP server authentication

Claude Desktop, Cursor or any MCP host can now connect to your Elasticsearch data with a one-time browser sign-in. OAuth 2.1 is now GA for the[Agent Builder MCP server](https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/mcp-server) in Elastic Cloud Serverless. Instead of pasting an API key into a config file, your agent gets a short-lived token tied to your permissions. Every connection can be audited individually and revoked without affecting anything else on your account. Refresh tokens roll for 30 days, so you rarely need to sign in again. Org owners can see exactly who authorized which agents, and Agent Builder is the first Elastic surface using this model, with the rest of the Elastic API to follow.


## Why OAuth is better than API keys for MCP server authentication


With OAuth, tokens are short-lived credentials that expire on their own, so a leaked token is a narrowing window rather than a standing grant. Every token traces back to an explicit consent: which user, which client, which time.


In contrast, an API key is a long-lived credential. It lives in a configuration file on the machine that runs the agent, working for whoever uses it until someone rotates or deletes it. That model is manageable for a CI pipeline you wrote and deployed for your team. It gets uncomfortable when the key is held by an AI agent that assembles its own requests, retrieves untrusted content that may contain injected instructions, and sometimes passes context to sub-agents.


The failure mode is familiar from every credential-leak postmortem: the key ends up somewhere it shouldn't (a log file, a prompt), and from that moment anyone who has the key can access everything its creator could. The audit trail doesn't help much: API key logs tell you that a key with a given name did something, but not who authorized the client that used it or when.


**Attribute**


**OAuth 2.1**


**API Key**


Credential lifetime


Short-lived, auto-refreshing


Long-lived until manually rotated


Audit trail


User, client and timestamp per connection


Key name only


Revocation


Per connection, immediate


Requires key rotation


Blast radius


Single client connection


Everything the key creator can access


## How to set up OAuth 2.1 for the Elasticsearch MCP server


Setup is a one-time step per project:


1. In your Elastic Cloud Serverless project, open Agent Builder → Tools library → MCP clients → Create MCP client (OAuth). This gives you a client ID and the MCP server URL.


2. Add the server to your MCP host. In Cursor or Claude Desktop, the configuration file entry looks like this:


```text
{
"mcpServers": {
"kibana-mcp": {
"command": "npx",
"args": [
"mcp-remote",    "https://<your-project>.kb.<region>.aws.elastic.cloud/api/agent_builder/mcp",
"--static-oauth-client-info",
"{\"client_id\":\"MYCLIENTID111\"}"
]
}
}
}
```


3. The first time the agent calls a tool, the host opens your browser on an Elastic Cloud consent screen. You sign in with your normal Elastic Cloud credentials and see what the agent is requesting access to.


4. Click Authorize. This creates an[application connection](https://www.elastic.co/docs/deploy-manage/app-connections) between you, your machine, and your project, and Elastic Cloud issues the agent a short-lived access token.


From there, your agent has access to your project’s data. The mechanics are invisible. When the access token expires, the host refreshes it using a refresh token with a 30-day rolling expiry, so you are not re-authenticating every hour. If you stop trusting a connection, open the application connections page in Elastic Cloud and revoke the connection. Both tokens die immediately, and nothing else about your account changes.


The agent acts with the permissions of the user who consented. Calls to[Agent Builder tools](https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/tools) , such as ES|QL queries, Workflows, and Streams, are evaluated based on your role. If you want an agent with less access than your own, authorize the connection as a user with a tighter role, or keep using a scoped API key for that workload.


## How to manage and revoke MCP server connections in Elastic Cloud


Org owners can list every active application connection across an organization or a specific project: which MCP hosts were authorized, by whom, and when. Revocation is per connection, so cutting off one misbehaving agent does not disturb the authorizing user's account or any other integration. There is no shared credential to rotate and no blast radius beyond the one client.


This is the practical difference for teams. An OAuth app connection records who consented, to which client, and when. An API key log shows that` agent-key-3` queried an index, but not who authorized that agent.


## What's next for OAuth authentication across the Elastic API


Agent Builder is the first Elastic surface behind OAuth, and the same authorization model will carry to the rest of the Elastic API surface, including Elasticsearch and Elastic Cloud management, as we work toward a single Elastic MCP endpoint.


To try it now, open Agent Builder in an Elastic Cloud Serverless project and create an OAuth client, or start with the[documentation](https://www.elastic.co/docs/deploy-manage/app-connections/oauth-clients) . If you don't have a project yet, you can[start a free trial](https://cloud.elastic.co/registration) .


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


### Frequently Asked Questions


#### Should I use OAuth or an API key?


Use OAuth when a human is behind the agent: an MCP host on someone's laptop or an assistant acting on a user's behalf. Use API keys for non-interactive workloads, such as CI pipelines and scheduled jobs, where no user is present to provide consent.


#### What happens when the access token expires?


The host refreshes it automatically. Refresh tokens have a 30-day rolling expiry, so a connection you use regularly stays alive without re-authentication. Revoking the connection invalidates both tokens immediately.


#### Which MCP hosts work with this?


Any host that implements the MCP authorization specification works with OAuth. Claude Desktop and Cursor are both tested.


### Related Content


[Agentic AI](https://www.elastic.co/search-labs/blog/category/agentic-ai)


### Faster, cheaper support investigations with precomputed context


August 11, 2026 |


[Abhimanyu Anand](https://www.elastic.co/search-labs/author/abhimanyu-anand)


[AI](https://www.elastic.co/search-labs/blog/category/ai)[Agentic AI](https://www.elastic.co/search-labs/blog/category/agentic-ai)[ES|QL](https://www.elastic.co/search-labs/blog/category/esql)


### Building context in Elasticsearch: how AI Indices power smarter agents using fewer tokens


August 11, 2026 |


[Kathleen DeRusso](https://www.elastic.co/search-labs/author/kathleen-derusso) |


+3


[Agentic AI](https://www.elastic.co/search-labs/blog/category/agentic-ai)[Operations](https://www.elastic.co/search-labs/blog/category/operations)


### Your agents have been keeping receipts: turning Elastic Agent Builder's built-in OTel traces into token cost dashboards in Kibana


July 28, 2026 |


[Meghan Murphy](https://www.elastic.co/search-labs/author/meghan-murphy) |


+1


[Agentic AI](https://www.elastic.co/search-labs/blog/category/agentic-ai)[Integrations](https://www.elastic.co/search-labs/blog/category/integrations)


### One prompt, a complete workflow: Elastic's AI agent writes your automation for you


July 28, 2026 |


[Tinsae Erkailo](https://www.elastic.co/search-labs/author/tinsae-erkailo) |


+1
