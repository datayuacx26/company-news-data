---
schema_version: "1.0.0"
document_id: "d7600e97654b53ea081b6235fcac2243a04ec2c85bd4d67ecff6e0d8f348cd90"
company_key: "yc-newsblur"
company: "NewsBlur"
source_id: "yc-newsblur-rss-0646845e5ccf"
canonical_url: "https://forum.newsblur.com/t/what-happened-to-the-activity-feed/13663"
published_at: "2026-07-03T20:44:58+00:00"
first_seen_at: "2026-07-25T16:10:32.307561+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:57fd6c05ca531d40d86cfb7bca47708a7e06b0cf14e8340035cd2fc02e3e6a0c"
---

# What happened to the activity feed?

[xref](https://forum.newsblur.com/u/xref)


May 2, 2026, 4:48am 1


Before the ios26 update the activity feed was an electric symbol in the top right of the Home Screen…which is now a blank spot on the interface?


In a submenu I see “interactions” for which the first tab never loads anything, just spins for several minutes. And the second tab “your activities” after a minute or two shows stories I’ve shared. Am I looking in that wrong place to see comments on shared articles/replies etc?


[samuelclay](https://forum.newsblur.com/u/samuelclay)


May 2, 2026, 4:59am 2


It should be there in the Interactions menu option. If it’s spinning, that’s a bug. Let me take a look…


Yep, you’re right, both spin forever. I’ll get that fixed in the next release. Thanks for letting me know!


[samuelclay](https://forum.newsblur.com/u/samuelclay)


May 2, 2026, 5:00am 3


Just FYI, I moved the status of sync and offline story downloading from the bottom to the top right, where the interaction button is. Honestly, no one ever uses that button. A few users do, but they use it very infrequently, so I moved it to the menu.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


May 2, 2026, 5:25am 4


Fix is out in the next iOS app update, which I’m submitting tonight.


[xref](https://forum.newsblur.com/u/xref)


May 2, 2026, 5:28am 5


Thanks as always! Yeah figured it disappeared from the prime location for a reason, glad it’s not totally gone though. Now there’s a lot of prime real estate empty at the top status bar though! Oh the possibilities


[xref](https://forum.newsblur.com/u/xref)


July 3, 2026, 4:51pm 6


Did this fix ever make it out? Both still just spin forever for me, on 14.7


[samuelclay](https://forum.newsblur.com/u/samuelclay)


July 3, 2026, 7:46pm 7


That’s strange, I did fix it but you’re right, it’s broken again. I’ll take a look.


[samuelclay](https://forum.newsblur.com/u/samuelclay)


July 3, 2026, 8:44pm 8


Found the regression. The Interactions sheet could refresh before its views had loaded, which left both tabs stuck on the spinner. I checked and both Interactions and Your Activities load again. Should be out in the next week or so.


Fixed here:[https://github.com/samuelclay/NewsBlur/commit/6cfc81ec76e0b9e2135e98fb93eb7278b2ee4944](https://github.com/samuelclay/NewsBlur/commit/6cfc81ec76e0b9e2135e98fb93eb7278b2ee4944)
