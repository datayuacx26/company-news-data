---
schema_version: "1.0.0"
document_id: "941ada4283b33ae369307d1155d3fd36c79e4649a85bfa2777412db88b129f11"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/a-stronger-typed-react-apollo-c43bd52be0d8"
published_at: "2017-06-13T23:52:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:cdfb32aabd92efd9ecf9e77fa2ed46e3d8eecc118c7f19dfd955a9e09710d4cb"
---

# A stronger (typed) React Apollo

Static typing can be low-hanging fruit to improve your app. As data flows throughout the application, it is easy to forget its shape, and even for what it’s used. Although you could have unit tests for each small bit of functionality, static typing can help reduce this need by all but eliminating one of the most common classes of bugs in JavaScript: type errors. A shoutout to[Dmitrii Abramov](https://medium.com/u/d932193d1596?source=post_page-----c43bd52be0d8----------------------) for painting this clear picture:


testing pyramid in 2016[pic.twitter.com/1rTbfKoNsT](https://t.co/1rTbfKoNsT)


— Aaron Abrams א/א (@boujeepossum)[December 5, 2016](https://twitter.com/boujeepossum/status/805913874704674816?ref_src=twsrc%5Etfw)


Apollo has had TypeScript support since its[launch](https://github.com/apollographql/apollo-client/issues/6) , and support for Flow typings just landed in version 1.4. Today I’d love to show you how to use them with Apollo Client and React Apollo!


For TypeScript users, a follow up article will go through the improvements to types for React Apollo and similar examples.


## Getting started


If you haven’t taken a look at[Apollo](https://medium.com/u/3301bc8d891d?source=post_page-----c43bd52be0d8----------------------) yet, now is a great time! Check out the[blog](https://dev-blog.apollodata.com/) and the full-stack[React + Apollo tutorial](https://dev-blog.apollodata.com/full-stack-react-graphql-tutorial-582ac8d24e3b) before going too much farther.


You’ll get more out of this post if you have Flow configured with your project already. If you want to set it up, I recommend checking out the[official install guide](https://flow.org/en/docs/install/) .


Flow typings landed in version 1.4 of Apollo Client and were recently greatly improved. So make sure you have the latest version of React Apollo and Apollo Client to get the best typing experience.


```text
npm install --save react-apollo apollo-client
```


That’s it! Now you can start using the new types with Apollo.


---


## Understanding your render


Higher order components can introduce a level of indirection that makes data flow harder to reason about in React. I personally think they are well worth it for the benefits they provide, especially since static types can make this a lot better. Helping developers understand what they can use in their React components was our first priority when we set up flow types for Apollo.


Let’s take for example a simple React component wrapped with the graphql HOC from React Apollo. This component will display a hero from Star Wars and their friends with data returned from a GraphQL endpoint:


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


The above code pulls some data from a GraphQL API using a query and includes lifecycle information, such as loading and error information. With a few minor changes, we can tell Flow how to support us in writing code within this render function.


```text
+ // @flow
import React from "react";
import gql from "graphql-tag";
import { graphql } from "react-apollo";


+ import type { OperationComponent } from "react-apollo";


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


+ export type Hero = {
+   name: string,
+   id: string,
+   appearsIn: string[],
+   friends: Hero[],
+ };


+ export type Response = {
+   hero: Hero,
+ };


+ export const withCharacter: OperationComponent<Response> = graphql(HERO_QUERY, {
- export const withCharacter(({ data: { loading, hero, error } }) => {
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


Let’s walk through what is happening in this diff.


1. ` // @flow` This line tells Flow to check our file
2. We also import the main type export from react-apollo,` OperationComponent` .` OperationComponent<Response, InputProps, Props>` is an interface (a polymorphic type) that takes three possible type parameters. The rest of this article will walk you through when and how to use these types.


```text
import type { OperationComponent } from "react-apollo";
```


3. Now we get into the fun stuff! We need to tell Flow what the shape of our data from our graphql server will look like. We manually write the types for our response data. Alternatively, you can use[apollo-codegen](https://github.com/apollographql/apollo-codegen) to generate the types for you! Shoutout to[Lewis Chung](https://medium.com/u/2ab2eb7f698c?source=post_page-----c43bd52be0d8----------------------) for working to make this easier and[Martijn Walraven](https://medium.com/u/592c66ff70d?source=post_page-----c43bd52be0d8----------------------) and[Robin Ricard](https://medium.com/u/abc034e10a70?source=post_page-----c43bd52be0d8----------------------) for the codegen framework!


```text
+ export type Hero = {
+   name: string,
+   id: string,
+   appearsIn: string[],
+   friends: Hero[],
+ };+ export type Response = {
+   hero: Hero,
+ };
```


5. The last line is where the magic happens. We tell Flow what the shape of the result will look like from the server when the graphql enhancer wraps a component.` + export const withCharacter: OperationComponent<Response> = graphql(HERO_QUERY, {` .


If you already are a user of Flow and have your data typed already, with a couple of imports and a single type addition, you can type any component that is wrapped with` graphql` !


Currently Flow does not support the ES7 decorator syntax so you won’t be able to get all of the benefits of stronger type support if you are using decorators. Shoutout to TypeScript for supporting them!


The immediate benefits can be seen by writing some bad code! Let’s make a mistake and see what happens:


```text
export default withCharacter(({ loading, hero, error }) => {
// $ExpectError [number] this type cannot be compared to boolean
if (loading > 1) return <div>Loading</div>;
if (error) return <h1>error.name</h1>;
return ...;
}
```


Since` withCharacter` is now typed, Flow will error out preventing a potential production bug!


## Taking control of your tree


Wrapped components are almost always exported and used by a component somewhere else in your tree, so if your exported component has prop requirements, we need to tell Flow so it can help prevent errors elsewhere in our tree. With the second type parameter in` OperationComponent` , we can do just that! Let’s make a change to our initial component that converts it to use a prop called` episode` :


```text
+ export type InputProps = {
+  episode: string,
+ };


+ export const withCharacter: OperationComponent<Response, InputProps> = graphql(HERO_QUERY, {
- export const withCharacter: OperationComponent<Response> = graphql(HERO_QUERY, {


+   options: ({ episode }) => ({
+     variables: { episode },
-   options: () => ({
-     variables: { episode: "JEDI" },


}),
});
```


We can define the props that our exported component will need (` InputProps` ) and attach them to our enhancer using the second type generic of` OperationComponent` . Now our component can pass \`episode\` to pick which characters to load and Flow can know how to enforce that! In practice it would look like this:


```text
// @flow
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
// $ExpectError property `episode`. Property not found in. See: src/Character.js:43
<Character />
</ApolloProvider>;
```


The other benefit we get is typing of the` options` function itself. Let’s say we forgot what props are passed into our component and we thought the episode was a number:


```text
export const withCharacter: OperationComponent<Response, InputProps> = graphql(HERO_QUERY, {
options: ({ episode }) => ({
// $ExpectError [string] This type cannot be compared to number
variables: { episode: episode > 1 },
})
});
```


Thanks Flow!


## Shaping your props


GraphQL is awesome at allowing you to only request the data you want from the server. The client still often needs to reshape or do client side calculations based on these results. React Apollo has a handy dandy props function which lets you reshape the resulting props into a new object. In practice, I use this for almost every enhancer I write.


*It also pairs nicely with*[Ramda](http://ramdajs.com/) *for some fun fun functional code.*


Since this function can shape passed props, plus a potential data result, potential error, and loading state, having it strongly typed would prevent a lot of save => see error => check data => save workflows. Let’s take a look at how to set this up with React Apollo:


```text
+ import type { OperationComponent, QueryProps } from "react-apollo";
- import type { OperationComponent } from "react-apollo";


+ export type Props = Response & QueryProps;


+ export const withCharacter: OperationComponent<Response, InputProps, Props> = graphql(HERO_QUERY, {
- export const withCharacter: OperationComponent<Response, InputProps> = graphql(HERO_QUERY, {
options: ({ episode }) => ({
variables: { episode },
}),
+   props: ({ data }) => ({ ...data }),
});


+ export default withCharacter(({ loading, hero, error }) => {
- export default withCharacter(({ data: { loading, hero, error } }) => {
```


The most important part of the change above is including what the result of the props function will look like on line 7.


Now, let’s check it in action a couple of weeks after we first wrote this file and forgot some things.


```text
export const withCharacter: OperationComponent<Response, InputProps Props> = graphql(HERO_QUERY, {
options: ({ episode }) => ({
variables: { episode },
}),
props: ({ data, ownProps }) => ({
...data,
// $ExpectError [string] This type cannot be compared to number
episode: ownProps.episode > 1,
// $ExpectError property `isHero`. Property not found on object type
isHero: data && data.hero && data.hero.isHero,
}),
});
```


1. The first error shows we don’t have a good memory around what an episode represents. Maybe we should change it to` episodeName` ! Luckily, Flow has our back!
2. I really thought we had an isHero field in our query? Whelp, looks like we don’t based on the types!


These two examples show type checking passed props and resulting data as well as the fact that data may not have arrived yet from the server!


---


This is an early start to Flow types for Apollo Client and React Apollo. I’m hoping to continue to improve and would love feedback from contributors and production users alike!


In the past couple of years JavaScript has come so far and become a pretty incredible tool for end users and developer alike. With the rise in popularity of ES6, powerful testing tools (like J[est](http://facebook.github.io/jest/) ), excellent linting ([Eslint](http://eslint.org/) and[Prettier](https://github.com/prettier/prettier) ), and static typing ([Flow](https://flow.org/) and[TypeScript](https://www.typescriptlang.org/) ), you can create large scale applications with confidence that they can be maintained and loved by your users.


I’d like to give a special thanks to[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----c43bd52be0d8----------------------) ,[Lewis Chung](https://medium.com/u/2ab2eb7f698c?source=post_page-----c43bd52be0d8----------------------) ,[Brett Jurgens](https://medium.com/u/b1f108c2bfa9?source=post_page-----c43bd52be0d8----------------------) ,[Ian](https://github.com/ianks) , and[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----c43bd52be0d8----------------------) for reviewing the initial type PR and[kurtiskemple](https://medium.com/u/8be181d9f877?source=post_page-----c43bd52be0d8----------------------) and[Peggy Rayzis](https://medium.com/u/c827782c6410?source=post_page-----c43bd52be0d8----------------------) for plugging into some MLS code for feedback!


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
