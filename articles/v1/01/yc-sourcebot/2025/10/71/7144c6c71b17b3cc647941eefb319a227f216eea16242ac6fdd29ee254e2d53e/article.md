---
schema_version: "1.0.0"
document_id: "7144c6c71b17b3cc647941eefb319a227f216eea16242ac6fdd29ee254e2d53e"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/permission-syncing-30-10-25"
published_at: "2025-10-30T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:07e1bad02a041707aceeb9a355b2008b81b0af5908a81ecdf0c0b082c9fc5bb2"
---

# GitHub & GitLab permission syncing

Code hosts like GitHub & GitLab support configuring access control lists (ACLs) that define what repositories a user can and cannot see. We’ve shipped experimental support for syncing these ACLs with Sourcebot. The TL;DR is:


-


Users sign into Sourcebot using OAuth connected to the code host (either GitHub or GitLab).


-


Once signed in, the user will **only** be able to access repositories they would have access to on the code host. Specifically:


-


Sourcebot periodically syncs with the configured code host to ensure what repositories the user should (or should not) have access to are up to date.


This feature is a part of our[enterprise offering](https://www.sourcebot.dev/pricing) . If you’d like to learn more, checkout the[docs](https://docs.sourcebot.dev/docs/features/permission-syncing) .
