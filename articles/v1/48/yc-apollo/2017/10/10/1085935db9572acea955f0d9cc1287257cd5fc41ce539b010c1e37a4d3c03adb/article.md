---
schema_version: "1.0.0"
document_id: "1085935db9572acea955f0d9cc1287257cd5fc41ce539b010c1e37a4d3c03adb"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-tote-declarative-patterns-and-a-sneak-peek-into-apollo-2-0-f23bd884943e"
published_at: "2017-10-10T23:51:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:fcd951c814f908afd3de940ded8dadd4774e00743f6ef3ec48f6ddb33868d23c"
---

# Apollo Tote, Declarative Patterns and a Sneak Peek into Apollo 2.0

*This is a guest post from*[Peter Piekarczyk](https://medium.com/u/cff8a83d5fc3?source=post_page-----f23bd884943e----------------------) *, technical co-founder and Apollo / React Native guru at*[Orchard.ai](https://www.orchard.ai/) *.*


[Apollo Tote](https://github.com/peterpme/apollo-tote) is a declarative React component. Take what you’ve been doing inside your “smart” containers and apply it with JSX! It’s meant to be a simple helper library for your Apollo queries until declarative components are released in Apollo 2.


I created this because I don’t believe separate components and containers need to be applied everywhere. There are times where I’d prefer to see everything in one, succinct file.


I came up with the idea for Apollo Tote while working on[Orchard Ai](https://www.orchard.ai/) . We’re utilizing[Expo](https://www.expo.io/) and[Apollo](https://apollodata.com/) to help you stay on top of your relationships & network the same way you do about diet, health and fitness.


At the very minimum, give it a query and a few render props:


```text
<ApolloTote
query={`
user {
imageUrl
}
`}
renderLoading={() => <Avatar.Loading />}
render={result => <Avatar imageUrl={result.user.imageUrl />}
/>
```


Note: Apollo Tote is not meant to be used everywhere. We only support queries for now. That being said, there are places where Apollo Tote shines!


## Use Cases


### Logged In / Logged Out (Authentication / Authorization)


Authentication & Authorization have always been a drag, for whatever reason. You’ll save a user token in localStorage, but you start to realize there’s a few cases you need to handle:


- A token exists but the user no longer does (this sounds so dark IRL)
- A token exists but has expired
- There is no token (this one’s easy!)


```text
<ApolloTote
query={`
query {
user {
id
}
}
`}
test={data => !!(data && data.user && data.user.id)}
handleFail={() => Store.dispatch({ type: 'LOG_OUT' })}
handlePass={() => Store.dispatch({ type: 'LOG_IN' })}
renderError={error => this._renderError(error)}
renderLoading={() => <App.Loading />}
render={() => <App />}
/>
```


` renderError, renderLoading, render` are all Apollo-specific and map directly to { error, loading, data }.


` test, handleFail, handlePass` are additional helper methods you can use to test for something specific.


In this case I’m testing for` user.id` , but you can use it for anything where you might use` branch` in Recompose: anything that needs to pass or fail.


In this case, I’m dispatching` LOG_IN / LOG_OUT` with Redux, but feel free to use an event emitter or call a class method, etc.


### Handling Loading / Error States


I like to load a different component when I’m handling error state. My favorite way of doing that is by exporting a` Loading` component. This way I don’t have to fk around with any of the styling to get the loading state I’d like. It just works!


Apollo Tote makes that super easy for ya by giving you the` renderLoading` prop:


```text
<ApolloTote
query={`
query {
user {
imageUrl
}
}
`}
renderLoading={() => <Avatar.Loading />}
render={value => <Avatar imageUrl={value.user.imageUrl} />}
/>
```


## Final Thoughts


Apollo Tote & render props are a great option when it comes to working with a complex application structure. While Apollo Tote is around today, a version of it will appear in Apollo 2. I’ll keep the API as similar as I can so when its ready, all you’ll have to do is replace the import!


This is an awesome opportunity for the community to help make Apollo 2’s declarative components an awesome experience.


Shoutout to my friends[Kye Hohenberger](https://medium.com/u/93de0780c5e6?source=post_page-----f23bd884943e----------------------) and[Kyle Shevlin](https://medium.com/u/e55117c5994d?source=post_page-----f23bd884943e----------------------) for the feedback. It’s something I always appreciate!


If you want to find out more about Apollo Tote:


- Check out Apollo Tote on[Github](https://github.com/peterpme/apollo-tote) .
- Sign up for the[Orchard Ai](https://www.orchard.ai/) beta for a sneak peek of it in action. 🙂
- Follow me on[Twitter](https://twitter.com/peterpme) &[Github](https://github.com/peterpme) and learn more about what I’m working on!


Written by


Peter Piekarczyk


[Read more by Peter Piekarczyk](https://www.apollographql.com/blog/author/peterpme)
