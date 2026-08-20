---
schema_version: "1.0.0"
document_id: "eada770f9c4bebe6b040aaf983560c2ce8a40d33d4ea3e54ce55695d66b33040"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/simplifying-state-management-with-compose"
published_at: "2023-07-05T19:45:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:715caa9fccfcf034577412657cec3709c5d1a4d113cf5c27d14ca406c8c3b673"
---

# Simplifying State Management with Compose

The recent release of Doximity’s refreshed physician scheduling tool Amion involved a significant overhaul of the user interface (UI) layer using Jetpack Compose. As part of the development process, we decided to experiment with shifting from ViewModels to utilizing the Compose Runtime for managing screen state. This article explores the motivations behind this approach and demonstrates an overall improvement to the view layer and presentation logic using the patterns developed in[Amion 6.0.0](https://play.google.com/store/apps/details?id=com.doximity.amiondroid) .


*Note: This is part one of a two-part article. In part two, we will[build a note-taking app](https://technology.doximity.com/articles/composable-presenters-part-2-case-study) using the foundations covered here.*


## Understanding the Presentation Layer


The Android team at Doximity builds apps following the basic principles of[clean architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) , which involves separating the app into layers, each with their own responsibilities. In our app, we have a view layer, responsible for rendering UI components (Compose UI, XML views, etc.), and a presentation logic layer, responsible for producing screen state. Every navigable point in the app corresponds to one or more UI components and is associated with one or more screen state producers.


## Inspiration and Initial Experiments


The decision to leverage Compose for managing state was inspired by (at the risk of sounding like r/mAndroidDev) Jake Wharton's article[The State of Managing State (with Compose)](https://code.cash.app/the-state-of-managing-state-with-compose) . While the release of[Circuit](https://slackhq.github.io/circuit/) occurred midway through our development, it was deemed more of a framework that required significant buy-in, whereas we aimed to build a flexible pattern with smoother interoperability with our existing architecture, that at the time mostly utilized` ViewModel` with XML and Compose UI.


When we think about the presentation layer in Android as developers, we have to think about: What is the state of the screen? That is, not just when the user first sees it, but what does it look like when it's loading, what happens when there is an error, how will the state of components of the screen interact, and any number of other scenarios. To create any given screen state, we have to go through a series of conditions based on data, inputs and the overall state of the application (e.g. permission) or phone (e.g. internet connection). Once we have all the required conditions we can produce a screen state based on presentation and business logic, and that state remains the same until there are changes to any of the underlying inputs, data, etc. Turns out, the Compose Runtime is really good at building and reacting to state changes.


*Note: Compose itself is essentially two libraries – Compose Compiler and Compose UI. Most folks usually think of Compose UI, but the compiler (and associated runtime) are actually not specific to UI at all and offer powerful state management APIs. Jake Wharton has[an excellent post](https://jakewharton.com/a-jetpack-compose-by-any-other-name/) also about this.*


## Separating UI and Screen State


Assuming you're already familiar with Jetpack Compose, take a look at the following Counter example (for a primer on Compose, check out the[official documentation](https://developer.android.com/jetpack/compose) ).


```text
@Composable
fun    Counter  ()    {
var    count    by    remember    {    mutableStateOf  (  0  )    }


Button  (  onClick    =    {    count  --    })    {
Text  (  text    =    "-"  )
}


Text  (  text    =    count  .  toString  ())


Button  (  onClick    =    {    count  ++    })    {
Text  (  text    =    "+"  )
}
}


```


In the above, when a button is tapped, the` count` is incremented or decremented, which causes recomposition. When this occurs, the value of` count` is updated, the function reruns, and the` Text` Compose UI component renders the new value. This works as is, but following our architectural principles, we want a clear separation of the view layer from the presentation logic layer. This will allow us to write unit tests against our presentation logic, and it follows the battle-tested design principle of separation of concerns. In other words, the view layer should only be responsible for rendering the screen state. It should not contain presentation logic.


So, let’s isolate the screen state and presentation logic from the UI. First, let's define a` data class` that represents the state of the` Counter` :


```text
data class    CounterState  (
val    count  :    String  ,
val    increment  :    ()    ->    Unit  ,
val    decrement  :    ()    ->    Unit
)


```


In this` data class` ,` count` represents the current value we want to display. The two lambdas are used to manipulate the value of` count` .


Now, if we add a Composable function that creates that` data class` , it would look like:


```text
@Composable
fun    counterState  ():    CounterState    {
var    currentCount    by    remember    {    mutableStateOf  (  0  )    }


return    CounterState  (
count    =    currentCount  .  toString  (),
increment    =    {
currentCount  ++
},
decrement    =    {
currentCount  --
}
)
}


```


Notice that this function only relies on the Compose Runtime. There are no UI components created here; only presentation logic in the form of a function that returns` CounterState` with the value of` currentCount` and two lambdas for changing the value of` currentCount` .


Now if we update our UI to interact with our new` CounterState` , we have:


```text
@Composable
fun    Counter  ()    {
val    counterState    =    counterState  ()


Button  (  onClick    =    {    counterState  .  decrement  ()    })    {
Text  (  text    =    "-"  )
}


Text  (  text    =    counterState  .  count  )


Button  (  onClick    =    {    counterState  .  increment  ()    })    {
Text  (  text    =    "+"  )
}
}


```


As you can see, we now have a very clear separation between the part of Compose that renders the UI components and the part that produces the screen state for that UI. Using this foundational concept in Amion 6.0.0, we built structures that established a pattern for working with Compose in this manner.


### Applying the Pattern


First, for creating a data class that will act as the screen state, we added a` UiModel` marker interface:


```text
@Immutable
interface    UiModel


```


We added this interface to flag the screen state data class as` @Immutable` which means[‘all publicly accessible properties and fields will not change after the instance is constructed’](https://chrisbanes.me/posts/composable-metrics/#immutability--stability) . Or to put it a different way, the UiModel will remain the same until a new one is produced.


To facilitate dependency injection and enforce a common pattern, we created a` Presenter` interface. This will act as a wrapper for our compose function that will return the screen state:


```text
interface    Presenter  <  Model    :    UiModel  ,    Params  >    {
@Composable    fun    present  (  params  :    Params    ):    Model
}


```


Now, with the` UiModel` , we can define the output of our` @Composable` function that creates screen state. We added a` Params` class to the presenter as well, which is used for passing inputs (in the form of other data or other classes that can’t easily be injected at runtime) to the presenter.


In our original example, we had two lambdas that manipulated the mutable state of` count` . This might work for one or two functions, but would get unwieldy if there were more (and there's always more). To simplify this, we added an` EventHandler` and a` UiEvent` marker interface:


```text
interface    UiEvent


@Immutable
data class    EventHandler  <  E    :    UiEvent  >(  val    key  :    Any  ?    =    null  ,    val    handle  :    (  E  )    ->    Unit  )    {
override    fun    equals  (  other  :    Any  ?):    Boolean    =    this  .  key    ==    (  other    as  ?    EventHandler  <*>)  ?.  key
override    fun    hashCode  ():    Int    =    handle  .  hashCode  ()
}


```


Now, instead of passing two lambdas for two different user interactions (e.g. increment and decrement), we have a single lambda wrapped in a data class. This` EventHandler` takes any` UiEvent` as a parameter. We override the equality operators here to allow equality assertions against UiModels in our unit tests (because equality can not be asserted against lambdas), and we provide a key for rare situations where the equality is needed by the Compose runtime (for example, when a` UiModel` has only one property that is the` EventHandler` ).


If we restructure our example using our new pattern, we have the following:


```text
data class    CounterUiModel  (
val    count  :    String  ,
val    eventHandler  :    EventHandler  <  Event  >
)    :    UiModel    {


sealed    interface    Event    :    UiEvent    {
object    Increment    :    Event
object    Decrement    :    Event
}
}


class    CounterPresenter    :    Presenter  <  CounterUiModel  ,    CounterPresenter  .  Params  >    {
data class    Params  (  val    initialCount  :    Int  )


@Composable
override    fun    present  (  params  :    Params  ):    CounterUiModel    {
var    currentCount    by    remember    {    mutableStateOf  (  params  .  initialCount  )    }


return    CounterUiModel  (
count    =    currentCount  .  toString  (),
eventHandler    =    EventHandler    {    event    ->
when    (  event  )    {
is    Increment    ->    currentCount  ++
is    Decrement    ->    currentCount  --
}
}
)
}
}


```


*Note: The sealed interface gives us nice code completion when using a` when` block. It also means if further events are added, linting in the latest versions of Kotlin will show an error for the missing branches.*


Finally, we add the new presenter and connect it to our UI:


```text
@Composable
fun    Counter  ()    {
val    counterPresenter    =    remember    {    CounterPresenter  ()    }    //or some DI framework
val    uiModel    =    counterPresenter  .  present  (
CounterPresenter  .  Params  (  initialCount    =    0  )
)


Button  (  onClick    =    {
uiModel  .  eventHandler  .  handle  (  CounterUiModel  .  Event  .  Decrement  )
})    {
Text  (  text    =    "-"  )
}


Text  (  text    =    uiModel  .  count  )


Button  (  onClick    =    {
uiModel  .  eventHandler  .  handle  (  CounterUiModel  .  Event  .  Increment  )
})    {
Text  (  text    =    "+"  )
}
}


```


*Note: Replace` val counterPresenter = remember { CounterPresenter() }` with your favorite dependency injection framework. E.g.` val counterPresenter: CounterPresenter = koinInject()` .*


## Improvements and Benefits


The adoption of Jetpack Compose and the pattern we developed brought several improvements that follow our Doximity development principles: testability, reusability, and readability. Reusability was enhanced by the pattern's simplicity, allowing for nesting presenters that align with the Compose UI tree-like hierarchy.


The most significant improvement was to readability. The` present()` function of every` Presenter` can be read top to bottom and is easier to reason about the behavior of the screen, because the function now represents the state of the screen based on its inputs and members. This has allowed us to["shorten the conceptual gap"](https://homepages.cwi.nl/~storm/teaching/reader/Dijkstra68.pdf) between understanding the class as written and understanding how the state of the class changes based on inputs and data over time. These improvements also made testing easier and more thorough, which aligned with our principle of testability.


## Conclusion


The transition to Jetpack Compose and the adoption of a pattern for managing screen state with Compose in Amion 6.0.0 proved to be highly successful. It has shown to have fewer bugs and faster development cycles. (Over 90K sessions, with a 99.9% session stability rate!) By leveraging the Compose Runtime and establishing a clear separation between the UI layer and presentation logic, the team achieved improvements in testability, reusability, and readability, leading to a more efficient and maintainable codebase.


Further resources:


- Modern Compose Architecture with Circuit by Zac Sweers and Kieran Elliott -[https://www.youtube.com/watch?v=ZIr_uuN8FEw](https://www.youtube.com/watch?v=ZIr_uuN8FEw)
- Compose in Cash App with Jake Wharton and Saket Narayan | Talking Kotlin -[https://www.youtube.com/watch?v=-ZExs9Gncic](https://www.youtube.com/watch?v=-ZExs9Gncic)


---


A huge and special thanks must go to John Petitto, who is the co-author of this pattern and whose many hours of collaboration and discussion made it possible.


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech) if you'd like to be notified about new blog posts.
