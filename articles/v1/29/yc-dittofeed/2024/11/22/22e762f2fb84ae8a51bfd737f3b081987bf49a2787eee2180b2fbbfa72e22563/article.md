---
schema_version: "1.0.0"
document_id: "22e762f2fb84ae8a51bfd737f3b081987bf49a2787eee2180b2fbbfa72e22563"
company_key: "yc-dittofeed"
company: "Dittofeed"
source_id: "yc-dittofeed-news-import-09ec8ea46c9e"
canonical_url: "https://www.dittofeed.com/post/release-v0-18-0"
published_at: "2024-11-05T00:00:00+00:00"
first_seen_at: "2026-07-27T01:53:28.118221+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:d1c477ede2e302a3ddcb887137d28aec15c49f292c93860329643dbe2df859d3"
---

# Release v0.18.0

Hi all, Dittofeed v0.18.0 is an absolutely huge release! We’ve included a number of additions which have the potential to majorly impact the way you use Dittofeed.


The biggest part of this release is our new notion-like low code email editor! This has been a long time coming, and requested by many of our users.


To learn more details about this release, keep reading!


If you like this project, we'd really appreciate a star on GitHub! You can find our repository at[https://github.com/dittofeed/dittofeed](https://github.com/dittofeed/dittofeed) .


## Release Highlights


### New: Low-code Email Editor


Dittofeed has release a low code email editor. The low code editor makes it easy to compose primarily text based emails. This editor features a Notion-inspired “/” command interface for inserting new blocks.


Low-code email template editor


This feature has been requested by many teams and has been a long time coming. We’re excited to get feedback on the feature, improve on it, and see what teams use it to build!


### Improved: Journey Statuses


The Journeys table now displays the status of the journey on each row (running, paused).


Journey status


‍


### New: Copy Journeys and Segments Settings


We’ve added a new settings menu, that includes commands you can use to copy sample` curl` commands you can to use recreate journeys and segments.


Settings drop-down with curl command copy


‍


### New:` unsubscribe_url` Liquid Tag


We now support an` unsubscribe_url` tag in addition to an` unsubscribe_link` tag, which allows you to customize styling of your unsubscribe links in your templates.


[https://docs.dittofeed.com/resources/subscription-groups#adding-unsubscribe-links-to-emails](https://docs.dittofeed.com/resources/subscription-groups#adding-unsubscribe-links-to-emails)


### Fix: Admin API Endpoints, Segments and Templates


We fixed the admin API’s for the` GET /api/admin/content/templates` and` GET /api/admin/segments` to retrieve templates and segments respectively.


### Fix: Bug Fix In Process Assignments Pipeline


We fixed a bug in the pagination of process assignments pipeline that could cause assignments not to be processed when under a heavy load.


## Wrap Up


This deployment is a big deal for Dittofeed. We’re excited for people to try out our low code email editor and let us know what they think.


Also, a big thank you to our new contributor:


- [lyamone](https://github.com/lyamone)


Until next time!


‍
