---
schema_version: "1.0.0"
document_id: "39fa5505e78ed6440ab1798ca000574f1abe771e059db3889d10bf0c32ded706"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/why-you-should-change-your-mobile-app-version-format-to-year-week-iteration"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:d224930dd9306a7951787efd223ee36750a2f9b48e02975b34b8f1c85f040373"
---

# Why you should change your mobile app version format to [year].[week].[iteration]

As a software engineer, I’ve been trained to think that there’s only one way to version software:[semantic versioning](https://semver.org/) .


At[Photoroom](https://www.photoroom.com/) , we used to increment the major version number on big releases to celebrate. Recently, however, we moved our mobile apps to a versioning scheme like like 2025.51.01 (so *\[year\].\[weekNumber\].\[iteration\]* )


The main reason is convenience. At a glance you can know whether a user contacting support is on an old version or not or if a coworker reporting a bug forgot to update.


It’s also quite useful to understand version distribution. You can see below that most users on iOS are on version 2025.50.03 (meaning the third iteration of the 50th week of the year) and are progressively upgrading to 2025.51.01:


Version distribution, with Android on the left and iOS on the right


One convenient thing you can do is deprecate some routes by looking at the version number to know which clients are older than one year/some duration, here’s some pseudocode:


```text
version = request.headers["pr-app-version"]
weeksSinceAppWasReleased = parseWeeksSinceAppRelease(version)
if weeksSinceAppWasReleased >= 52:
return {error: "APP_OUTDATED", errorMessage: "Your app is outdated, please visit the app store to update"}


```


**The downsides**


As App Store version have to be incremental, so once you try this setup, you can’t go back.


When we implemented it, I also worried that such long version numbers would be a bit scary for users on the app store listing page, but it seems they don’t mind.


On my side, I’m still a bit worried when I see Uber’s super long version number


Overall, I’d recommend this setup in a heartbeat if you run a mobile app at scale.


If you find this article useful, go discuss it on[Hacker News](https://news.ycombinator.com/item?id=46580399) .
