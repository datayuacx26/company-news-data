---
schema_version: "1.0.0"
document_id: "56655379cbf6bdb42acf6cb121bb140ff6447d30c6947660f02555dd0ff517b0"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/eliminating-flaky-tests-with-traffic-replay/"
published_at: "2025-05-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:3993b5f50b0764422afb03563e67c6711f2d0e9b649d8df01793c38574d8b474"
---

# Eliminating Flaky Tests with Traffic Replay

There are few things that can derail developer productivity and undermine your pipeline like a flaky test.


Testing is the backbone of a good development process, ensuring that your code is as accurate and usable as possible. When these tests point towards faulty development, the impacts can be significant. This information is predicated on an assumption, however - the assumption that what the test says is accurate.


When these tests aren’t accurate - or worse, when they’re undependable - they can introduce huge issues, sapping hours from your engineering teams, clogging your CI/CD pipelines, and introducing doubt into decision-making. You can quickly find yourself asking: Is this test failure an actual issue, or just a ghost in the machine?


As the systems we develop grow ever more complex, with mounting third-party dependencies, complex agentic AI integrations, and complex microservice interconnections, these flaky tests become much more than a nuisance, mounting to true blockers and bottlenecks.


What if your tests could be deterministic, stable, and fast - at the same time?


Today, we’re going to look at why flaky tests are flaky, how they get that way, and what issues this introduces. We’ll look at a way to make your tests actually useful, powerful, and accurate - and give you a way to start testing better in **just a few minutes** . Reliable and accurate tests are crucial for maintaining the effectiveness and adaptability of your testing strategies.


## Introduction to Traffic Replay


Traffic replay is a technique used to capture and replay network traffic in a controlled environment, allowing developers to test and validate their applications with real traffic. This approach enables teams to identify and fix issues before they affect users, reducing the risk of deployment and ensuring a smooth user experience. By replaying traffic, developers can verify the performance and behavior of their applications under various conditions, including load testing and security testing. Traffic replay tools, such as GoReplay, provide a cost-effective solution for capturing and replaying traffic, making it an essential feature for software applications. This technique allows developers to capture and replay real traffic, ensuring that their applications can handle real-world conditions and providing a reliable way to test and validate their systems.


## **The Problem: Why Tests Flake**


Flaky tests can often feel random, but in actuality, they typically have some common root causes.


### **Third-Party Instability**


Services are growing more complex by the day, and they often depend on complex API (Application Programming Interface) integrations. While integration testing can ensure that the API itself is integrated properly, this only represents what the integration looks like when the third party is in proper operation with its expected behavior. What happens when the API isn’t up, or isn’t doing what it says it should be doing?


In these cases, your test could be flaking because of a series of outages, issues with rate limits, or even simple service timeouts. Your integration might be stable, but if the system you’re integrating with isn’t, you can introduce significant issues at scale into your testing regime.


### **State Changes and Behavioral Issues**


Often, you may be testing systems that can be altered slightly based on stateful changes on the backend. In these cases, you may run into simple errors that aren’t the result of faulty code or implementations, but are instead non-errors that act like errors due to state changes between test runs. This can result in false negatives or false positives that make your testing report incorrect.


While software applications often lean on state, the issue of state management in APIs is a complex one, especially if you’ve chosen RESTful design as opposed to the previously more common SOAP APIs paradigm. As such, you need more control in order to effectively test configurations.


### **Data Changes - Especially Dynamic Ones**


