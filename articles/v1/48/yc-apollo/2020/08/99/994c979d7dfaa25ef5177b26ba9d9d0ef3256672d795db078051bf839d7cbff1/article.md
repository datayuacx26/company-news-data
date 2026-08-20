---
schema_version: "1.0.0"
document_id: "994c979d7dfaa25ef5177b26ba9d9d0ef3256672d795db078051bf839d7cbff1"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/building-a-portable-apollo-server-config"
published_at: "2020-08-17T11:06:57+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:161ad43c46d632fe520299f2403ece2dbd90fd75afa406d57ee6cee56310b643"
---

# Building a Portable Apollo Server Config

One of the great things about[Apollo Server](https://www.apollographql.com/docs/apollo-server) is that there are lots of[different Node.js middleware library integrations](https://www.apollographql.com/docs/apollo-server/integrations/middleware/) , and thus lots of different places you can run it. However, sometimes the best way to run Apollo Server in production isn’t necessarily the best way to run it during local development or testing.


For example, AWS Lambda functions are a fantastic place to run Apollo Server in production, as most of your deployment and scaling concerns are taken care of for you. However, using Lambda functions with a local emulator (like that provided by the[Serverless](https://www.serverless.com/) or[AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-reference.html#serverless-sam-cli) frameworks) can be slow to develop and test with, and fiddly to set up.


[Express](https://expressjs.com/) , on the other hand, is great for doing local development, as it’s lightweight, embeddable and easy to use with other tooling (for example[nodemon](https://www.npmjs.com/package/nodemon) ,[ts-node](https://github.com/TypeStrong/ts-node) ,[Jest](https://jestjs.io/) , etc). However, you need to surround it with a lot of supporting infrastructure if you want to also run it in production.


So wouldn’t it be great if you could do most of your local development with Express, and then deploy to an AWS Lambda function, without having to duplicate any code? In this post I’m going to show you how to do just that, whilst still accomodating environment-specific variables and integration-specific request-processing code.


## Introducing` config`


Let’s start with the basics. If you’ve used Apollo Server before, you’ll know that to get started, you first install a package that is specific to your preferred integration. For example, here’s a script that shows how ridiculously easy the` <a href="https://www.npmjs.com/package/apollo-server#installation-standalone">apollo-server</a>` package makes it to start a server that’s running in Express and lets you query a single` greeting` field:


```text
const { ApolloServer } = require("apollo-server");


const server = new ApolloServer({
typeDefs: `
type Query {
greeting: String!
}
`,
resolvers: {
Query: {
greeting: () => "Hello!",
},
},
});


server.listen();
```


To run the same code in an AWS Lambda function instead, we’d use the` <a href="https://www.npmjs.com/package/apollo-server-lambda">apollo-server-lambda</a>` package to write a module that exports a handler. For example:


```text
const { ApolloServer } = require("apollo-server-lambda");


const server = new ApolloServer({
typeDefs: `
type Query {
greeting: String!
}
`,
resolvers: {
Query: {
greeting: () => "Hello!",
},
},
});


exports.handler = server.createHandler();
```


We can see that the` typeDefs` and` resolvers` options that we provide to each class constructor are exactly the same. How can we refactor this duplication? At first glance you might think that the` ApolloServer` class might be a good place to start, but note that in each example this class actually comes from different packages and has different methods.


However, it turns out that the different` ApolloServer` class constructors all[accept the same argument](https://www.apollographql.com/docs/apollo-server/api/apollo-server/#constructoroptions-apolloserver) . We’ll refer to this argument as the “config object”. A config object can specify a *lot* of stuff. In fact, it can encapsulate everything about a particular server that is sharable across integrations.


This means you can share all of the schema-related code, the same resolver code (which can include any[custom scalar and enum types](https://www.apollographql.com/docs/apollo-server/schema/scalars-enums/) that you have), the same[datasources](https://www.apollographql.com/docs/apollo-server/data/data-sources/) , the same[custom directives](https://www.apollographql.com/docs/apollo-server/schema/creating-directives/) and the same context-creation code (although this last one requires a little extra work, which I’ll get back to later).


The only things you *can’t* share are those things which are specific to particular integration type. For example, you can’t run subscriptions in a lambda function, so it wouldn’t make sense to try and describe them in a config object that you want to drop into a lambda function.


So in summary, when I talk about “running Apollo Server anywhere”, what I actually mean is “running an Apollo Server config object anywhere”. In the remainder of this post, I’m going to refactor our code so that it can be run in both Express and an AWS Lambda with no duplication, then extend it to support a couple of more common real-world development scenarios.


## ` typeDefs` and` resolvers`


Let’s start by refactoring the stuff that is already duplicated between our two integrations, by putting it in a module we’ll call` config` :


```text
exports.config = {
typeDefs: `
type Query {
greeting: String!
}
`,
resolvers: {
Query: {
greeting: () => "Hello!",
},
}
};
```


If you’re using TypeScript, note that config objects have a type definition that can be imported from the` <a href="https://www.npmjs.com/package/apollo-server#installation-standalone">apollo-server</a>` package:


```text
import { Config } from "apollo-server"


export const config: Config = {
typeDefs: `
type Query {
greeting: String!
}
`,
resolvers: {
Query: {
greeting: () => "Hello!",
},
}
}
```


Whilst I highly recommend using TypeScript with Apollo Server, for the sake of brevity, I’m going to use vanilla JavaScript for the remainder of this post.


## ` dataSources`


At this point our config object isn’t doing much. But what if we wanted our “Hello” string to come from another REST server? We’d use a[datasource](https://www.apollographql.com/docs/apollo-server/data/data-sources/) for that.


Let’s start by defining a simple` MessageDataSource` module that uses the` <a href="https://www.apollographql.com/docs/apollo-server/data/data-sources/">apollo-</a><a href="https://www.npmjs.com/package/apollo-datasource-rest">datasource</a><a href="https://www.apollographql.com/docs/apollo-server/data/data-sources/">-rest</a>` package:


```text
import { RESTDataSource } from "apollo-datasource-rest"


exports.MessageDataSource = class extends RESTDataSource {
constructor() {
super()
this.baseURL = "https://localhost:8882"
}


async getMessage() {
return this.get("/")
}
}
```


Then we can import this datasource into our` config` module, and use it in our resolver:


```text
const { MessageDataSource } = require("./MessageDataSource");
...
exports.config = {
...
dataSources: {
message: new MessageDataSource()
},
resolvers: {
Query: {
greeting: async function(source, args, context) {
return `${await context.dataSources.message.getMessage()}!`
}
},
...
}
}
```


## Using` config`


Now let’s look at how we can drop this config object into a couple of different integrations. Let’s start with Express:


```text
const { ApolloServer } = require('apollo-server');
const { config } = require('./config');


const server = new ApolloServer(config);


server.listen()
```


And then a lambda function:


```text
const { ApolloServer } = require('apollo-server-lambda');
const config = require('./config');


const server = new ApolloServer(config);


exports.handler = server.createHandler();
```


But hang on a second: you might remember that the` baseURL` of the` message` datasource is hard-coded to` <a href="http://localhost:8882/">http://localhost:8882</a>` . That’s not going to work in a lambda environment!


To make our config truly portable, we need to parameterise what can change between environments. We’ll first start by tweaking our` MessageDataSource` so that it can have the value of the` baseURL` passed into it.


```text
...
exports.MessageDataSource = class extends RESTDataSource {
constructor(baseURL) {
super()
this.baseURL = baseURL
}
...
}
```


Next, we’ll make it that the` config` module now exports a function rather than a constant. The function will take an argument that contains environment-specific values, and return a new config object:


```text
...
exports.createConfig = function(env) {
return {
...
dataSources: {
message: new MessageDataSource(env.messageServerUrl)
},
}
}
```


Then we’ll tweak the lambda function to pass[process.env](https://nodejs.org/dist/latest-v8.x/docs/api/process.html#process_process_env) to` createConfig` :


```text
const { ApolloServer } = require('apollo-server-lambda');
const createConfig = require('./config');


const server = new ApolloServer(createConfig(process.env));


exports.handler = server.createHandler();
```


Now we can use[Lambda environment variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html) to set the value of` messageServerUrl` to whatever we want. We can even pass in different values for different deployment environments. For example,` messageServerUrl` might have one value in our production deployment environment , and another in our test deployment environment.


Whilst we could also use` process.env` locally with Express, I’m not a big fan of environment variables unless you have no other choice, as they are basically global. Instead, when I’m doing local development, I prefer to load my environment-specific values from a file. That way I can can easily symlink to different files to switch between different environments. So let’s do that right now for our Express server:


```text
const { ApolloServer } = require('apollo-server');
const { createConfig } = require('./config');
const env = require('./env.json');


const server = new ApolloServer(createConfig(env));


app.listen()
```


Note how we load` env.json` from the filesystem, then drop its values into` createConfig` . For development against a local server,` env.json` could look like:


```text
{ messageServerUrl: "http://localhost:8882" }
```


Alternately, we could have other versions of the file that point to our production or testing deployment environments.


## ` context`


I mentioned earlier on that it’s also possible to share context-related code between integrations, but a little bit more complex to set up. We’ll now work through how to do this, using the common scenario of[putting user info on the context](https://www.apollographql.com/docs/apollo-server/security/authentication/#putting-user-info-on-the-context) as our example. Specifically, we’ll extend the` message` resolver to include the current user’s name in its response.


For the sake of this example, let’s assume that user’s name has been encoded in a[JWT token](https://jwt.io/) that is placed in a HTTP header called` Authorization` . We will use the` <a href="https://www.npmjs.com/package/jsonwebtoken">jsonwebtoken</a>` package to do the decoding (note that in this example, we won’t be verifying the token).


```text
const jwt = require('jsonwebtoken');
...
exports.createConfig =  function(env) {
return {
resolvers: {
Query: {
message: async (source, args, context) =>
`${await context.dataSources.message.getMessage()}, ${
context.userName
}!`,
}
},
...
context: function(integrationContext) {
const authHeader = integrationContext.event.headers["Authorization"]
const payload = jwt.decode(authHeader)


return {
userName: payload.name
}
},
}
}
```


Note how the` message` resolver is now able to get the` userName` out of the context. Furthermore, to put this value into the context, we added a` context` function to our config object. This function gets the header, decodes it, and gets the user name out of the result.


Note also that, despite the naming, the` integrationContext` argument to the function should not be confused with a GraphQL context. It is instead an integration-specific object from which we can extract information about the particular integration we are running in – including the current request.


The problem is that the code we have just written will only work in an AWS lambda function. If we wanted to use Express instead, the` integrationContext` parameter would have a different shape, and we’d have to write the code slightly differently:


```text
...
exports.createConfig = function(env) {
return {
...
context: function(integrationContext) {
const authHeader = integrationContext.req.header("Authorization")
const payload = jwt.decode(authHeader)


return {
userName: payload.name
}
},
}
}
```


There’s only one line different here – everything else is the same. How can we deal with the duplication? Let’s try passing` createConfig` an additional argument – a function that gets a specific header out of the` integrationContext` :


```text
...
exports.createConfig =  function(env, getHeader) {
return {
...
context: function(integrationContext) {
const authHeader = getHeader(integrationContext, "Authorization")
const payload = jwt.decode(authHeader)


return {
userName: payload.name
}
},
}
}
```


So now our lambda code can look like this:


```text
const { ApolloServer } = require('apollo-server-lambda');
const createConfig = require('./config');


const server = new ApolloServer(createConfig(
process.env,
(integrationContext, headerName) => integrationContext.event.headers[headerName]
));


exports.handler = server.createHandler();
```


And our Express server code can look like:


```text
const { ApolloServer } = require('apollo-server');
const { createConfig } = require('./config');
const env = require('./env.json');


const server = new ApolloServer(createConfig(
env,
(integrationContext, headerName) => integrationContext.req.header(headerName)
));


server.listen();
```


We’ve done it! We’ve encapsulated all of the shared code in one place, and all of the integration-specific code in another. Some readers may find this technique reminiscent of[dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) , because we’re injecting integration-specific code into the config. Dependency injection patterns can also be used for composing more complex datasources, but that’s a topic for another blog 🙂


## Let’s Wrap This Up


Apollo Server config objects lets us package up schemas, resolvers, datasources and context-related logic in a single unit. Furthermore, we can assemble config objects at runtime in a way that makes them portable across environments and integrations. This means that you can do most of your local development and testing using something like Express, then only use an AWS Lambda function when you really need to.


I have used this approach successfully on several large projects. It’s also possible to bundle up more advanced features like custom scalar types and custom directives. Other than subscriptions (which can’t go into Lambda functions), I haven’t found anything else that can’t be made portable.


If you’re interested, I’ve created a[Github repository for the code in this post](https://github.com/shinesolutions/portable-apollo-server) . This repository also includes a Jest test suite that drops the same config object into an embedded Express instance and wires it up to a stubbed Message server. It’s a testament to the design of Apollo Server that, with a little bit of thought, we can deploy so much of our code into different environments.


*This post has been re-published with permission from[Shine Solutions](https://shinesolutions.com/) . See the original post[here](https://shinesolutions.com/2020/07/22/building-a-portable-apollo-server-config/) .*


Written by


Ben Teese


[Read more by Ben Teese](https://www.apollographql.com/blog/author/benteese)
