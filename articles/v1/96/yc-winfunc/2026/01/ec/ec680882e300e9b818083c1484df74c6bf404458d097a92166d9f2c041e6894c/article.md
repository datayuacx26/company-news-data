---
schema_version: "1.0.0"
document_id: "ec680882e300e9b818083c1484df74c6bf404458d097a92166d9f2c041e6894c"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/GHSA-xhj4-vrgc-hr34"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:63cd9a496d11a3134ce6e5b86b22186f3ac4517a972dc5ac37bab77ddf4549e3"
---

# HTTP/1.1 CL.TE request smuggling in actix-http (GHSA-xhj4-vrgc-hr34)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Actix


Medium


2026


# HTTP/1.1 CL.TE request smuggling in actix-http (GHSA-xhj4-vrgc-hr34)


## Summary


actix-http accepted conflicting Content-Length and Transfer-Encoding: chunked request framing


The HTTP/1.1 request parser in` actix-http` accepted requests containing both` Content-Length` and` Transfer-Encoding: chunked` . During header conversion,` MessageType::set_headers` recorded the content length and also marked the message as chunked. The payload selection logic then preferred the chunked decoder whenever` chunked` was true, instead of rejecting the conflicting framing.


In a CL.TE deployment, an upstream proxy can frame the request using` Content-Length` while forwarding the ambiguous request to an Actix backend that frames it as chunked. When the backend stops at the terminating chunk marker on a reused HTTP/1.1 connection, trailing bytes can be parsed as a second backend request. The fix, released in[actix-http 3.12.1](https://github.com/actix/actix-web/releases/tag/http-v3.12.1) , rejects HTTP/1 requests with` Transfer-Encoding` when HTTP/1.0 is used, when the transfer encoding is not chunked, or when` Content-Length` is also present. The GitHub advisory is[GHSA-xhj4-vrgc-hr34](https://github.com/advisories/GHSA-xhj4-vrgc-hr34) ; the security change is[commit 3c056bd36128d41ba130a8704ef5d648de3f2870](https://github.com/actix/actix-web/commit/3c056bd36128d41ba130a8704ef5d648de3f2870) , and the release prep is[commit 0fb89457eda4a78a4cb7ccb3fdebe49a143ce2d5](https://github.com/actix/actix-web/commit/0fb89457eda4a78a4cb7ccb3fdebe49a143ce2d5) .


### CVSS Score


Vector


N


Complexity


L


AT


P


Privileges


N


User Interaction


N


VC


N


VI


L


VA


N


SC


N


SI


N


SA


N


CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:N/VC:N/VI:L/VA:N/SC:N/SI:N/SA:N


### Vulnerability Location


Source


Line 75


actix-http /src


/h1


/decoder.rs


MessageType::set_headers


Sink


Line 197


actix-http /src


/h1


/decoder.rs


PayloadDecoder::chunked selection


## Source-to-Sink Analysis


1


actix-http/src/h1/decoder.rs:75-150


` set_headers` consumed the raw httparse header indexes and accepted both framing headers:` Content-Length` populated` content_length` , while` Transfer-Encoding: chunked` set` chunked = true` .


RUST


```text
let    mut   chunked   =  false  ;
let    mut   content_length   =  None  ;


match   name {
header::CONTENT_LENGTH => {
content_length =  Some  (len);
}
header::TRANSFER_ENCODING  if   version == Version::HTTP_11 => {
if   val. eq_ignore_ascii_case  ( "chunked"  ) {
chunked =  true  ;
}
}
_ => {}
}


```


2


actix-http/src/h1/decoder.rs:197-209 (before fix)


After all headers were processed, the vulnerable parser selected chunked decoding whenever` chunked` was true. It did not treat the simultaneously present` Content-Length` as an error.


RUST


```text
if   chunked {
Ok  (PayloadLength:: Payload  (PayloadType:: Payload  (
PayloadDecoder:: chunked  (),
)))
}  else    if    let    Some  (len) = content_length {
Ok  (PayloadLength:: Payload  (PayloadType:: Payload  (
PayloadDecoder:: length  (len),
)))
}


```


3


actix-http/src/h1/decoder.rs:275-293 (fix)


The patched decoder validates transfer-encoding semantics after headers are installed and before payload decoding is chosen. If both` Transfer-Encoding` and` Content-Length` are present, parsing fails with` ParseError::Header` .


RUST


```text
if   msg. head  ().headers. contains_key  (header::TRANSFER_ENCODING) {
if   ver == Version::HTTP_10 {
return    Err  (ParseError::Header);
}


if   !crate::HttpMessage:: chunked  (&msg)? {
return    Err  (ParseError::Header);
}


if   msg. head  ().headers. contains_key  (header::CONTENT_LENGTH) {
return    Err  (ParseError::Header);
}
}


```


4


actix-http/src/h1/codec.rs:241-252


The regression test sends a CL.TE request followed by a second request on the same buffer and asserts that the first decode fails rather than leaving the second request to be parsed on the backend connection.


RUST


```text
let    mut   buf   = BytesMut:: from  (
"POST /test HTTP/1.1\r\n\
content-length: 11\r\n\
transfer-encoding: chunked\r\n\r\n\
0\r\n\r\n\
GET /test2 HTTP/1.1\r\n\r\n"  ,
);


assert!  (codec. decode  (& mut   buf). is_err  ());


```


## Impact Analysis


### Critical Impact


The bug creates a backend request desynchronization primitive. In affected proxy deployments, an attacker can influence the integrity of requests processed by the Actix service behind the intermediary. The advisory scores no direct vulnerable-system confidentiality or availability impact, but the practical effect depends on how the frontend and backend route, authenticate, and reuse the desynchronized connection.


### Attack Surface


Actix services using vulnerable` actix-http` versions behind an HTTP/1.1 proxy, load balancer, WAF, or other intermediary that can disagree with the backend about whether` Content-Length` or` Transfer-Encoding: chunked` defines the request body.


### Preconditions


The attacker needs network access to the frontend. Exploitation requires a CL.TE topology: the frontend must honor` Content-Length` , forward the conflicting` Transfer-Encoding: chunked` header, and reuse a backend HTTP/1.1 connection to the Actix service.


## Proof of Concept


### Environment Setup


Use` actix-http` 3.12.0 or earlier behind an HTTP/1.1 intermediary that forwards conflicting` Content-Length` plus` Transfer-Encoding: chunked` requests to the Actix backend over a reused connection.


### Target Configuration


The vulnerable parser is specific to HTTP/1.x request parsing. HTTP/2 framing is not part of this issue.


### Exploit Delivery


Send an ambiguous request shaped like:


HTTP


```text
POST /test HTTP/1.1
Host: victim
Content-Length: 11
Transfer-Encoding: chunked


0


GET /test2 HTTP/1.1
Host: victim


```


The exact` Content-Length` value and smuggled request layout depend on the frontend parser behavior.


### Outcome


In a vulnerable CL.TE proxy topology, the trailing bytes can be interpreted as a second backend request on the reused Actix connection.


**Expected Response:** Affected versions accept the first message and choose chunked decoding. Fixed versions return a parse error before a payload decoder is selected.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/actix/actix-web/commit/3c056bd36128d41ba130a8704ef5d648de3f2870)
