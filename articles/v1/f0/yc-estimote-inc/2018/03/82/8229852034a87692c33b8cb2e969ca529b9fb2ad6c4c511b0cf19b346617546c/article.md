---
schema_version: "1.0.0"
document_id: "8229852034a87692c33b8cb2e969ca529b9fb2ad6c4c511b0cf19b346617546c"
company_key: "yc-estimote-inc"
company: "Estimote, Inc."
source_id: "yc-estimote-inc-rss-9f8374160089"
canonical_url: "https://blog.estimote.com/post/171926435365"
published_at: "2018-03-16T09:52:09+00:00"
first_seen_at: "2026-07-27T09:08:36.947635+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:649dcf7401dcece6f6886466192ebd0c419a3e84ad82f0f793f4d9e1665e7f61"
---

# Build your first proximity app in 5 minutes

We’ve recently launched our[Proximity SDK](http://blog.estimote.com/post/165007958785/launching-the-most-reliable-configurationless) enabling the world’s most reliable beacon notifications and proximity zone monitoring. Getting to know a new SDK and learning its API can be time-consuming. It often means hours spent on manual integration and troubleshooting. Now there’s a better way. You can save that time and focus on your use case, if you let us guide you through our new[App Templates](http://blog.estimote.com/post/134932353130/introducing-app-templates-in-estimote-cloud-fewer) , which come with our Proximity SDK integrated from the start. We’ll pick the right template and go through what’s possible—defining proximity zones, adjusting range and more. Now, buckle up and let’s go!


### Starting off with the right template


The most popular use cases for proximity are either about adjusting the content of an app depending on where the user is, or sending notifications when someone enters or leaves an area. You can easily accomplish both with our App Templates available in the[Estimote Cloud](https://cloud.estimote.com/#/apps) .


In the first case, users actively interact with the app and you adjust content based on where they are. Think about a museum application that presents different content to a visitor depending on which artwork they are closest to (similar to the Guggenheim Museum’s[case](http://blog.estimote.com/post/157200820650/the-icon-of-modern-art-puts-estimote-beacons-on) ). Here you would go for “Proximity for a Single Beacon” and “Proximity for Multiple Beacons” templates as they monitor proximity zones in foreground.


It’s different when your app is supposed to stay idle until a specific event occurs—a user entering or leaving an area. In this case, monitoring runs in the background—without the app being active. The moment you want to engage your users, you can quickly bring the app into focus with a notification. It could be used for proximity marketing in retail, like in the Tesco Lotus’[case](http://blog.estimote.com/post/158169494355/tesco-and-estimote-pioneering-grocery-stores-of) . They send different discounts and coupons based on the department you enter. Here, the “Notification” template would do as it works in background.


Got a completely different idea and want to start from scratch? Go for the “Blank” template with the Proximity SDK already integrated.


All those templates are available for both iOS & Android and now support Kotlin! Now let’s see how you can tweak them to your use case.


### Defining proximity zones with attachments


After choosing the right template, it’s time to connect to your beacons, adjust their range… Well, that’s not the case anymore! Proximity zones are software-defined!


Each beacon has a Cloud attachment associated with it. This means you can keep simple data (for example names of the areas) in the Estimote Cloud using key-value pairs. They live there and can be modified at any time directly from the Estimote Cloud dashboard in the Beacon Settings section:


Then, zones are defined based on those attachments in the mobile app using *EPXProximityZone* objects. You can easily adjust their range with the *desiredMeanTriggerDistance* argument:


```text
let zone = EPXProximityZone(
range: EPXProximityRange.custom(desiredMeanTriggerDistance: 3.0)!,
attachmentKey: "room",
attachmentValue: "kitchen"
)


```


### Taking it further with zone based interaction


You can decide what happens when a user enters or exits an area defined by a proximity zone:


```text
zone.onEnter = { attachments in
// Send a notification saying “Hey, looking for coffee?”
}


zone.onExit = { attachments in
// Send a notification saying “Have a nice day and see you soon!”
}


```


It’s possible to subscribe to both of the events at once. You could use that for gathering analytics to better understand how your users interact with the space:


```text
// Both enter and exit events
zone.onChangeAction = { attachments in
// Send analytics to your cloud
}


```


So when entering the kitchen, a user would get this:


### Grab a devkit and build your app!


Now that you know all about templates, go to[Estimote Cloud](https://cloud.estimote.com/#/apps) and start building your first app with the Proximity SDK. Keep in mind, it runs on[Estimote Monitoring 2.0](http://blog.estimote.com/post/163745555265/estimote-monitoring-20-with-even-better-accuracy) and Proximity Beacons have this enabled by default. However, if you bought a dev kit before September 2017, just[enable Estimote Monitoring](https://t.umblr.com/redirect?z=https%3A%2F%2Fcommunity.estimote.com%2Fhc%2Fen-us%2Farticles%2F226144728-How-to-enable-Estimote-Monitoring-&t=Y2YzMzgwNWQ4OTA1Mjk2OTUyYWExNjQxMzNhNjFiZDVmNTZkYzdjMyxONEF6S3RhNQ%3D%3D&b=t%3AoQuTRuM8nwj_3gRKFqtcwA&p=http%3A%2F%2Fblog.estimote.com%2Fpost%2F166210528555%2Ftry-our-improved-proximity-experience-in-5-minutes&m=1) and you are good to go!


If you need more help, clarification, or just want to brag about an amazing app you’ve built, head straight to our[forums](https://forums.estimote.com/) or ping us at[contact@estimote.com](https://t.umblr.com/redirect?z=mailto%3Acontact%40estimote.com&t=NjBkNjJhZWNhZWM1MmZhODcyODg4NTVlNTIzOWFjZWU4YTBhYWM2NyxONEF6S3RhNQ%3D%3D&b=t%3AoQuTRuM8nwj_3gRKFqtcwA&p=http%3A%2F%2Fblog.estimote.com%2Fpost%2F166210528555%2Ftry-our-improved-proximity-experience-in-5-minutes&m=1) .


*Przemek Blasiak, Software Engineer at Estimote*
