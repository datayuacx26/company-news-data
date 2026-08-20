---
schema_version: "1.0.0"
document_id: "4c9992cf0e5dddb3b3136d689530048a423a3ea1affb2f7823830a02a4e4261b"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-26fc52334fc9"
canonical_url: "https://forum.newsblur.com/t/intelligence-trainer-overhaul-url-classifiers-regex-mode-and-manage-all-training-in-one-place/13377"
published_at: "2026-01-22T19:40:57+00:00"
first_seen_at: "2026-07-25T16:10:33.287397+00:00"
fetched_at: "2026-07-28T20:54:41.720106+00:00"
content_hash: "sha256:c4485d834c3061db2a1dde798df512c79d7282fe78ac030e39ddd40129a56ec6"
---

# Intelligence Trainer Overhaul: URL classifiers, regex mode, and manage all training in one place

[samuelclay](https://forum.newsblur.com/u/samuelclay)


January 22, 2026, 7:40pm 1


```text
<p>The Intelligence Trainer is one of NewsBlur’s most powerful features. It lets you train on authors, tags, titles, and text to automatically sort stories into Focus, Unread, or Hidden. But until now, there were limits—you couldn’t train on URLs, regex support was something power users had been requesting for years, and managing hundreds of classifiers meant clicking through feeds one by one.</p>


```


Today I’m launching three major improvements: URL classifiers, regex mode for power users, and a completely redesigned Manage Training tab.


### Train on URLs


You can now train on story permalink URLs, not just titles and content. This opens up new filtering possibilities based on URL patterns.


The URL classifier matches against the full story permalink. Some use cases:


- **Filter by URL path** : Like or dislike stories that contain` /sponsored/` or` /opinion/` in their URL
- **Domain sections** : Match specific subdomains or URL segments that indicate content types
- **Landing pages vs articles** : Some feeds include both—filter by URL structure to show only what you want


URL classifiers support both exact phrase matching and regex mode. The exact phrase match is available to Premium subscribers, while regex mode requires[Premium Pro](https://newsblur.com/?next=premium) .


When a URL classifier matches, you’ll see the matched portion highlighted directly in the story header, so you always know why a story was filtered.


### Regex matching for power users


For years, the text classifier only supported exact phrase matching. If you wanted to match “iPhone” and “iPad” you needed two separate classifiers. Now you can use regex patterns in the Title, Text, and URL classifiers.


A segmented control lets you switch between “Exact phrase” and “Regex” mode. In regex mode, you get access to the full power of regular expressions:


- **Word boundaries** (` \\b` ): Match` \\bapple\\b` to find “apple” but not “pineapple”
- **Alternation** (` |` ): Match` iPhone|iPad|Mac` in a single classifier
- **Optional characters** (` ?` ): Match` colou?r` to find both “color” and “colour”
- **Anchors** (` ^` and` $` ): Match patterns at the start or end of text
- **Character classes** : Match` \[0-9\]+` for any number sequence


A built-in help popover explains regex syntax with practical examples. The trainer validates your regex in real-time and shows helpful error messages if the pattern is invalid.


Regex matching is case-insensitive, so` apple` matches “Apple”, “APPLE”, and “apple”. This mode is available to Premium Pro subscribers.


### Manage all your training in one place


Over the years you may have trained NewsBlur on hundreds of authors, tags, and titles across dozens of feeds. But when you wanted to review what you’d trained, you had to open each feed’s trainer individually and click through them one by one.


The new Manage Training tab provides a consolidated view of every classifier you’ve ever trained, organized by folder. You can see everything at a glance, edit inline, and save changes across multiple feeds in a single click.


Open the Intelligence Trainer from the sidebar menu (or press the` t` key). You’ll now see two tabs at the top: “Site by Site” and “Manage Training”. The Manage Training tab is available everywhere you train—from the story trainer, feed trainer, or the main Intelligence Trainer dialog.


The Site by Site tab is the existing trainer you know—it walks you through each feed showing authors, tags, and titles you can train. That’s still the best way to train new feeds with lots of suggestions.


The Manage Training tab shows only what you’ve already trained. Every thumbs up and thumbs down you’ve ever given, organized by folder just like your feed list. Each feed shows its trained classifiers as pills you can click to toggle.


#### Filtering made easy


The real power comes from the filtering options. At the top of the tab you’ll find several ways to narrow down your training:


**Folder/Site dropdown** — Only folders and sites with training appear in this dropdown. Select a folder to see all training within it, or select a specific site to focus on just that feed’s classifiers. This is especially useful when you have hundreds of trained items and want to review just one area.


**Instant search** — Type in the search box and results filter as you type. Search matches against classifier names, feed titles, and folder names. Looking for everything you’ve trained about “apple”? Just type it and see all matches instantly.


**Likes and Dislikes** — Toggle between All, Likes only, or Dislikes only. Want to see everything you’ve marked as disliked? One click shows you all the red thumbs-down items across your entire training history.


**Type filters** — Filter by classifier type: Title, Author, Tag, Text, URL, or Site. These are multi-select, so you can show just Authors and Tags while hiding everything else. Perfect for when you want to audit just the authors you’ve trained across all your feeds.


#### Edit inline and save in bulk


Click any classifier pill to toggle it between like, dislike, and neutral. The Save button shows exactly how many changes you’ve made, so you always know what’s pending. Made a mistake? Just click again to undo—the count updates automatically.


When you click Save, all your changes across all feeds are saved in a single request. No more clicking through feeds one at a time to clean up old training.


### Subscription tiers


Feature Tier Required


Title/Author/Tag/Feed classifiers Free


Manage Training tab Free


URL classifiers (exact phrase) Premium


Text classifiers (exact phrase) Premium Archive


Regex mode (Title, Text, URL) Premium Pro


All three features are available now on the web. If you have feedback or ideas for improvements, please share them on the[NewsBlur forum](https://forum.newsblur.com/) .


---


This is a companion discussion topic for the original entry at[https://blog.newsblur.com/2026/01/22/intelligence-trainer-overhaul/](https://blog.newsblur.com/2026/01/22/intelligence-trainer-overhaul/)


2 Likes


[Allow Intelligence Trainer to filter by URL](https://forum.newsblur.com/t/allow-intelligence-trainer-to-filter-by-url/5761/15)


[Url match - (Training Tool)](https://forum.newsblur.com/t/url-match-training-tool/7863/9)


[MikeC](https://forum.newsblur.com/u/MikeC)


January 23, 2026, 4:18am 2


Great new features.


Have you maybe broken the removing of a training keyword by mistake? In the new section and in each feed. If I right click on a feed and then have something in red marked with a thumbs down, if I want to remove that I’d normally click twice so it cycles through green and then to grey and then press save training. When you do this and go back in it is still there.


I tested adding a new keyword from the subject to training and then removing that and that worked OK, so its just removing existing training it seems broken.


One thing that I noticed was when looking at Manage Training it still had all the training for feeds I have deleted in the past. Is this intentional that this is kept incase you re-add it later or just an overside to remove the training on a feed when it is removed? I tried to remove them without success but could be related to the above.


The Training dialog feels a little big by default, I have a MBP and use Chrome. I have the Bookmarks bar visible and it looks like this (circled in Red where its not fitting) :


[trainingwindowtoobig 2928×1920 430 KB](https://forum.newsblur.com/uploads/default/original/2X/d/d4bc47143e938413bae127877cbdd1bfc3ef35f1.jpeg)


[samuelclay](https://forum.newsblur.com/u/samuelclay)


January 23, 2026, 4:29am 3


Those previously deleted classifiers are now visible on the Manage Training tab, and it’s probably related that they can’t be deleted. I’ll get that fixed tomorrow. What’s your username? Feel free to DM it to me.


[Chris_Minett](https://forum.newsblur.com/u/Chris_Minett)


January 29, 2026, 11:53am 4


Awesome feature for the URL filtering, although I’m finding that it doesn’t appear to apply to articles in the iOS app.


I use it for site feeds which cannot filter on their own categories, and do not have tags. I set a ‘dislike’ for the publisher so all articles are hidden as standard, and then set a ‘like’ for the desired category path in the URL, so only those articles are shown in Newsblur. Works perfectly in the web. On iOS, the feed list shows an unread article, but clicking into the feed shows zero articles.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


January 29, 2026, 1:38pm 5


We’re working hard on getting the iOS app updated. It’s a huge rewrite and I’m spending all day every day on getting it done. It’s still in progress but know that it’ll be out asap.


2 Likes


[samuelclay](https://forum.newsblur.com/u/samuelclay)


January 30, 2026, 7:46pm 6


Thanks for reporting this! I tracked down the bug and it’s now fixed.


The removal bug: When you cycled a classifier to gray (neutral) and saved, there was a code path where the classifier got saved with a score of 0 instead of being deleted. This happened specifically when there were duplicate classifier records in the database. The duplicate handling code would save the first one with score=0 and skip the deletion step. So the classifier would persist and keep showing up even after you removed it.


Deleted feed classifiers: Those are intentionally kept around. If you re-subscribe to a feed, your training comes back with it. But the removal bug was preventing you from clearing them out through the Manage Training tab. With the fix deployed, cycling to gray and saving will now properly remove them.


[MikeC](https://forum.newsblur.com/u/MikeC)


January 31, 2026, 4:15am 7


[@samuelclay](https://forum.newsblur.com/u/samuelclay) thanks but I think it could still be broken I’ll send you a DM with a video link showing what I’m doing/seeing still. Its only applying to the removed sites, so probably not a massive thing. I’m not phased about it being there still, not like I’m gonna go in there often to check stuff.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


January 31, 2026, 5:30am 8


Ohhh, haha, Claude pointed out that there’s a bunch of classifiers on deleted sites and I told it to keep them, they might come in handy. Clearly the wrong decision. I’ll get that fixed soon!


[undead](https://forum.newsblur.com/u/undead)


January 31, 2026, 5:41am 9


How to apply a filter globally?
And do we we have something like super thumb down, to NEVER bubble up content with specific labels? I mean, normal filtering stuff, expected from sane filtering system.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


January 31, 2026, 4:14pm 10


Global filters are a great idea and something I would only consider for the premium pro tier. I might build that this week.


1 Like


[VlaKor1](https://forum.newsblur.com/u/VlaKor1)


February 11, 2026, 10:52pm 11


Hello! For self-hosting, does the Regex function work the same way?


[samuelclay](https://forum.newsblur.com/u/samuelclay)


February 11, 2026, 10:53pm 12


Yep, regex is a premium pro feature while global/folder classifiers are a premium archive feature. On self hosted you should automatically be a premium pro.


1 Like


[VlaKor1](https://forum.newsblur.com/u/VlaKor1)


February 12, 2026, 9:10am 13


[изображение 1063×798 163 KB](https://forum.newsblur.com/uploads/default/original/2X/7/777a49700a768849f58c219e5f78e5c94797652e.jpeg)


For some reason, it isn’t active.


[VlaKor1](https://forum.newsblur.com/u/VlaKor1)


February 12, 2026, 9:13am 14


But in the filter interface I can use Regex, but I’m not sure this function works.
