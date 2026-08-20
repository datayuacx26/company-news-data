---
schema_version: "1.0.0"
document_id: "747f6abdbbdfe63f4e5c113cb664db78466570442c7e52cbc159b4d0dd26f337"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/research/endginx"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-23T16:08:30.630794+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:ef32b8645a57d28dce583a65eddb3d0fef48202d580f7619da2429cb1fc0d9ba"
---

# Finding six NGINX vulnerabilities with open models

We used open models to test whether they could find new vulnerabilities in mature systems code. We selected NGINX because it is widely deployed, written in C, and has received extensive security review.


NGINX uses two-pass encoders, compact binary protocols, shared request state, and configuration-dependent control flow. These features require analysis across functions and modules. A local code fragment is often not enough to confirm a vulnerability.


We ran Winfunc with GLM-5.1 and GLM-5.2 from Z.AI. The scan produced six public NGINX findings:


- two heap overflows in HTTP/2 upstream request builders;
- one heap overflow in the rewrite engine;
- one heap overflow in the stream scripting engine;
- one HTTP/2 frame-injection bug; and
- one mTLS authorization bypass where a revoked certificate was accepted.


CVEs:[CVE-2026-28755](https://winfunc.com/hacktivity/CVE-2026-28755)[CVE-2026-42926](https://winfunc.com/hacktivity/CVE-2026-42926)[CVE-2026-9256](https://winfunc.com/hacktivity/CVE-2026-9256)[CVE-2026-42055](https://winfunc.com/hacktivity/nginx-grpc-forwarded-header-hpack-overflow) (gRPC)[CVE-2026-42055](https://winfunc.com/hacktivity/nginx-proxy-v2-forwarded-header-hpack-overflow) (proxy)[CVE-2026-42533](https://winfunc.com/hacktivity/CVE-2026-42533) .


All F5/NGINX advisories for the six vulnerabilities credit "Mufeed VH of Winfunc Research".


A note on the traces


We reconstructed the analysis below from stored traces from several NGINX scans. We rerun targets when the agent or model changes. These scans therefore used more than one model provider. The traces show how the agent inspected the code and rejected invalid hypotheses. They do not identify which model first found each CVE. This experiment used GLM-5.1 and GLM-5.2.


The source-analysis agents used read-only repository tools. They produced evidence and test plans. Separate build and reproduction environments produced the AddressSanitizer and live-service results on the Hacktivity pages.


## Why NGINX


We did not use a known-vulnerability corpus. In those corpora, the affected function is usually known before the scan starts. We wanted the agent to select relevant code, verify reachability, and produce evidence that a maintainer could review.


NGINX also has several code patterns that require cross-file analysis. A field named` len` can refer to an allocation size, a serialized size, or a 24-bit wire value. Script modules can evaluate the same expression once for sizing and again for copying. Header validation depends on configuration. HTTP, stream, gRPC, and upstream HTTP/2 use some shared helpers but have separate enforcement paths.


Each finding in this post involved a mismatch between two related operations.


## How the scan worked


The following description omits prompts, target-specific mission plans, ranking rules, and model-routing settings.


An initialization pass maps the repository. It records exposed entry points, authentication, trust boundaries, configuration surfaces, and attacker-controlled inputs. A planner uses this information to create bounded scan tasks. For NGINX, a task can focus on data that crosses a wire-format limit, expands during encoding, or passes through separate size and copy operations.


Discovery runs in two paths. A low-cost gate selects files for file-level analysis. At the same time, mission agents follow hypotheses across files and modules. The harness groups and deduplicates the resulting candidates.


The judge checks the attacker-controlled source, trigger path, failed condition, sink, prerequisites, and failure mode. A reporter then reads the relevant code again. The reporter can confirm the finding, reject it, or retain it as a non-standalone gadget. A later pass can combine confirmed findings and gadgets.


Select a stage in the diagram to see its input, output, and validation rule.


winfunc-agent / public system view


GLM-5.1


GLM-5.2


Map target: Record repository modules, entry points, configuration, and trust boundaries before source analysis.


Init


Map target


InputSource tree + scan scope


OutputRepository map + security context


Record repository modules, entry points, configuration, and trust boundaries before source analysis.


File paths are restricted to the target repository.


Stage 1 / 6


Select any stage


The first two stages decide **where to look** and **what condition to test** . The remaining stages manage concurrent work, retries, deduplication, validation, and stored evidence.


## Six findings, five CVE numbers


The gRPC and HTTP/2 proxy reports share CVE-2026-42055. They are separate findings because they affect different modules and have separate trigger paths. The view below summarizes all six findings.


Six paths / five CVE identifiers


Select a finding


CVE-2026-28755: Stream TLS did not enforce the OCSP result


CVE-2026-28755


Medium · 5.4


Stream TLS did not enforce the OCSP result


ngx_stream_ssl_module


[Full report](https://winfunc.com/hacktivity/CVE-2026-28755)


Attacker control


A revoked client certificate and its private key


Failed condition


Stream TLS did not check the stored OCSP result


Observed proof


OCSP reported revoked, but stream returned application data


Sourcengx_stream_ssl_handler()


Sinkmissing ngx_ssl_ocsp_get_status()


Fixed byNGINX 1.28.3 / 1.29.7


### 1. Stream TLS did not enforce the OCSP result


[CVE-2026-28755](https://winfunc.com/hacktivity/CVE-2026-28755)


**Medium · CVSS 3.1 5.4 ·` ngx_stream_ssl_module`**


The scan trace compared the HTTP and stream TLS paths. It searched for` ngx_ssl_ocsp_get_status` . NGINX uses this helper to retrieve the result of client-certificate OCSP validation.


The HTTP TLS path called the helper before it allowed a request to continue. The stream TLS path did not call it.


A missing call was not enough to confirm a vulnerability. Common handshake code could enforce the result. OpenSSL could reject the certificate. A later stream phase could also reject the connection.


The agent checked each case. It first read the HTTP enforcement path. It then traced the OCSP state that NGINX stores during the handshake. It inspected the shared SSL callback and searched the stream module for another status check. It also compared the mail TLS path.


The agent also followed` ngx_stream_ssl_handler()` into the stream phase engine. This confirmed that a successful return allowed access to the configured upstream application.


The stream handler checked the standard certificate verification result:


C


```text
/* Reduced to the relevant decisions. */
if   (SSL_get_verify_result(ssl) != X509_V_OK) {
reject_connection();
}


if   (client_certificate_required && no_peer_certificate(ssl)) {
reject_connection();
}


/* The stored OCSP result was not consulted here. */
continue_stream_session();


```


Certificate-chain verification and OCSP validation are separate checks. OpenSSL can accept the certificate chain while NGINX's asynchronous OCSP check reports that the leaf certificate is revoked.


The HTTP path checked both results. The stream path checked only the standard certificate verification result.


The issue required all of these conditions:


- NGINX terminated stream TLS;
- ` ssl_verify_client on` and` ssl_ocsp on` were configured; and
- the attacker had a revoked client certificate and its private key.


Under these conditions, the revoked certificate could complete stream mTLS. The connection could then reach the protected upstream until the certificate expired.


Later runtime validation confirmed the behavior. The OCSP check reported the certificate as revoked, but the stream connection still received application data.


NGINX Open Source 1.27.2 through 1.29.6 was affected. The fixes shipped in 1.28.3 and 1.29.7. F5 assigned CVSS 3.1 5.4 and credits Winfunc.


The[full Winfunc report](https://winfunc.com/hacktivity/CVE-2026-28755) contains the configuration and proof. The[F5 advisory](https://my.f5.com/manage/s/article/K000160368) contains the supported-version matrix.


### 2. HTTP/2 upstream proxy truncated an oversized body length


[CVE-2026-42926](https://winfunc.com/hacktivity/CVE-2026-42926)


**Medium · CVSS 3.1 5.8 · HTTP/2 upstream proxy**


The scan trace started at these assignments:


C


```text
header->length_0 = (u_char) (body_len >>  16  );
header->length_1 = (u_char) (body_len >>  8  );
header->length_2 = (u_char) body_len;


```


An HTTP/2 frame uses a 24-bit payload length. The maximum value is 16,777,215.


NGINX stored the generated` proxy_set_body` length in a` size_t` . It copied only the low 24 bits into the DATA-frame header. It then placed the full generated body after that header.


The agent traced the code that compiles` proxy_set_body` as a complex value. Request variables can affect the output of that value. The agent also confirmed that` proxy_http_version 2` selects the affected upstream path.


The agent compared the gRPC DATA-frame builder and found the constant for the maximum HTTP/2 frame size. It searched the proxy-v2 code for two protections: a body-length limit and a loop that divided the body into valid frames. The affected code had neither protection.


The following example shows the length mismatch:


TEXT


```text
generated body length      0x01000009  (16,777,225 bytes)
encoded 24-bit length         0x000009  (9 bytes)
bytes actually transmitted  16,777,225


```


The upstream parser reads nine bytes as the DATA payload. It then treats the next bytes as another HTTP/2 frame header. Request-derived data can control those remaining bytes. An attacker can therefore place additional frames in the NGINX-to-upstream HTTP/2 stream.


The issue changes the upstream protocol stream. It does not overwrite the NGINX worker heap.


The vulnerable configuration used` proxy_http_version 2` . It also used a` proxy_set_body` value that could exceed 16,777,215 bytes. Practical exploitation required attacker-controlled data after the wrapped DATA-frame boundary.


Later validation captured the upstream byte stream and confirmed the frame-length mismatch.


NGINX Open Source 1.29.4 through 1.30.0 was affected. Versions 1.30.1 and 1.31.0 route generated bodies through the normal bounded DATA-frame output path. The fix is commit` c24fb259` .


The[full finding](https://winfunc.com/hacktivity/CVE-2026-42926) shows the frame layout. F5 scores it 5.8 under CVSS 3.1 and 6.3 under CVSS 4.0 in its[advisory](https://my.f5.com/manage/s/article/K000161131) .


### 3. Overlapping rewrite captures caused an undersized allocation


[CVE-2026-9256](https://winfunc.com/hacktivity/CVE-2026-9256)


**High in Hacktivity · CVSS 3.1 8.1 · HTTP rewrite engine**


NGINX compiles a rewrite directive into a length program and a copy program. The length program calculates the allocation size. The copy program writes the result. Both programs must calculate the same output size.


The scan trace started at` dup_capture` . The compiler uses this flag to detect repeated references to the same capture number.


The agent followed the capture-only fast path in the rewrite compiler. In this path, the compiler could discard the exact capture-aware length bytecode. The agent then checked whether the generic size calculation could differ from the copy operation.


The agent traced the request flags that control URI escaping. These included` plus_in_uri` and` quoted_uri` . It read the escape routine and followed the request-pool allocation. It then checked two different capture numbers that refer to overlapping byte ranges.


For example:


NGINX


```text
rewrite ^/((.*))$ http://127.0.0.1:18081/$1$2 redirect;


```


` $1` and` $2` have different capture numbers. The duplicate-capture guard does not treat them as the same capture. However, both captures include bytes from the same URI region.


Some URI bytes require escaping in the replacement. The fast length path adds the URI escape expansion once. The copy path escapes` $1` and` $2` separately. It therefore escapes the overlapping bytes twice.


The size difference is:


TEXT


```text
allocated = base + escaped_length(uri)
written   = base + escaped_length(capture_1)
+ escaped_length(capture_2)


```


For overlapping captures,` written` can be greater than` allocated` .


The scan checked three possible false-positive conditions. First,` dup_capture` did not reject the nested groups because they had different indices. Second, the optimized form did not retain the slow length program. Third, request parsing preserved the flags that caused escaping during rewrite execution.


Later AddressSanitizer validation used a URI that contained many` +` characters. The copy operation wrote past a 4,096-byte request-pool allocation in` ngx_http_script_copy_capture_code()` .


The fix makes the length calculation process each capture in the same way as the copy program.[NGINX PR #1395](https://github.com/nginx/nginx/pull/1395) and commit` ca4f92a` added the fix. It shipped in versions 1.30.2 and 1.31.1. F5 assigned CVSS 3.1 8.1 and CVSS 4.0 9.2.


The complete trigger and ASan trace are in the[Winfunc report](https://winfunc.com/hacktivity/CVE-2026-9256) and affected-version details are in the[F5 advisory](https://my.f5.com/manage/s/article/K000161377) .


### 4. The gRPC request builder undercounted HPACK length fields


[CVE-2026-42055](https://winfunc.com/hacktivity/nginx-grpc-forwarded-header-hpack-overflow)


**High in Hacktivity · CVSS 3.1 8.1 ·` ngx_http_grpc_module`**


HPACK uses variable-length integers. The NGINX gRPC upstream request builder reserved four bytes for an encoded forwarded-header length. The HPACK encoder can require five bytes for a raw field above` NGX_HTTP_V2_MAX_FIELD` .


For each affected field, the allocation was one byte too small. Multiple oversized fields increased the size difference.


The scan first examined the HPACK constants and the integer writer. It then followed client-header parsing and the effect of` large_client_header_buffers` .


Default NGINX settings reject the exact oversized input. The default configuration was therefore not affected. The agent then identified a non-default configuration that retains otherwise invalid raw headers and increases the buffer limit:


NGINX


```text
ignore_invalid_headers off;
large_client_header_buffers 8 5m;


location / {
grpc_pass grpc://backend;
}


```


The buffer sizes are operator-controlled. The configuration must permit a forwarded raw header name or value that crosses the HPACK integer-width boundary.


The agent confirmed that retained headers reach` ngx_http_grpc_create_request()` . It also confirmed that` grpc_pass` selects the affected request builder. It found no outgoing` NGX_HTTP_V2_MAX_FIELD` check before allocation.


The agent then compared the allocation expression with the HPACK encoder. The difference was:


TEXT


```text
buffer budget per oversized raw length = 4 bytes
HPACK serialization requirement        = 5 bytes
deficit across N forwarded fields      = N bytes


```


The builder also inserts HTTP/2 CONTINUATION frames. The accumulated size difference can move the write past the end of the temporary request buffer.


Later AddressSanitizer validation confirmed an out-of-bounds write during construction of the upstream gRPC request.


The report classifies the demonstrated result as unauthenticated worker memory corruption under a non-default configuration. It does not claim reliable remote code execution. F5 states that code execution is possible if address-space randomization is disabled or bypassed. The validation did not demonstrate that additional condition.


The[gRPC finding page](https://winfunc.com/hacktivity/nginx-grpc-forwarded-header-hpack-overflow) contains the full configuration, field construction, and sanitizer evidence.


### 5. The HTTP/2 proxy request builder had the same HPACK size error


[CVE-2026-42055](https://winfunc.com/hacktivity/nginx-proxy-v2-forwarded-header-hpack-overflow)


**High in Hacktivity · CVSS 3.1 8.1 · HTTP/2 upstream proxy**


A separate request builder reserved the same four-byte allowance before it called the variable-length HPACK encoder.


The scan searched the HTTP/2 upstream proxy implementation for the HPACK constants and write helpers. It confirmed that` proxy_http_version 2` selects this implementation.


The agent then traced how NGINX retains and forwards HTTP/1.x client headers. It repeated the large-header configuration checks from the gRPC finding. It also followed the choice between raw and Huffman encoding. This confirmed that the five-byte raw encoding path was reachable.


The affected function was` ngx_http_proxy_v2_create_request()` .


The confirmed write occurred in the` ngx_memmove()` loop that inserts CONTINUATION-frame headers. The temporary buffer was already short by one byte for each over-limit field. The move operation then wrote serialized data past the allocation.


Later validation confirmed the out-of-bounds write in the HTTP/2 proxy request builder.


This path has a separate finding because users can enable either upstream mode independently. The gRPC and proxy implementations use different request builders. They also reach different write operations.


Both implementations had the same size-calculation error. The fix rejects forwarded field names and values above the supported HTTP/2 field limit before buffer allocation.


Both paths were fixed by[NGINX PR #1474](https://github.com/nginx/nginx/pull/1474) and commit` 26d824e` . The gRPC path dates back to 1.13.10. The HTTP/2 upstream proxy path only exists in the newer 1.29.x line.


The mainline, stable, and NGINX Plus ranges differ. Open Source fixes shipped in 1.30.3 and 1.31.2. F5 scores CVE-2026-42055 at 8.1 under CVSS 3.1 and 9.2 under CVSS 4.0.


The[proxy-v2 finding](https://winfunc.com/hacktivity/nginx-proxy-v2-forwarded-header-hpack-overflow) and[gRPC finding](https://winfunc.com/hacktivity/nginx-grpc-forwarded-header-hpack-overflow) contain their separate proofs. F5's[CVE-2026-42055 advisory](https://my.f5.com/manage/s/article/K000161584) covers both paths.


### 6. Stream capture state changed between the length and copy passes


[CVE-2026-42533](https://winfunc.com/hacktivity/CVE-2026-42533)


**High in Hacktivity · CVSS 3.1 8.1 · stream scripting engine**


NGINX stream complex values can use a length program and a copy program. The length program calculates the allocation size. The copy program writes the value.


Both programs read regular-expression captures from the stream session. The capture state must remain unchanged between the two passes.


The scan searched for the mutable session fields` captures` ,` ncaptures` , and` captures_data` . It then found each stream regular-expression operation that could update those fields.


The agent followed complex-value evaluation through the stream map module and the return module. It also checked variable expansion, preread processing, and TLS SNI extraction.


One affected expression is:


NGINX


```text
return "$1$m";


```


` $m` is a variable backed by a regular-expression map over attacker-controlled SNI.


At the start of the length pass,` $1` has no capture value. It contributes zero bytes. Evaluation of` $m` then runs the map regular expression. This operation updates the shared capture state on the stream session.


The copy pass evaluates the expression again. It now sees the capture that the map created and copies it.


TEXT


```text
length pass:
len($1) = 0
eval($m) -> regex matches SNI, session captures change
allocation = 1 byte


copy pass:
copy($1) -> capture now references attacker SNI
copy 12,000 bytes into the one-byte allocation


```


The scan checked the complete evaluation order. It confirmed that the map lookup updates the shared capture state. It confirmed that attacker-controlled SNI supplies the capture data. It also followed the pool allocation and compared the HTTP script implementation for the same unbounded capture-copy operation.


Later AddressSanitizer validation confirmed the result. NGINX copied 12,000 bytes into a one-byte allocation in` ngx_stream_script_copy_capture_code()` .


NGINX fixed the issue with centralized bounds checks. The copy operation now stops if a capture does not fit in the allocated buffer. Mainline changes landed through[PR #1561](https://github.com/nginx/nginx/pull/1561) , with stable backports in[PR #1563](https://github.com/nginx/nginx/pull/1563) .


NGINX Open Source 0.9.6 through 1.31.2 was affected. The fixes are in 1.30.4 and 1.31.3.


F5 scores the issue 8.1 under CVSS 3.1 and 9.2 under CVSS 4.0. That CVSS 4.0 score maps to Critical in F5's advisory; Hacktivity keeps the finding labeled High. The[full Winfunc analysis](https://winfunc.com/hacktivity/CVE-2026-42533) includes the reproducer and sanitizer output, while the[F5 advisory](https://my.f5.com/manage/s/article/K000162097) contains the product matrix.


## How the agent evaluated each lead


The agent did not promote an initial code match directly into a finding. It checked the condition that was most likely to invalidate each lead.


Finding Initial lead What had to be ruled out


Stream OCSP HTTP consumed an OCSP result that stream did not common-handshake enforcement, OpenSSL failure, later stream rejection


Frame injection body length was wider than the 24-bit frame field frame splitting, generated-body bounds, unreachable HTTP/2 upstream path


Rewrite overflow capture fast path discarded exact sizing duplicate-capture guard, URI normalization, slow length bytecode


gRPC HPACK the builder reserved four bytes for a variable-length encoding default input limits, header rejection, outgoing max-field enforcement


Proxy HPACK the proxy builder used the same four-byte budget feature dispatch, Huffman selection, forwarded-header normalization


Stream captures two passes read mutable regex state evaluation order, session ownership, attacker-controlled SNI reachability


In each trace, the agent formed a hypothesis. It then identified a condition that could disprove the hypothesis and used read-only tools to check that condition.


The reports did not rely on the reasoning traces alone. Each report identified the source, reachable configuration, failed condition, sink, proof, and false-positive checks. The six reports resulted in upstream fixes.


## The harness doesn't really matter


The harness is necessary for scan operations. It limits repository access, coordinates concurrent work, applies backpressure, retries failed jobs, removes duplicate candidates, and stores an audit trail. It also separates an initial hypothesis from a confirmed report.


The harness now contributes less to the vulnerability reasoning than it did in earlier versions. Earlier versions gave the model detailed steps for source identification, sink analysis, reachability, vulnerability classes, and review. Current models can perform more of this work without step-by-step prompts.


Raw candidates still include false positives. The judge and reporter must verify each candidate before it becomes a report. After these checks, the false-positive rate is low enough for a researcher to review the reports directly instead of sorting the raw candidates.


The NGINX findings required code-specific analysis. The OCSP issue required a comparison of separate authorization paths. The stream scripting issue required tracking capture state across two evaluations. The rewrite issue required recognizing that two capture numbers could refer to overlapping bytes.


Scan planning is necessary because a large repository contains more hypotheses than one scan can test. The threat model identifies attacker-controlled inputs, trust boundaries, relevant configurations, encoding steps, duplicated security checks, and size conversions.


Before code analysis, Winfunc allocates compute to two tasks:


1. **Select code paths.** Rank reachable modules, protocol boundaries, security decisions, stateful interpreters, and high-risk code paths.
2. **Generate hypotheses.** Use deployment details, attacker control, trust boundaries, and required code conditions to create target-specific hypotheses.


This planning requires domain knowledge about the target. For NGINX, configuration is part of reachability. A request can be encoded again for a different upstream protocol. Several script engines use separate length and copy programs. TLS verification data is split between OpenSSL state and NGINX state.


Winfunc is reducing the amount of model scaffolding in the harness. Development now focuses on the target model, the threat model, and the evidence checks.


## A shared failure pattern


All six findings involved two operations that were expected to use the same value or state:


- OCSP produced a revocation result; the stream authorization path consumed only the ordinary verification result.
- ` proxy_set_body` produced a` size_t` ; the wire header consumed only 24 bits.
- Rewrite sizing reasoned about a URI; the writer consumed overlapping captures.
- The HPACK builders reserved a fixed allowance; the encoder consumed a variable-width integer.
- Stream sizing read one capture state; copying consumed a later capture state.


This pattern provides specific scan hypotheses. The scan can compare count and copy operations, validation and authorization paths, parsers and forwarders, or in-memory and wire-format sizes. It can then check whether mutable state, configuration, encoding, or integer width changes the value between those operations.


## What Winfunc has found beyond NGINX


At the time of publication,[Winfunc Hacktivity](https://winfunc.com/hacktivity) contains 28 public findings across 12 projects, with 19 distinct CVE identifiers. The public set includes 4 Critical, 11 High, and 13 Medium findings.


We were behind the recent CVEs in React and Node.js (one of each). Three additional Chromium CVEs are visible there in redacted form and are not included in those public-detail totals until the original reports are publicly disclosed by the Chromium team.


Hacktivity also includes these projects and findings:


Project Representative Winfunc finding Severity / status


NGINX The six findings in this post, plus DAV path overlap and SCGI truncation High / Medium


Chromium V8 type confusion (` CVE-2026-10910` ), ANGLE uninitialized use (` CVE-2026-10994` ), and ANGLE integer overflow (` CVE-2026-10019` ) High / Medium · reports redacted


seroval[Promise resolver type confusion during deserialization](https://winfunc.com/hacktivity/CVE-2026-59940) Critical


React[RSC $K FormData amplification](https://winfunc.com/hacktivity/CVE-2026-23864) High


Node.js[Permission Model bypass through Unix-domain sockets](https://winfunc.com/hacktivity/CVE-2026-21636) Medium


Anthropic[FastMCP custom routes skip authentication](https://winfunc.com/hacktivity/anthropic-fastmcp-auth-bypass) Critical


Supabase[Queue-name SQL injection](https://winfunc.com/hacktivity/supabase-sql-injection-via-queue-names) Critical


Bun[Exponential YAML merge-key denial of service](https://winfunc.com/hacktivity/bun-yaml-dos) High


Gumroad[Zero-click account takeover through an authorization bypass](https://winfunc.com/hacktivity/gumroad-helper-auth-bypass-ato) Critical


Mattermost[Oversized-password login denial of service](https://winfunc.com/hacktivity/CVE-2026-24458) , among ten public findings High


Better-Auth[Forged multi-session cookie revocation](https://winfunc.com/hacktivity/better-auth-multi-session-signout-ato) Medium


Actix[CL.TE request smuggling](https://winfunc.com/hacktivity/GHSA-xhj4-vrgc-hr34) Medium


Hoppscotch[CLI sandbox escape](https://winfunc.com/hacktivity/CVE-2024-34347) High


Winfunc surfaced each public entry autonomously. The team then reproduced the issue, reviewed the report, and coordinated disclosure with the maintainer.


We also have undisclosed 0-days under coordinated disclosure. We will add them to Hacktivity after fixes are available.


---


And readers, please challenge us to find 0-days in other hardened targets (literally any target we could read the code of). We'll pick them one-by-one, responsibly disclose them, and with their permission, write a detailed report on how we (as in, Winfunc) discovered them.
