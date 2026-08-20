---
schema_version: "1.0.0"
document_id: "c1f508597b98d72e76ddbf5066d8feed4b33b80fdc5909dacc4eeeeb648b9b6e"
company_key: "yc-emi-labs"
company: "Emi Labs"
source_id: "yc-emi-labs-rss-87232385bc09"
canonical_url: "https://medium.com/@EmiLabsTech/all-things-microservices-part-i-74973b23cdcf"
published_at: "2022-02-09T15:07:52+00:00"
first_seen_at: "2026-07-27T09:02:34.241036+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:1ebb9cdfc09cfcd3dca02854cb6c0fa799723c321319cb8b354d298f9205c60b"
---

# All things microservices (Part I)

# All things microservices (Part I)


[Emi Labs Tech - Ravens](https://medium.com/@EmiLabsTech?source=post_page---byline--74973b23cdcf---------------------------------------)


5 min read


·


Feb 9, 2022


--


by[Marina Huberman](https://www.linkedin.com/in/marinahuberman/) &[Federico Sánchez](https://www.linkedin.com/in/fede-sanchez/)


In this post we’ll share how we handle architecture decisions and design our microservices in Emi, hoping that those experiences can be useful or serve as an inspiration for others. Let’s dive in!


## Why Microservices?


Services should be built and maintained by small teams, designed around business capabilities (not horizontal layers such as data access or messaging) and autonomous (deployed as an isolated service and change independently of each other). Some of their benefits are:


- **Organizational alignment** : Smaller teams are more productive and have better ownership
- **Ease of deployment** : Isolate businesses, release faster
- **Resilience** : Allows isolation of a problem so the rest of the system can carry on working
- **Scaling** : Different scaling policies and types of hardware can be set for each service
- **Composability** : Functionality can be consumed in different ways
- **Optimizing for replaceability** : Ease of replacing/refactoring services
- **Technology heterogeneity:** Each service can use different programming languages, databases, tools, etc.


But nothing is free in this world and microservices aren’t the exception. This architectural pattern comes with the same challenges as in any distributed solution or architectural and organizational decision.


## Why Node.js?


Node.js is a JavaScript runtime environment and JavaScript is one of the most used languages (mainly because it runs in every browser). In the early stages of Emi, we decided to build most of our services using Node.js because:


- it’s easy to learn and it has a huge community
- it helps with asynchronous programming (better performance and great for event-based architectures)
- its dynamic language offers more velocity when coding, less ceremony, and ease of testing
- in the need to mix dynamic code with strongly typed code, something like TypeScript could be used


## Challenges


Resilient interservice communications, accelerate services creation, solve common problems like configurations, error handling, message consumer, and last but not least, distributed tracing, are some of the inherent challenges to microservices architecture.


At Emi, we have built and actively maintain npm packages to help solve these challenges for those working with microservices in Node.js.


## But wait, shouldn’t teams and services be autonomous?


Gotcha. Yes, by following a set of guidelines, each team can decide which language and libraries to use while they comply with some architectural rules, so every service is resilient, observable, and scalable.


In the end, architectural rules and best practices are the foundation for every service within a microservices ecosystem. These packages ease our ride to comply with these rules, allowing us to move quickly and guided by a value-driven mindset.


## Resilient interservice communications: Timeout and Retry


With so many services talking to each other it is key that they can tolerate failures from other components they depend upon. The two most common patterns to achieve this are timeout and retry.


In queue messaging communication, these are easy to implement because the infrastructure would probably handle it out-of-the-box. But what happens with our HTTP communication? Well, timeout is probably handled by the HTTP client library, but it’s important that everyone is using it right. If the timeout value is always 5 minutes (or disabled, as[Axios](https://github.com/axios/axios) does), you will probably wait for dead services too long and your consumers will timeout before you finish.


On the other side, retries are not usually handled by most HTTP clients. We have built an HTTP module to be used by every Node.js-based service. This one uses Axios, for which we have overridden some default values:


We also used[axios-retry](https://github.com/softonic/axios-retry) which intercepts failed requests and performs retries whenever possible using exponential backoff:


## Distributed Tracing


Distributed tracing is crucial for understanding complex microservices applications. Without it, teams can be blind into their production environment when there is a performance issue or other errors.


Let’s picture it: Tasks often span multiple services. Each service handles a request by performing one or more operations, e.g. HTTP requests, database queries, message queueing. That said, to ease understanding and debugging of an application behavior, one should be able to trace all the requests and operations that belong to the same task.


We pack our services with an express middleware that assigns each task a unique trace id (UUID). This id is meant to be added in the first controlled service that handled an external request (e.g., Backend for Frontend, Public API) or where the request was internally initiated (e.g., cron task). This middleware also adds the initiator’s name (service and component) and the logged-in user id as context. Then, the trace and referrer data is passed through to all services that are involved in handling the operation, allowing to include the trace data in all log messages, metrics, and API responses.


Reading a trace ID or generating a new one is easy. But, how to pass this ID to every function in the chain of calls that our process will do? There is no doubt that adding this ID as an argument on every function is not an option. Some “instrumentation code” needs to be added to our business code in order to track these traces transparently. To solve this special problem, we chose to go with the simplest approach and use[Continuation-Local Storage (Hooked)](https://github.com/jeff-lewis/cls-hooked) , a module that uses async hooks and allows to set and get values that are scoped to the lifetime of these chains of function calls 😉.


Here you can see a snippet to get an idea of how this module works:


Then, in your logging formatter you can read this trace ID to later include it in logs:


We maintain modules for logging, messaging, an HTTP client, and middlewares for Web APIs and Workers, and all of them are prepared to send and/or read these trace ids.


## TBC


Did it seem like a lot, huh? It’s okay to feel a little overwhelmed if this is your first time with microservices. It’s a hard topic, and as with every hard topic, it takes time and practice to master it.


Next week we’ll cover some other topics like configurations, microservice templates, and linters. There’s still a lot to learn, stay tuned!


And if you read up to here, it might be a good idea to have a look at our[current job openings](https://jobs.lever.co/emilabs) . We are looking for passionate and curious people to join in!
