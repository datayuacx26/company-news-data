---
schema_version: "1.0.0"
document_id: "a84c76751aa3785d9f7bb70fcb9939589ce298548e2c6411bcd17801d8cd15af"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/simple-dev-focused-workflow-for-ai-evals"
published_at: "2024-05-03T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:269f2795c408ce2107f35f813727b0ba964935e1c765c37860519bc0a27bd20f"
---

# Simple, dev-focused workflow for AI evals

May 3, 2024


[Simple, dev-focused workflow for AI evals](https://www.openlayer.com/changelog/simple-dev-focused-workflow-for-ai-evals)


Most of us get how crucial AI evals are now. The thing is, almost all the eval platforms we’ve seen are clunky – there’s too much manual setup and adaptation needed, which breaks developers’ workflows.


Last week, we released a radically simpler workflow.


You can now connect your GitHub repo to Openlayer, and every commit on GitHub will also commit to Openlayer, triggering your tests. You now have continuous evaluation without extra effort.


You can customize the workflow using our CLI and REST API. We also offer template repositories around common use cases to get you started quickly.


You can leverage the same setup to monitor your live AI systems after you deploy them. It’s just a matter of setting some variables, and your Openlayer tests will run on top of your live data and send alerts if they start failing.


We’re very excited for you to try out this new workflow, and as always, we’re here to help and all feedback is welcome.


## Features


- •


Integrations


Developer workflow (GitHub integration, CLI and REST API, Sample repositories for various workflows, Ability to clone sample repositories in Openlayer UI)
- •


Evals


New test: column A grouped by column B


## Improvements


- •


UI/UX


Move test options to header bar in modals
- •


UI/UX


Improvements to test results modals
- •


UI/UX


Improve layout of workspace onboarding
- •


UI/UX


Ability to delete tests
- •


Evals


Relevant tests created automatically upon project creation in onboarding
- •


UI/UX


Polished design of in-app callouts
- •


UI/UX


Polish to activity log
- •


Documentation


Reorganization of docs
- •


API


Allow None values in token column


## Fixes


- •


UI/UX


Row outputs in panel are injected into chat history format when they should not be
- •


UI/UX


Row panel dropdowns do not appear when opened from a test modal
- •


UI/UX


Monitoring graphs showed no recent results even when there were some
- •


UI/UX


Opening create test modal for Group by Column test crashed the app
- •


UI/UX


Column parameters was not able to be changed for Group By tests
- •


Platform


Creating a commit without a model breaks
- •


UI/UX


Project filtering did not work in overview page
- •


UI/UX


Creating Character Length tests runs into client-side error when there are no input variables
- •


UI/UX


Client-side exception when opening requests
