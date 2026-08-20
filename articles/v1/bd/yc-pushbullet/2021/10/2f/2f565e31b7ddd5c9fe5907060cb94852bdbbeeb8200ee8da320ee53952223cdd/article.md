---
schema_version: "1.0.0"
document_id: "2f565e31b7ddd5c9fe5907060cb94852bdbbeeb8200ee8da320ee53952223cdd"
company_key: "yc-pushbullet"
company: "Pushbullet"
source_id: "yc-pushbullet-rss-4ba42a362971"
canonical_url: "https://blog.pushbullet.com/2021/10/04/closing-the-portal-app"
published_at: "2021-10-04T05:00:00+00:00"
first_seen_at: "2026-07-25T20:08:48.518680+00:00"
fetched_at: "2026-08-20T03:16:18.952870+00:00"
content_hash: "sha256:3390b71b14be932defaa7eadac10ab2a8e60364409bf24608b8a00231d8ba48a"
---

# Closing The Portal App

[Portal](https://blog.pushbullet.com/2015/06/16/introducing-portal/) was a passion project that we built to experiment with other ways of moving data between phones and computers. We built the slickest app we could and have continued to support the app, site and server for over 6 years now.


Unfortunately, over 6 years later, I’m now writing this post because many things have changed and the time has come to close Portal.


**Important: The Pushbullet app is not affected by this announcement.**


### Why close Portal?


We are closing Portal because Google has made changes to both Android and Chrome that prevent Portal from working in the way we built it to work.


### Changes to Android


We chose to have Portal put files into the shared storage on your phone so other apps could have access to the files after they were transferred. This played very nicely with other apps, making interactions possible just like on a regular computer. Let’s say you had a game console emulator installed, you could use Portal to drop a bunch of ROMs onto your phone and the emulator was able to find them. The same was true for playing video files, etc.


Android has since moved away from shared storage and all apps will be required to use[Scoped Storage](https://developer.android.com/training/data-storage#scoped-storage) in just a couple months. Our philosophy of enabling apps to easily interact with files no longer fits the direction Android has taken.


### Changes to Chrome


Starting with Chrome 94,[Chrome no longer allows HTTP requests to local IP addresses](https://developer.chrome.com/blog/private-network-access-update/) . Since Portal worked by accessing a local server running on your phone to do all file transfers locally over WiFi, this means Portal no longer works for anyone using Chrome. Since Chrome and Chromium variants have overwhelming browser dominance, this means Portal will soon no longer work for the vast majority of people.


### Some final details


We will be shutting down Portal on Friday, October 8th. We have already unpublished the Portal Android app.


Thanks to everyone that used Portal over the years!
