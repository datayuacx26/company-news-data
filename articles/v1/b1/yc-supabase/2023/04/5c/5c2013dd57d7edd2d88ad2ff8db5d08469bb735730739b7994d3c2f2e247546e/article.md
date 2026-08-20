---
schema_version: "1.0.0"
document_id: "5c2013dd57d7edd2d88ad2ff8db5d08469bb735730739b7994d3c2f2e247546e"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-studio-2.0"
published_at: "2023-04-14T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:8e2077dadecd083e912f4b1b2f9e9469d38aae80d227e823855cc45e68b48822"
---

# Supabase Studio 2.0: help when you need it most

Today we're announcing Supabase Studio 2.0, packed with a ton of new stuff. We have features people have been asking for forever, and new capabilities that will change the way you work.


Here's the birds-eye-view:


-


Supabase AI : ChatGPT, right in the Studio


- GraphiQL : Query your database with GraphQL
- Cascade deletes : Our #1 most requested feature from the community
- Query Performance : Investigate slow running queries
- Foreign key Selector : Look up a row in reference table
- Postgres Roles : Manage your Postgres roles and privileges
- Database Webhooks : Database triggers for Edge Functions
-


Table/View definitions : View SQL definitions for tables and views


- API Autodocs : View auto-generated docs right from the Table Editor
-


Support for many tables : Table Editor now supports 1000s of tables


- JSON editing : Improved JSON editing
- Nullable columns : Allow text/bool cells to be null or empty


## Supabase AI — assisted database development#


One of our guiding principles at Supabase is to make SQL more accessible to developers. We don't want to just abstract it away with a custom implementation. By embracing SQL, developers learn important, transferable skills while they build on top of Supabase.


In the past few months, AI advancements have made this easy! Writing a complex SQL query is now as simple as asking ChatGPT, and we're leaning into this approach with our Studio. AI gives developers superpowers, and by providing relevant context, Supabase gives AI superpowers.


AI has already changed a lot about the way we work as developers. We can use ChatGPT to write code for us as fast as we can prompt it. We wanted to bring this power into the Studio with integrations that will help you work even faster than before.


Today you'll be able to do many common SQL operations with the help of AI. You can create tables, views and indexes, write functions and triggers, and more, right from the ⌘K menu.


Soon, we'll also let you opt-in to sending your table schemas to OpenAI to help fine tune queries to your project. All this should result in a dramatic boost in development speed. Maybe you'll only need half a weekend to scale to millions now!


As with every ⌘K menu, quick navigation is at the heart of things. You'll now be able to jump to any page in the Studio in a couple of keystrokes. You can also search the Docs directly from the menu.


We'll be actively working on our ⌘K menu in the coming months to make it faster and even more useful for you. Next, we're going to focus on better support for writing RLS policies. Stay tuned, and make sure to explore how we're integrating AI[behind the scenes](https://supabase.com/blog/tags/AI) !


Huge props for the amazing work done on the[⌘K package](https://cmdk.paco.me/) we've built on top of here.


---


Along with the new AI features, we also doubled-down on some of the critical missing pieces for a Postgres UI. Before we started to work on any of these, we combed through user feedback and tallied up the most submitted Feature Requests on GitHub.


📢 Many of the features and enhancements below came from user requests —[please keep them coming](https://github.com/orgs/supabase/discussions/categories/feature-requests) !


## GraphiQL#


While GraphQL has been available in Supabase for just over a year, we haven't provided a visual tool for using it. You can now use the very popular GraphiQL directly from the Studio. GraphiQL is a browser tool for writing, validating, and testing GraphQL testing. Being able to use a fully integrated GraphQL IDE is a huge DX win.


## Cascade deletes#


Cascade deletes are a core feature of Postgres, but have not been available in the Studio UI until now. This has been the #1 feature request from the community for a long time. You can now choose what you want to happen when deleting a referenced row in another table, right from Table Editor side panel.


## Query Performance#


We've just released a new Query Performance tool to help you identify slow queries. Using this tool, along with our new[guide](https://supabase.com/docs/guides/platform/performance#examining-query-performance) should help you speed things up. These tools can help you uncover where you might have inefficient queries or schemas, or where you might need indexes or even additional compute resources.


## Foreign key selector#


You can now select a referencing row from another table, rather than having to pass a value manually. You can also quickly jump to the row being referenced in another table with the *View referencing* record button from the grid itself.


## Postgres Roles#


The Roles page got a huge revision, making it easy for you to manage access control through users, groups, and permissions. It's now easy to see how many connections you're using and where they're coming from.


## Database Webhooks#


We've wanted to improve the webhooks interface for a long time, and we've finally gotten to it. We now have full support for editing your webhooks, and you can now select an edge function to call instead of having to pass in a url. We're happy to roll this very common request out to the community.


## Table/View definitions#


You can now see the SQL definitions for your tables and views, directly from the Table Editor. This is a great way to see how Supabase is translating your UI changes into SQL. This can be useful for creating migrations, getting help, or just for learning more about how Supabase and SQL works.


## API Autodocs#


You can now view the auto-generated API docs right from the Table Editor. Grab the SQL and` supabase-js` code for all of the CRUD operations, straight from the table. You can also quickly generate and download a Typescript types file for your table, right from the editor.


### Quality of life improvements#


#### Support for many tables#


Previously, the Table Editor would get slow and unresponsive when you had many tables. We've made a number of improvements to make it much faster and more responsive. Feel free to make all the tables you need!


#### JSON editing#


You've always been able to use the JSON data type in your tables, but editing the data wasn't easy. We've improved the inline Table Editor, and also now allow you open json cells from the side panel for a more spacious editing experience. Next Launch Week we're hoping to decide if it's pronounced “Jason” or “Jay-sawn”. Stay tuned.


#### Nullable columns#


Speaking of extremely common feature requests, we've gotten this one a lot in the past few months. You used to have to handle this manually, but now you can now allow text/boolean cells to be null or empty. Supabase, where productivity is more than just an empty (or null) promise.


## Wrapping Up#


We hope you get a lot of value out of these new features and enhancements. As we mentioned earlier, many of the features listed here came directly from[Feature Requests](https://github.com/orgs/supabase/discussions/categories/feature-requests) on GitHub. Thanks to everyone who have taken the time to submit these, and encourage submissions for anything else you'd like to see.
