---
schema_version: "1.0.0"
document_id: "e6db98269a102bae4d817135a41fca840ea6928953e64a1d1ba2acf465ae6eb7"
company_key: "yc-winfunc"
company: "winfunc"
source_id: "yc-winfunc-rss-7ebe4d6da681"
canonical_url: "https://winfunc.com/hacktivity/CVE-2026-21386"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:05.938183+00:00"
fetched_at: "2026-07-28T22:24:25.720028+00:00"
content_hash: "sha256:c0c027a397a3208644d0056da4d5a206c7507bd9c9bb351b26f98179646cb25d"
---

# Private channel enumeration through /mute error messages (CVE-2026-21386)

[Back to Hacktivity](https://winfunc.com/hacktivity)


### Status: Patched


This vulnerability has been verified as resolved and deployed.


Mattermost


Medium


CVE-2026-21386


2026


# Private channel enumeration through /mute error messages (CVE-2026-21386)


## Summary


` /mute` exposed whether a private channel existed by returning a distinct not-member error


The` /mute` slash command accepts an optional channel handle, strips the leading` ~` , and looks up the named channel with` Store().Channel().GetByName(channel.TeamId, channelName, true)` . That lookup can resolve a private channel by name even when the caller is not a member. If the channel does not exist, the handler returns` api.command_mute.error` ; if it exists but` ToggleMuteChannel` fails because the caller is not a member, the handler returned` api.command_mute.not_member.error` . Those two responses gave authenticated users a yes/no oracle for private channel names.


The fix keeps the behavior but normalizes the not-member path to the same generic error used for nonexistent channels, and removes the now-unused translation. The original fix is PR #35099 / commit` 5bb5261c72faa476558a694c23581d24b734da41` ; backports include #35145, #35147, #35148, and #35149.


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


Line 48


server /channels


/app


/slashcommands


/command_mute.go


MuteProvider.DoCommand


Sink


Line 65


server /channels


/app


/slashcommands


/command_mute.go


MuteProvider.DoCommand


## Source-to-Sink Analysis


1


server/channels/app/slashcommands/command_mute.go:48-56


The slash command treats the user-supplied argument as a channel name, supporting both` ~channel` and bare channel names.


GO


```text
channelName :=  ""
splitMessage := strings.Split(message,  " "  )
if   strings.HasPrefix(message,  "~"  ) {
channelName = splitMessage[ 0  ][ 1  :]
}  else   {
channelName = splitMessage[ 0  ]
}


```


2


server/channels/app/slashcommands/command_mute.go:58-63


The handler looks up the channel by name and returns` api.command_mute.error` when no channel exists.


GO


```text
if   channelName !=  ""   && message !=  ""   {
channel, _ = a.Srv().Store().Channel().GetByName(channel.TeamId, channelName,  true  )


if   channel ==  nil   {
return   &model.CommandResponse{Text: args.T( "api.command_mute.error"  ,  map  [ string  ]any{ "Channel"  : channelName}), ResponseType: model.CommandResponseTypeEphemeral}
}
}


```


3


server/channels/app/slashcommands/command_mute.go:65-68


Before the fix, an existing channel that the user could not mute returned a different` not_member` error. The patched code returns the generic nonexistent-channel error instead.


GO


```text
channelMember, err := a.ToggleMuteChannel(rctx, channel.Id, args.UserId)
if   err !=  nil   {
}


```


## Impact Analysis


### Critical Impact


The issue leaks the existence of private channels and their names. It does not expose channel messages, but private channel names often reveal incident, customer, project, or organizational information.


### Attack Surface


The authenticated` /mute` slash command in any team where users can submit slash commands.


### Preconditions


The attacker must be authenticated and able to run slash commands in a team. They do not need to be a member of the private channels they probe.


## Proof of Concept


### Environment Setup


Use a vulnerable build before PR #35099. Create a team, a normal user, and a private channel named` secret-ops` that the normal user is not a member of.


### Target Configuration


The normal user only needs access to any channel where they can run` /mute` .


### Exploit Delivery


As the normal user, run` /mute ~secret-ops` and` /mute ~definitely-does-not-exist` .


### Outcome


The response no longer distinguishes private channel existence from nonexistent channel names.


**Expected Response:** Vulnerable builds return different translation IDs or messages for existing-not-member versus nonexistent channels. Fixed builds return the same generic` api.command_mute.error` response.


## Run this level of analysis on your repo.


Winfunc traces source-to-sink paths, validates exploitability, and gives your team patch-ready remediation.


[Vulnerability Detection](https://winfunc.com/products/scanner/vulnerability-detection)


[View Patch PR Verify fix on GitHub](https://github.com/mattermost/mattermost/pull/35099)
