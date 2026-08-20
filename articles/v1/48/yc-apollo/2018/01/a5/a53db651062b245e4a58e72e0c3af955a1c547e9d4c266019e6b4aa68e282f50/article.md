---
schema_version: "1.0.0"
document_id: "a53db651062b245e4a58e72e0c3af955a1c547e9d4c266019e6b4aa68e282f50"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/exploring-reason-and-graphql-ff877df60d2a"
published_at: "2018-01-04T22:07:36+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:b4cee001d5bb06a4ed8eff65ed6a95659a60548e0467dcc152786955da405813"
---

# Exploring Reason and GraphQL

Over the holiday break, while the in-laws slept soundly and not a child was making a peep, I had the chance to play around with[ReasonML](https://reasonml.github.io/) . Reason is a new programming language built on top of OCaml and designed to be compiled into JavaScript. After spending a couple of years with TypeScript over the development of Apollo, I thought it would be fun to try another compile to JavaScript language.


### Why Reason?


There are a[lot of benefits](https://reasonml.github.io/guide/what-and-why) to choosing a language like Reason, but honestly I just wanted to try it out to see what all the excitement is about! After Cheng Lou’s talk at[React Conf in 2017](https://www.youtube.com/watch?v=_0T5OSSzxms) I wanted to see for myself if it was an exciting, but shiny new toy, or if it could make a big difference in how we write and ship JavaScript.


## The project


I only had a few hours to play around with a project so I opted to rebuild something I know quite well, a GraphQL endpoint! I choose to remodel the[Authors and Posts](https://launchpad.graphql.com/1jzxrj179) example from Apollo Launchpad. If you haven’t checked it out before, now is the time! The app is less than 80 lines of JavaScript so how hard could it be to model in Reason?


Getting started was pretty easy thanks to the excellent documentation at[https://reasonml.github.io](https://reasonml.github.io/) . I quickly bootstrapped a project using` bsb -init authors-and-posts -theme basic-reason` and I was off to the races!


### Modules Everywhere


The most jarring change for me as a JavaScript developer was the lack of explicit import definitions. In Reason, each module is available without manually requiring it and each file is converted into a module. Turns out this can be pretty great for GraphQL because you can easily model each type in its own module.


```text
open Data;


let typeDef = {|
type Author {
id: Int!
firstName: String
lastName: String
posts: [Post] # the list of Posts by this author
}
|};


type resolvers = {. "posts": Js.Array.t(post)};


let resolvers = {
"posts": (author: Data.author) =>
Js.Array.filter((post) => post##authorId === author##id, posts)
};
```


Did you notice the` open Data;` at the top of that file? It acts kinda like a JavaScript` import` but allows you to access any named export from that file without having to manually reference it. Variables like` post` and values like` posts` are actually coming from the` Data` module! I left the` Data.author` to show that you can still use the module reference which can make the code easier to understand.


Defining types also shows where graphql-tools shines for building GraphQL schemas. Looking at this file, it is super easy to know what is being built and the default resolver from GraphQL means I only need to figure out how to link an author and post together!


### Immutable by design


At this point, everything is running super smoothly so surely something has to go wrong? Absolutely.


In the Authors and Posts example, we see there is some local data that is updated with a mutation:


```text
Mutation: {
upvotePost: (_, { postId }) => {
const post = find(posts, { id: postId });
if (!post) {
throw new Error(`Couldn't find post with id ${postId}`);
}
post.votes += 1;
return post;
},
},
```


In Reason, when you are defining JavaScript objects, all of the fields are immutable by default. So trying to update the votes in the post will throw some errors in the console when you compile it. Luckily, there is a growing community of Reason developers in Discord that are happy to help![Yawar λmin](https://medium.com/u/516e71756f17?source=post_page-----ff877df60d2a----------------------) walked me through using` ref` to tell Reason that the` votes` field could be mutated in place. When defining a type, like on line 9, you pass the underlying mutable type, in this case int, to` ref` . When declaring data, like on line 12, you pass the initial value that can be mutated to the` ref` call. It’s a pretty cool way to be very specific about what can and cannot change in your data!


```text
type author = {. "id": int, "firstName": string, "lastName": string};


let authors = [|
{"id": 1, "firstName": "Tom", "lastName": "Coleman"},
{"id": 2, "firstName": "Sashko", "lastName": "Stubailo"},
{"id": 3, "firstName": "Mikhail", "lastName": "Novikov"}
|];


type post = {. "id": int, "authorId": int, "title": string, "votes": ref(int)};


let posts = [|
{"id": 1, "authorId": 1, "title": "Introduction to GraphQL", "votes": ref(2)},
{"id": 2, "authorId": 2, "title": "Welcome to Apollo", "votes": ref(3)},
{"id": 3, "authorId": 2, "title": "Advanced GraphQL", "votes": ref(1)},
{"id": 4, "authorId": 3, "title": "Launchpad is Cool", "votes": ref(7)}
|];
```


```text
open Data;


let typeDef = {|
type Mutation {
upvotePost(postId: Int!): Post
}
|};


type upvotePostArgument = {. "postId": int};


type root;


type resolvers = {. "upvotePost": (root, upvotePostArgument) => post};


exception MissingPost(string);


let resolvers: resolvers = {
"upvotePost": (_, filter) =>
switch (Js.Array.find((post) => post##id === filter##postId, posts)) {
| None => raise(
MissingPost("Couldn't find post with id " ++ string_of_int(filter##postId))
)
| Some(p) =>
incr(p##votes);
p
}
};
```


### Putting it together


At this point my egg nog was getting warm and I had re-watched[Brandon Dail](https://medium.com/u/25b2696a8b5d?source=post_page-----ff877df60d2a----------------------) ’s videos on building with Reason ([here](https://www.youtube.com/watch?v=Yflk43afWj4) and[here](https://www.youtube.com/watch?v=k77aR_JyvEE) ) a couple times, I was ready to fire up an express app and try it out! I opted to write the express portion in JavaScript to save time and the[Apollo Server docs](https://www.apollographql.com/docs/apollo-server/example.html) are pretty much copy, paste, magic!


One of the coolest parts of Reason is its ability to compile into JavaScript using a tool called Bucklescript. By default, it will even place the generated JavaScript files right next to the Reason ones making it easy to understand what is happening under the hood, and so easy to use alongside existing JavaScript apps! All I had to do was import my schema from the generated JavaScript file and I had an app up and running!


```text
const express = require("express");
const bodyParser = require("body-parser");
const { graphqlExpress, graphiqlExpress } = require("apollo-server-express");


// import schema from generated bucklescript files
const { schema } = require("./src/Schema.bs.js");


// Initialize the app
const app = express();


const { PORT = 3000 } = process.env;
const endpointURL = `/`;


// The GraphQL endpoint
app.post(endpointURL, bodyParser.json(), graphqlExpress({ schema }));


// GraphiQL, a visual editor for queries
app.get(endpointURL, graphiqlExpress({ endpointURL }));


// Start the server
app.listen(PORT);
```


### Deployment with Up


Now that I had this super cool little Reason and GraphQL app, I had to show it off! Luckily there is an incredible tool for setting up and deploying to AWS Lambda called[Up](https://github.com/apex/up) by[TJ Holowaychuk](https://medium.com/u/bbb3c7ccb0a0?source=post_page-----ff877df60d2a----------------------) . I setup my AWS credentials, bundled my app using browserify for node, and ran` up` in my console…


In under a minute I had a working GraphQL app, built with Reason, running on AWS Lambda! You can check out the app in action[here](https://rhi2j2y368.execute-api.us-east-1.amazonaws.com/development) and see the source code for it[over here](https://github.com/jbaxleyiii/authors-and-posts-reason) .


---


I had a couple takeaways from this short, but fun, adventure into a new language. One is that the tooling for developers to learn and build products is absolutely incredible today. Auto formatting, strong editor integration, and helpful errors make a big difference in picking something up.


The other, and more important takeaway, is how important a community is to a project. Even as young as Reason and Up are, I was able to get help from people like[Jared Forsyth](https://medium.com/u/ecb657c8b22d?source=post_page-----ff877df60d2a----------------------) and[TJ Holowaychuk](https://medium.com/u/bbb3c7ccb0a0?source=post_page-----ff877df60d2a----------------------) when I ran into problems in almost no time. I’ve seen this time and time again in the Apollo community and in 2018 I hope that we all continue to explore and help each other as we build new things and try new ideas. Come find me in the Apollo Slack if you want to chat about GraphQL, Reason, or eating cookies for Santa while building an app.


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
