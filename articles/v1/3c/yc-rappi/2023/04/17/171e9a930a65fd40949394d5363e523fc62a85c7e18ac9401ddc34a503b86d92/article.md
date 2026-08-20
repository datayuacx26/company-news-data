---
schema_version: "1.0.0"
document_id: "171e9a930a65fd40949394d5363e523fc62a85c7e18ac9401ddc34a503b86d92"
company_key: "yc-rappi"
company: "Rappi"
source_id: "yc-rappi-rss-63ff898fda0d"
canonical_url: "https://engineering.rappi.com/improving-ios-developer-productivity-and-swiftui-adoption-at-rappi-4a38d3661c53"
published_at: "2023-04-28T15:01:17+00:00"
first_seen_at: "2026-07-20T23:20:59.100260+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:149b6aa496217bb656acca1ee79e3675554e80c47cdd157561376a03ce2a4d02"
---

# Improving iOS developer productivity and SwiftUI adoption at Rappi

Mobile App Development


IOS


Modular Architecture


Developer Tools


Swiftui


## iOS Engineering


# Improving iOS developer productivity and SwiftUI adoption at Rappi


## How we use a modular architecture and developer tools to adopt SwiftUI at scale.


[Pablo Cornejo](https://medium.com/@pablocornejo?source=post_page---byline--4a38d3661c53---------------------------------------)


8 min read


·


Apr 28, 2023


--


Press enter or click to view image in full size


The pods (not cocoa ones) of a modular village ([source](https://www.arch2o.com/language-modular-architecture/) ).


We


consider Rappi a super-app—a type of app that has tons of functionality included in it. This means that our users get to enjoy multiple services within a single application: restaurant delivery, banking, grocery shopping, errands, plane and hotel bookings, among others. On the other hand, this also means that we have to care about not letting the size of the project negatively impact developer productivity, and think carefully before adopting new technologies that can change how people work—like SwiftUI.


### Modular architecture


For additional context on our approach to modularization, I’ve written about how our app is modularized and how we decouple implementation modules in the super-app by using interface modules in a previous article:


[Modular iOS Architecture at Rappi: Interface Modules A glimpse into how we make the modularization of our app more effective by decoupling implementation modules. engineering.rappi.com](https://engineering.rappi.com/modular-ios-architecture-at-rappi-interface-modules-327b44f723e4?source=post_page-----4a38d3661c53---------------------------------------)


This model—having per-team modules divided in an implementation component and an interface one—has been working as a central part of our modular architecture strategy. However, when thinking about starting to adopt SwiftUI, we realized that the initial approach was not modular enough. It would be useful to further decompose the per-team modules into pieces that are smaller, more focused, and isolated, to be able to leverage the advantages of SwiftUI’s declarative and composable nature.


### The problem


Having a single module per team, modules contain multiple features (which you can think of as screens or flows of the app), that can range from a few to quite many depending on the size and characteristics of a team. Some of these modules had gotten pretty big and accumulated lots of code. Additionally, these modules are configured as static libraries in our main project due to the launch speed that this allows our users to enjoy when using our app, compared to having them as dynamic frameworks.


This configuration works great for the full project that gets built and shipped to our users. But, how would it impact the developer experience of implementing a new screen with SwiftUI? Because of the static nature of the modules, any preview code would just not work under that configuration. Even if we somehow managed to make the previews work, the whole module of the team would have to be compiled for a single preview to be rendered—including all the UIKit code and business logic of the team that may not be relevant to a particular SwiftUI preview.


Press enter or click to view image in full size


SwiftUI previews are not supported when defined in static libraries.


To deal with these problems, we defined a new kind of module that would contain a much more isolated scope of the UI of a team: a feature module.


### Feature modules


Instead of having all of the code of a team grouped in a single target, with feature modules it would be divided into multiple smaller targets that are focused on a more narrow subset of the UI: a single app screen or a few screens that are part of an indivisible flow. And, although a feature module represents a different scope, the topology of its components can be kept similar to the one of the existing per-team modules—meaning that a feature would have two module components: implementation and interface.


Press enter or click to view image in full size


The topology of a feature’s components is the same to that of per-vertical modules, but at a smaller scale


To review the roles of the implementation and interface modules more in depth, you can refer to[my previous article](https://medium.com/rappitech/modular-ios-architecture-at-rappi-interface-modules-327b44f723e4) , as they are conceptually equivalent in this case: the interface component contains the public API of the implementation component, and all external modules that need to consume the implementation would only need to depend on its interface component.


Press enter or click to view image in full size


The implementation of a feature is isolated. All other modules’ dependencies point to its interface component.


Feature implementation modules are also the best location to place implementations of new screens built with SwiftUI, or existing ones that get re-written with SwiftUI. Because of the rules that we’ve applied to implementation modules, a feature module where a SwiftUI screen is implemented in would depend only on the lightweight interface modules, and could be built without needing their heavier implementation components. This can dramatically reduce the amount of code that needs to get compiled if we want to focus on a single feature module’s implementation.


## Get Pablo Cornejo’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


However, it wouldn’t be too convenient to work on a feature module when embedded in the full project. They’d be to need part of a smaller and more focused Xcode project to fully take advantage of the implementation isolation and have a more agile development environment.


### Working with dev apps


To make it convenient to work on a single isolated feature module—i.e., to compile only its required dependencies, display it’s SwiftUI previews, show it’s UI on a simulator, compile and run only the single feature’s tests, and iterate on these workflows as needed—we started using the concept of development applications (dev apps).


Artistic representation of a Rappi dev app, which may or may not be their app icon.


Dev apps are meant to be lightweight workspaces that our team members can generate on demand to work on a project focused on a single module, without the overhead of the rest of the super-app’s codebase, nor other implementation modules that aren’t essential for the module’s development.


The modules in the workspace of a dev app for a feature consist of only the feature module, its direct and transitive dependencies, its tests, and an app target—which is needed to run the feature’s code on a simulator or device. Because these projects are used only for development, the targets that contain SwiftUI code can be safely configured as dynamic frameworks in order for previews to work, without affecting the static library configuration that we use in the main project.


Press enter or click to view image in full size


The targets in the workspace of a dev app for a feature module include only its dependencies and tests.


The compilation time—and, therefore, iteration time—of a dev app project is greatly reduced compared to the full project because of the decoupling of implementation modules that is possible with interface modules. Interface modules are lightweight by definition and there’s just not much to compile there.


When the implementation of a module needs to be consumed for development, instead of compiling the full implementation module corresponding to an interface (like` NetworkInterface` in the diagram above) mocks can be registered and injected using the[dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) framework that we use.


Press enter or click to view image in full size


Sample dependency injection setup to inject network module mocks to a feature dev app.


Although setting up dev apps in this way allows for faster development speeds and a better development experience when working on the features of a huge project like the Rappi iOS app; creating, updating, and maintaining such projects manually would put a lot of friction to using the dev app concept. That’s why we thought about an automated generation of dev apps since their inception, which makes working with them a breeze.


### Automating with a software army knife


Press enter or click to view image in full size


The Rappi Army Knife for iOS developer productivity


Yes, we’ve developed a software army knife to help us with all sorts of development tasks. Among the most impressive tricks that this tool has under its imaginary metal blades is the concocting of feature modules that follow our carefully determined architecture guidelines; and the generation of dev app projects in a couple of seconds, which are ready to be compiled, run, and iterated on until becoming your next favorite feature of the Rappi app.


Whimsical comments aside, Rappi Army Knife—RAK for short—is a command line tool that has been designed to make it easy for our team to create and work on the isolated feature modules that contain SwiftUI code. Any type of module that we have defined can be created, and all of the characteristics and benefits of a dev app project mentioned before can be reaped with a couple of commands:


```text
$ rak make module Demo --feature --team Restaurants   $ rak make dev-app Demo --open
```


Running the above would first create a Demo feature for this article that is owned by the Restaurants team. This includes the implementation and interface modules with corresponding templates, proper target configuration and definition, strings files, and tests, among others niceties.


The second command would then package the newly created feature components in a dev app project and open Xcode:


Press enter or click to view image in full size


A freshly created dev app project.


Having an easy way of creating and working on modules with a dev app was an essential part of our adoption of SwiftUI. It is with these concepts and tools in place that we have spearheaded the adoption of SwiftUI in the super-app, in a controlled way, and while improving the developer experience.


Our tools are tailored to our project’s characteristics and our team’s needs. The inner details of making a tool like that will vary based on the specifics of every project. We’ve developed part of the functionality in-house to, for example, navigate and analyze the dependency tree of a module and determine which other modules need to be included in the dev app project. And we’ve also taken advantage of some existing tools that can be useful to a wide array of projects types. Particularly,[Swift Argument Parser](https://github.com/apple/swift-argument-parser) is the backbone of our command line tool;[XcodeGen](https://github.com/yonaskolb/XcodeGen) is a very useful tool for project generation, manual or automated;[Swift Syntax](https://github.com/apple/swift-syntax) , although not being the easiest to use, is very powerful to implement syntax-level analysis over Swift code, and allowed us to do some cool things like automatic mock generation, when combined with a mocking framework like[Mockolo](https://github.com/uber/mockolo) .


### Final thoughts / TL;DR


With Rappi being a super-app that offers multiple services through a single application, the adoption of new technologies like SwiftUI can become a challenge. To address the problem of the static nature of the modules and the impact on the developer experience of implementing new screens with SwiftUI, we have defined feature modules, to contain a much more narrow scope of the UI of a team.


Feature modules allow developers to work on smaller, and more isolated targets that can be built without needing the implementation components of its dependencies. This allows our team members to use our developer tools to create dev apps—lightweight, on-demand projects to work on a single module without the overhead of the rest of the codebase, dramatically improving compilation and iteration times. Applying these concepts and implementing these tools has allowed us to improve Rappi’s modular iOS architecture, and improve developer productivity, which is then derived in us providing a better experience for our users.


Thanks for reading! If you have any comments, questions, or just want to connect you can reach out to me on[Twitter](https://twitter.com/pablo_cx) .
