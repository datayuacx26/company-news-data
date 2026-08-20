---
schema_version: "1.0.0"
document_id: "a3d4e1af0b6b028eaa31dd533ccb35cc0d07d9b2fe8277582aac6a165090e51c"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-3115"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:571f1ace8874b97269906f49f9736b6907cb714cee9c785540f8b3f5a259761d"
---

# Group member IDs leaked because GetGroup bypassed view restrictions (CVE-2026-3115)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Mattermost


Medium


CVE-2026-3115


2026


# Group member IDs leaked because GetGroup bypassed view restrictions (CVE-2026-3115)


## Summary


Group member ID expansion ignored the caller's ViewUsersRestrictions


The` GET /api/v4/groups/{group_id}` handler computed` ViewUsersRestrictions` for the session user and passed those restrictions into` App.GetGroup` .` GetGroup` correctly applied restrictions when returning` IncludeMemberCount` , but when` IncludeMemberIDs` was requested it called` Store().Group().GetMemberUsers(id)` , a raw unpaginated store method with no restrictions parameter. Guests or restricted users could therefore request a group with` include_member_ids=true` and receive member user IDs outside the teams or channels they were allowed to see.


The fix changes the member-ID expansion path to page through` GetMemberUsersPage(id, page, perPage, viewRestrictions)` , which applies` applyViewRestrictionsFilter` in the SQL store. The original fix is PR #35172 / commit` a06d5065e709e26baed531a528d4b9950f26e3ea` ; backports include #35209, #35277, and #35333.


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


N


Availability


N


CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:L/I:N/A:N


### Vulnerability Location


Source


Line 119


server /channels


/api4


/group.go


getGroup


Sink


Line 28


server /channels


/app


/group.go


App.GetGroup


## Source-to-Sink Analysis


1


server/channels/api4/group.go:119-128


The API handler derives` ViewUsersRestrictions` for the current session and calls` GetGroup` with` IncludeMemberIDs` controlled by the query string.


GO


```text
restrictions, appErr := c.App.GetViewUsersRestrictions(c.AppContext, c.AppContext.Session().UserId)
if   appErr !=  nil   {
c.Err = appErr
return
}


group, appErr := c.App.GetGroup(c.Params.GroupId, &model.GetGroupOpts{
IncludeMemberCount: c.Params.IncludeMemberCount,
IncludeMemberIDs:   c.Params.IncludeMemberIDs,
}, restrictions)


```


2


server/channels/app/group.go:28-39


Before the patch,` IncludeMemberIDs` called` GetMemberUsers(id)` and appended every returned user ID. The patched code pages through` GetMemberUsersPage` with the same view restrictions.


GO


```text
if   opts !=  nil   && opts.IncludeMemberIDs {
perPage :=  100
users, err := utils.Pager( func  (page  int  )    ([]*model.User,  error  ) {
return   a.Srv().Store().Group().GetMemberUsersPage(id, page, perPage, viewRestrictions)
}, perPage)
if   err !=  nil   {
return    nil  , model.NewAppError( "GetGroup"  ,  "app.member_count"  ,  nil  ,  ""  , http.StatusInternalServerError).Wrap(err)
}


for   _, user :=  range   users {
group.MemberIDs =  append  (group.MemberIDs, user.Id)
}
}


```


3


server/channels/store/sqlstore/group_store.go:494-508


` GetMemberUsers` is the unrestricted store method that previously fed the response. It filters only deleted rows and group ID, not caller visibility.


GO


```text
builder := s.groupMemberUsersSelectQuery.
Where(sq.Eq{
"GroupMembers.DeleteAt"  :  0  ,
"Users.DeleteAt"  :         0  ,
"GroupMembers.GroupId"  :  groupID,
})


if   err := s.GetReplica().SelectBuilder(&groupMembers, builder); err !=  nil   {
return    nil  , errors.Wrapf(err,  "failed to find member Users for Group with id=%s"  , groupID)
}


```


4


server/channels/store/sqlstore/group_store.go:515-523


The patched path uses` GetMemberUsersPage` /` GetMemberUsersSortedPage` , where` applyViewRestrictionsFilter` limits results to users visible through the caller's teams or channels.


GO


```text
userQuery := s.groupMemberUsersSelectQuery.
Where(sq.Eq{ "GroupMembers.DeleteAt"  :  0  }).
Where(sq.Eq{ "Users.DeleteAt"  :  0  }).
Where(sq.Eq{ "GroupMembers.GroupId"  : groupID})


userQuery = applyViewRestrictionsFilter(userQuery, viewRestrictions,  true  )


```


## Impact Analysis


### Critical Impact


The vulnerability leaks membership information across Mattermost's user visibility boundary. It does not expose message content, but it reveals user IDs and group membership relationships that restricted users should not be able to enumerate.


### Attack Surface


The authenticated groups API when the caller can access a group object and request` include_member_ids=true` , especially guest or restricted-user deployments where` ViewUsersRestrictions` are expected to hide unrelated users.


### Preconditions


The attacker must be authenticated and able to call the groups API for a referenceable group. The target deployment must have users outside the attacker's visible teams or channels who are members of that group.


## Proof of Concept


### Environment Setup


Use a vulnerable build before PR #35172 with guest accounts or restricted user visibility enabled. Create a group with members spread across a channel/team the guest can see and users the guest cannot see.


### Target Configuration


Ensure the restricted caller can resolve the group but cannot view all users in the group under normal member-list APIs.


### Exploit Delivery


As the restricted user, call` GET /api/v4/groups/<group_id>?include_member_ids=true` with a valid session token.


### Outcome


The fix aligns` IncludeMemberIDs` with the already-restricted member-count path.


**Expected Response:** Vulnerable builds return` MemberIDs` containing users outside the caller's view restrictions. Fixed builds return only IDs allowed by` ViewUsersRestrictions` , or an empty list if no members are visible.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/mattermost/mattermost/pull/35172)
