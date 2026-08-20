---
schema_version: "1.0.0"
document_id: "aff49c6cf755fbe5961fb74f60029b82cecbb7886a892906257c3e65e3b97e67"
company_key: "yc-bitrig"
company: "bitrig"
source_id: "yc-bitrig-news-import-267282887e75"
canonical_url: "https://bitrig.com/blog/testflight-for-beginners"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T15:36:09.774643+00:00"
fetched_at: "2026-08-05T15:36:11.362737+00:00"
content_hash: "sha256:6732737c5ed80ad431ec9c118601aefad716c1a04d4bf52771b5967429ab0d2d"
---

# What Is TestFlight? Beta Testing for Beginners

TestFlight is Apple’s official service for beta testing apps before they’re released on the App Store. It lets you distribute unfinished versions of your app to testers, collect feedback, crash information, and fix problems before real customers find them.


You can use TestFlight to test apps for iPhone, iPad, Mac, Apple Watch, Apple TV, and Apple Vision Pro. Instead of manually installing your app on every tester’s device, you upload a build to App Store Connect and invite people to install it through Apple’s free TestFlight app.


TestFlight is where your app starts meeting real users. Those users will tap buttons you never expected them to tap, use devices you don’t own, and find problems you completely missed during development.


##
What is TestFlight?


