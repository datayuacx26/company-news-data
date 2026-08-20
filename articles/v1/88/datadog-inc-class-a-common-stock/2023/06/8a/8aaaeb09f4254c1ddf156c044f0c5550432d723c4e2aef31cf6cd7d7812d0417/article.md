---
schema_version: "1.0.0"
document_id: "8aaaeb09f4254c1ddf156c044f0c5550432d723c4e2aef31cf6cd7d7812d0417"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/migrating-acceptance-tests-to-synthetic-monitoring/"
published_at: "2023-06-30T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:1ddaf30d77193dfc0c7bfc50a1be5b118b68d5011efb9e96a81970ccf805a8fb"
---

# How we migrated our acceptance tests to use Synthetic Monitoring

Yoann Moinet


Cecilia Watt


The Frontend Developer Experience team strives to improve the lives of 300 frontend engineers at Datadog. We cover build systems, tests, deployments, code health, internal tools, and more—we’re here to remove any friction and pain points from our engineers’ workflows.


One such pain point was difficult-to-maintain acceptance tests. This is the story of how we migrated a codebase from flaky, unmanageable acceptance testing with Puppeteer (Chromium Headless Browser) to more robust and maintainable Synthetic tests.


## Identifying pain points—with data


Since our team is in charge of making sure that all engineers are equipped with the best tools and workflows, it’s critical for us to be aware of difficulties as soon as possible. Every quarter, we survey all of our frontend engineers to gauge how satisfied they are. We ask about everything related to the developer experience.


Our very first survey, from February 2019, had some clear results: acceptance testing was a major pain point.


## The problem with flaky acceptance tests


We were using a pretty standard approach: our acceptance tests were written in Node.js and ran on our very own custom runner built on top of Puppeteer. There were some heavy drawbacks: tests were flaky and hard to implement, they had to be written manually, and both the tests and platform had to be maintained.


In particular, flakiness was one of the most frustrating aspects. End-to-end tests are usually flaky because there are a lot of components that go into making them run. You need a dedicated machine that manipulates a web browser, uses a virtual graphic engine, and navigates a website. Each component has quirks and hiccups that are out of your control—and may fail unexpectedly.


In addition to this flakiness, these tests are hard to write. You have to transcribe user interactions by script, which isn’t natural at all. You have to juggle timing and interaction details to be certain you end up with a steady and reliable result. For instance, this is what you’d have to do just to click a button: you would have to first verify that it’s actually present in the page; then, verify that the button isn’t disabled; finally, you can click on it—and hope that its event handler is compatible with Puppeteer. And this is just the process for a native button—complexity rises significantly for custom elements. Imagine the steps you’d need to take to safely and reliably interact with a custom dropdown element.


To make matters worse, you also have to maintain a complex testing ecosystem. Every time the product is updated, you have to update the tests impacted by the update. This is often just as difficult as writing a new test.


Meanwhile, on the infrastructure side of things, these tests take tens of minutes to run—and this gets longer as the tests get more sophisticated. In our case, we had six different jobs in our CI, resulting in 14 minutes for the longest one to cover most of our application. The more features you add to your product, the more code you have to compile, the more tests you add, the longer it takes, and the more machines you need.


We had to find a better solution that would allow us to continue testing our product and keep our trust in shipping new code safely and quickly.


## Identifying and implementing a solution: Synthetic Monitoring


We decided to use our own product—[Synthetic Monitoring](https://docs.datadoghq.com/synthetics/) , which would let us record page interactions without having to script anything manually.


We wanted to supply a list of tests, with configurations, and have the API respond with a list of result IDs. We could then poll a new API endpoint with this list of result IDs, to get all their statuses. We worked with the Synthetic Monitoring team on a well-defined and friendly CLI, called` synthetics-ci` , because we want to execute Synthetic tests from the CI. Pragmatic naming.


To sum up, this CLI runner had to:


1.


parse the codebase to find test files that are named` my-test-file.synthetics.json` ,


2.


accept configuration overrides,


3.


trigger tests,


4.


poll their results,


5.


and output the results in a human-readable way.


We noticed that it was our first product to specifically propose an interaction from a CI environment—so we decided to generalize this tool and call it[datadog-ci](https://github.com/DataDog/datadog-ci) , which enables users to execute commands with Datadog from within CI/CD scripts.


## Transitioning to the new system—what could go wrong?


Our plan was theoretically sound, but it is a different task to actually get buy-in from every frontend engineer. At the time of this writing, there are 300 of them, all working and pushing code in a single repository.


This approximately represents, every 24 hours:


-


90 new PRs.


-


160 authors.


-


190 releases.


-


1,120 commits and as many CI/CD pipelines.


-


1,680 modified files.


We’ve been using acceptance tests since October 2017, and we started the migration process in June 2021. At that moment, we had:


-


6 CI/CD jobs, only for acceptance tests.


-


35 minutes of machine time (per commit).


-


84 files in the codebase.


-


565 tests to maintain.


-


100k lines of code—tests and infra included.


There was a lot to migrate, and we needed to gain the engineers’ trust in the system we had built.


## Driving adoption through trust, information, and tooling


First, we wrote a lot of documentation to answer common questions: How do you write a good test? What’s worthy of being tested? What should you avoid when creating a test?


Once we had enough written support, we held presentations at our global events and our monthly frontend gatherings to demonstrate the system we’d built. We explained how it could help engineers, how to create tests with the UI, how it was better than acceptance tests and needed way less maintenance, what kind of use case it covered, how to use scheduled tests in the CI, etc.


Finally, we reached out to specific teams—the ones that had the most tests. We helped them implement new tests (based on the acceptance tests they owned) and use their existing scheduled tests directly in the CI.


We also worked on tooling. We implemented a non-blocking job in our CI/CD so engineers could add tests in the CI without actually blocking everyone in case of failures. This allowed them to become familiar with the technology without the fear of impacting everyone with it. Instead of blocking the CI, we were just surfacing the failures at the PR level as a comment, so teams were still aware of each failing test. This built trust in the new workflow.


Finally, we tracked the migration of every single acceptance test in Jira. We assigned each ticket to the team that owned it, and we explained the migration process. We had specific checkpoints along the way, which helped us spread the workload on our team, which was maintaining the platform used to run the acceptance tests. In this way, we could progressively sunset our old platform instead of deleting everything at once at the very end, which could have been riskier. Once we made sure every test was running smoothly, we removed the signaling at the PR level and made the pipeline blocking.


## What we learned: not only how to migrate, but also how to work together, better


The whole migration project took place over one year. It was a long process, but we managed—by being nosy neighbors—to migrate all of our acceptance tests. We also had the opportunity to improve one of our products. Despite the challenges we faced, we were able to create tools that serve our frontend community’s needs.


We learned that changing such an ingrained process can be difficult, especially with so many people involved and impacted. But with planning, dedication, and perseverance, we were able to successfully complete it in a timely manner. It took time to make the migration progressive and incremental, but it was necessary.


We knew we could not simply switch off the old process and switch on the new one—we had to craft an intermediate workflow, where engineers could opt in to familiarize themselves with the new process, and build trust until everyone migrated. Having them opt in to the new process also made our support way easier, as people were actually motivated to go through the migration and knew why they needed to do it. As much as possible, we tried not to impose it, but instead explained and proved that it was the way to go.


Finally, we also learned that when we have a consequent need for a product—like here, building a whole CI integration for an existing product—it’s best to offer all the help we can to design and implement it. We couldn’t just share our requirements, let another team work on their own, and wait for the results. Working together helped us craft this new feature quickly and ensured that it filled our actual needs in the best way possible.


-
-
-
