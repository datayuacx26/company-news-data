---
schema_version: "1.0.0"
document_id: "a107b4013efe95ef514cf2ecfd8a1584f1da0dca78b0c5830317893d06478792"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-federation-2-and-gateway-2-0-are-now-available-in-preview"
published_at: "2022-03-16T08:36:18+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:c0016373cede94613845ef9ca39456e66a3fce5001b9f2475ac6d6646242f7c7"
---

# Apollo Federation 2 and Gateway 2.0 are now available in preview

*🚀 🎉 Heads up – Federation 2 and Gateway 2.x are now GA! Check out the[GA announcement blog](https://www.apollographql.com/blog/apollo-federation-2-is-now-generally-available/) or head over to the[Federation docs](https://www.apollographql.com/docs/federation/) to get started.*


---


Four months ago we announced Apollo Federation 2 – the next generation of Federation with[new features](https://www.apollographql.com/blog/announcement/backend/announcing-federation-2/) designed to improve the graph developer experience and make it easy for anyone to build, manage, and scale a unified graph.


Since then, we’ve been collecting feedback from members of the community and our customers that have tried out the alpha release and have been iterating on the Federation 2 spec to make it viable for the larger GraphQL ecosystem.


With the improvements we’ve made in recent months, we’re excited to move[Federation 2](https://www.apollographql.com/docs/federation/v2/) along with[Apollo Gateway 2.0](https://www.apollographql.com/docs/federation/v2/gateway/) out of alpha into public preview!


## What you need to know about the preview


With Federation 2 and Gateway 2.0 now in preview, here are some highlights and improvements to be aware of:


- There are **no breaking changes** between Federation 2 alpha and preview.
- Apollo Gateway 0.X does not support Federation 2. If you would like to use Apollo Gateway with Federation 2, you must use Gateway 2.0 or higher.
- Graphs in Apollo Studio with Federation 2 composition enabled will automatically be updated from` alpha` to` preview` .
- If you have Federation 2 composition enabled, Federation 1 subgraphs will automatically be converted to Federation 2 during composition. You can continue to use your Federation 1 subgraphs as long as you like.
- ` @link` and` @shareable` are now supported in the Fed 2 subgraph syntax.
- ` subgraph-js` subgraph library now supports Fed 2 subgraph syntax.


## Getting started with Federation 2 & Gateway 2.0


Ready to start using Federation 2? **Check out our**[docs on moving to Federation 2](https://www.apollographql.com/docs/federation/v2/federation-2/moving-to-federation-2) **.** Here’s a quick overview of the steps you’ll need to take:


### 1. Upgrade` @apollo/gateway`


To use Federation 2, you must be using Gateway 2.0 or higher (or[Apollo Router](https://www.apollographql.com/blog/announcement/backend/apollo-router-our-graphql-federation-runtime-in-rust/) ). Upgrading from Gateway 0.X to Gateway 2.0 is as easy as bumping the version in your` package.json` .


Gateway 2.0 is still compatible with Federation 1, so you can upgrade without breaking your existing graph.


### 2. Turn on Federation 2 composition


Once your runtime is compatible with Federation 2, you’ll need to take a moment to turn on Fed 2 composition for Managed Federation or for the Rover CLI, depending on how you compose your federated graph.


Federation 2 composition is backward compatible with Federation 1 subgraphs (🎉), so turning it on won’t break your existing graph.


**Managed Federation**


If you use managed Federation, you can turn on Fed 2 composition for specific variants. Navigate to the “Manage Variants” section of your graph’s settings page and click the meatball menu next to a variant.


Click “Edit Build Configuration” and select Federation 2 under the “Federation Version” dropdown.


**Rover CLI**


If you use the Rover CLI for Federation composition, you’ll need to install the latest version of Rover and install the Federation 2 plugin. You can find detailed instructions on how to do so[here](https://www.apollographql.com/docs/federation/v2/federation-2/moving-to-federation-2/#using-the-rover-cli) .


### 3. Modify your subgraphs at your own pace


At this point, your graph is already running on Federation 2 – congrats! Now, to make the most out of Fed 2, you’ll want to modify your subgraphs to take advantage of its more flexible composition model.


Since Fed 2 composition is backwards compatible with Fed 1 subgraphs, service teams can modify their subgraphs at their own pace without breaking composition! You can learn about all of the improvements available in the Fed 2 subgraph syntax in the[documentation](https://www.apollographql.com/docs/federation/v2/federation-2/new-in-federation-2/#more-flexible-composition) .


## What’s next?


We can’t wait for you to experience the next generation of Apollo Federation and Gateway 2.0! 🚀 If you’re currently using Federation 1, check our guide on[moving to Federation 2](https://www.apollographql.com/docs/federation/v2/federation-2/moving-to-federation-2/) , or if you’re new to Federation, our[quickstart guide](https://www.apollographql.com/docs/federation/v2/quickstart/setup/) is a great place to get started. And don’t forget, we would love to know about your experience with Federation 2, so be sure to leave us your thoughts on[community.apollographql.com](https://community.apollographql.com/) .


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
