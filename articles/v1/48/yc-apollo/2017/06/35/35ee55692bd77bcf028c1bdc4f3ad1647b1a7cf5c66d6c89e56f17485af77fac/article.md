---
schema_version: "1.0.0"
document_id: "35ee55692bd77bcf028c1bdc4f3ad1647b1a7cf5c66d6c89e56f17485af77fac"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-we-built-launchpad-cb09cbf39ab6"
published_at: "2017-06-15T23:28:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:b0c0905dbe6cc09c419602165e025f122ed08f31c788162fa7715eeef3876a87"
---

# How we built Launchpad

Last month we[announced Apollo Launchpad](https://dev-blog.apollodata.com/introducing-launchpad-the-graphql-server-demo-platform-cc4e7481fcba) , a new tool to prototype GraphQL servers and create GraphQL learning experiences. We were amazed by the response: Launchpad was mentioned during several GraphQL Europe talks, and the community gave us amazing feedback as they tried it out between sessions. In this blog post, I’ll talk about how we built Launchpad and go over some important design decisions we made along the way.


### What is Launchpad?


Launchpad is a platform to write and deploy GraphQL servers in Javascript. It’s similar to projects like[JSFiddle](https://jsfiddle.net/) ,[Expo Snack](https://snack.expo.io/) ,[Plunker](https://plnkr.co/) , and[CodeSandbox](https://codesandbox.io/) , but has an important distinction — it runs Node.js server code, not UI code. After the code is saved, the server can be accessed through the built-in Graph *i* QL interface, and through a publicly-available GraphQL endpoint.


Here’s what Launchpad looks like. This pad lives at[https://launchpad.graphql.com/1jzxrj179](https://launchpad.graphql.com/1jzxrj179)


## The programming model


One of the goals of Launchpad was to provide an experience as close to writing regular GraphQL server code as possible. At the same time, we wanted to avoid unnecessary complexity, both for the users and for our own implementation. For this reason, we decided to expose a specialized API rather than full HTTP server capabilities.


### Exporting GraphQL-specific objects


Since we wanted to focus on deploying GraphQL servers specifically, rather than hosting generic Node projects, we designed Launchpad so that user code exports GraphQL-specific objects, like schema or context. Then, Launchpad internally wraps it into a complete server during deployment. The wrapper code handles basic HTTP functionality, catches errors to avoid unhandled exceptions, and calls the user-provided GraphQL schema and functions.


The basic Launchpad Hello World example looks like this:


```text
import { makeExecutableSchema } from 'graphql-tools';


const typeDefs = `
type Query {
hello: String
}
`;


const resolvers = {
Query: {
hello: (root, args, context) => {
return 'Hello world!';
},
},
};


export const schema = makeExecutableSchema({
typeDefs,
resolvers,
});


export function context(headers, secrets) {
return {
headers,
secrets,
};
};
```


Here, a GraphQL schema is created using the schema definition language, and resolvers are added for custom functionality. Then you just export the schema object, which you can create with GraphQL tools or any other GraphQL schema library. It’s also possible to export a context function to inject a global environment into the schema, to provide access to things like HTTP headers.


The goal here was to be as close to how one would write a GraphQL server locally as possible. In fact, making a` schema.js` file with similar content would probably be the way to start a GraphQL project normally. Thankfully, you don’t have to do that manually — Launchpad allows you to download the full server code of any saved pad, together with the boilerplate to run an HTTP server, so that you can just` npm install` and` npm start` .


### Transpilation with Babel


Modern JavaScript developers often rely on transpilers to use the most modern language features, for example async/await. Therefore, we had to introduce Babel in our toolchain. We decided to do it in the client to avoid loading the server with transpilation. This also enables Launchpad to catch and present typos and small errors immediately, without a server roundtrip.


We used[babel-standalone](https://github.com/babel/babel-standalone) , a pre-packaged version of Babel that is easy to use in browser without packaging all the dependencies. babel-standalone with the required plugins is quite large, so we split it into a separate bundle. The bundle is only loaded and imported when the first compilation happens.


### User-provided secrets to enable database and API access


One of the goals of Launchpad was to allow real-world use cases, like a GraphQL server with a database, or one that uses a third-party API. Our first idea was to add a built-in database to each Pad, so that users can use it to develop database-driven APIs. However, this would lock the user into that database, limit the set of examples that could be built, and incur significant hosting costs for us. Instead, Launchpad provides users with “secrets” — a secure way to pass the server context, like a database url and password, to the pad. This way you have the flexibility to connect any kind of database or third-party service with Launchpad.


## How to run and scale user-provided server code?


Launchpad poses a unique architectural challenge. Other similar “code playground” applications, like JSFiddle, CodeSandbox, and Expo Snack, run in the user’s browser or on a mobile device, which doesn’t require scaling servers up and down during normal operation. A naïve implementation of Launchpad would spawn a new server or container for each user’s pad. Unfortunately, this wouldn’t work for us: We anticipated a large number of users with a low resource usage per user, so didn’t want to over-provision resources and needed deploys to be super fast to enable iterative development in the browser.


One approach we considered was running the user code inside the browser. This way we wouldn’t need to have actual, separate, server-side deployments for each user. However, this would have pretty severe drawbacks: Users would lose the ability to have a public server endpoint, and this would also require implementing a big part of the node runtime inside the browser. One of our goals with Launchpad was to not have any kind of “artificial” experience; we wanted the code that users write in Launchpad to be the same code that they write when developing GraphQL servers normally. In addition, it would be too much work to reimplement all of the Node APIs, including database access, file system, and more.


After considering those options, we started looking into hosting solutions that scale (and charge) based on actual usage (deployment requests), rather than for reserved capacity. One important condition was that there was no scalability or pricing limitation to the number of dormant deployments. This way there would be no need to clean up old code or to limit the number of deployments per user. The only important factor would be which pads are actually being accessed at any given time.


### Selecting Auth0 Extend


Based on the requirements, we decided that one of the “function-as-a-service” platforms would be a good choices for us. Currently, these services include[Auth0 Extend](https://auth0.com/extend/) (built on the Webtask technology), AWS Lambda, Google Cloud Functions, and Azure functions. Two of our main priorities were low-maintenance with super fast deploy times. Webtask had the simplest API and was very quick to deploy, so we were able to build a proof of concept in just a few days. Once the prototype was done, we got in touch with the team at Auth0 Extend. The team was very excited about our Launchpad idea, and offered a lot of support to get us up and running on the Extend platform in a matter of days.


## Implementation overview


The codebase is composed of two main parts:


- **Launchpad server** : This is a Node server that manages pad storage and permission handling for the users. In addition, it deploys the pad code to[Auth0 Extend platform](https://auth0.com/extend/developers) by calling their API. Naturally, the Launchpad server is itself a GraphQL server.
- **Launchpad UI** : This is a React app that displays the code editor, GraphiQL, and other controls. It also handles compiling the client-side code with Babel before sending it to the backend.
- **Launchpad proxy** : A simple node proxy that handles all of the requests and forwards them to the correct Auth0 extend container. This way it’s possible to control how exactly people call the Extend platform. This can easily be expanded to include things like rate limiting, analytics, redirects, and subscriptions. It’s also easier for us to control the public API URL with a proxy than otherwise.


Launchpad architecture diagram


## Deploying code on Auth0 Extend


Setting up Launchpad to deploy to Extend was pretty simple, but there were a few specifics we had to think about:


### Code isolation


Deploying through Extend API is done through a token. It’s a JSON Web Token that specifies the *container* to which the code will be deployed. Deployed code running on the same container may share the runtime. Therefore, we issue a new token for each pad and deploy it on a separate container. This way, users’ code won’t interfere with other pads, and secrets are actually secret to each deployment.


### Versioning and drafts


Two separate versions of the pad are deployed on each container: draft and saved. The draft version is the one deployed repeatedly when the code is edited in the editor, while the saved version is only deployed on explicit pad save. The main reason for this split is to make sure that improving your pad code won’t break other people who might be using the deployed pad url at the time. It also makes resetting the pad to a previous state a very simple operation that doesn’t require any API calls or fancy version control, as it’s possible to just revert to the saved version of the pad.


### npm dependencies


Extend allows us to easily install any npm dependencies, but it’s not quite as simple as a usual deploy. The Extend CLI knows how to automatically resolve dependencies, but the Extend API requires additional requests to the API to resolve and deploy dependencies. Our solution to this was very simple, (and shamelessly copied from Extend CLI library 🙂): We do a request to[unpkg.com](https://unpkg.com/) to get the latest` package.json` of the desired package, find the latest version, and request that from the Extend API. We figure out each pad’s dependencies when they are used by analyzing all import and require statements in the code using[babel-plugin-detective](https://github.com/avajs/babel-plugin-detective) .


## Future plans and conclusion


We believe Launchpad is a great tool for GraphQL community. Hopefully it will help lower barriers to GraphQL adoption and make it much easier to teach about GraphQL server development.


The[Auth0 Extend](https://auth0.com/extend/developers) platform helped us to implement Launchpad in just 1.5 months (more on this to come!). Basic proof-of-concept was done in just one week. The simple API and absence of additional required configuration made both initial development and iteration very fast, which was a huge win over heavier platforms like AWS Lambda.


### Help create more examples


At the end of the day, Launchpad will be successful only if it enables people to create lots of great GraphQL examples. It’s our hope that the whole GraphQL community can help us build those examples. Check out the[launchpad repository](https://github.com/apollographql/launchpad) to see what already exists, and our[suggestions](https://github.com/apollographql/launchpad/blob/master/README.md#ideas-for-examples) for what GraphQL examples people would like to see, and together we can spread GraphQL even further!


Written by


Mikhail Novikov


[Read more by Mikhail Novikov](https://www.apollographql.com/blog/author/freiksenet)
