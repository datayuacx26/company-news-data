---
schema_version: "1.0.0"
document_id: "a2295e6997c98493221c2d9688a4e8831d6af83e235232ac36a72532712025d5"
company_key: "yc-send"
company: "SEND"
source_id: "yc-send-news-import-308d5d8ef15e"
canonical_url: "https://blog.send.ng/blog/ci-cd-optimising-for-engineering-speed-and-product-stability/"
published_at: "2021-05-06T00:00:00+00:00"
first_seen_at: "2026-07-24T00:42:30.669100+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:7dcf3eaef6e708f3ae298ecfccfa60c322a60dea5526d5b3f06aa98d006f2f54"
---

# CI-CD: Optimising for Engineering Speed and Product Stability

Richard Igbiriki


Published On:


06 May, 2021


## Tech Stack


Before going into detail about our CI-CD process, it is essential for us to talk a little about our stack and how we build out products at SEND.


At the core of it, we run a typical MERN stack with associated tooling. On the backend, we have Express and Mongo while on the frontend we have React. Unit tests for express are written with Mocha while the frontend runs Jest/Enzyme for unit tests and Cypress for integration tests.


Now that we know about the stack we can proceed to delve into the details of our development pipeline.


## CI-CD and Environments


A team of four(4) engineers building three(3) products is not unheard of but it does present a challenge when you need to do it right and do it quickly. At startups, we often hear "build fast and break things" because speed is essential to your survival, but so is building a stable product. Furthermore, it would be counter-productive if you build fast but end up spending the next week fixing bugs that could've been caught and/or prevented. Our mantra thus moved from "build fast, break things" to "build relatively fast, ensure nothing breaks". To achieve our goal, we had to setup a process that helps us catch bugs faster, while increasing engineering output.


###### Environment - Development


Each newly created branch (with a commit) runs the base workflow which includes lint, prettifier, unit tests, deploys to a development server, and runs integration tests against the newly deployed instance. Cypress plays a key role in ensuring that our builds are stable since historically, the bugs we found in core aspects of our products were not syntax but user-story related. Cypress enables us to "mimic" user-stories and test to ensure that as we continue to build the product, we do not intentionally or unintentionally break a user-story. Unit tests are great, but Cypress tests have proven to be greater.


###### Environment - Hold Dev


While slightly less used, there is a provision for engineers to deploy any branch to a separate AWS S3 bucket for QA or demo purposes. Why? Because the development environment gets overwritten with every new PR. I know what you're thinking but so far so good, we've not had any two engineers open PRs simultaneously (at the same time) on the same repo. But if they did, may the best (wo)man win.


Given the volatility of the development environment, **Hold Dev** becomes useful when you need to ensure that no one overwrites your changes while your work is being QA'ed. In most cases, given the team size, QA can be done on the development server.


###### Environment - Staging


Our default branch is staging. The setup for staging is identical to that of development, however, there is the added certainty/assurance that whatever is in staging is stable (or so we think). Staging at any point in time is our most up to date and "stable" version, containing all the recent work completed by the team. Internally, staging is the most used environment. This is where engineers can have account balance in millions! Now that I think about it, I don't have an account on staging (I need one). For extra added caution, every merge to staging runs our unit tests, deploys to the staging environment, and runs the integration tests against the newly deployed instance.


###### Environment - Production


Our goal with the pipeline is to increase our confidence in the stability of the platform for our customers, thus the need to run all tests twice - call that 2FA- (development and integration) before merging into production. Despite the integration tests that assure us that our core functionalities are stable, production environment uses ***Sentry*** to track any unexpected or unseen bug that makes its way through to production. Retrospectively, building our pipeline was in an effort to reduce the number of errors Sentry reported weekly, while also cutting down on the back and forth with sales and operations.


> Build relatively fast, don't break too many things


## Conclusion


At SEND, we want to build and keep building. In our attempt to build amazing products, we needed to ensure that those products work as expected when we release it to our customers. Manual testing was not feasible given the size of the team, the speed with which we are building, and the product lines we support. Thus increasing our confidence level when we release updates became a paramount concern.


One issue we tried to fix was overwriting changes on the development environment. But solutions around that were too complicated or convoluted, given the size of the team, it was not a worthy exploit. Given our current setup, we have seen a dip in the bugs reported, and can focus our energy on building new features and/or product lines.
