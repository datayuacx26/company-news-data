---
schema_version: "1.0.0"
document_id: "ea5a77712c810ef5efd1c495acd1089a9132990360787376ddad1dbff394ec1a"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/bits-release/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:9f09b268f70f191a234a0137f9658f895319d763994499ccae6f24352987c401"
---

# Ship code safely at AI speed with Bits Release

Ala Shiban


Guillaume Turbat


A huge portion of pull requests (PRs) already come from coding agents, and the trend is only accelerating. In the first few months of 2026, the number of AI-attributed commits in public GitHub repos surged[from just under two million to an estimated 37 million per month](https://botcommits.dev/) . Teams are merging more code now than ever, but traditional staging environments, written tests, and human review were not built to support deployments at this volume or velocity.


[Bits Release](https://www.datadoghq.com/product-preview/bits-release/) , now available in Preview, is a release validation and rollout intelligence system that automatically follows every change from PR to production. It generates a validation plan from each PR, runs the plan continuously by using existing Datadog telemetry data and[Bits Testing](https://www.datadoghq.com/blog/bits-testing-test-coverage/) , and surfaces evidence in the places engineers already work: PR comments and Slack. When a release fails in production,[Bits Code](https://docs.datadoghq.com/bits_ai/bits_ai_dev_agent/) investigates the regression and opens a PR fix.


In this post, we’ll explore how Bits Release helps teams:


-Track every change from PR to production


-Safely roll out features in production


-Detect silent production failures before users report them


-Automatically investigate and fix issues


-Turn temporary validation checks into permanent guardrails


## Track every change from PR to production


Most engineering teams rely on a handful of disconnected tools to answer the same basic questions about a PR: Where is it deployed? Is it working as expected? Did it cause any regressions? PRs live in GitHub, deployment status sits in a CI/CD system, and rollout exposure runs through a feature flag platform. Validation signals, meanwhile, are scattered across CI, review, QA, logs, metrics, traces, synthetic tests, and Slack channels. Bits Release replaces that fragmentation with a single, continuous workflow that tracks every change from the moment a PR opens.


### Analyze the PR and generate a validation plan


Consider a developer working on Couch Cache, an ecommerce site. The engineer uses a coding agent to add an AI shopping assistant chat widget that recommends products to customers, and opens a PR.


The feature works locally, but the engineer has no way to know how it will behave under real production traffic.


Bits Release analyzes the PR and builds a validation plan based on what the change is supposed to do. For example, in the Couch Cache PR, it detects that the developer has added an AI shopping assistant designed to help customers find couches through chat. Once the PR is deployed, Bits Release interacts with the assistant like a shopper would and checks to determine whether the assistant returns useful recommendations via Bits Testing. It also verifies from live telemetry data that:


-


The landing page still loads properly.


-


The` shop-front` and` product-catalog` services remain healthy.


-


Shoppers continue moving through the browse-to-checkout funnel.


The plan also changes based on the changes in the PR. For an endpoint update, Bits Release watches request volume, latency, and error rates. For a bug fix, it verifies that the original failure no longer occurs and then looks for regressions in related flows.


The developer can review the validation plan directly in Bits Release, next to current information about the deployment and rollout state. This gives the team a single place to determine whether the change is producing the behavior intended, first in staging and then in production.


## Safely roll out features in production


Once the PR merges, Bits Release continues following the change. Engineers deploy through the CI/CD provider they already use, and Bits Release monitors the rollout automatically. Additionally, teams that are using[Datadog Feature Flags](https://www.datadoghq.com/blog/feature-flags/) can control exposure progressively, from small canaries to broader user populations. As the rollout expands, Bits Release activates production-specific validations that monitor customer-facing outcomes.


Production-specific validations include:


- Conversion metrics


- Checkout funnel progression


- Customer interaction quality


These signals run alongside infrastructure telemetry data, and session replays scoped to the new code version let teams inspect what real users experienced during the rollout. Visual regression checks compare pre- and post-deploy UI snapshots to catch rendering issues that traditional tests miss. If any signal deviates from baseline, Bits Release surfaces the issue in the rollout workflow, and engineers can pause or roll back exposure without leaving Slack or GitHub.


## Detect silent production failures before users report them


Some of the most damaging regressions never trigger an alert. Services keep returning 200s, latency stays inside its SLOs, and dashboards look quiet, but the feature itself is broken from a customer’s point of view. Teams usually find out from a support ticket or a drop in conversion metrics.


Following the Couch Cache example, let’s assume that, after the AI shopping assistant rollout reaches production, Bits Testing reruns the same end-to-end validation flows against live traffic. During one of those runs, Bits Release detects that the chatbot is returning empty recommendations to customers, despite the following positive signs:


- No 500 errors are present.


- Infrastructure alerts remain quiet.


- Latency metrics look healthy.


From a systems perspective, the deployment looks fine. From a customer perspective, the feature is failing. Bits Release surfaces the regression in the rollout workflow and posts a notification to the team’s Slack channel before customers begin filing tickets.


## Automatically investigate and fix issues


When Bits Release detects that a PR did not have the expected impact or introduced a regression, Bits Code investigates the root cause by analyzing deployment context, recent commits, service-to-service interactions, and production telemetry data.


In the next step of the Couch Cache scenario, Bits Code determines the root cause of the issue to be a missing IAM permission that prevents the` shop-front` service from calling the` product-catalog` service in production. The change passed staging because staging permissions differed from production permissions, which is the kind of issue that is hard to catch without environment-aware investigation.


Bits Code then opens an automated fix-forward PR linked to the original PR and the detected regression.


After the fix deploys, Bits Release revalidates the rollout end to end. Recommendation quality returns to baseline, the conversion funnel recovers, and the release moves forward—all without manual coordination between observability, security, and platform teams.


## Turn temporary validation checks into permanent guardrails


The checks that Bits Release runs during a rollout are PR-scoped by design. They exist for the life of the release and retire once the rollout completes. Some checks, though, are worth keeping, such as one for detecting a sporadic error that reappears no more than once per week.


Bits Release promotes some validation that runs during a rollout into a long-lived guardrail. The promoted asset moves into Datadog’s existing monitoring stack as a[monitor](https://docs.datadoghq.com/monitors/) or a[Synthetic test](https://docs.datadoghq.com/synthetics/) that sits alongside everything else that the team already maintains. Long-lived guardrails can continue monitoring things like recommendation relevance, conversion flow, and checkout success, which extends the safety loop well beyond the release window.


## Work at AI speed with Datadog


Bits Release gives engineering teams a continuous safety loop that runs from PR to production. With Bits Release, teams can verify performance levels through existing telemetry data, roll out features behind Datadog Feature Flags, and catch silent regressions before users report them. Teams can also bring Bits Code in to investigate and fix issues as they appear. These same checks that protect a release can carry forward into Datadog’s existing monitoring stack as long-lived guardrails.


To get started with Bits Release,[request access to the Preview](https://www.datadoghq.com/product-preview/bits-release/) . You can also learn more about[Bits Testing](https://www.datadoghq.com/product-preview/bits-testing/) ,[Bits Code](https://docs.datadoghq.com/bits_ai/bits_ai_dev_agent/) , and[Feature Flags](https://docs.datadoghq.com/feature_flags/#overview) to see how they fit together.


If you’re new to Datadog, you cansign up for a 14-day free trial to get started.


-
-
-
