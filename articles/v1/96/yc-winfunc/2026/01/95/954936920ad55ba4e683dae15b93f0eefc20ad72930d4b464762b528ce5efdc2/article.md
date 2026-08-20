---
schema_version: "1.0.0"
document_id: "954936920ad55ba4e683dae15b93f0eefc20ad72930d4b464762b528ce5efdc2"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-2455"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:6a2b4514f4cae3e14217806d7cb2400560f8d45a24ce2bac3747c2bdb011e5ed"
---

# SSRF protection bypass via IPv4-mapped IPv6 literals (CVE-2026-2455)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Mattermost


Medium


CVE-2026-2455


2026


# SSRF protection bypass via IPv4-mapped IPv6 literals (CVE-2026-2455)


## Summary


IPv4-mapped IPv6 addresses were not canonicalized before reserved-range checks


Mattermost's shared HTTP service protects untrusted outbound requests by resolving the target host and passing each IP through` allowIP` , which calls` IsReservedIP` and` IsOwnIP` .` IsReservedIP` contained IPv4 private, loopback, and link-local CIDRs, but it compared the raw` net.IP` value directly against those ranges. For an address such as` ::ffff:127.0.0.1` , Go represents the value as an IPv4-mapped IPv6 address. Without canonicalization, the intended IPv4 reserved ranges could be bypassed and the request could be allowed even though the effective target is loopback or another internal IPv4 address.


The fix calls` ip.To4()` at the start of` IsReservedIP` and, when it returns non-nil, compares the canonical native IPv4 bytes against the reserved ranges. The original fix is PR #35097 / commit` 5d787969c2d5ab591a9dcd61b0810475eed7a646` ; backports include #35122, #35128, #35129, and #35130.


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


H


Integrity


N


Availability


N


CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N


### Vulnerability Location


Source


Line 1021


server /channels


/app


/post_metadata.go


App.getLinkMetadataForURL


Sink


Line 30


server /public


/shared


/httpservice


/client.go


IsReservedIP


## Source-to-Sink Analysis


1


server/channels/app/post_metadata.go:1021-1040


Link preview/OpenGraph metadata fetching is one reachable path for user-controlled URLs. It creates a protected HTTP client with` MakeClient(false)` .


GO


```text
func    (a *App)    getLinkMetadataForURL(rctx request.CTX, requestURL  string  ) (*opengraph.OpenGraph, *model.PostImage,  error  ) {
// ...
client := a.HTTPService().MakeClient( false  )


```


2


server/public/shared/httpservice/httpservice.go:88-115


` MakeClient(false)` installs an` allowIP` callback that rejects reserved or self-assigned IPs unless explicitly allowed.


GO


```text
allowIP :=  func  (ip net.IP)     error   {
reservedIP := IsReservedIP(ip)


ownIP, err := IsOwnIP(ip)
if   err !=  nil   {
return   fmt.Errorf( "unable to determine if IP is own IP: %w"  , err)
}


if   !reservedIP && !ownIP {
return    nil
}


if   reservedIP {
return   fmt.Errorf( "IP %s is in a reserved range and not in AllowedUntrustedInternalConnections"  , ip)
}
return   fmt.Errorf( "IP %s is a self-assigned IP and not in AllowedUntrustedInternalConnections"  , ip)
}


```


3


server/public/shared/httpservice/client.go:129-164


The transport resolves the hostname, checks each IP with` allowIP` , and dials the first allowed address.


GO


```text
ips, err := net.LookupIP(host)
if   err !=  nil   {
return    nil  , err
}


for   _, ip :=  range   ips {
if   err := allowIP(ip); err !=  nil   {
forbiddenReasons =  append  (forbiddenReasons, err.Error())
continue
}


conn, err := dial(ctx, network, net.JoinHostPort(ip.String(), port))
if   err ==  nil   {
return   conn,  nil
}
}


```


4


server/public/shared/httpservice/client.go:30-41


The patch canonicalizes IPv4-mapped IPv6 addresses before checking reserved CIDRs, so` ::ffff:127.0.0.1` is evaluated as` 127.0.0.1` .


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


## Impact Analysis


### Critical Impact


The bypass can expose internal-only services reachable from the Mattermost server, including loopback services, RFC1918 resources, or cloud metadata endpoints, through any feature that returns or processes fetched metadata. The practical data exposure depends on the specific URL-fetching feature and response handling.


### Attack Surface


Features that fetch untrusted URLs through` HTTPService().MakeClient(false)` , including link metadata, image proxy paths, marketplace access, SAML metadata fetches, and integrations depending on configuration and caller permissions.


### Preconditions


The attacker must reach a feature that causes Mattermost to fetch an attacker-supplied URL through the protected HTTP client. In common chat paths this requires an authenticated user who can post a link or otherwise submit a URL.


## Proof of Concept


### Environment Setup


Use a vulnerable build before PR #35097 and a feature that fetches untrusted URLs through` MakeClient(false)` , such as link preview metadata.


### Target Configuration


Run Mattermost with default internal-connection protections. Have an authenticated user capable of posting a link in a channel.


### Exploit Delivery


Post a message containing a URL such as` http://\[::ffff:127.0.0.1\]:8065/api/v4/system/ping` or another internal IPv4 target encoded as an IPv4-mapped IPv6 literal.


### Outcome


IPv4-mapped IPv6 literals are evaluated using the same reserved-range policy as their native IPv4 equivalents.


**Expected Response:** Vulnerable builds allow the outbound connection to the internal IPv4 target. Fixed builds reject the address as reserved unless explicitly configured in` AllowedUntrustedInternalConnections` .


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/mattermost/mattermost/pull/35097)
