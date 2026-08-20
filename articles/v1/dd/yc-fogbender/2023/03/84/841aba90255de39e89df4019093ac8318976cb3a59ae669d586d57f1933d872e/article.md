---
schema_version: "1.0.0"
document_id: "841aba90255de39e89df4019093ac8318976cb3a59ae669d586d57f1933d872e"
company_key: "yc-fogbender"
company: "Fogbender"
source_id: "yc-fogbender-news-import-46cf0bf99c36"
canonical_url: "https://fogbender.com/blog/using-groups-to-configure-oncall-agents"
published_at: "2023-03-26T00:00:00+00:00"
first_seen_at: "2026-07-21T20:41:04.212025+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:aa3f0fb0a808cf006ae9f3da5391eb15a37f0914c813754d6f42b6109a80274e"
---

# Using groups to configure on-call agents

Whenever a new user asks a question in support, who on the vendor’s end should be notified?


Of the possible options — “everyone”, “someone”, and “nobody” — we’ve so far focused on ensuring it’s never the latter, by notifying everyone.


This works well for early startups, where support conversations are anything but routine — they are precious signs of life on otherwise hostile and questionable worlds.


For established teams, however, the answer must be a version of “someone”, that, depending on the situation, is smart enough to deal with work shifts, time zones, vacations, and offsites.


Below, we introduce four new features designed to address some of these concerns.


## # 1. Agent groups


To create an agent group, head to[https://fogbender.com/admin/-/team](https://fogbender.com/admin/-/team) and look for the “Groups” section. Once there, you can create a group, for example “oncall” or “devops”, and populate the group with agents.


Note that if you delete a group and then create a group with the same name, the previous group membership will be retained. Also, note that the group “all” is always there by default.


## # 2. Assigning groups to rooms


Once your group is ready, you can use the assignment control the room header — just below the close button — and select (or search for) the group in the dropdown:


Once assigned, only group members (as well as other room assignees, if any) will be notified on new messages. Multiple groups can be assigned at the same time.


Note that if a room’s only assignee is a group with no members, it’s equivalent to “no assignees”, which means all agents will be notified on new messages. Also note that a mention always triggers a notification, even if the mentionee is not assigned directly or as a group member.


## # 3. Default group assignment


To avoid having to manually assign groups to customer-facing rooms and to ensure all new customer-facing rooms are automatically assigned to the appropriate agent group, you can use the **Default group assignment** setting in[workspace notifications](https://fogbender.com/admin/-/-/settings/notifications) .


## # 4. PagerDuty integration


Since updating the list of on-call agents manually is rather cumbersome, you can automate this process by integrating Fogbender with PagerDuty under[Incident response integrations](https://fogbender.com/admin/-/-/settings/integrations) .


Once you’ve successfully authenticated PagerDuty, create or select an existing group to sync with the result of PagerDuty’s[/oncalls](https://developer.pagerduty.com/api-reference/3a6b910f11050-list-all-of-the-on-calls) API, which lists all the PagerDuty users in your account who are currently on call.


Note that if you’re using an existing group for this integration, its membership will be overridden.


The photo (cropped and enhanced) of the Motorola Bravo Express pager used as a thumbnail for this post is by[hades2k](https://www.flickr.com/photos/hades2k) , published under the[CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/) license.
