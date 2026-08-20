---
schema_version: "1.0.0"
document_id: "b0b6e9b87f5148d4e1218afea9204256fdf676694c86a71cbf5640840d2b224f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/5-things-they-dont-want-you-to-know-about-react-apollo-6fab7a7bf22f"
published_at: "2017-06-21T18:41:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:3f710bc02c97a4fefa54450239cfb617c4209374c32edf1a8ab49fc4200ecf34"
---

# 5 things they don’t want you to know about React-Apollo

*This is a guest post from Kurtis Kemple, tech lead on the UI team*[@MLS](https://twitter.com/MLS) *.*


All jokes aside,` <a href="http://dev.apollodata.com/react/" target="_blank" rel="noreferrer noopener">react-apollo</a>` is an amazing tool that allows you to solve some really hard data fetching problems with very minimal code. It integrates very easily with existing and greenfield apps *and* supports Redux integration. If you want to use GraphQL, it’s a tool that I highly recommend investigating.


Over the last few months at[Major League Soccer](https://labs.mlssoccer.com/) , we have been working a lot with GraphQL/Apollo and have discovered some really interesting tricks (and things that aren’t exactly “tricks”, but are still pretty awesome) you can use to solve some of the more common data fetching/handling issues in building highly dynamic applications. Here are our top five, along with code examples showing how they work.


This is not an “intro to react-apollo” example, so if you’re unfamiliar with the project I highly recommend[reading the docs](http://dev.apollodata.com/react/) first!


## 1.Dealing with Pagination


A problem that most developers face is fetching paginated data. React-apollo makes this a pretty trivial thing with the` fetchMore` method off of the` data` prop. This method allows you to repeat the same query with new variables and then alter the original response, adding the new results to the old.


```text
import React, { Component } from 'react';
import { graphql } from 'react-apollo';
import { gql } from 'graphql-tools';


const QUERY = gql`
query MatchesByDate($date: Int) {
matches(date: $date) {
id
minute
period
highlights {
...
}
...
}
}
`;


class MatchList extends Component {
render() {
const ({ data: { loading, errors, matches } }) = this.props;


return (
<div className="matchlist">
{loading && <div className="loader" />}
{errors && <div className="errors">...</div>}


{!loading && !matches && <span className="none">No Matches Found!</span>}


{!loading &&
matches &&
matches.map(match => <div className="match">...</div>)}


{!loading &&
matches &&
<button onClick={this.onFetchMore}>
Fetch More Matches
</button>}
</div>
);
}


onFetchMore = () => {
const ({ data: { matches, fetchMore } }) = this.props;


fetchMore({
variables: { date: matches[matches.length - 1].date },
updateQuery: (previousResult, { fetchMoreResult, queryVariables }) => {
return {
...previousResult,
// Add the new matches data to the end of the old matches data.
matches: [
...previousResult.matches,
...fetchMoreResult.matches,
],
};
},
});
}
}


export default graphql(QUERY)(MatchList);
```


---


## 2. Conditional Data Fetching


There may be times when you only want to fetch data if some requirements are met — for example, one case might be offering more functionality for desktop users. We can accomplish things like this by using the` <a rel="noreferrer noopener" href="http://dev.apollodata.com/react/higher-order-components.html#compose" target="_blank">compose</a>` higher order component that` react-apollo` provides in conjunction with the` skip` config property. The` skip` property allows you to tell` react-apollo` to skip queries based on certain parameters (in most cases based on the props passed to the component).


```text
import React from 'react';
import { compose, graphql } from 'react-apollo';
import { gql } from 'graphql-tools';


import MatchSummary from '...';
import MatchHighlights from '...';


const MATCH_SUMMARY_QUERY = gql`
query MatchSummary($id: Int) {
match(id: $id) {
minute
period
home {
score
}
away {
score
}
}
}
`;


const MATCH_HIGHLIGHTS_QUERY = gql`
query MatchHighlights($id: Int) {
match(id: $id) {
highlights {
...
}
}
}
`;


const Match = ({ media, data: { loading, errors, match } }) =>
<div className="matchlist">
{loading && <div className="loader" />}
{errors && <div className="errors">...</div>}
{!loading && <MatchSummary match={match} />}


{/* if on desktop render highlights */}
{!loading && media === 'desktop' && <MatchHighlights match={match} />}
</div>;


export default compose(
graphql(MATCH_SUMMARY_QUERY),
graphql(MATCH_HIGHLIGHTS_QUERY, {
// skip passes props if given a function
skip: ({ media }) => media !== 'desktop',
}),
)(Match);
```


---


## 3. Batching Requests


Another common problem faced when building client applications is dealing with overly chatty networks. We need to be mindful of how much we talk to services, especially when target audiences may have low connection speeds or limited data plans.


To deal with this problem at MLS, we actually bring in` <a rel="noreferrer noopener" href="http://dev.apollodata.com/core/" target="_blank">apollo-client</a>` directly. I’ve included it in this post because we’re going to create our own network interface to give to` react-apollo` — one that supports batching requests!


```text
import React from 'react';
import { ApolloClient, ApolloProvider } from 'react-apollo';
import { createBatchingNetworkInterface } from 'apollo-client';


import App from '...';


const client = new ApolloClient({
networkInterface: createBatchingNetworkInterface({
uri: '...', // have to provide uri to interface!
batchInterval: 100, // time in ms to throttle queries
}),
});


export default () =>
<ApolloProvider client={client}>
<App />
</ApolloProvider>;
```


---


## 4. Fallback Data Fetching


Sometimes you may want to show a fallback to users when you get an empty response from a particular query. At MLS we faced an issue where we wanted to display a match timeline if there were no highlights available. To solve this we either have to 1) fetch the data for both upfront every time (not ideal), or 2) see if we got a result for the highlights, and if not, request the timeline instead.


```text
import React from 'react';
import { compose, graphql } from 'react-apollo';
import { gql } from 'graphql-tools';


import MatchSummary from '...';
import MatchHighlights from '...';
import MatchTimeline from '...';


const MATCH_SUMMARY_QUERY = gql`
query MatchSummary($date: Int) {
match(date: $date) {
id
minute
period
home {
score
}
away {
score
}
}
}
`;


const MATCH_HIGHLIGHTS_QUERY = gql`
query MatchHighlights($date: Int) {
match(date: $date) {
id
highlights {
...
}
}
}
`;


const MATCH_TIMELINE_QUERY = gql`
query MatchTimeline($date: Int) {
match(date: $date) {
minute
goals {
...
}
bookings {
...
}
}
}
`;


// timeline component to render if no highlights
const Timeline = ({ data: { loading, errors, match } }) =>
<div className="timeline">
{loading && <div className="loader" />}
{errors && <div className="errors">...</div>}


{!loading && <MatchTimeline match={match} />}
</div>;


// connect to graphql, will only be called if this component is rendered
const TimelineWithData = graphql(MATCH_TIMELINE_QUERY)(Timeline);


const Match = ({ data: { loading, errors, match } }) =>
<div className="match">
{loading && <div className="loader" />}
{errors && <div className="errors">...</div>}


{!loading && <MatchSummary match={match} />}


{/* if we have highlights, render them, never render timeline */}
{!loading && match.highlights && <MatchHiglights match={match} />}


{
/*
* if we don't have highlights
* render timeline instead and fetch timeline data
*/
}
{!loading && !match.highlights && <TimelineWithData />}
</div>;


export default compose(
graphql(MATCH_SUMMARY_QUERY),
graphql(MATCH_HIGHLIGHTS_QUERY),
)(Match);
```


---


## 5. Polling


At MLS we decided to move away from real-time push in favor of polling for a number of reasons, but a big driving factor was moving to GraphQL. Once we saw how easy it was to fine-tune data fetching in` react-apollo` via the` pollInterval` option on the config object, we knew we would easily be able to achieve that real-time feel that is necessary for our industry.


```text
import React from 'react';
import { graphql } from 'react-apollo';
import { gql } from 'graphql-tools';


import MatchSummary from '...';


const MATCH_SUMMARY_QUERY = gql`
query MatchSummary($date: Int) {
match(date: $date) {
id
minute
period
home {
score
}
away {
score
}
highlights {...}
goals {...}
bookings {...}
}
}
`;


const Match = ({ data: { loading, errors, match } }) =>
<div className="match">
{loading && <div className="loader" />}
{errors && <div className="errors">...</div>}


{!loading && <MatchSummary match={match} />}
</div>;


export default graphql(MATCH_SUMMARY_QUERY, {
// options passes props if given a function
options: props => ({ pollInterval: props.pollInterval }),
})(Match);
```


---


If you’re curious about how we are implementing GraphQL on the backend, check out my other[post](https://labs.mlssoccer.com/implementing-graphql-at-major-league-soccer-ff0a002b20ca) on the MLS engineering blog. If you want to know more about the benefits we saw from moving to GraphQL, take a look at my co-worker[Peggy Rayzis](https://medium.com/u/c827782c6410?source=post_page-----6fab7a7bf22f----------------------) ’s[post](https://dev-blog.apollodata.com/reducing-our-redux-code-with-react-apollo-5091b9de9c2a) on the subject!


If you have any questions about our experience, please feel free to leave a comment or reach out to me on the[Twitter](https://twitter.com/kurtiskemple) !


Written by


Kurt Kemple


[Read more by Kurt Kemple](https://www.apollographql.com/blog/author/kurtkemple)
