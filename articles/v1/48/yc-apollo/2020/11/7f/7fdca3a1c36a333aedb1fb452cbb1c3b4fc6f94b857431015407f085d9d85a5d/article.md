---
schema_version: "1.0.0"
document_id: "7fdca3a1c36a333aedb1fb452cbb1c3b4fc6f94b857431015407f085d9d85a5d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/when-to-use-refetch-queries"
published_at: "2020-11-16T14:57:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:66702200fab71e7b5604d2e2cb0487f10a4d01327b39597d5abf136181b5b824"
---

# When To Use Refetch Queries in Apollo Client

One of the most common use cases front-end developers face is re-render the UI after executing a` mutation` and changing something in the backend.


To solve this problem, a lot of developers like to use the` refetchQueries` API.


For example, if I wanted to **add** a` todo` to a list of` todos` , I might pass a` GET_ALL_TODOS` query to my list of queries to refetch, like so:


```text
import { gql, useMutation } from "@apollo/client";
import * as AddTodoTypes from './__generated__/AddTodo';
import { GET_ALL_TODOS } from "../queries/getAllTodos";


export const ADD_TODO = gql`
mutation AddTodo ($text: String!) {
addTodo (text: $text) {
success
error {
message
}
}
}
`


export function useAddTodo () {
const [mutate, { data, error }] = useMutation<
AddTodoTypes.AddTodo,
AddTodoTypes.AddTodoVariables
>(
ADD_TODO,
{
refetchQueries: [
{ query: GET_ALL_TODOS }
]
}
)
return { mutate, data, error };
}
﻿
```


While this approach works, the biggest disadvantage is that we have to make multiple network round-trips to get our UI to re-render, even if we already have the data we need on the client-side.


It also introduces the question of “what do we do if we have a lot of queries relying on the same data? Do we have to remember to add all of *those* to the same` refetchQueries` list as well?”


Luckily, you don’t.


In this post, I’ll explain when it makes sense to use` refetchQueries` , and when you should rely on Apollo Client’s automatic cache normalization algorithm or` update` functions.


The following example uses the Apollo Client 3 Todos app example from[@apollographql/ac3-state-management-examples](https://github.com/apollographql/ac3-state-management-examples) .


## Use refetchQueries when you’re first getting started


If you’re just getting started with GraphQL, I think the mental model of passing in the queries that you’d like to re-run after a mutation is an easy one to wrap your head around.


Looking back at our trivial` todo` app, if we have a list of` todos` and we want to **add** an item, we can accomplish that and re-render the UI with the` ADD_TODO` operation passed into a` useMutation` hook and the` GET_ALL_TODOS` query passed into the` refetchQueries` array.


```text
const [mutate, { data, error }] = useMutation<
AddTodoTypes.AddTodo,
AddTodoTypes.AddTodoVariables
>(
// Execute this
ADD_TODO,
{
// Then re-run
refetchQueries: [
{ query: GET_ALL_TODOS }
]
}
)
return { mutate, data, error };
﻿
```


This works. It runs the query and after successfully completing the mutation, it executes the` GET_ALL_TODOS` query and pulls in all the new` todos` .


The advantage here is that this approach is straightforward. The disadvantage is that we’re fetching the entire list of data again when we might not need to.


## Cache Normalization and Update Functions


For a more efficient use of bandwidth and network round-trips, we can rely on cache normalization and` update` functions.


As we previously learned in the “[Demystifying Cache Normalization](https://www.apollographql.com/blog/demystifying-cache-normalization/) ” blog post, for certain operations, the Apollo Client cache is smart enough to **automatically update the cache for you** . For others, we can utilize an` update` function to tell the cache how we want it to update.


### Automatically Updating with Cache Normalization


For mutations that **change only a single entity,** the cache is smart enough to update the item in place automatically, but only if we return the changed item (containing the unique identifier) in the mutation response.


Here’s an example of **editing a**` todo` .


```text
import { gql, useMutation } from "@apollo/client";
import * as EditTodoTypes from './__generated__/EditTodo'


export const EDIT_TODO = gql`
mutation EditTodo ($id: Int!, $text: String!) {
editTodo (id: $id, text: $text) {
success
todo { # Return the new todo in the mutation response
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


export function useEditTodo () {
const [mutate, { data, error }] = useMutation<
EditTodoTypes.EditTodo,
EditTodoTypes.EditTodoVariables
>(
EDIT_TODO
)


return { mutate, data, error };
}
﻿
```


And since we’ve returned the **changed data** in the` mutation` response, it works.


But for other operations, like *adding* to a list or *removing* elements from a list, we need to give the cache a little bit of help to figure out exactly what we want to do.


### Using Update Functions


Here’s the` ADD_TODO` example again, but this time using an` update` function to update the existing list of` todos` .


```text
import { gql, useMutation } from "@apollo/client";
import * as AddTodoTypes from './__generated__/AddTodo';
import { GET_ALL_TODOS } from "../queries/getAllTodos";
import { GetAllTodos } from "../queries/__generated__/GetAllTodos";


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


export function useAddTodo () {
const [mutate, { data, error }] = useMutation<
AddTodoTypes.AddTodo,
AddTodoTypes.AddTodoVariables
>(
ADD_TODO,
{
update (cache, { data }) {
// We use an update function here to write the
// new value of the GET_ALL_TODOS query.
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
return { mutate, data, error };
}
﻿
```


In this example, we use the` readQuery` and` writeQuery` APIs to write the new value of the` todos` list which contains the` todo` we created in the` mutation` .


One way we could omit needing to write this` update` function is if the mutation response returned the **entire list of new todos** in the` mutation` response, but that’s not always possible.


Sometimes you don’t own the API.


Sometimes, it can get expensive to be doing that after every` mutation` .


So here, we utilize the` update` function.


Admittedly, this is a little bit more work than using` refetchQueries` , but that’s the trade off for a performance improvement. You don’t have to re-request the same data from your server and wait for another entire roundtrip to update the UI.


## Conclusion


In summary best times to use` refetchQueries` are:


1. When you don’t yet know how ACC normalization works
2. If you’re not able to return the changed data in a mutation response


To reduce round-trips after mutations, we generally recommend relying on the Apollo Client automatic caching normalization and when necessary,` update` functions.


## Resources


- “[Demystifying Cache Normalization](https://www.apollographql.com/blog/demystifying-cache-normalization/) ” from the Apollo Blog
- “[Updating after a Mutation](https://www.apollographql.com/docs/react/caching/advanced-topics/#updating-after-a-mutation) ” from the Apollo Docs


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
