---
schema_version: "1.0.0"
document_id: "dd29b6b9dba06e816be382c0f725459e80c7e9baff80ff80cd1a82cf5a3532af"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/demystifying-cache-normalization"
published_at: "2020-07-17T10:55:32+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:dd13ed4ec8733756fece2b2775b48957aff9e29508398a55d37e63e7b8c9bc09"
---

# Demystifying Cache Normalization

Apollo Client has a particularly challenging responsibility: **to** **make interconnected GraphQL data *easy to use* on the client-side** .


In most rich client applications, we need the ability to cache data and pass it to components. We also need to **know when to re-fetch data vs. when to return what’s already cached;** this helps to avoid making unnecessary network requests.


This kind of caching logic can be hard to implement, even if you’re not using GraphQL.


To make efficient use of GraphQL’s *graphical* data, and to gain insight into when to pull from the cache vs. when to make network requests, Apollo Client acts as an **abstraction** over top of those small normalized segments of a data graph that we cached on the client-side.


Apollo Client is a robust abstraction with cache APIs overtop of the data that it normalizes and caches.


Something exceptional happens here.


By acting as a *storage facade* , Apollo Client can **intercept requests for queries** and auto- *magically* deduplicate them.


It can automatically cache and normalize new data in query responses.


It can also automatically update the cache after mutations, though this depends mainly on whether the mutation updates a **single existing entity** or creates, deletes, or modifies` multiple` entities.


In this article, we’re going to learn:


- The architecture of caching in Apollo Client
- The algorithm that the cache uses to normalize objects returned from operations (queries/mutations)
- What types of operations that cache *can* automatically update cache for.
- What types of operations the cache *can’t* automatically update the cache for, and examples for handling those scenarios.


**Before reading this article, you would benefit from the following:**


