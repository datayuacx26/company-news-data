---
schema_version: "1.0.0"
document_id: "164812642b01306c3976cf04392ec7409166e63c7d5d1d57f1d1b43479ba7286"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-apollo-client-2-5-c12230cabbb7"
published_at: "2019-02-26T21:51:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:fd1ba81655e032b14b18ff1c8295cedcc708119f79146cb3988346a19c24ddad"
---

# Announcing Apollo Client 2.5

One year ago, we had a vision for[the future of state management](https://blog.apollographql.com/the-future-of-state-management-dd410864cae2) . We wanted to provide developers with *one source of truth* for all of their app’s data and *one unified interface* to access it. Our dream is finally a reality with the release of **Apollo Client 2.5** , now with local state management!


Managing local data with Apollo Client follows the same programming model as managing remote data from a GraphQL server. Build a schema to model local data, write resolvers to obtain the data, and then query both local and remote data using GraphQL. Thanks to GraphQL’s static typing, you can also enjoy autocomplete inside your editor and automatic TypeScript definitions for your local data! 🚀


While local state management is new to the Apollo Client core library, it has already been tested in production by teams at Airbnb, TaskRabbit, and Hilton through our extension` <a href="https://github.com/apollographql/apollo-link-state" target="_blank" rel="noreferrer noopener">apollo-link-state</a>` . Before merging` apollo-link-state` into core, we also decreased our bundle size to offset the new features.


Now, let’s dive into an example from[our official tutorial](https://www.apollographql.com/docs/tutorial/introduction.html) to learn what local state management with Apollo is all about:


## 1. Define your data with a client schema


Suppose we’re building an app that lets customers book a seat on a rocket launch. It needs a shopping cart that holds the launches customers are interested in.


Our` Launch` type from our GraphQL server looks like this:


```text
type Launch {
id: ID!
site: String
rocket: Rocket
}
```


We’re managing the shopping cart on the client, which means our GraphQL API has no knowledge of what’s in a cart. Wouldn’t it be great if we could query for a specific` Launch` from the server and find out if that launch is already in a customer’s cart all at the same time?


Through schema composition, Apollo Client 2.5 allows us to define a **client schema** that extends our server schema. To build this particular feature, we need to` extend` our` Launch` type to have a property called` isInCart` . Once we’ve written this schema fragment, we pass it to the` ApolloClient` constructor via the` typeDefs` option:


```text
const client = new ApolloClient({
cache: new InMemoryCache(),
link: new HttpLink({
uri: 'http://localhost:4000/graphql',
}),
typeDefs: gql`
extend type Launch {
isInCart: Boolean!
}
`,
});
```


Making Apollo Client aware of this client schema extension opens up new possibilities. Developer tooling now has access to type information for both client and server schemas, which streamlines GraphQL development across all parts of your app. You can also build against a planned future schema before it’s implemented on the server by extending your schema on the client and mocking local resolvers.


Now that we’ve let Apollo Client know we want the` isInCart` field to be part of our` Launch` type, how can we make sure this information gets included when querying the server for` Launch` details? Local resolvers to the rescue!


## 2. Write a local resolver for your data


Local resolvers have the same API as resolvers you write for[Apollo Server](https://www.apollographql.com/docs/apollo-server/) , only they’re run by Apollo Client. Local resolvers can calculate a result for any part of a GraphQL query, and Apollo Client will merge those results with any data received from your remote API.


Let’s tell Apollo Client about our` isInCart` local resolver:


```text
const client = new ApolloClient({
cache: new InMemoryCache(),
link: new HttpLink({
uri: 'http://localhost:4000/graphql',
}),
resolvers: {
Launch: {
isInCart: (launch, _args, { cache }) => {
const { cartItems } = cache.readQuery({
query: GET_CART_ITEMS
});
return cartItems.includes(launch.id);
},
},
},
});
```


Here we’re passing a` resolvers` map to Apollo Client that defines the` isInCart` function for the` Launch` type. It checks the Apollo cache to see if a specific launch is already in our cart. When run, the boolean returned from` isInCart` is seamlessly merged with the rest of the` GET_LAUNCH_DETAILS` query result.


## 3. Fetch local and remote data in one query


All we have to do to query our local data alongside our remote data is add a` @client` directive to the` isInCart` field:


```text
const GET_LAUNCH_DETAILS = gql`
query LaunchDetails($launchId: ID!) {
launch(id: $launchId) {
isInCart @client
site
rocket {
type
}
}
}
`;
```


Now that we’ve learned how to query local data, let’s learn how you can enhance your experience even further with GraphQL-aware tooling.


Autocomplete for local data in VS Code


### Autocomplete in your editor with Apollo VS Code


Apollo Client 2.5 helps developers maximize the benefits of GraphQL across their entire development experience.[Apollo VS Code](https://marketplace.visualstudio.com/items?itemName=apollographql.vscode-apollo) leverages knowledge of your client and server schemas to drive features like autocomplete when building your queries, jump to schema definitions, and more.


### Automatic type definitions with the Apollo CLI


We’re huge fans of TypeScript at Apollo, which is why we’re so excited that you can now automatically generate TypeScript definitions for local data with the[Apollo CLI](https://github.com/apollographql/apollo-tooling) !


### Apollo DevTools Chrome extension


The[Apollo Client DevTools](https://github.com/apollographql/apollo-client-devtools) Chrome extension has also been fully integrated with Apollo Client 2.5 to let you inspect your client schema details, run` @client` based queries and mutations, inspect the client side Apollo Cache, and watch fired queries, all inside your browser.


## More new features 🔥


In addition to updated[docs](https://www.apollographql.com/docs/react/essentials/local-state.html) , we’d also like to call out some new features enabled by merging local state management into core:


### [Code splitting](https://www.apollographql.com/docs/react/essentials/local-state.html#code-splitting)


Depending on the complexity and size of your local resolvers, you might not always want to define them upfront. If you have local resolvers that are only needed in a specific part of your app, you can leverage Apollo Client’s` addResolvers` and` setResolvers` methods to dynamically add resolvers. This can be really useful when leveraging route or component based code splitting.


### Local subscriptions


In addition to query and mutation resolvers, you can also enable realtime local data with subscription resolvers.


### [@export directive](https://www.apollographql.com/docs/react/essentials/local-state.html#client-variables)


You can now use` @client @export(as: "someVariable")` to load a value from local state, and use that value as a variable for a remote query, all within the same query definition.


### [@client(always: true)](https://www.apollographql.com/docs/react/essentials/local-state.html#forcing-resolvers)


Local resolvers adhere to the Apollo Client query[fetch policy](https://www.apollographql.com/docs/react/api/react-apollo.html#graphql-config-options-fetchPolicy) , which means they might not be run on every request depending on your configuration. You can use` @client(always: true)` to override this behavior and have specific resolvers fire every time a query is run.


### No bundle size increase! 🎉


Apollo Client 2.5 kickstarts a big focus we’re putting on bundle size reduction. This work has allowed us to bake in all of the new local state features without increasing Apollo Client’s bundle size.


## To Apollo Client 3.0 and beyond


Not only do Apollo Client 2.5’s local state features introduce new ways to interact with GraphQL across your entire application, they are also stepping stones towards larger changes coming in Apollo Client 3.0.


Apollo Client 3.0 is focused on delivering a new way to manage the storage of local state by improving the current Apollo Cache implementation. Cache invalidation and garbage collection are by far the most requested Apollo Client features, and we’re excited to be focusing on them next. If you’re interested in helping out,[please join in the discussion](https://github.com/apollographql/apollo-feature-requests/issues/4) . We’ll be posting more updates in that issue soon.


## Try Apollo Client 2.5 today!


Apollo Client 2.5 is ready for production (` apollo-client@latest` and` react-apollo@latest` ), and you can find the full updated documentation[here](https://www.apollographql.com/docs/react/essentials/local-state.html) . We’ve also included a[migration guide](https://www.apollographql.com/docs/react/essentials/local-state.html#migrating) for current` apollo-link-state` users. We’re super excited about the possibilities enabled by Apollo Client’s new local state management features, and we can’t wait to see the amazing things the Apollo community builds with them.


A big **THANK YOU** to everyone who helped with this release, through alpha/beta testing, PRs, code reviews and discussions. Your contributions have been both incredible and massively appreciated!


If you have any questions or comments about Apollo Client 2.5, please reach out via[Spectrum](https://spectrum.chat/apollo) .


Written by


Hugh Willson


[Read more by Hugh Willson](https://www.apollographql.com/blog/author/hwillson)
