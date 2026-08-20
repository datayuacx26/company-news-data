---
schema_version: "1.0.0"
document_id: "3641f0d3a4e950cf0385648872e4f89687233c0cc803e9b8cbfad73ee21198a1"
company_key: "yc-onesignal"
company: "OneSignal"
source_id: "yc-onesignal-rss-a77922638bdd"
canonical_url: "https://onesignal.com/blog/live-activities-vs-push-when-a-lock-screen-widget-beats-a-notification/"
published_at: "2026-07-30T23:11:53+00:00"
first_seen_at: "2026-07-31T17:21:07.157075+00:00"
fetched_at: "2026-07-31T17:21:09.425019+00:00"
content_hash: "sha256:c8cb79b3d86ae148ceab132fab23722be89159ddec48aeb9f25d1fb154ae82f6"
---

# Live Activities vs. Push: When a Lock Screen Widget Beats a Notification

*Your delivery is three stops away, and your ride is two minutes out.*


For years, apps told you all of this with a stream of push notifications, one buzz per update, until your Lock Screen looked like a group chat you never joined. Every one of those buzzes costs you a sliver of goodwill, and users pay it back by muting your app or deleting it outright. There's a better way to keep people in the loop without spending down that goodwill one notification at a time: a single, living surface that updates itself instead of interrupting someone a dozen times over.


