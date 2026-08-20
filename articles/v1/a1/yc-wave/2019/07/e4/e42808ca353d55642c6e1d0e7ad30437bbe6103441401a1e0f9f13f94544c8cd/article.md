---
schema_version: "1.0.0"
document_id: "e42808ca353d55642c6e1d0e7ad30437bbe6103441401a1e0f9f13f94544c8cd"
company_key: "yc-wave"
company: "Wave"
source_id: "yc-wave-rss-404ae0f13af9"
canonical_url: "https://www.wave.com/en/blog/android/"
published_at: "2019-07-29T16:08:29+00:00"
first_seen_at: "2026-07-24T06:52:44.556575+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:41323439cdc82b427337842fb02987b416fbc594bc267f0343737416e47a6add"
---

# Our Android framework

# Our Android framework


Ben Kuhn


on


July 29, 2019


engineering


---


We built our first Android app in 2014. Like most 2014-era Android apps, it was a mess of fat Activities, heavily-nested callbacks, and spaghetti dataflow. Fortunately, since then Google has iterated a lot on their Android best practices, and we’ve[iterated](https://www.wave.com/posts/split/) a lot on our apps, giving us plenty of chances to make things better.


During the most recent time we rebuilt our agent app, we looked at the new[androidx](https://developer.android.com/jetpack/androidx) libraries and decided that they *almost* met our needs–but they left a few crucial gaps that still made writing Android apps more painful than it should be. So we built a lightweight set of components and patterns that work together to smooth over those final wrinkles and make Android development truly tolerable.


The rest of this post is an excerpt from our internal documentation explaining how these components work together.


---


**Goal of our framework:** eliminate the crappiness of default Android development by…


- Keeping Activity classes minimal, usingViewmodels andActions to decouple the code from the Android UI layer, making it easy to test
- UsingLiveData to eliminate the toil of propagating data from the viewmodel to the UI
- Using theRepository and GraphQL to eliminate all bookkeeping associated with tracking server-originated data
- UsingCoroutines to eliminate callback hell
- UsingTyped Intents to make it type-safe and boilerplate-free to transition between activities


## First, understand the basics


- Learn what an Activity (“a thing that occupies the screen”) is ([intro](https://developer.android.com/guide/components/activities/intro-activities) ), and its[lifecycle](https://developer.android.com/guide/components/activities/activity-lifecycle)
- Learn how Android UIs are specified ([XML](https://developer.android.com/guide/topics/ui/declaring-layout#write) )
- Kotlin[intro](https://kotlinlang.org/docs/tutorials/kotlin-for-py/introduction.html)
- GraphQL[intro](https://graphql.org/learn/)


## Major concepts


### ViewModels


Relevant Android docs:[viewmodels](https://developer.android.com/topic/libraries/architecture/viewmodel) ,[databinding](https://developer.android.com/topic/libraries/data-binding)


**Historical context:** In the bad old days, all your UI-related code (and unless you were thoughtful, all your code period!) lived in Activity subclasses. Your Activity would start out by inflating some XML layout. It would search through the resulting views for the ones it cared about and add event handlers to them. The event handlers would trigger some updates, and the updates would run hand-written glue code that propagated data back to the UI. This had many downsides:


- To test effectively you need to run code that depends on Android libs, which means running code on an actual Android phone to test effectively, and that’s really flakey.
- Activities take up a lot of memory and it’s easy for the Activity to trigger background work that retains a reference to the Activity, causing the Activity never to get garbage-collected.
- Android destroys and recreates activities all the time—for instance, when you rotate the screen, switch to a different application, or just because they felt like it. By default this destroys all UI state, but ViewModels can survive these destroy/recreate events.


**New hotness:** Instead, the Activity constructs a ViewModel class, and the ViewModel doesn’t have any dependencies on Android libraries. The layout XML gets “bound” to the view model, which means that you write little expressions in the XML, and an Android tool called the “databinding compiler” generates a class named e.g.` AgentHomeBindings` that glues the UI to the ViewModel. To run business logic, the bindings glue UI events to ViewModel methods.


### Actions


Wave’s docs:` Actions.kt`


If the ViewModel doesn’t depend on Android, what happens when our business logic needs to do something Android-specific (like launch an activity, or present a dialog)? In this case, the viewmodel uses an` Actions` instance.` Actions` is an interface that defines functions, like` showDialog` , that the ViewModel can invoke but which require cooperation from the Activity. When the ViewModel’s Activity is running, its` actions` instance is an object that talks to the Activity and tells it to do UI stuff; when the Activity is inactive, the corresponding ViewModel’s actions become a “no-op” instance to avoid holding a reference to the Activity. In tests, the actions are faked so that we can test ViewModel behavior without needing an Activity (or anything from the Android framework) at all.


### LiveData


[Relevant Android docs](https://developer.android.com/topic/libraries/architecture/livedata)


LiveData is the blessed Android abstraction for a variable where you can get notified if it updates. This is how data flows from the ViewModel to the UI—the databinding compiler (mentioned above) generates code that subscribes to each relevant LiveData, takes their updates and sticks them in the appropriate place in the UI. Generally, a ViewModel’s main job is to construct a tree/DAG of LiveData instances, and then receive callbacks that update the LiveDatas (or do other business logic, e.g. hit the server).


(Note that Android supports *bidirectional* data flow too—for instance, a LiveData connected to the text field, where the user edits and the ViewModel both change the livedata value. We prefer not to do this, because bidirectional data flow is confusing. Instead, we have the UI “request” a change to the LiveData through a callback, so that the ViewModel is the sole authority on what the UI state should be. This prevents hard-to-test ViewModel↔UI interaction bugs.)


### Repository


[Underlying GQL library (Apollo-Android) docs](https://www.apollographql.com/docs/android/essentials/get-started/)


Inspired by:[Relay](https://relay.dev/)


The` Repository` deals with fetching and caching data from the server, and submitting updates. Its main nicety is that you can submit a GraphQL query, and then subscribe to updates to it—even updates that the Repository found out about via a different query or mutation. For instance, the Chooser screen fetches the logged-in user’s balance via the UserQuery operation, and subscribes to the result. If the user later sends a transaction, it will invoke a SendMutation, which will re-fetch the user’s balance, but via a different fragment. The Repository will be smart enough to know that the two balances belong to the same user, and will update the balance on screen.


The Repository is a wrapper around the[apollo-android](https://www.apollographql.com/docs/android/essentials/get-started/) GraphQL library, which generates (strongly-typed) Java classes for every` .graphql` query/mutation we write and implements the caching/subscribing infrastructure.


The Repository knows which objects have been updated at an appropriately granular level (e.g., the Wallet in the above example) by keying on the ID columns returned by your queries, so you should generally be requesting “id” in most of your queries and mutations. Internally it has a disk-based persistent cache called the “normalized cache”; Apollo-android provides most of the implementation of the cache but we’ve overridden certain aspects of the cache key generation.


### Coroutines


[Relevant Kotlin docs](https://kotlinlang.org/docs/reference/coroutines-overview.html)


**Historical context:** Like most UI toolkits, Android requires some computation (that interacts with views) to happen on the main thread. But no *long-running* computation can happen on the main thread, because the main thread is also responsible for handling user input. So if you block, the UI will lock up.


So what happens if you want to run a computation that takes a long time (e.g. a network request) on the main thread? You can’t just write` result = doNetworkRequest(params)` , because` doNetworkRequest` could lock up the UI for seconds. The default way of solving this is to write` doNetworkRequest(params, callback)` , where` callback` is a function (or an anonymous inner class, because this is Java) accepting` result` as a parameter. This leads to super-deep nesting of code.


**New hotness:** A *coroutine* (in Kotlin, a function prefixed by the` suspend` keyword) is a function that has the ability to “yield” to something else on the main thread, and be resumed at some point in the future. They are written like normal functions, and the compiler turns them into effectively the same type of callback-oriented code (called *[continuation passing style](https://labs.pedrofelix.org/guides/kotlin/coroutines/coroutines-and-state-machines)* ) under the hood.


Every time normal Android development uses callbacks, we try to use coroutines instead—they lead to dramatically cleaner and more readable code.


### Typed Intents


**Historical context:** Different Android activities can only communicate with each other using “Intents.” An Intent is basically a dict with string keys and values that can be various different primitive types. Notably, Intents can’t carry objects, because they are supposed to be able to cross process/application boundaries. Even though they’re mostly used for communicating within applications. (Yeah, it doesn’t make much sense.) Like most other Android things, Activities also return results to their caller via a callback (` Activity.onActivityResult` ), and it’s the same callback for every activity—to figure out which activity’s result you’re handling you need to switch on the intent’s “result code.”


**New hotness:** We built a system called` TypedIntents` that hides all the Intent nonsense behind (a) a coroutine-based wrapper for the callbacks, and (b) a type-safe system where you declare an activities “params” and “return” types as Kotlin data classes, and they get turned into Intents for you behind the scenes. Inherit from` TypedWaveActivity` to allow other people to call you with types. Use` Actions.call` to call another typed activity.


### Flows


Almost all of the business-logic functions in ViewModels follows the same formula:


- Do a bunch of (suspending) actions.
- If one of them raises a UserFacingError, display it to the user
- If the user cancelled the sequence of actions (e.g. by hitting “cancel” on a dialog), do nothing
- If any of your assumptions (about e.g. the data returned by the server) is violated, instead of crashing, display “an unknown error occurred”


To factor out the common error handling, we wrote a wrapper` launchFlow` . Most ViewModel callbacks look like` fun foo(args) = launchFlow { ... }` . Inside the braces, user-facing errors will be caught and displayed in a dialog. And you can use` nullableObj.or(err)` as a replacement for` obj!!` to present “an unknown error occurred” instead of crashing if the object is null.


---


We work on Wave because we think it’s[an extremely effective way to improve the world](https://www.wave.com/en/blog/world/) . If that’s how you want to spend your career too,[come work with us](https://www.wave.com/en/careers/) !


---


If you liked this post, you can subscribe to[our RSS](https://www.wave.com/en/blog/index.xml) or our mailing list:
