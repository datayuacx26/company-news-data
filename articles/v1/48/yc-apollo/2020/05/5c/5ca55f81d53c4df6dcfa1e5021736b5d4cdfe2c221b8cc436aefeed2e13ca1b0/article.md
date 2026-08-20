---
schema_version: "1.0.0"
document_id: "5ca55f81d53c4df6dcfa1e5021736b5d4cdfe2c221b8cc436aefeed2e13ca1b0"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/dispatch-this-using-apollo-client-3-as-a-state-management-solution"
published_at: "2020-05-13T08:48:59+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:f5b2276b1a9e0e7c0c33e678cb32471297e101771a4862cfbe141e8eb709eb60"
---

# Dispatch This: Using Apollo Client 3 as a State Management Solution

A lot of developers understand how to use Redux or React Context for state management but are left confused about how to use Apollo Client as a replacement.


In this article, we’ll break down the **responsibilities of a state management solution** , discuss **what’s involved in building a solution from scratch** , and **how Apollo Client 3 introduces new ways to manage local state and remote data in harmony** .


You can also watch the talk presented at Apollo Space Camp 2020 by Khalil Stemmler[here on YouTube](https://youtu.be/xASrlg9rmR4) .


## The responsibilities of a state management solution


State management is one of the most challenging parts of every application, and historically, when starting on a new React project, we’ve had to design and implement the state management infrastructure from scratch in a bare-bones way.


No matter which approach we take, in every client-side application, the generic role of a state management solution is the same: to handle storage, update state, and enable Reactivity.


### Storage


Most apps need to hold onto some data. That data may contain a little slice of **local state** that we’ve configured client-side, or it could be a subset of **remote state** from our backend services.


Often, we need to combine these two pieces of data, local and remote, and then call upon them in our app at the same time. This task alone has the potential to get pretty complicated, especially when we need to perform **updates to the state** of our app.


### Update state


The[Command-Query Segregation Principle](https://khalilstemmler.com/articles/oop-design-principles/command-query-segregation/) states that there are two generic types of operations we can perform:` commands` and` queries` .


In GraphQL, we refer to these as` queries` and` mutations` .


In REST, we have several` command` -like operations like` delete` ,` update` ,` post` , etc and one` query` -like operation called` get` .


Most of the time, after invoking an operation in a client-side web app, we need to update the state stored locally as a side-effect.


### Reactivity


When storage changes, we need an effective way to notify pieces of our UI that relied on that particular part of the store, and that they should present the new data.


### Each state management approach has a slightly different approach


Just about every library available out there right now can adequately handle all three of these responsibilities! Here are some of the most popular approaches right now in the React realm.


**Redux**


- Storage: Plain JS object
- Updating state: actions + reducers
- Reactivity: Connect


**React Context + Hooks**


- Storage: Plain JS object
- Updating state: useReducer (or not)
- Reactivity: useContext


**Apollo Client**


- Storage: Normalized cache
- Updating state: Cache APIs
- Reactivity: (Auto) Broadcast change notifications to Queries


## Choosing a state management approach


Comparing Redux or React Context for state management vs. Apollo Client is that with Redux and React Context, we still have to write design the infrastructure around handling fetch logic, async states, data marshaling, and more. In the end, we still have to make a lot of those design decisions on our own.


For smaller projects, I think this is an elegant approach, but having worked on massive Redux projects in the past, I’m acutely aware of how challenging it can get to test and maintain the additional code.


### State management infrastructure can account for up to 50% of the work in building a client-side application


At 3:05 in the talk, we discuss a *generic client-side architecture* that breaks down the components involves in modern apps as:


- Presentational components with UI Logic
- Container components
- Interaction logic
- Infrastructure (fetch and state management logic)
- and a client-side store


Laying this all out visually, we can identify that the section of the architecture that handles storage, updating state, and reactivity – is actually about half of the work.


At Apollo, we call this half of the work the client-side data layer.


There’s more to this discussion. If you’d like to dive deeper, see examples and arguments for each concern in a modern **client-side architecture** , read the full[Client-Side Architecture Basics](https://khalilstemmler.com/articles/client-side-architecture/introduction/) essay.


### Building data layer infrastructure is expensive


Having to spend time building out the entire data layer’s infrastructure is not a light task. Here are a number of concerns handled from within the data layer:


- Fetch logic
- Retry-logic
- Representing async logic (loading, failure, success, error states)
- Normalizing data
- Marshaling request and response data
- Facilitating optimistic UI updates
- … and more


We consider these tasks a part of the necessary **infrastructure** needed to interact with our data. They provide the baseline for which we can build our app on top of. With Redux or React Context, if we want this infrastructure, we have to build it, test, and maintain it ourselves using our own custom code.


At Apollo, we see these concerns as things that should be done for you so that you can move onto the **application-specific** tasks like:


- Declaratively asking for the data you want
- Building out presentational components
- Implementing UI & interaction logic


These are things that only *you* can do because they’re specific to your app, and it’s what makes your app special.


Using Apollo Client 3 as a state management solution, we start out with an answer to all of our data layer concerns. Out of the box, Apollo client comes with storage, updates, and reactivity set up.


When working with Apollo Client, most of the work we do can be thought of as falling within one of three categories:


- 1 – Cache configuration
- 2 – Fetching Data: Query for the data you need using the` useQuery` hook
- 3 – Changing Data: Update data using the` useMutation` hook


Apollo Client gives us a set of APIs to configure, fetch, and update our client-side state


### Tradeoffs of using Apollo Client 3 over other state management solutions


Choosing Apollo Client 3 over other state manage solutions like Redux or React with Context + useReducer is a tradeoff.


Going the Apollo Client route, we’re left to **learn the cache and query APIs** at the benefit of having the data layer infrastructure done for us. Choosing Redux or React with Context over Apollo Client means **building the infrastructure to interact with the data layer and then testing, consuming, and maintaining them as well** .


## Using Apollo Client 3 for common state management use cases


Apollo Client 3 also just shipped with several new APIs that introduce much cleaner ways to work with the cache and handle common use cases.


You can learn how to use Apollo Client 3’s new APIs to build a Todo app using Local State (Reactive Variables), and the new Advanced Cache Modification APIs by checking out the[Apollo Client 3 State Management Examples](https://github.com/apollographql/ac3-state-management-exampleshttps:/github.com/apollographql/ac3-state-management-examples) on Github.


### Configuring the cache


A basic configuration involves creating a new` InMemoryCache` with no options pointing the` ApolloClient` instance to the remote GraphQL API.


```text
import { ApolloClient, InMemoryCache } from '@apollo/client';


const cache = new InMemoryCache();
const client = new ApolloClient({
uri: '<https://api.todos.com/>',
cache
});
```


*One improvement of Apollo Client 3 is that everything has been moved to live under` @apollo/client`* .


Check out the[Getting Started docs](https://www.apollographql.com/docs/react/v3.0-beta/get-started/) for more detail.


### Fetching data


The data fetching experience hasn’t changed much in the latest Apollo Client 3 release. To pull data from a remote API into the cache so that you can present it in a component, see the following code.


- Import the` useQuery` hook from @apollo/client
- Ask for the data you want using a GraphQL query
- Pass the query into the hook
- (Optionally) Handle async states
- (Optionally) Pass the data to a presentational component


```text
import React from 'react'
import { TodoList, Loader } from '../components';


import { filterTodos } from 'utils';


import { useQuery } from '@apollo/client';
import { gql } from '@apollo/client'


export const GET_ALL_TODOS = gql`
query GetAllTodos {
todos {
id
text
completed
}
}
`


export default function ActiveTodosList () {
const { loading, data, error } = useQuery(
GET_ALL_TODOS
);


if (loading) return <Loader/>
if (error) return <div>An error occurred</div>
if (!data) return <div>No data!</div>;


return <TodoList
todos={filterTodos(data.todos, ‘active')}
/>;
}
```


The` useQuery` hook abstracts away all of the complex data fetching, state management, and marshaling logic that we used to have to do with bare bones approaches. A couple of the things it does behind the scenes are:


- Forming and sending the request
- Updating async states (loading, error, data)
- Normalizing and caching the response
- Saving bandwidth by delegating future requests for the same data and sourcing them from a local copy in the cache instead of going out onto the network


### What does cached data look like?


A lot of developers are familiar with what the cache looks like in Redux or React with Context because the cache is a plain ol’ JavaScript object. Let’s see what the cache looks like in Apollo Client after having fetched some data.


Assume we have a basic` GetAllTodos` query like the one shown in the previous section, and calling it returns a list of todos that we render to the UI like so.


If we were to take a look at the cache, it would look something like this:


```text
{
"Todo:1": { __typename: "Todo", id: 1, text: "Getting started”…},
"Todo:2": { __typename: "Todo", id: 2, text: "Second todo”…},
"Todo:3": { __typename: "Todo", id: 3, text: "Third todo”…},


“ROOT_QUERY": { __typename: “Query", todos: {…} …},
...
}
```


Because the cache is **normalized** , it makes sure that there is only ever one reference to an item returned from a query.


Uniqueness is determined by constructing a cache key (which you can now configure using the key fields API). Read about this in[Configuring the Cache > Customizing identifier generation by type](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-configuration/#customizing-identifier-generation-by-type) in the docs.


### Updating data


In Apollo Client 3, there are a few new additions to the way we update the cache after our` mutations` , namely the name advanced cache manipulation APIs.


Assuming we’re working with the same todo app and we want to perform an` AddTodo`` mutation` , the steps are as follows.


- Write a GraphQL mutation (including the new item in the mutation response to automatically normalize it in the cache)
- Import the` useMutation` hook
- Pass the` mutation` into the hook
- Handle async states
- Connect the` mutation` to a component and invoke it on a user event.


```text
import React from 'react';
import { Header, Loading } from '../components'
import { gql, useMutation } from '@apollo/client';


export const ADD_TODO = gql`
mutation AddTodo ($text: String!) {
addTodo (text: $text) {
success
todo {
id
text
completed
}
error {
message
}
}
}
`


export default function NewTodo () {
const [mutate, loading] = useMutation(
ADD_TODO,
)


if (loading) return <Loader/>


return <Header addTodo={(text) => mutate({
variables: { text }
})}/>;
}
```


This is usually enough to create a new item in the cache. Though sometimes, when we’re working with **cached collections** (arrays of data that we’ve previously fetched), in order to get lists to visually re-render in the UI, we need to use the` update` function in the` useMutation` options to specify the *application-specific way* that the cache should handle the new item.


In the case of **adding a new todo** , the behavior we would like to take is to add the new item to the end of the list of **cached** todos. Here’s how we may do that using the` cache.readQuery` and` cache.writeQuery` APIs in the` update` function.


```text
const [mutate, loading] = useMutation(
ADD_TODO,
{
update (cache, { data }) {
const newTodoFromResponse = data?.addTodo.todo;
const existingTodos = cache.readQuery({
query: GET_ALL_TODOS,
});


cache.writeQuery({
query: GET_ALL_TODOS,
data: {
todos: existingTodos?.todos.concat(newTodoFromResponse)
},
});
}
}
)
```


Apollo Client 3 introduced advanced cache manipulation APIs for power users (` modify` ,` evict` ,` gc` ). You can read more about them in this[blog post](https://www.apollographql.com/blog/first-impressions-with-apollo-client-3-2ae2a069ab2f) and[the docs](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-interaction/#garbage-collection-and-cache-eviction) .


### Local State Management improvements with Cache Policies and Reactive Variables


My personal favorite new features about Apollo Client 3 are *Cache Policies* and *Reactive Variables.*


**Cache Policies** Cache Policies introduce a new way to modify what the cache returns before` reads` and` writes` to the cache. It introduces cleaner patterns for setting default values, local state management ⚡, pagination, pipes, filters, and other common client-side use cases.


You can learn more about[Cache Policies via the docs](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-configuration/) .


**Reactive Variables**


Reactive variables are containers for variables that we would like to enable cache Reactivity for. The cache is notified when they change (this means that if we’re using React, it will also trigger a re-render).


```text
// Create Reactive variable
export const todosVar = cache.makeVar<Todos>();


// Set the value
todosVar([]);


// Get the value
const currentTodosValue = todosVar();
```


The coolest part of this means that Reactive Variables can be changed from **wherever you want** , **however you want** – that’s a huge improvement over having to do all of your local state management in local resolvers (as we previously did in AC2.x).


For a walkthrough on how to use Cache Policies and Reactive Variables to perform local state management,[check out the demonstration from the talk](https://youtu.be/xASrlg9rmR4?t=965)[.](https://youtu.be/xASrlg9rmR4?t=876) Reactive Variables docs coming soon!


## Conclusion


We learned about the three basic pillars of state management solutions:


- Storage
- Update state
- Reactivity


We also learned how much work it is to build out the data layer yourself and when you might want to consider using Apollo Client instead of a bare-bones state management solution.


Finally, we took a look at some of the new Apollo Client 3 APIs and how they can be used to implement common client-side state use cases.


## Resources


- You can download the slides for this talk[here](https://drive.google.com/open?id=1THqZLPwuJ1dcVPl3SyJrKJFElh2c8_mL) .
- [Apollo Client 3 State Management Examples](https://go.apollo.dev/c/ac3-examples)
- [Apollo Client 3 Roadmap](https://github.com/apollographql/apollo-client/blob/master/ROADMAP.md)


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
