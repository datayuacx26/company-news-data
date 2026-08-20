---
schema_version: "1.0.0"
document_id: "432c17f2793b1f4a9945199270a7807d534065654dbee77362031410cb5f3719"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/how-to-make-unit-tests-easier-to-maintain"
published_at: "2022-04-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:6cbcb7e704a4f776a8c14a6a21ed192bf985192b4f4195b64826e4d03ff44cf1"
---

# How to Make Unit Tests Easier to Maintain

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


- How to Make Unit Tests Easier to Maintain


## Or, “When” and the Art of Unit Test Maintenance*


I was talking recently with a colleague about quality on his self-managed Guidewire implementation. Quality can be a hard sell when it’s up against budgets and timelines, but he’d made good headway getting his team to subscribe to what Steve McConnell calls the[General Principle of Software Quality](https://www.oreilly.com/library/view/code-complete-2nd/0735619670/ch20s05.html) , which is that improving quality reduces development time and costs.


“The best way to improve productivity and quality,” writes McConnell, “is to reduce the time spent reworking code.” But this requires that the team make investments upstream in the development flow to prevent defects so that the code downstream doesn’t have to be reworked.


My colleague talked enthusiastically about the upstream investments his team had made to prevent defects: more effective code reviews, more static code analysis rules, and more robust automated builds. Then he paused. I could hear the word but in the air. There’s always a but when I talk to people about quality.


The but was this: the team couldn’t get behind unit testing. They’d tried to do unit testing before, but it had ended badly. Instead of an aid to developers, the tests became a burden because they were difficult to maintain and time consuming to debug when they broke. So the team gave up on unit testing.


## Unit Testing Doesn’t Have to Be Difficult


But it doesn’t have to be this way. Our[cloud standard for unit testing](https://docs.guidewire.com/cloud/standards/latest/Testing/Standard-GW-TST-1086-UnitTesting.html?_gl=1*1uvw8il*_gcl_au*MTUwMTA3Nzg0NC4xNzI1MDQ1MDUz*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczMjMxMzY1NS44MC4xLjE3MzIzMTQ3OTMuMTUuMC4w) , which is available on the Guidewire Documentation site (login required to view), keeps tests easy to understand and easy to maintain. In this post, I’ll use GUnit tests for an InsuranceSuite address lookup integration to show how the standard works.


Before I get to that, here’s what I often see on projects:


class AddressLookupTest {


function testDoLookup() {


var addressLookup = new AddressLookup()


…


addressLookup.doLookup(…)


…


assert(…)


}
}


This test is difficult to maintain for two reasons. First, it’s tightly coupled to classes and functions in the integration code. If these are renamed (as so often happens), the test class or test function must also be renamed. That’s not such a big deal when we have an IDE that supports this kind of refactoring. But it’s still a maintenance overhead, especially if we need to fundamentally restructure the integration code.


The second reason this test is difficult to maintain is that it’s initially unclear what is being tested. If the test breaks, a developer needs to understand what the test is for before attempting a fix, which can take time. And this is true of all developers on the team — even the developer who wrote the test! We quickly forget the intent of the code that we write.


## An Easier Approach: Focus on Behaviors


The Guidewire Cloud standard for unit tests takes a different approach. Instead of tests that focus on the classes and functions of the integration, our tests focus on behaviors. For the test described in the preceding section, we can refocus on behaviors by making three changes.


**1. Rename the test class.**


The test class should tell us what business behavior we’re working on. A business behavior is just a high-level requirement, something the system needs to do from the point of view of the business. Here, we’re working on the behavior of looking up an address.


Thus, we rename our test class as follows:


class WhenLookingUpAnAddressTest {


function testLookup() {


var addressLookup = new AddressLookup()


…


addressLookup.doLookup(…)


…


assert(…)


}
}


This helps us stay focused on the business value when we write our tests. But we also need to focus on how we deliver that value.


**2. Rename the test function.**


This brings us to the second change: to rename the test function to tell us what technical behavior we’re working on. A technical behavior is just a low-level requirement, something the system needs to do from the point of view of a developer. This integration will have many technical behaviors. Here, I’ll deal with just one: the integration should throw an exception if the request object that contains the details of the address to look up is null.


Thus, we rename our test function as follows:


class WhenLookingUpAddressTest {


function testThatNullRequestThrowsIllegalArgumentException() {


var addressLookup = new AddressLookup()


…


addressLookup.doLookup(…)


…


assert(…)


}
}


**3. Add the right logic.**


The third change we need to make is to add the right logic to test this behavior. We should think of the logic in any test as comprising three sections:


- The context, which we call “Given”
- The behavior, which we call “When”
- The expected outcome, which we call “Then”


We use these three sections to make our test cleaner and easier to understand:


class WhenLookingUpAddressTest {
function testThatNullRequestThrowsIllegalArgumentException() {


// GIVEN
var request = null


// WHEN


var t : Throwable


try {


new AddressLookup().doLookup(null)


} catch (e : Exception) {


t = e
}


// THEN


assert(t typeis IllegalArgumentException)


}


}


## Good Unit Tests = Good Communication


This approach to unit testing is based on principles of[behavior-driven development (BDD)](https://cucumber.io/docs/bdd/history/) . Specifically, it draws on Chapter 10 of John Ferguson Smart’s book *[BDD in Action](https://johnfergusonsmart.com/)* (page 293):


From a BDD perspective, writing a good unit test is an exercise in good communication. When you practice BDD, you think of every unit test as a low-level specification that illustrates some aspect of how a class or component behaves…. But the implementation of your test is also sample code that illustrates how a particular requirement is satisfied, or how a particular goal is achieved. The code inside your tests doesn’t just exercise the application; it documents how to exercise the application.


Note: If you want to learn more about behavior-driven development methodology, you can[enroll in our self-paced Guidewire Education course](https://ilearning.seertechsolutions.com/lmt/clmsCatalogDetails.prMain?site=guidewire&in_region=us&in_offeringId=93523138&in_language_identifier=en-us&in_filter=%26in_courseName%3Dbehavior%2520driven%2520development%26in_location%3D%2525%26in_rows%3D50%26in_courseType%3D%2525%26in_orderBy%3DXD%26in_region%3Dus%26in_language_logged_out%3Den-us%26in_start%3D) .


Unit tests that focus on behaviors are more robust when the integration code is refactored because requirements change less frequently than the code that implements them. And because the tests document what the integration code is for and how to use it, a developer faced with a test break can quickly understand what requirement is broken and what to do to fix it. In addition, since the names of test functions describe what the integration code should do rather than how it does it, we’re prompted to think about edge cases — what the integration should do when it veers off the happy path.


In this blog post, we’ve seen one test for one edge case. However, the integration needs to do more than throw an exception when an invalid request is passed. The most important thing it needs to do, at least for business users, is to look up an address when passed a valid request. And we need a test for that behavior. But to write that test, we need to add a new technique to our testing toolbox: test doubles. That’s a topic for next time.


* This is a play on the title of the 1974 mega-best-selling philosophy book by Robert Pirsig, *[Zen and the Art of Motorcycle Maintenance](https://en.wikipedia.org/wiki/Zen_and_the_Art_of_Motorcycle_Maintenance)* . While the reference may be obscure to some, Pirsig’s book was recommended reading for one of the programming courses I took while attending university.


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
