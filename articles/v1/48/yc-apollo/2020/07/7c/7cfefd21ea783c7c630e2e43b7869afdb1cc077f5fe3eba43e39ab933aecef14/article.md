---
schema_version: "1.0.0"
document_id: "7cfefd21ea783c7c630e2e43b7869afdb1cc077f5fe3eba43e39ab933aecef14"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/client-side-architecture-basics"
published_at: "2020-07-17T10:56:10+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:b0b345fb1af5aa240929053d036ae3169525874ce5c542f3408ca955e705d673"
---

# Apollo Client & Client-side Architecture Basics

Architecture is the foundation, the skeleton, that sets your project up for success. It’s the stuff that we wish we got right from the start because making drastic architectural changes, later on, can be challenging and time-consuming.


Today, we React developers have tools like Context, Hooks, Redux, and xState. We write code that deals with four or five different kinds of state, and we frequently spend time trying to figure to handle things like auth, state management, networking, app logic, and subscriptions.


Since there aren’t a lot of “widely accepted standards for building React apps consistently, coherently, and with minimal risks to \[developers’\] professional reputations and livelihoods”, we’re often left to figure things out as we go ([Joel Hooks](https://twitter.com/jhooks/status/1273392253646434304) , June 17th, 2020).


In this article, we’ll discuss some client-side architecture basics. We’ll discuss the principles that influence modern architecture, the concerns we need to address, the types of state we need to handle, and the role Apollo Client plays.


**Disclaimer:** While there are other ways to design your application, working with state, and separating concerns, this is a conceptual model we like. Even if it doesn’t work for you, I hope it helps you guide conversations towards something that makes sense for your team and your project.


As always, I’m always happy to chat about this stuff and answer questions on[Twitter](https://twitter.com/stemmlerjs) .


## Client-side architecture basics


### Structure vs. Developer experience


An ideal design is something that solves the problem, but not at the expense of developer experience. This means we need to use both **logic** and **empathy** when considering a design that works.


**Logically** , we choose tools capable of solving the problem at hand. What’s going to *work* ? What won’t?


**Empathetically,** what tools or approaches are going to make us *feel good* ? What’s going to be pleasant to work with? What won’t?


If a tool can accomplish the task, and give us feelings of **control, mastery, satisfaction,** and even **pride** (from “The Design of Everyday Things” by Don Norman), then I think we’ve done a good job.


Being forced to use tools with steep learning curves, rules, or seemingly unnecessary complexity and structure don’t make us feel good. A lot of times, it can induce some pretty *negative* emotions. This might be why some developers prefer React.js with its minimal learning curve, slim API, and feeling of *freedom* vs. one like Angular, with a steeper learning curve but a lot more **guidance** around how to structure your app.


Both tools are great. And you can accomplish the same things with both. Some say React has a better **developer experience** , while Angular provides more **structure** . Both of these properties— structure and developer experience are essential. Yet, they seem to conflict with each other.


Software design = structure vs. developer experience


Instead of imposing arbitrary rules, let’s start with what we currently know about client-side architecture in React apps and see if we can move towards a pleasant, yet structured, starting point.


### Model-View-Presenter


Have you heard of[Model-View-Presenter](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter) ? It’s one of the original architectural patterns used to describe how to build user interfaces. Where MVC (Model-View-Controller) explained how to split up the client from the server, MVP decomposed the structure **of the client portion** .


Model-View-Presenter is the architectural pattern typically used within client applications. It’s a derivation of the MVC pattern.


In MVP,


- the **view** creates *user events*
- those user events get turned into **updates** or **changes** to the model
- when the **model** changes, we update the **view** with the new data.


Some developers might notice that this is a *fancy way* to say[Observer pattern](https://en.wikipedia.org/wiki/Observer_pattern#:~:text=The%20observer%20pattern%20is%20a,calling%20one%20of%20their%20methods.)) — and yes! *If you squint hard enough* , you’ll notice that **most client-side architectures *are*** an implementation of the observer pattern.


### The need for a better client-side architecture standard


Let’s be honest, as great as MVP is, it’s a little too generic for the needs of today’s projects.


In both MVC and MVP, it’s clear what the *View* and the *Controller/Presenter* refers to in a React app. That’s the presentation component and container components. But the *model?* We don’t entirely know.


**The biggest challenge is that the` M` is responsible for way too much** .


As a result, it can sometimes be hard to match up the proper tool for the proper task.


#### Tasks of the model


Today, some of the primary concerns that a *model* in a React app has to address are:


- **Networking & data fetching** — Do I have to build out the entire data-fetching layer? Do I have to signal async states manually (` isLoading` )?
- **Model behavior** — What happens when there’s decision-making logic I need to run before a` mutation` ? Where does that go? What if I was building a chess game with lots of game logic? How do I structure my code? How do I handle authenticating certain routes and actions? How do we handle validation logic?
- **State management (model data)** — How do I handle state that belongs to a single component? What about state that belongs to more than one component? What do I use for a state management library? Can I just use Context? How do I set up **reactivity** so that when my data changes, the view re-renders?
- … etc


And in 2020, this is what we see when we open the developer toolbox looking for a solution to the *ambiguous model* problem.


- React hooks
- Redux
- Context API
- Apollo Client
- xState
- react-query


By applying the same influential design principles used to solve the *ambiguous model* problem in back-end programming with a[clean architecture](https://khalilstemmler.com/articles/software-design-architecture/organizing-app-logic/) , we arrive at something reasonable on the client-side.


### Principle #1 – CQS (Command Query Separation)


Separate methods that change state from those that don’t


Command Query Separation is a design principle that states that an operation is either a` command` or a` query` .


- ` commands` change state but return no data, and
- ` queries` return data but don’t change state.


The primary benefit of this pattern is that it makes code **easier to reason about** . Ultimately, it urges us to carve out two code paths: one for *reads* and one for *writes* .


We can apply this principle at several design levels, but the simplest way to see it in action is at the *method level.*


#### **Commands**


Consider the methods` createUser` and` toggleTodo` . These are both` command` -like operations.


```text
function createUser (props: UserDetails): Promise<void> { ... }
function toggleTodo (todoId: number): void { ... }
```


Notice that neither of these methods returns anything. They’re both` void` . That’s what a valid` command` is.


This would also mean that the following methods **aren’t valid commands** .


```text
function createUser (): Promise<User> { ... }
function toggleTodo (): Todo { ... }
```


This is a useful pattern, but there are exceptions. Two good examples are popping an item off the top of a[stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)#:~:text=In%20computer%20science%2C%20a%20stack,that%20was%20not%20yet%20removed.) , or returning the changed data or at least the` id` of the changed data within` mutation` response (we’ll discuss this necessity in “[Demystifying Cache Normalization](https://www.apollographql.com/blog/demystifying-cache-normalization) “).


As a general rule of thumb, we like to start with this approach, and then break it if it feels right.


#### **Queries**


Queries are operations that return data and perform no side-effects. Like these, for example:


```text
function getCurrentUser (): Promise<User> { ... }
function getUserById (userId: UserId): Promise<User> { ... }
```


#### **Why does it matter?**


- Simplifies the code paths — this is what React hooks does with the accessor/mutator API of useState, and what GraphQL does with queries and mutations.
- Operations are easier to reason about — consider how hard (and disastrous) it would be to test a` query` was working properly if it always also performed a side-effect (` command` ) that changed the state of the system.
- You can think of *features* about as **operations** : commands or queries. If you want to make sure that all your features have integration tests, ensure a good separation of commands and queries that the user performs, and test each one. One other interesting discovery: since most **pages/routes** in your app invoke one or more *features,* a potentially maintainable folder structure could be formed by[co-locating](https://kentcdodds.com/blog/colocation) all the concerns and components by features, and then by page/route.
- Cache invalidation is one of the hardest problems in computer science— it’s easier with this. Using CQS, we can be **sure** that when if no new commands were executed (against a particular item), we can continue to perform queries for it by sourcing its data directly from the cache. The moment we execute a command against that item, we invalidate it in the cache. Consider how useful this might be for a state management library. As a side note, it’s good to be able to invalidate as precisely as possible, but it’s also generally safe to invalidate “too much” when you can’t be sure.


**Sometimes, we call operations** *interactions* because they’re *interactions* the user has had with the application. You can read more about CQS[here](https://martinfowler.com/bliki/CommandQuerySeparation.html) .


### Principle #2 – Separation of Concerns


Consciously enforcing logical boundaries between the architectural concerns of your app


Assume we have a list of todos.


When a user clicks *delete* on the todo, what happens next?


```text
export const Todo = (props) => (
<div className="todo">
<div class="todo-text">{props.text}</div>
<button onClick={props.onDeleteTodo}>Delete</button>
</div>
)
```


Well, the *view* passes off the event to a container. That could connect the user event to a method from a React Hook or a Redux thunk. From there, we might want to run some logic, decide if we should invoke a network request, update the state stored locally, and somehow notify the UI that it should update.


That’s a lot. And that’s a *simple* app. And when I said we might want to *run some logic* a moment ago, I wasn’t clear about **exactly** what kinds of logic it could be. It could be authorization logic, validation logic, interaction/domain logic, etc. All of these are valid concerns to address.


Separation of concerns is one of my favorite design principles. It’s about **thinking through the jobs to be done** in your app, delegating them to a particular layer, and ensuring those layers do their jobs, and their jobs *only* .


Separation of concerns doesn’t necessarily mean physically separating code into different folders and files. While you *can* do that, that’s not what SoC is about. It’s more about knowing what concerns your code is handling and ensuring that the correct construct is taking care of it. It’s a **logical separation** instead of a **physical separation.** It is a good idea, though, to locate code and files that often change together and be as close to each other as possible.


#### **Why does it matter?**


- Decomposition. Separation of concerns gives us better visibility as to which tasks need to be done, which layer they belong to, and which tools can address those concerns.
- Feature organization. You can think of all the features of your app as a set of[vertical slices](https://en.wikipedia.org/wiki/Vertical_slice#:~:text=So%20a%20vertical%20slice%20can,every%20component%20in%20the%20software.) . It is “the sum of the work that has to be done in every layer that is involved in getting a specific feature working”. Vertical slices of components, state, and behavior are the features of our app.
- Delegation. Should we build this layer ourselves? Or should we delegate the work to a library or framework. For example, most developers won’t build a custom view-layer library for presentational components — they’ll use React or Vue.js. However, lots of users ***will* build a custom state management system from scratch** .


### A better client-side architecture starting point


Using CQS and SoC, we arrive at a more explicit understanding of how the MVP pattern appears a modern React app. In summary, the concerns are:


- **Presentation components** — Renders the UI and create user events.
- **UI logic** — Contains view behavior & local component state.
- **Container/controller** — The glue layer. Connects user events to the correct interaction/application layer construct, and passes reactive data from global storage to presentation components. We can also think of these as *page* components.
- **Interaction layer** — Model behavior & shared component state. There could be several different stateful models in a single app. The most common way to write code in this layer is using tools like React hooks and/or xState.
- 🌟 **Networking & data fetching** — Performs API calls, fetches data, and signals the state of network requests (meta state).
- 🌟 **State management & storage** — Shared global storage, provides APIs to update data, and configure reactivity.


There’s more to this discussion. If you’d like to dive deeper, see examples, and arguments for each concern in a modern **client-side architecture** , read the full[Client-Side Architecture Basics](https://khalilstemmler.com/articles/client-side-architecture/introduction/) essay.


### The different types of state in client-side apps


Aside from architecture, state is the other big confusing part about designing client-side web apps. In[Jed Watson’s](https://twitter.com/JedWatson) talk from GraphQL Summit 2019 titled, “[A Treatise on State](https://www.youtube.com/watch?v=tBz3UmZG_bk&feature=emb_title) “, he describes five different types of state:` local (component)` ,` shared local (global)` ,` remote (global)` ,` meta` , and` router` .


- ` local (component)` : State that belongs to a single component. UI state. We can extract UI state from a presentation component into a React hook.
- ` shared local (global)` : As soon as state belongs to more than one component, it’s *shared* state. Components shouldn’t need to know about the existence of each other (ex: a header shouldn’t need to know about a todo).
- ` remote (global)` : State that lives behind backend services. When we work with` remote` state, we typically pull chunks of it from a remote API and work with local copies of data from our client app.` remote` state is typically another form of shared state.
- ` meta` : Meta state refers to state *about* state. The best example of this is the` loading` async states that tell us the progress of our network requests.
- and` router` state: The current URL of the browser.


## States and concerns of Apollo Client


Now that we understand the different types of **state** and the concerns to be addressed, we’re ready to discuss some of the problems that Apollo Client is intimately involved with helping you solve.


### Concern — Presentation components


Render the UI and create user events


Presentation components live within the boundaries of the *View* portion of Model-View-Presenter. Their entire purpose is to:


- Display data in the UI and
- Generate user events (from keypresses, button clicks, hover states, etc.)


#### Presentation components are an implementation detail


An implementation detail is a low-level concern that helps us accomplish our goals that **aren’t** the *main* goals. If one of our *main* goals is to hook up the *Add Todo* feature, the buttons, styling, and text in the UI is an implementation detail in realizing that feature.


#### Presentation components can be volatile


Anything subject to frequent change is said to be *volatile* . Us constantly changing the look and feel of components is what makes them so.


One way to accommodate this phenomenon is to decide on a[stable](https://khalilstemmler.com/wiki/stable-dependency-principle/) set of reusable components (that either you wrote or grabbed from a component library), then create your views from those.


Even though we could use reusable components, data requirements change frequently.


Take this simple CardDescription component that uses a GraphQL query to display the description of a card.


```text
const CARD_DESCRIPTION_QUERY = gql`
query CardDescription($cardId: ID!) {
card(id: $cardId) {
description
}
}
`;


const CardDescription = ({ cardId }) => {
const { data, loading } = useQuery({
query: CARD_DESCRIPTION_QUERY,
variables: { cardId }
});


if (loading) {
return null;
}


return <span>{data.card.description}</span>
}
```


How likely is it that we’d need to change the styling? What about displaying something like a` lastChanged` date beside it? Chances are pretty likely.


#### Should we include GraphQL queries in our presentation components?


Yes!


Place things that change for the same reason as close to each other as possible.


It’s good to have GraphQL queries **as close to the presentational component as possible** . Queries define the data requirements. And since they’ll likely need to be changed together if the requirements change, having them close together reduces unnecessary cognitive load accrued by flipping back and forth between files.


One potential *downside* to putting your queries in your components is that now if you ever wanted to switch away from GraphQL, your components aren’t pure — they’re coupled to GraphQL. If you wanted to switch transport-layer technologies, you’d have to refactor every component.


Another *potential* downside is that to test these components, you’d need to make sure they’re wrapped in a[mocked Apollo Client provider](https://www.apollographql.com/docs/react/development-testing/testing/#mockedprovider) . I see that as an upside. The` mockedProvider` API is a very powerful testing tool.


My recommendation is to couple the queries to the components. What you gain in an incredible developer experience is, in my opinion, worth the risk of going fully in with GraphQL and deciding you want to change it later down the road.


**Note on query performance** : It’s ok to have lots of queries for super-specific chunks of data like shown above. Using Apollo Client, Apollo handles that complicated logic of checking whether the data is cached already and if not — it emits a request to get it.


#### Queries subscribe to state changes


Normally, it’s the *controller/container component* that needs to be alerted when state has changed so that it can pass props down to presentation components. At least, when we used Redux with Connect, this is how it worked.


But since Apollo Client queries get notified automatically when the data they’re subscribed to changes, it **removes the need to feed props through a container component** .


What *do we need* a container component for then? Two things.


- Acting as a top-level *page component* in apps with multiple routes. The *page component* loads up all the presentation components and features for that page.
- Passing commands from presentation components up to the model **(interaction/application) layer** to perform decision-making logic. Sometimes, that decision-making logic may then decide that we should invoke a mutation.


#### Should we include GraphQL mutations in our presentation components?


I don’t recommend putting GraphQL mutations inside of presentational components.


Keep presentational components free of decision-making logic, and let the model portion of your app do that.


Why?


CQS was all about separating the read and write paths. It said that operations that read should read only and refrain from performing side-effects. We like this because it makes code easier to reason about. Earlier, we saw an example of this at the method level.


Think about how CQS applies to *components* as well.


If presentational components are for *reading* , doesn’t that imply they **shouldn’t be involved** in deciding when state changes? Doesn’t that mean presentational shouldn’t be responsible for *writes* ?


Correct.


If decision-making logic needs to happen, that’s a concern of the *model* , not the *view* .


For example, consider a chess game. If we had a ChessPiece component, we wouldn’t want the ChessPiece to execute the move mutation. Why? Because there’s **game logic** to enforce. Perhaps that piece is only allowed to move diagonally or in an L-shape. Delegate game logic and any decision-making logic that determines if we should invoke a command-like operation to the **interaction layer** .


The entire model portion of a modern client-side architecture containing interaction logic, data fetching, state management, and even a client-side cache (or store).


In React, we implement interaction (sometimes called *application* ) layer logic with React hooks.


Here’s an example of a` createTodoIfNotExists` operation using Apollo Client and React Hooks that first performs some decision-making logic before potentially invoking a` mutation` .


```text
function useTodos (todos) {


const createTodoIfNotExists = (text: string) => {
const alreadyExists = todos.find((t) => t === text);


if (alreadyExists) {
return;
}


...
// Validate text
// Perform mutation
}


return { createTodoIfNotExists }
}


// Container
function Main () {
const { data: todos } = useQuery(GET_ALL_TODOS);
const { createTodoIfNotExists } = useTodos(todos);


...
}
```


If state machines are your thing, you can also use a tool like xState to build your interaction logic model instead.


### Concern — State management and storage


Storage, updates, and reactivity


Apollo Client is a state management library that handles the *entire* state management portion of our application. In[Dispatch This: Using Apollo Client 3 as a State Management Solution](https://www.apollographql.com/blog/dispatch-this-using-apollo-client-3-as-a-state-management-solution) , we learned about the **three primary responsibilities** of a state management solution.


- **Storage** — To hold onto **global state** ; could be either` remote (global)` state of data retrieved from backend services,` shared local` state, or a mixture of both.
- **Updating state** — Most of the time, after invoking an operation, we need to update cached state as a side-effect.
- **Reactivity** — When state changes, we need to notify pieces of our UI that it should re-render with the new data.


In Apollo Client, we use the **normalized cache** as storage, **the cache APIs** to update state, and **(auto)** **broadcast changes** to queries throughout the app when state changes.


Apollo Client handles both the state management and data fetching concerns.


### Concern — Networking & data fetching


Performing API calls and reporting metadata state


Many state management libraries find it convenient to handle the **data-fetching &** **networking** concern as well. In Apollo Client, this is handled for you.


The responsibilities of a networking & data fetching layer are to:


- Know where the backend service(s) are
- Formulate responses
- Marshal response data or errors
- Report async statuses (` isLoading` , errors) — this is a form of` meta` state.


### State — Local (component) state


State belonging to a single component


You ***can*** use Apollo Client for components that want to maintain their own private component local state (as we’ll learn in[Local State Management with Reactive Variables](https://www.apollographql.com/blog/local-state-management-with-reactive-variables) ) but in most cases, you’ll likely use a tool like React’s` useState` instead.


### Don’t model your data graph for individual components


Yes, GraphQL *is* about modeling and utilizing a data graph to work with the data you need. But GraphQL is about interacting with data that **transcends individual components** .


For example, it might make sense to have an` isToggled` piece of state for each of your` Todo` items as a part of the UI component, but should your data graph know about` isToggled` ? No, that’s highly specific to the UI component.


If your graph contains objects that are specific to (or named after) your UI components, you might be designing your graph the wrong way around.


### State — **Remote state**


State retrieved from backend services


Apollo Client is solely responsible for` remote` state, which is cached chunks of a remote data graph.


Invoking queries with the` useQuery` hook fetches the data we need and provides it to the caller, but it also normalizes and caches.


```text
// `data` holds the remote state of our data
const { data, loading, error } = useQuery(GET_ALL_TODOS);
```


By caching chunks of the data graph, next time we ask for that same part of the graph, Apollo Client is smart enough to go straight to the cache instead of trying to pull it from another network request.


### State — **Meta state**


State *about* state


Out of these five different types of state, there are **three** that Apollo Client can specifically address.


The first is` meta` state. This is state *about* state. When we make` queries` or` mutations` using the` useQuery` and` useMutation` hook, we get an object that contains details about the current state of the request.


```text
// The 'loading' variable holds the meta state of the network request.
const { data, error, loading } = useQuery(GET_ALL_TODOS);
```


With Apollo Client, that’s handled for us. Though if we were to use a more barebones approach, like Axios and Redux, we’d have to write this signaling code ourselves within a Thunk.


```text
export function createTodoIfNotExists (text: string) {
return async (dispatch, getState) => {
const { todos } = getState();


const alreadyExists = todos.find((t) => t === text);


if (alreadyExists) {
return;
}


// Signaling start
dispatch({ type: actions.CREATING_TODO })


try {
const result = await todoAPI.create(...)


// Signaling success
dispatch({
type: actions.CREATING_TODO_SUCCESS,
todo: result.data.todo
})
} catch (err) {


// Signaling Failure
dispatch({ type: actions.CREATING_TODO_FAILURE, error: err })
}


}
}
```


### State — **Shared local state**


State used by multiple components


State is` shared` when used by more than one component. Since` remote` state is typically utilized by several components in an app, we consider it a form of` shared` state.


` shared` state can be either:


- ` remote` state,
- *client-side only*` local` state,
- or combination of both


#### **Combining both remote and client-only local state**


If we need to add additional data to our` remote` state, that’s possible. We can use Apollo Client for that.


For developers coming from Redux, here’s an example.


In a todo app, we can add a *client-side only* piece of` shared local` state to each of the` todos` before it gets merged to the store. In this case, we’re adding the` isSelected` attribute.


```text
switch (action.type) {
...
case actions.GET_TODOS_SUCCESS:
return {
...state,
// Add some local state to the remote state before merging it
// to the store
todos: action.todos.map((t) => { ...t, isSelected: false })
}
}
```


Though it’s less of a common use case, if we needed to do this in Apollo Client 3, here’s the equivalent with cache policies and reactive variables.


```text
import { makeVar } from "@apollo/client";


export const currentSelectedTodoIds = makeVar<number[]>([]);


export const cache: InMemoryCache = new InMemoryCache({
typePolicies: {
Todo: {
fields: {
isSelected: {
read (value, opts) {
const todoId = opts.readField('id');
const isSelected = !!currentSelectedTodoIds()
.find((id) => id === todoId)


return isSelected;
}
}
}
}
}
});
```


Finally, in order to request both` remote` and *client-only*` local` state from the same query, we can use the` @client` directive to inform the cache what exists on the client-side only, and what can be resolved remotely from the data graph.


```text
export const GET_ALL_TODOS = gql`
query GetAllTodos {
todos {
id
text
completed
isSelected @client
}
}
`
```


In the following guide,[Local State Management with Reactive Variables](https://www.apollographql.com/blog/local-state-management-with-reactive-variables) , we explore how to solve common scenarios that involve working with local state in addition to remote state.


## Conclusion


Building robust, testable, flexible, and maintainable React applications is not necessarily an *easy* task. But having a clear understanding of the different concerns and types of state we’ll need to deal with is a great starting point. From here, we can design out how we want to handle some of the most common problems in a client-side architecture.


Here’s a summary of recommendations:


- At the UI layer, keep your GraphQL queries close to your presentational components.
- For local (component) state that is **not shared** , React’s useState hook is the best tool for the job.
- For shared (global) state, you can use either Apollo Client 3’s local state management APIs or again, the React hooks API.
- Prefer placing mutations (and all *model behavior* ) in the interaction layer instead of in components.
- Let Apollo Client handle your entire **state management** and **networking & data fetching** concerns.
- Let Apollo Client handle your remote (global) state since it can efficiently fetch, normalize, and cache data for subsequent requests.


### Continue reading


[Local State Management with Reactive Variables](https://www.apollographql.com/blog/local-state-management-with-reactive-variables)


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
