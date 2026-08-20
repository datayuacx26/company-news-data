---
schema_version: "1.0.0"
document_id: "49d9c6fa4e3aad1ec6b1594124585bcaddbf9416cea7c44a75ee5c58a73056f2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/reducing-our-redux-code-with-react-apollo"
published_at: "2017-06-09T23:58:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:4458f07952ff3b6aa34ab8a1f2296a3f59bace353174b953c558f205577749c6"
---

# Reducing our Redux code with React Apollo

I’m a firm believer that the best code is no code. More code often leads to more bugs and more time spent maintaining it. At Major League Soccer, we’re a very small team, so we take this principle to heart. We try to optimize where we can, either through maximizing code reuse or lightening our maintenance burden.


In this article, you’ll learn how we offloaded data fetching management to Apollo, which allowed us to delete nearly **5,000 lines of code** . Not only is our application a lot slimmer since switching to Apollo, it’s also more **declarative** since our components only request the data that they need.


What do I mean by declarative and why is that a good thing? Declarative programming focuses on the end goal, while imperative programming focuses on the steps it took to get there. React itself is declarative.


## Fetching data with Redux


Let’s look at a simple Article component:


```text
import React from 'react';
import { View, Text } from 'react-native';


export default ({ title, body }) => (
<View>
<Text>{title}</Text>
<Text>{body}</Text>
</View>
);
```


Now, let’s say we want to render` <Article/>` in a connected` <MatchDetail/>` view, which takes a match ID as a prop. If we were accomplishing this without a GraphQL client, our process to retrieve the data necessary to render` <Article/>` might look something like this:


1. When` <MatchDetail/>` mounts, invoke action creator to fetch match by ID. Action creator dispatches action to tell Redux we’re fetching.
2. We hit an endpoint and receive data back. We normalize the data into the structure we need.
3. Once the data is in the structure we need, we dispatch an action to tell Redux we’re done fetching.
4. Redux processes the action in our reducer and updates our state tree
5. ` <MatchDetail/>` receives all of the match data via props and filters it down to render the article.


That’s a lot of steps to get the data for` <Article/>` ! Without a GraphQL client, our code is much more *imperative* because we have to focus on *how* we’re retrieving our data. What if we don’t want to pull down all of the match data just to render` <Article/>` ? You could build another endpoint and create a separate set of action creators for hitting it, but that can quickly become unmaintainable.


Let’s contrast with how you could approach this with GraphQL:


1. ` <MatchDetail/>` is connected with a higher order component that fetches the following query:


```text
query Article($id: Float!) {
match(id: $id) {
article {
title
body
}
}
}
```


…and that’s it! Once the client receives the data, it will map it to props that can be passed down to` <Article/>` . Much more *declarative* , because we’re only focusing on *what* data we need to render the component.


