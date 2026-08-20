---
schema_version: "1.0.0"
document_id: "2e5220a243843f63004ca5d950baa99a7ad9361835bacbc8a545a9af87784d81"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2024-34347"
published_at: "2024-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:ad3dc92e281328db0c6bb610566824d3ac0da520a49c57487ef4d7e181b804db"
---

# Hoppscotch CLI sandbox escape through Node vm pre-request scripts (CVE-2024-34347)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Hoppscotch


High


CVE-2024-34347


2024


# Hoppscotch CLI sandbox escape through Node vm pre-request scripts (CVE-2024-34347)


## Summary


Hoppscotch CLI ran collection scripts in Node's vm while exposing host-created objects


Hoppscotch CLI executes collection-supplied pre-request and test scripts while running` hopp test` . Unlike the web and desktop clients, the CLI could not rely on browser Web Workers and used the Node.js` vm` module through` @hoppscotch/js-sandbox` . The sandbox created a` vm` context, then injected host-created objects such as` pw` ,` atob` , and` btoa` into that context before calling` runInContext` on attacker-controlled collection script text.


Node's` vm` module is not a security boundary for untrusted JavaScript, especially when code inside the context can reach references to objects created outside the context. A malicious collection script could use the exposed` pw` object to recover the host global object and load` child_process` , gaining command execution on the machine running Hoppscotch CLI. The GitHub advisory is[GHSA-qmmm-73r2-f8xr / CVE-2024-34347](https://github.com/advisories/GHSA-qmmm-73r2-f8xr) . The fix, merged in[PR #3973](https://github.com/hoppscotch/hoppscotch/pull/3973) as[commit 22c6eabd133195d22874250a5ae40cb26b851b01](https://github.com/hoppscotch/hoppscotch/commit/22c6eabd133195d22874250a5ae40cb26b851b01) , removed the Node` vm` implementation and replaced it with an` isolated-vm` based Node sandbox. The patched npm release for` @hoppscotch/cli` is 0.8.0.


### CVSS Score


Vector


N


Complexity


H


Privileges


N


User Interaction


R


Scope


C


Confidentiality


H


Integrity


H


Availability


H


CVSS:3.1/AV:N/AC:H/PR:N/UI:R/S:C/C:H/I:H/A:H


### Vulnerability Location


Source


Line 14


packages /hoppscotch-cli


/src


/commands


/test.ts


test


Sink


Line 31


packages /hoppscotch-js-sandbox


/src


/pre-request


/node-vm


/index.ts


runInContext


## Source-to-Sink Analysis


1


packages/hoppscotch-cli/src/commands/test.ts:14-20


The CLI` test` command parses an attacker-supplied collection file and sends its requests through the collection runner.


TYPESCRIPT


```text
export    const    test   = ( path  :  string  ,  options  :  TestCmdOptions   ) =>  async   () => {
const   envs = options. env   ?  await    parseEnvsData  (options. env  ) : < HoppEnvs  >{  global  : [],  selected  : [] }
const   collections =  await    parseCollectionData  (path)


const   report =  await    collectionsRunner  ({ collections, envs, delay })
}


```


2


packages/hoppscotch-cli/src/utils/pre-request.ts:36-47


For every request, the CLI passed the collection-controlled` preRequestScript` string into` runPreRequestScript` from` @hoppscotch/js-sandbox/node` .


TYPESCRIPT


```text
export    const    preRequestScriptRunner   = ( request  :  HoppRESTRequest  ,  envs  :  HoppEnvs   ) =>
pipe  (
TE  . of  (request),
TE  . chain  ( ( { preRequestScript }  ) =>
runPreRequestScript  (preRequestScript, envs)
)
)


```


3


packages/hoppscotch-js-sandbox/src/pre-request/node-vm/index.ts:13-31 (before fix)


The vulnerable Node implementation created a` vm` context, injected host-created` pw` ,` atob` , and` btoa` references, then executed the untrusted script text in that context.


TYPESCRIPT


```text
return    createContext  ()


const   { pw, updatedEnvs } =  getPreRequestScriptMethods  (envs)


context. pw   = pw
context. atob   = atob
context. btoa   = btoa


runInContext  (preRequestScript, context)


```


4


packages/hoppscotch-js-sandbox/src/test-runner/node-vm/index.ts:42-50 (before fix)


