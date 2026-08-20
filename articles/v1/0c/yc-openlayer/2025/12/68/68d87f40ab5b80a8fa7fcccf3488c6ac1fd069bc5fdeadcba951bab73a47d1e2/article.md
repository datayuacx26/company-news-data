---
schema_version: "1.0.0"
document_id: "68d87f40ab5b80a8fa7fcccf3488c6ac1fd069bc5fdeadcba951bab73a47d1e2"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/pausing-tests-checks-for-duplicate-keys-and-new-integrations"
published_at: "2025-12-23T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T22:24:50.551214+00:00"
content_hash: "sha256:c7ce8e5f90c0b840d26c42706f1d4eefdbcf8b0269f623fd41285c3cdd870a5f"
---

# Pausing tests, checks for duplicate keys, and new integrations

December 23, 2025


[Pausing tests, checks for duplicate keys, and new integrations](https://www.openlayer.com/changelog/pausing-tests-checks-for-duplicate-keys-and-new-integrations)


This month, we shipped a range of improvements across Openlayer, including new integrations, tests, and developer features. We’re also now available on the AWS, Azure, and Google Cloud marketplaces, and we’ve added support for evaluating multi-agent systems built with Google’s Agent Development Kit. Read on for the full list of updates and enhancements.


## Features


- •


Platform


Added column distribution graphs for LLM projects
- •


SDKs


New integration with Google ADK
- •


Platform


Allow pausing test execution in Monitoring mode
- •


CLI


Added command to export tests for a project
- •


Platform


New test to check for duplicate unique and primary keys
- •


Integrations


Connect to Snowflake views and run tests via remote execution
- •


Platform


Run custom SQL tests joining multiple tables
- •


Security


Allow specifying custom certs as environment variables to access external services
- •


On-Prem


Support Azure managed identity for storage connections
- •


Platform


Openlayer available on AWS, Azure, and Google Cloud marketplaces


## Improvements


- •


Docs


New section in docs describing how to manage environment variables
- •


Platform


Allow defining specific columns to check for nulls in Null rows tests
- •


Platform


Improved Directory Sync Race Conditions Around Membership Creation
- •


UI/UX


Improved empty state for graphs throughout the app
- •


UI/UX


UI improvements to various components, including tags, multi-selects, and toggle button groups
- •


UI/UX


UI improvements to table display options
- •


UI/UX


Improved design of multi-select components
- •


Docs


Improved SAML docs page
- •


Platform


Assign static IPs to Openlayer servers for easy allowlistin


## Fixes


- •


UI/UX


Render higher decimal precision for test result values
- •


Platform


Handled failing gracefully on non-pandas custom metrics
- •


Platform


Missing insights for specific test results were erroring the data source
- •


Security


Lock down SAML SSO logins when directory sync enabled
- •


API


Resolved preventing sending empty request body when creating a secret
- •


On-Prem


Fixed nginx image name
- •


UI/UX


Performance improvements and fixes to monitoring test page that allow you to more easily view all historical test results
- •


UI/UX


Resolved monitoring mode set up test not switching status correctly
- •


UI/UX


Fixed project frameworks table showing frameworks outside selected project
- •


UI/UX


Fixed test result details not rendering in development mode
- •


API


Resolved OTel endpoint errors
- •


UI/UX


Resolved result chip rendering as unavailable in test page
- •


Platform


Resolved error retrieving production data metrics
- •


Integrations


Resolved Slack integration errors
- •


Platform


Resolved SSO directory sync bugs
- •


API


Resolved pagination issue with listing orgs
- •


UI/UX


Environment variable naming consistency in UI
- •


CLI


Resolved CLI profile login not overwriting existing profile
- •


UI/UX


Resolved test result data table not stretching to height
- •


Integrations


Resolved query hitting BigQuery's complexity limit
- •


Platform


Validate timestamp column name exists in table
- •


UI/UX


Resolved navigating to and back from or deleting a rule under framework navigation issues
- •


Platform


Several minor API and UI bugs and improvements
