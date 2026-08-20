---
schema_version: "1.0.0"
document_id: "25e2b370248b8f172ca7a33b28a8b79f04e0ed43a49e0e815294dbcd76f0ca8c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/a-first-look-at-async-react-apollo"
published_at: "2018-03-01T05:04:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:d8610207d2a1dea93834ca9fec6202b290e6c76ed47d5690b11c78cf11c6c774"
---

# A first look at Async React + Apollo

Like many members of the React community, the Apollo team eagerly woke up at 5:00 AM to catch[Dan Abramov](https://medium.com/u/a3a8af6addc1?source=post_page-----10a82907b48e----------------------) ’s talk on the future of React at[JSConf Iceland](https://www.facebook.com/react/videos/1552821821462886/) . With big mugs of coffee in hand, we glued ourselves to our laptops and watched as Dan explained how React async rendering would allow us to adapt our applications to our users’ many devices and networks.


Showcasing two outstanding demos, Dan illustrated some of async React’s new features such as **time slicing** and **suspense** . We couldn’t wait to get our hands dirty with these new features, so we started experimenting with what it would take to make Apollo Client async-ready. Thanks to[James Baxley III](https://medium.com/u/fd3d3dea3cd3?source=post_page-----10a82907b48e----------------------) , we shipped a prerelease of React Apollo 3.0 that’s 100% strict mode compliant, which is essential in order for your library to be async compatible.


Read on to learn how we made React Apollo async compatible and built an[Andrew Clark](https://medium.com/u/6025bd347b9a?source=post_page-----10a82907b48e----------------------) inspired demo with GraphQL and Apollo! 🚀


## The suspense was killing us


Optimizing fetching GraphQL data in your React app is something we’re constantly working on improving. That’s why we were thrilled to learn about React’s latest feature, suspense! Suspense lets you pause rendering a component while it’s loading async data. Here’s a step-by-step breakdown from Andrew about how it works:


Here's how suspending works: – in the render method, read a value from the cache. – if the value is already cached, the render continues like normal – if the value is not already cached, the cache *throws a promise* – when the promise resolves, React retries where it left off


— Andrew Clark (@acdlite)[March 1, 2018](https://twitter.com/acdlite/status/969171217356746752?ref_src=twsrc%5Etfw)


Let’s look at how Dan implemented data fetching with suspense in the I/O demo from his talk:


Fetching data in render! 😮


Under the hood, the` createFetcher` function uses React’s new cache implementation called` <a href="https://github.com/facebook/react/tree/master/packages/simple-cache-provider" target="_blank" rel="noreferrer noopener">simple-cache-provider</a>` in order to suspend the request from within the render method. With React Apollo, we can mimic this functionality by suspending the result of our GraphQL query.


To turn on this feature in our` Query` component, it’s as easy as passing a new` asyncMode` prop. No other changes necessary!


```text
const MOVIE_QUERY = gql`
query GetMovie($id: Int!) {
movie(id: $id) {
id
title
overview
poster_path
}
}
`;
function MovieInfo({ movie, clearActiveResult }) {
return (
<Query asyncMode query={MOVIE_QUERY} variables={{ id: movie.id }}>
{({ data: { movie } }) => (
<Fragment>
<FullPoster movie={movie} />
<h2>{movie.title}</h2>
<div>{movie.overview}</div>
</Fragment>
)}
</Query>
);
}
```


How did we implement` asyncMode` ? The snippet below is from our` getQueryResult` function inside React Apollo, which is called in the` Query` component’s render method. If we’re in async mode and we’re loading, which means the request isn’t found in Apollo’s cache, we **throw** the promise that will eventually resolve to the result of our query.


Throw promises in the air like you just don’t care! 🙌


Why are we throwing a promise here? 🤔 While this pattern might feel out of your comfort zone the first time you try it, it’s what allows React to suspend rendering until the promise is resolved. This unlocks the ability to handle loading state in a more thoughtful way by bubbling up the pending promise to a parent placeholder component. If the request time exceeds a threshold, the placeholder component can render a fallback UI until the request completes. This feature works similarly to[error boundaries](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html) created with` componentDidCatch` .


Suspense is also what allows React to pause low-priority updates like data fetching while continuing to render high-priority updates like entering text into a search input. Once the promise eventually resolves with our GraphQL result, React will continue rendering where it left off. You can think of pausing and resuming rendering as creating a branch and rebasing that branch onto master, as Dan eloquently explained in his talk.


## Tying it all together


Once we shipped a prerelease of async React Apollo, we couldn’t wait to give it a spin in an example app. Luckily, wrapping REST endpoints with a[GraphQL server](https://github.com/apollographql/movie-database-graphql) is our specialty, and with the power of GraphQL-specific caching built into[Apollo Engine](https://www.apollographql.com/engine) , the app is blazing fast!


If you’d like to skip to the code, you can check out our example below on CodeSandbox. Warning: the React API that powers deferred state updates is unstable, so proceed with caution!


Aside from setting the` asyncMode` flag on your` Query` component, you’ll notice not much has changed with how you build Apollo apps in an async React world vs. how you build them today. We want to help you take advantage of all the cool features async React has to offer without making you rewrite your entire application. Our goal is to bring you the best developer experience possible by empowering you to easily build performant, fluid applications with React and GraphQL.


## What’s next 🚀


Our work isn’t done yet. We’re just scratching the surface of all the creative ways async rendering will enable us to build better user experiences with React and GraphQL.


One new feature we’re super excited to explore further is the` <a href="https://github.com/facebook/react/tree/master/packages/simple-cache-provider" target="_blank" rel="noreferrer noopener">simple-cache-provider</a>` . We think it will inspire new ways of thinking about the Apollo Client cache and future developments to help improve performance. We’re especially interested in testing out the` <a href="https://github.com/facebook/react/tree/master/packages/simple-cache-provider" target="_blank" rel="noreferrer noopener">simple-cache-provider</a>` low-level APIs that Dan mentioned in his talk. It’s possible that we could eventually hook into these APIs to normalize your GraphQL result before React’s commit phase.


If you’d like to give async React Apollo a spin,` npm i react-apollo@canary` and make sure to show us what you’ve built. The API is highly likely to change, especially as the React team starts to refine async APIs, so please don’t use it in production. That being said, we’d love to hear what you think and can’t wait to see how React Apollo 3.0 evolves.


---


A special thanks to the React team for their feedback on our post and for giving us early access to some of the new features! 👏 We look forward to collaborating further to ensure that all Apollo apps can fully benefit from all of async React’s exciting new features. If you are interested in helping us integrate Apollo with async React, come join us on Apollo Slack!


Written by


Peggy Rayzis


[Read more by Peggy Rayzis](https://www.apollographql.com/blog/author/peggy)
