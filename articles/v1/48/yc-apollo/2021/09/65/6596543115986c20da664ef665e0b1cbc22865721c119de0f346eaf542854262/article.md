---
schema_version: "1.0.0"
document_id: "6596543115986c20da664ef665e0b1cbc22865721c119de0f346eaf542854262"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-filter-and-search-using-variables-in-apollo-client"
published_at: "2021-09-29T09:01:37+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:2ef822f129bf9bc7815b8ac2a7e447a188bc74dc966fc0a1bcf6934fe952336b"
---

# How to Filter and Search using Variables in Apollo Client

You know how to query data from Apollo Client using the` useQuery` hook, but what about searching and filtering it? How does that work?


In this post, we’ll walk through a tiny React and Apollo Client example demonstrating how to set up queries that let you search and filter using variables.


Just want the code?: You can find the code for this blog post @[stemmlerjs/apollo-examples](https://github.com/stemmlerjs/apollo-examples/tree/main/basic/apollo-server-filter-example) .


## Prerequisites


- [You know the basics of GraphQL](https://www.apollographql.com/blog/graphql/basics/what-is-graphql-introduction/)
- [You know how to setup and query a GraphQL API using Apollo Client](https://www.apollographql.com/docs/react/get-started/)


## Example: Querying and presenting a list of albums


In this example, we’ll use an API that returns a list of albums. The idea is that we’ll start out with a page that presents an entire list of albums.


When we type in the form and search for an album, the API should return only the albums that match the search.


Let’s get into it.


### GraphQL Schema


On the GraphQL API, assume that our schema looks like the following:


```text
type Album {
id: ID
name: String
}


input AlbumsInputFilter {
id: ID
name: String
}


type Query {
albums(input: AlbumsInputFilter): [Album]
}
```


You’ll notice that on the root` Query` object, we have a query called` albums` which takes in an optional` AlbumsInputFilter` input type and returns a list of albums. Not a whole lot going on here.


- **Input types:** If you’re unfamiliar with input types, I recommend[reading the docs on them](https://www.apollographql.com/docs/apollo-server/schema/schema/#input-types)
- **Implementing searching & filtering in a GraphQL API** : We’re not going to go into detail as to how to implement searching and filtering on the backend in this post. If you’d like to learn how this works, you can read “[How to Search and Filter results in GraphQL API](https://www.apollographql.com/blog/graphql/filtering/how-to-search-and-filter-results-with-graphql/) “.


### Testing the API with Apollo Studio Explorer


If you’re using Apollo Server, you can navigate to the localhost URL where your graph is running and test out the API using Apollo Studio Explorer.


Giving this a quick little test query, all looks well — let’s move on to hooking this up to an Apollo Client + React frontend.


## Frontend


After[initializing Apollo Client](https://www.apollographql.com/docs/react/get-started/) as we normally would in a new React app, to keep things simple, we’ll go ahead and write all of our code in the` App.js` file.


Here’s where we might start out.


```text
import React from 'react'
import "./App.css";


function App() {
return (
<div className="App">
<h1>Albums</h1>


<div>
<label>Search</label>
<input
onChange={(e) => { /* Handle updating search/filter text */ }}
type="string"
/>
</div>


<br/>


{[].map((album) => (
<div>{JSON.stringify(album)}</div>
))}


<br/>


<button
onClick={() => { /* Handle search/filter */ }}
>
Submit!
</button>
</div>
);
}


export default App;
```


### The initial query


Next, let’s write the query to make the initial fetch to our GraphQL API to retrieve the list of albums. We do this by:


- Writing a GraphQL query
- Utilizing the[useQuery hook](https://www.apollographql.com/docs/react/data/queries/) to fetch the data when the component loads
- Handling the[loading](https://www.apollographql.com/docs/react/data/queries/#inspecting-loading-states) and[error](https://www.apollographql.com/docs/react/data/queries/#inspecting-error-states) states
- Rendering the resulting data if the query is done loading and there aren’t any errors.


Here’s what the code looks like:


```text
import React from 'react'
import "./App.css";
import { useQuery, gql } from "@apollo/client";
import { useState } from "react";


const GET_ALBUMS = gql`
query Albums {
albums {
id
name
}
}
`;


function App() {
const { data, loading, error } = useQuery(GET_ALBUMS);


if (loading) return <div>Loading</div>;
if (error) return <div>error</div>;


return (
<div className="App">
<h1>Albums</h1>


<div>
<label>Search</label>
<input
onChange={(e) => { /* Handle updating search/filter text */ }}
type="string"
/>
</div>


<br/>


{data.albums.map((album) => (
<div>{JSON.stringify(album)}</div>
))}


<br/>


<button
onClick={() => { /* Handle search/filter */ }}
>
Submit!
</button>
</div>
);
}


export default App;
```


We should now see a list of albums returned from the API.


### Setting up search state


Now for the fun part. We need to hook up the searching and filtering state.


This means we’ll need to store the current value of the input field as local state. Why? Because when we click “Submit”, we’ll need access to the current value of what was typed in. We will then use that current value as a *GraphQL variable* in our query.


**GraphQL variables?:** To better understand GraphQL variables and how they work, I recommend reading “[The Anatomy of a GraphQL Query](https://www.apollographql.com/blog/graphql/basics/the-anatomy-of-a-graphql-query/) “.


To keep track of the input state, we can use a trivial React hook that exposes the` filters` local state and a single operation for updating the filters called` updateFilter` .


```text
import React from 'react'
import "./App.css";
import { useQuery, gql } from "@apollo/client";
import { useState } from "react";


...


function useAlbumFilters() {
const [filters, _updateFilter] = useState({
id: undefined,
name: undefined
});


const updateFilter = (filterType, value) => {
_updateFilter({
[filterType]: value,
});
};


return {
models: { filters },
operations: { updateFilter },
};
}
```


Let’s hook this up to our React component’s input field.


```text
...


function App() {
const { operations, models } = useAlbumFilters();
const { data, loading, error, refetch } = useQuery(GET_ALBUMS);


if (loading) return <div>Loading</div>;
if (error) return <div>error</div>;


return (
<div className="App">
<h1>Albums</h1>


<div>
<label>Search</label>
<input
onChange={(e) => operations.updateFilter("name", e.target.value)}
type="string"
/>
</div>


...
</div>
);
}
```


Great. Now, when we type, the value from the input field is stored as state. The last thing to do is to hook the search value up to the query as a GraphQL variable.


### Refetching and filtering


This last step is broken into two parts.


First, we update the query to use a query variable called` albumsInput` to refer to the input type. It gets passed in to the` albums` type as an argument.


```text
const GET_ALBUMS = gql`
query Albums($albumsInput: AlbumsInputFilter) {
albums(input: $albumsInput) {
id
name
}
}
`;
```


In reality, you could name this variable *anything you want* . For example,` someInput` would also work. It doesn’t matter what you name it. What *does* matter is that the input type matches the type used in the schema.


If you’ll recall from the schema, the parameter used in the type signature for the` albums` query is` AlbumsInputFilter` . Because GraphQL is strictly typed, we have to ensure we explicitly use the` AlbumsInputFilter` type for the variable.


```text
type Query {
albums(input: AlbumsInputFilter): [Album] # Variables are strictly typed
}
```


We then deconstruct the` refetch` function from the` useQuery` hook; and on the onClick callback, we invoke that` refetch` function with the query variable.


```text


...


function App() {
const { operations, models } = useAlbumFilters();
const { data, loading, error, refetch } = useQuery(GET_ALBUMS);


if (loading) return <div>Loading</div>;
if (error) return <div>error</div>;


return (
<div className="App">
<h1>Albums</h1>


...


<button
onClick={() =>
refetch({
albumsInput: { name: models.filters.name },
})
}
>
Submit!
</button>
</div>
);
}


export default App;
```


Take special note of the structure of the object passed to the` refetch` function. In the GraphQL query, we write the variable as $albumInput but when we call` refetch` , we pass in an object with the key albumsInput on the object. This is because` refetch` ‘s function expects an object containing any variables included in the query as keys.


You can read more about “[Providing new variables to refetch](https://www.apollographql.com/docs/react/data/queries/#providing-new-variables-to-refetch) ” in the docs.


### Solution


And that’s it — here’s what the complete code looks like:


```text
import React from 'react'
import "./App.css";
import { useQuery, gql } from "@apollo/client";
import { useState } from "react";


const GET_ALBUMS = gql`
query Albums($albumsInput: AlbumsInputFilter) {
albums(input: $albumsInput) {
id
name
}
}
`;


function useAlbumFilters() {
const [filters, _updateFilter] = useState({
id: undefined,
name: undefined
});


const updateFilter = (filterType, value) => {
_updateFilter({
[filterType]: value,
});
};


return {
models: { filters },
operations: { updateFilter },
};
}


function App() {
const { operations, models } = useAlbumFilters();


const { data, loading, error, refetch } = useQuery(GET_ALBUMS);


if (loading) return <div>Loading</div>;
if (error) return <div>error</div>;


return (
<div className="App">
<h1>Albums</h1>


<div>
<label>Search</label>
<input
onChange={(e) => operations.updateFilter("name", e.target.value)}
type="string"
/>
</div>


<br/>


{data.albums.map((album) => (
<div>{JSON.stringify(album)}</div>
))}


<br/>


<button
onClick={() =>
refetch({
albumsInput: { name: models.filters.name },
})
}
>
Submit!
</button>
</div>
);
}


export default App;
```


## Conclusion


In this post, we learned how to set up filter and search functionality in Apollo Client.


### What’s next?


Now, I’m a believer in “if we do it, we know it”. If you’re just getting started with GraphQL, I highly recommend you check out our *completely* *free* “Lift Off” GraphQL course.


In less than 30 minutes, you’ll learn the core components of a full-stack GraphQL app with Apollo through an interactive tutorial.


You can[get started here](https://odyssey.apollographql.com/?referrer=blog) . Enjoy!


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
