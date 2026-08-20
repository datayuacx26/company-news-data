---
schema_version: "1.0.0"
document_id: "66106dfad4a442c8e36c177af0213ca31ade29399971f8f85f04405b23ce82e8"
company_key: "tripadvisor-inc-common-stock"
company: "TripAdvisor Inc."
source_id: "tripadvisor-inc-common-stock-rss-6295d6870799"
canonical_url: "https://medium.com/tripadvisor/the-evolution-of-native-engineering-at-tripadvisor-part-3-design-system-flexible-framework-ebf9f27e62be"
published_at: "2025-12-02T16:45:13+00:00"
first_seen_at: "2026-07-20T23:18:15.449539+00:00"
fetched_at: "2026-08-20T00:33:41.838119+00:00"
content_hash: "sha256:4648b6a32499dc709b385eadcfe7c2e60b63b344689ea8e4e8571aa3d3dbe960"
---

# The evolution of Native Engineering at Tripadvisor: Part 3 — Design System Flexible Framework…

### The evolution of native engineering at Tripadvisor: Part 3 — Design System Flexible Framework Meets TCA


*This is the third of a three-part series on how we’ve evolved our native app architecture to ensure scalability, testability and added flexibility in delivering features to our customers.*


By[Andrew Frederick](https://www.linkedin.com/in/anfrederick/) , Principal iOS Developer


In the[second post](https://medium.com/tripadvisor/the-evolution-of-native-engineering-at-tripadvisor-part-2-5ce6659d32b2) of this series,[Mason Pomeroy](https://www.linkedin.com/in/masonpomeroy/) discussed the creation of a SDUI (Server Driven UI) framework known as the Design System Flexible Framework (DSFF). He covered the reasons why our previous SDUI approach required re-architecting to achieve a new level of flexibility and safety when launching new features. In the[first post](https://medium.com/tripadvisor/the-evolution-of-native-engineering-at-tripadvisor-part-1-577cc0e36ec8) ,[Benoit Sarrazin](https://www.linkedin.com/in/bsarrazin/) discussed why we’re adopting The Composable Architecture in our iOS app and in this post I’m going to discuss how we integrated the two.


My team was tasked with building the DSFF: a from-scratch SDUI framework that would allow Product, Design & Server to quickly define and iterate layouts presented in the app. It was fortuitous that Benoit’s team was simultaneously exploring TCA (The Composable Architecture) as our new underlying architecture to replace MVVM-C (Model-View-ViewModel-Coordinator). TCA was the perfect fit for DSFF as it allowed us to compose individual dynamic SwiftUI Views into complex components with a responder chain of Actions and deterministic state mutations.


### Flexible framework design


In order to achieve maximum performance the Android and iOS clients built their frameworks in Jetpack Compose and SwiftUI respectively. These two UI frameworks have commonalities but also differences in how they lay out views on screen. For example on Android, views lay out on the leading edge of their container by default whereas SwiftUI aligns in the center. Because our SDUI contract is shared between the two platforms as defined on the server, we constructed atomic units that would render the same way on either platform in a predictable manner.


#### Requirements


In the early stages of planning and development we came up with the following requirements:


- The size of a flexible view should be influenced by the server. They may either expand to fill each axis, contract to hug the intrinsic content size of the item view, or fill a percentage of its parent view.
- A flexible view should wrap preexisting components from the Design System-defined UI library if applicable. The flex wrapper view should apply the attributes provided by the server, for example, height, width, padding, interactions, etc.
- A flexible view should be able to do some asynchronous work and then later update its state again based on a server response. This state change should be observable by other interested views. In order to share state between sections we leverage the[@Shared property wrapper](https://www.pointfree.co/blog/posts/134-sharing-state-in-the-composable-architecture#the-shared-property-wrapper) .
- Interactions should bubble up the responder chain for inspection by some ancestor, whether that is a parent Reducer or a ViewModel, as not all screens in the app have been converted to TCA yet.


#### Flexible Items


In the Tripadvisor app we display many types of UI components: containers, text, buttons, badges, pickers, filter chips, image galleries, carousels, etc. For the sake of this article, I will discuss only a few which may constitute an interesting layout:


- *Horizontal Container* — aligns child views on the horizontal axis
- *Vertical Container* — aligns child views on the vertical axis
- *Text* — a static string
- *Button* — a tappable control
- *Interactive Bubble Rating* — a control that allows the user to rate a hotel, restaurant, attraction, etc.


Consider a Flexible Item to be a node in a tree structure that models the user interface. There is a root node which may contain any number of Container nodes, which may contain leaf Item nodes or other Container nodes. To accommodate this nesting, Containers are Items themselves and may optionally contain child Items. A Flexible Section is a logical grouping that contains the root node which must be a Container.


Below are sample implementations for Flexible Items, Containers, and a subset of other views we have:


**FlexibleSectionView.swift**


```text
@Reducer  struct FlexibleSectionReducer {        @ObservableState      struct State: Equatable {          var rootContainer: FlexibleContainerReducer.State      }        enum Action: Equatable {          enum DelegateAction: Equatable {              case interaction(APSInteraction<StandardAction>)   }            case delegate(DelegateAction)      }        var body: some ReducerOf<Self> {          /// Explained in a later part of the post      }  }    struct FlexibleSectionView: View {        let store: StoreOf<FlexibleSectionReducer>        var body: some View {          let rootContainerStore = Store(              initialState: store.rootContainer,              reducer: {                  FlexibleContainerReducer(),                  Reduce<FlexibleContainerReducer.State, FlexibleContainerReducer.Action { _, rootContainerAction in                      /// Explained in a later part of the post                  }              }             )            FlexibleContainerView(store: rootContainerStore)      }  }
```


Notice that the FlexibleSectionView merely renders its root Flexible Container and provides a Delegate action to send messages to a higher level parent Reducer or a ViewModel.


**FlexibleContainerView.swift**


```text
enum FlexibleContainerReducer {      case horizontal(FlexibleHorizontalContainerReducer)      case vertical(FlexibleVerticalContainerReducer)  }    struct FlexibleContainerView: View {        let store: StoreOf<FlexibleContainerReducer>        var body: some View {          switch store.case {          case .horizontal(let horizontalStore):              FlexibleHorizontalContainerView(store: horizontalStore)          case .vertical(let verticalStore):              FlexibleVerticalContainerView(store: verticalStore)          }      }  }
```


**FlexibleItemView.swift**


```text
@Reducer  enum FlexibleItemReducer {      case container(FlexibleContainerReducer.State)        case button(FlexibleButtonReducer.State)      case text(FlexibleTextReducer.State)      case interactiveBubbleRating(FlexibleInteractiveBubbleRatingReducer.State)  }    struct FlexibleItemView: View {        let store: StoreOf<FlexibleContainerReducer>        var body: some View {          switch store.case {          case .container(let containerStore):              FlexibleContainerView(store: containerStore)          case .button(let buttonStore):              FlexibleButtonView(store: buttonStore)          case .text(let textStore):              FlexibleTextView(store: textStore)          case .interactiveBubbleRating(let bubbleRatingStore):              FlexibleInteractiveBubbleRatingView(store: bubbleRatingStore)          }      }  }
```


In TCA parlance these are known as Enum Reducers. Depending on the enum case they will scope to and render a single Flexible Item type, some of which are defined below:


**FlexibleTextView.swift**


```text
@Reducer  struct FlexibleTextReducer {        @ObservableState      struct State: Equatable {          let alignment: TextAlignment          let padding: EdgeInsets?          let foregroundColor: Color          let text: String?          let lineLimit: Int?      }  }    struct FlexibleTextView: View {      let store: StoreOf<FlexibleTextReducer>        var body: some View {          Text(store.text ?? "")              .foregroundColor(store.foregroundColor)              .multilineTextAlignment(store.alignment)              .lineLimit(store.lineLimit)              .padding(store.edgeInsets ?? .zero)      }  }
```


Because Text is completely static each property of State is a constant and the Reducer contains no Actions. Below we have a Button, which unlike the Text Item will invoke an interaction when tapped:


**FlexibleButtonView.swift**


```text
@Reducer  struct FlexibleButtonReducer {        @ObservableState      struct State: Equatable {          let id: String          var interaction: APSInteraction<StandardAction>?          let padding: EdgeInsets?          let title: String?          let icon: String?      }        enum Action: Equatable {          enum DelegateAction: Equatable {              case interaction(APSInteraction<StandardAction>)          }            enum ViewAction: Equatable {              case buttonTapped          }            case delegate(DelegateAction)          case view(ViewAction)      }        var body: some ReducerOf<Self> {          Reduce<State, Action> { state, action in              switch action {              case .delegate:                  return .none                case .view(let viewAction):                  switch viewAction {                  case .buttonTapped:                      if let buttonInteraction = state.interaction {                          return .send(.delegate(.interaction(buttonInteraction)))                      }                        return .none                  }              }          }      }  }    struct FlexibleButtonView: View {        let store: StoreOf<FlexibleButtonReducer>        var body: some View {          PrimaryButton(              icon: Symbol(name: store.icon),              title: store.title ?? "",              size: .medium,              contentSizingMode: .fit,              action: { store.send(.view(.buttonTapped)) }          )          .padding(store.padding ?? .zero)      }  }
```


PrimaryButton is an existing Design System UI component that we wrap with FlexibleButtonView and provide it with an action closure that invokes a Reducer Action. The APSInteraction is a type provided by the server and defines different side effects which are typical of a mobile app: navigating to a different screen via Route, displaying a popup, dismissing the current screen and more. This action is not handled locally in the Flex Button but is instead delegated out to some handler higher up the chain so that all interaction handling is centralized.


One aspect of TCA Reducers, which Ben mentioned in his article, is that they act as a responder chain allowing ancestor Reducers to intercept Actions emitted from descendant Reducers. This allowed us to centralize shared functionality such as handling interactions. However when I began to implement this I ran into an issue: the standard method of intercepting Actions in TCA would not work for a completely dynamic tree of Reducers.


#### Intercepting TCA Actions in a dynamic tree


Consider the following Reducer hierarchy:


```text
AppReducer  --TabReducer  ----HomeReducer  ----SearchReducer  ----TripsReducer  ----AccountReducer
```


If an Action is emitted from the TripsReducer it may be intercepted higher up the chain via TCA’s usage of[Case Paths](https://github.com/pointfreeco/swift-case-paths) . For example, if AppReducer wanted to log an analytic event every time a particular Action is intercepted it would look something like this:


```text
@Reducer AppReducer {        @ObservableState      struct State: Equatable {          let tabs: TabReducer.State      }        enum Action: Equatable {          let tabs: TabReducer.Action      }        var body: some ReducerOf<Self> {          Scope(state: \.tabs, action: \.tabs, child: { TabReducer() })          Reduce<State, Action> { state, action in              switch action {                  case .tabs(.home(.someHomeAction)),                        .tabs(.search(.someSearchAction)):                           logAnalyticEvent(.trackSearch)                           return .none                    case .tabs(.trips(.tripSaved)):                           logAnalyticEvent(.trackTripSaved)                           return .none                    default:                      return .none              }          }      }  }
```


It is straightforward to match the case path for Actions of the various children of TabReducer because they are linear and known at compile time. With a dynamic tree structure we could not leverage linear case paths because the structure is only known to the app at run time.


As is often the case when building algorithms for a tree structure, we chose to utilize recursive functions. This brings us back to the parts of FlexibleSectionView that need to be filled out as mentioned earlier in the post.


The FlexibleSectionReducer is the root level of the reducer tree containing any number of child FlexibleItemReducers or FlexibleContainerReducers. It is the perfect spot to handle generic functionality such as tracking the click of some nested item. Rather than individual nodes handling their own tracking logic we can do it here. Our APSInteraction type contains the information that our tracking service requires so we extract it here and call out to that external service:


```text
@Reducer  struct FlexibleSectionReducer {        @ObservableState      struct State: Equatable {          var rootContainer: FlexibleContainerReducer.State      }        @CasePathable      enum Action: Equatable {          // These are the actions that bridge up to a parent Reducer or ViewModel.          enum DelegateAction: Equatable {              case interaction(APSInteraction<StandardAction>)          }            case delegate(DelegateAction)      }        var body: some ReducerOf<Self> {          Reduce<State, Action> { state, action in              switch action {                // If an APSInteraction funnels through this action it will be tracked as a click.              // Additional tracking may be executed by an ancestor if needed.              case .delegate(.interaction(let interaction)):                  // Make API call to track the click event                  trackClickInteraction(interaction.trackingInfo)                  return .none                case .delegate(.ratingSelected(let interactiveBubbleRating)):                  // Make API call to save the new rating                  saveRating(interactiveBubbleRating)                  return .none              }          }      }  }
```


In the switch statement above, for the second case, if a new screen requires this component it may be returned by the server and its side effects are handled with no client changes required. The component itself will emit this same Action passing the necessary data to make an API call happen.


Next, in the FlexibleSectionView struct, we create a Store for each Section as a performance optimization and as a delimiter between higher level Stores which don’t need to receive Actions, leaving each Section as a “grey box unit”.


We distinguish between two categories of action: Item or Container. The Reduce function maps FlexibleContainer.Action which will always be the root level Action, and we use that as input into one of two functions that will recursively dig through the case path of the emitted Action to return the most deeply nested Action. This case path is completely arbitrary and unknown until run time and depending on the view tree could be deeply nested.


```text
struct FlexibleSectionView: View {      let store: StoreOf<FlexibleSectionReducer>        var body: some View {            let rootContainerStore = Store(              initialState: store.rootContainer,              reducer: {                  FlexibleContainerReducer()                  Reduce<FlexibleContainerReducer.State, FlexibleContainerReducer.Action> { _, rootContainerAction in                      if let itemAction = nestedItemAction(in: rootContainerAction) {  /***  All leaf node delegate actions are enumerated. We use the naming convention of a Delegate action on leaf items to illustrate that they will bubble up some action to the section level for further handling.  ***/                          switch itemAction {                          case .button(.delegate(.interaction(let interaction))):                              store.send(.delegate(.interaction(interaction)))                            case .interactiveBubbleRating(.delegate(.ratingSelected(let interactiveBubbleRating))):                              store.send(.delegate(.onInteractiveBubbleRatingChanged(interactiveBubbleRating)))                          }                      } else if let containerAction = nestedContainerAction(in: rootContainerAction) {  /***  All container node delegate actions are enumerated. Containers have delegate actions to represent events such as a tap on a List cell.  ***/                          switch containerAction {                          case .horizontal(.delegate(.interaction(let interaction))),                               .vertical(.delegate(.interaction(let interaction))):                              store.send(.delegate(.interaction(interaction)))                          }                      }                        return .none                  }              }          )            FlexibleContainerView(store: flexStore)      }
```


Below, the nestedItemAction function takes a FlexibleContainerReducer.Action and unpacks it, further recursing if the inner action is also a FlexibleContainerReducer.Action, or otherwise just returns the unpacked FlexibleItemReducer.Action if it exists (i.e., we’ve reached a leaf node in the tree)


```text
/// Recursively drills down the path of Actions until it finds one belonging to a FlexibleItem, i.e. leaf node.      func nestedItemAction(in containerAction: FlexibleContainerReducer.Action) -> FlexibleItemReducer.Action? {          if case .horizontal(.items(.element(id: _, action: let itemAction))) = containerAction {              if case .container(let innerContainerAction) = itemAction {                  return nestedItemAction(in: innerContainerAction)              }              return itemAction          }            if case .vertical(.items(.element(id: _, action: let itemAction))) = containerAction {              if case .container(let innerContainerAction) = itemAction {                  return nestedItemAction(in: innerContainerAction)              }              return itemAction          }            return nil      }
```


Finally, the nestedContainerAction takes a FlexibleContainerReducer.Action and unpacks it, further recursing if the inner action is also a FlexibleContainerReducer.Action until it finds an action that is not a container action, whereby it returns the most deeply nested container action. This functionality is necessary because Containers may also be tapped or emit other actions.


```text
/// Recursively drills down the path of Actions until it finds the last one that is a FlexibleContainer's Delegate Action      func nestedContainerAction(in containerAction: FlexibleContainerReducer.Action) -> FlexibleContainerReducer.Action? {          if case .horizontal(.delegate) = containerAction {              return containerAction          }            if case .vertical(.delegate) = containerAction {              return containerAction          }            if case .horizontal(.items(.element(id: _, action: let itemAction))) = containerAction {              if case .container(let innerContainerAction) = itemAction {                  return nestedContainerAction(in: innerContainerAction)              }              return nil          }            if case .vertical(.items(.element(id: _, action: let itemAction))) = containerAction {              if case .container(let innerContainerAction) = itemAction {                  return nestedContainerAction(in: innerContainerAction)              }              return nil          }            return nil      }  }
```


The complete code sample for the FlexibleSectionReducer struct is listed below:


```text
@Reducer  struct FlexibleSectionReducer {        @ObservableState      struct State: Equatable {          var rootContainer: FlexibleContainerReducer.State      }        @CasePathable      enum Action: Equatable {          // These are the actions that bridge up to a parent Reducer or ViewModel.          enum DelegateAction: Equatable {              case interaction(APSInteraction<StandardAction>)          }            case delegate(DelegateAction)      }        var body: some ReducerOf<Self> {          Reduce<State, Action> { state, action in              switch action {                // If an APSInteraction funnels through this action it will be tracked as a click.              // Additional tracking may be executed by an ancestor if needed.              case .delegate(.interaction(let interaction)):                  // Make API call to track the click event                  trackClickInteraction(interaction.trackingInfo)                  return .none                  case .delegate(.ratingSelected(let interactiveBubbleRating)):                  // Make API call to save the new rating                  saveRating(interactiveBubbleRating)                  return .none              }          }      }  }    struct FlexibleSectionView: View {      let store: StoreOf<FlexibleSectionReducer>        var body: some View {            let rootContainerStore = Store(              initialState: store.rootContainer,              reducer: {                  FlexibleContainerReducer()                  Reduce<FlexibleContainerReducer.State, FlexibleContainerReducer.Action> { _, rootContainerAction in                        if let itemAction = nestedItemAction(in: rootContainerAction) {                            switch itemAction {                          case .button(.delegate(.interaction(let interaction))):                              store.send(.delegate(.interaction(interaction)))                            case .interactiveBubbleRating(.delegate(.ratingSelected(let interactiveBubbleRating))):                              store.send(.delegate(.onInteractiveBubbleRatingChanged(interactiveBubbleRating)))                          }                      } else if let containerAction = nestedContainerAction(in: rootContainerAction) {                            switch containerAction {                          case .horizontal(.delegate(.interaction(let interaction))),                               .vertical(.delegate(.interaction(let interaction))):                              store.send(.delegate(.interaction(interaction)))                          }                      }                        return .none                  }              }          )            FlexibleContainerView(store: flexStore)      }        /// Recursively drills down the path of Actions until it finds one belonging to a FlexibleItem, i.e. leaf node.      func nestedItemAction(in containerAction: FlexibleContainerReducer.Action) -> FlexibleItemReducer.Action? {          if case .horizontal(.items(.element(id: _, action: let itemAction))) = containerAction {              if case .container(let innerContainerAction) = itemAction {                  return nestedItemAction(in: innerContainerAction)              }              return itemAction          }            if case .vertical(.items(.element(id: _, action: let itemAction))) = containerAction {              if case .container(let innerContainerAction) = itemAction {                  return nestedItemAction(in: innerContainerAction)              }              return itemAction          }            return nil      }        /// Recursively drills down the path of Actions until it finds the last one that is a FlexibleContainer's Delegate Action      func nestedContainerAction(in containerAction: FlexibleContainerReducer.Action) -> FlexibleContainerReducer.Action? {          if case .horizontal(.delegate) = containerAction {              return containerAction          }            if case .vertical(.delegate) = containerAction {              return containerAction          }            if case .horizontal(.items(.element(id: _, action: let itemAction))) = containerAction {              if case .container(let innerContainerAction) = itemAction {                  return nestedContainerAction(in: innerContainerAction)              }              return nil          }            if case .vertical(.items(.element(id: _, action: let itemAction))) = containerAction {              if case .container(let innerContainerAction) = itemAction {                  return nestedContainerAction(in: innerContainerAction)              }              return nil          }            return nil      }  }
```


Action handling will funnel through a single function in the FlexibleSectionReducer which is vital for SDUI development. It’s usually difficult to debug SDUI when things go wrong, but with our Flexible Framework powered by TCA this task becomes trivial. To determine the source of any Action we simply set a breakpoint inside the Reducer provided to the root Container (designated above by 3.) whereby when you inspect the Action which is the input for this process you see something like:


```text
(lldb) po rootContainerAction  ▿ Action    ▿ vertical : Action      ▿ items : IdentifiedAction<String, Action>        ▿ element : 2 elements          - id : "579EACD9-D224-40D3-B4A6-878897A8280C"          ▿ action : Action            ▿ container : Action              ▿ vertical : Action                ▿ items : IdentifiedAction<String, Action>                  ▿ element : 2 elements                    - id : "B69188BC-9335-47A0-B20F-3C8AD887B876"                    ▿ action : Action                      ▿ button : Action                        - view : AppPresentationSections.FlexibleButtonReducer.Action.ViewAction.buttonTapped
```


And to see the final output or side effects of the Action handling, we set a breakpoint in the body of FlexibleSectionReducer which could look something like:


```text
(lldb) po action  ▿ Action    ▿ delegate : DelegateAction      ▿ interaction : APSInteraction<StandardAction>        - trackingTitle : "hotel_list_button_tapped"        ▿ action : StandardAction          ▿ navigation : NavigationAction<StandardAction>            ▿ route : Route              ▿ hotelList : HotelListParameters                - geoName : "Las Vegas"                - geoId : 123456
```


With the View and Action hierarchies in place, it became trivial to expand the Flexible Framework to add new interactive components.


### Conclusion


These systems may seem like overkill and for most apps I would agree, however Tripadvisor’s content is uniquely dynamic and thus our content delivery system should be as well. Hotel prices are constantly changing, new offers become available, the Design team wants to tweak colors, font weights or layout based on experiment results — the list goes on. Our product managers and designers have become empowered to iterate more quickly on features because they may request a single server-side change, deploy it within hours, and the output will look great natively on both iOS and Android.


The departure from MVVM-C to TCA has been fantastic. It is far simpler to inspect a View and understand all of its capabilities and to compose larger features together. Our ViewModels were rigid as they didn’t easily compose together. It was also difficult to understand the side effects that would take place from firing an event into all of the Combine Publishers owned by a ViewModel.


Since inception our SDUI Flexible Framework has matured and has become the fundamental mechanism for presenting UI to our users. As client developers we can think *bigger picture* about features themselves rather than the repetitive minutiae of building up SwiftUI layout code. We knew the difficulties of SDUI for client developers so we structured the architecture to optimize for easy debugging, client-side mocking of layouts delivered by the server, and many more niceties.


We hope this series of blog posts have been insightful and given you a glimpse into the engineering work that we’ve been doing here at Tripadvisor!


---


[The evolution of Native Engineering at Tripadvisor: Part 3 — Design System Flexible Framework…](https://medium.com/tripadvisor/the-evolution-of-native-engineering-at-tripadvisor-part-3-design-system-flexible-framework-ebf9f27e62be) was originally published in[Tripadvisor Tech](https://medium.com/tripadvisor) on Medium, where people are continuing the conversation by highlighting and responding to this story.
