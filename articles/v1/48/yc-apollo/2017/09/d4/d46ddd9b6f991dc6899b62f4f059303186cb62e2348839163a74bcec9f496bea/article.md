---
schema_version: "1.0.0"
document_id: "d46ddd9b6f991dc6899b62f4f059303186cb62e2348839163a74bcec9f496bea"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-first-in-practice-at-quri-7bf84b260135"
published_at: "2017-09-25T00:01:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:81c661603a504f7af892395d79b59b259ba8bcb28dff67f1ba319545b4dc4a93"
---

# GraphQL First in practice at Quri

GraphQL First, or “schema first”, development is a concept that was unveiled about a year ago when the Apollo Dev team first published an[article](https://dev-blog.apollodata.com/graphql-first-a-better-way-to-build-modern-apps-b5a04f7121a0) about it and then described it in detail at[GraphQL Summit](https://summit.graphql.com/) in San Francisco:


At[Quri](https://quri.com/) , we have been betting big on GraphQL since early 2016, and that philosophy was just another “Aha!” moment for us.


The concept is rather simple: before starting any implementation, the different stakeholders (usually front-end and back-end developers) look at the UI mockups and how the data would be accessed, and then draft a schema proposal either from scratch or based on the existing types. From there, since the *contract* is settled, *in theory* the back-end and front-end teams could work independently. Here are a few key slides from[Danielle Man](https://medium.com/u/47b3b199a712?source=post_page-----7bf84b260135----------------------) ’s presentation last year that demonstrate that workflow:


Developing new product features


After a little experimentation at Quri, we were able to introduce a GraphQL-based development workflow that has front-end devs and back-end devs building features in parallel. Read on to see how we got there, and what that workflow looks like in the real world.


## Putting GraphQL First into practice


Since this idea was first introduced, there has been only a[few](https://www.youtube.com/watch?v=SdWI7XaAeeY) posts about how GraphQL First works in practice. We tried a few things — one of them was to run a mocked version of our schema with[graphql-tools](https://github.com/apollographql/graphql-tools) and a copy of the schema. It didn’t really work since we weren’t able to build a process around it, and we found it hard to keep the *copy* of the schema in sync with our actual, constantly evolving schema.


Most of the mocking solutions we found were in[javascript](http://dev.apollodata.com/tools/graphql-tools/mocking.html) . We were able to experiment with them, but it was not a viable option for our development workflow since we use[graphql-ruby](http://graphql-ruby.org/) . There wasn’t a common language the back-end and front-end developers could use.


### Introducing[graphql-faker](https://github.com/APIs-guru/graphql-faker)


About two months ago, my colleague Prasanna found about[graphql-faker](https://github.com/APIs-guru/graphql-faker) . It is a CLI tool that allows you to extend an existing schema by specifying its endpoint and then writing the new types in the[GraphQL IDL Language](https://www.graph.cool/docs/faq/graphql-sdl-schema-definition-language-kr84dktnp0/) :


Being able to proxy an existing endpoint is one thing, but this tool went a step further to make it a great experience. You can:


1. Run the tool with a simple command
2. Extend the types in one tab of the browser
3. Switch tabs
4. Explore the schema with Graph *i* QL
5. Run queries against it
6. Get results with both real data and realistic mocked data (utilizing[faker.js](https://github.com/Marak/faker.js) )


We experimented with it, and since the results were positive we decided to adopt it and include this tool in our development process.


## Everyday GraphQL First in action


Here’s an overview of how this process operates at Quri. First, we’ve created a GitHub repository named` quri/schema-extensions` (I created a public[example repository](https://github.com/chollier/schema-extensions) , just fork it!).


In there, when either a front-end or back-end developer wants to suggest some changes to the schema, we create a pull-request that adds a` <a href="https://github.com/chollier/schema-extensions/blob/master/example.faker.graphql" target="_blank" rel="noreferrer noopener">feature.faker.graphql</a>` file. We can then use the normal GitHub review process to provide feedback, approval or ask for some modifications. Both the front-end and back-end teams have to approve the changes for it to be merged and then implemented.


Let’s have a look at that development workflow. For that use case, we will assume I’m an engineer looking at some product specifications: we want to add to the user page a profile picture. We should be able to see a thumbnail, and, when clicking on the picture, the full-size image.


1. First I would create a new extension file` user-avatar.faker.graphql` in the repository.
2. Then I would run the project:` npm start --open --extend http://localhost:3000/graphql/queries --header "Authorization:Bearer \[OAuth token\]" ./user-avatar.faker.graphql` (This assumes my GraphQL endpoint is running on localhost and has some Authentication requirements.)
3. Then we can start writing the type extensions in the browser:


Here I’m creating an Avatar type that contains two field: originalUrl and thumbnail64Url — it is possible with graphql-faker to use realistic data.


4. By switching tab on the left side, I can now visualize these additions in Graph *i* QL:


5. Finally, I can write a query against this schema:


Utilizing graphql-faker integration with faker.js I was able to provide a different set of mocked data for the two fields


6. The last part of the process would be to open a pull request, ask for feedback, incorporate it, and, when a consensus is reached, merge it. For instance, someone might suggest we use some real world examples:


```text
type Avatar {
# The original avatar URL
originalUrl: String @fake(type:avatarUrl, options: { imageCategory: people })
# The thumbnail resized to 64x64px
- thumbnail64Url: String @fake(type:imageUrl, options: { imageWidth: 64, imageHeight: 64, randomizeImageUrl: true })
+ thumbnail64Url: String @examples(values: ["http://example.com/image.jpg", "http://example.com/image2.jpg"])
}
```


There is one gotcha: since it is not possible to override types (you can only extend them) as the schema changes get incrementally worked on, we need the team to make pull requests to remove the types and fields that just got implemented from the` .faker.graphql` extension file.


---


With this workflow, not only have we improved our feedback loop by making sure every voice is heard during the design process, but we also got a huge gain in productivity and reduced sequencing: once the API contract is settled, the front-end team can start implementing components without the back-end even having started to implement it.


*Thanks to*[Pol Miro](https://medium.com/u/c62b9dac8567?source=post_page-----7bf84b260135----------------------) *and Prasanna, without whom we wouldn’t have gone this far with this project, and the entire Quri engineering team for contributing to it.*


Written by


Loïc Chollier


[Read more by Loïc Chollier](https://www.apollographql.com/blog/author/chollier)
