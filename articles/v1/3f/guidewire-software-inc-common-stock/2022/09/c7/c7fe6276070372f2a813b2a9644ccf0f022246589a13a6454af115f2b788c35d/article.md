---
schema_version: "1.0.0"
document_id: "c7fe6276070372f2a813b2a9644ccf0f022246589a13a6454af115f2b788c35d"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/how-to-use-fluent-assertions-in-your-unit-tests"
published_at: "2022-09-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:d7c1c944c47f6394fe4e3bf5c8c0a66c47919ae7d3cd7c29344b0b35a39c425a"
---

# How to Use Fluent Assertions in Your Unit Tests

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


- How to Use Fluent Assertions in Your Unit Tests


## Or: Safety First


A reader of these posts wrote to me recently to point out a problem. The assertions I used, she wrote, do not follow the Guidewire Cloud Standards for unit testing. She’s right. They don’t. And they’re less readable and more costly to maintain because of it. In this post, I’ll unpack the subtle effects that assertions have on readability and maintainability. Then I’ll rewrite my tests so that they comply with[Guidewire Cloud Standards](https://docs.guidewire.com/cloud/standards/latest/Overview/overview.html?_gl=1*pjehrh*_gcl_au*MTUwMTA3Nzg0NC4xNzI1MDQ1MDUz*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczMjMyMDk2OS44MS4xLjE3MzIzMjIxODAuNTcuMC4w) .


In[my previous blog post](https://www.guidewire.com/resources/blog/developers/why-speed-is-essential-for-unit-tests) , I argued that good unit tests provide fast feedback on code changes. Every change we make risks breaking something, and sometimes things break in unexpected ways. So we need a safety net to catch us when we slip up. Our safety net is our suite of unit tests. But a test must do more than simply fail when a change breaks something: it must tell us why it failed so we know how to repair the damage.


## The Wrong Assertions Can Create Confusion and Distrust


Let’s look again at a GUnit test from my post on stubs:


function testThatMissingAddressLine1InRequestThrowsIllegalArgumentException()
{


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


This test has a single assertion in the // THEN section. Looking at the second argument of the assertion, we see that it evaluates to true when the address lookup throws an IllegalArgumentException. When this happens, execution continues, the test completes, and the test framework reports the test as passed.


If, on the other hand, the address lookup doesn’t throw an exception, or if it throws an exception that is not of type IllegalArgumentException, the assertion evaluates to false and throws an AssertionFailedError. When this happens, test execution stops and the test framework reports the test as failed. The test framework also displays the explanatory message we pass as the first argument to the assertion: “Exception thrown is not an IllegalArgumentException.”


This assertion is fine as far as it goes, but it presents two problems. The first is that the assertion is not at the right level of abstraction. It’s an example of what Lasse Koskela (in his book[Effective Unit Testing: A guide for Java developers](https://www.amazon.com/Effective-Unit-Testing-guide-developers-ebook-dp-B09782PYWR/dp/B09782PYWR/ref=mt_other?_encoding=UTF8&me=&qid=) ) calls a primitive assertion. “A primitive assertion,” writes Koskela, “is one that uses more primitive elements than the behavior it’s checking.” The primitive elements here are the language operator typeis and the concepts of true and false. These are problematic because they obscure the intent of the test and create cognitive work that readers of our tests shouldn’t need to do.


To lighten the cognitive load, we add an explanatory message as above. This message also helps debug a failure, because without it the test framework would display no information to help developers understand the failure. Developers would have to go to the test and read the assertion to understand why the test failed. Again, this is extra work that they shouldn’t need to do.


But while an explanatory message helps developers, it incurs a maintenance cost. And this is the second problem with the assertion. If the assertion is refactored, we must remember to refactor the message as well. This is often overlooked. And when it is, we have an assertion with a condition and a message that say different things — perhaps even contradictory things. When readers of the tests uncover this sort of ambiguity, they lose faith in our tests.


## Fluent Assertions Create Trust and Safety


We can address these problems and safeguard trust in our tests by using fluent assertions. Fluent assertions are an example of a fluent interface, a design practice that has become popular in the last two decades. A fluent interface uses method names to create a domain-specific language (DSL) and chains method calls to make code read more like natural language.


Here’s my GUnit test rewritten to use fluent assertions:


uses gw.testharness.v3.PLAssertions#assertThat(Throwable)


…


function testThatMissingAddressLine1InRequestThrowsIllegalArgumentException()
{


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
assertThat(t).isInstanceOf(IllegalArgumentException)
}


The assertion in this test is expressed in terms of the DSL for assertions — specifically “assert that” and “is instance of” — which brings it to the right level of abstraction and makes its intent clear. And method chaining makes it easier to read. This means we no longer need the message in our code.


Moreover, we no longer need the message to help developers debug a failure. When this assertion fails, the test framework displays a useful message by default. If the test fails because the exception thrown is a NullPointerException, for example, the test framework displays the following message:


java.lang.AssertionError: expected instance of:<java.lang.IllegalArgumentException> but was instance of:<java.lang.NullPointerException>


This tells developers exactly what caused the failure: we were expecting one thing but got another. We can still add a custom message to our fluent assertion if we wish, but (as explained above) this incurs a maintenance cost, and the default message is usually enough to tell developers what they need to know. If developers need more context, they can use the names of the test class and test function, which are in the stack trace for the assertion error, which is also displayed by the test framework:


at org.fest.assertions.Fail.failure(Fail.java:228)
at org.fest.assertions.Assert.failure(Assert.java:149)
at org.fest.assertions.ObjectAssert.isInstanceOf(ObjectAssert.java:56)
at org.fest.assertions.ThrowableAssert.isInstanceOf(ThrowableAssert.java:75)
at acme.cc.integration.addresslookup.WhenLookingUpAddressTest.testThatMissingAddressLine1InRequestThrowsIllegalArgumentException(WhenLookingUpAddressTest.gs:64)
…


The combination of default failure message and expressive names for test functions and test classes clearly communicates the reason for the failure in terms of the technical behavior being tested. When we look up an address, we test that missing address line 1 in the request throws an IllegalArgumentException, but we got a NullPointerException instead. The developer knows what went wrong, why it went wrong, and where to go to fix it — all without needing to read the test code.


## It’s Your Turn to Put Fluent Assertions to the Test


In my post on[writing maintainable unit tests](https://www.guidewire.com/resources/blog/developers/how-to-make-unit-tests-easier-to-maintain) , I drew heavily on concepts from[John Ferguson-Smart](https://johnfergusonsmart.com/) ’s book BDD in Action: Behavior-driven development for the whole software lifecycle. I draw on that essential resource again to emphasize the benefits that fluent assertions bring to our tests (p.297):


Fluent assertion libraries are in no way specific to BDD \[behavior-driven development\], and they can be used to make any unit tests easier to understand. But their emphasis on readability, expressiveness, and communication makes them well aligned with the BDD philosophy.


My GUnit test rewritten to use fluent assertions is more readable, communicates its intent more clearly, and is more maintainable. In short, it’s a stronger knot in our safety net. More importantly, however, it’s compliant with the Guidewire Cloud Standards for unit testing (login required). But readers shouldn’t take my word for it. They should check out the standard for themselves. And if there’s still a problem, please drop me a line.


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
