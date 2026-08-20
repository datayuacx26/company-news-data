---
schema_version: "1.0.0"
document_id: "ea1df4ed4d080b69f2c707d49ada60fd8d67e74cd187f7ea92a3ce19be49dd83"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/query-components-with-apollo-ec603188c157"
published_at: "2017-12-13T22:42:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:ad828162eb6e39a51373d7052e0a6dab1dde8654d103a26ca00d25f4b6792581"
---

# Query Components with Apollo

A couple of months ago, we started to use Apollo Client (+React Native) at[Werkspot](https://werkspot.nl/) . From the very beginning, Apollo overwhelmed us by the simplicity and flexibility it provides. It inspired us to create our own vision about technology and experiment with various approaches to data fetching that we think are worth sharing.


Enjoy the ride! 🚀


Apollo + Query Components hype rocket!


## Anatomy of Query Components


One of the very powerful concepts that we found for ourselves with Apollo is Query Components. These components are meant to express a domain-specific data logic in a clear, declarative way:


```text
const QUERY = gql`
query allCities($country: String!) {
allCities {
id
name
}
}
`;


class CitiesQuery extends Component {
render() {
return this.props.children(this.props.data);
}
}


export default graphql(QUERY, {
options: ({ country }) => ({ variables: { country } }),
})(CitiesQuery);
```


` CitiesQuery` component is an oversimplified example of an average Query Component. It exports a` Component` , enhanced by a` graphql` wrapper for fetching data. After Apollo takes care of fetching and mapping props to query parameters, query component uses “function-as-a-child” approach to expose a child-independent API with injected data.


Comparing to the standard approach where you add a` graphql` wrapper to your higher-order components, query components have a set of advantages:


- **Testability** . Unless you have a very good reason, you can test your components without any data management logic.
- **Separation of concerns** . From now on, higher-order components are no longer concerned about the way data is fetched/pushed to the server.
- **Abstraction over data management layer** . Frankly speaking, it doesn’t really matter which library you use under the hood. Query Components abstracts out your data management layer so you can focus on business logic and change underlying implementation if needed.


Once a Query Component is defined, it can be used as any other component in your application:


```text
class MapPicker extends PureComponent {
render() {
return (
// ...
<CitiesQuery country="Netherlands">
{({ allCities }) => <CitiesList cities={allCities} />}
</CitiesQuery>
// ...
);
}
}
```


A nice side effect of decoupling higher-order components into Query Components is minimizing bloat. If the data management layer grows, it won’t affect presenter components unless they have to reflect new data properties.


## Mutations with Query Components


Mutations are not that straightforward as queries. When a query component is mounted, Apollo immediately tries to fetch. However, when we talk about mutations, there is no need (in 99% of our cases) to perform any mutations at the mount time. This idea brings us to the point that we should have a way to control a moment when we execute our mutations. For example:


```text
const MUTATION = gql`
mutation (name: String!) {
createProposal(input: $input) { clientMutationId }
}
`;


class UserMutation extends Component {
constructor(props, ctx) {
super(props, ctx);


this.execute = () => this.executeMutation();
}


executeMutation() {
this.props.mutate({
variables: {
name: this.props.name
},
});
}


render() {
return this.props.children(this.execute);
}
}


export default graphql(MUTATION)(UserMutation);
```


Therefore, you can use this component in a similar way:


```text
class Profile extends PureComponent {
render() {
return (
// ...
<UserMutation name="Alexey">
{execute => <Button onPress={execute} />}
</UserMutation>
// ...
);
}
}
```


So the concept is the same: Query Component receives a bunch of props that are later on used as parameters for the mutation query.


Testing Query Components like a pro 😎


## How to test Query Components?


When it comes to testing, Apollo-based components have a bunch of features that we need to take into account. The most complicated one is related to React context and Apollo Client reference in particular. All components that intent to use Apollo should be put into the subtree of` <ApolloProvider />` which gives them a reference to the client through the React context. Hopefully, if we use Enzyme and shallow rendering, we can supply context object as one of the options.


### Assertions


One of the very first things we would like to test is that every Query Component is able to produce variables, required by a query. To ensure that, we check a shape of resulting variables.


Another assertion that we would like to make is about response shape. In other words, we check if given` data.x` complies with the shape of the mock that was passed to the fake client during initialization.


And the last, but not least: query component should call a function that we pass as a child with the data pulled from Apollo. Altogether, the test suit looks like this:


```text
describe('CitiesQuery component', () => {
it('renders children with a query result', () => {
const spy = jest.fn(() => null);
const config = {
context: {
client: makeFakeClient({
resolveWith: {
allCities: allCitiesFixture,
},
}),
},
};


shallow(<CitiesQuery country="Netherlands">{spy}</CitiesQuery>, config).render();


expect(spy).toHaveBeenCalled();
expect(spy.mock.calls[0][0].variables.country).toEqual('Netherlands');
expect(spy.mock.calls[0][0].allCities).toMatchObject(allCitiesFixture);
});
});
```


The very last uncovered function is` makeFakeClient` . It is a self-written helper that generates an Apollo Client with a fake link:


```text
import { ApolloLink, Observable } from 'apollo-link';
import { InMemoryCache } from 'apollo-cache-inmemory';
import ApolloClient from 'apollo-client';


function createMockLink(options) {
return new ApolloLink(() =>
new Observable((observer) => {
if (!options.fail) {
observer.next({
data: options.resolveWith,
});
observer.complete();
} else {
observer.error(options.failWith);
}
}),
);
}


export default options => new ApolloClient({
link: createMockLink(options).request,
cache: new InMemoryCache(),
});
```


This should be more or less it. I didn’t cover mutation components testing, but it isn’t anyhow different. The only thing I can foresee is an additional assertion that` execute` handler triggers Apollo fetcher, but I’ll leave it to the reader.


## Where to go from here


- [A great blog post from Khan Academy about their approach writing Query and Mutation components](http://engineering.khanacademy.org/posts/creating-query-components-with-apollo.htm)


---


Don’t hesitate to ask questions, challenge this approach and share your ideas about Apollo and GraphQL overall. You can always find me on[Twitter](https://twitter.com/kureevalexey) or Discord (` kureev` on` #reactiflux` ).


Oh yeah, and last but not least: if you like this or any other of my articles, don’t hesitate to[become a part of it](https://www.patreon.com/kureevalexey) ! Your support is greatly appreciated!


Written by


Kureev Alexey


[Read more by Kureev Alexey](https://www.apollographql.com/blog/author/kureev)