This article defines[widgets](https://onesignal.com/glossary/widget) ,[push notifications](https://onesignal.com/blog/apple-push-notification-best-practices/) , and[Live Activities](https://onesignal.com/blog/get-started-with-push-to-start-live-activities/) , covers Android's version of the same idea, and gives you a clear rule for when the Lock Screen surface wins.


### What's the difference between a widget, a push notification, and a Live Activity?


These three surfaces get lumped together, but they solve different problems. Here is how they split apart.


**A widget** is a permanent fixture on the Home Screen or Today View. It shows glanceable information such as weather or a calendar, and it needs little lifecycle management because it just sits there and refreshes on its own schedule.


**A push notification** is a one-time alert. It lands in the notification center, waits for a tap, and then it's done. This is the workhorse of most mobile push notification strategies, and any push notification service is built to send them at scale.


**Live Activities are different from both** . They live only on the Lock Screen and in the Dynamic Island. They run for a bounded window of time and are tied to push. iOS Live Activities are built for events with a clear start and end, such as a delivery or a sports match.


> See[20+ examples of Live Activities](https://onesignal.com/blog/best-examples-of-apps-using-live-activities-to-enrich-their-ux/) and how apps from different verticals are taking advantage of this real-time engagement channel.


Historically, many apps sent a barrage of push notifications to communicate real-time transactional status, one alert for every change. Live Activities were designed to replace that barrage with a single, continuously updated surface. Android has a parallel concept called Live Updates, introduced in Android 16, which we cover later in this piece.


### How long can a Live Activity stay on the Lock Screen and Dynamic Island?


A Live Activity has two lifecycle windows, and knowing both keeps your timing honest. First, it can run with active updates for up to eight hours from the moment it starts. After that, or after your app calls an explicit end, it enters a “stale” state that stays visible for up to four more hours before iOS removes it automatically. OneSignal also caps concurrent activities at up to 5 simultaneous Live Activities per app, so it's worth designing for what a user can realistically track rather than stacking more than that. Those specifics come from OneSignal's[Live Activities documentation](https://documentation.onesignal.com/docs/en/live-activities) .


The permission model has a helpful quirk. The first Live Activity an app sends is provisional and doesn't require push permission. Later activities depend on the user's Live Activities setting, per the same OneSignal documentation.


Apple's own[Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/live-activities) show the wider picture. Live Activities appear not only on iPhone and iPad but also in the Mac menu bar, the Apple Watch Smart Stack, and CarPlay. Apple's guidance is direct: alert people only for essential updates rather than over-notifying. Treat the HIG as the primary reference for any Apple-defined behavior.


### How do Live Activities get their real-time updates?


Updates reach a Live Activity through two paths. The first path is an update pushed from an already-running app session. The app is open and active, and it refreshes the activity directly. The second path is a remote update sent through an ActivityKit push notification using a push-to-start token, a capability Apple introduced in iOS 17.2.


Here is the push-to-start flow at a conceptual level. The app requests a push-to-start token. It sends that token to a server. The server later uses the token to start a new Live Activity remotely, without the app being open. OneSignal's[Get Started with Push-to-Start Live Activities](https://onesignal.com/blog/get-started-with-push-to-start-live-activities) guide walks through this end to end. This is Apple's documented model, not a workaround: Apple's own ActivityKit developer documentation confirms a Live Activity can be requested while an app is foregrounded and updated remotely via push.


OneSignal's SDK and API manage the underlying update tokens for you. A single API call scales across every subscriber, and the platform records delivery and engagement analytics for each Live Activity send, per OneSignal's[Live Activities documentation](https://documentation.onesignal.com/docs/en/live-activities) . One detail worth handing to your engineering team early: Apple enforces a dynamic budget on high-priority updates, so mixing standard-priority and high-priority updates, rather than marking everything urgent, avoids getting throttled right when a user needs the update most.


### Do Live Activities work when a phone is locked, silenced, or in Focus mode?


Yes. Live Activities stay visible even in Focus and Do Not Disturb modes, per OneSignal's[Live Activities product page](https://onesignal.com/live-activities) . That is a meaningful edge over a standard push notification, which a user can silence.


The permission model matters here too. Live Activities don't require a user to have opted into push notifications. They have their own separate toggle, and that toggle is on by default in iOS settings.


For marketers, this is a talking point worth explaining clearly to product and growth stakeholders. A channel that reaches users even after they have muted notifications is a distinct value for engagement and retention. It gives you a way to stay useful without adding to the noise a user is actively trying to quiet.


### Android Live Updates: the Android 16 answer to Live Activities


Android has its own native version, and it arrived with Android 16.[Live Updates](https://developer.android.com/develop/ui/views/notifications/live-update) are system-promoted notifications, built on a new[Notification.ProgressStyle](https://developer.android.com/about/versions/16/features/progress-centric-notifications) API, that appear more prominently across the lock screen, the top of the notification shade, and a compact status bar chip. Android reserves this treatment for activities that are ongoing, user-initiated, and time-sensitive, such as active navigation, rideshare tracking, or food delivery, and apps have to explicitly request the permission and the promotion to qualify.


OneSignal's current Android implementation, documented as[Android Live Notifications](https://documentation.onesignal.com/docs/en/android-live-notifications) , takes a different route to a similar experience: a structured payload is sent over standard OneSignal push, intercepted by a Notification Service Extension inside the app, and used to update a single persistent notification in place using a shared collapse ID. It's a practical pattern that works across a wide range of Android versions today, rather than a wrapper around Android 16's newest promoted-notification APIs specifically, so it's worth asking your engineering team which layer they want to build against as Android's native support matures.


One platform difference is important to plan around. Android notifications, including OneSignal's Live Notifications, require the end user to have push notification permission enabled, unlike iOS, where the first Live Activity is provisional and needs no permission.


On a real device, the flow is straightforward. A user starts an action such as navigation or a delivery, and the update shows up as a lock screen card and, where supported, a compact status bar chip. If you're used to thinking of the lock screen surface as an iOS-only story, Android has closed that gap. Plan your cross-platform status experiences with both in mind.


### Live Activities and Live Updates vs. standard and rich push: a side-by-side comparison


Here is how the four surfaces compare across the dimensions that decide which one to reach for.


Criteria


iOS Live Activities


Android Live Updates


Standard push


Rich push


Purpose


Ongoing, time-bound status


Ongoing, time-bound status


One-time alert


One-time alert with media


Location & duration


Lock Screen and Dynamic Island; up to 8 hours active, then up to 4 hours stale


Lock screen, notification shade, status bar chip; runs for the activity's duration


Notification center; transient


Notification center; transient


Update mechanism


Server push (ActivityKit) or local app update


Native: system ProgressStyle API. Via OneSignal today: structured payload over push, updated in place with a collapse ID


Server push


Server push


Interactivity


Glanceable, limited actions


Glanceable, limited actions


Tap to open


Tap, with image or media preview


Permission


Provisional first activity, then user toggle (on by default)


Requires push notification permission enabled


Requires push permission


Requires push permission


Persists in Focus / DND


Yes


Persistent notification


Can be silenced


Can be silenced


Best-fit use cases


Deliveries, rides, live scores, workouts


Deliveries, navigation, download progress, live scores


Reminders, single alerts, re-engagement


Promotions, media-rich announcements


### When should you send a Live Activity instead of a push notification?


Use this rule. An ongoing, time-bound event with a defined start and end fits a Live Activity or Live Update. A one-time alert that needs an action or a decision fits a push notification.


The market has already proven the baseline cases, and they line up closely with the categories OneSignal sees most often across its own customer base:


- Delivery and rideshare tracking
- Live sports scores
- Fintech and crypto price or trade alerts
- Fitness and workout tracking
- Gaming stats, matches, and leaderboards
- Flight and travel status


Apple's Human Interface Guidelines add a rule worth following: don't send a redundant push notification for an update an active Live Activity is already showing. Doing so undermines the calm, glanceable value that makes the surface worth using. OneSignal's own[Live Activities documentation](https://documentation.onesignal.com/docs/en/live-activities) describes a clean way to enforce this: tag a user once they opt into a Live Activity, then exclude that tag from the push segment carrying the same update, so the two channels never step on each other.


The lock screen widget wins when a user wants to watch a situation unfold over minutes or hours. A push notification still wins when you need a single tap, a decision, or a re-engagement nudge that stands on its own.


### Does cutting notification volume with Live Activities actually improve engagement?


Start with the scale of the problem. Independent research aggregated by Business of Apps puts the average U.S. smartphone user at around 46 app push notifications a day, and most users say they'd prefer one or fewer. Every status change you send as its own alert adds to that pile, and pushes you further from what people actually want.


Live Activities consolidate a series of status updates into one persistent, glanceable card instead of a new push for every change. The information stays accessible without stacking up alert after alert. It's also not just a theory: OneSignal's own[State of Customer Engagement data](https://onesignal.com/2026-state-of-customer-engagement-report) shows apps using iOS Live Activities see 23.7% higher average 30-day retention than apps that don't.


The payoff ties back to what mid-market teams answer for: engagement, retention, and long-term customer value. A user who can glance at a delivery card is less likely to mute your app entirely, which protects the push channel you still need for one-time alerts.


Be clear with your stakeholders that push notifications and Live Activities work best as a pair: one carries the one-time alert, the other carries the ongoing status.


### How to implement Live Activities and Live Updates with OneSignal


OneSignal provides an SDK that integrates messaging into your app. It supports push across mobile and web with advanced targeting and automation, plus a full API reference for automating messaging, managing users, and tracking delivery. The setup below sits as a practical layer on top of Apple's ActivityKit and HIG reference docs.


### Setting up iOS Live Activities


Follow this sequence from OneSignal's[Live Activities developer setup guide](https://documentation.onesignal.com/docs/en/live-activities-developer-setup) :


- Add a Widget Extension in Xcode with “Include Live Activity” selected.
- Set the “Supports Live Activities” key to YES in Info.plist.
- Add the OneSignal framework to the widget extension target.
- Call OneSignal.LiveActivities.setup in the AppDelegate after SDK initialization.
- Send a Push To Start API request to launch the first activity remotely.


Mind the SDK version requirements from the same guide. You need OneSignal iOS SDK 5.2.0 or later for push-to-start support, and 5.2.15 or later for click tracking and confirmed receipt.


### Setting up Android Live Notifications


For Android, follow this sequence from OneSignal's[Android Live Notifications documentation](https://documentation.onesignal.com/docs/en/android-live-notifications) :


- Implement and register a Notification Service Extension in the Android Manifest.
- Define a structured live_notification payload.
- Send start, update, and end events through the Create Message API using a shared collapse ID.


The shared collapse ID is the key detail. It makes each update land on the same notification instead of stacking new ones, which is the whole point of a live, in-place surface. As noted earlier, this only appears once the end user has push permission enabled.


### Bringing Live Activities, Live Updates, and push together in one strategy


The decision comes down to the same rule laid out earlier: match ongoing, time-bound status to a Live Activity or Live Update, and match one-time, action-driving alerts to a push notification.


Treat these as complementary channels that each handle a different job. Used together, they cut notification fatigue by consolidating status into one glanceable card, while preserving the push channel for the moments that need a decision or a tap.


OneSignal lets your team manage push, Live Activities, and Android Live Notifications from one platform. Map your time-bound events to the lock screen, keep your one-time alerts on push, tag users so the two channels don't duplicate each other, and let each surface do the job it's built for.


[Get Started for Free](https://app.onesignal.com/signup)


###


Frequently asked questions


#### How do iPhone users enable or disable Live Activities?


Users control Live Activities from their iOS settings, and the toggle is on by default. Many apps also expose a per-app Live Activities switch inside Settings for that app, so a user can turn the surface off for one app without disabling it everywhere. Since Apple periodically moves settings between menus across iOS versions, point users to their device's own Settings search if the exact path doesn't match what you remember.


#### How many Live Activities can run on one device at the same time?


A device can run multiple Live Activities from different apps at once, and they stack on the Lock Screen. The Dynamic Island shows one expanded activity at a time and compacts the rest. OneSignal caps this at up to 5 simultaneous Live Activities per app; keep concurrent activities to what a user can genuinely track, because too many defeats the glanceable value the surface is built for.


#### Can a Live Activity be dismissed before it ends automatically?


Yes. A user can swipe a Live Activity away from the Lock Screen, and your app can end one programmatically with an explicit end call. You can also set a **dismissal_date** on that end call to control exactly when iOS removes it: a future date within the next four hours removes it sooner, and a past date removes it immediately (the user must have allowed the first Live Activity for that immediate removal to work). Without a **dismissal_date** , iOS removes the activity after the four-hour stale period or whenever the user dismisses it, per OneSignal's[Live Activities documentation](https://documentation.onesignal.com/docs/en/live-activities) .


#### Is there an extra cost to send Live Activities on OneSignal?


Live Activities are included on every OneSignal plan, including Free, up to 10,000 opted-in push subscribers. Past that volume, you'll need to move to a paid plan, where the subscriber ceiling scales up with your tier. Check OneSignal's[pricing page](https://onesignal.com/pricing) for current limits by plan, or reach out to OneSignal if you want to confirm what your plan includes before you build.


#### How do Android users turn on Live Updates?


On Android 16, native Live Updates depend on the app's push notification permission and the OS-level promoted-notifications setting for that app, both managed from the device's notification settings. Once both are on, starting a supported action such as navigation or a delivery generates the update on the lock screen and, where supported, the status bar chip.


#### Can Live Activities be used for promotions or marketing offers?


They can, but tread carefully. Apple's guidance is to reserve the surface for essential, time-bound updates a user actually wants to watch, so a time-limited flash sale countdown fits better than a generic promo. For a standalone marketing offer, a rich push notification is usually the stronger fit.


#
