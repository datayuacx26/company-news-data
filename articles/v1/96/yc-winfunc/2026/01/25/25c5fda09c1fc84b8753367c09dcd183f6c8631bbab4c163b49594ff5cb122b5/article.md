---
schema_version: "1.0.0"
document_id: "25c5fda09c1fc84b8753367c09dcd183f6c8631bbab4c163b49594ff5cb122b5"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-25783"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:b6aa6b2750eebbf1b10f28f871402c9f657da1ae379d179535f99f0f92d45f3b"
---

# User-Agent version parser panic during session creation (CVE-2026-25783)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Mattermost


Medium


CVE-2026-25783


2026


# User-Agent version parser panic during session creation (CVE-2026-25783)


## Summary


Malformed Mattermost-specific User-Agent prefixes could panic` getBrowserVersion`


` DoLogin` parses the HTTP` User-Agent` header while creating a session and stores platform, OS, browser name, and browser version on the session. The version helper looked for Mattermost-specific prefixes such as` Mattermost Mobile/` ,` Mattermost/` ,` mmctl/` , and` Franz/` , then immediately returned` strings.Fields(afterVersion)\[0\]` . If the header ended at the prefix or contained only whitespace after it,` strings.Fields` returned an empty slice and the` \[0\]` index panicked.


This is not reached before authentication in the normal password login flow:` login` calls` AuthenticateUserForLogin` first, then` DoLogin` . A malicious client needs a way to complete session creation, such as a valid account or an enabled registration/login flow. The fix centralizes prefixes in` versionPrefixes` , checks` len(fields) > 0` before indexing, and falls back to the parsed user-agent version. The original fix is PR #35098 / commit` 1346cf529aef0672c39a56ec10d1b8a9c8fb387d` ; backports include #35124, #35131, #35132, and #35133.


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


N


Integrity


N


Availability


H


CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:N/A:H


### Vulnerability Location


Source


Line 170


server /channels


/app


/login.go


App.DoLogin


Sink


Line 134


server /channels


/app


/user_agent.go


getBrowserVersion


## Source-to-Sink Analysis


1


server/channels/api4/user.go:2129-2130


After authentication succeeds, the login handler calls` DoLogin` with the original HTTP request, including the attacker-controlled` User-Agent` header.


GO


```text
isMobileDevice := utils.IsMobileRequest(r)
session, err := c.App.DoLogin(c.AppContext, w, r, user, deviceId, isMobileDevice,  false  ,  false  )


```


2


server/channels/app/login.go:170-179


` DoLogin` parses` r.UserAgent()` and asks` getBrowserVersion` for the version string that will be stored in the session props.


GO


```text
ua := uasurfer.Parse(r.UserAgent())


plat := getPlatformName(ua, r.UserAgent())
os := getOSName(ua, r.UserAgent())
bname := getBrowserName(ua, r.UserAgent())
bversion := getBrowserVersion(ua, r.UserAgent())


session.AddProp(model.SessionPropBrowser, fmt.Sprintf( "%v/%v"  , bname, bversion))


```


3


server/channels/app/user_agent.go:107-124 (before fix)


Before the patch, each special-case prefix indexed` strings.Fields(afterVersion)\[0\]` without confirming any field existed.


GO


```text
if   index := strings.Index(userAgentString,  "Mattermost Mobile/"  ); index !=  -1   {
afterVersion := userAgentString[index+ len  ( "Mattermost Mobile/"  ):]
return   limitStringLength(strings.Fields(afterVersion)[ 0  ], maxUserAgentVersionLength)
}


```


4


server/channels/app/user_agent.go:112-143


The patched version loops over known prefixes and only indexes the first field when` len(fields) > 0` ; otherwise it falls back to` getUAVersion` .


GO


```text
var   versionPrefixes = [] string  {
"Mattermost Mobile/"  ,
desktopAppVersionPrefix,
"mmctl/"  ,
"Franz/"  ,
}


func    getBrowserVersion  (ua *uasurfer.UserAgent, userAgentString  string  )     string   {
for   _, prefix :=  range   versionPrefixes {
if   _, after, ok := strings.Cut(userAgentString, prefix); ok {
if   fields := strings.Fields(after);  len  (fields) >  0   {
return   limitStringLength(fields[ 0  ], maxUserAgentVersionLength)
}
}
}
return   getUAVersion(ua.Browser.Version)
}


```


## Impact Analysis


### Critical Impact


Repeated successful login attempts with malformed User-Agent headers can generate panics and degrade availability of login/session creation. The blast radius is bounded by the requirement to reach` DoLogin` , but the missing bounds check is a server-side panic on attacker-controlled input.


### Attack Surface


Session creation paths that call` DoLogin` , including the password login endpoint after successful authentication and other login flows that create a session.


### Preconditions


The attacker must be able to complete login/session creation, for example with their own valid account. They control the` User-Agent` header.


## Proof of Concept


### Environment Setup


Use a vulnerable build before PR #35098 and an account that can log in successfully.


### Target Configuration


No special server configuration is required beyond a reachable login flow.


### Exploit Delivery


Send a valid login request with` User-Agent: Mattermost Mobile/` or` User-Agent: mmctl/` and correct credentials.


### Outcome


Malformed User-Agent prefixes no longer crash session creation.


**Expected Response:** Vulnerable builds panic with an index-out-of-range error in` getBrowserVersion` . Fixed builds complete the request and use the fallback parsed browser version when no prefix token exists.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/mattermost/mattermost/pull/35098)
