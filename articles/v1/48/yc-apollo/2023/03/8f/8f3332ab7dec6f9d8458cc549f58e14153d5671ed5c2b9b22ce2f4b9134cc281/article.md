---
schema_version: "1.0.0"
document_id: "8f3332ab7dec6f9d8458cc549f58e14153d5671ed5c2b9b22ce2f4b9134cc281"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/automate-graphql-query-testing-in-graphos-explorer"
published_at: "2023-03-07T08:32:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:509a47c0610ad9911ccfe5eb46e17ac6ba28276055d3b336df39ecbcfae66a95"
---

# Automate GraphQL query testing in GraphOS Explorer!

Almost a year ago we introduced[operation collections](https://www.apollographql.com/blog/announcement/platform/save-and-share-your-graphql-operations-in-apollo-explorer/) in GraphOS as a way to save your frequently used operations in Explorer and share them with your team. Today, we are making saved operations even more useful with operation scripts! Now, you can write scripts for your saved operations to build automated testing workflows that call other saved operations.


## **What are operation scripts?**


Operation scripts are a brand new, **free 🎊** feature in GraphOS Studio that let you write scripts that run before your operation to execute JavaScript code, make HTTP requests, and run other saved operations before your current operation. You may think this sounds a lot like what you can do with[preflight scripts](https://www.apollographql.com/blog/announcement/tooling/automatically-authenticate-with-preflight-scripts-in-apollo-studio-explorer/) , and you would be correct! An operation script, at a high level, is a preflight script that is defined *per operation* instead of *per variant* .


With operation scripts you and your team can:


- Easily debug the behavior of individual operations when they’re provided various values
- Design and write scripted workflows with your saved operations for testing


## **Adding an operation script**


Adding operation scripts to your saved operations is easy. To get started, head to Explorer in GraphOS Studio, open up the Operation Collections tab on the left panel, and open the operation you would like to write a script for. In the bottom pane, you’ll see a new tab named Script.


Click on the “Add Script” button to open up the operation script editor panel. To help you start writing your script, there are code snippets to the left of the editor. Clicking on the plus icon next to a snippet will insert it in the editor.


To run your operation script with your current operation, just save your operation and turn on the operation script setting within Explorer Settings > Personal Settings.


ℹ️ **Note** : Operation scripts are not only limited to GraphOS Explorer, they can also be created in Sandbox and within Explorer or Sandbox embeds!


## **Calling saved operations with runOperation**


In your script, you can also run *other* saved operations from any variant of your graph against your current endpoint using the` runOperation` command. This allows you to use values from the output of those operations as GraphQL variables for your current operation.


Below is an example of using` runOperation` to get a list of ships from the SpaceX GraphQL API and then passing the ID of the` firstShip` to an environment variable.


```text
const response = await explorer.runOperation({
scope: 'shared',
graphRef: 'SpaceX-pxxbxen@current',
collectionName: 'Mission Control',
operationName: 'Ships',
})


const firstShipID = response.result.data.ships[0].id


explorer.environment.set('shipID', firstShipID)
```


From here you can use any environment variables set in your script by passing them into the GraphQL variables panel. For instance, if I had this script set on a mutation that takes in a` shipID` and then sets the` status` of the ship to` Launched` , I would use the following GraphQL variables in my variables panel:


```text
{
"shipID": "{{ shipID }}",
"status": "Launched",
}
```


When you run your operation, it will run your script, populate your variables, and then run the current operation. Amazing! Now you can build operation workflows without you having to go back and forth between tabs copying and pasting values! And, if you forget the names of your saved operations or collections, the script editor provides automatic IntelliSense to suggest names of collections and operation names. To trigger it you can start typing` ''` and it will provide a dropdown for you to choose from.


### **Chaining operation scripts with runOperation**


In addition to running GraphQL operations, runOperation will also run any operation scripts that are saved to the operation if they exist. This allows you and your team to write chains of scripts to automate entire workflows!


ℹ️ **Note:** Be sure that you have the operation script flag turned on in your Explorer settings to test and chain operation scripts.


## **Start writing your Operation Scripts today!**


Head over to[GraphOS Studio](https://studio.apollographql.com/) to start writing scripts and automating testing workflows for your GraphQL operations! Learn more about this feature[here](https://www.apollographql.com/docs/graphos/explorer/connecting-authenticating/#operation-scripts) and as always we love hearing from you, so if you have any feedback, feel free to leave a comment in our[Studio Community GitHub repository.](https://github.com/apollographql/apollo-studio-community)


Written by


Evan Silverman


[Read more by Evan Silverman](https://www.apollographql.com/blog/author/evansilverman)
