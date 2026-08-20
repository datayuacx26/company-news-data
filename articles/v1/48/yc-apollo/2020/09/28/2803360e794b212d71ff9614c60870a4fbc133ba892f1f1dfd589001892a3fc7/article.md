---
schema_version: "1.0.0"
document_id: "2803360e794b212d71ff9614c60870a4fbc133ba892f1f1dfd589001892a3fc7"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/add-graphql-to-your-jetpack-compose-apps"
published_at: "2020-09-23T10:55:26+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:f1a149d7d61a46723c09d32e894b24e8b7194ea20209309a99b707315fd0391b"
---

# Add GraphQL to Your Jetpack Compose Apps

Change log: Updated on` 9/15/2022` to update to the latest Compose and Apollo Kotlin versions.


## Introduction


[Jetpack Compose](https://developer.android.com/jetpack/compose) is Android’s modern toolkit for building native UI. It simplifies and accelerates UI development on Android. Quickly bring your app to life with less code, powerful tools, and intuitive Kotlin APIs.


[Apollo Kotlin](https://github.com/apollographql/apollo-kotlin) is a strongly-typed, caching GraphQL client for the JVM, Android, and Kotlin multiplatform. The client will generate typesafe Kotlin models to hold data to be displayed. GraphQL and Apollo Kotlin and Jetpack Compose were destined to meet. 😄


In this post, we will show how to query a GraphQL endpoint with Apollo Kotlin and connect the response to a simple Compose UI. We will also explore how the Apollo normalized cache enables reactive scenarios that play nicely with Compose unidirectional data flow.


## Getting Started


This post assumes some familiarity with Android Studio, Jetpack Compose, GraphQL and Apollo Kotlin. If you are new to any of these concepts please check out the following resources before getting started:


- Compose:[https://developer.android.com/jetpack/compose/tutorial](https://developer.android.com/jetpack/compose/tutorial)
- Android Studio:[https://developer.android.com/studio](https://developer.android.com/studio)
- Apollo Kotlin:[https://www.apollographql.com/docs/android/tutorial/00-introduction/](https://www.apollographql.com/docs/android/tutorial/00-introduction/)
- GraphQL:[https://graphql.org/learn/](https://graphql.org/learn/)


The code for this post is available in the[compose branch](https://github.com/apollographql/apollo-android-tutorial/tree/compose) of the[Apollo Kotlin Tutorial](https://github.com/apollographql/apollo-android-tutorial/) . to pull this code down local open Android Studio’s terminal and run:


` git clone git@github.com:apollographql/apollo-kotlin-tutorial.git`


then run


` git checkout compose`


you can then follow the instructions for[setting up a new project in Android Studio.](https://developer.android.com/training/basics/firstapp/creating-project)


## GraphQL


For our application data we’re going to use the[full-stack tutorial GraphQL endpoint.](https://www.apollographql.com/tutorials/fullstack-quickstart/introduction) It’s an endpoint that lists all the SpaceX missions as well as additional data concerning their launches. As a bonus feature, it also allows you to book a trip to space! You can explore and test the API using the[Apollo Studio Explorer](https://apollo-fullstack-tutorial.herokuapp.com/graphql) .


Let’s take a look at the different GraphQL queries and mutation schemas we need for our application. To learn more about[GraphQL schemas](https://www.apollographql.com/docs/apollo-server/schema/schema/) refer to our Apollo Server documentation.


First we’ll need to query a list of launches to display in our UI, the[LaunchList.graphql](https://github.com/apollographql/apollo-kotlin-tutorial/blob/compose/app/src/main/graphql/com/example/rocketreserver/LaunchList.graphql) file contains the schema we need.


```text
app/src/main/graphql/com/example/rocketreserver/LaunchList.graphql


query LaunchList {
launchConnection: launches {
launches {
id
site
isBooked
mission {
name
missionPatch(size: SMALL)
}
}
}
}
```


This query fetches the launches and other data of the missions: the mission name, mission logo patch and the mission booked state. From this query, Apollo Kotlin generates the following Kotlin type-safe models.


```text
app/build/generated/source/apollo/service/com/example/rocketreserver/LaunchListQuery.kt


public data class Data(
public val launchConnection: LaunchConnection,
) : Query.Data


public data class LaunchConnection(
public val launches: List<Launch?>,
)


public data class Launch(
public val id: String,
public val site: String?,
public val isBooked: Boolean,
public val mission: Mission?,
public val __typename: String,
)


public data class Mission(
public val name: String?,
public val missionPatch: String?,
)
```


We’ll then need to be able to update trips that are booked with a mutation. the[BookTrip.graphql](https://github.com/apollographql/apollo-kotlin-tutorial/blob/compose/app/src/main/graphql/com/example/rocketreserver/BookTrip.graphql) file contains the schema we need.


```text
app/src/main/graphql/com/example/rocketreserver/BookTrip.graphql


mutation BookTrip($id:ID!) {
bookTrips(launchIds: [$id]) {
success
message
launches {
# The id is required here to lookup the correct entry in cache
id
# This will be written in cache and trigger an update in any watcher that is subscribed to this launch
isBooked
}
}
}
```


This mutation will update the state of a trip to booked. From this mutation, Apollo Kotlin generates the following Kotlin type-safe models.


```text
app/src/main/graphql/com/example/rocketreserver/BookTrip.graphql


public data class Data(
public val bookTrips: BookTrips,
) : Mutation.Data


public data class BookTrips(
public val success: Boolean,
public val message: String?,
public val launches: List<Launch?>?,
)


public data class Launch(
public val id: String,
public val isBooked: Boolean,
public val __typename: String,
)
```


Finally we’ll need one more GraphQL schema to get started on our Compose UI. We’ll need to be able to cancel trips that are previously booked with another mutation. the[CancelTrip.graphql](https://github.com/apollographql/apollo-kotlin-tutorial/blob/compose/app/src/main/graphql/com/example/rocketreserver/CancelTrip.graphql) file contains the schema we need.


```text
app/src/main/graphql/com/example/rocketreserver/CancelTrip.graphql


mutation CancelTrip($id:ID!) {
cancelTrip(launchId: $id) {
success
message
launches {
# The id is required here to lookup the correct entry in cache
id
isBooked
}
}
}


```


This mutation will update the state of a trip to canceled. From this mutation, Apollo Kotlin generates the following Kotlin type-safe models.


```text
app/src/main/graphql/com/example/rocketreserver/CancelTrip.graphql


public data class Data(
public val cancelTrip: CancelTrip,
) : Mutation.Data


public data class CancelTrip(
public val success: Boolean,
public val message: String?,
public val launches: List<Launch?>?,
)


public data class Launch(
public val id: String,
public val isBooked: Boolean,
public val __typename: String,
)
```


## The Compose UI


Now that we understand exactly what we need from GraphQL we can finally get to writing our Compose UI! Compose works by composing elementary functions, producing a tree of composable functions (Composables), renderable on demand.


Our approach will be to use 3 simple Composables for our UI:


- A` LaunchList` that will render all the of the` LaunchItems` available.
- A` LaunchItem` which will contain the actual trip data.
- A` BookButton` that will let someone book or cancel a trip.


At the end the UI will look like this:


## Reactive UI


Apollo Kotlin comes with a[normalized cache.](https://www.apollographql.com/docs/kotlin/caching/normalized-cache) Cache normalization is a powerful concept; it minimizes data redundancy and because your[normalized cache](https://www.apollographql.com/docs/kotlin/caching/normalized-cache) can de-duplicate your GraphQL data (using proper[cache IDs](https://www.apollographql.com/docs/kotlin/caching/declarative-ids) ), you can use the cache as the source of truth for populating your UI. When executing an operation, you can use the[ApolloCall.watch](https://www.apollographql.com/docs/kotlin/caching/query-watchers/#watching-a-query) method to automatically notify that operation whenever its associated cached data changes. You can learn more about cache normalization in[this post that demystifies cache normalization](https://www.apollographql.com/blog/demystifying-cache-normalization/) . In the next section you can see that we are using the` .watch` method on the` LaunchListQuery.`


## Build the Launch List


The` LaunchList` resides in the[MainActivity.kt](https://github.com/apollographql/apollo-kotlin-tutorial/blob/compose/app/src/main/java/com/example/rocketreserver/MainActivity.kt) file. In this first section of code we will be creating a container rendering the results of the` LaunchList` query. The` @Composable` annotation indicates this is a composable function.


```text
app/src/main/java/com/example/rocketreserver/MainActivity.kt


@Composable
fun LaunchList() {


val context = LocalContext.current
// tell Compose to remember the flow across recompositions
val flow = remember {
apolloClient(context).query(LaunchListQuery()).watch()
.map {
val launchList = it
.data
?.launchConnection
?.launches
?.filterNotNull()
if (launchList == null) {
// There were some error
// TODO: do something with response.errors
UiState.Error
} else {
UiState.Success(launchList)
}
}
.catch { e ->
emit(UiState.Error)
}
}
```


In the 2nd part of the composable we want to watch the cache to react to changes. For this we can use Coroutines Flows and the Compose` collectAsState` helper function. You can add this code block to bottom of code reference above. To check your work, you can refer to the[MainActivity.kt](https://github.com/apollographql/apollo-kotlin-tutorial/blob/compose/app/src/main/java/com/example/rocketreserver/MainActivity.kt) file for the final code.


```text
app/src/main/java/com/example/rocketreserver/MainActivity.kt


// collectAsState will turn our Flow into state that can be consumed by Composables
val state = flow.collectAsState(initial = UiState.Loading)


// Display thestate
when (val value = state.value) {
is UiState.Success -> LazyColumn(content = {
items(value.launchList) {
LaunchItem(it)
}
})
else -> {}
}
}
```


You can now execute a GraphQL query and get a typesafe Kotlin model to feed your Compose UI. With the Apollo Normalized Cache, you can have your Compose UI react and update automatically on mutations. We’ll see how that will work in the the` BookTrip` Button section below.


## Populate the Launch Items


The` LaunchItem` resides in the[MainActivity.kt](https://github.com/apollographql/apollo-kotlin-tutorial/blob/compose/app/src/main/java/com/example/rocketreserver/MainActivity.kt) file. In this 2nd section of code we will be creating a container for each trip. We’ll leverage the Jetpack Compose[Row](https://developer.android.com/reference/kotlin/androidx/compose/foundation/layout/RowScope) and[Column](https://developer.android.com/reference/kotlin/androidx/compose/foundation/layout/ColumnScope) to do this. The` @Composable` annotation indicates this is a composable function.


```text
app/src/main/java/com/example/rocketreserver/MainActivity.kt


@Composable
fun LaunchItem(launch: LaunchListQuery.Launch) {
Row() {
Column() {
AsyncImage(
modifier = Modifier
.width(100.dp)
.height(100.dp)
.padding(horizontal = 12.dp, vertical = 10.dp),
model = launch.mission?.missionPatch, contentDescription = null,
contentScale = ContentScale.Fit
)
}
Column() {
Text(
text = launch.mission?.name ?: "",
fontWeight = FontWeight.Bold,
fontSize = 20.sp,
style = MaterialTheme.typography.subtitle1,
modifier = Modifier
.fillMaxWidth()
.padding(horizontal = 6.dp, vertical = 8.dp)
)
Text(
text = launch.site ?: "",
fontWeight = FontWeight.Light,
fontSize = 20.sp,
style = MaterialTheme.typography.subtitle2,
modifier = Modifier
.fillMaxWidth()
.padding(horizontal = 12.dp, vertical = 12.dp)
)
BookButton(launch.id, launch.isBooked)
}
}
}
```


## Build the Book Trip Button


Finally we have just one more thing to do!


The` BookButton` resides in the[MainActivity.kt](https://github.com/apollographql/apollo-kotlin-tutorial/blob/compose/app/src/main/java/com/example/rocketreserver/MainActivity.kt) file. In this 3rd section of code we want to add a button to the` LaunchItem` Composable allow a user to either book or cancel a trip quickly. this button will trigger the mutations to update the` isBooked` state of a trip and then also update the cache. The` @Composable` annotation indicates this is a composable function.


```text
@Composable
fun BookButton(id: String, booked: Boolean) {
val context = LocalContext.current
Button(onClick = {


GlobalScope.launch {
withContext(Dispatchers.Main) {
try {
val mutation = if (booked) {
CancelTripMutation(id)
} else {
BookTripMutation(id)
}
val response = apolloClient(context).mutation(mutation).execute()
if (response.hasErrors()) {
val text = response.errors!!.first().message
val duration = Toast.LENGTH_SHORT
val toast = Toast.makeText(context, text, duration)
toast.show()
}
} catch (e: ApolloException) {
val text = e.message
val duration = Toast.LENGTH_SHORT
val toast = Toast.makeText(context, text, duration)
toast.show()
}
}
}
}) {
Text(if (booked) "Cancel" else "Book")
}
}
```


## Wrapping it all up


At this point if things aren’t working for you, the final code for[MainActivity](https://github.com/apollographql/apollo-kotlin-tutorial/blob/main/app/src/main/java/com/example/rocketreserver/MainActivity.kt) should look like this:


```text
app/src/main/java/com/example/rocketreserver/MainActivity.kt


package com.example.rocketreserver


import android.os.Bundle
import android.widget.Toast
import androidx.activity.compose.setContent
import androidx.appcompat.app.AppCompatActivity
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.runtime.collectAsState
import androidx.compose.runtime.remember
import androidx.compose.ui.platform.LocalContext
import com.apollographql.apollo3.cache.normalized.watch
import kotlinx.coroutines.flow.catch
import kotlinx.coroutines.flow.map
import androidx.compose.foundation.lazy.items
import androidx.compose.material.Button
import androidx.compose.material.MaterialTheme
import androidx.compose.material.Text
import androidx.compose.ui.Modifier
import androidx.compose.ui.layout.ContentScale
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import coil.compose.AsyncImage
import com.apollographql.apollo3.exception.ApolloException
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import androidx.compose.runtime.Composable as Composable


class MainActivity : AppCompatActivity() {


override fun onCreate(savedInstanceState: Bundle?) {
super.onCreate(savedInstanceState)
setContent { LaunchList() }
}
}


sealed class UiState {
object Loading : UiState()
object Error : UiState()
class Success(val launchList: List<LaunchListQuery.Launch>) : UiState()
}


@Composable
fun LaunchList() {


val context = LocalContext.current
// tell Compose to remember our state across recompositions
val state = remember {
apolloClient(context).query(LaunchListQuery()).watch()
.map {
val launchList = it
.data
?.launchConnection
?.launches
?.filterNotNull()
if (launchList == null) {
// There were some error
// TODO: do something with response.errors
UiState.Error
} else {
UiState.Success(launchList)
}
}
.catch { e ->
emit(UiState.Error)
}
}
// collectAsState will turn our flow into State that can be consumed by Composables
.collectAsState(initial = UiState.Loading)


// Display the com.example.rocketreserver.UiState as usual
when (val value = state.value) {
is UiState.Success -> LazyColumn(content = {
items(value.launchList) {
LaunchItem(it)
}
})
else -> {}
}
}


@Composable
fun BookButton(id: String, booked: Boolean) {
val context = LocalContext.current
Button(onClick = {


GlobalScope.launch {
withContext(Dispatchers.Main) {
try {
val mutation = if (booked) {
CancelTripMutation(id)
} else {
BookTripMutation(id)
}
val response = apolloClient(context).mutation(mutation).execute()
if (response.hasErrors()) {
val text = response.errors!!.first().message
val duration = Toast.LENGTH_SHORT
val toast = Toast.makeText(context, text, duration)
toast.show()
}
} catch (e: ApolloException) {
val text = e.message
val duration = Toast.LENGTH_SHORT
val toast = Toast.makeText(context, text, duration)
toast.show()
}
}
}
}) {
Text(if (booked) "Cancel" else "Book")
}
}


@Composable
fun LaunchItem(launch: LaunchListQuery.Launch) {
Row() {
Column() {
AsyncImage(
modifier = Modifier
.width(100.dp)
.height(100.dp)
.padding(horizontal = 12.dp, vertical = 10.dp),
model = launch.mission?.missionPatch, contentDescription = null,
contentScale = ContentScale.Fit
)
}
Column() {
Text(
text = launch.mission?.name ?: "",
fontWeight = FontWeight.Bold,
fontSize = 20.sp,
style = MaterialTheme.typography.subtitle1,
modifier = Modifier
.fillMaxWidth()
.padding(horizontal = 6.dp, vertical = 8.dp)
)
Text(
text = launch.site ?: "",
fontWeight = FontWeight.Light,
fontSize = 20.sp,
style = MaterialTheme.typography.subtitle2,
modifier = Modifier
.fillMaxWidth()
.padding(horizontal = 12.dp, vertical = 12.dp)
)
BookButton(launch.id, launch.isBooked)
}
}
}


```


## What’s next!


Having a unidirectional data flow that reacts to cache changes makes it very easy to write robust UIs that always show the latest available data. I’m looking forward to seeing even more of what we can build with Compose! As Compose becomes more of a standard for Android Developers, we expect many UI best practices will start to emerge. For example, in this post, we made the queries from the Composables, but we can also accomplish this from` ViewModels` or Repositories; this would account for better testing and separation of concerns.


Finally, if you have ideas about how to best integrate your GraphQL queries with Jetpack Compose, please[reach out on Github](https://github.com/apollographql/apollo-android/issues/new?assignees=&labels=Type%3A+Feature&template=feature_request.md&title=) ; we’re looking forward to making the experience as smooth as possible and planning better support of Jetpack Compose in the Apollo[Kotlin client in the future.](https://github.com/apollographql/apollo-kotlin/blob/main/ROADMAP.md#better-support-for-jetpack-compose)


Happy composing!


Written by


Martin Bonnin


[Read more by Martin Bonnin](https://www.apollographql.com/blog/author/martin)
