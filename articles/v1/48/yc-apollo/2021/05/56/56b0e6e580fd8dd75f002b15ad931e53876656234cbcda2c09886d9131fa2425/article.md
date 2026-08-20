---
schema_version: "1.0.0"
document_id: "56b0e6e580fd8dd75f002b15ad931e53876656234cbcda2c09886d9131fa2425"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/using-graphql-fragments-for-safer-cleaner-and-faster-code"
published_at: "2021-05-31T10:30:01+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:0c6b855b372d46fb896c26ac528078fa637eccfe531fb4ccf20f8b41c3e25050"
---

# Using GraphQL Fragments for safer, cleaner, and faster code

Whether you’re just getting started with building GraphQL applications or you’ve done it for years, you probably have a decent understanding of[what GraphQL queries are](https://www.apollographql.com/blog/what-is-a-graphql-query-graphql-query-examples-using-apollo-explorer/) . But what about GraphQL fragments?


In this article, we’ll delve into some common use-cases where fragments shine, learn how to get the most out of them, and hopefully, I’ll inspire you to use more fragments in your GraphQL queries.


## Use case: modeling a list


When you build a GraphQL-based application, the typical choice pattern is to start with building components — brilliantly simple UI components — that declare their data needs of which you can fetch using GraphQL queries. After a few data loading wins, you’re ready for something a little bit more complex — the infamous *List* component.


```text
export function SimpleGroceryList() {
const { data } = useQuery(gql`
query SimpleGroceryListQuery {
groceries {
id
name
}
}
`);


return (
<ul>
{data.groceries.map((grocery) => (
<li key={grocery.id}>
{grocery.name}
</li>
))}
</ul>
);
}
```


As shown above, we can write a single query that fetches the entire list, and for each item, we get back the id and name. Now, sometimes this is about all you need. Other times, though, rendering a list item is not so simple, and before long, we’re asking ourselves an age-old question: *should the list item be a separate componen* t?


### Refactoring to list items


We need to be aware of a tipping point here. When the complexity of the markup begins to hurt your head, keeping a stronger separation of concerns can be hugely beneficial (this doesn’t just apply to lists, but also any parent/child relationship in your render tree). The example we present here as a demonstration is relatively light, but we’ve seen some pretty gnarly scenarios. Let’s refactor this into two components.


```text
// item
export function GroceryListItem({ grocery }) {
return (
<li>
{grocery.name} x {grocery.quantity}
</li>
);
}


// list
export function GroceryList() {
const { data } = useQuery(gql`
query GroceryListQuery {
groceries {
id
name
quantity
}
}
`);


return (
<ul>
{data.groceries.map((grocery) => (
<GroceryListItem key={grocery.id} grocery={grocery} />
))}
</ul>
);
}
```


### Refactoring to separate files


We might tolerate these two components sharing the same file at first, but as complexity scales up, it often becomes harder to maintain without refactoring GroceryListItem into its own file.


We now have GroceryListItem.


```text
// GroceryListItem.jsx
export function GroceryListItem({ grocery }) {
return (
<li>
{grocery.name} x {grocery.quantity}
</li>
);
}
```


And we have the cleaner GroceryList component as well.


```text
// GroceryList.jsx
export function GroceryList() {
const { data } = useQuery(gql`
query GroceryListQuery {
groceries {
id
name
quantity
}
}
`);


return (
<ul>
{data.groceries.map((grocery) => (
<GroceryListItem key={grocery.id} grocery={grocery} />
))}
</ul>
);
}
```


While this separation of the parent and child component reduces clutter, it comes with two slight disadvantages we need to address.


### Disadvantage #1 — Undiscoverable data dependencies


Once we split the files apart, we lose the ability to scan the GraphQL query and compare it with the usage of the data in our UI code. So what can we do?


In the early days of React, to clarify the data dependencies of GroceryListItem, we might try addressing this with “PropTypes”.


```text
// GroceryListItem.jsx
export function GroceryListItem({ grocery }) {
return (
<li>
{grocery.name} x {grocery.quantity}
</li>
);
}


// re-describe our component's data dependencies
GroceryItemList.propTypes = {
grocery: PropTypes.shape({
name: PropTypes.string.isRequired,
quantity: PropTypes.number.isRequired,
}).isRequired,
};
```


PropTypes work, but it also exposes another disadvantage: duplicating data dependencies.


### Disadvantage #2 — Duplicating data dependencies


Regardless of if you’re using PropTypes or TypeScript to define component props, we still have a lot of typing to do. We’re duplicating the shape of our component’s data in two places: first in the GraphQL query and again in the component’s property list, which indeed makes fingers tired.


## Fragments to the rescue


At their core,[GraphQL fragments](https://www.apollographql.com/docs/react/data/fragments/) are just small pieces (or “fragments”) of a larger GraphQL query. But how can they help us in this situation?


### Express child component data dependencies


With fragments, we can bring back the benefits of co-locating our data dependencies with our UI code, but in a more distributed way across the application. In the following code, we define a fragment with only the data that we need and export it for inclusion in the parent query.


```text
// GroceryItemList.tsx


export const groceryListItemFragment = gql`
fragment GroceryListItemFragment on Grocery {
name
quantity
}
`;


/**
* Automatically generated fragment type (you don't have to type this!)
*
export interface GroceryListItemFragment {
name: string;
quantity: number;
}
*/


interface Props {
grocery: GroceryListItemGroceryFragment;
}


export function GroceryListItem({ grocery }: Props) {
return (
<li>
{grocery.name} x {grocery.quantity}
</li>
);
}
```


### Less typing, no duplication


Not only do fragments act as a way to express data dependencies, but[using Apollo codegen](https://www.apollographql.com/blog/tooling/apollo-codegen/typescript-graphql-code-generator-generate-graphql-types/) , we can type the data dependencies once and auto-generate Typescript types to ensure we pass the exact shape of data we need.


### Detect breaking API changes


We automatically generate typings using the schema from our GraphQL API.


If we take advantage of a schema repository like the one provided through Apollo Studio, we can generate your types for our queries against whichever graph variant you need.


The auto-generated typings are useful because we can run \`apollo codegen:generate\` in the CI to ensure that all of the queries and fragments we’re requesting are available in the API when the code goes live.


To get started with[schema checks](https://www.apollographql.com/docs/studio/schema-checks/) and several other free GraphQL tools to help you build better, safer, and faster, register your graph @[https://studio.apollographql.com/dev](https://studio.apollographql.com/dev) .


### Using fragments in parent components


Going back to our example, in the parent list component, we import the fragment into our query and fetch the data for each grocery item as before.


Notice that we also request the id field in addition to GroceryListItemFragment , clarifying that GroceryList has a dependency on id but no other fields.


```text
// GroceryList.tsx


import { groceryListItemFragment, GroceryListItem } from './GroceryListItem';


export function GroceryList() {
const { data } = useQuery(gql`
query GroceryListQuery {
groceries {
id
...GroceryListItemFragment // fragment is "spread" here
}
}
${groceryListItemFragment} // also include the fragment definition in the query
`);


return (
<ul>
{data.groceries.map((grocery) => (
<GroceryListItem key={grocery.id} grocery={grocery} />
))}
</ul>
);
}
```


Nice! What happens if we decide to add another piece of information to the list item? We modify our fragment definition, re-run code generation, and voila — and we have updated our strong types. We’ve also maintained the separation of concerns between fetching the list and describing how we render an item.


## Conclusion


When we started building[Apollo Studio’s](https://studio.apollographql.com/dev) frontend application, it was way back in the early days of React 14. We were making heavy use of GraphQL but still figuring out best practices. As our codebase began to expand in scope and complexity, we started to develop more scalable patterns and developed many new strategies for weaving together GraphQL, Typescript, and UI functionality. Some of the big lessons were the many benefits that fragments provide:


- Declaring fragments alongside UI code allows you to separate concerns between parent components and children.
- Defining and exporting fragments makes components more re-usable since data dependencies are expressively declared, and parents can compose as needed.
- Using fragments allows you to make a single query to fetch data for multiple components at once, reducing the need for repeated round-trips to the server.
- Automatic type generation FTW. Strict build-time checking prevents bugs before they happen; codegen eliminates duplication of code to describe data shapes across the component boundary.


Hopefully, this helps solve some problems that you might have in your codebase. Thanks for reading!


Written by


Tim Hingston


[Read more by Tim Hingston](https://www.apollographql.com/blog/author/timbotnik)
