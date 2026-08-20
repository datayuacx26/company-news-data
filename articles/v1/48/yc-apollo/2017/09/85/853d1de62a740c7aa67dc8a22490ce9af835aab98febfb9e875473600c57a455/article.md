---
schema_version: "1.0.0"
document_id: "853d1de62a740c7aa67dc8a22490ce9af835aab98febfb9e875473600c57a455"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-server-side-rendering-works-with-react-apollo-20f31b0c7348"
published_at: "2017-09-15T00:15:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:61174aa9f6232dafe30ae64b2d95c3dc0dcf90d2ac8c2cdd47b3f812190a29ab"
---

# Server Side Rendering with GraphQL

Server side rendering (SSR) is a technique used to speed up load time of JavaScript applications by pre-rendering HTML on the server, rather than waiting for the client to initialize to display the initial markup. I won’t cover server side rendering in detail, but feel free to browse the following articles if you want to learn more:


- [The Benefits of Server Side Rendering Over Client Side Rendering](https://medium.com/walmartlabs/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8)
- [What Exactly is client Side Rendering and Hows it Different from Server Side Rendering?](https://medium.freecodecamp.org/what-exactly-is-client-side-rendering-and-hows-it-different-from-server-side-rendering-bd5c786b340d)


In this post, we’ll walk through how server-side rendering works in` <a href="https://github.com/apollographql/react-apollo" target="_blank" rel="noreferrer noopener">react-apollo</a>` and how we can take advantage of this to start migrating our SSR-enabled applications to GraphQL.


## Two phases for server-side rendering


Because React’s rendering is synchronous, we need to run SSR in two phases:


1. An asynchronous phase that fetches data
2. A synchronous phase to render markup


*In future versions of React this could possibly be condensed into one phase, but that’s a topic for another post.* 😀


Data fetched in Phase 1 must be readily available in Phase 2 in order to render the full application markup. React Apollo achieves this with the help of[apollo-client](https://github.com/apollographql/apollo-client) , which caches fetched data in a normalized format.


## Getting into the details


If you’re using React Apollo already, you probably have a component that looks like this:


```text
class UserImage extends React.Component {
render() {
return (
<div>
<img src={this.props.userImageUrl} />
</div>
)
}
}


export default graphql(
gql`
query UserImage($userId: UserId) {
user(userId: $userId) {
userImageUrl
}
}
`, {
options: ({ userId }) => ({ variables: { userId }})
}
)(UserImage)
```


In fact, you probably have a whole tree of components that look like this. The[Server Side Rendering with React Apollo](http://dev.apollodata.com/react/api-server.html) documentation describes how you can load data for your tree of components through` getDataFromTree` and/or` renderToStringWithData` (which calls getDataFromTree).


But what does` getDataFromTree` actually do under the hood?


## React Apollo walks the component tree to find queries


One of the beautiful things about having data declarations represented as components in the React UI component tree is that we’re able to walk the branches of the tree to figure out precisely what queries your application needs to execute. Let’s say your React Component looks something like this:


A React Component Tree with Data Requirements A and B


The figure above represents what a React UI component tree might look like. The components decorated with the` graphql` higher order component, which require asynchronous server-side data, have dotted lines around them and are labeled with A and B.


The algorithm for crawling the tree (in pseudo-code) is as follows:


```text
define visitChildren (instance):
children = result of calling the `render` function
For each child in children:
visit(child's Component with the expected props and
context)


define visit (Component):
instance = result of running the component's constructor and
componentWillMount
if instance defines a data requirement function:
Initiate resolving that data requirement
When it resolves:
visitChildren(instance)
else
visitChildren(instance)
```


The tree walk is a fairly standard depth first traversal; however, when we meet a component that requires data, we wait until the data requirements are fulfilled before continuing. This is how a tree walk execution might look:


How a tree walk execution might look when making sure all of an application’s data requirements are loaded.


After the tree walk, all of the data requirements will have been fulfilled. A simple call of` React.renderToString` will render the entire UI tree using the data now loaded in Apollo Client’s store.


## Effects on performance


If your GraphQL application requires multiple GraphQL queries to load its data, Apollo Client offers the option to batch your queries into fewer API calls. You can find more information about batching[in a previous blog post](https://dev-blog.apollodata.com/query-batching-in-apollo-63acfd859862) . It’s an easy option to enable, and worth trying in some cases.


However, batching isn’t a silver bullet. Even though it reduces the number of HTTP calls to the API server, it could potentially slow down your application if some of your underlying APIs are slow. Here is a simple example of what I mean:


Given 4 data requirements:


- A that takes on average 20ms
- B, 200ms
- C, 20ms
- D, 100ms


Without batching enabled:


With batching enabled:


Without API Batching, a call for D will trigger immediately after C returns (~40ms into the data load). By turning on API batching, D won’t trigger until the batched call for B and C returns (around 220ms from the beginning)


Usually query batching is the right thing to do, but definitely make sure to instrument your network interface and your queries with timing information so you know how long they take before you decide whether or not your want batching enabled for an application. You can also try[Apollo Optics](https://www.apollodata.com/optics/) to understand more about how your queries perform on the server.


## Incrementally migrating existing SSR applications to GraphQL with React Apollo


So you want to use GraphQL with React Apollo, but your SSR Application uses another method of fetching data (e.g. Promises or Flux Actions). How might you migrate to React Apollo so you can use GraphQL?


One of the interesting things about React Apollo’s tree crawl, is that it doesn’t necessitate that data fetching components are` graphql` higher order components. It only requires that components that specify data requirements implement a` fetchData` instance method returning a Promise.


So, if the data requirements in your app are currently managed with Redux actions, instead of having to rewrite your application, you can change how your data fetches are being triggered.


For instance, a common React-Redux pattern for loading data (using redux-thunk) might look something like this:


```text
function loadUserImageUrlAction(userId) {
return function(dispatch) {
return fetch(USER_IMAGE_API)
.then(
res => res.json(),
err => console.log('An error has occured.', err)
)
.then(json => {
dispatch(receiveUserImage(json));
})
}
}


class UserImage extends React.Component {
constructor() {
dispatch(loadUserImageUrlAction(this.props.userId));
}


render() {
...
}
}
```


To instead, implement the` getDataFromTree` contract, we can move the dispatch into a` fetchData` function.


```text
class UserImage extends React.Component {
constructor() {
// getDataFromTree from react-apollo will call the
// constructor and fetchData automatically so there's
// no need to call it during SSR. This is here
// so if the app is being rendered on the client,
// it will still know to call fetchData.
if (window !== undefined) { this.fetchData() }
}


fetchData() {
// By using redux-thunk, this returns a Promise, which
// fulfills the contractual obligation for `fetchData`
// as expected by the tree walk
return dispatch(loadUserImage(this.props.userId));
}


render() {
...
}
}
```


With this, React Apollo’s` getDataFromTree` , when visiting this component, will call` fetchData` , which will in turn call` dispatch(loadUserImageUrlAction())` , which loads the data and sends it to the redux reducers.


## But … that sounds too good to be true?


It’s not a perfect approach, because there are additional constraints around when data is loaded and used. Since the tree walk only ever executes the lifecycle of each component once, data must be loaded before a node that wants to use it is visited.


Take the following example:


```text
class UserImage extends React.Component {
constructor() {
if (window !== undefined) { this.fetchData() }
}


fetchData() {
return dispatch(loadUserImage(this.props.userId));
}


render() {
<div>
<img src={this.props.userImageUrl} />
</div>
}
}


connect((state) => ({
userImageUrl: state.user.imageUrl
}))(UserImage)
```


When evaluating the` render` ,` userImageUrl` will be` undefined` because the` connect` higher order component is evaluated before the` UserImage` component. When the data resolves, the` connect` component won’t be able to run it’s` mapStateToProps` function again to provide updated props from the updated store state.To get around this, you can move the data loading into a parent container component:


```text
class UserImageLoader extends React.Component {
constructor() {
if (window == undefined) { this.fetchData() }
}


fetchData() {
return dispatch(loadUserImage(this.props.userId));
}


render() {
return this.props.children;
}
}


class UserImage extends React.Component {
render() {
return (
<div>
<img src={this.props.userImageUrl} />
</div>
);
}
}


const ConnectedUserImage = connect((state) => ({
userImageUrl: state.user.imageUrl
}))(UserImage)


<UserImageLoader>
<ConnectedUserImage />
</UserImageLoader>
```


This ensures that the` connect` component is a descendant of the component that fetches the required data.


## Making this all more concise


Better yet, since your “loader” code is very common, you can abstract it out into a higher order component, which would make your resulting code look something like this:


```text
class UserImage extends React.Component {
render() {
return (
<div>
<img src={this.props.userImageUrl} />
</div>
);
}
}


compose(
asyncAction((props, dispatch) => {
return dispatch(loadUserImage(props.userId));
}),
connect((state) => ({
userImageUrl: state.user.imageUrl
})
)(UserImage);
```


The asyncAction higher order component would look something like this:


```text
function asyncAction(dispatchFn) {
return function(Component) {
class AsyncAction extends React.Component {
static displayName = `Action(${Component.displayName})`
fetchData() {
return dispatchFn(props, dispatch);
}


render() {
return <Component {...this.props} />
}
}
hoistNonReactStatics(Component, AsyncActionComponent);


return AsyncActionComponent;
}
}
```


That’s it! Once you move all your Redux actions that load data into` asyncAction` higher order components, and exchange your existing data loading strategy with React Apollo’s, you can start slowly adding GraphQL in your Redux SSR Application!


*Note: The error messages from*` <em>getDataFromTree</em>` *say something about GraphQL failing whenever there is an error thrown in*` <em>fetchData</em>` *. If you do this, that error message could be misleading!*


## Recap


Any sort of server side rendering in React currently requires two phases:


1. Fetching data
2. Rendering the final markup that gets served


The data fetching phase is generally requires more thought because it’s not provided out of the box by React. In React Apollo, data fetching is done by walking your tree and calling just enough of your components’ life cycle events to ensure that all declared data dependencies (via` fetchData` , provided within the` graphql` higher order component) are executed and resolved.


For an incremental migration to React Apollo for your existing React SSR Application that currently uses some other data fetching style (e.g Flux actions backed by fetch), you can take advantage of React Apollo’s flexibility to hook up your Flux data, as long as it implements a` fetchData` function. In order to have less boilerplate code, you can abstract the adaptation out into a higher order component adapter.


## Conclusion


I hope that after reading this post, you can leave with a better understanding of how React Apollo achieves server side rendering. Additionally, if you’ve been thinking about moving to GraphQL and transitioning your SSR apps is a blocker, hopefully you now have a straightforward strategy for doing an incremental migration.


Thanks to my teammates[Jon Wong](https://medium.com/u/d987eade03ed?source=post_page-----20f31b0c7348----------------------) and[Bryan Kane](https://medium.com/u/ed74e2378fd7?source=post_page-----20f31b0c7348----------------------) and my manager[Nick Dellamaggiore](https://medium.com/u/1dcce2efe2ac?source=post_page-----20f31b0c7348----------------------) for providing moral support on the performance projects we have @[Coursera](https://www.coursera.org/) , through which I ultimately learned about all of the content presented above.


Thanks to[James Baxley III](https://medium.com/u/fd3d3dea3cd3?source=post_page-----20f31b0c7348----------------------) ,[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----20f31b0c7348----------------------) ,[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----20f31b0c7348----------------------) ,[Tom Coleman](https://medium.com/u/9ef1d7baf4fe?source=post_page-----20f31b0c7348----------------------) and all the other contributors to React Apollo for building a wonderful package that pretty much works out of the box — and not just for GraphQL applications … *I have to admit, I’ve used React Apollo’s*` <em>getDataFromTree</em>` *in my Non-GraphQL applications to enable SSR too! Whoops? 😝*


Written by


Lewis Chung


[Read more by Lewis Chung](https://www.apollographql.com/blog/author/lewis)
