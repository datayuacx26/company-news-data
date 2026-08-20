---
schema_version: "1.0.0"
document_id: "88d91e048722e150b34562624dcb3163d3880b5f086343431a5d7a477b76a1e4"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/10-tools-to-ship-an-ios-app-in-2-weeks"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:c7e4a99d944adb48b240b576636fb98b5d5179515c0d002ebe63eecb89ab9f4b"
---

# 10 tools used to ship an iOS app in 2 weeks

I just launched[BG](https://itunes.apple.com/us/app/photo-background-changer/id1455009060?mt=8) (now called[Photoroom](https://app.photoroom.com/) ). It’s an image assistant which understands your images, and we believe it will change the way people do image editing! It starts with a few simple tasks, such as erasing background or objects and selecting objects instead of layers that never made sense to anyone. The machine learning runs locally on the device's GPU and is crazy fast! It even works real-time on[video](https://twitter.com/matthieurouif/status/1126575118812110854) with the iPhone XS neural engine. But enough about the app…


It took two weeks to build the first version of BG, from the time we wrote the first line of code on GitHub until it was released on the App Store. You can work really fast — if you have the right tools! So if you’re new to mobile development, this post might help you.


## **Programming — the Classics**


**0.XCode:** This goes without saying.


**[1.GitHub](https://github.com/) :** You can’t do much without a versioning tool. Use any other competitor if you don’t like the octocat.


**[2.Fastlane](https://fastlane.tools/) :** Submit your apps effortlessly to the App Store with this mobile dev ops tool. Given the tools I used, I recommend[this fastlane](https://github.com/danielkiedrowski/fastlane-plugin-onesky) plugin for OneSky.


**[3.Cocoapods](https://cocoapods.org/)** : Mobile Dependency Manager for Swift. Everyone is using it. Mostly, it allows you to import and manage libraries called “pods” for the following tools or from other developers. Most of the following tools are actually pods.


**[4.Firebase](https://firebase.google.com/)** : Cloud service with Firebase is dead simple to set up. You can easily store your users account there. It also includes feature flagging, AB testing, crashlytics for stability, push notification. And it’s all mostly free.


## **Going Global**


**[5.OneSky](https://www.oneskyapp.com/) to localize text:** I like OneSky because I never had a single compile bug with translations in 4 years (we used it at GoPro before). There is a collaboration system with commenting and versioning with translators. I recommend automating translations download and upload with[fastlane](https://github.com/danielkiedrowski/fastlane-plugin-onesky) . Also, add screenshots for context for translators. Thanks to fastlane, we were able to ship BG in 14 languages for just a few hundred bucks.


OneSky APIs are not perfect, and the website could be 10x faster. If you have something better for this one, please share it with me — I would be happy to try it!


**[6.Figma](https://www.figma.com/) to easily localize design** : Figma is to Sketch what Google Docs is to Word. It’s online with built-in collaboration and versioning. Being online first makes it easy to reuse private & public templates for free. For instance,[here is a template to test and export iOS icons](https://www.figma.com/resources/assets/ios-app-icon-template/) easily and another template to export 140[screenshots](https://www.figma.com/file/HdLwisQBYf6d5XxvC5CoTe/App-Store-Screenshots-iPhone-Localized) (14 languages and 2 iOS device formats I make public here).


The geniuses behind Figma made the concept of classes and instances central to the product. Then reusing components for multiple languages and screenshot size became dead simple.


I also used Figma to create the design of the app


## **Process Payment**


**[7.RevenueCat](https://www.revenuecat.com/)** is the new cool guy in town. I remember a few years when including in-app payment doubled the dev time of an app — not to mention subscription. RevenueCat is Stripe for mobile; it saves time. I was a bit skeptical at first, but they do all the heavy lifting for error handling and web reporting (you don’t have to wait 1 day anymore like on iTunes). Then you can also sync them with your analytics and CRM service.


*Big bonus: It’s definitely great for an iOS app! When you go cross platform, I assume it gets amazing. Handling subscription across multiple platform (mobile and web) was a very big pain at GoPro.*


## **App Store Page**


**Figma** again. As mentioned earlier, Figma is the best tool today to easily localize in 14 languages for multiple screens.[Link to my template here](https://www.figma.com/file/HdLwisQBYf6d5XxvC5CoTe/App-Store-Screenshots-iPhone-Localized) .


Screenshot automation with Figma


**[8.Mojo](http://mojo.video/)** : Video Preview & Onboarding. Mojo is an amazing app to create professional-looking stories on Instagram. It turns out preview and onboarding are not far from a story, and you can use Mojo for your app. You can create beautiful motion design for your app preview. They even have an iPhone frame where you can paste a screencast of your app in action.


*Tip 1: Show touch interaction using a Touch Visualiser pod like this[one](https://github.com/morizotter/TouchVisualizer) .*


*Tip 2: My app preview ([you can see it here](https://www.youtube.com/shorts/6ReucPTrQZE) ) was rejected on the second review because it was using an iPhone frame template from Mojo. You might also have to re-encode the video to match iTunes format (Mojo has a bug with the audio track).*


*Tip 3: For the onboarding, I used one Mojo story per page and a UIPageViewController. I am very happy with the result.*


BG onboarding made with a simple UIPageViewController and videos from Mojo app


## **Knowing your Customers**


**[9.Amplitude](https://amplitude.com/) for analytics** : Amplitude is by far the best user-oriented analytics tool. If you’re serious about understanding your users, it’s a no brainer. It optimizes around retention, funnels and active users — which are the only metrics that matter for a user-centered company. Mixpanel is quite similar, but more focused on big cross-platform companies. I have also tried Localytics, but it’s really a CRM tool, and the analytics part is just marketing to say they are a turn-key solution.


**[10.Intercom](https://www.intercom.com/) for talking to your users** : The #1 rule about building a great product is to talk to your customers. Amplitude gives you a quantitative overview; Intercom gives a qualitative overview. A lot of apps use email for feedback, but it’s way too formal, and email is still a pain on mobile. You want to create an environment where feedback is as casual as possible to get the most honest feedback. Since nothing beats a chat interface, Intercom is the best tool here. Intercom is not a mobile-first tool, though; a lot of features come from a web-first approach. I wish you could create native surveys — for instance, move the widget around, suggest pre-written answers, etc.


*Tip: Use the same user id ([Vendor Identifier](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor) to respect privacy) for Amplitude, Intercom and RevenueCat. Then you’ll be able to segment users like paying or very active users in Amplitude and RevenueCat and talk to them in Intercom.*


*2023 Edit: since the launch we moved to a new localization service,[lokalise.com](http://lokalise.com/) . It's much faster and has cleaner APIs*


Thanks for reading! I hope you find one or more of these tools helpful. Also, feel free to share the word by clapping — and I would appreciate it if you give Photoroom a try. You can download it[here](https://itunes.apple.com/us/app/photo-background-changer/id1455009060?mt=8) .
