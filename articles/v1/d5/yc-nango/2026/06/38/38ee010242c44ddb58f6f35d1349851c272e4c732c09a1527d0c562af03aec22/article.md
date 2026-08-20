---
schema_version: "1.0.0"
document_id: "38ee010242c44ddb58f6f35d1349851c272e4c732c09a1527d0c562af03aec22"
company_key: "yc-nango"
company: "Nango"
source_id: "yc-nango-news-import-e2ee807d6673"
canonical_url: "https://nango.dev/blog/id-jag-agent-authentication/"
published_at: "2026-06-26T00:00:00+00:00"
first_seen_at: "2026-07-22T05:12:58.696716+00:00"
fetched_at: "2026-07-28T21:23:16.325685+00:00"
content_hash: "sha256:e2b20c5d522a59e54ebbea43f79e20eb0e0aa41ee6b1082d266a3e119cea3f05"
---

# How ID-JAG helps AI agents authenticate

AI agents need access to third-party APIs to act on a user’s behalf. So far, that access is mostly approved by the users themselves, clicking “Allow” on an OAuth screen for every API. In an organization where many people use the same agent, each user has to authorize every API. This doesn’t scale.


Organizations already control which apps their employees can use, and they want the same control and visibility over the API integrations their agents rely on. ID-JAG gives them that by moving the decision from the user to the organization’s identity provider (IdP).


On June 18, 2026, MCP shipped[enterprise-managed authorization](https://blog.modelcontextprotocol.io/posts/enterprise-managed-auth/) , and the OAuth extension behind it, ID-JAG, went from something conceptual to running in Claude, VS Code, and popular SaaS apps.


## What is ID-JAG?


[ID-JAG](https://datatracker.ietf.org/doc/draft-ietf-oauth-identity-assertion-authz-grant/) stands for Identity Assertion JWT Authorization Grant. It lets your identity provider (IdP), like Okta, give an AI agent a scoped, short-lived token for another app’s API, like Salesforce, without sending every user through a consent screen or handing the agent a long-lived API key.


It does not invent a new protocol. It chains two that already exist: OAuth[token exchange](https://datatracker.ietf.org/doc/html/rfc8693) to get the ID-JAG from your IdP, and the[JWT bearer grant](https://datatracker.ietf.org/doc/html/rfc7523) to trade it for a real access token at the target app.


The ID-JAG itself is a signed token (a JWT) that says, in effect: this app can get a token for that API, on behalf of this user, with these scopes. Okta’s implementation of it is called[Cross-App Access (XAA)](https://developer.okta.com/blog/2025/09/03/cross-app-access) .


## How it works


Without ID-JAG, every user connects every app themselves. A thousand employees using an AI assistant that reaches Salesforce means a thousand trips through Salesforce’s consent screen, one per person. Each grant is between that employee and Salesforce, so the company has no central control over which agents reach which apps, and nowhere to revoke it all at once.


ID-JAG moves that decision to the identity provider. The agent never asks the user to approve anything. It asks the IdP, which already knows what each employee is allowed to use:


1. The user signs in to the agent through their normal SSO, and the IdP issues an ID token.
2. The agent sends that ID token back to the IdP and asks for access to a specific app. The IdP checks the org’s policy: can this person, through this agent, reach Salesforce?
3. The IdP returns the ID-JAG, a signed token naming the agent, the user, and the app it is good for.
4. The agent presents the ID-JAG to Salesforce, which confirms the trusted IdP signed it and returns an access token.
5. The agent calls the API with that token.


The request in step 2 is a token exchange with the IdP:


#####


```text
POST   /token
grant_type=urn:ietf:params:oauth:grant-type:token-exchange
requested_token_type=urn:ietf:params:oauth:token-type:id-jag
subject_token=<the user's ID token>
audience=https://salesforce.example.com   # the app you want to reach
```


The agent then trades the ID-JAG at the app using the JWT bearer grant (` grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer` ).


When the access token expires, it comes back to the IdP for a new one rather than holding a refresh token of its own. That is what lets a company control and revoke agent access from the IdP.


## Why ID-JAG matters


The use case today is enterprise control: an admin sets one policy at the IdP for what agents can reach, and can revoke that access from one place.


That is why it is being adopted so quickly. The MCP launch shipped with Okta as the first IdP, Claude and VS Code as the clients, and Asana, Atlassian, Canva, Figma, Granola, Linear, and Supabase as the first apps behind it, with Slack on the way. ID-JAG is a standard OAuth grant, so any IdP and any app can adopt it.


## Where ID-JAG falls short


ID-JAG is new, and it has the rough edges of anything new. Org-level control over agent access is a real step forward. But from building auth across 900+ APIs, we see a few limits that hold it back today:


- **Every app has to support it, and most don’t yet:** your IdP supporting ID-JAG is not enough on its own. Each app or MCP server the agent reaches has to support it too. That list is very short today, but this could grow over time.
- **It only works over OAuth:** ID-JAG is built on OAuth, so a provider that uses API keys, plain JWTs, or its own scheme cannot take part unless it switches to OAuth. That leaves out a lot. Plenty of APIs are key-only (Stripe platform operations, SendGrid, most analytics tools), and plenty that do use OAuth ship their own version of it (see[why OAuth is still hard in 2026](https://nango.dev/blog/why-is-oauth-still-hard) ).
- **It controls who can act, not what they do:** the token says the agent may reach Salesforce as this user but it does not control what the agent does with that access nor make your agent inherently secure against prompt injection attacks (see[building reliable tool calls for AI agents](https://nango.dev/blog/build-reliable-tool-calls-for-ai-agents-integrating-with-external-apis) ).


## Common questions about ID-JAG


**Is ID-JAG replacing OAuth?**


No. ID-JAG is an OAuth extension, not a replacement. It reuses token exchange and the JWT bearer grant, and it sits on top of the OAuth your apps already run for SSO. What it removes is the per-app consent screen for apps that share an IdP.


**What is the difference between ID-JAG and XAA?**


ID-JAG is the standard: the grant type and the signed token. Cross-App Access (XAA) is Okta’s implementation and branding of it. Other identity providers can implement ID-JAG and call it something else.


**How is ID-JAG different from transaction tokens and workload identity federation?**


They secure different parts of an agent’s path, and they stack rather than compete:


Standard What it secures Example


ID-JAG A human delegating to an agent that calls another app's API Claude reaching a user's Asana


Transaction tokens Who is acting for whom as a request crosses internal services An agent's request fanning out across your own microservices


Workload identity federation A workload getting short-lived credentials instead of a static key An agent runtime authenticating to its model provider


**Is ID-JAG only for MCP?**


No. ID-JAG is a general OAuth grant. MCP’s enterprise-managed authorization is the first high-profile thing built on it, but any OAuth client and app can implement the same flow.


ID-JAG is a real step for the apps already inside your identity provider. Everything else an agent has to reach is still per-provider work, and one standard cannot cover all of it.
