---
schema_version: "1.0.0"
document_id: "eb4f84c2d6e8b30645a8de7d55f02ad23b37fc032fc6d75ae230eb868f2fde8d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/more-resilient-code-with-data-masking-in-apollo-client-3-12"
published_at: "2024-12-10T09:47:59+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:e682f8ff3461b516c986e438e060ffd37dc89d07a56371e5987eb1ab513fcc94"
---

# More resilient code with data masking in Apollo Client 3.12

We are excited to announce the release of Apollo Client 3.12, which primarily focuses on[data masking](https://www.apollographql.com/docs/react/data/fragments#data-masking) .


When building complex applications today, Apollo Client[users are leveraging GraphQL fragments](https://www.apollographql.com/blog/optimizing-data-fetching-with-apollo-client-leveraging-usefragment-and-colocated-fragments) to make their applications more maintainable. Data masking leverages fragments to help you write more resilient and performant code.


## What is data masking?


Data masking enforces that only the fields requested by a component in a query or fragment are available to it. This provides looser coupling between your components and avoids accidentally introducing implicit dependencies between them where a change in one component can break another.


As an example, let’s look at the following` Posts` component. The` Posts` component fetches and displays a list of` PostDetails` , optionally filtering out unpublished ones, using a GraphQL query that includes a fragment for post details.


```text
import { PostDetails, POST_DETAILS_FRAGMENT } from './PostDetails';


const GET_POSTS = gql`
query GetPosts {
posts {
id
...PostDetailsFragment
}
}


${POST_DETAILS_FRAGMENT}
`;


export default function Posts({ includeUnpublishedPosts }) {
const { data } = useQuery(GET_POSTS);
const posts = data?.posts ?? [];


// loading state omitted for brevity


const allPosts = includeUnpublishedPosts
? posts
: posts.filter((post) => post.publishedAt);


if (allPosts.length === 0) {
return <div>No posts to display</div>;
}


return (
<div>
{allPosts.map((post) => (
<PostDetails key={post.id} post={post} />
))}
</div>
);
}
```


The` PostDetails` component uses a fragment to define its data requirements necessary to render the associated UI elements.


```text
export const POST_DETAILS_FRAGMENT = gql`
fragment PostDetailsFragment on Post {
title
shortDescription
publishedAt
}
`;


export default function PostDetails({ post }) {
return (
<section>
<h1>{post.title}</h1>
<p>{post.shortDescription}</p>
<p>
{post.publishedAt ?
`Published: ${formatDate(post.publishedAt)}`
: 'Private'}
</p>
</section>
);
}
```


Suppose we no longer want to show the publish date with the post details and prefer to show it on individual posts. We’ll remove the UI elements and the` publishedAt` field from the fragment since` PostDetails` no longer uses it.


```text
export const POST_DETAILS_FRAGMENT = gql`
fragment PostDetailsFragment on Post {
title
shortDescription
}
`;


export default function PostDetails({ post }) {
return (
<section>
<h1>{post.title}</h1>
<p>{post.shortDescription}</p>
</section>
);
}


```


Uh oh, we just broke our app–the` Posts` component no longer shows any posts!` Posts` implicitly relied on data required by` PostDetails` so when` PostDetails` changed, it broke` Posts` .


This coupling is an example of an implicit dependency between our components. As your application grows in complexity, these types of implicit dependencies are more difficult to track.


Data masking solves this issue by returning only the fields requested by the component’s query or fragment. Here is what the data would look like in` Posts` with data masking enabled:


```text
{
"posts": [
{
"__typename": "Post",
"id": "1"
},
{
"__typename": "Post",
"id": "2"
}
]
}
```


It is more obvious where the bug is because` publishedAt` isn’t provided to the component to begin with. Catching this issue up-front is more straightforward.


## Fragments


Data masking encourages the use of[colocated fragments](https://www.apollographql.com/docs/react/data/fragments#colocating-fragments) where components define their data requirements using GraphQL fragments. With data masking, you read fragment data from a query with the[useFragment hook](https://www.apollographql.com/docs/react/data/fragments#usefragment) .


```text
function PostDetails({ post }) {
const { data, complete } = useFragment({
fragment: POST_DETAILS_FRAGMENT,
from: post,
});


// complete check omitted for brevity


return (
<section>
<h1>{data.title}</h1>
<p>{data.shortDescription}</p>
</section>
);
}
```


By structuring queries to be composed of smaller fragments, you’re optimizing the number of renders in your React app. It’s like having[@nonreactive](https://www.apollographql.com/docs/react/data/directives#nonreactive) applied to your query automatically! Any component containing` useFragment` will only re-render if the fields used in that fragment are changed, making your app more predictable and performant.


## Enabling data masking


You enable data masking by setting the` dataMasking` option to true in your Apollo Client instance.


```text
new ApolloClient({
dataMasking: true,
// ...
});
```


Once enabled, fragment spreads in your operations are masked.


## Incremental adoption


Many Apollo Client users today build large and complex applications where many areas of the codebase use Apollo Client functionality. Because of this, enabling data masking out-of-the-gate can be unrealistic due to the amount of work and testing required.


We’ve made it possible to incrementally adopt data masking in your application so that you don’t have to update your entire application at once. With Apollo Client 3.12, we’re introducing a new client-only directive` @unmask` .` @unmask` can be applied to fragments where you’d like the fragment data available to the component.


` @unmask` ships with a migration mode that provides development-only warnings when accessing would-be masked fields throughout your application. This makes it easy to determine what would break if the fragment were masked.


Adding` @unmask` to your entire application can be tedious, especially if you have many fragments or queries throughout your application. Apollo Client provides a codemod that will do the hard work for you.


Learn how to use the codemod and the steps required to incrementally adopt data masking in your application in the[documentation](https://www.apollographql.com/docs/react/data/fragments#incremental-adoption-in-an-existing-application) .


## TypeScript


Data masking wouldn’t be complete without a proper TypeScript integration. We’ve integrated with[GraphQL Codegen](https://the-guild.dev/graphql/codegen) and the format generated by their[Fragment Masking](https://the-guild.dev/graphql/codegen/plugins/presets/preset-client#fragment-masking) feature to provide masked types.


You can generate masked types using either[TypeScript Operations plugin](https://the-guild.dev/graphql/codegen/plugins/typescript/typescript-operations) or the[client preset](https://the-guild.dev/graphql/codegen/plugins/presets/preset-client) . The latest versions of each provide support for the` @unmask` directive to generate types with unmasked fields where applicable.


For more detailed information about the use of TypeScript with data masking, including the GraphQL Codegen configuration needed to generate masked types, check out the[documentation](https://www.apollographql.com/docs/react/data/fragments#using-with-typescript) .


Written by


Jerel Miller


[Read more by Jerel Miller](https://www.apollographql.com/blog/author/jerel)
