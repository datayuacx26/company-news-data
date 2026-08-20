---
schema_version: "1.0.0"
document_id: "2c8067226fe0f64e5b251b4a1e83b783ab7922e28d771b24ce2adf5b1eeff565"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/8-free-to-use-graphql-apis-for-your-projects-and-demos"
published_at: "2021-06-22T12:04:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:e7a7c60e6f71f10998e4c842f3c5f9e405540fe5e3dd0f5d0aa353bac3e64c24"
---

# 8 Free to Use GraphQL APIs for Your Projects and Demos

Whether you’re trying to build a demo, create a workshop, or learn how GraphQL works, it’s nice to have some examples to reference instead of having to build your own. While[Apollo Server](https://www.apollographql.com/docs/apollo-server/) makes building your own API a delight, sometimes you don’t want to get your hands dirty if you don’t have to. As luck would have it, some really awesome GraphQL APIs are available that you can use for free!


Here’s 8 of them you can use today along with an example query for each one to get you started:


## Space X API


Get all sorts of information about previous Space X launches, like information about the rocket, astronauts, and more![Try it out here!](https://studio.apollographql.com/public/SpaceX-pxxbxen)


```text
{
launchesPast(limit: 10) {
mission_name
launch_date_local
launch_site {
site_name_long
}
links {
article_link
video_link
}
rocket {
rocket_name
}
}
}
```


## SWAPI (Star Wars API)


Are you a fan of Star Wars? The SWAPI API provides a wealth of information about some of the older Star Wars movies. It’s a great API for showing how relationships work in GraphQL.[Try it out here!](https://studio.apollographql.com/public/star-wars-swapi/home?variant=current)


```text
{
allFilms {
films {
title
}
}
}
```


## Rick and Morty API


Get information about episodes, characters, and even their last known location![Try it out here!](https://studio.apollographql.com/public/rick-and-morty-a3b90u)


```text
{
characters(page: 2, filter: { name: "rick" }) {
info {
count
}
results {
name
}
}
}
```


## Countries API


Get country codes, emoji, and more for every country.[Try it out here!](https://studio.apollographql.com/public/countries)


```text
{
countries {
code
name
emoji
}
}
```


## PokeAPI


Get all the information you need to build a Pokedex! The Pokemon API is free for non-commercial use and rate-limited.[Try it out here!](https://studio.apollographql.com/public/poke-gql)


```text
{
gen1_species: pokemon_v2_pokemonspecies(
where: { pokemon_v2_generation: { name: { _eq: "generation-i" } } }
order_by: { id: asc }
) {
name
id
}
}
```


## DexAPI


Another Pokemon API with all the info you need to build a Pokedex! This API is in early development but has a great API for quick Pokemon queries![Try it out here!](https://studio.apollographql.com/sandbox/explorer?endpoint=https%3A%2F%2Fdex-server.herokuapp.com%2F&explorerURLState=N4IgJg9gxgrgtgUwHYBcQC4TADpIAR4CGANsQAoQDWCcE%2BO%2BBeShiuBAvrhyADQgA3QgCcAloQBGxBAGcMWbMO4gOQA)


```text
{
allPokemon {
name
}
}
```


## Anilist API


Anilist is an anime API with information about 1000s of animes. The API is free for non-commercial use and allows 90 requests per minute.[Try it out here!](https://studio.apollographql.com/sandbox/explorer?endpoint=https%3A%2F%2Fgraphql.anilist.co&explorerURLState=N4IgJg9gxgrgtgUwHYBcQC4TADpIAR4AKAhgOYJ474F6JgCWxluNNAzvSggKoBOANi1Z4UnfhSrCayUv3psAFkKlJiogG4JlNAL7aCYBGyi96AB1EQk2vdVs6QAGhDrip4gCNxbDFmy9cBx0gA)


```text
{
Page {
media {
siteUrl
title {
english
native
}
description
}
}
}
```


## GitHub API


The GitHub API is one of my favorite APIs for creating examples. It supports both queries and mutations (you can create repositories, add comments to PRs, and more) and has a vast amount of data making it possible to build really in-depth examples and demos or see what GraphQL looks like at scale.[Try it out here!](https://studio.apollographql.com/public/github)


While the API is free to use, you will still need to add an Authorization header in Apollo Explorer using a[personal access token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token) . Check out the[GitHub API docs](https://docs.github.com/en/graphql/guides/introduction-to-graphql) for more information.


```text
{
viewer {
login
repositories(last: 10) {
nodes {
name
}
}
}
}
```


Happy Querying!


Written by


Kurt Kemple


[Read more by Kurt Kemple](https://www.apollographql.com/blog/author/kurtkemple)
