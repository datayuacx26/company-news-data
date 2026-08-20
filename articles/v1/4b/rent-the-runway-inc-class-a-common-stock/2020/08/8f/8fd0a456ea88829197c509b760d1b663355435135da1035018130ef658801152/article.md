---
schema_version: "1.0.0"
document_id: "8fd0a456ea88829197c509b760d1b663355435135da1035018130ef658801152"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/power-of-snapshotting-microservice-versions"
published_at: "2020-08-06T20:07:02+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:ad483846f0608381171d80a66a4b7cf80d20a9a71908ee4bfddddcb173b85cf8"
---

# Ch-ch-changes: The Power of Snapshotting Microservice Versions

# Time May Change Me


Here at Rent the Runway, we have a number of microservices that handle various parts of what make Rent the Runway run. From our website to shipping, each service plays an essential role in making the customer experience a smooth one. In a previous blog post, we talked about how we[leverage acceptance tests](https://dresscode.renttherunway.com/blog/leveraging-flaky-tests) to determine the health of the customer experience. These acceptance tests run through scenarios that span across many services, and since we are constantly moving to make the experience better and better, our services are constantly changing.


While the use of microservices comes with many advantages, it also comes with disadvantages. One disadvantage is not knowing what effect a change in a single service will have on downstream services. These gaps are filled with various forms of testing, such as Acceptance, Contract, and End-To-End Testing. These tests give us confidence that we are keeping our promises to the customer. Since we are pushing code constantly, we are testing constantly. If these tests fail, it is important to know what changed. It is entirely possible for versions of services to change in between test runs, and even during the test run itself.


# But I Can’t Trace Time


Since the deployments can happen at any time, we record the microservice versions at the beginning and end of each test run. With these records, we perform the following diffs:


-


Start vs. Finish of Current Run


-


End of Previous Run vs. End of Current Run


The raw diffs are stored in a map in memory, and written as a CSV.


Why both diffs, though? Debugging comes down to the scientific method. You must isolate the changes in order to perform an experiment. Knowing when there was a code change, what was in that code change, and what errors occurred give us enough information to more quickly track down a root cause and understand why the hypothesis was not met.


# Turn And Face The Strange


How do we track what is happening at any point in time? To add visibility to version changes in each test run, we started by reporting the versions before and after each test run in Slack. We did so using the[Jenkins CI Slack app](https://slack.com/apps/A0F7VRFKN-jenkins-ci) provided by Slack. This provided immediate value, but was quite noisy, regardless of the formatting. We needed a diff, but we also needed the capability to use the more advanced features of Slack messaging.


[Jenkins, the organization, now considers](https://github.com/jenkinsci/slack-plugin/blob/master/README.md) t


[he Jenkins Slack app to be a legacy app](https://github.com/jenkinsci/slack-plugin/blob/master/README.md) . To take advantage of newer features in Slack, such as threads, file uploads, and block message formatting, we needed some infrastructure changes. In this case, we needed a custom Slack app, which creates a Bot User to post messages to our Slack Workspace. This turned out to be quite an easy change, as we just needed a new token, a setting in the Jenkins config specifying that we are using a custom app, and a couple of test runs to ensure all calls to the legacy app work with the new app.


# I Watch The Ripples Change Their Size


Now that we have our Slack App, we can better report on versions over time. At the beginning and end of each test run, we record the versions of our microservices in a map. We then archive the ending versions to Jenkins using the archiveArtifacts step. With those versions recorded both in memory and in artifacts, we have all the information we need for diffs.


We needed to write custom Jenkins step definitions, as diffs were specific to how we record our service versions. We found it more valuable to have all service versions listed in our diffs, as we needed the snapshot of the entire environment at the time, given the nature of microservices. We also needed to keep the noise in Slack low. To do so, we chose to only post diffs on failures, and only post in threads.


The Slack posts for successes and failures, respectively, look like this:


Successes have no associated diffs, and failures have 2 diffs. Our thread posts are formatted as such:


If a service version has changed, it is bolded, making it easier for everyone to find. The other versions are kept in, as we may need to understand how the changed service interacts with any of the unchanged services.


With these diffs, we are armed with even more information to appropriately root cause any issues that come up, so we can quickly return to delivering our customers the experience they deserve.
