---
schema_version: "1.0.0"
document_id: "9c39fd22d5cdaeb6085a14d53623ed3b8c1fbb838ae6e4593cc0ec54e288cc17"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/using-apollo-link-to-handle-dependent-queries"
published_at: "2020-04-13T18:59:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:36f0863e05f88c816fd167b64eca5587f76b0eaf8ce86e2a9c2c23d87512023b"
---

# Using Apollo Link to Handle Dependent Queries

If you’re using GraphQL and Apollo to build an app, sooner or later you’ll run into cases where there are queries that depend on each other. In an ideal world, each screen would use just one query to fetch all of the information it needs. However, in the real world, you may not always be able to accommodate your front-end needs in the GraphQL backend.


Imagine you’re creating a social network app for users from various planets. In each user’s profile screen, you want to display information about the user, such as their` name` ,` age` , and` bio` , as well as information about their` planet` , such as its` name` .


Ideally, all information for the profile screen should be provided to you from your GraphQL server using one query. But what if the backend was created by developers in a separate galaxy without your particular app in mind? Perhaps you have to use multiple queries for the profile screen? One query to get the user’s information, and one to get information about their planet. This post tackles this problem while comparing a few solutions:


1. Update the GraphQL server to adhere to frontend needs
2. Naively get the response from one query and pass it to another
3. Use Apollo Link to automatically read the value from previous queries and pass it to dependent queries


The goal of this article is to solve the problem above and inspire you to take advantage of Apollo Link in creative ways! Lots of GraphQL challenges can be addressed elegantly using Apollo Link.


## Problem


Given the example above, the user’s profile screen needs to run two queries.


- One to get the user’s information and their planet’s ID:


```text
query UserQuery {
user {
name
numberOfEyes
planetId
}
}
```


- And another to get the planet information:


```text
query PlanetByIdQuery($planetId: ID!) {
planet(planetId: $planetId) {
name
gravity
numberOfMoons
}
}
```


The problem that we’re trying to solve is not having to do the extra work to grab` planetId` from the first query and pass it to the second query.


### Option 1: Update the GraphQL server to adhere to frontend needs


Once you realize that a screen needs to run multiple queries, ideally you can update your server to satisfy the needs of that particular screen. From the frontend’s point of view, a single query like this would be ideal:


```text
query ProfileInfo {
user {
name
numberOfEyes
planet {
name
gravity
numberOfMoons
}
}
}
```


