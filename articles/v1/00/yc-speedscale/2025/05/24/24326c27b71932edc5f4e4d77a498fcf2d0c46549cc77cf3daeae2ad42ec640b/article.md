---
schema_version: "1.0.0"
document_id: "24326c27b71932edc5f4e4d77a498fcf2d0c46549cc77cf3daeae2ad42ec640b"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/take-control-of-your-ai-routing-mocking-claude-gemini-and-gpt-4/"
published_at: "2025-05-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:96e188e3983129e93ba2f676244b574c055e05de65a5b0d4732f99772130967c"
---

# Take Control of Your AI Routing: Mocking Claude, Gemini, and GPT-4

A few short years ago, the idea of using a Large Language Model was relegated to some specific models and implementations for a given industry or use case. In 2025, this has shifted dramatically - not only are LLM models more common than ever in production environments, but multi-model systems - those systems that use more than one model to perform work - have become more popular, often involving complex network routing to optimize performance.


Real-world use cases increasingly utilize complex routing logic that dynamically switches between Claude, Gemini, GPT-4, and other foundational models based on performance, availability, suitability, and cost. This requires a new (and better) understanding of routing strategies, load balancing, troubleshooting, and cost analysis across multiple AI models and multimodal routing strategies. Routers manage data packets by determining the best paths for them to travel through various network routes, ensuring efficient communication and processing.


In this piece, we’ll outline what makes mocking in a multi-model routing system so complex and walk through best practices for creating functional and reliable mocks for each major provider. Whether you’re using models in parallel, selecting dynamically per query, or just trying to keep dev/test environments stable, this guide is designed to help you build confidence before hitting production.


## Introduction to Routing and Mocking


Routing and mocking are two essential concepts in computer networking and software development that play a crucial role in ensuring efficient and reliable systems. Routing refers to the process of selecting a path for traffic within a network or between multiple networks. This process is vital for directing data packets from their source to their intended destination, ensuring that network traffic flows smoothly and efficiently.


Mocking, on the other hand, is a technique used in software development to simulate the behavior of a system or component. In the context of routing, mocking can be used to test and validate routing protocols and algorithms. Routing protocols, such as Open Shortest Path First (OSPF) and Routing Information Protocol (RIP), are designed to determine the best path for data packets to travel from their source to their destination. By using mocking tools and techniques, network administrators can test and troubleshoot these routing protocols, ensuring that data packets are delivered efficiently and reliably.


In addition to routing, mocking is also widely used in API development. Developers can create mock APIs that mimic the behavior of real APIs, allowing them to test and validate their code without relying on live systems. This approach helps identify and isolate issues early in the development process, improving the performance and reliability of networks and systems.


Overall, understanding the relationship between routing and mocking is essential for professionals in computer networking and software development. By leveraging these concepts, they can design and implement more efficient and reliable systems, ensuring that network traffic is managed effectively and that software components interact seamlessly.


## Routing Fundamentals


Routing is a critical function in computer networking that enables data packets to be delivered from their source to their destination. This process involves the use of routing protocols, such as OSPF and RIP, to determine the best path for data packets to travel. Routing protocols use various metrics, such as hop count and delay, to evaluate and select the optimal path.


Routing tables play a central role in this process. These tables store information about the network topology and the best paths to various destinations. Routers use routing tables to forward data packets to their next hop on the path to their final destination. This ensures that packets are routed efficiently through the network.


Dynamic routing protocols, such as OSPF and Enhanced Interior Gateway Routing Protocol (EIGRP), are designed to adapt to changes in the network topology. They automatically update routing tables to reflect the current state of the network, ensuring that data packets always take the most efficient path. In contrast, static routing protocols use pre-configured routing tables that do not change, making them less flexible but simpler to manage.


Link-state routing protocols, such as OSPF, use a link-state database to calculate the best path. These protocols gather information about the state of each link in the network and use this data to build a complete map of the network topology. Distance-vector routing protocols, such as RIP, use a distance-vector database to calculate the best path based on the distance to each destination.


Understanding routing fundamentals is essential for designing and implementing efficient and reliable networks. By mastering these concepts, network administrators can ensure that data packets are delivered quickly and accurately, optimizing network performance and reliability.


## What Makes Multi-Model Mocking Different


Multi-model routing, the process of linking multiple AI models together in a seamless and efficient loop, isn’t just a frontend concern that can be worked around with prompt enhancement - the very nature of this problem reflects a deeper architectural complexity in how routing work is managed within these systems.


Multi-model systems reflect the reality that no two models are alike - differences in data types, the machine learning models used in training, whether or not they’re open source or closed, differential parameters, and more can give each model significantly different request/response format quirks, different solution or scenario provisions, and wildly different service connects.


