---
schema_version: "1.0.0"
document_id: "21589e7ac26b1cbba37a42c2d563158a64bcb7e3abb1110becdb47fbce5d026f"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-26fc52334fc9"
canonical_url: "https://forum.newsblur.com/t/add-discover-sites-youtube-reddit-podcasts-newsletters-and-thousands-of-feeds-to-explore/13433"
published_at: "2026-03-04T17:56:26+00:00"
first_seen_at: "2026-07-25T16:10:33.287397+00:00"
fetched_at: "2026-07-28T20:53:35.081205+00:00"
content_hash: "sha256:9c36805c863884fd2a790b47dbdf9d468687bac176dc8c01070299659ef2bed4"
---

# Add + Discover Sites: YouTube, Reddit, podcasts, newsletters, and thousands of feeds to explore

[samuelclay](https://forum.newsblur.com/u/samuelclay)


March 4, 2026, 5:56pm 1


```text
<p>NewsBlur has always been great at reading feeds. But finding new ones? That was mostly on you. The old “Add Site” dialog was a search box and not much else. If you already had a feed URL, it worked fine. If you were looking for something new to read, you were on your own.</p>


```


The new **Add + Discover Sites** page changes that. It’s a full-page discovery experience with eight tabs covering YouTube channels, Reddit communities, podcasts, newsletters, Google News topics, trending sites, popular feeds, and of course the classic search-and-subscribe workflow. There are over 50,000 curated feeds to browse, all organized into dozens of categories and subcategories.


### Eight ways to find feeds


The tab bar across the top gives you eight different lenses into the world of RSS:


-


**Search** — The classic search bar, now with semantic search and autocomplete. Type a topic or URL and get instant suggestions. Below the search results you’ll find trending feeds ranked by a hybrid algorithm that combines subscription velocity, read engagement, and subscriber counts.


-


**Web Feed** — Create RSS feeds from any website. This one gets its own blog post.


-


**Popular Sites** — Thousands of curated RSS feeds organized into categories like Technology, Science, News, and Business. Each category has subcategories for drilling down further.


-


**YouTube** — Over 2,000 verified YouTube channels converted to RSS feeds. Browse by category or search for specific channels. Subscribe and read YouTube in your feed reader the way it should be.


-


**Reddit** — Nearly 6,000 real subreddits across 47 categories. From r/programming to r/sourdough, you can subscribe to any subreddit as an RSS feed.


-


**Newsletters** — Newsletters from Substack, Medium, Ghost, Beehiiv, and other platforms. Platform pills let you filter by newsletter provider if you have a preference.


-


**Podcasts** — Popular podcasts organized by genre. Search for shows or browse the curated collection.


-


**Google News** — Eight preset topics (World, Business, Technology, Sports, and more) that create feeds from Google News. One click to subscribe.


### Categories and subcategories


Most tabs are organized with a two-level taxonomy. Click a category pill at the top to filter, then drill into subcategories for more specific browsing. YouTube’s Technology category, for example, breaks down into Programming, AI & Machine Learning, Gadgets, and more.


The categories are consistent across tabs where it makes sense, so you can explore Technology feeds across YouTube, Reddit, Popular Sites, and Podcasts without having to rethink the navigation each time.


### Grid view and list view


Every tab supports two viewing modes. Grid view shows feed cards with thumbnails, descriptions, subscriber counts, and freshness indicators. List view compresses things into a denser layout when you want to scan quickly.


A style popover in the top right lets you toggle between views. Your preference is saved per tab.


### Try before you subscribe


Every feed card has a **Try** button that instantly fetches the feed and shows you the actual stories. No commitment, no subscribing. Just a quick look at what you’d get. If you like what you see, the subscribe button is right there with a folder picker.


A breadcrumb link at the top takes you back to where you were browsing when you’re done previewing.


### The new Add Site popover


If you don’t need the full discovery page, the popover that appears when you click “+” in the sidebar has been redesigned too. It still has the quick URL input for when you have a feed address handy, but now it also shows freshness indicators and has buttons to jump into any of the discovery tabs.


### Semantic search


The search tab uses Elasticsearch to find feeds by name with fuzzy matching. Type “cooking” and you’ll get cooking blogs, YouTube cooking channels, cooking subreddits, and cooking podcasts. It searches across all feed types, not just traditional RSS. If Elasticsearch doesn’t find anything, the search falls back to a database query so you’ll always get results.


### Where all these feeds came from


Building the discovery page meant curating a lot of feeds. I wrote management commands to discover and verify channels, subreddits, podcasts, and newsletters from real sources. The collection includes over 2,000 YouTube channels, 6,600 subreddits, 7,300 newsletters, 32,000 podcasts, and 14,000 RSS feeds. Over 63,000 feeds in total, all real, verified, and categorized.


The Add + Discover Sites page is available now on the web for all users. If you have feedback or ideas for new categories, platforms, or features, please share them on the[NewsBlur forum](https://forum.newsblur.com/) .


---


This is a companion discussion topic for the original entry at[https://blog.newsblur.com/2026/03/04/add-and-discover-sites/](https://blog.newsblur.com/2026/03/04/add-and-discover-sites/)


4 Likes


[B111](https://forum.newsblur.com/u/B111)


March 4, 2026, 9:18pm 2


Great update had been hoping to get the built-in RSS feed builder, trending sources, etc. Look forward to utilizing.


I’m not certain if this is a bug or not, but any Google News feed just gives me a cookie message:


[image 3019×1234 410 KB](https://forum.newsblur.com/uploads/default/original/2X/a/ae84225bb857a48f7229eecf4f081bee56118102.png)


[viq](https://forum.newsblur.com/u/viq)


March 8, 2026, 12:47am 3


Also, is there maybe a way to change the article links to direct ones, not going through Google News ?


[BeingTheWriter](https://forum.newsblur.com/u/BeingTheWriter)


March 29, 2026, 5:38pm 4


This is an exciting update!!! I’m so glad I thought to check Newsblur this morning (usually I scroll through the app and miss this). I did notice when looking up podcasts that they tend to bring up shows that really haven’t been updated in a while especially when looking up a particular type (i.e. careers). Also that banner is really in the way while searching. And - one more thing - it seems like it attempts to bring up keywords (like job searching) than quickly goes to zero found.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


March 29, 2026, 10:44pm 5


When I launched this feature, I attempted to load every podcast just to get the stale date. If you’re seeing a lot of stale feeds, that is entirely possible. The point is that for most users, the archive is still handy, so even if they’re not publishing, the archive is a treasure trove of goodies. Because I fetched all the feeds and have a stale date on them, that way you can actually tell at a quick glance if they’re going to be fresh. If you have any specific queries that you know are turning up keywords that are performing poorly, let me know. I’d love to investigate.


[goliathonline](https://forum.newsblur.com/u/goliathonline)


April 28, 2026, 6:22pm 6


I’m having trouble pulling actual feeds from Beehiiv newsletters. E.g.,[https://lamaterial.com/](https://lamaterial.com/) and[https://www.doomsdayscenario.co/](https://www.doomsdayscenario.co/) . It wants to do web feeds for them but surely there’s a way to do actual newsletter integrations, right?
