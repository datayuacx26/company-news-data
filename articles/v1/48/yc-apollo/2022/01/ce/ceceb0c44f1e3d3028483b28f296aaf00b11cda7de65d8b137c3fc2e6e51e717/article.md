---
schema_version: "1.0.0"
document_id: "ceceb0c44f1e3d3028483b28f296aaf00b11cda7de65d8b137c3fc2e6e51e717"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/getting-started-with-vue-apollo"
published_at: "2022-01-11T15:30:07+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:2bb568e46ed4baf0f6e37f9fc223c03faa15aebd16fcd2dd40ec63832e1c6555"
---

# Getting Started with Vue Apollo

## Introduction


Vue is a modern JavaScript framework for building single-page applications. Apollo Client is a fully-fledged GraphQL client and state management library. Using Vue Apollo, we can combine them to substantially improve the developer experience involved in building complex UIs.


In this article, we’ll learn how to get started building with Vue, GraphQL, and Apollo Client using the latest versions of Apollo Client and Vue Apollo.


## Prerequisites


Before we get started, this article assumes:


- [You know what GraphQL is](https://www.apollographql.com/blog/graphql/basics/what-is-graphql-introduction)
- You’re familiar with[Apollo Client](https://www.apollographql.com/docs/react/) and know how[queries](https://www.apollographql.com/docs/react/data/queries/) and[mutations](https://www.apollographql.com/docs/react/data/mutations/) work
- You’re familiar with[Vue](https://vuejs.org/v2/guide/) or some other comparable front-end library or framework for building UIs (like React or Angular)
- You have **[Node.js](https://nodejs.org/)** version 8.9 or above (v10+ recommended for Vue CLI 4.x)


## Options vs. Composition API


We should start by first discussing In 2021, there are two ways to use Vue Apollo. We can use either the:


- [Option (classic) API](https://v4.apollo.vuejs.org/guide-option/setup.html) or
- the[Composition (Advanced) API](https://v4.apollo.vuejs.org/guide-composable/setup.html)


The Options API is based on the original` apollo` Vue component definition. It used the standard syntax for Vue 2, object structure to define queries, and requires additional logic for error state. In practice, it looks something like this:


```text
<script>
export default {
apollo: {
// Queries here
}
}
</script>
```


The newer Composition API, however, provides better abstractions for code reuse, scales better than the classic Options API, and will feel familiar to developers coming from a background using React hooks or Apollo’s` useQuery` and` useMutation` API.


**Additional reading:** You can[read more about the Composition API here](https://v3.vuejs.org/guide/composition-api-introduction.html#why-composition-api) .


Since the maintainers plan to prioritize the Composition API in the future, we’re going to tailor this tutorial towards that one.


## Setup


### Installing the Vue CLI


To begin, let’s install the[Vue CLI](https://cli.vuejs.org/) . It’s a command-line tool that gives you loads of customizability for starting a new project.


```text
npm install -g @vue/cli
```


### Create a new Vue app


Next, we’ll use the Vue CLI to create a new project. Let’s call it` app` .


```text
vue create app
```


Once you enter the command, you’ll be presented with a number of different configuration options for your project.


For this tutorial, we just selected the second option to use Vue 3. However, if you’d like to use TypeScript, add or remove linting, add unit and E2E testing features, you can select *Manually select features.* Make sure to use Vue version 3.


This should take a moment to install.


### Installing the base dependencies


Once you’ve set up your project, we’ll want to move into the project folder and` npm` install some base dependencies.


```text
cd app
npm install --save graphql graphql-tag @apollo/client
```


### Installing the Composition API


Here’s the point where if we wanted to use the classic Options API, we could go down a different configuration road. But since we’re going to use the Composable API, we’ll` npm` install that now with the following command.


```text
npm install --save @vue/apollo-composable
```


### Starting the app


To start the app, run` npm run server` and then go to` localhost:8080` .


```text
npm run serve
```


If all went correctly, you should see the Vue getting started page.


Now let’s configure Vue Apollo so that we can fetch GraphQL data.


### Initial Vue Apollo config


For this tutorial, we’re going to use the Rick & Morty API to demonstrate the basics. Inside of the` main.js` file, copy and paste the following setup code to configure Vue Apollo and Apollo client in your Vue app.


```text
// main.js


import { createApp, provide, h } from 'vue'
import { DefaultApolloClient } from '@vue/apollo-composable'
import { ApolloClient, InMemoryCache } from '@apollo/client/core'
import App from './App.vue'


const cache = new InMemoryCache()


const apolloClient = new ApolloClient({
cache,
uri: '<https://rickandmortyapi.com/graphql>',
})


const app = createApp({
setup () {
provide(DefaultApolloClient, apolloClient)
},


render: () => h(App),
})


app.mount('#app');
﻿
```


Next, let’s head over the main` App.vue` so that we can query some data.


## Querying data


In the` script` section of your` App.vue` Vue single-file component, we’re going to start by writing a query to fetch a list of characters from the Rick & Morty API.


```text
// <script>


import gql from 'graphql-tag'


const CHARACTERS_QUERY = gql`
query Characters {
characters {
results {
id
name
image
}
}
}
`
...


// </script>
﻿
```


Next, we’re going to import` useQuery` from` @vue/apollo-composable` and pass our query to it within a` setup` function on the object we export.


Make sure to decompose the return value of` useQuery` into` result` ,` loading` and` error` values so that we can utilize them within our template.


```text
// <script>
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable'


...


export default {
name: 'App',
setup () {
const { result, loading, error } = useQuery(CHARACTERS_QUERY);
return {
result,
loading,
error
}
}
}
// </script>
```


Finally, at the top of the file within our Vue template, we can perform[conditional rendering](https://vuejs.org/v2/guide/conditional.html) using the` v-if` directive.


```text
<template>
<p v-if="error">Something went wrong...</p>
<p v-if="loading">Loading...</p>
<p v-else v-for="character in result.characters.results" :key="character.id">
{{ character.name }}
</p>
<div></div>
</template>
```


If the value of` error` is defined, we’ll print out “Something went wrong…”


If the value of` loading` is true, we’ll be sure to print “Loading”.


And using the` v-else` directive, if` loading` is false, we’ll map over the characters using the` v-for` directive, making sure to let Vue know how to uniquely identify items with the` :key` directive.


If we’ve configured this correctly, we should see a list of characters.


Our final` App.vue` should look something like the following:


```text
<template>
<p v-if="error">Something went wrong...</p>
<p v-if="loading">Loading...</p>
<p v-else v-for="character in result.characters.results" :key="character.id">
{{ character.name }}
</p>
<div></div>
</template>


<script>
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable'


const CHARACTERS_QUERY = gql`
query Characters {
characters {
results {
id
name
image
}
}
}
`


export default {
name: 'App',
setup () {
const { result, loading, error } = useQuery(CHARACTERS_QUERY);
return {
result,
loading,
error
}
}
}


</script>


<style>
#app {
font-family: Avenir, Helvetica, Arial, sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
text-align: center;
color: #2c3e50;
margin-top: 60px;
}
</style>
﻿
```


## What next?


We just learned how to set up a Vue Apollo app and how to query a GraphQL server using the Vue Composition API.


I recommend continuing by reading the[composition API docs](https://v4.apollo.vuejs.org/guide-composable/query.html) . With respect to the topic of queries:


- Learn[how to pick results with useResult](https://v4.apollo.vuejs.org/guide-composable/query.html#useresult)
- Learn[how to use variables in your queries](https://v4.apollo.vuejs.org/guide-composable/query.html#variables)


With respect to[mutations](https://v4.apollo.vuejs.org/guide-composable/mutation.html) :


- Learn[how to execute mutations](https://v4.apollo.vuejs.org/guide-composable/mutation.html#executing-a-mutation)
- Learn[how to update the cache after a mutation](https://v4.apollo.vuejs.org/guide-composable/mutation.html#updating-the-cache-after-a-mutation)


Also be sure to learn about[subscriptions](https://v4.apollo.vuejs.org/guide-composable/subscription.html#overview) ,[pagination](https://v4.apollo.vuejs.org/guide-composable/pagination.html) , and[error-handling](https://v4.apollo.vuejs.org/guide-composable/error-handling.html) in Vue Apollo.


For a video walkthrough and more information on the difference between the Options API vs. the Composition one, check out[Natalia Tepluhina’s talk from GraphQL Summit Worldwide 2021](https://www.youtube.com/watch?v=pfWcYEBGqEc) .


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
