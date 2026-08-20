---
schema_version: "1.0.0"
document_id: "bbd0cbf18fa060274af34ffe4742d7af40bef633928134bd3347662ca40a7f4b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/getting-started-with-typescript-and-apollo-a9aa2c7dcf87"
published_at: "2017-07-27T18:11:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:f6b08eff2380d41d0ff2f1e9b93da77f0c3eea3f35fd1c117ad54df781af32c4"
---

# Getting started with TypeScript and Apollo

This is a follow up to a similar post on using Flow and Apollo which can be found[here](https://dev-blog.apollodata.com/a-stronger-typed-react-apollo-c43bd52be0d8) .


When the Apollo team started out to build a flexible and incrementally adoptable GraphQL client, one of the early[discussions](https://github.com/apollographql/apollo-client/issues/6) centered around static typing for the library code. Since the library deals with a complex data types (like the parsed abstract syntax tree of a GraphQL operation), it seemed like making the code type safe was a needed addition.


The team investigated both Flow and TypeScript, and ultimately went with TypeScript for its active development, editor integration, and strong tooling. It’s been a huge help ever since.


### Close, but not enough


Even though Apollo Client and React Apollo are both written in TypeScript, the actual type usage for application developers was not as strong as it should have been. In many places, React Apollo used either` any` , essentially bypassing static types, or left types off all together where allowed.


Luckily, thanks to the efforts of[Ian Ker-Seymer,](https://medium.com/u/ce97660ec72?source=post_page-----a9aa2c7dcf87----------------------) these shortcomings were brought to the attention of the Apollo team. With his help, application developers can opt in to much stronger TypeScript checking of their React apps!


## Getting Started


**This article will follow the same progression as the**[article on Flow](https://dev-blog.apollodata.com/a-stronger-typed-react-apollo-c43bd52be0d8) **,** and it also assumes you already have TypeScript configured in your project. If you don’t, check out this[great starter tooling from Microsoft](https://github.com/Microsoft/TypeScript-React-Starter#typescript-react-starter) .


We will start with a simple React component wrapped with the` graphql` higher order component (HOC) from React Apollo. This component will display a hero from Star Wars and their friends, with data returned from a GraphQL endpoint:


```text
import React from "react";
import gql from "graphql-tag";
import { graphql } from "react-apollo";


export const HERO_QUERY = gql`
query GetCharacter($episode: Episode!) {
hero(episode: $episode) {
name
id
friends {
name
id
appearsIn
}
}
}
`;


export const withCharacter = graphql(HERO_QUERY, {
options: () => ({
variables: { episode: "JEDI" },
}),
});


export default withCharacter(({ data: { loading, hero, error } }) => {
if (loading) return <div>Loading</div>;
if (error) return <h1>ERROR</h1>;
return (
<div>
{hero &&
<div>
<h3>{hero.name}</h3>
{hero.friends.map(friend =>
<h6 key={friend.id}>
{friend.name}:
{" "}{friend.appearsIn.map(x => x.toLowerCase()).join(", ")}
</h6>
)}
</div>}
</div>
);
}
```


The above code pulls some data from a GraphQL API using a query and includes lifecycle information, such as loading and error information. With a few minor changes, we can tell Typescript how to support us in writing code within this render function.


Let’s walk through what is happening in this diff.


1. We need to tell TypeScript what the shape of our data from our graphql server will look like. We manually write the types for our response data. Alternatively, you can use[apollo-codegen](https://github.com/apollographql/apollo-codegen) to generate the types for you! Shoutout to[Lewis Chung](https://medium.com/u/2ab2eb7f698c?source=post_page-----a9aa2c7dcf87----------------------) for working to make this easier and[Martijn Walraven](https://medium.com/u/592c66ff70d?source=post_page-----a9aa2c7dcf87----------------------) and[Robin Ricard](https://medium.com/u/abc034e10a70?source=post_page-----a9aa2c7dcf87----------------------) for the codegen framework!


```text
+ export type Hero = {
+   name: string;
+   id: string;
+   appearsIn: string[];
+   friends: Hero[];
+ };


+ export type Response = {
+   hero: Hero,
+ };
```


2. The last line is where the magic happens. We tell TypeScript what the shape of the result will look like from the server when the graphql enhancer wraps a component.


If you already had your project set up with TypeScript, and have already typed your response data, all you have to do is add the type to the` graphql` HOC and you are off!


The immediate benefits can be seen by writing some bad code! Let’s make a mistake and see what happens:


```text
export default withCharacter(({ loading, hero, error }) => {
// Operator '>' cannot be applied to types 'boolean' and 'number'.
if (loading > 1) return <div>Loading</div>;
if (error) return <h1>error.name</h1>;
return ...;
}
```


Since` withCharacter` is now typed, TypeScript will error out preventing a potential production bug!


## Taking control of your tree


Wrapped components are almost always exported and used by a component somewhere else in your tree, so if your exported component has prop requirements, we need to tell TypeScript so it can help prevent errors elsewhere in our tree. Since the` graphql` wrapper supports polymorphic types, we can use the second type parameter of it to do just that! Let’s make a change to our initial component that converts it to use a prop called` episode` :


```text
+ export type InputProps = {
+  episode: string,
+ };


+ export const withCharacter = graphql<Response, InputProps>(HERO_QUERY, {
- export const withCharacter = graphql<Response>(HERO_QUERY, {
+   options: ({ episode }) => ({
+     variables: { episode },
-   options: () => ({
-     variables: { episode: "JEDI" },
}),
});
```


We can define the props that our exported component will need (` InputProps` ) and attach them to our enhancer using the second type argument. Now our component can pass` episode` to pick which characters to load and TypeScript can know how to enforce that! In practice it would look like this:


```text
import React from "react";
import ApolloClient, { createNetworkInterface } from "apollo-client";
import { ApolloProvider } from "react-apollo";


import Character from "./Character";


export const networkInterface = createNetworkInterface({
uri: "https://mpjk0plp9.lp.gql.zone/graphql",
});
export const client = new ApolloClient({ networkInterface });


export default () =>
<ApolloProvider client={client}>
// Type '{}' is not assignable to type 'Readonly<InputProps>'.
// Property 'episode' is missing in type '{}'.
<Character />
</ApolloProvider>;
```


The other benefit we get is typing of the` options` function itself. Let’s say we forgot what props are passed into our component and we thought the episode was a number:


```text
export const withCharacter: OperationComponent<Response, InputProps> = graphql(HERO_QUERY, {
options: ({ episode }) => ({
// Operator '>' cannot be applied to types 'string' and 'number'.
variables: { episode: episode > 1 },
})
});
```


Thanks TypeScript!


## Shaping your props


GraphQL is awesome at allowing you to only request the data you want from the server. The client still often needs to reshape or do client side calculations based on these results. React Apollo has a handy dandy props function which lets you reshape the resulting props into a new object. In practice, I use this for almost every enhancer I write.


*Note: It also pairs nicely with*[Ramda](http://ramdajs.com/) *for some fun, fun functional code.*


Since this function can shape passed props, plus a potential data result, potential error, and loading state, having it strongly typed would prevent a lot of *save => see error => check data => save* workflows. Let’s take a look at how to set this up with React Apollo:


The most important part of the change above is including what the result of the props function will look like on line 5.


Now, let’s check it out in action a couple of weeks after we first wrote this file and forgot some things.


```text
export const withCharacter = graphql<Response, InputProps, Props>(HERO_QUERY, {
options: ({ episode }) => ({
variables: { episode },
}),
props: ({ data, ownProps }) => ({
...data,
// Operator '>' cannot be applied to types 'string' and 'number'.
episode: ownProps.episode > 1,
// Property 'isHero' does not exist on type 'Hero'.
isHero: data && data.hero && data.hero.isHero,
}),
});
```


1. The first error shows we don’t have a good memory around what an episode represents. Maybe we should change it to` episodeName` ! Luckily, TypeScript has our back.
2. I really thought we had an isHero field in our query? Whelp, looks like we don’t based on the types.


These two examples show type checking passed props and resulting data as well as the fact that data may not have arrived yet from the server!


https://giphy.com/gifs/broad-city-hillary-clinton-3osxYdwmc9Oy8H7h7i


---


I hope that this article shows both how awesome TypeScript is, and how subtle the differences between Flow and TypeScript can be for application developers. They are both amazing choices!


I’d like to give a special thanks to[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----a9aa2c7dcf87----------------------) ,[Lewis Chung](https://medium.com/u/2ab2eb7f698c?source=post_page-----a9aa2c7dcf87----------------------) ,[Brett Jurgens](https://medium.com/u/b1f108c2bfa9?source=post_page-----a9aa2c7dcf87----------------------) ,[Ian](https://github.com/ianks) , and[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----a9aa2c7dcf87----------------------) for reviewing the initial type PR. I’ve also[started the effort to include documentation](https://github.com/apollographql/react-docs/pull/258) for Flow and TypeScript in the official React docs.


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
