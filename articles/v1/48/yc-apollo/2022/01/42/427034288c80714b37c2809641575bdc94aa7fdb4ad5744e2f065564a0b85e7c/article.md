---
schema_version: "1.0.0"
document_id: "427034288c80714b37c2809641575bdc94aa7fdb4ad5744e2f065564a0b85e7c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/automatically-authenticate-with-preflight-scripts-in-apollo-studio-explorer"
published_at: "2022-01-12T08:36:16+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:d527faf2b3ab18b1dad867062f0bb055fbdf468ddf67ace03c79ed00d1dc0056"
---

# Automatically Authenticate with Preflight Scripts in Apollo Studio Explorer

We recently introduced Preflight Scripts to Explorer! With preflight scripts you can automatically run any custom authentication before your GraphQL operation is executed. Preflight scripts are especially useful for managing authentication flows like OAuth by refreshing an access token, for example. Let’s take a look.


## Setting up your preflight script


For a graph and variant you want to create a script for, scroll down to find the “Preflight script” section in Apollo Studio Explorer Settings. You will only be able to set up a script if you have the role of Contributor or Admin for this particular graph.


Clicking on the “Add script” button will open up the preflight script editor panel. In the editor you’ll find some code snippets to help you get started. You can simply click on the plus icon for adding snippets and start writing your preflight script.


## Test and publish your script


As you develop your script in the script editor panel, you can click “Test script” to test its execution. Console messages are printed in the Console output panel. Setting any[environment variables](https://www.apollographql.com/docs/studio/explorer/connecting-authenticating/#environment-variables) in the above dialog will also be reflected in Explorer. Once you are ready, click Save and the script will be available to all team members executing operations against this graph variant in Explorer.


Below is an example of a preflight script that refreshes access tokens after a specific period of time.


⚠️ **Note:** This script exactly as is will not work since it is using a dummy OAuth server.


```text
if (new Date().getTime() > explorer.environment.get("token_expires_at")) {
const { access_token, expires_at, refresh_token } = await explorer.fetch(
"https://example.com/token",
{
method: "POST",
header: {
"Content-Type": "application/json",
//...
},
body: JSON.stringify({
refreshToken: explorer.environment.get("refresh_token"),
}),
},
)
.then((response) => response.json());


explorer.environment.set("token", access_token);
explorer.environment.set("refresh_token", refresh_token);
explorer.environment.set("token_expires_at", expires_at);
}
```


Additionally, we will need to perform the following steps:


1. Authenticate server outside of Apollo Studio to retrieve token, refresh_token and token_expires_at.
2. Add these values as environment variables in Explorer.
3. Now, with the preflight script running, when the token expires the script will automatically refresh the token and there is no need to manually authenticate again.


## Write your preflight script today!


Head over to Apollo Studio Explorer and write your preflight script to make authentication before operations are executed easy for your consumers! Learn more[here](https://www.apollographql.com/docs/studio/explorer/connecting-authenticating/#preflight-scripts) and as always, we love hearing from you, so if you have any feedback, please drop a post on the[Apollo Studio GitHub Community](https://github.com/apollographql/apollo-studio-community/issues) !


Written by


Parul Schroff


[Read more by Parul Schroff](https://www.apollographql.com/blog/author/parul)
