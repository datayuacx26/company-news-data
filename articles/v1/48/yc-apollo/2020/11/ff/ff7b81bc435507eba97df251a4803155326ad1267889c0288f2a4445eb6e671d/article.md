---
schema_version: "1.0.0"
document_id: "ff7b81bc435507eba97df251a4803155326ad1267889c0288f2a4445eb6e671d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/building-a-next-js-app-with-slash-graphql"
published_at: "2020-11-11T15:11:41+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:556840339efa543cebb6e1dc1dd4d707382d433d192021e3f5471ae653f01508"
---

# Building a Next.js App with Apollo Client & Slash GraphQL

[Slash GraphQL](https://dgraph.io/slash-graphql) is a managed GraphQL cloud service that gives your app a` /graphql` API endpoint from nothing more than your app’s GraphQL schema. It’s the fastest way to start a GraphQL app. In this blog post, we will build a simple[NextJS](https://nextjs.org/) app from scratch, using[Apollo Client](https://www.apollographql.com/docs/react/) to consume the GraphQL API of[Slash GraphQL](https://slash.dgraph.io/) .


## What we’re going to build


The app will be a simple[EPL](https://www.premierleague.com/) (English Premier League) player directory in which you can search for players by their club, position, country, etc and a page that shows the EPL points table.


### Player Directory


### Search


### EPL Table


You can find the complete code for this project[here on Github](https://github.com/vardhanapoorv/epl-nextjs-app) .


## Create a basic GraphQL schema


Let’s talk about what we need to represent player data. A player contains the attributes` name` ,` position` ,` overall` ,` club` , and` country` . Since the` position` attribute can have a value from a set of possible values, so we can represent it as an enum. Both` country` and` club` can be represented as their own types so that they can contain more details like the` stadium` name. The schema for our project looks something like this:


```text
type Player {
id: ID!
name: String!
position: Position
overall: Int
club: Club
country: Country
}


enum Position {
GK
RB
LB
CB
DM
CM
LM
RM
CF
ST
}


type Club {
id: ID!
name: String!
league: String
stadium: String
}


type Country {
id: ID!
name: String!
stadium: String
}
```


With the schema decided upon, the next step for us is to create a backend GraphQL API by submitting this schema on Slash GraphQL.


### Slash GraphQL


You’ll need an account to create GraphQL backends on Slash GraphQL. There’s a generous free tier. If you don’t have an account, head over to[https://slash.dgraph.io/](https://slash.dgraph.io/) and register for your free account.


### Create a GraphQL Deployment


You’ll see the empty dashboard screen when you first log into Slash GraphQL.


Just press the “Launch a backend” button. That takes you to a screen to create the backend. You can also checkout the “Interactive Tutorial” if you like.


I named my deployment` epl` , set it up in AWS US West region selected the` Free` billing plan. Clicking “Launch” spins up the backend infrastructure to serve your GraphQL App. That’ll spin for just a few moments, and once you have the green tick, it’s live.


While it’s spinning up, note down the URL of your GraphQL API. You’ll need that to connect it to the NextJS app. Once the GraphQL backend is live, you give it your GraphQL schema, it serves a GraphQL API – no layers, no translations, no SQL, just GraphQL. So press “Create your Schema”, paste the schema in and press “Deploy”.


## Setup a NextJS app with Apollo Client


Let’s create a NextJS app using the following command


```text
yarn create next-app
```


Add the Apollo Client & GraphQL dependencies.


```text
yarn add @apollo/client graphql
```


Create a file[lib/apolloClient.js](https://github.com/vardhanapoorv/epl-nextjs-app/blob/main/lib/apolloClient.js) to store the Apollo client[config](https://www.apollographql.com/docs/react/api/core/ApolloClient/#the-apolloclient-constructor) . We’re going to create a basic function that returns an Apollo Client instance with:


- the` uri` value set to the Slash GraphQL endpoint which we created in the last step.
- ` ssrMode` to should be` true` since when page is pre-rendered using SSR (server-side rendering), and` false` when it’s rendered on the client. To accomplish this, we can check the` typeof` the` window` object to determine if Apollo Client is isomorphically running on the client or the server.


```text
// lib/apolloClient.js
import { useMemo } from "react";
import { ApolloClient, HttpLink, InMemoryCache } from
"@apollo/client";


let apolloClient;


function createApolloClient() {
return new ApolloClient({
ssrMode: typeof window === "undefined", // set to true for SSR
link: new HttpLink({
uri: "YOUR-SLASH-ENDPOINT",
}),
cache: new InMemoryCache(),
});
}
```


Note: If you forgot to copy the URL in the last step, you can find in the Slash GraphQL dashboard.


Now we have a basic function that returns an Apollo client instance for the given config, we don’t want to create a new instance for the different pages, we just want to merge the Apollo cache with initial state. We have the` initializeApollo` function which calls the` createApolloClient` function to create a new client if it doesn’t exist. If it does, then it merges the Apollo cache with the` initialState` (if not null) which is the Apollo cache value that is passed to` initializeApollo` .


```text
export function initializeApollo(initialState = null) {
const _apolloClient = apolloClient ?? createApolloClient();


// If your page has Next.js data fetching methods that use Apollo Client,
// the initial state gets hydrated here
if (initialState) {
// Get existing cache, loaded during client side data fetching
const existingCache = _apolloClient.extract();


// Restore the cache using the data passed from
// getStaticProps/getServerSideProps combined with the existing cached data
_apolloClient.cache.restore({ ...existingCache, ...initialState });
}


// For SSG and SSR always create a new Apollo Client
if (typeof window === "undefined") return _apolloClient;


// Create the Apollo Client once in the client
if (!apolloClient) apolloClient = _apolloClient;
return _apolloClient;
}
```


We want the Apollo client instance to be updated only when the cache value has changed, let’s use a` useMemo` hook to achieve that. The` useApollo` function is defined which calls a[useMemo](https://reactjs.org/docs/hooks-reference.html#usememo) hook which returns the memoized value of the Apollo client returned by the call to` initializeApollo` and it is recomputed only when the` initialState` value changes. This returns the Apollo client instance.


```text
export function useApollo(initialState) {
const store = useMemo(() => initializeApollo(initialState), [initialState]);
return store;
}
```


Now we use this in our[pages/_app.js](https://github.com/vardhanapoorv/epl-nextjs-app/blob/main/pages/_app.js) to pass the Apollo Client instance to different pages. This gets the[pageProps](https://nextjs.org/docs/advanced-features/custom-app) for each page and we will talk more about the cache value` initialApolloState` a little later.


```text
import { ApolloProvider } from "@apollo/client";
import { useApollo } from "../lib/apolloClient";


export default function App({ Component, pageProps }) {
const apolloClient = useApollo(pageProps.initialApolloState);


return (
<ApolloProvider client={apolloClient}>
<div style={{ margin: "20px" }}>
<Component {...pageProps} />
</div>
</ApolloProvider>
);
}
```


We have Apollo Client setup now, let’s query for players.


## Create a basic UI


Let’s create a component` components/playersList.js` which will query and display the list of players. We use the[useQuery](https://www.apollographql.com/docs/react/data/queries/#executing-a-query) hook to fetch the list of players, to which we pass the` ALL_PLAYERS_QUERY` query.


```text
import { gql, useQuery } from "@apollo/client";


export const ALL_PLAYERS_QUERY = gql`
query allPlayers {
queryPlayer {
name
position
country {
id
name
stadium
}
club {
id
name
stadium
}
id
}
}
`;


...


const { loading, error, data } = useQuery(ALL_PLAYERS_QUERY);
```


Now let’s show them on the UI. We will be using[Material-UI](https://material-ui.com/) , so let’s add that dependency.


```text
yarn add @material-ui/core @material-ui/lab
```


Create a grid to display all the players.


```text
// components/playersList.js


import Card from "@material-ui/core/Card";
import Grid from "@material-ui/core/Grid";
import { makeStyles } from "@material-ui/core/styles";
import CardContent from "@material-ui/core/CardContent";
import Button from "@material-ui/core/Button";
import Typography from "@material-ui/core/Typography";


const useStyles = makeStyles({
root: {
minWidth: 275,
},
bullet: {
display: "inline-block",
margin: "0 2px",
transform: "scale(0.8)",
},
title: {
fontSize: 18,
},
pos: {
marginBottom: 12,
fontSize: 12,
},
});


...


const classes = useStyles();


...


export default function PlayersList() {


const { loading, error, data } = useQuery(ALL_PLAYERS_QUERY);


if (error)
return <div>Error loading players.</div>;
if (loading)
return <div>Loading</div>;


const { queryPlayer: allPlayers } = data;
...


return (
<Grid style={{ marginTop: "20px" }} container spacing={2}>
{allPlayers.map((player) => (
<Grid item xs={4} key={player.id}>
<Card className={classes.root}>
<CardContent>
<Typography
className={classes.title}
color="textPrimary"
gutterBottom
>
{player.name}
</Typography>
<Typography className={classes.pos} color="textSecondary">
{player.club.name}
</Typography>
<Typography variant="body2" component="p">
Position - {player.position}
<br />
Country - {player.country.name}
</Typography>
</CardContent>
</Card>
</Grid>
))}
</Grid>
);
}
```


Refer to the[component](https://github.com/vardhanapoorv/epl-nextjs-app/blob/main/components/playersList.js) to see the complete code. Now, let’s include the` PlayersList` component in the[pages/index.js](https://github.com/vardhanapoorv/epl-nextjs-app/blob/main/pages/index.js) page so that we can loop over all players and display them.


```text
// pages/index.js


import PlayersList, { ALL_CLUBS_QUERY, ALL_COUNTRIES_QUERY } from "../components/playersList";
import Link from "next/link"


const IndexPage = () => {
return (
<div>
<h1 style={{ textAlign: "center" }}>
EPL Players Directory
</h1>
<PlayersList />
</div>
)
};
```


Let’s add players by executing some mutations, open your favourite GraphQL client like Postman, Insomnia or GraphQL playground. Add a club –


```text
mutation addClub {
addClub(input:[{name: "Arsenal"}]) {
club {
id
name
}
}
}
```


Result –


```text
{
"data": {
"addClub": {
"club": [
{
"id": "0x2713",
"name": "Arsenal"
}
]
}
},
...
}
```


Now let’s add players to the club –


```text
mutation addPlayers($players: [AddPlayerInput!]!) {
addPlayer(input: $players) {
player {
id
name
}
}
}
```


Query variables ( **Remember to change the` id` value below with what you got in the result above when you ran the mutation** ) –


```text
{
"players": [
{
"name": "Xhaka",
"country": {
"name": "Switzerland"
},
"position": "CM",
"club": {
"id": "0x2713"
},
"overall": 85
},
{
"name": "Leno",
"country": {
"name": "Germany"
},
"position": "GK",
"club": {
"id": "0x2713"
},
"overall": 88
},
{
"name": "Bellerin",
"country": {
"name": "Spain"
},
"position": "RB",
"club": {
"id": "0x2713"
},
"overall": 82
},
{
"name": "Tierney",
"country": {
"name": "Scotland"
},
"position": "LB",
"club": {
"id": "0x2713"
},
"overall": 85
},
{
"name": "Luiz",
"country": {
"name": "Brazil"
},
"position": "CB",
"club": {
"id": "0x2713"
},
"overall": 80
},
{
"name": "Saka",
"country": {
"name": "England"
},
"position": "LM",
"club": {
"id": "0x2713"
},
"overall": 83
}
]
}
```


Result –


```text
{
"data": {
"addPlayer": {
"player": [
{
"id": "0x9c4f",
"name": "Saka"
},
{
"id": "0x9c52",
"name": "Bellerin"
},
{
"id": "0x9c53",
"name": "Tierney"
},
{
"id": "0x9c54",
"name": "Luiz"
},
{
"id": "0x9c56",
"name": "Xhaka"
},
{
"id": "0x9c57",
"name": "Leno"
}
]
}
},
...
}
```


Now successfully we have added few players, feel free to add more players and clubs. Refer the docs section in the your GraphQL client to learn more about the queries/mutations that the GraphQL API supports.


Run` yarn dev` and check the browser, you should see a list of players!


## Add Search/filter


Let’s add a search so that we can filter from the list of all players.


The set of attributes we’ll use to filter via dropdown are` country` ,` club` , and` position` . To populate the dropdown options, we’ll need to first fetch them with a query. Since we are quite certain that these values like clubs, countries won’t change, we can fire these queries as part of pre-rendering (or[SSG](https://nextjs.org/docs/basic-features/pages#static-generation-with-data) static site generation) which runs at build time (` getStaticProps` ). We’ll fire these queries from the[pages/index.js](https://github.com/vardhanapoorv/epl-nextjs-app/blob/main/pages/index.js) page’s` getStaticProps` function.


```text
import { initializeApollo } from "../lib/apolloClient"


...


export async function getStaticProps() {
const apolloClient = initializeApollo();


await apolloClient.query({
query: ALL_COUNTRIES_QUERY,
});


await apolloClient.query({
query: ALL_CLUBS_QUERY,
});


return {
props: {
initialApolloState: apolloClient.cache.extract(),
},
revalidate: 1,
};
}
```


Here, we acquire an instance of Apollo Client, fire the queries off, and then[extract the cache](https://www.apollographql.com/docs/react/api/cache/InMemoryCache/#extract) which contains the result for these queries. We then pass the result as props, which is to be used by the` page/_app.js` to create an updated Apollo Client instance to be used by the pages, as we have seen earlier in this blog post.


**Note: In development mode, getStaticProps runs on each request instead.**


Now let’s update our` components/playerList.js` component to add dropdowns for country & club.


```text
const [country, setCountry] = useState(null);
const {
loading: loadingCountries,
error: errCountries,
data: countries,
} = useQuery(ALL_COUNTRIES_QUERY);
```


The result for these queries is returned by the Apollo cache since it is already present there, because of what we did above.


```text
<Autocomplete
id="combo-box-country"
options={allCountries}
getOptionLabel={(option) => option.name}
value={country}
style={{ width: 300 }}
renderInput={(params) => (
<TextField {...params} label="Country" variant="outlined" />
)}
onChange={(e, value) =>
value
? setCountry({
id: value.id,
name: value.name,
})
: setCountry(null)
}
/>;
```


This will display a autocomplete dropdown for the country, similarly, you can add for the club. Now let’s add search field to search by` position` and player` name` . To achieve this, we will update the schema to use the[@search](https://dgraph.io/docs/graphql/schema/search) directive. Since we know` Position` is an enum in the schema, we can directly store an array of possible values in the UI.


```text
type Player {
id: ID!
name: String! @search(by: [fulltext])
position: Position @search
overall: Int
club: Club
country: Country
}


enum Position {
GK
RB
LB
CB
DM
CM
LM
RM
CF
ST
}


type Club {
id: ID!
name: String!
league: String
stadium: String
}


type Country {
id: ID!
name: String!
stadium: String
}
```


Update the schema on your Slash instance. Navigate to the` Schema` option under` Develop` and paste the updated schema there then click` Deploy` .


The position attribute will be an autocomplete dropdown similar to country & club with the only difference that the options are populated by an array stored in the UI. Lastly, let’s add an input field to search by player name.


```text
const [searchText, setSearchText] = useState("");
...
return (
...
<TextField
id="outlined-basic"
label="Player"
variant="outlined"
value={searchText}
style={{width: 300, marginLeft: "10px" }}
onChange={(event) => setSearchText(event.target.value)}
/>
)
```


Now we have added all the attributes by which we want to filter the players. The search query uses the` countryID` &` clubID` and passes it to the filter of its respective type. The filter that is passed to` queryPlayer` is responsible to filter by position & player name, it looks something like this` filter:{name: {anyoftext: "player-name"}, position: {eq: GK}}` . The[@cascade](https://dgraph.io/docs/graphql/queries/cascade/) directive is used at the top-level to ensure that the result contains only nodes which have all the fields specified in the query.


```text
export const FILTER_PLAYERS_QUERY = gql`
query filterPlayers(
$filter: PlayerFilter
$countryID: [ID!]
$clubID: [ID!]
) {
queryPlayer(filter: $filter) @cascade {
name
position
country(filter: { id: $countryID }) {
id
name
}
club(filter: { id: $clubID }) {
id
name
}
id
}
}
`;
```


We have the search query ready, let’s use it in the component. We will use the[useLazyQuery](https://www.apollographql.com/docs/react/data/queries/#executing-queries-manually) hook since we want to trigger the search query only when we click on the search button, this returns a function which we will invoke from the search function and pass the query variables to it. The` searchStatus` state is used to switch between filtered and all players list.


```text
...
const [searchStatus, setSearchStatus] = useState(false);
const [
getFilteredPlayers,
{ loading: filterLoading, data: filteredPlayers, error: filterError },
] = useLazyQuery(FILTER_PLAYERS_QUERY);
...


const searchPlayers = () => {
let filter = {};
setSearchStatus(true);
if (position) {
filter.position = { eq: position };
}
if (searchText !== "") {
filter.name = { anyoftext: searchText };
}
if (Object.keys(filter).length === 0) {
if (!club && !country) {
setSearchStatus(false);
return;
}
}
getFilteredPlayers({
variables: {
filter: filter,
clubID: club ? [club] : allClubs.map((club) => club.id), // if no club is selected then return all clubs id
countryID: country
? [country.id]
: allCountries.map((country) => country.id), // if no country is selected then return all countries id
},
});
};


const dataset =
searchStatus && filteredPlayers ? filteredPlayers?.queryPlayer : allPlayers;


return(
...
<Button
variant="contained"
color="primary"
onClick={searchPlayers}
style={{ marginLeft: "10px" }}
>
Search
</Button>
...
)
```


Refer to the[component](https://github.com/vardhanapoorv/epl-nextjs-app/blob/main/components/playersList.js) to see the complete code. Run` yarn dev` (if it isn’t already running) and check the browser, you should be able to use the search now!


## Use SSR to fetch EPL table


We have already used one form of pre-rendering (SSG), now let’s quickly use another one[SSR](https://nextjs.org/docs/basic-features/pages#server-side-rendering) to fetch the EPL table. In the case of SSR, the generation happens on each request rather than build time which is the case for SSG. Similar to` getStaticProps` for SSG, there is a` getServerSideProps` function for SSR. Let’s create a file at` pages/table.js` to display the EPL table. We will use the API from[apifootball.com](https://apifootball.com/) to get the table, you can signup for a free plan and get an API key.


```text
export async function getServerSideProps() {
// Fetch data from external API
const res = await fetch(
`https://apiv2.apifootball.com/?action=get_standings&league_id=148&APIkey=YOUR-API_KEY`
);
const data = await res.json();
// Pass data to the page via props
return { props: { data } };
}
```


Create table on UI by using the` data` prop.


```text
function EPLTable({ data }) {
return (
<div>
<Link href="/">Back to players directory</Link>
<h1 style={{ textAlign: "center" }}>EPL Table</h1>
<TableContainer component={Paper}>
<Table aria-label="simple table">
<TableHead>
<TableRow>
<TableCell>Position</TableCell>
...
</TableRow>
</TableHead>
<TableBody>
{data.map((row) => (
<TableRow key={row.overall_league_position}>
<TableCell component="th" scope="row">
{row.overall_league_position}
</TableCell>
...
</TableRow>
))}
</TableBody>
</Table>
</TableContainer>
</div>
);
}
```


Let’s add the link to this page on the main page:[pages/index.js](https://github.com/vardhanapoorv/epl-nextjs-app/blob/main/pages/index.js) .


```text
...
<div>
<h1 style={{ textAlign: "center" }}>
EPL Players Directory <Link href="/table">(EPL Table)</Link>
</h1>
<PlayersList />
</div>
...
```


Refer to the[table page](https://github.com/vardhanapoorv/epl-nextjs-app/blob/main/pages/table.js) to see the complete code. Run` yarn dev` (if it isn’t already running) for a final time and check the browser, you should be able to see the table on the` /table` page!


## Conclusion


In this blog, we learned how to use Apollo client with NextJS along with its pre-rendering methods – SSG and SSR. We also learned how to use Slash GraphQL to get an instant GraphQL endpoint and iterate on its schema.


### References


- Apollo Client docs:[https://www.apollographql.com/docs/react/](https://www.apollographql.com/docs/react/)
- NextJS docs:[https://nextjs.org/docs/getting-started](https://nextjs.org/docs/getting-started)
- Slash GraphQL docs:[https://dgraph.io/docs/slash-graphql/introduction/](https://dgraph.io/docs/slash-graphql/introduction/)


Written by


Apoorv Vardhan


[Read more by Apoorv Vardhan](https://www.apollographql.com/blog/author/apoorv)
