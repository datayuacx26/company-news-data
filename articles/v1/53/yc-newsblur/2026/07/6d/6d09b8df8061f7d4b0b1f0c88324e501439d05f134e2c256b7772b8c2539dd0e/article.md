---
schema_version: "1.0.0"
document_id: "6d09b8df8061f7d4b0b1f0c88324e501439d05f134e2c256b7772b8c2539dd0e"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-0646845e5ccf"
canonical_url: "https://forum.newsblur.com/t/swapping-stories-malfunctions-on-android/13780"
published_at: "2026-07-24T12:52:00+00:00"
first_seen_at: "2026-07-25T16:10:32.307561+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:19e7fd65ac7078b88ebd0b65edec5e69a93c2fef71d2c0c6843f7fe86f6177be"
---

# Swapping stories malfunctions on Android

[Guybrush](https://forum.newsblur.com/u/Guybrush)


July 17, 2026, 8:46am 1


Hi


C/c from[Adding tags and swapping stories malfunctions on Android](https://forum.newsblur.com/t/adding-tags-and-swapping-stories-malfunctions-on-android/13222)


> (…) when swapping stories if I’m swapping forward, then one story back and then forward, the first story forward is ok, the second one is empty but stories after are ok.
> If I’m swapping forward, then one story back and then continue to swap back, all stories after the first one are empty.


<give us some feedback!>


app version: 14.5.4 (279)
android version: 17 (CP2A.260705.006)
device: Google Pixel 7a (lynx)
username: Guybrush
server: default
pending actions: pre:0 post:0
premium: yes
prefetch: no
notifications: no
keep read: no
thumbs: yes


[Guybrush](https://forum.newsblur.com/u/Guybrush)


July 24, 2026, 12:52pm 2


Up


[samuelclay](https://forum.newsblur.com/u/samuelclay)


July 24, 2026, 3:41pm 3


Thanks for bumping this. I reproduced the July 17 report using NewsBlur 14.5.4 (279).


The regression came from a recent WebView memory optimization. A story’s WebView could be recreated while the app still considered its content loaded, leaving it blank after swiping back and forth. The fix now detects empty recreated WebViews and reloads their story content.


The fix was committed today and will ship in the next Android release after 14.5.4. The exact version number and Play Store date have not been set yet but should be out soon.


[Guybrush](https://forum.newsblur.com/u/Guybrush)


July 28, 2026, 6:31pm 4


It seems to work now with the last update, thanks
