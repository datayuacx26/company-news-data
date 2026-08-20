---
schema_version: "1.0.0"
document_id: "346562423215f1827546bfc82519272cfe924384c124154087c769a56b5a6a22"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/a-new-apollo-docs-experience-5645b9d56260"
published_at: "2019-03-29T21:55:02+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:f029abbe3f57ec3d4f6ff776f06c1afb830dc2aea949e9df52ae773050ed8cb9"
---

# A new Apollo docs experience

As developers, we constantly interact with documentation. It’s what welcomes us to a project for the first time, it’s a trusted companion that guides us toward celebrating our first “Hello World,” and it’s the first place we turn when we’re frustrated with a bug or curious about a new feature.


Good documentation is at the heart of crafting an excellent developer experience, which is why we’re making a big investment in improving our docs at Apollo. Today, we’re excited to share the first big milestone on our journey: **Apollo’s**[new docs site](https://www.apollographql.com/docs/) **built with Gatsby!** 🚀


## From Hexo to Gatsby


Previously, our docs site was built with Hexo, a static site generator, and shared its theme with Meteor. Although we love our Meteor roots, the shared theme was becoming difficult for our team to maintain and evolve over time.


We wanted to migrate to React so we could share UI across all Apollo sites and add interactive elements to our docs. While I wouldn’t normally advocate for a rewrite, our team decided it was necessary in order to modernize our stack. Both our[homepage](https://www.apollographql.com/) and the[GraphQL Summit site](https://www.apollographql.com/) are built with[Gatsby](https://www.gatsbyjs.org/) , so it was an easy choice for our new docs infrastructure.


### Gatsby themes


With nearly 200 repositories in the[Apollo organization on GitHub](https://github.com/apollographql) , it’s important that each project’s documentation lives alongside the code. When we first heard about[Gatsby’s new themes feature](https://www.gatsbyjs.org/blog/2018-11-11-introducing-gatsby-themes/) , Apollo engineer[Trevor Blades](https://medium.com/u/e9878a0ec91c?source=post_page-----5645b9d56260----------------------) quickly built a proof-of-concept that confirmed it would be perfect for our needs.


Gatsby themes allow you to easily compose UI, configuration, and functionality across multiple sites. Themes are distributed as npm packages, and we use them for sharing versioning logic, constructing the left-hand navigation, common layout components, and more. Not only are Gatsby themes awesome for building docs sites, they’re also great for quickly spinning up derivative sites like[Principled GraphQL](https://principledgraphql.com/) , which was built by extending` <a href="https://github.com/apollographql/gatsby-theme-apollo/" target="_blank" rel="noreferrer noopener">gatsby-theme-apollo</a>` .


We’ve been really happy with themes so far, and can’t wait to explore them further. Thank you to[Christopher Biscardi](https://medium.com/u/8cb9e0fea905?source=post_page-----5645b9d56260----------------------) and the rest of the Gatsby team for all their support along the way! 🤗


## Focusing on discoverability


When designing our new UI, we wanted to focus on making our content more discoverable. Previously, our layout displayed too much information on the left sidebar, which made it difficult to find what you were looking for without scrolling. Apollo designer[Jess Marina](https://medium.com/u/12880b6da5ba?source=post_page-----5645b9d56260----------------------) rose to the challenge and created a cleaner layout and navigation structure to solve this pain point.


Before: Our old docs site was difficult to navigate.


After: The content on our new docs site is easier to discover and read.


### Cleaner navigation 🗺


For our new docs site, we removed article details from the left sidebar and added a navigation panel to the right of the screen to show users all the topics that would be covered on a page. We also made each section on the left sidebar expand and contract so users could zero in on the pages that are relevant to them.


### Prominent search 🕵️‍♀️


Since our docs site spans a large breadth of content, we wanted to improve the search experience. We moved the search bar to a more prominent location, redesigned the search menu, and added a keyboard command to trigger search.


## Looking to the future


We’re happy with the progress we’ve made toward improving the[Apollo docs](https://www.apollographql.com/docs/) experience, but our work isn’t done yet! Over the next few months, we will be working on a refactor of our information architecture in order to help developers find what they’re looking for faster. Before embarking on this project, we will conduct user testing and publicize a plan to the Apollo community for feedback. If you’re interested in contributing, check out our[Docs channel on Spectrum](https://spectrum.chat/apollo/docs) to learn more.


With our new infrastructure, we’ll also be able to support[MDX](https://mdxjs.com/) , which will allow us to render React components alongside markdown in the same file. We’re super excited about the creative possibilities MDX will enable, especially with integrating more interactive elements into our docs. Some ideas we’ve started to explore include: interactive code blocks that compile TypeScript to JavaScript, expansion panels with step-by-step instructions, and embedding GraphiQL for running queries.


---


We can’t wait to see what the future holds for Apollo docs, and we hope you enjoy exploring the new site! If you have any feature requests or feedback on the site, please let us know by filing an issue in the[Gatsby theme repo](https://github.com/apollographql/gatsby-theme-apollo) .


Written by


Peggy Rayzis


[Read more by Peggy Rayzis](https://www.apollographql.com/blog/author/peggy)
