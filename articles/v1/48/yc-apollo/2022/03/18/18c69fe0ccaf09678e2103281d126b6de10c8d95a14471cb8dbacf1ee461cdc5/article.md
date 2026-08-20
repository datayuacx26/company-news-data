---
schema_version: "1.0.0"
document_id: "18c69fe0ccaf09678e2103281d126b6de10c8d95a14471cb8dbacf1ee461cdc5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/save-and-share-your-graphql-operations-in-apollo-explorer"
published_at: "2022-03-22T08:06:18+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:e07fd4db31c9c57640161f4adc56ad25c037c3c4f91e60234116827d14141f6f"
---

# Save and share your GraphQL operations in Apollo Explorer!

Last year on popular request, we brought[tabs](https://www.apollographql.com/blog/tooling/graphql-ide/were-shipping-3-of-our-most-requested-features/#1-tabs-in-explorer) to Explorer. And today, we are taking it up another notch! You can now save your operations in Explorer and share them with your team using Operation Collections!


## What is an Operation Collection?


Operation Collections are a new, **free** feature in Apollo Studio that help you save and organize operations in folders that align to different use cases, client apps, personas and more.


Saving your operations into a collection in Explorer means that you and your team can:


- Stop writing the same operation over and over
- Organize common operations by how they’re used and who uses them
- Design, execute, and share operations all in a single interface
- More easily onboard new team members


Let’s see how!


## Creating a collection


To get started, head over to the Explorer in Apollo Studio and select any graph/variant combination that you want to create a collection for. Now let’s create a new collection:


1. In Explorer on the left panel, click on the bookmark icon and click on` +Create collection` .
2. A collection can be created as Personal or Shared. Personal collections are only visible to you while shared collections are visible to all Studio org members who have access to that variant. Configure collection details in the modal and hit Add.
3. That is it! You should see your newly created collection in the Operation Collection index 🥳


## Saving an operation


Now let’s save an operation to the newly created collection:


1. Navigate to any tab in the Explorer with an operation, or build a query using the Explorer’s query builder.
2. Click on the save icon in the editor pane, left of the run button. You can also use the keyboard shortcut of` Cmd/Ctrl+S` .
3. Give the operation a name, select the collection to save in, and hit Save!
4. Your operation is now saved and available for access within that collection 🎊


## Sharing a collection


A collection created as Shared for a graph/variant is automatically visible to every member of the Studio organization who has access to that graph/variant. Shared collections are editable by any graph or org admin. Optionally, editing settings can also be given to other roles in the org.


⚠️ **Note:** If you are an enterprise user, you can configure granular[role](https://www.apollographql.com/docs/studio/org/members/) level permissions, otherwise you can choose whether you want to grant edit permissions to all org users.


But that’s not it! Here are a few ways to make it even easier for your team to find shared operations:


### **1. Share a link**


You can also give team members a link directly to a particular shared collection or operation. To do so, click the` •••` button next to any shared collection or operation in the list and select the Copy Link option.


### **2. Embed in your graph’s README**


Have you added a README for your graph yet? If not, no better time like the present! We added the[README page](https://www.apollographql.com/blog/announcement/platform/create-a-readme-to-onboard-developers-to-the-graph/) last year to make it easy for graph producers to onboard new developers on their graphs.


Now you can embed saved operations and collections directly into your README:


1. Click on the edit icon on the README.
2. Using the graph shortcodes, select a saved operation or a collection to embed.
3. Check out the preview to make sure everything looks good, and then click Update.
4. Tada! 🎉 With a few clicks you’ve added an example set of operations to the README.


Now any developer looking to explore the graph can get started right away!


### **3. Create a collection for a public graph**


If you have a[public graph](https://www.apollographql.com/blog/announcement/tooling/make-your-graphql-schema-publicly-visible-without-introspection/) , you can publish a collection of example operations to help developers accessing the public graph onboard quickly. Any developer accessing the public graph can view and execute operations in a public collection, without having to login!


⚠️ **Note:** Unauthenticated users do not have edit access for collections


### Collections in Sandbox


Now you might be thinking, “this is great! But I use Sandbox for my local development. Can I save my operations in collections in Sandbox too?” **Yes you can!**


In fact, not only can you save operations in collections across all of your Sandbox endpoints, you can also access collections for any registered graph that you have access to! That means that when you’re using Sandbox to build or make changes to a registered graph in Apollo Studio, you can test out operations in collections for the registered graph and edit them directly as you are making your schema changes!


## And a few more things…


You thought that was it? Not quite.


**After you have created a collection you can:**


- Edit its configuration.
- Duplicate the collection between graphs, including between sandbox and registered graphs.


- You can also choose to make the copy public or shared.
- Duplicating a collection does not keep the copy in-sync with the original.


- Mark it as favorite, to keep it at the top of the list. ⭐


**After you have created an operation you can:**


- Rename the operation.
- Save a copy of the operation.
- Drag and drop the operation between collections!


## So what are you waiting for? Save and share your operations today!


Head over to Apollo Studio or Apollo Sandbox, and save, organize and share your frequently used operations today! And as always, we love hearing from you, so if you have any feedback, please fill out this[feedback](https://forms.gle/CbY8mEp5vK7hgYgA9) form or drop a post on the[Apollo Studio GitHub Community](https://github.com/apollographql/apollo-studio-community/issues) !


⚠️ **Note:** If you do not see Operation Collections in your Explorer, make sure you are on the latest version of the UI. Click on the bell icon at the top right corner of Studio or Sandbox and if you see *“Update available, click to relaunch”* at the bottom left of the modal then click that to get the latest version.


Written by


Parul Schroff


[Read more by Parul Schroff](https://www.apollographql.com/blog/author/parul)
