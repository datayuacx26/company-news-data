---
schema_version: "1.0.0"
document_id: "24849b7be52797d242babb652ea722ce729b572fa0fb0e4e2d822c9cc0006d24"
company_key: "yc-jitsu"
company: "Jitsu"
source_id: "yc-jitsu-news-import-65f0e2b767a6"
canonical_url: "https://jitsu.com/blog/server-side-tracking"
published_at: "2023-11-06T00:00:00+00:00"
first_seen_at: "2026-07-22T00:57:29.784311+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:446b49e0bbc25d9a153e196c3a319af10757b4410559e7163573fef66233e089"
---

# Server side tracking 101

## Introduction


This article outlines the differences between client-side and server-side tracking, explaining why server-side tracking can be more beneficial. It also offers practical recommendations for switching to server-side tracking. Let's start with the basics.


## What is Client-Side Tracking?


Client-side tracking has been the main way of tracking user behavior on the web for at least the past decade. The most popular tools, such as Google Analytics, Mixpanel, and Amplitude, are based on client-side tracking. It operates as follows:


- A tracking JavaScript is loaded from the server, or integrated into the front-end app as a JavaScript library, or both.
- The JavaScript is executed on the client, collecting basic information about the user, such as user ID, page title, etc.
- Then, this script sends events to the server. Some events, such as page views, are automatic. Other events, such as conversions or certain button clicks, are sent from the front-end app.
- The front-end app may interact with the tracking script in other ways, like providing a` user id` or` user email` to identify the user.


## What is Server-Side Tracking?


Server-side tracking operates differently. It doesn't rely on JavaScript libraries and sends all events from the backend. Events that are known to the backend by design, such as page views, are sent automatically. Other events are triggered by the front end, but the first event goes to the server, and the server sends it to the tracking platform.


## Why is Server-Side Tracking Better?


There was a time when JavaScript didn't exist on the web at all, and all tracking (if any) was done on the server. The server that renders the page inherently knows that the user has visited the page and can record this information for later analysis. Since JavaScript emerged, the world moved to client-side tracking for many reasons, but server-side tracking is still superior in several ways:


- Server-side tracking doesn't interfere with user experience and page speed. Client-side tracking scripts are often heavy and slow down the page.
- Server-side tracking is more precise. If the server returned the page (or data for the page in a SPA), the fact won't be lost. On the client side, the tracking script may fail to execute, or the user may close the page before the script has executed.
- Server-side tracking is immune to ad blockers. Ad blockers can block tracking scripts, but they can't block server-side tracking.


The only downside of server-side tracking is that it's harder to implement.


## When Server-Side Tracking is Not Possible


There are a few cases when server-side tracking can't be used. Let's review them.


### Advertising Platforms


Most advertising platforms rely on browser cookies to identify the user and cross-match user IDs with their internal data platforms. This means they need to be able to set cookies, which is only possible if their JavaScript is executed on the client.


Most disadvantages of client-side tracking do not apply to advertising platforms by design. For example, if the user has an ad blocker, they won't see ads anyway, and it's pretty pointless to attempt to track their behavior. Similarly, tracking precision is less critical. Advertising platforms operate on a large scale, and they can tolerate losing information on a small portion of users.


### Heatmap and Session Trackers


Heatmap and session trackers, like Hotjar, Logrocket, and Fullstory, are used to record user actions like mouse movements and clicks. These types of user actions never reach the server.


### Google Analytics


Since the release of GA4, Google has significantly reduced the functionality of their[measurement protocol](https://developers.google.com/analytics/devguides/collection/protocol/ga4) , so not all data can be sent from the server. If you want to use GA4, you'll need to rely on client-side tracking at least for page views.


## Jitsu's Recommendation: Use Client-Side Tracking for Google Tag Manager and GA


We recommend switching to server-side tracking whenever possible. For client-side, use Google Tag Manager as middleware, connecting all advertising platforms and Google Analytics to it. This ensures that no important data is lost.


You can also use Google Tag Manager for session recording and heatmap tools.
