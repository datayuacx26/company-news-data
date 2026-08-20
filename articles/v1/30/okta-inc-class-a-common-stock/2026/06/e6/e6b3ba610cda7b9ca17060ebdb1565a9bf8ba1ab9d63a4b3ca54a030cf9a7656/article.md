---
schema_version: "1.0.0"
document_id: "e6b3ba610cda7b9ca17060ebdb1565a9bf8ba1ab9d63a4b3ca54a030cf9a7656"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/ai/ai-agent-security-vs-service-apps/"
published_at: "2026-06-24T07:00:00+00:00"
first_seen_at: "2026-07-31T19:31:06.584573+00:00"
fetched_at: "2026-07-31T19:31:07.432136+00:00"
content_hash: "sha256:294b0cf602a176740076453d0404846df4630b8d39ab7149cdb482d2974708b4"
---

# An AI agent is not a service app: Securing agentic identity

### Topics


---


Privileged Access Management


,


AI Agents


,


Secure Access


,


IAM


### Table of Contents


---


---


### Share


-
-
-


---


Ready to make Identity a business advantage? Sign up today.


[Get started](https://www.okta.com/free-trial/)


## Executive summary: Securing AI agents at the identity layer


- **The core security flaw:** Enterprises are registering AI agents as traditional service apps to accelerate deployment, inadvertently creating critical privilege escalation vectors in their agentic stacks.
- **The risk demonstrated:** In testing against a finance MCP server, an agent modeled as a service app running via an OBO token exchange bypassed user-specific restrictions, granting a standard employee access to the company’s entire salary ledger.
- **The solution:** Transitioning from service apps to a first-class identity model utilizing Cross-App Access (XAA) ensures the authorization server evaluates both user and agent privileges, enforcing the most restrictive access ceiling.
- **Strategic impact:** First-class identity alignment allows agent deployments to natively inherit existing enterprise directory governance, compliance auditing, and continuous threat mitigation tools.


## What’s the privilege escalation risk in AI agent architecture?


As enterprises rush to put AI agents into production, the same shortcut keeps showing up: To ship fast and keep costs down, teams model the agent as a service app instead of giving it an identity of its own. The instinct is understandable, but the move is still wrong.


### Technical proof: Finance MCP server test results


**Model an AI agent as a service app, and you have built a privilege escalation into your agentic stack.** The agent can be issued more access than the person it acts for. In our test against a finance Model Context Protocol (MCP) server, an employee compensation assistant agent was given the scope to read every salary in the company, including the CEO's, and to change anyone's pay, none of which the employee could do.


Give the same agent a first-class identity, and the identical request is denied. Same user, same policy. The only thing that changed was the identity model, and the tokens in this post prove it.


A service app is the right tool for stable, service-to-service traffic, and it remains so there. It is the wrong identity for an AI agent. An agent reasons and bends to whatever lands in its context at runtime, so a shared service app credential gives that agent more reach than any other authorized user.


Agents are a distinct class of identity. Model them as one, or you give up the ability to govern what they do. The table below sets the two choices side by side.


### Comparing AI agent identity models: Service app vs. first-class identity


Dimension Agent modeled as a service app Agent modeled as a first-class identity


**Privilege escalation risk**
*Scenario: Agent requests more access than the delegating user holds* Bypasses user-specific access controls. The agent inherits broad application scopes and assumes privileges beyond the authoring user’s ceiling, creating an immediate unauthorized data-exposure vector.[Okta for AI Agents](https://www.okta.com/products/govern-ai-agent-identity/) evaluates both user and agent permissions dynamically. If an agent requests a scope that exceeds the user’s entitlements, the system rejects the call.


**Governance and compliance**
*Framework alignment for certification and auditing* Lacks an underlying identity object, forcing teams to build custom governance tools. Raises operational overhead and introduces compliance gaps that lead to enterprise audit failures. Inherits existing enterprise identity and access management (IAM) governance frameworks natively. Eliminates custom compliance engineering by running agent actions through standard directory review processes.


**Kill switch capabilities**
*Enforcing instant threat mitigation at runtime* Requires proprietary, siloed gateway-level builds with no shared framework standards. Blindsides the core identity provider (IdP) control layer by pushing session management to runtime. Integrates natively with Okta[Identity Threat Protection](https://www.okta.com/products/identity-threat-protection/) via open standards, including the Shared Signals Framework ([SSF](https://openid.net/wg/sharedsignals/) ) and the Continuous Access Evaluation Protocol ([CAEP](https://openid.net/wg/sharedsignals/) ). Enables immediate universal logout and global token revocation across agent sessions.


**Agent registry and onboarding**
*Provisioning multi-platform automated agents* Mandates manual provisioning and maintenance of custom service app wrappers to track automated agents across disparate, multi-platform sources. Streamlines cross-platform deployment by leveraging industry-standard protocols, such as[SCIM](https://datatracker.ietf.org/doc/html/rfc7644) , to onboard and register agents seamlessly into a central enterprise directory.


The pattern repeats down every row. A service app answers one question: What application is this?


An agent forces a harder question at runtime: Who authorized this action, and on whose behalf? Only an identity built for the agent can answer it.


## What’s the difference between an app and an AI agent?


The fastest way to get an AI agent talking to your systems is to register it as a service app and let it run an[RFC 8693](https://datatracker.ietf.org/doc/html/rfc8693) token exchange to act on behalf of the user. It works. An app does what it is coded to do, and nothing more. Its permissions are its behavior, fixed and predictable.


An agent has a brain. It decides at runtime what to call and what to reach, and it bends to whatever lands in its context. That is the point of an agent, and it is the reason the shortcut is dangerous. Hand a service app a broad, shared set of permissions, and the risk stays the same. Hand the same permissions to an agent, and it does not. You have given a reasoning agent the full reach of that access, used in ways you never scripted.


The stakes are higher than with any app you have shipped because the app cannot be redirected—but the agent can. And you cannot answer for it afterward. A shared service app cannot demonstrate whether the agent remained within the authority of the user it was acting on behalf of. When something goes wrong, the trail remains vague.


## How user entitlement ceilings dictate access control


Picture a finance MCP server that exposes employee compensation. It defines three scopes:


- ` Salary:Read:Self` : Read your own pay
- ` Salary:Read:All` : Read every employee's pay, including executives and the CEO
- ` Salary:Write` : Change anyone's pay


Access is granted by group, as you already do with role-based access control (RBAC).


Group` Salary:Read:Self`` Salary:Read:All`` Salary:Write`


Company employees (Charlie) **✓**


– –


Compensation planners (Alice) **✓**


**✓**


–


CFO office (Bob) **✓**


**✓**


**✓**


Charlie is an engineer, entitled to` Salary:Read:Self` . Alice plans compensation, so she reads everyone but makes no changes. Bob runs finance, so he reads everyone—the CEO included—and he changes pay.


Three people, three different ceilings. Now, each of them asks an assistant agent to work with compensation data, and the agent asks for more than the person is authorized to do. Hold that thought.


## Why service app architectures cause privilege escalation


Effective permission should be the intersection of what the agent is allowed, what the user is allowed, and what the resource exposes. The safe answer is the overlap. Model the agent as a service app, and the user drops out of that intersection, leading to privilege escalation.


### The intersection of effective user, agent, and resource permissions


Figure 1. Effective permission is an intersection. The green center is what Charlie can do. The red region is privileges he never had, the escalation.


Fill out the form to access this content.


## Testing OBO token exchange vs. Cross-App Access (XAA)


We modeled the same agent in two ways and ran every user through the same three requests. In the first model, the agent is a service app running an on-behalf-of (OBO) token exchange. In the second, the agent is a first-class identity using[Cross-App Access](https://www.okta.com/identity-101/cross-app-access-securing-ai-agent-and-app-to-app-connections/) (XAA), the[ID-JAG](https://datatracker.ietf.org/doc/draft-ietf-oauth-identity-assertion-authz-grant/) flow co-authored by Okta.


Same users, same groups, same resource-server policies. The only thing that changed was the identity model.


The tokens below are decoded. The hostnames and email addresses are sanitized; the scopes,` sub_profile` values,` act` chain, and error are returned exactly as received from the authorization server.


## One model carries the user. The other does not.


Same agent, same finance MCP server, two ways to model the agent's identity:


### An AI agent modeled as a service app using an OBO token exchange


Figure 2. Agent modeled as a service app. The user is attributed at the client app, then the call runs on-behalf-of through an OBO token exchange. There is no separate agent identity in the exchange, so the actor on the final token is the service app.


Fill out the form to access this content.


### An AI agent modeled as a first-class identity using XAA


Figure 3. Agent modeled as a first-class identity. The agent is a first-class identity. It receives an ID-JAG, a signed delegation assertion, and exchanges it for the MCP token. The token names both the user and the agent.


Fill out the form to access this content.


## The escalation, in evidence: The service app grants whatever it's asked


We ran each user through the same three requests, each asking for a little more than the last. Here are the requests, the models' outputs, along with the deciding claim for each token.


The tool call


Agent modeled as a service app


Agent modeled as a first-class identity


**Charlie, a company employee entitled to` Salary:Read:Self`**


Requests` Read:Self`
*his own scope*


**✓**


Granted.
` "scp": \["Salary:Read:Self"\]`


**✓**


Granted, and the token names the agent.
` "scp": \["Salary:Read:Self"\], "act": ai_agent`


Adds` Read:All`
*past his group*


✗


Granted. Reads every salary, the CEO's included (privilege escalation).
` "scp": \["Salary:Read:All", "Salary:Read:Self"\]`


**✓** Denied. Past Charlie's entitlement.
` "error": "access_denied"`


Adds` Read:All` and` Write`
*read everyone, change pay*


✗


Granted. Reads every salary and rewrites pay (privilege escalation).
` "scp": \["Salary:Read:All", "Salary:Read:Self", "Salary:Write"\]`


**✓**


Denied. The boundary holds.
` "error": "access_denied"`


**Alice, a compensation planner entitled to` Salary:Read:Self` and` Salary:Read:All`**


Requests` Read:Self`
*within her entitlement*


**✓**


Granted.
` "scp": \["Salary:Read:Self"\]`


**✓**


Granted.
` "scp": \["Salary:Read:Self"\]`


Adds` Read:All`
*within her entitlement*


**✓**


Granted.
` "scp": \["Salary:Read:Self", "Salary:Read:All"\]`


**✓**


Granted. Alice can read every salary.
` "scp": \["Salary:Read:Self", "Salary:Read:All"\]`


Adds` Write`
*past her entitlement*


✗


Granted. Alice can now change anyone's pay. Escalation.
` "scp": \["Salary:Read:All", "Salary:Read:Self", "Salary:Write"\]`


**✓**


Denied. Write is past Alice's entitlement.
` "error": "access_denied"`


Three calls crossed the line. In each one, the service app issued access that the person never had, and the first-class identity path refused it:


- **Charlie, an engineer, asks for` Read:All` :** The service app lets his agent read every salary in the company, including the CEO's.
- **Charlie asks for` Read:All` and` Write` :** The service app lets his agent read every salary and change anyone's pay.
- **Alice, a read-only planner, asks for` Write` :** The service app lets her agent alter compensation she could never touch herself.


The first-class identity path denied all three. Notice where the security failures fall (the red cells in the table above). Charlie's line is at` Read:All` , and Alice's is at` Write` because each is capped at their own entitlement. The service app cannot see that line, so it grants whatever is asked. The first-class identity path is built around the user’s entitlement.


Bob, the CFO, is the control. He is entitled to all three scopes, so there is nothing to escalate, and both models grant him everything. That is the system working, not failing. Each identity has its own ceiling, and the escalation appears only when the agent exceeds the user's.


This is not hypothetical. In the[PocketOS incident](https://thenewstack.io/ai-agents-credential-crisis/) , an AI coding agent found a long-lived, over-scoped credential and used it to delete a production database in seconds. The credential carried far more authority than the task required, and nothing bound the agent to a smaller set.


While the path differs from ours—a standing token in a file rather than a token over-issued by an exchange—the failure belongs to the same family: An agent operating with privileges the situation never warranted. Bind the agent to what the user can do, and its reach collapses to what the user holds.


One nudge is all it takes. A prompt injection in a document the agent reads, or a hijack of the agent, turns “show me my comp” into “export every salary” or “set this person's pay.” The user was never allowed to do either; the agent, wearing the service app's identity, is.


## The three escalations, the full flow side by side


Here is a side-by-side comparison of each escalation as a complete flow using the service app and first-class identity models. The user signs in, the agent presents its delegation, and a token reaches the salary tool. The sign-in is the same for every request, so the divergence is everything below it.


### Scenario 1: Charlie, an engineer, asks for` Read:All`


Agent modeled as a service app


Agent modeled as a first-class identity


**User signs in (User ID Token)**


` {
"ver": 1,
"jti": "AT.6qfZ1mzOv7xRLJRDs4Euf7T8GT8KoUaWvf8dtyqBv84",
"iss": "https://acme.okta.com/oauth2/aus10bui9u2uVUeSf1d8",
"aud": "https://acme.example/finance-agent/authz",
"iat": 1781989570,
"exp": 1781993170,
"cid": "0oa10bttyvxad8SuK1d8",
"uid": "00u108raja7J5YWJo1d8",
"scp": \["openid", "Agent:Invoke"\],
"auth_time": 1781989568,
"sub": "charlie@acme.example"
}`


` {
"sub": "00u108raja7J5YWJo1d8",
"ver": 1,
"iss": "https://acme.okta.com/oauth2/aus10bui9u2uVUeSf1d8",
"aud": "0oa10bttyvxad8SuK1d8",
"iat": 1781947092,
"exp": 1781950692,
"jti": "ID.8ocze3eVHdOpuvU7csm6dBAyXkW4xyyOso7jY4GvDqM",
"amr": \["pwd"\],
"idp": "00ow25t4lmReOO2ol1d7",
"nonce": "e45d256f-63b4-4e35-ba17-1e7b3e481c4b",
"auth_time": 1781947090,
"at_hash": "VHFx9z9e7Wo6VwCXemkjNQ"
}`


**The agent gets a token for the tool**


*No ID-JAG here. The agent reuses the service app's OAuth client through an OBO token exchange (RFC 8693). Nothing in this step carries the user's entitlement into the decision.*


` {
"jti": "IDAAG.d2uOKx7WXgKLa_OHf28FrO9Y9SdDFrr5ThuZ3jJybwM",
"iss": "https://acme.okta.com",
"aud": "https://acme.okta.com/oauth2/aus10buo26zirzVB81d8",
"iat": 1781947272,
"exp": 1781947572,
"sub": "00u108raja7J5YWJo1d8",
"client_id": "wlp10bums7yMBeHST1d8",
"sub_profile": "user",
"scope": "Salary:Read:Self Salary:Read:All",
"act": {
"sub": "wlp10bums7yMBeHST1d8",
"sub_profile": "ai_agent",
"act": {
"sub": "0oa10bttyvxad8SuK1d8",
"sub_profile": "web_app"
}
}
}`


**Token at the salary tool**


` {
"ver": 1,
"jti": "AT.TyzgwmPtlWiVe0yJGm6dleDLu-61OYM2F8v8j9lhlDM",
"iss": "https://acme.okta.com/oauth2/aus10buo26zirzVB81d8",
"aud": "https://acme.example/finance-resource/authz",
"iat": 1781990877,
"exp": 1781994477,
"cid": "0oa10bu53fp7MIpxx1d8",
"uid": "00u108raja7J5YWJo1d8",
"scp": \["Salary:Read:All", "Salary:Read:Self"\],
"auth_time": 1781990877,
"sub": "charlie@acme.example"
}`


` {
" **error** ": "access_denied",
" **error_description** ": "Policy evaluation failed for this request, please check the policy configurations."
}`


**Observation**


**Escalation.** Charlie is entitled to` Salary:Read:Self` and nothing more. Yet, this token carries` Salary:Read:All` , allowing his agent to view the entire company payroll, including the CEO's salary. The service app path blindly granted the over-scoped request.


**Denied.** Same request, same policy. Because the resource server sees Charlie behind the agent, it recognizes that the requested scope exceeds his entitlements and rejects the call. The agent can do exactly what Charlie can do, nothing more.


### Scenario 2: Charlie asks for` Read:All` and` Write`


**Agent modeled as a service app** **Agent modeled as first-class identity**


**User signs in (User ID Token)**


*Charlie's sign-in, the same token shown in scenario 1.* *Charlie's sign-in, the same token shown in scenario 1.*


**The agent gets a token for the tool**


*No ID-JAG here. The agent reuses the service app's OAuth client through an OBO token exchange (RFC 8693). Nothing in this step carries the user's entitlement into the decision.*` {
"jti": "IDAAG.N6h1UIfRCIoHU89wb1P7CWiEnRW7AXemXmaqVNDOFL0",
"iss": "https://acme.okta.com",
"aud": "https://acme.okta.com/oauth2/aus10buo26zirzVB81d8",
"iat": 1781947479,
"exp": 1781947779,
"sub": "00u108raja7J5YWJo1d8",
"client_id": "wlp10bums7yMBeHST1d8",
"sub_profile": "user",
"scope": "Salary:Read:Self Salary:Read:All Salary:Write",
"act": {
"sub": "wlp10bums7yMBeHST1d8",
"sub_profile": "ai_agent",
"act": {
"sub": "0oa10bttyvxad8SuK1d8",
"sub_profile": "web_app"
}
}
}`


**Token at the salary tool**


` {
"ver": 1,
"jti": "AT.rZUYrlBqPZ6T8CFPA3kN3linHXw-d6DO_QfSwOqoEJA",
"iss": "https://acme.okta.com/oauth2/aus10buo26zirzVB81d8",
"aud": "https://acme.example/finance-resource/authz",
"iat": 1781990290,
"exp": 1781993890,
"cid": "0oa10bu53fp7MIpxx1d8",
"uid": "00u108raja7J5YWJo1d8",
"scp": \["Salary:Read:All", "Salary:Read:Self",
"Salary:Write"\],
"auth_time": 1781990290,
"sub": "charlie@acme.example"
}`


` {
" **error** ": "access_denied",
}`


**Observation**


**Escalation.** Now the token carries both` Read:All` and` Write` . The agent can view everyone’s salary and modify anyone's pay in the company. Because the service app path bypassed his identity entirely, the agent gained escalated permissions that Charlie himself does not have. **Denied.** Because the request exceeds Charlie’s entitlements, the token issuance is rejected. High-privilege scopes like` Read:All` and` Write` remain firmly out of the agent's reach.


### Scenario 3: Alice, a read-only planner, asks for` Write`


**Agent modeled as a service app** **Agent modeled as first-class identity**


**User signs in (User ID Token)**


` {
"ver": 1,
"jti": "AT.OPD_MKodcVirdXDYQfN9-W2KRcWKhUim8p6PuQbyE4A",
"iss": "https://acme.okta.com/oauth2/aus10bui9u2uVUeSf1d8",
"aud": "https://acme.example/finance-agent/authz",
"iat": 1781997411,
"exp": 1782001011,
"cid": "0oa10bttyvxad8SuK1d8",
"uid": "00u108r403dASqam21d8",
"scp": \["openid", "Agent:Invoke"\],
"auth_time": 1781997409,
"sub": "alice@acme.example"
}`


` {
"sub": "00u108r403dASqam21d8",
"ver": 1,
"iss": "https://acme.okta.com/oauth2/aus10bui9u2uVUeSf1d8",
"aud": "0oa10bttyvxad8SuK1d8",
"iat": 1782111930,
"exp": 1782115530,
"jti": "ID.KIyLS98FrfgOi6ikQ0DxIqxGXmWcmrMIfsxHy-dQNXw",
"amr": \["pwd"\],
"idp": "00ow25t4lmRe0O2oI1d7",
"nonce": "1110a129-38c6-482d-9b73-341ff914f317",
"auth_time": 1782111928,
"at_hash": "KEkc-udXIK3u4eSRzohTjQ"
}`


**The agent gets a token for the tool**


"jti": "IDAAG.7p-s6CJ7i8HJy-d4AI1qK3UqDXNTmu2HxnSHi7gTP24",
"iss": "https://acme.okta.com",
"aud": "https://acme.okta.com/oauth2/aus10buo26zirzVB81d8",
"iat": 1782112179,
"exp": 1782112479,
"sub": "00u108r403dASqam21d8",
"client_id": "wlp10bums7yMBeHST1d8",
"sub_profile": "user",
"scope": "Salary:Read:Self Salary:Read:All Salary:Write",
"act": {
"sub": "wlp10bums7yMBeHST1d8",
"sub_profile": "ai_agent",
"act": {
"sub": "0oa10bttyvxad8SuK1d8",
"sub_profile": "web_app"
}
}
}`


**Token at the salary tool**


` {
"ver": 1,
"jti": "AT.y4f_vIJ4r2B-mBX-jpKmzrOGWBA7UW3oWsKUybOyJV4",
"iss": "https://acme.okta.com/oauth2/aus10buo26zirzVB81d8",
"aud": "https://acme.example/finance-resource/authz",
"iat": 1781997649,
"exp": 1782001249,
"cid": "0oa10bu53fp7MIpxx1d8",
"uid": "00u108r403dASqam21d8",
"scp": \["Salary:Read:All", "Salary:Read:Self",
"Salary:Write"\],
"auth_time": 1781997649,
"sub": "alice@acme.example"
}`


` {
" **error** ": "access_denied",
}`


**Observation**


**Escalation.** Alice is authorized to read salaries, but never change them. Yet this token carries` Salary;Write` , allowing the agent to view anyone's pay. **Denied.** Write sits outside Alice's entitlement, so the same boundary that held for Charlie holds for her. Seniority does not widen the grant.


The pattern repeats:


- **The service app path returns a usable token** because it only checks the request and its own permissions.
- **The first-class identity path refuses all three** , because it can see the user behind the agent, and that the user could not do these things.


When a request stays within the user's entitlement, that same path issues the token, and the delegation chain rides along, so the action remains attributable.


## Addressing platform identity and availability objections


### Objection: “The agentic platform already gives the agent an identity.”


These platforms mint an identity for each agent, and a gateway can carry it. That identity is real, but it lives in the platform, not in your enterprise directory. It tells you which agent in the platform acted, but it does not carry the human's entitlement, and it does not plug into the governance you already run. You cannot certify or revoke the agent's access the way you do for the rest of your identities.


Lean on it alone, and the gateway becomes your control plane while your IdP becomes a token endpoint behind it. The agent still needs an enterprise identity bound to the user it serves, which the platform identity does not provide.


### Objection: “The service app path is available today, so why not just use it?”


This is exactly the path we tested. It returns whatever scopes the caller requests, constrained only by its own access grants and ignoring the delegating human identity's access grants. Certification and provisioning fall to custom work. You can stand it up, but you cannot bind the agent to the user's entitlement or govern it like the rest of your identities. That limit is why the agent needs a first-class identity of its own.


## The point: Give the agent an identity of its own


Every tool has a purpose. A service app is the right one for stable, service-to-service traffic, and it is not going anywhere. An AI agent is a different actor. Giving it a shared service app credential hands your most capable actor the least accountable identity in the building.


Give the agent an identity of its own, bound to the user it serves, and the tokens start answering the question you will eventually be asked: Which agent did this, and on whose authority?


### Read to solve the AI identity crisis in your own organization?


- **Go deeper on accountability:** Read[The AI Attribution Gap](https://www.okta.com/blog/ai/the-attribution-gap-ai-regulation/) to learn how to trace automated agent actions back to human owners and establish clear legal attribution.
- **Explore our solutions:** Discover how[Okta](https://www.okta.com/solutions/secure-ai/) and[Auth0](http://auth0.com/ai) secure AI agents and bring the entire agentic lifecycle under a single, unified control plane.
- **Build your roadmap:**[Talk to our team](https://www.okta.com/contact-sales/) to begin designing an identity-first AI security strategy tailored to your enterprise architecture.


About the Author


[Bala Ganaparthi Principal Forward Deployed Engineer, Okta for AI Agents Bala Ganaparthi is a Principal Forward Deployed Engineer at Okta, operating at the exact intersection of bleeding-edge innovation and real-world enterprise security. With two decades of cybersecurity experience, he turns complex identity challenges into seamless, human-centric solutions—building the invisible digital locks that keep companies open for business and bulletproof against threats. Today, Bala operates at the tip of the spear, spearheading Okta's zero-to-one initiative to secure the internet's next frontier: Agentic AI. As autonomous AI begins executing complex tasks on behalf of humans, he embeds directly with industry leaders to ensure these new systems remain safe, authorized, and strictly under control. He doesn't just help map the future of identity security; he gets deployed to the front lines to build it.](https://www.okta.com/blog/author/bala-ganaparthi/)


[Indranil Jha Senior Manager, AI Product Strategy, Solutions & Forward Deployed Engineering Results-driven technology leader with extensive expertise in Identity and Access Management (IAM), cloud security, and AI-driven solution architecture. Currently leads a team of Forward Deployed Engineering (FDE) engineers and AI Product Strategy, specializing in bridging the gap between complex customer requirements and innovative product development.](https://www.okta.com/blog/author/indranil-jha/)


[Rajesh Kumar Principal Forward Deployed Engineer, Okta for AI Agents Rajesh Kumar is a Principal Forward Deployed Engineer at Okta, driving the secure adoption of “Okta for AI.” With over 25 years of experience in Identity and Access Management (IAM), multi-cloud architectures, and enterprise security, Rajesh helps global organizations securely deploy GenAI, LLMs, and Agentic AI solutions. He is a recent graduate of MIT’s Professional Education program in AI product design and specializes in AI governance and ethical compliance.](https://www.okta.com/blog/author/rajesh-kumar/)


[Danny Fuhriman Senior Forward Deployed Engineer, Okta for AI Agents Danny is a Senior Forward Deployed Engineer at Okta focused on Agentic Identity. With five years at Okta and over a decade in identity and product–across fintech, hospitality, and now enterprise SaaS–he is now helping enterprises figure out what secure, governed AI actually looks like in practice.](https://www.okta.com/blog/author/danny-fuhriman/)


[Kundan Kolhe Sr Director, AI Product Strategy & Solutioning | Okta for AI Agents Building Okta’s agentic AI security business from zero to one, a $100M+ opportunity. Leads product strategy and solutioning for Okta’s AI Agents business, a CEO-level initiative defining how enterprises secure AI agents at scale before the market has fully settled. Leads a team of architects and partners with Fortune 500 security and IT executives across financial services, manufacturing, and technology. Shapes the agentic security product roadmap across Okta and Auth0. Brings two decades of experience across product, pre-sales, and marketing at startups and global enterprises across North America, the UK, Australia, and Asia.](https://www.okta.com/blog/author/kundan-kolhe/)