- Knowing the basics of state management using Apollo Client, Redux, React Context, or another approach.
- Knowing how to use Apollo Client to query for and mutate data using GraphQL.
- (optional) Understanding how[Fetch Policies](https://medium.com/@galen.corey/understanding-apollo-fetch-policies-705b5ad71980) work.
- (optional) Watching/reading “[Dispatch This: Using Apollo Client 3 as a State Management Solution](https://www.apollographql.com/blog/dispatch-this-using-apollo-client-3-as-a-state-management-solution) “.


Alright! Let’s get into it.


## Data normalization


Normalization is a technique used to organize data in a way that reduces **data redundancy** .


Typically, when we’re structuring data to be stored somewhere (whether that be a database, a client-side cache, or a JSON object), we want to reduce the amount of duplicate data saved. Ideally, we aim to have *no duplicate data* .


Relational databases set a great example of this. Through the use of **relationships** (primary keys, foreign keys) and constraints, we can **enforce** **unique** data getting added to the database only.


A normalized relational database example. There is no duplicate data between the todos and users table because they refer to each other.


Relational databases are pretty robust. If we set up the relationships and constraints correctly, we can ensure they **reject any attempts to add duplicate data or refer to objects that no longer exist** . I consider this a good thing because it keeps your data clean, consistent, and as small as possible.


It’s like the DRY principle, but for storing data.


### Storage facades


What do relational databases have to do with Apollo Client? Not a lot, except for maybe this *one* thing. **The** **architecture around how they provide access to the underlying data is similar** . They both use a **facade** .


The *facade pattern* exposes an additional top-level layer of code that is much easier to deal with than the lower-level stuff. So essentially, a facade is an API.


In a relational database, we are given ways to:


1. Define the shape of the database (DDL), and
2. Retrieve and modify saved data (SQL)


Most people prefer to work with these high-level APIs than interact with the data (stored in files) directly.


Similarly, in Apollo Client, we get *cache APIs.* These enable us to:


1. Configure and design the shape of our client-side cache with cache policies
2. Query data using` useQuery` (or even` client.readQuery` or` client.watchQuery` )
3. Mutate data using` useMutation`


Apollo Client, and any other technology that provides a set of tools for you to interact with cached data, is a **Storage Facade** .


The important conclusion to draw here is that by minimizing direct access to the *actual* data with a facade or API, it provides the ability for the tool to enable things like *data* *normalization* (and reactivity) under the hood.


That’s what Apollo Client does.


In more bare-bones approaches like Redux or React Context, *data normalization* is something that the developer must build into their state management architecture manually.


## Understanding Apollo Client’s normalization algorithm


When we perform operations, Apollo Client normalizes the response data before saving it to the cache.


From the docs on[Data Normalization](https://www.apollographql.com/docs/react/caching/cache-configuration/#data-normalization) *,* the algorithm can be explained in three steps. It works by:


1. Splitting the results into individual objects
2. Assigning a **logically** **unique identifier to each object** so that the cache can keep track of the entity in a stable way
3. Storing the objects in a flattened data structure (normalized items)


Let’s walk through a real-world example step by step and observe how the algorithm works.


The following example uses the *Apollo Client 3 Todos app example from*[@apollographql/ac3-state-management-examples](https://github.com/apollographql/ac3-state-management-examples) *.*


### Fetching and splitting a list of todos


Assume we have a todo app. To get all of the` Todos` behind our data graph, we can call the` GetAllTodos` query.


Getting all todos from a GraphQL server.


The query response contains a list of` todos` .


Response data containing a todos array returned from a GraphQL server.


The first step of the normalization algorithm is to **split the array’s items into individual objects** like so.


### Assigning a unique identifier to each object


The next stage is to **assign a unique identifier to each of the items** . By default, Apollo Client uses the` id` +` __typename` to create one.


It’s important to note that it’s also a very real possibility you may be using a GraphQL API that returns data *without* an` id` **field** . If we have the ability to adjust the design of our data graph to include an` id` field for each type, then it’s recommended to take that approach.


If we *can’t* change it, then we might be forced to think of other ways to **reliably establish uniqueness** for each of our items.


The[key fields API](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-configuration/#customizing-identifier-generation-by-type) provides us with the ability to customize what we want to use as a unique identifier.


For example, perhaps the` id` field went by a different name. Maybe it was named` todoId` . That’s a quick fix.


```text
const cache = new InMemoryCache({
typePolicies: {
Todo: {
// The unique identifier for a todo was actually listed
// as "todoId", so let's use this instead.
keyFields: ["todoId"],
}
},
});
```


What if there wasn’t a` todoId` field either? What do we do now?


Hopefully, there are other fields that we could use to construct a unique identifier.


Consider what we might be able to do if a` Todo` object looks like this:


```text
{
__typename: "Todo",
text: "First todo",
completed: false,
date: "2020-07-08T15:05:32.248Z",
user: {
email: "me@apollographql.com",
}
}
```


Potential uniqueness could be constructed using the` date` field and the nested` email` field as well.


```text
const cache = new InMemoryCache({
typePolicies: {
Todo: {
// If one of the keyFields is an object with fields of its own, you can
// include those nested keyFields by using a nested array of strings:
keyFields: ["date", "user", ["email"]],
}
},
});
```


This` keyFields` configuration would create an identifier string that looks like.


```text
Todo:{"date":"2020-07-08T15:05:32.248Z","user":{"email":"me@apollographql.com"}}
```


Why is uniquely identifying our items so important?


Uniquely identifying items is important for Apollo Client because that’s the way it keeps track of the same object being returned from multiple queries. It’s how the object’s fields can be merged together over time in the cache.


### Storing the objects in a flattened data structure


Once each item has a unique identifier, Apollo Client stores the objects in a **flattened JavaScript object** . This is the raw, normalized JavaScript object at the center of the Apollo Client cache. It looks like this.


By storing each of the normalized items *flat* , it makes them accessible through their unique ids (like a hash-table). If you know a thing or two about hash-tables, you’ll know that retrieval is **very fast** given we know the identifier of the item we’re looking for.


One other concern is **ordering** .


Since we fetched an *array* of items, we want to maintain the original ordering the items came in by.


To accomplish this, the cache actually stores the` GetAllTodos` query, any variables we passed to it, and the result as well.


A couple things to note about this:


- Apollo Client caches any GraphQL operations, the variables included, and the results. Apollo Client does this for both` queries` and` mutations` .
- Saving the entire` todos` query response maintains the ordering when displayed to the UI.
- Instead of duplicating each` todo` in the cached` todos` query, it maintains **references** to the **normalized** todo items by their unique identifiers. This is normalization at work. This is how we keep the size of the cache as small as possible and prevent duplicate data.
- This internal data is intended to be easily **JSON-serializable** , so you can take a snapshot with` cache.extract()` , save it somewhere, and later restore with` cache.restore(snapshot)` .


## Using cached data


Traditionally speaking, the whole point of a cache is to reduce needing to make extra network calls, right?


By default, when we ask for data, Apollo Client attempts to source it from the cache directly. If the data is present, then that’s whats used.


If the data wasn’t already cached, **or if we’re asking for more fields** , then we make another request and cache the response again. There’s a feature called *fetch policies* . It dictates how the cache behaves when we ask for data that may or may not be cached. The default fetch policy is called **cache-first** , and this is how it works.


For example, if we were to:


1. Perform a` GetTodoById` query for a` Todo` with an` id` of` "1"` , persisting that entity into our normalized cache
2. Call` GetTodoById` with the same` id` argument` "1"`


… then Apollo Client could just reach into the cache and get the object directly without making another request.


For more info on fetch policies, read “[Understanding Apollo Fetch Policies](https://medium.com/@galen.corey/understanding-apollo-fetch-policies-705b5ad71980) ” and read[the docs on Fetch Policies](https://www.apollographql.com/docs/react/api/react-apollo/#optionsfetchpolicy) .


In contrast, consider the following scenario:


1. Perform a` GetAllTodos` query, normalizing and caching all` Todo` s from a backend
2. Call` GetTodoById` with an` id` argument that matches the` id` of one of the` Todo` objects we fetched in step 1


This scenario still results in two network calls by default. That’s because Apollo Client does not assume that` GetTodoById` will return the same type of object as an item returned by` GetAllTodos` .


For more info on specifying cache behavior across different queries, read the[Cache redirects](https://www.apollographql.com/docs/react/caching/advanced-topics/#cache-redirects) documentation.


### How to ensure Apollo Client updates the cache


In order for Apollo Client to update the cache automatically, we have to remember to **always return the new data in operation responses.**


For` query` responses, that’s the point. The entire purpose of a query is to return data and cache it.


But for` mutations` , like` editTodo` that *change* a single entity, we should be able to update the item automatically **if we return the value in the mutation response** .


Let’s walk through it.


Here’s a mutation called` EditTodo` that returns the new` todo` value in the mutation response.


```text
mutation EditTodo ($id: Int!, $text: String!) {
editTodo (id: $id, text: $text) {
success
todo {          # <- Returning it here
id
text
completed
}
error {
... on TodoNotFoundError {
message
}
... on TodoValidationError {
message
}
}
}
}
```


By returning the new version of the` todo` that we’re editing in the` mutation` response, the Apollo Client normalization algorithm does the following:


1. Parses the` todo` object from the response.
2. Determines its unique identifier using the default` __typename` +` id` field or Key Field configuration.
3. Determines that the identifier already exists as a **normalized item** in the cache, then *merges* with that object, preferring the *new* field values over the old ones. It’s also helpful to note that you can use a[custom merge function](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-field-behavior/#the-merge-function) to change the default behavior of simply overwriting the old fields.


We can invoke the` editTodo` mutation using the` useMutation` hook.


```text
import React from 'react';
import { gql, useQuery } from "@apollo/client";
import Todo from '../components/Todo'


const EDIT_TODO = gql`
mutation EditTodo ($id: Int!, $text: String!) {
editTodo (id: $id, text: $text) {
success
todo {
id
text
completed
}
error {
... on TodoNotFoundError {
message
}
... on TodoValidationError {
message
}
}
}
}
`


export const TodosContainer = () => {
const todos = getTodos();
const [mutate, { data, error }] = useMutation(
EDIT_TODO
)


...


return todos.map((todo, i) => (
<Todo
key={i}
actions={{
editTodo: (id, text) => mutate ({
variables: { id, text }
})
}}
/>
))
}
```


If we ran the` EditTodo`` mutation` on the third todo (` Todo:3` ), changing the text from “best todo” from “Third todo”, the` mutation` response data would look like this.


```text
{
editTodo: {
todo: {
id: 3,
text: "Best todo",
completed: false
}
}
}
```


And without any further intervention on our part, the Apollo Client should automatically *merge* the response data into the cache because it recognizes the` Todo:3` identifier that was returned by the earlier query.


And since the todos query points to the updated` Todo:3` , any components in the UI that rendered that list of todos (such as a` <TodoList/>` component), would get a re-render to display the newly changed text value of` Todo:3` .


## Operations the cache can automatically update


The cache can automatically normalize, cache, and update *queries* , *mutations that update a single existing entity* , and *bulk update mutations* that return the entire set of changed items.


### **Queries**


As shown before, if we return new data, the cache splits it into singular objects, creates unique identifiers, and saves each of those items (in addition to the` query` itself and any the variables included) to the cache.


**Examples**


` GetAllTodos`


Normalizes and caches all items returned in the query response. If an item already exists, it merges it, preferring the new data.


```text
import React from 'react';
import { gql, useQuery } from "@apollo/client";
import Todo from '../components/Todo'


export const GET_ALL_TODOS = gql`
query GetAllTodos {
todos {
id
text
completed
}
}
`


export default function TodoList () {
const { loading, data, error } = useQuery(
GET_ALL_TODOS
);


if (loading) return <div>Loading...</div>
if (error) return <div>An error occurred {JSON.stringify(error)}</div>
if (!data) return <div>No todos!</div>;


return todos.map((todo, i) => (
<Todo key={i} todo={todo} />
))
}
```


` GetTodoById`


If the entity returned from the response has never been seen before, the cache will normalize it and store it as a flattened object on the cache.


```text
import React from 'react';
import { useParams } from 'react-router-dom';
import { useQuery } from '@apollo/client';
import Todo from '../components/Todo'


const GET_TODO_BY_ID = gql`
query GetTodoById($id: Int!) {
todo (id: $id) {
... on Todo {
id
text
completed
}
... on TodoNotFoundError {
message
}
}
}
`


export function TodoDetails() {
let { id } = useParams();


const { loading, data, error } = useQuery(GET_TODO_BY_ID, {
variables: { id: Number(id) }
})


if (loading) return <div>Loading...</div>
if (error) return <div>{error}</div>


return data?.todo.__typename === "Todo" ? (
<ul className="todo-list">
<Todo todo={data?.todo} />
</ul>
) : (
<div>Todo not found</div>
)
}
```


### Mutations that update a single existing entity


These types of operations *update* a **single entity** in question. No matter what the operation is, as long as we return a new object containing the` id` and the changed fields, Apollo Client can automatically update the item in the cache and trigger a re-render to the UI.


**Examples**


` EditTodo`


```text
import React from 'react';
import { gql, useMutation } from "@apollo/client";
import Todo from '../components/Todo'


const EDIT_TODO = gql`
mutation EditTodo ($id: Int!, $text: String!) {
editTodo (id: $id, text: $text) {
success
todo {
id
text
completed
}
error {
... on TodoNotFoundError {
message
}
... on TodoValidationError {
message
}
}
}
}
`


export const TodosContainer = () => {
const todos = getTodos();
const [mutate, { data, error }] = useMutation(
EDIT_TODO
)


...


return todos.map((todo, i) => (
<Todo
key={i}
actions={{
editTodo: (id, text) => mutate ({
variables: { id, text }
})
}}
/>
))
}
```


` CompleteTodo`


```text
import React from 'react';
import { gql, useMutation } from "@apollo/client";
import Todo from '../components/Todo'


const COMPLETE_TODO = gql`
mutation CompleteTodo ($id: Int!) {
completeTodo (id: $id) {
success
todo {
id
text
completed
}
error {
... on TodoNotFoundError {
message
}
... on TodoAlreadyCompletedError {
message
}
}
}
}
`


export const TodosContainer = () => {
const todos = getTodos();
const [mutate, { data, error }] = useMutation(
COMPLETE_TODO
)


...


return todos.map((todo, i) => (
<Todo
key={i}
actions={{
completeTodo: (id) => mutate({
variables: { id }
})
}}
/>
))
}
```


### Bulk update mutations that return the entire set of changed items


If we were to perform a bulk update against a set of items and in the mutation response, we returned the **entire set of objects that changed** and their **new values** , then the cache **can** update automatically.


We have to really think back to the **normalization algorithm** .


1. New data comes in
2. The cache checks to see if it has seen it before.
3. If yes, it merges to the already normalized items preferring the new data.
4. If not, it splits up the items, assigns unique identifiers, and caches ’em for the first time.


**Examples**


` CompleteAllTodos`


In essence, it doesn’t matter if we perform a query or a mutation — if we return a dataset of items in a response, the cache will run the normalization logic against it. This results in either a *merge* or an addition of a new item to the cache.


```text
import { gql, useMutation } from "@apollo/client";
import * as CompleteAllTodosTypes from './__generated__/CompleteAllTodos'


export const COMPLETE_ALL_TODOS = gql`
mutation CompleteAllTodos {
completeAllTodos {
success
todos {
id
text
completed
}
}
}
`


export default function TodoList () {
const { loading, data, error } = useQuery(
GET_ALL_TODOS
);


const [mutate] = useMutation<
CompleteAllTodosTypes.CompleteAllTodos
>(
COMPLETE_ALL_TODOS
)


if (loading) return <div>Loading...</div>
if (error) return <div>An error occurred {JSON.stringify(error)}</div>
if (!data) return <div>No todos!</div>;


return <Layout>
<button onClick={() => mutate()}>Complete all todos</button>
{todos.map((todo, i) => (
<Todo key={i} todo={todo} />
))}
</Layout>
}
```


` EditTodosBulk`


Same concept as` CompleteAllTodos` .


## Operations the cache cannot automatically update


*Application-specific side-effects* and *update* *operations that add* , *remove, or reorder* items in a cached collection.


When building out a` mutation` , if any one of these is true,


- ***if the side-effect we want to occur has nothing to do with the return data***
- ***we*** * ***can’t return the entire set of objects changed***
- ***the mutation*** ***changes the ordering of a cached collection***
- ***the mutation*** ***adds or removes items***


… then we need to write an` update` function to tell the cache exactly how to update.


### Application-specific side-effects


Application-specific side-effects are things that you want to happen to the cache after a` mutation` that may not use anything from the response data.


**Examples**


` Logout`


Perhaps after you invoked a` logout`` mutation` , you wanted to clear the entire cache of a user’s information so that a new user could start a session.


The response data for a` logout`` mutation` might look like this:


```text
{
logout: {
success: true,
message: "User successfully logged out"
}
}
```


That’s great — but you know, there’s nothing here we actually need to cache.


Instead, we might want to erase the entire cache. You can do that with the` client.clearStore()` method in the` update` function.


```text
import { gql, useMutation } from "@apollo/client"
import { client } from "./client"


const LOGOUT = gql`
mutation Logout {
logout {
success
message
}
}
`


const Navbar = () => {
const [logout] = useMutation(LOGOUT, {
update () {
client.clearStore()
}
});


return <div onClick={() => logout()}></div>
}
```


` Updates to local state variables`


In Apollo Client 3, we use Reactive Variables and[Cache Policies](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-field-behavior/) to setup local state. It’s possible that after performing an operation, we need to update some piece of local state.


Reactive variables (or functions with[interaction logic](https://khalilstemmler.com/articles/client-side-architecture/introduction/#Interaction-layer) that operate *against* reactive variables) can be imported directly in the` update` function of a` mutation` .


Read[Local State Management with Reactive Variables](https://www.apollographql.com/blog/local-state-management-with-reactive-variables) to learn more about local state management in AC3.


### Bulk updates that *do not* return the entire set of changed items


Updates work **only** if you return the entire set of objects that were changed. Taking the same examples from the previous section, here’s how we can update the cache if we can’t return the entire set of items that changed.


**Examples**


` CompleteAllTodos`


```text
import { gql, useMutation } from "@apollo/client";
import * as CompleteAllTodosTypes from './__generated__/CompleteAllTodos'


export const COMPLETE_ALL_TODOS = gql`
mutation CompleteAllTodos {
completeAllTodos {
success
todos {
id
# Does not return all the data
}
}
}
`


export default function TodoList () {
const { loading, data, error } = useQuery(
GET_ALL_TODOS
);


const [mutate] = useMutation<
CompleteAllTodosTypes.CompleteAllTodos
>(
COMPLETE_ALL_TODOS,
{
update (cache, { data }) {
const completedTodos = data?.completeAllTodos.todos;
const allTodos = cache.readQuery<GetAllTodos>({
query: GET_ALL_TODOS
});


cache.writeQuery({
query: GET_ALL_TODOS,
data: {
todos: allTodos.map((t) => !!completedTodos
.find((completed) => completed.id === t.id)
}
})
}
}
)


if (loading) return <div>Loading...</div>
if (error) return <div>An error occurred {JSON.stringify(error)}</div>
if (!data) return <div>No todos!</div>;


return <Layout>
<button onClick={() => mutate()}>Complete all todos</button>
{todos.map((todo, i) => (
<Todo key={i} todo={todo} />
))}
</Layout>
}
```


` EditTodosBulk` (doesn’t return all changed items)


Same concept as` CompleteAllTodos` .


### **Additions**


The cache doesn’t know when it should add newly created entities to existing queries for data. In these cases, we have to write an update function.


**Examples**


` AddTodo`


```text
const [mutate, { data, error }] = useMutation<
AddTodoTypes.AddTodo,
AddTodoTypes.AddTodoVariables
>(
ADD_TODO,
{
update (cache, { data }) {
const newTodoFromResponse = data?.addTodo.todo;
const existingTodos = cache.readQuery<GetAllTodos>({
query: GET_ALL_TODOS,
});


if (existingTodos && newTodoFromResponse) {
cache.writeQuery({
query: GET_ALL_TODOS,
data: {
todos: [
...existingTodos?.todos,
newTodoFromResponse,
],
},
});
}
}
}
)
```


### **Deletions**


Similarly, the cache has no idea when we might want to remove items from an existing query. To handle these scenarios, we have to update the cache value manually by filtering out items in an update function.


**Examples**


` DeleteTodo`


```text
const [mutate, { data, error }] = useMutation<
DeleteTodoTypes.DeleteTodo,
DeleteTodoTypes.DeleteTodoVariables
>(
DELETE_TODO,
{
update (cache, el) {
const deletedId = el.data?.deleteTodo.todo?.id
const allTodos = cache.readQuery<GetAllTodos>({ query: GET_ALL_TODOS });


cache.writeQuery({
query: GET_ALL_TODOS,
data: {
todos: allTodos?.todos.filter((t) => t?.id !== deletedId)
}
});


cache.evict({ id: el.data?.deleteTodo.todo?.id })
}
}
)
```


## Summary


We’ve covered the basics of Apollo Client’s cache normalization.


We learned how Apollo Client’s cache normalizes objects and stores them both **flattened on the cache** in a **list that maintains the order, and points to each of the flattened objects by id.**


We also learned that the cache is smart enough to update *single existing objects on the cache only if we return the new value in the mutation response.*


Lastly, we learned that the cache **doesn’t make assumptions about how you would like your collections/arrays of items to change** after a mutation. In these cases, we need to decide what the appropriate thing to do is, and we can implement it in the update function of a mutation with either cache.readQuery/writeQuery.


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