If it doesn’t make sense to nest the queries like above, you could keep the queries separate, but take advantage of[Apollo Server’s Context](https://www.apollographql.com/docs/apollo-server/data/resolvers/#resolver-arguments) to resolve the other dependent queries.


The` context` argument is available to every GraphQL resolver on the server, which you can use to access the current user’s information, such as their` planetId` in our example.


By taking advantage of context, you can create a query like this one:


```text
query MyPlanetQuery {
myPlanet {
name
gravity
numberOfMoons
}
}
```


Notice how this time we didn’t have to pass the` planetId` to the query? Configured correctly, the server knows which user made the call. The` myPlanet` resolver uses context to figures out the user’s` planetId` , and returns the correct` planet` for the query above.


You can learn more about this by reading[Putting user info on the context](https://www.apollographql.com/docs/apollo-server/security/authentication/#putting-user-info-on-the-context) in the Apollo docs.


### Option 2: Get the response from one query and pass it to another


Ideally, your schema is designed with your frontend’s needs in mind, but that’s not the case for our current example, since the API was created on an entirely different planet.


Another common situation where this can happen is when you’re using an auto-generated GraphQL server, such as one that acts as a wrapper for a REST API.


In cases like that, you can get the results from one query and pass them as a variable to the second query, like this:


```text
const { data: profileData } = useQuery(PROFILE_INFO)
const planetId = profileData?.planet?.id
const { data: planetData } = useQuery(PLANET,
skip: !planetId,
variables: { planetId }
)
```


The` PROFILE_INFO` query runs first and gets the user’s data, including` planetId` . That value is then passed to the` PLANET` query.


Note that the` PLANET` query will skip if` planetId` is not retrieved, whether due to an error or a loading state.


### Option 3: Use Apollo Link to automatically read the value from previous queries and pass it to dependent queries


The solution above solves our problem for cases where a single query depends on another single query. But again, due to non-ideal circumstances, you might run into cases where one particular value (like` planetId` ) is needed for every single subsequent operation.


You could start by solving your problem with option 2 above, but as you add more features to your app, you might discover that you have the problem of dependent queries on every single screen. It’s annoying and messy to have to query` user` data on every screen just to get the` planetId` required by other queries.


In cases like this,[Apollo Link](https://www.apollographql.com/docs/link/) can help you seamlessly inject that variable into every query that needs it, somewhat similar to how you would do it on the server using Apollo Server’s context.


According to the documentation, “Apollo Links are chainable *units* that you can snap together to define how each GraphQL request is handled by your GraphQL client”. They are kind of similar to Redux middleware, in that they can modify queries and mutations before a call is made, or once the result has returned. Common use cases for Apollo Link include adding analytics or logging to every operation, or to transform incoming or outgoing data.


In our case, as a workaround, we can use Apollo Link to automatically insert the value of` planetId` into each query, so you don’t have to provide it anymore. This is a **workaround** , because in an ideal scenario this concern would have been addressed on the server.


Below is where all the magic happens:


```text
const injectVariables = async operation => {
const variableName = "planetId";
if (
queryRequiresVariable({
variableName,
operation
})
) {
const results = await client.query({
query: USER_QUERY,
fetchPolicy: "cache-first"
});
const planetId = results?.data?.user?.planetId;    operation.variables[variableName] = planetId;
}
};
```


This link runs for every single query and mutation. It looks at the operation and uses a helper function (` queryRequiresVariable` ) to check whether it require` planetId` . It does so by looking at the query definition, the first line in below:


```text
query planet($planetId: ID!) {
planet(planetId: $planetId) {
id
name
}
}
```


Whenever the link confirms that an operation needs` planetId` , it inserts the variable and its value into the operation. Since our` fetchPolicy` is` cache-first` , it first looks into the cache to find the value of` planetId` . If the value isn’t available, it runs the prerequisite query to get` planetId` before running any dependent queries.


Curious about how` queryRequiresVariable` works? It looks at the query definition and searches for the` variableName` like below.


```text
const queryRequiresVariable = ({ variableName, operation }) =>
operation.query.definitions?.some(({ variableDefinitions }) =>
variableDefinitions?.some(
({ variable }) => variable.name.value === variableName
)
);
```


It might seem complicated at first, but all it’s doing is looping through all definitions (since there can be multiple queries in an operation) and their respective` variableDefinitions` to see if the` variableName` (` planetId` ) is defined in the operation.


You can check the full implementation and[usage of the link](https://github.com/arrygoo/apolloLinkBlogpost/blob/master/web/src/apolloClient.js) here:[https://github.com/arrygoo/apolloLinkBlogpost](https://github.com/arrygoo/apolloLinkBlogpost/tree/master/web) .


## Final Words


The solution above is just one example of how you can use Apollo Link to address issues that affect a large number of queries or mutations.


If you’re looking for inspiration, check out some of the other links[created by the community.](https://www.apollographql.com/docs/link/links/community/)


And if you’d like to try creating your own link, visit the[docs for a deep dive](https://www.apollographql.com/docs/link/overview/) into the architecture behind Apollo Link.


---


I’m Aryan, feel free to reach me for GraphQL consulting at **aryangoharzad@gmail.com**


Written by


Aryan Goharzad


[Read more by Aryan Goharzad](https://www.apollographql.com/blog/author/arrygoo)
