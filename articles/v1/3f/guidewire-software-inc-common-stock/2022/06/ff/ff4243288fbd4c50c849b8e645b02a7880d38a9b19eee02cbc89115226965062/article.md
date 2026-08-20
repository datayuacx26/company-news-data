---
schema_version: "1.0.0"
document_id: "ff4243288fbd4c50c849b8e645b02a7880d38a9b19eee02cbc89115226965062"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/how-to-write-unit-tests-for-outbound-integrations-stubstitution"
published_at: "2022-06-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:45747a2430522f8b1ae9d1750355fdbfcb1ba2d3ad064daceaab78dac31d9dfb"
---

# How to Write Unit Tests for Outbound Integrations: “Stubstitution”

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- How to Write Unit Tests for Outbound Integrations: “Stubstitution”


In my ten years working on Guidewire implementation projects, I’ve heard many excuses for not writing unit tests. “It takes too long.” “It slows the build.” “It’s not my job.” “It can’t be done.”


That it can’t be done is an excuse I’ve often heard when developers discuss unit tests for outbound integrations. Because an outbound integration connects to a third-party system, their argument goes, unit tests that call the integration will fail if that system is down or otherwise acting up. Such failures are spurious because the integration code is not at fault. And we can’t have spurious failures. The conclusion? “We can’t have unit tests for outbound integrations.”


The problem with this argument is that it relies on a hidden assumption: the code that connects to the third-party system can’t be separated from the code that makes up the rest of the integration. But this assumption holds only if we design our integrations poorly.


## Why Design Matters


A well-designed integration isolates the code that can’t be unit-tested from the code that can, and it packages the former into dependencies that are injected into the latter. For an outbound integration, a dependency might be code that uses HTTP calls to send and receive data between the integration and the third-party system. The code making the calls can’t be unit-tested because it might generate spurious failures. But if the calls are isolated as a dependency and injected into the integration, the rest of the integration code can be unit-tested. To do this, however, requires us to add a new tool to our testing toolbox: test doubles.


There are different types of test doubles, each designed for a particular testing job. In this post I’ll talk about just one: the test stub.


## What Is a Test Stub?


As Lasse Koskela explains in his book Effective Unit Testing (page 34), a test stub is an unusually short thing. More concretely, the purpose of a stub in a unit test is “to stand in for the real implementation with the simplest possible implementation.” Some example tests will help us clarify how a stub works.


Let’s return to the InsuranceSuite address lookup integration that I introduced in my previous post about unit-testing maintenance. We’ll write a test for a new technical behavior: the integration should throw a particular exception if the request object that contains the details of the address to look up is missing the first address line. Here’s a test for that behavior:


function testThatMissingAddressLine1InRequestThrowsIllegalArgumentException() {


// GIVEN
var stubRequest = new AddressLookupRequest() {
override property get AddressLine1() : String {
return ""
}
override property get PostalCode() : String {
return "02840"
}
}


// WHEN
var t : Throwable
try {
new AddressLookup().doLookup(stubRequest)
} catch (e : Exception) {
t = e
}


// THEN
assertTrue("Exception thrown is not an IllegalArgumentException",
t typeis IllegalArgumentException)
}


## Stubs Are Strikingly Simple


The stub, which makes up the GIVEN section of the test, is striking in its simplicity — an almost absurd implementation of the AddressLookupRequest interface. The real implementation of this interface will likely define a constructor that takes an Address entity as parameter and stores some columns in private variables, but Address entities have no bearing on this test. All we require for this test is that the first line of the address be empty and that the postal code be an arbitrary but realistic value. So we hard-code our implementation to be just that.


While eminently simple, the stub is sufficient to replace the real implementation for the behavior we want to test. We see this “stubstitution” in action in the WHEN section of the test, where the stub is passed as a parameter to the main lookup() function of the integration.


Our example test shows how to use stubs to stand in for the real implementation of an upstream dependency such as an Address entity. But stubs can also stand in for the real implementation of a downstream dependency, such as HTTP calls to a third-party service. Let’s write another test to illustrate this:


function testThatConnectionExceptionReturnsZeroMatches() {


// GIVEN
var stubRequest = new AddressLookupRequest() {
override property get AddressLine1() : String {
return "63 Burnside Avenue"
}


override property get PostalCode() : String {
return "02840"
}
}


var stubService = new AddressLookupService() {
override function doLookup(request : AddressLookupRequest) : AddressLookupResponse {
throw new java.net.ConnectException("Test Connect Exception")
}
}


// WHEN
var response = new AddressLookup().doLookup(stubRequest, stubService)


// THEN
assertEquals(0, response.AddressDTOs.length)
}


As before, the stub is the simplest implementation of the service interface that is sufficient for the behavior we want to test.


We can do more with stubs. We can, for example, use a different stub implementation to write a positive test for the integration: a test in which the service returns a valid response, all without ever connecting to the third-party service. I leave this as an exercise for the reader.


## Test Stubs Improve Focus and Speed


There is only one acceptable excuse for not writing unit tests: we don’t know how to write them. In this post, I’ve shown how to write unit tests for outbound integrations using stubs.


In addition to giving us the means of testing the apparently untestable, a stub endows our tests with two desirable properties. The first is focus. A stub focuses a test more precisely on the technical behavior we’re working on. If the unit tests we’ve written in this post fail, for example, we’ll know that the problem is likely in the integration code and not in the dependency code. This makes it easier to track down and fix the failures.


The second desirable property that stubs bestow on our tests is speed. Since a stub is the simplest possible implementation of a dependency that is sufficient to test the technical behavior we’re working on, it is extremely fast compared to the real dependency. If we needed to set up an Address entity in the GIVEN section of the previous tests, for example, we would need to interact with the database, which would really slow the tests down. With a stub, we don’t need a database at all, which means that our tests stay speedy. Stubs, therefore, are a tool we can use to counter the excuse that writing tests “slows down the build.”


But there’s more we can do to keep our tests out of the slow lane. More on that next time.


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
