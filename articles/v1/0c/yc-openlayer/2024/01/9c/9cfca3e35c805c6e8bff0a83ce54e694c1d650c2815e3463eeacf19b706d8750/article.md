---
schema_version: "1.0.0"
document_id: "9cfca3e35c805c6e8bff0a83ce54e694c1d650c2815e3463eeacf19b706d8750"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/cost-per-request-new-tests-subpopulation-support-for-data-tests-and-more-precise-row-filtering"
published_at: "2024-01-17T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:5a3b71e5b93e2d02c29b7f111b632528e3f10410c7cca7db9200d128ad6b3eac"
---

# Cost-per-request, new tests, subpopulation support for data tests, and more precise row filtering

January 17, 2024


[Cost-per-request, new tests, subpopulation support for data tests, and more precise row filtering](https://www.openlayer.com/changelog/cost-per-request-new-tests-subpopulation-support-for-data-tests-and-more-precise-row-filtering)


We’re excited to introduce the newest set of tests to hit Openlayer! Make sure column averages fall within a certain range with the Column average test. Ensure that your outputs contain specific keywords per request with our Column contains string test, where the values in Column B must contain the string values in Column A. Monitor and manage your costs by setting Max cost, Mean cost, and Total cost tests.


As additional support for managing costs, we now show you the cost of every request in the Requests page.


You can now filter data when creating integrity or consistency tests so that the results are calculated on specific subpopulations of your data, just like performance goals.


That’s not all, so make sure to read all the updates below. Join our Discord community to follow along on our development journey, and stay tuned for more updates from the changelog! 📩🤝


## Features


- •


Evals


New tests (Column average test – make sure column averages fall within a range, Cost-related tests – max cost, mean cost, and total cost per evaluation window) Column contains string test – column B must contain the string in column A)
- •


Platform


View your production data associated with each of your tests in monitoring mode
- •


Observability


Support for cost-per-request and cost graph
- •


Platform


Filter rows by row-level metrics such as conciseness
- •


Evals


Subpopulation support for data goals
- •


UI/UX


The timeline page is back - see how your commits perform on goals over time


## Improvements


- •


Platform


Ability to update previously published production data by setting existing columns or adding new columns
- •


Performance


Sample requests are paginated
- •


Performance


Latency rendered in ms in the requests table
- •


UI/UX


Requests filters no longer require selecting a filter type
- •


UI/UX


Suggested tests modal auto-opens after project creation outside of the onboarding
- •


UI/UX


Notifications callout not shown until the project is fully setup
- •


UI/UX


Enabled filtering without datasets in development and monitoring modes
- •


Performance


Render cost in requests table
- •


Performance


Render monitoring data correctly in test diagnosis modals
- •


Evals


Row-level scores and explanations rendered for gpt-based metric tests
- •


UI/UX


Activity log is now collapsible
- •


UI/UX


Individual rows in data tables within the test diagnosis modal can be expanded
- •


UI/UX


Input and output columns rendered next to each other in data tables
- •


SDKs


New example notebook showing how to send additional columns as metadata with the monitor
- •


SDKs


Cleaned up example notebooks


## Fixes


- •


UI/UX


Irrelevant reserved columns no longer presented in requests table
- •


UI/UX


Column filtering did not dismiss in requests page
- •


UI/UX


Button to create commit from UI was rendered for non-LLM projects
- •


Platform


Navigating back from certain pages was broken
- •


UI/UX


Dismissing modals caused the app to become unresponsive
- •


UI/UX


Monitoring onboarding modal did not open
- •


Performance


Production tests with subpopulation filters rendered incorrect insights in results graph
- •


UI/UX


Clicking outside of dropdowns within a modal dismissed the whole modal
- •


UI/UX


Improved discoverability of the data points that a test is run on in test diagnosis modal
- •


UI/UX


Subsequent pages of monitoring requests would not always render
- •


UI/UX


Some rows contained latency, cost, and tokens columns even if they were left unspecified
- •


UI/UX


Suggested test modal reappeared unexpectedly
- •


UI/UX


When table columns are very large, other columns were not readable
- •


UI/UX


LLM rubric tests did not show score or explanations in monitoring
- •


UI/UX


Requests pane was not scrollable
- •


UI/UX


Some error states for test creation and results weren’t being shown
- •


UI/UX


Column Value test title was not updating upon threshold change
- •


UI/UX


Default color scheme to system
- •


SDKs


Added new and updated existing examples of how to incorporate the Openlayer TypeScript client for various use cases
- •


UI/UX


Data table columns no longer cut off
