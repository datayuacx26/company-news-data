---
schema_version: "1.0.0"
document_id: "9745664fdb5ae44e4285ac318db0e6272df9725fc7c08137b065747a003c2026"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-launchpad-the-graphql-server-demo-platform-cc4e7481fcba"
published_at: "2017-05-17T00:50:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:74e279c8fa1f79b03344fd9547c7d37a31401786e589a616851a1efefe07eb20"
---

# Introducing Launchpad: The GraphQL server demo platform

***Important news (11/29/2018):*** *We will be sunsetting Launchpad on December 15, 2018 now that our hosting platform, Auth0 Extend, is*[discontinuing their service](https://goextend.io/blog/we-are-sunsetting-extend) *. We decided to not invest time into moving to another service because more full-featured Apollo Server playgrounds currently exist. Please migrate all examples to either*[CodeSandbox](https://codesandbox.io/s/apollo-server) *or*[Glitch](https://glitch.com/~apollo-launchpad) *before December 15, 2018 so you don’t lose your work.*


*Thanks for your support and all of your creative examples over the years! 🚀 We can’t wait to see what you build on*[CodeSandbox](https://codesandbox.io/s/apollo-server) *and*[Glitch](https://glitch.com/~apollo-launchpad) *. If you have any questions, please reach out to the Apollo team on*[Twitter](https://twitter.com/apollographql) *or*[Slack](https://www.apollographql.com/slack/) *.*


---


In collaboration with the new[AuthO Extend](https://auth0.com/extend/developers) platform and[Mikhail](https://twitter.com/freiksenet) from[Reindex](https://www.reindex.io/) , today we’re thrilled to announce a new experience that will make it easy for the GraphQL community to share different API development patterns, host example code, and write great tutorials.


It’s called[Apollo Launchpad](https://launchpad.graphql.com/) **.**


### What is Launchpad?


Launchpad is an in-browser GraphQL server playground. You can write a GraphQL schema example in JavaScript, and instantly create a serverless, publicly-accessible GraphQL endpoint. We call these code snippets that live on Launchpad “pads”.


Here’s what you can do with Launchpad:


1. **View, run, and share examples of GraphQL server patterns.** Security, connecting to backends and databases, schema design conventions, error handling, and more.
2. **Demo a GraphQL server package.** If you’ve written a great package like Join Monster, apollo-resolvers, or GraphQL Factory, you can put a live example on Launchpad and share it with the world!
3. **GraphQL server tutorials.** If you’re writing a tutorial about how to develop a certain aspect of a GraphQL server, directing your readers to Launchpad will let you skip the parts about setting up a project with Babel, installing npm packages, and writing a bunch of server boilerplate. You can just focus on the GraphQL part.
4. **Client-side examples.** If you’re creating a client-side demo using something like Expo Snack, you’ll need a server to talk to, and it’s hard to find good public APIs that don’t require auth. Since Launchpad endpoints are publicly accessible, you can easily create a specific backend for your particular client example.


The best part about Launchpad is that you’re writing regular Node.js GraphQL code. Every pad comes with a “download” button right in the UI that you, or anyone looking at your pad, can use to eject into a vanilla Node GraphQL server project. So when you direct someone to a code sample on Launchpad, they can easily use it as the base for their next project.


### Starter examples


The easiest way to get started with Launchpad and see what’s possible is to build off of an existing sample. Here are some to start you off:


- [Hello World!](https://launchpad.graphql.com/new)
- [Relational data: Authors and posts](https://launchpad.graphql.com/1jzxrj179)
- [Loading from a MongoDB database](https://launchpad.graphql.com/vkmr1kl83)
- [Star Wars API](https://launchpad.graphql.com/mpjk0plp9)


Build your own example and send a PR to add it to our list:


[https://github.com/apollographql/awesome-launchpad](https://github.com/apollographql/awesome-launchpad)


We’ll add the best ones right here in this post!


## How to use Launchpad


We’ve tried to make Launchpad as intuitive as possible. Here’s what you need to know to get coding:


### Ownership and visibility


Every pad on Launchpad is owned by the user that created it, and can only ever be edited by that owner. Every pad is also publicly visible by anyone that knows the URL. That means everyone can access code and run queries for any endpoint. It’s a demo platform, so you should not run critical production code, or anything you want to keep secret, on Launchpad.


### Secrets


We’ve included a feature called *secrets* that lets the owner of a pad put in API keys and other sensitive information, which is hidden from everyone else. So while anyone can read the code and call the endpoint, they can’t extract database passwords or API keys because they are only provided through the server-side` context` function you define in your code.


### Fork to edit


You can’t edit pads you don’t own. If you want to build on someone else’s pad, you can log in and click “Fork” to make a copy. In the process, any secret keys will be deleted, and you will need to provide your own values for those keys. For example, if you fork the MongoDB example from above, you’ll need to go to a MongoDB hosting provider like mLab, create a database, and put in the new database URL.


### npm dependencies


It’s easy to use any package from npm in your pad, just import it:


```text
import { makeExecutableSchema } from 'graphql-tools';
```


Launchpad automatically detects imports and loads the newest version of that package for you.


### Public endpoint


Once you save a project, you’ll see a` /graphql` URL in the footer. This is your pad’s public GraphQL endpoint, which can be called from anywhere. Use it to build a cool app with an online tool like[Expo Snack](https://snack.expo.io/) or[CodeSandbox](https://codesandbox.io/) !


### Downloading a project


If you want to take your project to production, you can easily eject out of Launchpad by clicking the “download” button on every pad. You can even download code from pads you don’t own! When you click the button you’ll get a standard Node.js GraphQL server project with everything you need to` npm install` and` npm start` : A` package.json` with your dependencies, an HTTP server boilerplate, and your schema code.


### Sharing examples


We’re still working on a site for browsing and sharing pads. We hope people will discover pads through links around the internet, from package READMEs, documentation, and tutorials. In the meanwhile, we’ve put up a repository where you can send a PR with a link and description for your pad to share it with the world! Discover great examples and post yours there:


### Read the[docs](https://github.com/apollographql/awesome-launchpad/blob/master/docs.md)


To get all of the details about how Launchpad works and how to use it, make sure to take a quick glance at the[docs](https://github.com/apollographql/awesome-launchpad/blob/master/docs.md) on GitHub!


---


## It’s just the beginning


There’s one thing we’ve been hearing over and over— there just aren’t enough examples of how to write custom GraphQL server code beyond hello world. How do you set up authorization and permissions? How do you connect to MongoDB, or a SQL database? How do you fetch data from an existing REST API or microservice? What is the code to set up different models for pagination?


It’s important to us that as many people can adopt GraphQL as possible, so we wanted to work empowering the community to help people with the above questions. Launchpad is the first big resource we’ve built to help people learn and discover how to develop GraphQL servers.


We’re excited to work with the community to make the platform better, and look forward to your in-depth feedback! If you’re interested in throwing around some ideas, want to contribute, or have a really cool example to share, file an issue on the[repository](https://github.com/apollographql/awesome-launchpad/blob/master/docs.md) or join us in the #launchpad channel on the[Apollo Slack](http://dev.apollodata.com/#slack) !


Huge thanks to[Danielle Man](https://medium.com/u/47b3b199a712?source=post_page-----cc4e7481fcba----------------------) and[Jonas Helfer](https://medium.com/u/39cbd38c57c8?source=post_page-----cc4e7481fcba----------------------) , who contributed significant features to the project.


### Building on Auth0 Extend


We’re going to publish an in-depth post about the design and implementation of Launchpad soon, but there’s one thing to call out especially — how crucial the[Auth0 Extend](https://auth0.com/extend/developers) team and technology was to our ability to ship this project. Extend, which you might know of as Webtask, gives us a clean and simple way to run user-provided code without worrying about all of the operational burden, and the serverless model means we can handle as many pads as people can throw at us.


---


If you’re as excited about advancing GraphQL technology and enabling developers as we are, join us! We’re actively hiring for[open source](https://www.meteor.io/jobs/#5e11e6cf-5303-4c12-a3e7-11e5f8da4be1) ,[backend](https://www.meteor.io/jobs/#b8ec842e-e79a-455e-a665-b312892d946e) , and other roles to work on next-generation API technology.


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
