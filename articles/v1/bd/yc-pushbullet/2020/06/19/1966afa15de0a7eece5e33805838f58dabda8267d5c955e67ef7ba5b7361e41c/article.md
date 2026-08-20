---
schema_version: "1.0.0"
document_id: "1966afa15de0a7eece5e33805838f58dabda8267d5c955e67ef7ba5b7361e41c"
company_key: "yc-pushbullet"
company: "Pushbullet"
source_id: "yc-pushbullet-rss-4ba42a362971"
canonical_url: "https://blog.pushbullet.com/2020/06/08/updating-our-chrome-extension-after-our-google-scare"
published_at: "2020-06-08T05:00:00+00:00"
first_seen_at: "2026-07-25T20:08:48.518680+00:00"
fetched_at: "2026-08-20T03:16:18.952870+00:00"
content_hash: "sha256:bde591b91990b16a4c5797c508333084ecba4b053c261ce0943d73637e5d2cb4"
---

# Updating Our Chrome Extension After Our Google Scare

**Three weeks ago[Google was close to removing our Chrome extension](https://blog.pushbullet.com/2020/05/13/lets-guess-what-google-requires-in-14-days-or-they-kill-our-extension/) from the Chrome Web Store. Fortunately things[ended up working out](https://blog.pushbullet.com/2020/05/15/our-extension-is-safe/) . Not fun though!**


To meet Google’s requirements and stay in the Chrome Web Store we needed to remove a few permissions from our extension. It has become clear over the past few weeks, however, that those permissions enabled our extension to work better than it does now without them.


**Here’s our challenge: make our extension work for you as well as it did before all while continuing to require less permissions.**


The permissions we removed while trying to get our extension approved were **tabs** , **http://*/*** and **https://*/*** . These permissions grant access to private information and can be misused. In our case, however, they are required to enable some valuable functionality:


-


The **tabs** permission lets extensions see what tabs are open. Pushbullet uses this permission to avoid opening new tabs for websites that are already open when[mirrored notifications](https://blog.pushbullet.com/2013/11/12/real-time-notification-mirroring-from-android-to-your-computer/) are clicked.


-


The **http://*/*** and **https://*/*** permissions grant extensions access to data on all webpages. The Pushbullet extension uses this permission to be able to push images and files from websites that do not permit[Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) as[documented here by Google for Chrome extensions](https://developer.chrome.com/extensions/xhr) .


**So how do we get these permissions after Google had us remove them?**


We’re going to try adding them as[optional permissions](https://developer.chrome.com/extensions/permissions) instead of requiring them right away.


Optional permissions must be specifically granted by users after installation. This makes them much less scary since you can safely install extensions without granting these optional permissions.


For our extension, you can choose to grant these optional permissions on our extension’s options page. We’ve added two new options:


**Will something bad happen if we request these permissions again as optional permissions?**


This is a mystery to us to be honest. Are we allowed to request these permissions as optional permissions without upsetting Google? The only way to find out is to try so that’s what we’re doing. We’re optimistic things will work out.


**Today we submitted an update to our Chrome extension with the optional permissions support.**


We hope everything goes smoothly and we’re approved super quick!
