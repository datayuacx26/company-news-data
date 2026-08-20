---
schema_version: "1.0.0"
document_id: "92aa361d995ec84414d56664e4931a95ad563727c873cdb54ec5a363496505df"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/mock-grpc-apis/"
published_at: "2023-11-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:d22fd66223a547a871a87372334f92a78400cb885f1082bc636fcd0df6f60513"
---

# How to Mock gRPC APIs

Developing APIs can be a complex process, particularly when you want to ensure they perform as expected under various real-world conditions. API mocking offers a streamlined, effective way to simulate these conditions during the testing phase. But how can you mock gRPC APIs, which are inherently more complex due to their use of[protocol buffers](https://grpc.io/docs/what-is-grpc/introduction/) for data transfer?


This post covers the differences between HTTP and gRPC mocking protocols, plus steps and considerations for implementing a gRPC mock server using Speedscale’s[production traffic replication](https://speedscale.com/blog/definitive-guide-to-traffic-replay/) , a technique that leverages actual user interaction data to create a highly realistic mock server.


```text
HTTP API vs. gRPC API
```


[HTTP is a stateless, request-response protocol](https://speedscale.com/blog/rest-api-testing/) designed for transferring data on the web. HTTP-based APIs usually send and receive data in a text-based format like JSON or XML. This text-based nature means you can easily inspect the headers and body of an API response, making it straightforward to generate these mock responses.


On the other hand,[gRPC is an open-source](https://grpc.io/docs/what-is-grpc/core-concepts/) , high-performance[RPC (Remote Procedure Call)](https://www.geeksforgeeks.org/remote-procedure-call-rpc-in-operating-system/) framework developed by Google. Unlike HTTP, gRPC APIs use[protocol buffers (protobuf)](https://protobuf.dev/) to transfer data. This enables gRPC to offer some significant advantages, such as efficient serialization, language neutrality, and type checking.


However, the use of protocol buffers presents challenges when it comes to mocking. Since it’s not as readily interpretable or manipulable as JSON or XML, a gRPC API requires a more sophisticated approach to decode and encode messages, making a gRPC mock server more complex.


## The importance of realistic mock servers


Despite the complexities of simulating and mocking gRPC APIs, the importance of having realistic mock servers cannot be overstated. Mock servers play a crucial role in the software development lifecycle, enhancing the[efficiency, cost-effectiveness, and overall quality](https://speedscale.com/blog/the-6-best-performance-testing-tools/) of your software.


Additionally, efficient and realistic mock servers can significantly[improve the agility of your development teams](https://speedscale.com/blog/mock-services-top-use-cases/) , as they allow teams to work independently—when fixing a simple bug, and collaboratively—when developing a new feature. Realistic mock servers have proven beneficial in various use cases. For instance, they can help in[detecting bugs that would only surface under real-world conditions](https://speedscale.com/blog/resilience-testing/) .


## What is production traffic replication?


[Production traffic replication](https://speedscale.com/blog/definitive-guide-to-traffic-replay/) imitates real user interactions by capturing user data in a live environment and then replaying it in a testing environment.


For example, if you have an online bookstore, conventional test scenarios might cover common use cases like searching for a book, adding it to the cart, and checking out. Conventional test scenarios, however, may not account for real-world complexities, such as: multiple users accessing the site concurrently, searching for unavailable or out-of-stock books, or unexpectedly abandoning transactions mid-funnel. A mock server replicating production traffic captures these use cases, providing a more accurate model of your service under real-world conditions.


### The complete traffic replay tutorial


Learn how to set up traffic capture and replay it for more accurate testing of your Kubernetes apps.


```text
Get it here


Step 1: Capture traffic
```


When capturing traffic, there are a few considerations to keep in mind:


### The recorded traffic selection


### Use a varied sample, beyond basic request types


Be sure to get a varied sample of your production traffic. For example, if your gRPC service facilitates different procedural calls or varying message structures, include them in your recorded traffic. The power of traffic replication lies in the accurate simulation of real-world conditions, so don’t restrict your selection to just common user operations or basic request types.


### Use a longer sample, but not too long


The duration of your recorded traffic also plays a key role in building representative mock servers. If it’s too short, you might miss out on variations in usage patterns, but if it’s too long, you might drown in a sea of data. You’ll need to strike a balance to ensure your mock service accurately mirrors your live service.


### Leverage snapshots to experiment easily


Speedscale captures traffic continuously, allowing you to focus on the amount of traffic to store in a snapshot, with the possibility of creating snapshots with overlapping traffic. This means you can experiment with different snapshots. Mock servers use a snapshot-id to determine which traffic should be used, so you can easily switch. Filters to narrow down traffic to just the calls you’d like to include in the snapshots are also available.


```text
Data sensitivityProtect sensitive dataProduction traffic might contain sensitive user information, making it crucial to anonymize or eliminate data during traffic capture. For instance, if your gRPC service handles user authentication, you’ll need to mask sensitive data such as passwords and tokens to abide by regulations like GDPR.
Maintain data security & complianceTo avoid potential security breaches, it’s equally important to secure all captured data as rigorously as you would with production data. If you’re dealing with sensitive data like health records, then maintaining compliance with regulations like HIPAA is essential. Speedscale’s single-tenant, SOC2 Type 2 certified architecture ensures safe storage of all your traffic.


Inspecting traffic
```


If your service uses encrypted gRPC traffic, ensure that Speedscale can decrypt it, as the usefulness of traffic is otherwise limited. For instance, if SSL/TLS encryption is used, you’ll need to[provide Speedscale with the necessary keys](https://docs.speedscale.com/setup/sidecar/tls/) for decryption.


## Step 2: Create a mock gRPC server


In gRPC, correctly decoding protobuf messages is crucial. Improper decoding can result in a less representative mock server, potentially leading to inaccurate testing results. If your gRPC service employs nested protobuf messages, ensure the structure is decoded correctly so the mock maintains identical behavior in your mock service.


While most gRPC mock servers rely on the service definition from your .proto files to understand your gRPC service’s structure, Speedscale[decodes gRPC into a general-purpose JSON format](https://docs.speedscale.com/guides/advanced-filters/#filter-by-grpc-value) , eliminating access needs to any .proto file. This grants greater flexibility when filtering out specific transactions for testing, as well as reducing the complexity of creating mock servers.


To test a wider variety of scenarios, consider adding simulated network latencies to your gRPC mock server. You may also consider removing the latency to speed up development, like a developer testing locally. While this introduces a certain level of artificiality, it can offer valuable insights into your service’s response to delays and packet loss. And, you’ll still get the benefits of traffic data being realistic.


## Step 3: Integrate with your test environment


The service under test can now interact with this mock gRPC server as if it were the actual service, providing an environment that closely mirrors your production setup and the behavior of actual users. Next, ensure that your test environment’s network setup allows for communication with the gRPC mock server. This usually involves configuring network rules or setting up service discovery to locate the mock server. If you’re running services in Kubernetes, Speedscale[automates this process](https://speedscale.com/blog/test-kubernetes-ingress/) .


### What to know about mocking APIs within Kubernetes?


Learn network implementations, how much of the environment to replicate, and best practices when creating mocks.


```text
Read The Blog
Handling development dependencies
```


## Step 4: Test and iterate


You can now start testing gRPC APIs by executing your usual suite of tests against the gRPC mock server. At this point, there may be mock functionality or API usage highlighted by your traffic capture that’ll lead you to consider adding new test cases.


Also, make sure to implement logging and[monitoring](https://speedscale.com/blog/a-guide-to-kubernetes-visibility-best-practices-and-tools/) for your test environment. It’s rarely enough to know that something is broken; you’ll need to determine the cause of the issue in order to fix it. This especially comes into play during[performance testing](https://speedscale.com/blog/a-developers-guide-to-continuous-performance-testing/) , which involves testing your service under varying levels of load and observing that behavior.


Lastly, your mock gRPC server should evolve in parallel with your live service. By continuously capturing traffic, you can ensure that your mock server stays up-to-date with your live service. All that’s needed is to change the snapshot-id during traffic replay executions.


Remember, the goal is to make your test environment as close to the production environment as possible. This means the traffic should be similar, the behaviors should be consistent, and the responses should be representative of what users would actually experience. If you need to test cases that are not in production already, use recorded traffic as the foundation and modify it. By doing this, you ensure that your service is ready to handle the real world.


Learn more about using gRPC
BLOG
Choosing an API technology: gRPC, REST, GraphQL


```text
Learn More


BLOG
Using gRPC with Golang


Learn More
```
