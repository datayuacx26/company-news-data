---
schema_version: "1.0.0"
document_id: "430cb8840714f23085a41d2e1a05c9328458c82e379eabadaacae8a07a393d83"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/improving-loading-experience-in-swiftui"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:2a593a7f4c7695df68a643ba06550882865454ebafc84d54d8173b3bb9460248"
---

# Improving the Loading Experience in SwiftUI

## Intro


At Photoroom we strive to build an iOS app that offers the best user experience possible! Today I’m going to show you a pretty cool SwiftUI trick we came up with to improve the UX when you’re displaying a loader while a network call is happening.


## The problem we had to solve


We’ve implemented a new feature called Magic Studio that creates an AI-generated scene around the photo of an object or a person.


When a user opens this feature, we first need to make a network call to load the list of all available scenes.


Our first approach was relatively straightforward: we were displaying a loader while the data was being loaded from the network:


```text
Group {
switch viewModel.viewState {
case .loading:
ProgressView()
.frame(maxWidth: .infinity, maxHeight: .infinity)
case .fetched:
MagicStudioListView()
case .error:
MagicStudioErrorView(viewModel: viewModel)
}
}
.task {
await viewModel.fetchScenes()
}
```


However, when we started testing the feature, we quickly noticed that there was an issue with this approach!


A lot of times, the network call would complete very quickly, which meant that the loader would be displayed only for a very short amount of time.


This led to a UI that wasn’t very pleasant in terms of user experience, because the few frames where the loader was displayed gave the frustrating feeling of a UI that flashes:


## How to solve the problem from a UX perspective


So we started looking for a solution to this problem!


My first idea, which wasn’t the best, was to make it so that the loader couldn’t be displayed for less than half a second, regardless of whether the network call would complete sooner.


This approach would have solved the problem of the flashing UI, however, it would have solved it at the cost of introducing an unnecessary delay to access the feature, which is less than ideal!


But fortunately, our design team had a better idea: instead of displaying the loader immediately, we would wait for one second to actually display it.


This way, when the network call goes fast, no loader would appear and the UI wouldn’t flash. And when the network call takes longer than usual, only then would we eventually display the loader.


## How to implement the solution with SwiftUI


Now that we have the solution, all that’s left is to implement it using SwiftUI!


We want to implement a mechanism to delay the appearance of a` View` and as it turns out SwiftUI comes with the perfect tool for this: we need to implement a custom` ViewModifier` :


```text
import SwiftUI


struct DelayAppearanceModifier: ViewModifier {
@State var shouldDisplay = false


let delay: Double


func body(content: Content) -> some View {
render(content)
.onAppear {
DispatchQueue.main.asyncAfter(deadline: .now() + delay) {
self.shouldDisplay = true
}
}
}


@ViewBuilder
private func render(_ content: Content) -> some View {
if shouldDisplay {
content
} else {
content
.hidden()
}
}
}


public extension View {
func delayAppearance(bySeconds seconds: Double) -> some View {
modifier(DelayAppearanceModifier(delay: seconds))
}
}
```


The logic of this custom modifier is quite simple:


-


First, we store a boolean` shouldDisplay` to decide whether or not the view should be displayed


-


Then we give that boolean a default value of` false`


-


Finally, when the view is rendered for the first time, we schedule a piece of code that will set` shouldDisplay` to` true` to be executed after some` delay`


And now all that’s left to do is to actually use our new modifier` delayAppearance(bySeconds:)` on our` ProgressView()` :


```text
Group {
switch viewModel.viewState {
case .loading:
ProgressView()
.frame(maxWidth: .infinity, maxHeight: .infinity)
.delayAppearance(bySeconds: K.Durations.delayBeforeShowingLoader)
case .fetched:
MagicStudioListView()
case .error:
MagicStudioErrorView(viewModel: viewModel)
}
}
.task {
await viewModel.fetchScenes()
}
```


And that’s it, we can now test the new behavior of our UI, first with a fast network:


As you can see, the loader was never displayed, because the network call was completed in less than one second.


And now if we test with a much slower network:


This time the network call took more than one second to complete, so the loader was eventually displayed to let the user know that the feature is still loading.


## Conclusion


I hope you've enjoyed this nice little UX trick to avoid displaying a loader when it would result in a subpar user experience!


While this might look like a detail, at Photoroom we aim to deliver the best image editing app available on iOS and we strongly believe that providing the best UX possible is a key part of achieving this goal.
