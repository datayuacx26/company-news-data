---
schema_version: "1.0.0"
document_id: "4caf09f72565f52ab6443dc4ad90e85ea16e0aec6a435377f415306540d05818"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/why-speed-is-essential-for-unit-tests"
published_at: "2022-08-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:f06fefc6694d7bc95e944e30b08348db1510f3102e562cd714dbff7a07809f50"
---

# Why Speed Is Essential for Unit Tests

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


- Why Speed Is Essential for Unit Tests


## Or: Never trust a surgeon who operates with a butter knife


In the early days of our discipline, performance was paramount. Programmers would scrutinize code for hours looking for ways to use one byte less of storage or to take one millisecond off execution time.


Rear Adm. Grace Hopper, one of the first programmers, underscored the importance of execution time for the pioneers of our profession in a 1985 lecture she gave at MIT:


Hopper held up a section of wire 984 feet long, the maximum distance light or electricity travels in a microsecond. In her deadpan delivery, she said, “I think it’d be a fine idea to hang one over every programmer’s desk — or maybe around their necks — so they’ll know exactly what they’re throwing away when they throw away a microsecond.”


## Good Unit Tests Are Fast by Design


We modern practitioners of the profession throw away microseconds all the time. We must, however, pay close attention to execution time if we’re serious about writing unit tests. As Michael Feathers argues in his influential book *[Working Effectively with Legacy Code](https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052)* , unit tests must run fast.


“If they don’t run fast,” Feathers writes, “they aren’t unit tests.”


Speed, of course, is just one quality of good unit tests. The Guidewire Cloud Standard on unit testing details others. For example, unit tests must describe behaviors (as I discussed in my[first post](https://www.guidewire.com/resources/blog/developers/how-to-make-unit-tests-easier-to-maintain) , on keeping unit tests maintainable). Unit tests must also be reliable and independent, and they must illustrate a worked example. Feathers distills these other qualities into a single mandate: Unit tests must help us localize problems.


Helping to localize problems and running fast are, in fact, two sides of the same coin. If tests don’t run fast, we won’t run them. We’re an impatient lot. But if we don’t run our tests, they can’t detect problems and definitely won’t be able to localize them. On the flip side, if our unit tests help localize problems, they must test code in isolation, and testing code in isolation makes tests fast (as I argued in my[previous post, on stubs](https://www.guidewire.com/resources/blog/developers/how-to-write-unit-tests-for-outbound-integrations-stubstitution) ). In short, fast tests help localize problems — and tests that help localize problems are fast.


## Faster Tests = Faster Feedback = More Careful Changes


But why this fixation with speed? Or, looking at the other side of the coin, why this fixation with localizing problems? The answer is that we need fast feedback on our changes. Every time we make changes to our code, we risk introducing a problem. If, after making changes, we don’t have unit tests that we can quickly run to verify that everything still works as expected, not breaking something depends on our ability to make those changes with care. But, as Feathers writes:


*“I don’t think any of us would choose a surgeon who operated with a butter knife just because he worked with care. Effective software change, like effective surgery, really involves deeper skills…When we have a good set of tests around a piece of code, we can make changes and find out very quickly whether the effects were good or bad. We still apply the same care, but with the feedback we get, we are able to make changes more carefully.”*


## How to Ensure That Your Unit Tests Run Fast


How do we ensure that unit tests run fast so that we get fast feedback and can make changes more carefully? Feathers suggests some rules:


- Don’t talk to a database.
- Don’t communicate across a network.
- Don’t touch the file system.
- Don’t rely on special environment settings (such as configuration files).


In GUnit, we can use the unit test base class in InsuranceSuite version 10 (and later versions) to make sure that our unit tests never talk to the database and never rely on configuration files or other special environment settings. When we execute a test class that extends the unit test base class, we get a minimal server context with no database and none of the InsuranceSuite frameworks. In ClaimCenter, for example, we use the base class CCUnitTestClassBase:


class WhenLookingUpAnAddressTest extends CCUnitTestClassBase {
function testThatNullRequestThrowsIllegalArgumentException() {
…
}


function testThatMissingAddressLine1InRequestThrowsIllegalArgumentException() {
…
}
function testThatConnectionExceptionReturnsZeroMatches() {
…
}
}


The unit test base class doesn’t prevent developers from accessing the file system or communicating over a network. Following these rules is up to us, but it’s critical that we do follow them. Unit tests that include expensive operations, such as those highlighted by Feathers, simply won’t be fast enough to be of any use to us.


## Bottom Line: Fast Unit Tests Lower the Cost of Code Changes


Fast feedback is, in the end, about keeping the cost of code changes low. The sooner we know we’ve introduced a problem — which is what a test failure signals — the cheaper it is to fix. The fix is cheapest when the failed test helps us understand where the problem lies. The first step in understanding a test failure is to look at the assertion error and the assertion statement that threw it. At a minimum, this statement must be easy to read. One of the best ways to ensure that this is the case is to use fluent assertions. And that’s the topic of my next blog post.


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
