---
schema_version: "1.0.0"
document_id: "0bd7be8de35dc51020fd3ae271421765d9f5a821c732566c2abfc1cb3eb04f1c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-next-for-react-apollo-4d41ba12c2cb"
published_at: "2018-01-25T22:05:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:d36ed45b7dab9656fb9494777c42ce52a9ad38eed2c20cf76d1750936aee42d3"
---

# What’s next for React Apollo

### tldr;


```text
npm i react-apollo@beta --save
```


I’m super excited to announce the first beta of React Apollo version 2.1. It is a non-breaking change (unless you are using TypeScript) with a ton of new features, so go install it right now! It adds a new component API, smaller bundle size, and better TypeScript support, *and* even supports Preact for server-side rendering.


Query component in action!


## Design is never done


It has been over a year and a half since this[pull request](https://github.com/apollographql/react-apollo/pull/120) was merged, creating the current React Apollo API. Back then,[Sashko Stubailo](https://medium.com/u/803918030a60?source=post_page-----4d41ba12c2cb----------------------) and[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----4d41ba12c2cb----------------------) were hard at work making the core Apollo Client the best way to use GraphQL, and I was a member of the Apollo community happily designing the integration my team at the time wanted to use to ship our app faster. At that time, Apollo Client was averaging around 3,000 downloads per week and React had a little over 400,000. Redux and HOC’s were all the rage, with libraries like Recompose being a pretty incredible way to build with React.


### Fast forward to today.


Apollo Client is now used over 150,000 times a week and React has grown to a massive 1.7 million a week! With both of these projects growing, the community was bound to grow and change. Full, “Component-based” APIs like React Router 4, Formik, and more are pushing more composition into the render function and away from patterns like higher order components. At the same time, the abilities and usage of Apollo has grown to include things like local state, REST endpoints, and more and more features around working with GraphQL endpoints.


Now, it’s time for a change, or rather an addition:


With React Apollo 2.1, we are introducing the` Query` component, a great way to bring data into your render.


This design was worked on for over a month with a ton of members of the Apollo community. A huge shout out goes to[Kevin Ross](https://medium.com/u/2e2136fb7ed0?source=post_page-----4d41ba12c2cb----------------------) ,[Dirk-Jan Rutten](https://medium.com/u/b016d103ae51?source=post_page-----4d41ba12c2cb----------------------) , and[Leonardo A Garcia C](https://medium.com/u/82263c594756?source=post_page-----4d41ba12c2cb----------------------) who really put in the hard work of writing, testing, and rewriting to find an API that is flexible but brings all of the improvements that render props can offer into an app. The work of these three show how strong and passionate the Apollo community is in building what app developers need.


## Query Component


The standout feature of React Apollo 2.1 is the` Query` component. Using it goes a little something like this:


```text
import gql from 'graphql-tag';
import { Query } from 'react-apollo';


const query = gql`
query SomeQuery {
foo {
bar
baz
}
}
`;
function MyComponent() {
return (
<Query query={query}>
{(result) => {
if (result.loading) return <Loading />;
if (result.error) return <Error error={error} />;


const { data } = result;


return <h1>Hello {data.foo.bar} {data.foo.baz}!</h1>;
})
</Query>
);
}
```


Instead of exporting a wrapped component using the` graphql` HOC, you put your query right into the render function whenever you need data! It takes a function as a child, which is quickly becoming the go-to way to use a[render prop in React](https://reactjs.org/docs/render-props.html#using-props-other-than-render) . In doing this, a few exciting features are opened up:


- Choosing a dynamic query based on what props are used
- Updating the state of the component that is showing the data without passing down a prop
- Easy composition of multiple operations


Though partially cosmetic, the design decisions that went into this component come from real-world use by a ton of Apollo developers.[Peter Piekarczyk](https://medium.com/u/cff8a83d5fc3?source=post_page-----4d41ba12c2cb----------------------) started this off with his project[Apollo Tote](https://github.com/peterpme/apollo-tote) , which was solving real needs of his app. We always want Apollo to be driven by the needs of product teams and I think this is a great example of the community designing what is best!


## Preact support


It (kinda) all started from a[James Kyle](https://medium.com/u/cc2eaf4f2cd2?source=post_page-----4d41ba12c2cb----------------------) tweet…


…which led to[Max Stoiber](https://medium.com/u/908fb8fea30c?source=post_page-----4d41ba12c2cb----------------------) pointing out a problem…


Can't do[@apollographql](https://twitter.com/apollographql?ref_src=twsrc%5Etfw) server-side rendering iirc? Is that a lazy reason?


— Max Stoiber (@mxstbr)[January 17, 2018](https://twitter.com/mxstbr/status/953607229940338690?ref_src=twsrc%5Etfw)


…which brought out[Jason Miller](https://medium.com/u/30b8f5921914?source=post_page-----4d41ba12c2cb----------------------) to find a solution!


I'd recommend changing this line to check for Component.prototype.render:[https://t.co/o7CfiFgovb](https://t.co/o7CfiFgovb)


— Jason Miller 🦊⚛ (@_developit)[January 17, 2018](https://twitter.com/_developit/status/953646725327138817?ref_src=twsrc%5Etfw)


Support for Preact has always been on the list of wants for React Apollo, but sometimes you need a little help from your friends. With Jason’s guidance, React Apollo 2.1 supports Preact (using` preact-compat` ) for both client *and* server usage.


## TypeScript improvements


We are always looking to leverage the typed nature of GraphQL to make client apps type safe. For example, the work[Grégoire Vda](https://medium.com/u/a0ae4f5c7bc0?source=post_page-----4d41ba12c2cb----------------------) has been doing with[Reason Apollo](https://github.com/apollographql/reason-apollo) informed some of the work on the new React Apollo design. Now the React Apollo code base doesn’t use the` any` type at all, and the test suites are actually type-checked as well!


The` graphql` HOC parameterized types have been streamlined to support full typing, better ease of use, and consistency with the rest of the project. The new parameterized signature is` graphql<TProps, TData, TGraphQLVariables, TChildProps>` , where none are required and full typing only requires the first three types. This means that your variables are now correctly checked when performing mutations or queries!


## The road ahead


This is a first beta of this design and we are eager to get your feedback! Try installing` react-apollo@beta` today and report any issues you run into. The upgrade should work out of the box (no breaking changes) and an initial usage doc of the new` Query` component can be found[here](https://github.com/apollographql/react-apollo/releases/tag/v2.1.0-beta.0) . I even updated[Peggy Rayzis](https://medium.com/u/c827782c6410?source=post_page-----4d41ba12c2cb----------------------) ’ famous bitcoin demo to show the new component in action:[CodeSandboxCodeSandbox is an online editor tailored for web applications.codesandbox.io](https://codesandbox.io/embed/71ro40qk16)


Thanks to all of the amazing contributors who have helped out on this release! We hope you love it!


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
