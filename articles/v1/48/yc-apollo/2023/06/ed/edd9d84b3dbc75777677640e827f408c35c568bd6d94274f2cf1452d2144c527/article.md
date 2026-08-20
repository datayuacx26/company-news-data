---
schema_version: "1.0.0"
document_id: "edd9d84b3dbc75777677640e827f408c35c568bd6d94274f2cf1452d2144c527"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-apollo-clients-nonreactive-directive-and-usefragment-hook"
published_at: "2023-06-15T08:53:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:df7a8bdc3386534f2422c75d83c5c1f54b34800bd47909f81242a04d33e16d92"
---

# Don’t Overreact! Introducing Apollo Client’s new @nonreactive directive and useFragment hook

Efficient React apps are selective in their re-rendering, using a variety of techniques (such as[memo](https://react.dev/reference/react/memo) ) to *skip* re-rendering unchanged components. With the beta release of Apollo Client 3.8, we’re excited to spotlight a couple of *new* techniques to help you re-render exactly the components that you need to:


- **The[@nonreactive directive](https://www.apollographql.com/docs/react/data/directives/#nonreactive) :** mark GraphQL query fields and fragments that *shouldn’t* trigger a re-render if their cached value changes.
- **The**[useFragment hook](https://www.apollographql.com/docs/react/api/react/hooks-experimental/#usefragment) **:** create a live binding into the Apollo Client cache for a GraphQL fragment. This allows Apollo Client to broadcast specific fragment results to individual React components so child components can re-render *without* needing to re-render the parent.


Let’s look at how we can use these (both individually and in combination) to keep our app snappy!


## (Re-)Rendering Lists


You might have found yourself reaching for` memo` while building an app with Apollo Client that renders a **list** . Consider the following example:


```text
const UserFragment = gql`
fragment UserFragment on User {
name
}
`;


const ALL_USERS = gql`
query AllUsers {
users {
id
...UserFragment
}
}
`;


function UserComponent({ user }) {
return <li>{user.name}</li>;
};


function UserList() {
const { data } = useQuery(ALL_USERS);
return (
<ul>
{data?.users.map((user) => (
<UserComponent key={user.id} user={user} />
))}
</ul>
);
};


const client = new ApolloClient({
link,
cache: new InMemoryCache({
fragments: createFragmentRegistry(gql`
${UserFragment}
`),
}),
});


const container = document.getElementById("root");
const root = createRoot(container);


root.render(
<ApolloProvider client={client}>
<UserList />
</ApolloProvider>
);
```


By registering our` UserFragment` with Apollo Client’s` InMemoryCache` via` <a href="https://www.apollographql.com/docs/react/data/fragments/#registering-named-fragments-using-createfragmentregistry">createFragmentRegistry</a>` , we can reference it by name inside our queries (as we do here with the` ...UserFragment` spread in` ALL_USERS` ) without interpolating it directly.


In this example, the` UserList` component fetches a list of users with Apollo Client’s` useQuery` hook, then maps over the result to render a` UserComponent` for each user in the list.


But what happens when a user record is updated in the Apollo Client cache? Because these cached fields are watched by our` useQuery` invocation, the hook re-runs. This re-renders` UserList` with the latest values, which in turn recursively re-renders every child component ( *by default* ). In other words, if a *single user* in our list is updated, the vast majority of our` UserComponent` s will re-render even though they haven’t changed!


These unnecessary renders often go unnoticed by end users (and sometimes developers), but recursively re-running application and library code can add up to cause noticeable dips in performance.


## To` memo` or not to` memo` ?


The React docs provide excellent guidance for this question in the deep dive section[“Should you add memo everywhere?”](https://react.dev/reference/react/memo#should-you-add-memo-everywhere) Like many optimizations,` memo` has a cost: the additional step of comparing old and new props on every render. Although that cost is often outweighed by the benefits, it’s still a cost you should consider.


I encourage you to read that section in its entirety, but here are two criteria from it that can help us determine whether we should optimize with` memo` (emphasis mine):


- Does our component **re-render often** with **the same exact props** ?
- And is its **re-rendering logic expensive,** causing **perceptible lag** when it re-renders?


Let’s take a look at an example that extends the` UserList` code above.


When editing a single user and writing to Apollo Client’s` InMemoryCache` on every keystroke, we see that the parent component re-renders, along with *all* of the 2,000` UserComponents` in our list. Typing in the input feels sluggish 🐌


Of course, this is a contrived example: there are other ways we might write this code in a real app to work around the problem: using[pagination](https://www.apollographql.com/docs/react/pagination/overview/) , debouncing cache writes so we aren’t updating the user list on every keystroke, or writing to the cache only after the user presses “Done editing”.


But there are reasons we shouldn’t be satisfied with these “fixes”. First, although rendering fewer users and/or performing fewer cache writes would improve perceived performance, we’d still be re-rendering **parent** and **child** components unnecessarily every time a user updates. Reducing the number of renders is a good place to start, but the dynamic nature of lists—iterating over N users, where N is usually known only at runtime—can make this tricky.


Maybe at first we don’t think we’re handling enough data to really *need* pagination, only to later notice performance has degraded when our` ALL_USERS` query returns more users than expected (been there 🙋🏻‍♀️). Or maybe our` UserComponent` starts out with a simple implementation that does minimal work on each render, but as our app grows it either becomes more complex *or* renders more complex children of its own.


I hope by now I’ve convinced you this is a problem worth solving! But is` memo` the answer?


Using the criteria from the React docs, our` UserComponent` **is a good candidate for memoization** : all other users in the list are re-rendering with the same exact props ✅, and each component is simulating expensive rendering logic, causing the DOM to perceptibly lag when applying updates to 2,000+ nodes on every keystroke ✅.


Sure enough, after wrapping` UserComponent` in` memo` , we can see the lag disappear:


Fun fact: even though[objects, arrays and functions normally need to be memoized/cached when passing them as props](https://react.dev/reference/react/memo#my-component-rerenders-when-a-prop-is-an-object-or-array) to memoized React components (so they pass React’s` Object.is` same value equality check when determining whether the child needs to re-render), we don’t have to memoize the user objects we’re passing into our` UserComponent` s. This is because Apollo Client’s normalized cache has already done the work to ensure the user objects we get from the cache are referentially stable 🎉


## Rendering` memo` unnecessary


The lag is gone, but our work isn’t done. Notice that the *parent component is still re-rendering on every keystroke* , because our` useQuery` call is watching for changes on all` users` . **We’re still re-rendering the parent, but now React is able to skip re-rendering all unchanged child components.**


Re-rendering the parent has its own drawbacks: in a larger application, we’d also have to remember to wrap every other child of` UserList` in` memo` when needed (see the criteria above, rinse and repeat). And` memo` ‘s comparison of all of those props still comes with a cost—one we’d like to avoid altogether.


Our Apollo Client team saw an opportunity here. We wondered: what if a parent component could ignore updates to certain fields on the selection set it’s watching in the cache? And what if each child component could react to updates for a single cache entity, *without* being prompted to re-render by its parent?


## Enter` @nonreactive` and` useFragment`


Together, this new directive and hook unlock a pattern for rendering lists that selectively re-render individual list items by default— **no` memo` required** .


You can apply the` @nonreactive` directive to fields or fragments on the parent component’s query, and you can use it with or without Apollo Client’s React bindings (for example, in queries passed to` client.watchQuery` ).


In our demo app, the` AllUsers` query becomes:


```text
query AllUsers {
users {
id
...UserFragment @nonreactive
}
}
```


Note: The parent component *should* receive updates for the` id` field (or whatever the equivalent cache key is for an entity), so we intentionally *don’t* apply` @nonreactive` to the` id` field here. This ensures that the parent **does** update when items are added or removed from the list.


Now, when cache updates arrive and Apollo Client is comparing the previous result with incoming data for a selection set to determine whether to update, it can avoid comparing fields that exist on subtrees of the` @nonreactive` fragment altogether. Less work for Apollo Client, and less work for React as a result.


Instead of passing each user object into` UserComponent` and wrapping it with` memo` , child components can use the` id` and fragment document to receive cache updates directly for each user in the list:


```text
function UserComponent({ id }) {
const { data: user } = useFragment({
fragment: UserFragment,
from: {
__typename: "User",
id,
},
});


return <li>{user.name}</li>;
});
```


Any component watching for changes for a specific user via` useFragment` will re-render on its own when data changes. It no longer needs to wait for a parent to trigger a re-render by passing it fresh data via props!


Finally, our parent component **no longer re-renders at all when updating users** —only when users are **added** or **removed** . This gives us predictable behavior *and* performance, whether our list is 10, 100, or 1,000 users long! And we achieved all of this without` memo` . (You definitely *should* still use` memo` as needed to solve other performance issues, per the guidance in the docs!)


View the full CodeSandbox demo[here](https://codesandbox.io/s/github/alessbell/nonreactive) . You can try out both` @nonreactive` and` useFragment` in our latest 3.8 beta via` npm i @apollo/client@beta` and view the documentation for both` <a href="https://www.apollographql.com/docs/react/data/directives/#nonreactive">@nonreactive</a>` and` <a href="https://www.apollographql.com/docs/react/api/react/hooks-experimental/#usefragment">useFragment</a>` for more information. We’d love to hear what you think!


Written by


Alessia Bellisario


[Read more by Alessia Bellisario](https://www.apollographql.com/blog/author/alessia)
