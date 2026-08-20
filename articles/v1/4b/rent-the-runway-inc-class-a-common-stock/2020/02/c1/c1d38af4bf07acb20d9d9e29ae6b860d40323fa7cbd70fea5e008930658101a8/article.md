---
schema_version: "1.0.0"
document_id: "c1d38af4bf07acb20d9d9e29ae6b860d40323fa7cbd70fea5e008930658101a8"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/leveraging-flaky-tests"
published_at: "2020-02-03T21:11:30+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:22a5cdaf9f7afdcac60c11b63d95ea29cf25055fe86d877668e6643828a6dc6a"
---

# Leveraging Flaky Tests to Identify Race Conditions Using Cypress

I’m Nicole Weber, a front-end engineer at RTR working on personalization and product-discovery features for[subscribers](https://www.renttherunway.com/plans) . I'm passionate about automated testing, and since joining RTR I've helped implement a Cypress-based solution to test our complex systems.


## End-to-end testing our website with Capybara


To test our customer-facing website,[www.renttherunway.com](http://www.renttherunway.com/) , we've been using a suite of[Capybara](http://teamcapybara.github.io/capybara/) tests for a while. They do a great job of catching bugs in our development environment before our customers ever see them, and are generally very easy to develop. When a test fails, we see a description of the failed step or assertion along with a screenshot of the application (our website) on failure, which we can use to try to understand the state of the application when the test failed.


One feature that we monitor with Capybara tests is a short onboarding sequence for new subscribers: When customers initially purchase RTR Unlimited or Update subscriptions, they answer a few questions so we can recommend clothes they'll love. In many of our end-to-end tests, we generate new test subscribers and have them complete this onboarding flow before proceeding to more complex subscription flows.


The last step of the flow is an address confirmation screen, so we know where we’re shipping orders:


## We experienced intermittent failures, or “flakiness”, in our Capybara tests


Sometimes we saw this onboarding sequence fail in an odd way. Based on the screenshots, it looked like the test never completed the address confirmation step and was stuck there. It didn't make sense because the required fields had been selected and the “Save” button was enabled and visible. Other times, the tests would complete these steps and continue just fine. When it failed, we saw a screenshot exactly like the one above, of the last onboarding step to select a shipping address.


We'd attempt to replicate these failures manually and never could. Looking at the application code, there was no logic that should have prevented the application from completing onboarding. It seemed like something was wrong with the tests — like they were flaky, and behaving differently than a real user would. Often the same tests would pass on the next run, so we grew a little desensitized to these failures, thinking there was nothing we could do about them.


## Giving Cypress a try


In the past year, we started developing similar end-to-end tests with[Cypress](https://www.cypress.io/) . We'd heard about the great development tools it provides, and even that "it's less flaky" than Capybara, and thought we could add better overall system coverage with Cypress. We wrote a bunch of tests, loving the ability to wait on specific XHR steps we knew blocked interactions and the control over our tests that specificity provided. Nevertheless, one day we started seeing intermittent failures in Cypress tests that covered the same subscriber onboarding process. When we tried to replicate the issues manually, we couldn't.


Thankfully, Cypress's development console let us look more closely at each step of a test. Instead of a single screenshot of the application when the test failed, we got snapshots of the DOM before and after each step execution. This let us zero in on what exactly was happening in this test step and revealed that, actually, the address confirmation step was not failing to complete like we thought!


The test user completed the step:


The onboarding modal closed as expected:


And then the onboarding modal re-opened!


This revealed a race condition in the application where, in some cases, a delayed network response could cause the onboarding modal to re-open itself, as if the user hadn’t submitted the last step yet. The test user had completed all the steps in the sequence, so the onboarding modal opened to the last one.


With this information, we were able to look at the correct piece of code (the conditions controlling when the modal is closed or open, as opposed to the conditions that submit the questionnaire) and identify the root cause. The network-oriented nature of the race condition explained why the failure happened intermittently. It also explained why humans never replicated this behavior — it's possible that the state where the modal re-opened after closing was so quick that users didn't notice it. Nonetheless, it stopped automated tests from executing their next expected steps, causing failures. After fixing the race condition, we haven't seen the same test failure since!


After this experience, I urge developers to stop being annoyed by their flaky end-to-end tests and test frameworks, and appreciate flakiness as feedback about application design that leads to flaky behavior. Cypress is a great tool to dig into these kinds of issues — behavior like this would have been very difficult to debug without it!
