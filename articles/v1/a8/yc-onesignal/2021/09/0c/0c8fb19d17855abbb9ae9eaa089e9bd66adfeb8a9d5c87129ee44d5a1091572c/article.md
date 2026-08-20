---
schema_version: "1.0.0"
document_id: "0c8fb19d17855abbb9ae9eaa089e9bd66adfeb8a9d5c87129ee44d5a1091572c"
company_key: "yc-onesignal"
company: "OneSignal"
source_id: "yc-onesignal-news-import-11f34d3780c9"
canonical_url: "https://onesignal.com/blog/confirmed-delivery-improve-ctr/"
published_at: "2021-09-22T18:44:00+00:00"
first_seen_at: "2026-07-22T07:23:19.307690+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:a83ceb0f71119a3cc35ea741c720a612ea7bfb8675dbaa323675074926f3c705"
---

# Confirmed Delivery: Improve CTR Accuracy & Monitor Delivery Health

## Overview


[Confirmed Delivery](https://onesignal.com/blog/understanding-confirmed-delivery/) , a powerful new feature you’ve all been asking for, is now available on mobile and web. Confirmed Delivery provides near-perfect[Notification Click-Through Rate](https://documentation.onesignal.com/docs/notification-ctr) **** (CTR) estimates and gives app developers peace of mind when it comes to confirming that their notifications have successfully delivered.


Each device that receives a notification sent through our platform will now acknowledge successful receipt, sending this data back to OneSignal and then making it available to all paid users. Look out for the “Confirmed” metric in a notification’s message report.


The newest launch of our[Advanced Analytics](https://onesignal.com/blog/advanced-analytics-now-available-across-platform/) is available across all channels (Web, Mobile & In-App).


## How do Confirmed Deliveries work?


To understand[Confirmed Deliveries](https://onesignal.com/blog/new-onesignal-features-confirmed-delivery-carousel-analytics-additional-language-support/) , you must first understand what goes into delivering a notification to a device. In general, push providers must go through Apple and Google in order to deliver notifications. This means that there are multiple "hops" between OneSignal servers and the intended device.


Without device-level confirmation of notification delivery, OneSignal can only confirm the successful delivery to[Apple Push Notification Service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) (APNS) and[Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging) (FCM) servers. However, our Confirmed Delivery feature now takes the form of an extra network request sent from the device back to our servers acknowledging the notification:


## Why are Confirmed Deliveries important?


### 1. Click-Through Rate


Without Confirmed Deliveries, CTR’s are calculated by dividing the number of confirmed notification opens by the total number of subscriber devices targeted:


Without **Confirmed Deliveries**


The issue with using CTR’s alone is that they count "phantom subscribers," which ultimately results in less accurate[CTR](https://onesignal.com/blog/increase-click-through-rates-with-push-notifications-and-in-app-messaging/) over time. Phantom subscribers are devices that have never unsubscribed (e.g. by deleting your app), but are either no longer used or are powered off.


For example, let’s say a user who just upgraded to a new smartphone throws their old device in a drawer and never touches it again. Despite permanent inactivity, this device is technically still subscribed to your app. When calculated this way, CTR’s become gradually less accurate as seen in the graph below:


*This model assumes users upgrade their phones every 24 months and a 10% "true" CTR (CTR only of ACTIVE devices). You can see that on the 24th month, the click through rate was half of the initial percentage since there are twice as many subscribed devices.*


A better way to calculate CTR is to divide the number of users who clicked a notification by the number of users who received that notification:


With **Confirmed Deliveries**


Until now, OneSignal didn't offer a way to calculate how many devices actually received the notification. With Confirmed Deliveries **,** you will now be equipped with the power of near-perfect delivery statistics.


### 2. Delivery Health


Confirmed Deliveries help you monitor the health of your messaging pipeline and use metrics to more effectively debug your implementation.


## Why isn't this feature offered by most push providers?


To confirm device-level delivery of a notification, push-providers must process an extra inbound request for each push. This leads to a large burst in traffic to their servers, which requires heavy infrastructure. Processing inbound requests requires more than simply sending requests. When our infrastructure sends out pushes at a sustained speed of one million pushes per second, it must also be able to handle over one million incoming confirmed deliveries at the same rate. Luckily, we’re up for the challenge.


To learn more about how to understand and optimize your push notification performance, check out the related article below.


[Read: Guide to Understanding Push Performance](https://onesignal.com/blog/guide-to-understanding-push-notification-performance/)
