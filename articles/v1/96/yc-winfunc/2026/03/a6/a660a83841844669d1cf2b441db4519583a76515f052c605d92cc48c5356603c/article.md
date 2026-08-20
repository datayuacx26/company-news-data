---
schema_version: "1.0.0"
document_id: "a660a83841844669d1cf2b441db4519583a76515f052c605d92cc48c5356603c"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/research/what-an-automated-vulnerability-research-system-actually-found"
published_at: "2026-03-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:00:16.885857+00:00"
content_hash: "sha256:c2cb211e68d3f5cdcf638a900a444566b222f615fba9bd7a4c142c3f8a7a5e01"
---

# What an automated vulnerability research system actually found

I'll start with the evidence: thirteen patched bugs.


Over the past few months we've been running an automated vulnerability research system against open source software. A system that reads code, forms attack ideas, writes tests and harnesses, runs them, and throws most of them away.


So far it has produced thirteen confirmed issues across nine projects. Seven CVEs. Three critical. Three high. Six medium. One low. All patched or fixed upstream.


The useful claim here is simpler: once a model has to survive contact with a real build, it can find bugs maintainers will actually fix.


Here's the list.


Project Bug Severity CVE


Node.js[Permission Model bypass via Unix Domain Sockets](https://winfunc.com/hacktivity/CVE-2026-21636) Medium CVE-2026-21636


React[RSC decoder DoS via $K FormData amplification](https://winfunc.com/hacktivity/CVE-2026-23864) High CVE-2026-23864


NGINX[stream module accepts revoked client certificates despite OCSP](https://winfunc.com/hacktivity/CVE-2026-28755) Medium CVE-2026-28755


NGINX[SCGI unbuffered mode sends truncated Content-Length](https://winfunc.com/hacktivity/nginx-scgi-content-length-unbuffered) Medium -


Mattermost[SSRF bypass via IPv4-mapped IPv6 literals](https://winfunc.com/hacktivity/CVE-2026-2455) Medium CVE-2026-2455


Mattermost[User-Agent version parser panic during session creation](https://winfunc.com/hacktivity/CVE-2026-25783) Medium CVE-2026-25783


Mattermost[Oversized password login DoS in legacy password comparison](https://winfunc.com/hacktivity/CVE-2026-24458) High CVE-2026-24458


Mattermost[Private channel enumeration through /mute error messages](https://winfunc.com/hacktivity/CVE-2026-21386) Medium CVE-2026-21386


Supabase[SQL injection via queue name interpolation](https://winfunc.com/hacktivity/supabase-sql-injection-via-queue-names) Critical -


Gumroad[0-click account takeover via helper endpoint](https://winfunc.com/hacktivity/gumroad-helper-auth-bypass-ato) Critical -


Anthropic MCP SDK[FastMCP custom routes skip auth middleware](https://winfunc.com/hacktivity/anthropic-fastmcp-auth-bypass) Critical -


Bun[Exponential merge keys in YAML parser](https://winfunc.com/hacktivity/bun-yaml-dos) High -


Better-Auth[Forged multi-session cookies revoke arbitrary sessions](https://winfunc.com/hacktivity/better-auth-multi-session-signout-ato) Medium -


Thirteen findings over a few months is a meaningful result. The sample is still small, the hit rate still swings by target, and the system still burns through plenty of dead hypotheses. Even so, it's enough evidence to stop treating model-driven vulnerability research as a toy.


## The pattern was familiar


Most of these bugs were familiar. They came from mismatches.


A protection existed, but one equivalent input slipped around it. Node.js blocked outbound TCP under the Permission Model but missed Unix domain sockets. Mattermost tried to block internal addresses and forgot that` \[::ffff:127.0.0.1\]` is still loopback in practice. NGINX stream checked the certificate chain, but not the OCSP revocation result that the HTTP path already enforced.


Another cluster was authentication that looked configured but failed at the point of use. FastMCP protected the built-in endpoints and left custom routes outside the auth middleware. Gumroad's helper endpoint checked that an` Authorization` header existed, not that it was valid. Better-Auth trusted a cookie name pattern during sign-out without verifying where it came from.


And then there were the usual resource-amplification bugs. React's RSC decoder could be pushed into repeated scans of attacker-controlled` FormData` . Bun's YAML merge-key handling turned a tiny file into seconds of work. Mattermost accepted oversized login passwords on legacy comparison paths before failing them.


None of that is glamorous, and that matters. Real software keeps breaking at trust boundaries, parser edges, and boring wiring mistakes. The system did well because it kept checking those areas mechanically, without getting bored and without assuming similar-looking code paths were actually equivalent.


## Three findings worth looking at closely


### NGINX: revoked certificate, accepted connection


This one is my favorite because the bug is tiny and the consequence is large.


In NGINX stream, you can require client certificates and turn on OCSP revocation checks. A revoked client cert should be dead on arrival. In the vulnerable path, it still connected.


The stream handler did this:


C


```text
if   (sscf->verify) {
rc = SSL_get_verify_result(c->ssl->connection);


if   (rc != X509_V_OK
&& (sscf->verify !=  3   || !ngx_ssl_verify_error_optional(rc)))
{
return   NGX_ERROR;
}


if   (sscf->verify ==  1  ) {
cert = SSL_get_peer_certificate(c->ssl->connection);


if   (cert ==  NULL  ) {
return   NGX_ERROR;
}


X509_free(cert);
}
/* no ngx_ssl_ocsp_get_status() here */
}


return   NGX_OK;


```


The HTTP module already had the extra check:


C


```text
if   (ngx_ssl_ocsp_get_status(c, &s) != NGX_OK) {
ngx_log_error(NGX_LOG_INFO, c-> log  ,  0  ,
"client SSL certificate verify error: %s"  , s);
ngx_http_finalize_request(r, NGX_HTTPS_CERT_ERROR);
return  ;
}


```


That's the whole bug. Stream learned the certificate was revoked, then never enforced the OCSP result. A revoked client cert kept working until expiry.


The system found it by comparing two modules that were supposed to honor the same security setting and then driving both with a revoked certificate. One path rejected. One path didn't. That's a good shape of bug for automation: same feature, two code paths, subtle enforcement drift.


NGINX fixed it in[PR #1213](https://github.com/nginx/nginx/pull/1213) .


### Mattermost: SSRF through an address-formatting gap


Mattermost had an` IsReservedIP` check meant to stop server-side requests to internal ranges. The function looked fine if you read it quickly:


GO


```text
func    IsReservedIP  (ip net.IP)     bool   {
for   _, ipRange :=  range   reservedIPRanges {
if   ipRange.Contains(ip) {
return    true
}
}
return    false
}


```


The problem sat in what *wasn't* in` reservedIPRanges` : the checks were written for IPv4 CIDRs. Go does not treat an IPv4-mapped IPv6 address as interchangeable with a 4-byte IPv4 value here. So` ::ffff:127.0.0.1` slid past an IPv4-only check even though it still points at loopback.


The fix was tiny:


GO


```text
func    IsReservedIP  (ip net.IP)     bool   {
if   ip4 := ip.To4(); ip4 !=  nil   {
ip = ip4
}
for   _, ipRange :=  range   reservedIPRanges {
if   ipRange.Contains(ip) {
return    true
}
}
return    false
}


```


Canonicalize first. Then check.


I've seen this class of SSRF bug more than once, which is exactly why a machine is useful here. It will keep trying dumb representation variants long after a human has talked themselves into thinking the filter is "basically fine."


### Supabase: yes, SQL injection


This one was almost insulting.


Supabase Studio's queue page took a queue name from the route and interpolated it into SQL:


JAVASCRIPT


```text
"pgmq"  . "q_${queueName}"  ;


```


No parameterization. No proper quoting. No validation that mattered. If a project member clicked a crafted URL, the dashboard could execute attacker-controlled SQL with service-role access.


The code says exactly what it says: SQL injection. In 2026. In a production SaaS dashboard.


What matters here is the category the system kept checking. Human reviewers often think they are too senior to miss route params flowing into SQL. The system kept tracing that path anyway. String interpolation near SQL. Boring source-to-sink work. Still effective.


Supabase fixed it in[PR #40290](https://github.com/supabase/supabase/pull/40290) .


## The part that mattered: execution


The model's raw output fails constantly.


Most hypotheses are wrong. Some are obviously wrong. Some are polished, plausible, technically worded nonsense. The model will happily describe an IDOR where the user ID comes from the session, a traversal where validation happened three functions up the stack, or a race in code that already sits under a mutex.


The execution loop made this usable.


The workflow is simple to describe and messy to run:


1. map the code and identify trust boundaries,
2. generate attack ideas around those boundaries,
3. turn those ideas into something executable,
4. run it against the target,
5. throw away nearly everything that doesn't survive reality.


Step three is where a lot of systems quietly fall apart. The model writes broken harnesses all the time. Missing imports. Wrong startup assumptions. Bad paths. Tests that fail before the hypothesis is even meaningfully exercised. Sometimes the environment is the problem. Sometimes the generated test is. Sometimes the idea was junk from the start.


That's fine. Broken harnesses are cheaper than bogus reports. Without a working reproducer, the finding dies there.


I care much more about that filter than I do about the model's ability to talk convincingly about a bug. Security tooling has enough fake confidence already.


## What these results support


These results support a straightforward claim: model-driven vulnerability research can produce real findings across different languages and bug classes. The list here spans C, Go, JavaScript, Python, Ruby; auth bugs, parser blowups, protocol mistakes, a sandbox bypass, and plain injection.


Human involvement remains heavy. Humans still choose targets, make the environments runnable, inspect the survivors, write up impact carefully, and handle disclosure. The system still depends on people at every stage that matters.


Industrial-scale throughput is still an open question. Thirteen findings is real evidence, and still a small sample. Some runs produced multiple issues. Some produced nothing useful. A lot of hypotheses died for good reasons.


The more useful comparison is marginal cost per extra target: one more parser, one more auth layer, one more dusty module. Once that cost drops, you start looking at code you would never justify auditing by hand.


The shift is economic. Once the machinery exists, it becomes cheap to be stubborn. Cheap to keep trying variant inputs. Cheap to compare two code paths that ought to behave the same way. Cheap to burn time on a module nobody thinks is worth a week of manual attention.


The hit rate still swings wildly by target. Some targets are miserable. Hardware-bound systems are awkward. Bugs that need long-lived state or weird deployment setups are slow and expensive to validate. And if a bug depends on a very human read of product intent, the model can still miss the point entirely.


Still, the floor has moved.


A year ago I would've bet against a system like this finding a Node.js sandbox escape, an NGINX revocation bypass, a React parser DoS, and a Mattermost SSRF filter miss in the same stretch of work. I was wrong.


## One practical takeaway


If you maintain software, ask for a reproducer. Or simply put,` PoC||GTFO` .


At scale, I trust a script, a request, or a test case I can run against the vulnerable build and then run again after the fix.


That advice applies to human reporters and machine-assisted ones alike. But the gap between execution-backed findings and model-generated slop is getting wider, and maintainers need a fast way to sort one from the other. A working PoC does that better than anything else I've seen.


The development that matters is the scaffolding around the models: with enough of it, they can grind through code, keep trying dumb variants, and occasionally hit something real.


That capability exists now. The open questions are where it works reliably, how often it fails, and how far it scales.


## Fixes and advisories


- [CVE-2026-21636](https://github.com/advisories/GHSA-7xhv-hcmf-4rfv) , fixed in Node.js[25.3.0](https://nodejs.org/en/blog/release/v25.3.0/) ,[24.13.0](https://nodejs.org/en/blog/release/v24.13.0/) ,[22.22.0](https://nodejs.org/en/blog/release/v22.22.0/) ,[20.20.0](https://nodejs.org/en/blog/release/v20.20.0/)
- [CVE-2026-23864](https://github.com/facebook/react/security/advisories/GHSA-83fc-fqcc-2hmg) , fixed in React[19.0.4, 19.1.5, 19.2.4](https://react.dev/blog/2025/12/11/denial-of-service-and-source-code-exposure-in-react-server-components)
- [CVE-2026-28755](https://github.com/nginx/nginx/pull/1213) , fixed in NGINX via[PR #1213](https://github.com/nginx/nginx/pull/1213)
- NGINX SCGI, fixed via commit` fe2d109` , tracked in[PR #1118](https://github.com/nginx/nginx/pull/1118)
- [CVE-2026-2455](https://github.com/mattermost/mattermost/pull/35097) ,[CVE-2026-25783](https://github.com/mattermost/mattermost/pull/35098) ,[CVE-2026-24458](https://github.com/mattermost/mattermost/pull/35092) with the 10.11 fix in[PR #35062](https://github.com/mattermost/mattermost/pull/35062) , and[CVE-2026-21386](https://github.com/mattermost/mattermost/pull/35099) , all fixed in Mattermost
- Supabase SQL injection, fixed via[PR #40290](https://github.com/supabase/supabase/pull/40290)
- Gumroad helper auth bypass, fixed via[PR #2098](https://github.com/antiwork/gumroad/pull/2098)
- Anthropic FastMCP auth bypass, fixed via[PR #1660](https://github.com/modelcontextprotocol/python-sdk/pull/1660)
- Bun YAML DoS, fixed via[PR #24729](https://github.com/oven-sh/bun/pull/24729)
- Better-Auth session revocation, disclosed via[GHSA-wmjr-v86c-m9jj](https://github.com/better-auth/better-auth/security/advisories/GHSA-wmjr-v86c-m9jj)
