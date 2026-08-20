---
schema_version: "1.0.0"
document_id: "042a8ca93a795ed35c52c22a6c041bb7a004e17db33778fa047ff3908425e5db"
company_key: "thredup-inc-class-a-common-stock"
company: "ThredUp Inc."
source_id: "thredup-inc-class-a-common-stock-rss-cddb21541ec6"
canonical_url: "https://medium.com/connect-the-dots/how-and-why-we-built-our-own-headless-cms-60373206b66f"
published_at: "2022-01-12T01:09:53+00:00"
first_seen_at: "2026-07-20T23:18:30.437987+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:8682b95e1e1978d25fcd4031065d57378008c2cc51ea8fcfca231283eac578f7"
---

# How (and why) we built our own headless CMS

# How (and why) we built our own headless CMS


[Pierre Roussel](https://medium.com/@pierre.roussel_89624?source=post_page---byline--60373206b66f---------------------------------------)


10 min read


·


Jan 12, 2022


--


*A headless CMS (content management system), is one that provides APIs to retrieve content in a universal format, usually JSON. The clients are responsible for displaying this content however they see fit.*


*An upside to this approach is that it does not tie the CMS to specific client technologies, for example native mobile clients are compatible with the response format as it does not output HTML content directly.*


## Another CMS? Why?


ThredUP needed to replace a 5+ year-old decaying CMS, the goal was to have a flexible headless CMS that allows for content targeting based on user data. In order for it to be successful it should be easy to use and allow for the previewing of content as it will appear in web or native mobile applications.


[Source: xkcd Standards](https://xkcd.com/927/)


In the world of CMSes you have a lot of options. At a high-level, there are traditional, hybrid and headless content management systems. When you focus on just headless CMSes, there are dozens upon dozens of commercial and open-source offerings. So why would we build our own?


All of these CMS solutions attempt to solve the same problem: provide a way to create and update static content in a user friendly way. This is great for simple use cases when there are no moving parts, for example blog posts, simple HTML pages and so on.


They all provide similar APIs that allow you to fetch content based on a key ( *I want this blog post* ), fetch multiple instances ( *show me the latest posts* ) and basic filtering. But when you need to create and deliver the right combination of content to the right user at the right time… at scale, you need something more.


Press enter or click to view image in full size


### What about user targeted content?


Well, you start running into issues when trying to alter the content of a page based on user information. We have benchmarked multiple CMS solutions and noticed that the filtering capabilities are too limited for complex use cases and cumbersome to use as they are meant to filter content inside collections of items.


Among the solutions we tested were “classic” headless CMSes:


- [Contentful](https://www.contentful.com/)
- [DatoCMS](https://www.datocms.com/)
- [Prismic](https://prismic.io/)


But also more visually-oriented CMSes:


- [Builder.io](https://www.builder.io/)
- [Storyblok](https://www.storyblok.com/home)


A brief example of targeting limitations using DatoCMS: let’s say I want to have a homepage with 2 variations A and B to run an A/B test. You create your models, a Homepage in our case, a[modular content field](https://www.datocms.com/docs/content-modelling/modular-content) to allow for a few limited variations of the content shape and then create your Homepage content.


You can’t add variations to this content now and there is no good way to filter your content after having created it, you might want to add tags to it and attempt to filter by tag but you’ll quickly run into issues as it requires:


- Updating the clients to add the filtering logic for the A/B test, this is especially painful for native mobile apps as they have to go through an approval process and are bound to a release cycle.
- Filtering the collections of homepages[by tags, name or any identifiable data](https://www.datocms.com/docs/content-delivery-api/filtering-records)
- Hacking your A/B test using collections of Homepages which are not intended to be used this way, this is the only way to filter down records using arbitrary data in DatoCMS


What happens after 5–10 updates to your homepages and how do you deal with your ever growing collection of homepages?


Other CMSes are similar in that clients have to pass discrete filtering criteria with each request in order to get the desired content item(s). Instead, we wanted the CMS to figure out which content to assemble and return based on user context rather than explicit filters and query parameters.


### What sets our CMS apart?


Clover, our CMS, is built using a tree-like content structure and a *reversed content filtering model* , the clients provide context with each request (user information, location in the application, etc.) and the server responds with personalized content.


Press enter or click to view image in full size


### A tale of fragments…


The building block of all the content in Clover is called a fragment, fragments are designed to be very generic to accommodate most content needs. The end goal is to be able to nest them and build larger content trees.


In many cases, we assemble fragments into a tree containing all of the content for an entire web page, but in other cases, we build smaller trees which can be embedded into subsections of a web page or mobile application.


The data structure looks like this:


Press enter or click to view image in full size


Anatomy of a Clover Page


Each fragment is associated with a component, specifically a React component, that defines how the content should be handled by a client application.


We can have visual components, layout components and logic components which can all be nested together to support complex layouts and interactions.


When fulfilling a content request, the CMS returns the content tree as a JSON blob and clients are able to construct the tree of components by recursively matching each fragment type with the proper components across platforms.


### Reversed filtering you say?


The nested structure allows a lot of flexibility, we can filter fragments at any level and A/B test any content on the page. Let’s say we need to provide two different rows of vertical merchandising blocks for logged in/out users, we can add a logic fragment above our 2 layouts and show one or the other (red and green variations in the following diagram):


Press enter or click to view image in full size


Simple A/B test setup using Clover


Logic fragments use[JSON logic](https://jsonlogic.com/) to either return one Fragment or another. These fragments are “transparent” to clients as they are stripped from the JSON response. This means the API has to traverse the tree and apply the logic before sending a response back.


Logic Fragments can be placed anywhere in the tree and can be combined together:


Press enter or click to view image in full size


A more complex logic setup


If a logged in user lands on the page and has never purchased any item from us, they would end up with a page like this:


Press enter or click to view image in full size


The main difference with other CMS providers is that user context is provided by the client. Rather than clients holding the logic to fetch the correct variation from a CMS, Clover CMS holds the logic and decides which variations to show.


Of course this approach comes with a few drawbacks as editing content trees and understanding the filtering logic is more complex than with simple pages.


### Visual Preview


To help combat this added complexity we decided to include visual previews in the editor.


Each fragment type is associated with two modes of interaction — the first is a form, made up of a set of fields, each with simple controls for authoring properties of a fragment.


The second is a preview, which uses a React component to render the fragment directly in the editor. The combination of these two modes greatly simplifies the user’s ability to create/edit and test content within the Clover management web application… well before publishing it for consumption by client applications.


Press enter or click to view image in full size


Clover’s fragment editor


On the technology side: each fragment is tied to a[JSON schema](https://json-schema.org/) , the schemas are generated from React components interfaces using a Typescript JSONSchema generator.


*We are using Typescript in all node services and front-end web apps as it’s the best way to enforce types in Javascript. It manages to catch all potential type errors before the code is even run when used properly.*


The renderer used in the Fragment preview is shared across multiple apps, it is bundled as a separate npm package that is then imported into web clients and CMS. This allows the CMS preview to be a one-to-one match with what the web clients will eventually render.


Here’s what the process looks like:


The benefits of this approach are multiple, we can:


- Automatically generate forms based on JSON Schemas
- Tie the forms to a visual preview
- Validate the form data on client and server side using a JSON Schema validator such as[ajv](https://github.com/ajv-validator/ajv)


### Native mobile app support


We want our CMS to be cross platform and tying our components to React props doesn’t seem like a good idea…


So, for native mobile app support we created mock components to show a visual preview of the native components in the CMS while editing.


We sadly can’t share rendering code between Clover’s management application, Android and iOS native clients, since we’re not using React Native for mobile apps, so this solution is still a bit brittle but provides a good visual preview:


Press enter or click to view image in full size


Press enter or click to view image in full size


CMS Preview (Typescript) / Native Android App (Kotlin/Java)


## Caveats


There are a few caveats to reversing the filtering:


- Performance: Caching becomes really hard to do as you can’t cache based on query based keys (each query will look different to the CMS)
- UX: All the logic resides on the CMS and end-users need to build the targeting using the tools available


### Performance


When releasing content we denormalize the content tree and store it as a JSON blob. This avoids having to recursively find the next fragment in the database at runtime to rebuild the tree. Another benefit of this approach is that it also allows us to properly version a release as we end up with a static snapshot of the content tree.


When receiving a client request the API will then retrieve the correct content based on a key, and apply the logic based on the user’s context, a set of graphQL inputs. This means traversing the tree and applying the logic contained in logic fragments before returning it to the clients.


We initially thought this recursive tree traversal could mean longer response time, this assumption turned out to be wrong and we haven’t seen any performance problems with our API.


The tree traversal takes less than a millisecond:


Press enter or click to view image in full size


P95 Latency is also very good, mostly hovering around 5ms:


Press enter or click to view image in full size


There is a 10x speed difference between our node JS server responding to a request and running the tree traversal logic, most of the time is spent running the database query.


### UX Problems


How to provide a good user experience when building content and adding logic?


A crucial part of Clover is the ability to show different arrangements of content based on criteria. This core part of the CMS is handled by the logic fragment.


In the UI we use a logic builder to generate valid JSON Logic:


Press enter or click to view image in full size


The corresponding JSON logic that will run in the API is:


```text
{    "and": [      { "==": [{ "var": "user.loggedIn" }, true] },      { "in": ["hello", { "var": "query.utm_campaign" }] },      { "==": [{ "var": "user.hasPurchasedOrder" }, true] }    ]  }
```


Instantaneous feedback is also very important for the user experience. When editing logic the visual preview will update instantly to conform to the logic changes made.


You can set various flags and simulate a page for different users to see how it would look:


Press enter or click to view image in full size


Here is what a basic setup looks like:


Another challenge with the tree structure is helping the users build a mental model for the current page they’re working on.


To guide designers working with the CMS we have added tools to show the non-visual layout and logic fragments for a given content tree:


Press enter or click to view image in full size


All layout fragments and logic fragments are shown when the wireframe view is enabled, this also allows for quickly diving into a specific fragment to make changes.


## Conclusion


Clover CMS is now powering multiple sections of our website and native mobile applications, the main ones being the homepage (web) and featured screen (native).


Presently there are roughly 40 variations of homepage and featured screen content trees, each targeted to specific user context attributes.


By making content authoring and testing easier, Clover has enabled us to reduce the time and effort to setup and test these complex content campaigns by more than 75% on average. So while there are lots of CMSes available to choose from, building our own to suit our unique needs is paying off.


We continue to enhance capabilities and ease-of-use as usage increases, and we expect to reap even more benefits from this home-grown solution.


Thanks for reading!
