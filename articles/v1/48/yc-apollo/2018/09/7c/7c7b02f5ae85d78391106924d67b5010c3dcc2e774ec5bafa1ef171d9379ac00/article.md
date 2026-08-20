---
schema_version: "1.0.0"
document_id: "7c7b02f5ae85d78391106924d67b5010c3dcc2e774ec5bafa1ef171d9379ac00"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/deploy-a-fullstack-apollo-app-with-netlify-45a7dfd51b0b"
published_at: "2018-09-13T23:36:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:e328d1461a6aa0331ff2b7be569e318833fb0ae4c85ef2bd9af93e497cda93d0"
---

# Deploy a fullstack Apollo app with Netlify

Apollo makes it easier than ever for product developers to integrate data from any backend into a React app. With new releases like Apollo Server 2.0 that make it super simple to set up a production-ready GraphQL layer, only one question remains: Where do you host your code?


Recently, I’ve been extremely excited about one answer to that question:[Netlify Functions](https://www.netlify.com/docs/functions/) , a new way to deploy server-side code to AWS Lambda alongside your frontend, right from GitHub. Here’s why I think Netlify’s new functions feature is a great way to get started running your whole Apollo app:


1. Use the same platform to deploy your frontend and API code.
2. Deploy previews of both your frontend *and API* on every pull request.
3. No need to worry about scaling containers due to a serverless architecture.


In this post, we’ll set up a “Hello world” API and frontend for an Apollo app on Netlify. Let’s get to it!


---


## Downloading and running the Netlify starter kit


Before we add Apollo functionality, let’s download and run the[official Netlify](https://github.com/netlify/create-react-app-lambda)` <a href="https://github.com/netlify/create-react-app-lambda" target="_blank" rel="noreferrer noopener">create-react-app</a>`[and Lambda starter kit](https://github.com/netlify/create-react-app-lambda) . This will set us up with a base for our frontend and API code.


```text
git clone  https://github.com/netlify/create-react-app-lambda.git
cd create-react-app-lambda
npm install
```


Let’s take a quick look at the file structure here:


- ` package.json` : our dependencies, shared between client and server code. You can also split them up into separate directories later, depending on your preference.
- ` netlify.toml` : configuration for Netlify and the Netlify functions
- ` src/` : code for our React app
- ` src/lambda/` : code that will be deployed to Lambda by Netlify


In` package.json` , you’ll find scripts to run the frontend and lambda code:


```text
"scripts": {
"start": "react-scripts start",
"start:lambda": "netlify-lambda serve src/lambda",
"build": "react-scripts build",
"build:lambda": "netlify-lambda build src/lambda",
}
```


Let’s quickly run the code to see that everything is working. We have to open two terminal windows to run the frontend and functions at the same time:


```text
# In the first terminal
npm run start:lambda# In the second terminal
npm start
```


If everything looks like it started correctly, let’s add a GraphQL API with Apollo Server!


---


## Setting up Apollo Server


Netlify functions run on AWS Lambda, so we can use the` apollo-server-lambda` package to easily integrate Apollo Server for our API layer. Let’s install the packages we need for that:


```text
npm install --save apollo-server-lambda graphql
```


Now, we can create a “Hello world” GraphQL API. Let’s put that in a new file in the lambda folder, at` src/lambda/graphql.js` :


```text
// src/lambda/graphql.js
const { ApolloServer, gql } = require("apollo-server-lambda");


const typeDefs = gql`
type Query {
hello: String
}
`;


const resolvers = {
Query: {
hello: (root, args, context) => {
return "Hello, world!";
}
}
};


const server = new ApolloServer({
typeDefs,
resolvers,
introspection: true,
playground: true
});


exports.handler = server.createHandler();
```


Now, make sure you’ve run` npm run start:lambda` , and navigate to[localhost:9000/graphql](http://localhost:9000/graphql) in your browser. You should see GraphQL Playground, where you can run queries against your API!


If you can see GraphQL Playground and run a simple query, you’ve done everything properly! It’s pretty neat that getting an API up and running was just a few lines of code. Now let’s add Apollo to our frontend.


## Adding Apollo Client to React


Open up` src/App.js` . Let’s convert this file to load a simple query from the API we just set up. First, let’s install Apollo Client and the React integration, and we’ll use the` apollo-boost` package to get started easily:


```text
npm install --save apollo-boost react-apollo
```


Now, add some code to initialize Apollo Client:


```text
// src/App.js
import ApolloClient from "apollo-boost";const client = new ApolloClient({
uri: "/.netlify/functions/graphql"
});
```


The way that the starter kit and Netlify are set up, any requests that start with` .netlify/functions` are automatically redirected to the Lambda functions in our project. So since we created our API with the filename` graphql.js` , we can call it with` .netlify/functions/graphql` from our frontend.


Let’s replace the LambdaDemo component from the starter kit with a new one that uses Apollo to query our API:


```text
import { gql } from "apollo-boost";
import { ApolloProvider, Query } from "react-apollo";// Replace the previous LambdaDemo with the code below:
const LambdaDemo = () => (
<ApolloProvider client={client}>
<Query
query={gql`
{
hello
}
`}
>
{({ data }) =>
<div>A greeting from the server: {data.hello}</div>}
</Query>
</ApolloProvider>
);
```


If we’ve got everything set up correctly, we’ll see the greeting that we wrote in our resolver show up in the frontend:


Sweet, now we’ve connected our client to our API, and the only thing left is to deploy it! We can deploy both our frontend and API to Netlify in just one step.


## Deploying to Netlify


The best way to deploy to Netlify is to set it up to deploy automatically from a GitHub repository. So let’s create a new repository and push our code there. Here’s one I just made:


To start our project, we cloned the Netlify starter kit. If we want our local repository to point to our new GitHub repo instead of the starter kit, we need to change the` origin` , like this:


```text
# Replace the URL with the one to your repository
git remote set-url origin git@github.com:stubailo/apollo-netlify-lambda-app.git
```


Let’s commit and push:


```text
git add .
git commit -m "Add Apollo Server and Client"
git push -u origin master
```


Now let’s check the repository to make sure the code showed up:


[Sign up for a Netlify account](https://app.netlify.com/) if you don’t have one yet. Then, make a new site, and select the right repository:


The starter kit came with a` netlify.toml` configuration file, so we don’t have to change any settings in the UI. Just continue to the deploy step right away.


Wait for it to deploy, and in the meanwhile you can set a custom name for your app. Once it’s done, you’re live! Check out my deployment here:


[https://apollo-netlify-lambda-app.netlify.com/](https://apollo-netlify-lambda-app.netlify.com/)


The best part is, you don’t need to do anything special to scale this up. Since the frontend is running on Netlify’s CDN and the API is a serverless function on Lambda, you don’t have to worry about adding or removing servers.


When I first got to this step, it blew my mind — I never thought it could be so easy to get both my API and frontend deployed all at once.


## Deploy previews


Netlify’s killer feature is the “deploy preview”. You can see a preview of your application deployed for every single PR on your repository. Let’s try changing something in the app, and opening a PR to test it out! To check that the preview works for the API as well, let’s make a change to both the frontend and backend in one commit. Why not make our page display an image of a dog?


First, let’s make a new branch in our repository so that we can open a PR for code review and to get a preview:


```text
git checkout -b dog-photo
```


Let’s go to` src/lambda/graphql.js` and add a field and resolver to our API:


```text
const typeDefs = gql`
type Query {
hello: String
+    dogPhotoUrl: String
}
`;


const resolvers = {
Query: {
hello: (root, args, context) => {
return "Hello, world!";
+    },
+    dogPhotoUrl: (root, args, context) => {
+      return "https://images.dog.ceo/breeds/pomeranian/n02112018_1090.jpg";
}
}
};
```


Then, we can add code to use that field in the frontend in` src/App.js` :


```text
<Query
query={gql`
{
hello
+          dogPhotoUrl
}
`}
>
-      {({ data }) => <div>A greeting from the server: {data.hello}</div>}
+      {({ data }) => (
+        <div>
+          A greeting from the server: {data.hello}<br />
+          <img src={data.dogPhotoUrl} />
+        </div>
+      )}
</Query>
```


Great, let’s check if it works locally. With the frontend and API running, go to[localhost:3000](http://localhost:3000/) , and you should see something like this:


Now let’s commit and push that to a branch:


```text
git commit -am "Add dog photo"
git push -u origin dog-photo
```


And in the GitHub UI we can open a PR, and in a second we’ll see a deploy preview:


If you click Details on the Netlify status check, you’ll see a completely fresh version of your app, with the new frontend and API code we just added, which was deployed straight from the PR.[Check out my preview here!](https://deploy-preview-2--apollo-netlify-lambda-app.netlify.com/)


This makes it super easy to check that everything worked correctly in the production environment, and send a link to your colleagues showing off your new feature. Sweet!


---


## Conclusion


To me this feels like a huge step forward in the space of API development and deployment — you can now get a custom frontend running with its own API in just a few minutes, in a way that can actually scale to production workloads and runs on tested technologies like AWS Lambda. When you run into any issues, it’s really nice that you can just look up resources for Lambda, and not need anything specific to Netlify.


I’ve been using this approach for my personal projects recently, and it’s helped me get off the ground very quickly while having confidence in my ability to scale up later. I hope you find it useful as well! As a bonus, Netlify has a very generous free tier that lets you get very far with prototyping your project before you need to pay for anything. Go forth and deploy some Apollo apps!


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
