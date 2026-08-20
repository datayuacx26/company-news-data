---
schema_version: "1.0.0"
document_id: "12a126f479156dfd4cd8cdc1e90739373ead494276593047f459ab3a09fe5d36"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/log-multi-turn-interactions-sort-and-filter-production-requests-and-token-usage-and-latency"
published_at: "2023-12-21T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:092ffc1f6d83be6a71af9dd7ff62930f03ff7229e44454a7020206d74247295c"
---

# Log multi-turn interactions, sort and filter production requests, and token usage and latency graphs

December 21, 2023


[Log multi-turn interactions, sort and filter production requests, and token usage and latency graphs](https://www.openlayer.com/changelog/log-multi-turn-interactions-sort-and-filter-production-requests-and-token-usage-and-latency)


Introducing support for multi-turn interactions. You can now log and refer back to the full chat history of each of your production requests in Openlayer. Sort by timestamp, token usage, or latency to dig deeper into your AI’s usage. And view graphs of these metrics over time.


There’s more: we now support Google’s new Gemini model. Try out the new model and compare its performance against others.


⬇️ Read the full changelog below for all the tweaks and improvements we’ve shipped over the last few weeks and, as always, stay closer to our development journey by joining our Discord!


## Features


- •


Observability


Log multi-turn interactions in monitoring mode, and inspect individual production requests to view the full chat history alongside other meta like token usage and latency
- •


UI/UX


Sort and filter through your production requests
- •


Observability


View a graph of the token usage and latency across all your requests over time
- •


Integrations


Support for Gemini is now available in-platform: experiment with Google’s new model and see how it performs on your tests
- •


Evals


View row-by-row explanations for tests using GPT evaluation


## Improvements


- •


SDKs


Expanded the Openlayer TypeScript/JavaScript library to support all methods of logging requests, including those using other providers or workflows than OpenAI
- •


UI/UX


Improved commit selector shows the message and date published for each commit
- •


UI/UX


New notifications for uploading reference datasets and data limits exceeded in monitoring mode
- •


Collaboration


Only send email notifications when test statuses have changed from the previous evaluation in monitoring
- •


Templates


Added sample projects for monitoring
- •


UI/UX


Enhancements to the onboarding, including a way to quickstart a monitoring project by sending a sample request through the UI
- •


UI/UX


No longer navigate away from the current page when toggling between development and monitoring, unless the mode does not apply to the page
- •


UI/UX


Allow reading and setting project descriptions from the UI
- •


UI/UX


Update style of selected state for project mode toggles in the navigation panel for clarity
- •


UI/UX


Clarify that thresholds involving percentages currently require inputting floats
- •


Platform


Allow computing PPS tests for columns other than the features
- •


UI/UX


Test results automatically update without having to refresh the page in monitoring mode
- •


UI/UX


Add dates of last/next evaluation to monitoring projects and a loading indication when they recompute
- •


UI/UX


Surface error messages when tests fail to compute
- •


UI/UX


Add callouts for setting up notifications and viewing current usage against plan limits in the navigation
- •


UI/UX


Graphs with only a single data point have a clearer representation now
- •


UI/UX


Improvements to the experience of creating tests with lots of parameters/configuration
- •


UI/UX


- •


UI/UX


Add alert when using Openlayer on mobile
- •


UI/UX


Default request volume, token usage, and latency graphs to monthly view


## Fixes


- •


UI/UX


Title suggestions for certain tests during creation were unavailable or inaccurate
- •


UI/UX


Fixes to test parameters, including incorrectly labeled and invalid options
- •


UI/UX


Certain LLM tests would not allow selecting target columns that are not input variables
- •


UI/UX


Code in development onboarding modals was not syntax highlighted
- •


UI/UX


Create test card content would overflow improperly
- •


UI/UX


Sample projects would not show button for creating suggested tests after some were created
- •


UI/UX


Graphs in monitoring test cards were cut off
- •


UI/UX


Requests table would break when rows were missing columns
- •


UI/UX


Full-screen onboarding pages would not allow scrolling when overflowed
- •


UI/UX


Options were sometimes duplicated in heatmap dropdowns
- •


UI/UX


Thresholds would not faithfully appear in test result graphs
- •


UI/UX


Skipped evaluations would not appear in test result graphs
