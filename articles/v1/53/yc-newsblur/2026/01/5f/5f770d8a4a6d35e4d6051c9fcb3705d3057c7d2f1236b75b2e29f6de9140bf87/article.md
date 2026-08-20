---
schema_version: "1.0.0"
document_id: "5f770d8a4a6d35e4d6051c9fcb3705d3057c7d2f1236b75b2e29f6de9140bf87"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-26fc52334fc9"
canonical_url: "https://forum.newsblur.com/t/premium-archive-per-feed-and-per-folder-auto-mark-as-read-settings/13392"
published_at: "2026-01-27T21:45:45+00:00"
first_seen_at: "2026-07-25T16:10:33.287397+00:00"
fetched_at: "2026-07-28T20:54:41.720106+00:00"
content_hash: "sha256:234741e0244179e10ee28f98742a7e03faeab9c4e6168fe83bea53e164f5c118"
---

# Premium Archive: Per-feed and per-folder auto-mark-as-read settings

```text
<p>Some feeds I want to read every single story. Others I’m happy to skim once a week. And a few high-volume feeds I only check occasionally, so stories older than a day or two aren’t worth catching up on. <a href="https://newsblur.com/?next=premium">Premium Archive</a> subscribers get the site-wide “days of unread” setting, but it was too blunt, applying the same rule to everything. Now you can set how long stories stay unread on a per-feed and per-folder basis.</p>


```


### How it works


Open the feed options popover (click the gear icon in the feed header) and you’ll see a new “Auto Mark as Read” section. Choose how many days stories should remain unread before NewsBlur automatically marks them as read:


The slider goes from 1 day to 365 days, with a “never” zone at the far right for feeds where you truly want to read every story regardless of age. Choose “Default” to inherit from the parent folder or site-wide setting, “Days” to set a specific duration, or “Never” to disable auto-marking entirely.


### Folder inheritance


Settings cascade down from folders to feeds. Set a folder to 7 days, and all feeds inside inherit that setting unless they have their own override. This is perfect for organizing feeds by how aggressively you want to age them out:


- **Must Read** folder: Set to “Never” so nothing ages out
- **News** folder: Set to 2 days since news gets stale fast
- **Blogs** folder: Set to 30 days for long-form content worth revisiting
- Individual feeds can still override their folder’s setting


The status text below the slider shows where the current setting comes from: the site-wide preference, a parent folder, or an explicit setting on this feed.


### Site settings dialog


You can also configure auto-mark-read from the site settings dialog (right-click a feed and choose “Site settings”). The same controls are available there, redesigned to match the popover style.


### Availability


Per-feed and per-folder auto-mark-as-read settings are a[Premium Archive](https://newsblur.com/?next=premium) feature, available now on the web. They work alongside the existing site-wide “days of unread” preference in Manage → Preferences → General → Days of unreads, which is also a Premium Archive feature.


If you have feedback or ideas for improvements, please share them on the[NewsBlur forum](https://forum.newsblur.com/) .


---


This is a companion discussion topic for the original entry at[https://blog.newsblur.com/2026/01/23/per-feed-auto-mark-read/](https://blog.newsblur.com/2026/01/23/per-feed-auto-mark-read/)
