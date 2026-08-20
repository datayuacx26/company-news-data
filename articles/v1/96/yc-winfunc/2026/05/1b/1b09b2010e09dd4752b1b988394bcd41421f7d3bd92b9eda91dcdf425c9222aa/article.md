---
schema_version: "1.0.0"
document_id: "1b09b2010e09dd4752b1b988394bcd41421f7d3bd92b9eda91dcdf425c9222aa"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-7184"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:13:05.421454+00:00"
content_hash: "sha256:b9dde5054f7c8a46faa15030d3dac88cb1d905f4985ce04fc5e9beb7f1ae3559"
---

# Remote cluster PATCH response leaked authentication tokens (CVE-2026-7184)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Mattermost


Medium


CVE-2026-7184


2026-05-18


# Remote cluster PATCH response leaked authentication tokens (CVE-2026-7184)


## Summary


Remote-cluster PATCH returned the updated model before clearing token fields


Mattermost's remote-cluster API already treated` token` and` remote_token` as sensitive fields: list, create, accept, and get handlers called` RemoteCluster.Sanitize()` before serializing objects to clients. The PATCH handler was the missed trust boundary. After decoding a caller-controlled` RemoteClusterPatch` and applying it through` App.PatchRemoteCluster` ,` patchRemoteCluster()` wrote` updatedRC` directly to the HTTP response without sanitizing it first.


The public advisory[MMSA-2026-00662](https://mattermost.com/security-updates) /[CVE-2026-7184](https://github.com/advisories/GHSA-9p44-r552-4wp9) describes this as a Medium CWE-201 issue affecting Mattermost` 11.6.x <= 11.6.1` ,` 11.5.x <= 11.5.4` , and` 10.11.x <= 10.11.15` , with fixed versions` 11.7.0` ,` 11.6.2` ,` 11.5.5` , and` 10.11.17` . The original fix is PR #36288 / commit` bd8fc92226726da06c8fabaef568cc9ebaee1cb8` ; release-line backports include #36310, #36311, and #36313.


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


Line 638


server /channels


/api4


/remote_cluster.go


patchRemoteCluster()


Sink


Line 666


server /channels


/api4


/remote_cluster.go


patchRemoteCluster()


## Source-to-Sink Analysis


1


server/channels/api4/remote_cluster.go:638-642


An authenticated caller supplies the PATCH body. The handler decodes that body into a` RemoteClusterPatch` ; changing a harmless editable field such as` display_name` is enough to reach the vulnerable response path.


GO


```text
var   patch model.RemoteClusterPatch
if   jsonErr := json.NewDecoder(r.Body).Decode(&patch); jsonErr !=  nil   {
c.SetInvalidParamWithErr( "remotecluster"  , jsonErr)
return
}


```


2


server/channels/api4/remote_cluster.go:623-626


The endpoint is intentionally reachable to users with` manage_secure_connections` . That role can manage secure connections but is not equivalent to full system-administrator access to remote-cluster secrets.


GO


```text
func    patchRemoteCluster  (c *Context, w http.ResponseWriter, r *http.Request)    {
c.RequirePermissionToManageSecureConnections()
if   c.Err !=  nil   {
return
}


```


3


server/channels/api4/remote_cluster.go:657-661


` App.PatchRemoteCluster` returns the updated stored` RemoteCluster` object. That object still contains the trust credentials used by the remote-cluster relationship.


GO


```text
updatedRC, err := c.App.PatchRemoteCluster(c.Params.RemoteId, &patch)
if   err !=  nil   {
c.Err = err
return
}


```


4


server/public/model/remote_cluster.go:67-74


The model exposes both secret fields as JSON fields and provides` Sanitize()` specifically to blank them before response serialization.


GO


```text
Token        string    `json:"token"`
RemoteToken  string    `json:"remote_token"`


func    (rc *RemoteCluster)    Sanitize() {
rc.Token =  ""
rc.RemoteToken =  ""
}


```


5


server/channels/api4/remote_cluster.go:663-666 (before fix)


Before PR #36288, the handler added the unsanitized object to the audit result and encoded the same object directly to the client, exposing` token` and` remote_token` in the PATCH response.


GO


```text
auditRec.Success()
auditRec.AddEventResultState(updatedRC)


if   err := json.NewEncoder(w).Encode(updatedRC); err !=  nil   {
c.Logger.Warn( "Error while writing response"  , mlog.Err(err))
}


```


6


server/channels/api4/remote_cluster.go:660-668 (fix, commit bd8fc92)


The accepted upstream fix sanitizes the updated object before both audit result capture and JSON encoding. Regression coverage now checks that PATCH responses return empty` Token` and` RemoteToken` values.


GO


```text
updatedRC.Sanitize()


auditRec.Success()
auditRec.AddEventResultState(updatedRC)


if   err := json.NewEncoder(w).Encode(updatedRC); err !=  nil   {
c.Logger.Warn( "Error while writing response"  , mlog.Err(err))
}


```


## Impact Analysis


### Critical Impact


The direct impact is disclosure of remote-cluster authentication material that should remain server-side. In deployments where those values are accepted by token-authenticated remote-cluster endpoints, the leaked credentials can let the caller impersonate a trusted remote-cluster peer or abuse remote-cluster trust flows.


### Attack Surface


Mattermost deployments with remote-cluster support enabled and at least one existing remote-cluster record. The vulnerable route is` PATCH /api/v4/remotecluster/{remote_id}` and requires an authenticated session with` manage_secure_connections` .


### Preconditions


The attacker must have the secure-connection management permission and know a valid remote cluster ID. No user interaction is required, and the PATCH body can change an ordinary editable field such as` display_name` .


## Proof of Concept


### Environment Setup


Use a vulnerable Mattermost build before PR #36288 with remote-cluster support enabled. Authenticate as a user that has` manage_secure_connections` , and identify an existing remote cluster ID through the admin UI or a permitted API path.


### Target Configuration


Set` MM_URL` to the Mattermost base URL and` RC_ID` to an existing remote cluster ID. Use a session cookie or bearer token for a secure-connection manager.


### Exploit Delivery


Send a benign PATCH request and inspect the returned object:


BASH


```text
curl -sk \
-H  "Authorization: Bearer  $MM_TOKEN  "   \
-H  'Content-Type: application/json'   \
-X PATCH  " $MM_URL  /api/v4/remotecluster/ $RC_ID  "   \
--data  '{"display_name":"leak-check"}'   | jq  '.token, .remote_token'


```


### Outcome


A permitted secure-connection manager can read remote-cluster trust secrets from the PATCH response on vulnerable builds. PR #36288 prevents the disclosure by sanitizing before serialization.


**Expected Response:** Vulnerable builds return non-empty` token` and/or` remote_token` values. Fixed builds return those fields as empty strings, matching the behavior of the other remote-cluster response paths.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/mattermost/mattermost/pull/36288)
