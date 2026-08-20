---
schema_version: "1.0.0"
document_id: "dab6b11d014f284e8338c00c459a6699926d785cda38828cbae688dfb4ce9f10"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2021-10-05-postmortem-removing-all-users-from-github-trivago/"
published_at: "2021-10-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:6c8a23ca1ab922514b7414297b36cebb8a0f59d3aff71d440c0d50896e9fd18b"
---

# Postmortem: Removing all users from github.com/trivago

While engineering, we fix bugs, create new systems, build workflows and establish processes. Our job is to change things. Changing things can involve mistakes that ultimately lead to the failure of a particular system. To learn from these failures, a retrospective is helpful to get to the root of this problem. In the tech industry, a[Blameless PostMortem](https://codeascraft.com/2012/05/22/blameless-postmortems/) is the right tool for this job.


We engineers at trivago started applying this practice in 2019 and have been refining it continuously since then. The lessons learned and knowledge shared by a single post mortem make it an incredible source of information, and an essential part of our transparent engineering culture.


In this post, we’re sharing one of our recent internal post mortems related to our source code management platform of choice,[github.com/trivago](https://github.com/trivago) .


## Context


We use GitHub as our primary source code management and collaboration platform. All employees who need access to code, pull requests, continuous integration, and deployment runs or documentation have access to the single organization[github.com/trivago](https://github.com/trivago) .


This includes


- Software Engineers
- Site Reliability Engineers
- Quality Assurance
- Product Managers
- Data Analysts and Scientists
- User Research Experts
- and others


The majority of trivago employees require this platform to execute their daily work.


## The postmortem


- Context
- The postmortem


- Summary
- Detection
- Impact
- Timeline
- Root Cause


- Background about the current setup
- What happened


- Resolution and Recovery


- Restoring the Azure Active Directory security group
- Restoring GitHub organization administrative access
- Restoring all Users


- Corrective Actions
- Lessons Learned


- What went well
- Where we got lucky
- What did not go well


### Summary


Our automatic user synchronization mechanism between GitHub ↔ Azure Active Directory (AAD) removed all synced user accounts from our GitHub organization[github.com/trivago](https://github.com/trivago) .


The root cause was the deletion of a particular Azure Active Directory security group indicating which AAD user gets access to our GitHub organization[github.com/trivago](https://github.com/trivago) .


The respective AAD group was restored, and all users were invited to the particular GitHub organization again. Actions from each employee were required to accept the newly created invite.


The incident lasted 1h 19min.


### Detection


trivago talents lost access to our GitHub organization[github.com/trivago](https://github.com/trivago) , weren’t able to interact with the source code stored under this GitHub organization, and received an email that they have been removed from this GitHub organization.


Many of them reached out to various Slack channels with similar concerns.


The team responsible for maintaining the AAD security group detected that the particular access group was missing.


### Impact


trivago talents lost access to our GitHub organization[github.com/trivago](https://github.com/trivago) for at least 1h 19min.


During this time, local software development was still possible, but all actions that require access to private source code stored under[github.com/trivago](https://github.com/trivago) , like


- git pull/git push (or similar git operations)
- reviewing and merging Pull Requests via the GitHub UI
- interacting with GitHub actions


were not possible.


trivago talents had to accept the new GitHub email invite to regain access.


SSH keys and Personal Access Tokens needed to be re-enabled to access our organization via SSO by every talent.


### Timeline


> (all times in CEST, 24h format)
>
>
> 2021-09-07 16:13 Azure Active Directory security group was removed
>
>
> 2021-09-07 16:17 GitHub email “\[GitHub\] You’ve been removed from the “trivago N.V.” organization” arrived in the user’s inbox
>
>
> 2021-09-07 16:17 First user reported in Slack that they have lost their membership in[github.com/trivago](https://github.com/trivago) and can’t git push/pull anymore
>
>
> 2021-09-07 16:23 It was detected that the Azure Active Directory security group was not there anymore
>
>
> 2021-09-07 16:33 Azure Active Directory security group was recreated
>
>
> 2021-09-07 16:52 GitHub organization administrators involved in the incident have been re-added to the Azure Active Directory security group and received invitation emails “\[GitHub\] < Azure Active Directory Sync User > has invited you to join the @trivago organization” from GitHub again
>
>
> 2021-09-07 17:01 The same administrators have been re-granted their Organization Owner status
>
>
> 2021-09-07 17:15 The first batch of users have been re-added to the Azure Active Directory security group and received new invites into the Github organization
>
>
> 2021-09-07 17:32 All remaining users have been re-added to the Azure Active Directory security group and received new invites into the Github organization


### Root Cause


#### Background about the current setup


Our GitHub organization[github.com/trivago](https://github.com/trivago) has SAML single sign-on and provisioning via SCIM enabled. As a user backend, we use AAD (via a GitHub Enterprise Application).


All Azure Active Directory users of a particular group get synced with GitHub.


When an Azure Active Directory user gets added to the group, the user receives an invitation email from GitHub to join the organization[github.com/trivago](https://github.com/trivago) . If an Azure Active Directory user gets removed from the group or deactivated in Azure Active Directory, the GitHub user will be removed from[github.com/trivago](https://github.com/trivago) .


#### What happened


On Monday, the 6th of September, 2 Factor Authentication (2FA) was enforced for all members of our GitHub organization[github.com/trivago](https://github.com/trivago) (incl. external contributors). All trivago employees had been informed about this before and needed to enable 2FA on their GitHub Account. Employees who didn’t do this were removed automatically from the GitHub organization[github.com/trivago](https://github.com/trivago) . To regain access, a new Rights Management Ticket had to be created in our internal issue tracker.


One employee requested access to our GitHub organization[github.com/trivago](https://github.com/trivago) again for this reason. Because GitHub removed the user’s GitHub account from the organization, the Azure Active Directory account of the user was still part of the Azure Active Directory security group. Therefore, to regain access, a new GitHub invitation email needed to be triggered. To do this:


1. the Azure Active Directory user had to be removed from the Azure Active Directory security group
2. the synchronization between GitHub ↔ Azure Active Directory got triggered
3. the Azure Active Directory user had to be added to the Azure Active Directory security group
4. the synchronization between GitHub ↔ Azure Active Directory got triggered


While taking action on step 1, the Azure Active Directory security group got deleted rather than the Azure Active Directory user account removed from the group.


Respectively the Azure Active Directory users got removed from the “GitHub Enterprise Application” in Azure Active Directory and de-provisioned all users of this group from GitHub via SCIM 💥


The root cause boils down to a human error due to a repetitive task in combination with missing concentration.


### Resolution and Recovery


#### Restoring the Azure Active Directory security group


A quick Google search revealed that Azure Active Directory security groups could not be restored.


The previous GitHub organization administrators (a few trivago employees) have been assigned to the GitHub Enterprise Application directly, without a particular Azure Active Directory security group. This prevents the de-provisioning of GitHub organization administrators in case of a similar event.


A new Azure Active Directory security group was created and assigned to the GitHub Enterprise Application in Azure Active Directory.


#### Restoring GitHub organization administrative access


Single Sign-On via SCIM requires one bot-account on GitHub to be an administrator to execute the user synchronization operations like sending user invites, removing a user, and similar.


This bot account is an Azure Active Directory user who is directly assigned to the GitHub Enterprise Application and therefore did not get removed when removing the Azure Active Directory security group. By logging in via this bot account, the previous GitHub organization administrators could be promoted to organization administrators again.


#### Restoring all Users


An email message trace was run on our Microsoft Exchange Online server to find out who received a “You’ve been removed from the “trivago N.V.” organization” email.


Provisioning logs from the Azure GitHub Enterprise Application were checked to find users deprovisioned since thedeletion of the group at 16:13 CEST .


This list of users was then re-added to the new Azure Active Directory security group, and provisioning via SCIM was restarted.


GitHub started to send out invitation emails to our GitHub organization[github.com/trivago](https://github.com/trivago) again.


Once users accepted the invite again,[a GitHub automated process to restore the previous repository and team permissions starts](https://docs.github.com/en/organizations/managing-membership-in-your-organization/reinstating-a-former-member-of-your-organization#about-member-reinstatement) .


What was not restored automatically:


- public visibility of a user account being part of the organization


### Corrective Actions


- Backups: Create a regular and automated backup of AzureAD security group membership.
- Access and roles: Review all users and their respective access to AAD. Think about establishing more granular permissions and require re-login to a different account to execute destructive actions.
- Automation: Automate the Rights Management ticket process with a self-service portal or similar.


### Lessons Learned


- Manual routine tasks are going to fail eventually.
- Automate manual processes if possible.
- Azure Active Directory security groups do not have restore options.
- GitHub automatically restores repository access and team roles when re-adding a previously enrolled account.
- GitHub admins are now assigned to the Enterprise Application directly, not via an Azure Active Directory security group. In case of repeated group deletion, the admins will not be de-provisioned.


#### What went well


- The synchronization mechanism between GitHub ↔ Azure Active Directory worked well and removed all human accounts.
- We tested the off-boarding process of all talents
- The Azure Enterprise App de-provisioning log is accurate and can be trusted.


#### Where we got lucky


- We were able to gather a list of affected Azure Active Directory accounts and their email addresses by searching for the “You’ve been removed from the “trivago N.V.” organization” email on our Microsoft Exchange server.
- We also gathered a list of de-provisioned people from the Azure GitHub Enterprise Application integration from the Azure Active Directory log.
- GitHub automatically restores team and repository permissions and roles.
- The GitHub user account we use for syncing users between Azure Active Directory and GitHub was still part of the organization, had the owner role, and we could log in and restore organization administrative access for previous GitHub admins.
- External contributors haven’t been affected because they are not required to log in via Single Sign-On.


#### What did not go well


- Assigning members that rejoined the organization to the teams that they were part of before leaving didn’t work in all cases. Some manual assignments had to be done. → We don’t have details about the GitHub process here.
- All (human) GitHub organization owners got removed as well.
- Some GitHub invite emails were not sent, and some invitation links did not work (Invite not found), resulting in manual work on our side and removing, re-adding, and re-provisioning via SCIM to re-trigger the invitation for those (5-10 out of 430 cases).
