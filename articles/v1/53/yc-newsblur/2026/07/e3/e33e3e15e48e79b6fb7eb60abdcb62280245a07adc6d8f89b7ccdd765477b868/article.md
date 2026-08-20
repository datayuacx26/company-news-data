---
schema_version: "1.0.0"
document_id: "e33e3e15e48e79b6fb7eb60abdcb62280245a07adc6d8f89b7ccdd765477b868"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-26fc52334fc9"
canonical_url: "https://forum.newsblur.com/t/good-reads-and-a-redesigned-global-shared-stories/13781"
published_at: "2026-07-17T22:43:33+00:00"
first_seen_at: "2026-07-25T16:10:33.287397+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:070eb874f70ca24ef2f92e4e0b10655d28a1f1e8cdfd234681316160b7d5b8d5"
---

# Good Reads and a redesigned Global Shared Stories

[samuelclay](https://forum.newsblur.com/u/samuelclay)


July 17, 2026, 10:43pm 1


```text
<p>I’m announcing two new global feeds: a redesigned Global Shared Stories and a new feed called Good Reads.</p>


```


### Redesigned Global Shared Stories


Global Shared Stories has been in the sidebar for years, and for years it worked in a way I never liked. It was not the shares of everybody on NewsBlur. It was the shares of the accounts that the @popular account happened to follow, a list I put together by hand a long time ago and rarely touched. If you were on that list and you shared a lot, you decided what everybody else saw. If you shared one story a month and wrote a paragraph about why it mattered, you were drowned out by someone who shared thirty without a word.


So I rebuilt it. Every hour, NewsBlur now gathers every story shared across the whole site in the last few hours, caps each person at three so nobody can flood the pool, drops private blurblogs, and then picks a few worth reading. The picks accumulate, so the river stays deep enough to scroll back through.


The picking is done by Claude Haiku, once an hour, and it is worth being precise about what it does and does not do. It does not go looking for stories. It does not write anything. It only ranks stories that NewsBlur readers already chose to share, and the thing it weighs most heavily is the comment the sharer wrote, because a share with a few sentences attached is a share somebody thought about. It is allowed to pick nothing at all in a quiet hour, and in testing it regularly passes on half of what it is offered. If the API is ever unreachable, a plain heuristic takes over and the river keeps flowing.


### Good Reads


The new feed in the sidebar is **Good Reads** , and it asks a question the other feeds do not: which stories did somebody finish and then do something about?


A story lands in Good Reads when at least two people read it closely, thirty seconds or more, and at least one of them then saved it, shared it, or trained it up. Finishing is not enough. Somebody has to have bothered to act. On top of that, the score is tilted toward feeds with few subscribers, so a story from a site with forty readers can beat a story from a site with forty thousand. That tilt is the whole point. The big sites do not need help getting seen.


### Four feeds, four questions


There are now four curated rivers sitting together in the sidebar, and the reason there are four and not one is that they each answer a different question.


**Global Shared Stories** asks what people chose to hand to someone else. It runs on sharing, a deliberate human act.


**Widely Read Stories** asks what held the most attention across NewsBlur. It runs on reading time, not clicks, so a headline nobody read cannot buy its way in.


**Long Reads** asks what was worth an afternoon. Features and essays that readers gave real time to, rather than skimmed.


**Good Reads** asks what somebody finished and then kept, and leans toward the small sites you have probably never heard of.


Widely Read Stories and Long Reads have been around since April, and I wrote about how they work[when they launched](https://blog.newsblur.com/2026/04/05/widely-read-stories-and-long-reads/) . None of the four ranks by clicks, and none of them is trying to keep you scrolling. They are all built out of what NewsBlur readers actually did with their time.


Because it was never obvious from looking at them which was which, each of the four now explains itself in a line at the top of its story list. It scrolls away with the stories, so it is there when you arrive and gone once you are reading.


Your classifiers still apply to all four. If you have trained a tag, an author, or a site, those green and red scores carry through, including on stories from feeds you do not subscribe to. And all four work as dashboard rivers, so you can park any of them next to your regular feeds.


Good Reads and the rebuilt Global Shared Stories are available now on the web. If a story shows up in one of these feeds that clearly should not have, I want to hear about it on the[NewsBlur forum](https://forum.newsblur.com/) .


---


This is a companion discussion topic for the original entry at[https://blog.newsblur.com/2026/07/17/good-reads-and-a-redesigned-global-shared-stories/](https://blog.newsblur.com/2026/07/17/good-reads-and-a-redesigned-global-shared-stories/)


1 Like


[jaredutah](https://forum.newsblur.com/u/jaredutah)


July 23, 2026, 10:27pm 2


The Train button on the right side of Global Shared Stories doesn’t seem to work. It does in the other three categories.
