---
schema_version: "1.0.0"
document_id: "143a7e24860f3bfcc0d3b1d14a65912e714ba1f47689a034d839a3ff8249da8d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/performance-impact-of-customizing-the-apollo-router"
published_at: "2023-07-10T10:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:be37cb15b8d1c35c835b8829ab2251efe8ab77aead61bef2f738de30c34dcfcd"
---

# Performance impacts of Apollo Router customizations

The Apollo Router features three customization options out of the box to adjust behavior:[configuration options](https://www.apollographql.com/docs/router/configuration/overview) ,[Rhai scripting](https://www.apollographql.com/docs/router/customizations/rhai) , and[external coprocessing](https://www.apollographql.com/docs/router/customizations/coprocessor) . We refer to these as extensibility options.


Extensibility of the Router is one of its primary strengths and is a critical feature for many organizations large and small. Understanding the performance implications of these options is important as the Router is designed for performance and serves as the entry point to your supergraph. We’re excited to share results with you covering these extensibility options, as well as a public GitHub repository where you can test them yourself.


Before diving into the methodology and detailed results, the high-level results are:


- When you’re able to choose, configuration options perform better than Rhai scripts, and Rhai scripts perform better than external coprocessing.
- When you must use an external coprocessor for more complex logic, we see about 350μs overhead.
- The choice of programming language, complexity of task, network proximity, type of host machine, and other factors will have varying impacts on the real-world performance of your Router and coprocessor, so ensure you consider the best tool for the job, so to speak, and keep the coprocessor as close as possible to the Router. Ideally this would be within the same Pod (if using Kubernetes) or similar for other orchestration systems.


[The GitHub repository](https://github.com/apollosolutions/router-extensibility-load-testing) includes more information about the results, including specific numbers, as well as the ability to run these tests yourself.


### **Methodology**


As with any test, determining how to best isolate the variable you want is critical to ensure you’re not introducing other factors into the results. For the tests we ran, we wanted to isolate the overhead for each extensibility option, not the performance impact of each test we ran.


To help isolate overhead latency, we ensured that we pre-warmed the query plan cache to remove the initial overhead of query planning to the first request (which may have skewed results by adding a request with a higher maximum latency value). We do this by running curl commands after the Router restart with both of the test queries.


We also limited the resources for each running Docker image to use only 1 CPU core and 1GB of RAM as a way to ensure a consistent result from run to run. While this may have impacted some programming language implementations, we wanted to give a level playing field for each test.


With that in mind, we designed three different tests for the extensibility options. This allowed us to show various overheads, as well as showing the impact of not only the complexity, but potential impact of the coprocessor stages.


- Setting a static subgraph header, which is supported by all three extensibility options. This was the simplest example available, and helped test the overhead of a SubgraphRequest coprocessor stage. Since this logic is run per-subgraph on both the coprocessor and in Rhai, we wanted to understand the overhead of multiple invocations.
- Setting 10 GUID headers on a response, which is supported by only Rhai and external coprocessing. This test was designed to have slightly more complex logic, while also showing something that wasn’t possible with a basic configuration.
- Lastly, we tested setting the client awareness headers using claims within a JSON Web Token (JWT) which can only be done in a coprocessor as of writing. This was the most complex logic we tested, and was a way to show how complexity could affect results as well as being an example of something only possible with external coprocessing.


Each of these tests were run against a baseline Router (that is, a basic Router without any configured extensibility options), and a Router with the respective extensibility options configured with coprocessors being implemented in Node.js, Python, Go, Java, and C#. Between each run, we restart the Router and pre-warm the query plan cache.


For every test, we utilized[a custom version](https://github.com/apollosolutions/router-extensibility-load-testing/tree/main/loadtester) of[Vegeta](https://github.com/tsenart/vegeta) , a Go-based load testing utility. Our version handles GraphQL errors within the payload and treats them as non-2XX results for the purposes of reporting failures appropriately. Each run was set to 100 requests per second for 60 seconds to adequately load-test the system and provide enough data points for reference.


### **Results**


#### **Overview**


Diving into the results, we saw that most workloads benefit by using configuration options over Rhai scripts, and Rhai scripts are better than external coprocessing in terms of overhead.


Rhai performance was excellent across the board, with latency overheads in p95s being around 100μs. This overhead makes it an excellent choice for most customization needs, as it tends to cover most use-cases that can’t traditionally be done with the basic configuration options.


External coprocessing overhead was still excellent at 350μs overhead per stage average across all tests and languages, so for those that need more complex logic (such as the client awareness test showed), it can still be an excellent fit with great performance. With that in mind, there are additional elements here that are worth diving into.


- Your runtime/programming language matters, and will affect results depending on the type of customizations you need.


Each language has its strengths and weaknesses, so ensure you pick the right tool for the job when deciding what to use for your coprocessor.


- Even more important than runtime, your networking setup is critical to ensuring optimal performance.


Since external coprocessing communicates over HTTP, reducing the latency between your Apollo Router and external coprocessor will ensure optimal performance and reduce overhead. If you are using Kubernetes, for example, running the coprocessor as a container alongside the Router in the same Pod can ensure network proximity.


- Ensure you need to use SubgraphRequest/Response stages before using them


The coprocessor overhead is per stage, so if you run a query that hits 5 subgraphs, you’ll end up with 5 invocations of the SubgraphRequest/Response stages for a coprocessor. This overhead can add up quickly, so it is advisable to validate your need to use this stage before utilizing it.


#### **Per-Test Results**


Looking at each test individually, we can start to make sense of some of the above information, as well as see the percentage differences from the baseline.


The numbers used were from the most recent test, running on Windows using WSL2, but your latency numbers may vary depending on resources available, CPU, OS, and more. The takeaways themselves should remain the same.


Results for setting a static subgraph header


For the static subgraph header test, we added a “coprocessor” header with the value of the type of extensibility (e.g. rhai for Rhai scripts). This leveraged the SubgraphRequest stage for a coprocessor, and we aimed to test the overhead of making multiple requests. For this test, we averaged 1.5 subgraphs per request.


We can start to see that we are running into minute variance issues. In the above chart, we show that both Python and the config option are faster than the baseline- but the raw data shows that they are only 40μs and 10μs difference, respectively. Regardless, the actual overhead was measured around 330μs averaged across all coprocessor types. For specific details on each option, see the[full results in the GitHub repository](https://github.com/apollosolutions/router-extensibility-load-testing/tree/main#static-subgraph-header) .


Results for adding 10 GUIDs to a header in the response


In the GUID response headers test, we wanted to add a level of logic that the configuration couldn’t provide, as well as test the impact of a very basic looping logic to add dynamically generated GUIDs to the Router’s response. To do so, we leveraged the RouterResponse stage for a coprocessor.


Breaking down the numbers, we see clearly that Rhai is far more performant- only 11% higher p95 latency than baseline, or only 10μs in our tests versus the minimum 17% change when using a Go-based coprocessor, and the others being higher.


Results for identifying clients based on JWT claims


For our last test, we wanted to test something logically complex as well as something only a coprocessor could do. This test decoded a JWT using an HMAC256 secret, and then used the claims to populate the apollographql-client-name and apollographql-client-version headers[to enable client awareness](https://www.apollographql.com/docs/router/managed-federation/client-awareness/) for visibility in Apollo Studio.


This test shows how, while the HTTP overhead itself is minimal, the actual logic is far more important. Introducing complex logic can affect performance in a meaningful way, so using other extensibility options whenever possible will help reduce the overall overhead.


### **Conclusion**


1. Prefer Router configuration options over other customization options whenever possible.
2. External coprocessors add, on average, only 350μs of overhead plus the overhead of your logic. This number can change depending on other factors, identified above.
3. Keep coprocessors as close as possible to the Router to ensure the overhead doesn’t become too burdensome.


View the full results and implementation on the[public GitHub page](https://github.com/apollosolutions/router-extensibility-load-testing) .


Written by


Lucas Leadbetter


[Read more by Lucas Leadbetter](https://www.apollographql.com/blog/author/lucas)
