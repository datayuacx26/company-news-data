---
schema_version: "1.0.0"
document_id: "ff4a502fe1a53deddd0d80e55a0066c48cdf6f15dc7fe393b95c14e0c0c6fe22"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/building-react-apps-with-graphql-graphcool-apollo-a54215ccd202"
published_at: "2017-06-07T00:01:38+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:5772ff2f5445e81ff22056e11ef399bff162aa2753012018e9ad868aff63871b"
---

# Building React Apps with GraphQL, Graphcool & Apollo

It’s been less than two years since Facebook first open-sourced[GraphQL](http://graphql.org/) as a new way of exposing data from an API. In that short period, it’s already revolutionized the API space and many major companies, like GitHub, Twitter and Yelp, have started using it in production.


In this article, you’ll learn how to quickly get started with React & GraphQL by using[Apollo Client](http://dev.apollodata.com/) and[Graphcool](https://www.graph.cool/) to building a simple Instagram app. You can find the final version of the code on[GitHub](https://github.com/graphcool-examples/react-graphql/tree/master/quickstart-with-apollo) .


To follow along, grab the starter code and prepare yourself for some GraphQL awesomeness:


```text
git clone https://github.com/graphcool-examples/react-graphql.git
cd react-graphql/quickstart-with-apollo
```


## Creating a GraphQL Server


Since GraphQL has only been released as a *specification* , the burden is on the developer to implement their own GraphQL servers if they want to benefit from this new technology. Tools like[Apollo Launchpad](https://launchpad.graphql.com/) or[graphql-up](https://www.graph.cool/graphql-up/) make it easy to spin up GraphQL APIs, but are not intended for production use. In this tutorial you’ll use[Graphcool](https://www.graph.cool/) , a serverless GraphQL platform, to build a production-ready GraphQL backend.


The first thing you need to do is install the[Graphcool CLI](https://www.graph.cool/docs/reference/cli/overview-kie1quohli/) :


```text
npm install -g graphcool
```


The CLI allows you to manage your project locally rather than using the web-based[Console](https://console.graph.cool/) .


Every GraphQL API is based on a[schema](https://www.graph.cool/docs/reference/schema/overview-ahwoh2fohj/) that defines the data model of the application. Here’s what the schema looks like for our Instagram app, expressed in the[GraphQL SDL](https://www.graph.cool/docs/faq/graphql-idl-schema-definition-language-kr84dktnp0/) syntax:


```text
type Post {
description: String!
imageUrl: String!
}
```


### Using the Graphcool CLI to build your Backend


You can now create your GraphQL backend by referring to this schema that’s stored at a[remote url](https://graphqlbin.com/instagram.graphql) :


```text
graphcool init --schema https://graphqlbin.com/instagram.graphql --name Instagram
```


This command produces two endpoints — for the purpose of this tutorial you’ll use the one for the` Simple API` . It also creates a file called` project.graphcool` that contains all the configurations for your GraphQL backend and that enables you to make changes from the comfort zone of your terminal.


If you want to explore the capabilities of your API, you can open up a[GraphQL Playground](https://www.graph.cool/docs/faq/tips-and-tricks-graphql-playground-ook6luephu/) that allows you to send queries and mutations in an interactive manner. Simply type the following command in a terminal:


```text
# in the directory where project.graphcool is located
graphcool playground
```


## Connecting the App with the GraphQL Backend


The app is already completely implemented, so you don’t have to write any code to get started. However, you still need to configure the` ApolloClient` instance with the information about the server you just created.


Open` ./src/index.js` and replace the placeholder` __SIMPLE_API_ENDPOINT__` with the endpoint for your` Simple API` . If you lost the endpoint, you can use the` graphcool endpoints` command in the terminal to see it again.


That’s it, you can now go ahead and use the app already:


```text
yarn install
yarn start
```


Here’s what it looks like:


## Exploring the Code


Let’s understand how the app works by exploring the` ListPage` and` CreatePage` components.


### ` ListPage.js`


This is the initial component of the app and will display all the posts that are currently stored in the backend.


At the bottom of the file, we define the` FeedQuery` that’s responsible for fetching all the posts from the API. With Apollo, it’s super easy to access the response data from a query in a React component. You can use the` graphql` higher-order component to wrap your component with the query:


```text
const FeedQuery = gql`query allPosts {
allPosts(orderBy: createdAt_DESC) {
id
imageUrl
description
}
}`


const ListPageWithData = graphql(FeedQuery)(ListPage)
```


The` ApolloClient` instance will then fetch the data for you behind the scenes and pass it down into the props of your component. In our case, we use the fetched posts in` render` :


```text
{this.props.data.allPosts.map(post => (
<Post
key={post.id}
post={post}
refresh={() => this.props.data.refetch()}
/>
))}
```


Apollo Client has full control over the network request that’s happening when data is fetched. However, it’s a common use case to display a *loading* state on the UI whenever a network request is running.


That’s why Apollo also injects a` loading` property into your component’s props which provides precisely that information. We use it to render a loading state while data is being requested from the server:


```text
if (this.props.data.loading) {
return (
// render loading element
)
}
```


### ` CreatePage.js`


` CreatePage` is the component that we use to create a new post in the backend. Similar to the` FeedQuery` , we define a mutation and then wrap the` CreatePage` component with that mutation using the` graphql` higher-order component again. This makes the mutation available in the props of the component:


```text
const addMutation = gql`
mutation addPost($description: String!, $imageUrl: String!) {
createPost(description: $description, imageUrl: $imageUrl) {
id
description
imageUrl
}
}
`
const PageWithMutation = graphql(addMutation, {name: 'addPost'})(CreatePage)
```


The` name` that we specified refers to the field that gets injected into the props, we can thus invoke the mutation as follows:


```text
const {description, imageUrl} = this.state
await this.props.addPost({variables: {description, imageUrl}})
```


## Bonus: GraphQL Subscriptions


[GraphQL Subscriptions](http://facebook.github.io/graphql/#sec-Subscription) only recently became part of the official GraphQL Spec. It’s a feature that lets you bring realtime functionality to an app by subscribing to specific events that are happening in the backend.


A subscription follows the same syntactical structure as queries and mutations, but you need to use the` subscription` keyword. Here’s an example for a subscription that will be triggered every time a new` Post` is created:


```text
subscription {
Post(filter: {
mutation_in: [CREATED]
}) {
node {
id
description
imageUrl
}
}
}
```


The` node` field contains the information about the newly created` Post` .


Subscriptions also work in GraphQL Playgrounds.


With Apollo Client, it’s easy to use subscriptions! First, you’ll need to add a new dependency to the project:


```text
yarn add subscriptions-transport-ws@0.6.0
```


**Note:** Version 0.6.0 (or lower) is required because later versions are incompatible with the Graphcool Subscriptions API at the moment.


Then you need to adjust the configuration of the` ApolloClient` since it now also needs to be aware of the subscription server’s URL. This is what a typical setup looks like:


```text
import { SubscriptionClient, addGraphQLSubscriptions } from 'subscriptions-transport-ws'
import { ApolloClient, createNetworkInterface } from 'react-apollo'
const networkInterface = createNetworkInterface({
uri: 'https://api.graph.cool/simple/v1/__PROJECT_ID__' // Your GraphQL endpoint
})
// Create WebSocket client
const wsClient = new SubscriptionClient('wss://subscriptions.graph.cool/v1/__PROJECT_ID__', {
reconnect: true
})
const networkInterfaceWithSubscriptions = addGraphQLSubscriptions(
networkInterface,
wsClient
)
const apolloClient = new ApolloClient({
networkInterface: networkInterfaceWithSubscriptions
})
```


With this setup in place, you can now start subscribing to events. For example, you could subscribe to new posts being added in` ListPage` . Since you only want to subscribe *once,* this code can be placed in` componentDidMount` :


```text
const NewPostsSubscription = gql`
subscription {
Post(filter: {
mutation_in: [CREATED]
}) {
node {
id
imageUrl
description
}
}
}`


class ListPage extends React.Component {
// ...
componentDidMount() {
this.subscription = this.props.data.subscribeToMore({
document: NewPostsSubscription,
updateQuery: (previousState, {subscriptionData}) => {
const newPost = subscriptionData.data.Post.node
return {
allPosts: [
{...newPost},
...previousState.allPosts
]
}
},
onError: (err) => console.error(err),
})
}


// ...


}
```


The` updateQuery` function that is passed as an argument works in the same way as a[Redux reducer](http://redux.js.org/docs/basics/Reducers.html) in that it lets you merge the new data into the Apollo store.


## Conclusion


GraphQL is becoming the new standard in the API space. In this tutorial you learned how to get started with React & GraphQL by building a simple Instagram app.


If you want to learn more about what you can do with a Graphcool backend, check out some of the[advanced features](https://www.graph.cool/docs/tutorials/advanced-features-eath7duf7d/) or a quick walkthrough of the[whole platform](https://www.graph.cool/docs/tutorials/graphcool-features-overview-ped6wohw0o/) . Make sure to subscribe to the[GraphQL Weekly](https://graphqlweekly.com/) newsletter to stay up-to-date about everything that’s happening in the GraphQL community!


Written by


Graphcool


[Read more by Graphcool](https://www.apollographql.com/blog/author/graphcool)
