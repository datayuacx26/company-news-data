---
schema_version: "1.0.0"
document_id: "f05681db4fbaa7744f3af8e54174ebb062928064e0d937f82f57863eb7361c4d"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/best-practices-for-assertions-in-unit-tests"
published_at: "2022-12-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:0087af1e964083eb2a73ef5edd932bbbfef75eae171fe04a013eeeb6e2e95318"
---

# Best Practices for Assertions in Unit Tests

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


- Best Practices for Assertions in Unit Tests


## Or: How to Keep Our Promises


In[my last post](https://www.guidewire.com/resources/blog/developers/how-to-use-fluent-assertions-in-your-unit-tests) , I argued that fluent assertions build trust in our tests. But to declare our tests completely trustworthy, we need more than the right assertion library.


Let’s consider the following unit test for our InsuranceSuite address lookup integration:


function testThatMissingPostalCodeInRequestThrowsIllegalArgumentException() {
…
}


For this test to be trustworthy, it must deliver on the promise made by the name of the test. Since the name of the test refers to throwing an IllegalArgumentException, it needs an assertion that tests that an IllegalArgumentException is thrown. So, something like this:


function testThatMissingPostalCodeInRequestThrowsIllegalArgumentException() {
// GIVEN
…


// WHEN
var t : Throwable
…


// THEN
assertThat(t).isInstanceOf(IllegalArgumentException)
…
}


## What’s in a Name?


What I’ve said so far may seem obvious. Nonetheless, trustworthiness is an important concept in testing. In fact, trustworthiness is a corollary of a more general principle of good software design: the principle of least surprise. As Robert C. Martin writes of this principle in his book[Clean Code: A Handbook of Agile Software Craftsmanship](https://www.goodreads.com/book/show/3735293-clean-code) (pp.288–289):


*“…any function or class should implement the behaviors that another programmer could reasonably expect…When an obvious behavior is not implemented, readers and users of the code can no longer depend on their intuitions about function names. They lose their trust in the original author and must fall back on reading the details of the code.”*


While the principle of least surprise is usually cited when talking about code in the production system, it also applies to the classes and functions that test that code. Like production functions, the name of a test weighs heavily in users’ expectations of how it will behave. But unlike production functions, how a test behaves is down to its assertions. This means that if a test aspires to the principle of least surprise, it must assert what the name of the test promises will be asserted. This is the essence of a trustworthy test.


## Three Mistakes to Avoid in a Unit Test


With respect to unit tests specifically, there are three ways we might unintentionally compromise trustworthiness:


1. The unit test does not include an assertion
2. The unit test name is at odds with the assertion
3. The unit test includes an assertion that never executes


## 1. The Unit Test Does Not Include an Assertion


The first, and most egregious, is when we write a unit test without an assertion. A unit test without an assertion is just a function. With no assertion, the test might never fail, and a test that can’t fail can’t signal a problem in our code.


If readers and users of our tests can’t find an assertion in what appears to be a unit test, readers of our tests will be very surprised. But surprise is hardly the worst of it. As Lasse Koskela writes in his book[Effective Unit Testing: A guide for Java developers](https://www.goodreads.com/book/show/17282399-effective-unit-testing) :


*“tests that don’t deliver on their promises…\[put\] ourselves and our colleagues at risk of wasting precious time by making false assumptions, making flawed decisions based on those assumptions, and looking at the wrong places when we find out that things aren’t as they should be. Time is one of the true constraints we have in our lives—don’t waste it with such trivial mistakes as a test making a shallow promise.”*


## 2. The Unit Test Name Is at Odds with the Assertion


Besides a shallow promise, a unit test might make a crooked one. This is the second way we might introduce an untrustworthy test into the codebase, and it happens when the assertions in the test body are at odds with the name of the test.


Returning to the unit test for our address lookup integration, readers would be very surprised if they didn’t find an assertion that tests whether an IllegalArgumentException is thrown. But readers would be equally surprised if they found that same assertion in a test named testThatMissingPostalCodeInRequestReturnsZeroResults. As before, surprise will quickly give way to doubt and confusion. Is the name of the test correct? Are the assertions correct? Or are they both, in fact, incorrect? Addressing these questions wastes yet more precious time, which further increases the cost of making changes to the codebase.


## 3. The Unit Test Includes an Assertion That Never Executes


The third and most costly way we might introduce an untrustworthy test into the codebase is a subtle variation on the first. It’s so subtle, in fact, that reading alone is rarely enough to detect it. It happens when we write a unit test with an assertion that never executes. Like a “test” without an assertion, a test with an assertion that never executes might never fail, and a test that can’t fail can’t signal a problem in our code.


Let’s consider one final time the unit test for our address lookup integration. We might be tempted to write it as follows:


function testThatMissingPostalCodeInRequestThrowsIllegalArgumentException() {
// GIVEN
var stubRequest = new AddressLookupRequest() {
override property get AddressLine1() : String {
return "63 Burnside Avenue"
}
override property get PostalCode() : String {
return ""
}
}


// WHEN
var t : Throwable
try {
new AddressLookup().doLookup(stubRequest)
} catch (e : Exception) {
// THEN
assertThat(t).isInstanceOf(IllegalArgumentException)
}
}


The problem with this test is that if the doLookup() function never throws an Exception, the assertion is never executed and the test passes, even though the technical behavior documented in the name of the test is not present. We clearly can’t trust this test. We might goof when making a change to how doLookup() validates requests and this test would never alert us to the problem.


## A Good Rule of Thumb: See the Test Fail


There is, thankfully, a rule of thumb that helps guard against untrustworthiness in tests: see every test fail at least once. If we never see our test fail, we may inadvertently signal to our teammates that system behaviors are present and correct when in fact they are missing or broken. Of course, once we see our test fail, we must make it pass again, and do it in a way that ensures we keep our promises.


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
