---
schema_version: "1.0.0"
document_id: "6eb7b4bd460320d8f67328f42317a7601250a48844687c66f286823c2b979e9f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introduction-to-testing"
published_at: "2021-04-22T17:54:59+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:073152c6adac3c1854078ccaa31a0a06269f53403bf24878d99f9a4c3e6a7e8d"
---

# Testing Apollo Client Applications

Testing is likely one of the most important (yet challenging) aspects of developing high-quality software that can safely withstand change. Tests give you confidence that your code works and will continue to work — even as you add new features over time.


This article will discuss three different approaches to testing Apollo Client applications: unit, integration, and end-to-end tests. We’ll discuss what they test, their tradeoffs, and where they belong within a comprehensive testing strategy.


## Prerequisites


- You know the basics of Apollo Client ([see the lift-off course to get started](http://odyssey.apollographql.com/?referrer=blog) )
- (optional) You’ve read[Apollo Client & Client-Side Architecture Basics](https://www.apollographql.com/blog/apollo-client-client-side-architecture-basics) to learn about the different concerns involved in a client-side web application


## Unit testing


The first type of test that most developers learn about when they get into testing is *unit testing* .


### What is a unit test?


A unit test is the type of test most concerned with testing an **individual unit of code** .


That’s admittedly abstract, but within the context of an Apollo Client application, an individual unit of code is most likely to be either a function, a React hook, or a **React component** .


Out of all the different tests that we can write, unit tests are the *cheapest* . Because of their small surface area, their simplicity, and the little to no dependencies they have, these tests are the fastest to *write* and *run* . We write unit tests so that they don’t rely on expensive operations like *real* network requests.


### What are we testing?


There are two things we could test for in React components: *presentation* and (optionally) *behavior* .


**Presentation** : When fetching data using Apollo Client, components can be in either` loading` ,` success` , or` error` states. For each of these states, it’s a good idea to test that the presentation is what we intend it to be.


For example, consider we have a component that presents details about a specific dog (performs a` GetDogByName` query).


```text
import React from 'react';
import { gql, useQuery } from '@apollo/client';


// The query for the `Dog` component is close to the component
export const GET_DOG_QUERY = gql`
query GetDogByName($name: String) {
dog(name: $name) {
id
name
breed
}
}
`;


export function Dog({ name }) {
const { loading, error, data } = useQuery(
GET_DOG_QUERY,
{ variables: { name } }
);
if (loading) return <p>Loading...</p>;
if (error) return <p>Error!</p>;


return (
<p>
{data.dog.name} is a {data.dog.breed}
</p>
);
}
```


In this scenario, we’d likely want to test:


- ` loading` — How the component renders when it’s fetching the dog.
- ` success` — How the component renders after it’s successfully fetched the dog.
- ` error` — How the component renders if it was unable to fetch the dog. Due to the nature of errors, there could be any number of domain or infrastructure-specific reasons why the GraphQL query failed (like` DogNotFound` ,` NetworkError` ,` PermissionDenied` , or` UnexpectedError` for example).


To test this, we can use Apollo’s` MockedProvider` component to wrap our component under test with one that lets us pass in mock GraphQL query responses. This enables us to force the code paths necessary to test our components.


```text
import TestRenderer from 'react-test-renderer';
import { MockedProvider } from '@apollo/client/testing';
import { GET_DOG_QUERY, Dog } from './dog';


it('displays a loading message when fetching', () => {
const component = TestRenderer.create(
<MockedProvider mocks={mocks} addTypename={false}>
<Dog name="Buck" />
</MockedProvider>,
);


const tree = component.toJSON();
expect(tree.children).toContain('Loading...');
});


it('displays the dog details on success', () => {
...
});


it('displays a "not found" message when the dog doesnt exist', () => {
...
});


it('displays a generic error message when anything else goes wrong', () => {
...
});
﻿
```


**Behaviour (optional)** : We may also choose to place *behavior* in our React components. In Client-Side Architecture Basics, we call this *interaction logic* — a form of decision-making logic executed after the user interacts with the page somehow — like a keypress or a button click.


```text
import { useQuery } from '@apollo/client';
import gql from 'graphql-tag';
import React from 'react';


const DARK_MODE_QUERY = gql`{
query DarkModeQuery {
app {
darkMode @client
}
}
}`


const DarkMode = (props) => {
const { data, loading, error } = useQuery(DARK_MODE_QUERY);
return (
<button onClick={(e) => data.app.darkMode
? props.turnOffDarkMode(e)
: props.turnOnDarkMode(e)}>
{/** Notice the decision-making logic above? **/}
</button>
)
}


export default DarkMode;
﻿
```


To test this, we’d have to mock out and spy on the state-changing functions in order to test them.


```text
import DarkMode from "./DarkMode"
import { MockedProvider } from '@apollo/client/testing';
import { render } from "@testing-library/react";
import { InMemoryCache } from "@apollo/client";


describe('darkMode', () => {
it('toggles between dark and light mode', async () => {


const turnOffMock = jest.fn();
const turnOnMock = jest.fn();


const mocks: any [] = [
// ... queries
]


const cache = new InMemoryCache({
// ...configuration options...
})


const component = render(
<MockedProvider cache={cache} mocks={mocks}
addTypename={false}>
<DarkMode
turnOffDarkMode={turnOffMock}
turnOnDarkMode={turnOnMock}
/>
</MockedProvider>,
);


// find the button and simulate clicks
const button = await component.findByRole('button');
button.click();
button.click();


expect(turnOffMock).toHaveBeenCalledTimes(1);
expect(turnOnMock).toHaveBeenCalledTimes(1);
})
})
﻿
```


Not only does this increase the surface area of what we need to test in our presentational components, but it makes the setup a little messier and tends to lead to *prop drilling* .


```text
const Layout = ({ children }: { children: any }) => (
<div className="layout">
<DarkMode
turnOffDarkMode={(e: any) => { // Not the cleanest }}
turnOnDarkMode={(e: any) => {  // Not the cleanest }}
/>
{children}
</div>
)
```


While we can certainly write our components this way, we simplify our testing strategy by extracting state and decision-making logic into React Hooks (an *interaction layer* concern) instead.


```text
export function useDarkMode () {
// Using a reactive variable to toggle
const toggleDarkMode = () => {
if (darkModeVar()) {
darkModeVar(false);
} else {
darkModeVar(true);
}
}


return {
operations: { toggleDarkMode }
}
}
```


This allows us to focus on the *presentation* aspect of our React components. And if we wish to test component state and how it changes, we can write unit tests for our custom React Hooks instead.


### How to start unit testing React components


Stay tuned for an in-depth blog post on testing React components, hooks, and[reactive variables](https://www.apollographql.com/blog/local-state-management-with-reactive-variables/) in an Apollo Client app.


Until then, I recommend reading the Apollo client docs on[Testing React Components](https://www.apollographql.com/docs/react/development-testing/testing/) to learn how to test error, success, and loading states.


To make arranging, acting, asserting, and running your tests more enjoyable, I recommend the following tools:


- [Jest](https://jestjs.io/) — A popular and minimal test runner.
- [React-Testing Library](https://testing-library.com/docs/react-testing-library/intro/) — A library of declarative DOM APIs that helps you write more behaviour-driven tests.


### Tradeoffs?


Benefits


- Simplest of all tests
- Fast


Disadvantages


- Component-level unit tests don’t cover a massive amount of surface area — they can only bring so much confidence that your application is working properly.


## Integration testing


This next type of test is in my opinion, the best type of test you can write ([and a few others tend to agree](https://kentcdodds.com/blog/write-tests) ).


### What is an integration test?


It’s hard to really pin down what an integration test is. But if you ask me, I’d call it the type of test that *verifies that a group of (cohesive) components work together to realize a feature.*


These are also the types of tests that:


- Should be relatively fast
- Mock out expensive stuff like network requests or animation libraries
- Try to cover as much surface area as possible
- Start at the very outer boundary of the application — and in the context of a client-side web application, that usually means starting at the *page* level.


### What are we testing?


**Integration tests** test **features** .


What’s a feature exactly? You can think of a *feature* as a use case, user story, functional requirement, or as a *vertical slice* that makes up one entire *request* , whether it be a query or a mutation.


For example, in a job board application, we may have the following features:` login` ,` signup` ,` createJob` ,` editJob` ,` makePayment` ,` applyToJob` .


Once all the features work, we’re done, right? This is a great way to divide up the cohesive modules that we’re going to test.


**How are features organized?**


One way to think about features is to consider that they live within *pages* . And pages have a 1-to-many relationship with features.


Therefore, if we have an architecture that makes it easy to spin up a page and all of the components contained within it, then setting up tests that verify the features within it work could be easier as well.


**How do we test features?**


We want to test that our application behaves the way we think it should for each feature. Each feature has at least one happy path and often — multiple sad paths.


For example, if we were testing the` login` feature’s **happy path** , we may want to assert that:


- We see a success modal
- That local storage has an` auth-token` saved in it
- And finally, that[React-Router](https://reactrouter.com/) gets called with a push to the “/dashboard” route


Alternatively, we may want to test the **sad path of “trying to login with an account that doesn’t yet exist”** . In that case, we assert what the application should do by saying that we should:


- See an error modal
- The text says we haven’t created an account yet
- The page *doesn’t* get redirected anywhere


To do integration testing well means that we need to *thoroughly* understand the features, their happy paths, and the many sad paths.


### How to start integration testing your features


**1 — Document the high-level features in your app** . If your team uses user stories,[Given-When-Then style tests](https://martinfowler.com/bliki/GivenWhenThen.html) are the most succinct and one-liner Jest spec tests are a bit looser. Either works, but you’ll want to ensure you express the pre and post-conditions in your tests.


Here’s an example of a loosely written test case (because it doesn’t express the preconditions *— ie: what if I haven’t created my account yet?* ).


And here’s an example of how we could be more explicit.


- **2 — Ensure you have a testable architecture** . A *feature-driven* architecture helps us think about how to implement features so that they can be tested with integration tests.
- **3 — Learn how to mock out expensive things** . Just like we mock out Apollo Client’s GraphQL requests with the` MockedProvider` , if we’re using other things that make network requests or do things that would slow our tests down, we need to learn how to mock them out.
- **4 — Learn how to assert correct behavior with the various tools within our stack** . Was an auth token added to local storage? Was the` BrowserRouter` called? We need to learn how to mock out certain objects so that we keep our tests independent and so that we can spy on when they’re called, and with what arguments.


Here’s an example of what an integration test might look like:


```text
import { RouterTestUtils } from "../../shared/testing/RouterTestUtils"
import LoginPage from "./LoginPage"
import { MockedProvider } from '@apollo/client/testing'
import { fireEvent } from "@testing-library/dom"
import { LOGIN } from './useLogin'
import { act } from "@testing-library/react"
import { Login, LoginVariables } from "./__generated__/Login"
import { waitForResponse } from "../../shared/testing/WaitForResponse"


describe('Feature: Login', () => {
describe('Scenario: Successful login', () => {
describe('Given I have an account', () => {
describe('When I try to login', () => {
test('Then I should be redirected to the dashboard', async () => {


// Arrange
const mocks = [
{
request: {
query: LOGIN,
variables: {
input: {
email: 'khalil@apollographql.com',
password: "tacos"
}
} as LoginVariables,
},
result: {
data: {
login: {
__typename: 'LoginSuccess',
token: "test-auth-token",
},
} as Login,
},
}
];


const { component, router } = RouterTestUtils.renderWithRouter(
<MockedProvider mocks={mocks} addTypename={true}>
<LoginPage/>
</MockedProvider>
);


const routerSpy = jest.spyOn(router, 'pushState');


// Act
const emailInput = await component.getByPlaceholderText(/email/);
fireEvent.change(emailInput, { target: { value: 'khalil@apollographql.com'}});


const passwordInput = await component.getByPlaceholderText(/password/);
fireEvent.change(passwordInput, { target: { value: 'tacos'}})


const button = await component.findByRole('button');


button.click();


await act(async() => {
await waitForResponse()
})


// Assert
expect(RouterTestUtils
.wasRouteCalled(routerSpy, '/dashboard'))
.toEqual(true);


})
})
})
})
})
﻿
```


I recommend using either Jest + React-Testing Library or Cypress to write and run your integration tests.


### Tradeoffs


Benefits


- Tests cover a large surface area
- They can run just as fast as unit tests


Disadvantages


- Requires more knowledge of how each tool in the stack works under the hood (React rendering, React-Router, Apollo Client, etc) so as to mock them out and spy on functions to assert correctness.


## End-to-end testing


The last type of test is an easier way to test features: end-to-end tests.


### What is an end-to-end test?


An **end-to-end test** is a test that acts like a *real user* using your app and engages in an operation that cuts across the entire stack including the backend application and the database.


This type of test:


- Covers the entire surface area of code involved in implementing a feature
- Runs slower because it relies on rendering, network requests, and real-life infrastructure


### What are we testing?


Again, we’re testing **features** with end-to-end tests. We can use the same methodology for thinking about end-to-end tests as we do when we think about integration tests.


The major difference is in the way that we realize the features. We apply a sort of *white-box* testing in integration tests, meaning that we’re testing the code from the inside out. With E2E testing, we’re applying a form of *black-box* testing, which means that we’re testing the observable output of our code — that is, to act as a user using the app and assert correct behavior *from the outside* .


### How to start end-to-end testing your features


We’ll dive into the specifics with a hands-on blog post, but today — if you want to get started with E2E testing, I highly recommend taking a look at Cypress’ docs on “[Testing Your First App](https://docs.cypress.io/guides/getting-started/testing-your-app) “.


### Tradeoffs


Benefits


- Tests the features of your application
- Gives us confidence that features work from top-to-bottom
- Easier to write than integration tests


Disadvantages


- Slower


## Testing strategies


Overall, the testing strategy that you choose depends on time, your team’s skill level, if you practice TDD, and the general testability of your application.


Integration tests are the clear winner in ROI, but they are certainly the most challenging to write. They strike an outstanding balance between the amount of surface area they cover, speed, and the confidence they can bring you in terms of knowing that your application is running correctly. That comes at the cost of having a better understanding of *white-box* testing strategies.


It’s a great idea to write **unit tests** to test your React components regardless of writing integration or E2E tests. Still, they will likely not give you the amount of confidence you need to know if your application (overall) is working correctly. Consider writing unit tests every time you create a new React component that relies on some data from a GraphQL query.


**End-to-end tests** tend to be the most approachable way to test the features of your application. Your team’s skill level can vary, and because the Cypress folks have abstracted a lot of the white-box complexity away for us, we can focus on writing higher-level tests that give us the most confidence in the *most critical features.*


Suppose you were interested in TDD-ing your application. In that case, I’d recommend using an approach called[Double-Loop TDD](https://khalilstemmler.com/articles/test-driven-development/introduction-to-tdd/#Double-Loop-TDD) . This approach means you start with a high-level feature/acceptance test (can be written as either integration or E2E) and then create unit tests on the fly for all the React components needed to realize the feature.


Finally, if writing tests *afterward* to give you some confidence is more your style, then E2E tests are the way to go.


## Conclusion


In this post, we discussed three different ways to test your Apollo Client applications: unit tests, integration tests, and end-to-end testing. We looked at how they work, what they test against, and we discussed their tradeoffs.


In future blog posts, we’ll demonstrate how to implement each of these types of testing strategies in a real-world Apollo Client application.


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
