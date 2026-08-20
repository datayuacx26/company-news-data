---
schema_version: "1.0.0"
document_id: "0c6672a6a7584ec598dd97a86661849d970d09afac5b8324d51e3ed221ba088c"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/what-is-google-zanzibar"
published_at: "2021-05-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:bc1eff037af59a5bdaada111339a9f71113618c2b8b161dda261e0a36181c72c"
---

# Understanding Google Zanzibar: A Comprehensive Overview

Lately there has been a lot of talk about Google Zanzibar that gets sent my way. I guess this is to be expected when you[found a company](https://authzed.com/blog/introducing-authzed/) based on the[Zanzibar paper](https://research.google/pubs/pub48190/) and give a talk on Google's Zanzibar system for Papers We Love.


In most of these discussions, there seem to be a number of misconceptions about why Google built Zanzibar, what it does for them, and why it’s important.


At its core, Google Zanzibar is a globally distributed authorization system capable of processing "[more than 10 million client queries per second](https://zanzibar.tech/2SuZL2BPQU:0:15) ," but there is a lot needed to make that possible. In this post I’ll break down and answer common questions I've received from developers using the contents of the paper and our experience building an open source implementation of Google Zanzibar outside of Google.


Google had a rich permissions system before Zanzibar. So why did they decide to build Zanzibar?


## A Brief History of Application Permissions


There are a variety of ways to add permissions to an application. The most common is to create an ad-hoc implementation for each and every app that gets built. For dynamic permissions, such as those that an end-user can grant, this usually manifests itself in source code as interpreting rows stored in a database. This code ends up being a critical part of every request, is extremely sensitive, and can be tricky to write. A mistake in this code can easily cause security holes and exploits in the application. To minimize the surface area and sheer amount of code that gets written, this code is often abstracted into a library that can be shared across many such applications.


Some companies have a common authorization library that they re-use whenever permissions code needs to be called in all of their applications and services. This is the approach that Google used before deciding to build and deploy Zanzibar. Often these libraries are made more generic and programming language agnostic by putting them behind an IPC interface, which then takes the form of a[policy engine](https://docs.authzed.com/authz/policy-engine/) . However, libraries and policy engines have a major downside in this context: They are stateless. Both library and policy engine implementations require that they be given the policy and full set of required data, which is then combined to make permissions decisions. For a standard web application, this means loading database rows representing relationships and passing them to the engine on every single permissions decision.


Because of this limitation, the next logical step is to start storing information related to the decision making in a place that’s directly accessible by the permissions engine. Now when a permission decision needs to be made, the application can simply ask the permissions system about policy and data that it already has accessible. For example, if the service already knows that I am the author of an article, and that authors should be allowed to edit their own articles, it already has all of the information that it needs to answer the question: “Is Jake allowed to edit this article?” This does mean that you need to continually “teach” the permissions system about changes to the relationships between users and data. On the other hand, the permissions system can actually become the sole, canonical storage for these relationships! When you implement this as a permissions service, it unlocks many very powerful properties.


Google Zanzibar is the realization of such a service, and Google presents a strong case as to why the properties of having permissions calculated in a service were important for Google.


## Why is Google Zanzibar Necessary?


In the Zanzibar paper, Google lays out a number of reasons for deciding that they would benefit from having a permissions service. First, as a service, they’ve reduced the amount of code duplication and version skew to a minimum. Second, Google has a large number of applications and services, and they often need to check permissions between resources from one application in another. For example, when you send an email using Gmail and it warns you that the recipient can’t read a document linked in the email, that works because Gmail is asking Zanzibar about permissions on the linked Google Doc. And similar authorization requests happen across Calendar, Cloud, Drive, Maps, Photos, YouTube, etc. Third, Google has built common infrastructure on top of the permissions system, which is only something you can do when you’ve got a consistent API against which to program. Lastly, and most importantly, permissions are *hard* .


People expect any permissions implementation to adhere to a few common requirements. First and foremost, it should be correct. With permissions, correctness is easy to define. All authorized users should be able to interact with the protected resources, and no unauthorized users should be allowed to interact with the protected resources. This seems easy at first, until you start to take into account the challenges that computers, and especially large scale hosted applications, have to deal with. Things like network and replication delay, node failure, and clock synchronization.


Secondly, if you’re going to use one permissions system for everything, it should reasonably allow you to model all of the different types of primitives that you need for your applications. In Google’s case, they have at least the following permissions models: point-to-point sharing in Docs, public/private/unlisted videos in YouTube, and RBAC in Cloud IAM. Google Zanzibar was made to be flexible enough to model different types of permissions.


Because you usually need to check permissions on every single request, and the failure to receive an affirmative permissions check success must be interpreted as a deny ([fail closed](https://community.microfocus.com/cyberres/b/sws-22/posts/security-fundamentals-part-1-fail-open-vs-fail-closed) ), you need this system to be highly reliable and fast.


Lastly, as Google operates at an extreme scale, Google Zanzibar must also scale to millions of authorization requests per second across billions of users and trillions of objects.


Taken together, these requirements can almost certainly only be solved through some kind of massive distributed system. Now that we’ve laid out some of the requirements, let’s explore the API, or programmer-facing experience, of Zanzibar.


## What is Google Zanzibar to the Developer?


At its core, Google Zanzibar is a globally distributed authorization system capable of processing "[more than 10 million client queries per second](https://zanzibar.tech/2SuZL2BPQU:0:15) ," but from the developer’s perspective, it's an API. The API let's you outsource your user and data relationships, which then gets you rapid and accurate permissions decisions at the point of access. As an example, when a new user registers, you tell Google Zanzibar. When that user creates a protected resource, such as a document, video, or bank account, you tell Zanzibar. When that user shares the resource with other users or creates related resources, you tell Zanzibar. Finally, when it is time to answer the question: “Is X allowed to read/write/delete/update Y?”, Zanzibar already has all of the necessary information to answer the question quickly.


The way that Zanzibar computes permissions is somewhat novel. The relationship information that the application developer writes to the service is used to build up a[directed graph](https://en.wikipedia.org/wiki/Directed_graph) representation of the relationships between users, other entities, and resources. Once this graph is available, permissions checks become a[graph traversal](https://authzed.com/blog/check-it-out/) problem. We attempt to find a path through the graph from the requested resource and relation (e.g. owner, reader, etc) to a user, usually the user making the request.


Often one relationship implies other relationships. For example, if a user is allowed to write a piece of data, it almost (but not always) implies that they can also read the same data. To reduce the amount of redundant information that has to be stored, Zanzibar offers a mechanism called relationship rewrites that describe a method for reinterpreting certain edges and relationships in the graph. Another example of a rewrite would be to say: “readers of the folder in which a document is nested should also be considered readers of the document.” The process of eliminating redundant information in this way is more formally referred to as[normalization](https://en.wikipedia.org/wiki/Database_normalization) .


So far we have only talked about how one checks permissions, but now that we have a normalized graph version of all of the entities in our application, we can also perform other operations against this data, and Zanzibar exposes APIs for those concepts as well. In the Zanzibar paper, sections 2.4.1 and 2.4.5 describe the read and expand APIs, which allow for directly querying the graph data, allowing one to build UIs and downstream processes based on the data being stored. Section 2.4.3 describes the[watch API](https://authzed.com/zanzibar/2IsqbhEn3a:0:B) , which can be used to be notified of changes to the underlying graph data, which is the basis for some of the performance improvements that are described in the implementation of Zanzibar .


The last developer-facing part of the paper that we haven’t mentioned is Zookies. Zookies (presumably a portmanteau of Zanzibar and cookie) allow the developer to set a lower bound on the staleness of data that will be allowed. The main insight here is that by allowing *slightly* stale data to be used for common permissions checking, performance can be drastically improved for those operations. There are still some cases where you absolutely **do not** want stale data to be used when computing permissions, and in those cases you can force Zanzibar to use more recent data by explicitly specifying a Zookie. If you want to learn more about Zookies and how they enforce consistency, I have written about them extensively in a[prior blog post](https://authzed.com/blog/new-enemies/) .


So now that we’re familiar with the API of Zanzibar, let’s take a look at how Zanzibar is implemented to achieve low latency at high scale.


## How is Google Zanzibar Implemented?


Because this service is constantly being accessed, and is in the critical path of serving requests, it has to be **fast** . For Zanzibar at Google, the 50th and 99th percentile latencies for check requests are 3ms and 20ms respectively, all while serving[at peak 12.4 million permissions check and read requests per second](https://zanzibar.tech/2IudGpP-JZ:0.7QDsITJT7:0) from all over the world. How is Zanzibar able to achieve such low latency and high scale?


High scale is achieved by running many, many, many copies of the Zanzibar server:


> Zanzibar distributes this load across more than 10,000 servers organized in several dozen clusters around the world. The number of servers per cluster ranges from fewer than 100 to more than 1,000, with the median near 500. Clusters are sized in proportion to load in their geographic regions.


Global distribution is handled through the use of[Spanner](https://static.googleusercontent.com/media/research.google.com/en//archive/spanner-osdi2012.pdf) , Google’s planet-scale database system. With Spanner, data that is written anywhere on the planet, is available immediately and is externally consistent. While those properties are excellent for the storage layer of a permissions system, that doesn’t mean data that is stored in Spanner is available within the latency requirements of Zanzibar. The read latencies perceived by F1 (another service at Google) from Spanner are 8.7ms at the median, with a standard deviation of 376.4ms. Zanzibar will regularly have to do several round trips to the datastore to compute a single permissions check. Clearly, it’s not achieving 20ms 99.9th percentile latency without some serious caching.


Zanzibar has caching at several layers of the service. The first layer of caching is at the service level. When the service gets a request for a permissions check that it has recently computed, and for which the result can still be considered valid (meaning the time at which it was computed isn’t earlier than a passed Zookie), the value can be returned directly. This eliminates **all** round trips to the datastore. Service level caching is a powerful way to improve performance, but at the scale at which Zanzibar operates wouldn’t help much by itself. The sheer data volume flowing through Zanzibar would lead to very low hit rates or prohibitive memory requirements if any request were allowed to be served from any cache.


To increase the hit rates, Zanzibar uses a[consistent hash](https://en.wikipedia.org/wiki/Consistent_hashing) to distribute requests (and therefore the resulting cache entries) to specific servers. The first benefit that we get from this is much higher hit ratios for the cache. If we expect a specific type of request to only be served by a small subset of the copies of Zanzibar, we’re much more likely to have that value in cache. The second, and more subtle, improvement that this gives is to allow duplicate requests to be combined and the value only calculated once and returned to all callers. In this case we amortize the backend datastore round trips across all deduplicated requests.


The final form of server side caching that Zanzibar performs is a special kind of denormalization specific to Google’s use case. When engineers noticed that groups (as used by Docs, Cloud IAM, the product Groups) were often deeply nested they created a service called Leopard Indexing System. Leopard keeps an in-memory[transitive closure](https://en.wikipedia.org/wiki/Transitive_closure) of all groups that are subgroups of a higher level group. Nested relationships in Zanzibar by default require multiple serial requests to the backing Spanner database because you need to load direct children before being able to compute their children. By keeping all subgroups for all top-level and intermediate groups in memory, Leopard allows Zanzibar to reduce all nested group resolutions to a single call to the index. Because Leopard stores data in-memory and is run as a separate service from Zanzibar, it uses the watch API from section 2.4.3 of the paper to continually keep up to date with changes to the underlying group structure data.


Zanzibar does one more neat trick to reduce tail latencies:[request hedging](https://medium.com/swlh/hedged-requests-tackling-tail-latency-9cea0a05f577) . When Zanzibar detects that responses from Spanner or Leopard are taking longer than usual, it will send out another request for exactly the same data to one or more other servers that are hopefully responding more quickly.


## Toward a Safer and More Consistent Future


Zanzibar (the service) is directly solving real flexibility, scalability, testability, and reusability problems for Google at scale. The Zanzibar paper is superbly written and easy to understand, so much so that when I first read it I identified the proposed solution as one that could have solved permissions flexibility and scalability problems in systems I’ve[built in the past](https://quay.io/) . At[CoreOS](https://www.redhat.com/en/about/press-releases/red-hat-acquire-coreos-expanding-its-kubernetes-and-containers-leadership) we coined the term #GIFEE, which stands for Google’s Infrastructure for Everyone Else.[Authzed](https://authzed.com/) is the next step in that journey, bringing Zanzibar: Google’s Consistent, Global Authorization System… to everyone else.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Consistency is the Key to Performance and Safety](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


Originally published May 12, 2021


On this page


- A Brief History of Application Permissions
- Why is Google Zanzibar Necessary?
- What is Google Zanzibar to the Developer?
- How is Google Zanzibar Implemented?
- Toward a Safer and More Consistent Future
- Additional Reading


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)