[TestFlight](https://developer.apple.com/testflight/) is a beta distribution and feedback service provided by Apple.


A beta is a version of your app that’s still being tested. It may be close to finished, but it hasn’t officially launched on the App Store yet.


With TestFlight, you can:


-


Upload beta builds of your app


-


Invite internal team members to test


-


Invite people outside your development team


-


Organize testers into groups


-


Distribute updates to testers


-


Collect written feedback and screenshots


-


Review crash reports and testing metrics


-


Test changes before submitting them to the App Store


TestFlight is managed through[App Store Connect](https://bitrig.com/blog/app-store-connect) , which is Apple’s website for managing your app’s distribution, product page, pricing, analytics, TestFlight builds, and App Store submissions.


##
Do you need a paid Apple Developer account for TestFlight?


Yes. You need an active, paid Apple Developer Program membership to distribute an app through TestFlight.


You can learn Swift, build apps, use Xcode, and install an app on your own devices with a free Apple developer account. However, the free account does not include TestFlight distribution.


The standard Apple Developer Program membership costs $99 per year in the U.S. (as of 2026). You can learn more about the Apple Developer Program in our[guide](https://bitrig.com/blog/apple-developer-program-free-vs-paid) .


##
How does TestFlight work?


The TestFlight process can be broken down into a few basic steps.


###
1. Create your app in App Store Connect


Before you can upload a TestFlight build, your app needs an App Store Connect record.


This record represents your app inside Apple’s distribution system. It includes information such as the app’s name, bundle ID, supported platforms, pricing, App Store listing, and uploaded builds.


Creating the record does not publish your app. It simply gives Apple a place to process and manage it.


###
2. Upload a build


A build is a compiled version of your app that can be installed on a device.


You can upload a build through Xcode or Bitrig can[archive your app and upload its builds](https://bitrig.com/blog/bitrig-publishes-to-the-app-store) to App Store Connect for you.


After the upload, Apple processes the build. Once processing is complete, the build appears under the TestFlight tab in App Store Connect.


###
3. Create a testing group


TestFlight lets you organize testers into groups and decide which builds each group can access.


For example, you might create separate groups for:


-


Your development team


-


Friends and family


-


Existing customers


-


A private launch group


-


People testing a specific feature


-


Testers using a particular language


-


Testers with a particular device or operating system


I once created a dedicated TestFlight group for native Spanish speakers while localizing one of my apps. That let me send the translated version to the people most likely to notice awkward wording or text I had forgotten to translate.


Groups make it much easier to test with a purpose.


###
4. Invite testers


You can invite internal testers from your App Store Connect team or add external testers who aren’t part of your development account.


Once someone accepts the invitation, your beta app appears in their TestFlight app. They can install it, receive new builds, and submit feedback.


###
5. Collect feedback and fix problems


As people use your app, TestFlight can collect information about sessions, crashes, devices, operating system versions, and tester feedback.


You can use that information to fix bugs, improve confusing screens, test your design, and prepare another build.


Then you upload the updated build and repeat the process.


##
Internal vs. external TestFlight testers


TestFlight has two main types of testers: internal testers and external testers.


Internal testers


External testers


Who they are


Members of your App Store Connect team


People outside your App Store Connect team


Maximum number


100


10,000


Common uses


Testing during active development


Wider beta testing before release


TestFlight review


Generally not required


The first build requires TestFlight App Review


Invitation method


Added through App Store Connect


Email invitation or public link


Good examples


Developers, designers, product managers


Friends, customers, community members, localization testers


These limits are current as of August 2026 and may change in the future.


###
What are internal testers?


Internal testers are people who already have access to your app through your App Store Connect team.


This might include:


-


Other developers


-


Designers


-


Product managers


-


QA testers


-


Marketing team members


-


Business partners working on the app


Apple currently allows up to 100 internal testers. They must be App Store Connect users with access to the app’s content.


Internal testing is useful while you’re still actively building. You might distribute a build so your teammates can test a new onboarding flow, verify a bug fix, or review a feature that isn’t ready for outside users yet.


Because these people are part of your App Store Connect team, internal builds can generally begin testing without going through TestFlight App Review.


###
What are external testers?


External testers are people who aren’t members of your App Store Connect team.


These might be:


-


Friends and family


-


Newsletter subscribers


-


Existing customers


-


Members of an online community


-


Potential users


-


Native speakers reviewing a localization


-


People with devices you don’t personally own


Apple currently allows up to 10,000 external testers per app, so TestFlight can support anything from a small private test to a fairly large public beta.


External testing is usually the next step after your team has completed its initial internal testing and you feel comfortable letting people outside the project use the app.


##
How do you invite external TestFlight testers?


There are two main ways to invite external testers: email invitations and public links.


###
Email invitations


With email invitations, you add each tester’s email address to App Store Connect.


Apple sends that person an invitation explaining how to join the beta. After accepting it, the tester can install your app through TestFlight.


Email invitations are useful when you want a controlled, curated beta.


For example, you might use email invitations for:


-


A small group of trusted customers


-


Clients or business partners


-


People who agreed to test a particular feature


-


Localization testers


-


A private beta with limited access


The main downside is that you need to collect and manage everyone’s email address.


###
Public TestFlight links


A public link gives you a URL that anyone can use to join your beta, provided the testing group still has space and they meet any criteria you configure.


You can share the link through:


-


Social media


-


Your website


-


A newsletter


-


A Discord or Slack community


-


Messages


-


A private customer group


As of 2026, Apple also lets you limit the number of people who can join through a public link and filter eligibility by device, platform, or operating system version.


A public link is the easiest option when you want to reach more people without manually collecting email addresses.


Just remember that anyone who receives the link may be able to share it with someone else. **Don’t use a wide-open public link if your beta needs to remain tightly controlled** .


##
Does Apple review TestFlight builds?


Builds distributed to external testers may need to pass TestFlight App Review.


The first external build you submit requires a full review. Later builds for the same app version might not require another full review, although Apple can still review them.


This review is separate from the full App Store review required to publicly release your app. Passing TestFlight review does not guarantee that your final App Store submission will be approved.


Apple does not publish a guaranteed TestFlight review time. It's typically within 48 hours, but that's not always the case. Plan ahead rather than uploading a build immediately before a scheduled beta launch.


That’s an easy detail to forget when you’re excited to get a new version in front of testers. Give yourself some breathing room.


##
How long do TestFlight builds last?


Each TestFlight build is available for up to 90 days, starting on the day it was uploaded. After 90 days, testers can no longer open that build.


Uploading a newer build gives the new build its own 90-day testing period. It does not extend the expiration date of the older build.


For example:


1.


You upload build 1 on September 1.


2.


Testers begin using it and send you feedback.


3.


You fix several bugs and upload build 2 on September 8.


4.


Build 2 receives a new 90-day testing window.


5.


Build 1 still expires based on its original September 1 upload date.


TestFlight is intended for temporary beta testing. It is not designed to be a permanent alternative to releasing an app through the App Store or another approved distribution method.


##
What feedback can TestFlight testers submit?


Testers can submit screenshots, written comments, and feedback related to crashes.


That feedback appears in the TestFlight section of App Store Connect, where you can view details such as:


-


The screenshot the tester submitted


-


The tester’s written comments


-


The app version and build


-


The device they were using


-


Their operating system version


-


Crash details, when available


You can also open supported feedback and crash information in Xcode to investigate what happened in your code.


This is where TestFlight becomes much more valuable than simply asking someone, “Did the app work?”


A tester can show you the exact screen where something went wrong and explain what they were doing at the time.


While testing a Spanish localization, for example, one of my testers found an alert I had forgotten to translate. It only appeared when the app was set to Spanish and the user canceled an in-app purchase partway through the process.


That was a very specific scenario I had missed during my own testing. A real user found it almost immediately.


You can test your app a hundred times and still follow roughly the same paths each time. Beta testers bring different habits, devices, languages, accessibility needs, and assumptions.


##
A beginner TestFlight checklist


Before inviting people to your beta, work through this checklist:


1.


**Create the app in App Store Connect.**
Confirm that the app record uses the correct bundle ID and platform.


2.


**Upload a working build.**
Test it yourself before sending it to other people.


3.


**Add clear testing instructions.**
Tell testers which features changed and what you want them to focus on.


4.


**Create focused testing groups.**
Separate internal testers, general beta users, localization testers, and feature-specific testers when useful.


5.


**Choose the right invitation method.**
Use email for a controlled beta and a public link when you want broader participation.


6.


**Allow time for external review.**
Don’t assume a new external build will be approved immediately.


7.


**Monitor crashes and feedback.**
Check App Store Connect regularly instead of waiting for testers to contact you elsewhere.


8.


**Upload new builds as you make improvements.**
Include useful “What to Test” notes so testers know what changed.


9.


**Watch the 90-day expiration date.**
Upload another build when testing needs to continue.


10.


**Thank your testers.**
Good beta testers are giving you their time and helping you ship a better app.


##
Frequently asked questions about TestFlight


###
Is TestFlight free?


The TestFlight app is free for testers to download. Developers need a paid Apple Developer Program membership to upload and distribute their own apps through TestFlight.


###
Is TestFlight only for iOS apps?


No. TestFlight supports apps across Apple platforms, including iPhone, iPad, Mac, Apple Watch, Apple TV, and Apple Vision Pro.


###
How many people can test an app with TestFlight?


As of August 2026, an app can have up to 100 internal testers and up to 10,000 external testers.


###
Do TestFlight testers need an Apple developer account?


External testers do not need to join the Apple Developer Program. They need a compatible Apple device, the free TestFlight app, and an invitation from the developer.


Internal testers must be users on the developer’s App Store Connect team.


###
Does a TestFlight app need to pass App Review?


The first build distributed to external testers requires TestFlight App Review. Later builds may not require a full review.


Internal testing generally does not require TestFlight App Review. A separate App Store review is still required before the app can be released publicly.


###
Can anyone join a TestFlight beta through a public link?


Anyone with the link can attempt to join, but the developer can set a tester limit or restrict the beta to particular devices, platforms, and operating system versions.


The group must also have space available.


###
What happens when a TestFlight build expires?


The build stops opening after its 90-day testing period ends. The developer must upload and distribute a newer build if testing needs to continue.


###
Can you use TestFlight instead of publishing on the App Store?


Not permanently. TestFlight builds expire after 90 days and are intended for beta testing. TestFlight should not be used as a long-term replacement for an App Store release.


##
The bottom line


TestFlight is Apple’s official way to beta test your apps before releasing them on the App Store.


You upload a build to App Store Connect, assign it to internal or external testing groups, invite testers, and use their feedback and crash reports to improve the app. Internal testing is best for your immediate team, while external testing helps you learn how the app performs with friends, customers, community members, and other real users.


If you’re getting close to launching your first app, don’t skip this step. A focused TestFlight beta can uncover crashes, confusing designs, untranslated text, and unusual edge cases before they reach paying customers.


[Bitrig](https://bitrig.com/) helps new developers build native Swift apps with AI and can[handle](https://bitrig.com/blog/bitrig-publishes-to-the-app-store) the process of archiving and uploading builds to App Store Connect. Once your app is ready for real-world feedback, you can distribute it through TestFlight and keep improving it before launch.
