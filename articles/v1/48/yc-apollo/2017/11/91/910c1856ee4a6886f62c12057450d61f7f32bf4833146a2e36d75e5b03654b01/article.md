---
schema_version: "1.0.0"
document_id: "910c1856ee4a6886f62c12057450d61f7f32bf4833146a2e36d75e5b03654b01"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/changing-the-architecture-of-express-com-23c950d43323"
published_at: "2017-11-09T23:15:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:ed1f25285bc3cc143efdaf5d66e371cbc255fe55bf2ca3545e73bc67c7c7212f"
---

# Changing the architecture of Express.com

*This is a guest post by Dan Shapiro, Senior eCommerce Engineer at*[Express](https://www.express.com/) *.*


As one of the biggest specialty retail brands in the world, Express keeps up with both fashion and technology trends. Keeping pace technologically means balancing innovation with performance and security. Overhauling the architecture behind the shopping experience’s key eCommerce pages for[Express.com](https://www.express.com/) is no task to be taken lightly.


I’m part of the team building a new shopping architecture. Today, Express leverages Adobe Experience Manager (AEM) as the customer-facing experience layer powered by our proprietary APIs built on Oracle’s ATG Web Commerce engine. We wanted to upgrade to a more responsive stack for the website’s experience to be “faster and snappier,” while also being flexible enough to react to changing design trends and customer experience patterns. To meet this goal, we formed a new product development team steeped in Javascript, React, and GraphQL expertise and focused on the product page to introduce a new technical stack.


Product Page using our new architecture


We chose React as our foundational Javascript framework. Since more than 50% of our traffic is mobile, we selected GraphQL, which lets the client-side developer control how much data is flowing to the client. That optimization benefit alone was worth the integration. We considered other data orchestration engines, but GraphQL was selected as the best fit with our familiarity and general community acceptance and knowledge.


## Discovering Apollo Server and Client


Since[graphql-js](https://github.com/graphql/graphql-js) is the reference implementation for a GraphQL server, but isn’t built to scale, we needed a production-tolerant solution. Apollo was new to the team. During GraphQL proof-of-concepting, we decided to explore[Apollo Server](https://github.com/apollographql/apollo-server) based on its reputation as being very developer friendly and production ready. Apollo Server adds numerous features on top of that reference implementation, such as error handling and easy-to-understand responses. After selecting Apollo’s GraphQL Server,[Apollo Client](https://github.com/apollographql/apollo-client) was an obvious choice. Elegant Apollo Client design choices (like the easy-to-use pagination model) made for a great developer experience as well as an efficient new tool in our tool-set.


## GraphQL Boosts Our Development Velocity


Now GraphQL helps us move faster in product development. We recently had a scenario calling for a new field to be added to a product response to determine the products category. Our API team estimated two weeks to complete the addition. By simply creating a new resolver, we added the new Boolean value, specifying whether the product ID was one of a special set of 25 products, to the response. Within thirty minutes, everyone who uses that API on our side had access to the new field, instead of it being hidden in a client-side module. We saved two weeks of development time.


## The Apollo Engine Advantage


Early on in our POC we started using[Apollo Engine](https://www.apollographql.com/engine/) (previously Apollo Optics) to understand performance of our new stack. During this proof of concept, we wrote a query to get product data which had a large variance in running time. As product developers, Engine helped us understand our backend APIs’ impact on performance. In production we now had the ability to see all the data thanks to Engine. Now we could see all the calls coming to the website and deeper specific traffic data. From a monitoring perspective, we are complementing our traffic and conversion data. We now have a view of how the stack is handling traffic that has illustrated where we need optimization and has provided a more accurate view of scale of traffic and any potential issues.


We leverage Akamai to cache our GraphQL queries every 24 hours. Each product on page is seen by Engine once every 24 hours. In Engine we should see a maximum of around 5,000 products if it’s cached properly. However, in Engine we were seeing around 300,000 a day. This called out a caching challenge we are still researching.


Another challenge Engine helped identify was backend API return errors. We were alerted to a window of 20-second responses that our API team verified as 503s. Those errors were impacting customer experiences, but we were not aware of them. Once identified we adjusted our integration. In short, Engine has been instrumental in identifying issues across our product architecture to improve the end user experience. It has given us confidence in the entire stack.


Shopping Bag page using our new architecture


Next up, we are extending this stack further down the conversion funnel into the shopping bag and check out experiences. With React and GraphQL monitored by Apollo Engine, we are confident we will be delivering a fast, accessible customer experience **.**


Written by


Dan Shapiro


[Read more by Dan Shapiro](https://www.apollographql.com/blog/author/dan)
