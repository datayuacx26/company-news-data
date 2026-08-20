---
schema_version: "1.0.0"
document_id: "411e35bfaedeffb80c638ea2d1696717e372b83ce16b9ff66f17ee34b6d68fb4"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/ai-agents-aws-agentcore/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-22T15:57:37.609410+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:a8b040ea1cea170101ecb3a0a20d2c08cf8527d37b5a298eb75fc2f12a21d232"
---

# When AI Agents Call AWS, Who Does AWS Think They Are?

In Part 1,[Your AI Agent Needs to Know Who You Are](https://goteleport.com/blog/your-ai-agent-needs-to-know-who-you-are/) , we showed how Teleport JWTs give MCP tools a verified identity for every request. This post extends that pattern to AWS, specifically to Amazon Bedrock AgentCore, where the same identity gap exists but requires a different solution stack.


You ask an AI agent to list your S3 buckets. The agent calls an MCP tool. The tool reaches out to AWS. However, CloudTrail records the action under something like` agentcore-bot` , but not your identity. A generic service account tells you almost nothing.


This is the default when you build on[Amazon Bedrock AgentCore](https://docs.aws.amazon.com/bedrock-agentcore/) without identity propagation, which makes the audit trail almost useless for accountability. And when someone leaves, their agent-powered access stays behind.


The fix is to carry caller identity through the entire chain from login, through the agent, through AgentCore, into the Lambda, and into the AWS API call. Here’s how it works.


## Why AgentCore is the right chokepoint


AgentCore is a managed MCP gateway, which means every tool call passes through it before reaching a Lambda. This makes it the natural place to enforce identity. By default, the gateway validates that requests are structurally correct, but it doesn’t propagate caller identity into the tools. This means that Lambda knows it was invoked by AgentCore, but it doesn’t know which human was behind the request.


This gap has three consequences:


- **Shared permissions:** All users run as the same IAM role. You either over-provision it or break power users.
- **Anonymous audit trail:** CloudTrail shows the Lambda execution role but not the person who triggered the action.
- **Stale access:** IAM roles on Lambda functions aren’t tied to individuals. When Alice leaves, the tools she could invoke keep running for anyone using the same agent.


## Teleport as the identity source for AgentCore


Teleport addresses these gaps in AgentCore. When a user authenticates and connects to the MCP gateway, Teleport issues a short-lived signed JWT with their verified identity. For example:


```text
tsh login your-cluster.teleport.sh
tsh mcp connect <mcp-server>
sub:      [email protected]
roles:   [ "mcp-user"  ,  "order-admin"  ]
aud:     mcp+https://<mcp-server>/mcp
exp:     < 1   hour  from   now>


```


Teleport then[attaches this JWT to every outbound agent request](https://goteleport.com/docs/enroll-resources/mcp-access/integration-guides/aws-bedrock-gateway/) as` Authorization: Bearer <token>` . This is signed by Teleport’s private key and verifiable against Teleport's JWKS URL, so no downstream component needs to trust the caller’s word about who they are.


One config change in` teleport.yaml` wires this up:


```text
app_service:
enabled:    true
apps:
-    name:    identity-aware-mcp
uri:    "mcp+http://localhost:9999/mcp"
rewrite:
headers:
-    "Authorization: Bearer  {{internal.id_token}}  "


```


In this example,` {{internal.id_token}}` resolves to the signed JWT for the authenticated user at request time. Without this,` tsh mcp connect` reaches the server but no identity context flows through.


## The AWS stack


### AgentCore: Validation at the edge


Configure[AgentCore with a JWT authorizer](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/inbound-jwt-authorizer.html) pointing to Teleport's[OIDC discovery URL](https://goteleport.com/docs/enroll-resources/mcp-access/jwt/) . On every incoming tool call, the gateway fetches Teleport’s current public signing keys and validates the JWT before routing anything to a Lambda. For example:


```text
authorizerType= 'CUSTOM_JW  T ',
authorizerConfiguration={
'  customJWTAuthorizer ': {
'  discoveryUrl ': '  https: //<cluster>/.well-known/openid-configuration',
'allowedAudienc  e ': [mcp_audience],
'  customClaims ': [{
'  inboundTokenClaimName ': '  roles ',
'  claimMatchValue ': {'  matchValueString ': '  mcp-user '},
'  claimMatchOperator ': '  CONTAINS '
}]
}
}


```


Expired token, wrong audience, forged signature, or missing required role causes the call to be rejected, and no Lambda runs. However, the gateway validates the JWT and then drops it, so the Lambda still receives no identity context. This is what the interceptor solves.


### REQUEST interceptor: Bridging the gap


AgentCore supports[interceptor Lambdas that run before tool invocations](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-interceptors.html) . Configure one as a REQUEST interceptor with` passRequestHeaders: true` and AgentCore forwards the original` Authorization header` to it. Because the gateway already validated the JWT, the interceptor decodes the payload and injects the identity claims into the tool call arguments, like so:


```text
def  lambda_handler  ( event  , context  ):
auth_header   =  event  . get  ( 'requestHeaders'  , {}). get  ( 'Authorization'  ,  ''  )
token = auth_header.replace( 'Bearer '  ,  ''  )
payload = jwt.decode(token, options={ "verify_signature"  : False})   # gateway verified


event  [ 'requestParameters'  ][ 'arguments'  ][ '_teleport_user'  ]  = payload. get  ( 'sub'  ,  'unknown'  )
event  [ 'requestParameters'  ][ 'arguments'  ][ '_teleport_roles'  ] = json.dumps(payload. get  ( 'roles'  , []))
return    event


```


### Amazon Verified Permissions: Per-tool authorization


[Amazon Verified Permissions](https://docs.aws.amazon.com/verifiedpermissions/latest/userguide/what-is-avp.html) evaluates Cedar policies. The interceptor calls` avp.is_authorized()` with the caller's Teleport identity and the requested tool name. Cedar policies map Teleport roles to tools:


```text
// Any mcp-user can read orders
permit   (
principal in  TeleportRole  :: "mcp-user"  ,
action ==  Action  :: "invoke_tool"  ,
resource ==  Tool  :: "get_order_tool"
);


// Only order-admin can modify orders
permit   (
principal in  TeleportRole  :: "order-admin"  ,
action ==  Action  :: "invoke_tool"  ,
resource ==  Tool  :: "update_order_tool"
);


```


On` deny` , the interceptor returns an error and the tool Lambda never runs. Policy changes take effect on the next request, with no Lambda redeployment or gateway restart. These are the same Teleport roles already controlling SSH, database, and Kubernetes access, with no second RBAC system to maintain.


### Tool Lambdas: Identity as parameters


Teleport ensures tools receive identity as clean, already-verified function parameters and without requiring auth code. For example:


```text
def get_order_tool(orderId, *,  _teleport_user  = 'unknown'  , _teleport_roles= '[]'  , **kwargs):
roles   = json.loads(_teleport_roles)
if 'order-admin' in roles:
return query_all_orders()
return query_orders( owner  =_teleport_user)   # scoped to caller


```


## What this looks like in practice


Consider two examples of what this workflow looks like in practice. Both Alice and Bob are attempting to update an invoice using an agent. However, Alice’s assigned role is not scoped to this action, while Bob’s role is.


Alice (` role: mcp-user` ) asks her agent to update order 42. AgentCore validates her JWT, and the interceptor asks AVP: “Is[\[email protected\]](https://goteleport.com/cdn-cgi/l/email-protection#6c0d00050f092c0f03011c0d0215420f0301) allowed to invoke update_order_tool?” However, there’s no Cedar policy granting` mcp-user` that access, so AVP returns` deny` . Alice’s agent gets a 403, and the Lambda never runs.


Bob (` role: order-admin` ) makes the same agent call. AVP finds the matching policy, returns allow, the interceptor injects Bob’s identity, and the tool runs scoped to Bob. CloudTrail records Bob’s identity.


There are three audit sources in this workflow:


1. Teleport session logs (who connected and when)
2. AVP authorization logs (every decision with the matching policy ID)
3. CloudTrail (the AWS API call attributed to the user)


Identity is recorded across these sources, making correlation and incident investigations straightforward.


## What it takes to set this up


If you’re already on Teleport, this workflow is very easy to configure. JWT issuance is built into` tsh mcp connect` . The AgentCore OIDC authorizer is a configuration change. The interceptor Lambda is around 30 lines. Cedar policies are readable by anyone who has written an access control rule.


The notebooks in our[aws-agentcore GitHub repo](https://github.com/gravitational/rev-tech/tree/main/use-cases/ai/aws-agentcore) are the fastest way to see it working. Each notebook leaves the infrastructure from the previous one in place, so the final state is the complete working system. The whole sequence takes under an hour if you have a Teleport cluster and an AWS account ready.


Learn how to[Connect an Amazon Bedrock AgentCore Gateway to Teleport.](https://goteleport.com/docs/enroll-resources/mcp-access/integration-guides/aws-bedrock-gateway/)


---


## Jeffrey Ellin


Jeffrey Ellin is a cloud-native technologist with 10+ years of hands-on experience in Kubernetes architecture, enterprise-scale containerized deployments, and AI-powered applications. Jeffrey actively builds and experiments with agentic AI systems, including Model Context Protocol (MCP) integrations, OAuth workflows, and machine identity with SPIFFE to bridge the gap between emerging technology and real-world, production-ready solutions.
