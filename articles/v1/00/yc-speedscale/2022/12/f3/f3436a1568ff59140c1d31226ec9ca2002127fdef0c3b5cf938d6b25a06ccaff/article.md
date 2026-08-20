---
schema_version: "1.0.0"
document_id: "f3436a1568ff59140c1d31226ec9ca2002127fdef0c3b5cf938d6b25a06ccaff"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/combining-load-tests-mocks/"
published_at: "2022-12-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:8456e5e9a570632ab00d0b6858914b25e36a3c0657e8730dc389874482f6e827"
---

# How Load Testing and Mocks Work Together

If you’ve worked with load testing before, you know that there are a lot of[things to consider](https://speedscale.com/considerations-to-make-when-running-a-load-test/) . Whether or not you should combine[load testing](https://speedscale.com/blog/what-is-load-testing/) and mocks is one of those considerations.


Getting to the answer requires knowledge of your infrastructure and your development procedures, which will all influence what questions you need to ask yourself. However, one question that almost anyone needs to ask is, “Do I want to test the single service, or my entire infrastructure?”


If that question doesn’t make sense to you yet, go ahead and read on. In this post you’ll get an idea of how load testing and mocks work together, as well as the benefits you get from combining the two.


## Load Testing without Mocks


Over the years there have been many tools available to create load tests. These days, one of the most commonly used local tools is[K6](https://speedscale.com/kubernetes-load-testing-comparison-speedscale-vs-k6/) . With K6, creating a load test can be as easy as creating a` load-test.js` file with the following contents:


```text
import   http   from   'k6/http'  ;
import   { sleep }   from   'k6'  ;
export   default   function   () {
http.  get  (  'https://test.k6.io'  );
sleep  (  1  );
}
```


Unless you’re completely new to JavaScript or programming in general, it doesn’t get much easier. There are even[tools to create K6 tests from recorded traffic](https://speedscale.com/blog/speedscale-k6-tutorial/) However, this simple approach does present one challenge. What if` https://test.k6.io` has any dependencies?


There’s a high likelihood it will. Some common dependencies are:


- A database,
- Third-party APIs like payment gateways or data providers,
- Storage services like S3.


From anecdotal experience, almost all services will have at least one of those dependencies, and most will have even more.


This is great in production, as it allows for separation of concerns, and there are few arguments against it. However, it can create unwanted results when you start to implement load tests.


Assuming that` https://test.k6.io` has a single dependency: a database. If you use the example above to send 1.000 requests, then you’ll inevitably also be sending 1.000 requests to the database.


### Shouldn’t all of the Infrastructure Withstand High Load?


It’s easy to assume that this isn’t a problem in reality, as you don’t want to test whether a frontend application can handle 1,000 requests/s, only to discover that the database can’t handle it. While that is true, it only covers one of the use cases of load testing.


Imagine that you’re running a webshop. You know that the entire infrastructure as a whole is capable of handling 1,000 requests/s with no issues. One day, you have to modify how the backend API handles data validation on incoming requests.


Once you’ve finished coding the new feature, you need to verify that it can still handle the same amount of load, and this is where mocks come into the picture.


Having to spin up replicas for all the dependencies of the backend API is not only complex — it can also quickly become expensive. As you know the rest of the infrastructure still works, you can focus on testing the API itself, and mock any dependencies.


On top of keeping cost and complexity low, it’ll also make your load tests **much faster** . This is just one example of the[advantages of using API mocks](https://speedscale.com/blog/mock-services-top-use-cases/) . Other advantages are:


- They prevent rate-limiting on third-party APIs;
- They simulate responses;
- They can[transform requests](https://docs.speedscale.com/concepts/transforms/) .


On top of that, if your[mocks are built from recorded traffic](https://speedscale.com/how-to-mock-apis-in-kubernetes/) you can simulate a wide variety of responses.


## Implementing Mocks


With a firm understanding of why you might want to combine load testing and mocks, you also need to understand how it can be done. There are a few different ways it can be accomplished.


### Using Docker Containers as Mocks


If you are running in Docker-based environments like Kubernetes, it should be fairly easy to spin up a new Docker container that can act as a mock. It just has to be configured to provide the correct responses to any given request.


The advantage of this approach is that it’s “free” — that is, free in the sense that you only have to pay for the computing resources. You don’t have to pay for a third-party tool.


The downside to this approach is that you now have a lot more to manage. Not only are there now additional containers to keep track of — you also have to make sure that the mocks are always kept up to date, meaning there’s more code to maintain.


### Automate it with Speedscale


Rather than adding more complexity to your infrastructure, you can start using mocks in your load test almost immediately if you set up Speedscale.


You can read the[complete guide on load testing with Speedscale](https://speedscale.com/kubernetes-load-test-tutorial/) if you’re curious about specifics, but in short, Speedscale installs into your Kubernetes cluster as an[Operator](https://docs.speedscale.com/setup/install/kubernetes-operator/) , and captures all traffic to and from the services you instrument with the Speedscale sidecar.


The Speedscale sidecar is what helps you not only with mocks but also with load testing. The sidecar acts as a proxy, and when you need to mock any dependency of a service, the proxy will take care of it.


By launching a load test with Speedscale, you can instruct it to use mocks. In that case, the proxy will catch any outgoing request from the service-under-test (SUT), and respond according to previously captured traffic, rather than letting the request go out to other services.


This approach is streamlined enough that you might not even realize that mocks are being used.[Play around with the Speedscale UI](https://play.speedscale.com/) to get a feel for how seamless the experience is.


Not only will this approach help you reduce the complexity of running load tests in general, but it also highly increases the likelihood of successfully implementing load tests in your CI/CD pipelines.


## Focus Your Load Tests with Mocks


Whether or not you want to combine load testing and mocks is a choice you’ll have to make, depending on what your infrastructure and your needs are. But now you at least have a good amount of base knowledge to help you make an informed decision.


If you’re still wondering about how your load tests can be optimized, take a look at how[Speedscale is twice as fast as K6](https://speedscale.com/optimize-kubernetes-load-tests/) .
