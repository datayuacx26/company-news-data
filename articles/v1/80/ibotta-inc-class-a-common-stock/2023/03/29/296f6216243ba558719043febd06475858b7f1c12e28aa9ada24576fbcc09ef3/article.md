---
schema_version: "1.0.0"
document_id: "296f6216243ba558719043febd06475858b7f1c12e28aa9ada24576fbcc09ef3"
company_key: "ibotta-inc-class-a-common-stock"
company: "Ibotta Inc."
source_id: "ibotta-inc-class-a-common-stock-rss-dcf741155171"
canonical_url: "https://medium.com/building-ibotta/shipping-support-for-live-activities-in-ibottas-ios-app-654bf4d5cd25"
published_at: "2023-03-06T17:44:24+00:00"
first_seen_at: "2026-07-25T01:07:04.216753+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:a60891c13762ad6d4e021af1e30724f7d82d7e308385fe77d5e729999aa05497"
---

# Shipping Support For Live Activities in Ibotta’s iOS app

IOS


Swiftui


Apple


Engineering


Software Engineering


# Shipping Support For Live Activities in Ibotta’s iOS app


[Brett Markowitz](https://medium.com/@brett.markowitz?source=post_page---byline--654bf4d5cd25---------------------------------------)


6 min read


·


Feb 2, 2023


--


*How Ibotta integrated a new mobile feature using ActivityKit*


Press enter or click to view image in full size


ActivityKit (image from Apple)


Here at Ibotta, we’re always focused on improving the experience of using our products and making every purchase rewarding. We do that by experimenting early and often.


During our most recent company hackathon, my team built out a concept for Live Activity support in the Ibotta iOS app which we iterated on and shipped at the beginning of February.


In this post, we’ll discuss how we leveraged the` ActivityKit` APIs announced by Apple at WWDC 2022 to create a Live Activity of our own.


Ibotta’s Live Activity is focused on the idea of an in-person shopping trip. When you’re going out for groceries or other items, there’s a defined start and end time for the trip: when you arrive at the retailer, and when you leave. This aspect makes a shopping trip a good candidate for being represented with a Live Activity.


Press enter or click to view image in full size


Ibotta’s Live Activity in the Dynamic Island (compact state)


The Live Activity includes a handful of buttons providing access to product barcode scanning, your list of offers, gift cards, and more.


Press enter or click to view image in full size


Ibotta’s Live Activity on the Lock Screen


When it comes to the implementation, Live Activities are started, ended, and updated using the new` ActivityKit` framework. Similar to widgets, their UI is built using` WidgetKit` and` SwiftUI` . At a minimum, to support Live Activities, your app must include:


1. A widget extension with a widget bundle ([docs](https://developer.apple.com/documentation/WidgetKit/Creating-a-Widget-Extension) )
2. An entry for` Supports Live Activities` in the` Info.plist` file


The three main parts of any Live Activity are:


1. ` ActivityAttributes`
2. ` ContentState`
3. The view(s)


` ActivityAttributes` is a protocol defined by the` ActivityKit` framework. A type that conforms to it represents both the static and dynamic data you will use to populate your Live Activity.


The protocol requires that a conforming type provide a` typealias` named` ContentState` .


The activity attributes will contain the data for your Live Activity that won’t change over the course of its lifetime, while the content state will contain any data that may change between updates.


For example, in our Live Activity, we display the name of the retailer that the shopping trip was started at and the number of offers on your list for that retailer. The retailer name is a piece of static data since it won’t change and can therefore live in the activity attributes. As we’ll see shortly, the offer count is considered dynamic and should live in the content state, since we may want to update it later.


You can also store data that won’t be displayed in the Live Activity if it’s something that helps you in determining *what* to display. We have a boolean property that lets us control whether or not to show the list count badge, and we can leverage it later on in our view code.


Once user taps a button to start a shopping trip, we start the Live Activity by providing` ActivityKit` with two things:


1. The initial (and therefore only) state of the static data
2. The initial state of the dynamic data


We do this by creating instances of the attributes and content state.


Again, for the activity attributes (static data), we provide what we know will not change about this particular shopping trip. The content state (dynamic data) is given the current amount of offers on the user’s list, as well as whether or not the list icon should be badged. Both of those things could change during the lifetime of the Live Activity.


Then we call` request(attributes:contentState:)` on ActivityKit’s` Activity` class, passing in the attributes and the content state. At this point, the Live Activity will have started and will appear immediately on the Lock Screen. It will also appear in the Dynamic Island when the app is in the background.


After we start a Live Activity, we may want to update its state to show new information about the in-progress event. A user may unlock additional offers, so we want to reflect that by updating the count in the badge on the list icon; this is where our dynamic data comes into play.


Updating is generally as simple as:


1. Grabbing the in-progress Live Activity that you want to update
2. Creating a new version of the content state
3. Calling` update(using:)` on the Live Activity, passing in the new content state


The main difference between starting and updating a Live Activity is that updates cannot be given a new version of the activity attributes, only the content state. Also, Live Activities can be updated via push notifications. Doing so is beyond the scope of this article, so we recommend checking out[Apple’s docs](https://developer.apple.com/documentation/activitykit/updating-and-ending-your-live-activity-with-activitykit-push-notifications) for more info.


Finally, Live Activities can be ended when they’re no longer relevant or needed, and the process is very similar to making updates.


1. Grab the in-progress Live Activity that you want to end
2. Optionally, create a final set of dynamic data
3. Call` end(using:dismissalPolicy)`


The` dismissalPolicy` argument allows us to control when exactly the Live Activity is ended. Using` default` will keep the Live Activity around until either the user removes it manually or 4 hours have passed, while` after(_:)` will let the system end it at a particular time in the next four hours. For our Live Activity, it makes more sense to have it end immediately when a user ends their shopping trip, rather than have it hang around until the system cleans it up.


On the UI side of things, a Live Activity is essentially a[widget](https://developer.apple.com/documentation/SwiftUI/Widget) .


New in` WidgetKit` is a` ActivityConfiguration` type which describes the contents of a Live Activity and allows us to configure its appearance when for the Lock Screen and Dynamic Island. When you create an` ActivityConfiguration` , you’ll need to provide a type conforming to` ActivityAttributes` that this particular Live Activity will be responsible for displaying the data from. For us, that would be the type we created earlier:` ShoppingTripLiveActivityAttributes` .


Within the configuration,` content` provides the views for the Live Activity as it appears on the Lock Screen, while the appropriately named` dynamicIsland` closure will provide different views for the different areas and states of the Dynamic Island. Both closures are passed an` ActivityViewContext` , which contains the static and dynamic data from when we started, updated, or ended the Live Activity. We can simply pass the context object into our` SwiftUI` views and use it to configure the appearance of our Live Activity.


Notice how we’re pulling the retailer’s name from the` attributes` property on the` ActivityViewContext` . The` attributes` property is of type` ShoppingTripLiveActivityAttributes` which contains our static data, and the` context` object also has a` state` field, which is of type` ShoppingTripState` and contains our dynamic data.


We also use[Universal Links](https://developer.apple.com/documentation/xcode/allowing-apps-and-websites-to-link-to-your-content?language=objc) and Swift UI’s`[Link](https://developer.apple.com/documentation/swiftui/link)` control to allow the buttons in our Live Activity to deep link the user to a specific spot in the app.


> Buttons in a Live Activity will always open your app. It’s not currently possible to build something like Apple’s Live Activities, where actions like pausing or skipping a song don’t open the app.


Finally, add the Live Activity widget to a widget bundle. If you don’t already have a widget bundle, Apple has some docs on setting one up[here](https://developer.apple.com/documentation/WidgetKit/Creating-a-Widget-Extension) .


For more information on` ActivityKit` and Live Activities — as there’s certainly a ton that we didn’t cover — check out Apple’s write-up[here](https://developer.apple.com/documentation/activitykit/displaying-live-data-with-live-activities) . Also, be sure to consider the Human Interface Guidelines around Live Activities[here](https://developer.apple.com/design/human-interface-guidelines/components/system-experiences/live-activities) when building your own.


> **Note:** Some of the APIs discussed above have been deprecated by Apple starting in iOS 16.2 and replaced with new ones. Fortunately, usage is largely the same; the new APIs differ only in their naming.
>
>
> For example,` *request(attributes:contentState:pushType:)*` has changed to` *request(attributes:* ***content:*** *pushType:)*` .
>
>
> If your app still targets iOS 16.1, you’ll need to continue using the original APIs. Otherwise, Xcode will let you know if the methods you’re using are the deprecated ones and offer to fix them.


In terms of what’s next for us, there’s still a lot of investigating to do, not only around Live Activities, but also around the in-store experience. We feel this is an area worth diving into as we continue on our quest to make it as easy as possible to earn cash back — we’re really excited about the possibilities.


Huge shoutout and thanks to the iOS engineers on our Saver Discovery squad — Oniel Rosario, CC Cooper, and Shravya Machanna — as well as Kristie Stalberger, Danilo Caetano, Alysia Palmer, Praneeth Yaramosu, and Sven Parisi for their hard work to help bring this feature to life!


*Interested in working at Ibotta? Check out*[https://ibotta.com/careers](https://ibotta.com/careers) *to browse openings and learn more about us!*
