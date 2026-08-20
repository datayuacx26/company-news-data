---
schema_version: "1.0.0"
document_id: "f91e1890ac15fe40203f505a414125cfb4e6fb3e7c70d99c45f99f7ca4010c21"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/go-deep-on-test-result-history-and-add-multiple-criteria-to-gpt-evaluation-tests"
published_at: "2024-02-01T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:371b70c34dd1365d32fbc1120d53ed03978cdc7771271b762f89b8323f7896dd"
---

# Go deep on test result history and add multiple criteria to GPT evaluation tests

February 1, 2024


[Go deep on test result history and add multiple criteria to GPT evaluation tests](https://www.openlayer.com/changelog/go-deep-on-test-result-history-and-add-multiple-criteria-to-gpt-evaluation-tests)


You can now click on any test to dive deep into the test result history. Select specific date ranges to see the requests from that time period, scrub through the graph to spot patterns over time, and get a full picture of performance.


We’ve also added the ability to add multiple criteria to GPT evaluation tests. Let’s say you’re using an LLM to parse customer support tickets and want to make sure every output contains the correct name, email address and account ID for the customer. You can now set a unique threshold for each of these criteria in one test.


## Features


- •


UI/UX


Scrub through the entire history of test results in the individual test pages (See which requests were evaluated per each evaluation window, See results and requests from a specific time period)
- •


Evals


Improved LLM-as-a-judge test (Add multiple criteria to a single test, Choose how you want each row to be scored against the criteria: on a range from 0-1, or a binary 0 or 1)


## Improvements


- •


Performance


Bolster backend server to handle higher loads
- •


UI/UX


Table headers no longer wrap
- •


UI/UX


Null columns hidden in data table
- •


UI/UX


Test metadata moved to the side panel so that the test results graph and data are viewed more easily
- •


UI/UX


Skipped test results are rendered with the most recent result value
- •


UI/UX


Test results graph height increased in the page for an individual test
- •


UI/UX


Date labels in tests results graph improved
- •


Performance


Only render rows that were evaluated for GPT metric threshold tests
- •


UI/UX


Test card graphs no longer fla


## Fixes


- •


UI/UX


Results graph was not sorted correctly
- •


UI/UX


Test results graph did not overflow properly
- •


UI/UX


Test results graph did not render all data points
- •


Platform


Empty and quasi-constant features test creation was broken
- •


UI/UX


Undefined column values now rendered as null
- •


UI/UX


Most recent rows were not being shown by default
- •


UI/UX


Label chip in results graphs for string validation tests was not inline
- •


UI/UX


Test results were not rendering properly in development mode
- •


UI/UX


Plan name label overflowed navigation
- •


UI/UX


Buttons for exploring subpopulations was active even when no subpopulations existed
- •


UI/UX


Results graph rendered loading indicator even after networking completed for skipped evaluations
- •


UI/UX


Rows for the current evaluation window were not rendered in test modals
- •


UI/UX


Commit, metrics, and other tables were not rendering rows
- •


UI/UX


Duplicate loading and empty placeholders rendered in monitoring mode
