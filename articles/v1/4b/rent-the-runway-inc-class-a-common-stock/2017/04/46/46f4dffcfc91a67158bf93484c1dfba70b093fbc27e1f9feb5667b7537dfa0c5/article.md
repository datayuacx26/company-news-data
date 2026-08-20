---
schema_version: "1.0.0"
document_id: "46f4dffcfc91a67158bf93484c1dfba70b093fbc27e1f9feb5667b7537dfa0c5"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/viewcontroller-transitions"
published_at: "2017-04-25T16:03:23+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T22:27:28.230343+00:00"
content_hash: "sha256:1f443ed2cf9cafbb8cc996ff6b0acf6a465f9663e81e9a4dd07a7bd3262265a5"
---

# Efficient ViewController Transitions

Anyone familiar with RTR’s app knows that there are several different viewControllers that the user is able to navigate between; however, anyone *not* familiar with the code would most likely assume this navigation is done through simply pushing the relevant viewController via the navigation controller or by presenting the viewController modally (ex below).


Prior to joining the team, I would have made the same assumption. This assumption is ***wrong!*** You are probably now asking yourself “what other way is there to navigate between viewControllers?” Good question!


To handle efficient routing, we implemented a unique pattern to navigate between viewControllers. All of the data on the app’s viewControllers are populated by modelControllers. With that in mind, when a viewController is instantiated, the viewController’s initialization requires a modelController as a parameter. You don’t *have* to do it this way, but for reasons related to a whole different (future) blog post, we gather all the relevant information from the network at an earlier stage and persist this information throughout the app’s lifecycle.


After having created the viewController and established which modelController the viewController “owns,” it is time to create the route that will link to this viewController. RTR’s app has an entire class (AppRouter) dedicated to routing. One thing to note: we use JLRoutes, a cocoa pod that manages routes for URL schemes, to handle routing within the app. The main function we are concerned with inside AppRouter is adding a new route, which is a function provided by JLRoutes.


As you can see from the code above, addRoute requires a path as a parameter. We store all paths required for routing in a class, RTRURL. The enum Route (below) is the path to the specific viewController and the enum Param (also below) holds the paths to any parameters required to instantiate the viewController. In this example, case Experiment is the path to experimentsMC, which is the modelController that is required to initialize RoutingVC.


Great! We now have a way of routing the app to a given viewController based on the viewController’s path provided in the addRoute func. So…how do we present it?


We made a class, Presentation, that subclasses from RTRURL. This means that Presentation has access to all of the properties held in RTRURL. Yes, the code inside Presentation *could* be done inside RTRURL. However, we made this Presentation class as a way of abstracting implementation details to make the code easier to read. 👏


In order to “tie together” all of the components that contribute to navigating to a given viewController (aka creating a new route and providing the viewController’s route path), we decided to create a “route struct” for every viewController. Specifically, this struct encapsulates all of the information required to instantiate the viewController. It does this by adopting and conforming to the PresentationProtocol.


PresentationProtocol has three requirements: a path, required parameters and valid parameters. What does this mean, exactly? With this particular example, RoutingVC can’t be instantiated without an experimentsMC; therefore, this property *must* be provided in the RequiredParams. Similarly, a new route can’t be added without a path, thus, a path must be provided.


ValidParams takes in properties that can be displayed by the viewController but aren’t *required* in order to instantiate the viewController. ValidParams is encapsulated within the PresentationProtocol but can be an empty array. An example of ValidParams could be the way in which the viewController is presented. Is it pushed? Is it modal? You choose!


FINALLY we are ready to show the viewController. The params property includes the required experimentsMC parameter as well as presentationStyle, which is a valid (but not required) parameter. These parameters are passed through to the Experiment “routes struct” held on the Presentation class.


After creating an instance of the “routes struct” (in this example, Experiment), we are now ready to *actually present* the viewController. openViewConrollerWithPresentation() takes in a conforming type of PresentationProtocol as a parameter. After establishing the path, parameters and URL, routeURL is called. Using JLRoutes, routeURL presents the viewController associated with that url.


This may seem like a complicated solution for a “simple” transition between viewControllers. So, why did we decide to do it this way? For two key reasons:


1.


By creating a route for each viewController, integrating deep linking with universal links is much simpler. The url path already exists!


2.


When testing new features on the app, it is much more efficient to link directly to the viewController that contains the feature in question rather than navigating throughout the app to finally get to the relevant viewController.


The next time you use RTR’s app, you now have a “behind the scenes” understanding of what’s happening when you navigate between viewControllers. Thoughts? Comments? Please don’t hesitate to reach out to us!
