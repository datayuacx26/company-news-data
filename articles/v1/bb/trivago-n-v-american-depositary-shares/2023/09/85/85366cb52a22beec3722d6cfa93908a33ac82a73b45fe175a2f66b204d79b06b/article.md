---
schema_version: "1.0.0"
document_id: "85366cb52a22beec3722d6cfa93908a33ac82a73b45fe175a2f66b204d79b06b"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2023-09-27-end-to-end-tests-retry-strategies/"
published_at: "2023-09-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:b361a4f1f6636ac8b18ba78555e6f1ed2f9bbe644eab1dbc56a4b8dbfdc638f5"
---

# End-to-end tests retry strategies

End-to-end tests retry strategies


## Retry on failure, good or bad?


Why should you retry all tests on failure? Why not? This article will not go into details, listing pros and cons of each approach. There are already enough[resources on the Web](https://mrslavchev.com/2023/01/04/retry-harder-why-rerunning-tests-is-a-bad-idea/) about the topic, listing valid points for both opposing views. As trivago Hotel Search frontend QA team over the last years we tried to stay away from a brute-force retry policy for failures and we rather tried to execute test retries only in selected cases. Recently, when we switched to a Continuous Deployment approach for our[new frontend Web application](https://tech.trivago.com/post/2022-05-16-warp-a-web-application-rewrite-project) (which empowers developers to merge and release some pull requests autonomously), we faced a greater need than before for understandable and stable test results. Due to that, showing as few “red flags” as possible for the automated checks on pull requests became even more important to ensure enough confidence in test results and to avoid slowing down the software development life cycle. The requirements and the balance between deterministic results and success ratio shifted, at least in some cases.


## Our different retry strategies


As our new Web application repository is on GitHub and we run the end-to-end automated tests for it in GitHub Actions, we approached the new requirements by having different retry strategies for the different test suites or workflows. They are:


- Retry anything on failure, for any cause -> Implemented on the “core” tests running on each commit. The reason for retrying in any case is to provide increased confidence on pull requests, without causing too much effect on the feedback cycle. The tests are few and can be executed quickly and cheaply a second time.
- Filtered retry on failure, based on specific failure causes -> Implemented on the “extended” tests, leveraging the rerun-detector plugin part of our test automation framework. That’s a plugin to compare test failures against a custom list of exceptions (e.g. environment issues) in order to trigger them for a second rerun phase. An example might be encountering an empty result list on a search for hotels, assuming it might be caused by a backend outage. We considered that some failure causes that belong exclusively or mostly to external factors, should be a valid reason to retry the same test at the end, when all other tests were run. Examples of exceptions substrings can be:` Session timed out or not found` ,` Empty page detected` ,` Error communicating with the remote browser` and many others.
- Manual retry of a scenario subset -> The latest arrow added to our quiver. It’s also a feature belonging to the “extended” tests execution and it simplifies identifying real failures, while giving our QA engineers more control on test execution times and resources usage. We’ll dig into details in the next paragraphs.


## A look at the trupi-extended workflow in GitHub Actions


Some background on how our tests are executed is now needed. The automated tests for our Web platform are executed by an internally developed framework called trupi. Trupi is based on Selenium and Cucumber, and it’s written in Java. Our test scenarios are stories written in Cucumber. We have some “core” tests that run whenever a new pull request is opened, and on every subsequent push event, directly in the CI workflow, and they are identified by a custom[Cucumber tag](https://cucumber.io/docs/cucumber/api/?lang=java#tags) “@core”. Besides those core tests that are testing the most frequent user flows and ensure that the core functionalities are behaving correctly, we have “extended” suites that are far larger and thus slower to run. Their execution is triggered manually, with a chatbot-style approach. The decision if and when to run such tests is left to QA engineers, or developers, but it’s usually happening at least once - after the code review is completed and possibly before extensive exploratory testing.
Leaving a comment that starts with the text` /trupi-extended` on a pull request will trigger the execution of those extended tests.


At the moment, within the trupi-extended workflow there are three different jobs, using a matrix strategy, that run the tests respectively in Chrome desktop, Chrome mobile (mobile emulation mode) and Chrome without JavaScript enabled. Each one can handle different test suites from the same folder, choosing individual tests based on tag expressions.


The outcome of the extended tests run is then added as a new comment on the same pull request, with info about the failure or success and links to the related test reports. For better feedback, comments are actually added and modified incrementally: The execution adds a first comment on execution start, then, on success or failure, replaces it and reacts with emojis to the original triggering comment. The command has optional arguments to execute only subsets, for example only “desktop”, or to apply certain variations while executing the tests. The latest addition has been a “failures” argument that enables manually rerunning the jobs that had failures on a previous run, by only picking those failed tests for execution.


## Test execution times and the flakiness problem


Even though we constantly apply optimization strategies to several aspects of the testing setup and to each test scenario, all end-to-end test suites are plagued by[some flakiness](https://testing.googleblog.com/2020/12/test-flakiness-one-of-main-challenges.html) . That’s when tests might pass or fail on different runs, even though code didn’t change. A 98% or 99% success rate could be considered good on paper for Selenium tests, but it’s clearly not as good as real stability. Getting even just 1 or 2 failed tests out of a large amount like 300 or 400 leaves some sort of doubt and it’s not as good as having a full success and green check. What happens when a few failures are present? The individual decision might be to run the whole suite again. Running our extended suite of hundreds of tests one more time takes around 10 minutes, delaying release, consuming more resources and hence also causing extra costs. On top of that, it doesn’t guarantee that some other test will not run into environment issues or some different random failure cause. Facing this challenge, we looked for a solution that could enable developers and QA engineers to have a faster and more accurate feedback cycle in such circumstances.


## How to rerun failed tests semi-automatically


I once gave a presentation titled “Test semi-automation”, focused on the Chrome browser extensions that I coded to support myself and the team in exploratory testing. Thinking about the “automation” topic, as long as AI doesn’t completely take over the whole testing and feedback cycle (if it will ever manage to), I still believe that the most significant part in testing is human. Automation tools are just that, tools that can help do our testing job faster and better. When we thought about how to improve the experience with automation, specifically with the long-running trupi-extended workflow, we chose to take a step backwards and semi-automate. Semi-automation here means to require a further deliberate human action in the process, while keeping the experience simple and fast.


We had a first problem to solve: as we use the open-source[Cucable](https://tech.trivago.com/post/cucable-plugin) plugin for parallelization of the test execution, developed by our fellow Test Automation Engineer[Benjamin Bischoff](https://www.softwaretester.blog/) , the resulting “sliced” runners were no longer based on the original feature files and we weren’t able to list where the original Cucumber scenarios were located in our feature files. Benjamin came to the rescue and released a new version of Cucable,[1.10.0](https://github.com/trivago/cucable-plugin/releases/tag/v1.10.0) , that provides exactly that, as it creates a` generated-features.properties` file that stores all generated feature names and their reference to the respective source feature. The content is something like:


```text
Calendar_scenario001_run001_IT=src/test/resources/features/search-form/Calendar.feature:124
GuestSelector_scenario003_run001_IT=src/test/resources/features/search-form/GuestSelector.feature:31
```


The following question was how to handle the failure list that we would have at the end of the test job run. Since custom data is not persisted when a GitHub Actions workflow run is over, and considering that the workflow always runs from the main branch, hence making the sequence nonlinear (all pull requests have trupi-extended runs identified by the same` main` branch), we thought about a couple of options:


- Attach the resulting txt file(s) as workflow run artifact and retrieve it from the “retry failures” run with a download action.
- Push the resulting txt file(s) to a Google Cloud Storage (GCS) bucket and download from there.


Both of the options listed above would actually need some steps to identify the ID of the previous run or a unique identifier for GCS, before downloading the list(s). Overall, several additional steps would be needed. Therefore we thought about a third, possibly simpler, option:


- If failures happened, each tests job in our workflow will parse the failed_scenarios.txt content. Then it would remove any duplicates that could have been created by failures of[Scenario Outline](https://cucumber.io/docs/gherkin/reference/#scenario-outline) examples, as they are different tests but have the same location in the Cucumber feature file. Finally it would add a hidden comment section wrapped in` <!-- -->` to the outcome comment.


That approach has several advantages:


- The data persists where it is needed: in the pull request.
- It can be retrieved by simply looking at the last comment whose body includes a specific text like “tests executed with result: FAILED”.
- It could even be manually checked or edited if needed.


Then we added the actual rerun of failures. By using a “failures” argument with the command, i.e.` /trupi-extended failures` , the workflow will not create the usual default matrix of three jobs, but it will instead read the hidden section in the last existing comment with a failure message. It will look for specific keys identifying the failures lists (e.g. “mobile_failed=“ etc.) delimited by a separator “;”, collect the lists for each key and based on that create a matrix of variable size, 1 to 3 jobs, feeding each one with the appropriate list of scenarios to execute.


The outcome comment will then also reflect that only previously failed tests were executed and it will also point to the previous run id for reference. There is no limitation to also comment /trupi-extended failures once more and rerun again recursively, but hopefully at that point any flaky test would have passed. Real failures would be evident, if failing with the same exception on a second try.


## Conclusion


Our retry strategies for end-to-end tests differ based on requirements of the test suite and execution context. Our latest addition of a manual retry solution for failures helped us to find a valuable compromise between a “retry everything in any case” approach and a full manual re-run of the whole test suite. Our QA engineers can now evaluate the situation by themselves and most of the time get full success of the largest set of automated tests by writing another comment and waiting an additional couple of minutes.


Did we solve all of our issues? Nope, as of course flakiness and environment issues will keep existing and sometimes trouble us, even though we address them on a daily basis, as briefly mentioned[in a previous article](https://tech.trivago.com/post/2023-02-15-how-we-improved-reporting-and-monitoring-of-test-automation-results) . Besides the constant observation of test results and their maintenance, the different retry approaches employed increase our confidence before merging something, which is then directly released to production through our CD process and presented to our millions of users.
