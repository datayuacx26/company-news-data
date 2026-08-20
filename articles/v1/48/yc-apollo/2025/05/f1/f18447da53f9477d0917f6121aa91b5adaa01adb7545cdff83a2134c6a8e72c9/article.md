---
schema_version: "1.0.0"
document_id: "f18447da53f9477d0917f6121aa91b5adaa01adb7545cdff83a2134c6a8e72c9"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/simplify-your-rest-api-logic-in-react-with-connectors-for-rest-apis-and-graphql"
published_at: "2025-05-28T16:41:05+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:574c4d0064b8cdfa35584e779b35fcd070d21531eaf2613bd4812731eaf8aabe"
---

# Simplify Your REST API Logic in React with Connectors for REST APIs and GraphQL

If you’ve built a React or Next.js app that talks to multiple REST APIs, you’ve probably got a file like *actions.ts* set up as a central spot for fetch calls to public services like the USGS Earthquake API or Nominatim’s reverse geocoder. It works, but the code often ends up repetitive, brittle, and difficult to maintain or scale when different endpoints need to talk to each other.


In this post, we’re going to take that same setup and clean it up with a GraphQL layer powered by[Apollo Connectors](https://www.apollographql.com/docs/graphos/connectors) . Instead of orchestrating data in *actions.ts* , we’ll define a GraphQL schema that does the work for us. Using a declarative configuration, we’ll unify earthquake and location data in a single query with no need for custom resolvers or custom backend logic. The goal: to make your frontend simpler and your data fetching smarter, without giving up the REST services and patterns you already know.


## Prerequisites


- Create an[Apollo Studio](https://studio.apollographql.com/) account. This allows you to create and manage your graph, providing the necessary credentials APOLLO_KEY and APOLLO_GRAPH_REF (more on those later…), which the Apollo Router uses to fetch the supergraph schema and run locally.
- [Install](https://www.apollographql.com/docs/rover/getting-started#installation-methods) and[authenticate](https://www.apollographql.com/docs/rover/configuring) Rover CLI, which we will use for configuring our graph and running Apollo Router locally.


## Setup


In the directory you want to host your schema in run:


```text
rover init
```


Follow the prompts in the CLI to create a new project.


Rover will generate a command for you that contains your APOLLO_KEY and GRAPH_REF. You can start the Router with the command provided, but I would recommend putting these values in an VSCode > settings.json.


```text
{
"terminal.integrated.profiles.osx": {
"graphos": {
"path": "zsh",
"args": ["-l"],
"env": {
"APOLLO_KEY": "<YOUR_KEY>",
"APOLLO_GRAPH_REF": "<GRAPH_REF>"
}
}
},
"terminal.integrated.defaultProfile.osx": "graphos"
}
```


Next create a file at the root called router.yaml and place this code in it. This is to override CORS errors while working in dev.[This is not for use in production](https://www.apollographql.com/docs/graphos/routing/configuration/yaml#cors) .


```text
cors:
allow_any_origin: true
```


If you haven’t already open your project in VSCode or your IDE of choice and from the terminal run:


```text
rover dev --router-config router.yaml --supergraph-config supergraph.yaml
```


## Building the Schema


Create a new file called earthquake.graphql and paste the following code.


*Note: If you prefer,*[the completed schema can be found here](https://github.com/apollographql/connectors-community/blob/main/connectors/usgs/earthquakes-nominatum/earthquake-simple.graphql) *.*


```text
@link(url: "https://specs.apollo.dev/federation/v2.10", import: ["@key"]) # Enable this schema to use Apollo Federation features
@link( # Enable this schema to use Apollo Connectors
url: "https://specs.apollo.dev/connect/v0.1"
import: ["@connect", "@source"]
)
@source(
name: "usgs"
http: { baseURL: "https://earthquake.usgs.gov/fdsnws/event/1/" }
)


type EarthquakeProperties {
mag: Float
place: String
time: Float
updated: Float
url: String
title: String
}


type EarthquakeGeometry {
#this returns 3 values - lon,lat,depth in km
coordinates: [Float]
}


type Earthquake {
id: ID!
properties: EarthquakeProperties
geometry: EarthquakeGeometry
}


type Query {
recentEarthquakes(limit: Int! lat: String! lon: String! maxRadius: Int!): [Earthquake]
@connect(
source: "usgs"
http: {
GET: "query?format=geojson&latitude={$args.lat}&longitude={$args.lon}&maxradiuskm={$args.maxRadius}&limit={$args.limit}"
}
selection: """
$.features
{
properties{
mag
place
time
updated
url
title
}
geometry{
coordinates
}
id
}


"""
)
}
```


The first two directives following @link are required at the top of any file to enable Connectors and federation. The @source directive is used to point to our base URL for USGS earthquakes.


Underneath this there are three types: Earthquake, EarthquakeProperties, and EarthquakeGeometry. Here we are defining what we want to make available in our schema. You can build this however you want and include as much of the REST API as is valuable for your application. Here you will also define all the types and what values are nullable.


Finally you will see the query type. In this schema we only have one query, but you can build out as many as you need. For example, while this calls recent earthquakes and takes in four parameters, you may also want to include a query for retrieving one earthquake by Id. What you design in this schema is dependent upon the needs of your application and frontend team. Inside this query, you will see the @connect directive which is where you declare what populates this query and how it will return in the selection set below.


## Adding a second REST API


Next, we want to add extended location details for each earthquake using Nominatim. To do this, add in another source directive below the USGS one.


```text
@source(
name: "location"
http: {
baseURL: "https://nominatim.openstreetmap.org/"
headers: [{ name: "User-Agent", value: "testing" }]
}
)
```


Nominatim requires user-agent headers but the value passed can be anything you want.


Next, in your Earthquake type add a new field “display_name” and the following code:


```text
type Earthquake {
id: ID!
properties: EarthquakeProperties
geometry: EarthquakeGeometry
display_name: String
@connect(
source: "location"
http: {
GET: "reverse?lat={$this.geometry.coordinates->slice(1,2)->first}&lon={$this.geometry.coordinates->first}&format=json"
}
selection: """
$.display_name
"""
)
}
```


Here we use a Connector for REST to declare that display_name comes from Nominatum. Later in the React app, we’ll cover how you can alias this to what you are using on your frontend. You can also alias here in the schema if you choose. The $this key references the parent Earthquake object.


Your schema is ready, the last thing we need to do before testing it is to update your supergraph.yaml to point to your file. It should look like this:


```text
subgraphs:
earthquake:
routing_url: http://localhost:4000
schema:
file: earthquake.graphql
federation_version: =2.10.0
```


To learn about configuring your Router, you can[head over to the documentation](https://www.apollographql.com/docs/rover/commands/supergraphs#yaml-configuration-file) to see more options especially relevant once you are ready to go to production.


## Testing the Query


Head to http://localhost:4000 to create and test the query for your app.


Once you have confirmed that your graph is working as expected, it’s time to move to the React app to see what we need to modify. Leave your local Router running as you will need it to be able to test your application.


## Modifying the React App


[Clone the repo](https://github.com/AmandaApollo/earthquake-finder) and install the dependencies, run the project.


Open[actions.ts](http://actions.ts/) so we can investigate the API calls.


There are two functions here, one to call USGS and a second to then call Nominatim on each returned value. There is also some code here to create the necessary object for our frontend.


```text
export async function searchEarthquakes(
latitude: number,
longitude: number,
maxRadius: number,
limit = 20
): Promise<Earthquake[]> {
try {
// Build USGS Earthquake API URL
const url = `https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&latitude=${latitude}&longitude=${longitude}&maxradius=${maxRadius}&limit=${limit}`;


// Fetch earthquake data
const response = await fetch(url);


// rest of code omitted here. . . . .


return earthquakesWithLocation;
} catch (error) {
console.error("Error fetching earthquake data:", error);
return [];
}
}
```


Integrating your GraphQL query doesn’t have to be complicated. If you’re comfortable writing a query and calling fetch, you already know 90% of what you need. Head back to the GraphQL sandbox and click the 3 dots to the right of your query. Then select copy to cURL.


*Note: If you would rather follow along with the final version,*[take a look at this branch](https://github.com/AmandaApollo/earthquake-finder/tree/graphql-version) *.*


In *actions.ts* inside the try/catch block at the top, paste your curl. This is a little difficult to read so if you are in VSCode, this is a good place to use copilot to make this more readable. The end result should look like this.


```text
const query = `
query RecentEarthquakes($limit: Int!, $lat: String!, $lon: String!, $maxRadius: Int!) {
recentEarthquakes(limit: $limit, lat: $lat, lon: $lon, maxRadius: $maxRadius) {
id
locationDetails: display_name
properties {
mag
place
time
updated
url
title
}
geometry {
coordinates
}
}
}
`;


const variables = {
limit,
maxRadius,
lat: latitude.toString(),
lon: longitude.toString(),
};


const response = await fetch("http://localhost:4000/", {
method: "POST",
headers: {
"Content-Type": "application/json",
},
body: JSON.stringify({
query,
variables,
}),
});
if (!response.ok) {
throw new Error(`GraphQL API error: ${response.statusText}`);
}


const {data} = await response.json();


const earthquakes = data.recentEarthquakes ?? [];
console.log(earthquakes)
return earthquakes;
```


Here you have your query, the parameters needed from the frontend, and a fetch call to your graph. Notice we are providing an alias to display_name to locationDetails to match our frontends shape.


Let’s clean up the old code.


In the try/catch block you can remove the rest of the code.


```text
// Build USGS Earthquake API URL
// const url = `https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&latitude=${latitude}&longitude=${longitude}&maxradius=${maxRadius}&limit=${limit}`;


// // Fetch earthquake data
// const response = await fetch(url);


// if (!response.ok) {
//   throw new Error(`USGS API error: ${response.statusText}`);
// }


// const data = await response.json();
// const earthquakes = data.features as Earthquake[];


// Get location details for each earthquake
// const earthquakesWithLocation = await Promise.all(
//   earthquakes.map(async (quake) => {
//     try {
//       // Get location details from Nominatim
//       const locationDetails = await getLocationDetails(
//         quake.geometry.coordinates[1], // latitude
//         quake.geometry.coordinates[0] // longitude
//       );


//       // Add location details to earthquake properties
//       return {
//         ...quake,
//         locationDetails: locationDetails ?? undefined,
//       };
//     } catch (error) {
//       console.error(
//         `Error getting location details for earthquake ${quake.id}:`,
//         error
//       );
//       return quake;
//     }
//   })
// );


// Log the first earthquake with location details for debugging
//   if (earthquakesWithLocation.length > 0) {
//     console.log(
//       "First earthquake with location details:",
//       JSON.stringify(
//         {
//           id: earthquakesWithLocation[0].id,
//           hasLocationDetails:
//             !!earthquakesWithLocation[0].locationDetails,
//           locationDetails:
//             earthquakesWithLocation[0].locationDetails,
//         },
//         null,
//         2
//       )
//     );
//   }


//   return earthquakesWithLocation;
```


You can also remove the entire function call for getLocationDetails. We are now calling exactly what we need in one GraphQL query so we do not need this second function or API call here.


```text
async function getLocationDetails(
//   latitude: number,
//   longitude: number
// ): Promise<string | null> {
//   try {
//     // Add a small delay to avoid rate limiting
//     await new Promise((resolve) => setTimeout(resolve, 100));


//     const url = `https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}`;


//     const response = await fetch(url, {
//       headers: {
//         "User-Agent": "EarthquakeSearchApp/1.0",
//       },
//       // Ensure we don't cache the response
//       cache: "no-store",
//     });


//     if (!response.ok) {
//       console.error(
//         `Nominatim API error: ${response.status} ${response.statusText}`
//       );
//       return null;
//     }


//     const data = await response.json();


//     // Log the response for debugging
//     console.log(
//       `Location details for ${latitude},${longitude}:`,
//       JSON.stringify({ display_name: data.display_name }, null, 2)
//     );


//     return data.display_name ?? null;
//   } catch (error) {
//     console.error("Error getting location details:", error);
//     return null;
//   }
//}
```


By modifying this file to use our new GraphQL query, we eliminated over 50 lines of redundant code without changing any UI logic. It’s a reminder that simplicity scales and a practical example of how to keep your GraphQL integration dead simple.


Another interesting thing to notice here is that searchByPlace is still intact and using REST. Using Graphql is not all or nothing, you can adopt it in your applications when it makes sense and use it alongside your other REST API calls. This allows you to adopt Graphql slowly without breaking other parts of your application which is especially important if you work with multiple teams.


Save your files and navigate to http://localhost:3000 to see your updates in action.


## Wrapping up


While this example uses Apollo Router and Connectors for REST APIs to keep things simple and local, in a production app you’d typically pair this setup with Apollo Client on the frontend. Apollo Client handles caching, state management, and reactive updates, all the things you’d expect in a mature GraphQL application.


But the key point here is GraphQL doesn’t have to be all-or-nothing or hard to adopt. By adding a lightweight GraphQL layer over your existing REST services using Connectors, you can reduce boilerplate, simplify frontend data fetching, and set yourself up for more scalable, maintainable code. And when you’re ready, tools like Apollo Client make it easy to fully integrate this into your application architecture.


To get started building your first Apollo Connector for REST APIs today, check out[the documentation](https://www.apollographql.com/docs/graphos/connectors) or get started with[pre-built connectors](https://github.com/apollographql/connectors-community) .


Written by


Amanda Martin


[Read more by Amanda Martin](https://www.apollographql.com/blog/author/amanda)
