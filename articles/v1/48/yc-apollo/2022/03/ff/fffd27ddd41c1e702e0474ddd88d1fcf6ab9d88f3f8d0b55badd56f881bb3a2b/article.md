---
schema_version: "1.0.0"
document_id: "fffd27ddd41c1e702e0474ddd88d1fcf6ab9d88f3f8d0b55badd56f881bb3a2b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/using-the-github-graphql-api-with-apollo-studio-explorer"
published_at: "2022-03-03T15:39:31+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:0bf98bc79830166d0e07a802bfde425f784a1806ffa2afc2ce3b60f5d06d60b6"
---

# Using the GitHub GraphQL API with Apollo Studio Explorer

GraphQL APIs provide data that can be extremely useful to you, your team, or your project. Being able to test and run queries for GraphQL APIs can help you build well-designed queries for your application. Apollo Studio Explorer helps you do this.


This is especially useful when you’re working with large APIs that contain lots of types and fields. The GitHub GraphQL API is one such API. So let’s test out Apollo Studio Explorer by building queries for the GitHub API.


This article will discuss the capabilities of the GitHub GraphQL API. You’ll also get a step-by-step guide on building 3 useful queries. By the end, you’ll be familiar with building queries and using Apollo Studio Explorer.


## **Before you read…**


There are a few things you’ll need before diving into this tutorial:


- You have an understanding of what GraphQL is. If you need help with this, check out this[What is GraphQL](https://www.apollographql.com/blog/graphql/basics/what-is-graphql-introduction/) article.
- You have a GitHub account and an understanding of Git and GitHub features specifically repositories, repository stars, and issues.


If you’ve got all this covered, you’re more than ready to jump in! Let’s start by going over the GitHub API and some of its features.


## **What Is the GitHub GraphQL API?**


GitHub has a public GraphQL API. Many developers are familiar with how GitHub works so using GitHub’s GraphQL API is a great way to get familiar with GraphQL and Apollo Studio Explorer without having to build your own graph from scratch.


With the GraphQL API, you can…


- run queries and mutations.
- access and mutate data for most of GitHub’s staple features including repositories, users, issues, and much more.
- create custom integrations and automate workflows with the data you retrieve.


A tool like Explorer can help you navigate the API, quickly build queries, and will help you to take full advantage of all the GitHub API has to offer.


## **The Setup**


Before we begin building queries, we need to do a few things to get up and running.


### Getting Your GitHub API Access Token


As mentioned earlier, you need a GitHub account. To access GitHub’s GraphQL API, you’ll also need your personal access token. If you don’t know how to retrieve that, check out this[guide on creating personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) . When you’re choosing permissions, you only need to select **repo.** Once you have your token, be sure to save it for later use and DO NOT share with anyone.


### Authentication


To run the GitHub API in Apollo Studio Explorer, you’ll need to authenticate your API requests using the access token you retrieved in the previous section.


Navigate to the[GitHub API home page in Apollo Studio](https://studio.apollographql.com/public/github/home?variant=current&referrer=blog) . Click the **Run in Explorer** button located in the top right-hand corner.


Before you can test and run queries, you’ll need to add your token as a **Header** . Click **+New Header** and select **Authorization** . In the next field type` bearer` followed by your access token. It should look like` bearer XX-GH-PERSONAL-ACCESS-TOKEN-XX`


Notice that the first pre-filled header is deselected.


With authentication out the way, let’s test out the API and Explorer by building some queries.


## Building Queries


The GitHub API is massive and there is so much data available. In this tutorial, we’ll focus on retrieving information about your repositories. We want to create a query that will return all of your repositories, return the number of stars for a repository that you search by name, and return open issues for a repository that you search by name.


First, start by deleting all the preselected fields.


### Building a query that returns your repositories


To retrieve the names of your repositories, your query will look like this:


```text
<meta charset="utf-8">query getRepositories($last: Int, $isFork: Boolean) {
viewer {
repositories(last: 10, isFork: false) {
nodes {
name
}
}
}
}
```


The query is named` getRepositories` . (You can change the name if you like). The` viewer` field holds information about your GitHub account including` repositories` . We need to include` last` and` isFork` as arguments. The` last` argument takes an integer and indicates the number of repositories that will be returned to you. The` isFork` argument is defined by a boolean and determines if the repositories returned will be forked repositories or not. In this example,` last` is set to 10 and` isFork` is set to false.


There is a lot of data available in` repositories` but to get the names of your repositories, you need to access` nodes` and` names` which is a field within` nodes` . Your query is complete and now you can run it to see your repository names being returned.


### Building a query that returns your repository stars


To retrieve the number of stars on a repository that you search by name, this is what your query will look like:


```text
query getRepostioryStars($name: String!, $owner: String!){
repository(name: "repo-name", owner: "repo-owner") {
stargazerCount
}
}


```


The query is named` getRepositoryStars` . The` repository` field contains data about a repository that you search by name and owner. You specify what repository you want data for by defining the` name` and` owner` arguments. Finally, the` stargazerCount` returns an integer which will be the number of stars on the repository you previously specified.


Your query is complete and you can run it in Explorer to see how many stars your repository has.


### Building a query that returns your repository issues


You would use the following query to retrieve the open issues of a repository that you search by name:


```text
query getIssues($name: String!, $owner: String!, $last: Int, $states: [IssueState!]){
repository(name: "repo-name", owner: "repo-owner") {
issues(last: 10, states:OPEN) {
edges {
node {
author {
login
}
title
url
publishedAt
}
}
}
}
}


```


This query is called` getIssues` . You worked with` repository` already. Just remember to replace` repo-name` and` repo-owner` with the name of the repository you’re interested in and your GitHub username respectively. The` issues` field is selected and it has` last` and` states` are arguments. You’ll want to pull the last 10 open issues so set` last` to` 10` and` states` should be` OPEN` . The data we want is within` edges` and` node` .


Within` node` , there are a few pieces of information you’ll need. The issue author name is important. The` author` field and` login` subfield will give you that information. The title of the issue and the date it was made are also valuable. You might also want the URL to the issue. You can get this information with` title` ,` urll` , and` publishedAt` , all of which are subfields of` node` .


Your query for retrieving repository issues is complete. You can run it to see the information the API will return to you.


## What Next?


By now, you’ve gotten more familiar with Apollo Studio Explorer by building and testing queries with GitHub GraphQL API. What can you do next?


If you’d like to try the queries outlined in this article yourself, you can easily access them in this[Operations Collection](https://studio.apollographql.com/public/github/explorer?collectionId=93aef769-11bb-470d-abec-855dd45aed93&overlay=view-collection-details&variant=current) . Operations Collection is a new feature we recently released that enables you to save, organize, and share your graph’s frequently used GraphQL operations. You can read about them more in[this blog post announcement](https://www.apollographql.com/blog/announcement/platform/save-and-share-your-graphql-operations-in-apollo-explorer/) .


If you want to improve your GraphQL knowledge, check out this[free and interactive GraphQL training course.](https://odyssey.apollographql.com/?referrer=blog) You can learn more about Explorer and its features with our[guide to getting started with Explorer](https://www.apollographql.com/docs/studio/explorer/explorer/) .


Written by


Ceora Ford


[Read more by Ceora Ford](https://www.apollographql.com/blog/author/ceora)
