---
schema_version: "1.0.0"
document_id: "583db83b43391f0ad247032bb8669179f9453c45721a0c2d98c3c1033f3583f2"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/custom-metrics-rotating-api-keys-and-new-models-for-direct-to-api-calls"
published_at: "2024-07-01T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:e2b2fb617d04bce0314192e9ef36ae509c66e5c319fb7ca06883b997a14544ce"
---

# Custom metrics, rotating API keys, and new models for direct-to-API calls

July 1, 2024


[Custom metrics, rotating API keys, and new models for direct-to-API calls](https://www.openlayer.com/changelog/custom-metrics-rotating-api-keys-and-new-models-for-direct-to-api-calls)


We understand that you may have metrics that are highly specific to your use case, and you want to use these alongside standard metrics to eval your AI systems. That’s why we built custom metrics. You can now upload any custom metric to Openlayer simply by specifying them in your openlayer.json file. These metrics can then be used in a number of ways on platform; they’ll show up as metric tests that you can run, or as project-wide metrics that will be computed on all of your data. Now, your evals on Openlayer are more comprehensive than ever.


We’ve shipped more exciting features and improvements this month, including the ability to create multiple API keys, and a bunch of new models available for direct-to-API calls, so be sure to read below for a full list of updates!


## Features


- •


Evals


Custom metrics (Upload your own custom metrics to Openlayer, which can be used: as project-wide metrics, as tests)
- •


API


Create multiple Openlayer API keys (create new personal Openlayer API keys so that you can rotate API keys, rename and delete keys)
- •


API


Specify desired metrics in openlayer.json
- •


API


New models available for direct-to-API calls (GPT-4o, GPT-4 Turbo, Claude 3.5 Sonnet, Claude 3 Haiku, Claude 3 Opus, Claude 3 Sonnet, Command R, Command R Plus, Gemini 1.0 Flash, Gemini 1.5 Flash, Gemini 1.5 Pro


## Improvements


- •


UI/UX


Test creation page design improvements
- •


Integrations


Link to git repository and organization in git settings pages
- •


UI/UX


Add button to view status of commit during processing in project loading state
- •


UI/UX


Updated solid danger buttons’ shade of red
- •


UI/UX


Modal background overlay opacity is no longer too light
- •


UI/UX


Toast messages no longer overflow the page
- •


UI/UX


Improved text sizing in various places
- •


UI/UX


Added error toast when Assistant requests fail
- •


UI/UX


Navigation polish
- •


UI/UX


Different icons for different commit sources
- •


UI/UX


Suggested titles for GPT evaluation tests now reference the criteria name
- •


SDKs


Improvements to docs (updated Python code snippets with the new SDK syntax, tracing for Anthropic models, updated example notebook links)


## Fixes


- •


UI/UX


Commit log processing time does not use relative time
- •


Collaboration


New users that were invited to a workspace do not auto-navigate to invites page
- •


UI/UX


Help breadcrumb is hidden and shows in place of user dropdown options
- •


UI/UX


Cost values close to 0 rendered as $0.00
- •


UI/UX


Progress bars did not render in chrome
- •


UI/UX


Test metadata disappeared entirely when collapsed
- •


UI/UX


Navigating to project from breadcrumb prevents back navigation
- •


UI/UX


Switching projects prevented back navigation
- •


UI/UX


Creating commit from the UI does not generate outputs
- •


UI/UX


Commit processing icon was broken in navigation dropdowns
- •


UI/UX


Activity log overflows screen
