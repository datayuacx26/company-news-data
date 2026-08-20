---
schema_version: "1.0.0"
document_id: "e80543a10e6a4e01b5a43a2991233f5be80a4e6c88475e5d1135f3c5887d8c81"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-server-1-0-a-graphql-server-for-all-node-js-frameworks-2b37d3342f7c"
published_at: "2017-07-18T18:21:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:dae7fdadd7ecc3edb239dd320eb046320e16c3ebbbba104d28007d172fcbe0e4"
---

# Apollo Server 1.0 — A GraphQL server for all Node.js frameworks

Today, after more than a year of development, testing and production use, we’re proud to announce the 1.0 release of[Apollo Server](https://github.com/apollographql/apollo-server) ! 🎉


Late edit: We also just announced[Apollo Optics is now free for most apps](https://dev-blog.apollodata.com/apollo-optics-is-now-free-up-to-1-million-graphql-requests-a-month-d759248ea084) , up to 1 million queries per month![Try out Optics](http://apollodata.com/optics) to visualize, monitor, and track your GraphQL API.


## Apollo Server principles


Apollo Server is a community-maintained, open-source GraphQL server. It works with all Node.js HTTP server frameworks: Express, Connect, Hapi, Koa, AWS Lambda, Restify and Micro. It is built on top of the` <a href="https://github.com/graphql/graphql-js" target="_blank" rel="noreferrer noopener">graphql-js</a>` reference implementation.


Apollo Server is built with the following principles in mind:


- **By the community, for the community** : Apollo Server’s development is driven by the needs of developers.
- **Simplicity** : by keeping things simple, Apollo Server is easier to use, easier to contribute to, and more secure.
- **Performance** : Apollo Server is well-tested and production-ready — no modifications needed.


Those were the principles that we laid out when the project started, and I’m happy to see that the principles still hold today.


## Zero to 4K


The development of Apollo Server has been driven by the community from the very beginning, and it’s been really exciting to watch. In a little more than a year, it went from zero to[over 600 commits by 45 contributors](https://github.com/apollographql/graphql-server/graphs/contributors) , and NPM downloads have grown to[almost 4K per day](https://npm-stat.com/charts.html?package=graphql-server-core&from=2016-10-01&to=2017-09-30) !


NPM downloads for Apollo Server


Among the production users, there are a couple of names that you might recognize:


- [Credit Karma](https://www.creditkarma.com/auth/logon/)
- [Ticketmaster](http://www.ticketmaster.com/)
- [OpenTable](https://www.opentable.com/)
- [Mozilla’s Coral project](https://coralproject.net/) (with NYTimes & Washington Post)
- [express.com](https://www.express.com/)
- [Meteor](https://www.meteor.com/) &[Apollo](http://www.apollodata.com/)


Those are just the ones that we know about, but I’m sure there are many others. If you’re using Apollo Server in production and your company wasn’t mentioned, let us know in the comments!


If you’re not using Apollo Server just yet but want to give it a try, read the next section to learn how to set it up.


## How to get started


If you want a more in-depth introduction for how to set up a GraphQL server and connect it to SQL, Mongo or REST backends, I highly recommend doing our recently updated tutorial:[How to Build a GraphQL Server](https://dev-blog.apollodata.com/tutorial-building-a-graphql-server-cddaa023c035) . It is by far the most viewed post on our blog!


A note for those who’ve used Apollo Server previously: You may notice we now import from` apollo-server-` rather than` graphql-server-` npm packages. We felt the rename better reflects that it’s part of the Apollo project and family of libraries.


Here’s how easy it is to use Apollo Server once you’ve built a GraphQL schema (for example with[graphql-tools](https://dev-blog.apollodata.com/graphql-tools-a-simpler-way-to-create-graphql-apis-eadf018f3766) ).


```text
import express from 'express';
import bodyParser from 'body-parser';
import {
graphqlExpress,
graphiqlExpress
} from ' apollo-server-express  ';


const myGraphQLSchema = // ... define or import your schema here!
const PORT = 3000;


const app = express();


// bodyParser is needed for POST requests.
app.use('/graphql', bodyParser.json(),  graphqlExpress  ({ schema: myGraphQLSchema }));// GraphiQL is reachable on http://localhost:3000/graphiql
app.use('/graphiql',  graphiqlExpress  ({ endpointURL: '/graphql' }));app.listen(PORT);
```


This example uses Express, but it’s almost the same for all other integrations. Check the[docs](http://dev.apollodata.com/tools/graphql-server/index.html) or the[readme](https://github.com/apollographql/graphql-server) for how to set up Apollo Server with Hapi, Koa, Lambda, Restify or Micro!


## A huge ‘Thank You’ to everyone who contributed!


The 1.0 release of Apollo Server would never have been possible without the hard work of all the contributors. There are two people whose contributions really stand out: **Hagai Cohen** and **Nick Nance.**


Nick was there when we laid the foundation for what became Apollo Server, and he built most of the Hapi and Micro integrations. If it wasn’t for him, this project may never have gotten started. Thank you Nick!


Hagai came in a bit later and turned Apollo Server into what it is today. He migrated the project to Lerna, refactored large parts of the codebase and was the project’s chief maintainer for more than 6 months. Without him, Apollo Server wouldn’t be nearly as good as it is today. Thank you Hagai!


There were many other contributors over the course of the last year, whose big and small contributions made a difference. Thank you[stubailo](https://github.com/stubailo) ,[NeoReyad](https://github.com/NeoReyad) ,[HriBB](https://github.com/HriBB) ,[nicolaslopezj](https://github.com/nicolaslopezj) ,[Urigo](https://github.com/Urigo) ,[dotansimha](https://github.com/dotansimha) ,[conrad-vanl](https://github.com/conrad-vanl) ,[spicydonuts](https://github.com/spicydonuts) ,[timneutkens](https://github.com/timneutkens) ,[creatorkuang](https://github.com/creatorkuang) ,[Tallyb](https://github.com/Tallyb) ,[kamilkisiela](https://github.com/kamilkisiela) ,[soda0289](https://github.com/soda0289) ,[kievechua](https://github.com/kievechua) ,[rdrey](https://github.com/rdrey) ,[n1mmy](https://github.com/n1mmy) ,[NeoPhi](https://github.com/NeoPhi) ,[trotzig](https://github.com/trotzig) ,[jadkap](https://github.com/jadkap) ,[RafaelCosman](https://github.com/RafaelCosman) ,[papandreou](https://github.com/papandreou) ,[ClaudiuCeia](https://github.com/ClaudiuCeia) ,[phra](https://github.com/phra) ,[nevir](https://github.com/nevir) ,[benjamn](https://github.com/benjamn) ,[pradel](https://github.com/pradel) ,[xeoneux](https://github.com/xeoneux) ,[dahjelle](https://github.com/dahjelle) ,[siegesmund](https://github.com/siegesmund) ,[sammkj](https://github.com/sammkj) ,[od1k](https://github.com/od1k) ,[pcarrier](https://github.com/pcarrier) ,[Dattaya](https://github.com/Dattaya) ,[larsw](https://github.com/larsw) ,[AlexLeung](https://github.com/AlexLeung) ,[alanchristensen](https://github.com/alanchristensen) ,[Chris-R3](https://github.com/Chris-R3) ,[ephemer](https://github.com/ephemer) ,[thebigredgeek](https://github.com/thebigredgeek) ,[comus](https://github.com/comus) ,[blakeembrey](https://github.com/blakeembrey) ,[omegablitz](https://github.com/omegablitz) !


## Where we go from here


Apollo Server is a community project, so if you have ideas or find a bug then don’t be shy filing issues or opening pull requests on GitHub!


We’re always looking for more contributors and maintainers, so if you think that could be something for you, let us know in the #contributors channel on the[Apollo Slack](http://www.apollodata.com/#slack) or directly on[GitHub](https://github.com/apollographql/graphql-server) !


---


*If you liked this post and want to hear more about Apollo and GraphQL, make sure to click the “Follow” button below, and follow us on Twitter at*[@apollographql](https://twitter.com/apollographql) *and*[@helferjs](https://twitter.com/helferjs) *.*


Written by


Jonas Helfer


[Read more by Jonas Helfer](https://www.apollographql.com/blog/author/jonas)