Often, your testing may be built upon data assumptions that are no longer accurate. Perhaps you built a test to[validate load balancing based on a certain load amount](https://speedscale.com/blog/what-is-load-testing/) , which is now one half, or even a quarter, of the current traffic. In such a state, your testing might say you’ve got everything locked down, but in practice, the sheer scale may overwhelm what was an accurate (at the time) test. Changes in the database can also affect testing outcomes, as data captured from SQL database interactions aids in understanding queries and effectively simulating production loads during testing.


Dynamic data enables users to utilize more complex systems, but they, in turn, introduce significant issues in testing that can disrupt your software systems testing regimen.


### **Timing-Based Issues**


In some cases, the timing of your systems could introduce issues. For instance, if you have a server that has a certain load that delays an internal response metric, you may fail on a call to an otherwise stable stack because of a simple timing issue that may be entirely isolated from the code itself. While this may reflect concerns around load distribution or caching, they may also just be happenstance - for instance, an ephemeral network glitch from your service provider rather than the stack using the infrastructure.


Development teams are not omniscient, and it’s foolish to assume that they can control the timing from end-to-end in a system across so many dependencies and platforms, especially when those third-party systems may have poor developer experience or platform implementations that hamper these efforts.


### **Deterministic Testing on Non-Deterministic Variables**


Another common issue is trying to test services that are deterministic on variables that aren’t. Put another way, you may be testing a service against a variable like LLM calls or a service requiring randomized data - in such a case, you are introducing failures because the data itself is not going to be deterministic, even if what you need out of it will be. This can complicate software testing, but can also complicate everything from validation testing to API monitoring.


### **Traditional Testing Shortfalls**


These aren’t impossible to overcome, but the reality is that traditional testing against these problems is ultimately quite brittle, requiring ongoing maintenance to get a semblance of accuracy, accuracy that will ultimately be misaligned against the realities of your implementation.


Given this reality, how can we create better tests that are not brittle? What[types of API testing](https://speedscale.com/blog/api-testing-tools/) might overcome the relatively restrictive issues with the classic API testing process, and what are the actual results of such a solution?


## **The Solution - Capture Once, Replay Always**


The best way to resolve these issues is to deploy traffic-based mocks.


Traffic-based mocks are the idea of using actual, real-world traffic, capturing that data, and then using that data for your ongoing testing. In theory, this will allow you to bypass the issues we’ve noted above. Companies like Netflix and Uber have created tools for traffic replication and replay to effectively manage production traffic scenarios.


Let’s look at these types of issues and see how traffic capture and replay solves the problem!


### **Third-Party Instability - Mock the Third Party!**


When testing solutions leveraging multiple endpoints or test cases, you are far less concerned about what the third-party APIs are doing outside of the fact that they give your functions the correct response and data to do what is actually being tested. In other words, if you’re connected to a third-party service providing weather data, and you’re testing an overall locale service, you don’t care if the weather is from a week ago - you just want to make sure nothing is failing against the contractual expectations.


Accordingly, capturing real traffic will reveal what past responses have been observed, and will replay those responses. You’ll be testing the integration of these services rather than the data itself, allowing you to test whether the actual responses of your service look like the expected results. This can help you bypass all the wild variables introduced by third-party instability. Using a proxy operates on the network level, while traffic replay solutions capture HTTP traffic at the system level, providing more comprehensive data handling.


### **State Changes and Behavioral Issues Controlled by State Storage!**


[Traffic capture and replay](https://docs.speedscale.com/concepts/replay/) is a snapshot of your traffic as it currently exists. Accordingly, it’s also a snapshot of the current state and behavior of the underlying server. When performing testing, especially functional testing requiring a state to be reflected against production conditions, simulating real-life application traffic during load tests provides a more accurate representation of production environments, making rapid iteration against tests that much easier.


By isolating the state to a single variable, you can fix issues against a known problem, identify performance bottlenecks, or event-specific security flaws that arise from a particular configuration or a set of flawed encryption methods. This traffic snapshot gives you an expected slate to work against that isn’t present in more variable setups.


### **Data Changes - Dynamic Data - Made Static!**


Snapshots of data allow you to create a snapshot of the dynamic data state as well, making it much more static in nature. Picture this - you have a Claude API that you are making repeat calls against. Can you imagine running hundreds of tests against this integration, and with each new call, making a call against the Claude API? Imagine the sheer cost of that process - and also imagine how inaccurate and faulty your findings would be!


Capture and replay allow you to get an idea of *how* the interaction works and looks, and then test against that over time! You don’t need to make constant API calls or test the same prompt repeatedly. You can use a captured traffic snapshot, and even default to local LLM mocks - all without incurring the cost of using the external service! It is crucial to handle raw data carefully during traffic capture to ensure sensitive information is properly anonymized.


### **Timing-Based Issues Frozen in Place!**


Sick of error messages or alert-generating problems that are due to simple timing problems? Wish your network could be perfect so you can just test that fickle endpoint? With traffic capture and replay, you can identify a point in time where the system performed as desired, and then test against that by accurately reproducing network packets.


This helps eliminate a lot of complexity in testing API endpoints and security vulnerabilities by creating a static testing environment. Slow response times are problematic, but if you’re not testing them, they shouldn’t be changing the outcome of your overall test. This allows you to get a perfect scenario for testing - notably, if you’re running into these issues, the very act of creating a perfect condition can help you find where these errors arise by eliminating the noise and complexity burying the lede!


### **Deterministic Testing Becomes Deterministic - Truly!**


Since you are using a snapshot that is unchanging (unless you want it to change), you can validate a wide range of issues that can subtly change from test to test. Captured network traffic can be replayed to recreate scenarios, validate application behavior, and identify regressions. You can group test cases, validate the handling of input data, or validate error codes at scale against the same case. Once you resolve that case, you can create a new snapshot that is a bit different, and then validate against that!


In essence, this gives you far more control than in traditional testing, allowing you to target specific functions such as authorization checks, sensitive data parsing, API layer issues, or even UI layer faults. You have full control over the test, and your test results are specific to what is actually being tested rather than the quirks of the testing tool or the tested environment.


### **Traditional Testing Gets Levelled Up**


In many cases, traditional testing needs to be hyper-focused and specific because of these issues we’ve discussed. They require setting specific heuristics and then engaging in complex manual testing to see how the software or API performs. By using[traffic capture](https://speedscale.com/blog/shift-right-testing/) , you can sidestep the problems inherent with traditional testing without introducing such complexity as to make your testing impossible to scale effectively.


Additionally, the replay phase in traffic replay technology is crucial for determining the capabilities and effectiveness of a testing strategy.


## Benefits of Traffic Replay


The benefits of traffic replay include improved testing accuracy, reduced risk, and increased efficiency. By replaying real traffic, developers can test their applications in real-world conditions, identifying bugs and issues that may not be apparent in simulated environments. Traffic replay also enables teams to test various scenarios, including load testing and security testing, without impacting live traffic. Additionally, traffic replay solutions provide advanced features, such as capture and replay, allowing developers to verify the performance and behavior of their applications under various conditions. With traffic replay, developers can create test cases that mimic real-user interactions, reducing the effort required to identify and fix issues. This approach ensures that applications are thoroughly tested and validated, leading to more reliable and robust software.


## **How Traffic Capture and Replay Works**


Traffic[capture](https://docs.speedscale.com/concepts/capture/) and replay is relatively simple, working across three distinct stages.


### **Stage 1 - Capture**


In this stage, you record the real API interactions from staging, development, or production environments. At this point, you are capturing all of the typically performed actions and their responses, with recording both in the inbound and outbound phases.


Production traffic capture is crucial here as it allows for accurate recording and replaying of network traffic, which is essential for testing and validation purposes.


### **Stage 2 - Analysis**


This stage is where we get into the data itself and start to identify the variables and systems in question. At this point, you can make changes to the recorded traffic, changing everything from the test APIs to the operating systems of the ephemeral services running them. This mutation allows you to change the request and response data for the ultimate in “what if” testing.


### **Stage 3 - Replay**


With our snapshot captured and mutated, we can now use the same data and traffic in integration testing, API load tests, automated testing across security risks or source code concerns, general security testing, and unit testing - the sky is the limit! If you can dream the test and the change to your snapshot to perform it, you can execute replays of that data!


## Using a Traffic Replay Tool


Using a traffic replay tool, such as GoReplay, is a straightforward process that involves capturing traffic, configuring the tool, and replaying the captured traffic. The tool provides a simple and intuitive interface for managing and configuring traffic replay, making it accessible to teams of all sizes and technical expertise levels. To get started, developers can capture traffic using pcap files or other tools, and then configure the traffic replay tool to replay the captured traffic. The tool also provides features, such as speed control and packet manipulation, allowing developers to customize the replay process to suit their specific needs. This flexibility enables developers to tailor the replay to match their testing requirements, ensuring accurate and reliable results.


## Traffic Replay and Continuous Integration


Traffic replay is an essential component of continuous integration (CI), enabling teams to automate the testing process and ensure that applications are thoroughly tested before deployment. By integrating traffic replay into their CI pipeline, teams can automatically capture and replay traffic, verifying the performance and behavior of their applications under various conditions. This approach enables teams to identify and fix issues early in the development cycle, reducing the risk of deployment and ensuring a smooth user experience. Traffic replay tools, such as GoReplay, provide APIs and command-line interfaces, making it easy to integrate them into existing CI pipelines. This integration ensures that testing is consistent and reliable, helping teams maintain high-quality standards throughout the development process.


## Traffic Replay and Test Automation


Traffic replay is also closely related to test automation, enabling teams to automate the testing process and reduce the effort required to identify and fix issues. By using traffic replay tools, such as GoReplay, teams can create automated test cases that mimic real-user interactions, reducing the need for manual testing. The tool provides features, such as recorded data and capture and replay, allowing developers to create complex test scenarios and automate the testing process. Additionally, traffic replay tools provide advanced features, such as load testing and security testing, enabling teams to thoroughly test their applications under various conditions. By automating the testing process with traffic replay, teams can ensure that their applications are thoroughly tested and validated before deployment, reducing the risk of bugs and issues. This approach leads to more efficient and effective testing, ultimately resulting in higher-quality software.


## **Real-World Scenarios - From Chaos to Confidence**


Let’s look at some specific types of tests that can leverage[traffic capture and replay](https://speedscale.com/blog/definitive-guide-to-traffic-replay/) to great effect.


### **Integration Tests That Fail on Tuesdays**


Imagine you have an app that returns data to your end user by referencing a body of APIs. One of these APIs continuously fails on Tuesday, and you’re relatively confident that the issue is due to the fact that the API you’re calling updates at a different hour each Tuesday. The integration is fine, but the API oddness is getting in the way of testing your system.


You can get around this by capturing a known-good response and replaying it to test against that known input. This will allow you to do everything from load testing to GUI testing without having to worry about the tertiary-related weather app with the odd update schedule. For example, you can use traffic replay to simulate the API responses during integration tests, ensuring consistent and reliable testing conditions.


### **Microservice Dependency Woes**


Let’s say that a downstream service your app depends on is under active development. It changes behavior frequently, and the provider doesn’t have very good API documentation or doesn’t push endpoint change notifications for automated API testing. This is breaking your tests, even though you only use this API for a minor function that is known to operate just fine after a brief update is issued.


Speedscale lets you freeze responses from a stable snapshot and[mock that service deterministically](https://speedscale.com/blog/wiremock-alternatives/) . You can look at the last version that worked well with your systems, and start API testing against that good state. This can stop the odd start-stop relationship you have with the third-party API in your API lifecycle, and may even help identify solutions leveraging test scripts or identification heuristics to fix the problem affecting your test in production itself.


### **Non-Deterministic Model Output**


You’re calling an LLM that may return slightly different text with each request. The response is ultimately working as intended, but the change in response size - and thus the payload size as well as the transit data - is creating false detections across penetration testing systems and cross-site scripting controls. Your quality assurance is taking the non-deterministic output and running complex logic, even though you know the response is right, just overly wrong or complex in certain cases.


Replaying captured interactions ensures you can validate the UI or downstream logic without worrying about response drift. The complexity of modern software applications, which encompass numerous microservices and connections, acts as an extension of the difficulties faced in manually scripting test cases. You can capture a known response and test against it. Better yet, you can capture multiple snapshots to determine the average response load or type, allowing you to engage in API design that is aligned within a certain range of regular functions that can reach production systems. This is especially useful for solutions such as coding agents, allowing you to engage in relatively complex testing without having to undergo complex testing regimens.


## **Conclusion: Make Tests a Feature, Not a Gamble**


Flaky tests aren’t just annoying - they’re a sign of unreliable tests or systems that are making your tests less effective. There are a lot of reasons to consider API testing important, but making inaccurate tests is, by many measures, more useless than having no tests at all.


[Speedscale](https://speedscale.com/) helps eliminate test instability by replacing external dependencies and dynamic responses with stable, replayable mocks based on real traffic. It’s time to take the guesswork out of testing and bring observability-driven reliability into every part of your CI/CD process.


Capture once. Replay forever. Deploy with confidence. You can[get started with Speedscale](https://app.speedscale.com/signup) today in mere minutes!