Test scripts had the same trust-boundary break: the host-created` pw` object plus response data were exposed directly to untrusted` testScript` code before` runInContext` .


TYPESCRIPT


```text
const   { pw, testRunStack, updatedEnvs } =  getTestRunnerScriptMethods  (envs)


context. pw   = { ...pw,  response  : responseObjHandle. right   }
context. atob   = atob
context. btoa   = btoa


runInContext  (testScript, context)


```


5


packages/hoppscotch-js-sandbox/src/node/pre-request.ts:20-70 (fix)


The patched implementation creates an` isolated-vm` isolate and context, copies serialized API methods into the isolate, builds an in-isolate proxy, and runs the script inside that isolate.


TYPESCRIPT


```text
const    isolate  : ivmT. Isolate   =  new   ivm. Isolate  ()
const   context =  await   isolate. createContext  ()
const   jail = context. global


const   { pw, updatedEnvs } =  getPreRequestScriptMethods  (envs)
const   serializedAPIMethods =  getSerializedAPIMethods  (pw)
jail. setSync  ( "serializedAPIMethods"  , serializedAPIMethods, {  copy  :  true   })


const   finalScript =  `
const pw = new Proxy(serializedAPIMethods, {
get: (pwObjTarget, pwObjProp) => {
const topLevelEntry = pwObjTarget[pwObjProp]


// "pw.env" set of API methods
if (topLevelEntry && typeof topLevelEntry === "object") {
return new Proxy(topLevelEntry, {
get: (subTarget, subProp) => {
const subLevelProperty = subTarget[subProp]
if (subLevelProperty && subLevelProperty.typeof === "function") {
return (...args) => subLevelProperty.applySync(null, args)
}
},
})
}
}
})


${preRequestScript}
`


const   script =  await   isolate. compileScript  (finalScript)
await   script. run  (context)


```


6


packages/hoppscotch-js-sandbox/src/node/utils.ts:6-23


Host API functions are converted to` isolated-vm` references recursively instead of passing a raw host object graph into a Node` vm` context.


TYPESCRIPT


```text
export    const   getSerializedAPIMethods = (
namespaceObj  :  Record  < string  ,  unknown  >
):  Record  < string  ,  unknown  > => {
const    result  :  Record  < string  ,  unknown  > = {}


for   ( const   [key, value]  of    Object  . entries  (namespaceObj)) {
if   ( typeof   value ===  "object"   && value !==  null   && ! Array  . isArray  (value)) {
result[key] =  getSerializedAPIMethods  (value  as    Record  < string  ,  unknown  >)
}  else    if   ( typeof   value ===  "function"  ) {
result[key] =  new   ivm. Reference  (value)
}  else   {
result[key] = value
}
}


return   result
}


```


## Impact Analysis


### Critical Impact


Successful exploitation gives arbitrary command execution on the victim's machine. The attacker can read or modify local files, exfiltrate secrets available to the CLI process, run additional payloads, and pivot through any credentials or network access available to that user.


### Attack Surface


Hoppscotch CLI users who run` hopp test` on untrusted or attacker-supplied Hoppscotch collection exports containing pre-request or test scripts.


### Preconditions


The attacker must convince a victim to download or otherwise use a malicious Hoppscotch collection and run it with an affected` @hoppscotch/cli` version from 0.5.0 before 0.8.0. Hoppscotch Web and Desktop are not affected by this Node-specific sandbox path.


## Proof of Concept


### Environment Setup


Install an affected` @hoppscotch/cli` release before 0.8.0, then prepare a Hoppscotch collection containing a request with a pre-request script.


### Target Configuration


The vulnerable path is the Node CLI sandbox. The web and desktop clients use the browser worker sandbox and are not affected by this specific Node` vm` escape.


### Exploit Delivery


Place this script in the collection's pre-request script field:


JS


```text
outside = pw. constructor  . constructor  ( 'return this'   )(  )
outside. process  . mainModule  . require  ( 'child_process'  ). execSync  ( 'id > /tmp/pwnd'  )


```


Then have the victim run the collection with Hoppscotch CLI.


### Outcome


A collection-supplied script escapes the intended sandbox and executes operating-system commands as the CLI user.


**Expected Response:** On vulnerable CLI versions, the command executes and writes the result of` id` to` /tmp/pwnd` . On fixed versions, the Node` vm` implementation is gone and the script runs in the isolated-vm based implementation.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/hoppscotch/hoppscotch/pull/3973)
