---
schema_version: "1.0.0"
document_id: "b1148ed33278b0337ce61b4bb688afb0e128e9e8a07fc5a40674bf339f63f1b5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/setting-up-authentication-and-authorization-apollo-federation"
published_at: "2020-05-15T12:17:09+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:63b4e4acd1ee6d2987c551c0605d1a834940716a94e6952b3d7838c10180e40f"
---

# Setting Up Authentication and Authorization with Apollo Federation

## Update: Auth now available in Apollo Router


Apollo has shipped our new authentication and authorization features as built-in Router features. Checkout the launch post: https://www.apollographql.com/blog/graphql/security/enforcing-graphql-security-best-practices-with-graphos/


---


When building out a distributed GraphQL architecture with[Apollo Federation](https://www.apollographql.com/docs/apollo-server/federation/introduction/) , we will often need to limit query access based on who requested the data (authentication) and whether they’re allowed to see or change the data they requested (authorization).


Using JSON Web Tokens (or JWTs) to manage user authentication with Apollo Federation is similar to a standard GraphQL API, but there are some special considerations we need to make to receive and verify access tokens at the gateway-level of the API and then forward them on to an implementing service so that service can manage access to its queries.


In this tutorial, we will cover how to:


- Set up Apollo Gateway and an implementing service with a federated schema to manage access to user account data
- Sign a JWT for a user when they send a login mutation and then use Express middleware to verify the token when sent with subsequent requests from the authenticated user
- Add an authorization layer to check user permissions before running resolver functions


You can also watch the talk presented at Apollo Space Camp 2020 by Mandi Wise[here on Yo](https://youtu.be/xASrlg9rmR4)[uTube](https://www.youtube.com/watch?v=v_1bn2sHdk4) .


## Getting Started


The GraphQL API we will build in this tutorial will consist of a gateway API in front of a single implementing service that manages user account data. To begin, we’ll need to install some dependencies. Start by creating a directory for this project:


```text
mkdir apollo-federation-auth-demo && cd apollo-federation-auth-demo
```


Next, create` package.json` file:


```text
npm init --yes
```


Now we can install the packages we need to set up Apollo Federation:


```text
npm i @apollo/federation @apollo/gateway concurrently apollo-server apollo-server-express express graphql nodemon wait-on
```


We install` apollo-server` and` apollo-server-express` because we’ll use a regular Apollo Server for the accounts service, but we’ll use Apollo Server with Express for the gateway API so we can use Express middleware to validate JWTs sent from the client.


Next, we’ll need an` index.js` file for the gateway API and a directory with another` index.js` file for the accounts service:


```text
mkdir accounts && touch index.js accounts/index.js
```


Lastly, we’ll need some mocked user data to fetch in the accounts service’s resolvers. To do that, add a` data.js` file to the project directory with the following code:


```text
module.exports = {
accounts: [
{
id: "12345",
name: "Alice",
email: "alice@email.com",
password: "pAsSWoRd!",
roles: ["admin"],
permissions: ["read:any_account", "read:own_account"]
},
{
id: "67890",
name: "Bob",
email: "bob@email.com",
password: "pAsSWoRd!",
roles: ["subscriber"],
permissions: ["read:own_account"]
}
]
};
```


Note that in a real-world scenario we wouldn’t leave clear text passwords exposed like this, but to keep things succinct we’ll leave out password hashing for this mocked data.


## Configure the Accounts Service and the Gateway API


Before we can set up authentication we’ll need at least one implementing service and a gateway API running. Let’s set up the accounts service first. Add the following code to` accounts/index.js` :


```text
const { ApolloServer, gql } = require("apollo-server");
const { buildFederatedSchema } = require("@apollo/federation");


const { accounts } = require("../data");


const port = 4001;


const typeDefs = gql`
type Account @key(fields: "id") {
id: ID!
name: String
}
extend type Query {
account(id: ID!): Account
accounts: [Account]
}
`;


const resolvers = {
Account: {
_resolveReference(object) {
return accounts.find(account => account.id === object.id);
}
},
Query: {
account(parent, { id }) {
return accounts.find(account => account.id === id);
},
accounts() {
return accounts;
}
}
};


const server = new ApolloServer({
schema: buildFederatedSchema([{ typeDefs, resolvers }])
});


server.listen({ port }).then(({ url }) => {
console.log(`Accounts service ready at ${url}`);
});
```


This federated schema is configured much like a regular schema, but with three notable differences. The first difference is that we use the` @key` directive to make the` Account` type an[entity](https://www.apollographql.com/docs/apollo-server/federation/entities/) so it can be extended and referenced by any other implementing services we create in the future. We must add an accompanying[reference resolver](https://www.apollographql.com/docs/apollo-server/federation/entities/#resolving) for the` Account` entity as well.


The second difference is that we use the` buildFederatedSchema` function imported from the Apollo Federation package to add federation support to this schema. We pass` typeDefs` and` resolvers` into` buildFederatedSchema` and use its return value for the` schema` option in the` ApolloServer` constructor (rather than passing the` typeDefs` and` resolvers` into the new` ApolloServer` directly).


The final difference is that we use the` extend` keyword in front of` type Query` because the` Query` and` Mutation` types originate at the gateway level so the Apollo documentation says that all implementing services should extend these types with any additional operations.


The accounts service is ready to go, so we can turn our attention to the gateway API now. In the top-level` index.js` file, we’ll create another` ApolloServer` using the Express integration this time:


```text
const { ApolloGateway } = require("@apollo/gateway");
const { ApolloServer } = require("apollo-server-express");
const express = require("express");


const port = 4000;
const app = express();


const gateway = new ApolloGateway({
serviceList: [{ name: "accounts", url: "http://localhost:4001" }]
});


const server = new ApolloServer({
gateway,
subscriptions: false
});


server.applyMiddleware({ app });


app.listen({ port }, () =>
console.log(`Server ready at http://localhost:${port}${server.graphqlPath}`)
);
```


To integrate Express with Apollo Server, we call the` applyMiddleware` method on the new` ApolloServer` instance and pass in the top-level Express` app` . Additionally, to turn this Apollo Server into a gateway, we create a new instance off` ApolloGateway` and pass it an array containing an object describing our single implementing service.


To launch our GraphQL API, we’ll create a series of scripts in the` package.json` file. We’ll use` nodemon` to automatically reload our Node.js applications when files change and we’ll use` concurrently` with a wildcard to start up all of the scripts with the` server:` prefix at once. We also use` wait-on` to ensure that the accounts service is ready on port 4001 before starting the gateway application:


```text
{
...
"scripts": {
"server": "concurrently -k npm:server:*",
"server:accounts": "nodemon ./accounts/index.js",
"server:gateway": "wait-on tcp:4001 && nodemon ./index.js"
},
...
}
```


We’re now ready to run` npm run server` in our project directory to start both the gateway API and the accounts service (on ports 4000 and 4001 respectively). The gateway API will be accessible in GraphQL Playground at[http://localhost:4000/graphql](http://localhost:4000/graphql) .


## Add JWT-based Authentication with Express Middleware


To protect our API we will require a valid access token to be sent with any queries. Specifically, we will require a valid JWT to be sent in the` Authorization` header of every request. JWTs conform to an open standard that describes how information may be transmitted as a compact JSON object. JWTs consist of three distinct parts:


1. **Header:** Contains information about the token type and the algorithm used to sign the token (for example, HS256).
2. **Payload:** Contains claims about a particular entity. These statements may have predefined meanings in the JWT specification (known as *registered* claims) or they can be defined by the JWT user (known as *public* or *private* claims).
3. **Signature:** Helps to verify that no information was changed during the token’s transmission by hashing together the token header, its payload, and a secret.


A typical JWT will look something like this:


```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJodHRwczovL2F3ZXNvbWVhcGkuY29tL2dyYXBocWwiOnsicm9sZXMiOlsiYWRtaW4iXSwicGVybWlzc2lvbnMiOlsicmVhZDphbnlfYWNjb3VudCIsInJlYWQ6b3duX2FjY291bnQiXX0sImlhdCI6MTU4NjkwMDI1MSwiZXhwIjoxNTg2OTg2NjUxLCJzdWIiOiIxMjM0NSJ9.31EOrcKYTsg4ro8511bV5nVEyztOBF_4Hqe0_P5lPps
```


Even though the JWT above may look encrypted, it has only been base64url-encoded to make it as compact as possible, so all of the information inside can just as easily be decoded again. Similarly, the signature portion of the JWT only helps us ensure that its data hasn’t been changed while in transmission between the sender and receiver. The signature plays no role in actually encrypting the information contained within. For these reasons, it’s important to not put any secret information inside of the JWT header or payload in clear text.


The header section of the above token would decode to:


```text
{
"alg": "HS256",
"typ": "JWT"
}
```


And the payload section would decode as follows:


```text
{
"https://awesomeapi.com/graphql": {
"roles": [
"admin"
],
"permissions": [
"read:any_account",
"read:own_account"
]
},
"iat": 1586900251,
"exp": 1586986651,
"sub": "12345"
}
```


In the token’s payload, the` sub` ,` iat` , and` exp` claims represent *registered* claims. The` sub` claim (short for “subject”) is a unique identifier for the object described by the token. The` iat` claim is the time at which the token was issued. The` exp` claim is the time that the token expires. These claims are a part of the JWT specification.


The claim with the` https://awesomeapi.com/graphql` key is a user-defined public claim added to the JWT. Custom public claims included in a JWT must be listed in the[IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml) or be defined with a collision-resistant namespace such as a URI, as was done above.


You can experiment with encoding and decoding JWTs at[https://jwt.io](https://jwt.io/) .


Using a JWT like the one above, a typical user authentication flow would follow these steps:


1. A user would submit their username and password in a request from a client
2. The server would verify the submitted username and password against data saved in a database and then send a JWT back to the client to be used as an access token (until the token expires)
3. The client will send that access token back in an` Authorization` header or in a cookie with subsequent requests to the server
4. The server will verify the JWT and then send back the protected data to the user in its response if the JWT is valid


To accomplish the first two steps, we can add a basic` login` mutation to the accounts schema to get a JWT from the server to use in any requests from GraphQL Playground that require authentication. We’ll need to install the jsonwebtoken package to help create a signed JWT in the new mutation:


```text
npm i jsonwebtoken
```


Next, we’ll use` jsonwebtoken` in` accounts/index.js` to add the` login` mutation to the schema:


```text
const { ApolloServer, gql } = require("apollo-server");
const { buildFederatedSchema } = require("@apollo/federation");
const jwt = require("jsonwebtoken");


// ...


const typeDefs = gql`
# ...


extend type Mutation {
login(email: String!, password: String!): String
}
`;


const resolvers = {
// ...
Mutation: {
login(parent, { email, password }) {
const { id, permissions, roles } = accounts.find(
account => account.email === email && account.password === password
);
return jwt.sign(
{ "https://awesomeapi.com/graphql": { roles, permissions } },
"f1BtnWgD3VKY",
{ algorithm: "HS256", subject: id, expiresIn: "1d" }
);
}
}
};


// ...
```


The code above represents a simplified representation of what might happen in an API that handles authentication requests. The submitted email and password would be verified against a user account in a database, a JWT would be signed containing some information about the user in the payload, and the token would be sent back to the client.


The` jwt` object’s` sign` method accepts the following arguments:


1. An object containing the JWT information we want to add to the payload of the token
2. A secret to sign the JWT
3. Additional options such as the unique` subject` value, a token expiration time, and the signing algorithm to use (HS256 is the default)


Again, in a real-world application, we wouldn’t want to hard-code the JWT secret into a file like this. Instead, we would typically use environment variables to store this value, but they have been omitted in this tutorial for brevity.


In GraphQL Playground, we can run the following mutation now:


```text
mutation {
login(email: "alice@email.com", password:"pAsSWoRd!")
}
```


We’ll paste the returned token it into the “HTTP Headers” panel of GraphQL Playground as follows:


```text
{
"Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJodHRwczovL2F3ZXNvbWVhcGkuY29tL2dyYXBocWwiOnsicm9sZXMiOlsiYWRtaW4iXSwicGVybWlzc2lvbnMiOlsicmVhZDphbnlfYWNjb3VudCIsInJlYWQ6b3duX2FjY291bnQiXX0sImlhdCI6MTU4NjkwMDI1MSwiZXhwIjoxNTg2OTg2NjUxLCJzdWIiOiIxMjM0NSJ9.31EOrcKYTsg4ro8511bV5nVEyztOBF_4Hqe0_P5lPps"
}
```


Next, we’ll install Express middleware that verifies and decodes the JWT when it’s sent with requests from GraphQL Playground:


```text
npm i express-jwt
```


Then we’ll add the middleware to the gateway’s` index.js` file, using the same secret that was used to sign the JWT in the mutation, choosing the same signing algorithm, and setting the` credentialsRequired` option to` false` so Express won’t throw an error if a JWT hasn’t been included (which would be the case for the initial` login` mutation or when GraphQL Playground polls for schema updates):


```text
const { ApolloGateway } = require("@apollo/gateway");
const { ApolloServer } = require("apollo-server-express");
const express = require("express");
const expressJwt = require("express-jwt");


const port = 4000;
const app = express();


app.use(
expressJwt({
secret: "f1BtnWgD3VKY",
algorithms: ["HS256"],
credentialsRequired: false
})
);


// ...
```


The middleware we just added to Express will get the token from the` Authorization` header, decode it, and add it to the request object as` req.user` . It’s a common practice to add decoded tokens to[Apollo Server’s context](https://www.apollographql.com/docs/apollo-server/security/authentication/#putting-user-info-on-the-context) because the` context` object is conveniently available in every resolver and it’s recreated with every request so we won’t have to worry about access tokens going stale. Below, we’ll extract the` user` data from the request and add it to the gateway API’s` context` in` index.js` :


```text
// ...


const server = new ApolloServer({
gateway,
subscriptions: false,
context: ({ req }) => {
const user = req.user || null;
return { user };
}
});


server.applyMiddleware({ app });


app.listen({ port }, () =>
console.log(`Server ready at http://localhost:${port}${server.graphqlPath}`)
);
```


With Apollo Federation, adding the` user` object to the gateway API’s context doesn’t automatically make this information available to the resolvers in the implementing services. To pass the` user` data on to the accounts service, we’ll need to add a` buildService` method to the` ApolloGateway` configuration.


The` buildService` method must return an object that implements the` GraphQLDataSource` interface, so for our purposes, we will return a[RemoteGraphQLDataSource](https://www.apollographql.com/docs/apollo-server/api/apollo-gateway/#remotegraphqldatasource) (available in the` @apollo/gateway` package). This object represents a connection between our gateway API and accounts service and it exposes a` willSendRequest` method to modify a request from the gateway to the implementing service before it’s sent.


The` willSendRequest` method has access to the gateway’s` context` object, so we will retrieve the` user` data from it and add it as an HTTP header to the request the gateway sends to the accounts service:


```text
const { ApolloGateway, RemoteGraphQLDataSource } = require("@apollo/gateway");
// ...


const gateway = new ApolloGateway({
serviceList: [{ name: "accounts", url: "http://localhost:4001" }],
buildService({ name, url }) {
return new RemoteGraphQLDataSource({
url,
willSendRequest({ request, context }) {
request.http.headers.set(
"user",
context.user ? JSON.stringify(context.user) : null
);
}
});
}
});


// ...
```


Now over in` accounts/index.js` , we can intercept the new HTTP header in the Apollo Server context for the accounts service and add it to that` context` object:


```text
// ...


const server = new ApolloServer({
schema: buildFederatedSchema([{ typeDefs, resolvers }]),
context: ({ req }) => {
const user = req.headers.user ? JSON.parse(req.headers.user) : null;
return { user };
}
});


// ...
```


By adding the` user` to the` context` object, we now have access to that data inside of the accounts service’s resolvers. With this information in hand, we can add a` viewer` query to the federated schema that uses the` user.sub` value to retrieve the account information of the currently logged-in user:


```text
// ...


const typeDefs = gql`
# ...


extend type Query {
account(id: ID!): Account
accounts: [Account]
viewer: Account!
}


# ...
`;


const resolvers = {
// ...
Query: {
// ...
viewer(parent, args, { user }) {
return accounts.find(account => account.id === user.sub);
}
},
// ...
};


// ...
```


## Authorize API Requests with GraphQL Shield


While we now have a way to identify users based on access tokens in place, we still don’t have any mechanism to limit API access to authenticated users. What’s more, the new` viewer` query will throw an error with a` Cannot read property 'sub' of null` message when it’s sent without the` Authorization` header (because there won’t be a` user` object the context). To remedy these issues, we’ll add authorization to our API as a final step.


We have a few options available for adding authorization to a GraphQL API. We could explicitly check the authenticated user’s ID and permissions inside of each resolver and throw an` AuthenticationError` as needed, but this wouldn’t be very DRY. Alternatively, a popular option for adding authorization in GraphQL APIs involves adding[custom schema directives](https://www.apollographql.com/docs/apollo-server/security/authentication/#authorization-via-custom-directives) to control access to various types and fields.


Yet another option is to abstract authorization into a separate layer and add it to the schema as middleware, allowing us to check permissions before a resolver function is invoked. This is the approach we’ll choose and we’ll implement it using a library called[GraphQL Shield](https://github.com/maticzav/graphql-shield) .


We’ll need to install two more packages to add authorization:


```text
npm i graphql-middleware graphql-shield
```


Next, we’ll create a` permissions.js` file in the` accounts` directory to create a set of rules that check user permissions stored in the custom claim of the JWT before running field resolvers. First, we’ll create a rule that checks if a user is authenticated and then apply that rule to the` viewer` query:


```text
const { rule, shield } = require("graphql-shield");


const isAuthenticated = rule()((parent, args, { user }) => {
return user !== null;
});


const permissions = shield({
Query: {
viewer: isAuthenticated
}
});


module.exports = { permissions };
```


GraphQL Shield’s` rule` function has all of the same parameters as a resolver function, so we can destructure the` user` object from the` context` parameter as we would in a resolver, and then check that the user is not` null` , otherwise we will return` false` to throw an authorization error for this rule.


To apply our new authorization rule, we must use the` applyMiddleware` function from GraphQL Middleware to add the permissions middleware to the federated accounts schema:


```text
const { ApolloServer, gql } = require("apollo-server");
const { applyMiddleware } = require("graphql-middleware");
const { buildFederatedSchema } = require("@apollo/federation");
const jwt = require("jsonwebtoken");


const { accounts } = require("../data");
const { permissions } = require("./permissions");


// ...


const server = new ApolloServer({
schema: applyMiddleware(
buildFederatedSchema([{ typeDefs, resolvers }]),
permissions
),
context: ({ req }) => {
const user = req.headers.user ? JSON.parse(req.headers.user) : null;
return { user };
}
});


// ...
```


If we try running the` viewer` query from GraphQL Playground without an` Authorization` header, then we’ll see an error of` Not Authorised!` now (which is the expected behavior). Lastly, we can add authorization for the` account` and` accounts` queries by creating additional rules that check user permissions. For these rules, we’ll also use GraphQL Shield’s` and` and` or` functions to check multiple rules per query:


```text
const { and, or, rule, shield } = require("graphql-shield");


function getPermissions(user) {
if (user && user["https://awesomeapi.com/graphql"]) {
return user["https://awesomeapi.com/graphql"].permissions;
}
return [];
}


const isAuthenticated = rule()((parent, args, { user }) => {
return user !== null;
});


const canReadAnyAccount = rule()((parent, args, { user }) => {
const userPermissions = getPermissions(user);
return userPermissions.includes("read:any_account");
});


const canReadOwnAccount = rule()((parent, args, { user }) => {
const userPermissions = getPermissions(user);
return userPermissions.includes("read:own_account");
});


const isReadingOwnAccount = rule()((parent, { id }, { user }) => {
return user && user.sub === id;
});


const permissions = shield({
Query: {
account: or(and(canReadOwnAccount, isReadingOwnAccount), canReadAnyAccount),
accounts: canReadAnyAccount,
viewer: isAuthenticated
}
});


module.exports = { permissions };
```


Try running the` account` ,` accounts` , and` viewer` queries with valid access tokens for both Alice and Bob now. You will see that Alice is authorized to run any query based on her permissions, but Bob is only able to run the viewer query or query his specific account by ID.


## In Summary


In this tutorial, we set up a GraphQL API using Apollo Federation and Express and issued JWTs to authenticate users via a mutation.


We then added Express middleware to verify a JWT in an` Authorization` header and passed the decoded JWT from the gateway API context to an implementing service using a` RemoteGraphQLDataSource` .


Finally, we protected our GraphQL API by creating permissions-based rules with the GraphQL Shield middleware in an accounts service using data contained within a custom JWT claim.


You can find the[complete code for this tutorial on GitHub](https://github.com/mandiwise/apollo-federation-auth-demo) .


For more details what’s possible with the Apollo Federation and Apollo Gateway APIs, be sure to[visit the official docs](https://www.apollographql.com/docs/apollo-server/federation/introduction/) .


Written by


Mandi Wise


[Read more by Mandi Wise](https://www.apollographql.com/blog/author/mandiwise)
