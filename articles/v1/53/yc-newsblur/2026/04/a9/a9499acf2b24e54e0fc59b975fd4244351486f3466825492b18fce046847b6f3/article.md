---
schema_version: "1.0.0"
document_id: "a9499acf2b24e54e0fc59b975fd4244351486f3466825492b18fce046847b6f3"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-26fc52334fc9"
canonical_url: "https://forum.newsblur.com/t/widely-read-stories-and-long-reads-surfacing-stories-worth-your-time/13600"
published_at: "2026-04-06T03:56:31+00:00"
first_seen_at: "2026-07-25T16:10:33.287397+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:873d8ab9de12267b8f3416711c1169b002598ef5c1f35b1bf883ecea63842cdf"
---

# Widely Read Stories and Long Reads: surfacing stories worth your time

[samuelclay](https://forum.newsblur.com/u/samuelclay)


April 6, 2026, 3:56am 1


```text
<p>There are two questions I keep coming back to when I open my reader: what are other people reading right now, and what’s worth setting aside time for? Not what’s trending on social media or what an algorithm thinks will get clicks. What are actual readers spending their time on?</p>


```


Two new feeds in the sidebar answer those questions: **Widely Read Stories** and **Long Reads** . Both are ranked by actual reading time rather than clicks or social shares.


### Widely Read Stories


This feed collects stories that three or more NewsBlur readers have spent meaningful time with over the past week. “Meaningful time” means at least 3 seconds of actual reading, so quick scrolls and accidental opens don’t count. The list is refreshed every hour and stories are sorted by publish date, so you see the latest widely read articles first.


Think of it as a window into what the NewsBlur community is reading right now. It’s not an algorithm trying to maximize engagement. It’s just the stories that real people are spending their time on.


### Long Reads


The Long Reads feed takes a different angle. Instead of counting readers, it ranks stories by average time spent per reader. A story that five people each read for four minutes ranks higher than a story that fifty people glanced at for thirty seconds. This naturally surfaces longer, deeper articles that reward sustained attention.


### Your classifiers apply here too


These aren’t just raw lists of stories. When you open Widely Read Stories or Long Reads, your trained classifiers run against every story. Tags, authors, titles, and text classifiers all apply, so stories from feeds you’ve trained show up with the same green and red intelligence scoring you see everywhere else in NewsBlur. If you’ve trained a tag as interesting or an author as disliked, those scores carry through into the trending feeds.


### Dashboard support


Both feeds are available as dashboard river options. Open your dashboard settings, pick “Widely Read Stories” or “Long Reads” from the river dropdown, and the feed appears as a panel alongside your other dashboard rivers.


### Availability


Widely Read Stories and Long Reads are available now on the web. Look for them in the sidebar below Global Shared Stories. If you have feedback or ideas, please share them on the[NewsBlur forum](https://forum.newsblur.com/) .


---


This is a companion discussion topic for the original entry at[https://blog.newsblur.com/2026/04/05/widely-read-stories-and-long-reads/](https://blog.newsblur.com/2026/04/05/widely-read-stories-and-long-reads/)


[cmhotb](https://forum.newsblur.com/u/cmhotb)


April 6, 2026, 11:47am 2


Good morning. Can we have an option to hide “Widely Read”, “Long Reads”, and “Add + Discover” from the left column from the web reader (and the iPhone if that’s coming too in a future release)? I like a minimalistic look…see what I want to see, hide what I don’t. Hope that makes sense. Thanks, Samuel.


2 Likes


[MikeC](https://forum.newsblur.com/u/MikeC)


April 6, 2026, 12:09pm 3


You’re like me … I prefer the minimalist look so have already got this configured.


You can hide them with custom CSS, on the web, click on the Gear, Account, Custom CSS/Javascript and add:


> /* Hide Add + Discover Sites */
> .NB-feeds-header-container.NB-feeds-header-river-container.NB-feeds-header-add-site-container {
> display: none !important;
> }
>
>
> /* Widely Read Stories */
> .NB-feeds-header-container.NB-feeds-header-river-container.NB-feeds-header-river-well-read-container.NB-sticky-sidebar-section {
> display: none !important;
> }
>
>
> /* Hide Long Reads */
> .NB-feeds-header-container.NB-feeds-header-river-container.NB-feeds-header-river-long-reads-container.NB-sticky-sidebar-section {
> display: none !important;
> }


[cmhotb](https://forum.newsblur.com/u/cmhotb)


April 6, 2026, 3:22pm 4


That worked….thank you!!!


[samuelclay](https://forum.newsblur.com/u/samuelclay)


April 7, 2026, 4:09am 5


I just deployed the actual fix. You can now turn it off without custom CSS.


[Screenshot 2026-04-06 at 9.08.37 PM 1414×416 32.7 KB](https://forum.newsblur.com/uploads/default/original/2X/7/72ef7648774b026c41ec90ed8641e6b540d4b201.png)


3 Likes


[xref](https://forum.newsblur.com/u/xref)


June 6, 2026, 6:43pm 6


Will these eventually come to the mobile apps?


1 Like
