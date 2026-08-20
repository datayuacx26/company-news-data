---
schema_version: "1.0.0"
document_id: "443a1ab127dae507ce798809a0c94623faf202fcf85c1d24e4ee4188bb6f8fee"
company_key: "yc-promptless"
company: "Promptless"
source_id: "yc-promptless-news-import-48c94b307195"
canonical_url: "https://promptless.ai/blog/product-updates/teams-private-channel-file-access/"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-08-07T08:03:50.270603+00:00"
fetched_at: "2026-08-07T08:03:52.181710+00:00"
content_hash: "sha256:3f6a20db0d2bbdd9a87c8c769a59ccbc1bb8e07fcd6f476286cf32d352ec6363"
---

# Grant Promptless Access to Files in Private and Shared Teams Channels

# Grant Promptless Access to Files in Private and Shared Teams Channels


[← Back to Blog](https://promptless.ai/blog)


Promptless can now read files attached in private and shared Microsoft Teams channels. Standard channels were already accessible. Private and shared channels were not, because Microsoft gives each of them its own separate SharePoint site.


## The problem


Section titled “The problem”


Private and shared channels each get their own isolated SharePoint site, separate from the team root. Microsoft exposes a channel’s site only to its members, so Promptless had no way to discover those sites at connect time. Only the team-level site appeared in the site picker.


That meant any file attached in those channels was outside Promptless’s reach. A design spec posted in a private channel was invisible to Promptless. So were a partner’s API references dropped into a shared channel. The files Promptless most needed for context were often in the channels it could not access.


## What changed


Section titled “What changed”


The site picker in the Microsoft Teams connection flow now lists each private and shared channel as its own selectable site. Each channel appears alongside the team-level site. You can grant Promptless read access to individual channels the same way you grant it to team-level sites.


Microsoft exposes a private or shared channel’s site only to that channel’s members. Promptless resolves available sites using the account you authenticate with at connect time. Only channels the connecting account belongs to appear in the site picker. If a channel does not appear, reconnect as a member of that channel. It then appears in the site picker.


This change only affects which attached files Promptless can read. The Promptless bot’s presence and behavior in your channels are unchanged.


## Who benefits most


Section titled “Who benefits most”


Teams that keep working docs in private or shared channels benefit most. If design specs, architecture decisions, or API references live there, those files can now inform Promptless’s suggestions.


Shared channels matter most for teams that collaborate externally. Your product team might run a shared channel with a partner’s engineers. Technical decisions documented there can now feed into Promptless’s context.


*We regularly share actionable insights grounded in research, experiments, and real-world product learnings. Subscribe to get future posts in your inbox.*


## How to set it up


Section titled “How to set it up”


Reconnecting requires the Promptless Admin role. Open the[Integrations page](https://app.gopromptless.ai/integrations) and reconnect as an account that belongs to the private or shared channels you want to include. After reconnecting, select those channels from the site picker and follow the connection flow to complete the access grant. Selecting a site surfaces it for granting but does not grant access on its own.


If you join new private or shared channels later, reconnect again so they appear in the site picker. Then select and complete the grant for each new channel.


## More from the blog


- [Fix skill slop before it makes your AI workforce worse](https://promptless.ai/blog/product-updates/introducing-promptless-for-agent-instructions) Product Updates


- [Promptless Now Alerts You When an Integration Has a Problem](https://promptless.ai/blog/product-updates/integration-health-alerts) Product Updates


- [Google Drive as a Context Source for Documentation Suggestions](https://promptless.ai/blog/product-updates/google-drive-context-source) Product Updates


[← Back to Blog](https://promptless.ai/blog)
