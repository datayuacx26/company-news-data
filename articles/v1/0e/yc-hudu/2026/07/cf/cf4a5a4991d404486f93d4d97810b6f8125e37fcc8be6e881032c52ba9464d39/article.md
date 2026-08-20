---
schema_version: "1.0.0"
document_id: "cf4a5a4991d404486f93d4d97810b6f8125e37fcc8be6e881032c52ba9464d39"
company_key: "yc-hudu"
company: "Hudu"
source_id: "yc-hudu-news-import-7b5cd22111cb"
canonical_url: "https://www.hudu.com/blog/release-update-2.44.0"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-08-18T20:50:28.572174+00:00"
fetched_at: "2026-08-18T20:50:30.768646+00:00"
content_hash: "sha256:172cf6c74b042ffc377efaed6e6d2e12015f8bb1e62e47618e01619e7497b271"
---

# Release: Hudu 2.44.0

Hudu 2.44.0 is live now for self-hosted environments, and rolling out to cloud-hosted environments today. This release is about organization and connection — giving you better ways to categorize and link your data, while the integration experience itself gets a cleaner front door.


## Action required


Two changes need your attention before (or shortly after) you update:


- **Password Created/Updated alerts are being deprecated.** Update this alert type to **Record Created/Updated/Deleted** with record type "password," and make sure your webhook information carries over, to avoid any disruption to your alerting.
- **Addigy API V1 is no longer supported.** If you haven't already updated your Addigy integration settings to V2, do so now — V1 connections will stop working.


## Highlights


### Cross-record labels


Records in Hudu can now carry custom, color-coded labels — scoped to specific record types and companies, so a label built for passwords doesn't clutter your asset views. Apply them while viewing, editing, or creating a record, and manage the label types themselves from Admin. Labels can also be set on records directly via API.


If you were using password tags before, don't worry — those convert to labels automatically the next time you update them.


### Integration directory, redesigned


The integration directory has been redesigned with collapsible sections and clearer status indicators, so it's easier to see what's connected, what's paused, and what needs attention at a glance.


For teams running a dozen-plus integrations, a directory that's actually easy to navigate means less time hunting for the right connector and more time acting on what it tells you.


### Connect the fields that matter


Hudu now supports connecting Halo custom fields, not just standard asset and site fields. That means you can map a Halo asset, site, or custom field, and pull that data straight into your Hudu asset fields instead of re-entering it by hand.


If your team already relies on Halo's custom fields for site-specific or client-specific tracking, this closes a real gap — that data can now populate directly into Hudu without manual upkeep.


## Improvements


- Reorganized admin left-hand navigation menu
- UI updates to Admin/Alerts pages, alongside the password alert deprecation noted above
- UI updates to Museum and Central KB bulk actions
- Photo endpoint now supports both uppercase and lowercase` photoable_type` , and accepts` 0` (in addition to` null` ) when moving a photo out of a folder
- Authors and Editors can now add and remove individual records from the portal
- Asset layouts are now organized by folders on the Global page
- Public share links for process runs now require explicit opt-in, rather than generating automatically
- Process run share pages now display the run name instead of the process name
- Standardized naming of existing webhook variables for alerts (no functional change)
- MCP optimization improvements to reduce token usage
- Performance improvements addressing recent memory creep and CPU spikes


## Integrations


- Integration directory redesign, including collapsible sections and integration status indicators
- Bulk "Create/match for all" and "Delete unmatched options" actions when the matchers table spans more than one page, across all V2 integrations
- Ability to bulk change matches for already-matched companies in all V2 integrations
- Connected Field data point input updated to a type-to-search picker
- New FAQ List access control setting for Hudu/Halo KB sync (applies to articles that haven't already synced to Halo)
- Removed the "Show for all users" option in Hudu/Halo KB sync settings (applies to articles that haven't already synced to Halo)
- Added support for Atera JWT tokens
- Auvik integration redesign for easier setup and less page-hopping — see highlight above for details
- Removed support for deprecated Addigy API V1


## Bug fixes


- Fixed a 500 error when accessing an unauthorized resource from a Teams link
- Fixed Admin role being unable to unlock a locked-out Super Admin
- Fixed websites being unable to be archived via API
- Fixed MCP CompanyIndexTool missing its` q` param
- Fixed the "Create new account" page not scrolling correctly on mobile
- Fixed Spectators incorrectly being able to enable/disable public share links
- Fixed Addigy V2 connected fields (e.g., serial numbers) not syncing to asset fields
- Minor bug fixes & performance improvements
