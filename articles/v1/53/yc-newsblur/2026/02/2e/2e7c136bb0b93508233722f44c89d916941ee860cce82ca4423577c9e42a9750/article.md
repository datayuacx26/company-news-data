---
schema_version: "1.0.0"
document_id: "2e7c136bb0b93508233722f44c89d916941ee860cce82ca4423577c9e42a9750"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-26fc52334fc9"
canonical_url: "https://forum.newsblur.com/t/global-and-folder-scoped-intelligence-training-train-once-apply-everywhere/13404"
published_at: "2026-02-05T17:31:32+00:00"
first_seen_at: "2026-07-25T16:10:33.287397+00:00"
fetched_at: "2026-07-28T22:21:32.388193+00:00"
content_hash: "sha256:8d9d6df1f1edbfe14a4db731df09a0b140c7d9934fac343413f8af4d7d8385c4"
---

# Global and folder-scoped intelligence training: Train once, apply everywhere

[samuelclay](https://forum.newsblur.com/u/samuelclay)


February 5, 2026, 5:31pm 1


```text
<p>Until now, the Intelligence Trainer was strictly per-feed. Train a title, author, or tag on one site and it only affected that site. If you wanted to hide a topic everywhere, you had to repeat that training on each feed. With a few feeds, that’s fine. With a hundred, it’s tedious. With five hundred, it’s a non-starter.</p>


```


If you’re a[Premium Archive](https://newsblur.com/?next=premium) subscriber, you can now set any classifier to apply globally across all your feeds, or scoped to a specific folder. Train “sponsored” as a dislike once, and it hides sponsored stories everywhere. Train “kubernetes” as a like in your Tech folder, and it highlights kubernetes stories across every feed in that folder without touching the rest of your subscriptions.


### Three scope levels


Every classifier pill in the Intelligence Trainer now shows three small scope icons on the left: a feed icon, a folder icon, and a globe icon.


- **Per Site** (feed icon) — The default. The classifier only applies to the feed you’re training. This is how classifiers have always worked.
- **Per Folder** (folder icon) — The classifier applies to every feed in the same folder. If you later move the feed to a different folder, the classifier stays tied to the original folder.
- **Global** (globe icon) — The classifier applies to every feed you subscribe to.


Click any scope icon to switch. The active scope is highlighted, and a tooltip explains each level. Your choice is saved with the classifier.


### Real-world examples


**Hide a topic everywhere.** Subscribe to lots of news feeds but never want to read about a recurring topic? Open the trainer on any feed, add the topic as a text or title classifier, thumbs-down it, and click the globe icon. Done — it’s hidden across all your feeds.


**Focus on a topic within a folder.** Have a “Tech” folder with 40 feeds? Train “machine learning” as a like with the folder scope, and every feed in that folder will surface machine learning stories in your Focus view. Your cooking and sports feeds stay untouched.


**Dislike a prolific author.** Some authors are syndicated across multiple sites. Instead of training the same author name on each feed, set it to global and it applies everywhere at once.


### Manage Training scope filter


The Manage Training tab now includes a scope filter alongside the existing sentiment, type, and search filters. You can quickly see all your global classifiers, all your folder-scoped classifiers, or narrow down to just per-site training.


Each classifier pill in the Manage Training list also shows a small colored scope badge, so you can tell at a glance whether a classifier is site-level, folder-level, or global.


### How scoping works under the hood


When NewsBlur scores a story, it checks all classifiers that apply to that story’s feed — including any folder-scoped classifiers for the feed’s folder and any global classifiers. The same “green always wins” rule applies: if a story matches both a liked global classifier and a disliked per-site classifier, the story is marked as Focus.


Scope controls work with all classifier types: titles, authors, tags, text, and URLs. They also work with regex classifiers.


### Subscription tiers


Feature Tier Required


Per-site classifiers (default) Free


Global and folder-scoped classifiers Premium Archive


Manage Training scope filter Premium Archive


Global and folder-scoped classifiers are available now on the web. If you have feedback or ideas for improvements, please share them on the[NewsBlur forum](https://forum.newsblur.com/) .


---


This is a companion discussion topic for the original entry at[https://blog.newsblur.com/2026/02/02/global-and-folder-scoped-intelligence-training/](https://blog.newsblur.com/2026/02/02/global-and-folder-scoped-intelligence-training/)


1 Like


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 6, 2026, 1:11am 2


Thank you!!!


1 Like


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 6, 2026, 3:49pm 3


I sent you a message…can you please delete everything in my account regarding intelligence trainer? Thanks.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


February 6, 2026, 4:32pm 4


You should be able to delete all of your training classifiers in the new Manage Training tab. Are you finding that you can’t delete them? That’s a bug and we need to get to the bottom of that. I won’t delete your training because it should now be easy for you to do it, so let’s get that fixed. Have you tried to remove them all or do you have too many training filters?


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 6, 2026, 4:41pm 5


I just tried to delete them and it seems to work….I do have a lot of filters already in place. I would appreciate it if you could remove all of them.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


February 6, 2026, 5:29pm 6


Ok I just shipped a new intelligence training classifiers deletion tool inside the Account dialog. Should be pretty handy, with a confirmation about how many of each classifier you’re deleting.


Let me know how that works for you.


[Screenshot 2026-02-06 at 9.15.45 AM 1528×1108 161 KB](https://forum.newsblur.com/uploads/default/original/2X/8/82b42405a9fc958e79382d4c73771a4f085a53d0.jpeg)


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 6, 2026, 6:04pm 7


Very nice….looks like that took care of everything. Again, thank you!


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 6, 2026, 7:36pm 8


Would it be possible to separate “Global and Folder Classifiers” into two separate categores within the “Manage Training” tab? Thank you.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


February 6, 2026, 9:24pm 9


They are separate, no?


[Screenshot 2026-02-06 at 1.24.01 PM 1610×380 41.1 KB](https://forum.newsblur.com/uploads/default/original/2X/4/40905428efe64cfec19e959ee9b9927b19349a73.png)


Oh, you mean in the sections below? You’ll have to filter them if you want to see one or the other.


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 7, 2026, 1:51am 10


Hi Samuel. I need some help…I searched for “Ohio” in “All Folders & Sites” but how can I tell where (folder or feed) I initially applied the “Ohio” title? The purple header says “Global & Folder Classifiers” but the “Per Folder” says 2 and “Global” 0. Thank you.


[Example 990×357 28.8 KB](https://forum.newsblur.com/uploads/default/original/2X/f/f1147ac6622c19cf4b0f515c8b227bb7678e5c4c.png)


[samuelclay](https://forum.newsblur.com/u/samuelclay)


February 7, 2026, 4:56am 11


Ok, you got it, I moved folder-specific classifiers to each folder in the Manage Training dialog.


[Screenshot 2026-02-06 at 8.54.05 PM 1602×940 104 KB](https://forum.newsblur.com/uploads/default/original/2X/6/619f764fb2665823108e623ba9f7591996aeb864.png)


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 7, 2026, 4:37pm 12


That helps a ton and again, thank you!


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 8, 2026, 3:56am 13


Hi, Samuel. I might have found somthing…I have set excluded words from a folder named “Apple”…the intelligence trainer shows me this only when search for one of the excluded words:


[apple tags 407×288 15.2 KB](https://forum.newsblur.com/uploads/default/original/2X/9/96376514872a35b003f8ffcea083b5cf846a6b33.png)


However, even though I have exclusions set up for the “Apple” folder, I’m not seeing this folder in the Folder/Site drop down list (listed in alphabetical order):


[Folders 393×253 3.6 KB](https://forum.newsblur.com/uploads/default/original/2X/6/6bdf3ae7b9f7e5382a1816d0e4a8c1dec78d80d9.png)


…and I’m not seeing the Apple focused sites listed in the Folder/Site drop down list (listed in alphabetical order):


[Missing from list 822×572 65.3 KB](https://forum.newsblur.com/uploads/default/original/2X/6/6944851db0b28566e4e5c20ea31a08e4a2a43f74.png)


I’m seeing this also occur in two other folders.


Thank you for the recent changes…it has changed the way I use NewsBlur.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


February 8, 2026, 5:07am 14


Good catch, this is now fixed. The folder dropdown in the Manage Training tab was only looking at feeds with per-site classifiers when deciding which folders to show. So if a folder only had folder-scoped classifiers (like your excluded words on the Apple folder), it wouldn’t appear in the dropdown even though the classifiers themselves were working correctly.


Now the dropdown also checks for folder-scoped classifiers and includes those folders too.


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 8, 2026, 2:08pm 15


Thanks again. I’m trying to open the “Manage Training” tab but it’s hung on “Loading classifiers.” Thanks.


Update: It’s also happeing when I want to “Train a story”.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


February 8, 2026, 4:32pm 16


Thanks for being so patient and responsive. I shipped a fix and it should now work. Please keep these updates coming, they are so immensely helpful!


1 Like


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 10, 2026, 3:41pm 17


Hi Samuel. An idea…if I’m training a folder or a feed, when I pull up a specific folder/feed, can we only see what’s focused/excluded for just that folder? Example below…it’s a folder for truck stuff, but when I want to train that feed, I’m seeing customization from other folders/feeds too. They way it’s set up currrently and how it reads, the “More from this folder” implies I have the “Text” tags set up to look for {term here} in this specific folder when they really exist in other folders/feeds. Just a thought and thank you.


[Seeing Everything 886×602 60.9 KB](https://forum.newsblur.com/uploads/default/original/2X/7/7461dc0659cdd36bff4dae57342775337a0d6503.png)


[samuelclay](https://forum.newsblur.com/u/samuelclay)


February 10, 2026, 4:40pm 18


Well all of these classifiers are applying to this feed, which is why they are showing up. Now, they may not match anything, but they might for future stories. I think it’s important they show up here, as that’s how you know what’s being applied.


[cmhotb](https://forum.newsblur.com/u/cmhotb)


February 10, 2026, 5:05pm 19


I think I narrowed it down a bit more. The above screenshot was taken from “Train this story." The following two screenshots are taken from Settings/“Manage Training” and to me (I could be very wrong), they are showing two different sets of criteria when compared to “Train this story.” The view in “Manage Training” appears to be correct where if you train on the individual story, you get the results from my above screenshot. Please let me know if I’m whiffing on this. Thank you.


[Truck only 1426×1170 115 KB](https://forum.newsblur.com/uploads/default/original/2X/c/c4665175d16f65a4ca766022ac7aabc9fff69189.png)


[Four 1297×352 28 KB](https://forum.newsblur.com/uploads/default/original/2X/9/9aaae6eb5712be6f97f12b9989c09f9dd4a12e92.png)


[samuelclay](https://forum.newsblur.com/u/samuelclay)


February 10, 2026, 5:19pm 20


I got to admit I’m confused because in the above screenshot, you have a bunch of text filters at the folder scope, and in the below screenshot under manage training, you only have 8 global and 8 per-site classifiers.


Can you do me a favor and take screenshots with what you see and tell me what you expect to see?
