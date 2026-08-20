---
schema_version: "1.0.0"
document_id: "0de44f894b642d9dd99b5879d76ac55ee6d28fc0458c3abb45422e4ed80e1246"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/file-upload-best-practices"
published_at: "2020-02-12T18:44:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:0c025f279f1765e9b91eea6e49747df8bd6a980aeed3428bd0fe8469a5249ad6"
---

# Apollo Server File Upload Best Practices

**Update: May 2022**


This post was originally published in 2020. Since then, there have been two major changes.


In Apollo Server 3.0 in 2021, we removed the built-in integration with (a specific outdated version of)` graphql-upload` . We have continued to document how to manually integrate with` graphql-upload` if you want to implement multipart upload requests.


In May 2022, we realized that including multipart uploads in your GraphQL server can make it vulnerable to[CSRF attacks](https://www.apollographql.com/docs/apollo-server/security/cors/#preventing-cross-site-request-forgery-csrf) . We’ve updated this post to make it clear that you should not use` graphql-upload` without preventing CSRFs, such as by enabling Apollo Server 3.7’s[CSRF prevention](https://www.apollographql.com/docs/apollo-server/security/cors/#preventing-cross-site-request-forgery-csrf) feature.


When building an app, you often want to allow users to upload images and other files. When your app is a graph, it’s natural to ask how to handle uploads via GraphQL.


There are several approaches to implementing this. One approach is to ensure that the GraphQL server itself can directly accept uploads, by adding a parser for *multipart requests* that contain file data. Another approach is for your GraphQL server to direct the client to upload its files directly to a cloud service such as S3 via *signed URLs* . Finally, you can use an *image upload service* .


The outdated Apollo Server 2.0 shipped with support for multipart requests enabled. Unfortunately, as described below, supporting multipart requests in your app has performance and security costs. Current versions of Apollo Server do not contain built-in support for multipart requests, but you can integrate with the` graphql-upload` package yourself if you are careful to avoid security traps.


Depending on your problem domain and your use case, the way you set up file uploads may differ. In this article, we’ll compare the advantages and disadvantages of three common ways to perform file uploads within a GraphQL architecture.


1. Signed URLs
2. Using an image upload service
3. Multipart Upload Requests


## Approach #1: Signed URL uploads


Several cloud services offer the ability to perform file uploads using Signed Upload URLs.


With ***Signed Upload URLs,*** we give the client a temporary URL to which they can upload the file directly, bypassing the need to pass file data through a GraphQL server entirely.


### Advantages


- Scales well. Since the expensive work of shuffling around file data has been offloaded to cloud services that handle this type of operation effectively, our services can continue to serve regular site traffic.
- It’s possible that the URL may point to a[POP (place of presence)](https://en.wikipedia.org/wiki/Point_of_presence) edge node that is closest to the client and can speed up the time it takes to perform an upload.


### Disadvantages


Admittedly, this approach is a bit more complex to configure, and some services might not offer Signed URL Uploads (though several do).


Also, take into consideration the scenario where we want to update a user’s profile picture. **How does the server get to know about the uploaded URL?**


That may require either:


- The client to perform a subsequent mutation to update their profile picture.
- The server to listen for a webhook or event notifying it that a file was uploaded. This approach is preferred because it decouples the un-ideal relationship of the server relying on the client. Depending on the cloud service, this feature may or may not be possible.


Another disadvantage of this approach is:


- Revoking access to signed URLs isn’t trivial — sometimes, it’s impossible. One solution is to make the lifetime of the URL small, but that may have other negative consequences (large or slow uploads could fail).


## Approach #2: A dedicated image service


One of the most common reasons an app would need uploads is to allow users to upload images. There are special considerations that go into image uploads. For example, you might want to normalize images to a specific format, size, or resolution. You might want to scan images for problematic content as well. Fortunately, there are many services out there that provide APIs (including web-specific APIs) for handling image uploads. These APIs are similar to signed URLs but generally provide a higher level of abstraction, more functionality, and client-side SDKs.


There are several services like this that you can find by searching for “image upload API”. Just integrate it into your GraphQL API appropriately.


(You can also create your own dedicated image service, but that’s a pretty complex undertaking and if you’re ready to do that, you probably don’t need a blog post to suggest it.)


### Advantages


Optimized for image uploads. Provides extra features. No need to manage storage manually.


### Disadvantages


It’s an extra service to evaluate and pay for. Like any cloud service, you now rely on their uptime and continued existence.


## Approach #3: Multipart Upload Requests (not recommended)


A multipart request is an HTTP request that is able to contain, *well, multiple parts* 😅 *.* In a single request, the spec enables you to send text, file data, JSON objects, and whatever else you like. All in a single request.


This capability seems to lend itself closely to the way that we conceptualize API communication with GraphQL. Both` queries` and` mutations` are capable of sending or asking for exactly what we want — no more, no less.


From Apollo Client, it’s possible to, in the form of a` mutation` , send along a stream of file data using a multipart request in order for the server to pipe it to its final destination. A third-party package named` graphql-upload` exists that allows you to receive these requests in Apollo Server.


In[Principled GraphQL](https://principledgraphql.com/operations#10-separate-the-graphql-layer-from-the-service-layer) , a guide created by the Apollo founders on the principles of the Data Graph, it suggests that we *“Separate the GraphQL Layer from the Service Layer”* . Normally, in **a production client-server architecture, the client doesn’t speak directly to the backend service** . Normally, we use an additional layer to “delegate concerns such as load balancing, caching, service location, or API key management to a separate tier”.


For greenfield hobbyist projects (less critical or proof of concept), it’s common that the client-facing GraphQL service **is also the backend service** that performs business logic, talks directly to a database (perhaps through an ORM), and returns data needed for the resolvers. **While we don’t recommend this architecture for production environments, it’s not a bad way to get started learning the Apollo ecosystem** .


Approach #3 to file uploads using a Hobbyist GraphQL Architecture.


In a hobbyist project depicted above, the front-end app using Apollo Client can upload a file to a` graphql-upload` -enabled server using the[apollo-upload-client](https://github.com/jaydenseric/apollo-upload-client) package.


On the server side,` graphql-upload` exposes an` Upload` scalar that can be referenced from within your GraphQL schema in an upload mutation. During an upload mutation, the` Upload` type exposes a stream that can be piped to the destination of your choosing (filesystem or cloud storage).


**Supporting multipart requests directly in your GraphQL server introduces major security issues** unless you specifically address them. Multipart requests use a special` multipart/form-data` HTTP content type, and this content type is effectively a loophole around some browser logic that helps prevent Cross-Site Request Forgery (CSRF) attacks. **You should not enable multipart requests (ie,` graphql-upload` ) in your GraphQL server** unless you understand how CSRF attacks work and are confident that you have prevented them in your app. The easiest way to prevent them is to ensure your Apollo Server is at version 3.7 or newer and[enable its CSRF prevention feature](https://www.apollographql.com/docs/apollo-server/security/cors/#preventing-cross-site-request-forgery-csrf) .


**Warning** : Apollo Server 2 ships with` graphql-upload` directly integrated but does not have a CSRF feature! If you want to safely use multipart uploads in your app (though we still don’t recommend using this feature at all), you should avoid Apollo Server 2 and upgrade to Apollo Server 3.7 and use its security feature.


(This feature is so dangerous that it makes your server vulnerable to CSRF attacks even if you do not use it! Apollo Server 2.25.4 changes the defaults so that this dangerous feature is only enabled if you actually use the` Upload` scalar in your schema. Anybody using any older version of Apollo Server 2 should upgrade at least to 2.25.4 as a temporary step while working on the Apollo Server 3 migration.)


### Advantages


It only requires you to run a single server with a single endpoint; other approaches involve an extra endpoint, server, or cloud service.


### Disadvantages


Processing` multipart/form-data` requests as GraphQL operations exposes your server to CSRF mutation attacks unless you’ve specifically prevented them.


If you’re using the outdated Apollo Server 2, then you don’t have to do install anything in your server to set it up. Er, isn’t ease of use an advantage? Problem is, Apollo Server 2 lacks CSRF prevention features, so this means Apollo Server 2 is insecure by default.


Scaling file uploads this way would likely impose some challenges due to the expensiveness of the operation. File Uploads put a lot of stress on the GraphQL Server, which we recommend using as a proxy to backend services rather than handling the heavy lifting.


### Federated architecture considerations


This approach breaks down even further in a larger organization, taking advantage of the separation of concerns at the service level using[Apollo Federation](https://www.apollographql.com/docs/apollo-server/federation/introduction/) . In such an environment, a data stream initiated from the client must pass through Apollo Gateway, then to the federated GraphQL service before finally being able to construct an upload stream to pipe the incoming data to a cloud provider and eventually resolve with a URL.


Approach #3 using Apollo Federation puts a lot of stress on the Apollo Gateway.


Problems with file uploads using this approach are plenty.


- 1) Not an efficient use of network design. This approach would mean that the entire stream needs to travel from the client to the Apollo Gateway to the Federated Service to the cloud service. Requests to perform this upload mutation could take a long time.
- 2) This is an unnecessarily expensive operation that we may wish to separate from regular` query` and` mutation` traffic. Separating expensive operations from regular ones may help to maintain high throughput since the regular traffic will be much more common. Essentially, the role of an Apollo GraphQL Gateway is to proxy requests to a backend service to handle the heavy lifting, and we want our GraphQL Gateways to resolve as much data as possible.


We really really don’t think you should use multipart uploads with GraphQL. But if you really really want to use it, you can take a look at a tutorial we wrote back before we understood the security implications: “[☝️ GraphQL File Uploads with React Hooks, TypeScript & Amazon S3 \[Tutorial\]](https://www.apollographql.com/blog/graphql/file-uploads/with-react-hooks-typescript-amazon-s3-tutorial/) ”.


## Summary


Some say that a developer’s favorite response to a technical question is “it depends”, and in this case, when deciding on which approach to take for implementing file uploads, it really does depend on what your use case is.


Here’s what we recommend:


1. **For hobbyist and proof-of-concept projects, use Approach #** 2 — a dedicated image service.
2. **For enterprise applications, consider Approach #** 1 — Signed URLs. It can require a little more work to set up than a dedicated image service but provides for finer control.
3. **Legacy app that already uses multipart requests: Approach #** 3 — multipart uploads. This approach has negative performance and security impacts and we do not recommend it for new apps. If you choose to use it, you must enable CSRF prevention in your app.


—


A special thank you to[Emelia Smith](https://twitter.com/thisismissem) for feedback on the initial draft of this post and[Sachin Shinde](https://github.com/sachindshinde) for your wizardry infrastructure knowledge.


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
