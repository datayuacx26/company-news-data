---
schema_version: "1.0.0"
document_id: "265080cdfa911887c9db4b9449ffe2305e784bdf4497c0bb3688f6870cde4761"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-rss-12a2d78c04c7"
canonical_url: "https://www.nutanix.dev/2026/05/01/vm-console-external-access/"
published_at: "2026-05-01T14:00:00+00:00"
first_seen_at: "2026-07-20T03:31:13.386524+00:00"
fetched_at: "2026-08-20T00:46:45.571645+00:00"
content_hash: "sha256:b8a3c3654e831d76d874057a872d3d2039c4642344b8a89e9b676195da416674"
---

# Launch VM console outside Nutanix Prism UI

The Nutanix VM console has been available since the introduction of AHV. Since then, the Nutanix VM console has been primarily accessible through the Prism Central web interface. It is used as an option to debug and solve critical issues when RDP is not available. Customers have been increasingly asking for a way to open a VM console programmatically from automation pipelines, external portals, and custom tooling.


With the new **v4 VM Console API** , Nutanix now provides a secure, standards-based, token-driven mechanism for launching VM consoles outside of the Prism Central web interface.


The details on the API are available in the Nutanix API Reference under the namespace Virtual Machine Management (v4.2+)[here](https://developers.nutanix.com/api-reference?namespace=vmm&version=v4.2) .


Below, we will walk through:


- How the new v4 architecture works
- What is different from the traditional console model
- How to generate a console token
- How to launch the VM console using your own client or application


## Why a New VM Console API?


Historically, launching a VM console was not available through public APIs. Instead it required a complex workaround using Prism Central to construct a WebSocket connection to the VM’s VNC endpoint through Acropolis and it relied on a number of internal mechanisms that were not designed for this use.


The new v4 VM console design introduces the following:


- A **public, supported API** for generating a VM console token
- A **WebSocket URI** that the client can connect to directly
- A **JWT-based authorization model**
- A secure proxy path between Nutanix services


**This opens the door for automation, CI/CD access, custom dashboards, and third-party integrations.**


## Process Overview


When choosing to setup a client to launch a VM console, the process can look like this:


### Client calls the v4 “Generate Console Token” API


This is an async task API that returns:


- A **WebSocket URI**
- A **JWT console token**


### Client opens a WebSocket connection to the URI


This is a **WSS** (TLS-secured) WebSocket request.
The JWT token is passed as a query parameter.


### Nutanix infrastructure validates the token


Prism Central authenticates the request using:


- Your Prism user session cookie


Prism Central service validates the JW token:


- The JWT console token (contains VM UUID, user ID, expiration)


### Connection is proxied to the VM


The backend establishes a secure bidirectional tunnel from:


- Client → Prism Central → AHV Gateway → VM socket


Once the handshake completes, the console behaves exactly as it would in the Prism web interface.


## Step-by-Step Guide


Below is a simple workflow that can be used to launch a VM console using the new API.


**Permissions for** ***Generate_VM_Console_Token*** **are required.** This can be provided through the Prism Central RBAC (role-based access control) settings. The current system defined roles which already include this permission today include: Super Admin, Prism Admin, Project Admin, Project Manager, Developer, Consumer, Operator, and Virtual Machine Operator.


### Step 1 — Generate a VM Console Token


Make a POST request to generate the console token.


```text
POST https://<PC-IP>:9440/api/vmm/v4.0/ahv/config/vms/<VM_UUID>/$actions/generate-console-token
```


Include authentication headers (basic auth or session cookie).


**Task UUID** will be returned.


Poll the` GenerateVmConsoleToken` API from above until it completes.


### Step 2 — Retrieve Token + WebSocket URI


When the task finishes, the task completion details in response includes:


- **console_websocket_uri**
- **console_token** (JWT)


Example:


```text
{
"WsUri": "console/launch/<VM_UUID>",
"VmConsoleToken": "<jwt-token>"
}
```


### Step 3 — Connect via WebSocket


Your client should open the WebSocket URL similar to the following:


```text
wss://<PC-IP>:9440/console/launch/<VM_UUID>?VmConsoleToken=<jwt-token>
```


**You must also include your session cookie** (or bearer token) so the system can validate your identity.


Any NoVNC WebSocket-capable client will work. In addition to the NoVNC client, a Proxy client may also be required to ensure that it can establish the connection between the no VNC client and the Metropolis websocket server (WSS call).


Once connected, your tool can send/receive VNC frames exactly like Prism.


## Security Considerations


The v4 model adds multiple layers of protection:


### JWT token


Signed by a Nutanix-managed private key. Encodes:


- VM UUID
- User UUID
- Expiration (1 hour)


### User authentication


The caller must still be logged into Prism Central and have the correct permission to generate a VM console token.


### Per-VM connection limits


AHV Gateway enforces a **maximum of 32 connections per VM** .


### Short-lived authorizations


Tokens cannot be reused beyond expiration.


This design helps keep the console secure even when accessed outside Prism.


## Conclusion


We have demonstrated how the generate-console-token api can be utilized by a client to authenticate and then launch the VM Console. This unlocks the potential to include VM Console in custom cloud management tools and use this tool in your preferred location. For example a Service Provider could integrate this direction into their self-service portal for tenants.
