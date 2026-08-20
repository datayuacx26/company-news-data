---
schema_version: "1.0.0"
document_id: "befd396c1b579fe6b871906726cba9af5d8d45624cc71ee139d00e7776900223"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-0646845e5ccf"
canonical_url: "https://forum.newsblur.com/t/newsblur-app-on-ios-orientation-issue/13672"
published_at: "2026-08-05T16:39:50+00:00"
first_seen_at: "2026-08-05T17:41:21.115563+00:00"
fetched_at: "2026-08-05T17:41:23.509057+00:00"
content_hash: "sha256:b87b4f533cd2f8ced8cda763a868f7bab2c5336f6338d9f3c44ec806744fa8fa"
---

# NewsBlur app on iOS orientation issue

[Kal](https://forum.newsblur.com/u/Kal)


May 6, 2026, 4:24am 1


So I think this may have been around for a while, but I dismissed it as user error since there’s no way this is working as designed.


On the iOS app, when you turn your phone from portrait to landscape, the view changes to accommodate the new orientation.


This view includes your list of feeds, and, presumably your list of articles for the last feed you were looking at. The list of feeds takes up nearly half the screen, and the list of articles takes up only part of the other half of the screen. The article view looks like it doesn’t belong. It starts after a large gap at the top of that side of the screen. Why? Not sure. The list should start at the top of that part of the screen. Add the fact that there is a different colored “sidebar” with nothing in it running up the far right of the screen in landscape mode. If I had to guess this is to pad out the rest on the screen rather than letting the list of articles expand to fill the rest of the space on that side of the screen. To make thingss messier, if the articles have pictures associated with them, those small preview images overlap with the article bar that appears just above the list.


This doesn’t look great, but this isn’t why I’m here.


Now, keep all of that in mind while we return to portrait mode and tap into a feed, then select an entry to read. This brings up the entry by itself on the screen so you can read it or otherwise interact with the content.


Now imagine while you are reading, you put your phone down for a second, or maybe you drop it. The phone detects this as an orientation change and switches to the landscape feed list and article list split-column view mentioned above. Ok. It’s understandable that it might do that, even though I wasn’t there just now. I was reading and article from the feed. So my place is lost in this view. Not great, but maybe there’s a reason for this.


Here’s the worst part. When I retrieve my phone and hold it up in portrait mode again, the view changes again. However, it doesn’t go back to where I was, reading an article. It instead takes me back to the feed list (not even the article list).


The long and the short of it is, if I rotate my phone, unless I happen to be in the list of feeds already, I will be kicked out of where I just was and be taken to the feed list. Every single time.


Now I know I could lock my phone to portrait mode to avoid this, but that misses the point that under no circumstances should this happen. I should always to back to the view I was last in if I didn’t tap on anything while rotated.


I looked through the preferences to see if there was some setting I changed that might cause this behavior, but I couldn’t find anything.


what am I missing? How can I get newsblur to act properly? It should show me my article if that was the view I was on before the orientation change. The list of articles can fill part of the screen in landscape mode, since there is room to show it, but the article view must remain visible if that was the view I was on before the orientation change and when I return to portrait orientation, I should be back on the article view right where I was reading (unless I tapped on something else while in the other orientation).


what setting do I need to change to get Newsblur to act properly?


[samuelclay](https://forum.newsblur.com/u/samuelclay)


May 13, 2026, 1:15am 2


The latest iOS app update should fix this issue. Let me know if it does. You shouldn’t be booted out to the feed list.


[Kal](https://forum.newsblur.com/u/Kal)


May 13, 2026, 3:46pm 3


Thanks, but it still boots me to the feed list when I rotate the phone and rotate it back again.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


May 13, 2026, 5:26pm 4


I’ve tested this thoroughly, so can you make a screen recording of this happening so I can reproduce your exact conditions? If you don’t want to post it publicly, feel free to DM it to me.


[Kal](https://forum.newsblur.com/u/Kal)


May 13, 2026, 7:40pm 5


Thanks. I messaged you with the video.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


May 14, 2026, 1:45am 6


Ahha, iPhone 17 pro max. I just never tested with that configuration before. Instantly repro’d. Will fix shortly!


[Kal](https://forum.newsblur.com/u/Kal)


May 14, 2026, 2:38am 7


Thank you!


[nicked](https://forum.newsblur.com/u/nicked)


August 5, 2026, 4:39pm 8


I’ve been having a related problem except I get a 100% reproducible crash every time I rotate my phone in the feed stories view or in an article. When I rotate back to portrait the app just hangs, every time. Been happening for a couple of months I’d say, maybe since this update?


iPhone 12 Pro Max on iOS 26.6
