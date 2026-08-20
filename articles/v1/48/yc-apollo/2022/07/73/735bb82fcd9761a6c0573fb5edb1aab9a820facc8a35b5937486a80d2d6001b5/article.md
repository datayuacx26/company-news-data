---
schema_version: "1.0.0"
document_id: "735bb82fcd9761a6c0573fb5edb1aab9a820facc8a35b5937486a80d2d6001b5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/building-a-graphql-server-in-go-with-go-graphql"
published_at: "2022-07-20T16:55:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:422f21659e00498b764c6f6ba1a482a826ac449959e01839285a20d55b6d93bb"
---

# Building a GraphQL server in Go with go-graphql

Over the last decade, Golang has cemented its place among the most popular multipurpose programming languages. With great documentation, a highly active community, and a relatively easy learning curve, it’s become the *go* -to (hehe) choice for many developers. Go can be used for lots of things, but its speed and code efficiency makes it a particularly great choice for building GraphQL servers!


In this post, I’ll walk through building your first GraphQL server in Go. We’ll cover:


- The prerequisites
- Setting up your schema
- Hooking up some data
- Setting up your server and mutations
- [Embedding the Apollo Sandbox IDE](https://www.apollographql.com/blog/tooling/graphql-ide/how-to-use-apollo-sandbox-on-your-localhost/) to explore your graph


Let’s get started!


## Prerequisites


To follow along in this how-to, you should have your dev environment setup to write Golang code[(see the official docs)](https://golang.org/doc/install) , and have an understanding of how GraphQL works. If you are just starting out with GraphQL, or if you are looking to expand your knowledge,[Apollo Odyssey](https://www.apollographql.com/tutorials) has some great interactive tutorials to help you.


We will be using the[graphql-go](https://github.com/graphql-go/graphql) library to set up our server. They have great examples and documentation if you want to continue down the GraphQL x Go path!


You can follow along & find the final code for this tutorial on[GitHub](https://github.com/mayakoneval/bestiary) .


## What are we building?


In the Middle Ages, folks created encyclopedias of real and fictional animals called[bestiaries](https://en.wikipedia.org/wiki/Bestiary) . Inspired by[The Odd Bestiary](https://www.amazon.com/Odd-Bestiary-Alan-Robinson/dp/0252013530) by Alan James Robinson and Laurie Block, we want to digitize an encyclopedia of all of these creatures, bringing together lots of different publications into one Bestiary API!


## Setting up our schema


First, we are going to build out the schema for our Bestiary GraphQL API. To know what shape to make our schema, we have to know what questions we want to answer with this API.


I’d like to be able to see a list of beasts, as well as ask for information about an individual beast. On our Query type, we should have fields for a list of beasts as well as a field to query for an individual creature!


We will be defining our schema and resolvers in` schema.go` . We can start by defining our object Query type in with the` graphql-go` package by creating a` NewObject` like so:


```text
package main


import (
"github.com/graphql-go/graphql"
)


var rootQuery = graphql.NewObject(graphql.ObjectConfig{
Name: "RootQuery",
Fields: graphql.Fields{},
})
```


The fields that we decided to include on our Query type were` beastList` and` beast` .` graphql.Fields{}` takes a map of field name to type, description, args, and a resolver. To fill out these fields, we have to define the shape for a Beast type.


The GraphQL type for our beast is going to be an object, and can be defined by` graphql.NewObject` . Each field needs a type associated with it, and in our case, we have five fields:` name` ,` description` ,` imageUrl` ,` id` and` otherNames` , a list of other names for this animal. In GraphQL syntax that looks like:


```text
type Beast {
name: String!
description: String
id: Int!
imageUrl: String
otherNames: [String!]
}
```


In graphql-go we can define this type similarly:


```text
var beastType = graphql.NewObject(graphql.ObjectConfig{
Name: "Beast",
Fields: graphql.Fields{
"name": &graphql.Field{
Type: graphql.String,
},
"description": &graphql.Field{
Type: graphql.String,
},
"id": &graphql.Field{
Type: graphql.Int,
},
"otherNames": &graphql.Field{
Type: graphql.NewList(graphql.String),
},
"imageUrl": &graphql.Field{
Type: graphql.String,
},
},
})
```


Super! Now that we have the shape of the data we want to expose for our Beast type, we can add our` beastList` and` beast` fields to our Query type. For` beast` , we will want to query for beasts by name, so we will need to add an argument of type string to the` beast` field in our Query type.


```text
var rootQuery = graphql.NewObject(graphql.ObjectConfig{
Name: "RootQuery",
Fields: graphql.Fields{
"beast": &graphql.Field{
Type:        beastType,
Description: "Get single beast",
<strong>           Args: graphql.FieldConfigArgument{
"name": &graphql.ArgumentConfig{
Type: graphql.String,
},
},</strong>
Resolve: func(params graphql.ResolveParams) (interface{}, error) {
return nil, nil
},
},


"beastList": &graphql.Field{
Type:        graphql.NewList(beastType),
Description: "List of beasts",
Resolve: func(p graphql.ResolveParams) (interface{}, error) {
return nil, nil
},
},
},
})
```


For now we are returning` nil` from our resolver and get to the data bit next!


## Hooking up some data!


We have the shell of our Query type so far, but we don’t have actual data being returned from these resolvers. For our purposes we are going to use a json file as our data source.


The data for our beasts[can be found at beastData.json](https://github.com/mayakoneval/bestiary/blob/main/beastData.json) and is in the same shape as the` beastType` we defined earlier. This makes it easy to map our fields to our` beastType` . We can set up a` Beast` Golang struct with json tags to map the camelcase fields in our json file to the Pascal case fields of our struct:


```text
type Beast struct {
ID int `json:"id"`
Name   string `json:"name"`
Description string `json:"description"`
OtherNames []string `json:"otherNames"`
ImageURL string `json:"imageUrl"`
}
```


We will need a function to read the data from the json file, and then we can set the data from our json file to a` BeastList` variable and use this as our source of truth for data as we write our resolvers.


```text
package main


import (
"github.com/graphql-go/graphql"
"encoding/json"
"fmt"
"io/ioutil"
)


// Helper function to import json from file to map
<strong>func importJSONDataFromFile</strong>(fileName string, result interface{}) (isOK bool) {
isOK = true
content, err := ioutil.ReadFile(fileName)
if err != nil {
fmt.Print("Error:", err)
isOK = false
}
err = json.Unmarshal(content, result)
if err != nil {
isOK = false
fmt.Print("Error:", err)
}
return
}


var BeastList []Beast
<strong>var _ = importJSONDataFromFile("./beastData.json", &BeastList)</strong>


type Beast struct {
ID int `json:"id"`
Name   string `json:"name"`
Description string `json:"description"`
OtherNames []string `json:"otherNames"`
ImageURL string `json:"imageUrl"`
}
```


Now all we have to do to set up our resolver for the` beast` field is to look through our array of beasts. For our` beasts` field, we can just return our list of beasts.


```text
var rootQuery = graphql.NewObject(graphql.ObjectConfig{
Name: "RootQuery",
Fields: graphql.Fields{
"beast": &graphql.Field{
Type:        beastType,
Description: "Get single beast",
Args: graphql.FieldConfigArgument{
"name": &graphql.ArgumentConfig{
Type: graphql.String,
},
},
<strong>           Resolve: func(params graphql.ResolveParams) (interface{}, error) {</strong>
<strong>
nameQuery, isOK := params.Args["name"].(string)
if isOK {
// Search for el with name
for _, beast := range BeastList {
if beast.Name == nameQuery {
return beast, nil
}
}
}


return Beast{}, nil
},</strong>
},


"beastList": &graphql.Field{
Type:        graphql.NewList(beastType),
Description: "List of beasts",
<strong>           Resolve: func(p graphql.ResolveParams) (interface{}, error) {
return BeastList, nil
},</strong>
},
},
})
```


Excellent, now we have a functional Query type that is capable of returning data. We now need to define our GraphQL schema with this Query type and get our server up and running. In` graphql-go` , we define a schema with` .NewSchema` &` .SchemaConfig` like so:


```text
// define schema, with our rootQuery
var BeastSchema, _ = graphql.NewSchema(graphql.SchemaConfig{
Query:    rootQuery
})
```


## Setting up our server


To set up our server and handle HTTP requests, we are going to use the[graphql-go-handler package](https://github.com/graphql-go/handler) . We are going to put our server logic in a new file:` main.go` . All we have to do with this library is define a handler:


```text
package main


import (
<strong>	"github.com/graphql-go/handler"
</strong>)


func main() {
<strong>    h := handler.New(&handler.Config{
Schema: &BeastSchema,
Pretty: true,
GraphiQL: false,
})</strong>
}
```


And use the[http golang library](https://pkg.go.dev/net/http) to serve our schema at` /graphql` on port 8080.


```text
package main


import (
<strong>	"net/http"
</strong>	"github.com/graphql-go/handler"
)


func main() {
h := handler.New(&handler.Config{
Schema: &BeastSchema,
Pretty: true,
GraphiQL: false,
})


<strong>	http.Handle("/graphql", h)
</strong>
<strong>	http.ListenAndServe(":8080", nil)</strong>


}
```


Nice! Now we get to test if everything worked as expected. We can run` run go .` from the directory root & curl against our` localhost:8080/graphql` to get a list of beasts.


```text
➜  ~ curl -X POST -H "Content-Type: application/json" --data '{ "query": "{ beastList {id name } }" }' http://localhost:8080/graphql
{
"data": {
"beastList": [
{
"id": 1,
"name": "Amphisbaena"
},
{
"id": 2,
"name": "Monocerus"
},
{
"id": 3,
"name": "Manticore"
},
...
]
}
}
```


🕺🕺🕺 **🥳**


## Mutations


We want to be able to add beasts to our list as we come across them. To do this, we have to add some mutations to our schema. We can follow the same pattern as we did for queries, and add our` rootMutation` to our schema like so:


```text
// define schema, with our rootQuery and rootMutation
var BeastSchema, _ = graphql.NewSchema(graphql.SchemaConfig{
Query:    rootQuery,
Mutation: rootMutation,
})
```


To see the full mutation resolver & schema code,[check out the repo on GitHub](https://github.com/mayakoneval/bestiary/blob/main/schema.go#L64-L164) . Note that mutations on this example API are not persistent.


## Setup Sandbox on our GraphQL endpoint


Now for the fun part!


I want to give folks an awesome experience for exploring my beast API.` graphql-go-handler` comes with a few default IDE options, but I’d rather use Apollo Sandbox since it comes with all of the[great features of Apollo’s Explorer](https://www.apollographql.com/blog/tooling/graphql-ide/the-5-most-useful-features-of-apollo-studio-explorer-that-you-didnt-know-existed/) .


Now that there are options for embedding Sandbox, we totally can! All we have to do is host some html at` /sandbox` on our endpoint using the embedded Sandbox code hosted by Apollo:


```text
var sandboxHTML = []byte(`
<!DOCTYPE html>
<html lang="en">
<body style="margin: 0; overflow-x: hidden; overflow-y: hidden">
<div id="sandbox" style="height:100vh; width:100vw;"></div>
<script src="https://embeddable-sandbox.cdn.apollographql.com/_latest/embeddable-sandbox.umd.production.min.js"></script>
<script>
new window.EmbeddedSandbox&lpar;{
target: "#sandbox",
// Pass through your server href if you are embedding on an endpoint.
// Otherwise, you can pass whatever endpoint you want Sandbox to start up with here.
initialEndpoint: "http://localhost:8080/graphql",
});
// advanced options: https://www.apollographql.com/docs/studio/explorer/sandbox#embedding-sandbox
</script>
</body>


</html>`)
```


In this snippet, we fetch the cdn code in question, and use the` EmbeddedSandbox` class exposed on \`window\` to show Sandbox on our` /sandbox` endpoint.


Now, we can write our html to` /sandbox` on our endpoint.


```text
http.Handle("/sandbox", http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
w.Write(sandboxHTML)
}))
```


Now, when we run` run go .` from our root again, and head to[http://localhost:8080/sandbox](http://localhost:8080/sandbox) , we can explore our schema, make queries and mutations from the Explorer and run checks or diffs against our registered schemas.


You can also save collections of operations in your embedded Sandbox to refer back to! These will persist and show up next time you come back to your` /sandbox` route.


There are a couple options for embedding Sandbox on your site, including our` @apollo/sandbox` npm package. Read more about all of the options you have in embedding Sandbox[here](https://www.apollographql.com/docs/studio/explorer/sandbox/#embedding-sandbox) .


## Thanks!


This tutorial is heavily based on the[examples](https://github.com/graphql-go/graphql/blob/master/examples) that the graphql-go folks have put together, so thanks to them!


Also, thank you to[https://bestiary.ca](https://bestiary.ca/) for the beast info!


Written by


Maya Koneval


[Read more by Maya Koneval](https://www.apollographql.com/blog/author/maya)