Similarly, each model can have its own latency and token policy approach, which can vary widely from provider to provider, even within the same modality. How a service integrates can change how you interact with it, and these differences can introduce high complexity into standard mocks. Evaluating models often involves different metrics to assess their performance and suitability for specific tasks.


More complex issues, ranging from failover, quota limits, or even region-specific routing, can change behavior over time and at scale, even in mid-session implementation. When you tie in multiple ML models and systems into a single workflow, this complexity faces rapid growth in a relatively short time, with even simple inference or prompt requests integrating with multimodal transportation and transformation that can feel dizzying.


Ultimately, this means that mocking can no longer just represent the model underpinning the system - it must reflect the system itself and the system that selects and talks to the model. If you only mock stubs and static responses from a single model, you’re only mocking half the picture - and in many cases, this essential gap in coverage can introduce fatal failure into your testing process.


## Solutions for Multi-Model Dynamic Routing


How, then, as a provider in this multi-model reality, should you mock this system? Let’s look at some best practices for mocking these multi-model roundtable systems, including effective path selection to ensure optimal routing.


Routing decisions in these systems often involve multiple nodes, which are the fundamental components of the network. These nodes facilitate communication and routing processes, directing data efficiently through the network.


### **Use Schema-Aware Mocking per Model**


One big step in the right direction is to ensure that each model is mocked using schema-aware systems, considering the importance of users in the process. Models behave according to their end result - an image-based model generating understanding and context is going to ingest and send prompts differently than a textual system transforming raw data into insight. Accordingly, treat these systems as if they are different experts, and mock them accordingly.


The following are three prevalent models in the current model economy - let’s look at how they might be mocked differently.


- **OpenAI GPT-4** - this model uses message arrays for chat, and has strict token limits. It also supports pretty advanced streaming with SSE. Accordingly, mocking this system will require more stringent controls over token-based communications and transformations, and these communications and requests must take the form of a message array.
- **Anthropic Claude -** unlike GPT-4, Claude takes a prompt string with some few-shot examples embedded, and returns structured completion with safety applied on the backend. Accordingly, this model must be mocked with a safety transformation embedded to mirror the potential latency or review introduced into the data flow.
- **Google Gemini -** this model allows for tool and function calling in the payload itself, meaning that mocking should reflect the potential of additional calls and transformations in the prompt itself. Gemini also supports images in the prompt natively, and in mocking, this requires some local image storage or transformation to ensure the prompt is handled realistically.


In all of these cases, there are some specific aspects of each model that require more intelligent and contextual mocking. When mocking the responses themselves, you must also ensure that you use the actual structure and error types returned by each model. Additionally, the links between nodes in a network play a crucial role in facilitating communication through various paths, which should be considered in the mocking strategy. A good mocking strategy should:


- Validate payloads against the real schema;
- Include latency and quota headers to simulate rate-limiting behavior; and
- Simulate streamed vs. non-streamed responses as appropriate.


Tools like Proxymock from Speedscale can make short work of a lot of this process, allowing you to use actual traffic to and from each model to generate replayable mocks that reflect live behavior, representing headers, status codes, timing, and core functions.


### **Abstract the Router**


In most routing systems, an internal router within an autonomous system - whether native or vendor-managed - is used to determine which model to hit. This determination is often based on a variety of systems and metrics, including:


- Cost thresholds and token allowance;
- Availability or outage info of the service;
- Confidence or prior history in the generated output quality; and
- User-specific SLAs or latency targets.


Routing algorithms play a crucial role in making these routing decisions by assigning cost metrics to links, allowing routers to select the most efficient paths for data transmission. Due to the nature of mocking, this information can often be forgotten in building your mock. To get around this, you should isolate this logic and mock it separately. Instead of hardcoding “send to GPT-4,” write tests against your router’s decision tree. For instance, you can set logic like the following statements:


- Given prompt characteristics X, assert the router selects model Y;
- Mock upstream results and test fallback behavior depending on the data returned; or
- Use fuzzing to simulate edge cases (for instance, empty responses or 429 throttles).


This requires heightened control over the testing environment. Solutions such as Speedscale allow you to control these variables effectively. No matter what you do, ensure that these controls are applied realistically - do not over-implement or over-control your environment, and where possible, stick to real-world observed limitations and traffic patterns.


## Replay Real Multi-Model Flows


Since LLM interactions are often dynamic, you can’t thoroughly test behavior with static mocks alone. Replaying traffic, especially when one model fails or underperforms in the context of multiple models or routing systems, gives better insight into the behavior of your router as well as the downstream consumers depending on your system. Routers play a crucial role in how they forward packets across networks, ensuring data reaches its destination efficiently.


Speedscale is again an excellent tool for this process. You can use Speedscale (or a similar capture-and-replay tool) to:


