---
schema_version: "1.0.0"
document_id: "2933f9a62bb049f7537d06454c27cbacaafbfa86b333f90e9d4b0436111b3723"
company_key: "yc-alter"
company: "Alter"
source_id: "yc-alter-rss-08ac1704852e"
canonical_url: "https://blog.alterauth.com/p/introducing-alter-vault"
published_at: "2026-05-27T18:27:04+00:00"
first_seen_at: "2026-07-24T15:31:02.362502+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:e3111d3d5e0c7cd885ec8b1fdb377c19b8292dcd8f4345f2d5372ad1345a5e10"
---

# Introducing Alter Vault

# Introducing Alter Vault


### The authorization layer for AI agents


[Alter](https://substack.com/@srikardandamuraju)


May 27, 2026


## **The problem: agents need API access, but credentials are a mess**


AI agents are becoming the primary way software interacts with external APIs. A single agent might read from Google Calendar, post to Slack, create a GitHub PR, and query Datadog, all in one task.


Today, most teams handle this with scattered environment variables, hardcoded tokens, and ad-hoc authorization logic bolted on after the fact. The result: tokens stored in plaintext databases, no visibility into which agent accessed what, no way to revoke access without redeploying, and zero audit trail when something goes wrong.


It works until it doesn’t. And when it doesn’t, you have no idea what happened.


## **What Alter Vault does**


Alter Vault is the authorization layer for AI agents. It sits between your agents and the APIs they call. Every request flows through the vault, which handles three things:


**Encrypted credential storage.** OAuth tokens and API keys are stored in an encrypted vault — never in your application database. Your code only holds a reference ID. Tokens are automatically refreshed before they expire, so agents never hit a 401.


**Identity-aware policy enforcement.** Every request is checked against two identities: the end user who authorized the connection


*and* the agent making the request. Policies define which agents can access which APIs, with what scopes, during what time windows. Unauthorized requests are blocked before they reach the provider. Fail-closed by default — if a policy can’t be evaluated, the request is denied.


**Dual-identity audit logging.** Every API call is logged with full context: the organization, the end user, the agent, the specific tool invocation, and the result. You get a complete chain of custody for every action, ready for SOC 2, GDPR, or HIPAA compliance.


## **Two types of credentials, one API**


Alter Vault supports two credential types, both accessed through the same


` alter_app.request()` method:


**OAuth connections** are end-user authorized. A user clicks “Connect Google” in your app, authorizes access through the standard OAuth flow, and the tokens are stored encrypted in the vault. The user can see and revoke their connections from the Wallet Dashboard at any time.


**Managed secrets** are developer-stored. API keys, service tokens, and other credentials that don’t require end-user authorization — stored through the Developer Portal and accessed with the same


` alter_app.request()` call. Supports Bearer tokens, API keys, Basic Auth, and AWS SigV4.


## **How a request flows through the vault**


When an agent calls


` alter_app.request()` , five things happen in under 15ms:


1.


**Identity resolution** — The vault resolves the full chain: organization, app, end user, and agent. If the agent registered with a


` run_id` or


` thread_id` , those are captured too.


2.


**Policy evaluation** — Access policies are checked against the resolved identity. Scope restrictions, time windows, and custom rules are all evaluated. If any check fails, the request is denied immediately.


3.


**Token retrieval** — The encrypted credential is decrypted from the vault. If the token is expired, it’s automatically refreshed before injection.


4.


**Request proxying** — The token is injected into the authorization header, and the request is forwarded to the provider. Connection pooling and retry logic are built in.


5.


**Audit logging** — The full request is logged with both identities (end user + agent), the policy decision, the provider response, and latency.


## **Agent identity tracking**


When multiple agents access the same credentials, you need to know exactly which agent did what. Alter Vault tracks agent identity at two levels:


**Agent registration** — each agent type (e.g., “scheduling-bot”, “data-pipeline”) is registered once. Every request from that agent is attributed to it in audit logs.


**Per-request context** — for agent frameworks like LangChain and LangGraph, pass


` run_id` ,


` thread_id` , and


` tool_call_id` on each request. This gives you full traceability through multi-step agent workflows, down to the individual tool invocation.


```text
from alter_sdk import App, ActorType, HttpMethod


alter_app = App(
api_key="alter_key_...",
actor_type=ActorType.AI_AGENT,
actor_identifier="scheduling-bot",
)


response = await alter_app.request(
grant_id,
HttpMethod.GET,
"https://www.googleapis.com/calendar/v3/calendars/{calendar_id}/events",
path_params={"calendar_id": "primary"},
query_params={"maxResults": "10"},
run_id="run_abc123",
thread_id="thread_xyz",
)
```


The TypeScript SDK has the exact same interface:


```text
import { App, ActorType, HttpMethod } from "@alter-ai/alter-sdk";


const alterApp = new App({
apiKey: "alter_key_...",
actorType: ActorType.AI_AGENT,
actorIdentifier: "scheduling-bot",
});


const response = await alterApp.request(
grantId,
HttpMethod.GET,
"https://www.googleapis.com/calendar/v3/calendars/{calendar_id}/events",
{
pathParams: { calendar_id: "primary" },
queryParams: { maxResults: "10" },
runId: "run_abc123",
threadId: "thread_xyz",
},
);
```


## **Three ways to connect end users**


Alter Connect supports three trigger methods depending on your application type:


-


**Headless** — for CLI tools, scripts, and Jupyter notebooks. The SDK opens a browser for the user to authorize, then returns the connection.


-


**Redirect** — for server-rendered apps. Your backend redirects the user to the hosted Connect page, and they’re redirected back after authorization.


-


**Popup** — for SPAs. The


` @alter-ai/connect` frontend SDK opens a popup window with the provider picker. No page navigation required.


All three methods result in the same thing: an encrypted credential in the vault and a connection ID your app can use with


` alter_app.request()` .


## **60+ providers, zero provider-specific code**


Alter Vault supports 50+ OAuth providers out of the box — Google, Slack, GitHub, Notion, Atlassian, Salesforce, HubSpot, Linear, and many more. Once a provider is configured in the Developer Portal, your agents access it through the same


` alter_app.request()` call regardless of the provider. No provider-specific token handling, no refresh logic, no scope management.


## **Get started**


Install the SDK and make your first authenticated API call in minutes:


```text
pip install alter-sdk
```


```text
npm install @alter-ai/alter-sdk
```


Read the


[full documentation](https://docs.alterauth.com/) to learn more, or sign up at the


[Developer Portal](https://portal.alterauth.com/) to create your first app.


[←](https://alterauth.com/blog)
