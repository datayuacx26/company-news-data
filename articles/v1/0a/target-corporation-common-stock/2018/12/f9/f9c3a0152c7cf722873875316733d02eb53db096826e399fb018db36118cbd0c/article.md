---
schema_version: "1.0.0"
document_id: "f9c3a0152c7cf722873875316733d02eb53db096826e399fb018db36118cbd0c"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/spring-boot-service-to-service-communication"
published_at: "2018-12-18T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:938d326fd0be10f9eb71779c5c334adf6adf4f170d45e4e3aa0416ea5538c087"
---

# Spring Boot Service-to-Service Communication

Scalable, nimble and efficient are terms commonly used to describe microservices, and as such, services are built to meet specific needs based on user features or application requests. However, when services need to communicate among one another, this can become somewhat convoluted and can lead to a significant amount of technical debt if not managed effectively. Target was faced with such a scenario in which it owned 40+ Spring Boot services and service-to-service communication was necessary to ensure service handoffs and SLAs were met. This post will walk through our implementation of Spring Feign Client, our learnings, and how Spring Feign Client has helped manage our inner-service communication while reducing the amount of development time.


Before introducing


[Feign](https://cloud.spring.io/spring-cloud-netflix/multi/multi_spring-cloud-feign.html) , let me shed some light into the tech stack we're working with. All services are Java Spring Boot services,


[Eureka](https://cloud.spring.io/spring-cloud-netflix/multi/multi__service_discovery_eureka_clients.html) and


[Zuul](https://cloud.spring.io/spring-cloud-netflix/multi/multi__router_and_filter_zuul.html) for service discovery and routing, and centralized configuration using Consul. All services are built via Docker and deployed on a number of different infrastructure services that provide service scalability based on workload and various other parameters.


Now, for the problem statement: We're faced with 40+ Spring Boot services and, at any time, service-to-service communication is/could be required to meet a need. As a result, we needed to implement service communication in a reusable way that provided minimal technical debt, required minimal maintenance and enabled our developers to build quickly. When we started, we reviewed a couple methodologies:


Spring RestTemplate: a template for making HTTP calls methods with the support of all HTTP protocol’s methods:


HEAD


,


GET


,


POST


,


PUT


,


DELETE


, and


OPTIONS


. Spring Feign Client: a simple HTTP API client that enables developers to interact with services through interfaces and annotations.


Using the below REST endpoint example, we'll break out some of the issues you'd face if you elect not to use the Spring Feign Client approach. Our rest endpoint is coded as follows and runs under route


https://www.localhost.com/items-service/items


:


```text
@RequestMapping(value = "/items", method = RequestMethod.GET)                    public Items getItems(                   @RequestHeader("sm_user") String userName                   ){                       Items items = database.getItems();                       log.info("user:{} items:{}", userName, items.size())                       return database.getItems()                   }
```


Spring RestTemplate


Spring's RestTemplate, especially with centralized and annotation driven configuration, can facilitate service-to-service communication; however, it introduces significant amount of maintenance and potential for technical debt. To give some context, here is an example of the Sprint RestTemplate with annotation driven HTTP routing to call the items-service and retrieve all items.


```text
@Value("${service.uri}")                   String uri;                              private Items getItems(){                       RestTemplate restTemplate = new RestTemplate();                       return restTemplate.getForObject(uri, Items.class);                   }
```


This example would import the URI parameter from configuration, make the


GET


call to the imported URI, and expect an Items response. So assuming you're using our tech stack, you'd have centralized configuration and would have to import the


service.uri


route from configuration, which would mean you'd have to maintain all service routes and documentation around service routes in the event of a route change. In addition to the configuration, you'd have to import the Items Java object to cast the response to the appropriate contract or else additional code is required to extract values for additional needs.


To break that down further, as you scale up services and build models to meet feature needs, you'd be faced with either significant technical debt or a maintenance nightmare. Continuing with the


getItems( )


example, imagine a handful of services having to make the same call. You're likely going to find yourself in one of the three scenarios:


Build, publish and import an Items Java object artifact into all services where you will need to maintain versioning and implementation to make


REST


call.


- Copy/paste implementation methods across services.


- Write different implementations without any consistency as new services are introduced.


Each of these paths includes a significant amount of maintenance, introduces technical debt and can cause nightmares when debugging service delays. Not only that, but because there isn't a centralized monitoring format, this can lead to confusion and be difficult to identify bottlenecks when SLAs aren't met.


Reviewing the


getItems( )


RestTemplate, we didn't implement any monitoring. The service that is making this call would have no logging to assist in debugging any service-to-service communication. That gets us to Feign and how we've been able to manage all service-to-service communication with Spring Feign Client and avoided these nightmares.


Spring Feign Client


As mentioned, Spring Feign Client is a simple HTTP API client that enables developers to interact with services through interfaces and annotations. Rewriting our getItems method using Feign would look something like:


```text
@FeignClient(name = "items-service", url = "${service.baseUri}")                   public interface ItemServiceClient {                                  @RequestMapping(value = "/items-service/items", method = RequestMethod.GET)                       Item getItems();                   }
```


Now that the


getItems( )


service call is written using Feign, implementing the Feign Client into your service requires a base URI (


https://www.localhost.com


) to be imported through configuration and the following code:


```text
={`@Value("${service.baseUri}")                   String uri;                              @Autowired                   FeignClient feignClient;                              public Items getItem(){                   return feignClient.getItem();                   }
```


While there is a little more code footprint, following the Feign approach provides a lot of flexibility and can shorten development time into new services or enhancements to existing services. To break that down, our team has found the following benefits to using Feign:


- Domain model updates are isolated to core services and Feign client so versioning maintenance time is reduced.


- Inner-service communication is templated, monitored and trusted, making it easy to debug and maintain.


- Routing changes are isolated to few locations, reducing maintenance time.


- Configuration is limited to the base URL, and service routing is documented in code through Feign.


- Logging can be overridden at the Feign Client level to document pre- and post-HTTP actions (not documented in this post).


In order to make this successful, our team has built a reusable Feign client artifact that is shared among all services that require service-to-service communication. In doing so, as new features are introduced and dependencies on other services are needed, we can trust that the Feign client will not only allow for service communication, but also consistent service response contracts are made throughout all services.


Final Notes


The Feign Client is something that an application built using service-based architecture should look at using. It provides developers the confidence that service communication is consistent across all applications, provides centralized service-to-service communication monitoring, and shortens maintenance/development time on existing or new services.


Additional Spring Boot Feign Resources


Below are references to Spring Clouds Feign Documentation and code samples:


- [Spring Cloud Feign Client Documentation](https://cloud.spring.io/spring-cloud-netflix/multi/multi_spring-cloud-feign.html)
- [Spring Cloud Feign Client Examples](https://github.com/spring-cloud-samples/feign-eureka)
