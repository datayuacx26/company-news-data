---
schema_version: "1.0.0"
document_id: "9c91f576d5527a77755d48ea04deea186cd5f4898bd175340a9c461ad6d5556f"
company_key: "yc-loops"
company: "Loops"
source_id: "yc-loops-news-import-7f2c6ba5f1f0"
canonical_url: "https://loops.so/engineering/gmail-does-not-like-youtube"
published_at: "2026-01-07T00:00:00+00:00"
first_seen_at: "2026-07-24T09:57:10.497635+00:00"
fetched_at: "2026-07-28T21:58:18.576112+00:00"
content_hash: "sha256:132095cf162790ffc9a30762964ba83aa8e4628e5685337140fd6fa679160dc8"
---

# Gmail doesn’t like YouTube

Recently some of our users have reported that their emails to Gmail contacts are landing in spam with a very scary banner:


Given the sporadic nature of the reporting, our initial suspicion was that someone was abusing our link tracking feature. We provide unique redirect links in marketing emails to log clicks before sending users to the intended destination. But after checking our system, we found no signs of such abuse.


After checking the specific reported emails, we found they all used shortened YouTube share links (like[http://youtu.be/dQw4w9WgXcQ](http://youtu.be/dQw4w9WgXcQ) ) instead of the full link (like[https://www.youtube.com/watch?v=dQw4w9WgXcQ](https://www.youtube.com/watch?v=dQw4w9WgXcQ) ).


Bingo! That was the clue we needed, so we ran some more tests:


Link


Tracking


Placement


Warning


[google.com](http://google.com/)


N


Inbox


No


[google.com](http://google.com/)


Y


Inbox


No


[youtu.be](http://youtu.be/)


N


Inbox


No


[youtu.be](http://youtu.be/)


Y


Spam


Yes


[youtube.com](http://youtube.com/)


N


Inbox


No


[youtube.com](http://youtube.com/)


Y


Inbox


No


We now understood the issue: double-wrapped YouTube links were causing the problem. So to avoid triggering Gmail’s spam filters: either disable link tracking on YouTube share links, or use the full YouTube link instead.


We also received reports that the share link alone was causing mail to land in spam.


To prevent further deliverability problems, we’ve added a warning[to Guardian](https://loops.so/docs/deliverability/youtube-links-warning#shortened-youtube-links-warning) to catch shortened YouTube links.