This is the beauty of delegating data fetching to a GraphQL client, whether it’s Relay or Apollo. When you start “[thinking in GraphQL](https://facebook.github.io/relay/docs/thinking-in-graphql.html) ,” you become more concerned with *what* props your component needs to render and less concerned with *how* you’re going to get them.


At some point, you will need to take care of the “how,” but this concern is now server-side and the complexity is drastically reduced. If you’re new to GraphQL server architecture, make sure you check out` <a href="http://dev.apollodata.com/tools/graphql-tools/index.html" target="_blank" rel="noreferrer noopener">graphql-tools</a>` , Apollo’s library that helps you structure your schema in a modular way. For brevity’s sake, we will be focusing on the front-end today.


Although this post will teach you how to reduce your Redux code, you won’t be getting rid of it entirely! Apollo uses Redux under the hood, so you still get the benefit of immutability and all of your favorite Redux Dev Tools features like time-travel debugging. During[setup](http://dev.apollodata.com/react/redux.html) , you can hook Apollo into your existing Redux store to maintain one source of truth. Once your store is configured, you pass it into a` <ApolloProvider/>` component that wraps your application. Sound familiar? This component replaces your existing` <Provider />` from Redux, except you also need to pass down your` ApolloClient` instance through the client prop.


Before we start slicing up our Redux code, I want to call out one of the best features of GraphQL: **incremental adoption** . You don’t have to commit to refactoring your entire application at once. Since Apollo integrates into your existing Redux store, you can switch over your reducers incrementally. The same thing applies to the backend — if you’re working on a large scale application, you can use GraphQL side by side with your existing REST endpoints until you’re ready to convert them. Fair warning: once you try it, you will fall in love with it and want to refactor your entire application. 😉


---


## Our requirements


Prior to switching from Redux, we thought carefully about whether Apollo would meet our needs. Here’s what we looked at when making our decision:


- **Aggregating data from multiple sources:** A match is comprised of data from 4 different sources: content from our REST API, stats from our MySQL database, media from our video API, and social data from our Redis store. Originally, we were using a server plugin to gather all of the data into one match object to send to the client. It almost functioned just like a GraphQL layer! As soon as we realized this, it became apparent that our application would be a perfect candidate for GraphQL.
- **Near realtime updates:** During a live match, we typically receive updates every minute. Before Apollo, we were handling live updates with sockets and dispatching them to our match reducer. This wasn’t a terrible solution, but it wasn’t the most elegant as we were sending down an entire match object to avoid complicated sequencing. With Apollo, we can easily customize a polling interval per component depending on the game’s status.
- **Simple pagination:** Since we were building out a schedule page with an infinitely scrolling list of matches, we needed a way to handle pagination that wasn’t headache inducing. Sure, we could have built a custom reducer. But why write it ourselves when Apollo’s` fetchMore` function does all the heavy lifting for us? 💪


Not only did Apollo satisfy our current requirements, it also covered some of our future needs, especially since enhanced personalization is on our roadmap. While our server is currently “read only,” we may need to introduce **mutations** in the future to save a user’s favorite team. If we decide to add realtime commenting or fan interaction that can’t be solved with polling, Apollo supports **subscriptions** .


## From Redux to Apollo 🚀


The moment you’ve been waiting for! Originally, when I thought of writing this post, I was going to show before/after code samples, but I think it’s difficult to directly compare the two approaches, especially if you’re new to Apollo. Instead, I’m going to quantify what we deleted entirely and walk you through familiar concepts in Redux that you can apply to building your container components in Apollo.


### What we deleted


- Matches reducer (~300 lines of code)
- Data fetching action creators & epics (~800 LOC)
- Action creators & business logic for batching & receiving socket updates for live matches (~750 LOC)
- Local storage action creators & epics (~1000 LOC). This is a bit unfair to count in the total because offline support for our project is postponed, but it’s achievable if we want to add it back in by customizing the Apollo` fetchPolicy` & exposing the reducer to` redux-persist` .
- Redux container components that separated our Redux logic from our presentational components (~1000 LOC)
- Tests associated with all of the above (~1000 LOC)


### connect() → graphql()


If you know how to use` connect` , then Apollo’s` graphql` higher order component will seem very familiar! Just like` connect` returns a function that takes a component and connects it to your Redux store,` graphql` returns a function that takes a component and “connects” it to Apollo Client. Let’s see it in action!


```text
import React, { Component } from 'react';
import { graphql } from 'react-apollo';
import { MatchSummary, NoDataSummary } from '@mls-digital/react-components';
import MatchSummaryQuery from './match-summary.graphql';


// here we're using the graphql HOC as a decorator, but you can use it as a function too!
@graphql(MatchSummaryQuery, {
options: ({ id, season, shouldPoll }) => {
return {
variables: {
id,
season,
},
pollInterval: shouldPoll ? 1000 * 60 : undefined,
};
};
})
class MatchSummaryContainer extends Component {
render() {
const { data: { loading, match } } = this.props;


if (loading && !match) {
return <NoDataSummary />;
}


return <MatchSummary {...match} />;
}
}


export default MatchSummaryContainer;
```


The first argument supplied to` graphql` is our` MatchSummaryQuery` . This is the data we want to receive back from our server. We’re using a[Webpack loader](https://github.com/apollographql/graphql-tag) to parse our query into the GraphQL AST, but if you’re not using Webpack, you will need to wrap your query in a template string and pass it into the` <a rel="noreferrer noopener" href="http://dev.apollodata.com/react/api.html#gql" target="_blank">gql</a>` function exported from Apollo. Here’s an example of the query to fetch the data needed for this component:


```text
query MatchSummary($id: String!, $season: String) {
match(id: $id) {
stats {
scores {
home {
score
isWinner: is_winner
}
away {
score
isWinner: is_winner
}
}
}
home {
id: opta_id
record(season: $season)
}
away {
id: opta_id
record(season: $season)
}
}
}
```


Great, we have our query! In order for this query to execute, we’re going to need to supply it with two variables,` $id` and` $season` . Where are we going to get those variables from? 🤔 That’s where the second argument to` graphql` comes in, our config object.


The config object has several properties that you can specify to customize the behavior of your HOC. One of the most important is the` options` property, which takes a function that receives your container component’s props. This function returns an object with properties like` variables` , which supplies your variables to your query, and` pollInterval` , which allows you to customize your component’s polling behavior. Notice how we’re using our container component’s props to supply` id` and` season` to our` MatchSummaryQuery` . If this function gets too long to write it in the decorator, we’ll break it out into its own function called` mapPropsToOptions` .


### mapStateToProps() → mapResultsToProps()


In your Redux containers, you probably wrote a function called` mapStateToProps` that you passed to` connect` in order to map data from your state tree into props to pass down to the component. Apollo allows you to define a similar function. Remember the config object from earlier that we passed into our` graphql` function? The config object also has another property,` props` , that receives a function that takes props and maps them before passing them down to your container. You can define it inline if you want, but we like to call ours` mapResultsToProps` .


Why would you want to map your props? Your results from your GraphQL query will be attached to the` data` prop. Sometimes, you might need to flatten this data before passing it to your component. Here’s an example:


```text
import React, { Component } from 'react';
import { graphql } from 'react-apollo';
import { MatchSummary, NoDataSummary } from '@mls-digital/react-components';
import MatchSummaryQuery from './match-summary.graphql';


const mapResultsToProps = ({ data }) => {
if (!data.match)
return {
loading: data.loading,
};


const { stats, home, away } = data.match;


return {
loading: data.loading,
home: {
...home,
results: stats.scores.home,
},
away: {
...away,
results: stats.scores.away,
},
};
};


const mapPropsToOptions = ({ id, season, shouldPoll }) => {
return {
variables: {
id,
season,
},
pollInterval: shouldPoll ? 1000 * 60 : undefined,
};
};


@graphql(MatchSummaryQuery, {
props: mapResultsToProps,
options: mapPropsToOptions,
})
class MatchSummaryContainer extends Component {
render() {
const { loading, ...matchSummaryProps } = this.props;


if (loading && !matchSummaryProps.home) {
return <NoDataSummary />;
}


return <MatchSummary {...matchSummaryProps} />;
}
}


export default MatchSummaryContainer;
```


Not only does the data object contain your query results, it also contains properties like` data.loading` to let you know if your query still hasn’t returned a response. This can be useful if you want to display another component to your user, like we’re doing with` <NoDataSummary />` .


### compose()


Compose isn’t a function that’s unique to Redux, but I do want to point out that Apollo includes it for your convenience. This is super useful if you want to compose several` graphql` functions to be used by one container. You can even use` compose` with Redux` connect` and` graphql` together! Here’s how we use` compose` to display different match tile states:


```text
import React, { Component } from 'react';
import { compose, graphql } from 'react-apollo';
import { NoDataExtension } from '@mls-digital/react-components';
import PostGameExtension from './post-game';
import PreGameExtension from './pre-game';
import PostGameQuery from './post-game.graphql';
import PreGameQuery from './pre-game.graphql';


@compose(
graphql(PreGameQuery, {
skip: ({ gameStatus }) => gameStatus !== 'pre',
props: ({ data }) => ({
preGameLoading: data.loading,
preGameProps: data.match,
}),
}),
graphql(PostGameQuery, {
skip: ({ gameStatus }) => gameStatus !== 'post',
props: ({ data }) => ({
postGameLoading: data.loading,
postGameProps: data.match,
}),
}),
)
export default class MatchExtensionContainer extends Component {
render() {
const {
preGameLoading,
postGameLoading,
gameStatus,
preGameProps,
postGameProps,
...rest
} = this.props;


if (preGameLoading || postGameLoading)
return <NoDataExtension gameStatus={gameStatus} />;


return gameStatus === 'post'
? <PostGameExtension {...postGameProps} {...rest} />;
: <PreGameExtension {...preGameProps} {...rest} />;
}
}
```


` compose` is great for when your container has multiple states. What if you need to execute a different query depending on its state? That’s where` skip` comes in, as you can see above on the config object. The` skip` property takes a function that receives props and allows you to skip the query if it doesn’t meet the criteria you specify.` compose` +` skip` = 😍


All of these examples demonstrate that if you know Redux, you’ll pick up Apollo quickly! Its API draws upon many Redux concepts while reducing the code you need to write to achieve the same results.


---


I hope learning about Major League Soccer’s experience switching to Apollo was helpful! As with any library decision, the best solution to manage your application’s data fetching will depend on your project’s requirements. If you have any questions about our experience, please feel free to leave a comment or reach out to me on[Twitter](https://twitter.com/peggyrayzis) ! 🙋


Written by


Peggy Rayzis


[Read more by Peggy Rayzis](https://www.apollographql.com/blog/author/peggy)
