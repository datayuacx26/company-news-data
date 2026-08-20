---
schema_version: "1.0.0"
document_id: "9316f6862b040892b750e9e19b395e80fc6071403c93b66daa4f29cac76408ae"
company_key: "yc-protonet"
company: "Protonet"
source_id: "yc-protonet-news-import-7573e14add71"
canonical_url: "https://support.protonet.info/en/news/soul-4-1-permissions-editor-bug-fixes-and-more/"
published_at: null
first_seen_at: "2026-07-27T11:22:08.735893+00:00"
fetched_at: "2026-07-28T21:16:46.713740+00:00"
content_hash: "sha256:d7310cfde4f93f4ec82b4ed72bd53a1947787e1b653d5e196aaf8bcd703d45be"
---

# SOUL 4.1 – PERMISSIONS EDITOR, BUG FIXES AND MORE

**SOUL Stable/124**


*Prerequisite for this update is an active SOUL+ contract and a Protonet server with SOUL version stable/xxx. You will see the version after logging into your SOUL on the help page.*


**Updated Backup to allow better disk handling**


**Updated Letsencrypt to new architecture, selfhost**


This fixes the auto renovation fails affecting Boxes with custom node names.


**Introduced User Permissions for the ‘Notes’ feature.**


In SOUL Settings under “Advanced” Box admins can now choose, if guest users should be allowed to edit and/or create notes.


**Added missing German translations on ‘Box-Status’.**


We fixed the missing translation and error messages on the Box-Status page under SOUL Settings.


**Added Typing indicator settings (On/Off)**


in SOUL Settings under ‘General’ any user can now choose to turn the typing indicators on or off.


**Fixed the navigation to item from the notification view.**


The button “open” on the notification stream works properly again.


**Fixed ‘adding link to an object’ button in the project chat.**


**Fixed the time-frame-limit issue on the calendar feature**


Now the calendar feature can show events, “forever into the past” again


*(ics creation excepted for technical reasons)*


**Disabled editing of archived notes.**


**Fixed creation of all day events.**


**Tweaks under the hood:**


- Improved the logic for running-time errors for the failed sensors.
- Improved performance of todo lists filters.
