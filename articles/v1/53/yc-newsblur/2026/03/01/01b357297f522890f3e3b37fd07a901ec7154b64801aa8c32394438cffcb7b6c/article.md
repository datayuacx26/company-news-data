---
schema_version: "1.0.0"
document_id: "01b357297f522890f3e3b37fd07a901ec7154b64801aa8c32394438cffcb7b6c"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-26fc52334fc9"
canonical_url: "https://forum.newsblur.com/t/story-clustering-automatically-group-duplicate-stories-across-your-feeds/13512"
published_at: "2026-03-18T15:03:37+00:00"
first_seen_at: "2026-07-25T16:10:33.287397+00:00"
fetched_at: "2026-07-28T21:56:54.694470+00:00"
content_hash: "sha256:4fc6f7ab5382183f179b3bbb600c9210dd6c307d32bf8dd115b5d7a9fe04d607"
---

# Story clustering: automatically group duplicate stories across your feeds

[samuelclay](https://forum.newsblur.com/u/samuelclay)


March 18, 2026, 3:03pm 1


```text
<p>If you subscribe to more than a handful of news feeds, you’ve hit this problem: a story breaks, and suddenly the same headline appears across five, ten, twenty of your subscriptions. You’re reading the same article over and over, just published by different outlets. Your river view fills up with duplicates, and the stories you haven’t read yet get buried.</p>


```


Story clustering solves this. When NewsBlur detects that multiple feeds are covering the same story, it groups them together and shows you the highest-scoring version. The duplicates don’t disappear – they fold neatly underneath, so you can still see who else reported it and jump to their version if you want a different perspective.


### How it works


In the story titles list, clustered stories show their sources directly below the representative story. Each source shows the feed’s favicon, feed name, story title, and how long ago it was published. Click any source to read that version instead.


When you open a clustered story, the detail view shows rich cards for each alternative source at the bottom. These cards include the feed icon, story title, a content preview, the article’s thumbnail image, author, and date. Click any card to jump to that version of the story.


### Two layers of detection


Clustering uses two complementary approaches to catch duplicates:


**Title matching** is the fast, obvious check. NewsBlur normalizes story titles (lowercasing, stripping punctuation) and groups exact matches. But it also does fuzzy matching using significant-word overlap – so “Apple Announces New iPhone” and “Apple Reveals the New iPhone at WWDC” will still cluster together, even though the titles aren’t identical.


**Semantic matching** goes deeper. NewsBlur sends each story’s title to Elasticsearch’s more_like_this query, searching across all your subscribed feeds for articles covering the same topic. This catches stories that are about the same event but written with completely different headlines. The two layers are merged, so title matches and semantic matches combine into a single cluster.


Clustering runs automatically in the background every time a feed updates. Results are cached for 14 days, so clusters are ready instantly when you load your river.


### Mark duplicates as read


When you read a clustered story, you can optionally have NewsBlur mark all the duplicates as read too. This is off by default – enable it in the feed options popover under “Story Clustering” or in Manage > Preferences > Stories.


There are two controls:


- **Cluster related stories / Keep stories separate** – Toggles clustering on or off. When enabled, duplicate stories are grouped in your river view. When disabled, every story appears individually as before.
- **Mark all as read / Keep others unread** – When you read the representative story, this controls whether the other stories in the cluster are automatically marked as read.


The same options are available in the global Preferences dialog under the Stories tab.


### Availability


Story clustering is available to all NewsBlur users on the web. If a feed you subscribe to has cluster data, you’ll see grouped stories automatically – no configuration needed. Clustering is now enabled by default for all users, and can be toggled off or back on in your account Preferences.


**Premium Archive subscribers** get full control over clustering: choose between single-line and expanded preview styles, and automatically mark duplicate stories as read when you read the representative story.


**Premium and free users** see clustered stories on popular feeds where cluster data already exists. You’ll see clusters most often on widely-subscribed news feeds. To unlock clustering settings and get clustering across all your feeds,[upgrade to Premium Archive](https://newsblur.com/?next=premium) .


If you have feedback or ideas for improvements, please share them on the[NewsBlur forum](https://forum.newsblur.com/) .


---


This is a companion discussion topic for the original entry at[https://blog.newsblur.com/2026/03/18/story-clustering/](https://blog.newsblur.com/2026/03/18/story-clustering/)


1 Like


[My feeds are getting all mixed up](https://forum.newsblur.com/t/my-feeds-are-getting-all-mixed-up/13520/2)


[Ike582](https://forum.newsblur.com/u/Ike582)


March 18, 2026, 4:17pm 2


Samuel, Story clustering sounds like a great feature. Unfortunately I don’t see that option anywhere in my Preferences settings. I’m currently using the web browser version on Safari. Any idea how to find it?


Thanks


[samuelclay](https://forum.newsblur.com/u/samuelclay)


March 18, 2026, 4:33pm 3


Make sure you reload the page so that you load the latest code. You’ll see it automatically on any stories that get clustered, since it’s automatically turned on for all users. If you’re not a Premium Archive subscriber, then you won’t see it on as many of your feeds, because you’re only benefiting from other Premium Archive users who are subscribed to the same feeds as you are.


That said, you can see the options under the feed options pop over, which is in the top right of the screen when you open a feed, and you can configure it there. You can also enable and disable it in the global preferences dialog.


[Ike582](https://forum.newsblur.com/u/Ike582)


March 18, 2026, 4:47pm 4


Reloading the page was the key, now I see it. Thanks!


[frga](https://forum.newsblur.com/u/frga)


March 18, 2026, 5:26pm 5


Great news and great feature! At least in one instance, though, I’ve noticed that the related story is not actually related (see attached screenshot). I guess the feature is not yet perfect, I just wonder why that happens. Great job otherwise!


[Screenshot 2026-03-18 171013 929×712 77 KB](https://forum.newsblur.com/uploads/default/original/2X/d/d78312deb305305061adbbaa4206927b44378686.png)


[GinnyMaive](https://forum.newsblur.com/u/GinnyMaive)


March 18, 2026, 5:39pm 6


This is awesome! Reading the announcement as a Premium user I was a little nervous about this feature not being able to toggle off (I’m going to try it, but it’s important to me that I’d be able to turn it off if I did not want to keep using it).


I do see in my settings I can do that – this is great, and thank you for not forcing a feature as is the way of our times. I just have a nitpick on the announcement that made me initially suspect I wouldn’t have this control:


> Premium Archive subscribers get **full control over clustering: toggle it on or off,** choose between single-line and expanded preview styles, and automatically mark duplicate stories as read when you read the representative story. Clustering is enabled by default for archive subscribers.
>
>
> **Premium and free users see clustered stories on popular feeds where cluster data already exists.** You’ll see clusters most often on widely-subscribed news feeds. To unlock clustering settings and get clustering across all your feeds, upgrade to Premium Archive.


Just reading this as-is, perhaps with a pessimistic mindset, this implies (to me at least) that control on/off is a feature/setting only Premium Archive subscribers get since it’s mentioned there but not below.


I know this is a nitpick and easily enough I can just see if the setting is there myself, but I am pedantic about communication lol. A strawman edit:


> Story clustering is available to all NewsBlur users on the web. If a feed you subscribe to has cluster data, you’ll see grouped stories automatically – no configuration needed. **Clustering is now enabled by default for all users, and can be toggled off or back on in your account Preferences.**
>
>
> Premium Archive subscribers get full control over clustering: **toggle it on or off,** choose between single-line and expanded preview styles, and automatically mark duplicate stories as read when you read the representative story. **Clustering is enabled by default for archive subscribers.**
>
>
> Premium and free users see clustered stories on popular feeds where cluster data already exists. You’ll see clusters most often on widely-subscribed news feeds. To unlock clustering settings and get clustering across all your feeds, upgrade to Premium Archive.


(Emphasis added to my edits.)


BTW, thanks for giving Premium users access to this feature where the data already exists. Very cool and probably a good way to upsell us eventually too


[samuelclay](https://forum.newsblur.com/u/samuelclay)


March 18, 2026, 6:21pm 7


Thanks for editing, I’ll make those changes. And you’ll be able to see them with the “Show Story Changes“ button.


[Screenshot 2026-03-18 at 11.20.48 AM 1294×700 101 KB](https://forum.newsblur.com/uploads/default/original/2X/e/e61a2cadc02f7980d053b8812d530f15e41512d1.png)


[mkucek](https://forum.newsblur.com/u/mkucek)


March 18, 2026, 6:22pm 8


First, this is absolutely fantastic. I’ve encountered a bug, and I think I know what’s happening.


Here is a cluster in my News folder. Semafor is the “main” story and the Engadget one is in my Tech folder.


[image 585×225 45.3 KB](https://forum.newsblur.com/uploads/default/original/2X/9/9ecd1dd1fc2f5b6e783bb2531147d7e6c67e31a5.png)


If I click the Engadget story to view their coverage, the browser is redirected to the Engadget feed in the Tech folder (I can see the URL and the selection UI in the sidebar), but the story does not open.


[image 2075×1005 203 KB](https://forum.newsblur.com/uploads/default/original/2X/3/3fae313360fd0fe509c0eb1ba71cd9f62bebdef9.jpeg)


In my limited testing, if the clustered story is in the same folder, it opens correctly.


[andyhat2](https://forum.newsblur.com/u/andyhat2)


March 18, 2026, 7:31pm 9


Seems the clustering thinks all the feeds from[comicsrss.com](http://comicsrss.com/) are the same, even though each is a distinct comic strip. I guess it doesn’t look at the image content at all?


[image 997×726 146 KB](https://forum.newsblur.com/uploads/default/original/2X/0/06cfad7998bd6ffa0ad6bcc5175f18466c49a31b.jpeg)


[samuelclay](https://forum.newsblur.com/u/samuelclay)


March 18, 2026, 8:39pm 10


Very good feedback - thanks for showing me these screenshots. I’ve adjusted the algorithm and just deployed it. It should be a little stricter about matching stories up that shouldn’t match (i.e., false positives), while still maintaining the core clustering ability.


[mkucek](https://forum.newsblur.com/u/mkucek)


March 18, 2026, 8:45pm 11


One more small (I think) bug:


This cluster of stories came first in my feed:


[image 766×585 109 KB](https://forum.newsblur.com/uploads/default/original/2X/5/52a99e3a0d8f11e85d3db749e16bbd6533a1ac0a.jpeg)


As I scrolled, each was marked read. I’ll call this group A.


Later down in the folder, the article appeared again, say group B.


The primary group B story was already marked as read since it appeared as story A1. Primary A was sorted as B2.


[image 748×657 106 KB](https://forum.newsblur.com/uploads/default/original/2X/a/a0873d855a0d141f4744a074a554cfafc3b3a54e.jpeg)


I would personally expect all the stories to be rolled up and appear exactly once. Instead, in my example, the articles will appear 5 times each – once as the primary and four times as a part of the cluster.


[walterbishop](https://forum.newsblur.com/u/walterbishop)


March 18, 2026, 10:43pm 12


Love the new feature. This works really well with news feeds that are very similar. I have two that overlap 30% of the time.


[huWkffKNBo](https://forum.newsblur.com/u/huWkffKNBo)


March 19, 2026, 11:27am 13


I’ve refreshed Newsblur but I still don’t see anything in the global preferences menu (but I do see clustering in my feed, and I can see the per-feed clustering settings options).


For some reason, the only clusters I’ve seen so far have been dupes. E.g. here, before-click:


[image 1670×923 141 KB](https://forum.newsblur.com/uploads/default/original/2X/c/c878eb598c3413793e8b5385cd46595b37d3393b.jpeg)


…and here, after-click:


[image 1670×923 95.5 KB](https://forum.newsblur.com/uploads/default/original/2X/8/89f2993f4307e1563fca56fae3c33d45884da735.jpeg)


I wondered if maybe this was because I had the feed stored in multiple folders and maybe it was treating each feed+folder combo as a different entity, but this particular feed ([http://feeds.bbci.co.uk/news/world/rss.xml](http://feeds.bbci.co.uk/news/world/rss.xml) ) is only in one folder so that’s not it


[leonick](https://forum.newsblur.com/u/leonick)


March 19, 2026, 7:51pm 14


1. Maybe the clusters shouldn’t contain stories that are from sites you subscribe to and have already read?
2. If they do include that then the stories should look read in the cluster, currently they are styled like an unread item.
3. If you click a cluster item that is read and the feed it is part of is set to show unread only, you’ll be sent to the feed in question but the story won’t show.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


March 19, 2026, 8:50pm 15


Yeah, clusters should absolutely display stories that you’ve already read. That is by design; that’s how story clustering works and how you know it’s working. When you mark a story as read, if you are a premium archive, then you can mark all those clusters as read as well, but they’re still going to show up. If you don’t want them to show up, then turn off the story clustering feature entirely.


As for previously read stories showing up as unread in the story cluster, that’s a bug, but I’ll say that I built it to show the correct read status, so if that’s something I’m not aware of yet.


The third point is a bug. You are correct. I thought there would be. I can probably build it in so that it searches for the story if it can’t find it by scrolling, like I do for other story finding algorithms.


1 Like


[richard4339](https://forum.newsblur.com/u/richard4339)


March 20, 2026, 3:57am 16


I like the clustering, but I honestly find the UI for it very distracting. It draws your attention to the cluster and away from all other stories. Would you consider letting us collapse them by default and show an indicator that there are collapsed stories below it?


[Nrbelex](https://forum.newsblur.com/u/Nrbelex)


April 3, 2026, 3:53am 17


[@samuelclay](https://forum.newsblur.com/u/samuelclay) will clustering be coming to the mobile apps? Thanks!


[samuelclay](https://forum.newsblur.com/u/samuelclay)


April 3, 2026, 3:57am 18


Clustering just launched on iOS today and is in the review queue for the Android app and should be out in the next day or two.


1 Like


[nelas](https://forum.newsblur.com/u/nelas)


April 9, 2026, 11:44am 19


Hello, I’m finding the story clustering algorithm too greedy, often clustering stories that are not duplicates. Here’s an example from academic article feeds:


[image 1182×381 85.8 KB](https://forum.newsblur.com/uploads/default/original/2X/9/98e83f5ffa00263845e22cc59e748c0843df4de9.png)


They are related to the same topic (single-cell), but not duplicates. Is story clustering meant to find duplicates or related stories? Happy to give you the specific feeds for testing.


Cheers,
Bruno


[andyhat2](https://forum.newsblur.com/u/andyhat2)


April 16, 2026, 4:53pm 20


Another example of 3 different stories getting incorrectly clustered:


[image 979×407 31.2 KB](https://forum.newsblur.com/uploads/default/original/2X/5/5422d4ef4eff54ac54824ba598a9f915c642c701.png)