- Capture live traffic through your router (including the upstream and downstream data) to create a realistic and lifelike testing data stream;
- Re-run the same prompt through different models and compare outputs to isolate potential faults, identify more accurate or effective models, and optimize your overall model flow; and
- Validate downstream effects - for instance, overall tool use or the process of ingesting data into your logging framework - to ensure accurate and effective use against the contract. Evaluating the capacity of your router is essential in this process to manage data traffic and prevent congestion.


Replay also helps you spot brittle assumptions. For example, if you assume that all models return an array of messages when only most of your models do that, then you are introducing potential fail states or complexities that might not be observable or surfaceable at first blush. Replay helps you find these little issues hidden away in the implementation.


## Simulate Multi-Model Streaming


Models like GPT-4 and Claude support streaming responses via SSE, which is now standard in most, but not all, agentic pipelines. These streaming responses facilitate the establishment and management of connections between different networks, ensuring efficient data routing and communication. If your mocks always return complete responses in one chunk, you’re missing the chance to test intermediate state updates, validate incremental rendering, and ensure cancellation works correctly mid-stream.


What is key here is to ensure that you are representing the actual and entire flow of data and models in the system. The internet plays a crucial role in routing data packets, enabling seamless communication across various platforms. You can mock streamed responses by sending partial data with chunks on time delays, passing intermediate tokens, etc. You can mock non-streamed responses by providing chunked data returns. Importantly, you can blend these modes depending on your observed data state and model interactions using traffic capture, ensuring that you are creating mocks that reflect production realities.


## Centralize Response Normalization


Many multi-model systems end up writing translation logic - that is, the map each model’s response into a standard intermediate format for downstream consumers, considering the total cost of maintaining such a system. This translation layer can introduce significant complexity, but is also an area of the model that is often missed during the mocking stage.


To get this right, you need to mock the model, but you must also mock the *translator* of that model. Your test cases should:


- Validate that structured responses (e.g., tool calls) are normalized correctly and that the transformed output still reflects the original intent;
- Handle mismatches or missing fields gracefully to ensure that prompts and multi-model routing don’t fail due to a dropped field entry or value; and
- Account for post-processing steps like safety filters or prompt injection protection, which are becoming increasingly more common in models of all stripes and purposes.


This is especially important if you’re testing agents or downstream pipelines that assume a uniform schema. Models are typically quite opinionated - in large part because they assume they’re the primary model in a working flow. Accordingly, you need to reflect these opinions, practices, and transformations effectively in your mock in order to make the mock represent the actual flow and utilization of data within the service being mocked. Additionally, routing decisions often depend on assigning a cost number to each link, which helps determine the most efficient path for data packet forwarding.


## Best Practices and Troubleshooting


When implementing routing and mocking, it’s essential to follow best practices to ensure efficient and reliable systems. One key best practice is to use dynamic routing protocols, such as OSPF and EIGRP, which can adapt to changes in the network topology. These protocols help maintain optimal routing paths, even as the network evolves.


Another best practice is to use mocking tools and techniques to simulate the behavior of systems and components. This approach allows developers to test and validate their code in a controlled environment, identifying potential issues before they impact production systems.


When troubleshooting routing issues, it’s essential to check the routing tables and protocols to ensure they are configured correctly. Common issues include incorrect routing table configurations and protocol misconfigurations. Tools like ping and traceroute can help network administrators identify the source of the problem by tracing the path of data packets through the network.


For mocking issues, it’s crucial to check the configuration of the mocking tools and techniques being used. Common problems include incorrect tool configurations and misimplementations of mocking techniques. Developers can use tools like debuggers and loggers to identify and resolve these issues, ensuring that the mocks accurately simulate the behavior of the real systems.


Overall, following best practices and promptly troubleshooting issues can help ensure efficient and reliable systems. By leveraging dynamic routing protocols and effective mocking techniques, network administrators and developers can optimize network performance and reliability, ensuring that data packets are delivered accurately and that software components interact seamlessly.


## Conclusion


Mocking in multi-model systems is less about stubbing one response and more about simulating an ecosystem. A network administrator plays a crucial role in managing these multi-model systems, ensuring that the unique capabilities and quirks of GPT-4, Claude, and Gemini are effectively integrated into the testing strategy while supporting the architecture’s routing and fallback logic.


Speedscale’s capture and replay tooling is particularly well-suited to this environment, enabling real-world simulation of all three models with minimal effort. If your system depends on consistent behavior across models, you need mocks that can evolve as the models do. Addressing routing issues is essential, as DNS translates domain names into machine-readable IP addresses, and routers utilize these addresses in routing tables to direct packets to their proper destinations within a network. You can get started with Speedscale in[mere minutes with a free trial](https://speedscale.com/) , and there’s[ample documentation](https://docs.speedscale.com/) that you can apply in order to make your mocks more effective, representative, and valuable!
