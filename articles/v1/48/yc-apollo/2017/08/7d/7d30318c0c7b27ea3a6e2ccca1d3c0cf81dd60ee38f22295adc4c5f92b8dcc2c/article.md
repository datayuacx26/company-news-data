---
schema_version: "1.0.0"
document_id: "7d30318c0c7b27ea3a6e2ccca1d3c0cf81dd60ee38f22295adc4c5f92b8dcc2c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/scala-js-and-apollo-better-together-b066c6b09af4"
published_at: "2017-08-08T18:08:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:f140d0c7fcb53e28577649d07decfcd2ad8b92114ab71d42293c76fe8b653a2c"
---

# Using Scala.js with React and GraphQL

Scala is an excellent language for writing GraphQL apps, especially with the awesome[Sangria](http://sangria-graphql.org/) library for writing GraphQL servers by[Oleg Ilyenko](https://medium.com/u/93c50e4b5ebe?source=post_page-----b066c6b09af4----------------------) . Scala is a powerful language, combining object oriented and functional programming concepts to make writing complex applications easy. The functional programming aspects of Scala are especially helpful when writing GraphQL schemas, allowing you to declare resolvers without any boilerplate.


With[Scala.js](https://www.scala-js.org/) , you can now use Scala to write client-side apps, by writing Scala code that compiles to JavaScript that runs in your browser. With a straightforward interop layer between Scala and JavaScript, it’s easy to use frameworks like React and Angular to create Scala.js apps. And starting today, Apollo joins in the fun with a new library for using Apollo Client with React in Scala:[react-apollo-scalajs](https://github.com/apollographql/react-apollo-scalajs) !


Adding the react-apollo graphql HOC in Scala


## Introducing` <a href="https://github.com/apollographql/react-apollo-scalajs" target="_blank" rel="noreferrer noopener">react-apollo-scalajs</a>`


As its name suggests,` react-apollo-scalajs` provides the glue to use Apollo Client with React in your Scala.js app. Beyond providing static typing facades for Apollo Client,` react-apollo-scalajs` includes layers that allow you to use Apollo Client through idiomatic Scala code. For example, results of queries are returned as Scala` Futures` and query results can be implicitly converted into fragments they can be viewed as.


Through the excellent` <a href="https://github.com/apollographql/apollo-codegen" target="_blank" rel="noreferrer noopener">apollo-codegen</a>` tool, which is currently used for generating static types for Swift, TypeScript, and Flow, you can now generate Scala case classes which represent the responses of GraphQL queries you will be running in your app. With strong query typing, most issues can be caught at compile-time, with` react-apollo-scalajs` performing extra type checks as you set up React components to receive query results.


## A simple example


*Download and run*[the example on GitHub](https://github.com/apollographql/react-apollo-scalajs/tree/master/example) *!*


To demonstrate how you can use` react-apollo-scalajs` , let’s go through a quick example of putting together an app with queries and mutations targeting a GraphQL server on[Apollo Launchpad](https://launchpad.graphql.com/) .


First, set up your project by installing the Scala.js plugin and the necessary library dependencies.


In your` project/plugins.sbt` add the Scala.js plugin and the[Scala.js Bundler](https://scalacenter.github.io/scalajs-bundler/) plugin (for Webpack-based bundling):


```text
addSbtPlugin("org.scala-js" % "sbt-scalajs" % "0.6.18")


addSbtPlugin("ch.epfl.scala" % "sbt-scalajs-bundler" % "0.7.0")
```


And in your` build.sbt` specify the Scala version, add NPM and Scala library dependencies, and configure the code generator:


```text


enablePlugins(ScalaJSBundlerPlugin)


resolvers += "Apollo Bintray" at "https://dl.bintray.com/apollographql/maven/"
libraryDependencies += "com.apollographql" %%% "react-apollo-scalajs" % "0.1.0"


libraryDependencies += "me.shadaj" %%% "slinky-web" % "0.1.0"


npmDependencies in Compile += "react" -> "15.6.1"


npmDependencies in Compile += "react-dom" -> "15.6.1"


npmDependencies in Compile += "react-apollo" -> "1.4.8"


scalaJSUseMainModuleInitializer := true


val namespace = "com.apollographql.scalajs"


(sourceGenerators in Compile) += Def.task {
import scala.sys.process._


val out = (sourceManaged in Compile).value


out.mkdirs()


Seq(
"apollo-codegen", "generate", ((sourceDirectory in Compile).value / "graphql").getAbsolutePath + "/*.graphql",
"--schema", (baseDirectory.value / "schema.json").getAbsolutePath,
"--target", "scala",
"--namespace", namespace,
"--output", (out / "graphql.scala").getAbsolutePath
).!
Seq(out / "graphql.scala")
}


watchSources ++= ((sourceDirectory in Compile).value / "graphql" ** "*.graphql").get
```


Eventually, the source generators configuration at the end of the file will be moved into an SBT plugin, but for now it is necessary to include this in all projects.


### Creating a UI


Before we jump into using GraphQL for loading our data, let’s start by setting up a simple React UI to verify that our setup works. Create a file` src/main/scala/com/apollographql/scalajs/Main.scala` where we’ll put the initializer for our application. For using React in Scala, we’ll use[Slinky](https://github.com/shadaj/slinky) , which makes React in Scala look very similar to React in plain JavaScript.


```text
package com.apollographql.scalajs


import me.shadaj.slinky.web.ReactDOM
import me.shadaj.slinky.web.html._ // imports tags, such as "h1"


import scala.scalajs.js.JSApp
import org.scalajs.dom.{document, html}


import scala.scalajs.js


object Main extends JSApp {
override def main(): Unit = { // called when the app launches
val reactContainer = document.createElement("div")
document.body.appendChild(reactContainer)


ReactDOM.render(
h1("hello!"), // this is Scala's version of JSX!
reactContainer
)
}
}
```


If you run` sbt fastOptJS::startWebpackDevServer` you should be able to navigate to[http://localhost:8080/webpack-dev-server,](http://localhost:8080/webpack-dev-server,) select the` main-bundle` and see your app running live!


### Adding GraphQL


Now that we have a simple app set up, we can add use GraphQL to load data for it! For this example, we will be running GraphQL queries against the Apollo Launchpad at[https://launchpad.graphql.com/1jzxrj179](https://launchpad.graphql.com/1jzxrj179) , loading and displaying posts in our app. First, we add the GraphQL query for loading all posts. In` src/main/graphql/posts.graphql` add the query:


```text
query AllPosts {
posts {
id
title
votes
}
}
```


If you run` sbt/compile` you’ll see that the code generator kicks in (make sure you have previously run` npm i -g apollo-codegen` ) and emits static types for our query to` target/scala-2.12/src_managed/main/graphql.scala` . We can now use these static types to power our React app!


Wiring GraphQL data into a component through` react-apollo-scalajs` is just like using Apollo in your JS code. First, we create a component to receive the data from our query. In` src/main/scala/com/apollographql/scalajs/PostsView.scala` we set up a new component:


```text
package com.apollographql.scalajs


import me.shadaj.slinky.core.StatelessComponent
import me.shadaj.slinky.core.facade.ReactElement
import me.shadaj.slinky.web.html._


import scala.scalajs.js
import scala.scalajs.js.annotation.ScalaJSDefined


object PostsView extends StatelessComponent {
type Props = AllPostsQuery.Props // we use the static Props type from our generated code as the props for
// this is where we create the definition class, which maps to an ES6 class for defining a component


@ScalaJSDefined
class Def(jsProps: js.Object) extends Definition(jsProps) {
override def render(): ReactElement = {
div(
props.data.map[ReactElement] { d => // if we have a result
d.posts.toList.flatten.flatten.map { post =>
// for each not-null post (we use flattens here to remove the null values)
div(key := post.id.toString)( // render the post
h1(post.title.getOrElse[String]("Unknown title")),
h2(post.votes.getOrElse(0).toString)
)
}
}.getOrElse[ReactElement](h1("loading!"))
)
}
}


// wire the component to our query using the familiar graphql method
val WithData = graphql(AllPostsQuery)(this)
}
```


And that’s it! You now have a React component that’s wired to a GraphQL query. The last step is to adjust our application initializer to wrap the application inside an` ApolloProvider` and configure Apollo Client to run queries against the Launchpad server.


First, at the top of our main method, we initialize Apollo Client:


```text
val client = ApolloClient(ApolloClientOptions(
networkInterface = Some(createNetworkInterface(NetworkInterfaceOptions(
uri = Some("https://1jzxrj179.lp.gql.zone/graphql")
)))
))
```


Then we replace the contents inside` ReactDOM.render` to render our new component inside an` ApolloProvider` wrapper.


```text
ReactDOM.render(
ApolloProvider(ApolloProvider.Props(client))(
div(
PostsView.WithData(()) // we pass in () because there are no variables for the query
)
),
reactContainer
)
```


If you run` sbt fastOptJS::startWebpackDevServer` again and reload the application page you should see a working GraphQL app, showing “Loading” and then displaying the posts data!


It works!


You now have a working GraphQL application written in Scala that uses Apollo Client and React! There are a lot more things that` react-apollo-scalajs` supports, such as query variables and mutations, so check out the` examples` folder in the[repository](https://github.com/apollographql/react-apollo-scalajs) to see how to use those features. This is still a very initial release of Scala.js support for Apollo, so please let us know at[http://github.com/apollographql/react-apollo-scalajs/issues](http://github.com/apollographql/react-apollo-scalajs/issues) if you find any bugs or have ideas for new features.


Written by


Shadaj Laddad


[Read more by Shadaj Laddad](https://www.apollographql.com/blog/author/shadaj)
