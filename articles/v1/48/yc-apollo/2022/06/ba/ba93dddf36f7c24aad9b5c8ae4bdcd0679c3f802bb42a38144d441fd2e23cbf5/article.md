---
schema_version: "1.0.0"
document_id: "ba93dddf36f7c24aad9b5c8ae4bdcd0679c3f802bb42a38144d441fd2e23cbf5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-embed-explorer-for-a-private-graph"
published_at: "2022-06-09T09:24:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:edba383e1d0a390daab2a2d97795e877ee5561a7dc97c35ab7b53013f2131b6e"
---

# How to embed Explorer for a private graph

Strong documentation is an essential best practice for any GraphQL API. One of the best parts of GraphQL is that it’s self-documenting, but nearly every engineering organization has additional internal documentation sites and dev portals designed to provide developers additional context and make them more successful.


To prevent context switching, GraphQL API documentation is often embedded into these other resources using tools like GraphQL Playground or GraphiQL. But GraphQL Playground and GraphiQL are missing a ton of features like ⌘ + K type and field search, one click query building, operation collections, and more that Apollo’s Explorer GraphQL IDE includes.


That’s why earlier this week, we introduced[support for embedding Explorer](https://www.apollographql.com/blog/tooling/devtools/improved-embedding-for-apollo-explorer/) for *any* graph in Apollo Studio, public or private, on your dev portals, documentation sites, and other websites! In this post, I’m going to walk you through how to set up an embedded Explorer for a private graph in a demo website for the very fake Botanists International developer community portal.


## Setup


Before you embed your graph into your dev portal, you will need to[set up your graph in Apollo Studio.](https://www.apollographql.com/docs/studio/getting-started/#2-create-your-first-graph) If you already have a registered graph with a published schema and an endpoint, you are good to go! You can check the endpoint on your registered graph by going to the Explorer page -> Settings tab and verifying the endpoint you see at the top of this section. As you can see, my Pomology graph is ready to be queried against.


If you are looking to embed a demo graph, you can check out the[GitHub graph](https://studio.apollographql.com/public/github/home?variant=current) , or our[Countries example graph](https://studio.apollographql.com/public/countries/home?variant=current) .


## **Step 1: Configure your embed preferences**


Now that you have a registered graph, you are ready to embed the Explorer into any website. Navigate to the Explorer page of the variant you want to embed, click on the settings tab from the left panel, scroll down to find the **Embed Explorer** section and clock on the “Copy code snippet” link.


When you click on this link, a dialog will appear with configuration options for your Explorer.


You can choose to embed a dark or light themed Explorer. You can also choose to have the left panel of the Explorer open or closed by default. If you prefer to pre-set headers and environment variables for your developers, and you want to hide the option to change those from folks visiting your site, you can toggle on ‘Hide Headers and Environment Variables’.


There are also options to change the functionality of the embedded Explorer. By default, every time a user visits your webpage with the Explorer embedded, they will be in a fresh state. If you are setting up a developer portal or documentation site, where folks might be coming back to this page to run queries, you will want to toggle *on* the “Persist client-side state between page loads” option to save the user’s last state between page loads.


The final option here is the most exciting! You have the option to invite developers to your Studio org whenever they visit the Explorer on your webpage.


I want every new developer who joins my org to be a Consumer. Now, when a user visits the Botanist International Developer Portal webpage that I am setting up, and they log in with the embedded Explorer, they are automatically added to my org botanist-international as a Consumer.


Great, I have all the options I want set up, and I can copy a code snippet and begin embedding!


## Step 2: Embed!


You may have noticed that there are three tabs in the Embed Explorer dialog in Studio. There are three options for choosing the code you will use to embed the Explorer in your website. There is a React snippet, a JS snippet and a snippet you can paste directly into your html. We are going to walk through how to use each one.


### React


Let’s start with the react component! I have a React app for my Botanist International Developer Portal[here](https://codesandbox.io/s/botanistinternationaldeveloperportal-react-kq42fm) .


1. Before I copy the code from Studio, I run` npm i @apollo/explorer` in this project to install the Explorer npm package.
2. Now, I make sure I have the ‘React’ tab selected from Studio, and I paste the copied code in a[new file in my project.](https://codesandbox.io/s/botanistinternationaldeveloperportal-react-kq42fm?file=/src/PomologyExplorer.js)
3. Now, I can call the React component from my[App.js file](https://codesandbox.io/s/botanistinternationaldeveloperportal-react-kq42fm?file=/src/App.js) , and when I render the app, the explorer is embedded in my website:


We will get into what happens when you log in in a bit, but first let’s walk through how to use the vanilla JS export from the @apollo/explorer library, and the html snippet option.


### Vanilla JS


If you don’t use React for your app, you might just want to use our vanilla JS class exported from the @apollo/explorer npm package. To use this export, we are going to copy step 1 above, and then select the ‘Javascript’ tab in the embed modal.


This code snippet needs some work, as we need to put the construction of the new` ApolloExplorer` object where we want it to go. You can see two` ...` ’s in this snippet, one at the top after the import statement, and one at the bottom after the class constructor is called.


We will need to place each of these in the right place in our app. In my[example app](https://codesandbox.io/s/botanistinternationaldeveloperportal-js-p5fjsu?file=/src/index.js:120-177) for the Botanist International Developer portal, I opted to drop the div element that we will be anchoring the embedded Explorer to in my` index.js` file by setting the` innerHTML` of my` app` wrapper element.


```text
document.getElementById("app").innerHTML = `
<div id="embedded-explorer" class="explorer-container"/>
`;
```


Under that, I opted to call the constructor of our imported` ApolloExplorer` class as shown in the code snippet I copied. Now, if we open this app:[https://p5fjsu.csb.app/](https://p5fjsu.csb.app/) , we see the authentication screen for the embedded Explorer! Success!


### CDN / HTML


If you just need to render some html to embed your private graph, or you don’t want to use the npm package, you can select the last tab on the ‘Embed Explorer’ dialog in Studio. This code snippet references the code you need directly from our CDN. The` EmbeddedExplorer` class is available on` window` once you load that code via a` <script` tag!


To use this option, simply copy and paste the code you see here directly into the` body` of your html. You can see how I did this with my app[here](https://codesandbox.io/s/botanistinternationaldeveloperportal-cdn-nqbdf1?file=/index.html:1140-2080) . As long as you have an anchor` <div` element with an` id` that matches the` target` passed to the` EmbeddedExplorer` class, you should be good to go.


### Authentication


One of the main new features added to the Explorer, now that folks can embed private graphs, is the ability to log in to Studio from the embed. Now that we’ve embedded the Pomology@current graph on our webpage, we see a log in screen. This is because this graph is private, and Studio needs to verify that you have access before letting you use the Explorer.


I set up this graph using my GitHub Studio account. Now, if I switch accounts and open up the React webpage I made, I am asked to log in. When I click ‘Authorize’ I am taken to Studio to sign in and validate that I *do* want to let this webpage that Explorer is embedded on use my log in.


Now, if I navigate to the botanist-international org in Studio, I can see that I have been invited as a Consumer!


### **** Styling the embedded Explorer


Now that we have the embedded Explorer rendering, we need to make it look nice! The most important css to apply is to give the` #embedded-explorer` div a class with a width and height set according to how you want it to display! I wanted the explorer to fit into my background image, which changes size depending on the height of the screen, so I used responsive values in my width, height and top margin.


```text
.explorer-container {
position: relative;
z-index: 20;
margin: auto;
margin-top: 17.6vh;
height: 63.5vh;
width: min(90vw, 1058px);
}
```


```text
.explorer-container {
position: relative;
z-index: 20;
margin: auto;
margin-top: 17.6vh;
height: 63.5vh;
width: min(90vw, 1058px);
}
```


### Operation Collections in Explorer


Now that any botanist can gain access to our pomology@current graph and query for fruit information, we might want to point these folks in the right direction. We can do this by adding Operation Collections to our graph. When folks visit our internal website, they can check out any collections of queries that have been set up and use these queries as a jumping off point to explore our graph.


## That’s it!


Once you have an embedded Explorer rendering and styled as you wish, you can deploy your web app & send a link to your developers. Now, anyone who logs into the Explorer with a Studio account is automatically invited to your organization, and can use all of the features baked into the Apollo Studio Explorer on your very own site.


See the final version of these three sites here:


**React** :[https://codesandbox.io/s/botanistinternationaldeveloperportal-react-kq42fm](https://codesandbox.io/s/botanistinternationaldeveloperportal-react-kq42fm)


**JS** :[https://codesandbox.io/s/botanistinternationaldeveloperportal-js-p5fjsu](https://codesandbox.io/s/botanistinternationaldeveloperportal-js-p5fjsu)


**CDN** :[https://codesandbox.io/s/botanistinternationaldeveloperportal-cdn-nqbdf1](https://codesandbox.io/s/botanistinternationaldeveloperportal-cdn-nqbdf1)


Written by


Maya Koneval


[Read more by Maya Koneval](https://www.apollographql.com/blog/author/maya)
