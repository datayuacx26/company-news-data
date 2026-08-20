---
schema_version: "1.0.0"
document_id: "b62345f47296ea91a15ff577a9aeeca97e0ec958d04902427d49d5367f1c4060"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-0646845e5ccf"
canonical_url: "https://forum.newsblur.com/t/same-newsletter-create-multiple-feeds/13668"
published_at: "2026-08-13T17:04:21+00:00"
first_seen_at: "2026-08-13T20:35:18.275402+00:00"
fetched_at: "2026-08-13T20:35:20.424798+00:00"
content_hash: "sha256:4f23988620552609334b1ead5a2d6ab1f0178bfb785d47d422f35b23541c094d"
---

# Same newsletter create multiple feeds

[3fr](https://forum.newsblur.com/u/3fr)


May 4, 2026, 5:53pm 1


I have lots of newsletters forwarded into NewsBlur and it creates a feed for each incoming source email, which is what I want, the problem is it believes the same email containing an alias is considered as different emails and therefore creates different feeds for same origin.


For instance I am subscribed to a newsletter called hipersonica, they send emails from:


hipersonica@substack.com ← original address


hipersonica+valladolid@substack.com


hipersonica+tier-list@substack.com


hipersonica+buenos-dias@substack.com


hipersonica+marketing@substack.com


etc… all them in the form:


hipersonica+something@substack.com


every time they change the +something part a new feed is being created, if I don´t delete them frequently I end up reaching my feed limit. Same thing happen with others newsletters.


So, is it possible to ignore the +something part, or at least leave that as an option to the user to decide whether they should be treated as different feeds?


[samuelclay](https://forum.newsblur.com/u/samuelclay)


May 13, 2026, 1:16am 2


Ok, I launched a fix for this a few days ago and any email newsletter with the same` List-Id` should coalesce into the same newsletter feed. Let me know how that’s been working out.


1 Like


[3fr](https://forum.newsblur.com/u/3fr)


May 14, 2026, 5:34pm 3


I have been testing it and seems to be working, thanks


1 Like


[greetingsearthling](https://forum.newsblur.com/u/greetingsearthling)


May 16, 2026, 2:56pm 4


[@samuelclay](https://forum.newsblur.com/u/samuelclay) I just wanted to pop by and say, thanks. I think this change resolved a long standing issue I had where all newsletter emails from a large org (e.g.nytdirect@nytimes.com ) were all routed to the same feed even if a different list-id. The behavior I’m seeing now changed not only to merge emails from different senders with the same list-id but also to split emails from the same sender with different list-id. Thank you!


1 Like


[SteveBuscemiDress](https://forum.newsblur.com/u/SteveBuscemiDress)


August 13, 2026, 5:04pm 5


Don’t know if it is related to this, but when I change a newsletter from the ‘Newsletters’ folder (that Newsblur automatically creates) to another folder, as soon as I receive another mail from the same newsletter, a new feed is created again in the ‘Newsletters’ folder.
