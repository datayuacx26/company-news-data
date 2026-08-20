---
schema_version: "1.0.0"
document_id: "b4bb790f4d174eb82e2ade4bff12749f2ccca8dba8a4564a2791221abb1e22d0"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/ios-signals"
published_at: "2017-03-08T15:21:57+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T22:27:28.230343+00:00"
content_hash: "sha256:f20526713812463d3fedb699ac4c9e163048c6a596a2fc94ddb69826ad304764"
---

# iOS Data Binding is Better with Signals

Here at Rent the Runway, we want to make sure that all of our users have a responsive and rewarding iOS mobile app experience. With our backend architected to use many highly performant microservices, there is a whole lot of data and user interactions to keep in sync. We have services for user memberships, promotions, products, search categories, user hearts, and so much more. Without a one-to-one mapping of API service calls to application views, it was critical that we come up with a robust and coordinated approach for data binding and content updates.


One of the main iOS communication patterns is delegation. Delegation creates a one-to-one messaging structure between two objects, where one object can act on behalf of another. Using this pattern, it is typical that a completed API call would inform its delegate object that new data exists and is ready for processing and/or display somewhere else in the app.


On this diagram we can see that we have several UIViewControllers (ProductCollectionsVC, ProductsGridVC, ProductVC) that hold a ModelController (HeartsMC), which is responsible to fetch all the hearts from a user.
While this approach is useful in many situations, it makes it difficult for communicating data updates from one object to many objects at the same time as we can see on the diagram.


As an alternative to delegates, we can use[ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa) ’s framework to use signals for communication. In short ReactiveCocoa is a functional reactive programming framework which represents dataflows by the notion of events. An event is just a concept that represents that something happened, which could represent a button being tapped or the status of a network call being made. ReactiveCocoa uses Signals and SignalProducers to send messages representing these events. A Signal will only notify an observing object of events that have occurred after the observer has started observing, not any past actions. On the other hand, a SignalProducer allows you to start observing for an event but also have the visibility for past and future events.


ReactiveCocoa explains these concepts by comparing them to a person (the observer) watching television (the signal or signal producer).


> “
>
>
> \[A signal\] is like a live TV feed — you can observe and react to the content, but you cannot have a side effect on the live feed or the TV station. \[A signal producer\] is like a on-demand streaming service — even though the episode is streamed like a live TV feed, you can choose what you watch, when to start watching and when to interrupt it. ”


— https://github.com/ReactiveCocoa/ReactiveSwift


Signals work very well for UI events like tapping a button, where an observer would only be interested in reacting to touch events after it has started observing. A Signal Producer would be more useful for a network request, where its on-demand nature would help provide more contextual information about a network request, such as allowing an observer to know if it is in progress, has it previously failed, should we cancel it, etc.


Objects throughout our app can then observe these event-driven signals and take any action they need to bind data to views, process data, or trigger any other actions. This allows multiple views and viewControllers in our app to be notified of data changes at the same time without the need for complex delegation chains.


In the code snippet above, we are setting up signals for a modelController. When it receives a signal, it can call a method that is defined by our protocol. If we were to configure these methods and interactions with a delegate, other ViewControllers would not receive any message and would not know to update their data.


In the app, we have a TabBarController that allows users to quickly switch between viewControllers that provide different functionality. However, we do have some shared data sources between these controllers, so our signals allow us to update data on every accessible viewController so the user can interact with them immediately after switching tabs.


This approach has been very helpful for broadcasting changes of the datasource on a ModelController throughout the app. While implementing, we paid special attention to weak/strong references with our observers to prevent retain cycles, routinely testing that objects were being deallocated when needed. Additionally, deadlocks can occasionally occur if signals trigger other signals that are dependent on the completion of another signal. However, with well thought out protocols and implementation patterns, we have been able to provide a responsive and stable app experience for our users.
