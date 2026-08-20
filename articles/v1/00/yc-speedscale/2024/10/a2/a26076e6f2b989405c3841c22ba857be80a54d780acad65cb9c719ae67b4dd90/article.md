---
schema_version: "1.0.0"
document_id: "a26076e6f2b989405c3841c22ba857be80a54d780acad65cb9c719ae67b4dd90"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/mockserver-https-apis/"
published_at: "2024-10-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:e59842de1661b92912dc83341acfc22933501cfef5ca822ab8f7724ccdaad27f"
---

# How to Mock HTTP APIs With MockServer

MockServer is a powerful tool for mocking HTTP APIs, making running integration tests more manageable and efficient in the initial development phases. Allowing developers to make HTTP calls on an HTTP client without needing a real server becomes essential for ensuring tests run reliably. With MockServer, you can create a fully developed[mock](https://speedscale.com/blog/mock-services-in-software-development/) API that simulates the behavior of a real server, including response headers, request headers, and even network failure scenarios. This capability is particularly useful when the server is not yet available or under development, enabling you to continue your work without interruptions.


## Why use MockServer to mock APIs?


As the name suggests,[MockServer](https://www.mock-server.com/) is a powerful tool that allows developers to[mock HTTP APIs](https://speedscale.com/blog/mock-api-toolset/) , making it much easier and more efficient to run an integration test in initial development phases. It aims to solve the problem of testing APIs that are still in development or not yet available, allowing developers to make http calls on an http client without a real server.


While there are other popular mocking tools available, such as[WireMock](https://www.baeldung.com/introduction-to-wiremock) and[Pact](https://www.testingwithmarie.com/post/contract-testing-with-pact-js-and-jest) , MockServer stands out for its focus on providing a quick and easy mock solution that’s supported in most major programming languages and technologies. MockServer interacts with single-page applications by loading static resources through a web server. The web server serves HTML, CSS, and JavaScript while utilizing AJAX calls to access separate services, emphasizing the importance of mocking these interactions for isolation and debugging purposes.


This post will cover step-by-step instructions on how to use MockServer to mock an HTTP API, as well as an overview of any limitations you might encounter. It’s important to note that while MockServer is a great tool, it may not be the best fit for every use case. This post intends to present a great use case for MockServer, but remember to evaluate for yourself and your unique scenario.


## What is MockServer?


A mock server is designed to simulate external HTTP APIs (REST API, RPC service, etc.) through a mock, proxy, or both. By letting an http client invoke real http calls, a mock server speeds up the initial development phases without needing a real server. MockServer stands out from other mocking tools due to its impressive range of deployment options. Just in[Java](https://www.mock-server.com/mock_server/running_mock_server.html#client_api) , you can mock server endpoints through test assertions like a JUnit 4 @Rule, a JUnit 5 Test Extension, or a Spring Test Execution Listener, so MockServer can fit into a variety of workflows.


The versatility and adaptability of MockServer set it apart from its peers. No matter the environment – be it your local machine, Docker-enabled, or[Kubernetes](https://www.mock-server.com/mock_server/running_mock_server.html#helm_chart) , MockServer likely integrates well into your infrastructure. Its compatibility with popular build tools like Maven and Grunt and the ability to be used programmatically through Java or Node.js makes it applicable in a variety of testing scenarios. MockServer can also be used to mock interactions with a web server, which serves HTML, CSS, and JavaScript, and utilizes AJAX calls to access separate services.


It’s an excellent choice when you want to keep things local or need a tool that can be worked directly into the code regardless of the language you’re using.


## Benefits of Using MockServer


MockServer offers several benefits that make it an essential tool for developers and testers. Here are some of the key advantages of using MockServer:


1.


Improved Test Reliability: One of the standout benefits of MockServer is its ability to ensure that tests run reliably and consistently, regardless of the availability or stability of external services. By mocking out dependencies, you can isolate your application’s behavior and ensure that tests are not affected by irrelevant external changes. This means that even if an external service is down or experiencing issues, your tests will still run smoothly.


2.


Faster Development: With MockServer, you can start developing your application without waiting for external services to be fully developed or deployed. This allows you to work more efficiently and make progress on your project without being held back by dependencies. For instance, you can mock the API of a service that is still under development and continue building your application without delays.


3.


Reduced Test Complexity: MockServer simplifies the testing process by allowing you to focus on specific components or behaviors without worrying about the complexity of external services. This makes it easier to write and maintain tests, and reduces the overall complexity of your testing infrastructure. By isolating the component under test, you can create more targeted and effective test cases.


4.


Increased Test Coverage: By using MockServer, you can test scenarios that would be difficult or impossible to test with real external services. This includes testing for network failure, errors, or other edge cases that might not be easily replicable with real services. For example, you can simulate a network failure to see how your application handles it, ensuring that your application is robust and resilient.


5.


Improved Collaboration: MockServer enables developers and testers to work independently of each other, without being blocked by dependencies on external services. This improves collaboration and reduces the risk of conflicts or delays in the development process. Developers can mock the APIs they need, while testers can create and run their tests without waiting for the actual services to be available.


## Configuring MockServer


Configuring MockServer is a straightforward process that involves setting up the server and defining expectations for how it should behave. Here are the general steps for configuring MockServer:


1.


Download and Install: Start by downloading the MockServer distribution from the official website and installing it on your system. You can choose to install it as a standalone server or integrate it with your existing development environment, such as Docker or Kubernetes.


2.


Define Expectations: Define expectations for how MockServer should behave in response to incoming requests. This includes specifying the request headers, method, and body, as well as the expected response headers, status code, and body. You can define these expectations using JSON, Java, or JavaScript, depending on your preference and project requirements.


3.


Configure Proxying: Configure MockServer to proxy requests to external services. This allows you to test scenarios where your application needs to interact with external services, while still maintaining control over the behavior of those services. By setting up proxying, you can simulate the behavior of external services and test how your application handles different responses.


4.


Set up HTTPS Tunneling: Set up HTTPS tunneling to enable secure communication between your application and MockServer. This is particularly important when testing scenarios that involve sensitive data or secure protocols. By configuring HTTPS tunneling, you can ensure that your tests accurately reflect real-world conditions and maintain the security of your data.


5.


Customize Behavior: Customize the behavior of MockServer to suit your specific testing needs. This includes setting up custom response headers, status codes, and bodies, as well as defining custom expectations for specific scenarios. By tailoring MockServer to your requirements, you can create more effective and targeted tests.


## Running MockServer


Running MockServer is a simple process that involves starting the server and verifying that it is functioning correctly. Here are the general steps for running MockServer:


1.


Start the Server: Start the MockServer process, either manually or as part of your automated testing infrastructure. You can run MockServer as a standalone process, or integrate it with your CI/CD pipeline to ensure that it starts automatically during your test runs.


2.


Verify Functionality: Verify that MockServer is functioning correctly by sending test requests to the server and verifying the responses. This step ensures that MockServer is set up correctly and that it is responding as expected to the defined expectations.


3.


Test Scenarios: Test specific scenarios that involve interacting with external services, such as sending a POST request or retrieving data from a web service. By testing these scenarios, you can ensure that your application handles different types of requests and responses correctly.


4.


Monitor Behavior: Monitor the behavior of MockServer and your application to ensure that they are interacting correctly and that tests are running as expected. This includes checking the logs and verifying that the responses match the defined expectations.


5.


Stop the Server: Stop the MockServer process when testing is complete, either manually or as part of your automated testing infrastructure. This ensures that MockServer does not continue running unnecessarily and consuming resources.


## How to mock HTTP APIs


Aiming to provide value to as many as possible, this tutorial will use one of the language-agnostic options:[docker-compose](https://www.mock-server.com/mock_server/running_mock_server.html#docker_container) . MockServer will be used to establish a mock HTTP API for a hypothetical online game platform. This platform offers a RESTful API that game developers use to fetch player statistics, update game scores, and manage in-game purchases. The web server serves static resources like HTML, CSS, and JavaScript for the platform, and MockServer can mock these interactions. Our objective is to replicate this API’s behavior with MockServer, allowing us to test the client-side interactions independently, without requiring a connection to the actual game platform’s servers.


### Configure expectations using JSON initializer


In the MockServer ecosystem, request/response pairs are referred to as[Expectations](https://www.mock-server.com/mock_server/creating_expectations.html) , which are test assertions written as Java or JavaScript code, or, you can define them in a JSON file—which is what you’ll do now. This approach allows you to separate your mock definitions from your code, making them easier to maintain and reuse, especially across projects. The JSON initializer can include expectations for interactions with a web server serving static resources like HTML, CSS, and JavaScript.


### Create an initializer.json file in your current directory with the following content:


```text
[
{
"httpRequest"  : {
"method"  :   "GET"  ,
"path"  :   "/player/stats"  ,
"queryStringParameters"  : {
"playerId"  : [  "1234"  ]
}
},
"httpResponse"  : {
"statusCode"  :   200  ,
"body"  :   "{"  playerId  ": "  1234  ","  gamesPlayed  ": 57,"  averageScore  ": 3200,"  topScore  ": 5600}"  ,
"headers"  : {
"Content-Type"  : [  "application/json"  ]
}
}
}
]
```


This JSON file declares an expectation for a GET request to /player/stats with a playerId query parameter. When this request is received, the server responds with a 200 status code and a JSON body containing some made-up player statistics.


## MockServer Functionality


MockServer provides a range of functionality that makes it an essential tool for developers and testers. Here are some of the key features of MockServer:


1.


**Mocking** : MockServer allows you to mock out external services, including web services, RPC services, and other HTTP-based systems. By creating mock responses, you can simulate the behavior of these services and test how your application handles different scenarios.


2.


**Proxying** : MockServer can proxy requests to external services, allowing you to test scenarios where your application needs to interact with external services. This feature is particularly useful for testing how your application handles different responses from external services.


3.


**HTTPS Tunneling** : MockServer supports HTTPS tunneling, enabling secure communication between your application and the server. This is important for testing scenarios that involve sensitive data or secure protocols, ensuring that your tests accurately reflect real-world conditions.


4.


**Customizable Behavior** : MockServer allows you to customize its behavior to suit your specific testing needs, including setting up custom response headers, status codes, and bodies. By tailoring MockServer to your requirements, you can create more effective and targeted tests.


5.


**Expectations** : MockServer allows you to define expectations for how it should behave in response to incoming requests, making it easier to test specific scenarios and edge cases. By setting up expectations, you can ensure that MockServer responds correctly to different types of requests and test how your application handles various situations.


## Using MockServer as a Proxy


MockServer can be used as a proxy to test scenarios where your application needs to interact with external services. Here are some of the benefits of using MockServer as a proxy:


1.


**Test External Services** : MockServer allows you to test external services, including web services and RPC services, without actually calling the external service. By proxying requests, you can simulate the behavior of these services and test how your application handles different responses.


2.


**Isolate Dependencies** : MockServer enables you to isolate dependencies on external services, making it easier to test your application in isolation. This ensures that your tests are not affected by irrelevant external changes or issues with external services.


3.


**Test Edge Cases** : MockServer allows you to test edge cases and error scenarios that might be difficult or impossible to test with real external services. By simulating different responses, you can ensure that your application handles various situations correctly.


4.


**Improve Test Reliability** : MockServer improves test reliability by ensuring that tests are not affected by irrelevant external changes or network failures. This means that even if an external service is down or experiencing issues, your tests will still run smoothly.


5.


**Reduce Test Complexity** : MockServer reduces test complexity by allowing you to focus on specific components or behaviors without worrying about the complexity of external services. By isolating the component under test, you can create more targeted and effective test cases.


### Set up MockServer using Docker-Compose


With the expectations defined you can now spin up the MockServer using docker-compose. Docker-compose may not be strictly necessary in this case as it’s only one container, however, it helps provide a clear and approachable overview of container configurations.


### Create a docker-compose.yaml file with the following content:


```text
version  :   '3'
services  :
mockServer  :
image  :   mockserver/mockserver:latest
ports  :
-   1080:1080
volumes  :
-   ./initializer.json:/config/initializer.json
environment  :
MOCKSERVER_INITIALIZATION_JSON_PATH  :   /config/initializer.json
```


This configuration exposes the MockServer on port 1080 and mounts the initializer.json file from your current directory into the /config directory inside the container, which the environment variable` MOCKSERVER_INITIALIZATION_JSON_PATH` points to so MockServer can load your expectations from it.


To start the MockServer container, run docker-compose up -d which will start the MockServer in the background.


### Interacting with the mock


Once MockServer is configured and running, interacting with the mock API is very similar to interacting with any other RESTful API. Given that you’ve only set up an expectation for a “GET” request to /player/stats endpoint with a playerId of 1234, let’s test that on your local machine.


Get a response from the mock API by running:


```text
curl "http://localhost:1080/player/stats?playerId=1234"
```


Barring any bugs, you should see a response like above, which would indicate a successful mock implementation. However, MockServer does provide a better way of verifying mock API interactions.


### Verify requests


Verification is used to ensure that the MockServer received a specific request. For instance, after running some tests, you might want to verify that your client sent a GET request to fetch player stats:


```text
curl -v -X PUT   "http://localhost:1080/mockserver/verify"   -d '{
"httpRequest"  : {
"method"  :   "GET"  ,
"path"  :   "/player/stats"  ,
"queryStringParameters"  : {
"playerId"  : [  "1234"  ]
}
},
"times"  : {
"atLeast"  :   1  ,
"atMost"  :   1  ,
}
}'
```


This code verifies that the MockServer received exactly one GET request to /player/stats for the player with id 1234.


And there you have it! You’ve successfully utilized MockServer with docker-compose and JSON configuration to mock an HTTP API.


### Advanced Features and Use Cases


MockServer offers a range of advanced features that make it a versatile tool for integration testing. One of its key features is the ability to create test assertions, allowing developers to verify that the system is behaving as expected. This ensures that your tests are running and producing the correct outcomes. Additionally, MockServer supports HTTPS tunneling and proxying, making it easy to test secure APIs. This is particularly useful for applications that require secure communication channels. Furthermore, MockServer can simulate RPC services, allowing developers to test complex systems in isolation. This flexibility makes MockServer an invaluable tool for many testing scenarios.


## Best Practices for Reliable Testing


To get the most out of MockServer, following best practices for reliable testing is essential. One key best practice is to ensure that the mock API is fully developed and simulates the behavior of the actual server as closely as possible. This includes setting up request and response headers and even simulating network failure scenarios. Doing so allows you to create a testing environment that closely mirrors real-world conditions, making your tests more complete and reliable. Another best practice is using MockServer with other testing tools, such as Java code, to create a comprehensive testing suite. This approach allows you to leverage the strengths of multiple tools, ensuring that your tests cover all possible scenarios and[edge cases](https://speedscale.com/blog/validate-edge-cases-kubernetes/) .


## The limitations of MockServer


While MockServer is a powerful tool for testing and developing applications, it’s important to be aware of its limitations. For instance, while MockServer can mock interactions with a web server, it may have limitations in handling complex scenarios involving dynamic content. Here are some aspects you should consider when deciding if MockServer is the right solution for your project.


### Limited support for non-HTTP protocols


MockServer has been primarily designed to handle HTTP and HTTPS requests, which makes it a versatile tool for testing web applications. However, when it comes to non-HTTP protocols, its capabilities become limited. For example, protocols such as gRPC and WebSocket, which are widely used in real-time applications, are not supported in MockServer. This could be a significant limitation if your application heavily relies on these real-time, bidirectional communication protocols. Additionally, while MockServer can handle HTTP and HTTPS requests, its limitations might affect interactions with a web server serving static resources like HTML, CSS, and JavaScript, which are crucial for single page applications utilizing AJAX calls.


Furthermore, while MockServer allows for updating expectations on-the-fly, its support for real-time updates and feedback during testing is not as robust as some might wish. This becomes particularly relevant in scenarios that require rapid adjustment of expectations based on ongoing test results or changing application states.


For instance, testing a system that needs to adapt to fluctuating network conditions or[user loads](https://speedscale.com/blog/determine-throughput-performance-testing/) would necessitate a tool that can accurately simulate these real-world behaviors. Unfortunately, MockServer’s real-time adaptation capabilities are currently limited.


### Possible technical and human performance overhead


MockServer does provide some[basic templating](https://www.mock-server.com/mock_server/response_templates.html) and dynamic response capabilities, but these might not suffice for more complex, data-driven scenarios. For instance, if your use case involves crafting intricate responses based on specific request parameters, or if you require random data generation for chaos testing, you might find the tool’s capabilities restrictive, which may mean you need to resort to additional programming or scripting to achieve the desired level of dynamism in your responses.


Coupling this with the performance overhead that MockServer might introduce, especially when dealing with a large number of tests or high request volumes, you have a two-fold challenge. While MockServer is highly efficient, the fact that it runs as a separate external process means that it does consume additional resources. In scenarios where hundreds or even thousands of tests are run concurrently, this overhead could potentially impact the application’s performance during testing. Additionally, the performance overhead of MockServer might impact interactions with a web server serving static resources like HTML, CSS, and JavaScript, which are crucial for single page applications utilizing AJAX calls.


In fairness to MockServer, this is true for any mock or load generation tool. However, MockServer comes with few infrastructure optimizations, handing over that responsibility to you.


### Limited traffic shaping capabilities


One of the challenges when using MockServer is its limited traffic shaping capabilities. Traffic shaping is the process of controlling the speed, volume, and nature of network traffic. It’s a valuable technique when trying to simulate real-world network conditions for applications, such as latency, packet loss, or bandwidth limits. Unfortunately, MockServer doesn’t inherently provide these features. This limitation might affect interactions with a web server serving static resources like HTML, CSS, and JavaScript, which are crucial for single page applications utilizing AJAX calls.


While you can delay responses in MockServer to simulate network latency, the functionality doesn’t extend much beyond that. For instance, it lacks native support for simulating different network qualities, such as 3G or 4G, or varying conditions, such as congested networks.


You’d need to resort to additional tooling or potentially complex workarounds to replicate these conditions effectively. This limitation may pose challenges in accurately simulating real-world scenarios or conducting comprehensive performance testing. This is of course before mentioning how some tools are moving away from traffic shaping, favoring[production traffic replication](https://speedscale.com/blog/definitive-guide-to-traffic-replay/) instead.


### Limited built-in monitoring and analytics


Monitoring and analytics are crucial aspects of API testing and development. They provide insights into the performance, usage, and potential issues of your APIs. However, MockServer provides limited built-in monitoring and analytics features, as seen with the /verify endpoint from earlier. This limitation might affect interactions with a web server serving static resources like HTML, CSS, and JavaScript, which are essential for single page applications utilizing AJAX calls.


While MockServer does[log all interactions](https://www.mock-server.com/mock_server/debugging_issues.html) , including setting up expectations, matching expectations, clearing expectations, and verifying requests, these logs might not be sufficient for deeper analysis or long-term monitoring. For example, you might want to monitor the rate of specific types of requests, track response times over time, or analyze usage trends for capacity planning. These advanced analytics capabilities are not natively available in MockServer.


While you can extract some of this information from the logs, doing so can be time-consuming and might require additional tooling or processing. Without a robust, built-in analytics solution, diagnosing issues, optimizing performance, or gaining insights into your API’s usage can be more challenging.


### Troubleshooting and Support


If you encounter any issues with MockServer, several resources are available to help you troubleshoot and resolve the problem. The MockServer documentation provides detailed instructions on how to use the tool and troubleshooting guides for common issues. Additionally, the MockServer community is active and responsive, with many developers contributing to the project and providing support through forums and GitHub. These resources can be invaluable when you encounter challenges, offering solutions and advice from experienced users.


## Align the tool with your use case


In conclusion, this post has provided a comprehensive guide on how to use MockServer to mock HTTP APIs, with a focus on an intriguing example of an online game platform’s RESTful API. MockServer can be used to effectively isolate client-side interactions for testing, with minimal dependency on actual server availability. Additionally, MockServer can be used to mock interactions with a web server serving static resources like HTML, CSS, and JavaScript. This is particularly useful for single page applications that utilize AJAX calls to access separate services, emphasizing the importance of mocking these interactions for isolation and debugging purposes. However, while MockServer is a powerful tool, it’s important to consider its limitations as a mock server. If simulating complex scenarios or real-world network conditions, a different mock server might be better for your integration test.


For instance, while MockServer does support deployment in[Kubernetes](https://speedscale.com/blog/setting-up-a-multi-architecture-kubernetes-cluster/) , you may be overwhelmed by the additional infrastructure management it requires. Instead, you’ll want to research other possible ways of[mocking APIs in Kubernetes](https://speedscale.com/blog/how-to-mock-apis-in-kubernetes) .


## Conclusion


In conclusion, MockServer is a powerful tool for mocking HTTP APIs and ensuring reliable integration testing. With its advanced features, versatility, and active community, it’s an essential tool for any developer looking to improve their testing workflow. By following best practices and using MockServer with other testing tools, developers can create comprehensive testing suites that simulate real-world scenarios, including network failure, HTTPS tunneling, and proxying. Whether you’re working on a web server, RPC service, or any other system, MockServer is an invaluable resource for ensuring that your tests run reliably and efficiently.
