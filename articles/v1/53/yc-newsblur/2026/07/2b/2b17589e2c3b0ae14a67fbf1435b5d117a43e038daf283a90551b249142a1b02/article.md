---
schema_version: "1.0.0"
document_id: "2b17589e2c3b0ae14a67fbf1435b5d117a43e038daf283a90551b249142a1b02"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-0646845e5ccf"
canonical_url: "https://forum.newsblur.com/t/reddit-forum-stories-going-to-url-in-thread-rather-than-thread-itself/13752"
published_at: "2026-07-03T22:38:08+00:00"
first_seen_at: "2026-07-25T16:10:32.307561+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:b43266036970bb94497936aa9ec58495355f562aeadfcfae264ed73be98c65a7"
---

# Reddit Forum Stories going to url in thread, rather than thread itself

[Jason3](https://forum.newsblur.com/u/Jason3)


June 25, 2026, 4:25pm 1


[https://www.reddit.com/r/cincinnati/.rss](https://www.reddit.com/r/cincinnati/.rss)


When a given reddit thread has a link or a photo at the top of the thread, clicking on that thread in Newsblur goes to the link or photo and not the reddit thread itself.


[Jason3](https://forum.newsblur.com/u/Jason3)


June 25, 2026, 6:49pm 2


For more context, the URL seen while hovering over a normal Reddit thread shows the full URL to that reddit thread ([www.reddit.com](http://www.reddit.com/) etc) but if the thread has a photo or URL first in the thread, the hover over URL is the link to that photo (i.redd.it/blahblahblah.jpg) or the URL of the link in the thread, not the thread itself.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


July 3, 2026, 7:45pm 3


Fixed. Reddit feed items now link to the Reddit comment thread, while the external link or image still appears inside the story body.


Commit:[https://github.com/samuelclay/NewsBlur/commit/e8b33a665338109cb148ec313c11bf183da14442](https://github.com/samuelclay/NewsBlur/commit/e8b33a665338109cb148ec313c11bf183da14442)


[Jason3](https://forum.newsblur.com/u/Jason3)


July 3, 2026, 10:38pm 4


Thanks!


1 Like
