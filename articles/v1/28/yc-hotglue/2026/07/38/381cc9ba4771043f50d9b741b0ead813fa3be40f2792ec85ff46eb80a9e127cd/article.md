---
schema_version: "1.0.0"
document_id: "381cc9ba4771043f50d9b741b0ead813fa3be40f2792ec85ff46eb80a9e127cd"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/hotglue-melt-february-2024"
published_at: null
first_seen_at: "2026-07-23T23:13:49.909100+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:2b48c383b5d40fb35de24a05a2bbe8eef9892fdf50953e94484335293188a93d"
---

# hotglue melt: February 2024

Hey everyone! Welcome to our monthly hotglue melt. Here’s a rundown of the hotglue features you may have missed.


## Faster jobs ⏩


Over the past few weeks, our backend team has pushed some big changes to help make jobs faster. We’ve seen job time savings of anywhere from 40 seconds to over 3 minutes, and there’s more to come. The improvements to date include:


- **ETL Dependency Caching** : Jobs now only update and install ETL dependencies when necessary.
- **Asynchronous Job Status Webhooks** : Jobs no longer wait for webhook responses. If your webhook endpoint takes a while to respond, you’ll notice big gains here.
- **Dynamic Installation of Database Drivers** : hotglue now intelligently installs database drivers, only when required.
- **Refined Python Venv Inits** : We’ve refactored how we initialize virtual environments, to remove unnecessary overhead from the runtime.


Next steps: We’re working through individual connectors to add more concurrency, and adding some additional optimizations to the job executor.


## Redesigned on-premise connectors (Sage 50 UK and Quickbooks Desktop)


Sage 50 UK and Quickbooks Desktop are installed executables available on our Basic Plan and above. They can now be linked in under a minute and managed remotely, with:


- JWT-secured password authentication
- Remote logging
- Auto-updates with the hotglue launcher
- Remote config and state management


Here’s a[demo](https://www.loom.com/share/8801a703e72f4da29b993c07ceb2f0dd?sid=529e8201-63c0-4acb-ac10-8cd6aa24c12b) of how this works for QBD.


## New Integrations �


Over the past few weeks, our integrations team contributed three new connectors to hotglue’s library and the Singer community:


- Linnworks (tap), built by Vinnie
- Precoro (target), built by Keyna
- Sellercloud (tap), built by Adil


Our customer[Plentive](https://www.plentive.com/) also contributed four new Singer connectors, which are all available to use in any hotglue environment:


- BQE
- VantagePoint
- Quickbooks Time
- Crelate


Lastly, we improved some existing open-source connectors and added them to the hotglue library:


- Redshift (tap), originally built by[Monad](https://github.com/Monad-Inc/tap-redshift)
- LinkedIn Ads (tap), originally built by Singer.io
- Clickhouse (target), originally built by Shaped.ai


## Fixes and improvements �


Frontend:


- The billing page now shows your invoices for download, and you can assign billing page access to multiple user accounts
- The job download process runs faster now, and no longer requires pop-ups to be enabled.
- You can now “clone” connectors across the same environment
- Jupyter notebooks run on Python 3.10 if you use our V3 job executor.
- The widget custom mapping tab text can now be customized


Jobs and stability:


-


A new environment setting to retrigger jobs automatically when they hit memory limits


-


When a catalog’s size surpasses AWS API Gateway’s max limits, it is now trimmed to a “lite” version in API responses. The full catalog is still used by syncs and ETL scripts.


-


Added code coverage and mock classes for Salesforce Apex triggers and classes


-


Google sheets discovery is now up to 12x faster in the widget.


-


We added new CLI functions and API endpoints, including:


- CLI: \`hotglue tenants custom-etl\` to find tenants with forked ETLs
- API: \`PATCH /linkedConnectors\` for updating v2 flow configs


Got ideas? Shoot us a message in Slack or send us suggestions athello@hotglue.xyz .


See you next month! 👋


TABLE OF CONTENTS


- Faster jobs ⏩
- Redesigned on-premise connectors (Sage 50 UK and Quickbooks Desktop)
- New Integrations �
- Fixes and improvements �


RECOMMENDED BLOGS


[hotglue melt: 2023 Feature Roundup This is your 2023 hotglue feature update!](https://hotglue.com/blog/hotglue-melt-2023-Feature-Roundup)


[hotglue 101 Learn about the power of seamless integrations for your SaaS app by offering easy customer facing integrations quickly with hotglue.](https://hotglue.com/blog/hotglue-101)


[How to build a database integration for your SaaS app and why you need one Learn how to build a database integration, from collecting and validating credentials to syncing relevant tables and fields.](https://hotglue.com/blog/how-to-build-a-database-integration-for-your-saas-app-and-why-you-need-one)
