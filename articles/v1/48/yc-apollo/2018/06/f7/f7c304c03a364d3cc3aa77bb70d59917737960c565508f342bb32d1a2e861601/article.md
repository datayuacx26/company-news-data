---
schema_version: "1.0.0"
document_id: "f7c304c03a364d3cc3aa77bb70d59917737960c565508f342bb32d1a2e861601"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/testing-apollos-query-component-d575dc642e04"
published_at: "2018-06-14T22:48:16+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:5533ffe23a360d9602c0f5560bfa72f5bd56a83bd22c2ffc2dea368188a2e112"
---

# Testing Apollo’s Query Component

We’ve been working on a new[guides and best practices site](https://www.apollographql.com/docs) for Apollo and GraphQL that covers some of the most important topics we’ve heard from our users. This post reproduces part of the[Testing React Components](https://www.apollographql.com/docs/react/recipes/testing) article, and if you’d like to read more about testing (including mutation components), and other best practices, check out the[full article](https://www.apollographql.com/docs/guides/testing-react-components.html) on the site!


As always, we’re grateful to the community help that makes Apollo possible. Thanks to[Dirk-Jan Rutten](https://medium.com/u/b016d103ae51?source=post_page-----d575dc642e04----------------------) for[kick-starting this article on GitHub](https://github.com/apollographql/apollo-client/pull/3309) !


---


Running tests against your code has long been a best practice. Testing provides additional security for the code that’s already written, and prevents accidental regressions in the future. Components utilizing` react-apollo` , the React integration for Apollo Client, are no exception.


Although` react-apollo` has a lot going on under the hood to make your life as a React developer easy, the library provides multiple tools for testing that simplify those abstractions, and allows complete focus on the component logic. These testing utilities have long been used to test the` react-apollo` library itself, and they will be supported long-term.


The goal: Lots of green checkmarks!


## Testing against remote data


The` react-apollo` libary relies on[context](https://reactjs.org/docs/context.html) in order to pass the` ApolloClient` instance through the React component tree. In addition,` react-apollo` makes network requests in order to fetch data. This behavior affects how tests should be written for components that use` react-apollo` .


This guide will explain step-by-step how to test` react-apollo` code. The following examples use the[Jest](https://facebook.github.io/jest/docs/en/tutorial-react.html) testing framework, but most concepts should be reusable with other libraries. These examples aim to use as simple of a toolset as possible, so React’s[test renderer](https://reactjs.org/docs/test-renderer.html) will be used in place of React-specific tools like[Enzyme](https://github.com/airbnb/enzyme) and[react-testing-library](https://github.com/kentcdodds/react-testing-library) .


Consider the component below, which makes a basic query, and displays its results:


```text
import React from 'react';
import gql from 'graphql-tag';
import { Query } from 'react-apollo';


// Make sure the query is also exported -- not just the component
export const GET_DOG_QUERY = gql`
query getDog($name: String) {
dog(name: $name) {
id
name
breed
}
}
`;


export const Dog = ({ name }) => (
<Query query={GET_DOG_QUERY} variables={{ name }}>
{({ loading, error, data }) => {
if (loading) return 'Loading...';
if (error) return `Error!`;


return (
<p>
{data.dog.name} is a {data.dog.breed}
</p>
);
}}
</Query>
);
```


Given this component, let’s try to render it inside a test, just to make sure there are no render errors:


```text
// Broken because it's missing Apollo Client in the context
it('should render without error', () => {
renderer.create(<Dog name="Buck" />);
});
```


This test would produce an error because Apollo Client isn’t available on the context for the` Query` component to consume.


In order to fix this we could wrap the component in an` ApolloProvider` and pass an instance of Apollo Client to the` client` prop:


```text
// Not predictable
it('renders without error', () => {
renderer.create(
<ApolloProvider client={client}>
<Dog name="Buck" />
</ApolloProvider>,
);
});
```


However, this will cause the tests to run against an actual backend which makes the tests very unpredictable for the following reasons:


- The server could be down.
- There may be no network connection.
- The results are not guaranteed to be the same for every query.


## MockedProvider


The` react-apollo/test-utils` module exports a` MockedProvider` component which simplifies the testing of React components by mocking calls to the GraphQL endpoint. This allows the tests to be run in isolation and provides consistent results on every run by removing the dependence on remote data.


By using this` MockedProvider` component, it’s possible to specify the exact results that should be returned for a certain query using the` mocks` prop.


Here’s an example of a test for the above` Dog` component using` MockedProvider` , which shows how to define the mocked response for` GET_DOG_QUERY` :


```text
// dog.test.js


import { MockedProvider } from 'react-apollo/test-utils';


// The component AND the query need to be exported
import { GET_DOG_QUERY, Dog } from './dog';


const mocks = [
{
request: {
query: GET_DOG_QUERY,
variables: {
name: 'Buck',
},
},
result: {
data: {
dog: { id: '1', name: 'Buck', breed: 'bulldog' },
},
},
},
];


it('renders without error', () => {
renderer.create(
<MockedProvider mocks={mocks} addTypename={false}>
<Dog name="Buck" />
</MockedProvider>,
);
});
```


The` mocks` array takes objects with specific` request` s and their associated` result` s. When the provider receives a` GET_DOG_QUERY` with matching` variables` , it returns the corresponding object from the` result` key.


### addTypename


You may notice the prop being passed to the` MockedProvider` called` addTypename` . The reason this is here is because of how Apollo Client normally works. When a request is made with Apollo Client normally, it adds a` __typename` field to every object type requested. This is to make sure that Apollo Client’s cache knows how to normalize and store the response. When we’re making our mocks, though, we’re importing the raw queries *without typenames* from the component files.


If we don’t disable the adding of typenames to queries, the imported query won’t match the query actually being run by the component during our tests.


*In short, if queries are lacking*` <em>__typename</em>` *, it’s important to pass the*` <em>addTypename={false}</em>` *prop to the*` <em>MockedProvider</em>` *.*


## Testing loading states


In this example, the` Dog` component will render, but it will render in a loading state, not the final response state. This is because` MockedProvider` doesn’t just return the data but instead returns a` Promise` that will resolve to that data. By using a` Promise` it enables testing of the loading state in addition to the final state:


```text
it('should render loading state initially', () => {
const component = renderer.create(
<MockedProvider mocks={[]}>
<Dog />
</MockedProvider>,
);


const tree = component.toJSON();
expect(tree.children).toContain('Loading...');
});
```


This shows a basic example test that tests the loading state of a component by checking that the children of the component contain the text` Loading...` . In an actual application, this test would probably be more complicated, but the testing logic would be the same.


## Testing final state


Loading state, while important, isn’t the only thing to test. To test the final state of the component after receiving data, we can just wait for it to update and test the final state.


```text
const wait = require('waait');


it('should render dog', async () => {
const dogMock = {
request: {
query: GET_DOG_QUERY,
variables: { name: 'Buck' },
},
result: {
data: { dog: { id: 1, name: 'Buck', breed: 'poodle' } },
},
};


const component = renderer.create(
<MockedProvider mocks={[dogMock]} addTypename={false}>
<Dog name="Buck" />
</MockedProvider>,
);


await wait(0); // wait for response


const p = component.root.findByType('p');
expect(p.children).toContain('Buck is a poodle');
});
```


Here, you can see the` await wait(0)` line. This is a utility function from the` <a href="https://npm.im/waait" target="_blank" rel="noreferrer noopener">waait</a>` npm package. It delays until the next “tick” of the event loop, and allows time for that` Promise` returned from` MockedProvider` to be fulfilled. After that` Promise` resolves (or rejects), the component can be checked to ensure it displays the correct information — in this case, “Buck is a poodle”.


✅ is a great feeling


For more complex UI with heavy calculations, or delays added into its render logic, the` wait(0)` will not be long enough. In these cases, you could either increase the wait time or use a package like` <a href="https://npm.im/wait-for-expect" target="_blank" rel="noreferrer noopener">wait-for-expect</a>` to delay until the render has happened. The risk of using a package like this everywhere by default is that *every* test could take up to five seconds to execute (or longer if the default timeout has been increased).


## Testing error states


Since they can make or break the experience a user has when interacting with the app, error states are one of the most important states to test, but are often less tested in development.


Since most developers would follow the “happy path” and not encounter these states as often, it’s almost *more* important to test these states to prevent accidental regressions.


To simulate a GraphQL error, an` error` property can be included on the mock, in place of or in addition to the` result` .


```text
it('should show error UI', async () => {
const dogMock = {
request: {
query: GET_DOG_QUERY,
variables: { name: 'Buck' },
},
error: new Error('aw shucks'),
};


const component = renderer.create(
<MockedProvider mocks={[dogMock]} addTypename={false}>
<Dog name="Buck" />
</MockedProvider>,
);


await wait(0); // wait for response


const tree = component.toJSON();
expect(tree.children).toContain('Error!');
});
```


Here, whenever the` MockedProvider` receives a` GET_DOG_QUERY` with matching` variables` , it will return the error assigned to the` error` property in the mock. This forces the component into the error state, allowing verification that it’s being handled gracefully.


Testing UI components isn’t a simple issue, but hopefully these tools will create confidence when testing components that are dependent on data.


For a working example, check out[this CodeSandbox](https://codesandbox.io/s/40k7j708n4) .


---


### Read more in the full guide!


For more, including how to test` Mutation` components, check out the[Testing React Components](https://www.apollographql.com/docs/guides/testing-react-components.html) page on[our new guides and best practices site](https://www.apollographql.com/docs/) !


Written by


Jake Dawkins


[Read more by Jake Dawkins](https://www.apollographql.com/blog/author/jake)
